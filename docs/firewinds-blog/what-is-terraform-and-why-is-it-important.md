# 什么是 Terraform，为什么它很重要

> 原文：<https://www.fairwinds.com/blog/what-is-terraform-and-why-is-it-important>

 运行任何规模的基础设施几乎总是会带来令人眼花缭乱的组件和配置。更复杂的是，一个组织中的不同团队可能需要相似的基础设施，只是略有不同。该基础架构可能分布在多个拓扑结构上，从内部部署到一个或多个云供应商。

虽然可以使用基础设施提供商的用户或命令行界面一次启动和配置一个组件，但最终结果通常很难以简单明了的方式组织和维护。

在 Fairwinds，我们为客户构建可扩展、安全可靠的 Kubernetes 基础设施。除了使用 Kubernetes 运行应用程序，我们的工作还包括配置网络、计算资源、存储以及监控等支持组件。为了确保为我们所有的客户维护构建基础设施的最佳实践，我们为我们的客户使用通用模式，在需要的地方提供一致性和定制。

Terraform 是我们管理基础设施整个生命周期的首选工具，使用[基础设施作为代码](/blog/why-infrastructure-as-code-kubernetes)。这意味着在配置文件中声明基础设施组件，然后 Terraform 使用这些组件在各种云提供商中提供、调整和拆除基础设施。

例如，假设您正在 AWS 中工作，并且想要旋转几个特定类型的 EC2 实例。您将在配置文件中定义实例的类型和数量，Terraform 将使用它与 AWS API 通信来创建这些实例。然后可以使用同一个文件来调整配置，例如增加或减少实例的数量。

因为基础架构通常包括比计算实例多得多的组件，所以我们使用称为模块的 Terraform 功能将多个基础架构组件组合成大型、可重用和可共享的块。

在由四部分组成的博客系列中，我们将展示如何使用 Terraform 模块在三个云提供商中轻松建立一个基本的 VPC 和 Kubernetes 集群: [AWS](/blog/terraform-and-eks-a-step-by-step-guide-to-deploying-your-first-cluster) 、 [GCP](/blog/how-to-use-terraform-with-gke-a-step-by-step-guide-to-deploying-your-first-cluster) 和 [AKS](/blog/getting-started-with-terraform-and-aks-a-step-by-step-guide-to-deploying-your-first-cluster) 。

## **Terraform 核心元素**

在这篇文章的剩余部分，我们将深入了解 Terraform 的一些核心元素。如果你从未使用过 Terraform，并且正在寻找更多的实践经验，Hashicorp 网站上的[教程](https://learn.hashicorp.com/terraform)会很有用。

让我们来谈谈平台提供商。Terraform 服务的云提供商超过 100 家。在 Fairwinds，我们将 Terraform 用于三种 AWS-、[、【GCP】、](https://www.terraform.io/docs/providers/google/index.html) [Azure](https://www.terraform.io/docs/providers/azurerm/index.html) 。提供者支持与特定 API 的接口，并公开您定义的任何资源。HCL 或 HashiCorp 语言是用于定义资源的通用语言，不管使用什么提供者。

在任何使用 Terraform 的项目中，需要定义的第一个资源是提供者，因为这是您访问 API 的途径，您将与 API 交互以创建资源。一旦配置和验证了提供者，现在就可以创建大量的资源了。每个云提供商都有自己的一套资源；几个例子是一个`aws_vpc,``google_dns_record_set,`和一个 `azuread_application.` 资源，根据 Terraform，是“Terraform 语言中最重要的元素”。这是您描述要创建的基础设施的地方；这可以从计算实例到定义特定权限等等。

在大多数情况下，您将需要比 Terraform 文档中显示的基本示例更多的配置，用于 AWS 的 [terraform vpc 资源详细提供了必需和可选的参数参考；以及您可以访问该资源中的哪些属性。通过使用这些文档，你可以利用 Terraform 获得许多资源，这些资源可能会满足你的所有需求。](https://www.terraform.io/docs/providers/aws/r/vpc.html)

既然我们已经概述了 Terraform 如何与各种提供者交互来创建和管理资源，那么让我们来谈谈维护和组织项目的最佳实践。

## 模块:打包和重用代码

Terraform 提供了一种很好的方式来以模块的形式封装和重用公共代码。Terraform 模块相当于脚本或编程语言中的函数或方法。它们通过提供输入和返回输出来提供创建资源的标准接口。模块还通过增加可读性和允许团队在逻辑块中组织基础设施来简化项目。模块可以很容易地共享，并提供给任何 Terraform 项目。

模块通常用作创建和管理多种资源的简单界面。这大大减少了项目中重复代码的数量。复制和粘贴代码片段，同时只更改几个参数，这可能会很乏味。例如，如果任务是为不同的环境创建多个 VPC，那么可以多次调用单个 VPC 模块，而不是为一个完整运行的 VPC 创建每个必要的资源。

输入变量用于定制模块的行为，以及潜在的资源命名方式。有些模块可以非常灵活地决定如何以及是否创建资源。例如，我们的[谷歌云 VPC](https://github.com/FairwindsOps/terraform-gcp-vpc-native/tree/master/default) 模块可以根据声明的输入改变行为。如果用户设置了 `enable_cloud_nat = true` ，该模块将创建额外的云 NAT 资源。查看官方文档了解更多关于[输入变量](https://www.terraform.io/docs/configuration/variables.html)的信息。

与函数类似，Terraform 模块也可以返回输出。这个输出可以用作另一个模块或资源的输入。

## **状态管理**

状态管理是任何长期地形项目的关键组成部分。Terraform 状态文件跟踪环境中的所有变化。状态文件也可以作为数据源，由其他 Terraform 项目导入。默认情况下，状态文件存储在文件系统中。然而，保持状态文件的安全、可靠和备份是很重要的——这通常意味着将它保存在高度可用的对象存储中。通过利用这种远程存储，团队可以安全地共享和交互始终最新的单一状态。如需更多信息，请查看远程状态的 Terraform 文档，此处为。

## **如何使用 Terraform 构建 Kubernetes 基础设施**

在接下来的博客中，我们将深入探讨这些话题，展示如何使用 Terraform 在不同的云提供商中构建 Kubernetes 基础设施。查看我们针对主要云提供商的入门指南:

## 资源

[![Download Kubernetes Best Practices Whitepaper](img/c38df324d5163c7ccc9c6b998a78ad26.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/cb39a009-a458-4282-9211-41e010cb3376)