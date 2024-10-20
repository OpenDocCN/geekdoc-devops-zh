# 简化 Kubernetes 多集群管理的三个步骤

> 原文：<https://www.fairwinds.com/blog/three-steps-to-streamlining-kubernetes-multi-cluster-management>

 Kubernetes 作为一项技术，使组织能够跨不同的云基础设施和分布大规模运行容器化的应用程序。it 部门(目前)无法做到的是以合规、可靠的方式集中管理大量现成的集群。这一现实使得跨不同集群交付治理和标准化变得困难，更不用说抑制创新了。但是，随着新集群的不断增加，找到简化多集群管理的新方法将成为云成功的金戒指。

## 多集群管理

有效的多集群管理意味着减少两件事—冗余工作和运营开销。它还包括建立正确的框架来发展您现有的治理模型，以减少跨集群生命周期管理中的开销和冗余工作。但是，如果您在一个集群数量不断增加的企业环境中运营，所有集群都是独立管理的，很少有统一性，简化这一管理流程的复杂性可能会成为企业成功的巨大障碍。

随着 Kubernetes 集群数量的增长，从业者被迫花费越来越多的时间进行管理，而用于生产的时间越来越少。他们需要的是一种在发现不同集群时集中查看、管理和整合这些集群的方法，以便能够适当地优化资源和处理问题，而不会浪费宝贵的时间。

> 寻找更多提示？ [阅读我们的 Kubernetes 最佳实践白皮书](https://www.fairwinds.com/kubernetes-best-practices-comprehensive-white-paper) ！

## 简化集群的步骤

如果您组织中的团队正在努力应对多集群 Kubernetes，不要惊慌——您并不孤单。这里有三个基本步骤，您可以立即采取，以减少一次管理多个集群所花费的时间和资源。

**1。采用零信任方法**。这种安全模型假设网络中以及网络之间的所有系统、服务和用户都不会自动受到信任并被授予访问权限。Kubernetes 包括实现对机群中每个集群的这种级别的控制访问所需的所有挂钩，利用了诸如认证、授权、加密和配置验证等技术。对 Kubernetes 环境应用零信任原则意味着控制对 API 服务器的访问，这是每个集群的 Kubernetes 控制平面的核心。因为 API 调用用于查询名称空间、pod 和配置图等对象，所以控制对 API 使用的访问是保护您的工作负载和实现 Kubernetes 零信任的关键。

零信任最佳实践允许组织创建一个安全的环境，在这个环境中，所有单个元素都得到正确配置和协调。当这种安全模式在多集群环境中实施时，可以大规模运行多个集群，而且风险更低。

**2。启用多集群部署**。在多云、多集群 Kubernetes 环境中，频繁的应用程序和基础架构更新是常态。团队经常需要同时部署到多个 Kubernetes 集群，这使得几乎不可能避免漂移，这是集群之间的不一致和错误配置，通常会导致停机或更糟。

由于 GitOps 可以通过将熟悉的工具引入基础设施管理和持续部署(CD)来帮助解决这些挑战，因此它经常被寻求更好的整体标准化、安全性和工作效率的组织所使用。当对存储库进行更改时，代码会从您的集群中回滚，以快速可靠地实现部署自动化。尽管可以使用标准的 Git 工具实现 GitOps 工作流，但要享受全部好处，还需要额外的工具，包括确认所需状态维护的能力。

被动或短期调整可能会迷失在集群的海洋中。这里有一些工具可以帮助您发现车队中的意外偏差。

*   Fairwinds [Polaris](https://polaris.docs.fairwinds.com/) 提供集群内工作负载的仪表板视图，以及它们与最佳实践的关系。
*   Fairwinds [Goldilocks](https://github.com/FairwindsOps/Goldilocks) 通过在推荐模式下使用垂直 pod-autoscaler，帮助您“恰到好处”地获得 Pod 资源请求和限制，在仪表板中可视化。
*   Fairwinds [RBAC Lookup](https://github.com/FairwindsOps/rbac-lookup) 是一个命令行工具，可以方便地找到与用户、组或向 Kubernetes 认证的服务帐户相关的角色。

**3。简化生命周期管理**。正如我们所知，Kubernetes 环境随着时间的推移而增长，许多云服务，如亚马逊 EKS 和 Azure AKS，提供了有益的覆盖。尽管在本质上是相似的，但是所有这些类型的 Kubernetes 都有不同的管理工具，这意味着在每个环境中部署和更新集群可能看起来非常不同。

这里的最佳解决方案是围绕单一类型的 Kubernetes 实现组织标准化，这种 Kubernetes 可以对整个车队进行生命周期管理。也就是说，这个目标很难实现，因为现有的工具很少能同时涵盖流行的 Kubernetes 发行版和云服务。简化生命周期管理的最佳实践是找到一家 SaaS 服务提供商，如 [Fairwinds Insights](https://www.fairwinds.com/insights) 、 ，它允许用户从单一控制台部署、管理和升级所有集群，这是一个提高可见性、可靠性和一致性的仪表板。

## 费尔温德见解

如今，组织需要跨集群安装和配置多种工具，以确保安全性、资源优化和可靠性检查。如果没有对正在发生的事情的集中了解，时间和资源就会被浪费。这就是我们创建 Kubernetes 治理平台[fair winds Insights](https://www.fairwinds.com/insights)的原因。

> Fairwinds Insights 可供免费使用。你可以在这里报名。

Insights 将优秀的开源工具(包括这里描述的工具)的结果聚集到一个视图中，这允许您跨多个集群评估和实施您的标准。Insights 使用 Polaris 和开放策略代理为您的部分或全部集群提供集中策略管理。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)