# 为什么修复 Kubernetes 配置不一致对于多租户和多集群环境至关重要

> 原文：<https://www.fairwinds.com/blog/why-fixing-kubernetes-configuration-inconsistencies-is-critical-for-multi-tenant-and-multi-cluster-environments>

 在大多数情况下，组织用一个应用程序来试验 Kubernetes。一旦成功，这些组织将致力于跨多个应用、开发和运营团队的 Kubernetes。通常是自助服务模式，开发运维人员和基础架构负责人会让许多用户跨许多不同的集群进行构建和部署。

虽然自助服务模式消除了应用程序团队和基础架构之间的障碍，但由于工作负载可能配置不一致或需要手动部署，因此管理群集配置变得非常困难。如果没有护栏，集装箱和集群之间的配置可能会有差异，这对于识别、纠正和保持一致性是一个挑战。当用户从在线示例(如 StackOverflow 或其他开发团队)中复制并粘贴 YAML 配置时，工作负载被过度配置以“让事情正常工作”，或者如果没有现有的流程来验证配置，就会发生这种错误配置。

> 63%的部署允许其工作负载作为 [根用户](https://www.fairwinds.com/blog/how-to-identify-kubernetes-pods-running-root)
> 来源:Fairwinds Insights 对数百个生产级集群的调查结果

配置不一致的负面后果非常重要，不容忽视:

*   安全漏洞:错误配置会导致权限提升、易受攻击的映像、来自不可信存储库或容器的映像以 root 用户身份运行。69%的公司由于 Kubernetes 的错误配置发生过安全事故。来源:[企业项目](https://enterprisersproject.com/article/2020/6/kubernetes-statistics-2020)。

*   效率:由于使用了太多的资源或者陈旧的工作负载没有得到清理，成本可能会攀升得太高。45%的容器使用<30% of requested Memory (inefficiency). Source: [DataDog 容器报告](https://www.datadoghq.com/container-report/)。

*   可靠性:可能存在围绕活动或就绪性调查的可靠性问题，或者可扩展性问题(由于缺乏 PodDisruptionBudgets 或 HorizontalPodAutoscalers，扩展不够或扩展过于频繁)导致您的应用程序或服务停机。

> **29%的部署缺乏就绪性调查**
> 来源:Fairwinds Insights 对数百个生产级集群的调查结果

手动识别这些错误配置非常容易出错，并且会很快让运营团队不堪代码审查的重负。

### **技术影响**

*   未解决的容器漏洞

*   过度许可的部署

*   缺少健康探测器

*   不适当的资源请求和限制

### **业务影响**

*   安全风险增加

*   应用停机时间

*   云成本超支

*   不合规

## **为什么实施一致的配置模式对 Kubernetes 至关重要**

随着团队在整个组织中部署 Kubernetes，DevOps 团队不可能手动编写或审查进入集群的每个 docker 文件和 Kubernetes 清单。这就是实施配置模式对 Kubernetes 的成功至关重要的地方。如果没有一套一致的标准，工程团队将不可避免地产生安全漏洞，过度消耗计算资源，并引入嘈杂的工作负载。然后，DevOps 团队很快耗尽精力来响应页面和灭火，几乎没有时间对基础架构进行实质性改进。升级也可能成为一个主要问题。组织在可避免的错误配置和意外中断上浪费金钱。

## 如何加强与 Kubernetes 政策的一致性

当与一个工程师团队管理多集群环境时，创建一致性需要您建立 Kubernetes 策略来加强安全性、效率和可靠性。那么，如何加强一致性呢？实现一致配置的一个关键工具是 [Kubernetes 策略](/kubernetes-policy-enforcement)。策略和策略实施工具允许您定义部署的操作防护，以及特定的法规遵从性和安全性要求。这些策略通常被称为策略即代码，使平台工程和运营团队能够围绕一组通用的规则、模式和最佳实践与其他利益相关方协作，以供 IT 和工程团队遵循。

我们撰写了一篇[论文](/kubernetes-policy-enforcement)，概述了在 Kubernetes 中执行政策所面临的挑战以及不执行政策的负面后果。它详细介绍了哪些策略是必不可少的以及如何创建它们，并提供了如何实施策略的信息。

本白皮书非常适合管理多用户、集群和租赁环境的工程和 DevOps 领导者。

寻找一个完整的 Kubernetes 治理平台？Fairwinds Insights 是免费的。今天就开始吧。

[![New call-to-action](img/154350e87174c62c0f2d20e8a08d9722.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/397d414a-d137-4f8e-9cfc-f6f2363bf2ad)