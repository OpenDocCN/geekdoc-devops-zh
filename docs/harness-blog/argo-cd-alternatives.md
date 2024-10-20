# 最佳 Argo CD 替代品和竞争对手|装具

> 原文：<https://www.harness.io/blog/argo-cd-alternatives>

你在寻找 Argo CD 的替代品吗？我们将 Argo CD 的备选方案进行了细分，以便您可以评估所有的最佳选项。

Argo CD 是一个开源的[连续部署](https://harness.io/blog/continuous-delivery-vs-continuous-deployment/)解决方案，提供 Kubernetes-first 支持。它遵循基于 Git 的工作流来自动化 Kubernetes 中的服务部署。作为 Kubernetes 控制器，它确保您的资源的当前状态与您的资源的期望或指定的目标状态相匹配。可以在 GitHub 上的 [argoproj](https://github.com/argoproj) 下找到。

Argo CD 利用了所谓的 Argo GitOps 引擎，这是一个实现核心 GitOps 功能的开源库，允许您管理、自动化、审计和了解您的部署生命周期。在过去的帖子中，我们讨论了 Argo CD 的[起源和特性，提供了对其功能的深入和有竞争力的观察。这篇博客文章将提供一些 Argo CD 的替代品，供你在](https://harness.io/blog/argo-cd-for-kubernetes/)[连续交付](https://harness.io/blog/what-is-continuous-delivery/) (CD)搜索中考虑。

## **什么是 GitOps，为什么它对持续交付很重要？**

在所有的实践和工作方式中， [GitOps](https://harness.io/blog/what-is-gitops/) 已经为实施持续交付的组织获得了更多的采用和关注。GitOps 利用包含基础设施声明性描述的 Git 存储库，以自动化的方式管理和定义生产环境。在这个模型中，Git 存储库是“事实的唯一来源”

如果您想要部署一个新的应用程序或者更新一个现有的应用程序，您只需要更新存储库——自动化的过程会处理所有其他的事情。这是有益的，因为它允许团队更快、更频繁地部署，从错误中恢复，管理凭证，并跟踪部署。

如果你想了解更多关于 GitOps 的信息，我推荐 [gitops.tech](https://www.gitops.tech/) 。

## **我们的评估方法**

我们根据对考虑使用 [CD 工具](https://harness.io/blog/continuous-delivery-tools/)的组织来说重要的许多功能和特性来评估这些工具。考虑到这一点，我们正在考虑以下标准:

*   kubernetes-原生的、开箱即用的支持
*   GitOps 功能和支持
*   安全性
*   费用

## **连续交货选择**

### **GitOps 替代方案:通量**

Flux 和 Argo CD 一样，也是开源的持续部署工具，支持 Kubernetes 内部的 GitOps。虽然 2019 年的计划意味着 Flux 项目将与 Argo GitOps 引擎集成，但 Flux 的最新版本使用了所谓的 [GitOps 工具包](https://toolkit.fluxcd.io/)，“以更好地服务于他们的 GitOps 愿景。”Flux 项目用以下描述定义了 GitOps:

*   **您在 Git 中声明性地描述了系统的整个期望状态。**这包括应用、配置、仪表盘、监控和其他一切。
*   **能描述的都能自动化。**使用 YAML 来加强系统的一致性。不需要运行 kubectl，所有的修改都通过 Git。使用 diff 工具来检测观察到的状态和期望的状态之间的差异，并获得通知。
*   你推的是代码，而不是容器。一切都通过拉请求来控制。新开发人员没有学习曲线，他们只是使用你的标准 Git PR 流程。Git 中的历史允许您从任何快照中恢复，因为您有一个事务序列。通过拉式请求进行操作更改要透明得多，例如，通过拉式请求修复生产问题，而不是对正在运行的系统进行更改。

Flux 项目整体上是一个云计算原生计算基金会(CNCF)孵化项目，为 Kubernetes 提供一个开放和可扩展的交付解决方案。[了解更多关于 Flux](https://github.com/fluxcd/flux2) 的信息。

### **开源 CD 平台的替代品:Jenkins X**

Jenkins X 是一个面向 Kubernetes 和云原生开发者的 CI/CD 平台。这是一个开源解决方案，支持内部自我管理安装。生态系统提供了一千多个插件，允许你集成和扩展你的 Jenkins X 实例。Jenkins X 为您的部署和安全需求提供支持。

它以代码形式提供对 GitOps 和配置的支持。通过它的 CLI 工具 jq，您可以使用 Jenkins X 来管理您的 Kubernetes 部署。使用自定义和脚本化管道模板，您可以控制希望如何触发部署管道。您还获得了对基于角色的访问控制的完全支持，允许您定义用户组并控制对特定 CI/CD 资源的访问。

Jenkins X 还集成了 Slack 和 HipChat，用于部署自动标签、批准和通知。审计跟踪记录了所有用户事件的目录，并支持对您的 [CI/CD 管道](https://harness.io/blog/ci-cd-pipeline//)中的敏感数据进行定制机密管理。

最后，詹金斯 X 是免费尝试和使用。

### **“GitOps，但更多”备选方案:GitLab CI/CD**

GitLab 是一个 DevOps 平台，提供从开发规划和源代码管理到 [CI/CD](https://harness.io/blog/what-is-ci-cd/) 和监控的解决方案。

如果您正在考虑 GitOps 功能和支持， [GitLab Kubernetes 代理](https://docs.gitlab.com/ee/user/clusters/agent/)使用 [Argo GitOps 引擎](https://github.com/argoproj/gitops-engine)为管理您的 Kubernetes 部署提供额外的工作流。因此，它共享并包装了属于 Argo CD 的 GitOps 功能。如果您同意或已经熟悉 Argo CD 如何工作和民主化 GitOps，这可能是实现完整部署体验的一个很好的选择。

GitLab 和 Jenkins 等许多其他 CI 解决方案一样，可以通过脚本扩展来提供 CD 功能。例如，GitLab 用户通过定义脚本来操纵到其目标环境的流量，从而实现蓝/绿或淡黄色[部署发布策略](https://harness.io/blog/blue-green-canary-deployment-strategies/)。

还可以通过原生 GitLab 平台获得变更管理批准和自动标签，从而为您的软件交付提供一个一体化平台。GitLab 生态系统围绕着将所有 DevOps 工具替换并统一到其单一的端到端平台。因此，GitLab 目前不提供与 Atlassian 吉拉或 ServiceNow 的第三方集成。

GitLab 在以下领域也很出色:

*   **源代码管理:**允许工程师对他们的源代码进行版本控制。
*   **持续集成:**允许工程师通过云原生 CI 将代码带入工件(构建和测试)。
*   **应用安全测试(AST):** 允许工程师扫描漏洞。
*   **问题跟踪&计划:**允许工程师记录并跟踪任务和需求。

最后，GitLab 支持自我管理和 SaaS 模式，按用户计费的高级和终极计划。更多详情见他们的[定价](https://about.gitlab.com/pricing/)。

### **多功能一体的替代品:线束光盘**

线束是一个[软件交付平台](https://harness.io/platform/)。Harness CD 允许工程师使用 UI 管道构建器按需部署并投入生产。或者，用户也可以将他们的脚本包含在他们的管道中，并利用 Harness 对流行的第三方秘密管理的现成支持(如 HashiCorp Vault、CyberArk 和 AWS KMS 等工具)、[部署验证(如 New Relic、DataDog、Splunk、Prometheus 等工具)和票务解决方案(如吉拉和 ServiceNow 等工具)。](https://harness.io/platform/continuous-delivery/continuous-verification/)

Harness 还具有广泛的 GitOps 功能，因此团队可以引用一个或多个 Git 存储库和分支。完全双向同步存在冲突解决方案，以及用于工程师跟踪变化的差异可视化。Harness 还能够基于 Git 事件触发管道，比如在存储库或定制 webhooks 中检测到的‘on commit’和‘new artifact version’。最后，它支持云原生和传统工作负载，涉及容器、Helm、Kubernetes、ECS、无服务器以及 WebLogic 或 WebSphere 等 web 服务器堆栈。

Harness 区别于其他解决方案的主要优势包括:

*   易用性，
*   AI/ML 智能，
*   用于软件交付的单一平台，包括 CI/CD、云成本管理和功能标志。

最后，Harness 支持自我管理和 SaaS 模式，计划按服务实例计费。更多详情见[定价](https://harness.io/pricing/)。

## **使用顶级开发工具评估线束**

这篇博文分享了 Argo CD 的一些替代方案，包括开源、Kubernetes-native 和 GitOps-capable 选项。如果您想对 Harness 等解决方案和其他 CI/CD 提供商进行更深入的比较，请查看我们的 [DevOps 比较工具](https://harness.io/devops-tools/)页面。