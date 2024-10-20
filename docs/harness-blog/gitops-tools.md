# GitOps 工具& kuble gitops 工具| Harness

> 原文：<https://www.harness.io/blog/gitops-tools>

我们编制了一份最佳 GitOps 工具列表，帮助您选择最适合您组织的工具。我们包含了开源和付费工具，并为您比较了它们的特性。请继续阅读我们对市场上 GitOps 工具的公正观察。

## GitOps 简介

GitOps 项目正席卷软件工程团队，吸引他们的是完全声明式的部署和更高的[生产率](https://harness.io/blog/devops/developer-productivity/)。各公司争相提供工具来简化和实现大规模 GitOps。很难解析目前市场上所有不同的 GitOps 解决方案，所以这篇博客将为您搜索最佳 GitOps 工具提供一个起点。

GitOps 是一种以 Git 存储库为中心的部署方法，其中任何合并到 Git repo 的新代码都会触发对各种环境和集群的实时部署。由于 Git 是开发人员社区中熟悉的工具，GitOps 被吹捧为以开发人员为中心的部署策略。

GitOps 是一种支持连续交付的新方法，但它在过去几年中一直用于基础设施管理。运营团队需要一种方法来自动执行云基础架构调配请求，以减少创建新环境、集群等所需的资源。提议的解决方案使用存储为代码的配置文件([基础架构作为代码](https://harness.io/blog/devops/infrastructure-as-code/))来根据新部署的需要启动不同的环境或新的集群。基础设施团队继续迭代这个过程，直到他们创建了一个安全且可重复的方法来管理声明性基础设施。

鉴于 GitOps 基础设施团队的成功，将该方法应用于软件交付用例只是时间问题。创建了几个工具来支持 GitOps 部署。这些工具使用 Git 存储库作为代码更改的唯一来源。然后，它们检测代码更改，并将应用程序状态同步到存储在 Git 中的状态。开发人员现在可以通过完全声明性的低接触流程部署映像或新功能，而不是处理管道、阶段和批准。

供应商抓住机会成为 GitOps 部署市场的领导者。他们开始添加企业功能来补充基本的 GitOps 方法。

在这篇博客中，我们将讨论活跃在 GitOps 市场的不同供应商，以及您应该评估哪些工具来开始您自己的 GitOps 之旅。要了解有关 GitOps 的更多信息，请查看我们的"[什么是 GitOps？](https://harness.io/blog/devops/what-is-gitops/)“博客。

## 评定标准

在评估 GitOps 解决方案时，需要考虑三个主要类别:基本特性、易用性和企业功能。

### 基本特征

每个 GitOps 解决方案都应包括漂移检测和应用同步等功能。一些解决方案可能比其他解决方案更好地提供了基本要素，但是缺乏这些基本特性的解决方案将被质疑为可行的 GitOps 工具。

### 易用性

软件工具只有在易于使用时才是有用的。GitOps 是一种简单的部署方法，但是很难设置和扩展到期望的状态。该列表中的工具将基于客户可用性和总体[开发者体验](https://harness.io/blog/devops/developer-experience/)进行评估。

### 企业能力

GitOps 不能单独存在。它需要一套功能和产品与之协同工作，以实现速度和可扩展性。我们将讨论这些 GitOps 工具带来的额外的企业特性，以便让用户完全控制。

## 开源 GitOps 工具

### 阿尔戈光盘

Argo CD 是市场上最受欢迎的开源 Kubernetes GitOps 工具。开发人员称之为“自切片面包以来最好的东西”，因为它易于设置，有一个很棒的 GUI 和仪表板。Argo CD 抽象出使用 Kubernetes 的复杂性；用户不需要拥有 Kubernetes 的博士学位来部署代码，只需将代码提交给 Git，就可以看到更改同步到适当的环境中。

Argo 擅长基本的 GitOps 功能。这是一个直观的工具，安装起来不费时费力。但是，它是适合您的组织的工具吗？Argo 缺乏保持部署安全和合规所需的企业功能。像审计跟踪、集中实例管理和[基于角色的访问控制(RBAC)](https://harness.io/blog/continuous-delivery/rbac/) 这样的特性要么是 Argo 平台中后来才想到的，要么根本就不存在。GitOps 是一种快速部署软件的方式，但是如果您打算在不同的开发团队之间扩展 GitOps，您将需要这些企业功能。

### 流量

Flux 是另一个开源解决方案，与 Argo 同时上市。它由 Weaveworks 创建，目前是 CNCF 的孵化项目。Flux 有一个命令行界面(CLI)优先的方法，用户界面可以在以后添加。对许多人来说，这造成了负面的开发体验。仪表板必须手动设置，这增加了 GitOps 管理的难度。也就是说，Flux 提供了一个开始使用 GitOps 的框架，一些开发人员更喜欢实用的 CLI 方法。

Flux 还支持基本的 GitOps 工作流，但许多用户表示 Flux 需要更长的时间来设置。与 Argo 类似，Flux 需要额外的功能才能扩展到企业。安全和治理控制要么必须定制脚本，要么开发团队必须创建一套非正式的实践来满足法规遵从性标准。

## SaaS GitOps 工具

### CodeFresh

CodeFresh 是一个独立的持续交付工具，由 Argo 提供支持。CodeFresh 创建的环绕式解决方案用于管理新的和现有的 Argo 实例，以提高可见性和安全性。它为用户提供了一种通过增强的企业级控制和可见性来使用 ArgoCD 的方法。CodeFresh 提供安全性、集中管理和[审计跟踪](https://harness.io/blog/continuous-delivery/audit-trails/)以实现合规性。

CodeFresh 提供了 ArgoCD 的可用性和功能性，同时还填补了扩展 GitOps 所需的企业空白。使用 CodeFresh 的团队成员将能够实践 GitOps 原则，同时还可以创建必要的防护栏来保持 GitOps 流程的合规性。但是 GitOps 只是 DevOps 的一个子集，正如我们之前所说，它不能独立存在。Codefresh 不提供 CD 管道或功能标志。如果 DevOps 团队希望实现真正的持续部署，他们将需要这些组件。

### 编织作品

Weaveworks 是一个开源 Flux 的环绕式解决方案。Weaveworks 目前没有提供 SaaS 产品，而是为客户构建 Flux 解决方案。他们将在未来发布独立的企业产品。

Weaveworks 通过集中式仪表板提供了对 GitOps 部署的更多可见性。它还提供了高级治理控制，如 RBAC 和审计跟踪。

仔细研究 Weaveworks 可以发现需要手动编写脚本来创建企业解决方案的基本 Flux 功能。从短期来看，这可以作为一个基本的 GitOps 工具包，但是从长远来看，每当您想要更新您的流程时，您仍然需要管理流量。当然，当 Weaveworks 发布他们的 SaaS 解决方案时，情况会有所变化。与 CodeFresh 类似，Weaveworks 缺乏交付管道以及在您的组织中成功交付软件所需的额外产品和特性。

### GitLab

GitLab 提供了一个应用交付平台，包括一个核心开源版本和可供购买的附加功能。GitLab 希望成为包括 GitOps 在内的 CD 工具的唯一来源。

GitLab 是这个领域的一个有趣的参与者，因为源代码管理和 CI/CD 是其平台的一部分。这意味着 GitLab 用户只需一个应用程序即可管理 GitOps 和整个软件交付流程。

他们的 [CI](https://harness.io/learn/comparison-guide/gitlab-vs-drone) / [CD](https://harness.io/learn/comparison-guide/gitlab-cd-vs-harness) 平台看起来是一个有吸引力的替代方案，可以替代仅仅专注于 GitOps 的解决方案。然而，仔细观察 GitLab 会发现它缺乏真正的 GitOps 功能。GitLab 通过提供代码环境、协作平台和 CI/CD 自动化流程来“支持”GitOps。正如我们在评估标准中所讨论的，真正的 GitOps 解决方案应该提供漂移检测和应用同步。GitLab 没有直接的方法来实现这两个功能，所以 GitOps 这个术语真的不应该用在他们的平台上。我们推荐一种不同的方法。

### 马具

Harness 是首要的商业和企业级软件交付平台。Harness CD 提供了两种部署软件的方法:连续部署管道方法和 GitOps 方法。Harness GitOps 是一个直观的解决方案，具有企业级功能，可确保安全性和合规性。它专为希望为开发人员提供最高效的软件部署方式，同时为开发运维团队提供扩展所需的控制的组织而构建。

Harness 提供了所有必要的 GitOps 功能，并且易于设置和使用。它提供了市场领先的安全性和治理，以及集中化的实例管理、精细的 RBAC 和详细的审计跟踪。最重要的是，Harness 提供了将代码投入生产所需的所有附加工具:生成工件的最佳 CI 解决方案，利用管道和 GitOps 的最佳 CD 解决方案，以及向客户安全发布功能的最佳[功能标志解决方案](https://harness.io/products/feature-flags/)。Harness 还提供了许多其他工具来为您的组织节省时间和金钱。

## 结论:并非所有的 GitOps 工具都是同等创建的

GitOps 已经从基础设施自动化发展成为在 DevOps 社区中部署软件的首选方式。由于这是一个相对较新的学科，各公司争相成为 GitOps 的单一工具。

我们已经讨论了只专注于创建 GitOps 管道的工具，并且我们已经讨论了专注于整个软件交付过程的其他工具。我们建议您在评估工具时，考虑 GitOps 将如何在您的公司中扩展。

GitOps 不可能存在于真空中。它需要一个完整平台的支持才能长期成功实施。要详细了解如何在您的组织中实施 GitOps 部署流程，请查看我们的博客[最新 GitOps 最佳实践](https://harness.io/blog/devops/6-gitops-best-practices/)。同样，您可能也会对我们关于 [CI/CD 最佳实践](https://harness.io/blog/continuous-delivery/ci-cd-best-practices/)的博客感兴趣，以便进一步确定您的软件交付过程。

我们相信 Harness GitOps 是 GitOps 功能和平台能力的最佳结合。如果你对驾驭 GitOps 感兴趣，你可以[今天就注册免费试用](https://harness.io/gitops-beta/)。