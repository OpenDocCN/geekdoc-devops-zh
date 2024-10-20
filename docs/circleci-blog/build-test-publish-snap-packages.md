# Snapcraft - Snapcraft ip | CircleCI

> 原文：<https://circleci.com/blog/build-test-publish-snap-packages/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

Snapcraft 是一个软件包管理系统，它正在努力争取在 Linux 平台上的一席之地，它重新想象了你如何交付你的软件。一组新的跨发行版工具可以帮助您构建和发布“快照”。我们将介绍如何使用 CircleCI 2.0 来支持这个过程，以及在这个过程中可能遇到的一些问题。

## 什么是快照包？还有 Snapcraft？

**快照**是 Linux 发行版的软件包。它们的设计吸取了在 Android 以及物联网设备等移动平台上交付软件的经验教训。 **Snapcraft** 是一个包含快照和构建快照的命令行工具的名称，[是一个网站](https://snapcraft.io/)，几乎是围绕实现这一点的技术的整个生态系统。

Snap 包旨在隔离和封装整个应用程序。这一概念实现了 Snapcraft 的目标，即提高软件的安全性、稳定性和可移植性，允许单个“snap”不仅可以安装在多个版本的 Ubuntu 上，还可以安装在 Debian、Fedora、Arch 等版本上。Snapcraft 网站上的描述:

> 为每个 Linux 桌面、服务器、云或设备打包任何应用程序，并直接提供更新。

## 在 CircleCI 2.0 上构建快照包

在 CircleCI 上构建快照与您的本地机器基本相同，用 [CircleCI 2.0 语法](https://circleci.com/docs/)包装。在这篇文章中，我们将浏览一个样本配置文件。如果你不熟悉 CircleCI 或者想了解更多关于 2.0 的入门知识，可以从这里开始[。](https://circleci.com/docs/first-steps/)

### 基本配置

```
version: 2
jobs:
  build:
    machine: true
    working_directory: ~/project
    steps:
      - checkout
      - run:
          command: |
            sudo apt update && sudo apt install -y snapd
            sudo snap install snapcraft --edge --classic
            /snap/bin/snapcraft 
```

这个例子使用`machine`执行器来安装`snapd`，这个可执行文件允许您管理快照并启用平台，以及`snapcraft`，这个工具用于创建快照。

使用了`machine`执行器而不是`docker`执行器，因为我们需要一个更新的内核用于构建过程。这里提供了 Linux 4.4，这对于我们的目的来说已经足够新了。

### 用户空间依赖性

上面的例子使用了`machine`执行器，它目前是一个带有 [Ubuntu 14.04(可信)](https://circleci.com/docs/1.0/differences-between-trusty-and-precise/)和 Linux v4.4 内核的虚拟机。如果您的项目/snap 需要可信任的存储库中可用的构建依赖项，这是没问题的。如果您需要不同版本的依赖项，比如 Ubuntu 16.04 (Xenial)，该怎么办？我们仍然可以在`machine`执行器中使用 Docker 来构建我们的快照。

```
version: 2
jobs:
  build:
    machine: true
    working_directory: ~/project
    steps:
      - checkout
      - run:
          command: |
            sudo apt update && sudo apt install -y snapd
            docker run -v $(pwd):$(pwd) -t ubuntu:xenial sh -c "apt update -qq && apt install snapcraft -y && cd $(pwd) && snapcraft" 
```

在这个例子中，我们再次在`machine` executor 的 VM 中安装`snapd`，但是我们决定安装 Snapcraft，并在用 Ubuntu Xenial 映像构建的 Docker 容器中构建我们的 snap。所有在 Ubuntu 16.04 中可用的`apt`包将在构建期间对`snapcraft`可用。

## 测试

在我们的博客[、我们的文档](https://circleci.com/blog/)【我们的文档】[以及互联网上，对你的软件代码进行单元测试已经有了广泛的报道。搜索你的语言/框架和单元测试或 CI 将会找到大量的信息。在 CircleCI 上构建快照意味着我们以一个`.snap`文件结束，除了创建它的代码之外，我们还可以测试这个文件。](https://circleci.com/docs/)

### 工作流程

假设我们构建的快照是一个 webapp。我们可以构建一个测试套件来确保此快照正确安装和运行。我们可以试着安装卡扣。我们可以运行 [Selenium](http://www.seleniumhq.org/) 来确保正确的页面加载、登录、工作等。这里有一个问题，快照被设计成可以在多个 Linux 发行版上运行。这意味着我们需要能够在 Ubuntu 16.04、Fedora 25、Debian 9 等平台上运行这个测试套件。CircleCI 2.0 的工作流可以有效地解决这个问题。

CircleCI 2.0 测试版最近增加了一个功能，那就是工作流。*(更新:CircleCI 2.0 现已上线，截止 2017 年 7 月 11 日。点击查看[。)](https://circleci.com/docs/)*这允许我们在 CircleCI 中以一定的流程逻辑运行离散任务。在这种情况下，**在**我们的 snap 构建完成后，这将是一个单独的作业，然后我们可以开始 snap 发行版测试作业，并行运行。我们想要测试的每个发行版都有一个。这些作业中的每一个都是该发行版的不同的 [Docker 映像](https://circleci.com/docs/building-docker-images/)(或者在将来，会有额外的`executors`可用)。

下面是一个简单的例子:

```
workflows:
  version: 2
  build-test-and-deploy:
    jobs:
      - build
      - acceptance_test_xenial:
          requires:
            - build
      - acceptance_test_fedora_25:
          requires:
            - build
      - acceptance_test_arch:
          requires:
            - build
      - publish:
          requires:
            - acceptance_test_xenial
            - acceptance_test_fedora_25
            - acceptance_test_arch 
```

这个设置构建了 snap，然后用四个不同的发行版在其上运行验收测试。如果所有发行版构建都通过了，那么我们可以运行 publish `job`来完成任何剩余的 snap 任务，然后将它推送到 Snap Store。

### 持久化。快照包

为了在工作流示例中测试我们的`.snap`包，需要一种在构建之间持久保存该文件的方法。这里我提两种方式。

1.  **工件**——我们可以在`build`任务期间将快照包存储为 CircleCI 工件。然后在以下作业中检索它。CircleCI Workflows 有自己处理共享工件的方式，可以在这里找到[。](https://circleci.com/docs/workflows/#using-workspaces-to-share-artifacts-among-jobs)
2.  **snap store 频道** -在 snap store 发布快照时，有不止一个`channel`可供选择。将 snap 的主分支发布到`edge`渠道进行内部和/或用户测试已经成为一种惯例。这可以在`build`作业中完成，以下作业从边缘通道安装 snap。

第一种方法完成速度更快，其优势是能够在快照进入快照商店和接触任何用户(甚至是测试用户)之前对其进行验收测试。第二种方法的优势在于，从快照存储安装是 CI 期间运行的测试之一。

## 向快照存储验证

脚本[snap craft-config-generator . py](https://gist.github.com/3v1n0/479ad142eccdd17ad7d0445762dea755)可以生成商店凭证并将它们保存到`.snapcraft/snapcraft.cfg`(注意:在运行公共脚本之前，一定要检查它们)。您不希望在回购中以明文形式存储该文件(出于安全原因)。您可以对文件进行 base64 编码，并将其存储为一个[私有环境变量](https://circleci.com/docs/1.0/environment-variables/#setting-environment-variables-for-all-commands-without-adding-them-to-git)，或者您可以[加密文件][加密文件]并将密钥存储在一个私有环境变量中。

下面是一个将商店凭证保存在加密文件中，并在`deploy`步骤中使用凭证发布到快照商店的示例:

```
- deploy:
    name: Push to Snap Store
    command: |
      openssl aes-256-cbc -d -in .snapcraft/snapcraft.encrypted -out .snapcraft/snapcraft.cfg -k $KEY
      /snap/bin/snapcraft push *.snap 
```

根据前面的工作流示例，这可以是一个仅在验收测试作业通过时运行的部署作业，而不是部署步骤。

## 更多信息