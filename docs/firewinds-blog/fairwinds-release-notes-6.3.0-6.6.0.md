# Fairwinds 发行说明:6.3.0-6.6.0

> 原文：<https://www.fairwinds.com/blog/fairwinds-release-notes-6.3.0-6.6.0>

 我们一直像往常一样忙碌，升级我们的 Fairwinds Insights 软件的各种元素。但在分享这些改进之前，我们想花点时间强调一下我们最近对 AWS 的[计费准确性所做的一些更改。](/blog/you-can-now-accurately-measure-application-costs-on-kubernetes-using-your-aws-bill)

### **Kubernetes AWS 的计费准确性**

Fairwinds Insights 已经包含了 Kubernetes 的成本特性。我们帮助团队调整其应用程序的规模，以确保他们最大限度地利用可用资源，经济高效。我们最新的 Kubernetes 成本分配功能允许 AWS 客户使用他们的实际账单，按照工作负载、名称空间或标签来计算 Kubernetes 成本。这种集成比传统的云成本工具更深入，可以跨多个业务维度提供准确的、基于使用情况的成本数据。DevOps 团队可以满足服务所有者和财务的需求，而无需质疑底层节点定价假设。

> **阅读更多！** [您现在如何使用您的 AWS 账单在 Kubernetes 上准确测量应用成本](https://www.fairwinds.com/blog/you-can-now-accurately-measure-application-costs-on-kubernetes-using-your-aws-bill)

### **发行说明**

在 11 月，我们升级了几个软件元素，包括对 Helm 库的远程扫描的改进和对现有问题的许多错误修复。本博客将涵盖您需要了解的关于新版本 6.3.0-6.6.0 的一切，以及它们的进步将如何影响您的 Kubernetes 所有权的治理和安全性。

[fair winds Insights](https://www.fairwinds.com/fairwinds-insights-trial?utm_campaign=Fairwinds%20Insights%3A%20Release%20notes&utm_medium=email&_hsmi=2&_hsenc=p2ANqtz-8NSjSStDb8S0MyXLWD0AWZ_9QhupEwmfzpisIOjKSUtRweiyJ0xoetUWZszVE-4ZTQu1wlAVOjzrRxixa3bOXgqhCC0FCmQ3RGBCWhtOWe3WQqy_c&utm_content=2&utm_source=hs_email)在这里是为了确保您的组织继续发现正在进行的 [Kubernetes 安全](https://www.fairwinds.com/kubernetes-security) 监控、 [策略执行](https://www.fairwinds.com/enforce-kubernetes-policy) 、和 [成本优化](https://www.fairwinds.com/kubernetes-compliance) 。

### **6.3.0 扫描远程舵库**

组织现在可以从远程 helm 存储库中扫描图表来查找问题。通过在 fairwinds-insights.yaml 文件中提供名称、repo 和图表，Fairwinds Insights 现在可以从远程存储库下载图表并运行适当的扫描。

错误修复包括:

*   现在，修改自动化规则会显示哪个用户更新了它，以及何时更新的。
*   通过行动项目现在在健康记分卡上可见/可用。

### **6.4.0 启用和禁用准入控制器被动模式**

现在，用户可以通过 API 启用和禁用准入控制器的被动模式。你可以在这里 了解更多关于准入控制器被动模式 [。我们还更新了 Insights 代理，进行了一些小的改进和修复。**注意**:从这个版本开始，对于任何新的集群，接纳控制器默认设置为被动模式。](https://insights.docs.fairwinds.com/run/admission/setup/#installation)

### **6.5.0 准入控制器事件包括部署者的用户名**

当用户导航到准入控制器页面时，他们将看到包含进行部署的用户名称的事件。这种可见性有助于支持集群中多个团队/用户的集群管理员更好地了解是谁进行了特定的部署。

### **图像漏洞行动项目中可用的基本图像信息**

Insights 现在可以检测图像漏洞的基础图像层。您可以在映像漏洞的操作项描述中找到此信息，这有助于开发人员快速轻松地识别他们正在运行的基础映像。这允许开发人员检查是否有更新的版本可以修复任何报告的 CVE。

错误修复包括:

*   加快组织仪表板的加载速度
*   跨洞察的各种 UI 改进

### 6.6.0 全新设计的行动项目 UI！

用户现在可以在其组织的导航栏以及特定集群的导航栏下找到新的 Action Items 页面。该页面包括一个完全重新设计的 Action Items 表版本，具有一些新功能，例如隐藏和显示表列。

### **用“列表”跟踪重要的行动项目**

用户可以创建带有行动项目的列表，用于跟踪重要的行动项目以及解决这些项目的任何进展。

### **更好地管理系统级行动项目的新自动化规则**

创建了一个新的自动化规则，以便用“不会修复”解决方案清楚地标记某些名称空间中的操作项。默认情况下，此规则将对现有组织禁用，但对新组织启用。

错误修复包括:

*   没有实例的策略不再有加载和编辑问题
*   现在只允许管理员和编辑者集群更改工作负载的 QoS
*   具有可用修复的漏洞现在可以被正确过滤

您可以随时在此 查看发布说明 [的完整列表。](https://insights.docs.fairwinds.com/release-notes/?utm_campaign=Fairwinds%20Insights%3A%20Release%20notes&utm_medium=email&_hsmi=2&_hsenc=p2ANqtz-_lgp8r2KcXX2TXvtC2Jtizl9Xga1slQH8C_PsPwwVbdmJZd6O-gRleTd9W4zyR52z5er6_6xDp0-VVH2CIpMGykqz6ELYGDe1YijMG8SgcuMWzeQg&utm_content=2&utm_source=hs_email)

### **免费使用 Fairwinds Insights】**

如果您正在寻找 Kubernetes 治理和安全软件，Fairwinds Insights 是免费的。今天就开始吧。

你们中的一些人可能已经在使用我们的开源项目了。我们最近推出了一个开源用户组，我们一直希望有更多的人加入我们。 [在这里](https://cfvbp04.na1.hubspotlinks.com/Btc/I2+113/cfVbp04/VVVHms5Y6dgsV18gyd5KW-V0W6tdK5Q4zpNHgN7Mjfyw3q90JV1-WJV7CgH3QW54cSXs2JVHJQMPFXrq3pCY5W3J1qK96Y78_sW10CXtY4d6FN6W3km4v98dr92wW1rfrrK5R0F6WW2gyqZt34cfz-W5gFPQK5R7tHXW6qHTBZ4z-_5sW88NfJG46DV85W3F254p7Vz4VqW2mGbCT8P9x-DW3wTFNh74JZ41VRQ2Tm4cSyFbW3n8sxz2Jr16cW3f49hq1qwcdYW3XmB2-5dXqkqW6XNfJF3_q-q-W4WL-Kg6mF48sW8Gjz7z5PTTHPVZH0BS23RkfqW8TDc2m2GpW40W7smLZd5TYCHJN9dMbpy5vTTrVNZy_78q1rMXW6BX0mC4JYgMjW3rvDv78Nx3mCW7TrGhr3HF7G132YG1) 注册 Fairwinds 开源用户组，加入我们 12 月的下一次聚会吧！

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)