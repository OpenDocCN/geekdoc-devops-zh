# 如何识别 Kubernetes Pods 是否以 Root 用户身份运行

> 原文：<https://www.fairwinds.com/blog/how-to-identify-kubernetes-pods-running-root>

 用 root 访问权限过度授权 Kubernetes 部署通常更容易使某些东西正常工作，但不推荐这样做。它会导致安全问题和过度特权用户。虽然这在开发中可能没问题，但在生产中却是个大问题。随着越来越多的 pod 被创建，您可能会不知不觉地以 root 身份运行许多 pod。

## **如何识别 Kubernetes pods 是否以 root 用户身份运行**

让个人贡献者设计他们自己的 Kubernetes 安全配置几乎可以确保不一致和错误。这通常不是有意发生的，通常是因为工程师专注于让容器在 Kubernetes 中运行。不幸的是，许多人在过程中忽略了重新审视配置，导致了[安全性](https://www.fairwinds.com/blog/kubernetes-best-practices-for-security)和[效率](https://www.fairwinds.com/blog/kubernetes-best-practice-efficient-resource-utilization)的差距。

负责安全的平台团队可以尝试手动检查每个 pod，以检查错误配置的部署。但是许多 DevOps 团队人员不足，没有足够的带宽来手动检查由各种工程团队引入的每个变更。

这就是为什么我们创建了 [Fairwinds Insights](https://insights.docs.fairwinds.com/) ，这是一个配置验证平台，它集成了可信的开源工具，以便团队可以自动扫描集群来检查错误配置。它节省了时间，降低了安全风险。

> “我们使用 Fairwinds Insights 作为集群的整体监控工具。它将我们所有的警报和安全性整合在一个地方，有助于减少发现问题所需的资源。”Boxed 公司首席开发工程师 Brent Jaworski
> 
> 阅读[案例研究](https://www.fairwinds.com/case-studies/Boxed)

## **Fairwinds Insights 为您提供配置验证**

Fairwinds Insights 是一个工具，它向您显示您的团队在 Kubernetes 的哪些地方配置错误。然后，它会提出改进建议，并帮助跟踪和确定修复的优先级。

你可以通过[创建账号](https://insights.fairwinds.com/auth/register)，创建集群[安装代理](https://www.youtube.com/watch?v=QYwNmtJc5no)来免费试用。我们提供了两个代理选项:一个 Helm chart(这允许您定制您的安装)或一个 kubectl 命令。

## **检查集群的安全状况**

一旦安装了 Fairwinds Insights 代理，您将在 5-10 分钟内获得结果。现在，您可以轻松检查集群的安全状况。这里有一个关于它如何工作的快速视频。

使用 Fairwinds Insights 将显著降低生产中的安全事故风险。配置验证平台确保在整个组织范围内遵循安全最佳实践。

[![Fairwinds Insights | Managing Kubernetes Configuration for Security, Efficiency and Reliability ](img/b1ac50ea4fc469770b0daa1ea986820e.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/166939eb-9ebe-4094-a151-45bce5bd4f2f)