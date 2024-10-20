# 关于 FinOps 和 Kubernetes 的 3 件事

> 原文：<https://www.fairwinds.com/blog/3-things-to-know-about-finops-and-kubernetes>

 最近我参加了 [FinOps 基础培训](https://learn.finops.org/) 并获得认证。如果你想拓展你的 FinOps 知识，我强烈推荐你。培训内容广泛，涵盖了 FinOps 框架和原则、利益相关者以及成熟度的不同阶段——通知、优化和运营。

在 Fairwinds，我们专注于容器和 Kubernetes 配置，并通过治理平台支持该技术，包括确保 K8s 得到优化以避免不必要的云成本。在我接受培训时，有三件关于 Kubernetes 的事情让我印象深刻。

## 1.优化到只使用你需要的东西比听起来要难

云使用优化是 FinOps 基金会概述的核心领域之一。它说，“组织确定并采取行动，将运行的云资源与任何给定时间运行的工作负载的实际需求相匹配…仅当我们需要它们产生业务价值时，才使用正确的资源，这是云的可变使用模式最终允许我们实现业务价值最大化的原因。”

Kubernetes 面临的一个挑战是如何预测并优化您的云支出，以确保您不会过度调配资源，或者在资源上过度支出。使用 Kubernetes，您需要调整应用程序 CPU 和内存的大小，并成功地进行扩展。用户需要为每个工作负载设置特定的资源请求和限制。通过合理和正确的限制和请求，可以最大限度地利用 CPU 和内存。对应用程序设置太低的限制会导致问题。例如，如果内存限制太低，Kubernetes 一定会因为违反其限制而终止应用程序。同时，如果设置过高，资源将被过度分配，导致更高的账单。

虽然 Kubernetes 的最佳实践规定应该一致地设置资源限制和对工作负载的请求，但没有工具，这只是猜测。团队经常根本不设置要求或限制，而其他团队在初始测试时将它们设置得太高，然后永远不会正确。确保扩展操作正常工作的关键是拨入每个 pod 上的资源限制和请求，以便工作负载高效运行。

在实现 FinOps 框架时，一定要考虑可以使用哪些工具来了解 CPU 和内存的使用情况。这将有助于告知(基金会模型的一个阶段)团队如何只使用需要的东西。

有两个工具需要注意: [Goldilocks](https://www.fairwinds.com/goldilocks) ，这是一个开源项目，它可以帮助适当规模的 Kubernetes 工作负载获得“刚刚好”的结果，还有[fair winds Insights](//www.fairwinds.com/insights)，这是一个成本优化工具，也是 FinOps 认证的。

## 2.把信息放在工程师的路径上

在 FinOps 基金会框架的 [运营](https://www.finops.org/framework/phases/) 阶段，它概述了“组织开始持续评估业务目标和他们针对这些目标所跟踪的指标，以及它们的趋势。”然而，这一阶段的成功取决于战略利益相关者的合作。您需要团队报告结果，讨论结果，并根据早期阶段做出明智的决策。

该培训的一个关键要点是“将信息放在工程师(和其他人)的路径上”，这一点真正引起了 Kubernetes 用户的共鸣:利用仪表盘，在他们工作的地方与他们见面(Slack、Confluence、JIRA、团队)，或者通过产品设计工具

这对于您希望团队采用的所有工具的成功是至关重要的，但是使用 Kubernetes，并不是所有的团队都能够在命令行中工作。您的团队将使用混合的工具来帮助他们“走得更快”——更快地编码和发布。当你在开发生命周期中遇到阻碍时，你会遇到阻力。对 FinOps 模型变得重要的信息需要随时可供所有利益相关者使用和理解。这就是为什么无论你采取什么样的策略，都要放在工程师的道路上，这一点非常重要。

在 Fairwinds，我们确保在[fair winds Insights](//www.fairwinds.com/insights)中提供的成本结果提供了避免浪费云资源的建议。这些建议被转化为 Insights 中的行动项目，但我们不会让工程师跳到仪表盘上采取行动。相反，我们通过与吉拉、Slack、Azure DevOps 等的集成来提供这些行动项目。它有助于在 Kubernetes 实现 FinOps 计划。

## 3.你需要合适的工具

拥有合适的工具来支持您采用 FinOps 是有意义的。有许多工具具有不同程度的功能和价位。该基金会表示，“找到合适的工具，让你以合适的详细程度获得你需要的数据，以达到你的成熟度水平，从而做出你需要做出的实时决策，这一点至关重要。”

对于团队来说，在 Kubernetes 中获得这些“正确的数据”可能是一件非常头疼的事情。首先，这是一个不断变化的短暂环境。你是如何衡量的？你怎么知道一个不再存在的容器用的是什么？当您开始您的 FinOps 之旅时，您是否需要这种程度的细节，或者您只是需要知道如何开始调整应用程序？

[Goldilocks](https://goldilocks.docs.fairwinds.com/) 是一款开源工具，可以帮助运营团队调整应用程序的规模，即“让它们恰到好处”这是你拥抱 FinOps 和使用 Kubernetes 的一个很好的跳板。随着集群和节点数量的增长，团队需要更多的数据来做出正确的决策。这就是像 Fairwinds Insights 这样的工具的用处。Insights 让 FinOps 采用过程中的所有利益相关者看到 Kubernetes 和容器消耗了多少资源。它提供了关于如何调整规模和节约成本的建议。此外，对于财务团队和开发团队来说，它很容易消化。这一点尤其重要，因为许多 FinOps 领导者，或者那些开始 FinOps 实践的人，并不总是知道该问 DevOps 团队什么问题。您需要合适的工具，虽然您需要云级别的工具，但随着您采用 cloud native/Kubernetes，您将需要一个工具来弥合 FinOps 和 DevOps 之间的差距。

Kubernetes 可以是 FinOps 的 [黑洞，但不一定。通过使用正确的工具，在工程师的道路上，您可以优化您的云原生资源。Fairwinds Insights 可以帮助您采用 FinOps 和服务所有权模型，开发人员和运营团队可以优化成本并与财务部门合作。](https://www.fairwinds.com/blog/kubernetes-blackhole-of-finops)

如果您正在努力将 FinOps 模型应用到 Kubernetes，请联系我们。我们可以帮忙。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)