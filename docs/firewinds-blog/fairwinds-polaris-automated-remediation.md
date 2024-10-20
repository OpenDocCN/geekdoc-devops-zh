# “北极星”自动补救的实际应用

> 原文：<https://www.fairwinds.com/blog/fairwinds-polaris-automated-remediation>

 Fairwinds 的核心任务是让使用 Kubernetes 的团队能够以更低的风险更快地发布应用程序。平台工程领导者处于这一领域的最前沿:他们的工作是为下游应用团队提供获得成功的正确工具。然而，要在多个应用团队和集群中大规模实现这一点，平台工程师必须投资于自动化和 [Kubernetes guardrails](https://www.fairwinds.com/blog/what-are-kubernetes-guardrails) ，以便向 DevOps 工程师提供关于其配置更改的安全性、成本效益和可靠性的持续反馈。

Polaris 是平台工程团队赖以获得持续反馈和防护的工具之一。它目前以不同的模式运行:

*   作为集群内控制面板，突出显示工作负载最佳实践或需要改进的领域

*   作为验证准入控制器，检查请求并确定是否应该部署它们

*   作为 CI/CD 平台的基础设施代码审计工具

我们很高兴地宣布，我们已经为北极星增加了自动修复功能。更具体地说，Polaris 准入控制器现在包括一个可变的 webhook，它可以修改请求——比如基于特定的策略标准添加、更改或删除对象。[了解其工作原理](/blog/how-polaris-kubernetes-mutations-work)。

## 变异准入控制器的实际使用案例

变异准入控制器可用于自动将最佳实践应用于每个部署，使团队能够更快地前进。然而，改变准入控制器的一个缺点是潜在的配置漂移，即当 git repo 中的基础设施代码与运行时状态不匹配时。尽管如此，在某些情况下，这种折衷可能是合适的。以下是我们从 Polaris 社区中了解到的三个变异准入控制器的实际用例:

1.  保证 Kubernetes 的最佳实践:例如，在某些情况下，您可能希望确保将[图像拉取策略设置为‘Always’。](https://www.fairwinds.com/blog/kubernetes-how-to-ensure-imagepullpolicy-set-to-always)如今，当工程师忽略设置正确的图像拉取策略时，Polaris 可以在拉取请求阶段提醒他们。然而，变异的准入控制器可以保证期望的图像拉取策略，即使工程师忽略了进行改变。

2.  为成本分配应用标签:改变准入控制器还可以确保您的集群工作负载被正确标记，而不会减慢开发人员的速度。例如，[fair wind Insights](https://www.fairwinds.com/kubernetes-cost-optimization)等成本分配解决方案通过标签报告工作负载的成本，因此应用正确的标签是通过相关业务维度了解 Kubernetes 成本的关键。

3.  减轻安全威胁:今天，Polaris 将报告可能被过度许可或以不安全的配置运行的工作负载。然而，一个自动将您的工作负载设置为以非根用户身份运行的突变策略可以帮助平台和安全团队减少像 [CVE-2021-25741](https://www.fairwinds.com/blog/kubernetes-cve-symlink-exchange-identify-containers-running-as-root) 这样的漏洞。

## 最后一件事…

北极星也使得将突变应用于原始 YAML 文件成为可能。这有助于团队在“编码”阶段减少配置偏差——从一开始就自动生成包含特定最佳实践的 Kubernetes YAML。最终，像这样的解决方案可以帮助组织节省时间和金钱——特别是在大规模部署像 [Fairwinds Insights](http://fairwinds.com/insights) 这样的 SaaS 平台时。

## 资源

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)