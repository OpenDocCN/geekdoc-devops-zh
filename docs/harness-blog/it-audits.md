# IT 审计-持续交付如何帮助|驾驭

> 原文：<https://www.harness.io/blog/it-audits>

大家最喜欢的词，IT 审计。不像在大学里审计一门课，审计并不局限于 IT 风险人群。

审计这个词甚至比我们关于[日志](https://harness.io/blog/modern-log-layers/)的最新帖子更能唤起人们的情感。在保密的情况下，IT 审计可能是一项资源密集型工作。IT 审计触及人员、流程和技术[类似于 [DevOps 咒语](https://harness.io/blog/devops-lessons-learned-from-the-field/) ]。IT 审计肯定有许多种类，从能力评估到基于合规性/法规的审计。

IT 审计旨在确保我们的控制是有效的。取决于我们何时进入一个项目，永远悬而未决的 IT 审计将检查在我们进入项目之前做出的技术和控制决策。随着我们对系统和平台的了解，我们必须开发新项目的功能，并且解释系统和平台是如何组合在一起的会很有挑战性。尽管如此，IT 审计并不全是坏事。事实上，它们对于满足法规要求非常重要。即使您的组织没有受到监管，IT 审计仍然可以在您的未来。

### 您获得 IT 审计的机会有多大？

你不一定要成为向联邦政府提供服务的医疗保健提供商才能被审计。几乎任何组织都有某种审计需求要满足。即使你是一个小公司，最终你也会遇到一个想要一份审计报告的客户。此外，随着你的创业公司的成长，拥有某种基线可能是一个好的实践。

法规也可能是您的组织进行审核的驱动力，如果您的组织出现了以下缩写之一，那么您的组织很可能会进行法规审核。这些类型的审核频率从一年一次到一年多次不等。

*PCI* : [支付卡行业数据安全标准](https://en.wikipedia.org/wiki/Payment_Card_Industry_Data_Security_Standard)【PCI DSS】:如果您根据一个月的处理量来进行信用卡支付，PCI DSS 会规定所需的安全标准。

*HIPAA* : [健康保险可携带性和责任法案](https://en.wikipedia.org/wiki/Health_Insurance_Portability_and_Accountability_Act):更加关注责任部分、患者隐私和保护，特别是当我们转向电子健康记录的时候 [EHR](https://en.wikipedia.org/wiki/Electronic_health_record) 。

*FISMA* : [联邦信息安全管理法案](https://en.wikipedia.org/wiki/Federal_Information_Security_Management_Act_of_2002):如果你接触任何联邦的东西，FISMA 都会对你产生影响。

另一种常见的 IT 审计被称为 [SOC 2](https://www.aicpa.org/interestareas/frc/assuranceadvisoryservices/aicpasoc2report.html) 。SOC 2 源于美国注册会计师协会(American Institute of CPAs[[AICPA](https://www.aicpa.org/)]，衡量存储在云中的数据的安全性、可用性和完整性。对于大多数采用 SaaS 模式的公司来说，在成熟的某个阶段，通过 SOC 2 审计是正常的。

IT 审计还可以用来衡量您组织的 IT 环境的能力和成熟度。最有可能的是，能力审计将由专门从事您的特定行业的咨询公司或分析公司来执行。与法规/合规性审计不同，基于能力的 it 审计更加特别，由业务决策触发。

尽管 IT 审计非常耗时，但它包括信息收集和发现共享的一些基本组成部分。

### 参加审计是什么感觉？

通常在审计中，审计人员需要根据补偿控制的证据来回答问题。这有点像展示你的数学成果。就审计而言，需要有一个设定的范围(尽管可以感觉到可以很宽)。

例如，审计需求可能是在面向客户的系统中密码是安全的。补偿控制是至少六个字符长的密码。审计员可能需要密码标准和策略证明的副本，并且可能想要运行该策略。你可以看到这是多么耗时。

与审计人员保持持续的关系也有所帮助，因为有一个尽职调查期，审计人员可以了解业务以及为什么要实施哪些控制措施。

补救期可以是审计正在进行的时候，如果有负面的发现，则在此之后的某个时间，您可以有一些时间来进行建议的更改。从监管的角度来看，如果你的组织真的出了问题，市场可能会疯狂争抢。

通过利用连续交付解决方案，您可以拥有一个计划记录系统，允许更短的尽职调查周期，并可能有更多的时间与审计师一起证明/补救，并减少疯狂的争夺。

### 连续交付——记录的程序系统

我在多德-弗兰克法案监管下的银行工作，我们有一个奇怪的要求。我们被要求回顾五年前围绕我们所做的信贷批准或拒绝的自动化/程序化决策。基本上，我们对我们使用的数据点和业务规则进行了重播。

数据点的数据库很好，但是应用程序/业务规则是个麻烦。简而言之，我们必须根据某个日期重新部署应用程序，如果有要求提供更多细节的请求的话。现在有了比 JAVA 发行版更多的东西[又名[战争](https://en.wikipedia.org/wiki/WAR_(file_format)) ]。

因为您的持续交付解决方案应该是您的发布/部署的核心，所以建立信任的步骤以及应用程序和工件的组成很容易推断。回到过去可能是连续交付解决方案的一个用例；Harness 平台允许通过编程访问来创建审计跟踪。

### 探索 API Explorer -审计跟踪

我们一直忙于在 Harness 平台上用我们的 [GraphQL API](https://developer.harness.io/docs/platform/apis/api-quickstart/) 来构建功能。使用 API 的一个很好的方法是 Harness API Explorer。在 [Harness API Explorer](https://developer.harness.io/docs/first-gen/firstgen-platform/techref-category/api/harness-api-explorer/) 中，您也可以通过编程访问您的组织内部正在发生的事情。我们提供了一个[示例查询](https://developer.harness.io/docs/platform/apis/api-quickstart/)，在 Harness 平台内部生成一个典型的审计跟踪。

在我们的一个脊甲培训环境中，我正在为下一批学生准备环境，下面我们可以看到我在这个环境中做了什么。

要利用可视化 API 资源管理器，请导航到设置-> API 资源管理器。

通过运行[审计查询](https://developer.harness.io/docs/first-gen/firstgen-platform/techref-category/api/use-audit-trails-api/)，我们可以看到我一直在运行的所有删除，为下一组学生准备好环境。

聚焦于删除:

就这样，你对平台上发生的事情有一个非常全面的系统记录，也就是你的审计跟踪。

### 线束，IT 审计的快速通行证

Harness 当然适合作为您的发布和部署的记录系统。Harness 是建立信任步骤的指挥者，例如批准和测试覆盖；当执行线束工作流/管道时，会捕获建立信任的步骤。回到我工作的银行，马具会是天赐之物。不要害怕 IT 审计-帮助增压您的经验与利用今天！

干杯，

-拉维