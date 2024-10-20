# 使用代表优先的 Approach‍ |装具，在 5 分钟内开始部署

> 原文：<https://www.harness.io/blog/deploy-in-5-minutes-with-a-delegate-first-approach>

我们很高兴地宣布，有一个新的委托优先的方法来管理管道。如果您刚刚开始使用 Harness 平台，您会发现这种方法有助于快速入门。

我们很高兴地宣布，有一个新的委托优先的方法来管理管道。如果您刚刚开始使用 Harness 平台，您会发现这种方法有助于快速入门。现在，对于任何尝试 Harness 的人来说，入职流程都更加简单和流畅。

在您注册了 Harness 之后，这个面向初学者的教程将指导您完成在 Kubernetes 集群上设置代理的过程。

首先，让我们从解释为什么委托是一个必要的和基本的服务开始。

## 为什么要设置委派？

设置 Harness 首先要在您的目标基础设施(如 Kubernetes 集群、ECS 集群、EC2 子网或 Pivotal Cloud Foundry 空间)中安装至少一个 Harness 委托。

对于来自其他平台的人来说，代理类似于自主持的跑步者。一旦安装了您的委托，它将帮助您在管道创建或执行过程中通过 Harness Manager 执行您请求的任何任务。这些任务包括但不限于:

*   通过连接器连接到 GitHub 或 Gitlab，获取存储库并在需要时进行更改
*   通过连接器连接到 DockerHub、JFrog 等工厂
*   连接到您的云提供商，如 GCP、Azure、AWS 等
*   向您的验证提供者和机密提供者验证和认证
*   作为持续集成管道的一部分运行代码测试和构建操作
*   将工件作为连续交付管道的一部分部署到您连接的基础设施中
*   当事情出错或成功时，便于通知 Slack、吉拉和其他应用程序

因此，您可以看到，委托是 Harness 中任何操作的中心，为了利用上述特性，您的基础设施中需要一个委托。我们将从创建一名代表开始，但首先，您需要[注册使用](https://app.harness.io/auth/#/signup)并满足以下先决条件:

*   运行中的 Kubernetes 集群
*   在您的机器上安装并验证了 kubectl CLI

## 硬件要求

您的 Kubernetes 集群应该为代理和剩余操作提供以下资源:

*   **节点数** : 2
*   **vcpu，内存，磁盘大小**:2 vcpu，16 GB 内存，50GB 磁盘
*   **联网**:线束连接到 app.harness.io、github.com、hub.docker.com 的出站 HTTPS(443)
*   需要具有在目标名称空间中创建实体的权限的 Kubernetes 服务帐户。权限集应该包括列表、获取、创建和删除权限。一般来说，集群管理权限或管理权限就足够了。

## 创建代理

假设满足上述要求，并且您已经注册了 Harness 平台，您将从入门页面开始:

您可能还记得，安装代表是第一步，所以点击**安装代表**按钮，然后选择代表类型。本教程将侧重于安装 Kubernetes 代表，所以点击 **Kubernetes** 按钮，如下所示，以生成您的 YAML 清单。

点击 Kubernetes 后，Harness 将为您创建一个定制清单和一些说明，所以让我们来完成每个步骤。

第一步。将包含 Kubernetes 对象定义的 YAML 清单下载到本地机器上。要查看清单，您可以点击**预览 YAML** ，或者在文本或代码编辑器中打开下载的文件。

第二步。将您的清单应用到您的集群，以创建您的 Kubernetes 对象，并使您的代理能够从 Harness Manager 接受作业。您可以点击**检查您是否可以连接到您的集群**，了解如何获得您的的详细说明。本地的 kubeconfig 文件。

要安装，导航到通过终端下载您的代理文件的目录，并使用命令“ku bectl apply-f harness-Delegate . yml”开始创建您的代理。成功的输出将如下所示:

第三步。代理安装完成后，您将收到一条成功消息。这可能需要 5-10 分钟。当您看到绿色框时，恭喜您，您的代理安装完成！

‍

要更深入地了解您的代理创建的状态，您可以在您的终端上运行命令“kubectl get pods-n harness-Delegate-ng ”,并检查您的 Pods 的状态。

如果您运行命令来查看 Pod 状态，那么在您的终端上将会成功显示如下:

## 后续步骤

完成委托安装后，您现在可以通过“创建管道”来创建您的第一个管道，并开始增强您的软件开发过程。

要创建一个项目，用任何名称填充 name 字段，然后通过易于使用的可视化编辑器继续创建管道。作为一个简单的演示，您可以使用[滚动更新策略](https://docs.harness.io/article/xsla71qg8t)在 Kubernetes 集群中部署一个公开的 NGINX Docker 映像和一个 Kubernetes 清单。

要了解更多关于代理的信息，您可以浏览[代理文档](https://docs.harness.io/article/2k7lnc7lvl-delegates-overview)。

不要忘记[请求您的个性化线束演示](https://harness.io/demo/)或[下载免费试用](https://app.harness.io/auth/#/signup/?module=cd)。如果你有任何问题，[加入我们的社区松弛频道](https://join.slack.com/t/harnesscommunity/shared_invite/zt-1fpgkrr4j-0fEENRq7Ekg8ytD7Sz3WsQ)，让你的问题得到解答！我们总是乐意帮忙。