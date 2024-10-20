# 什么是金发女孩？(或者如何设置您的 Kubernetes 资源请求)

> 原文：<https://www.fairwinds.com/blog/goldilocks-kubernetes-resource-requests>

 当我们在 2019 年 10 月[开源 Goldilocks](/blog/introducing-goldilocks-a-tool-for-recommending-resource-requests) 时，我们的目标是提供一个仪表板实用程序，帮助您确定设置 Kubernetes 资源请求和限制的基线。我们[将继续完善 Goldilocks](/blog/new-in-goldilocks) ，因为让资源请求和限制恰到好处对大多数组织来说都是一个持续的挑战。

## 为什么要设置资源配置？

您可以在 pod 中的每个容器上设置两种类型的资源配置:请求和限制。资源请求定义了容器需要的最小资源。限制定义了容器可以使用的最大资源量。设置这些有助于防止您过度使用资源，同时也保护您的资源用于其他部署。请求会影响 Kubernetes 中 pods 的计划方式；调度程序读取 pod 中每个容器的请求，然后找到适合该 pod 的最佳节点。Kubernetes 使用这些信息来优化您的资源利用。

## 正确处理 Kubernetes 资源请求

我们在推荐模式下使用 Kubernetes [垂直窗格自动缩放器](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler) (VPA)来查看我们每个应用程序上的资源请求建议。Goldilocks 为名称空间中的每个部署创建一个 VPA，然后向它们查询信息。虽然 VPA 可以设置资源请求，但 Goldilocks 中的仪表板可以方便地查看所有建议，并根据您对 Kubernetes 环境的了解做出决策。learnk8s.io 上有一个关于在 Kubernetes 中设置正确的要求和限制的好帖子，有很多代码、细节和有趣的类比。

一旦您的 VPA 就绪，Goldilocks 仪表盘中的建议如下所示:

![](img/0db33908351c0a2fec9195999334976b.png)

## 金发女孩如何产生推荐？

Goldilocks 使用 VPA 中的[推荐器](https://github.com/kubernetes/autoscaler/blob/master/vertical-pod-autoscaler/pkg/recommender/README.md)进行推荐。根据 [VPA 的文献](https://github.com/kubernetes/autoscaler/blob/master/vertical-pod-autoscaler/pkg/recommender/README.md#implementation):

“启动二进制文件后，recommender 会将运行 pod 的历史记录及其使用情况从 Prometheus 读入模型。然后，它循环运行，并在每一步执行以下操作:

*   *   使用资源的最新信息更新模型(使用基于观察的列表器)，
    *   使用来自 Metrics API 的新使用示例更新模型，
    *   为每个 VPA 计算新推荐，
    *   将任何更改的建议放入 VPA 资源中。"

Goldilocks 生成的建议基于 pod 在一段时间内的历史使用情况，因此您的决策不仅仅是基于一个时间点。

Goldilocks 在仪表板中显示了两种类型的建议。这些建议基于 Kubernetes [服务质量(QoS)等级](https://kubernetes.io/docs/tasks/configure-pod-container/quality-service-pod/)。QoS 类别是 Kubernetes 的一个概念，它决定了 pods 被调度和驱逐的顺序，Kubernetes 自己将 QoS 类别分配给 pods。有三种类型的 QoS 类别:保证、突发和尽力而为。具有相同 CPU 和内存限制和请求的 pod 被放入有保证的 QoS 类别。CPU 或内存请求低于其限制的 pod 被赋予突发 QoS 类别。BestEffort QoS 类别适用于由没有任何请求或限制的容器组成的 pod。(我们完全不建议这样做。)

使用 Goldilocks，我们从历史数据中生成两种不同的 QoS 类别的建议:

*   对于**保证的**，我们从建议中提取目标字段，并将其设置为该容器的请求和限制。这保证了容器请求的资源在被调度时可用，并且通常产生最稳定的 Kubernetes 集群。
*   对于 **Burstable** ，我们将请求设置为 VPA 对象的下限，将限制设置为上限。调度器使用请求将 pod 放在一个节点上，但是它允许 pod 在被终止或限制之前使用更多的资源，直到达到限制。这有助于处理峰值工作负载，但如果使用过多，会很快导致节点过度配置，使其不堪重负。

## 金发女孩有多准确？

金凤花开源软件完全基于底层的 VPA 项目，特别是推荐者。根据我们的经验，Goldilocks 是设置您的资源请求和限制的良好起点。但是每个环境都是不同的，Goldilocks 不能代替针对您的特定用例调整您的应用程序。

## 贡献给金发女孩

[Goldilocks 是开源的，可以在 GitHub 上获得](https://github.com/FairwindsOps/goldilocks)。我们继续进行大量的改进，提高其处理具有数百个名称空间和 VPA 对象的大型集群的能力。我们还改变了 Goldilocks 的部署方式，所以现在它包括了一个 [VPA 子图](https://github.com/FairwindsOps/charts/tree/master/stable/vpa)，你可以用它来安装 VPA 控制器和它的资源。我们和[哈里森·卡茨](https://github.com/hjkatz)在 SquareSpace 做了很多改变，基于他和团队的宝贵反馈。我们希望不断改进我们的开源项目，并欢迎您的贡献！

Goldilocks 也是我们的 [Fairwinds Insights 平台](/insights)的一部分，该平台提供对您的 Kubernetes 集群的多集群可见性，因此您可以针对规模、可靠性、资源效率和安全性配置您的应用。

> 您可以永远免费使用 Fairwinds Insights。 [拿到这里](https://www.fairwinds.com/coming-soon) 。

[加入我们的开源小组](/open-source-software-user-group)——参加我们 9 月 23 日或 12 月 14 日的下一次聚会！

[![Join the Fairwinds Open Source User Group today](img/8ab607311768483f3bb5136a75381d4b.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/b163554e-b5ef-4f40-a053-03afe6ecbee6)