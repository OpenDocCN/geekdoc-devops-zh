# 在 Google 容器引擎上运行 Kubernetes 的好处

> 原文：<https://www.fairwinds.com/blog/the-benefits-of-running-kubernetes-on-google-container-engine>

 所有迹象都表明 Kubernetes 正在成为容器编排平台的事实标准。假设您已经完成了研究，并且您知道您想要使用[【Kubernetes】](https://kubernetes.io/)作为您的容器管理系统。一切都好。你也知道你有两个主要的云托管选项:  [亚马逊网络服务(AWS)](https://aws.amazon.com/) 和  [谷歌云平台(GCP)](https://cloud.google.com/) 。亚马逊是显而易见的选择，因为许多开发人员对 GCP 和  [谷歌容器引擎(GKE)](https://cloud.google.com/container-engine/) 的接触和经验有限。

然而，如果你想在 AWS 上运行 Kubernetes，你还有很多工作要做。如果你正在寻找基于容器的工作流，GCP 将使 Kubernetes 更容易管理和更新。自从谷歌创建了 Kubernetes，GKE 基本上给你一个开箱即用的 Kubernetes 集群。换句话说，比起在 AWS 上建立 Kubernetes，有一个更好的选择:让建立 Kubernetes 的人也托管它。

## GKE vs. AWS 上的 kubernetes on gke vs . aws

让我们从在 GCP 上运行 Kubernetes 比在 AWS 上运行更好这个前提出发。一个合理的问题是为什么——有什么不同？

最简单的答案是，谷歌参与了 Kubernetes 的开发，因此谷歌自动支持 Kubernetes 的新功能，并且比其他云提供商更快——有时快得多。谷歌容器引擎(GKE)比其他云提供商更快地支持最新和最好的 Kubernetes 版本。

如果你知道你想使用 Kubernetes，那么你在 Google 上设置它的时间和金钱可能会比在 AWS 上少。让我们看看这实际上意味着什么。

***封闭 vs 开源***
[亚马逊 EC2 容器服务(ECS)](https://aws.amazon.com/ec2/) 基本上是亚马逊打造容器集群的尝试。它完全是闭源的，而 Kubernetes 是完全开源的。你可以看到谁在开发 Kubernetes(以及他们在开发什么)，并且你不会被一个云提供商(AWS 或 Google)所束缚。

***不常见的与常见的范例***
亚马逊已经自制了他们自己的容器集群软件，使用 ECS 时您需要构建的技能与其他平台上使用的不同。ECS 基本上是亚马逊其他服务的大杂烩，这意味着它既不健壮，也不是以一种易于在生产工作负载上使用的方式构建的。

您使用 Kubernetes 开发的技能可以应用于许多其他产品和系统，而您使用 ECS 构建的技能非常专业，不适用于您所做的大多数其他工作。因此，在 AWS 上运行 ECS 或 Kubernetes 将需要大量的人工工作，并且您需要经历相当多的考验。

***自动化与人工努力***
按照这些思路，如果你想使用 Kubernetes，GKE 大大缩短了学习曲线，因为谷歌为你设置了基线功能。使用 GKE，您可以在 10 分钟内启动并运行 Kubernetes 集群，而在 AWS 上，您必须做大量的工作来了解如何设置集群，使用什么工具以及如何在准备就绪时构建新的集群。您还必须对出现的任何问题进行故障排除，然后能够评估集群是否已经启动并按照您期望的方式运行。

GKE 消除了这些耗时的步骤。只要告诉 GKE 一些关于你的集群的基本情况，它就会自动引导你的集群。在实际创建集群的过程中，GKE 节省了大量的麻烦和时间，而且您实际上可以快速地在集群上部署应用程序，而无需一开始就保持集群运行的开销。

***简单与复杂集成***
AWS 没有像 GKE 那样的托管 Kubernetes 安装。另一方面，GKE 有谷歌的支持，并集成了谷歌的所有其他工具。它在主机和容器级别都有内置的日志记录、日志管理和监控功能。与 AWS 不同，它可以为您提供自动缩放、自动硬件管理和自动版本更新。与在 AWS 上手工构建相比，它通常为您提供一个包含更多电池的生产就绪型集群。

## AWS/GCP 的决定是你的

[集装箱化集群的社区支持](https://github.com/kubernetes/kubernetes) 正在库贝内特斯周边固化。因为 Kubernetes 和 GKE 都是在谷歌内部开发的，谷歌工程师比其他云服务更快(更好)地支持所有 Kubernetes 功能。亚马逊没有付钱给他们的工程师让 Kubernetes 在亚马逊上运行得更好。

通过 GKE，您可以获得一个生产就绪的 Kubernetes 集群，其中包含所有必要的工具，以及确保您的软件包和版本保持最新所需的持续支持。GKE 为你处理安全默认设置，并与其他谷歌服务整合。这种组合需要更少的集群管理开销，并使其在长期使用中更加无缝。

需要明确的是， [Fairwinds](/) 可以处理 AWS 和 GCP 上的 Kubernetes。这两种解决方案对我们来说都很好。然而，如果你有政治资本和选择的能力(例如，如果你还没有一家亚马逊商店)，GKE 会让你的生活更轻松。

简单来说，Kubernetes 在 GKE 上比在 AWS 上更好。在 GCP 上运行 Kubernetes 将会节省你的时间和金钱，并给你一个更短的学习曲线，更多的协同作用和更好的体验。