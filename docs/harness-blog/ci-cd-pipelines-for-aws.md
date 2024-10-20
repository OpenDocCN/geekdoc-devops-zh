# 为 AWS 利用 CI/CD:立即在 AWS 市场上找到我们！装具

> 原文：<https://www.harness.io/blog/ci-cd-pipelines-for-aws>

利用 AWS 的 CI/CD:AWS 客户可以跨 AWS 应用程序和环境授权他们的开发人员——我们在 AWS 市场！

今天，我们宣布 Harness 现已成为 [AWS 市场](https://aws.amazon.com/marketplace/pp/B07PZY3369)的一部分，让 AWS 客户能够在其 AWS 应用和环境中通过[持续交付即服务](https://harness.io/products/continuous-delivery/)来增强其开发人员的能力。

超过 50%的 Harness [客户](https://harness.io/customers/)目前在 AWS 中运行他们的应用程序。因此，在过去的两年中，我们对 AWS 服务的支持越来越强，AWS 客户如 [Advanced](https://harness.io/customers/case-studies/empower-engineers-with-cd/) 、 [Linedata](https://harness.io/customers/case-studies/global-continuous-delivery/) 和 [OpenBank](https://harness.io/customers/case-studies/openbank-on-demand-deployments/) (桑坦德的数字银行)在 Harness 方面取得了显著的成果。

这里有一个关于 Harness Continuous Delivery 如何支持您的应用程序在 AWS 中运行的快速总结。

## 全面的 AWS 工件支持

Harness 支持以下应用程序构件/服务:

*   亚马逊机器映像(AMI)
*   EC2 和 Fargate 上的亚马逊弹性容器服务(ECS)
*   亚马逊 Kubernetes 服务(EKS)
*   自动气象站λ
*   AWS 代码部署

Harness 是第一家支持 AWS Lambda 的连续交付供应商，早在 2017 年 11 月。

此外，Harness 还支持 Amazon Elastic Container Registry(ECR)和 Amazon S3，因此客户可以自动检测和控制版本，并将新的工件直接从他们的存储库拉入他们的部署管道。

Harness 还支持将 Tomcat、JBoss 和 IIS 等传统应用程序部署到标准 AWS EC2 实例。

## 对基础架构配置的云形成支持

早在 2018 年 10 月，Harness 就发布了对 AWS CloudFormation 和 HashiCorp Terraform 的支持，因此客户可以在 Harness 部署管道中重用和引用他们的模板。这允许在执行部署管道时动态地供应和停用云基础架构。

这里有一个 2 分钟的视频来解释这种整合:

## 全面的发布策略

Harness 支持以下 AWS 部署的发布/部署策略:

下面是如何使用 AWS ECS 创建自动化的 canary 部署:

## 自动验证和回滚 AWS 部署

除了自动化应用程序部署，Harness 还可以使用机器学习(ML)自动化 AWS 部署的验证和回滚。Harness 为 CloudWatch(和其他监控工具)提供一流的公民支持，因此我们能够分析您的应用和部署环境中的所有指标，然后利用 ML 来检测性能和质量的异常/倒退。回滚可以完全自动化，也可以根据需要进行手动干预。

例如，下面是一个自动化的 ECS canary 部署工作流，它使用 CloudWatch 指标来验证每个 canary 阶段:

## AWS KMS 对机密管理的支持

亚马逊 KMS 是一种在部署管道中加密和解密秘密的流行方式。

Harness 为 KMS 提供了一流的公民支持，因此您可以跨 AWS 应用程序、环境和管道引用机密。

如您所见，Harness 在我们的客户群中广泛而深入地支持 AWS 应用。

我们很想让你亲自试试马具看看。今天就获得的[免费试用。](https://app.harness.io/auth/#/signup/?module=cd)

干杯，
史蒂夫。