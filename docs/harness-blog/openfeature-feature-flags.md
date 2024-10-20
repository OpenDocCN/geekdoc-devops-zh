# 为特征标志|线束创建开放标准

> 原文：<https://www.harness.io/blog/openfeature-feature-flags>

Harness 很高兴地宣布我们加入了 OpenFeature，这是一个新的开源项目，为功能标志管理建立了一个标准。

Harness 很高兴地宣布我们参与了 [OpenFeature](https://openfeature.dev/) ，这是一个新的开源项目，为功能标志管理建立了一个标准。OpenFeature 的创建是为了支持使用云原生技术的强大功能标记生态系统，它最近被[云原生计算基金会](https://www.cncf.io/) (CNCF)接受为沙盒项目。

我们认为 OpenFeature 的创建进一步证明了工程行业认识到特性标志可以提供的许多好处，包括 A/B 测试、生产中的测试，或者只是控制新特性向生产的发布。由于功能标志越来越受欢迎，今天许多企业正在决定是建立自己的功能标志解决方案还是购买一个。企业也可能担心供应商锁定，因为当团队(尤其是企业)寻求采用特性标志工具时，定制的、定制的架构是常见的。OpenFeature 旨在通过开放的功能标记标准来应对这些挑战。

作为一个新的和即将到来的标准，OpenFeature 得到现有供应商的支持是很重要的。在 [Harness](https://harness.io/products/feature-flags) 上，我们很高兴成为这个开放标准的贡献者之一。

## 什么是 OpenFeature？

OpenFeature 为[特征标志](https://harness.io/blog/what-are-feature-flags)的厂商中立实现提供了一个[规范](https://github.com/open-feature/spec/tree/main/specification)。正如其网站上所说，“OpenFeature 将提供统一的 API 和 SDK，以及开发者优先的云原生实现，并具有开源和商业产品的可扩展性。”

OpenFeature 提供了一个共享的标准化功能标记客户端(SDK ),它可以插入到各种第三方功能标记提供程序中。无论您使用的是开源系统还是商业产品，无论是自托管还是云托管，OpenFeature 都为开发人员提供了一致、统一的 API，以便在他们的应用程序中使用功能标记。

这意味着采用 OpenFeature 的组织可以确信他们不会被某个特定的供应商所束缚。此外，那些刚开始使用特性标志的人可以采用基于环境变量的非常基本的实现(也称为配置管理)，然后随着需求的成熟，迁移到提供更多控制和特性的供应商。

## 如何实现 OpenFeature

OpenFeature SDKs 目前正在许多最常见的语言中实现。在技术层面上，SDK 提供了一个评估 API，消费者可以使用它来获取特性标志的值。该 API 的核心是评估不同标志类型的调用，例如:

*   getBooleanValue
*   getStringValue
*   获取数字值
*   getObjectValue

然后，供应商可以实现包装他们自己的 SDK 的提供者 API。OpenFeature SDKs 可以加载和使用这些提供者，通过评估 API 调用它们。

下图给出了一个例子，其中应用程序代码调用 OpenFeature SDK 上的 getBooleanValue()。OpenFeature SDK 然后调用提供者 API。根据加载的提供者，将调用特定于供应商的 SDK 代码。

## 为什么 Harness 参与 OpenFeature

Harness 致力于构建一个开放的生态系统来支持和改进整个软件交付生命周期。2021 年，我们开始开源我们自己的产品，包括我们的[功能标志](https://harness.io/products/feature-flags)SDK。通过对 OpenFeature 做出贡献，我们将继续支持开源项目，这些项目将使我们的客户受益，并通过改善开发人员的工作方式推动行业向前发展。

当我们与软件工程和 DevOps 领导者交谈时，我们经常发现他们已经构建了自己的解决方案，但是他们已经发展到维护和支持太耗时的地步。因此，大规模增加新功能几乎是不可能的。当他们寻求采用更好的商业工具时，他们担心会被单一的供应商所束缚，并且缺乏简单的移植或可移植性。

使用 OpenFeature，如果一个团队或公司想要通过自己构建一些东西来开始他们的功能标志之旅，他们可以从构建一个符合他们需求并符合开放标准的简单解决方案开始。当他们的需求变得更高级时，他们可以很容易地采用支持 OpenFeature 的供应商解决方案，比如 Harness Feature Flags。

在 Harness，我们相信让我们的客户挑选对他们最有意义的模块。OpenFeature 标准正符合这一精神，因为它确保当客户的功能标记需求超出基本需求时，我们可以提供所需的额外功能，而不必改变他们的应用程序与 Harness 功能标记的集成方式。功能标记的底层实现在整个行业中标准化得越多，我们就越能专注于为功能标记的概念增加创新和新价值。它变得越来越少涉及创建跨供应商的多种标准，而更多地关注简化特性标志的采用，并创新它们如何在基础之上增加价值。

## 我们能帮上什么忙？

Harness 致力于支持 OpenFeature。最初，我们已经为 Golang SDK 做出了贡献，并且我们计划为我们的每个特性标志 SDK 构建开放特性提供者。我们还在推广[功能标志最佳实践](https://harness.io/blog/feature-flags-best-practices)，以帮助用户充分利用他们的功能标志流程。

我们很高兴能够帮助推动这项计划，并与 Feature Flag 社区合作，为 OpenFeature 做出贡献，作为我们行业的最佳前进道路。

如果您想了解利用特性标志如何帮助软件工程和 DevOps 团队在您的软件交付中实现控制，您可以[免费注册](https://app.harness.io/auth/#/signup/?module=ff)并从今天开始使用[利用特性标志](https://harness.io/products/feature-flags)。