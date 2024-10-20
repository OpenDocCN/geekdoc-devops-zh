# 线束 CI 和线束 CD -您的第一个真正的 CI/CD 管道|线束

> 原文：<https://www.harness.io/blog/harness-ci-and-harness-cd>

让我们看看利用 Harness CI / Drone 管道构建什么，然后利用 Harness CD 部署到 Kubernetes 集群。

Harness 现在允许您同时拥有同类最佳的[持续集成](https://harness.io/blog/what-is-continuous-integration/)和[持续交付](https://harness.io/blog/what-is-continuous-delivery/)能力。让我们看看利用 Harness CI / Drone 管道构建什么，然后利用 Harness CD 部署到 Kubernetes 集群。

本例的目标是让 Harness CI / Drone 构建一个 GoLang Docker 映像并将其推送到 DockerHub，然后让 Harness CD 拾取新工件并部署到一个等待的 Kubernetes 集群。这可能会让你比以往任何时候都更容易从想法到生产。

### 移动的零件

没有太多移动的部分来看一个端到端的例子。您需要一个 Harness CI / Drone 实例、一个 Docker Hub 帐户、一个 Harness 帐户，当然还有一个 Kubernetes 集群，它甚至可以是 [Minikube](https://community.harness.io/t/minikube-setup-with-harness/22) 。

像往常一样，你可以关注博客和/或观看视频。

[https://www.youtube.com/embed/xvPGHYaDsDw](https://www.youtube.com/embed/xvPGHYaDsDw)

视频

如果利用[示例 Harness CI / Drone GitHub 项目](https://github.com/ravilach/firstdrone)，您将不得不编辑信息，以作为您自己的 Docker 注册表，用于 Docker 推送的位置。我将在下面引用我的。

### 线束 CD 片

如果这是你第一次利用 Harness 平台，最快的开始方式是利用一个 [Kubernetes 委托](https://developer.harness.io/docs/first-gen/firstgen-platform/account/manage-delegates/install-kubernetes-delegate/)。

在 Kubernetes 集群中安装 Harness 委托的第一步非常简单。

设置->利用代理->下载代理-> Kubernetes YAML

给代理起一个名字。

点击提交下载。展开下载的 tar.gz。

在代理文件夹中，运行*ku bectl apply-f harness-delegate . YAML*来安装代理。

几分钟后，您的马具代表就可以使用了。

接下来，通过将 Kubernetes 集群添加为一个[云提供者](https://developer.harness.io/docs/first-gen/firstgen-platform/account/manage-connectors/add-kubernetes-cluster-cloud-provider/)，添加 Kubernetes 集群以供 Harness 部署。

设置->云提供商+添加云提供商

添加到永久群集

为 Kubernetes 集群指定一个名称，然后对于集群详细信息，从选定的委托[已部署的委托]继承。点击测试，然后提交，你就一切就绪了。

接下来，创建一个[线束应用程序](https://docs.harness.io/article/bucothemly-application-configuration)是一个简单的过程。

设置->添加应用程序。

应用程序的下一步是创建一个[服务](https://developer.harness.io/docs/category/add-services)。您可以通过访问以下网站来创建服务

设置- >金丝雀船长- >服务+添加服务。部署类型将是 Kubernetes。

在惊人的应用服务中，从 Docker 注册表中添加工件源

可以连接到您选择的 Docker 图像。

一旦您点击提交，脚手架将在那里部署，不需要额外的配置。

有了服务，我们可以连接一个[线束环境](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/environments/environment-configuration/)来部署。

设置->金丝雀船长->环境+添加环境

一旦点击提交，next 就可以连接一个[基础设施定义](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/environments/infrastructure-definitions/)。

接下来单击+ Add Infrastructure 并添加 Kubernetes 集群。云提供商类型和部署类型将是 Kubernetes。

一旦点击提交，就可以连接一个[线束工作流](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/workflows/workflow-configuration/)来定义要部署的步骤。

设置->金丝雀船长->工作流程+添加工作流程。工作流类型将是滚动部署。选择在前面的步骤中创建的环境、服务和基础设施定义。

接下来，您可以在 DockerHub 中出现新工件时创建一个[线束触发器](https://developer.harness.io/docs/category/triggers)。一旦 Docker 推送在 Harness CI / Drone 管道中完成，这将触发工作流。

设置->金丝雀船长->触发器+添加触发器

说出名字后点击下一步。DockerHub 可以发送一个 webhook，或者我们可以使用来自 Harness 的轮询间隔来寻找一个新的工件。不需要管理 webhooks，我们可以简单地配置新的工件。选择您的工件源，并可以过滤特定的标签。对于这个例子，我们只想部署获得的内容和"*。** “作为滤镜是可以的。

点击“下一步”并定义操作。执行类型将是工作流，并将执行在前面步骤中创建的工作流。可以利用上次收集的藏物。如果利用该示例，将使用相同的过滤器"*拾取最新的显式标签。** ”。

单击下一步并检查触发器，然后点击提交。

一旦你点击提交，你所要做的就是启动一个配置项/无人机构建和推送。

### 利用 CI /无人机步骤

如果你没有一个没有运行的线束配置项/无人机实例，不用担心，不难完成。我们有详细的[博客](https://harness.io/blog/your-first-harness-ci-installation/)和[视频](https://harness-1.wistia.com/medias/zm3np0ryea)让你开始。为了让这个例子工作，需要构建和推送一个 Docker 工件。[示例项目](https://github.com/ravilach/firstdrone)有一个简单的 GoLang 应用程序。线束 CI /无人机介绍[博客](https://harness.io/blog/your-first-harness-ci-installation/)和[视频](https://harness-1.wistia.com/medias/zm3np0ryea)通过线束 CI /无人机到达 GitHub 存储库。

要开始线束 CI / Drone 构建和推送，只需修改 configuration / Drone.yaml，即线束 CI / Drone 正在监视哪个线束。为了显式构建一个特定的版本，can [向 Drone.yaml](https://plugins.drone.io/drone-plugins/drone-docker/) 添加标签，这在本例中运行良好。

在示例中，推送构建版本 1.0.2。请确保更新您的存储库信息。

可以修改/增加，然后提交。

一旦你点击提交，线束 CI /无人机将接管。

发布步骤完成后，DockerHub 中将会出现一张新图片。

### 观看魔术

在轮询间隔内，Harness 将选择最新的构建/标记，并代表您开始部署。

随着部署的开始，您拥有了一个端到端的 [CI/CD 管道](https://harness.io/blog/ci-cd-pipeline//)。从代码到生产！

### 在您的 CI/CD 之旅中与 Harness 合作

无论您处于 CI/CD 之旅的哪个阶段，Harness 都可以简化和扩展您的能力。借助轻量级和基于约定的 Harness CI / Drone 以及 Harness 平台的强大功能，实现 CI/CD 涅槃变得前所未有的简单。立即开始使用[线束 CI /无人机](https://docs.drone.io/server/overview/)并注册一个[线束帐户](https://plugins.drone.io/plugins/docker)！

干杯，

拉维