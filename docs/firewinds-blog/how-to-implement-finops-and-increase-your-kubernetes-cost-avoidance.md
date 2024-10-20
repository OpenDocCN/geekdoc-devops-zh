# 如何实施 FinOps 并增加您的 Kubernetes 成本规避

> 原文：<https://www.fairwinds.com/blog/how-to-implement-finops-and-increase-your-kubernetes-cost-avoidance>

 许多组织最近开始在生产中使用 Kubernetes，并且刚刚开始看到 Kubernetes 和云成本的真实情况。团队发现 Kubernetes 的成本高于预期并不罕见。随着越来越多的工作负载转移到生产中，许多组织发现他们需要控制这些 Kubernetes 成本，或者(至少)了解这些成本。有一些策略和工具可以帮助组织了解他们的 Kubernetes 成本，以及如何实施 Kubernetes 成本规避策略——实施 FinOps 可以帮助实现这些目标。

## 什么是 FinOps？

FinOps 是一种尝试确定与您的云支出相关的单位成本，并将您组织(尤其是大型组织)中的不同孤岛整合在一起，以持续讨论并更好地了解云成本，以及这些成本如何影响业务决策的实践。使用 Kubernetes 的团队发现这尤其具有挑战性。在这里，我们将讨论 FinOps 基金会确定的 FinOps 的[六项核心原则，以及在使用 Kubernetes 时它们之间的关系:](https://www.finops.org/framework/principles/)

### 1。团队必须合作

为了拥抱 FinOps，团队必须一起工作。财务团队需要以与 IT 团队相同的速度和粒度前进。工程团队还必须从效率的角度考虑成本，并采用成本作为他们考虑的衡量标准。组织必须不断改进其 FinOps 实践以提高效率和创新，同时定义云和 [Kubernetes 治理](https://www.fairwinds.com/kubernetes-compliance) 并实施对云使用的控制

### 2。采用云使用的所有权模式

当功能和产品团队能够管理他们自己的云使用并根据他们的预算进行衡量时，他们就能够做出更好、更具成本效益的决策。Kubernetes [所有权模型](https://www.fairwinds.com/kubernetes-service-ownership-whitepaper) 有助于团队提高对跨工作负载的云支出的可见性，并通过在团队级别进行跟踪来提高责任性。

### 3.集中化推动财务运营

有几种方法可以让团队集中管理和控制云的使用，比如使用承诺使用折扣、保留实例和云提供商的批量/定制折扣。通过使用折扣购买的集中流程，工程团队不再需要考虑价格谈判。云使用的集中方法支持精细的成本分配，因此负责的团队和成本中心可以看到所有成本，无论是直接成本还是分摊成本。

### 4。及时、可访问的报告

创建一个报告及时且易于访问的环境至关重要，因为它使团队能够及时利用关于支出和效率的反馈。这还可以帮助服务所有者评估资源是过度调配还是调配不足。当资源被自动分配时，它推动了云使用的持续改进。

### 5。业务驱动的云价值决策

使用趋势和差异分析有助于团队理解成本增加的原因以及他们可以做些什么来避免成本。内部团队也可以使用基准来显示最佳实践的有效使用，并确保这些实践得到执行。使用行业同行的基准测试也可以帮助您的组织了解它与其他组织相比表现如何。

6。利用云的可变成本模型

有许多工具可以帮助您正确地确定实例和服务的规模，这将帮助您的组织确保拥有适当的资源配置级别。了解正确的资源配置水平将有助于您做出更好的决策，因为您可以根据自己的实际需求比较不同服务和资源类型的定价。

## FinOps 为什么重要？

现在，随着我们进入潜在的经济衰退，许多组织都在考虑他们的开支。许多组织最近转向了云原生架构，并采用了 Kubernetes。如果你是其中之一，这是一个很好的时机来审视你的工具，你的云足迹的成本，并为 KubernetesT3 采用[FinOps 模型。能够理解为什么事情要花费成本，实际成本是多少，单位成本是多少，将有助于您对 Kubernetes 环境做出更好、更明智的决策。有几个不同的指标可以帮助您了解您的支出，为什么成本可能会增加，以及这些增加是否符合您的业务目标。](https://www.fairwinds.com/blog/kubernetes-blackhole-of-finops)

例如，在 Fairwinds，我们关注我们环境中每个集群的云支出。我们还会查看每位客户的支出，然后以几种方式对数据进行分割。这让我们可以看到我们的总账单是否在上升。如果是，增加的原因是:

*   我们产品中更大的功能集？

*   更大的客户足迹？

*   扩大的客户群？

*   个人客户现在的足迹更大了？

如果整体账单在上涨，而其他要素持平或下跌，那就是一个需要注意的信号。审查每个客户的云支出是一个持续的过程，可以提供整个组织的云成本的可见性。

## FinOps 在 Kubernetes 有什么不同？

云支出和 Kubernetes 支出之间的差异——以及采用 FinOps 方法——实际上源于容器和其他类型的抽象。这种抽象去除了人们熟悉的一些工件。例如，在过去，您可以查看账单并了解实例的成本、实例 ID、您熟悉的卷，或者在您的云环境中查找标签。

Kubernetes 引入了另一层抽象，部分是因为 Kubernetes 中的东西是短命的。今天在这里的节点明天就会消失，当您考虑云成本和共享资源时，这变得很难跟踪。这使得很难将支出分配到每个客户的成本、每个团队的成本或不同环境的成本。这些分配挑战增加了组织的复杂性，因为它们在云 API 和您正在部署和运行的东西之间注入了另一个抽象层。

## 为什么 FinOps 对容器和 Kubernetes 很重要？

在 Kubernetes，FinOps 面临两个团队难以应对的挑战。第一个是现在集群的分离方式。有一个叫做名称空间的概念，它是集群中的一个[隔离结构](https://kubernetes.io/docs/concepts/security/multi-tenancy/)。它允许您隔离资源，无论它们是应用程序、团队还是不同的环境。你可以把它想象成一个虚拟集群。这个概念很棘手，因为它存在于您在构建中跟踪的不同节点和部分中。你的一对多关系越多，就越难追踪。

另一个挑战是扩展。Kubernetes 的优势之一是，当流量增加时，它会根据您的需要不断添加更多资源，以使环境变得更高可用或更具可扩展性。然而，这种扩展能力推高了成本，因为云中的一切都是计量的。当财务团队询问成本增加的原因时，您需要一种简单的方法来解释每个应用程序的成本。准备好这些信息可以增强组织内部的信心，这也有可能推动向 Kubernetes 的迁移，因为财务团队确信他们了解组织的相关成本。

## 和你的首席财务官成为好朋友

财务团队通常不知道 Kubernetes 如何工作，Kubernetes 是什么，或者成本是多少。特别是在更大的组织中，旧的财务模型认为，要花钱，你需要做一个业务案例，提交给财务或采购部门，获得批准，然后你就可以花钱了。FinOps 模型迫使业务和工程之间进行对话，财务团队解释他们的约束以及他们预测成本和预算需求所需的信息。组织的工程方面必须能够理解更大的背景。理想情况下，工程团队了解并能够解释他们可以预测什么，他们如何围绕支出创建 [护栏](https://www.fairwinds.com/kubernetes-guardrails-explained-reg) ，以及云模型在扩展产品或扩大客户足迹的能力方面提供的机会。这种方法鼓励整个组织的伙伴关系。

## 让 FinOps 和工程部门谈谈

你如何和你的工程师谈论管理成本？大多数工程师只想部署东西；他们想写运行的代码，他们不关心 [成本](https://www.fairwinds.com/blog/a-kubernetes-overview-says-proper-configuration-is-key-to-saving-money) 。对于工程师、用户或开发人员来说，要了解部署基础设施的最重要的事情是所部署内容的影响。如果您没有正确分配资源或过度配置，您的工程师需要了解他们的行为会推动组织的盈利能力。通常，如果您做出不正确的云承诺，实际上可能会比使用[按需](https://www.techtarget.com/searchitoperations/definition/on-demand-computing#:~:text=On%2Ddemand%20computing%20(ODC),by%20a%20cloud%20service%20provider.)或[按需付费](https://www.techtarget.com/searchstorage/definition/pay-as-you-go-cloud-computing-PAYG-cloud-computing)花费更多，这在财务上是危险的。由于不正确地两面下注，你花的钱远远超过了你应该花的。您需要在工程和财务团队之间进行有节奏的对话，以便他们能够一起获得潜在的节约，并了解一些原始成本以及这些决策对组织的业务影响。

## 如何实现 FinOps？

创建一个持续的流程，在此流程中，您可以回顾员工的工作内容，一起查看数字，并查看单位成本，这有助于您提出重要的问题，例如:

*   这个费用合理吗？

*   为什么或为什么不？

*   为什么会产生这些成本？

*   如果我们认为有问题，我们能做些什么来降低成本？

*   缓解成本值得吗？

*   你会创造出与你所花费的时间和金钱相等或更好的节约吗？

回顾这些事情，并从长期成本和节约的角度考虑它们，将有助于您做出更好的 Kubernetes 成本避免决策。这是一个持续的过程，它将帮助你建立一个 FinOps 实践，使 [Kubernetes 成本](https://www.cncf.io/blog/2022/10/20/kubernetes-best-practice-how-to-correctly-set-resource-requests-and-limits/)与商业决策相一致。

## 工具支持 FinOps 方法

有本地 Kubernetes 工具可以帮助您了解您的成本。 [AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/) 是一个很棒的工具，还有来自不同云提供商的其他工具。这些工具有助于您了解云的使用情况以及账单上消费的单个行项目。您可以使用标签和标记来帮助您了解成本。设置名称空间并始终如一地使用它们也将帮助您更好地了解您的成本。

[Goldilocks](https://www.fairwinds.com/blog/introducing-goldilocks-a-tool-for-recommending-resource-requests) 是一个与[垂直 Pod 自动缩放器](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler) (VPA)结合的工具，后者是 Kubernetes 的一个附加组件，允许您自动垂直调整 Pod 的大小(这意味着 [设置您的资源请求和限制](https://www.fairwinds.com/blog/kubernetes-resource-limits) )。开源工具 Goldilocks 位于 VPA 之上，并根据您的工作负载的历史使用情况提出更改建议。

当您查看单个集群时，很容易部署一个导航图或安装并使用一些工具，然后查看一两个控制面板。当您开始在整个组织中采用 FinOps 方法，并需要在不同的环境中创建标准时，很难大规模地做到这一点。Kubernetes 的政策往往不一致，您可能不了解整个组织的所有成本。在这种情况下，拥有一个统一数据的平台是非常有用的，并且可以更容易地看到整个组织在 Kubernetes 和云成本方面发生了什么。[fair winds Insights](/insights)使用开源工具，包括 Goldilocks，来形成一个平台，帮助您在所有集群中部署工具，自动执行策略，并轻松查看统一数据，以增强控制和可见性。

观看网络研讨会“ [Kubernetes 成本规避:实施 FinOps](https://webinars.devops.com/kubernetes-cost-avoidance-implementing-finops) ”,与工程运营副总裁 Elisa Hebert、首席技术官 Andy Suderman 和高级解决方案架构师约翰·哈什姆三世一起了解如何在您的组织中实施 FinOps。