# Docker Swarm vs Kubernetes:如何选择容器编排工具

> 原文：<https://circleci.com/blog/docker-swarm-vs-kubernetes/>

世界各地的企业越来越依赖容器技术的好处来减轻部署和管理复杂应用程序的负担。**容器**将所有必需的依赖项组合在一个包中。它们可移植、快速、安全、可扩展且易于管理，是传统虚拟机的首选。但是要扩展容器，你需要一个**容器编排**工具——一个管理多个容器的框架。

今天，最著名的容器编排平台是 Docker Swarm 和 T2 Kubernetes。它们都有优点和缺点，并且都有特定的用途。在本文中，我们将研究这两种工具，以帮助您确定哪种容器编排工具最适合您的组织。

## 码头工人群

Docker Swarm 是一个由 Docker 构建和维护的开源容器编排平台。在幕后，Docker Swarm 将多个 Docker 实例转换成一个虚拟主机。Docker Swarm 集群通常包含三个项目:

1.  节点
2.  服务和任务
3.  负载平衡器

节点是 Docker 引擎的单个实例，它控制您的集群并管理用于运行服务和任务的容器。Docker Swarm 集群还包括负载平衡，以便在节点间路由请求。

### Docker Swarm 的优势

Docker Swam 安装起来很简单，尤其是对于那些刚刚进入容器编排领域的人来说。它重量轻，易于使用。此外，Docker Swarm 比更复杂的编排工具更容易理解。它在 Docker 容器中提供自动负载平衡，而其他容器编排工具需要手动操作。

Docker Swarm 与 Docker CLI 配合使用，因此无需运行或安装整个新的 CLI。此外，它可以与现有的 Docker 工具(如 Docker Compose)无缝协作。

如果你的系统已经在 Docker 中运行，Docker Swarm 不需要改变配置。

### Docker Swarm 的缺点

尽管有好处，使用 Docker Swarm 也有一些你应该知道的缺点。

首先，与 Kubernetes 相比，它是轻量级的，并且绑定到 Docker API，这限制了 Docker Swarm 中的功能。同样，Docker Swarm 的自动化功能也不像 Kubernetes 那样强大。

## 库伯内特斯

Kubernetes 是一个开源的容器编排平台，最初由 Google 设计来管理他们的容器。Kubernetes 拥有比 Docker Swarm 更复杂的集群结构。它通常有一个构建器和工作节点架构，进一步分为 pod、名称空间、配置映射等等。

### Kubernetes 的优势

Kubernetes 为寻找健壮的容器编排工具的团队提供了广泛的好处:

*   它有一个庞大的开源社区，谷歌也支持它。
*   它支持所有的操作系统。
*   它可以支持和管理大型架构和复杂的工作负载。
*   它是自动化的，并且具有支持自动扩展的自我修复能力。
*   它具有内置的监控和广泛的可用集成。
*   它由三个主要的云提供商提供:Google、Azure 和 AWS。

由于其广泛的社区支持和处理最复杂部署场景的能力，Kubernetes 通常是管理基于微服务的应用程序的企业开发团队的首选。

### Kubernetes 的缺点

尽管 Kubernetes 功能全面，但它也有一些缺点:

*   它有一个复杂的安装过程和陡峭的学习曲线。
*   它要求您安装单独的 CLI 工具并学习每一个工具。
*   从 Docker Swarm 到 Kubernetes 的过渡可能很复杂，也很难管理。

在某些情况下，Kubernetes 可能过于复杂，导致生产力的损失。

## Docker Swarm vs Kubernetes:异同

到目前为止，我们已经讨论了每个平台的优点和缺点。现在，让我们来分析它们的主要区别和相似之处。我们将从设置要求、应用部署能力、可用性和可扩展性、监控功能、安全性和负载平衡等方面对这两个平台进行比较。

### 安装、配置和学习曲线

