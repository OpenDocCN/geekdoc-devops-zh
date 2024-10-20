# 介绍 fair winds Insights:Kubernetes 管理工具

> 原文：<https://www.fairwinds.com/blog/insights-kubernetes-management-tool>

 我在应用安全和数据安全公司担任首席技术官超过 15 年，在此期间，我目睹了许多公司在尝试保护关键数据和基础架构时遇到的挑战。Kubernetes 和 Linux 容器的加速采用为开发、DevOps、信息技术和安全团队带来了许多新的挑战和机遇。

与任何新技术一样，随着 Kubernetes 的采用增加，以及团队学习和掌握保护这些新的 kube 和 Linux 环境所需的方法，会产生许多潜在的差距。有了 Kubernetes 集群和 Docker 容器，它代表了一种全新的软件部署方式。突然开发者需要理解 API、微服务和工作负载；需要跟踪新的指标，了解大量新的功能和配置。软件开发生命周期已经改变，容器化的应用程序越来越成为规范而不是例外。除了这些创新技术带来的挑战之外，许多传统的安全框架和流程不再有效，或者需要进行重大修改来保护这一新领域中的应用程序和数据。

## 绝不错过一步

作为一个组织，Fairwinds 专注于 Kubernetes 的管理和实施。今天，我非常自豪地宣布[fair winds Insights](https://www.fairwinds.com/insights)正式上市。这一首次发布标志着我们将 Kubernetes config 多年的专业知识转化为 Kubernetes 管理软件的高潮，该软件有助于验证配置以提高安全性、降低成本、节省时间和运行可靠的工作负载。Fairwinds Insights 通过保护和优化任务关键型应用程序，继续支持 Kubernetes 的成功部署。

我们一直在 [更新](https://insights.docs.fairwinds.com/release-notes/) 对 Fairwinds 的见解，以改善我们用户的体验，使其更容易成功部署 Kubernetes。我们构建并利用开源软件，如 Prometheus、[Polaris](https://www.fairwinds.com/polaris)、Nova 和[Goldilocks](https://www.fairwinds.com/goldilocks)，在我们的 Insights 平台中充分利用 Kubernetes 容器编排功能。我们采用的方式使您能够利用您选择的公共云提供商，如 AWS、GCP 和 Azure，以及虚拟机或裸机基础架构，因此您可以以对您的组织最有意义的方式部署您的应用程序和服务。

## 安全、经济、可靠的工作负载

Fairwinds Insights 旨在解决 Kubernetes 团队面临的三大挑战:安全、经济高效和可靠的工作负载。

### 一致、安全的基于 Kubernetes 的应用

随着云原生世界的不断成熟，大多数组织以及运行它们和构建云原生应用的人员都了解 Kubernetes 生态系统的优势，以及为什么打破孤岛并最大限度地减少开发、安全和运营团队之间的摩擦非常重要。虽然亚马逊[弹性 Kubernetes 服务](https://aws.amazon.com/eks/) (EKS)、 [Azure Kubernetes 服务](https://azure.microsoft.com/en-us/products/kubernetes-service/) (AKS)和[谷歌 Kubernetes 引擎](https://cloud.google.com/kubernetes-engine) (GKE)都提供了自动部署、扩展和管理容器化应用程序的简单方法，但开发、安全和运营团队仍在努力维护一致、安全的基于 Kubernetes 的应用程序。

在某种程度上，这是因为 Kubernetes 环境非常复杂，并且因为围绕 kube 的生态系统不断成熟，带来了新的插件、附加组件和灵活性。Fairwinds Insights 通过创建策略执行自动化来帮助组织自动执行 Kubernetes 最佳实践。通过提供所有 Kubernetes 集群的仪表板视图，Insights 平台增加了 DevOps 团队对 Kubernetes 环境的可见性，这有助于团队了解导致安全和合规性风险的错误配置。

Insights 还减少了漏洞管理所需的时间，因为它提供了内置的漏洞扫描和深入的故障排除信息。它还有助于运行容器的团队识别错误配置和漏洞，因为它集成了命令行界面(CLI)工具，如 kubectl(Kubernetes 的默认 CLI 工具)和开发团队常用的其他工具。洞察有助于向负责解决这些问题的人员或团队发送通知和票证。

### 试错 CPU 和内存资源使用设置

Kubernetes 自动调整 CPU 和内存资源使用设置，以实现自动伸缩，但许多组织都在努力找出如何设置这些 CPU 和内存资源使用设置。试图找出最佳设置会占用工程时间，并导致过度调配云容量或应用程序性能低下。因此，一些团队在初始测试中根本不设置要求或限制，或者将它们设置得太高，以确保一切正常运行——然后他们再也不会回去调整设置。确保扩展操作按预期工作的关键是根据指标设置每个工作负载的资源限制和请求，以便每个工作负载高效可靠地运行。

我们构建了一个开源项目， [【金凤花】](https://www.fairwinds.com/goldilocks) ，来帮助团队分配资源并获得正确的资源使用设置。该项目建立在洞察力的基础上，它帮助组织了解他们的资源使用、资源成本以及如何围绕效率应用最佳实践。Goldilocks 使用 Kubernetes Vertical Pod auto scaler(VPA)来评估工作负载的历史内存和 CPU 使用情况以及当前使用情况，以便就如何设置您的资源请求和限制提出建议。基本上，Goldilocks 为名称空间中的每个部署创建一个 VPA，查询它，并在仪表板中显示建议。Insights 通过自动化推荐流程来帮助团队消除猜测。无需反复试验，Insights 包括 Goldilocks，因此您可以提高 Kubernetes 集群效率并减少云支出。

### **监控维护可靠的工作负载**

传统的监控工具不能提供主动识别维护可靠的 Kubernetes 工作负载所需的变化所需的一切。虽然配置要简单得多，但学习如何优化 Kubernetes 需要时间，特别是如果您使用的技术早于云原生应用程序和配置管理工具。如果您使用旧的监控解决方案，并在其上部署 Kubernetes 管理工具，那么优化、可靠性和可伸缩性就更难实现。

提高 Kubernetes 可靠性的一个方法是正确设置配置。这说起来容易做起来难。云原生架构的挑战之一是理解并真正拥抱容器和 Kubernetes pods 的短暂本质。一旦您更好地理解了这个生态系统，并且熟悉了供应和容器编排，您就可以分析您的指标，以便在设置 CPU 和内存的请求和限制方面做出更好的决策。这允许 Kubernetes 调度程序完成它的工作。Fairwinds Insights 的内置工具持续审计 CI/CD 管道和运行时环境，以识别甚至纠正 Kubernetes 在软件开发生命周期中出现的错误配置。

## **从开发、安全和运营方面改进移交**

在过去的五个月里，我们对 Fairwinds Insights 进行了测试，有几十个客户积极使用它进行 Kubernetes 管理。我们已经确保 Insights 与部署工具集成，支持不同的开源 Linux 发行版，并且可以扫描容器映像以发现漏洞。Insights 还为各种 Kubernetes 环境提供 Kubernetes 监控，并利用开源 Kubernetes 工具，作为其对云原生计算基金会(CNCF)社区承诺的一部分。我们收到了大量反馈，并对我们解决的问题进行了验证:

> 对“我这样做对吗？”提供可行的答案面向平台工程师、开发人员和站点可靠性工程师。

随着公司采用 DevOps 来缩短交付新软件和服务的上市时间，现实是该过程永远不会像希望或想象的那样无缝。遗漏的步骤会导致错误配置，从而导致安全漏洞、成本低效率以及不可靠的应用和服务。虽然这种构建和运行应用程序的新方式能够实现更快的配置和自助服务，但它也要求您的监控和可观察性工具和策略发生变化。

我们的 Fairwinds 站点可靠性工程(SRE)团队在整个 Kubernetes 社区中一直看到同样的问题，并希望解决它们。这正是我们创建许多开源项目的原因:

*   Polaris，一个验证和修复 Kubernetes 资源的开源策略引擎

*   Goldilocks，一个帮助你获得资源请求和限制的实用程序

*   [Nova](https://nova.docs.fairwinds.com/) ，这是一个 CLI，它可以交叉检查导航图表，以定位您的 Kubernetes 集群中过时和不推荐使用的图表

*   [布鲁托](https://pluto.docs.fairwinds.com/) ，在基础设施即代码(IaC)存储库和 Helm 版本中定位已弃用的 Kubernetes API 版本，提供在 Kubernetes API 服务器中不可用的版本信息

*   [RBAC 经理](https://rbac-manager.docs.fairwinds.com/) ，一个支持基于角色的访问控制(RBAC)的声明性配置的操作员，具有新的定制资源，以简化 Kubernetes 中的授权

我们维护这些开源项目，并邀请合作来改进我们的开源工具，并随着需求的变化和 Kubernetes 的不断成熟添加功能。这些工具都是 Insights 提供的价值的一部分，我们将其与其他开源工具相结合，如开源漏洞扫描器 [Trivy](https://www.aquasec.com/products/trivy/) ，以及提供指标和警报的开源监控解决方案 Prometheus。Insights 使用收集的所有数据来提供一个 Kubernetes 仪表盘，显示 CPU 和内存的总使用情况。

Fairwinds Insights 使用 YAML 文件轻松部署，并贯穿整个开发生命周期。它包括与当今一些最普遍的解决方案的集成，例如:

*   一个松散的集成，以确保用户得到关于他们的 Kubernetes 集群的关键变化的通知

*   从 Insights 输入数据的 Datadog 集成，每当发现或修复一个行动项目时，就会创建一个事件，以帮助团队将问题与尝试的修复关联起来。

*   使用自动化规则为行动项事件创建事件的 PagerDuty 集成

*   吉拉集成允许 Insights 用户手动或使用自动化规则从行动项目创建吉拉票证

*   Azure devo PS 集成允许用户从操作项创建工作项

通过将我们的专业知识与值得信赖的开源工具相结合，Fairwinds Insights 发现了开发、安全和运营之间交接过程中遗漏的步骤，并使用自动化来设置护栏，以保持 Kubernetes 平稳高效地运行。借助[fair winds Insights](https://www.fairwinds.com/insights)，平台工程师、DevOps 团队、开发人员和站点可靠性工程师现在拥有了一个 Kubernetes 管理平台，可以帮助他们以安全、高效和可靠的方式改进他们的云原生工作流。