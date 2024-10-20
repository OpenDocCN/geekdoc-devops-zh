# 针对 CI/CD 管道使用 Docker 的指南

> 原文：<https://circleci.com/blog/guide-to-using-docker-for-your-ci-cd-pipelines/>

## Docker 是什么？

Docker 是开发人员和系统管理员使用[容器](https://circleci.com/blog/benefits-of-containerization/)开发、部署和运行应用程序的平台。

Docker 也被称为应用程序打包工具。这意味着可以将启用的应用程序配置并打包到 Docker 映像中，该映像可用于生成运行应用程序实例的 Docker 容器。它提供了许多好处，包括运行时环境隔离、代码一致性和可移植性。

Docker 容器可以在任何支持 Docker 引擎的操作系统上运行。在 [CircleCI 和 Docker:你需要知道的事情](/blog/docker-what-you-should-know/)中找到更多信息

## 什么是码头集装箱？

Docker 容器旨在隔离和大规模运行应用程序。它们允许简化应用程序的管理和实现。

Docker 容器是基于并衍生自 Docker 图像的对象。Docker 图像是模板，就像饼干切割器一样。曲奇切片机使您能够快速、一致地从面团中生产出一块块曲奇。就像 cookie 是通过 cookie cutter 的类型来区分形状一样，Docker 映像决定了它们所创建的 Docker 容器的规格。

## 在 CircleCI 上构建 Docker 图像

Docker 映像包含构建 Docker 容器的指令，因此特定于希望在容器中运行的应用程序类型。在 CircleCI 上构建 Docker 映像被简化了，因为 CircleCI 原生支持 Docker。这允许在 Docker 中构建 Docker 和[多级 Docker 构建](/blog/multi-stage-docker-builds/)。CircleCI 支持使用`setup_remote_docker:`键在 CI/CD 管道中构建 Docker 映像，从而能够从 Docker executor 作业中构建、运行和推送映像到 Docker 注册表。

CircleCI 通过 Dockerfile 向导使[构建定制 Docker 映像变得更加容易。此外，看看这些优化 Docker 构建的](/blog/build-custom-docker-images-faster-and-more-easily-with-our-dockerfile-wizard/)[技巧](/blog/tips-for-optimizing-docker-builds/)，帮助你开始优化你的 Docker 映像开发和构建过程。不是每个人都想构建自定义图像。[创建一个定制的 Docker 映像来运行您的 CI 构建](/blog/creating-a-custom-docker-image-to-run-your-ci-builds/)不是一项简单的任务。在需要安装大量软件包、需要编译源代码或通过慢速连接下载的情况下，这是您自己的自定义 Docker 映像大放异彩的最佳时机。

## 使用 Docker 图像

Docker 实现了[持续集成和交付](https://circleci.com/continuous-integration/)最强大的好处:独立构建和测试。[用 Docker](/blog/build-cicd-piplines-using-docker/) 构建 CI/CD 管道利用干净的容器来消除本地应用程序开发中出现的任何依赖问题。这个过程需要将你的申请归档。

## 归档应用程序

在 Docker 中包装应用程序是加强正确的依赖关系和操作系统的强大方法。它还需要额外的知识来正确地配置和打包容器映像。有几个工具可以帮助你整理你的应用程序。例如，你可以用 Jib 对 Java 应用程序进行 Dockerize。使用 Dockerfile 对应用程序进行 Dockerfile 化是最常见的方式。以下是一些使用 Dockerfile 对应用程序进行 Dockerizing 的不同示例:

### 发布 Docker 图像

还有[很多 Docker 注册中心](https://www.g2.com/categories/container-registry) : Docker Hub、AWS 弹性容器注册中心、Azure 容器注册中心、Google 容器注册中心等等。CircleCI 拥有部署 orb，可以帮助设置这些服务的部署。一个 [orb](https://circleci.com/orbs/) 是一个可重用的 [YAML 配置](https://circleci.com/blog/what-is-yaml-a-beginner-s-guide/)包，它将重复的配置压缩成一行代码。例如，您可以使用 [AWS ECR orb](https://circleci.com/developer/orbs/orb/circleci/aws-ecr) 自动将私有 Docker 映像部署到 AWS ECR 。

## Docker 图像的安全性

使用公开可用的 Docker 镜像涉及到减轻与 OSS 软件相关的风险类型。因此，扫描图像对于发现漏洞至关重要。图像扫描可以在 orbs 的帮助下进行，以简化扫描工具的集成，如 [Anchore](https://circleci.com/developer/orbs/orb/anchore/anchore-engine) 、 [AquaSec](https://circleci.com/developer/orbs/orb/aquasecurity/microscanner) 和 [Snyk](https://circleci.com/developer/orbs/orb/snyk/snyk) 。例如，请参见[使用 Anchore](/blog/Adding-container-security-scanning-anchore/) 将容器安全扫描添加到 CircleCI 管道或[使用 Twistlock orb 将容器图像扫描集成到 CircleCI 构建中](/blog/integrating-container-image-scanning-into-circleci-builds-with-the-twistlock-orb/)或 CircleCI 工作流中的[使用 Snyk 安全。](/blog/security-with-snyk-in-the-circleci-workflow/)

## 测试码头集装箱

测试 Docker 容器是任何 CI/CD 管道的关键部分。大多数团队在应用程序级测试方面做得很好，并且有大量的框架(包括 JUnit 和 RSpec)来支持它。但是服务器级测试——服务器配置和服务的验证——经常被忽略。要了解如何针对自定义 Docker 映像执行测试，请参见[使用 CircleCI 和 Goss 测试 Docker 映像](/blog/testing-docker-images-with-circleci-and-goss/)。

### 我应该使用 Docker 吗？

是的，您应该使用 Docker，因为它支持隔离测试。测试驱动开发是 CI/CD 的重要组成部分。经过良好测试的应用程序更有可能顺利交付给用户。对于部署应用的完全自动化的 CI/CD 管道，开发人员需要相信测试结果揭示了用户可能遇到的任何问题。我们进行隔离测试，以消除本地开发机器的影响。独立构建和测试的好处是，您可以放心地将自动通过所有测试的代码交付给您的用户。

## Docker 和 CircleCI

CircleCI 上的 Docker 是一流的体验。CircleCI 为各种编程语言和一些数据库维护了一批 Docker 映像，称为[便利映像](https://circleci.com/developer/images)。这些映像专门设计为在持续集成(CI)环境中运行良好。它们的存在是为了给用户提供一个快速方便的起点。我们还拥有强大的高级功能，如 [Docker 层缓存](/blog/config-best-practices-docker-layer-caching/)，以保持您的 Docker 映像构建尽可能快。

现在就开始使用 CircleCI 上的 Docker:

Docker 和工作流一起为构建过程增加了功能和定制。通过工作流，您的 VCS 提供商(例如 GitHub)将获得一个状态列表，每个作业一个状态。这样，更容易一眼看出故障发生在哪里，并且您可以直接导航到失败的作业。使用工作流保持快速高效的构建。

## 在 CircleCI 上构建 Docker 图像

为了帮助您最大限度地构建 Docker 图像，CircleCI 提供了对 Docker 层缓存的支持。CircleCI 将尝试重用在之前的作业或工作流程中构建的任何 Docker 图像(层)。在之前的作业中构建的每个未更改的层都可以在远程环境中访问。然而，有些情况下，即使配置指定了`docker_layer_caching: true`，您的作业也将在干净的环境中运行。这些情况涉及到为依赖于相同环境的相同项目运行许多并行作业。

有关更多信息，请查看[配置最佳实践:Docker 层缓存](https://circleci.com/blog/config-best-practices-docker-layer-caching/)和 [Docker 层缓存文档](https://circleci.com/docs/docker-layer-caching/)。