# 配置最佳实践:Docker 层缓存| CircleCI

> 原文：<https://circleci.com/blog/config-best-practices-docker-layer-caching/>

让我们面对现实:创建最佳 CI/CD 工作流并不总是一项简单的任务。事实上，编写有效和高效的配置代码是许多开发人员在 DevOps 之旅中面临的最大障碍。但是，您不需要成为专家就可以建立快速、可靠的测试和部署基础设施。通过一些简单的技巧，您可以优化您的`config.yml`文件并释放您的 CI/CD 管道的全部潜力。

在本系列中，我们将深入探讨我们的解决方案工程师在与企业级客户进行一对一配置审查时提出的一些常见建议。今天，我们重点关注 Docker 层缓存(DLC)，这是一种用于在作业之间保存 Docker 层并加快工作流程的强大技术。

DLC 直到最近才在性能和规模计划中提供，现在已包括在新的 CircleCI 免费计划以及 CircleCI 服务器的安装中。在本帖中，我们将讨论什么是 Docker 层缓存，为什么它很重要，以及如何将它添加到您的管道中。

## 什么是 Docker 层缓存？

Docker 是 CircleCI 平台上使用的容器化技术。在 [Docker 执行器](https://circleci.com/docs/executor-types/#using-docker)和[机器执行环境](https://circleci.com/docs/executor-types/#using-machine)中，您可以运行 Docker 构建命令来封装您的应用程序以进行测试和部署。使用 Docker 层缓存，您可以保存您构建的 Docker 映像的各个层，以便在后续管道运行中重用它们。

Docker 层缓存可以绕过部分或全部映像构建步骤，从而在构建过程中为您的团队节省大量时间。Docker 映像是从 docker 文件构建的，docker 文件中的每个命令都会在映像中创建一个新层。当您使用`docker build`或`docker compose`构建 Docker 映像时，DLC 会将各个层保存到与运行作业的机器或远程 Docker 实例相连的卷中。下次运行影像构建作业时，CircleCI 将从缓存中检索任何未更改的图层。

如果您的 Dockerfile 在管道运行之间保持不变，将从缓存中检索整个图像。如果您在两次运行之间对 docker 文件进行了更改，CircleCI 将检索到该更改之前的所有层，然后基于新的 docker 文件构建图像的其余部分。您的 docker 文件在运行之间更改得越少，您的映像构建得就越快。

## 什么时候应该使用 Docker 层缓存？

Docker 容器从根本上改变了软件开发的前景，通过将二进制文件和依赖项打包到一个可移植的软件单元中，消除了潜在的环境冲突或“它在我的机器上工作”的难题，允许团队协作、安全和一致地构建、测试和部署应用程序。许多团队已经采用[容器化](/blog/benefits-of-containerization/)作为实现云原生开发实践的方式，并将他们的软件架构转向更分布式的方法。

如果您的应用程序依赖于容器，您可能会发现自己在每次运行 CI/CD 管道时都构建相同的映像，这会严重消耗您的构建时间。通过启用 Docker 图层缓存，您可以重用缓存的图像图层，而不是在每次运行构建时从头开始构建，从而显著减少构建时间。

请注意，DLC 只会减少在远程 docker 环境中使用`docker build`、`docker compose`或类似的 Docker 命令构建自己的 Docker 映像所需的时间。它不影响旋转[主对接容器](https://circleci.com/docs/glossary/#primary-container)所花费的时间。如果您在 Docker 容器中运行管道，但不在工作流中构建新的映像，那么您不会看到通过实现 DLC 来减少构建时间。

## 如何将 Docker 层缓存添加到管道中

您可以使用 Docker 层缓存来加速 Docker 和机器执行环境中的 Docker 映像构建。在 Docker 执行环境中，您创建的任何 Docker 映像都构建在一个名为 remote Docker environment 的二级容器中，这为构建过程增加了额外的安全性，并允许您访问各种 Docker 命令以及将 [SSH 到您的构建](/blog/debugging-ci-cd-pipelines-with-ssh-access/)中进行调试。您可以使用`setup-remote-docker`键启动远程 Docker 环境，然后通过将`docker_layer_caching`设置为`true`来向构建作业添加 Docker 层缓存:

```
version: 2
jobs:
  build:
    docker:
      # DLC does nothing here, its caching depends on commonality of the image layers.
      - image: cimg/node:17.3.0
        auth:
        username: mydockerhub-user
          password: $DOCKERHUB_PASSWORD
    steps:
      - checkout
      - setup_remote_docker:
          docker_layer_caching: true
      # DLC will explicitly cache layers here and try to avoid rebuilding.
      - run: docker build . 
```

在这个示例`config.yml`文件中，您将`docker`设置为`build`作业的执行环境，并设置一个远程 Docker 环境，将`docker_layer_caching`设置为`true`。CircleCI 将缓存您使用`docker build`命令构建的 Docker 图像的图层，以便您下次运行该作业时，可以避免重新构建任何未更改的图层。

机器执行环境不需要远程 Docker 环境来运行`docker`或`docker-compose`命令，因此您可以将`docker_layer_caching: true`键直接包含在机器执行器键的下面:

```
version: 2
jobs:
  build_elixir:
    machine:
      image: ubuntu-2004:202104-01
      docker_layer_caching: true
    steps:
      - checkout
      - run:
          name: build Elixir image
          command: docker build -t circleci/elixir:example . 
```

在本例中，您将`ubuntu-2004:202104-01` Linux 映像设置为机器执行器类型，然后设置`docker_layer_caching: true`为该环境启用 Docker 层缓存。当您使用`docker build`命令构建 Elixir 映像时，CircleCI 将缓存各个层，以便在以后的运行中重用。

为项目启用 DLC 后，保存的所有图层将在作业运行之间的三天内可用。任何三天后不再使用的缓存图层都将被删除。每个项目最多可以创建 50 个 DLC 卷。有关如何在您的项目中实现 Docker 层缓存的更多信息，以及一些潜在的用例和限制，请访问 [DLC 文档](https://circleci.com/docs/docker-layer-caching/)。

## 结论

Docker 层缓存是在 CircleCI 上加速 Docker 构建的重要部分，现在包含在 CircleCI 免费计划中。如果您的团队构建容器化的应用程序，您可以通过在项目中重用未更改的 Docker 层来节省宝贵的开发时间。这不仅使您的团队更加高效，并为您的组织节省资金，而且还帮助您更快地向您的用户交付价值，这是当今软件开发环境中的一个关键优势。

DLC 只是您可以对配置进行的许多不同优化中的一种。其他基于缓存的优化包括将数据保存在工作区中，以及缓存依赖项以加快构建。关于依赖缓存的更多信息，请参见[配置最佳实践:依赖缓存](https://circleci.com/blog/config-best-practices-dependency-caching/)。请继续关注本系列后续文章中对可用配置优化的其他深入探讨。

要开始使用最强大的 CI/CD 平台更快、更自信地构建、测试和部署您的应用，[立即注册免费计划](https://circleci.com/signup/)。如果您对 CircleCI 配置的专家级审查感兴趣，可以通过注册[高级支持计划](https://circleci.com/support/plans/)，获得专门支持工程师的个性化一对一评估。

* * *