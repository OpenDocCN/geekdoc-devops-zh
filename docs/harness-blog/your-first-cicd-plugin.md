# 您的第一个 CI/CD 插件|线束

> 原文：<https://www.harness.io/blog/your-first-cicd-plugin>

了解如何利用 Harness 的 API 创建您的第一个 Harness CI / Drone 插件，以部署在 Harness CD 平台上。

Harness CI / Drone 如此灵活的原因之一是能够快速创作插件来实现目标。让我们开始制作一个插件，它将直接触发线束工作流程。如果这是您第一次查看 Harness 平台，那么完成即使是复杂的目标也是很容易的，并且是基于惯例的。

本例的目标是让 Harness CI / Drone 构建一个基于 Python 的插件，然后将 GoLang Docker 映像推送到 Docker Hub。一旦推送步骤结束，Harness CI / Drone 将直接调用 Harness CD，并将工件部署到一个等待的 Kubernetes 集群。从创意到生产，将你的开发技能提升到精英水平。

## 让我们开始——背景和方法

我们将在示例中完成一些移动部分。您需要一个 Harness CI / Drone 实例、一个 Docker Hub 帐户、一个 GitHub 帐户，当然还有一个 Kubernetes 集群，甚至可以是 [Minikube](https://community.harness.io/t/minikube-setup-with-harness/22) 。

像往常一样，你可以跟随博客和/或观看视频，作为一种特殊的待遇，甚至在开始之前看看最终产品。

Harness CI / Drone 有一个健壮的插件架构，允许几乎任何人创建和利用插件。[利用 CI / Drone 插件](https://docs.drone.io/plugins/overview/)是预定义的任务，它们被配置为 [CI/CD 管道](https://harness.io/blog/ci-cd-pipeline/)中的步骤。在本例中，我们将在 Python 中创建一个 Harness CI / Drone 插件，该插件将调用 Harness CD API 来触发 Harness 工作流。

如果利用[示例 Harness CI / Drone GitHub 项目](https://github.com/ravilach/firstdrone)，您将不得不编辑信息，以作为您自己的 Docker 注册表，用于 Docker 推送的位置。我将在下面引用我的。

如果利用[示例](https://github.com/ravilach/firstdrone)，我们将假设我们知道最终想要部署的工件的名称。如果您自带，可以修改线束 CD 步骤以匹配您自己的工件。

### 线束 CD 准备

如果这是你第一次使用 Harness 平台，最快的入门方式是利用一个 [Kubernetes 委托](https://ngdocs.harness.io/article/f9bd10b3nj-install-a-kubernetes-delegate)。

在 Kubernetes 集群中安装 Harness 委托的第一步非常简单。一旦签约/签约，可以安装一个线束代理。

设置->线束代理->安装代理。然后选择 Kubernetes YAML，并给出一个名称，如“cicd”。

下载完成后，我们可以扩展 TAR 并安装 Harness Kubernetes 委托。安装说明将在展开文件夹内的 *README.txt* 中。

要安装 Kubernetes Delegate，运行*kube CTL apply-f harness-Delegate . YAML*。

几分钟后，您的线束代理将准备就绪。您可以通过返回到“设置”->“利用代理”并在列表上看到您的新代理来进行验证。

接下来，可以通过添加 Kubernetes 集群作为云提供商来连接 Kubernetes 集群，以便进行部署。

设置->云提供商+添加云提供商-> Kubernetes 集群。

由于 Harness 委托部署在集群上，我们可以很容易地从委托本身继承细节。将集群命名为“CICD·库伯内特集群”，点击 Test，然后点击 Next，然后提交。

Kubernetes 集群连接到 Harness 后，下一步是开始定义 Harness 将执行的工作流。Harness 在一个抽象模型上工作，在这个模型中，您定义了完成工作流所需的部分。第一步是创建一个 Harness 应用程序，它是所有相关抽象部分的家。

设置->应用程序+添加应用程序。可以将这个线束应用程序命名为“DevOps Dojo”。

一旦创建了 Harness 应用程序，就可以开始构建抽象模型了。线束服务是一个很好的起点。

设置-> DevOps Dojo ->服务+添加服务。可以把马具服务命名为“惊艳 App”。部署类型将是 Kubernetes。

在惊人的应用程序服务中，从 Docker 注册表中+添加工件源。

在这里，您可以将我们的[示例应用程序/项目](https://github.com/ravilach/firstdrone)或您自己创建的应用程序/项目连接在一起。如果您没有想要构建的现有工件，可以在稍后的线束配置项/无人机配置期间返回到此步骤。我在 public Docker Hub 上有一个名为“ [rlachhman/amazingapp](https://hub.docker.com/repository/docker/rlachhman/amazingapp) ”的应用程序。

一旦您点击提交，脚手架将在那里部署，不需要额外的配置。

有了服务，我们就可以连接一个部署到其中的环境。

setup-> devo PS Dojo-> Environments+添加名为“最伟大的环境”的环境。

点击 Submit 后，next 可以连接一个基础设施定义来定义要部署到的环境。

接下来，单击+ Add Infrastructure 并添加 Kubernetes 集群。云提供商类型和部署类型将是 Kubernetes。能给出“最伟大的 K8s 星团”这个名字。

点击“提交”后，可以将线束工作流连接在一起，以定义要部署的步骤。

设置-> DevOps Dojo ->工作流+添加工作流。工作流类型将是滚动部署。选择在前面的步骤中创建的环境、服务和基础设施定义。命名为“部署惊人的应用程序”

点击提交后，工作流概述应该如下所示。

最后，让我们创建一个 Harness 触发器，这样我们的 Harness CI / Drone 插件就有了一个执行端点。线束触发器将执行刚刚创建的工作流。

Setup -> DevOps Dojo-> Triggers +添加名为“Web Deploy Trigger”的触发器。

单击“下一步”后，条件将是一个 Webhook 事件，我们将从将要创建的 Harness CI / Drone 插件中生成该事件。

单击“下一步”后，操作将是执行刚刚创建的线束工作流。在 webhook 的有效负载中将定义版本/标签。

单击“下一步”后，我们可以验证线束触发器配置。

当您单击 Submit 时，将会创建触发器并公开您需要的有效负载信息。

我们可以通过单击 Manual Trigger 来获取有效负载信息，我们将为其构建 Harness CI / Drone 插件。

最后，我们可以设置一个 [API 键](https://docs.harness.io/article/smloyragsm-api-keys)来认证对线束触发器的远程调用。要创建 API 密钥，请转到安全- >访问管理- > API 密钥+添加 API 密钥。可以将 API 密钥命名为“线束 CI /无人机 API 密钥”

点击提交后，您可以查看屏蔽的密钥。

随着 Harness CD 的发布，是时候看看您可以/将要构建什么来部署了，然后看看如何创建一个插件来触发部署。

### 创建要部署的应用程序

如果你没有一个没有运行的线束配置项/无人机实例，不用担心，不难完成。我们有详细的[博客](https://cd.foundation/blog/2020/08/20/your-first-drone-installation-and-build/)和[视频](https://harness-1.wistia.com/medias/zm3np0ryea)让你开始。我们将把 GitHub 项目一分为二。一个将用于创建利用 CI /Drone 来利用 CD 触发器插件，另一个将是一个更典型的项目，发布一个工件并利用您新创建的插件。

如果您没有要部署的现有应用程序，并且想要在 Docker 注册表中植入一个，那么您可以在[示例存储库](https://github.com/ravilach/firstdrone)中修改几行。将存储库放入您自己的 GitHub，并对 [Drone.yaml](https://github.com/ravilach/firstdrone/blob/master/Drone.yaml) 做一些修改。如果您这样做，请确保针对您的特定存储库和凭证遵循[激活 Harness CI / Drone](https://harness.io/blog/your-first-harness-ci-installation/) 步骤。

要开始初始的 Harness CI / Drone 构建和推送，只需修改配置/ Drone.yaml。要显式构建到特定版本，可以[向 Drone.yaml](https://plugins.drone.io/drone-plugins/drone-docker/) 添加标签，这在本例中运行良好。在示例中，让我们从推送构建版本 1.0.1 开始。请确保更新您的存储库信息。

一旦对 Drone.yaml 进行了更改，就可以开始自动化部署了。

当发布步骤完成时，工件的新版本将会出现在 Docker 注册表中。

有了创建工件的能力和线束 CD 连线，现在是时候创建您的第一个线束 CI /无人机插件了。

### 线束配置项/无人机插件创建

[示例插件](https://github.com/ravilach/drone-harness-trigger/blob/master/harness-trigger-plugin.py)将是一个 Dockerized Python 应用程序，它将制定一个调用 Harness CD API 进行部署的请求。如果您是编写 Python 的新手，那么一个很好的 Python 开发环境是 [Pycharm](https://www.jetbrains.com/pycharm/) 。

有几种方法可以调用 Harness API，例如，您可以使用 graph QL API——但是在我的例子中，我使用 JSON 利用了 CURL like payload。

在您的 YAML 配置和插件本身之间传递项目的主要机制可以在文档中的 [GoLang 示例中描述。在引用插件的 YAML 配置的“设置”部分，获取“PLUGIN_FIELD”后面的环境变量是两者之间的连接。](https://docs.drone.io/plugins/golang/)

查看已完成的[示例插件项目](https://github.com/ravilach/drone-harness-trigger)，其结构与您正在编写一个定制应用程序并利用 Harness CI / Drone 进行部署几乎相同。在插件 [Drone.yaml](https://github.com/ravilach/drone-harness-trigger/blob/master/Drone.yaml) 中，我们很容易地切换到 Python 构建，并将我们新创建的工件发布到一个新位置。

一旦连接到存储库，进行修改或最初提交您的线束 CI /无人机 YAML 将开始构建。

插件已经准备好了。

插件准备就绪后，您现在可以开始在其他项目中利用该插件了。

### 配置您的第一个插件

在你投入插件创作的所有工作之后，是时候享受胜利的果实了。对于这个特定于线束的示例，有几个字段必须通过插件连接。您可以决定在 YAML vs 中使用 Harness CI / Drone secrets 设施暴露什么是有意义的。包含诸如版本/标签 ID 之类的项目是安全的，但是大多数特定于帐户的项目应该作为秘密来引用。

包含大部分细节的手动触发窗格的提醒。

利用 CD ->设置-> DevOps Dojo ->触发器-> Web Deploy 触发器，然后单击手动触发器。

在此字段中可以找到 Account _ ID(在线束 CD 中)、您的线束帐户 ID(在您的帐户的 URL 中)或线束触发器的手动触发器视图。API _ 尹柯线束光盘，位于安全->访问管理-> API 密钥 KeysApplication _ IDIn 线束光盘中，线束触发器的手动触发器视图中。Artifact _ VersionEither 由您想要部署的内容定义。通常在您的 Docker 注册表或线束 CI /无人机配置中。Harness _ Webhook _ IDIn 在线束 CD 中，线束触发器的手动触发器视图中。harness _ Service _ name 您要部署到的服务。在这个例子中是“惊人的应用”。Harness _ Artifact _ NameThe 为您的服务指定的工件名称。在这个例子中是“rlachhman_amazingapp ”,表示存储库和映像。

有了谨慎的细节，你可以开始[在脚手架](https://github.com/ravilach/drone-harness-trigger/blob/master/plug-in-snippet.yaml)中构建插件将如何执行。

有了合适的脚手架，您现在可以开始将秘密连接到您希望构建和发布的存储库中。

有了这些秘密，剩下的就是将脚手架包含到现有的或样例线束 CI /无人机管道中。

### 执行您的第一个管道

得出这个例子的结论。我们现在需要做的就是在一个线束 CI /无人机管道的末尾添加脚手架/插件配置。将利用产生 1.0.1 版神奇应用程序的同一管道。让新开发的插件负责部署 1.0.2 版本。

承诺，你就可以去比赛了！

通过示例，可以在 Harness CD 平台中验证您的部署已经启动。

## 与马具搭档

无论您处于 CI/CD 之旅的哪个阶段，Harness 都可以简化和扩展您的能力。借助轻量级和基于约定的 Harness CI / Drone 以及 Harness 平台的强大功能，实现 CI/CD 涅槃变得前所未有的简单。立即开始使用[线束 CI /无人机](https://harness.io/products/continuous-integration/)，并注册一个[线束账户](https://app.harness.io/auth/#/signup/?module=ci)。如果你创建了一个很酷的插件，不要羞于回报！