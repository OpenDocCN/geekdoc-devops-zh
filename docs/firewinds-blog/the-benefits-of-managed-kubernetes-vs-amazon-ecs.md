# 托管 Kubernetes 与 Amazon ECS 的优势

> 原文：<https://www.fairwinds.com/blog/the-benefits-of-managed-kubernetes-vs-amazon-ecs>

 [在之前的一篇文章](/thoughts/deciding-on-amazon-ecs-vs-kubernetes-for-container-orchestration)中，我们讨论了 AWS 弹性容器服务(ECS)等容器编排解决方案的优缺点。截至 2019 年，Kubernetes 已成为生产中运行容器的前端和中心解决方案。对于使用 AWS ECS 的客户来说，这迫使他们围绕 Kubernetes 的业务优势做出决定。

2018 年 CNCF 调查显示，83%的组织使用 Kubernetes 作为其容器编排解决方案，而 24%的组织使用 ECS。AWS 在 2018 年增加了弹性 Kubernetes 服务(EKS)，以应对客户在 AWS 上使用 Kubernetes 的增长。Kubernetes 还使公司能够在内部和云环境之间转移工作负载，支持该技术的新用例，但也增加了额外的运营开销。EKS 的目标是简化其中的一些任务，使 AWS 成为不断增长的托管 Kubernetes 市场的主要参与者。

## 【Kubernetes 可以提供帮助:

*   *无锁定，完全开源:* Kubernetes 既可以在内部使用，也可以在云中使用，无需重新构建您的容器编排策略。该软件是完全开源的，可以重新部署，而不会产生传统的软件许可费用。
*   *强大的灵活性:*此外，如果您有一个重要的创收应用程序，Kubernetes 是满足高可用性要求的一个很好的方式，而不会牺牲对效率和可伸缩性的需求。借助 Kubernetes，您可以精确控制工作负载的扩展方式。这允许您在需要切换到更强大的平台时，避免受 ECS 或其他容器服务供应商的限制。
*   *高可用性和可伸缩性:* Kubernetes 包含内置的高可用性特性，有助于负载平衡和自我修复。自动扩展可确保您的工作负载始终有适量的计算可用。
*   充满活力的社区 : Kubernetes 是一个活跃的社区，拥有广泛的模块化开源扩展，得到了像 CNCF 这样的大公司和机构的支持。数千名开发人员和许多大公司为 Kubernetes 做出了贡献，使其成为现代软件基础设施的首选平台。
*   *长期解决方案*:鉴于 Kubernetes 的崛起，许多云提供商正在将他们的研发重点转移到扩展他们的托管 Kubernetes 服务，而不是传统选项。在制定长期 IT 战略时，请考虑 Kubernetes。

## **采用 Kubernetes 的主要考虑事项**

Kubernetes 确实有一个学习曲线。现有的托管 Kubernetes 服务，如谷歌 Kubernetes 引擎(GKE)、Azure Kubernetes 服务(AKS)和 AWS Elastic Kubernetes 服务(EKS)，在保持 Kubernetes 控制平面可用方面做得很好，但仍需要运营团队做出一系列技术决策。例如，运营部门仍然必须添加监控、日志记录、审计、[部署验证](/polaris)、DNS 和入口功能等等。除此之外，运营团队还能及时了解配置、升级和修补每个附加组件的最新最佳实践。所有这些对平台工程团队来说都是沉重的负担，因为他们要兼顾多个基础设施和开发运维优先级。

[Fairwinds ClusterOps](/clusterops) 通过利用容器和 Kubernetes 提供专业管理的云原生基础设施，为平台工程和 DevOps 团队解决了这一问题。ClusterOps 是一个完全托管的 Kubernetes 平台，提供由站点可靠性工程师团队全天候管理的生产级集群。ClusterOps 在云基础设施层和客户工作负载之间运行，集成了 Fairwinds 的工程专业知识以及我们的开源软件和自动化产品。它提供了所有必要的服务和管理控制，以确保部署持续优化、高度可用且安全。

如果您对 Fairwinds ClusterOps 感兴趣，请务必[下载解决方案表以了解更多信息](/clusterops-solution-sheet)。

[![ Fairwinds Managed Kubernetes Services Get the Datasheet](img/b7432d78ee47cfb59707e486d127e6e4.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/14d392aa-db7c-492b-8233-fb071ff3b1b9)