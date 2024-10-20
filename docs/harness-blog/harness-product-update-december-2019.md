# 线束产品更新| 2019 年 12 月|线束

> 原文：<https://www.harness.io/blog/harness-product-update-december-2019>

2019 年是 Harness 激动人心的一年！今年，我们将迎来数百名客户、200 多名员工和 300 多项产品发布。我们[完成了我们的 B 轮融资](https://harness.io/2019/04/helping-enterprises-gain-an-easy-button-for-continuous-delivery-announcing-harness-series-b-financing/),包括 Google Ventures 和 ServiceNow 加入这轮融资，总估值为 5 亿美元。我们推出了[持续洞察](https://harness.io/2019/10/harness-continuous-insights-know-your-ship/)、[吉拉票务支持](https://harness.io/2019/01/harness-introduces-atlassian-jira-ticketing-support/)、 [24x7 服务守护](https://harness.io/2019/02/harness-product-update-january-2019/)、[掌舵支持](https://harness.io/2019/05/helm-support-for-harness-continuous-delivery/)、an [AppDynamics 合作伙伴](https://harness.io/2019/06/harness-partners-with-appdynamics/)、[标签管理](https://harness.io/2019/08/introducing-tag-management/)、上的[、](https://harness.io/2019/02/harness-product-update-january-2019/)上的[、](https://harness.io/2019/08/august-updates/)上的[。我们很高兴能够在年底对我们的持续验证能力进行更多更新。当你为假期做好准备时，这里只是一些你可以享受的改进。](https://harness.io/2019/11/pipeline-governance-measuring-regulatory-compliance/)

# 持续验证

## 增强的可视化

当在管道中作为工作流验证性能时，验证窗口已得到改进，包括图表和指标作为交通灯。当您单击每个指标时，图表将自动更新。基本机制保持不变，但现在视觉效果有所改善。

## 修订活动类别和吉拉票务

日志消息现在分为三种类型:

*   异常事件
*   不冒险
*   已知事件

我们引入了将异常事件与 P0 和 P5 之间的优先级相关联的能力。您还可以将事件标记为*非风险*以及可选的*反馈注释*。下次 Harness 看到相同的事件时，它将继承您最初标记它的优先级。更多信息可以在[这里找到](https://developer.harness.io/docs/first-gen/continuous-delivery/continuous-verification/continuous-verification-overview/concepts-cv/harness-verification-feedback-overview/)。

你不仅可以优先处理你的问题，还可以直接从 Harness 创建一张吉拉机票。在查看您的简历工作流程中的问题时，点击蓝色菱形按钮自动创建吉拉机票。如果将来出现该问题，如果已经为同一问题创建了吉拉票证，则该票证将自动与该问题关联。

## 通过 JSON 定制连续验证源

无论您使用什么 APM 工具，如果您的 APM 供应商可以输出 JSON，那么 Harness 就可以连接到它。使用我们支持定制日志的相同连接器，我们现在可以连接到定制 APM 解决方案或可以输出 JSON 的供应商。一个完美的例子是 Scaylr。[更多详情可点击此处](https://developer.harness.io/docs/platform/Connectors/connect-to-monitoring-and-logging-systems#step-add-custom-health)。

## 服务保护更新

我们改变了表示指标的方式，也使用了新的红绿灯视图。以前，我们有一个平面表示，将每个事务和指标称为一行。现在，我们将每个事务分组为一个单独的行，并带有与我们监控的指标的运行状况相对应的红绿灯:

*   绿色:低风险
*   红色:高风险
*   格雷:我们收集，但不分析

# 再来点好吃的

*   如果您正在使用 Jenkins 多分支管道，Jenkins 工作流命令现在将允许您选择要执行的分支特定管道。

# 马具上还有什么？

网络研讨会和案例研究

# 给我们你的马具技巧和窍门！

任何对如何使用安全带有独特建议或技巧的顾客都将获得一张价值 100 美元的礼品卡。(免责声明:每人限用一个，必须是一个独特的使用案例，不在我们的任何材料中销售，并适用于所有线束，而不是一家公司的独特使用案例)。将您的故事发送至电子邮件 marketing@harness.io。