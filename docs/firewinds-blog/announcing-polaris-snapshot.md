# 宣布北极星快照

> 原文：<https://www.fairwinds.com/blog/announcing-polaris-snapshot>

Two months ago at Fairwinds  (formerly ReactiveOps), we launched Polaris, an [open source dashboard](https://github.com/FairwindsOps/polaris) for auditing Kubernetes workload configuration. We saw a great response from the community, and have been hard at work keeping up with all the interesting ideas and use cases that have come in since launch.

例如，一个常见的反馈是，虽然 Polaris 非常擅长审计实时集群中的工作负载，但许多人希望能够在问题被签入他们的基础架构即代码存储库之前发现问题。所以我们修改了 Polaris 来审计本地 YAML 文件，并添加了几个新的标志来简化 CI/CD。现在您可以在您的 CI/CD 管道中运行这样的东西:

`polaris --audit-path ./deploy --set-exit-code-on-error -set-exit-code-below-score 90`

如果 Polaris 检测到任何错误级别的问题，或者如果您的 Polaris 得分下降到 90%以下，管道将会失败。

However, there was one useful feature we found difficult to implement: an easier way to share Polaris reports. To help address this need, we’re excited to announce a new service: [Polaris Snapshot](https://fairwinds.com/polaris-snapshot). Polaris Snapshot will let you generate a report, hosted at polaris.fairwinds.com, that you can share with your team. We see this as a great way for teams to sync on how to make their Kubernetes workloads more stable, efficient, and secure.

## 为什么是北极星？

在 Fairwinds，我们与几十个开发团队合作，将数百个应用程序发布到 Kubernetes 集群中。通常，我们承担与集群本身相关的工作 (例如 处理集群升级，维护工具，如入口和证书管理，响应中断)，而开发团队负责需要应用上下文的一切 (例如 编写部署配置，设置 CI/CD)。但是对于不完全熟悉 Kubernetes 的开发人员来说，很容易忽略应用程序的 Kubernetes 配置的一些关键部分。

例如，您的部署在没有准备就绪和活动探测的情况下，或者在没有资源请求和限制的情况下，可能看起来工作得很好。但是如果没有这种配置，Kubernetes 就很难做它最擅长的事情——确保您的应用程序保持健康并高效地伸缩。我们帮助团队应对的许多中断和性能下降都是由一些问题引起的，这些问题可以通过构建更坚固的应用程序配置来轻松避免。

北极星旨在缓解平台工程团队和应用开发团队之间的这种交接。 我们专门针对可能导致扩展性、稳定性、效率和安全性问题的应用配置问题。Polaris 得分每增加一分，您经历停机、安全漏洞或性能下降的机会就会减少一分。

## 为什么要拍北极星快照？

一旦我们完成了 Polaris，是时候在我们客户的集群中实际运行它了。在会议前设置一个临时仪表板很容易，但是我们不想让一个不必要的流程在每个客户的环境中运行，不管它有多小和多便宜。并且报告会根据新的部署而改变的事实可能会导致误解。

我们需要的是一种生成一次性报告的方式，即特定时间点的集群状态快照。这份报告可以被保存、传阅和讨论，因为我们制定了一个计划来优先处理 Polaris 发现的问题。

不幸的是，在 Kubernetes 中安全地维护持久状态 c 可能会很复杂，所以像这样的特性似乎超出了开源项目的范围。我们尝试简单地将仪表板保存为 PDF 格式，并展开每个警报。结果还可以，但是对于一些客户来说，导致报告长达几十页，很难浏览。

最后，我们选择了北极星快照作为发布北极星报告的方式。这不仅解决了我们眼前的问题——它还推进了 Fairwinds 的使命，使组织能够轻松地操作生产级 Kubernetes 集群。

**它是怎么工作的？**

北极星快照运行于(注: 您需要输入您的电子邮件地址以 生成报告。虽然我们不能为您提供免费午餐，但我们承诺尊重您的收件箱)。当您到达时，Snapshot 将为您生成一个新的会话，它是一个长的、唯一的 ID。然后你会看到一个kubectl命令运行，类似于:

`kubectl apply -f https://polaris.fairwinds.com/session/e0475993dc407007418319ca468d25ff/yaml`

该命令将下载北极星，运行一次，将结果发送回，然后自行卸载。我们建议在将 YAML 文件传递给ku bectl apply之前检查它——您会看到 Polaris 只需要最低限度的 RBAC 权限，并且在一个临时的、唯一的名称空间中运行。

一旦审计完成并且 Polaris 从您的集群中卸载，您的报告结果将自动出现在 https://polaris.fairwinds.com/session/YOUR_SESSION_ID。您可以与您的队友分享此链接，以帮助协调和优先考虑补救工作。该网址将在 90 天内保持有效，之后我们将完全删除您的数据。您也可以随时手动删除数据。

## 未来工作

We’ve got big plans for Polaris in the future. We’ve just published the [Q3 Roadmap](https://github.com/FairwindsOps/polaris/blob/master/ROADMAP.md#q3-2019), which includes a few exciting new features:

*   我们将开始检查更多类型的资源，而不仅仅是部署——我们将支持 StatefulSets、Jobs、CronJobs 和 DaemonSets。
*   我们将添加对豁免的支持，这将允许您指定，例如，您的一个部署确实需要以 root 用户身份运行。如果你给它一个豁免，这将不再影响你的北极星分数。
*   我们将在 探讨让你创建 自己定制北极星支票的方法

We’re also quite interested in expanding [Polaris Snapshot](/polaris-snapshot) to include features that are difficult to implement as part of an open source project - things like saving report history, multi-cluster management, and email/Slack alerts for degradations. If you’re interested in Polaris and would like to help us plot the course forward, reach out to opensource@fairwinds.com or get in touch [on GitHub](https://github.com/FairwindsOps/polaris)!