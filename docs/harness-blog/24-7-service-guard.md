# 利用线束 24/7 服务防护|线束，快速移动和快速固定

> 原文：<https://www.harness.io/blog/24-7-service-guard>

自从[线束](https://www.harness.io)开始每天部署到生产中已经有几年了。
我们的目标不仅是销售持续交付，还要以身作则，通过我们的 staging-dev harness io . kin sta . cloud 平台吃我们自己的狗粮。
我们需要依靠广泛的自动化和监控来保持我们的高速度，而不是让我们的待命团队心脏病发作。
如果你有幸在一家进行过这些投资的机构工作，你可以:

1.  将您的服务部署到开发或 QA 环境中。
2.  运行您的自动化/性能套件。
3.  在你的服务投入生产之前，使用 Harness 的机器学习来标记异常。

你才能真正实现我们的使命宣言:*“动作要快，不要弄坏东西！”*
但是让我们诚实地说:除非你正在部署简单的应用程序，否则不可能在 QA 中模拟生产。
作为一个部署平台，Harness 平台功能的广度和深度完全排除了这种可能性。我们都同意这是拥有复杂应用程序的大型企业的共同主题。
那么，我们真正的座右铭应该是什么？我们当然想快速行动，但我们也想更快地解决问题。在 Harness，我们设计的 24/7 服务保护产品正是为了做到这一点。
实现这一目标有 3 个主要的组成部分:

1.  你当然需要[挽具](https://harness.io))——否则你怎么能获得持续交付和 [24/7 服务保护](https://harness.io/2018/12/harness-24-7-service-guard-empowers-developers-with-total-operational-control/)能力？如果你在想“等等，我有 APM”，你需要读一下“[APM 今天怎么了](https://harness.io/2019/09/what-ails-application-performance-monitoring-a-data-scientists-perspective/)”
2.  您需要为您的生产环境识别和收集应用信号。这些信号被输入到 [24/7 服务保护装置](https://harness.io/2018/12/harness-24-7-service-guard-empowers-developers-with-total-operational-control/)中，利用其机器学习来标记异常。以下是适用于大多数应用的信号:

***事务性指标*** :您可以在下面的
中看到一个我们如何捕捉一个关键事务的响应时间增长的例子

下面是另一个例子:当整个应用程序响应时间开始恶化时:

如果你是一个敏锐的观察者，你会问:“热图是干什么的？”问得好。它代表风险，在 15 分钟内从绿色到红色进行颜色编码。颜色越暖，风险越大。
一个眼神告诉你所有你需要知道的关于你在过去 1 小时内的反应时间的健康状况:

***JVM 指标*** :

我将停止在图片上画圆圈。我想现在你可以很容易地看到线程数在早上 7:30 开始攀升，在短时间内几乎翻了一番。
***数据库指标*** :我们使用 Mongo 和 TimescaleDB。在监控 Mongo 时，操作计数器、文档插入/删除等方面的指标非常有用。

我已经开始厌倦抓截图了。让我把所有其他的信号放在这里:

*   队列大小
*   来自 Codahale 注册表的应用程序内部指标
*   基础设施指标:来自 Stackdriver 的 CPU、内存等(我们是一家 GCP 商店)
*   来自 Stackdriver(也是 GCP)的应用程序日志
*   容器重新启动
*   网关/负载平衡器指标
*   UI 的错误日志

我们[将](https://docs.staging-devharnessio.kinsta.cloud/article/myw4h9u05l-verification-providers-list)与大量供应商整合，并支持多种[策略](https://docs.staging-devharnessio.kinsta.cloud/article/myw4h9u05l-verification-providers-list)。
这是一个名为“生产”和“免费增值”的两个环境中一项名为“验证服务”的特定服务的 24/7 Service Guard 仪表盘的冷视图

您可以看到不同信号的热图特征，以及用红色和绿色圆圈表示的部署事件。
一旦您启用了 24/7 Service Guard，您就可以使用通知系统配置基于风险的警报。我们使用 Slack 和 PagerDuty。

我们在上午 9:31 收到了一个关于 Slack 的警报，它也被发送到了 PagerDuty。值班人员介入，情况在上午 9:49 得到解决，如事故关闭警报所示。快速的转变。还不错！！好了，你知道了。这是马具，真正的马具就是马具。对于那些怀疑者，他们认为这是我精心设计的一个策略来伪造所有的 ML 能力，我有一个桥卖给你:)
但是说真的，这里有一个关于“[Build.com 如何检测问题并在生产中回滚”的视频](https://harness.io/2018/01/how-build-com-rolls-back-in-production/)
我们使用[无监督技术](https://www.youtube.com/watch?v=ZO5otWQ4PIc&feature=youtu.be)对任何时间序列的各个方面进行建模(首先阅读“[APM 今天遇到了什么问题](https://harness.io/2019/09/what-ails-application-performance-monitoring-a-data-scientists-perspective/)”)。由于这是我们不拥有的实时数据，它必须符合无监督学习的类别(意味着没有标签)。用无监督深度神经网络进行预测建模。如果你想知道引擎盖下所有的机器学习细节，那你就得加入我的团队:)。本次讲座到此结束。记住*“行动要快，但修复要快。”*
干杯！Sriram。