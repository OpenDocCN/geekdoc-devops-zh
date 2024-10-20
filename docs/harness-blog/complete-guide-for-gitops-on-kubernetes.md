# Kubernetes | Harness | Harness 上的 GitOps 完整指南

> 原文：<https://www.harness.io/blog/complete-guide-for-gitops-on-kubernetes>

让我们深入了解 GitOps 和 Kubernetes 如何支持持续交付(CD)和集成(CI)的基础知识。

几年前，GitOps 开始只是一个萌芽的工作框架，但后来已经成为一个行业的必需品。它在 2017-2018 年成为供应商、生态系统和社区的部署流程。

另一方面，Kubernetes 展示了为什么我们必须继续投资于开源技术。作为现代企业 IT 世界不可或缺的一部分，Kubernetes 已经发展成为大规模执行基于容器的云原生应用程序的有效标准。它的开源使得它对云原生计算至关重要。

今天，我们将看看 GitOps 和 Kubernetes 如何像饼干和奶油一样，作为持续交付(CD)和集成(CI)的一部分。

## 了解 GitOps

让我们从一个快速的 [GitOps 介绍](https://harness.io/blog/devops/what-is-gitops/)开始。从它的名字，我们已经知道 GitOps 包含了 Git 和 operations。Google 是这样定义的:“GitOps 是一个运营框架，它将 DevOps 基础设施自动化的最佳实践(包括版本控制、协作、合规性和 CI/CD)与那些用于应用程序开发的实践联系起来。”

对 GitOps 原则的需求始于责任和自动化。它使用自动化、软件代理和单一的事实来源来发现已部署的内容和源代码控制中的内容之间的异常。Git 存储库存储声明性的系统状态，从而使监控和记录更容易，因为它将开发和操作任务集中在部署管道的安全性和合规性上。GitOps 最佳实践在许多开发团队中被广泛接受，尤其是在云基础设施方面。

## 让我们讨论 DevOps 和实现 GitOps

GitOps 通常被认为是 DevOps 的子集，而实际上它是非常不同的。GitOps 处理与软件 Git 相关的特定系统操作程序。尽管 GitOps 和 DevOps 确实有一些共同的原则和目标，但也存在一些显著的差异:

总结一下这个比较，GitOps 是一个革命性的实践，它是在采用 DevOps 时发展起来的。GitOps 和 DevOps 不一定是齐头并进的。不使用 GitOps 原则也可以有 DevOps 文化，但不能反过来。尽管它的基本原则与 DevOps 没有什么不同，但 GitOps 提供了几个连续部署元素的实际应用。

## GitOps Kubernetes 的作用和优势

GitOps 为企业提供了他们想要的保证，同时将代码从 Git 存储库部署到生产 Kubernetes 集群。此外，它简化了 Kubernetes 和容器的复杂性。多年来，许多组织在考虑了 GitOps 的好处后，简化了他们的 DevOps 实践。

首先，我们来了解一个版本控制系统。源代码控制，或版本控制，被定义为管理和跟踪软件代码变更的实践。GitOps 将运行时环境中的每一个软件或云基础设施视为版本控制系统中的一个或多个文件，程序会自动运行以协调版本控制和运行时环境的状态。Kubernetes 作为一个编排平台，在帮助 GitOps 实现这一目标方面起着至关重要的作用，支持您团队的拉动请求。

在 GitOps 中使用软件代理可以警告 Git 和 Kubernetes 集群中运行的东西之间的任何差异。此外，如果存在差异，Kubernetes 协调器会根据具体情况自动更新或回滚集群。这种方法特别适合 Kubernetes，因为它建议的设置过程是声明性的(使用 YAML 清单)。

## kublers gitops 检查清单

### 1.忽必烈忽必烈忽必烈忽必烈忽必烈忽必烈忽必烈忽必烈忽必烈忽必烈

采用集装箱和基础设施管理的核心技术是必须的。这有助于组织高效地管理工作流，加速应用程序开发，并更快地满足市场需求。除了协调容器，Kubernetes 还可以管理硬件，并为管理数据提供基本的中间件元素。

### 2.团队知识和文化

为了在组织中增加 devo PS/gitop 的采用，开发团队必须意识到并愿意适应新的知识和前进的方向。给团队足够的空间进行实验、尝试、错误和培训，以理解新的工作流、工具和技术。

### 3.GitOps 管道

请注意这里的三个关键因素。首先，不是 CI 系统进行部署。第二，要在 Kubernetes 集群([亚马逊弹性 Kubernetes 服务(亚马逊 EKS)](https://aws.amazon.com/eks/) 、裸机、OpenShift 等部署一个 GitOps 代理(比如 Harness 连续部署)。).最后但同样重要的是，我们引入了一个配置存储库，它将声明我们希望环境处于的状态。

### 4.交付

在软件交付管道中实现和维护安全性、合规性和操作性最佳实践或法规的过程被称为可信交付。这些障碍是利用策略即代码来实现的，并且在软件开发生命周期中采用了编码策略的形式。

## GitOps Kubernetes 操作员查看线束连续交付

在 Harness CD-as-a-service 平台的帮助下，工程和开发团队现在可以更快、更安全地发布应用程序。Harness 除了发布 CI 平台之外，还发布了 CD 开源版本，这两个版本目前都可以在开源许可下免费下载和访问。此外，它现在是免费的，可以通过源代码许可获得。开发人员可以使用 Harness 在他们选择的任何公共或私有云架构上部署、验证和自动回滚 Kubernetes 和其他云原生应用，Harness 是一种支持[最新 CI/CD 最佳实践](https://harness.io/blog/continuous-delivery/ci-cd-best-practices/)的尖端自助服务解决方案。

Harness CD 旨在将您的修改安全地发布到您的生产环境中。采用重新创建的方法包括杀死所有的豆荚并替换它们，而不是用滚动策略逐步完成。Kubernetes 可能部署得非常快，但这会导致停机。因为我们大多数人都有持续的工作负载，任何停机时间都会适得其反。由于 Kubernetes 的即时性，拒绝尽快部署似乎不合逻辑，但这是信心所必需的。

## 使用 Kubernetes 上的 GitOps 将开发提升到一个新的水平

GitOps 是行业中一个新的有前途的舞台，它使 CI/CD 对我们的开发人员和架构师来说变得简单而不费力。随着 IaC 的崛起，其逻辑演变为 GitOps，优先考虑 Git 作为真理的单一来源。由于 Git 和目标系统之间完美的协调，消除了 CI/CD 管道和授权过程的巨大复杂性。这个声明性的目标系统是 GitOps 最关键的需求，因为没有它，实践只能部分实现。这就是 GitOps for Kubernetes 介入并帮助解决这个难题的地方。

现在，100%的线束 CI/CD 源代码都可以通过开放源代码轻松访问。没有许可证或使用费，用户可以快速、轻松地开始使用 Harness CI/CD 来构建敏捷的生产环境。一种简单的升级方法是扩展到更多的企业级服务和 SaaS 计划。要了解更多关于 Harness 如何提升您的持续部署以及 Kubernetes 、[的](https://harness.io/company/contact-sales?utm_source=Website&utm_medium=Mutiny&utm_campaign=Mutiny-Nav-Contact-Sales-vs-Get-Pricing&utm_content=Get-Pricing)[未来，请注册](https://harness.io/blog/continuous-delivery/future-of-kubernetes/)了解更多！