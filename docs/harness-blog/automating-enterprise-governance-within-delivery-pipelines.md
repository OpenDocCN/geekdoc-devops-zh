# 交付管道中的自动化企业治理|管理

> 原文：<https://www.harness.io/blog/automating-enterprise-governance-within-delivery-pipelines>

Harness 的持续交付即服务平台可以帮助受到严格监管的行业管理其治理流程，并证明交付工件的完整性。

在一个开发人员不断将代码推向生产的组织中，管理风险可能很困难。[在《衡量和管理信息风险:一种公平的方法》](https://www.fairinstitute.org/fair-book)中，Jack Freund 和 Jack Jones 将治理描述为一种“治理组织的风险前景”的经济高效的方法您希望确保您的组织积极了解和管理风险，尤其是在受到严格监管的行业，这些行业需要遵守监管机构和标准，请参见[合规](https://en.wikipedia.org/wiki/Regulatory_compliance)，或我们关于[测量管道合规](https://harness.io/blog/ci-cd-pipeline-governance/)的博客文章。

治理、风险管理和法规遵从性(GRC)是一个涵盖组织跨这三种实践的方法的总称:[治理](https://en.wikipedia.org/wiki/Governance)、[风险管理](https://en.wikipedia.org/wiki/Risk_management)和[法规遵从性](https://en.wikipedia.org/wiki/Regulatory_compliance)。弗罗因德和琼斯描述了风险和合规。

“这个[风险]目标完全是为了做出更明智的风险决策，这可以归结为三点:(1)识别“风险”，(2)有效地对“风险”进行评级和优先排序，以及(3)就如何减轻足以保证减轻的“风险”做出决策。”

“在这三个目标中，法规遵从性管理是最简单的—至少从表面上看是这样。从表面上看，法规遵从性只是识别相关期望的问题(例如，由巴塞尔、支付卡行业(PCI)、SOX 等定义的要求)。)，记录和报告组织如何(或不)符合这些期望，并跟踪和报告活动以消除任何差距。”

因此，如果 GRC 是关于调整组织来管理风险，那么开发人员扮演什么角色呢？

## 从代码提交到生产

我们在之前的博客文章中讨论了采用系统化方法开发软件交付过程的重要性。我们分享了类似于[价值流图](https://harness.io/blog/value-stream-mapping-guide/)的实践，为组织提供工具来更好地了解他们的价值流，并[加速他们的开发之旅](https://harness.io/blog/beginning-your-devops-journey/)。这些 DevOps 实践表明，每一个软件交付涉众都对他们交付的价值负责。但另一方面，它们也表明利益相关者要对他们创造或引入的任何风险负责。

DevOps 自动化治理参考架构在这里找到了[，分享了如何进一步采用系统方法进行交付。](https://files.ontraport.com/media/phpPOS1G9?Expires=1726511330&Signature=XOE6ZLIDGWxbVvGvWD-JKfFPQ9q4~2go4oHaSiOOyx7NVRo3eOHxm0VGI-5QMt9zufboL~ldfCcomHSgCo4cfAJ8fb5gUoGCJHQ5MWtzvlbFNzWI-G0~u9goJyvcEX8zAowxgRlMdiWnO8skHfCX7C4GzQpRREwFwIs8BUIimBAZt55gTtS-PugKS7CGJVDQgkQww7bE9fokM2ZBEoYOB5RSvq6n9unEWpVB0TRDvH-5eiz4gHqZBelu2vZQbuyx9iIhYl2vzNQu6VNPMFCCVYJzBJ7eMVulvavobOHE3jHwaVjpKObQDuWyCRhRaws2DDl3yWSQdTI7k-7L~kH6Tw__&Key-Pair-Id=APKAJVAAMVW6XQYWSTNA)

通过查看交付管道中的每个阶段，您可以定义与该阶段相关的输入、输出、参与者、操作、风险和控制点。

治理的基本部分是开发人员意识到每个阶段的风险。参考架构白皮书分享了一些与代码提交相关的常见风险，如源代码中未经批准的更改和凭证。同样，当部署到生产环境中时，您可能会有风险，例如生产环境中的低质量代码、缺少质量关卡以及生产环境中的意外系统行为。

这些风险有助于定义有助于管理风险的控制点区域。如果您面临未经批准的变更的风险，请引入变更批准流程。同样，您可以通过秘密管理、应用程序质量分析、质量关评估和强制部署策略来控制风险。

从代码提交到生产的过程中涉及的每个人都有责任降低风险。

## 企业治理的组成部分

现在，让我们讨论云环境治理流程的组成部分。DevOps 自动化治理参考架构，见[此处](https://files.ontraport.com/media/phpPOS1G9?Expires=1726511330&Signature=XOE6ZLIDGWxbVvGvWD-JKfFPQ9q4~2go4oHaSiOOyx7NVRo3eOHxm0VGI-5QMt9zufboL~ldfCcomHSgCo4cfAJ8fb5gUoGCJHQ5MWtzvlbFNzWI-G0~u9goJyvcEX8zAowxgRlMdiWnO8skHfCX7C4GzQpRREwFwIs8BUIimBAZt55gTtS-PugKS7CGJVDQgkQww7bE9fokM2ZBEoYOB5RSvq6n9unEWpVB0TRDvH-5eiz4gHqZBelu2vZQbuyx9iIhYl2vzNQu6VNPMFCCVYJzBJ7eMVulvavobOHE3jHwaVjpKObQDuWyCRhRaws2DDl3yWSQdTI7k-7L~kH6Tw__&Key-Pair-Id=APKAJVAAMVW6XQYWSTNA)，分享了一种导航您的自动化治理之旅的方法。这里要讨论的许多概念在该参考文件中有详细解释。

**注释**为元数据定义。**事件**是为每个需要这个注释的工件或资源生成的。例如，注释可以提供特定漏洞的详细信息，如影响、名称和状态。我会为每个具有该安全漏洞的容器映像生成一个事件。类似地，我可以有一个定义特定应用程序部署的注释，当我在不同的环境中推广部署时，我会生成一个事件。笔记和事件之间存在一对多的关系。

**证明**是一种特殊类型的注释，表示您在治理过程中已经满足的验证。证明与**证明者**相关联，证明者拥有验证治理过程中的控制点的权限。例如，确定您已经通过了代码审查或单元测试就是一种证明。每个证明代表治理过程中的一个**控制点**。

**二进制授权策略**使用证明者列表将您的治理表示为代码。二进制授权策略充当一系列**门**。该策略在您的交付过程中充当屏障，确保每个指定的证明者都提供证明。

在您的 Kubernetes 环境中打开二进制授权 **(BinAuthz)** 是一种常见的做法，以确保您能够控制变更和部署。你的 Google Kubernetes 引擎(GKE)中会有一个准入控制器，当你与环境交互时，它会检查认证。这里有更多关于 BinAuthz 如何为 GKE 工作的信息。

如果你想了解更多关于为你的治理过程设计控制点的知识，Captial One 通过一个叫做“16 个门”的概念分享了它的管道设计。

## 利用您的治理过程

治理过程需要自动化来加速软件交付；否则，它会损害您的速度和上市时间。过去一年中出现的一个热门话题是自动化管道治理，它使企业能够证明交付管道中资产的完整性。管道治理超越了传统的 CI/CD，在传统的 CI/CD 中，开发人员只是简单地自动化交付，而没有真正降低风险。Harness 的[连续交付平台](https://harness.io/platform/continuous-delivery/)可以帮助受到严格监管的行业管理其治理流程，并证明交付工件的完整性。[点击此处](https://harness.io/t-shirts/)安排今天的装具演示。