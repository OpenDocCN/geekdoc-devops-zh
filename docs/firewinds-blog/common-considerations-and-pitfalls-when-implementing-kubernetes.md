# 实现 Kubernetes 时的常见注意事项和陷阱

> 原文：<https://www.fairwinds.com/blog/common-considerations-and-pitfalls-when-implementing-kubernetes>

 Kubernetes 是一个全新的世界。许多组织一直在运行他们自己的服务器，并且习惯于能够走过数据中心的地板来拨动开关；或者也许他们已经在云中呆了一段时间，但是他们仍然在处理那些他们像宠物一样对待的实例。但是 Kubernetes 不一样。在你开始之前，有些事情你需要知道。在这里，我们讨论了在实现 Kubernetes 时需要注意的所有陷阱。

## **做同一件事的许多方法**

当我开始使用 Kubernetes 时，让我感到惊讶的是你需要以多么不同的方式来思考它。即使我有几年的 AWS 和云计算经验，Kubernetes 也感觉像一门外语，为了理解它，我需要改变我对计算的想法。

最大的问题是，建立 Kubernetes 集群有这么多不同的方法，而且有这么多不同的插件做着大致相同的工作。在第一次建立集群之前做大量的研究是很重要的；否则，你可能会发现一些约束，要求你拆掉一切，重新开始。

## **立方“喇叭”**

Kubernetes 的困难之一是炒作。有如此多的信息可用，从大型企业分享他们的战争故事，到业余爱好者在地下室的三个树莓派上站成一团，写下 Kubernetes 如何改变世界。

关注那些在生产环境中使用安全、高效和可靠的配置大规模运行工作负载的人的观点非常重要。很难找到真正权威的人，所以要小心你从哪里得到信息。你需要做大量的研究。试着把注意力集中在有意义的地方。

## **Kubernetes 是短暂的**

系统管理员吹嘘服务器有 17 年正常运行时间的日子已经一去不复返了。Kubernetes 世界的一切都是短暂的，意思是短暂的。工作负载本身是短暂的，但是我们甚至鼓励您将集群级别的工作负载视为完全短暂的。您应该能够毫无问题地销毁和重新创建您的集群。

## **立方 vs ECS，Heroku？**

在云中运行工作负载有许多不同的方式。为什么选择 Kubernetes 而不是亚马逊的 ECS 或 Salesforce 的 Heroku？

主要答案是 Kubernetes 是一个开源项目，这意味着您可以免费使用它，修改它以适应您的需求，并且永远不必担心它会过时或被锁定。开源精神培育了一个丰富的 Kubernetes 扩展生态系统；几乎所有安装在 Kubernetes 上的核心插件都是免费和开源的。

此外，Kubernetes 可以在任何主要的云上运行，也可以在本地运行，帮助您避免局限于特定的工作方式或特定的服务提供商。Kubernetes 真的意味着每个人的一切。这是一个非常灵活的框架——现有的模式和附加组件将允许您在 Kubernetes 之上实现一个无服务器架构或类似 Heroku 的平台。它足够灵活，可以做其他 PaaS 解决方案可以做的任何事情，并且通过正确的配置，Kubernetes 可以完全根据您的需求进行定制。

像 ECS 和 Heroku 这样的平台(都是专有的)，你为了简单而牺牲了灵活性。例如，您可能被限制使用特定的操作系统，或者他们希望您以特定的方式设置网络流量。如果您选择 ECS 或 Heroku，开始可能会容易一点，但是您会冒最终超越它们的风险，并且不得不进行痛苦的迁移。

[![CNCF Webinar Watch Now Best Practices for Running and Implementing Kubernetes](img/fde43cfc86fc5611fc7ce50a70ef7480.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/31c8e725-00d4-4891-81fd-9fe05485eaa6)

## **Kubernetes:devo PS 文化的答案？**

许多组织希望实施 DevOps 实践，并相信转换到 Kubernetes 将有助于这种转变。虽然 Kubernetes 可以帮助催化 DevOps 转型，但它并不是灵丹妙药。你可以有没有 Kubernetes 的 DevOps 文化，也可以有没有 DevOps 的 Kubernetes。

然而，部署 Kubernetes 可以帮助公司加速向 DevOps 的过渡。因为它充当基础设施和工程师之间的 API 层，Kubernetes 使得在开发人员和操作人员之间建立协作和通信模式变得容易。这允许开发人员对他们的应用程序的部署配置承担更多的责任，并帮助运营团队追溯生产问题到特定的提交。

当开发人员能够编写代码、部署代码、查看代码在生产环境中的运行情况，并了解代码的运行方式时，他们就能了解什么在运行，什么在中断。他们将对运送到生产现场的一切拥有完全的所有权。这是真正的 DevOps 文化。

同样，Kubernetes 可以在 DevOps 中发挥作用，但这只是拼图的一部分。公司仍然需要改变团队相互交流的方式，以成功实施 DevOps 文化。

## **寻求帮助**

Kubernetes 生态系统在过去几年里已经成熟了很多。有一些成熟的模式，比如使用[基础设施作为代码](https://www.fairwinds.com/blog/why-infrastructure-as-code-kubernetes)，使用[管理的附加组件集](https://www.fairwinds.com/elements)来做诸如入口或证书管理之类的事情。坚持久经考验的[最佳实践](https://www.fairwinds.com/kubernetes-best-practices-comprehensive-white-paper)是确保你在实施 Kubernetes 时不会遇到任何奇怪的死角的好方法。

找一些在大规模运营 Kubernetes 方面有丰富经验的人，或者找一个能帮你完成旅程的合作伙伴。在实现 Kubernetes 时有很多“未知的未知”，所以找一个以前经历过这些战斗并且有伤疤可以证明的人。

人们对 Kubernetes 赞不绝口，这是有原因的。这是一个非常有益的平台。会不会用错了？是的。如果是新手，在习惯之前是不是很难用？是的。但是一旦你开始运行，它就不会辜负宣传。

[![Free Download: Kubernetes Best Practices Whitepaper](img/b53e4ee22b6ef19bc06a035649ea1dc6.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/4f92c7e1-1646-4985-9a0a-b1091903dddb)