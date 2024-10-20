# 如何识别 Kubernetes 中缺失的就绪性探针

> 原文：<https://www.fairwinds.com/blog/how-to-identify-missing-readiness-probes-in-kubernetes>

 Kubernetes 提供了两种类型的健康检查:就绪性探测和活性探测。

准备就绪探测器旨在确保应用程序已达到“就绪”状态。在许多情况下，在 web 服务器进程启动和准备接收流量之间有一段时间。

> 29%的 Kubernetes 部署缺乏就绪性调查
> 来源:Fairwinds Insights 对数千个受监控部署的调查结果

如果准备就绪探测器丢失，Kubernetes 不知道您的 pod 是否完全准备好接收流量。pod 可能会在准确处理请求之前收到请求。准备就绪探测可以确保流量在实际准备好接收流量之前不会被发送到 pod。

## 如何识别缺少就绪性探测的应用

遗漏准备就绪探测可能是应用程序可靠性的一个问题。手动审核这些探测器的所有 pod 可能非常耗时。当用户复制和粘贴 YAML 时，或者如果没有现有的流程来验证就绪性探针，这也是必须重新考虑的事情。

Fairwinds Insights 是一个策略驱动的配置验证平台(社区版本可以免费使用)，它允许负责 Kubernetes 的团队识别缺失的就绪性探针，并确保从开发到生产都设置了活动和就绪性探针。

> 您可以永远免费使用 Fairwinds Insights。 [拿到这里](https://www.fairwinds.com/coming-soon) 。

作为一款 SaaS 解决方案，Fairwinds Insights 会自动扫描集群，以检查就绪探测器缺失的部署。您可以将时间花在包含这些探针上，而不是花在识别缺失探针上。

您可以通过 [创建账户](https://insights.fairwinds.com/auth/register) ，创建集群 [安装代理](https://www.youtube.com/watch?v=QYwNmtJc5no) 来免费试用 Fairwinds Insights。我们提供了一个舵图表，允许您定制您的安装。

一旦安装了 Fairwinds Insights 代理，您将在 5-10 分钟内获得结果。您可以轻松地检查是否缺少活动探测器以及其他健康检查，如图像拉取策略未设置为“始终”或缺少标记。

## 防止遗漏就绪或活性探测

通过在您的 CI/CD 流程中使用 Fairwinds Insights，您可以确保您的策略即代码(OPA 策略)与开箱即用检查一起实施，以防止遗漏就绪性或活性探测。您可以使用它:

*   作为 CI/CD 挂钩，作为代码评审过程的一部分，审计基础设施即代码

*   作为准入控制器(又名验证 Webhook)，它将阻止有问题的资源进入集群

*   作为集群内代理，重复扫描有问题的资源

Fairwinds Insights 可以采用相同的 OPA 策略，并将它们联合到所有三个上下文，以及您想要的任意多个集群。

使用 Fairwinds Insights 将显著改善从 CI/CD 到生产的集群运行状况。策略驱动的配置验证平台可确保整个组织都遵循可靠性最佳实践。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)