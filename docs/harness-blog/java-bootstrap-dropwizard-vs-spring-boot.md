# Java Bootstrap:drop wizard vs . Spring Boot | Harness

> 原文：<https://www.harness.io/blog/java-bootstrap-dropwizard-vs-spring-boot>

如何在尽可能短的时间内启动一个生产就绪的 Java 应用程序。

我不是一个喜欢早起的人，所以有时需要一段时间才能得到“所有系统都准备好了”的提示。直到不久前，Java 应用程序也是如此，但与闹钟上的贪睡功能不同，我们在这里讨论的解决方案实际上更有意义。有了像 Dropwizard、Spring Boot、Groovy 的 Grails 和 Scala 的 Play 这样的现代开源框架！您可以在几分钟内从头开始构建一个生产就绪的应用程序。即使你不是一个早起的人。即使你不喜欢巫师帽。在这篇文章中，我们将讨论基于轻量级 Java 的框架与 Dropwizard 和 Spring Boot 的异同。

嘶！无论你决定使用 Dropwizard 还是 Spring Boot，确保你拥有保持应用质量所需的一切。OverOps 跟踪软件质量，并在软件交付生命周期的所有阶段为所有错误和减速提供可操作的代码级洞察。

## 权衡:选择的自由与速度的需求

无论你选择哪种框架，你都要牺牲一些选择的自由，因为 Dropwizard 和 Spring boot 都非常固执己见，并且坚信约定胜于配置。有多强？在我们所做的并排比较中，您会发现每个第三方库都加入了不同的风格。生产级应用程序所需的大部分(如果不是全部)核心功能都是现成的或作为集成提供的。

这种牺牲的好处是速度，尽管摆弄新的库和定制自己的完美环境有时很有趣。当你需要快速做一些事情并开始运转时，你最好将这些决策委托给别人，并摆脱随之而来的复杂性。这并不完全是蓝色药丸对红色药丸的场景:当您开始运行时，如果需要，您很可能能够定制和调整它。现在只要把你最喜欢的构建工具，不管是 Gradle 还是 Maven，指向 drowizard & Spring Boot，你就可以开始了。

让我们深入研究并发现每个框架的哪些方面会使您受到限制，哪些方面可以更加灵活。

剧透警告:我们在 OverOps 这里面临类似的困境，并决定使用 Dropwizard 为企业提供 OverOps 的内部版本。但是曾经被视为 Dropwizard 默认(也是唯一)的选择，让我们打破了对 Spring boot 和令人疲惫的 XML 配置的偏见。

## Dropwizard vs. Spring Boot:谁支持你？

生产级应用依赖于许多组件，每个框架都为我们做出了选择。让一个 RESTful web 应用起飞的所有武器都摆在这张桌子上，左边是戴着巫师帽的 Dropwizard，右边是穿着绿色短裤的 Spring Boot。核心的开箱即用库和插件用颜色分开，Spring 的内部依赖项用白色标记。

好了，现在我们对这片土地有了更好的了解，让我们看看它到底告诉了我们什么。我还建议仔细看看每一个框架，因为所有的东西都是开源的，你可以在 GitHub 上愉快地查看:这里是 Dropwizard 源文件，这里是 Spring Boot。

### 弹簧依赖关系

就像电视上说的那样，Spring Boot 专注于春季应用。因此，如果您想进入 Spring 生态系统或者已经熟悉它，并且需要建立一个快速的应用程序，这可能是一个不错的选择。REST 支持和 DevOps 特性(我们很快会谈到，如指标和健康检查)基于 Spring 框架的核心，而 DropWizard 将其 REST 支持放在 Jersey 中。这几乎是 Spring Boot 让你陷入困境的唯一方面，尽管它在其他方面更灵活(编辑:JAX-RS 支持是在 1.2 版添加到 Spring 的，也可以用于 Jersey。谢谢皮特。)

### HTTP 服务器

在这里我们可以看到 Spring Boot 是如何变得更加灵活的。Dropwizard 采用了比 Spring Boot 更极端的约定胜于配置的方法，并且完全基于 Jetty，而 Spring Boot 默认采用了 Tomcat 的嵌入式版本，但是如果你更喜欢 Jetty 甚至 RedHat 的 Undertow，你就有办法了。

### 记录

这是关于配置问题的相同约定的另一个例子，Dropwizard 在 v0.4 中从 log4j 切换到 Logback。我猜随着 log4j2 最近的 GA 发布，这可能会发生变化(编辑:看起来不是这样，请查看评论部分)。在 Spring Boot 方面，如果我们需要日志记录，我们需要在 Logback、log4j 和 log4j2 之间做出选择。顺便说一下，如果您正在使用 Logback，您可以查看我们运行的这个基准测试，以比较不同日志记录方法的性能。

### 依赖注入

