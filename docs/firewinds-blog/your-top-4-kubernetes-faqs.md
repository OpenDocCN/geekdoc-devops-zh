# 你永远不应该在 Kubernetes 第 5 部分:你的 4 大常见问题

> 原文：<https://www.fairwinds.com/blog/your-top-4-kubernetes-faqs>

 正如我们在本系列中所讨论的，有些事情你绝对不应该在 Kubernetes 中做。[云中尖叫创始人兼鸭嘴集团首席云经济学家 Corey Quinn](https://twitter.com/QuinnyPig) 、Fairwinds 总裁 Kendall Miller 和 Fairwinds 技术负责人(CRE 团队)Stevie Caldwell 就他们看到的一些[基本错误](/blog/never-should-you-ever-in-kubernetes-1-do-k8s-the-hard-way)，以及他们在过去几年帮助客户部署 Kubernetes 时遇到的一些最常见的 Kubernetes [安全性](/blog/never-should-you-ever-in-kubernetes-part-2-kubernetes-security-mistakes)、[可靠性](/blog/6-kubernetes-reliability-mistakes)和[效率](/blog/3-kubernetes-efficiency-mistakes)问题进行了精彩对话。在网上研讨会期间，与会者向我们提出了许多问题，我们很乐意与您分享。让我们来看看你的四个常见问题，如果开发和运营团队想从领先的容器编制器中获得最大收益，他们绝对不应该在 Kubernetes 中做什么。

## 1.我们可以(或者应该)运行工作平面和控制平面的组合节点吗？

答案是否定的。最好缩小控制平面的节点大小，称之为好。这又回到了“[不要在你的控制平面](/blog/never-should-you-ever-in-kubernetes-1-do-k8s-the-hard-way)上运行工作负载”这一条。这是因为最好的做法是将它们分开。通常这类事情是出于成本考虑，但是有其他更好、更安全的方式来运行集群并控制成本，而不是试图将所有节点组合成一个。通过让您的`etcd`集群也与控制平面位于同一位置，您已经采取了一种中间立场。(一些 Kubernetes 专业人士说，您应该从控制平面外部运行`etcd`。)如果您减小控制平面节点的大小，您可以调整工作节点的资源大小，这样您就不会使用太多资源，并且您应该利用[自动缩放](/blog/3-kubernetes-efficiency-mistakes)来缩小您的集群。你可以做各种各样的事情来节省运行 Kubernetes 的成本，但不要运行工人和控制平面节点。

## 2.我应该在 Kube 中运行数据库吗？

数据存储应该在 API 的后面，驱动数据存储的实际上是一个实现问题。如果您打算在 Kubernetes 上运行数据库，您需要有一个关于卷、持久性、备份、恢复等等的计划。虽然这个想法并不荒谬，但你确实需要为它设计架构。考虑一下，如果一个容器消失了，并且在一段时间内没有被替换，而这就是你的数据库运行的地方，会发生什么？

如果您正在运行的环境中提供了托管解决方案，那就值得一看。例如，如果您在 AWS 上运行您的工作负载，使用他们的数据存储产品，如 Aurora 或 RDS，比运行您自己的要容易得多。这将增加很多复杂性，所以如果没有必要，为什么要这样做呢？Kubernetes 建立的前提之一是所有数据都是短暂的。这就是为什么事物像它们一样自由地移动，为什么你失去一两个节点也没有关系。但是，当我们开始遇到持续的数据问题时，就像您处理数据库一样，那么您就必须相应地进行规划:

*   你把数据存储在哪里？
*   当运行您的数据库的 pod 需要去别的地方时会发生什么？
*   然后，pod 会被安排在另一个可用性区域中的节点上吗？
*   如果将它移动到另一个可用性分区，它正在写入的卷是否不再对它可用？
*   是否必须设置持久卷并设置区域关联或节点关联？

当你在 Kube 中运行一个数据库时，它变得非常复杂。所以你需要考虑这样做会有什么收获。这可能会增加一些事情的灵活性，但对于其他事情来说没有意义，所以请仔细考虑您的用例，因为在 Kubernetes 中运行您的数据库会增加很多复杂性。

## 3.K8s 计费有没有成本细分的解决方案？

例如，如果每个客户端有一个名称空间，并且想要计算每个名称空间的每月和班级，该如何做呢？我们有一个工具, [Fairwinds Insights](/insights) ,可以计算相对每日成本，并按服务进行细分。它还告诉您这些东西在哪个名称空间中，并帮助您优化您的资源请求，确定您正在请求什么和您可能会请求什么。Fairwinds Insights 还提供数据，向您展示随着时间的推移您将节省多少。

还有其他工具可以做类似的事情。 [CloudZero](https://www.cloudzero.com/) 比如云成本智能平台。 [Kubecost](https://www.kubecost.com/) 专门关注 Kubernetes 的成本。 [VMware](https://www.vmware.com/) 收购了另一家公司 [CloudHealth](https://www.cloudhealthtech.com/) ，该公司提供了一些跨云环境优化成本和使用的功能。来自 Apptio 的[云能力](https://www.apptio.com/products/cloudability/)帮助团队优化云资源的速度、成本和质量。

关心 Kubernetes 工作负载的人通常有两个很难找到好答案的问题。一个是跟踪共享资源；对于必须存在的事物，你如何分配和归属它们？另一个问题与数据传输有关，目前还没有很好的工具选项。例如，如果您正在使用 AWS，并且您有两个相互通信的服务，则数据传输要么是免费的，要么如果出于持久性目的，它们位于两个可用性区域中，则每千兆字节的成本为 2 美分。当我们谈论 Pb 级工作负载时，这是一大笔钱。

## 4.可以在 prem 上使用 Fairwinds Insights 吗？

简而言之，答案是肯定的——我们现在有了一个独立的 Fairwinds Insights 版本。Fairwinds Insights 从 [Polaris](/polaris) 以及近十几个其他 Kubernetes 审计机构(如 [Trivy](https://github.com/aquasecurity/trivy) 、 [Goldilocks](/blog/introducing-goldilocks-a-tool-for-recommending-resource-requests) 和 [kube-bench](https://github.com/aquasecurity/kube-bench) )获取数据，并将所有结果放在单一窗口中。自从推出我们的 SaaS 产品以来，我们发现一些北极星用户担心将数据发送给第三方，特别是医疗保健和金融等数据敏感行业的企业。为了解决这些问题，我们在过去几个月中努力构建了一个可以完全在客户环境中运行的 Insights 版本。我们最近发布了一篇文章[介绍我们新的自托管 Fairwinds Insights](/blog/an-easier-way-to-audit-your-kubernetes-infrastructure-self-hosted-fairwinds-insights) ，其中解释了它的工作原理以及使用本地版本的利弊。如果您有兴趣查看 Fairwinds Insights，请查看 [Fairwinds Insights 演示](/fairwinds-insights-demo)。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)