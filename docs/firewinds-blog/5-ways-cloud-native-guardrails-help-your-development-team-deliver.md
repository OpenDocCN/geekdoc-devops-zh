# 云原生护栏帮助您的开发团队交付的 5 种方式

> 原文：<https://www.fairwinds.com/blog/5-ways-cloud-native-guardrails-help-your-development-team-deliver>

 传统的治理方法，例如为 IT 服务和资产管理创建了一套详细实践的信息技术基础设施库(ITIL)方法，限制过多，最终减慢了开发团队的速度。这种模式在云原生环境中适得其反，平台工程师和开发人员对采用新的治理模式持谨慎态度是可以理解的，他们认为这可能会在快节奏的开发和交付环境中减慢交付速度。

随着组织越来越多地采用 [Kubernetes](http://kubernetes.io/) 来快速构建和交付云原生应用，他们了解他们正在部署的技术的战略重要性，以及管理成本和符合整体业务需求的需求。那么，他们如何采用护栏来保持一切顺利运行，而不在他们的开发团队面前设置路障呢？如果云原生治理实际上更像我们在弯曲山路上的护栏会怎么样。您可能永远不需要它们，但它们就在那里，阻止您坠入悬崖——或者在软件开发中，部署带有安全漏洞和错误配置的代码，违反合规性，并可能导致过高的云成本。

Kubernetes 生态系统固有的 [治理护栏](https://www.fairwinds.com/blog/what-is-kubernetes-governance) 使组织能够跨云原生基础架构环境以及其上部署的应用和服务创建、管理、部署和实施策略。通过将 [声明性](https://www.fairwinds.com/blog/gitops-best-practices-and-the-kubernetes-guardrails-you-need) 和自动化治理放在适当的位置，平台工程师可以使开发人员能够自助服务并更容易地满足业务计划。方法如下:

## 1。敏捷、集成的软件开发和部署

每个人都已经看到敏捷方法是如何加速软件开发和部署的。与 Kubernetes 集成的云原生 [护栏](https://www.fairwinds.com/kubernetes-guardrails-explained-reg) 部署在整个 Kubernetes 应用生命周期中，从第 0 天(规划)到第 1 天(部署)再到第 2 天(全面生产)。在这些阶段的每一个阶段都应用并执行不同的策略，因此所采用的任何治理解决方案都必须与 CI/CD 工具相集成。

Kubernetes 和治理解决方案之间的集成为平台工程和 DevOps 团队提供了在整个软件开发生命周期(SDLC)中维护策略合规性的能力，而无需手动干预和频繁的代码审查。策略还可以应用于 [基础设施配置](https://www.fairwinds.com/blog/why-infrastructure-as-code-scanning-matters-for-kubernetes-configuration) 以及直接影响应用程序开发人员的应用程序特定问题。

## 2。法规合规性

监管可能是一个持续的挑战，因为开发人员要努力确保他们部署的应用程序和服务能够正确处理数据并安全部署。无论一个组织是否必须满足财务法规，如 [、萨班斯奥克斯利](https://www.soxlaw.com/) 或 [PCI DSS](https://www.pcisecuritystandards.org/) 、医疗法规( [HIPAA](https://www.hhs.gov/hipaa/index.html) )或数据隐私法规([【GDPR](https://gdpr-info.eu/))，总有需要满足的要求。云原生防护栏包括许多支持合规性和云配置策略的声明性策略语言。这些护栏还可以自动跟踪政策合规性，使团队更容易遵守不断变化的法规，并跟踪监管机构的合规性。

## 3。威胁表面的可见性:云、SaaS、PaaS、&(？)aaS

平台和 DevOps 团队可以使用云原生防护栏在云中和内部适当地自动识别 Kubernetes 环境中的 [漏洞](https://www.fairwinds.com/blog/mitigate-kubernetes-risk-with-vulnerabilities-explorer) 和错误配置。这些工具还可以根据需要向开发人员提供补救建议，并识别任何已识别问题的关键程度。这些信息让开发人员能够放心地进行自助服务，因为他们知道护栏已经就位。

从第 0 天到第 2 天，Cloud native guardrails 还可以通过监控所有集群的安全错误配置来自动执行许多安全任务。云原生治理平台可以在扩展到多个集群和团队时持续配置 Kubernetes。这使得平台团队能够自动识别 Kubernetes 生命周期中的错误配置，简化了发现和修复漏洞和错误配置的过程，即使他们的 K8s 环境变得更加复杂。

## 4。成本效率

管理云和 Kubernetes 成本对大多数组织来说都是一项挑战。CNCF 针对 Kubernetes 的 FinOps 报告“[Kubernetes 成本监控不足或不存在导致超支](https://www.cncf.io/wp-content/uploads/2021/06/FINOPS_Kubernetes_Report.pdf) ”显示，在过去一年中，68%的受访者的 Kubernetes 成本增加，其中一半人的成本增加超过 20%。由于团队难以监控和预测 Kubernetes 的支出，这些不断上升的成本变得更加复杂。Cloud native guardrails 可以持续监控 Kube 集群效率，帮助团队设置适当的请求和限制，以确保团队能够以最低的支出实现最大的可靠性。它还可以自动配置 Kubernetes 并一致地应用策略，以确保即使在团队扩展时，成本效率和优化也能继续得到适当的控制。

## 5。一致性和可靠性

guardrails 治理方法依赖于将 [策略](https://www.fairwinds.com/polaris) 表示为声明性元数据的能力。当平台工程师使用以公共语言通信的工具和系统构建内部开发平台时，它简化了这种策略的表示和实施。现代声明性策略语言对于支持软件开发人员自助服务是不可或缺的，因为它们在执行组织策略的底层系统和工具的框架内工作。

云原生防护栏帮助平台工程师和开发人员调整应用的大小，以便根据使用情况适当调整云资源请求和限制。正确的解决方案可以帮助团队了解 Kubernetes，不仅可以分配资源，还可以将成本归因于工作负载和团队。这些信息通过优化工作负载的可靠性和可伸缩性，帮助团队达到应用程序的可用性标准。

## 采用云端原生护栏

虽然过去的治理模型阻碍了速度和敏捷性，但是软件开发和技术的新方法需要集成到 Kubernetes 环境中而不降低交付速度的解决方案。保持安全性、合规性和成本一致的云原生防护栏，不会给软件开发人员带来成为 Kubernetes 专家或要求平台工程师充当 Kubernetes 服务台的负担，可以实现这些目标。[fair winds Insights](https://www.fairwinds.com/insights)为当今的平台工程和软件开发团队提供云原生护栏。尝试一下 [免费层](https://www.fairwinds.com/blog/get-started-with-fairwinds-insights-free-tier) (可用于多达 20 个节点、两个集群和一个 repo 的环境)，看看其内置和定制的 Kubernetes 最佳实践如何在降低风险的同时改善开发人员体验。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)