# 在不受信任的分叉| CircleCI 上触发受信任的 CI 作业

> 原文：<https://circleci.com/blog/triggering-trusted-ci-jobs-on-untrusted-forks/>

在公共代码库中，通常允许任意用户进行分叉和发出拉请求(我们称之为“分叉 PRs”)。这些用户可能来自您的组织之外，通常被认为是不可信的，无法运行自动化构建。如果您希望一个[持续集成](https://circleci.com/continuous-integration/)服务运行需要访问凭证或敏感数据的测试，这会导致一个问题，因为恶意开发人员可能会提出将凭证暴露给 CI 日志的代码。即使对于私有存储库，您也可能希望组织中的一些用户能够查看存储库，而不会触发泄露机密的构建。

之前，在[当您有来自外部贡献者](https://circleci.com/blog/managing-secrets-when-you-have-pull-requests-from-outside-contributors/)的拉请求时管理秘密中，我们讨论了如何隐藏来自分叉的 pr 的凭证并跳过需要它们的构建。但是这假设了有凭证的构建是为了准备构建工件，并且只有合并真正需要掌握。如果在合并之前，一个可信作业的输出对于变更的适当审查是必需的，那么我们不能简单地跳过这个作业。我们想要的是一种方法，让我们团队中受信任的成员检查分叉的 PR，验证它不会引入可能泄露机密的更改，然后启动我们需要的受信任的 CI 工作，以确保更改的完整性。在这篇文章的前半部分，我们将讨论如何使用 Git 本身作为将代码标记为可信并启用该工作流的手段。第二部分将全面演示如何将这些概念应用于特定的存储库主机(GitHub)和 CI 提供者(CircleCI)。

## 通过向上游推送将代码标记为可信

将拉请求合并到存储库中的行为可以部分地看作是将代码标记为有效和可信的行为。分叉可以被看作是一种隔离用户更改的方式，直到它们被验证。外部贡献者通常不能将代码推送到主存储库的任何分支，而只能推送到他们自己的分支。

像 GitHub 这样的提供商已经以支持这种模型的方式建立了他们的安全控制:通常，只有一组特定的指定用户被允许对存储库进行任何更改。CI 提供者在设置他们的权限时也考虑到了这一点:CI 权限通常是这样配置的，CI 系统将只向推送至主存储库时触发的作业公开凭证。

如果构建是在创建 PR 时自动触发的，那么任何将构建配置直接放入存储库中的系统都有运行不可信代码的风险。但这也意味着将代码推送到上游分支的行为可以被用作信任的表达，从而允许受信任的作业运行。如果来自分叉 PR 的代码被推送到上游分支，则上游分支和分叉分支的 Git 引用(“refs”)将是相同的，指向相同的提交。GitHub 知道它们是相同的代码，许多 GitHub 的 API 端点只关心提交引用，而不关心引用所关联的分支。在 CI 作业的情况下，GitHub 通过提交进行索引，因此作为将代码推送到上游分支的结果而运行的可信作业也将与指向相同提交的分叉 PR 相关联。

## 实施触发可信配置项作业的工作流

现在，让我们通过构建一个示例项目将上述理论付诸实践。我们将有一个在任何 PR 上运行的基本`test`作业，即使它来自一个分支，但我们也将有一个更全面的`test-with-data`作业，它需要 AWS 凭证来从亚马逊 S3 拉一些私人数据。我们希望避免触发后一个作业，直到对存储库具有提交访问权限的人审查 PR 以确保它不会泄漏凭证。

### 创建项目

让我们从我们在[管理秘密中创建的同一个小型 Java 项目开始，当您有来自外部贡献者](https://circleci.com/blog/managing-secrets-when-you-have-pull-requests-from-outside-contributors/)的拉请求时。使用 [Apache Maven](https://maven.apache.org/index.html) 作为构建工具，我们可以通过以下方式生成项目:

```
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=managing-secrets -DarchetypeArtifactId=maven-archetype-quickstart -Dversion=1.3 -DinteractiveMode=false 
```

现在，我们将创建一个简单的 CircleCI 工作流来运行一个`test`任务:

```
version: 2.1

jobs:
  test:
    docker:
      - image: circleci/openjdk:8u171-jdk
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

### 为分叉的拉请求启用 CI

现在，我们想要为来自分叉存储库的拉请求启用相同的工作流。这不仅允许没有提交权限的贡献者提出变更，而且对喜欢从自己的分支工作的提交者也有帮助。

要为分叉拉取请求启用 CI，我们在 CircleCI 中进入项目的设置页面，选择**构建设置** > **高级设置**，并启用“[构建分叉拉取请求](https://circleci.com/docs/oss/#build-pull-requests-from-forked-repositories)选项。

当我们在那里时，注意下一个选项，“从分叉的拉请求向构建传递秘密”。这是默认禁用的，这正是我们在这里想要的。下一步，我们将上传 AWS 凭证，我们不想意外地将它们暴露给组织外的用户。

### 添加秘密

我们通过将 AWS 凭证设置为[特定于项目的环境变量](https://circleci.com/docs/env-vars/#setting-an-environment-variable-in-a-project)，使它们对可信构建可用。注意，创建一个由多个项目共享的[上下文](https://circleci.com/docs/contexts/)也是可能的。

我们现在将移动到 CircleCI 中项目配置的**构建设置** > **环境变量**部分，并添加`AWS_ACCESS_KEY_ID`和`AWS_SECRET_ACCESS_KEY`变量，这些变量包含允许从亚马逊 S3 的一个位置读取的凭证，我们已经在该位置存放了我们的私有测试数据。这些变量将只针对由具有提交访问权限的人发起的对主存储库上的分支的推送进行设置，而不针对由分叉的 pull 请求触发的 CircleCI 作业。

### 添加不为分叉 PRs 运行的可信作业

我们现在准备设置一个额外的作业，该作业访问凭证并基于私有数据运行测试。让我们来看看更新后的配置:

```
version: 2.1

jobs:
  test:
    docker:
      - image: circleci/openjdk:8u171-jdk
    steps:
      - checkout
      - run: mvn clean test
  test-with-data:
    docker:
      - image: circleci/openjdk:8u171-jdk
    steps:
      - checkout
      - run: |
          if [ -z "$AWS_ACCESS_KEY_ID" ]; then
            echo "No AWS_ACCESS_KEY_ID is set! Failing..."
            exit 1;
          else
            echo "Credentials are available. Let's fetch the data!"
          fi

workflows:
  version: 2
  build:
    jobs:
      - test
      - test-with-data:
        filters:
          branches:
            # Forked pull requests have CIRCLE_BRANCH set to pull/XXX
            ignore: /pull\/[0-9]+/ 
```

这份新工作有点虚假；为了保持这篇文章的简短，我们还没有实现任何真正的数据获取或者使用这些数据的测试。相反，我们只是通过检查凭证的存在来避免这种情况。如果该作业是通过推送到主存储库来启动的，那么环境变量应该是可用的，并且该作业应该通过。

我们使用过滤器将`test-with-data`作业放入我们的工作流中，确保作业在被分叉的 PR 触发时不会运行。在上一篇文章中，我们实现了一种在 PR 分叉的情况下提前返回的技术；过滤器完成了几乎相同的事情，但是不太详细。

### 测试分叉 PRs 的审核工作流

现在让我们对我们的存储库做一个小的配置更改，以使测试被跳过变得更加明显；毕竟，如果我们正在经历添加一个可信作业来用真实数据运行我们的测试的麻烦，那么确保这些测试通过可能是确保我们项目的完整性的核心。

在我们的 GitHub repo 中，我们将转到**设置** > **分支**，我们将添加一个**分支保护规则**。在我们的特定例子中，目标分支是`triggering`，我们希望启用“合并前需要通过状态检查”的切换，并确保我们根据需要选择了两个 CI 作业(显示为`ci/circleci: test`和`ci/circleci: test-with-data`)。

现在，当有人从 fork 发出 PR 时，`test`作业将运行并且(希望)在成功时变成绿色。`test-with-data`作业不会运行(因为这是一个分叉的 PR)，但因为我们已将其标记为必需，它仍会显示在 PR 页面的“状态检查”部分，状态为“预期—等待报告状态”，请审核者注意该作业需要在合并前运行。更好的是，我们可以为评审者创建一个带有检查表的`PULL_REQUEST_TEMPLATE.md`，明确地指导他们在开始可信构建之前寻找潜在的安全问题。

让我们考虑一个在这个州的公关例子-[jklukas/管理秘密#4](https://github.com/jklukas/managing-secrets/pull/4) 。查看此 PR 的审查者将对代码进行初步扫描，发现 PR 在将凭证打印到日志的 CI 作业中引入了一个更改，然后请求更改或关闭 PR；`test-with-data`从不运行，因此凭据永远不会暴露。

相反，如果变更是良性的，并且评审者决定运行它是安全的，他们可以通过将 PR 的提交推送到主存储库的一个分支来开始可信的工作。这可以通过直接的`git`调用手动完成，但是有点繁琐。一般的工作流程是将 PR 的 fork 作为远程添加，下拉 PR 的分支，将该分支推到主存储库，然后进行清理。您可以从[jklukas/git-push-fork-to-upstream-branch](https://github.com/jklukas/git-push-fork-to-upstream-branch)安装一个小型 bash 脚本，使它成为一个更方便的一行程序:

```
git-push-fork-to-upstream-branch upstream <fork_username>:<fork_branch> 
```

一旦代码被推送到上游分支，分叉 PR 上的`test-with-data`状态检查将进入执行状态，并有希望让我们进入完全绿色状态，准备合并。

## 进一步阅读

在[管理来自外部贡献者的 pull 请求时的秘密](https://circleci.com/blog/managing-secrets-when-you-have-pull-requests-from-outside-contributors/)中，我们深入讨论了 CI 凭证管理的一些选项，并探索了一种允许分叉的 pr 在需要凭证来暂存工件的 CI 作业中提前返回的方法。

在这篇文章中，我们改进了这种方法，并探索了一种方法，通过将提交推送到上游分支，让存储库提交者在分叉的 PR 上触发可信构建。你可以在[Mozilla-services/Mozilla-pipeline-schemas](https://github.com/mozilla-services/mozilla-pipeline-schemas)看到一个更真实的例子。该存储库包含 JSON 模式，Mozilla 使用这些模式来验证传入数据管道的有效负载，这是一个非常有价值的用例，可以根据真实客户端数据的样本测试所有更改，以捕捉任何可能导致管道开始拒绝以前有效负载类的回归。虽然进入该存储库的 pr 通常是由 Mozilla 员工提议的，但是这些员工通常没有对主存储库的写访问权限，因此从 forks 发出他们的 pr，这使得支持提议和测试分叉 pr 的愉快过程变得很重要。

一些新的探索是 [CircleCI 最近宣布的受限上下文](https://circleci.com/blog/protect-secrets-with-restricted-contexts/)，它包括将秘密注入工作流图的一部分的可能性，由手动批准步骤触发。这可能是让评审者触发可信构建的更方便和灵活的方法的基础。

* * *

杰夫·克鲁卡斯有实验粒子物理学的背景，他既是教师又是帮助发现希格斯玻色子的研究人员。他现在在俄亥俄州哥伦布市的 Mozilla 公司的 Firefox 数据平台上远程工作，之前是 Simple 公司数据平台的技术负责人，Simple 是一家云端无分支银行。

[阅读更多杰夫·克鲁卡斯的文章](/blog/author/jeff-klukas/)