与 Kubernetes 相比，Docker Swarm 易于安装，并且实例通常在整个操作系统中保持一致。在 Docker Swarm 中配置集群比配置 Kubernetes 更容易。与它的对应物相比，它很容易学习，并且可以与现有的 CLI 一起工作。

与 Docker Swarm 相比，Kubernetes 的安装更复杂，需要手动操作。每个操作系统的安装说明可能会有所不同。学习起来很有挑战性，而且有单独的 CLI 工具。

### 应用程序部署

Docker Swarm 应用程序是您可以使用 YAML 文件或 Docker Compose 部署的服务或微服务。

Kubernetes 提供了更广泛的选项，比如名称空间、pod 和部署的组合。

### 可用性和可扩展性

Docker Swarm 提供高可用性，因为您可以轻松复制 Docker Swarm 中的微服务。而且 Docker Swarm 部署时间更快。另一方面，它不提供自动缩放。

Kubernetes 天生具有高可用性、容错性和自我修复能力。它还提供自动缩放功能，并可以在需要时更换有故障的 pod。

### 监视

Docker Swarm 只支持通过第三方应用进行监控。没有内置的监测机制。

相比之下，Kubernetes 具有内置的监控功能，并支持与第三方监控工具的集成。

### 安全性

Docker Swarm 依靠传输层安全(TLS)来执行安全和访问控制相关的任务。

Kubernetes 支持多种安全协议，如 RBAC、SSL/TLS、秘密管理、策略等等。

### 负载平衡

Docker Swarm 支持自动负载平衡，并在幕后使用 DNS。

Kubernetes 没有自动负载平衡机制。但是，Nginx Ingress 可以充当集群中每个服务的负载平衡器。

## K3s 作为替代产品

总之，两个平台的主要区别是 Docker Swarm 是轻量级的，更适合初学者，而 Kubernetes 是笨重而复杂的。寻找中间地带的开发者可能会考虑一个新的平台， [K3s](https://k3s.io/) 。K3s 消除了 Kubernetes 的复杂性，并提供了一种更轻松、更容易获得的体验。

K3s 是一个微小的二进制文件，实现了完整的 Kubernetes API。它很小，因为二进制文件没有不必要的包。您可以使用第三方加载项快速添加功能。它是轻量级的，易于使用，并且通过了云本地计算基金会(CNCF)的认证。

K3s 对 Docker Swarm 用户更友好，他们不确定自己是否准备好了全面使用 Kubernetes。它可能是笨重的 Kubernetes 的理想替代品。

## 你应该使用哪个平台？

Kubernetes 和 Docker Swarm 都服务于特定的用例。哪一种最适合您取决于您组织的需求。

对于初学者来说，Docker Swarm 是一个易于使用的简单解决方案，可以大规模管理您的容器。如果您的公司正在转向容器领域，并且没有复杂的工作负载需要管理，那么 Docker Swarm 是正确的选择。

如果您的应用程序很复杂，并且您正在寻找一个完整的包，包括监控、安全特性、自我修复、高可用性和绝对的灵活性，那么 Kubernetes 是正确的选择。

如果你需要 Kubernetes 的所有功能，但是被它的学习曲线所阻碍，那么 K3s 是一个不错的选择。

## 结论

在本文中，我们探讨了容器世界的两个主要协调者，Kubernetes 和 Docker Swarm。与 Kubernetes 相比，Docker Swarm 是一个轻量级、易于使用的编排工具，提供的服务有限。相比之下，Kubernetes 虽然复杂但功能强大，并且提供开箱即用的自我修复、自动伸缩功能。K3s 是 CNCF 认证的 Kubernetes 的一个轻量级版本，如果你想获得 Kubernetes 的好处而又不需要额外的学习开销，它可能是一个正确的选择。

哪种编排工具最适合您取决于您的业务需求。在做出选择之前，仔细考虑你的团队的目标和经验。无论您选择哪种平台，您都能够很好地扩展和管理您的容器化应用程序。