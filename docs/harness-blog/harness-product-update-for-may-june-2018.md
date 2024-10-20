# 2018 年 5 月/6 月线束产品更新|线束

> 原文：<https://www.harness.io/blog/harness-product-update-for-may-june-2018>

2018 年 5 月/6 月的产品更新:HashiCorp Terraform、PCF、AWS CloudFormation 支持等。看看我们都干了些什么！

在过去的六个星期里，我们的工程团队在五月和六月一直在努力工作。作为一家持续交付供应商，我们在过去 30 天内完成了大约 1，900 次部署，并在接纳大量新客户的同时保持了我们的速度。

以下是线束团队在 5 月和 6 月交付的产品:

请阅读以下内容，了解 2018 年 5 月/6 月的完整产品更新。

## HashiCorp Terraform 和 AWS 云形成支持

我们最近引入了一个新的概念，称为“基础设施供应器”这基本上允许客户参考和重用其现有的 HashiCorp Terraform 和 AWS CloudFormation 模板，以在部署过程中调配和停用基础架构。

与任何部署步骤一样，Harness 将提供 Terraform 和 CloudFormation 步骤的完整控制台可见性，以便您可以逐行跟踪它们的执行。

## 微软 IIS，。NET 和 Azure 支持

在发布时，我们决定从 Linux、Java、Docker、Kubernetes AWS 和 GCP 支持开始。在过去的六个月里，我们迅速将我们的支持扩展到了其他技术堆栈和云。

不出所料，微软的。NET、IIS 和 Azure 是最受欢迎的平台。我们现在已经添加了 IIS 网站、IIS 应用程序和 IIS 虚拟目录，作为工件支持的一等公民。我们还添加了 Powershell 和 WinRM 支持，因此 Harness 可以在微软生态系统内执行和通信。

## Pivotal Cloud Foundry 支持

客户请求第二多的云平台是 Pivotal Cloud Foundry。该平台在 F500 公司中占有很大的份额，特别是在金融服务领域，因此我们增加该平台支持非常重要。[看博客](https://harness.io/blog/ci-cd-piplines-for-pivotal-cloud-foundry/)。

## 微服务影响分析

Harness 现在能够跨应用环境中的所有微服务执行真正的服务影响验证。一家大型 F500 银行要求该功能，以便他们可以立即了解每个微服务部署的上游和下游影响。现在，当您部署新的微服务容器时，Harness 将识别并验证所有微服务依赖项，以便您了解真正的影响。[看博客](https://harness.io/blog/introducing-harness-service-impact-verification-for-appdynamics/)。

## 微服务流量控制

想要同步和控制许多微服务部署的流程吗？没问题。只需将“障碍”添加到您的部署工作流中，以便所有工作流在继续之前到达相同的执行点。

想要并行部署许多微服务，但只想在所有部署完成后开始验证的客户需要此功能。欲了解更多信息，[阅读博客](https://harness.io/blog/flow-control-for-microservice-deployments/)。

## APM 的线束展开标记

想在 APM 解决方案中将线束部署注册为事件或标志吗？没问题。现在，您可以添加“部署标记”作为部署工作流中的验证步骤。
例如，点击“添加验证”并选择“新遗迹部署标记”接下来，选择新的 Relic 服务器和应用程序名称，以便 Harness 具有正确的上下文。

最后，您可以编辑部署标记的主体。这是将注入到 APM 解决方案中的消息，以便用户接收部署上下文。

上面的部署标记在 New Relic 中将会是这样的:

## 应用层变量/机密

过去，您可以在服务和环境级别定义/设置变量和秘密。现在，您可以为所有部署管道定义/设置全局应用程序变量。我们还在这方面开发更多功能，敬请关注！

## 其他线束增强功能

以下是我们提供的其他特性/功能的一个小列表:

*   Dockerized 委托支持
*   针对管道、部署、环境、触发器、服务和工作流的快速搜索过滤器
*   用于工件的谷歌云存储(GCS)
*   持续验证的自定义指标
*   断开的内部支持
*   添加队友按钮
*   Github webhook 支持触发构建工作流

感谢您阅读 2018 年 5 月/6 月的产品更新。下次请关注我们，了解更多更新！

问候，
史蒂夫。
@BurtonSays