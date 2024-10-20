# GitHub pull 请求-管理机密| CircleCI

> 原文：<https://circleci.com/blog/managing-secrets-when-you-have-pull-requests-from-outside-contributors/>

Mozilla 喜欢尽可能开放地工作，这意味着我们主要在可公开访问的代码库中进行开发，无论我们是否期望外部合作者。然而，这些存储库仍然需要连接到其他系统，这有时涉及到管理敏感凭证。我们如何让这些连接为维护人员提供丰富的[工作流](https://circleci.com/docs/workflows/)，同时也为外部贡献者提供良好的体验？

我们将在 GitHub 中构建一个示例 Java 项目，该项目使用 CircleCI 对所有 pull 请求(PRs)进行测试，无论是来自主存储库的分支还是 fork。然后，我们将添加条件逻辑，当可信提交者将代码推送到主存储库时，该条件逻辑将构建并部署一个 java jar 工件到亚马逊 S3。

## 创建项目

让我们使用 [Apache Maven](https://maven.apache.org/index.html) 作为构建工具来生成一个小的 Java 项目:

```
 mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=managing-secrets -DarchetypeArtifactId=maven-archetype-quickstart -Dversion=1.3 -DinteractiveMode=false 
```

现在，我们将创建一个简单的 CircleCI 工作流，运行单个`test`步骤:

```
 version: 2.0

    jobs:
      test:
        docker:
          - image: circleci/openjdk:8-jdk
        steps:
          - checkout
          - run: mvn clean test

    workflows:
      version: 2
      build:
        jobs:
          - test 
```

一旦我们将它提交到 GitHub 并在 CircleCI 中启用它作为一个项目，每次推送都会触发 CircleCI 中的`build`工作流的运行。从主存储库的任何分支发出的 PRs 将显示`test`作业的状态。

## 为分叉 PRs 启用 CircleCI

现在，我们希望为来自分叉存储库的拉请求启用相同的工作流。这不仅允许没有提交权限的贡献者提出变更，而且对于喜欢从自己的分支工作的提交者也很有帮助。

为了启用 CircleCI 的分叉拉取请求，我们在 CircleCI 中进入我们项目的设置页面，选择**构建设置** > **高级设置**，并启用 **[构建分叉拉取请求](https://circleci.com/docs/oss/#build-pull-requests-from-forked-repositories)** 选项。

当我们在那里时，注意下一个选项，**将秘密从分叉的拉请求**传递到构建。这是默认禁用的，这正是我们在这里想要的。在下一步中，我们将上传 AWS 凭证，我们不希望意外地将它们暴露给我们组织之外的用户。

## 添加秘密

我们通过将 AWS 凭证设置为[特定于项目的环境变量](https://circleci.com/docs/env-vars/#setting-an-environment-variable-in-a-project)，使它们对可信构建可用。注意，创建一个由多个项目共享的[上下文](https://circleci.com/docs/contexts/)也是可能的。

我们现在将移动到 CircleCI 中项目配置的**构建设置** > **环境变量**部分，并添加`AWS_ACCESS_KEY_ID`和`AWS_SECRET_ACCESS_KEY`变量，这些变量包含允许写入亚马逊 S3 中我们将存放[工件][14]的选定位置的凭证。这些变量不会为从分叉的 pull 请求触发的 CircleCI 作业设置，而只会为由具有 commit 访问权限的人发起的主存储库上的分支设置。

## 构建和部署工件

此时，我们已经准备好向我们的[持续集成(CI)][15]工作流添加逻辑，以构建一个 jar 并将其部署到 S3。为了让我们的 CI 作业尽可能快地运行，我们将把 jar 工件与`test`作业并行打包。一旦测试和打包成功完成，我们将把工件部署到 S3。

我们将以下作业定义添加到我们的`config.yml`中，使用[工作区](https://circleci.com/docs/workflows/#using-workspaces-to-share-data-among-jobs)在`package`和`deploy`步骤之间共享数据:

```
 jobs:
      test:
        ...
      package:
        docker:
          - image: circleci/openjdk:8-jdk
        steps:
          - checkout
          - run: mvn clean package
          - persist_to_workspace:
              root: target
              paths:
                - managing-secrets-1.3.jar
      deploy:
        docker:
          - image: python:3.7
        steps:
          - checkout
          - attach_workspace:
              at: target
          - run: pip install awscli
          - run: aws s3 cp target/managing-secrets-1.3.jar s3://mybucket/managing-secrets/$CIRCLE_BRANCH/managing-secrets-1.3.jar 
```

我们希望将这些新任务添加到我们的工作流中，并将`deploy`定义为依赖于`test`和`package`步骤。我们的工作流现在在配置中表示为:

```
 workflows:
      version: 2
      build:
        jobs:
          - test
          - package
          - deploy:
              requires:
                - test
                - package 
```

我们将这些更改提交给 master，并且我们有了第一次成功的部署！🎉Alice 是我们在另一家公司的朋友，她对项目的进展很兴奋，她想提出一个改进方案，所以她分叉这个项目并发出第一个拉动请求。不幸的是，Alice 的公关没有通过我们的 CI 测试。`deploy`步骤返回:

```
 upload failed: ... Unable to locate credentials 
```

这里有好有坏。好的一面是，CircleCI 完全按照我们的要求做了；它运行分叉公关的工作流程，没有暴露任何秘密。从坏的方面来说，这对爱丽丝来说是一次令人困惑的经历；在她打开 PR 之前，她确保她的新代码和测试在本地正常工作，所以她有理由期望她的 PR 应该通过我们的 CI 测试。

我们需要引入更多的逻辑来检测分叉的 PRs，并延迟部署，直到可信的提交者批准并合并代码。

## 定义在分叉 PRs 上提前返回的命令

CircleCI 2.1 配置引入了[可重用的用户定义命令](https://circleci.com/docs/reusing-config/#authoring-reusable-commands)，我们将利用这个概念来制作一个`early_return_for_forked_prs`命令。调用它将使我们知道分叉 PRs 不需要的工作或我们知道会失败的工作短路。确保参考[上的文档，启用配置重用](https://circleci.com/docs/reusing-config/#getting-started-with-config-reuse)。

首先，我们如何在一个工作运行中辨别这是否是一个分叉的 PR？我们可以直接检查传入的特定环境变量是否存在，比如`AWS_ACCESS_KEY_ID`，但是我们希望实现一个更通用的解决方案，它可以被复制到任何项目中，而不考虑我们已经定义的特定秘密集。相反，我们将使用由 [CircleCI 的内置环境变量](https://circleci.com/docs/env-vars/#built-in-environment-variables)提供的关于作业的丰富上下文。我们感兴趣的特定变量是`CIRCLE_PR_NUMBER`，记录为“相关联的[GitHub 或 Bitbucket][16] pull 请求的编号。仅在分叉的 PRs 上可用。如果`CIRCLE_PR_NUMBER`存在，那么我们知道我们正在运行一个无法访问秘密的分叉 PR 的构建。

为了在 shell 语法中表达这个条件，我们使用了`-n`(非零长度)测试。条件将是这样的:

```
 if [ -n "$CIRCLE_PR_NUMBER" ]; then
      # mark this job successful and stop processing
    fi 
```

CircleCI 上的大多数执行器都有一个可用的本地 [`circleci-agent`命令行界面](https://circleci.com/docs/local-cli/)，它提供了我们填写这个条件表达式所需的命令:

```
 circleci-agent step halt 
```

现在，我们准备将所有这些放入配置的一个新的顶级`commands`部分:

```
 commands:
      early_return_for_forked_pull_requests:
        description: >-
          If this build is from a fork, stop executing the current job and return success.
          This is useful to avoid steps that will fail due to missing credentials.
        steps:
          - run:
              name: Early return if this build is from a forked PR
              command: |
                if [ -n "$CIRCLE_PR_NUMBER" ]; then
                  echo "Nothing to do for forked PRs, so marking this step successful"
                  circleci step halt
                fi 
```

我们添加自定义命令作为部署作业的第一步:

```
 jobs:
      deploy:
        docker:
          - image: python:3.7
        steps:
          - early_return_for_forked_pull_requests
          - checkout
          - attach_workspace:
              at: target
          - run: pip install awscli
          - run: aws s3 cp target/managing-secrets-1.3.jar s3://mybucket/managing-secrets/$CIRCLE_BRANCH/managing-secrets-1.3.jar 
```

此时，我们可以为`package`作业添加相同的命令，因为它的唯一目的是为我们跳过的`deploy`作业暂存工件。我们还不如不要浪费计算时间去建造一个我们从来不用的工件。

如果爱丽丝在这些配置改变的基础上重新设置她的 PR，CircleCI 现在会运行得更快，并由于早期的返回而显示所有绿色。当她的变更被批准和合并时，包括秘密在内的完整工作流将在主分支运行，构建一个批准的工件并部署到 S3。

## 进一步阅读

这里讨论的演示项目的完整代码可以在 GitHub 上的 [jklukas/managing-secrets](https://github.com/jklukas/managing-secrets) 获得。

了解更多关于 CircleCI [上下文 REST API](https://circleci.com/blog/securing-ci-cd-pipelines-with-circleci-contexts-rest-api/) 的信息。

要查看这种方法在实际生产环境中的应用，请参见[mozilla/telemetry-batch-view][12]和[Mozilla/telemetry-streaming][13]，Mozilla 数据平台团队在这些存储库中定义了 Spark 转换作业，用于从 Firefox 遥测数据创建派生数据集。对这些存储库的每次推送都会触发一次构建，并将一个 jar 工件交付给 S3；我们通过旋转指向一个已部署 jar 的 Amazon EMR 集群来运行转换。默认情况下，夜间运行引用 S3 的`master/`路径中的工件，因此我们的 CircleCI 配置确保了白天合并到 master 的代码将在第二天晚上运行。

https://github . com/Mozilla/telemetry-batch-view/blob/33 D1 BF 1 cafd 29098 a 989d 08770358361 a 93 D7 BC 3/。circle ci/config . yml[12]:https://github . com/Mozilla/telemetry-streaming/blob/b 3318 acdfeae 5e 0 f 7d 5a 484 BFA be 809355 F3 ad C5/。circle ci/config . yml[14]:https://circleci.com/docs/artifacts/[15]:https://circleci.com/continuous-integration/[16]:https://circleci.com/docs/gh-bb-integration/

* * *

杰夫·克鲁卡斯有实验粒子物理学的背景，他既是教师又是帮助发现希格斯玻色子的研究人员。他现在在俄亥俄州哥伦布市的 Mozilla 公司的 Firefox 数据平台上远程工作，之前是 Simple 公司数据平台的技术负责人，Simple 是一家云端无分支银行。

[阅读更多杰夫·克鲁卡斯的文章](/blog/author/jeff-klukas/)