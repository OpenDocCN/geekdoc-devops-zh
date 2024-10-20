# 聚焦 Karpenter、Goldilocks 和开源集群自动扩展

> 原文：<https://www.fairwinds.com/blog/spotlight-on-goldilocks-karpenter-and-autoscaling-with-open-source>

 在 Fairwinds，我们致力于增强我们的开源社区。通过我们自己的工具，我们努力工作来回馈这个有价值的群体，为容器化的未来构建高质量的开源项目。在开放的环境中进行精心的组装也使我们能够与 Kubernetes 社区中的人们进行交流，从业余爱好者到企业，从而使工具更加强大、可靠和功能丰富。因此，这种开源软件使我们的客户和社区能够在 Kubernetes 生命周期的每个阶段做出更好的决策。

即便如此，在 Kubernetes 采用和实施的道路上，某些挑战仍然存在，包括需要“恰到好处地”获得资源请求和限制，在利用集群自动伸缩的集群中提供稳定性和效率。

## 定义的聚类自动缩放

使用 Kubernetes 的好处之一是能够根据用户需求动态扩展基础设施。集群自动缩放是一个增加或减少 Kubernetes 集群大小的组件。这是通过添加或删除工作节点来实现的，具体取决于挂起 pod 的存在。迄今为止，Kubernetes 中最常用的自动缩放器是 [Kubernetes 集群自动缩放器](https://github.com/kubernetes/autoscaler) 。

如果您有太多的资源，集群自动缩放器(CA)可以删除工作节点并节省资金。根据您的组织需求，将您的集群规模降至零也是可能的。CA 是一个行业采用的、开源的、厂商中立的工具。它是 Kubernetes 项目的一部分，由大多数主要的 Kubernetes 云提供商实施。最近，Karpenter 项目进入了这个领域，其目标是解决集群自动缩放器的一些限制。

在考虑 CA 和 Karpenter 之间的区别之前，了解一下弹性 Kubernetes 服务(EKS)是很重要的，这是亚马逊在 AWS 云中对 Kubernetes 的实现。集群由一个“控制平面”和一组称为节点的机器组成。在 EKS 中创建集群时，会创建一个控制平面和一个自动扩展组。自动缩放组中的 EC2 实例使用预定义的映像，启动脚本将实例加入集群。最近几个月，AWS 还引入了托管节点组的概念，AWS 将自动管理自动扩展组。

## 见见卡本特

寻找有效自动缩放的人们将很快有更多的工作要做。Karpenter 是一款开源软件，现在已经获得了 Apache License 2.0 的许可。作为设计用于在 AWS 中运行的 Kubernetes 集群的软件，Karpenter 观察未调度 pod 的聚合资源请求，并做出启动和终止节点的决策，以最大限度地减少调度延迟和基础设施成本。

Karpenter 在合适的时间用合适的节点简化了 Kubernetes 的基础设施。作为一种能力，这种“适用于任何 Kubernetes 集群的即时节点”是有价值的，因为它启动了理想的计算资源来处理集群的应用程序。通过这种方式，Karpenter 旨在让您通过快速简单的 K8s 集群计算资源调配来充分利用云。卡本特也:

*通过快速、自动地响应应用程序负载、时间安排和资源需求的变化，提高应用程序可用性*。这种能力将新的工作负载置于各种可用的计算资源上。

*最大限度地减少运营开销*在单个声明式供应器资源中提供一组自以为是的默认值，可轻松定制；不需要额外的配置！

在集群中安装 Karpenter 后，可以观察 Kubernetes 集群中的事件，并将命令发送到底层云提供商的计算机服务，如 Amazon EC2。这意味着 Karpenter 可以观察未调度 pod 的聚合资源请求，并启动新节点(或终止它们)以减少调度延迟和基础架构成本。

Karpenter 的一个主要区别是它能够直接对 EC2 进行 api 调用。Karpenter 可以做出所有最优的 EC2 实例选择来满足所有常量，而不是利用需要一组预定义实例类型的自动缩放组。只需 60 秒就可以配置一个新节点。如果您删除了一个节点，Karpenter 会很好地处理所有的节点退役，包括封锁和清空节点以及关闭相应的实例。

[还在迷茫？看看这个关于 Karpenter 如何工作的高度可视化的解释！](https://www.youtube.com/watch?v=3QsVRHVdOnM)

## 让金发女孩“恰到好处”

当使用像 Karpenter 这样的自动缩放器时，获得正确的资源请求和限制变得更加重要。如果您的请求太高，Karpenter autoscaler 可能会添加远远超过您在集群中实际需要的节点，这将导致更高的成本。如果它们太低，那么您可能会在较小的节点上遇到资源争用问题。

Goldilocks 为调整您的资源请求和限制提供了一个解决方案。作为我们在 Fairwinds Insights 中用来优化 Kubernetes 工作负载的开源项目，Goldilocks 消除了对 Kubernetes 生产部署中运行的应用程序设置资源请求和限制的猜测。它帮助工程师确定资源请求和限制的起点，同时确保应用程序正确运行。

要查看每个应用程序的资源请求建议，垂直窗格自动缩放器(VPA)效果很好。Goldilocks 为名称空间中的每个部署创建一个 VPA，然后向它们查询信息。虽然 VPA 可以设置资源请求，但 Goldilocks 中的仪表板可以方便地查看所有建议，并根据您组织的 Kubernetes 环境做出决策。

Goldilocks 使用 VPA 中的推荐器生成推荐。事实上，金凤花开源软件完全基于底层的 VPA 项目，特别是推荐者。我们发现 Goldilocks 是设置您的资源请求和限制的一个很好的起点。但是鉴于每个环境都是不同的，Goldilocks 不应该被视为针对特定用例调整应用程序的替代品。

## 贡献给金发女孩

[Goldilocks 是开源的，可以在 GitHub](https://github.com/FairwindsOps/goldilocks) 上获得。我们继续进行大量的改进，提高其处理具有数百个名称空间和 VPA 对象的大型集群的能力。我们还改变了 Goldilocks 的部署方式，所以现在它包括了一个 [VPA 子图表](https://github.com/FairwindsOps/charts/tree/master/stable/vpa) ，你可以用它来安装 VPA 控制器和它的资源。

我们在 SquareSpace 与 [哈里森·卡茨](https://github.com/hjkatz) 一起做了很多改变，这是基于他和那里的团队的宝贵反馈。我们希望不断改进我们的开源项目，并欢迎您的贡献！

Goldilocks 也是我们的 [Fairwinds Insights 平台](https://www.fairwinds.com/insights) 的一部分，该平台提供对您的 Kubernetes 集群的多集群可见性，因此您可以针对规模、可靠性、资源效率和安全性配置您的应用。Fairwinds Insights 可供免费使用。可以 [在这里报名](https://www.fairwinds.com/coming-soon) 。

[![Free Download: Kubernetes Best Practices Whitepaper](img/b53e4ee22b6ef19bc06a035649ea1dc6.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/4f92c7e1-1646-4985-9a0a-b1091903dddb)