# 如何在敏捷开发中使用快速反馈循环

> 原文：<https://www.gitkraken.com/blog/feedback-loops-agile-development>

*文章更新于 2020 年 9 月*

我最喜欢的格言之一是“快速移动，打破东西”。不幸的是，当谈到软件时，我们倾向于打破东西，而不是移动得很快。敏捷开发的目标是以更高的速度发布代码。关键是足够有效地管理过程，同时不破坏东西——或者至少减少东西。

在这篇文章中，我将提供一些关于如何实现 DevOps 和使用 Scrum 框架快速反馈循环来提高速度和质量的技巧。

*AXO soft 的 Scrum 开发时间表*

什么是反馈回路？

## 创建好的软件最关键的是沟通。反馈循环是用于验证和获得关于软件开发过程的反馈的机制。目标是获得能够立即反馈到流程中的正反馈和负反馈。尽可能快地这样做可以加速和改善整个开发过程。

反馈回路的类型

反馈循环不仅仅是验证你写的代码是否符合用户的需求。尽管这很重要。知道你的代码是否有效，是否充满了错误也是很重要的。反馈循环是**日常最佳实践、自动化和工具**的混合体。你最不希望的事情就是让你的用户对你所做的事情真正感到兴奋，然后当事情闹得满城风雨时，他们会发疯。

## 每日混战

能够快速说出你的进展并向你的整个团队提出简单的问题是持续分享反馈的一个很好的方式。简单地提及你正在做的事情可能会激发队友提及你可能想要避免的潜在问题。

召集您的团队进行一致的面对面会议，即使你们都远程工作，也可以提供有价值的集体项目可见性。每日例会(有时被称为每日站立会议)是一个向你的团队寻求反馈或帮助的好地方，这样你就可以让你的项目继续前进。

### Daily Scrum

会见利益相关者并获得用户反馈

没有什么比用户反馈更重要的了。你最不想做的事情就是花很多时间走错方向。同样，让任何有权改变项目基本方面的利益相关者参与进来，对你也有好处。

