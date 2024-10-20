# 线束产品更新| 2021 年 5 月|线束

> 原文：<https://www.harness.io/blog/harness-product-update-may-2021>

Harness 在 5 月份一直忙于创建新功能、新内容、举办活动以及构建 CI/CD 的未来。

五月给我们脊甲师带来了不少成果。我们从非 Git 来源发布了我们的 Terragrunt 集成和远程清单。我们还发布了一本关于 CI 现代化的电子书。最重要的是:我们不知疲倦地工作(真的不能对每个参与者说足够多的好话)为我们的{无脚本}会议做准备，由 CD 教父戴夫·法利做主题演讲。如果你还没有[注册](https://web.cvent.com/event/0b933007-0102-43f0-b17c-25c92983ae39/regProcessStep1)，你现在应该去注册了。继续吧。我们会等的。

## 在 ShipTalk 播客上谈论船

第 12 集:[O11Y 是什么？Splunk 的 Chris Riley 揭开了可观察性的神秘面纱](https://harness.io/blog/podcast/o11y-observability-demystified/)。Chris 拥有多年的专业服务和律师经验。克里斯是{unscripted} 2021 年可观测性小组的成员，他向我们介绍了 O11Y 的背景，也就是可观测性。

想成为 ShipTalk 的演讲嘉宾吗？我们希望收到您的来信。完成[您的演讲者提交](https://harness.io/shiptalk-podcast-call-for-speakers)，我们会联系您，看看您是否适合未来的播客！

## 新功能

### 线束 CD - Terragrunt 支架

Terragrunt 是一个围绕 Terraform 的包装工具。它允许用户在他们的 Terraform 代码中实践 DRY(不要重复自己)原则。使用 Terragrunt，用户只需定义一次他们的 Terraform 代码，无需为多种环境重复定义。Terraform 代码在各种环境中通常都是相同的，只是在输入、输出和状态文件中存在潜在差异。管理状态文件、模块、输入变量和输出变量是 Terraform 用户的维护工作。开发人员被 Terragrunt 所吸引，因为它很容易维护组件，同时还能帮助他们一次性定义代码。

Harness 允许用户在产品中配置 Terragrunt。这种体验类似于在 Harness 中配置 Terraform 或 CloudFormation。

点击了解更多关于我们新的 Terragrunt 集成[的信息。](https://harness.io/blog/product-updates/native-terragrunt-support/)

### 线束 CD -定制仪表板增强功能

Harness 增强了定制仪表板中包含的部署和服务小部件。用户现在可以按环境类型对部署进行筛选和分组，以比较非生产部署和生产部署。用户还可以使用服务小部件来显示正在部署什么类型的服务，并查看部署管道正在使用什么部署策略。欲了解更多信息，请查看我们的[文档](https://docs.harness.io/article/rxlbhvwe6q-custom-dashboards)。

## 报刊上值得注意的提及

## 即将举行的虚拟活动

## 即将举行的网络研讨会

## 展望未来

除了我们承诺在 6 月的[{无脚本}会议上向您发布的巨大公告，还有一些好东西即将为您呈现。在接下来的几周/几个月里，你会遇到以下情况:](https://web.cvent.com/event/0b933007-0102-43f0-b17c-25c92983ae39/summary)

*   使用 Webhook 触发器支持秘密——Harness 将能够使用秘密来认证传入的 GitHub webhooks，以确保 web hook 来自可信的来源。
*   粒度访问控制工具将添加回滚工作流的新权限。这允许用户在部署失败时回滚，即使他们没有执行工作流的权限。