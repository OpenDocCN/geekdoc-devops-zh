# 针对 CI 配置| CircleCI 的 6 个优化技巧

> 原文：<https://circleci.com/blog/six-optimization-tips-for-your-config/>

CircleCI 的客户工程团队帮助用户优化配置文件的设置。每天，他们都能为您的项目找到最有用的功能，平衡您的时间和信用消费。在对我们的 20 多家企业级客户进行了几十次配置审查后，我们的客户工程团队将这一切归结为一门科学。因为您和您的团队可能并不总是有时间与专家合作，所以我们整理了一份配置优化指南供您自己使用。本教程载有 CircleCI 客户工程师的最佳技巧和最有价值的建议。当您需要专家时，我们在这里，但我们也希望让像您这样的团队更容易优化您自己的配置。

## 从配置灾难到配置构建-更快

在这篇文章中，我们将介绍优化你的配置文件的六种最有效的方法，这样你就可以[构建得更快](https://circleci.com/circleci-versus-github-actions/)。您将学习选择正确的执行器、并行化作业、缓存、使用工作区、秘密管理以及在配置中使用 orb 的最佳实践。

## 选择合适的执行者

许多 CI 渠道将受益于包括我们的闪电般快速的 Docker 便利图像舰队之一。使用`docker` yaml 键在 Docker 容器中运行将会以最快的速度提供基本功能。

我们将这些发布到 [Docker Hub `cimg` profile](https://hub.docker.com/r/cimg/) 。如果你的应用需要其他工具，考虑运行一个[定制 Docker 镜像](https://circleci.com/docs/custom-images/)。这里有一个例子，使用了一个旧的，笨重的映像固定到某个版本的 Node 上。这个执行器是通过指定 Docker 映像在每个作业下定义的，比如在这个`test`作业中:

```
test:
    docker:
      - image: circleci/node:9.9. 
```

有了下一代 CircleCI [节点图像](https://hub.docker.com/r/cimg/node)，你可以剥离图层，获得更快的构建。更新到下一代执行器就像更新图像名称一样简单。

当前的配置是在 Node 9.9.0 中构建和测试的，但是我们希望它是使用 Node 的最新版本构建的。为此，我们将用于执行容器的图像替换为我们的下一代图像，如下所示:

```
docker:
  - image: cimg/node:latest 
```

如果你对跨多个环境的测试感兴趣，我们也有能力通过[节点 orb](https://circleci.com/developer/orbs/orb/circleci/node) 设置[矩阵作业](https://circleci.com/blog/circleci-matrix-jobs/)。这允许您在基本 Node Docker 层之上指定要测试的 Node 的不同版本。

## 并行的最佳实践

配置你的任务在多个容器中并行运行加速你的构建。例如，如果您有一个长时间运行的测试套件，其中有数百个独立的测试，那么可以考虑将这些测试分散到不同的执行器上同时运行。真正优化的配置意味着明智地使用并行性。您应该仔细考虑您运行了多少个并行执行器，以及拆分任务节省的时间是否值得多个容器的启动时间。还要确保您在这些执行器之间正确地分割了您的测试。
考虑下一个例子。这个测试工作的主要活动是使用`npm run test`命令运行测试:

```
test:
 …
 parallelism: 10
 steps:
    ...
     - run: CI=true npm run test 
```

虽然使用并行是朝着正确方向迈出的一步，但这并不是最佳的写法。这个命令将简单地在所有 10 个容器上运行完全相同的测试。为了跨多个容器分配测试，这个配置需要使用来自 CircleCI CLI 的 [`circleci tests split`命令。当按文件名、类名或计时数据分割时，测试可以自动地在这些容器之间分配。](https://circleci.com/docs/parallelism-faster-jobs/#using-the-circleci-cli-to-split-tests)[通过计时数据进行分割](https://circleci.com/docs/parallelism-faster-jobs/#splitting-by-timing-data)对于并行化来说是理想的，因为它将测试均匀地分布在各个容器上，所以不会有运行速度更快的容器空闲地等待长时间运行的测试容器。

最后，考虑这是否是这个测试套件的正确并行级别。如果启动环境需要大约 30 秒，但是在每个容器中测试只需要 30 秒，那么可能值得考虑降低并行度，以便在所有作业运行中花费更少的时间。测试运行时间和加速时间之间没有黄金比例，但是对于一个最佳的构建应该考虑这个比例。下面是优化后的配置的样子，优化后的配置通过文件名和计时来分割测试，并在给定的容器中运行更多的测试:

```
test:
 …
 parallelism: 5
 steps:
    ...
     - run: |
            TESTFILES=$(circleci tests glob "test/**/*.test.js" | circleci tests split --split-by=timings)
            CI=true npm run test $TESTFILES 
```

## 缓存的最佳实践

使用[缓存](https://circleci.com/docs/caching/)加速构建。这允许您重用耗时的获取操作中的数据。以下示例使用缓存从以前的作业运行中恢复其 npm 依赖关系。因为 npm 依赖项被缓存，`npm install`步骤将只需要下载在`package.json`文件中描述的新依赖项。这种依赖关系缓存通常与 npm、Yarn、Bundler 或 pip 等包依赖关系管理器一起使用，它依赖于两个特殊的步骤，即`restore_cache`和`save_cache`。该示例展示了如何在`test`作业中使用这些缓存步骤:

```
test:
  ...
  steps:
    …
    - restore_cache:
        keys:
          - v1-deps-{{ checksum "package-lock.json" }}
    - run: npm install
    - save_cache:
        key: v1-deps-{{ checksum "package-lock.json" }}
        paths:
          - node_modules 
```

注意到`restore_cache`和`save_cache`步骤都使用了键。关键字是定位缓存的唯一标识符。`save_cache`步骤指定在这个键下缓存哪些目录。在这种情况下，我们将保存`node_modules`目录，以便这些节点依赖项可以在以后的作业中使用。`restore_cache`步骤使用这个键来查找要恢复到作业的缓存。密钥是一个字符串，其中包含标识缓存的版本和编写为`checksum “package-lock.json”`的依赖项清单文件的内插散列。

虽然这是恢复和保存缓存的标准模式，但您可以使用[回退键](https://circleci.com/docs/caching/#partial-dependency-caching-strategies)进一步优化它。回退键允许您标识一组可能的缓存，以增加缓存命中的可能性。例如，如果将单个包添加到该应用程序的`package.json`中，checksum 生成的字符串将会改变，整个缓存将会丢失。然而，添加具有更广泛的可能的键匹配集合的回退键可以识别其他可用的缓存。下面是一个添加了回退密钥的缓存恢复示例:

```
test:
  ...
  steps:
    …
    - restore_cache:
        keys:
          - v1-deps-{{ checksum "package-lock.json" }}
          - v1-deps- 
```

请注意，我们刚刚向键列表中添加了另一个元素。让我们回到我们的`package.json`中单个包发生变化的场景。在这种情况下，第一个关键字将导致缓存未命中。然而，第二个键允许恢复先前保存在旧的`package.json`文件中的缓存。依赖项安装步骤`npm install`将只需要获取改变的包，而不是对所有的包使用不必要的和昂贵的获取操作。访问我们的文档，阅读更多关于[回退键和部分缓存恢复](https://circleci.com/docs/caching/#partial-dependency-caching-strategies)的信息。

## 有选择地坚持工作空间

下游作业可能需要访问前一个作业中生成的数据。工作区允许您在工作流的整个生命周期中存储文件。下一个示例配置说明了这个概念。`build`作业构建一个节点应用程序。工作流中的下一个作业部署应用程序。这个配置将整个工作目录持久化到`build`中的工作区，然后将目录附加到`deploy`中，因此 deploy 可以访问构建的应用程序。

```
 build:
    ...
    steps:
      ...
      - run: npm run build
      - persist_to_workspace:
          root: .
          paths:
            - '*'
  deploy:
    ...
    steps:
       ....
        - attach_workspace:
            at: . 
```

在`build`作业中创建的应用程序目录可由`deploy`作业访问。有效果，但不理想。工作区本质上只是创建 tarball 并将它们存储在 blob 存储中，附加工作区需要下载和解压缩这些 tarball。这可能是一个耗时的过程。有选择地保存您以后的作业需要的文件会更快。本例中的`npm run build`步骤产生一个`build`目录，该目录可以被压缩，然后存储在工作区中以供部署。以下是此配置的优化版本:

```
 build:
    ...
    steps:
      ...
      - run: npm run build
      - run: mkdir tmp && zip -r tmp/build.zip build
      - persist_to_workspace:
          root: .
          paths:
            - 'tmp'

  deploy:
    ...
    steps:
       ....
        - attach_workspace:
            at: . 
```

包含构建工件的`tmp`目录现在将被挂载到项目的工作目录中。这个配置有选择地存储压缩的、构建的应用程序，而不是上传和下载整个工作目录，以节省归档、上传和下载工作空间所花费的时间。压缩文件可以存储在工作区的临时目录中。任何附加了工作空间的下游作业现在都可以访问这个 zip 文件。你可以在这个[深入 CircleCI 工作区](https://circleci.com/blog/deep-diving-into-circleci-workspaces/)中了解更多。

## 机密管理的最佳实践

您不希望将您的秘密签入到版本控制中，并且不应该在您的配置中以纯文本的形式编写秘密。CircleCI 为您提供访问[上下文](https://circleci.com/docs/contexts/)的权限。这些允许您在组织中的项目间保护和共享环境变量。上下文本质上是一个秘密的存储，您可以将环境变量设置为在运行时注入的名称/值对。为了更好地理解这一点，请查看下一个代码示例，一个不安全的配置。这个配置包括一个用明文编写的 AWS 机密定义的`deploy`任务。

```
deploy:
 …
 steps:
    ...
     - run: 
            name: Configure AWS Access Key ID
            command: |
              aws configure set aws_access_key_id K4GMW195WJKGCWVLGPZG --profile default
        - run: 
            name: Configure AWS Secret Access Key
            command: |
              aws configure set aws_secret_access_key ka1rt3Rff8beXPTEmvVF4j4DZX3gbi6Y521W1oAt --profile default 
```

**注意** : *这些是仅用于演示目的的虚假凭证。*

在 CircleCI 上，所有能够访问您的项目的开发人员都可以看到这个文本。相反，这些秘密应该作为环境变量存储在上下文中。将密钥和访问 ID 作为键/值对添加到标题为`aws_secrets`的上下文中，可以将它们作为环境变量进行访问。然后，可以将此上下文应用于工作流中的作业。该配置的安全版本如下所示:

```
deploy:
 …
 steps:
    ...
     - run: 
            name: Configure AWS Access Key ID
            command: |
              aws configure set aws_access_key_id ${AWS_ACCESS_KEY_ID} --profile default
        - run: 
            name: Configure AWS Secret Access Key
            command: |
              aws configure set aws_secret_access_key ${AWS_SECRET_ACCESS_KEY} --profile default

workflows:
  test-build-deploy:
     ...
      - deploy:
          context: aws_secrets
          requires:
            - build 
```

请注意，机密已经从纯文本变成了环境变量，并且上下文应用于工作流中的作业。为了增加安全性，我们采用了[秘密屏蔽](https://circleci.com/blog/keep-environment-variables-private-with-secret-masking/)来防止用户意外打印秘密值。

## orb 和可重用的配置元素

因此，您已经为您的构建选择了正确的执行者，您正在适当地分割您的测试，并且您正在坚持工作空间以避免重复您的工作。现在你必须为你所有的其他项目这样做。多痛苦啊，我说的对吗？如果有一种方法可以在多次构建之间重用配置文件的共享元素就好了。好消息。

Circle CI 提供了一个名为 [orbs](https://circleci.com/orbs/) 的特性，它允许您在一个中心位置定义配置元素，并快速方便地在多个项目中重用它们。不仅如此，你实际上可以传递参数到 orbs。您可以创建一个 orb，根据您传递给它的参数，它可以在不同的项目中做多种不同的事情。

使用我们的 2.1 配置版本，您还可以定义配置的[可重用元素](https://circleci.com/docs/reusing-config/)，以便在同一管道的多个作业中重用，从简单的作业步骤到重用整个执行器。您还可以将参数传递给这些可重用的元素。当您需要跨管道的多个不同部分重用配置文件的多个元素时，这很有用。

球体在实践中是什么样子的？这里有一个部署到 S3 存储桶的例子，完全写在配置文件中，没有使用我们的 [AWS S3 部署 orb](https://circleci.com/developer/orbs/orb/circleci/aws-s3) :

```
- deploy:
    name: S3 Sync
    command: |+
      aws s3 sync \
        build s3://my-s3-bucket-name/my-application --delete \
        --acl public-read \
      --cache-control "max-age=86400" 
```

那将完成工作。但这是使用 S3 球体后的样子:

```
- aws-s3/sync:
     from: bucket
     to: ‘s3://my-s3-bucket-name/my-application’
     arguments: |
       --acl public-read \
       --cache-control "max-age=86400" 
```

您不需要为 S3 部署声明单独的部署阶段。您可以简单地从 orb 调用 S3 同步，作为配置文件中的一个步骤。请注意，仍然包含了许多相同的信息，但是它现在表示为传递到 orb 中的参数，而不是配置文件中的脚本。这不仅更加简洁，而且还使得根据需要添加、删除或更改参数来更改 S3 部署变得非常容易。这个版本更容易一目了然，您可以通过更新 orb 来扩展多个项目。从这里提到的所有建议中，最重要的一点是，D.R.Y [(不要重复自己)](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)不仅仅是一件审美的事情。有了 orbs，跨项目复制的能力就是黄金。快乐的球体优化！

## 配置审查

这篇文章提供了 6 种方法来优化你自己的配置。如果您需要帮助，您应该知道我们在黄金和白金高级支持计划中提供配置审查高级服务。通过配置审查服务，CircleCI DevOps 客户工程师将与您的团队合作构建新配置或审查现有配置。我们将审查您的需求并提供建议，以充分利用 CircleCI 的功能。要了解更多关于优质服务和支持计划，请联系我们在 cs@circleci.com。

* * *

如果没有整个客户工程团队的共同努力，这篇文章是不可能完成的:Anna Calinawan、Johanna Griffin 和 Grant MacGillivray。