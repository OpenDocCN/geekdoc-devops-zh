# 如何识别超限集装箱

> 原文：<https://www.fairwinds.com/blog/how-to-identify-over-permissioned-containers>

 过度许可的容器具有主机的所有根功能。容器可以访问普通容器中无法访问的资源。虽然可能有一些这样的用例，例如在容器中运行守护进程，但是过度许可/特权容器会破坏隔离。容器不是与它正在运行的主机隔离，而是获得对主机的资源和设备的访问。对于大多数容器，您希望避免这种情况，这样容器就不能:

## 如何识别超限集装箱

识别 Kubernetes 集群中的特权容器需要时间和资源。Fairwinds Insights 是一个策略驱动的配置验证平台(社区版可以免费使用)，它允许负责 Kubernetes 的团队识别特权容器，并从一开始就阻止特权容器的部署。

> Fairwinds Insights 社区版永远免费使用。在此注册[即可试用完整版 30 天。在 GKE、阿拉斯加或 EKS 进行测试，或者在测试集群上运行。](https://insights.fairwinds.com/)

Fairwinds Insights 是 SaaS 的一个解决方案，它自动扫描集群以检查特权容器。您的团队节省了识别和跟踪特权容器的时间，并能够利用这些时间来补救问题。

通过舵图 [创建账号](https://insights.fairwinds.com/auth/register) ，创建集群 [安装代理](https://www.youtube.com/watch?v=QYwNmtJc5no) 可以免费试用。

一旦安装了 Fairwinds Insights 代理，您将在 5-10 分钟内获得结果。您可以轻松检查设置了特权字段的容器，以及其他安全事件，如可写文件系统、作为根用户运行的容器进程和易受攻击的映像。

## 首先防止特权容器

Fairwinds Insights 是由政策驱动的。通过在整个部署过程中使用它，您可以确保您的策略即代码(OPA 策略)得到实施。您可以使用它:

*   作为 CI/CD 挂钩，作为代码评审过程的一部分，审计基础设施即代码

*   作为准入控制器(又名验证 Webhook)，它将阻止有问题的资源进入集群

*   作为集群内代理，重复扫描进入集群的有问题的资源

Fairwinds Insights 可以采用相同的 OPA 策略，并将它们联合到所有三个上下文，以及您想要的任意多个集群。

> 对使用 Fairwinds Insights 感兴趣吗？免费提供！点击了解更多[。](/coming-soon)

使用 Fairwinds Insights 可以从 CI/CD 到生产环境扫描您的配置，从而显著降低安全事故的风险。策略驱动的配置验证平台可确保整个组织都遵循安全最佳实践。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)