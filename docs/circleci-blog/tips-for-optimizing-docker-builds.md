# 优化 Docker 版本的技巧| CircleCI

> 原文：<https://circleci.com/blog/tips-for-optimizing-docker-builds/>

Docker 图像被用作 [Docker 执行器](https://circleci.com/docs/executor-types/#using-docker)中的主要图像。它们是容器的蓝图，为容器的产生提供了指导。在这篇文章中，我将阐述一些经常被忽视的概念，这些概念将有助于优化 Docker 映像开发和构建过程。

## 如何建立 Docker 形象？

让我们从 Docker 构建过程的简要描述开始。它是通过使用 [Docker CLI 工具](https://docs.docker.com/engine/reference/commandline/cli/#description)运行 [`docker build`命令](https://docs.docker.com/engine/reference/commandline/build/)触发的过程。

`docker build`命令基于名为 [Dockerfile](https://docs.docker.com/engine/reference/builder/) 的文件中指定的指令构建 Docker 映像。Dockerfile 是一个文本文档，包含用户在命令行上调用的所有有序命令，以组合一个图像。

Docker 图像由只读层组成。每一层代表一个 Dockerfile 指令。这些层是堆叠在一起的，每一层都是前一层变化的增量。我认为这些层是缓存的一种形式。只对改变的层进行更新，而不是在每次改变时更新每个层。

以下示例描述了 Dockerfile 文件的内容:

```
FROM ubuntu:18.04
COPY . /app
RUN make /app
CMD python /app/app.py 
```

该文件中的每个指令代表 Docker 映像中的一个单独的层。以下是每个指令的简要说明:

*   `FROM`从`ubuntu:18.04` Docker 图像创建一个层
*   从 Docker 客户端的当前目录添加文件
*   使用 make 构建您的应用程序
*   `CMD`指定在容器中运行什么命令

在构建过程中执行这四个命令时，它们将在 Docker 映像中创建层。

如果你有兴趣了解更多关于[图像和图层的信息，你可以在这里阅读](https://docs.docker.com/storage/storagedriver/#images-and-layers)。

## 优化映像构建流程

现在我们已经介绍了一些关于 Docker 构建过程的内容，我想分享一些优化建议来帮助高效地构建映像。

### 短暂的容器

docker 文件定义的图像应该生成短暂的容器。在这个上下文中，[临时容器](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#create-ephemeral-containers)意味着可以停止和销毁的容器，然后使用绝对最小的设置和配置重新构建并替换为新产生的容器。短暂的容器可以被认为是一次性的。每个实例都是新的，与之前的容器实例无关。在开发 Docker 映像时，您应该尽可能多地利用短暂的模式。

### 不要安装不必要的软件包

避免安装不必要的文件和软件包。Docker 图像应尽可能保持简洁。这有助于提高可移植性、缩短构建时间、降低复杂性和减小文件大小。例如，在大多数情况下，不需要在容器上安装文本编辑器。不要安装任何非必要的应用程序或服务。

### 实施。dockerignore 文件

[`.dockerignore`](https://docs.docker.com/engine/reference/builder/#dockerignore-file) 文件排除了与您在其中声明的模式相匹配的文件和目录。这有助于避免不必要地将大型或敏感的文件和目录发送到守护程序，并可能将它们添加到公共映像中。

要排除与构建无关的文件而不重构您的源存储库，使用一个`.dockerignore`文件。该文件支持类似于`.gitignore`文件的排除模式。

### 对多行参数排序

只要有可能，通过按字母数字顺序对多行参数进行排序来简化以后的更改。这有助于避免包的重复，并使列表更容易更新。这也使得 PRs 更容易阅读和审查。在反斜杠前加一个空格也有帮助。

这里有一个来自 [Docker Hub](https://hub.docker.com/_/buildpack-deps) 上 Docker 的`buildpack-deps`图像的例子:

```
RUN apt-get update && apt-get install -y \
  bzr \
  cvs \
  git \
  mercurial \ 
  subversion \
  && rm -rf /var/lib/apt/lists/* 
```

### 分离应用程序

依赖于其他应用程序的应用程序被认为是“耦合的”在某些情况下，它们托管在同一主机或计算节点上。这在非容器部署中很常见，但是对于微服务，每个应用程序都应该存在于它自己的容器中。将应用程序解耦到多个容器中使得水平伸缩和重用容器变得更加容易。例如，一个解耦的 web 应用程序堆栈可能由三个独立的容器组成，每个容器都有自己唯一的映像:一个用于管理 web 应用程序，一个用于管理数据库，一个用于内存缓存。

将每个容器限制为一个进程是一个很好的经验法则。使用你最好的判断来保持容器尽可能的干净和模块化。然后，如果容器相互依赖，您可以使用 Docker 容器网络来确保这些容器可以通信。

### 尽量减少层数

只有`RUN`、`COPY`和`ADD`指令创建层。其他指令创建临时中间映像，最终不会增加构建的大小。在可能的情况下，只将您需要的工件复制到最终图像中。这允许您在中间构建阶段包含额外的工具和/或调试信息，而不会增加最终映像的大小。

### 利用构建缓存

在构建映像的过程中，Docker 遍历 Docker 文件中的指令，按顺序执行每个指令。在每条指令中，Docker 都会在其缓存中搜索一个现有的映像来使用，而不是创建一个新的副本映像。这是 Docker 遵循的基本规则:

> 从已经在高速缓存中的父映像开始，将下一条指令与从该基础映像派生的所有子映像进行比较，以查看它们中是否有一个是使用完全相同的指令构建的。否则，缓存将失效。

在大多数情况下，只需将 docker 文件中的指令与子映像之一进行比较就足够了。但是，某些说明需要更多的检查和解释。

对于`ADD`和`COPY`指令，检查映像中文件的内容，并为每个文件计算校验和。这些校验和不考虑文件的最后修改时间和最后访问时间。在缓存查找期间，校验和会与现有映像中的校验和进行比较。如果文件中的任何内容(如内容和元数据)发生了更改，缓存将失效。

除了`ADD`和`COPY`命令，缓存检查不查看容器中的文件来确定缓存匹配。例如，当处理一个`RUN apt-get -y`更新命令时，不检查容器中更新的文件来确定是否存在缓存命中。在这种情况下，命令字符串用于查找匹配项。

一旦缓存失效，所有后续的 Dockerfile 命令都会生成新的图像，并且缓存不会被使用。利用您的缓存包括将您的图像分层，以便只有底层经常变化。您希望您的`RUN`步骤在 docker 文件的底部变化频繁，而变化不频繁的步骤应该在顶部排序。

## 优化 CI 管道中的 Docker 映像构建

到目前为止，我一直专注于优化 Docker 映像构建，同时从代码和 Docker CLI 构建的角度介绍了与此过程相关的概念。将这些优化策略实施到 [CI 管道](https://circleci.com/blog/what-is-a-ci-cd-pipeline/)中并没有太大的不同，CircleCI 有一个特定的 Docker 构建优化，它将显著加快您的自动化 Docker 构建工作。

首先，前面几节中提到的所有优化概念都适用于在您的 CI 渠道中实现。尤其是缓存。如果 docker 文件有变化，利用缓存仍然是减少构建时间的最佳方式。

作为您 CI 渠道的一部分，这是如何运作的？当使用 Docker executor 作为构建作业的运行时，您可以利用一个名为 [Docker 层缓存(DLC)](https://circleci.com/docs/docker-layer-caching/) 的特性来加速这些构建。

当构建 Docker 映像是 CI 流程的常规部分时，DLC 是一个很好的功能。DLC 将保存在您的作业中创建的图像层。DLC 缓存作业期间构建的任何 Docker 图像的各个层，然后在后续 CircleCI 运行中重用未更改的图像层，而不是每次都重建整个图像。

您的 docker 文件从提交到提交的变化越少，您的映像构建步骤将运行得越快。DLC 可以与机器执行器和远程 Docker 环境(`setup_remote_docker`)一起使用。值得注意的是，DLC 仅在使用`docker build`、`docker compose`或类似的 Docker 命令创建*您自己的 Docker 映像时有用，它**不会**减少所有构建启动初始环境所需的挂钟时间。如果你有兴趣了解更多关于 DLC 的知识，你可以在我们的[文档](https://circleci.com/docs/docker-layer-caching/#how-dlc-works)中阅读。*

*欲了解更多 Docker 内容，请参见[CI/CD 管道 Docker 使用指南](https://circleci.com/blog/guide-to-using-docker-for-your-ci-cd-pipelines/)。*

## 摘要

在这篇文章中，我介绍了构建 Docker 映像的优化技术。提供的构建建议将作为高效开发 Docker 映像的指南。这些构建建议极大地加快了 CI 管道的速度。

大多数人不需要构建自己的自定义映像。在 CircleCI，我们建立了一个由 [CI 优化 Docker 映像](https://circleci.com/developer/images)组成的车队，用于您的 CI 管道。在[宣布我们的下一代便利图像中，阅读我们关于图像开发的决定:更小、更快、更确定的](https://circleci.com/blog/announcing-our-next-generation-convenience-images-smaller-faster-more-deterministic/)。

感谢你关注这篇文章，希望你觉得有用。请随时在 Twitter [@punkdata](https://twitter.com/punkdata) 上寻求反馈。