两个框架之间的主要区别是依赖注入支持。正如你们中的一些人所知道的，Spring 的核心是内置的依赖注入支持，而 Dropwizard 不是现成的，你必须选择一个支持它的社区集成。一个流行的选择是使用 Google 的 Guice，并使用一个社区主导的集成。

### 测试–Fest vs . ham crest

两个框架都有专门的测试模块，dropwizard-testing 和 spring-boot-starter-test，包括 JUnit 和 Mockito 依赖关系。Spring Boot 自然也使用 Spring Test，这里的主要区别在于 matcher 对象的形状，检查不同的对象是否匹配相同的模式。Dropwizard 偏爱 FEST matchers(已经不开发了)(编辑:最新 0.8 版本移至 AssertJ)Spring Boot 去了 Hamcrest。

### 没有运营就没有发展

为了赢得生产级应用程序的称号，每个框架的核心特性都包括对指标、健康检查和任务的支持。简而言之，度量允许您跟踪统计数据，如内存使用情况和执行代码中的区域所花费的时间。健康检查是一种在旅途中进行测试的方式，可以回答诸如“这个插座还开着吗?”之类的问题。还是数据库连接是活动的？通过任务支持，您可以安排维护操作或定期任务。

Dropwizard metrics library 本身就很受欢迎，你可以将它添加到任何项目中，甚至可以与 Spring Boot 的 metrics 一起使用，以深入了解你的代码在生产中做了什么。一个很酷的特性是向 Graphite 或 Ganglia 等服务以及 20 多个可用的集成进行报告。Dropwizard 指标也提供健康检查，任务作为框架的一部分实现。在 Spring Boot 方面，框架使用 Spring 的核心特性来支持它的 Ops 角度。

更新(2018 年 3 月 13 日):从 Spring Boot 2.0 开始，Micrometer 作为应用度量门面被引入(就像 SLF4J 一样，但用于度量)。

## 关于无容器化的说明

促成 Dropwizard 创建的关键驱动因素是无容器 Java HTTP 服务器，几年后 Spring Boot 也紧随其后。与独立容器不同，您可以像在应用程序中添加任何其他库依赖项一样简单地添加 HTTP 服务器。简单明了，易于更新，你不需要处理任何 WAR 文件。XML 配置保持在最低限度。至于部署，Dropwizard 和 Spring Boot 都利用 fat JARs 将所有 jar 和它们的依赖项打包到一个文件中，使得使用快速的一行程序进行部署更加容易。

## 社区和发布周期

Dropwizard 最初是由 Coda Hale 在 2011 年末发布的，当时他还在 Yammer。从那时起，它通过了大约 20 个版本，目前是 0.8 版本，作为现代 Java 应用程序的指南，享有巨大的社区支持。不利的一面是，在过去每隔几个月发布一次之后，新版本的发布速度正在放缓。在最新的 0.8 版本中，我们主要看到第三方版本的更新和小的修复。Dropwizard 目前支持 Java 7 和更高版本，要在 Java 8 上使用它，您可以查看这个部分更新以享受它的一些好处和新功能(或者如果您出于某种原因不喜欢 joda-time)。

(编辑:此处提供了 Dropwizard 模块的完整列表)。

今天，你可以看到来自 Jochen Schalanda(编辑:和 0.8 团队)的大部分提交，有超过 160 名个人贡献者和数十个社区支持的集成，如 Datasift 的 drowizard-extra。在可用的 Dropwizard 集成中，还包括了 Spring 支持。还有一件事你一定要看看，就是这里的官方用户组。

随着 Pivotal 支持的 Spring Boot 在 2014 年以 1.0 版本加入游戏，有超过 40 个官方集成(Starter POMs)到几乎任何你能想到的第三方库。这包括从日志到社交 API 集成的任何东西。这里值得一提的一个新 Spring Boot 项目是 JHipster，它是 Spring Boot 和 Angular 公司的约曼发电机。

从根本上说，你可以说 Dropwizard 周围有一个更大的社区，Spring Boot 享有更好的官方和结构化支持，以及 Spring 现有的用户基础。

## 结论

1.如果你想进入春季生态系统，那么选择 Spring Boot 可能是显而易见的。它不仅仅是一个引导 RESTful Java 应用程序的方法，它还作为 Spring 的网关，集成了数十种服务。也许这里真正的问题是你是否应该开始寻找/回到春天？这可能是另一个需要讨论的话题。否则，Dropwizard 将最适合您的需求。

2.这里的第二个问题是你对依赖注入的依赖程度如何？如果 Guice 是你的选择，那么使用 Dropwizard 和一个社区集成将是一个简单的解决办法，而不是像 Spring 那样进行依赖注入。

3.最后但并非最不重要的一点是，看一看并列比较，如果您自己从头开始构建一个应用程序，您会选择哪个框架？请记住默认的选择，因为花费更多的时间来配置这个引导有点背叛了它的原因。

我希望你已经发现这个比较有用，我很高兴听到你对它的评论，以及让你选择一个而不是另一个的因素。