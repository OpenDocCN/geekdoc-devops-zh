# 通过头盔|安全带介绍代表安装

> 原文：<https://www.harness.io/blog/delegate-installation-via-helm>

本教程将向您展示如何在 Kubernetes 上安装您的代理。Helm 是在 Kubernetes 集群上安装和管理代理的好方法，因为包管理器通过交换 values.yaml 文件可以很容易地找到并安装正确的代理

对于来自其他平台的人来说，代理类似于自主持的跑步者。一旦安装了您的委托，它将帮助您在管道创建或执行过程中通过 Harness Manager 执行您请求的任何任务。

使用 Helm，本教程将向您展示如何在 Kubernetes 上安装您的代理。Helm 是在 Kubernetes 集群上安装和管理代理的好方法，因为包管理器通过交换 values.yaml 文件可以很容易地找到并安装正确的代理。

## 硬件要求

代理需要执行线束管理器分配给他们的计算任务。这使得代表有必要有一些资源需求。

您的 Kubernetes 集群应该为代理和剩余操作提供以下资源:

*   节点数量:2
*   vCPUs，内存，磁盘大小:2vCPUs，16 GB 内存，50GB 磁盘
*   联网:线束连接到 app.harness.io、github.com 和 hub.docker.com 的出站 HTTPS(443)
*   需要具有在目标名称空间中创建实体的权限的 Kubernetes 服务帐户。权限集应该包括列表、获取、创建和删除权限。一般来说，集群管理权限或管理权限就足够了。

# 安装舵

在你开始使用它之前，头盔应该被安装在本地。要进行测试，如果您有安装，请运行以下命令:

头盔版本

‍

如果下面没有结果，你必须安装头盔。要安装 Helm，请运行以下命令:

curl https://raw。githubusercontent。com/helm/helm/main/scripts/get-helm-3 | bash

‍

使用 ***helm version*** 命令测试安装。

## 安装代理

可以通过访问创建代理页面来安装代理。以下是到达那里的方法:

项目设置->委托->创建委托

选择 Kubernetes 作为您的安装空间是合适的，因为 Helm 是 Kubernetes 的软件包管理器。

指定代理名称，并根据您的要求选择大小。选择舵图点击 ***继续！***

‍

现在，您将看到一个文件，它是您的 values.yaml。该文件用作线束管理器和代理之间的链接。使用这个文件你需要安装你的代表，所以点击 ***下载 YAML 文件！***

现在，您可以使用以下命令安装您的委托:

舵报告添加线束 https://app . harness . io/storage/harness-download/harness-helm-charts/舵报告更新

‍

上述命令将帮助您将 Helm 存储库链接到您的计算机，以便您可以下载清单。要使用您下载的值应用这些清单，请使用以下命令:

helm install delegate-release harness/harness-delegate-ng-f harness-delegate-values . YAML-命名空间 harness-delegate

‍

***注意:确保你和你的 harness-delegate-values.yaml 在同一个目录下！***

成功大概是这样的:

恭喜你！您已经使用 Helm 成功创建了一个代理人！

## 后续步骤

完成委托安装后，您现在可以通过“创建管道”来创建您的第一个管道，并开始增强您的软件开发过程。

作为一个简单的演示，您可以使用[滚动更新策略](https://docs.harness.io/article/xsla71qg8t)在 Kubernetes 集群中部署一个公开的 NGINX Docker 映像和一个 Kubernetes 清单。

## 需要进一步的帮助吗？

欢迎在 [community.harness.io](https://community.harness.io/c/harness/7) 或[提出问题，加入社区 slack](https://join.slack.com/t/harnesscommunity/shared_invite/zt-y4hdqh7p-RVuEQyIl5Hcx4Ck8VCvzBw) 与我们的工程师在特定于产品的渠道中聊天，如:

[#连续交付](https://join.slack.com/t/harnesscommunity/shared_invite/zt-y4hdqh7p-RVuEQyIl5Hcx4Ck8VCvzBw)获得关于线束 CD 模块的支持。

[#持续集成](https://join.slack.com/t/harnesscommunity/shared_invite/zt-y4hdqh7p-RVuEQyIl5Hcx4Ck8VCvzBw)获得线束 CI 模块方面的支持。