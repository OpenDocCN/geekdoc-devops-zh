# 部署 AWS Lambda 无服务器功能|线束

> 原文：<https://www.harness.io/blog/deploying-aws-lambda-serverless-functions>

无服务器计算是一种云计算执行模型，其中云提供商动态分配服务实例消耗的机器资源。**零扩展**是一个重要的无服务器计算概念。它描述了无服务器运行时如何自动扩展以处理增加的负载，并在运行时不使用时扩展到零。

无服务器功能是单一用途的编程功能，允许组织优化软件开发和可维护性。无服务器服务或功能即服务(FaaS)体现了这些概念。AWS Lambda 是亚马逊网络服务的无服务器计算平台。它支持每个项目的无限功能，并允许每个地区每个帐户 1000 次执行。AWS 是第一家提供完整的无服务器计算平台的云提供商。Lambda 拥有对 JavaScript、Python、Java 和 C#的原生支持。此外，您可以利用包装器来执行用 Go、PHP 或 Ruby 编写的代码。

如果你想学习更多关于开发 Lambda 函数的知识，你可以在这里学习。这篇博文假设您已经准备好使用 Harness 平台部署 Lambda 函数。下面分享一下如何做一个简单的 AWS Lambda 部署。

## 我需要部署什么？

将 AWS Lambda 函数部署到 Harness 相当简单。您需要确保您有一个可以连接到 Harness 的 AWS 帐户。

在开始之前，请确保您已经在 AWS VPC 中安装了一个 Harness 委托。您将需要这个委托来连接到您的 S3 桶(您的 S3 桶充当工件源)和 Lambda 函数。此委托可能是安装在 EC2 实例上的 Shell 脚本委托，也可能是安装在 ECS 群集上的 ECS 委托。如果您在一个 [AWS EC2](https://developer.harness.io/docs/first-gen/firstgen-platform/techref-category/api/use-cloud-providers-api/#aws) 实例上安装代理，请确保您提供了 AmazonEC2FullAccess 策略。否则，对于现有的 [ECS 集群](https://developer.harness.io/docs/first-gen/firstgen-platform/account/manage-connectors/add-amazon-web-services-cloud-provider#review-use-kubernetes-cluster-cloud-provider-for-eks)，您将需要提供 amazonec 2 containerserviceforec 2 role 策略。

对于使用 Lambda 执行操作的委托，您将需要一个具有以下策略的 IAM 角色:

*   AWSLambdaFullAccess(arn:AWS:iam::AWS:policy/AWSLambdaFullAccess)
*   **awslabdarole**(arn:AWS:iam::AWS:政策/服务角色/awslabdarole)

在这个例子中，我们将使用一个委托来连接 S3 桶和 lambda 函数。将 **AmazonS3ReadOnlyAccess** 策略附加到您的代表。该代表将需要一个 S3 策略来访问您设置的 S3 时段。

我强烈建议转到这个 Harness 委托，并向选择器部分添加一个标记。这允许您在创建 AWS Cloud Provider 时引用这个特定的委托。

您需要确保的最后一件事是，您已经设置了一个 AWS Cloud Provider，它具有由安装的 Harness Delegate 使用的 IAM 角色。[点击此处](https://developer.harness.io/docs/continuous-delivery/onboard-cd/cd-quickstarts/serverless-lambda-cd-quickstart/)查看我们关于将您的 AWS 实例添加为线束云提供商的文档。

有了这一节，您就可以创建 Lambda 部署了。

## 利用你的 AWS Lambda 函数

下面是部署 AWS Lambda 函数的步骤。在 Harness 中，为您的 Lambda 部署管道创建一个新的或使用现有的 Harness **应用程序**。我们将使用这个应用程序来定义关于我们的部署的必要信息。填写以下申请详细信息:

Within our Application we'll be adding a service, a target environment and a workflow.

*   服务:您需要定义一个名称和工件类型。使用以下步骤编辑这个新创建的服务:
*   点击+ **添加工件源**
*   通过点击 **Lambda 功能规范**修改部署规范。在这里，您将修改运行时间、内存大小和执行超时的默认值
*   **通过提供函数名、处理程序和其他细节来添加函数**。
*   点击提交确认这些关于你的 Lambda 的细节。
*   环境:
*   点击**+添加环境**，提供必填字段并点击提交
*   点击新创建的环境，并选择**+添加基础设施定义**选项。
*   提供所需的信息，并使用 AWS Lambda 作为部署类型。
*   点击提交确认基础设施定义。
*   工作流程:
*   点击**+添加工作流**
*   输入定义基本部署所需的信息。您将使用您在本步骤前面设置的环境、服务和基础设施定义。
*   点击提交保存您的工作流程
*   单击您新创建的工作流。
*   或者，您可以决定在您的工作流程中添加额外的步骤(例如:验证)。
*   当您准备好开始部署时，在此屏幕上点击 Deploy。

As noted in the instructions, we can add additional workflow steps.

最后，看你的 Lambda 部署！

这里有一个视频，可以跟随上面分享的步骤。

部署完成后，您会看到成功或失败的状态。这就是如何定义使用 Harness 进行 AWS Lambda 部署的必要信息。

或者，您也可以选择定义以下应用详细资料:

*   利用您的工作流程的管道，以及
*   使用管道自动部署应用程序服务的触发器。

我还建议使用 AWS CloudWatch 来验证您的部署并监控您的生产服务。你可以在这里学习如何做。

我希望这篇博文能帮助你开始使用 Harness 部署 Lambda。如果你想免费试用 Harness，只需[点击此处](https://harness.io/)注册即可。