避免执行瓶颈，同时通过 [GitKraken Timelines](https://www.gitkraken.com/timelines) 等工具为正在进行的项目提供透明度，使项目时间表和里程碑的规划和沟通变得轻松愉快。

与利益相关者和用户会面是至关重要的，而且不一定要花很多时间。尽量避免持续的有组织的会议，这可能是一个很大的时间吸。相反，利用 Slack 这样的交流工具来保持联系。鼓励用户提交特性请求和 bug 报告的社区 Slack 频道可以很好地收集用户反馈。

### *在[吉克拉肯湖社区](https://gitkraken.slack.com/)的讨论*

没有什么比用户反馈更重要的了。你最不想做的事情就是花很多时间走错方向。同样，让任何有权改变项目基本方面的利益相关者参与进来，对你也有好处。

避免执行瓶颈，同时通过 [GitKraken Timelines](https://www.gitkraken.com/timelines) 等工具为正在进行的项目提供透明度，使项目时间表和里程碑的规划和沟通变得轻松愉快。

考虑利用项目管理工具，比如与 Slack 集成的 GitKraken Boards，这样你的团队就可以跟踪日常对话中出现的任务。

代码分析和跟踪

你的代码刚才做了什么？表现如何？开发人员现在可以使用一些神奇的工具来帮助实时回答这些问题。当您在工作站上编写和测试代码时，您可以获得关于代码执行情况和正在做什么的即时反馈。这些[应用性能管理](https://stackify.com/what-is-apm/) (APM)工具可以向您显示 SQL 查询、HTTP web 服务调用、错误、日志消息等等。查看[免费 APM 工具](https://stackify.com/application-performance-management-tools/)，如 Stackify Prefix、DevTrace、MiniProfiler 等。它们因您的编程语言而异。

此外，像 [Grafana](https://www.gitkraken.com/resources/devops-report-2020#grafana) 这样的监控工具能够本地支持来自多个来源的数十个数据库。生成热图、直方图、地理图、定制仪表板等。

拉式请求和代码审查

### [拉请求](https://support.gitkraken.com/working-with-repositories/pull-requests/)可以帮助确保你的代码在准备好之前不会被合并和部署。当很多人不停地签入代码时，很难知道您是否准备好进行部署。拉请求也提供了一个很好的机会来做一些快速的代码审查。在发布代码之前，来自团队的反馈对于发现潜在问题至关重要。

你的代码刚才做了什么？表现如何？开发人员现在可以使用一些神奇的工具来帮助实时回答这些问题。当您在工作站上编写和测试代码时，您可以获得关于代码执行情况和正在做什么的即时反馈。这些[应用性能管理](https://stackify.com/what-is-apm/) (APM)工具可以向您显示 SQL 查询、HTTP web 服务调用、错误、日志消息等等。查看[免费 APM 工具](https://stackify.com/application-performance-management-tools/)，如 Stackify Prefix、DevTrace、MiniProfiler 等。它们因您的编程语言而异。

在这个中级 Git 教程视频中，了解更多关于 Git 中的 pull 请求以及如何使用 PRs 提升您的工作流的信息。

[**学习 Git:什么是拉取请求？**](https://www.gitkraken.com/resources/video-pull-request)

### 拉式请求和代码审查

“拉”请求实质上是请求某人在您的更改最终完成之前对其进行审核。

*拉请求在[Git kraken Git GUI](https://www.gitkraken.com/git-client)*

在这个中级 Git 教程视频中，了解更多关于 Git 中的 pull 请求以及如何使用 PRs 提升您的工作流的信息。

作为 Git 客户端，GitKraken 更进一步，支持来自 GitHub、GitLab 和 Azure DevOps 存储库的 pull 请求模板。[了解如何通过模板](/blog/enhancing-pull-request-descriptions-templates)增强拉取请求。

立即开始使用 GitKraken Git 客户端处理拉请求。

*拉请求在[Git kraken Git GUI](https://www.gitkraken.com/git-client)*

持续集成和部署

您只能以部署代码的速度发布代码。自动化构建和部署的方式至关重要。它消除了人为错误，加快了流程。在您完成一个部署并需要快速修复一个 bug 之后，能够检查它并进行快速的新部署是非常重要的。利用持续集成来每天或持续运行单元测试也是一个非常有价值的反馈循环。

通常，持续集成(CI)包括每天多次将代码合并到共享的 repo 中，然后进行构建和自动化测试。[devo PS 领域流行的 CI/CD 工具](https://www.gitkraken.com/resources/devops-report-2020#ci-cd)包括 Jenkins 和 Travis CI。

立即开始使用 GitKraken Git 客户端处理拉请求。

在生产前环境中验证性能

### 希望您的 QA 团队在测试您的应用程序方面做得很好。在 QA 过程中，这是寻找应用程序错误和审查整体性能的好时机。应用监控解决方案可以帮助您做到这一点。如果你的应用在预生产环境中没有任何流量，合成测试和负载测试会有所帮助。

在一个完美的世界里，你希望在软件进入生产之前找到它们。谢天谢地，市场上已经出现了测试工具，这使得 QA 团队的工作变得更加容易。

单元测试

测试框架应该用来为跨团队创建和设计用例提供高层次的指导方针。一些面向 DevOps 开发人员的测试工具包括 JUnit、Jest、Selenium 和 PhPUnit。

### 单元测试、集成测试、自动化 web 测试等等，提供了一个快速的反馈循环。我开发的一个应用程序有超过 100 个复杂的集成测试。每当我对代码进行任何修改时，我都会在我的工作站上重新运行所有的测试，以确保我没有破坏任何东西。这些测试对我来说是一个关键的快速反馈环。没有它们，我无法对应用程序进行更改！

希望您的 QA 团队在测试您的应用程序方面做得很好。在 QA 过程中，这是寻找应用程序错误和审查整体性能的好时机。应用监控解决方案可以帮助您做到这一点。如果你的应用在预生产环境中没有任何流量，合成测试和负载测试会有所帮助。

监控生产绩效

你刚刚把你的新版本推向生产。恭喜你。！现在怎么办？这是一个很好的时机来**监控你的代码中出现的新错误**。很可能你会有几个。无论你在产品化之前对你的软件做了多少测试，你总会在产品中发现一些奇怪的问题。客户数据、流量和主机的差异都很难事先测试。

### 应用程序监控对于尽快发现潜在问题至关重要。您应该监控整体性能，以确保您的应用程序不会运行得更慢，使用更多的 CPU，等等。这些都是潜在的问题，如果您使用[应用程序监控最佳实践](https://stackify.com/application-monitoring/)，您可以快速检测并修复这些问题。

Azure 是微软的一个工具，它让你的应用程序、基础设施和网络变得非常透明。[为您的开发运维战略获取更多监控工具](https://www.gitkraken.com/resources/devops-report-2020#monitor)。

*微软 Azure Monitor*

跟踪产品使用情况

### 你知道有多少客户在使用你的新功能吗？了解你的产品是如何被使用的是一个重要的反馈环。有几种方法可以做到这一点。

你可以使用简单的 Google Analytics，但是如果你的应用程序使用 REST 风格的 URL，它就不能很好的工作。如果你正在使用 APM 解决方案，如 Retrace、New Relic、App Insights 等。，它们可能能够提供一些关于代码的某些部分被访问的频率的见解。如果你想要更高级的功能，试试[完整版](https://www.fullstory.com/)。看起来很神奇。

摘要

每个开发团队和软件项目都是不同的。你的目标应该是找出在保持高质量的情况下你能走多快，然后就走那么快。如果走得太快降低了质量，那么你就有一个好主意，在哪里释放加速器。希望这些提示和快速反馈会有所帮助！

*微软 Azure Monitor*

跟踪产品使用情况

### 你知道有多少客户在使用你的新功能吗？了解你的产品是如何被使用的是一个重要的反馈环。有几种方法可以做到这一点。

你可以使用简单的 Google Analytics，但是如果你的应用程序使用 REST 风格的 URL，它就不能很好的工作。如果你正在使用 APM 解决方案，如 Retrace、New Relic、App Insights 等。，它们可能能够提供一些关于代码的某些部分被访问的频率的见解。如果你想要更高级的功能，试试[完整版](https://www.fullstory.com/)。看起来很神奇。

摘要

每个开发团队和软件项目都是不同的。你的目标应该是找出在保持高质量的情况下你能走多快，然后就走那么快。如果走得太快降低了质量，那么你就有一个好主意，在哪里释放加速器。希望这些提示和快速反馈会有所帮助！

## Summary

Every development team and software project is different. Your goal should be to figure out how fast you can go while maintaining high quality, and then go that fast. If going any faster reduces quality, then you have a good idea of where to let off the accelerator. Hopefully, some of these tips and fast feedback loops will help!