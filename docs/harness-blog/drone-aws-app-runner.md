# 无人机和 AWS 应用程序 Runner -一起更好| Harness

> 原文：<https://www.harness.io/blog/drone-aws-app-runner>

开始使用 Drone 来处理构建和发布部分是轻而易举的事情，然后允许 AWS App Runner 部署到 AWS 基础架构中。

随着越来越多的开发者资源转向公共云，亚马逊网络服务最近发布了 [AWS App Runner](https://aws.amazon.com/apprunner/) 。AWS App Runner 的前提是工程师可以只关注源代码，不需要具备底层基础设施的知识。AWS App Runner 似乎是 AWS 首次尝试提供针对 AWS 平台的[平台即服务](https://harness.io/blog/continuous-delivery/what-is-paas/) (PaaS)。

AWS App Runner 酷似[牧场主里约](https://harness.io/blog/continuous-delivery/rancher-rio-vs-harness/)；通过为部署创建可公开访问的路由，消除服务的网络/路由复杂性。开始使用 Drone 来处理构建和发布部分是轻而易举的事情，然后允许 AWS App Runner 部署到 AWS 基础架构中。

### 入门指南

有几个活动部件可以开始使用。第一个将是[需要安装一个无人机](https://www.youtube.com/watch?v=kZmOCLCpvmk)如果你没有进入[无人机安装](https://cd.foundation/blog/2020/08/20/your-first-drone-installation-and-build/)。在这个例子中，我们将使用 [GitHub](https://docs.drone.io/server/provider/github/) 作为源代码管理平台。接下来，将创建一个 [Amazon ECR](https://aws.amazon.com/ecr/) 注册表/存储库，用于存储最终将由 App Runner 部署的 Docker 映像。

### 亚马逊 ECR 配置

无人机有一个[插件，方便亚马逊 ECR 发布](http://plugins.drone.io/drone-plugins/drone-ecr/)。如果您以前没有创建过 Amazon ECR 注册中心，那么配置很简单。您可以登录您的 AWS 帐户，前往 ECR - >创建存储库。

在这个例子中，我们将给出私有的 ECR 注册中心，然后是库名“sample-apprunner-ecr”

当您点击提交时，私有 ECR 存储库就准备好了。

有几种方法可以让[以编程方式访问](https://docs.aws.amazon.com/AmazonECR/latest/userguide/registry_auth.html)您的私有 ECR 存储库，比如向注册中心认证。现成的无人机插件使用访问键。您可以使用创建 ECR 存储库的帐户的访问密钥，也可以创建一个专门的 IAM 帐户。对于这个例子，我创建了一个 Drone-ECR 帐户，并附加了一个 ECR 策略[也称为 Drone-ECR]。

AWS IAM ->创建策略，然后选择适当的权限。对于这个例子，我向新策略授予了所有权限。在你自己的环境中，你可以更恰当地锁定。

创建后，您可以通过 IAM 组将 IAM 策略附加到 IAM 用户。

如果这是一个新用户，请确保记下该用户的访问和密钥。随着 ECR 权限的取消，是时候去 GitHub 了。

### GitHub 项目

您可以[派生这个示例项目](https://github.com/ravilach/drone-ecr)，它将有一个无人机的示例配方发布到 ECR。Drone 确实需要实例化项目(例如，代表您创建一个 webhook 来监视更改)。

在 Drone 中导航到您想要构建的 GitHub 项目。点击激活存储库。

如果您正在利用示例项目，无人机配置称为“drone.yaml ”,您可以在设置的底部进行配置。

一旦你点击了 Save，你的 GitHub 项目应该有一个 webhook 安装到你的安装 URL:

现在，您可以更深入地了解无人机设置了。

### 无人机设置

如果利用[的例子](https://github.com/ravilach/drone-ecr)，有 5 个项目是敏感的，可以作为无人机秘密共享:AWS IAM 用户的秘密和密钥、ECR 存储库名称、ECR 注册地址和 ECR 区域。

Drone.yaml:

进入*库* - >设置- >秘笈，即可连线秘笈，创建五大秘笈。

需要创造的秘密及其价值:

*   aws_access_key:用户帐户的 IAM 访问密钥
*   aws_secret_key:用户帐户的 IAM 密钥
*   aws_ecr_repo : AWS ECR Repo 名称，例如“sample-apprunner-ecr”
*   aws_ecr_registry : AWS ECR 注册表地址例如" *your_unique_id。*dkr . ECR . us-east-2 . Amazon AWS . com "
*   aws_region : AWS 区域 ECR 位于例如“美国东部-2”中

每个都连接到:

为了模拟变化，您可以修改 drone.yaml 文件，例如更改标签并在 GitHub 中提交。这将启动一个构建。

构建应该是成功的。

成功:

您可以通过在 ECR 中查找新图像来验证发布步骤。

既然您已经能够轻松地构建和发布映像，下一步就是配置 AWS App Runner。

### 你的第一个 AWS 应用跑步者

配置 AWS App Runner 很简单，尤其是在遵循他们第一个发布的例子时。

第一步是在您的 AWS 帐户中导航到 AWS App Runner。

点击创建一个应用程序运行服务。然后，选择 Container Registry 和 Amazon ECR 作为提供商。

你可以点击浏览，选择无人机上传的图片。

单击 Continue，对于这个例子，只需利用一个手动触发器。

如果您没有合适的服务角色，向导可以为您创建一个通用角色。

单击“下一步”配置服务。GitHub repo 中的示例应用程序通过端口 3000 发送流量，您可以将该端口配置为 App Runner 中的端口。

*main.go*

您可以利用所有默认设置来部署这个“AwesomeApp”

单击创建和部署。该服务需要一些时间来创建和部署适当的基础设施。

通过点击默认域，您现在可以访问您的第一个 AWS App Runner 服务。

导航到 URL:

就这样，您拥有了一个已部署的容器，该容器带有一个可通过 AWS App Runner 公开访问的路径！

### Harness 和 AWS App Runner 更好地结合在一起

如果您需要对规定的 AWS 应用程序运行者意见进行更多的控制，将 Harness 添加到组合中是一个很好的选择，可以利用两者的功能。AWS App Runner 是 AWS 最新的类似 PaaS 的服务，旨在简化来自[弹性豆茎](https://aws.amazon.com/elasticbeanstalk/)的用户体验。并非所有工作负载都适合在 AWS App Runner 上运行。有了[驾驭平台](https://harness.io/platform/)，你可以自由构建和部署非常异构的技术。某些工作负载可以在其他 AWS 服务中表现得更好，例如 [Fargate](https://aws.amazon.com/fargate/) 或者只是一个普通的 [AMI](https://docs.harness.io/article/wfk9o0tsjb-aws-ami-deployments) /EC2 实例。无论你部署在哪里，安全带都会保护你。

干杯，

拉维