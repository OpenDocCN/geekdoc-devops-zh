# 用于线束连续输送的舵支架|线束

> 原文：<https://www.harness.io/blog/helm-support-for-harness-continuous-delivery>

我们已经升级了我们的头盔集成，以提供金丝雀和蓝绿色部署，自动回滚，等等！现在就去看看。

Helm 已经成为一种广泛流行的打包、分发和管理 Kubernetes 应用程序的方式。在 Harness，我们看到越来越多的客户已经在使用 Helm，它的用途多种多样。在本帖中，我们将讨论 Harness 如何为打包为 Helm Charts 的微服务/应用提供一流的[持续交付解决方案](https://harness.io/products/continuous-delivery/)，并回顾我们为提供最佳 Helm 支持所做的增强。

## 舵图的淡黄色和蓝绿色部署

虽然 Helm 能够很好地将多个 Kubernetes 资源打包到 Kubernetes 应用程序中，但它在部署和回滚方面提供的功能有限。Helm 项目的目标不是解决持续交付，实施像 [Canary 和 Blue-Green](https://harness.io/blog/blue-green-canary-deployment-strategies/) 这样的高级部署策略并不简单。在许多情况下，我们看到人们使用聪明的模板技巧来实现它们。这通常需要手动使用不同的值文件运行 helm CLI。

在 Harness 平台中，我们为金丝雀和蓝绿色部署策略构建了一流的支持。我们还会进行部署状态检查，如果需要，还会进行自动回滚。我们的实现非常灵活，任何一组 Kubernetes 资源都可以作为服务的一部分进行部署。通过我们的舵集成，我们旨在为今天已经使用舵图表的客户带来相同的功能集。

下图说明了 canary 部署策略。Harness workflow 创建了一个并行 canary 部署。当 canary 部署通过所有验证时，主部署将升级。实现这一点不需要改变规格。

Figure 1: Harness Orchestrated Canary Deployment

Figure 2: Example Run of Canary Workflow

同样，下图说明了蓝绿策略。线束工作流创建两个并行部署(蓝色和绿色插槽)。服务对象(即主要服务)用于跟踪哪个部署服务于生产流量。新服务被部署到另一个插槽中。一旦所有测试都通过了，就可以通过更新服务将生产流量路由到新的 Pods。在任何时候，服务都可以更新为指向一个较旧的版本，以实现即时回滚。

Figure 3: Harness Orchestrated Blue-Green Deployment

在我们的方法中，我们使用 Helm 从存储库和模板渲染中获取图表。一旦我们将 Helm chart 渲染到 Kubernetes 资源中，我们就可以像上面描述的那样编排一个金丝雀或蓝绿色的部署，并在出现故障时自动回滚。所有这些都不需要改变微服务的图表规格。

## 使用 Helm 跨环境部署服务

使用 Helm 模板，很容易将图表部署到多个环境中。特定于环境的配置保存在值覆盖文件中。尽管如此，仍然需要管理特定于环境的秘密和集群配置，例如 Kubeconfig。当您扩展到更多环境时，跟踪哪些覆盖了哪些群集变得非常困难。

Harness 提供了环境抽象，所有这些都可以在环境资源中进行组织。当服务被部署到特定环境时，正确的集群具有正确的配置和值覆盖。

Figure 4: Harness Environment Encapsulates Cluster Details & Configuration Specific to an Environment

## 舵部署的自动回滚

Helm 安装/升级命令不跟踪展开的状态。在头盔升级完成后，仍然需要手动跟踪展示的状态。回滚也是手动操作。线束跟踪部署的部署状态，并可以在需要时自动回滚。

## 其他头盔增强功能

一些客户在 Git 存储库中维护他们的 Helm 图表，但是发现维护一个单独的 Helm 存储库很麻烦。许多人使用亚马逊 S3 和 GCS 桶来存储图表包。

许多人只使用 Helm 进行打包和模板化，而其他人则利用 Helm 客户端来管理版本的安装/升级/回滚。我们从一些客户那里了解到他们在使用 tiller 时遇到的挑战，他们更喜欢使用客户端模式。

基于这些经验，我们改进了舵面集成的以下方面:

### 舵库连接器

我们已经为头盔库添加了一个连接器。HTTP 服务器、亚马逊 S3 和基于 Google 云存储的存储库都支持开箱即用。

### 来自源存储库的舵图

舵图可以直接从 Git 存储库中的图表源获取。这避免了维护单独的 Helm 存储库服务器的开销。

### 远程值覆盖

服务和环境级别的值覆盖现在可以存储在远程 Git 存储库中。这使得 GitOps 流和环境级别覆盖可以保存在与图表相同的存储库中。

### 支持仅客户端使用头盔

我们增加了对头盔客户端模式的支持。许多人更喜欢客户端模式，因为 tiller 在设置、RBAC 和可用性方面带来了挑战。在这种模式下，Helm client 用于从存储库中获取图表并呈现模板。部署是通过标准的 kubectl 机制完成的。这避免了对舵柄的依赖。

我们希望这些增强功能对您有用。

问候，
安舒尔，瓦伊巴夫，伊桑特，文卡特什&普内特