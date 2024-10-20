# 如何保护您的 Kubernetes 工作负载|利用

> 原文：<https://www.harness.io/blog/how-secure-kubernetes-workloads>

了解如何保护您的 Kubernetes 集群和应用免受潜在威胁，并确保您的数据保持安全。

如今，DevOps 和云原生方法已经成为组织的标准实践。因此，软件开发人员越来越希望利用这些更新的计算模式来加快上市时间，同时保持高可用性并降低成本。Kubernetes 就是这样一个平台，组织希望采用它来加速软件交付。

尽管 Kubernetes 功能强大，但在没有适当安全措施的情况下采用 Kubernetes 会增加您的网络攻击风险。容器服务的分布式本质使您的环境暴露于不断增加的攻击媒介。然而，通过适当的安全协议和最佳实践，您可以保护您的 [Kubernetes 集群](https://www.harness.io/blog/kubernetes-cluster-deployment)和应用程序免受这些潜在威胁，并确保您的所有数据保持安全和机密。

## Kubernetes 安全性的重要性

ionir 的一项调查结果显示，大约 60%的受访者在 Kubernetes 上运行有状态应用程序，50%的受访者表示他们计划在未来 12 个月内这样做。此外，Red Hat 的[2022 年 Kubernetes 安全状况](https://www.redhat.com/en/blog/state-kubernetes-security-2022-1)调查报告称，78%的受访者选择 DevsecOps 作为他们部署 Kubernetes 的主要关注点。

在深入 Kubernetes 安全性的细节之前，让我们先看看这些安全协议背后的概念。Kubernetes 之所以如此安全，是因为它基于与主机操作系统完全隔离的 Linux 容器。这意味着如果攻击者要破坏 Kubernetes 集群，他们只能访问容器，而不能访问主机操作系统的其余部分。由于这种隔离，黑客需要单独破坏集群中运行的每个容器才能访问或修改敏感数据。这使得攻击者在未经授权的情况下访问 Kubernetes 集群中的数据变得更加困难。

虽然 Kubernetes 为现代组织提供了许多好处，但重要的是要记住它也是一种软件产品，这意味着它容易受到网络安全威胁。此外，由于 Kubernetes 是开源的，它可能比商业软件更容易受到安全风险的攻击。因此，在您的环境中引入 Kubernetes 之前，您必须确保您有一个安全策略来保护您的数据免受网络威胁。否则，攻击者可能会未经授权访问您的敏感数据并中断您的业务流程，从而导致重大的财务和声誉损失。

## Kubernetes 安全最佳实践

以下是一些确保 Kubernetes 集群安全的最佳实践。

### 使用可信容器映像

首先，确保您使用的是可信映像，而不是从头开始构建映像。相反，使用可信的开源图像。这降低了在容器中引入恶意代码注入的风险。此外，在配置容器时，应该遵循最小特权原则。这意味着你不应该给你的容器比他们实际需要更多的访问权限。这样做增加了违规的风险，因为恶意行为者如果没有权限，就无法获得对操作系统的完全访问权限。

### 了解集群管理和用户角色

您需要知道如何减轻集群管理。这是保护集群的关键的第一步。许多组织选择为 Kubernetes 中需要集群特权的部分创建服务帐户。本质上，您为 Kubernetes 中需要集群范围特权的每个组件创建一个特殊的用户帐户。

Kubernetes 有两种类型的用户:

*   集群用户用于集群的日常管理。他们可以创建 pod 和服务，但无权修改 Kubernetes API。
*   经过身份验证的用户使用 Kubernetes API 进行身份验证，并且拥有对 Kubernetes API 的完全访问权限。

### 监控您的 Kubernetes 工作负载

您必须持续监控您的 Kubernetes 工作负载和环境，以防恶意活动。这包括监控以下安全威胁:

*   **对未授权用户的访问**:您应该能够监控访问您的 Kubernetes 集群的用户的 IP 地址。理想情况下，这应该扩展到容器本身。这允许您查看访问您的容器的人是否是授权用户。
*   **API 滥用**:如果您正在使用 Kubernetes 的 API，您应该监控 API 调用，以确保恶意行为者不会滥用您的 API 来危害您的 Kubernetes 集群。这可能包括监视来自未授权 IP 地址的 API 调用、失败的 API 调用或比预期时间长的 API 调用。
*   **数据泄露**:您还应该监控 Kubernetes 集群中的数据泄露。这可能包括监控异常的文件活动、对敏感数据的意外和随机访问或异常的网络流量。

### 将 Kubernetes 安全工具集成到 CI/CD 管道中

Kubernetes 的安全工具/平台(如[kubiscape](https://github.com/kubescape/kubescape)、 [Datree](https://www.datree.io/) 、 [Trivy](https://aquasecurity.github.io/trivy/v0.17.2/) 等。)可以帮助您找到 yaml 文件和集群中的安全问题和漏洞。开发人员需要一种方法将这些工具集成到 CI/CD 管道中。

### 使用 GitOps

GitOps 可以轻松解决任何安全问题。使用 Git 作为主要工具，一切都可以追溯，这使得观察变得容易。将 CI/CD 工具链与 GitOps 方法结合使用，对于确保 Kubernetes 部署的安全性和在整个组织中维护标准方法至关重要。让我们深入学习如何实现这一点的教程。

## **如何**使用 Harness 在您的管道中配置 Kubernetes 安全性

在本教程中，我们将考虑三款 Kubernetes 安全工具，分别是 [Kubescape](https://github.com/kubescape/kubescape) 、 [Datree](https://www.datree.io/) 和 [Trivy](https://aquasecurity.github.io/trivy/v0.17.2/) 。我们将使用 Harness 在部署管道中配置这些工具。

首先，从 Harness 获得您的免费帐户，并使用 [Harness CD](https://app.harness.io/auth/#/signup/?module=cd?utm_source=internal&utm_medium=social&utm_campaign=community&utm_content=microservices-article-harness&utm_term=get-started) 模块设置一个简单的 CD 管道。

设置所需的连接器:

下一步是创建一个管道，如下所示。我们将检查工作负载是否有任何错误配置和错误。如果在 Kubernetes 工作负载中发现任何错误，我们将不会继续部署应用程序。在部署阶段，三个 Kubernetes 安全扫描被添加为使用 shell 脚本实用程序的步骤。

一旦集成了所有三个(Kubernetes、Datree、Trivy) Kubernetes 扫描工具，CD 管道看起来就像这样:

保存配置并运行管道。您将看到这些 Kubernetes 安全扫描器并行执行。

不要担心部署失败(如上图所示)。它将失败，因为在使用的 YAML 文件中有一些错误配置(Datree 检测到它，因此管道失败)。这个失败是本教程的目的，演示真正的失败会发生什么，以及如何识别它。

### 管道检测:Kubescape vs. Datree vs. Trivy

让我们看看每个工具做什么，它在管道中检测什么。

**Kubescape** 扫描集群并展示不符合安全标准的 YAML 文件。

‍ **Datree** 显示我们的 Kubernetes 清单文件中是否有任何错误配置。

‍ **特里维**扫描我们 Kubernetes 工作负载中的漏洞。

使用 Harness，所有三款 Kubernetes 安全扫描仪都可以轻松集成到您的管道中。

## 用安全带固定 Kubernetes

Kubernetes 是一个强大的容器服务，可以帮助组织加快新应用程序和服务的交付。然而，尽管这种技术提供了许多好处，但由于其分布式体系结构，它也增加了攻击的风险。为了最大限度地降低这种风险，您必须确保正确配置您的容器和工作负载，并监控您的集群是否有恶意活动。

随着 Kubernetes 部署越来越受欢迎，组织正在雇用 DevOps 专业人员来使用 DevOps 原则加速他们的软件交付过程。但是我们需要记住一件事:这不仅仅是速度的问题，还有安全问题。使用 Kubernetes 安全扫描器和漏洞检测器将帮助您领先网络犯罪分子一步，并确保您的 Kubernetes 相关工作负载保持安全。

有兴趣了解 Harness 如何保护您的 Kubernetes 部署吗？今天就免费试用 Harness CD 吧！