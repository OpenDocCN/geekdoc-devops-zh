# Kubernetes 大规模修复:可见性与行动

> 原文：<https://www.fairwinds.com/blog/kubernetes-remediation-at-scale>

 当我与各种规模的组织一起工作时，我遇到的一个主题是可见性不等于行动。Kubernetes 提供了很多功能，但也需要很多配置。仅仅因为你能看到这一点，并不意味着你能付诸行动。今天我要谈谈 Kubernetes 的大规模修复以及可见性和行动之间的区别。

## 开源很棒

整个云原生社区都是建立在开源技术之上的。我们正在使用 Kubernetes、Prometheus、etcd、Helm、Linkerd、Open Policy Agent 以及许多其他已毕业和正在孵化的项目。

这些开源技术非常棒，我们很高兴在 Fairwinds 开发了许多[开源工具来帮助社区。当您在多个集群上运行这些工具时，挑战就来了。你一直在做吗？您是否正确配置了工具？大规模开源可能是一个挑战，因此虽然您可以使用这些工具来获得可见性，但您很可能会浪费时间将数据集链接到多个集群和工作负载上。尽管如此，这只是可见性的一步。](https://www.fairwinds.com/open-source-software)

## Kubernetes 可见性与补救

当您运行一个多集群环境，并且可能跨多个云运行时，您如何知道发生了什么？我们采访的许多平台工程师和 DevOps 领导都花时间审计集群，以确定是否存在 Kubernetes 的错误配置或安全问题。这些人花时间手动检查和修复问题，甚至更糟，成为“Kubernetes 帮助台”通常在多集群环境中，他们会运行多个重叠的工具或构建不同的仪表板，只是为了弄清楚发生了什么。然而，这很费时间，而且不是每件事都做得好或正确。

大规模运行 Kubernetes 时所需的可见性很重要，但更重要的是大规模修复的能力。

## 需要什么？

工程时间被拉长了。没有时间去开发另一个不集成到工程师日常工作流程中的工具。我们需要的是一个 Kubernetes 治理解决方案，它能够提供可见性、补救建议，并集成到工程师和开发人员已有的工作方式中。

Fairwinds Insights 是一个 Kubernetes 治理平台，它正是这样做的。它在 CI/CD 上扫描，并在任何 Kubernetes 群集中持续扫描，以确定哪里存在配置错误、安全风险、浪费的云支出或性能挑战。Fairwinds Insights 的强大之处在于它与您的团队已经使用的第三方工具的集成。

例如，当 [log4j CVE 宣布](https://www.fairwinds.com/blog/how-fairwinds-insights-can-help-you-identify-log4j-container-vulnerabilities)时，Fairwinds Insights 平台扫描集装箱，并能够识别包括 log4j 在内的 cv。如果某个容器存在风险，Insights 会创建一个行动项，并将严重性提高到“严重”——表示与 log4j 相关的风险。但是如果安全工程师或首席开发人员那天没有登录呢？他们是如何知道这个漏洞的？

这就是为什么 Insights 与吉拉、Slack 和许多其他工具进行了本机集成。当识别出严重或关键的问题时，用户可以设置规则，以便将按集群或名称空间的操作项转化为吉拉票据或松弛通知。然后，工程师和开发人员无需改变工作方式，即可轻松解决问题(Insights 提供的解决建议将出现在吉拉票据或 Slack 票据中)。

> 对使用 Fairwinds Insights 感兴趣吗？免费提供！ [在此了解更多](https://www.fairwinds.com/coming-soon) 。

这一步将问题转化为行动，这对于任何想要大规模运营 Kubernetes 的组织来说都是必不可少的。目标是将问题转化为可管理的小步骤，无论问题可能是什么:降低安全风险、优化成本或提供护栏。

## 开发与发展的统一

使用像 Fairwinds Insights 这样的 Kubernetes 治理软件不仅可以将可见性转化为行动，还可以将开发人员和开发团队团结起来。DevOps 团队不再需要像帮助台一样行动。相反，他们为开发人员提供了修复应用程序和集群中的问题所需的信息。它真正包含了 Kubernetes 服务所有权模型，开发者可以“编码、运输和拥有它”

[![A Complete Guide to Kubernetes Service Ownership](img/4a2f8c5555ec3fc5fb87af12f68d2123.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/714735e4-a30a-4cc7-bfd6-b8b9aa67b341)