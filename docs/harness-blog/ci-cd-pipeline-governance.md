# 确保 CI CD 渠道管理|管理

> 原文：<https://www.harness.io/blog/ci-cd-pipeline-governance>

借助我们新的管道治理功能，您现在可以测量线束管道符合监管和运营标准的程度。

借助我们新的管道治理功能，您现在可以测量线束管道符合监管和运营标准的程度。

## 什么是管道治理？

法规和操作合规性在软件开发中至关重要。监管标准影响整个 SDLC，包括开发、测试、部署、运营和监控。不缺少旨在帮助保护组织安全和消费者隐私的法规，包括 PCI、HIPAA 和 SOX。借助我们新的[管道治理功能](https://docs.harness.io/article/zhqccv0pff-pipeline-governance)，您现在可以测量线束管道与您的监管和运营标准的合规性。当在 Harness 中触发部署管道时，部署可能会在产品发布之前等待经理批准。在审批过程中，经理需要了解管道是否符合监管标准。我们通过提供在批准发布之前“评分”管道的能力来解决这个问题。

## 什么是“分数”？

管道分数是衡量您的管道符合监管和运营标准的程度。与管道由各种工作流和阶段组成的方式相同，分数由指示合规性的标签组成。每个标签都有一个权重。标签的重量会影响总的百分比分数。例如，假设您有两个权重如下的标签:

然后，Foo 和 Bar 以 50%平均分配，以贡献总的 100%分数。然而，让我们引入权重为 2 的第三个标签:

该分布计算 Foo 占 25%，Bar 占 25%，Hop 占 50%。所以，如果 Foo 在符合性检查中缺失，那么你的分数是 75%(因为 Foo 占分数的 25%)。但是少了一跳，你的分数是 50%。

## 如何为管道评分

在管道的每个阶段以及相关的工作流程中，您都有机会应用标记。每个标签可以代表一个符合性标准。例如:

我们在这里深入探讨我们的[标签管理](https://harness.io/blog/introducing-tag-management/)功能。

根据您的要求，您可以为您的工作流添加任何必要的合规性标准。

## 创建治理标准

导航到*持续安全*，然后是*治理*。点击*+添加治理标准*。点击*添加规则*，然后继续添加您的规则。在这个例子中，我将添加三个标记:PCI、HIPAA 和 SOX。确保添加每个标签作为它自己的规则，而不是所有三个标签都在同一个规则下。这样，我们可以称量每个标签。所以，我给 SOX 的权重是 2，其余的权重是 1。点击*高级设置*，然后将您的治理标准与您想要治理的应用程序相关联。

## 衡量治理标准

导航回您的管道，您将在管道配置屏幕的底部找到您的治理分数(*设置>【您的应用】>管道*)。您将看到您正在监控哪些标签，它们对分数的权重影响，以及您的一致性的总体百分比分数！

您可以通过[访问文档](https://docs.harness.io/article/zhqccv0pff-pipeline-governance)了解更多关于此功能的信息。

## 更多最新更新

*   [Elasticsearch](https://www.elastic.co/elastic-stack) 的最新主要版本现已支持[工作流验证](https://docs.harness.io/article/ina58fap5y-what-is-cv)和 [24/7 服务保护](https://docs.harness.io/article/dajt54pyxd-24-7-service-guard-overview)。[阅读更多](https://docs.harness.io/article/qdajtgsqfj-elasticsearch-verification-overview)。
*   在工作流验证期间或由 24/7 服务卫士检测到的异常现在可以通过吉拉问题轻松跟踪。[阅读更多](https://docs.harness.io/article/v4d4pd5lxi-jira-cv-ti)。
*   线束验证事件现在可以分配优先级(P0…P5)或标记为无风险，以提高工作流验证和 24/7 服务保护的准确性。多读书。[阅读更多](https://docs.harness.io/article/q1m740uwca-harness-verification-feedback-overview)。
*   现在可以有条件地禁用或执行管道阶段，因此您可以:
*   暂时关闭管道阶段以对其进行编辑或故障排除。
*   根据用户输入或运行管道时确定的变量值，有条件地运行工作流。[阅读更多](https://docs.harness.io/article/6kefu7s7ne-skip-conditions)。

## 给我们你的马具技巧和窍门！

任何对如何使用安全带有独特建议或技巧的顾客都将获得一张价值 25 美元的礼品卡。(免责声明:每人限用一个，并且必须是不在我们的任何材料中销售的独特使用案例。将您的故事发送至电子邮件 marketing@harness.io。