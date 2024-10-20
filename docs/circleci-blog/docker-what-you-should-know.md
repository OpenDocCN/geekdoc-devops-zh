# 关于 Docker 和 CircleCI 你需要知道的

> 原文：<https://circleci.com/blog/docker-what-you-should-know/>

CircleCI 2.0 使用 Docker 执行器实现了构建支持。在发布时，我们很快意识到 CircleCI 用户遇到的最大障碍之一是缺乏使用 Docker 的经验。在本帖中，我们将讨论 Docker 和一些用户应该熟悉的基本命令。

## Docker 是什么？

*查看[为您的 CI/CD 管道使用 Docker 的指南](https://circleci.com/blog/docker-what-you-should-know/)*

" Docker 是开发者和系统管理员使用容器开发、部署和运行应用程序的平台."

Docker 也被称为应用程序打包工具，它使应用程序能够被配置并打包到一个 [Docker 映像](https://docs.docker.com/get-started/#images-and-containers)中，该映像可用于生成运行应用程序实例的 [Docker 容器](https://docs.docker.com/get-started/#images-and-containers)。它提供了许多好处，包括运行时环境隔离、代码一致性和可移植性。Docker 容器可以在任何支持 [Docker 引擎](https://www.docker.com/products/docker-engine)的操作系统上运行。对我们有利的是，大多数操作系统都支持它。

## 码头工人执行人

CircleCI 平台使用了一个叫做执行器的概念。执行器是您的 CI/CD 作业/配置在 CircleCI 平台中运行的环境。CircleCI 目前支持三种不同的执行器类型:

在这篇博文中，我们将讨论 Docker executor。

## 配置 Docker 执行器

CircleCI 作业定义在一个 [`config.yml`](https://circleci.com/docs/configuration-reference/) 构建配置文件中。您可以在这里为您的构建指定 Docker 执行器的用法。下面是一个`config.yml`文件的摘录，显示了一个执行者的定义:

```
version: 2
jobs:
  build:
    docker:
      - image: circleci/python:2.7.14 
```

`docker:`键指示平台在 Docker 执行器上构建，而`- image:`键指定在生成 Docker 容器时使用哪个 Docker 映像。构建将在基于`circleci/python:2.7.14`映像的 Docker 容器中执行。

## 基本 Docker 命令

为了回应用户缺乏经验的问题，我们编辑了一个基本 Docker 命令和更多信息链接的列表。这些有助于你了解 Docker，控制执行人。这些命令可以在任何安装了 Docker 引擎的计算机上本地运行。

### 构建 Docker 图像

### Docker 容器

### Docker 映像部署

此外，理解 [Dockerfile:](https://docs.docker.com/engine/reference/builder/) 的概念也很重要，Dockerfile 是一个文本文档，它包含用户可以在命令行上调用的所有命令来组合一个图像。

上面列出的命令是用户通过 Docker 引擎可以使用的所有 Docker 命令的一个子集。它们代表了一组相关的命令，可以让用户快速熟悉 Docker。你可以在这里了解更多关于 [Docker 命令的信息。](https://docs.docker.com/engine/reference/commandline/docker/)

### 在 CircleCI 上构建 Docker 图像

CircleCI 有一个非常酷的特性，使用户能够轻松地在其 CI/CD 管道中构建 Docker 映像。`setup_remote_docker:`键是一个特性，可以在 Docker executor 作业中构建、运行和推送映像到 Docker 注册表。当`docker_layer_caching`设置为`true`时，CircleCI 将尝试重新使用在之前的工作或工作流程中构建的任何 Docker 图像(层),这些图像保持不变。也就是说，在之前的作业中构建的每个未更改的图层都可以在远程环境中访问。然而，有些情况下，即使配置指定了`docker_layer_caching: true`，您的作业也将在干净的环境中运行。这些情况涉及到为依赖于相同环境的相同项目运行许多并行作业。有关 Docker 层缓存的更多信息，请查看[文档页面](https://circleci.com/docs/docker-layer-caching/)。

## 摘要

在这篇文章中，我们讨论了 CircleCI Docker 执行器和一些“必须知道”的 Docker 命令。熟悉这些基本命令将会消除 CircleCI 新用户遇到的一个更大的障碍。

如果你想了解更多关于 CircleCI 的信息，请查看[文档网站](https://circleci.com/docs/)，如果你真的遇到了困难，你也可以通过 https://discuss.circleci.com/[社区/论坛网站联系 CircleCI 社区。](https://discuss.circleci.com/)