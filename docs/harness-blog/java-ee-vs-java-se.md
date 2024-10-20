# Java EE vs Java SE:甲骨文放弃企业软件了吗？装具

> 原文：<https://www.harness.io/blog/java-ee-vs-java-se>

Java EE 与 Java SE 不同之处的详细解释。

**你需要知道的关于 Java EE 及其当前状态的一切**

Java 企业版是全球 Java 社区中最大的困惑来源之一。奇怪的是，即使你有 EE 开发的经验，完整的画面通常还是模糊的。在本帖中，我们将在 Java EE 8 专家组的 Werner Keil 和前 Oracle Java EE 宣传者和 Java EE guardians 创始人 Reza Rahman 的帮助下，了解所有最新消息，并进一步了解 Java EE，以拨开迷雾。

## Java EE 和 Java SE 到底有什么不同？

首先，我们需要做一个重要的区分。Java EE 构建在 Java SE 之上。与 Java SE 不同，企业版 Java 正式“仅仅”是一个规范，Oracle(如 Glassfish 参考实现)和其他供应商(如 RedHat 和 IBM)提供了实际的实现。

而 SE 的 API 提供了 java 语言(Java)的标准核心功能。*包)、EE 的 API(javax。*)提供对 Java 的扩展，对开发大规模应用程序非常有用。也就是说，可能会有导致额外混乱的异常。例如，Swing 一开始是一个扩展，最终成为核心 Java 的一部分。这不是防弹概念。

我们与 Java EE 8 专家组成员 Werner Keil 取得了联系，以获得进一步的见解。“最大的误解是 API 是否是规范的编码表现，或者更确切地说是它的实现，”Werner 说。“几乎每个 Java EE 项目现在都认为它是实现，因此绝大多数项目的所有代码都受到日益开放的许可证的保护。除了技术兼容性工具包(TCK)测试套件之外，专有的封闭 TCK 仍然存在很大问题，只有 Oracle 和公司授权厂商才能访问。”

另一方面，这些类型的许可问题最终导致 Apache Software Foundation 在 2010 年从 Java 社区流程执行委员会中撤销了其成员资格。

## 那么 EE 规范实际上包括什么呢？

实际上，Java EE 是企业 Java 扩展的总括规范。在其核心，它包括独立的功能，如企业 Java bean(EJB)、Java Servlet、Rest API (JAX-RS)、上下文和依赖注入(CDI)等等。

每个新版本都包括对个别技术的升级，以及新的功能。例如，Java EE 8 有望包含支持 HTTP 2.0 的 Servlet 4.0 规范。

因为 Java 是向后兼容的，所以您也可以在新的 SE 版本上运行旧的 EE 版本，并享受新的语言特性。例如，在 Java SE 8 之上为 lambdas 和 streams 提供了一个兼容 Java EE 7 的实现，因此您不需要等待 Java EE 8 来使用它。

