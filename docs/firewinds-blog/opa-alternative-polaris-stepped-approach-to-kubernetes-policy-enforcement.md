# OPA 替代方案:Kubernetes 政策执行的分步方法

> 原文：<https://www.fairwinds.com/blog/opa-alternative-polaris-stepped-approach-to-kubernetes-policy-enforcement>

 最近，我们围绕开放策略代理(OPA)进行了多次对话，Kubernetes 用户对此很感兴趣，并希望使用它，但他们试图找出最佳的初始策略，以及是否有一种不需要编写减压阀的更简单的方法。为了帮助解决这个问题，我们在这里讨论了更多关于 OPA 和一个替代的开源项目，它可以帮助您执行 Kubernetes 策略。

## **OPA 背景**

开放策略代理(OPA)是一个开源的通用策略引擎，它在整个体系中统一策略实施。它提供了一种高级声明性语言，允许您将策略指定为代码，并使用简单的 API 从您的软件中卸载策略决策。OPA 使您能够在微服务、Kubernetes、CI/CD 管道、API 网关等方面实施策略。您可以在 [OPA 文档](https://www.openpolicyagent.org/docs/latest/)中了解更多信息。

正如其创始人之一所解释的,“OPA 为策略提供了自己的生命周期和工具集，因此策略可以与应用策略的底层系统分开管理...OPA 提供本地执行，是为了比硬编码的服务逻辑或特定领域语言更高的可用性、更好的性能、更大的灵活性和更强的表达能力。”

## **OPA 的优势**

OPA 允许用户跨基础设施和应用程序设置策略。一些例子包括定义:

*   哪些用户可以访问哪些资源。
*   允许哪些子网的出口流量。
*   工作负载必须部署到哪些集群。
*   可以从哪些注册表下载二进制文件。
*   容器可以执行哪些操作系统功能。
*   一天中的哪些时间可以访问系统。

OPA 还具有上下文感知能力，因此管理员可以根据正在发生的事情(当前中断、新漏洞发布等)做出策略决策。

## **逐步实施政策的方法**

OPA 提供了很多好处，但是它可能是重量级的，因此一些用户可能需要轻松地执行策略。作为 Kubernetes 策略执行的一个分步方法，我们使用了一个开源项目 [Polaris](https://github.com/FairwindsOps/polaris) 。今年早些时候发布了 1.0 版本，该项目正在走向成熟，并被用于多个产品，包括 [Insights](//www.fairwinds.com/insights) 和 [Starboard](https://github.com/aquasecurity/starboard) 。

Polaris 通过运行十几种不同的检查来识别 Kubernetes 部署错误配置，以帮助用户发现经常导致安全暴露、中断、扩展限制等的 Kubernetes 配置。

北极星寻找的一些示例检查包括:

*   是否为 pod 配置了就绪或活动探测器
*   当图像标签或者未指定或者 `latest.`
*   当 `hostNetwork` 或 `hostpath` 属性被配置时。
*   当 `resources.requests.cpu,` `resources.requests.memory,` `resources.limits.cpu` 或 `resources.limits.memory` 属性未配置时。
*   当 `securityContext.privileged` 为真或者当 `securityContext.readOnlyRootFilesystem` 不为真时(在多个其他安全配置检查中)

Polaris 允许您检查常见的错误配置，而无需构建自己的策略。如果您想添加自己的定制策略和检查，您可以使用简单的基于 JSON 模式的语法来编写自己的策略和检查。当您知道需要执行策略，但不需要 OPA 提供的所有灵活性时，这是一个很好的起点。

北极星可以在三种不同的模式下运行:

*   作为一个仪表板，这样您就可以审计集群中正在运行的内容。
*   作为一个验证性的 webhook，您可以自动拒绝不符合组织策略的工作负载。
*   作为命令行工具，因此您可以测试本地 YAML 文件，例如作为 CI/CD 过程的一部分。

你可以在 GitHub 上查看[快速入门。](https://github.com/FairwindsOps/polaris)

在 Fairwinds，我们正致力于通过 Fairwinds Insights 简化 OPA 的使用。Fairwinds Insights 可供免费使用。你可以在 这里 [报名。](https://www.fairwinds.com/coming-soon)

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)