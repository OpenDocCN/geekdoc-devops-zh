# 最佳性能计划:为什么 CircleCI 改变我们的定价模式

> 原文：<https://circleci.com/blog/plans-for-optimal-performance-why-circleci-is-changing-our-pricing-model/>

当 CircleCI 在 2011 年成立时，[持续集成](https://circleci.com/continuous-integration/)和持续部署(CI/CD)对一些人来说是白日梦，对几乎所有人来说都不是惯例。当时我们所有的客户都在构建我们的 1.0 系统，我们引入了一个基于容器的定价计划，客户将为他们想要访问的每个容器支付月费。在过去的七年里，我们一直在提供同样的计划。今天，我们介绍一个更好的选择。

## 逐步淘汰集装箱

当我们开始的时候，容器模型很容易计划和考虑。七年前，团队使用 CircleCI 的简单性，容器模型反映了这一点。小团队？拿四个容器。大型团队？拿 40。这是最简单不过的了。

### 容器模型限制了优化

在过去的几年里，我们一直在 CircleCI 上发布一些功能来帮助客户优化他们的管道:[工作流来编排作业](https://circleci.com/blog/wide-world-of-workflows-job-orchestration/)，[上下文来管理跨项目的秘密](https://circleci.com/blog/protect-secrets-with-restricted-contexts/)，以及 [orbs](https://circleci.com/blog/announcing-orbs-technology-partner-program/) 来使配置可重用和集成更容易。有一点保持不变:**容器**。团队总是受到他们购买的容器数量的限制，许多团队要么资源闲置，要么开发人员排队等待开放的机器。

## 一个更好的选择:基于使用的循环定价

今天，我们将推出一种新的基于使用的定价模式。我们的[新付费计划](https://circleci.com/pricing/)的客户将能够同时运行他们想要的任意多的资源，而不是被限制在一定数量的容器中。

### 它是如何工作的

CircleCI 的绩效计划有每个座位的基本费用(前 3 个用户 15 美元，然后每个额外用户 15 美元)，然后根据信用包扩展使用。然后，客户在 CircleCI compute 上花费信用来运行他们的工作。CircleCI 提供多种类型的计算(Linux、Windows、macOS、Docker 等。)，并且每种类型的计算每分钟花费给定数量的信用。例如，中型 Docker 计算资源(Docker 的默认值)每分钟花费 10 个信用点(0.006 美元)，因此在中型 Docker 资源上运行 3 分钟的作业花费 30 个信用点(0.018 美元)。

根据您使用的计算机类型和您使用计算机的分钟数，您消耗的点数只有*个*;同时运行多少个作业并不重要。这意味着，如果您的管道中有 10 个可以独立运行的作业，它们将同时运行。

### 主要优势

与基于容器的计划相比，基于使用的计划有许多优势:

1.  **消除排队**
    使用任何付费使用计划(性能或定制)的团队几乎不会经历作业排队，因为他们不受一定数量容器的限制。CircleCI 会随着需求的增加而增加团队可用资源的容量，这样开发人员就不会在高峰时段排队。

2.  **同一计划中的 Linux、Windows 和 macOS**
    我们付费使用计划中的客户可以在同一计划中使用 Linux、Windows 和 MAC OS 计算。(免费层的客户可以访问 Linux 和 Windows)

3.  **优化计算能力**
    基于使用的定价使 CircleCI 能够提供大量不同规格的不同计算类型。性能计划中的客户可以在其配置文件中指定资源类，以更改单个作业的计算资源能力。在小型 Docker 资源(1 个 CPU、2GB RAM)、大型 macOS 资源(8 个 CPU、16GB RAM)、中型 Linux GPU 资源(32 个 CPU、244GiB RAM、2 个 GPU)或两者之间的任何资源上运行作业。

4.  **按需付费**
    除了为满足高峰需求而支付 100 个集装箱的费用，性能计划中的客户还可以在高峰时段扩展到同时运行 100 个作业，在无人推送代码时则减少到零。团队只为他们使用的东西付费。

## 这对我意味着什么？

基于容器的计划将不再可供购买。如果您想要升级您的 CircleCI 计划，您可以从您组织的帐户设置页面将您的免费帐户升级到绩效计划，或者您可以与我们的销售团队讨论定制计划。

### 如果你已经在 CircleCI 支付了集装箱的费用…

你目前的计划将保持不变。您仍将有权访问所有付费的容器，并且管理员仍将能够在组织的计划设置页面中添加和移除容器。**但是**，如果您降级为免费帐户并希望在以后再次升级，您将只能注册绩效计划或定制计划。此外，容器计划中的客户将无法访问为使用计划保留的功能，如高级资源类、Windows 构建和 Docker 层缓存。

### 如果你目前免费使用 CircleCI，或者你是一个新用户…

CircleCI 的免费层现在每周将为每个组织提供 2500 个免费信用点，用于私有存储库(见下文关于开源存储库的信息)。自由计划的组织将继续使用我们的[中型 Docker 计算](https://circleci.com/docs/configuration-reference/#docker-executor)，但现在**也可以使用我们的[小型 Linux](https://circleci.com/docs/configuration-reference/#machine-executor-linux) 、[中型 Windows](https://circleci.com/docs/configuration-reference/#windows-executor) 和[中型 macOS 计算](https://circleci.com/docs/configuration-reference/#macos-executor)(即将推出)**。

### 如果你在 OSS 项目中使用 CircleCI

CircleCI 将向我们的免费计划中的组织每月提供 400，000 信用点，用于开源存储库的中型 Docker 计算，但它们只能用于 Linux 计算。构建 OSS Windows 项目的组织仍然可以使用每周 2500 的免费配额，所有项目都可以在这些项目上使用，构建 OSS macOS 项目的组织可以通过联系 billing@circleci.com 请求免费的 macOS 访问。

如果您正在构建一个大型开源项目，并且您的项目需要更多的学分或更多的并发性，[请与我们](https://circleci.com/talk-to-us/?source-button=Open%20Source%20usage%20plan)讨论为您的组织定制计划。

## 升级到性能

如今，CI/CD 不仅仅是关于自动化构建，它还关于实现软件开发生命周期的最大优化，以便您的开发人员可以花更多的时间做他们最擅长的事情:编写代码。CircleCI 引入性能计划是因为开发人员的生产力不应该被并发限制所限制。

目前使用容器计划的 CircleCI 客户可以通过[向我们的支持团队](https://support.circleci.com/hc/en-us/requests/new)提交一张票来升级性能。我们免费计划的客户可以通过访问其组织的帐户设置页面进行升级。如果您想了解更多关于绩效计划的信息，请查看我们的[定价](https://circleci.com/pricing/)。

## 有问题吗？

阅读我们的[账单常见问题解答](https://circleci.com/docs/faq/#billing)文档或联系我们的销售团队。