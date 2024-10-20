# 如何利用 AWS Lambda 进行开发运维备份和恢复

> 原文：<https://www.fairwinds.com/blog/how-to-leverage-aws-lambda-for-devops-backup-and-recovery>

 丢失数据可能意味着失去业务，那么您如何确保云不会让您崩溃呢？通过密切跟踪哪些信息发生了更改，谁在何时更改了它。这意味着提前识别所有可能发生的故障或灾难情况及其潜在的业务影响。

**这里有三个有用的  [【亚马逊网络服务(AWS)](https://aws.amazon.com/) 工具可以考虑:**

I. [CloudTrail](https://aws.amazon.com/cloudtrail/) 是一个 web 服务，为控制台中和通过 API 发生的所有操作提供帐户审计跟踪。监控和警报可以让您知道是否正在进行意外的更改以及由谁进行的更改。
二世。 [Config](https://aws.amazon.com/config/) 是一项完全托管的服务，具有 AWS 资源清单、配置历史记录和配置更改通知，可实现安全性和监管。
三。 [Lambda](https://aws.amazon.com/lambda/) 可用于运行备份过程，无需配置或管理服务器。您可以直接利用 AWS API 来操作您的云资源，并自动触发来自其他 AWS 服务的事件。

Lambda 目前在 DevOps 领域特别热门，因为它可扩展且可靠，并且不包含大量开销和成本，此外，您可以使用它来定制您的备份计划，以满足您的业务和基础架构需求。Lambda 值得仔细看看。

Lambda 是一个事件驱动的无服务器计算平台，作为 AWS 的一部分提供。在 AWS 全球云计算大会  [AWS re:Invent 2014](https://reinvent.awsevents.com/) 上推出的 Lambda 让构建响应事件的按需应用变得更加容易。您可以编写自己的备份过程，而不必维护任何额外的基础结构。这是一个设置全自动备份系统和运行代码的理想计算平台——只要你知道它支持的语言之一( [Node.js](https://nodejs.org/en/) 、  [Java](https://www.oracle.com/java/index.html) 、  [C#](https://docs.microsoft.com/en-us/dotnet/csharp/) 和  [Python](https://www.python.org/) )。

Lambda 允许您将备份脚本设置为仅在需要时执行。您可以通过将脚本设置为基于时间(每分钟、每 10 分钟或每晚)或操作(例如当有人向 S3 存储桶写入内容时)触发来备份特定数据集。

例如，您可以每天晚上对数据库进行快照，然后触发 Lambda 函数将快照复制到您的备份帐户。如果你一天要写 500，000 张收据到一个 S3 桶，你可以运行你的 Lambda 脚本 500，000 次，并保持一个实时备份。

Lambda 是云原生的，所以你不需要支付一大笔许可费，也不需要投资一个不符合你需求的备份产品。云原生产品支持工具之间的简单通信。如果你正在备份一个 RDS 数据库，你的 Lambda 函数本质上可以直接与它对话，因为它们都生活在亚马逊世界中，说着同样的语言。还可以分配 Lambda 函数来处理安全策略和权限。

## 进入壁垒

迄今为止，Lambda 没有像更传统的解决方案那样被广泛使用。进入的障碍包括不熟悉平台和使用平台所需的努力程度。有了 Lambda，你必须决定你要备份什么，编写代码，确保代码不会失败，并创建处理异常值的应急计划。笨重、昂贵的备份解决方案看起来更高效，因为它们具有交钥匙性质。

## 简而言之 AWS Lambda

尽管如此，Lambda 的战略优势仍然大于挑战:

*   获得一个无需额外基础架构管理的云原生平台。
*   定制 AWS Lambda 备份以满足您的需求，利用 Python 和[Boto 3](https://boto3.readthedocs.io/en/latest/)(Python SDK)编写备份函数。
*   使用 AWS API 功能复制、备份、更改权限和移动资源。
*   获得更高的灵活性、可靠性和耐用性，以及更低的成本和自动可伸缩性(从每天几个请求到每秒几千个请求)。

为了充分利用 AWS Lambda 备份功能，您需要打破常规。选择人迹罕至的道路，你会在前进的道路上获得巨大的收益。