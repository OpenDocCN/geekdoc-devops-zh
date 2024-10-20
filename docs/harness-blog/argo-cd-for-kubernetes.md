# Kubernetes 的声明式连续交付:Argo CD | Harness 的详细介绍

> 原文：<https://www.harness.io/blog/argo-cd-for-kubernetes>

在本帖中，我们将仔细看看 Argo CD 的功能，这是一个用于 Kubernetes 的声明式连续交付系统。

Argo CD 是一个[连续部署](https://harness.io/blog/continuous-delivery-vs-continuous-deployment/)解决方案，为 Kubernetes 提供第一支持。它遵循基于 Git 的工作流来自动部署 Kubernetes 中的服务和应用程序。在本帖中，我们将仔细看看这个用于 Kubernetes 的声明式连续交付系统的功能，并分享我们对 Argo CD 的想法。

## **阿尔戈 CD 的起源**

Argo CD 于 2018 年宣布成为 Argo 社区和 Intuit 的开源项目。当 Spinnaker 等解决方案无法支持 Kubernetes 本地工作流时，它作为 Kubernetes 社区的连续部署解决方案被引入。Argo CD 基于 [GitOps](https://harness.io/blog/what-is-gitops/) 模型工作，其中 git 存储库是了解 Kubernetes 基础设施状态的唯一来源。

这就是所谓的拉模型，解决方案根据 Git 中定义的配置监控和更新您的 Kubernetes 资源。在推模型中，用户从外部系统或服务触发事件。Argo CD 可以支持基于拉和推的模型，以使您的目标环境与您期望的应用程序状态同步。一个常见的用例是使用 CI 管道来触发 CD 管道。

Argo CD 旨在成为“实现连续运行”的第一步，其基础是基于声明性规范和自动学习/系统行为自动分类的运行问题的监控、分析和自动补救。“其他有助于支持这项任务的 Argo 项目是 Argo Workflows[https://github.com/argoproj/argo](https://github.com/argoproj/argo)，它现在是云本地计算基金会(CNCF)下的一个孵化项目，用于在 Kubernetes 上编排并行作业。

## 【Argo CD 是做什么的？

Argo CD 允许用户控制和编排他们定义的 Kubernetes 清单。

Kubernetes 清单文件可以通过以下方式指定:

*   [kustomize](https://kustomize.io) 应用
*   [舵图](https://helm.sh)图
*   [ksonnet](https://ksonnet.io) 应用
*   [jsonnet](https://jsonnet.org) 文件
*   YAML/JSON 清单的普通目录
*   或者任何其他允许 Kubernetes 配置的管理工具和格式。

除了这些解决方案，Argo CD 还自动将这些定义交付到目标环境中。它很好地做到了这一点，Argo CD 的整体使用为团队和开发人员提供了快速部署的快捷方法。

虽然 Argo CD 有许多功能，但其中一些功能更适合企业使用。

## **顶级 Argo CD 功能**

作为一种持续部署解决方案，Argo CD 的一些最佳功能包括:

### **1。将应用程序自动部署到指定的目标环境。**

Agro CD 允许用户利用他们的工具对特定的 Kubernetes 环境进行任何必要的修改，比如舵图或应用程序部署。这对管理多个 Kubernetes 资源的组织来说非常好。本文对 Argo CD 资源进行了说明:[https://argoproj . github . io/Argo-CD/operator-manual/declarative-setup/](https://argoproj.github.io/argo-cd/operator-manual/declarative-setup/)。

Argo CD 也为这些部署提供了跟踪策略:[https://Argo proj . github . io/Argo-CD/user-guide/tracking _ strategies/](https://argoproj.github.io/argo-cd/user-guide/tracking_strategies/)。这是 Argo CD 入门时的示例截图。自动同步功能提供有关部署的状态和附加信息。

## **2。应用程序资源的健康状态分析。**

Argo 提供了有关部署健康状态的更多详细信息。还可以自动同步到您的 git 存储库，如下所示。

Argo CD 还具有自动配置漂移检测和可视化。漂移检测基于 kubectl diff 中的相同逻辑。如果实时 Kubernetes 清单文件和 git 中的真实来源清单文件之间存在差异，则部署不同步。我推荐这个资源来阅读更多关于不同步状态以及如何使用它的信息:[https://argoproj.github.io/argo-cd/user-guide/sync-options/](https://argoproj.github.io/argo-cd/user-guide/sync-options/)。如果您的环境或应用程序架构经常变化，这可能会有所帮助。

### **3。一个 web 用户界面(UI ),提供应用程序活动的实时视图。**

今天，大多数交付平台要么有 UI，要么正在引入 UI。了解应用程序的实时状态和活动可以提高团队的可见性，并确保与部署相关的每个人都能够做出响应。

## **Argo CD 顶级挑战**

采用适合您组织需求和使用案例的解决方案有助于加快您的上市时间。然而，评估往往不是直截了当的，事后诸葛亮往往是 20/20。这里有一些挑战，你可能会遇到阿尔戈光盘。

### **1。管理解决方案本身。**

开源解决方案提供了快速入门的方法。工程师可以下载并安装该解决方案的实例，从而加快交付所需的必要流程和技术概念的学习曲线。但是，随着组织、过程和技术的不断变化，这种模型通常不适合软件交付解决方案。一些关键的挑战可能包括缩放和引导 Argo 光盘。每个 Kubernetes 集群中都需要安装 Argo CD。更新和安装也是自我管理的，团队需要专门的能力来支持解决方案。

### **2。防呆变化。**

我们如何扩展软件交付的一部分是关于我们如何管理访问、许可和秘密。团队最不需要的就是未经授权的用户对生产环境进行更改或引入重大更改。Argo 光盘用户在保护其环境方面可能面临挑战。迄今为止，还没有内置的 Argo CD 支持加密秘密。讨论见 [#1364](https://github.com/argoproj/argo-cd/issues/1364) 。RBAC 和单点登录也没有关联，因此解决方案的管理员必须控制访问和定义用户组，而不利用单点登录提供商提供的用户数据。

### **3。满足连续交货的需要。**

对于许多组织来说，我们想要做的不仅仅是允许改变。站点可靠性工程、DevOps 和 CI/CD 都是为了支持连续的软件生命周期而存在的，并且我们的工作负载通常可以分布在不涉及 Kubernetes 和容器的其他平台和技术上。Argo CD 不支持非 Kubernetes 工作负载。用户也可以在应用程序或服务部署后，通过自动化所需的责任和需求来进行挑战。这包括验证和事件管理。在 Argo CD 中，回滚是手动的，并且可以审计 Argo 中所做的更改。由于 Argo 在 GitOps 模型上工作，所有的更改都提交给 git 存储库。这意味着您处理事件的行动手册可能需要包括额外的支持和集成，以自动化许多部署后活动。

## **判决结果**

Argo CD 有它的功能。虽然它为开发人员提供了无缝体验，但如果要作为交付解决方案进行扩展，它将需要更多功能来支持开发人员和运营团队。我乐观地认为，许多变化将有利于最终用户，特别是随着 Argo 社区的发展，他们将从如此众多的最终用户、开发者和贡献者那里得到指导。

如果您目前正在评估 Argo CD，请考虑您在软件交付方面最大的痛点。如果您有大量的集群，可能很难跨所有这些集群安装、管理、更新和配置 Argo CD 的实例。同样，如果您有大量的服务，可以考虑评估解决方案的水平伸缩性。我建议考虑将服务或工程师纳入解决方案的平均时间，以及是否有可能对工作流进行模板化。