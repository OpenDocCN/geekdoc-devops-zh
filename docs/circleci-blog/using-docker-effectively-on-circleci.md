# 充分利用 Docker 和工作流，第 1 部分:在 CircleCI 上有效地使用 Docker

> 原文：<https://circleci.com/blog/using-docker-effectively-on-circleci/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

作为一名成功的工程师，我经常在 CircleCI 上看到很多关于使用 Docker 的困惑。在 CircleCI 2.0 发布之前，无需考虑 Docker 就可以构建和测试应用程序:我们所有的构建都在 [LXC 容器](https://linuxcontainers.org/)中运行，这些容器由我们的推理引擎配置，以相当自动的方式包含任何必要的工具和依赖项。

然而，随着 CircleCI 2.0 的[发布，我们将 Docker 放在了构建过程的前端和中心:任何 Docker 映像，公共的或](https://circleci.com/blog/launching-today-circleci-2-0-reaches-general-availability/)[私有的](https://circleci.com/docs/private-images/)(我们现在也支持托管在亚马逊 [EC2 容器注册表](https://aws.amazon.com/ecr/)上的私有映像！)，可以是生成的主容器。这增加了构建过程的能力和定制，特别是当与[工作流](https://circleci.com/blog/introducing-workflows-on-circleci-2-0/)结合时，它允许多组作业(构建、测试、部署等)。)串在一起，有时并行运行，每个都有自己的基本 Docker 映像。为了帮助从 CircleCI 1.0 过渡到 2.0，我们甚至提供了广泛的[便利图像](https://hub.docker.com/r/circleci/)来满足常见的语言和依赖需求，调整以最大限度地兼容 CircleCI 平台。

然而，正如他们所说，权力越大，责任越大。使用 Docker 可能会令人生畏。对于一些开发人员来说，它似乎有很高的学习曲线。其他人担心维护或保养他们创造的任何图像。此外还有隐私问题——毕竟，Docker 主注册中心 [Docker Hub](https://hub.docker.com/) 上的许多图片是完全公开的。但是当你[在 CircleCI 2.0](https://circleci.com/docs/test/) 上构建和测试项目时，Docker 会是一个有价值的盟友。下面，我们概述了一些人们在使用 CircleCI 2.0 时遇到的常见问题，以及 Docker 如何帮助解决这些问题。

### 问题 1:构建时间太长

在 CircleCI 上运行一个构建可能需要很长时间的原因有很多。但是一个常见的因素是安装依赖项。当我帮助客户将项目迁移到 CircleCI 2.0 时，我通常会从添加一些缺失的包开始。不过，很快，这可能会变成一系列的`run`步骤，而且，即使启用了缓存，也会给大型项目的构建时间增加几分钟。缓解这种情况的一种方法是将这些依赖项卸载到 Docker 映像中。

您的 Docker 映像可能感觉像是与您的 CircleCI 构建分开的东西，但将它视为构建过程的第一步会有所帮助。如果每次运行构建时都要安装相同的几个包，那么没有理由不将它们安装到 Docker 映像中。这样，它们将在您每次开始构建时被预安装。作为一个例子，假设我们使用 CircleCI 便利图像。从这里开始，创建 docker 文件并添加几个简单的步骤来安装我们的依赖项并不困难。您的 docker 文件可能如下所示:

```
FROM circleci/ruby:2.4.1-node-browsers

USER root

# pulling in commands from 'install os packages'
RUN apt-get update
RUN apt-get install -y qt5-default libqt5webkit5-dev gstreamer1.0-plugins-base gstreamer1.0-tools gstreamer1.0-x postgresql-client

USER circleci 
```

这告诉 Docker 从我们的`circleci/ruby:2.4.1-node-browsers`便利图像开始，切换到`root`安装几个依赖项，然后切换回`circleci`(我们所有便利图像的默认用户)。

假设您已经[创建了 Docker ID](https://cloud.docker.com/) 并在您的计算机上安装了 Docker，那么您需要做的就是在本地构建您的映像并将其推送到 Docker Hub:

`docker build -t my-docker-id/my-image:my-tag /path/to/Dockerfile` `docker push my-docker-id/my-image:my-tag`

现在，您可以使用您的新映像进行构建，并从您的`config.yml`文件中删除那些`apt-get install`命令，节省宝贵的构建时间，同时保持您的`config.yml`代码简洁明了。更多信息请参见[我们的流程演练](https://circleci.com/docs/custom-images/)，或者查看[码头工人指南](https://docs.docker.com/docker-cloud/builds/push-images/)。

### 问题 2:我需要一个特定的[X]遗留版本

通过我们的便利图片，我们试图涵盖大多数语言和软件开发环境的最流行的版本。但是有时候你可能需要一个特定的版本，比如说，Node.js 如果我们不提供呢？

这是我们在 CircleCI 2.0 上与客户合作时遇到的一个相当常见的问题。不访问他们的源代码，很难知道使用特定依赖项的不同版本是否会破坏他们的构建，所以我通常宁可小心谨慎，也尽量复制他们的 1.0 执行环境。在这种情况下，所需要的只是 Dockerfile 中的几行文本来获取我们的便利 Node.js 图像并降级到不同的版本:

```
FROM circleci/node:6.11

USER root

# delete node 6.11 files
RUN rm -f /usr/local/bin/node /usr/local/bin/nodejs /usr/local/bin/npm /usr/local/share/man/man1/node.1 /usr/local/share/systemtap/tapset/node.stp
RUN rm -rf /usr/local/lib/node_modules /usr/local/include/node /usr/local/share/doc/node

# download node 6.8.1 from here: https://nodejs.org/en/download/releases
RUN wget https://nodejs.org/download/release/v6.8.1/node-v6.8.1.tar.gz
RUN tar xvf node-v6.8.1.tar.gz

WORKDIR node-v6.8.1

# install node 6.8.1 as described here: https://github.com/nodejs/node/blob/master/BUILDING.md#building-nodejs-on-supported-platforms
RUN ./configure
RUN make -j4
RUN make doc
RUN make install

USER circleci 
```

这里我们从节点 6.11 版本开始，删除所有相关的节点 6.11 文件和目录，下载/编译/安装节点 6.8.1 版本。在这种情况下，有几个节点版本管理工具可能会使这项任务变得更容易( [n](https://github.com/tj/n) 或 [nvm](https://github.com/creationix/nvm) 是流行的选择)，或者我们可以尝试使用 Ubuntu 的`apt-get`包管理系统，但对我来说，从源代码开始重新编译总是更安全。

一个简单的谷歌搜索可以帮助识别你正在使用的任何语言或工具的升级/降级过程，并且在一天结束时，你仍然可以受益于我们在 CircleCI 上运行的便利图像的优化。从那里开始，过程是相同的:构建您的映像，将其推送到 Docker Hub，并放入您的`config.yml`。

### 问题 3:但是等等，我不想在我的本地机器上构建 Docker 映像

这实际上很好，因为有了 CircleCI 2.0，您不必这样做。相反，你可以使用我们的 [`machine`执行程序](https://circleci.com/docs/executor-types/#machine-executor-overview)为你的构建启动一个完整的虚拟机，并在 CircleCI 中随心所欲地创建 Docker 映像。或者将`docker`执行器与 [`setup_remote_docker`键](https://circleci.com/docs/building-docker-images/)一起使用——我们实际上在六月份发布了一篇关于这种方法的[整篇博文](https://circleci.com/blog/how-to-build-a-docker-image-on-circleci-2-0/)。

您甚至可以建立一个完整的存储库来构建 Docker 映像— [这就是我们的工作方式](https://github.com/circleci/circleci-images)。随着计划作业的即将发布，您将能够定期使用我们的资源构建您的映像，无需维护。

### 问题 4:我不希望我的 Docker 图片被公开

您的 Docker 图像可能包含大量私人或敏感数据，您可能不希望这些数据被公众访问。

幸运的是，Docker Hub 支持私人 Docker 图像——这就像在您的 [Docker Hub 帐户设置](https://hub.docker.com/account/settings)中更改图像的默认可见性一样简单。你甚至可以更进一步，使用[完全私有的 Docker 注册表](https://circleci.com/docs/private-images/)来代替 Docker Hub。

* * *

准备好了吗？阅读本系列的第二篇博文，了解如何[利用工作流](https://circleci.com/blog/getting-the-most-out-of-docker-and-workflows-part-2-all-about-workflows/)将你新学到的码头工人技能提升到一个新的水平。或者参见[高级客户成功工程师 Ryan O'Hara 的这篇精彩讨论文章](https://discuss.circleci.com/t/so-you-want-to-build-a-docker-image/10030)，更深入地了解如何打造 Docker 形象。