# 使用 CircleCI webhooks | CircleCI 创建可定制的体验

> 原文：<https://circleci.com/blog/create-customizable-experiences-with-circleci-webhooks/>

在过去的 10 年里，CircleCI 的客户一直使用我们的平台来定制他们的软件开发流程。orb 通过可重用的配置包帮助标准化和扩展 CI/CD 管道。 [CircleCI API](https://circleci.com/docs/api/v2/) 允许用户为他们的开发人员创建健壮的内部工具，并与其他产品集成以进行更精细的监控。

时至今日，CircleCI 用户还有另一种方式来对事件做出反应，并使用 webhooks 定制他们的软件交付体验。

以前，如果开发人员想要接收关于 CircleCI 中特定事件的信息，他们需要完全依赖 API。现在，使用 webhooks，客户可以扩大他们对 CircleCI 内发生的事件做出反应的范围。通过这些事件，开发人员可以创建有效且有目的的可定制体验。

## CircleCI 合作伙伴如何使用 webhooks

CircleCI 的合作伙伴 Datadog 和 Sumo Logic 已经加入了通过 webhooks 扩展 CircleCI 的努力。

Datadog 在他们的 [CircleCI 集成](https://docs.datadoghq.com/continuous_integration/setup_pipelines/circleci/)中使用 webhooks 将分析数据呈现给用户，使他们能够在管道中发生事件时做出明智的决策。

“作为一个长期的合作伙伴，我们对 CircleCI webhooks 的发布感到兴奋，”Datadog 的产品总监 Borja Burgos 说。“最大限度地提高开发人员的时间和工作效率从未如此重要。多亏了 CircleCI webhooks，我们能够在 Datadog 中构建新的集成，以提供对 CI 的卓越可见性和洞察力。这种整合将为我们的客户提供巨大的价值，并加强我们与 CircleCI 社区的联系。”

通过 webhooks 的 CircleCI 和 Sumo Logic 集成从 CircleCI 收集事件，包括工作流和作业完成状态，允许团队更好地跟踪持续集成和部署管道的性能和健康状况。

Sumo Logic 业务开发总监 Drew Horn 表示:“收集、丰富和关联现代 DevOps 工具链中不同来源的数据是当今工程团队面临的最大挑战之一。“借助 CircleCI webhooks，开发人员现在只需点击几下鼠标，即可将详细的自动化管道数据推送至 Sumo Logic 的[持续智能平台](https://www.sumologic.com/continuous-intelligence-platform/)，通过对软件开发生命周期的深入洞察和实时分析，对其软件交付性能进行基准测试和优化。”

CircleCI webhooks 将为合作伙伴集成提供更多机会，并扩展对技术用例的支持。请继续关注其他可用的集成。

## circle CI web hooks:CI/CD 管道更加灵活

Webhooks 可用于在作业失败时向内部通知系统发出警报，或者使用 Airtable 等工具连接 CircleCI，以汇总已完成工作流或作业的数据。有了 webhooks，客户能够在他们的软件开发管道中创造更多的灵活性。

要创建您的第一个 webhook，请导航到 CircleCI UI，访问现有项目中的项目设置，并选择左侧菜单栏中的“Webhooks”项。阅读我们的文档了解更多关于如何创建网页挂钩的信息。

## 下一步是什么？

这仅仅是个开始——如果你有兴趣从 webhooks 看到更多，请访问 [Canny](https://circleci.canny.io/webhooks) 浏览或提交新想法。前往我们的社区论坛，[讨论](https://discuss.circleci.com/)，让我们知道你是如何使用 webhooks 的，访问[文档](https://circleci.com/docs/webhooks/#overview)获取更多关于实现 webhooks 的信息，查看[本教程](https://circleci.com/blog/using-circleci-webhooks/)学习如何设置你的第一个 web hooks。