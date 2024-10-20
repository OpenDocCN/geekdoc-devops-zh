# GitHub Kubernetes 最佳实践徽章是什么意思？

> 原文：<https://www.fairwinds.com/blog/github-kubernetes-best-practices-badge>

 如果你在 GitHub 领域呆过一段时间，你会对开源项目的各种徽章很熟悉。徽章的存在是为了证明第三方对回购部分质量的验证。我经常看到的包括“Go Report”徽章或 CircleCI 测试通过徽章，它们看起来像这样:

![go report A+ PASSED](img/32db37fb2e209fbac08d1d57cc3d2278.png)

这些标记的存在给用户一些信心，让他们相信回购中的代码符合公认的标准。这些徽章对贡献者来说很重要，因为它让你感到自信，你正在发布你可以引以为豪的代码。作为一个开源工具的用户，这可能对你也很重要；它提供了一定程度的信心，即该工具已经以某种方式进行了审查，并且没有充满漏洞，或者至少您知道它正在运行或者与某个东西的最新版本兼容(具体兼容什么取决于徽章和开源工具或项目)。

同样，今天我们公开宣布 Kubernetes 最佳实践徽章的可用性，该徽章将应用于包含旨在部署到 Kubernetes 集群中的代码的项目。这可能与您的开源项目的一小部分相关，也可能与整个项目相关。

例如，我们已经将徽章应用于 [Fairwinds GitHub repo 中的相关 Kubernetes 开源工具。](http://github.com/fairwindsOps/) 看起来是这样的:

![K8s Best Practices - No issues found](img/4065ceb55e4478a9b0ed3d5979867df5.png)

在后端进行验证的工具是[fair winds Insights](/insights)，这是我们用于 Kubernetes 安全性和配置验证的商业工具。当有人点击该徽章时，会将他们带入一个类似这样的 Fairwinds 仪表盘:

![Screenshot of FairwindsOps/polaris](img/f99a9872a7acbe4667336c288f629ab4.png)

通过将 K8s 最佳实践徽章应用到您的开源项目，您向您的用户展示了您的项目已经通过了许多可能领域的最佳实践检查，从已知的常见漏洞和暴露(CVE)到常见的安全错误配置。绝大多数安全问题仍然是错误配置的结果，如果您的项目打算部署到 Kubernetes 集群中，那么您可以对它进行这些检查，并对它进行改进，直到您可以用洞察力来验证它，从而让您的用户放心。

虽然 Fairwinds Insights 是一个用于扫描 Kubernetes 集群和基础设施代码的商业工具，但我们将它永久免费提供给开源项目。我们创建了这个徽章和它背后的工具，因为我们是 Kubernetes 和围绕它的开源技术社区的忠实拥护者。我们基于我们管理数千个集群的经验构建了 Fairwinds 本身和 Insights，创建了许多属于 Insights 的[](/blog/introducing-the-fairwinds-open-source-user-group)开源工具。例如，Polaris 会进行各种检查，以确保 pod 和控制器的配置符合 Kubernetes 的最佳实践。

> Fairwinds Insights 可供免费使用。你可以在这里报名。

如果你想知道如何获得我们的徽章，去看看我们的 [北极星项目](https://github.com/FairwindsOps/polaris) 上的例子，设置集成，并让我们知道你正在为一个开源项目使用它。当我们收到您的消息时，我们会确保您的许可证将永久免费。

[![Join the Fairwinds Open Source User Group today](img/8ab607311768483f3bb5136a75381d4b.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/b163554e-b5ef-4f40-a053-03afe6ecbee6)