# 选择 Kubernetes 时的 5 个关键考虑因素

> 原文：<https://www.fairwinds.com/blog/5-key-considerations-when-choosing-kubernetes>

It seems like this should be easy to know where to start, but then you find out there are some significant hurdles in Kubernetes adoption, and even knowing [how to explain Kubernetes](/thoughts/how-to-explain-kubernetes) and where to begin can feel overwhelming. 

在全力投入之前，这里有五件事需要考虑。

# 1。学习你不知道的东西

Kubernetes 是历史上最大的开源项目之一——生态系统是巨大的，项目本身的复杂性令人难以置信，有附加组件、第三方贡献和托管服务。哪些作品是必不可少的？哪些是碎的？你到底听谁的话才知道呢？

In a [2018 CNCF survey](https://www.cncf.io/blog/2018/08/29/cncf-survey-use-of-cloud-native-technologies-in-production-has-grown-over-200-percent/), 40% of respondents found Complexity to be one of the biggest challenges to using and deploying containers. Kubernetes *can* make things easier. But if you don’t know where to get started, it can also add incredible complexity.Similarly, [The New Stack](https://thenewstack.io/week-numbers-kubernetes-implementations-good-bad-ugly/)  found that 78% of companies will attempt Kubernetes adoption on their own, and, consequently, 38% of respondents indicated this  has taken longer than originally expected. You need to budget carefully, train your team or hire outside talent, and make sure your people have enough knowledge and comfort to also implement a sane on-call rotation. Some solutions, like [Fairwinds ClusterOps](https://www.fairwinds.com/clusterops), offer Advisory services to help augment you with sound architectural decisions along the way. 

# 2。构建和维护生产级堆栈

一旦你克服了最初的障碍，你将如何为生产做好准备呢？仅仅是启动和运行 Kubernetes 与为生产做准备是不同的。这里有几种选择，所有这些都取决于您选择部署到哪里。

如果您选择部署到公共云，三大主要云提供商都有 托管的 Kubernetes 服务产品。 这些服务专注于为您提供高度可用的控制平面，这意味着您不需要管理 Kubernetes 运行的底层节点或计算。托管的 Kubernetes 服务还将为您提供一个直观的 【点击式】 UI 来创建集群。 不幸的是，每个服务处理补丁、升级甚至自动伸缩的方式都不一样。确保你对第一个 CVE 来袭的时间，或者你期待的黑色星期五交通高峰有个计划。

下一个 ， 你们计划实施哪些附加产品？哪些附加组件使您的基础架构更适合生产，哪些会使您面临新的更复杂的安全问题？大多数插件都是开源的，但在推向生产之前，它们仍然需要大量的时间投资和内部 P o Cs。

投资培训。投资外部帮助。投资第三方来审计你所建立的东西，并确保你有正确的系统，否则它会回来咬你。Kubernetes 是一个不可思议的工具，但如果没有经验丰富的老手，它也可能是危险的。 

# 3。不要忘记基础设施是代码

在 GCP 的 GKE、AWS 的 EKS 和 Azure 的 AKS 之间，甚至是其他地方的 Rancher 你可以在某个地方的 GUI 上登录 ,点击一些按钮，获得 Kubernetes 集群。但是，如果您的生产基础架构是通过点击点--和--构建的，那么您就没有一个可重复的模型来构建-并在将来修改-it。

在一个 worstcase场景中， 你会希望你的每一个设置和配置决策都被记录为代码，以快速备份掉的所有东西。 这就是基础设施作为代码是灾难恢复策略的重要基础的原因。

基础设施作为代码对于 SaaS 公司 或者电子商务公司 来说也是得心应手。像国际扩张或 白色 - 标签 这样的商业机会，出于性能或数据主权的考虑，可能需要 服务 在当地国家运行。将你的整个基础设施定义为代码可以让你相对容易地 制造出你的 环境 的精确复制品。

作为代码的基础设施不仅方便某些用例，也是运营中的最佳实践。 基础设施作为代码给你提供了一致性、版本控制、可测试性、透明性和可审计性，并使你的基础设施可复制。 

# 4。实施全天候运营和监控

有时，您可能会觉得您的基础设施与您的业务无关，但是当基础设施出现故障，您的客户或顾客无法再访问任何东西时，您会意识到 It 对业务的重要性e盟友就是 。实施质量监控对您的 **收入** 和您的 **工程团队有重大影响。**

监控 *井* 很难，你的团队需要在出现问题时醒来，以便修复它们。不幸的是，仅仅给某人一个寻呼机是不够的。质量监控需要到位，这样您就可以实际知道何时发生或将要发生停机，并且您的团队需要有足够的装备，以便能够在出现问题时做出响应。

停机时间将直接影响您公司的收入。

构建良好的基础设施可以自动应对过去运营中的许多常见问题。Kubernetes 内置了许多这样的故障安全机制，但是您需要对它们进行适当的配置。

工程人员的士气和留任受到 监控不力的极大影响。

监控不力会直接影响你的工程幸福感。 确保建立一个合理的随叫随到的时间表，让工程师可以休假，而不会因为意外的生产事故而承受持续的压力。

# 5。验证您的部署

如果你已经建立了一个工作的生产级基础设施，完全以代码的形式记录和实现，并且它得到了很好的监控，你的团队正在处理随叫随到的轮换…祝贺你，你现在已经成功了 90%！

From here, your engineering team is more empowered than ever to work with Kubernetes and you have CI/CD in place. Now it’s time to validate each of the workloads being deployed to your cluster so you can be sure you stay aligned with best practices. These best practices can be dictated from an ops team, or they can be encoded  and enforced with a tool like  [Fairwinds Polaris](https://www.fairwinds.com/polaris).

现在，您的开发人员可以直接看到他们正在部署的工作负载在到达集群之前的运行状况。您的运营团队可以看到 工作负载 在哪里被 不安全或不可靠地配置， 以及如何修复 这些问题 。

# 结论

借助一点外界的帮助，一些训练，以及放慢速度去思考那些如果你做错了会让你停下来的事情，你就能达到 Kubernetes Zen。如果你决定雇人帮你更快到达那里，Fairwinds 有各种各样的产品可以加速 Kubernetes 的采用 (将时间从几个月缩短到几周，而不是几个月或更长)。