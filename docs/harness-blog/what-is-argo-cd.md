# 什么是 ArgoCD？装具

> 原文：<https://www.harness.io/blog/what-is-argo-cd>

Argo CD 是一个开源的 GitOps 连续交付工具。它监控您的集群和存储在 Git 存储库中的声明定义的基础设施，并解决两者之间的差异——有效地自动化应用程序部署。

最简单地说，Argo CD 是一个开源的 GitOps 连续交付工具。它监控您的集群和存储在 Git 存储库中的声明定义的基础设施，并解决两者之间的差异——有效地自动化应用程序部署。

你可能也听说过它是一个 Kubernetes 控制器。Kubernetes 帮助改变了基础设施的管理方式，但它也通过引入新概念，如 kubernetes 清单、kubernetes 资源和其他各种 Kuberentes 特定的概念，增加了开发人员和系统管理员的复杂性。随着 GitOps 运动开始减少一些复杂性并将其抽象到自动化层之下，Argo CD(“CD”当然是“连续交付”的简写)是使这种自动化成为可能的主要工具之一。

没有 Argo CD，GitOps 仍然是“ClickOps”您的 CI/CD 工具可能能够推出基础架构变更，但是它不能监控差异，因此基础架构仍然主要是手动的。

在这篇文章中，我们解释了 Argo CD 是做什么的，它是如何工作的，以及如何开始使用它的一个例子。

## Argo CD 是做什么的？

Argo CD 是一个声明性的 GitOps 连续交付工具，由金融软件巨头 Intuit 的团队开发。迁移到公共云后，必须管理容器化工具和基础架构，这妨碍了 Intuit 实现云的最大优势的目标。Intuit 团队开发了 Argo CD，以实现最大的释放速度和速度增益。他们希望应用一个自动化面板来消除一些手动工作，因此他们创建了控制器——Argo CD——并对其进行了开源。

