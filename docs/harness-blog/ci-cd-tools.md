# 开发运维|线束的 4 款最佳 CI/CD 工具

> 原文：<https://www.harness.io/blog/ci-cd-tools>

您是否正在寻找一个全面的 CI/CD 工具？我们列出了 4 个最佳 CI/CD 工具供选择。

好的 CI/CD 工具是能够在软件开发世界中创造一个“惊喜”因素的关键组成部分。无论是在工作流中构建自动化，降低连续交付管道中任务的复杂性，还是更有效地管理代码变更和错误，为未来的工作选择正确的工具包都是至关重要的。

在 DevOps 中，这与 CI/CD 和管理整体部署渠道密切相关。但是有这么多的步骤和这么多的贡献者，DevOps 团队如何更有效地创建使他们能够扩展的强大基础呢？在理想的世界中，工程团队(包括开发人员)总是从一开始就努力构建规模。好消息是，其他公司也认识到了这种需求，并为此开发了工具。虽然它们都提供了自己的风格，但最终目标是相同的:使软件交付更容易升级。

让我们探讨一下这些 CI/CD 工具，以及它们如何满足协作、轻松安装、环境许可、版本控制系统等需求，以及 DevOps 生命周期中的其他重要考虑事项。

## 马具

Harness 是首屈一指的商业和企业级 CI/CD 平台，功能强大得令人难以置信，它通过确保一种自助式、简单、高效的软件交付方法，为资源丰富或有限的团队而构建。

