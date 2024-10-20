# 如何选择:使用北极星与 Fairwinds Insights 的优势？

> 原文：<https://www.fairwinds.com/blog/benefits-polaris-fairwinds-insights>

 Fairwinds 的团队已经为几十个组织管理了数百个集群，这让我们对大多数组织在其 Kubernetes 环境中遇到的问题有了独特的见解。我们一次又一次地看到同样的问题，其中大多数都围绕着安全性、效率和可靠性。我们想给我们的客户一种识别和修复这些问题的方法，所以我们建造了 [Polaris](https://polaris.docs.fairwinds.com/) 。

Polaris 是一款开源工具，我们将继续使用它来帮助我们为我们的[客户](/customer-stories)管理数百个生产工作负载。它运行各种检查，以确保您已经使用 Kubernetes 最佳实践配置了 Kubernetes pods 和控制器，帮助您避免工作负载配置问题。

但是对于许多组织来说，北极星并不能满足他们所有的需求。为了更好地服务我们的用户，我们为需要以下服务的组织构建了 [Fairwinds Insights](/insights) :

*   随着时间的推移跟踪调查结果
*   跨多个集群和团队进行协调
*   想要与 [Slack](https://insights.docs.fairwinds.com/configure/policy/rules/#integrations) 、 [Datadog](/blog/prevent-risk-monitor-kubernetes-fairwinds-datadog) 和 [GitHub](https://github.com/) 集成
*   希望北极星数据与其他审计工具的数据进行对比

Fairwinds Insights 平台通过提供跨团队和集群的一致性来识别和补救 Kubernetes 安全风险，从而弥合了开发、安全和运营部门之间的差距。

## 一个工具(北极星)对多个工具(费尔温德见解)

北极星有一点做得非常好:配置验证。它运行二十多项检查，帮助用户发现 Kubernetes 的错误配置。这是一个很好的开源项目，Polaris 可以帮助您避免配置问题，并确保您的组织遵循 Kubernetes 的最佳实践。

另一方面，Insights 是一个提供更多功能的平台。任何开源工具都可以链接到 Insights，使标准化数据变得简单，并在一个地方获得所有数据，通过将多个来源的数据集中到一个视图中，提高您对 Kubernetes 部署的可见性。

> 您可以永远免费使用 Fairwinds Insights。[拿到这里。](/coming-soon)

Insights 提供了比 Polaris 更多的好处；它使用多种开源工具构建，为用户提供了许多功能，包括:

Fairwinds Insights 可以在多个集群和协作团队中应用 Polaris 和这些其他优秀工具的优势，在整个组织中保持一致。

## 数据持久性

北极星提供即时快照，这是一个快速了解你在做什么的好方法。

然而，北极星开源项目不包括数据库，所以这些快照中的变化不会被跟踪。Insights 跟踪随时间的变化，因此您可以看到调查结果的整个生命周期。例如，如果您修复了一个问题，然后它又出现了，您可以看到它何时再次出现，并且更容易发现它是如何发生的。你也可以看到你的北极星健康总分是上升还是下降。

Insights 还可以让您很容易看到何时引入新的变化，以及它们如何影响您的 Kubernetes 环境。

## 多集群支持

Polaris 在单个集群上运行得非常好，可以帮助您使用 Kubernetes 最佳实践来配置 Kubernetes pods 和控制器，但是如果您想在大型机群的每个集群上运行它，该怎么办呢？您需要运行几十个仪表盘来查看所有集群配置。

虽然我们只设计了 Polaris 来提供单个集群的快照，但 Insights 是为多集群部署而设计的。Insights 可以轻松地在您的整个车队中安装 Polaris 和其他工具，将您的所有发现放在一个单一的窗格中。这有助于您根据发现的问题所在的集群来确定优先级或降低优先级，因此，如果您在生产集群中发现问题，您知道快速解决该问题比解决开发集群中发现的配置问题更重要。这可以节省时间，帮助你专注于最重要的问题。

## 第三方集成

Polaris 通过仪表板或命令行界面(CLI)展示其调查结果:

![Polaris findings in a command line interface (CLI)](img/3c547ee49ab5882f5ddcd603489ab099.png)

这种方法工作得很好，但是通常您希望将这些发现发送到外部平台，以增加可见性并确保工程师得到通知。

当 Fairwinds Insights 发现问题时，它可以自动创建 GitHub 和吉拉票证，或者向您的团队发送 Slack 消息。Insights 还可以更容易地看到 [Datadog](https://docs.datadoghq.com/integrations/fairwinds_insights/) 中的趋势，汇总来自每个插件的发现，并在高度可定制的视图中呈现它们。

## 不是哪个，是什么时候

北极星在这方面做得很好。它可以帮助您验证您的配置，并使您可以轻松地创建用于验证的自定义策略。我们继续对 Polaris 进行更新，我们鼓励参与我们的开源用户组，帮助我们确定项目的方向。

Fairwinds Insights 是一个跨多个集群提供可见性的平台，因为它合并了多个开源项目，所以它为您的 Kubernetes 环境的整体安全性和合规性提供了大量有用且可操作的信息。我们在 Insights 平台中的集成使您可以轻松查看 Polaris 结果以及其他审计工具，跟踪随时间的变化，并将数据推送到其他位置，帮助您弥合开发、安全和运营团队之间的差距。

Polaris 和 Insights 都可以帮助您识别和修复 Kubernetes 中的安全性、效率和可靠性问题。处于 Kubernetes 之旅早期的组织可能希望从 Polaris 开始，但一旦您在多个团队中运行多个集群，Fairwinds Insights 可以帮助提供一致性和治理，以最大限度地降低风险，并保持集群平稳运行。

[![Fairwinds Insights is Available for Free Sign Up Now](img/90e93a941f22f2087c3a229a91ea6c10.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/d329e036-9905-4715-85b8-31a98b50623c)