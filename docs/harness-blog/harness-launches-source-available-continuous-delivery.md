# 线束启动来源可用连续交货|线束

> 原文：<https://www.harness.io/blog/harness-launches-source-available-continuous-delivery>

线束连续交付现已上市！加入我们的简短教程，了解如何下载和使用 CD Community Edition。

没有繁文缛节。没有进入壁垒。

随着公司转向 Kubernetes 和微服务，Harness 创建了第一个持续交付即服务解决方案来应对现代软件交付。Harness 通过抽象化新技术的复杂性，帮助企业和小型企业更快、更自信地部署。现在，我们通过一个可用的源代码许可证，将我们世界级的 CD 管道带给用户。我们的目标是消除阻碍开发者体验我们平台的障碍。

大型企业的开发人员为了获得开始使用 Harness CD 的许可，需要面对数小时的安全审查、高管会议和内部协商，他们可以绕过官僚主义，直接进行部署。

小型公司中需要部署软件而不需要签订企业合同的团队可以在大约 15 分钟内构建和部署他们的第一个管道。

想要在自己的机器上尝试最新最棒的软件的修补者有一个简单的方法来评估 Harness CD。

Harness CD Community Edition 是在一个名为 Polyform Shield License 的源可用许可证下提供的，该许可证可供大多数用户免费使用。我们有开源技术，比如无人机，并计划在未来通过开源分享更多代码。如果你想了解更多关于我们开源承诺的信息，请访问我们的[网站](https://harness.io/open-source/)。

**线束光盘社区版**

Harness CD 社区版加入了我们持续交付产品的现有企业版。第一次迭代只适用于 Kubernetes(在任何平台上)，在不久的将来，AWS (EC2，ECS，ASG，CloudFormation，Lambda 等。)，Azure (VMSS，WebApps，AKS，ACR，ARM，Blueprint)，。NET、Google Cloud Build、VMware Tanzu 和无服务器部署将得到[支持](https://docs.harness.io/category/1qtels4t8p-cd-category)。用户将可以利用 CD 的主要功能，如自动化金丝雀&蓝绿部署、自动化基础设施供应、集成审批&通知流程以及开发人员友好的管道代码体验。Harness CD Community Edition 还带有内置的用户和机密管理以及内置的批准工作流。

Harness CD Community Edition 可以下载并在您的笔记本电脑或具有 3 GB RAM 和 2 个 CPU 的虚拟机上运行。下载后，简化的快速入门指南将帮助您在 15 分钟内创建和部署管道。Harness CD Community Edition 用户可以随时升级到付费计划。

**如何安装、构建和部署管道**

Harness CD 社区版可以在 GitHub 上找到[。](https://github.com/harness/harness-cd-community)

在使用 Harness CD Community Edition 之前，你需要安装 Docker Desktop 或者安装 Kubernetes 集群，在那里你可以安装一个[舵图](https://github.com/harness/harness-cd-community/tree/main/helm)。对于这篇文章，我们将使用 [Docker 桌面](https://github.com/harness/harness-cd-community/tree/main/docker-compose/harness)选项。在 Docker 桌面的 Resources 选项卡中，将内存增加 3 GB RAM，将 CPU 增加 2 个。安装 Harness CD 后，您可以将微服务部署到任何 Kubernetes 集群，只要您拥有到该集群的网络连接。最简单的方法是启用 Docker Desktop 自带的内置 Kubernetes 集群，同时确保为该集群分配额外的内存和 CPU 资源。

完成先决条件后，您就可以安装 Harness CD Community Edition。运行以下命令将 Harness Git repo 的内容克隆到您的本地目录(docker-compose 文件夹)中。

git 克隆 https://github.com/harness/harness-cd-communitycd 线束-CD-社区/docker-撰写/线束

然后运行下面的命令，下载所有需要的线束图像并打开容器。

docker-撰写向上-d

接下来，您需要在这里创建一个帐户:[http://localhost/#/sign up](http://localhost/#/signup)。现在你上路了！线束向导将帮助您建立一个项目，建立一个管道，并完成您的第一次部署。

您还可以遵循[Harness CD Community Edition quick start 文档](https://ngdocs.harness.io/article/ltvkgcwpum-harness-community-edition-quickstart)，该文档向您展示了如何使用标准滚动部署策略将 nginx 服务部署到您的 Kubernetes 集群上。您可以运行这个管道，并看到类似于下面所示的部署。

## **刚刚开始**

这是我们第一次正式发布 Harness CD Community Edition，也是我们第一次采用 SaaS 产品的子集来创建您可以自己运行的软件。SaaS 和软件是完全不同的媒介，所以我们希望得到您的反馈！您可以在我们的[社区页面](https://community.harness.io/)上给我们反馈或问我们任何问题。

[加入我们的](https://www.meetup.com/harness/events/283055848/) live，了解更多关于 Harness CD Community Edition 的信息。

你可以在 GitHub 上找到 Harness CD 社区版[的链接。在 15 分钟内下载并部署，看看如何在新的一年里提升持续交付！](https://github.com/harness/harness-cd-community)