Java EE 的一个主要特性是 Servlet 规范。目前是 3.1 版，4.0 版正在开发中。它最流行的实现之一来自于 [TomEE](http://tomee.apache.org/tomcat-java-ee.html) ，这是 Tomcat 的 EE 兼容版本。

## 重量级 Java EE 是个神话

与普遍的看法相反，Java EE 比它看起来要轻得多。像工件大小、构建时间和部署时间这样的属性可能非常少。轻量级是一个设计决策，其他被认为是轻量级的框架可以变成…重量级。

“对 Java EE 最常见的误解可能是它太大、太重或太单一，而且不如 Play 灵活！、Spring、Node.js 或所有其他“时髦”的或新的或旧的替代品。我们让整个 Tomcat 或 Glassfish 服务器在树莓 Pi 上运行”——Werner Keil

## 在生产中调试 Java EE

对于分布式生产环境，尤其是微服务架构，一个反复出现的问题是了解生产中发生了什么。虽然不是特定于 EE 的，但是从一个服务开始的问题可能会在其他地方引起麻烦，然后你会独自一人在日志中挖掘，试图找到甚至可能不存在的线索。

我们正在采取一种新的方法来解决这类问题。每当发生异常、日志错误或警告时，我们都会提供所有需要的数据来找出其根本原因。这包括错误堆栈跟踪中所有相关的源代码和状态

## Java EE 与 Java SE 发布周期

Java EE 上的工作在一个单一的总括 Java 规范请求([这里是 Java EE 8](https://jcp.org/en/jsr/detail?id=366) 的请求)下管理，并等待 SE 完成以定义确切的规范。我们在下表中总结了所有版本的发布日期:

## 为什么 SE 之后 EE 发布一般要 2 年？

“我认为这是随着时间的推移而历史演变的。像 EJB 这样的第一批 J2EE 技术是在 1998 年提出的，比 Java 于 1995 年正式推出的时间晚了两年多一点。”沃纳·基尔说。“一旦大量的公司和贡献者开始在 JCP 下帮助 Java EE，企业技术自然需要一些时间来准备、测试和集成 EE 保护伞下的所有部分。”

Werner 补充道，“我个人认为，即使对于大型企业用户来说，将 Java EE 版本 X 严格绑定到同一个 JDK 版本 X 的必要性也变得不那么重要了。默认情况下，一些供应商已经开始将他们最新的 Java EE 6 或 7 兼容产品与 Java SE 8 捆绑在一起。”

“一旦 Java EE 8 准备就绪，我们将有希望看到 Java SE 9 及其 Jigsaw 模块化系统不仅是最终的，而且是相对成熟的。企业服务器可能需要一些时间来应对这个巨大的步骤，但是一旦模块化被正确理解和采用，我认为 ee 可能比 se 有更大的好处。数量相当少的 EE 配置文件应该增长并利用底层平台可以提供的所有可选性和模块化。”'

## Java EE 8 的现状如何？

Java EE 8 预计在 2017 年上半年发布。似乎我们预计会经历额外的延迟。Werner Keil 详细阐述了这些问题:

“不幸的是，不仅是由于 Java SE 9 的延迟，而且似乎是 Oracle 内部资源的强烈转移，以服务于其(私有)云客户，几乎每一个由 Oracle 主导的针对 Java EE 8 的 JSR 都被延迟了。”

“即使是 Oracle 作为共同规范领导者的 JCache，似乎也没有真正融入到 Java EE 8 中，只是缺少了企业功能的关键方面，如事务处理。”

“那些缺失的部分被专有的特定于供应商的扩展所覆盖，无论是 Oracle (Coherence)、Hazelcast 还是其他供应商。也许这就是最终的结局。”

Java EE 社区中的许多人普遍担心，Java EE 和相关标准越来越成为“遮羞布”,以覆盖专有的、大部分封闭源代码的产品或服务，这些产品或服务运行在“云中”,您只需租用和支付费用

由于看似不断变化的优先级，Oracle Java EE 传道者 Reza Rahman 与 Oracle 分道扬镳，并创建了一个名为 Java EE 守护者的社区驱动计划。“不偏不倚地来看，这不外乎是对 Java EE 的“采纳一个 JSR”。虽然很少尝试让 JUGs 或其成员通过 Adopt-a-JSR 计划采用 Java EE JSRs，但实际上它仅限于 Java SE 或独立的 JSR，Oracle 和主要 JUGs 的几乎所有活动都完全集中在 Adopt-OpenJDK 上。而在过去，企业部门总是被视为少数大型供应商的事情，如 IBM、BEA/Oracle 或 JBoss。”

“让 TomiTribe 或 Payara 这样的小公司以类似于 JBoss 的开源方式做出贡献，甚至 IBM 也押注于 OpenStack 或 WebSphere Liberty Profile 这样的开源驱动技术，这意味着范式的巨大转变，至少甲骨文的许多公司和法律人员似乎还没有完全理解。”

## Java EE 守护者

为了给这个新社区带来更多的信息，我们联系了礼萨·拉赫曼了解更多的细节。

“我们是一群对 Java EE 充满热情的人，他们非常关心 Oracle 对开放标准的承诺。我们承诺尽我们所能推动 Java EE 社区向前发展”——Reza Rahman

Reza 继续说道:“Oracle 和 Sun 对 Java EE 的发展方向一直有着不健康的影响。甲骨文目前的不作为使得这一现实的不利之处更加明显。从长远来看，我认为 Java EE 的正确答案是由社区和其他供应商(如 RedHat、IBM、Tomitribe 和 Payara)更加积极地推动。”

“Java EE 的现状非常令人担忧。该生态系统不断壮大，并一如既往地充满活力，背后有许多充满激情的人。尽管如此，Oracle resources 在 Oracle 主导的 JSR 方面的活动明显放缓。这将使满足当前的 Java EE 8 时间表变得非常具有挑战性，除非 Oracle 的明显行为发生变化，社区显著增加贡献或其他供应商弥补 Oracle 不活动留下的进展差距。”

‍