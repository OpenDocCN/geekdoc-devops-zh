# 关于 Kubernetes 基础知识、工具和更多内容的问答

> 原文：<https://www.fairwinds.com/blog/kubernetes-basics-tooling-and-more>

 最近我回答了 Kubernetes 社区提交的问题。这里是围绕工具、Kubernetes 基础知识和托管容器服务讨论的问题和答案的快速总结。

## **Kubernetes 工装**

### 头盔有什么好处，是否需要？

Helm 是 CNCF 的一个孵化器项目，由 Helm 社区维护。Helm Charts 允许您定义、安装和升级 Kubernetes 应用程序。11 月发布的最新版本 Helm 3.0.0 在许多方面都有所改进。虽然不一定需要，但它很受欢迎，因为它使管理 Kubernetes 上的应用程序变得更加容易。

没有 Helm，您将需要模板化您的清单，并跟踪从一个部署到另一个部署的变化。使用 Helm，您可以创建一个图表，然后释放该图表。Helm 会跟踪每个版本的变化。当您单独使用 Helm 进行部署时，如果部署出错，Helm 会提供回滚命令，以便您可以快速返回到工作版本。

Helm 的部分好处是能够使用 nginx-ingress、cert-manager、external-dns 等社区图表。如果您创建了一个图表，您可以与全世界共享它。

### **除了 Jenkins Blue Ocean 之外，使用 ArgoCD 的人还可以通过哪些方式实施“部署到阶段- >测试- >部署到生产”管道？**

CI/CD 遵循类似的过程，与您使用的工具无关。您需要建立实现管道，并需要在 Jenkins vs. GitLab vs. CircleCI 之间做出决定。我们使用 [rok8s-scripts](https://github.com/FairwindsOps/rok8s-scripts) ，这是一个开源框架，用于使用 Docker 和 Kubernetes 构建 GitOps 工作流。通过将 rok8s 脚本添加到您的 CI/CD 管道中，您可以使用 Kubernetes 的最佳实践来构建、推送和部署您的应用程序。

### **是否推荐 Terraform 降低成本，加快开发速度？**

当然，Terraform 允许团队像对待代码一样对待他们的基础设施，从而加快了开发速度。Terraform 具有声明性和可读性，降低了准入门槛。它与云无关，支持几十种不同的 API。这允许团队根据他们的应用混合和匹配你的开发。

## **Kubernetes 基础知识**

### **为什么是库柏人？**

虽然我们可能会在这个话题上花上几个小时，但这里有一些主要的好处:

*   没有供应商锁定
*   可再现性——您可以将一个应用程序部署到一个经过认证的 Kubernetes 集群，然后将其部署到任何 K8S 集群
*   这个社区非常庞大，而且还在增长。去年，我们看到了很多成熟和支持。
*   与自己构建框架相比，它为滚动发布和更新提供了一个通用框架

### 为了开发目的在本地运行 Kubernetes 有什么好处？

有了本地机器上的 Kubernetes，你可以 a)免费运行它，b)快速测试变化。无论您在本地机器上做什么，您都能够跨集群进行复制。

### 我的开发人员需要了解 Kubernetes 吗？

虽然理解基础知识很重要，但是如果你在管道中保留尽可能多的跑腿工作，那么你的开发人员将只需要知道基本的`kubectl` 命令。你维护管道，开发者发布代码。

### **Kubernetes 对我团队的应用来说太复杂了吗？**

像任何新技术一样，Kubernetes 在你第一次看到它时是复杂的。对于组织来说，这可能是一个重大的范式转变。然而，一旦部署和监控流程到位，好处是显而易见的。虽然肯定有一个学习曲线，但克服这个曲线的最好方法是分批使用 Kubernetes。首先建立一个集群，然后部署一个小型应用程序，然后开始构建自动化。你也可以与 Kubernetes 咨询服务公司合作，让你的团队跟上进度。

### **我想了解 Kubernetes，从哪里开始？**

首先，确保您了解如何构建和运行 Docker 容器。一旦你熟悉了，就从 Kubernetes 文档开始。这是一个从零到应用部署的好方法。此外，如果你关心 Kubernetes 的来龙去脉，读一读凯尔西·海塔尔的《艰难的 Kubernetes》。

## **Kubernetes 和托管集装箱服务**

### 【Kubernetes、微服务和亚马逊 EKS 是什么关系？

微服务在 Kubernetes 内部运行(尽管 Kubernetes 并不专门用于微服务)。Kubernetes 几乎可以在任何地方运行。亚马逊 EKS 是一个托管的 Kubernetes 服务(以及 GKE 和 AKS)。EKS 在创建基本集群方面做得很好。EKS 的好处是亚马逊管理 Kubernetes 控制平面，因此运营商只需担心 EC2 计算实例。为了投入生产，您仍然需要[单独管理集群附加组件和监控，](https://www.fairwinds.com/clusterops)不考虑 Kubenetes 托管提供商。

### 我目前正在 AWS ECS 上运行我的应用程序。采用 Kubernetes 的论据是什么？

如果您正在使用 ECS，并且您的应用程序没有任何问题，请不要移动。但是，如果你在 Kubernetes 中看到一个用 ECS 做不到的功能，那就开始评估吧。

采用 Kubernetes 有两个主要的理由，适用于任何容器服务。首先，如果您希望更好地管理您的服务，其次，如果您希望通过(更)自由地移动您的应用程序来避免供应商(ECS/AWS)锁定。

此外，采用 Kubernetes 的一个主要好处是社区。Kubernetes 在每个服务提供商或本地运行。有了 ECS，你只能在亚马逊上运行你的应用。你没有得到社区清单，舵轮图，K8S 自动化等的好处。

### 关于 EKS 的 Kubernetes，我需要了解什么？

EKS 抽象了 Kubernetes API 和控制平面，因此运营商不需要太担心维护集群正常运行时间。然而，有一些小的权衡。EKS 不遵循官方的 Kubernetes 发布周期，所以你通常会比最新版本落后几个版本。您也将无法配置控制平面(打开/关闭 Kubernetes API 特性)。所有 Kubernetes 服务提供商(EKS、GKE、AKS 等)都是如此。EKS 非常擅长处理基本的集群部署，但是当您转向生产时，您将不得不管理 Kubernetes 附加组件，如监控、日志记录和入口控制器。这是费尔温斯的面包和黄油。我们帮助 [EKS 用户管理他们的基础设施](https://www.fairwinds.com/clusterops)，这样他们就可以专注于运行他们的应用程序。

你可以听我们接下来的问答环节。我们涵盖更多问题，包括:

*   如果部署在 Kubernetes 上，解决方案是否符合 HIPAA？
*   Kubernetes 没有解决哪些问题？
*   您如何从容器中收集指标(应用程序和操作系统)？
*   您如何处理非生产集群/环境？

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)