# 你为什么不遵循这 5 个 Kubernetes 最佳实践？

> 原文：<https://www.fairwinds.com/blog/why-arent-you-following-these-5-kubernetes-best-practices>

 随着容器成为开发和部署云原生应用程序的标准方法，许多组织正在采用 Kubernetes 作为他们用于容器编排的解决方案。最近的[云本地计算基金会(CNCF)调查](https://www.cncf.io/wp-content/uploads/2022/02/CNCF-AR_FINAL-edits-15.2.21.pdf)显示，96%的受访者正在使用或评估 Kubernetes，93%的受访者正在生产环境中使用 Linux 容器。换句话说，容器和 Kubernetes 正在变得流行，特别是在非洲等新兴技术中心，那里许多人正在寻求 Kubernetes 部署，其中 73%已投入生产。不仅新兴公司采用这些技术，大公司也是如此——通常比小公司更多。但是这是否意味着这些公司正在遵循 Kubernetes 最佳实践的基础呢？有时候…

很多时候，组织在没有完全理解成功部署 Kubernetes 所固有的复杂性的情况下就急于采用它。许多团队刚刚开始理解控制平面，调度器做什么，Kubernetes 中的节点如何工作，kubelet 做什么，daemonsets 是什么，Kube 如何改变软件开发生命周期。理解 Kubernetes 集群，使用 Kube instrumentation 跟踪哪些指标以及如何进行跟踪，以及进行故障排除以发现问题的根本原因，这些对于大多数组织来说仍然是一个挑战。

无论您是否仍在考虑何时以及如何实施 Kubernetes 来部署 SaaS 解决方案和其他应用程序和服务，或者您已经有了 K8s，通过采用监控系统和建立监控指标来应用 Kube 最佳实践来帮助您创建流程、明确任务和设置优先级都不会太晚。CNCF 指导许多开源项目，使其易于采用 Kube 和优化云原生架构。

Prometheus 是一个开源的系统监控解决方案，它以时间序列数据的形式收集和存储指标，并提供警报。监控工具和监控解决方案对于帮助开发运维团队解决分布式系统中的问题至关重要。所以，让我们后退一步，看看今天你需要关注的五大 Kubernetes 实践，以帮助你最大化 K8s 可以提供的长期价值。

## 1。Kubernetes 安全-最佳实践

安全性始终是技术的重要组成部分，Kubernetes 也不例外。人们的一个常见误解是认为 K8s 在默认情况下是安全的。这听起来很棒，但事实并非如此。Kubernetes 通过平衡速度和弹性来管理无状态微服务在集群中的运行方式，这可以使开发人员在部署软件时更加灵活。然而，如果您在 Kubernetes 部署中没有适当的治理和风险控制，这些好处就不会没有安全风险。

当您的 K8s 部署顺利运行时，您可能会认为一切都配置正确。不幸的是，过度许可是一种简单的方法，可以让你正在努力解决的事情发挥作用。授予 root 访问权限可以解决许多难题，但同时也会使您的组织面临拒绝服务(DoS)攻击或安全漏洞。事实上，错误配置是 Kubernetes 环境中最具挑战性的概念之一。即使是很小的错误配置，特别是以根级别访问运行的容器，也越来越成为网络攻击者寻找的漏洞。这些安全配置在 Kubernetes 中不是默认设置的，它们是您的安全团队必须建立的设置，然后通过自动化和策略来实施。

## 2。Kubernetes 成本优化-最佳实践

大多数组织采用 Docker 容器和容器编排解决方案，因为它们在基础设施利用方面天生高效。很简单，容器化环境允许您在每台主机上运行多个应用程序——每个应用程序都在自己的 Docker 容器中。这有助于您减少所需的计算实例总数，从而降低基础架构成本，而不会牺牲功能或应用程序性能。

Kubernetes 动态适应您的工作负载的资源利用率，并允许自动扩展(使用 [水平 Pod 自动缩放器或 HPA](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) )和集群扩展(使用[集群自动缩放器](https://github.com/kubernetes/autoscaler))以提供可伸缩性。Kubernetes 允许您设置资源请求和工作负载限制，以便您可以最大限度地提高基础设施利用率，同时确保您的应用程序性能平稳。听起来很棒，对吧？只有在正确设置 Kubernetes 资源限制和请求的情况下。如果您的内存限制太低，K8s 会因为违反限制而终止您的应用程序，但是如果您将限制设置得太高，您会过度分配资源——换句话说，您会付出比您需要的更多的代价。弄清楚 [正确的资源限制和要求](https://www.fairwinds.com/blog/how-to-correctly-set-resource-requests-and-limits) 是具有挑战性的，对于 Kubernetes 的新采用者和已经使用 Kube 多年的组织都是如此。

## 3。Kubernetes 可靠性-最佳实践

可靠性永远是目标，但实现 [Kubernetes 可靠性](https://www.fairwinds.com/blog/you-can-establish-reliable-kubernetes-clusters-without-losing-sleep) 是一项复杂的任务。优化 Kubernetes 需要技巧，特别是如果您使用的技术早于云原生应用程序和配置管理工具，它们并不总是提供最可靠的云原生体验。许多组织继续使用旧的解决方案，并在此基础上构建 Kubernetes，但这使得优化、可靠性和可伸缩性更难实现，尤其是当您的业务扩展时。确保集群可靠性的一个很好的方法是转而使用 [基础设施作为代码](https://www.techtarget.com/searchitoperations/definition/Infrastructure-as-Code-IAC) (IaC)，这有助于减少人为错误，提高一致性和可重复性，提高可审计性，并使灾难恢复更容易。它还可以帮助您解决 Kubernetes 工作负载中的性能问题。

## 4。Kubernetes 政策执行-最佳实践

采用 Kubernetes 的一个常见方法是用一个应用程序进行试验，这是一个很好的开始方式。但是，一旦您的组织承诺在多个应用程序、开发团队和操作团队中使用 Kube，那么为部署不一致的工作负载管理集群配置就变得非常困难。当您的团队没有关于如何部署应用程序和服务的护栏时，您会很快发现您的容器和集群之间的配置存在差异。这些差异很难识别、纠正和保持一致。手动识别这些错误配置非常困难。

要管理多集群环境，您需要建立 [Kubernetes 策略](https://www.fairwinds.com/blog/kubernetes-policy-enforcement-for-developers) 来实施一致的安全性、效率和可靠性配置。虽然策略可以实现全面的最佳实践，但有些策略可能特定于您的组织或环境。最佳实践文档似乎是管理这些策略的好方法，但是它可能很快就半途而废了。采用 Kubernetes [策略实施工具](https://www.fairwinds.com/insights) 可以帮助您防止常见的错误配置被发布，实现 IT 合规性，并使您的团队能够充满信心地交付——因为他们知道有护栏来实施您的策略。

## 5。Kubernetes 监控&警报-最佳实践

Kubernetes 监控配置通常是事后才想到的——许多组织直到出现问题时才考虑设置它们。但是优化 Kubernetes 监控和警报可以帮助您确保您的基础设施和应用程序正常运行，这需要您使用正确的工具来优化您的监控功能。可观察性与监控密切相关，因为实时观察系统的能力有助于您关联数据以报告系统的健康状况，监控关键指标，调试生产环境，并提前预防停机。Kubernetes 监控工具，特别是那些提供 Kubernetes 仪表板的工具，可以跟踪 Kubernetes pods、Kubernetes 节点和 Kubernetes 服务，是全面监控策略的一部分。在 Kubernetes 平台中需要跟踪的几个关键 Kubernetes 指标包括:

*   内存使用量

*   名称空间

*   资源指标

*   资源使用量

*   资源利用率

*   内存分配

*   CPU 使用率

像[Grafana](https://grafana.com/)这样的工具可以帮助您可视化应用指标和应用性能。使用开源工具可以帮助您避免局限于单一云提供商，如 AWS、Azure、Google Cloud Platform 等。集成可以帮助最终用户提高 kube 使用率，减少延迟，从而优化可扩展性，最大限度地减少重启，并改善用户体验。监控 Kubernetes API 服务器使团队能够了解集群组件之间的通信。

对于大多数团队来说，这意味着您需要考虑需要监控什么以及为什么要监控——确定 Kubernetes 监控最佳实践对您的组织来说是什么样的。了解哪些配置存在风险或浪费资源，尽早发现安全性和合规性风险，并在部署前发现错误配置，可以帮助您尽早解决问题并防止许多可能的问题。

## 你今天应该做的 5 个 Kubernetes 最佳实践

安全性、成本优化、可靠性、策略执行以及 Kubernetes 监控和警报都很复杂。虽然 Kubernetes 提供了许多组织越来越多地采用和利用的功能，但它们也要求您的部署在部署级别和集群级别都能很好地工作。对于许多采用 Kube 的团队来说，甚至对于那些已经有了 Kube 的团队来说，在实现这些 Kubernetes 最佳实践时，都很难知道从哪里开始。

Kubernetes 可以让您的组织提高容器的效用和生产率，并构建可以在任何地方运行的云原生应用程序。为了最大化您的 Kubernetes 实现，确保您遵循这五个 Kubernetes 最佳实践是至关重要的。有了正确的技术和 Kubernetes guardrails，您将能够实现构建可扩展、高效的云原生应用的承诺，这些应用可以在任何地方可靠、安全地运行，而不受特定于云的要求的影响。

深入了解 [Kubernetes 最佳实践](https://www.fairwinds.com/kubernetes-best-practices-comprehensive-white-paper)的细节——立即阅读这份白皮书。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)