# AWS Karpenter 就绪性:确保您为迁移做好准备的 6 种方法

> 原文：<https://www.fairwinds.com/blog/aws-karpenter-readiness-6-ways-to-make-sure-youre-ready-for-the-move>

 对于熟悉 Kubernetes 的人来说，您已经知道它有许多可用的配置，要么是可扩展的，要么是性能更好的。过去，大多数组织使用 Kubernetes 的 [集群自动缩放器来帮助他们根据工作负载需求自动扩展 EKS 集群节点池。根据您为节点池指定的最小和最大大小，群集自动缩放器会在需求高时添加节点，在需求低时将节点减少到最小大小。](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler)[AWS kar penter](https://aws.amazon.com/blogs/aws/introducing-karpenter-an-open-source-high-performance-kubernetes-cluster-autoscaler/)是由亚马逊网络服务(AWS)打造的开源集群自动缩放器，旨在根据 AWS 环境中的应用工作负载，帮助提高应用可用性和集群效率。

## AWS Karpenter 比 Cluster Autoscaler 好吗？

Cluster autoscaler 和 Karpenter 基于对更多容量的需求添加节点的方式类似。Cluster autoscaler 依靠用户做出更多决策来动态调整其集群的计算能力。使用 cluster autoscaler 时，您需要评估不同集群和节点中所需的容量类型，并决定需要调配哪些类型的实例。最初做出这些决定并不容易，因为你不知道这些答案，你需要花费大量的时间和精力来做出这些决定并更新它们。

### AWS Karpenter 是 AWS 自带的 cluster autoscaler 的一个更智能的版本，它消除了使用 Cluster Autoscaler 时的一些猜测。[**kar penter 网站**](https://karpenter.sh/) **将其描述为“任何 Kubernetes 集群的即时节点”Karpenter 的设计目的是根据集群的应用程序自动启动合适的计算机资源，而不是要求操作员预先做出大量决定。这有助于使用 Kubernetes 的组织做出更有效的决策。但是真的那么容易吗？您和您的应用程序准备好迁移到 AWS Karpenter 了吗？以下是你搬到 Karpenter 之前需要了解的六件事。**

## 1.整合节点

作为一个自动缩放器，Karpenter 通过注释、节点选择器和其他配置选项为您提供了灵活性，从而可以轻松地进行缩放。需要注意的一点是，如果 Karpenter 发现有机会根据价格、节点数量或其他考虑因素创建更好的节点配置，它可以整合到更少的节点。这意味着 Karpenter 可能一直在摆脱节点。

您希望 Karpenter 能够整合您的 pods，因此您不希望设置注释，使应用程序不会被逐出节点，因为您担心如果您丢失 pods，应用程序会崩溃。如果 Karpenter 检测到您有太多的节点，它将开始驱逐 pod，试图将它们从它确定您不再需要的特定节点上移走。但是如果您设置了 do-not-evict-true 注释集，它将阻止 Karpenter 缩小该节点。

您可以使用开源工具，如 [北极星](https://www.fairwinds.com/polaris) ，以确保您遵循 Kubernetes 和 Karpenter 中的最佳配置实践，从而确保您应用的设置不会干扰 Karpenter 有效整合节点的能力。

## 2.管理集群拓扑

使用 Karpenter，您可以表明您只想为应用程序使用特定的节点类型。这可能会改变集群的拓扑结构，因为如果集群中运行多个应用程序，并且一个应用程序需要一种节点类型，而另一个应用程序需要不同的类型，则 Karpenter 必须同时运行这两种类型。如果你在十个不同的团队中这样做，这可能会成为一个问题。使用您的置备程序，您可以限制希望允许的节点类型的数量，或者使用 Polaris 禁止选择特定的节点类型。

## 3。与云承诺模型保持一致

如果您的组织决定从云服务提供商处预订或购买节省计划，您希望确保您在 [承诺模式](https://www.techtarget.com/searchcloudcomputing/feature/5-ways-to-reduce-cloud-costs) 上利用的资金得到有效利用。Karpenter 可以根据运营团队为您的组织所做的决策，帮助您确保资源得到充分利用。Karpenter 是 Kubernetes-native，这使得您的用户更容易:

## 4.设置资源请求和限制

您希望 Karpenter 高效，并使用正确的节点组合构建您的集群。但是，因为您仍然依赖 Kubernetes 调度的底层机制来完成这项工作，所以您需要设置资源请求和限制。Polaris 将任何没有资源请求和限制的事情标记为您需要解决的问题。当您在整体上对 CPU 数量和内存量设置限制时，您可以设置集群的最大值，因此您知道它永远不会超过某个成本。

[Goldilocks](https://www.fairwinds.com/blog/introducing-goldilocks-a-tool-for-recommending-resource-requests) 是一个开源工具，它使用 [垂直 Pod Autoscaler 的](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler) 推荐引擎来提供基本级别的资源请求和限制推荐。它会检查 pod 的使用情况，并针对您的资源请求和限制设置提出建议。如果您不确定设置这些请求和限制是什么(或者如果您根本没有设置它们)，这是一个很好的开始方式。随着时间的推移，您可以定期重新评估它们，并相应地更新您的资源请求和限制。

## 5.尊重你的分离舱破坏预算

Karpenter 在缩减节点规模和驱逐单元时会考虑单元中断预算。如果你没有所有应用的 pod 中断预算，你可能会失去大量的 pod。这可能是因为您的服务无法处理移除了那么多 pod 的标准流量。

你需要意识到这些限制，这是北极星帮助的另一件事。Polaris 中的默认检查之一是确保 pod 中断预算附加到您的部署中。Pod 中断预算是所有 Kubernetes 集群的标准最佳实践，当您以稍微更积极或更智能的自动伸缩形式(如 Karpenter)引入节点流失时，您需要确保它到位。

## 6.准备好活性和就绪性探测

如果您的单元正在服务流量，并且您经常上下扩展单元，那么您需要对节点变动具有弹性。在准备好为流量提供服务之前，pod 必须启动、连接到数据库并执行其他几项任务。如果您想在 Karpenter 引入的节点变动中保持弹性，那么您需要准备好活跃度和就绪性探测器。Polaris 包括对活性和就绪性探针的检查，甚至允许您编写需要特定范围的资源请求和限制的策略。

## 你准备好了吗？

卡本特的好坏取决于你给它的信息。您可以向 Karpenter 提供的信息越多，提供的约束越少，Karpenter 对您的组织就越有伸缩性和灵活性。Kubernetes 本身包括许多可以调整的旋钮——添加 Karpenter 会增加更多旋钮。这导致更多潜在的复杂性，因此您需要一个策略或治理解决方案来正确管理所有这些旋钮。Polaris 收集了久经考验的 Kubernetes 最佳实践，通过帮助您设置与 Karpenter 一致的检查和警报来提高您的 Karpenter 就绪性，从而通过为 K8s 集群提供快速简单的计算资源调配来利用云

**[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)**