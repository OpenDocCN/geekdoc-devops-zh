# 探索 DORA Metrics 和 DevOps 分析

> 原文：<https://www.gitkraken.com/gitkon/dora-metrics>

[https://www.youtube.com/embed/9OBLkxb85k0?feature=oembed](https://www.youtube.com/embed/9OBLkxb85k0?feature=oembed)

视频

你想开发成功的地图，没有任何人从你那里窃取结果的风险吗？那你来对地方了。让我们探索朵拉，但不是尼克国际儿童频道的角色，特别是朵拉矩阵。

朵拉是 [DevOps 研究与评估](https://www.devops-research.com/research.html)小组。由 Nicole Forsgren 博士和 Gene Kim 创建，它开始对 DevOps 以及组织如何在他们的软件交付组织中实现它进行学术风格的研究。目标是尝试并理解是什么促成了一个伟大的 DevOps 转型。

许多业内人士仍然在努力理解开发和运营是如何结合在一起的，以及这对敏捷或者软件的未来意味着什么。

现在，让我们回到 DevOps 刚起步的时候。一名开发人员走过一个服务器机房，门开着。这从来都不是一个好兆头。他们看到一名首席开发人员正在开发公司准备推向市场的新产品，这位首席开发人员将一把螺丝刀插入服务器，试图取出硬盘。该服务器用于跟踪他们正在做的所有工作和产品计划。硬盘出现故障，开发人员不知道下一步该做什么，所以他们没有编码，而是试图修复服务器。

这是经济学家喜欢称之为“机会成本”的一个例子在这种情况下，该团队致力于将一种新产品推向市场，即现有产品线的现代化，以在其有限的垂直市场中率先将基于平板电脑的调查问卷推向市场。这是那个项目的首席开发人员，实际上是建筑师，拿着一把螺丝刀在服务器上。

这个团队需要合适的开发工具，他们不需要用螺丝刀，这样他们就可以继续花时间为客户做工程工作。

如果您正在寻找一种工具来简化您团队的协作和工作流程，GitKraken 的 Git 客户端允许您的团队利用 Git 的真正功能，无论您的开发人员更喜欢 GUI 还是 CLI。

## **软件已经吃掉了世界**

众所周知，风险投资家马克·安德森说过“软件正在吞噬世界”考虑到我们生活在这样一个时代，你从一个应用程序订购你真正的食物，然后通过用另一个应用程序拍照来记录它，我们可以有把握地说，软件实际上已经“吃掉了世界”。

在这个世界上，能够生存和发展的企业是那些能够快速适应市场变化的企业。如今，企业快速适应的方式是通过软件。这就是为什么最近有人引用安德森的话说:“周期时间，即从想法到生产或从概念到现金的时间，是决定技术领域赢家和输家的最被低估的力量。”科技领域的赢家和输家将决定每个行业的赢家和输家。每个公司都是软件公司。他们以更快的速度发布更好软件的能力将决定他们在各自市场的成败。

## **快速突破:稳定性 vs 速度**

你可能会想“你不能跑得太快而弄坏东西。”某种程度上说，没错。你不仅要关心速度，关心你能以多快的速度将产品投放市场，以及你能以多快的速度使这些产品适应不断变化的市场条件，而且你还要考虑你的产品和你的平台的稳定性。只有当你能为客户提供稳定可靠的产品时，他们才会成为你的客户。一个他们可以依靠的人。安全可靠的人。

在技术世界中，你经常会看到这两种力量不同步。对交付安全和稳定的产品意味着什么的直觉或先入为主的想法使我们想到这样的事情:“哦，你知道保持系统稳定的方法是放慢速度”或“更多的变化只会破坏任何系统的稳定。”所以，为了稳定，你忍住了加快脚步的冲动。

那是完全错误的。这是生活中你可以鱼与熊掌兼得的情况之一。事实上，不仅在软件交付操作方面表现最好的人在速度和稳定性方面都很出色，实际上在速度和稳定性之间有一个积极的预测关系。

## **输入 DORA 指标**

这就是我们在多拉的朋友们的用武之地。他们已经进行了七年多的研究，将复杂的统计分析应用于从数千个组织收集的数据，以从所述数据中得出有意义的结论并识别模式。他们这样做是为了创建模型，可以帮助任何规模的组织知道应该关注什么来提高他们自己的软件交付性能。他们已经就这个主题写了一本书。DORA 计划的创始人 Nicole Forsgren 博士、Jez Humble 和 Gene Kim 根据他们多年研究的综合发现写了一本书，名为“ [Accelerate:精益软件和 DevOps 的科学:构建和扩展高性能技术组织](https://itrevolution.com/book/accelerate/)”这本书强烈推荐给所有的软件工程师、工程经理、产品经理、首席技术官、首席信息官和首席执行官。实际上任何一个依赖软件创业的人。

这是一次对科学数据和他们应用于这些数据集的原则的奇妙检查，这些数据集为从多年研究中得出的结论提供了信息。

## **来自 DORA Research 的令人惊讶的重要发现**

首先，也许是最令人惊讶的是，它彻底揭穿了必须在速度和稳定性之间进行权衡的想法。在研究中，他们发现被他们称为“精英执行者”的高绩效团队实际上在部署代码方面要快得多。他们比表现差的人部署得更频繁。

与此同时，对于那些精英员工来说，他们服务的“平均恢复时间”明显更短。这是指发生影响生产的事件时实施修复所需的时间。即使在控制其他变量，如公司规模、行业或其他信息时，也是如此。

不仅仅是数据显示一些高科技公司能够快速行动，拥有更稳定的系统；它实际上表明这种情况发生在各个行业。您可以看到零售、医疗保健、制造、政府和金融服务领域也存在同样的模式。

这些精英演员到底在哪些方面比表现不佳的同行做得更好？推动绩效的衡量标准是什么？工程师和领导者要想很好地提升业绩，应该重点关注哪些重要因素？幸运的是朵拉已经能够回答这些问题了。他们发现了四个关键的度量，这四个度量都与更高的性能相关联，并且与软件交付性能和整体组织性能有预测关系。专注于实现这些关键点的能力使组织能够衡量和改进这些 DORA 指标。

这些指标直接归因于组织在收入或其他组织目标方面的整体表现，以及工作满意度等人为因素。这些类别的改进可以由所有这些 DORA 指标来推动，因此这确实是一个突破性的发现，得到了直觉和数据的支持。

## 【DORA 的四个关键指标

4 个 DORA 指标是:

*   变革的准备时间
*   部署频率
*   故障率
*   恢复服务的平均时间

考虑软件开发过程的有效性和效率。上面列出的前两个指标实际上指的是速度，而后两个指标指的是稳定性。这些 DORA 度量得到了软件部署过程以及它们在为组织实现那些稳定性目标中的有效性。

Consider the effectiveness and efficiency of the software development process. The first two metrics listed above are really speaking to speed, while the last two speak to stability. These DORA metrics get at the software deployment processes and their effectiveness in achieving those stability goals for organizations.

**变更的交付时间**

### 考虑一下，变革的准备时间似乎是一个简单的指标。你可能知道在整个过程中做出改变需要多长时间。但是用科学的准确性来衡量它需要一个更具体的定义。

在制造业中，提前期指的是客户提出请求和该请求得到满足之间的时间。在软件开发中，要复杂一点，因为设计和产品工程需要时间来满足一些需求，如果不是全部的话。另一端也可以有多个定义。如果“生产中”意味着等待应用商店发布新版本，甚至是等待客户更新软件，那该怎么办？

为了关注软件交付性能，变更的交付周期具体指的是从代码被提交到生产环境中所花费的时间。或者更具体地说，代码已经发布并准备好投入生产，即使这意味着在这之后，在客户实际看到变更之前，还要花费额外的时间。这是软件和开发团队无法控制的时间。考虑一下:假设你有一行代码需要修改。那条生产线投入生产需要多长时间？那一行代码要经过的每一步是什么？从表面上看，这似乎是一个非常简单的问题，但是思考这个问题实际上可以导致关于将代码投入生产的整体瓶颈的极其富有成效的讨论。

To focus on software delivery performance, lead time for changes specifically refers to the time it takes from code being committed to being in a production-like environment. Or more specifically, that the code is released and ready for production, even if that means there’s additional time spent after that point, but before the customer actually sees the change. That is time that the software and DevOps teams don’t have control over.

Consider this: let’s say you have one line of code to change. How long does it take for that line to get into production? What is every step that line of code has to go through to get there? On the surface, that seems like a pretty simple question, but thinking through this can actually lead to extremely fruitful discussions about the overall bottlenecks of getting code to production.

**部署频率**

### 另一个 DORA 度量标准是*部署频率*，当提到更快地交付更好的软件时，它将团队区分开来。您多久将代码部署到生产环境中一次？起初，更频繁地部署代码，实际上更频繁地更改东西，实际上与系统稳定性正相关，这似乎有悖常理。直觉告诉我们，确保生产的变化缓慢且不频繁，最终会让系统变得更好，或者至少更稳定。

但实际上，那只是子虚乌有！多拉报告和之前提到的 DevOps 书中讨论了许多因素，并对此进行了详细说明。想象一下像肌肉一样部署到生产中。你锻炼肌肉越多，建立肌肉记忆越多，它就会变得越强越好。如果您发现您的团队害怕发布或部署日，或者您害怕在星期五部署，答案可能不是减少部署，而是增加部署！

表现最好的人每天都要进行多次部署。他们已经锻炼出了肌肉，所以周五的部署不会比其他任何一天差。他们建立的系统具有弹性和可靠性，因为它经过了多次部署和测试。

The other DORA metric that sets teams apart when it comes to delivering better software faster is *deployment frequency*. How often are you deploying code to production? At first, it may seem counterintuitive that deploying code more often, literally changing things more often, can actually have a positive correlation to system stability. Intuition says making sure changes to production are slow and infrequent will make the system better in the end, or at least more stable.

But in practice, that’s just not true! And there are a lot of factors discussed in the DORA reports and the previously mentioned DevOps book that go into detail about this. Think about deploying to production like a muscle. The more you work out that muscle and build muscle memory, the stronger and better it becomes. If you find that your team dreads releases or deploy days or you’re scared to deploy on Fridays, the answer might not be to deploy less, but to deploy more!

The top performers are deploying multiple times per day. They’ve built up that muscle so that deploying on Friday is no worse than any other day. The system they’ve built has resilience and reliability because it has had many at-bats with deployments and testing.

**改变故障率**

### 说到无忧无虑地部署，我们来谈谈在部署或更改某些东西时，您在生产中制造问题的频率。这就是所谓的*变化失败率*。

你的团队今天有没有一个实证的方法来看待这个问题？您知道有多大比例的生产变更会给您的用户带来问题吗？今天，许多人不跟踪这种类型的具体数据，大多数人充其量只是根据轶事信息进行计算。

即使组织有意识地努力跟踪这些数据，变更失败率也几乎无法衡量。很难在整个开发生命周期中真正追溯变更，并确定是什么导致了变更。因此，许多变化都以类似“嘿，让我们在马已经出来之后关上这个谷仓的门”的情况结束。

但是与其他 DORA 指标一样，衡量和关注这一比率的改进显示了变更失败率与我们团队的整体软件交付和运营绩效之间的直接预测关系。

Even if organizations are making a conscious effort to track this data, the change failure rate can be next to impossible to measure. It’s very hard to really trace the change back through the entire development lifecycle and identify what caused it. Thus, many changes end up in a situation like “hey let’s close this barn door after the horse already got out.”

But as with the other DORA metrics, measuring and focusing improvements on this rate shows direct predictive relationships between change failure rate and the overall software delivery and operational performance of our teams.

**平均恢复时间**

### 平均恢复时间(T1)或 MTTR 衡量修复措施生效的速度。生产事故的数量永远不会是零，即使是世界上拥有最好的分布式系统工程师的最大的技术组织也是如此。也就是说，精英执行者和精英执行软件组织使用像[站点可靠性工程](https://sre.google/) (SRE)和[服务水平目标](https://en.wikipedia.org/wiki/Service-level_objective) (SLO)这样的方法工作，并且有错误预算来确保当事件确实发生时，它们影响用户的时间长度尽可能地被最小化。

The *mean time to recovery*, or MTTR, measures how fast fixes go into effect. The number of production incidents will never be zero, even for the largest technology organizations in the world that have the best distributed systems engineers. With that being said, elite performers and elite performing software organizations work with methods like [Site Reliability Engineering](https://sre.google/) (SRE) and [Service Level Objectives](https://en.wikipedia.org/wiki/Service-level_objective) (SLO), and have error budgets to ensure that when incidents do happen, the length of time they impact users is minimized as much as possible. 

**可视化 DORA 指标**

## 这些 DORA 指标到底是什么样的？你如何将自己与业内同行进行比较？DORA 报告恰恰提供了这样的能力。

下表来自 [2019 年 DevOps 状况报告。](https://services.google.com/fh/files/misc/state-of-devops-2019.pdf)

What do these DORA metrics really look like? How can you measure yourself against peers in the industry? DORA reports provide the ability to do exactly that.

The table below is from the [2019 state of DevOps report.](https://services.google.com/fh/files/misc/state-of-devops-2019.pdf)

有趣的是，一天部署多次与每六个月部署一次相比，差距仍然存在。这对一个组织适应市场条件的能力有着巨大的影响。想想这些天事情变化有多快。那些能够专注于他们现在在这张图表上的位置并建立改进措施的人将能够以他们甚至还没有想到的新的和创新的方式击败他们的竞争对手。但是还有一个问题。

问题是衡量所有这些东西可能会非常复杂。如果你度量它，你可以专注于改进它，但是今天的工具并不总是很容易对即使是最简单的事情有清晰的理解。例如:代码是何时提交的？什么时候投产的？在一个工具互不关联的世界里，为软件开发生命周期过程做独立的工作可能是一个黑箱。代码从一个方向进入，在一群团队和不同的工具之间跳跃，然后有希望在某个时候从另一个方向进入生产。

如果您有许多不同的非集成系统，那么要理解如何度量这些类型的 DORA 指标，即使不是不可能，也是非常困难的。或者将它们可视化，然后使它们对您的团队可行。

If you have many different unintegrated systems, it can be really hard, if not impossible, to understand how to measure these types of DORA metrics. Or visualize them and then make them actionable for your team.

**让 DORA 指标具有可操作性**

## 要使 DORA 度量具有可操作性，您必须能够理解流程中时间被浪费在哪里。通常作为工程师，你可以很容易地看到这一点，但只是轶事。你可能会说“哦，安全性通过一个构建需要很长时间”或者“测试需要太长时间，我们还没有足够的时间来自动化很多测试。”将这些轶事转化为任何级别的任何人都可以看到的数据，从仪器仪表和控制工程师一直到首席技术官，这对于建立组织范围内的认同以改善真正浪费时间的地方至关重要

正如 DORA 团队已经看到精英们在他们多年的研究中适应和改进，我们也看到了团队思考他们的 DevOps 工具链的方式的演变。DevOps 已经超过 10 岁，经历了许多不同的阶段。

Just as the DORA team has seen elite performers adapting and improving over their years and years of research, we have also seen an evolution in the way teams think about their DevOps toolchains. DevOps is over 10 years old and has gone through a number of different phases. 

**devo PS 的四个阶段**

## **The Four Phases of DevOps**

DevOps 的四个阶段是:

自带(BYO)

同类最佳(BIC)

1.  自己动手(DIY)
2.  DevOps 平台
3.  **自带 DevOps**
4.  在自带(BYO)开发运维阶段，每个团队在一起创建单个产品或应用时选择自己的工具。当团队试图一起工作时，这种方法可能会导致问题，因为他们不熟悉其他团队的工具，或者甚至可能缺乏对相同工具和数据的访问。

考虑安全性测试全部由一个人完成的情况。在为期八周的开发周期结束时，只有一个审查者通常会导致数百个新的安全漏洞，必须仔细检查和修复。一直以来，在发布时间之前，工程师都无法访问这些数据。这导致到处都是挫折。

### **Bring Your Own DevOps **

**同类最佳开发工具**

为了解决这些工具和数据脱节的问题，许多组织转向第二阶段:同类最佳开发运维。在这个阶段，组织为所有团队标准化相同的工具集，为 DevOps 生命周期的每个阶段提供一个首选工具。有时，这些工具允许某种程度的集成，但通常，它仍然是一组非常多样化的独立工具。

这些独立的同类最佳工具可能有助于团队成员相互协作，但问题是如何最好地集成和调整当前的工作流以利用这些工具。实现这些工具的任务经常落在单个团队身上，导致团队之间缺乏一致性。

### **Best in Class DevOps**

**自己动手 DevOps**

自己动手(DIY) DevOps 构建于所有同类最佳工具之上。这个阶段的团队执行大量定制工作来将他们的 DevOps 点解决方案集成在一起。然而，由于这些工具是独立开发的，它们可能永远都不合适。不同的工具报告和与多个数据点交互的方式可能会有很大的不同，就像那些属于 DORA research 的工具一样。任何处理过大规模数据集成项目的人都会告诉你这是一场多么巨大的斗争。

这些 DIY DevOps 工具需要大量的升级和更改工作。对这些工具中的每一个进行修改都很容易破坏脆弱的集成点，甚至有可能使数据与当前数据集不兼容。与坚持使用同类最佳产品相比，所有这些努力实际上会导致更高的拥有成本。更糟糕的是，随着工程师花时间维护工具集成而不是致力于他们的核心产品，你又回到了螺丝刀和服务器。

### **Do It Yourself DevOps **

GitKraken 的构建考虑到了团队协作和开发人员体验，优先考虑我们的 Git 客户端跨操作系统的相同体验，使您的团队可以轻松采用简化的流程。

These DIY DevOps tools require significant effort for upgrades and changes. Modification to each of these tools can easily break the brittle integration points or even threaten to make the data incompatible with current data sets. All of this effort actually results in a higher cost of ownership when compared to just sticking with Best In Class. Worse yet, with engineers spending time maintaining tooling integration rather than working on their core product, you’re back to screwdrivers and servers.

**devo PS 平台方法**

需要一种平台方法来改善团队体验和业务效率。许多大型组织将他们的 DIY 工具称为“平台”，但他们缺乏单个应用程序、单个数据平面和用于用户角色管理的单个位置的集成。

devo PS 平台是一个单一的应用程序，由一致的用户体验提供支持，独立于自我管理或 SaaS 部署。它建立在具有统一数据存储的单一代码基础上，允许组织解决 DIY 工具链中的所有这些低效和漏洞。

DevOps 平台方法允许组织替换他们的 DIY DevOps。这允许对开发运维生命周期的所有阶段进行全程可见性和控制。一个公司的生存取决于它发布软件的能力。为了更快地发布更好的软件，您必须能够控制、度量和理解您的团队是如何构建软件的。

## 让团队团结起来不能只是一句口号。这必须是我们组织运作模式的现实。没有这一点，你永远不会理解和实现 DevOps 的真正承诺。用更实际的术语来说，这意味着让团队使用相同的工具来优化团队生产力。这一举措缩短了部署频率、MTTR 的周期时间，并降低了变更失败率。

A platform approach is needed to improve both the team experience and business efficiency. Many large organizations refer to their DIY tools as “platforms,” but they lack the integration of a single application, a single data plane, and a single place for user role management.

A DevOps platform is a single application, powered by a cohesive user experience, agnostic of being self-managed or SaaS deployed. It’s built on a single code base with a unified data store which allows organizations to resolve all these inefficiencies and vulnerabilities in DIY toolchains. 

A DevOps platform approach allows organizations to replace their DIY DevOps. This allows for visibility throughout and control over all stages of the DevOps lifecycle. A company’s very business survival depends on its ability to ship software. To ship better software faster, you have to be able to control, measure, and understand how your teams are building that software.

Bringing teams together can’t just be a catchphrase. It has to be the reality of the operating model of our organizations. Without that, you’ll never understand and realize the real promise of DevOps.

In much more practical terms, this means moving teams to using the same tools to optimize for team productivity. This move improves cycle time for deployment frequency, MTTR, and reduces the change failure rate.