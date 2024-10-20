# Kubecost 替代方案:Kubernetes 通过 Fairwinds 进行成本分配和优化

> 原文：<https://www.fairwinds.com/blog/kubecost-alternatives-kubernetes-cost-allocation-and-optimization-by-fairwinds>

 在今年的 KubeCon 上，有很多关于 Kubernetes 成本优化和分配的讨论；许多与会者都在寻找 Kubecost 的替代方案。本博客并不打算提供 Kubecost 的利弊，而是提供一些理由，说明组织为什么需要 Kubernetes 成本分配和优化解决方案，并提供一些关于 Fairwinds Insights 的信息，fair winds Insights 是一个提供工作负载成本分配和优化的平台，除此之外，它还提供 Kubernetes 的安全和护栏功能。

## 为什么要添加 Kubernetes 成本分配和优化工具

云原生工具提供了许多优势，如自动伸缩，但也让开发人员和开发者可以按照他们想要的方式进行配置。这意味着在许多情况下，Kubernetes 的限制和请求没有设置，CPU 和内存没有设置，云成本可能会飙升。结果是有许多未知因素的令人惊讶的云账单。随着组织寻求降低开支，这是一个需要关注的领域。

不幸的是，Kubernetes 平台可能成为云消费的黑洞。这就是像 Kubecost 和 Fairwinds 这样的供应商提供解决方案的原因。虽然绝对有一些跟踪云支出的优秀组织在云提供商(如 AWS、Azure、GCP、GKE、AKS 或 EKS)中处于较高水平，即像 CloudHealth 这样的公司，但大多数组织并不提供 Kubernetes 级别的详细信息。随着容器消耗量的增加，这是一个必需的解决方案。

[Kubernetes 成本分配](https://www.fairwinds.com/blog/kubernetes-cost-allocation-updates-to-fairwinds-insights)很难。Kubernetes 调度的动态特性意味着容器化工作负载的运行位置总是会发生变化。所有这些变化意味着在 Kubernetes 中很难按工作负载分解成本。需要适当的配置，这需要能够监控成本、提出优化建议、提供工作负载成本分配以及提供成本显示的解决方案。

## Fairwinds Insights:另一种 Kubernetes 成本监控和管理解决方案

[Fairwinds Insights](https://www.fairwinds.com/kubernetes-cost-optimization) 提供跨 Kubernetes 和 containers 的成本管理，为合适的应用资源提供建议。它提供了一个集中、一致和优化的 Kubernetes 成本视图。FinOps 和 DevOps 团队同样使用洞察来获得对许多人和工作负载的看法，以更好地调整减少支出的机会，或根据实际使用情况提供证据，说明为什么可能需要增加云消耗。

Fairwinds Insights Kubernetes 成本功能的核心功能包括:

*   **工作负载成本分配** -使用实际云支出和工作负载使用情况来了解跨多个集群、聚合和自定义时间段发生的历史成本(长达 13 个月)。

*   **Kubernetes 成本优化** -评估单个应用并寻找机会在不影响应用性能的情况下降低成本。

*   **合理调整建议** -通过对资源请求和限制的监控和建议，最大限度地提高 Kubernetes 工作负载的计算和内存利用率。

*   **Kubernetes 成本反馈** -向财务团队报告 Kubernetes 的使用成本，分配给开发人员，并跟踪一段时间内的节约情况。

*   **多集群成本和使用情况** -获得跨云资源的集群容量和使用情况的明细。了解在闲置容量、共享资源与特定于应用的资源以及有效的节点扩展方面花费了多少。

*   **云计费集成** -使用实际的 AWS 云账单，按工作负载、名称空间或标签计算 Kubernetes 成本。跨多个业务维度提供基于使用情况的准确成本数据。

*   **服务质量控制** -协作调整应用规模，提供专门构建的建议，消除猜测，提高工作负载效率和性能。

Fairwinds Insights 可以像 Kubecost 一样免费试用。用户只需向我们的团队注册，添加代理，并在 Insights 仪表盘中查看建议。

> 注意:如果你是 CNCF 沙盒开源项目 KubeCost 或 OpenCost 的用户，并想尝试 Fairwinds Insights，你可以使用现有的 Prometheus 安装，或者选择安装 Fairwinds Insights 的预配置 Prometheus 包。[取得联系](https://www.fairwinds.com/fairwinds-insights-trial)。

## Cost 和 Kubernetes 安全和护栏

KubeCon 的一个不足为奇之处是，Kubernetes 用户希望整合供应商，因为组织希望减少支出。随着经济衰退的逼近和云计费的增加，成本成为焦点也就不足为奇了。

Kubernetes 成本优化的核心是配置。这就是为什么该功能是 [Fairwinds Insights](//www.fairwinds.com/insights) 平台的一部分。Insights 扫描 Kubernetes 集群，以识别安全性、可靠性和成本效率方面的错误配置。它还实现了 Kubernetes guardrails(或 policy ),以便组织可以为开发人员构建安全、经济且合规的应用程序铺平道路。

> Fairwinds Insights 可供免费使用。你可以在 这里 [报名。](https://www.fairwinds.com/coming-soon)

在一个平台中，用户可以获得使 Kubernetes 与业务目标(即更快地交付应用、可靠地扩展、获得云成本报告等)和安全性保持一致所需的全部内容。开发人员可以使用安全网更智能地工作，而不必担心 Kubernetes 要求的所有安全性、合规性或成本配置。

Insights 针对 Kubernetes 的成本挑战提供了一个更全面的解决方案，不仅解决了一个痛点(如 Kubecost ),而且解决了三个痛点。DevOps 团队可以不再是 Kubernetes 的服务台，也不再花时间试图为财务团队破译云账单和定价。它为 DevOps 团队腾出了时间来做他们想做的创新平台工作——帮助公司留住他们需要的 Kubernetes 人才。

> 对于那些可能还没有准备好洞察的组织来说，一个开源工具 [【金凤花](https://goldilocks.docs.fairwinds.com/) 是可以尝试的。它有助于组织根据集群指标合理确定工作负载的规模。可以在 [GitHub](https://github.com/FairwindsOps/goldilocks) 或者 [文档](https://goldilocks.docs.fairwinds.com/) 上查看。

[![The Guide to Kubernetes Cost Optimization: Why it's hard and how to do it well to embrace a FinOps model](img/c4a50e63ed5a9cc61e1fd81724696a57.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/83c995e8-6ab0-47b8-92f2-e5cddc242a55)