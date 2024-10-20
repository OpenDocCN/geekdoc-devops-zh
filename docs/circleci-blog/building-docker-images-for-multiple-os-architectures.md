# 为多个操作系统架构构建 Docker 映像| CircleCI

> 原文：<https://circleci.com/blog/building-docker-images-for-multiple-os-architectures/>

经常有这样的情况，软件被编译并打包成工件，这些工件必须在多个[操作系统](https://en.wikipedia.org/wiki/Operating_system)和[处理器架构](https://en.wikipedia.org/wiki/Microarchitecture)上运行。几乎不可能在不同的操作系统/架构平台上执行应用程序。这就是为什么为许多不同的平台构建版本是一种常见的做法。当您用来构建工件的平台与您想要部署的平台不同时，这可能很难实现。例如，在 Windows 上开发一个应用程序并将其部署到 Linux 和 macOS 机器上，需要为每个操作系统和目标架构平台提供和配置构建机器。管道内的多操作系统构建可以使用各种技术来实现，但是由于处理器架构的严格特性，工件必须在它们所针对的同一硬件上编译和生产。

Docker 是一种将应用程序打包成不可变且可部署的工件的现代方式，其形式为 [Docker 映像](https://docs.docker.com/engine/reference/commandline/images/)和[容器](https://www.docker.com/resources/what-container)。与传统的工件封装一样，Docker 映像也面临着相同的处理器架构构建限制。Docker 映像还必须构建在它们打算运行的硬件架构上。在这篇文章中，我将讨论如何在针对多处理器架构的 [CI 管道](https://circleci.com/blog/what-is-a-ci-cd-pipeline/)中构建 Docker 镜像，例如 linux/amd64、linux/arm64、linux/riscv64 等。

## 入门指南

让我们来看一个由 Chad Metcalf 构建的[示例代码库](https://github.com/metcalfc/docker-circleci-example)，它演示了如何将一个应用打包成多架构 Docker 映像。我们将只关注构建这些多架构 Docker 映像的[持续集成](https://circleci.com/continuous-integration/)方面。CircleCI `config.yml`文件定义了 CI 管道构建指令。它位于`.circleci/config.yml`目录中。在这篇文章中，我将重点放在这次回购的`.circleci/config.yml`文件和`Makefile`文件上。

[Makefiles](https://www.gnu.org/prep/standards/html_node/Makefile-Basics.html#Makefile-Basics) 可以被视为自动化构建过程的 [make 实用程序](https://www.gnu.org/software/make/)所需的构建/编译指令。这个项目中的`Makefile`包含从 CI 管道执行的指令和命令。

## 使用 Docker Buildx

在我深入讨论`Makefile`和`config.yml`文件分解之前，我想花点时间讨论一下 [Buildx](https://docs.docker.com/buildx/working-with-buildx/) ，这是一个当前的 CLI 插件，它用莫比 BuildKit builder 工具包提供的全套功能扩展了 Docker CLI。它提供了与`docker build`相同的用户体验，并增加了许多新的特性，比如创建限定范围的构建器实例和同时构建多个节点。

在撰写本文时，Buildx 特性仍处于**试验**状态，需要在将要构建 Docker 映像的机器上进行一些环境配置。以下是 Docker 版本`19.03`及更高版本的 Buildx 安装说明。完整的 [Buildx 安装说明可以在这里找到](https://github.com/docker/buildx#installing)，下面是 TL；安装了 Docker 19.03 的 Linux 机器的灾难恢复说明。以下命令从源代码编译并构建`Buildx`二进制文件，并将其安装到 Docker 插件目录中:

```
export DOCKER_BUILDKIT=1
docker build --platform=local -o . git://github.com/docker/buildx
mkdir -p ~/.docker/cli-plugins
mv buildx ~/.docker/cli-plugins/docker-buildx 
```

你也可以在这里为你的操作系统下载[最新的 Buildx 二进制文件，并使用这些 Buildx 发布二进制文件的说明](https://github.com/docker/buildx/releases)安装它[。](https://github.com/docker/buildx#binary-release)

在 Docker builder 机器上安装 Buildx 之后，您可以利用 Buildx 的所有功能:

我建议您花时间更好地熟悉 Buildx 特性。在构建多架构 Docker 映像时，这是一项必不可少的技术，并且在下面的示例中大量使用。

## 配置您的 CI 渠道

示例项目中的`config.yml`文件利用`Makefile`文件及其功能来执行适当的命令，以完成多架构构建。这个`config.yml`演示了如何使用[机器执行器](https://circleci.com/docs/executor-types/#using-machine)来利用单个构建作业。这似乎有点不正常，因为 CircleCI 提供了使用 [Docker 执行器](https://circleci.com/docs/executor-types/#using-docker)构建 Docker 映像的能力。

Docker 平台利用[共享和管理其主机操作系统内核](https://docs.docker.com/get-started/overview/)与[虚拟机(VMs)](https://en.wikipedia.org/wiki/Kernel-based_Virtual_Machine) 中的内核仿真。由于运行 Docker 容器共享主机操作系统内核，它们在架构上与虚拟机非常不同。虚拟机不基于容器技术。它们由操作系统的用户空间和内核空间组成。虚拟机服务器硬件是虚拟化的，每个虚拟机都有自己独立的操作系统和应用。它共享来自主机的硬件资源，并可以在虚拟机中模拟各种处理器架构/内核。虚拟机的内核和硬件仿真能力是`machine executor`成为构建多架构 Docker 映像的最佳选择的主要原因。

让我们看看示例项目中的`config.yml`:

```
version: 2.1
jobs:
  build:
    machine:
      image: ubuntu-1604:202007-01
    environment:
      DOCKER_BUILDKIT: 1
      BUILDX_PLATFORMS: linux/amd64,linux/arm64,linux/ppc64le,linux/s390x,linux/386,linux/arm/v7,linux/arm/v6
    steps:
      - checkout
      - run:
          name: Unit Tests
          command: make test
      - run:
          name: Log in to docker hub
          command: |
            docker login -u $DOCKER_USER -p $DOCKER_PASS
      - run:
          name: Build from dockerfile
          command: |
            TAG=edge make build
      - run:
          name: Push to docker hub
          command: |
            TAG=edge make push
      - run:
          name: Compose Up
          command: |
            TAG=edge make run
      - run:
          name: Check running containers
          command: |
            docker ps -a
      - run:
          name: Check logs
          command: |
            TAG=edge make logs
      - run:
          name: Compose down
          command: |
            TAG=edge make down
      - run:
          name: Install buildx
          command: |
            BUILDX_BINARY_URL="https://github.com/docker/buildx/releases/download/v0.4.2/buildx-v0.4.2.linux-amd64"

            curl --output docker-buildx \
              --silent --show-error --location --fail --retry 3 \
              "$BUILDX_BINARY_URL"

            mkdir -p ~/.docker/cli-plugins

            mv docker-buildx ~/.docker/cli-plugins/
            chmod a+x ~/.docker/cli-plugins/docker-buildx

            docker buildx install
            # Run binfmt
            docker run --rm --privileged tonistiigi/binfmt:latest --install "$BUILDX_PLATFORMS"
      - run:
          name: Tag golden
          command: |
            BUILDX_PLATFORMS="$BUILDX_PLATFORMS" make cross-build 
```

您可能已经注意到，这个配置文件中的大多数`command:`键执行`Makefile`中定义的功能。这种模式在配置文件中产生的 YAML 语法要少得多，但是确实使在`Makefile`中实际执行的内容变得复杂了。

接下来，我将重点解释这个配置文件中的关键`command:`键。

```
version: 2.1
jobs:
  build:
    machine:
      image: ubuntu-1604:202007-01
    environment:
      DOCKER_BUILDKIT: 1
      BUILDX_PLATFORMS: linux/amd64,linux/arm64,linux/ppc64le,linux/s390x,linux/386,linux/arm/v7,linux/arm/v6
    steps:
      - checkout
      - run:
          name: Unit Tests
          command: make test
      - run:
          name: Log in to docker hub
          command: |
            docker login -u $DOCKER_USER -p $DOCKER_PASS
      - run:
          name: Build from dockerfile
          command: |
            TAG=edge make build
      - run:
          name: Push to docker hub
          command: |
            TAG=edge make push 
```

在上面的代码中，构建使用了一个`machine executor`并将值赋给了`DOCKER_BUILDKIT`变量，使 Docker 能够访问实验特性和 Buildx。`BUILDX_PLATFORMS`变量是将产生 Docker 映像的操作系统和处理器架构的列表。这个列表是针对 Linux 操作系统和各种处理器架构的。

剩下的`run:`和`command:`键演示了如何执行应用的单元测试，向 Docker Hub 认证以拉和推图像，使用在`/app`目录中找到的`Dockerfile`构建 Docker 图像，并将该图像推送到 Docker Hub。

**注意:** *上面的`docker login`步骤确保您对 Docker Hub 的请求得到验证。无论何时你用 CircleCI 从 Docker Hub 提取图像或向其推送图像，我们都建议你在 CircleCI 配置中的`docker pull`和`docker push`步骤中使用**登录你的 Docker Hub 账户**。登录将确保您的作业可以访问更高的 [Docker Hub 速率限制](https://docs.docker.com/docker-hub/download-rate-limit/)。*

```
 - run:
          name: Install buildx
          command: |
            BUILDX_BINARY_URL="https://github.com/docker/buildx/releases/download/v0.4.2/buildx-v0.4.2.linux-amd64"

            curl --output docker-buildx \
              --silent --show-error --location --fail --retry 3 \
              "$BUILDX_BINARY_URL"

            mkdir -p ~/.docker/cli-plugins

            mv docker-buildx ~/.docker/cli-plugins/
            chmod a+x ~/.docker/cli-plugins/docker-buildx

            docker buildx install
            # Run binfmt
            docker run --rm --privileged tonistiigi/binfmt:latest --install "$BUILDX_PLATFORMS" 
```

在上面的代码片段中，Buildx 特性用于安装 Buildx 二进制文件，并对其进行配置，以便在执行器中使用。Buildx 工具可以使用多种策略构建多架构映像，但是最简单的方法是使用 [Qemu 仿真](https://wiki.qemu.org/Main_Page)。它是一个通用的、开源的机器模拟器和虚拟器。当 BuildKit 需要为不同的架构运行二进制文件时，它会通过在`binfmt_misc`处理程序中注册的二进制文件自动加载它。为了让在主机操作系统上用`binfmt_misc`注册的 QEMU 二进制文件在容器中透明地工作，它们必须用`fix_binary`标志注册。

`docker run --rm --privileged tonistiigi/binfmt:latest --install "$BUILDX_PLATFORMS"`命令为文件前面定义的`$BUILD_PLATFORMS`变量中列出的每个平台拉出并生成一个 [binfmt](https://github.com/tonistiigi/binfmt#installing-emulators) 容器。

```
 - run:
          name: Tag golden
          command: |
            BUILDX_PLATFORMS="$BUILDX_PLATFORMS" make cross-build 
```

上面的代码片段指定了管道中要执行的最后一个命令。它构建了我们想要的多架构 Docker 映像。`command:`键调用在`Makefile`中定义的`cross-build`函数，所以让我们看看与这个函数相关的底层命令。

```
# Makefile cross-build function

.PHONY: cross-build
cross-build:
	@docker buildx create --name mybuilder --use
	@docker buildx build --platform ${BUILDX_PLATFORMS} -t ${PROD_IMAGE} --push ./app 
```

上面的代码片段是实际的`cross-build` `make`命令，它创建了一个新的 Buildx builder 实例。接下来，使用`docker buildx build`命令触发进程，为`${BUILDX_PLATFORMS}`环境变量中列出的每个平台构建一个单独的 Docker 映像。这被输入到指挥的`--platform`旗中。`-t`标志标记/命名 Docker 映像，而`--push`标志会自动将构建结果推送到 Docker 注册表。在这种情况下，它是 Docker Hub。

## 摘要

这篇文章展示了如何在 CI 管道中为多个操作系统和处理器架构构建各种 Docker 映像。这篇文章还简要介绍了 Docker Buildx 特性，它目前是一个实验性的工具，有望在 Docker 的未来版本中成为事实上的构建工具。我认为 Buildx 是下一代 Docker 映像构建工具，它将提供扩展、高级和优化的功能来增强当前的映像构建体验。

我还简要讨论了构建针对多个操作系统和平台架构的 Docker 映像的一些复杂性，这突出了 Docker 容器和 VM 之间的技术差异。虽然从抽象的角度看，它们看起来很相似，但本质上是不同的。最后，我要重申的是， **Docker 已经实施了新的 [Docker Hub 速率限制](https://docs.docker.com/docker-hub/download-rate-limit/)** ，该限制要求对 Docker Hub 的所有呼叫进行认证。每当您从 CircleCI 上的 Docker Hub 提取图像或向其推送图像时，我们建议您使用**登录您的 Docker Hub 帐户**,以完成 CircleCI 配置中的`docker pull`和`docker push`步骤。

感谢你关注这篇文章，希望你觉得有用。请随时在 Twitter [@punkdata](https://twitter.com/punkdata) 上寻求反馈。