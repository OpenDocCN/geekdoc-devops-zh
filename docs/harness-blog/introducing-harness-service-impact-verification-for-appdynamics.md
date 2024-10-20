# AppDynamics | Harness 的线束服务影响验证简介

> 原文：<https://www.harness.io/blog/introducing-harness-service-impact-verification-for-appdynamics>

Harness 将识别和验证所有微服务依赖关系，以便您了解真正的影响。

上周，AppDynamics [宣布与 Harness 建立新的合作关系](https://www.appdynamics.com/blog/news/appdynamics-partners-with-harness-to-help-customers-embrace-continuous-delivery/)，以帮助客户拥抱持续交付，并了解每个应用部署的业务影响。

我们当时的集成侧重于特定应用程序工件(容器、AMI、WAR、函数、舵图等)的部署和验证。例如，您将一个新的微服务容器部署到您的生产环境中，Harness 将使用 AppDynamics 数据来告诉您该微服务在部署后的性能是更好还是更差。

昨天，我们进一步增强了我们的 AppDynamics 集成:我们现在能够跨您应用环境中的所有微服务执行真正的服务影响验证。一家大型 F500 银行要求该功能，以便他们可以立即了解每个微服务部署的上游和下游影响。现在，当您部署新的微服务容器时，Harness 将识别并验证所有微服务依赖项，以便您了解真正的影响。

## 一个简单的微服务应用(从来没人说过)

假设我们正在使用 AppDynamics 监控一个简单的微服务环境:

上面的应用程序有 4 个微服务:库存、结账、支付和记录处理。
现在，假设我们使用 Harness 和 AppDynamics 部署并验证了新版本的支付微服务:

使用我们最初的 AppDynamics 集成，我们的验证流程应该是这样的:

您可以清楚地看到，在部署新版本后，我们的部署导致了支付服务的性能下降。线束验证和影响分析的范围被限制在新工件被部署的地方。

## 利用应用性能监控(APM)服务地图

像 AppDynamics 这样的 APM 解决方案的一个好处是它们能够自动映射应用服务依赖关系。他们通过跨各种应用服务和组件(也称为层)跟踪用户请求(也称为业务事务)来做到这一点。所有这些数据都被存储和建模为应用程序和事务流程图。

使用 REST，Harness 可以查询这些地图，以了解所有上游和下游服务的依赖关系。这个功能非常有用，因为它允许 Harness 验证与特定微服务部署相关的所有微服务依赖关系。

借助我们增强的集成，线束验证和影响分析现在看起来像这样:

利用[持续验证](https://harness.io/platform/continuous-delivery/continuous-verification/)现在发现所有与支付微服务相关的微服务依赖(通过查询 AppDynamics 服务模型)。这允许我们的无监督机器学习通过分析受影响微服务的所有时序指标来观察和评估上游和下游发生的事情。

在上面的例子中，我们可以看到支付性能回归对 checkout 微服务有上游影响，但对记录处理服务没有下游影响。

客户现在可以看到全局，并全面了解受微服务部署影响的一切。更好的是，当这些 AppDynamics 验证失败时，Harness 可以自动回滚这些部署，从而避免停机和糟糕的最终用户体验。

[注册免费试用](https://harness.io/try-continuous-delivery-as-a-service-for-free/)Harness，亲自测试这项新功能！

问候，
史蒂夫。@ BurtonSays 说