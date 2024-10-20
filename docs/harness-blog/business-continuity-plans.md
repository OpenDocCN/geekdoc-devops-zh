# 软件交付渠道的业务连续性|利用

> 原文：<https://www.harness.io/blog/business-continuity-plans>

我们所有人都在这艘船上。对于受监管的组织，业务连续性计划是法律要求的，但对于那些没有这种要求的组织，可以开始实施。

在这场全球健康危机中，作为一个社会，我们确实面临着挑战。我唯一一次接触到[业务连续性计划](https://en.wikipedia.org/wiki/Business_continuity_planning)是在我的商务旅行仅限于两个公司人员乘坐同一架航班的时候。

受监管行业和政府组织[需要业务连续性计划，或 BCP。BCPs](https://dynamicquest.com/industry-bcp-laws/) [关注整个组织](https://www.cio.com/article/2381021/best-practices-how-to-create-an-effective-business-continuity-plan.html)而不仅仅是 IT 系统的正常运行时间，后者在[灾难恢复](https://en.wikipedia.org/wiki/Disaster_recovery)计划中更受关注。要实施 BCP，您需要一份基线清单，然后围绕战略进行分析。

### BCP 的 1，2，3s

如果你对业务连续性计划不感冒，[前往 ready.gov](https://www.ready.gov/business-continuity-plan)对业务连续性计划有很好的了解。业务连续性计划就是这样，旨在保持组织的运转。bcp 当然是有条不紊的，旨在危机时刻限制情绪化决策。例如，航空公司飞行员有[快速参考手册](https://www.skybrary.aero/index.php/Quick_Reference_Handbook_(QRH))【QRHs】，这是在紧急情况下运行的必要实践的清单，以确保没有步骤被遗忘，这是 BCP 的精神。

设计您的 BCP 从盘点开始。你不知道你必须继续和保护什么，除非你对你所拥有的有一个好的想法。软件资产可能很难跟踪，因为我们不断地增加和减少服务，并跨越潜在的多个云/基础设施提供商。组织可能有一种 [IT 资产管理](https://www.cio.com/article/3437476/it-asset-management-itam-a-centralized-approach-to-managing-it-systems-and-assets.html)【ITAM】方法来将应用程序分类为资产，但是清点底层基础架构与应用程序之间的关系可能具有挑战性。

分析部分是 BCP 大部分内容的起草和批准部分。影响分析/情景是整个 BCP 战略的核心。与[平均恢复时间](https://en.wikipedia.org/wiki/Mean_time_to_repair)【MTTR】一样，[恢复时间目标](https://www.atlantic.net/disaster-recovery/rto-vs-rpo/)【RTO】是影响分析期间的主要指标。您的 RTO 是灾难发生后恢复的最长时间。组织将设计您的策略来满足 RTO。对于软件系统而言，这可能会改变基础架构/启动灾难恢复站点，甚至限制/优先处理应用程序和请求。

bcp 旨在处理我们无法控制的混乱或日常战术活动中没有想到的灾难场景。Harness 平台可以帮助您实现 BCP 目标。

### 这里有 3 种方式可以帮助你驾驭 BCP

Harness 的许多客户使用 Harness 平台作为他们应用程序的记录系统，因为我们是编排部署/发布过程的平台。首先，利用带有服务清单控制面板的 Harness 平台，可以更轻松地清点现有资源。在服务清单控制面板中，可以了解您的应用程序部署在什么基础架构上。

除了清点库存之外，Harness 还集成了流行的标签工具，如[吉拉](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/workflows/jira-integration/)和[服务 Now](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/workflows/service-now-integration/) ，因此不仅有形的 IT 资产易于跟踪，而且变更控制流程和基本原理也存在并可跟踪部署。

其次，Harness 平台抽象化了基础架构，因此多云部署就像部署到您自己的数据中心一样简单。Harness 平台通过我们的 [CD 抽象模型](https://developer.harness.io/docs/first-gen/starthere-firstgen/harness-key-concepts/)抽象出了管道中所需的大部分内容。

作为对 Harness 支持这一支柱的能力的证明，我们自己在 76 分钟内[将公有云供应商从无到有](https://harness.io/2019/08/migrating-clouds-with-harness-continuous-delivery/)。BCP 的一个支柱是根据需要重新搭建平台的能力。

CD 抽象模型允许您快速拼凑成功部署所需的信心建立步骤和基础设施。

第三，Harness 平台提供了基于约定的方式来自信地部署，并且具有关于出现问题时何时回滚的明确定义的约定。由于在 BCP 触发事件期间机构知识的潜在损失，围绕回退甚至通过环境促进部署的判断呼叫可能会丢失；利用为制度知识提供了系统的资源。

利用 Harness 平台，回滚可以轻而易举，例如，一个客户证明，[生产回滚仅用了 32 秒](https://harness.io/2018/01/how-build-com-automated-ci-cd-rollback/)。为 Harness 平台做好准备的投资可以帮助减轻 bcp 可能会排除的不可预见的风险，即在应用程序中进行更改的未来。

### 已准备

如果我们看一看 [IBM 的灾难恢复层级](https://share.confex.com/share/118/webprogram/Handout/Session10387/Session%2010387%20Business%20Continuity%20Soloution%20Selection%20Methodology%2003-7-2012.pdf)，显然我们都将目标定在最高层“*高度自动化、业务集成的解决方案*”。我们的应用程序应该是自动化的，不会中断正常运行时间。说起来容易做起来难。

目标是伟大的，但实现起来是复杂的，这就是为什么“*高度自动化、业务集成的解决方案*”是几层中的顶层。支持混合云工作负载的基础架构和平台通常非常复杂。具有讽刺意味的是，在 BCP 中，这些系统本身会成为单点故障，因为创建、管理和维护这些系统需要非常复杂的技能。

最终，一个人将不得不介入并做出某种改变。如果新资源在与他/她互动的平台上经验有限，学习并非不可能，但肯定需要时间，并可能做出经验知道不要做的改变。灾难恢复计划可以很好地恢复正常运行时间，但恢复做出改变的能力是一个挑战，在利用，我们随时准备提供帮助。

### 业务连续性+管理

让我们来看看目前发生在我们旅游业的一件令人沮丧的事情。

在旅游业中，旅游组织和提供商必须处理大量的请求。例如，航空公司为了应对这一全球性事件，正在免收费用等。尽管这需要改变航空公司多年来投资的系统的功能，这些系统通常会对改变收取费用。在您的 BCP 中，调整您的应用程序平台中的业务规则可能不是一个显而易见的规划领域，以满足您的 RTO。

Harness as a platform 旨在按照惯例和一致性部署和发布各种规模和不同堆栈的应用程序。在这个例子中，我们假设最熟悉部署到航空公司计费引擎的资源的容量减少了，另一组工程师站了出来。设计删除计费规则的更改在本地可能并不困难，但是部署到可能处理数百万个请求的生产系统中则是技能不足的地方。

Harness 平台可以帮助实现这些部署的民主化，这样在部署过程中具有初步经验的人就可以掌握并迈向成功的部署。一个发布/部署可以有几个不同的步骤。即使在跳过某些批准步骤的紧急发布场景下，如果部署成功，获得反馈也是有压力的，尤其是对于第一次运行的人。

随着基础设施承受的压力越来越大，向可用性更高或性能更好的基础设施迁移的速度会越来越快。Harness 承诺您和您的组织将继续促进民主化的交付渠道。

### 马具是来合作的

随着当前全球形势的继续发展，将有更多大大小小的组织制定或加强其业务连续性计划。Harness 旨在帮助与任何规模的垂直组织合作。

利用 Harness 平台，应用不同的部署策略和目标很容易，因为我们抽象出了基础设施提供者。从特定的数据中心转移到不同的云供应商是[线束管道](https://harness.io/build-pipelines-in-minutes/)中的一个简单变化。最后，由于 Harness 平台易于使用且基于惯例，如果您不得不为同事选择部署，学习曲线将会非常快。

注意安全，

-拉维