# 宣布我们的下一代便利图像:更小、更快、更确定——circle ci

> 原文：<https://circleci.com/blog/announcing-our-next-generation-convenience-images-smaller-faster-more-deterministic/>

CircleCI 为各种编程语言和一些数据库维护了一批 Docker 映像，我们称之为便利映像。这些映像是专门为在[持续集成](https://circleci.com/continuous-integration/) (CI)环境中运行良好而设计的。它们的存在是为了给用户提供一个快速方便的起点。

然而，我们的第一代便利图像是大约四年前设计的。从那以后，我们学到了很多关于设计高效映像的知识，[发布了名为 orbs](https://circleci.com/blog/announcing-orbs-technology-partner-program/) 的可重用配置包，Docker 工具本身也成长起来，变得更加复杂。

我们当前的 14 个 Docker 映像基于 14 个独立的上游 Docker 库映像，这些映像不是为 CI 设计的。这给我们和用户带来了令人惊讶的突破性变化，臃肿的图像，以及无处不在的低效架构。我们开始设计一个更好的系统。

## 介绍我们的下一代便利图像

我们新的便利形象是在 CI、效率和决定论的理念下从头开始构建的。以下是一些亮点:

*   **更快的起转时间** -在 Docker 术语中，这些下一代图像通常具有更少、更小的层。使用这些新映像将导致构建开始时**更快的映像下载，并且更有可能映像已经缓存在主机上**。

*   **提高可靠性&稳定性** -当前图像几乎每天都在重建，来自上游的潜在变化，我们无法总是足够快地进行测试。这导致频繁的破坏性变更，对于稳定的、确定性的构建来说，这不是最好的环境。**下一代映像将只针对安全性和关键错误**进行重建，从而产生更加稳定和确定的映像。

## 新一代图像现已推出

我很高兴地宣布，我们下一代车队的第一张便利图片正式发布。

#### 基础图像

```
image: cimg/base:2020.01 
```

这是一个全新的基于 Ubuntu 的镜像，旨在安装最少的。我们将在未来几周发布的所有下一代便利图片都基于这张图片。你可以在 [Docker Hub](https://hub.docker.com/r/cimg/base) 上找到这张图片，在 [GitHub](https://github.com/CircleCI-Public/cimg-base) 上找到源代码和文档。

**什么时候用？**

如果您需要一个通用映像在 CircleCI 上运行，与 orbs 一起使用，或者作为您自己的定制 Docker 映像的基础，这个映像就是为您准备的。了解如何[迁移到新一代便利映像](https://circleci.com/docs/next-gen-migration-guide/)。

### 语言图像

#### 节点图像

```
image: cimg/node:12.16 
```

这是对传统 CircleCI 节点映像(`circleci/node`)的直接替换。你可以在 [Docker Hub](https://hub.docker.com/r/cimg/node) 上找到这张图片，在 [GitHub](https://github.com/CircleCI-Public/cimg-node) 上找到源代码&文档。

#### 去形象

```
image: cimg/go:1.13 
```

这是对传统 CircleCI Go 图像(`circleci/golang`)的直接替换。你可以在 [Docker Hub](https://hub.docker.com/r/cimg/go) 上找到这张图片，在 [GitHub](https://github.com/CircleCI-Public/cimg-go) 上找到源代码&文档。

#### OpenJDK 图像

```
image: cimg/openjdk:14.0 
```

这是对传统 CircleCI OpenJDK 镜像(`circleci/openjdk`)的直接替换。你可以在 [Docker Hub](https://hub.docker.com/r/cimg/openjdk) 上找到这张图片，在 [GitHub](https://github.com/CircleCI-Public/cimg-openjdk) 上找到源代码&文档。

#### 红宝石图像

```
image: cimg/ruby:2.6.5 
```

这是对传统 CircleCI Ruby 图像(`circleci/ruby`)的直接替换。你可以在 [Docker Hub](https://hub.docker.com/r/cimg/ruby) 上找到这张图片，在 [GitHub](https://github.com/CircleCI-Public/cimg-ruby) 上找到源代码&文档。

#### 铁锈图像

```
image: cimg/rust:1.43.0 
```

这是对传统 CircleCI Rust 图像(`circleci/rust`)的直接替换。你可以在 [Docker Hub](https://hub.docker.com/r/cimg/rust) 上找到这张图片，在 [GitHub](https://github.com/CircleCI-Public/cimg-rust) 上找到源代码&文档。

## 下一步是什么

我们今天发布了这两个新的便利图片，不久的将来还会发布更多图片。我们目前有几个图像预览，将在未来几周内公开发行。你现在就可以在 [CircleCI 讨论](https://discuss.circleci.com/t/next-gen-circleci-convenience-images-public-beta/33010?u=felicianotech)上查看这些预览图片。

附:一切都是开源的，都是为你设计的。[投稿欢迎](https://github.com/CircleCI-Public/cimg-overview)。