# 数据在:Harness CI 是地球上最快的！装具

> 原文：<https://www.harness.io/blog/fastest-ci-tool>

新数据显示，Harness 持续集成的构建速度比其他领先的 CI 工具快四倍，这验证了三个重要新功能增强的影响:缓存智能、测试智能(Harness CI 独有的技术)和托管构建。

当我在 10 年前开始无人机作为一个开源项目时，我对使用现有 CI 工具发布代码所花费的时间感到沮丧。这导致了糟糕的开发体验，我想解决这个问题。我的愿景是创建一个快速、简单、开放和安全的 CI 工具。我很高兴现在通过 Harness CI 实现了这一目标。

在宣布线束[持续集成](https://harness.io/products/continuous-integration) (CI)模块的几个[新速度增强](https://harness.io/blog/announcing-speed-enhancements-and-hosted-builds-for-harness-ci)之后，我很高兴地分享我们的测试数据，这些数据显示**线束 CI 的构建速度比其他领先的 CI 解决方案快四倍**。我们测试结果中显示的快速构建时间验证了三个重要新功能增强的影响:缓存智能、测试智能(Harness 独有的技术)和托管构建，更重要的是，它们有助于创建出色的开发人员体验。

在 Harness，我们理解开发人员对缓慢、不稳定的构建感到沮丧。我们的重点是为开发人员消除摩擦点，这样他们就可以专注于快速、高效、可靠和安全地编写代码并交付代码。Harness CI 最新的速度增强功能就是对这一承诺的证明。

您可以在我们的[博客公告](https://harness.io/blog/announcing-speed-enhancements-and-hosted-builds-for-harness-ci)中了解更多关于这些新功能增强的信息，或者在我们的 [CI 产品功能页面](https://harness.io/product-features/continuous-integration)上深入了解技术细节。在这篇博客中，我们将进一步了解这些令人印象深刻的新数据，以及 Harness CI 如何通过这些最新的创新为企业节省高达 300%的基础设施成本。

## 后面的数据快了四倍

当我们查看最新 CI 功能增强的初始测试结果时，我们知道我们看到了前所未有的快速构建时间。我们也立即意识到，我们必须向开发人员展示我们可以帮助他们构建多快。

我们将测试设计为使用 Harness CI、GitHub Actions 和另一个流行的 CI 工具在 Apache Kafka、Apache RocketMQ 和 Apache Zookeeper 存储库中运行相同的构建。下图以分钟为单位显示了这些结果。*(注意，线束“最佳情况”的结果是在代码变化最小且没有运行测试用例的情况下获得的(例如，这是开源 repo* *中的* [*实际代码变化，其中确定要运行的测试用例非常少)。*](https://github.com/apache/kafka/pull/12688/commits)

测试结果显示，Harness CI 比最接近的竞争对手快 2 到 5 倍，比 GitHub Actions 快 3 到 4 倍。在某些情况下，Harness 比 GitHub 动作快 19 到 75 倍。

### 阿帕奇卡夫卡建造时代

[Apache Kafka](https://kafka.apache.org/) 是一个流行的分布式活动流媒体平台，在 GitHub 上拥有超过 [23，500 位明星。80%的财富 100 强公司也将其用于高性能数据管道、流分析、数据集成和任务关键型应用。](https://github.com/apache/kafka)

下面，您可以看到我们用 Apache Kafka 存储库记录的构建时结果。

在 50 多次管道运行过程中:

*   GitHub Actions 在 19 分钟内完成了 Kafka 的构建
*   CI 供应商 2 在 15 分钟内完成了 Kafka 构建
*   Harness 平均用 5 分钟完成构建，最好的情况下构建时间为 24 秒

由于 Apache Kafka repo 是一个非常大的项目，我们还使用它来展示我们的测试智能功能(市场上独一无二的产品)所节省的大量时间。使用机器学习(ML)，测试智能根据代码变化确定哪些测试是必要的。在我们的演示中，测试智能有效地将单元测试的数量从 16，000 减少到 700。但这还不是全部——测试智能将必要的测试分开并并行运行。

在我们详细介绍了其他测试和我们使用的方法之后，我们将进一步了解测试智能是如何工作的以及那些结果。

### Apache RocketMQ

[Apache RocketMQ](https://rocketmq.apache.org/) 是一个分布式消息和流媒体平台，在 GitHub 上拥有超过 [18，000 颗星星，这使得构建事件驱动的应用程序更加容易。](https://github.com/apache/rocketmq)

下图显示了 Apache RocketMQ 存储库测试的构建时测试结果。

在 50 多次管道运行过程中:

*   GitHub Actions 在 19 分钟内完成了 RocketMQ 的构建
*   CI 供应商 2 在 15 分钟内完成了 RocketMQ 构建
*   Harness 平均用 3 分 18 秒完成构建，最好的情况下构建时间为 1 分钟

### 阿帕奇动物园管理员

Apache Zookeeper 在 GitHub 上拥有超过 [10，000 颗星星，旨在使维护和协调分布式系统更加容易。](https://github.com/apache/zookeeper)

您可以在下面的图表中看到我们来自 Apache Zookeeper 库的构建时测试结果。

在 50 多次管道运行过程中:

*   GitHub Actions 在 19 分钟内完成了动物园管理员的构建
*   CI 供应商 2 在 13 分钟内完成了 Zookeeper 构建
*   Harness 平均需要 6 分钟来完成构建，最好的情况下构建时间为 15 秒

## 测试方法

我们选择测试这三个项目是因为它们在开源社区中的受欢迎程度，以及企业组织中开发团队的高采用率，包括 Goldman Sachs、Target、Intuit、Cisco 等等。由于我们使用开源项目，我们也有能力为每个构建复制相同的测试场景。

我们使用另一个流行的 CI 供应商 GitHub Actions 和 Harness 的免费产品运行这些项目的构建。性能数字是通过从每个存储库构建 50 多个拉请求来计算的。这允许我们使用[实际代码提交](https://github.com/apache/kafka/pull/12771)到开源库进行测试，以便模拟开发人员每天遇到的真实用例。我们为每个构建运行使用了开箱即用的默认配置，而不是微调查询，因为我们知道开发人员在不需要进行额外修改时会有更好的体验。

您可以从下面 Kafka 存储库中运行的测试中看到实际 pull 请求的屏幕截图:

## 缓存和测试速度数据

Harness CI 在这些测试中展示的速度结果是通过我们最新的功能增强实现的。让我们仔细看看这些不同的特性是如何影响测试结果的。

### 缓存智能节省时间

软件开发本质上是复杂的，在构建工具、包管理器等等之间有各种各样的依赖。所有这些复杂性增加了构建时间。开发人员并不总是记得手动打开缓存或指定所有必要的路径来为每个目录存储正确的数据。通过自动缓存众所周知的 Java 和 Node.js 目录，Cache Intelligence 帮助开发人员减少了等待构建完成的时间，这意味着有更多的时间进行编码。

### Test Intelligence 节省时间

Harness CI 独有的 ML-powered Test Intelligence 通过增加测试并发性节省了更多时间。Test Intelligence 根据代码的变化来决定哪些测试子集是必要的，同时提供了选择哪些测试及其原因的可见性。在确定了所需的测试之后，这些测试同时运行，以便在不影响质量的情况下进一步加快构建时间。

下图详细描述了在同一个 [Kafka pull 请求](https://github.com/apache/kafka/pull/12771)中更改 10 个文件时 Test Intelligence 的性能提升。Test Intelligence 只对这 10 个文件进行并行拆分和运行测试，而不是顺序运行它们。结果，Harness 运行了大约 700 个与代码更改影响相关的测试，而 GitHub Actions 和其他 CI 工具运行了整个套件大约 16，000 个测试。

您可以看到 Test Intelligence 如何通过只运行必要的测试并并行运行它们来节省大量时间。借助 Test Intelligence，Harness CI 采用了一种独特的方法来加快构建时间，而不牺牲质量。

## 利用 CI 节省时间和成本

在一个要求公司更快发展以保持竞争力的市场中，“它只是工作”对 CI 来说足够好的日子已经一去不复返了。

我们已经讨论了最近提高速度的改进，但在构建速度更快时也有成本节约。有了托管构建，公司不仅可以减轻托管基础设施的管理负担，还可以事半功倍。更快的构建意味着消耗的构建时间更少，占用的资源更少。

将 Cache Intelligence 和 Test Intelligence 与托管构建相结合，最终每年可为企业节省 200-300%的基础设施成本。下图显示了这一节省，其中使用了一个 Harness 客户的实际使用数据，该数据用于表明他们将在 Harness 和每个其他 CI 供应商上花费多少时间和金钱。

### 测试方法

这是我们根据真实的客户数据确定成本节约高达 300%的方法。

我们的客户有 90 名软件工程师，他们每月使用 Harness CI 构建 19，524 次。为了保持这个速度，他们每年建造 234，288 次。目前，构建的平均时间是 15 分钟。我们估计每个开发人员每周的平均构建次数是 11 次。所有的评估都假设构建在 Linux 上运行。根据这个客户，一个月总共需要 292，860 分钟，一年需要 3，514，320 分钟。

接下来，我们将这些数字与两家 CI 供应商进行了比较。GitHub Actions 每构建分钟花费 0.008，CI Vendor 2 每构建分钟花费 0.0120。相对于 GitHub Actions，Harness CI 的价格具有竞争力，并且价格低于 CI 供应商 2。

根据非常保守的估计，Harness CI 的速度是两家供应商的两倍，每月成本为 1，171 美元，每年为 14，057 美元。CI 供应商 2 的成本分别为每月 2，343 美元和每年 28，115 美元。GitHub Actions 每月花费 3514 美元，每年花费 42172 美元。这比 CI 供应商 2 每年节省 200%,比 GitHub Actions 每年节省 300%。

## 亲眼看看驾驭 CI 有多快

就个人而言，我非常兴奋地分享这些更新。过去几年，CI 创新仍有许多不足之处，但如今，随着运行构建的速度提高了四倍，游戏规则完全改变了。我的梦想是做一个简单、开放、快速、安全的 CI 系统，现在这个梦想变成了现实。我邀请您尝试[这个样本](https://github.com/harness-community/kafka/blob/trunk/.harness/README.md)来重现结果并亲眼看看。

准备好开始构建全球最快的 CI 平台了吗？[使用线束配置项免费开始使用](https://harness.io/products/continuous-integration)。

‍

*Bradley Rydzewski 是 Harness 的产品高级总监，也是无人机的创始人。*