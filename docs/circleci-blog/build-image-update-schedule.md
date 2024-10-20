# 构建映像更新计划

> 原文：<https://circleci.com/blog/build-image-update-schedule/>

### TL；速度三角形定位法(dead reckoning)

| 图像类型 | 时间范围 | 更多信息 |
| --- | --- | --- |
| Docker 便利图片 | 小于 24 小时(自动) | [了解更多信息](#docker-convenience-images) |
| 机器图像 | 需要时(手动) | [了解更多信息](#machine-image) |
| Xcode 图像 | 少于 7 天(手动) | [了解更多信息](#macos-image) |

### Docker 便利图片

CircleCI 上的大多数作业都是由`docker`执行程序运行的。这使得 Docker 映像能够用于作业的执行环境。虽然可以使用 Docker Hub、AWS 的 ECR 或 GCP 的 Container Registry 上的任何 Docker 映像，但我们提供了一组映像，如`circleci/golang`，其中预装了在 CI 环境中有用的工具。利用 Docker 的分层能力，我们的图像基于 Docker 和 Docker 社区 [Docker Library](https://hub.docker.com/u/library/) 发布的上游图像。

#### 更新

每当一个新的版本出现，例如最近的 Go v1.11 版本，Docker 库社区就会更新这个映像。在这种情况下，它们更新了`golang`图像。这需要多长时间完全由他们决定。有时，图像会在几小时内更新。如果有阻塞 GitHub 的问题，可能需要一两天。无论如何，我们的 [Docker 便利图像构建系统](https://circleci.com/blog/wide-world-of-workflows-how-we-build-our-docker-convenience-images/)将根据需要，使用[预定工作流程](https://circleci.com/docs/workflows/#scheduling-a-workflow)每 24 小时从上游重建新图像。因此，一旦上游图像通过 Docker 库可用，我们将在 24 小时内拥有我们的版本。

关于[文档页面](https://circleci.com/docs/circleci-images/)的更多信息。

#### 测试版和发布候选版

Docker 提供了很大的灵活性。使用 Docker 库映像以及 CircleCI 映像的一个好处是，许多软件开发人员会为不稳定的版本发布标签。例如，在 Go v1.11 问世和`golang/circleci:1.11`图像可用之前，几个发布候选(RC)标签已经可用了相当长一段时间。像`circleci/golang:1.11rc`或`circleci/golang:1.11-r2`这样的标签在每个 RCs 之后都是可用的。这使您能够在发布日之前开始在即将到来的语言版本上测试您的软件。

### 机器图像

当需要由`docker`执行器提供的低级访问时，通常使用`machine`执行器。它是一个虚拟机(VM)映像，而不是 Docker/LXC 映像。

由于其极其普通的用例，图像几乎不经常更新。它只安装了一些基本的构建模块工具。`machine`镜像安装了 Docker 和 Docker Composed。这就是为什么这个映像会定期更新的主要原因:为了跟上 Docker 的步伐。

#### 更新

没有设定更新此映像的时间表。它目前大约每月更新一次，但可能需要更长时间。提供此映像是为了让用户能够在虚拟机上安装他们需要的东西。可以在[文档页面](https://circleci.com/docs/configuration-reference/#machine)上找到关于`machine`可用图像版本的信息。

### macOS 映像

执行人是最简单的解释。我们基于 Xcode 版本发布，而不是处理基于月份的版本号，甚至 macOS 版本号。我们有基于 Xcode 版本的语义版本化( [SemVer](https://semver.org/) )的独立图像，比如`Xcode 9.4.1`图像。

#### 更新

当有新的 Xcode 版本时，我们会发布新的图像。节奏主要取决于苹果。一旦某个版本的 Xcode 发布，我们通常会在七天内为它准备一个映像。这一时间框架受版本变化的影响很大(例如，bugfix 版本花费的时间最少)，也受苹果发布更新的周末时间的影响。可用图像/支持的 Xcode 版本可在[文档页面](https://circleci.com/docs/testing-ios/#supported-xcode-versions)上找到。