无论您的 Kubernetes 集群是在谷歌云平台(GCP)、微软 Azure、亚马逊网络服务(AWS)——甚至是自主开发/自托管的，Harness 都为您提供了将您的[掌舵图](https://harness.io/blog/continuous-delivery/what-is-helm/)部署到任意多个集群的能力。

Harness 基于内置或定制的部署模板自动生成所有部署脚本——听到了吗，Jenkins？没错，没有脚本！它还拥有基于机器学习的部署验证，很快将成为自己的模块，名为[持续验证](https://harness.io/platform/continuous-delivery/continuous-verification/)，在[部署](https://harness.io/blog/continuous-verification/blue-green-canary-deployment-strategies/)后监控你的应用程序的异常。有关持续验证的更多信息以及一些先睹为快的截图，请访问我们关于[持续验证的重要性](https://harness.io/blog/continuous-verification/importance-of-continuous-verification/)的文章。

Harness 拥有与 Datadog、New Relic 和 AppDynamics 等可观测性平台的深度集成。基础架构供应器通过 CloudFormation 和 Terraform 的强大集成提供。此外，Harness 与吉拉和斯诺完美集成，用于问题跟踪。它还非常关注法规遵从性和治理，拥有细粒度的 RBAC、集成的机密管理、权限和审计跟踪。

完整的 Harness 软件交付平台具有持续交付、持续集成、功能标志、持续验证和[云成本管理](https://harness.io/platform/cloud-cost-management/)模块，允许您按需构建、测试、部署和验证。Harness 还通过收购无人机对开源社区进行了大量投资，你可以[今天](https://docs.drone.io/)下载。最后，Harness 现在完全支持 GCP、微软 Azure 和 AWS——在我们最近的[公告](https://devops.com/harness-empowers-developers-with-cloud-agnostic-end-to-end-software-delivery-platform/)中了解更多关于我们对厂商不可知的承诺。

线束特征

*   提供免费增值选项
*   SaaS 和内部
*   持续验证，金丝雀部署
*   具有命令行界面的完整 GitOps 功能
*   RBAC、SSO 和秘密管理
*   加速指标、仪表板和报告
*   快速启动允许在 15 分钟内完成管道

要查看 Harness 和其他提供商之间更深入的细分，请查看我们的 [DevOps 比较工具](https://harness.io/devops-tools/)页面。

## GitLab CI/CD

GitLab 提供了一个完整的软件交付平台，包括一个核心开源版本和可供购买的功能。GitLab 希望成为企业 CI/CD 的唯一真实来源。所有的软件交付基础都在这里，但是这个工具缺乏高级的治理特性，比如自动验证和自动回滚。

GitLab 的难点在于它的全有或全无模型。如果你想使用任何 GitLab 工具，你需要使用所有的 GitLab 工具。因此，如果你的团队对他们使用的工具固执己见，你必须说服他们放弃熟悉的东西，转而使用新的东西。我们对 GitLab 的建议？坚持 Git。

GitLab 特性:

*   SaaS 和内部
*   云原生和传统应用
*   地形补给者
*   GitOps
*   计划图编制
*   连续累计
*   持续部署
*   源代码管理
*   人工验证。

## Codefresh

就 CI/CD 工具而言，Codefresh 在几个领域达到了目标——即当它涉及到现代特性时。它们似乎触及了所有的流行词汇，像代码为 T9 的管道、GitOps 2.0、自动预览环境、我的天啊！

这个基于云的原生容器平台拥有强大的治理功能，如 RBAC、审计跟踪、SAML SSO、通过加密的内置秘密管理(或者，如果你是第三方的粉丝，可以使用 HashiCorp Vault)等等。

此外，Codefresh 还可以集成所有主要的云和 Git 存储库提供商，包括吉拉、Helm、Argo 和 GitHub Actions。谈到可观察性，Codefresh 用[蜂巢](https://www.honeycomb.io/)来处理。

对 Codefresh 最大的担忧是资源消耗和停机时间(即使是部分的)。浏览他们的状态页面，有相当数量的[中断](https://status.codefresh.io/uptime?page=1)。至于资源消耗，[的客户评论](https://www.g2.com/products/codefresh-codefresh/reviews)指出，由于 Codefresh 消耗了相当多的资源，因此检查计划限额很容易。然后他们需要支付日常费用，如果你预算紧张，这可不是什么好事。

Codefresh 功能

*   稳健的治理
*   小型团队的免费计划
*   Saas 和内部部署
*   用于内部安装的 Codefresh 转轮
*   完整的 GitOps 功能
*   预建管道模板
*   支持构建和测试中的并行性

查看更深入的 [Codefresh 与 Harness](https://harness.io/codefresh-cd-vs-harness/) 对比。

## CI/CD 工具#4: Azure DevOps

2018 年，Azure DevOps 诞生了——随之而来的是用于 CI/CD、报告、工件、Git 存储库管理、测试等的各种工具。他们有各种各样的插件和扩展来支持这个平台。

作为一个 CI/CD 工具，Azure DevOps 非常灵活，并提供可定制的管道。Azure DevOps 管道的一个(相当大的)缺点是在设置和配置过程中需要大量的脚本。但是，它们有管道模板和触发器来改进设置过程。

Azure DevOps 为 Azure、Kubernetes 和基于 VM 的资源提供支持。他们的 GitOps 功能仅用于管道，不用于发布。它们缺乏任何验证能力，回滚是使用插件实现的。您可以查看软件交付度量，例如构建历史和部署状态，但是仅限于确定部署速度。

像其他现代 CI/CD 工具一样，Azure DevOps 支持 docker 映像的创建，并且可以在 Azure 上轻松部署和运行 Docker 容器。

不出意料，微软和谷歌最近与 Codefresh 合作，成为推荐的第三方 CI/CD 解决方案。随着最近对 GitHub 的收购，微软现在正在销售两款竞争产品:GitHub Action 和 Azure DevOps。我们不确定这对 ADO 意味着什么，但是微软的 Sasha Rosenbaum 已经声明他们正在朝着只有一个 T1 的方向发展。要记住的事情！

Azure DevOps 功能

*   SaaS 和内部
*   云原生和传统应用支持
*   有限的基础设施提供者
*   有限的 GitOps 功能
*   粒状 RBAC 和 SSO

查看更多深入的产品评论 [Azure DevOps vs. Harness](https://harness.io/azure-devops-vs-harness/) 对比。

## 独立的 CI 和 CD 工具，以及我们不推荐的工具

说到 CI/CD，使用不同的供应商进行持续集成和持续部署的情况并不少见。例如，CI 可以用 CircleCI，CD 可以用 GoCD。或者，你可以选择开源项目，用无人机做 CI，用 Argo CD 做 CD。有如此多的选择，这取决于您是想要一个大部分是预构建的解决方案，还是一个需要脚本/扩展的解决方案。出于这个原因，虽然我们肯定会推荐一些工具，但也有一些工具我们会建议您避免使用。这里有一个简短的列表:

### 特拉维斯·CI

Travis CI 成立于 2011 年，是一个托管的[持续集成工具](https://harness.io/blog/continuous-integration/continuous-integration-tools/)，用于简化 GitHub 和 Bitbucket 上托管的项目的软件交付过程。在早期，Travis CI 背后的愿景是为测试和构建构建一个框架，就像 RubyGems 用于发行版一样，但创始人不希望它只用于 Ruby。Travis CI 现在支持 30 多种不同的语言，如 Javascript、Perl、PHP 和 Python。

从一开始，Travis CI 的主要关注点就是他们对开源的热爱。开源项目可以在 Travis CI 上免费测试，但在 2020 年，一种新的定价策略被引入，对免费的开源版本增加了一些限制。我们也看到了这一变化带来的许多负面评论:用户声称作为开源项目获得批准要困难得多，有时，他们甚至没有得到客户支持的回复，导致他们选择完全不同的工具。因为我们是开源的超级粉丝，所以推荐 Travis CI 并不合适。

### 詹金斯

Jenkins 是最著名的开源工具，用于持续集成和持续部署。因为它是一个开源自动化服务器，所有人都可以免费使用，并且有一个庞大的社区，这导致了额外的支持、文档和功能。这个持续集成服务器/工具是一个独立于平台的 Java 程序。它适用于大多数主流操作系统，如 Linux、Windows 和 MacOS。

我们通常将 Jenkins 放在这个列表中，因为它已经有十年的历史了，不是云原生的，并且坦率地说，它过于依赖插件来扩展它的功能。我们不愿意推荐一个如此依赖于额外扩展性的产品。它还被设计成一个持续集成工具，DevOps 工程师/软件工程师必须依靠脚本来将其扩展到持续部署中。

### 大三角帆

Spinnaker 最初由网飞创建，是一个 CD 平台，只是有太多的问题需要我们推荐。我们在我们的 [Spinnaker Alternatives](https://harness.io/blog/continuous-delivery/spinnaker-alternatives/) 帖子中讨论了它们，但是概括一下:它只在内部部署，缺乏本地机密管理，不提供传统的应用程序支持，并且被称为设置和配置的“噩梦”。对于一个不提供原生 CI，只提供 CD 的平台来说，这似乎不值得付出努力。

## 利用工具按需构建、测试、部署和验证

这篇博文介绍了几个强烈推荐的 CI/CD 工具，以及各种 CI 和 CD 供应商。如果你喜欢这篇文章，并认为它很有见地，你可以在我们的 [DevOps 比较工具](https://harness.io/devops-tools/)页面上找到更多关于 CI/CD 工具的深入分析——以及它们如何与 Harness 相比较。更好的是，免费获得一份我们的 [CI/CD 购买者指南电子书](https://harness.io/buyers-guide-for-ci-cd-ebook/)，深入、公正地了解目前市场上的许多 CI/CD 工具——长达 55 页，是一本令人爱不释手的书！

当你在评估解决方案时，为什么不花点时间注册一个[免费试用](https://harness.io/free-trial/)的线束呢？当你可以在一个解决方案中试验的时候，比较这个解决方案会容易得多！