作为 Argo 项目的一部分，Argo CD 现在由社区维护[,并嵌入到许多 GitOps 工具中，这些工具作为 Kubernetes 本地持续部署的交付工具，您可能很熟悉。(Harness 自己的](https://argoproj.github.io/) [GitOps 解决方案](https://harness.io/products/continuous-delivery)就建立在它的基础上。)

## 为什么是 Argo CD？

作为一个 GitOps 连续交付工具，Argo CD 持续监控您正在运行的基础设施(实际状态)，将其与声明定义的代码(所需状态或目标状态)进行比较，以确定它们是否不同步，这有助于补救配置漂移。

Argo CD 自动将新配置和新版本代码部署到目标环境中。根据您如何配置 Argo CD 的工作，它会在新的 git 提交后通知您事情不同步，或者采取行动。如果您已经将它设置为自动执行更改，它将使用存储在不可变的版本化 Git 存储库中的内容覆盖生产配置。该工具非常适合复杂的应用程序部署。

此外，当您的开发团队在受版本控制的环境中工作时，Argo CD 是必不可少的。通过自动化生命周期管理和应用程序部署，Argo CD 可在应用程序进入您的生产环境之前，主动监控您的应用程序配置，以发现任何潜在的同步问题。

## Argo CD 核心概念

要设置并开始使用 [Argo CD 和 Kubernetes](https://harness.io/blog/argo-cd-for-kubernetes) ，您需要非常熟悉:

*   **集装箱**，虚拟机，可能还有集装箱化工具 Docker
*   像 Kubernetes 这样的容器编排系统(尽管有替代方案)
*   **持续部署和集成工具**如线束
*   **(可能)管理 Kubernetes 集群**并在 YAML、头盔图或 Kustomize 中显示

如果您熟悉上面的内容，您就会知道，为了减少调配基础架构所需的人工工作量，在容器中部署、编排这些部署并将这些部署抽象成代码是很有帮助的。这是 GitOps 具有声明性和版本控制的核心前提——将所有东西都转移到 Git repo 中，在那里您可以发挥源代码控制和自动化的全部力量。

Argo CD 提供了 Git 定义的基础设施和容器编排工具之间的实际监控和同步。它有五个主要组成部分:

**1。Argo 光盘用户界面(UI)**

在 web UI 中，您可以创建应用程序，尽管对自动化感兴趣的高级人员会想要更多的控制，并且会在 Git 中声明性地这样做。您还可以直接在 web UI 中管理连接的 Git 存储库、访问这些存储库的证书、您的集群和项目。(项目允许您构建应用程序，以围绕每个团队的工作创建有用的筒仓。)

Argo CD 界面的一个更有用的组件是，您可以查看基础设施部署的可视化管道，以及各种应用程序的服务和集群。如果应用程序在崩溃循环中[，这将让您看到所有事件，以及清单和配置值，因此您可以可视化部署问题并进行调试。(也会隐藏你的秘密。)](https://www.youtube.com/watch?v=2WSJF7d8dUg&ab_channel=ThatDevOpsGuy)

**2。API 和命令行界面(CLI)**

用户无需接触 Argocd 登录页面，因为您可以通过 API 管理 Argo CD 或创建 YAML 资源定义(或使用 Kustomize 或 Helm Charts 来帮助管理 Kubernetes 资源)。一旦设置完成，开发人员不需要理解他们正在部署的基础设施的所有复杂性。他们只需要将应用程序定义包含在他们的拉请求或合并请求中。

**3。自定义资源定义(CRD)**

Argo CD 在您的 Kubernetes(或类似的)集群中创建自己的名称空间(kubectl create namespace argocd)。在那里，它储存了阿尔戈 CD 光盘。

**4。储存库服务**

Argo CD 在本地缓存您的 Git repo 并存储应用程序清单文件。

**5。应用控制器**

一旦您将 Argo CD 应用程序控制器下载到您的存储库服务器上，它就可以调用由软件开发生命周期事件定义的钩子。(例如预同步、同步、后同步。)

要下载 Argo CD，请访问该项目的“[入门](https://argo-cd.readthedocs.io/en/stable/getting_started/?_gl=1*1gn4tve*_ga*MjA1OTU3MzMzLjE2NjcyNTk2NjI.*_ga_5Z1VTPDL73*MTY2OTA3NzEwNC40LjAuMTY2OTA3NzEyOC4wLjAuMA..)”页面。

## 使用 Argo CD 部署基础结构更新的示例

一旦您安装了 Argo CD 并配置了您的证书和集群，您就可以通过实施 Git 定义的版本将它设置为自动“部署”基础设施。

例如，以下情况成为可能:

1.  开发人员将资源更改推送到单个 Git 存储库
2.  持续集成工具被触发并将新的容器映像保存到注册表中
3.  拉请求(PR)改变 Kubernetes 清单，该清单被合并并触发 Argo CD
4.  Argo CD 克隆存储库。它将期望的配置或期望的状态与集群上的当前状态进行比较
5.  阿尔戈光盘自动协调这些差异
6.  Argo CD 报告同步完成的时间

反之亦然，这就是 Kubernetes 控制器的真正优势。如果 Kubernetes 集群资源失去同步，Argo CD 会检测到这一点，并通过应用 Git 中的内容来协调这些变化。如果您只是简单地使用 CD 工具或 Jenkins，同步问题将无法检测和解决。

## 采用 Argo 光盘前需要考虑的事项

Argo CD 是部署 Kubernetes 应用程序的直观解决方案。但是在你全力投入开源项目之前，你应该意识到它的一些缺点。治理不是一个强大的 ArgoCD 套件。基于角色的访问控制有限，而且几乎没有审计跟踪。如果您有一个严格的部署过程，并且您需要确保应用程序被正确部署，那么 GitOps 和 ArgoCD 可能无法帮助您执行严格的规则。如果您只是使用 ArgoCD，像 blue green 或 canary 这样的高级部署策略将很难实现。多集群管理也会带来大规模的挑战。

Harness 提供 ArgoCD-as-a-Service 来解决这些差距，并使 ArgoCD 可扩展用于企业组织。

## Argo CD:将“ClickOps”变成 GitOps

Argo CD 是使 GitOps 运行的控制器(以及其他东西)。它由 Intuit 开发，由社区维护，逐渐成为调和声明定义的基础设施和生产集群之间变化的标准。

在本文中，我们回顾了 Argo CD 的重要性，它能做什么，核心概念，以及用它部署应用程序的例子。

如果您希望收回在 CI/CD 上的投资，并且希望基础架构主要是自行管理，那么这是支持复杂应用程序部署的一个好地方。

您是否希望[选择 Argo CD](https://harness.io/blog/software-deployment-tools) ？[请求一个利用 GitOps 作为服务的演示](https://www.harness.io/demo/gitops)。