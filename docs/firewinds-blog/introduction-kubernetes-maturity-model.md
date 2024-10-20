# Kubernetes 成熟度模型介绍——如何使用

> 原文：<https://www.fairwinds.com/blog/introduction-kubernetes-maturity-model>

 Fairwinds 团队在一年前开发了 Kubernetes 成熟度模型，我们将继续更新和完善它，以反映您在 Kubernetes 成熟度之旅中所经历的五个阶段。如果 Kubernetes 成熟度模型对你来说是新的，这是一个关于如何使用它的有用的介绍和指南。

在你做任何事情之前，考虑一下[云原生之旅对你和你的组织](/kubernetes-maturity-model/phase-1-prepare)意味着什么。Kubernetes 并不适合每个人，所以请确保您了解从哪里开始，谁可以信任，以及如何通过拥抱 Kubernetes 来证明价值。

1.  [成熟度模型的第一阶段](/kubernetes-maturity-model/phase-2-transform)是关于转型，比如建立 Kubernetes 基础设施和转移工作负载。
2.  [第二阶段](/kubernetes-maturity-model/phase-3-deploy)涵盖了实现和部署过程，设置 CI/CD，授权开发人员，以及有限的监控和可观察性。
3.  [第三阶段](/kubernetes-maturity-model/phase-3-deploy)是关于建立对你的 Kubernetes 核心竞争力的信心，因此你可以定期成功地部署和发布特性。
4.  [第四阶段](/kubernetes-maturity-model/phase-5-improve-operations)包括改善您的操作和控制，特别是集群的安全性、效率和可靠性。
5.  [第五阶段](/kubernetes-maturity-model/phase-6-measure-control)是最后一个阶段，实际上是关于控制的。在这最后一个阶段，您将使用复杂的监控来推动策略和控制，从而更深入地了解工作负载。

当然，任何成熟度模型都是一个过程，您可能会在不同阶段之间来回移动，有些阶段会比其他阶段花费更长的时间。即使你已经到了第五阶段，你也将一直致力于持续的优化，消除人为错误和努力，提高可靠性和效率。

现在您已经了解了基础知识，那么您实际上如何使用 Kubernetes 成熟度模型呢？在本帖中，我们将只探索最初的几个阶段来帮助你开始。首先，该模型旨在供任何人使用，无论您是 Kubernetes 的新手还是已经有经验的用户。让我们看一个例子。

### 示例 1:

您有两个开发人员尝试了 Kubernetes，并希望在您的工程团队中部署它。从准备阶段开始，用它作为检查清单，确保您(和您的组织)在这些关键问题上意见一致:

*   Kubernetes 要解决的问题
*   开源软件现在和未来的重要性
*   该项目符合您的业务目标

进入第 1 阶段:花时间概述你的业务目标，并获得整个组织的认可。现在，您已经准备好开始设置 Kubernetes 基础设施，并计划如何转移工作负载。在此阶段，您需要采取几个关键步骤来真正开始您的 Kubernetes 转型:

1.  确定工作负载的优先级—决定您要从哪些工作负载开始。不要期望一次迁移所有的工作负载；你需要计划一个分阶段的方法。
2.  与 Kubernetes 一起进行概念验证(POC ),以便您了解其中涉及的内容以及如何成功完成。您可能会发现，与 Kubernetes 专家合作，确保您正确地设置了第一个集群，以便它们能够满足您的工作负载需求，会很有帮助。
3.  进行技术改造。这不是一个小任务。当您迁移到 Kubernetes 时，您需要深入研究您现有的堆栈，并确定您的技术需求。您还需要考虑应用容器化、您的云和 Kubernetes 基础设施、YAML 或掌舵图、外部依赖、您的 Git 工作流、您的 CI/CD 管道、测试，以及最终将您的应用推广到生产环境。

准备步骤和进入第一阶段是耗时的步骤，通过这些步骤你会学到很多东西。不要急；这个学习阶段和技术转型步骤对于实现您的总体目标至关重要。

请继续关注本系列的下一篇文章，它将概述第二和第三阶段。我们将在最后一篇文章中讨论第四和第五阶段。与此同时，请阅读我们的参考资料中关于 Kubernetes 成熟度模型的所有内容，或者查看我们的新电子书[以了解更多信息。](/kubernetes-deployment-maturity)

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)