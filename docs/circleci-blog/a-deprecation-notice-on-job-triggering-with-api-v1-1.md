# 弃用通知

> 原文：<https://circleci.com/blog/a-deprecation-notice-on-job-triggering-with-api-v1-1/>

今天，我们将针对单个终端发布一份特别的弃用通知。在 2020 年 3 月 1 日或之后，我们将终止使用 1.1 版 API 触发作业的功能(在 API 中，这是通过对特定项目的 POST 请求来完成的)。

总之，只有当您在自己的脚本和程序中使用 CircleCI API v1 或 v1.1 来触发项目的构建(又名“作业”)时，才需要采取行动——如果是这样，我们会要求您使用一个新的端点，这可能需要您更改代码，在某些情况下，还需要对您的`.circleci/config.yml`进行一些调整。作为替代，我们敦促所有需要使用我们的 API 在 CircleCI 上触发运行的人使用 API v2 中新的触发管道端点。要获得迁移到新 API 端点触发的帮助或给我们关于此变更的反馈，请访问我们的[讨论页面](https://discuss.circleci.com/t/feedback-wanted-moving-from-v1-1-job-triggering-to-v2-pipeline-triggering/33494)。

v1.1 API 的其余部分很可能会在未来发布弃用声明，但在 [v2 API](https://circleci.com/docs/api/v2/#circleci-api) 拥有有效的特性对等性之前，我们不会发布该声明。一旦该公告发布，您将有至少六个月的时间迁移到 API v2。

要了解我们的 v2 API，请阅读[公告帖子](https://circleci.com/blog/introducing-circleci-api-v2/)。