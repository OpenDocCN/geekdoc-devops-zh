# 2023 年你需要的三大 Kubernetes 安全策略

> 原文：<https://www.fairwinds.com/blog/the-top-three-kubernetes-security-strategies-you-need-for-2023>

 整个世界都在试图搬到库伯内特。同时，每个人都害怕自己会做错事。他们害怕发布配置糟糕、过度配置或过度许可的应用和服务。这种担心是可以理解的，但并不需要如此。你可以更安全地采用 Kubernetes，通过制定计划来帮助你准备和防止与错误配置、供应和许可相关的问题——换句话说，你可以从本帖中讨论的三个 Kubernetes 安全策略开始。

## 您何时需要 Kubernetes 安全策略？

每个组织或安全团队都害怕对您的应用程序、数据、与客户的互动以及声誉造成意外影响的可能性。无论是违规、应用程序能够以您意想不到的方式相互通信，还是通过第三方扩大攻击范围，如果攻击者突破了边界，他们将能够访问系统和服务，并可能造成相当大的损失。

实现 Kubernetes 安全性 的最佳方法是尽早制定计划，因为安全性很难追溯。如果你的公司已经成立五年了，你的应用和服务已经建立起来了。那时你可能有一个复杂的技术栈，并且你可能有大量的技术债务。实现您需要的那种安全控制比您预先做的要困难得多。

## 为什么您需要 Kubernetes 安全策略？

当您转移到 Kubernetes 时，一个实质性的区别是您将更多的控制权交给了单个开发团队，让他们在 Kubernetes 中为自己提供基础设施。伴随着额外的权力而来的是责任。突然间，那些开发者可以做各种他们以前不能做的事情；过去，他们必须通过 DevOps 工程师为他们创建 EC2 实例。在 Kubernetes 中—根据配置，他们可以:

*   运行一个命令，让一个应用程序在集群内部运行

*   以 root 用户身份运行应用程序

*   访问主机节点上的文件

*   访问集群中的秘密

*   运行 [易损容器](https://www.fairwinds.com/blog/cve-2022-3602-and-cve-2022-3786-openssl-vulnerabilities-scanning-container-images)

如果您试图保护和管理您的 Kubernetes 环境，您可能希望阻止他们做这些事情。单个开发团队所拥有的额外能力使得 Kubernetes 的安全性与传统的基础设施平台大相径庭。另一件要记住的事情是，虽然 Kubernetes 是短暂的，但这并不能保护你的应用和服务免受安全风险。

## 您的 Kubernetes 安全策略应该涵盖哪些内容

您的 Kubernetes 安全策略需要涵盖三个基本要素:

1.  不允许人们做超过他们应该做的事情

2.  不允许可避免的已知错误

3.  按角色划分您的人员和团队

让我们更深入地探讨这些话题。

### 1。不允许人们做超过他们应该做的事情

从最低权限 的 [原则开始——将访问权限限制为用户完成工作所需的权限。也不仅仅是为了人。最低特权访问也适用于应用程序、系统和进程。以下是如何应用于 Kubernetes 的例子。](https://www.techtarget.com/searchsecurity/definition/principle-of-least-privilege-POLP)

#### *没有安全感的能力*

当你运行一个 [Docker](https://www.docker.com/) 容器时，你可以传递它的配置信息，这些信息表明它对运行它的机器有什么权限。您可以做的事情之一是向容器添加功能，以提供对主机的更多访问。除非您的应用程序将网络或基线工具作为基础设施的一部分来处理，否则您不需要允许这些不安全的功能。您的普通应用程序不需要任何特殊的权限或功能。然而，Kubernetes 确实使您能够添加这些功能，所以要确保大多数团队在大多数情况下都没有添加这些功能。

#### *权限升级允许*

特权提升意味着容器可以提升到拥有更多特权。在容器的安全上下文中，必须设置`allowPrivilegeEscalation=false`。如果有人利用漏洞逃离容器，这有助于减轻威胁。将其设置为 false 意味着攻击者将无法升级到在运行容器的主机上拥有 root 或管理权限。该容器运行在 Kubernetes 节点上，其他容器甚至不同类型数据的不同应用程序也在该节点上运行，因此您希望确保它只拥有所需的特权，并且不能提升这些特权。

#### *以根权限运行*

这是不安全功能和权限提升的结合。禁用以 root 用户身份运行的能力，以便潜在的容器转义具有更少的权限，可以造成更少的损害。如果向容器授予额外的权限，可能会影响主机节点，并可能影响该节点上的其他容器。禁用该功能会增加一层额外的保护，防止容器以 root 用户身份运行。要做到这一点，您可能需要在您的应用程序中做一些重新架构的工作。这是 Kubernetes 安全性的最佳实践。

### 2。不允许可避免的已知错误

这听起来像是一个容易遵循的安全策略，但是在您开始时很难避免已知的错误。一个有经验的 Kubernetes 工程师可以帮助你避免所有的节点错误或你可能犯的其他错误，但是也有很多[开源工具](/open-source-software)和其他第三方工具可用。这些之所以存在，是因为 Kubernetes 的一名工程师坐下来编写软件，帮助人们避免 [犯下那些](https://www.fairwinds.com/devops-webinar-kubernetes-mistakes-reg)的十大错误。考虑看看这个软件或者请一位 Kubernetes 专家，因为这将为你节省大量的时间和压力。

#### *镜像漏洞*

您在 Kubernetes 集群中运行的每个容器都从一个基本映像开始。这个基础映像可以是一个普通的操作系统，比如 Ubuntu 或 Alpine，也可以附带一些额外的软件，比如 Python 或 NodeJS..然后你在上面安装其他东西。您需要确保您的基本映像中的软件以及您在其上安装的东西是最新的，并且没有漏洞。漏洞可能在您部署应用程序时发生，但也可能在以后被发现或披露。您需要不断地评估您的集群中有漏洞的映像。然后，您需要解决这些漏洞，并重建和重新部署您的应用程序和服务，以便它所在的容器映像不包含这些漏洞。

每天都有[个新漏洞被披露](https://twitter.com/CVEnew/)。有时，这些漏洞存在于已经在您的环境中运行了(非常)长时间的代码中。减轻这种风险的一种方法是尽量减少在容器映像中安装不必要的东西。放入工具来帮助您在容器映像内部进行调试(万一有一天您需要它们)或者在非生产环境中有用的其他工具可能很有诱惑力。然后，当您将同一个映像提升到生产环境中时，您会得到一个包含应用程序运行所不需要的元素的映像，这增加了生产环境中映像漏洞的风险。相反，使用 kubectl debug 之类的命令来启动 [(一个临时容器](https://kubernetes.io/docs/concepts/workloads/pods/ephemeral-containers/) )与您的工作负载一起进行调试。

### 3。按角色划分您的人员或团队

Kubernetes 有一个基于角色的访问控制的概念([【RBAC】](https://kubernetes.io/docs/reference/access-authn-authz/rbac/))。您可以使用基于角色的访问控制向需要与 Kubernetes 交互的人授予特权。当您第一次建立一个集群时，很容易接受 Kubernetes 包含的主要管理角色，并将该角色赋予您的小公司中的所有人，因为它可以帮助您快速移动。不要那样做。

不是所有人都可以自由管理访问，而是使用您自己的集群角色和用户角色来授予特权，使用被认为是 [安全](https://www.fairwinds.com/kubernetes-security) 最佳实践的 [最小特权模型](https://www.techtarget.com/searchsecurity/definition/principle-of-least-privilege-POLP) 。授予最少的必要特权。一些容器映像要求它们以 root 用户身份启动，然后放弃权限，这意味着您必须以 root 用户身份运行一段时间，然后确保权限发生变化。确保您在角色的约束下工作，但不要关闭约束。

[审计日志](https://kubernetes.io/docs/tasks/debug/debug-cluster/audit/) 与 RBAC 相关，可以通过提供一组按时间顺序排列的集群中的操作序列来帮助您。当您围绕 Kubernetes 构建集群并运行您的团队时，请研究审计日志记录、如何查看这些日志，以及如何对它们进行调优以最大限度地减少干扰。审计日志显示谁在采取措施，以及这些措施与角色的关系，以及您为每个角色启用的访问权限。

## 支持 Kubernetes 安全策略的开源工具

我们收集跨组织 的 [统计数据，了解他们在 Kubernetes 环境中做对了什么，做错了什么。在整个组织中，我们看到 54%的组织将其一半以上的工作负载开放给](https://www.fairwinds.com/kubernetes-config-benchmark-report) [权限提升](https://www.youtube.com/watch?v=z75pCN6QRuY) ，因此存在安全漏洞。值得看看您在自己的集群中实现了什么，以确保您的工作负载不会受到权限提升的影响。幸运的是，有一些开源工具可以帮助您实施这些 Kubernetes 安全策略。

[北极星](https://www.fairwinds.com/polaris) 带有 30 多个基于 Kubernetes 最佳实践的内置检查，并支持自定义策略。使用 Polaris，您可以编写 JSON 模式来运行 Kubernetes 集群中的任何资源，并创建您自己的检查。当您获得了广泛的最佳实践并开始将特定于您组织的策略落实到位时，自定义策略就开始发挥作用了。北极星可以帮助你确保你不允许人们做他们不应该做的事情——这是本帖中概述的第一个 Kubernetes 安全策略。

[Nova](https://nova.docs.fairwinds.com/) 提醒您集群中部署的舵图有可用更新。更新通常与修补的安全漏洞或新的可用功能有关，因此 Nova 可以帮助您提高 Kubernetes 的安全状况。 [冥王星](https://pluto.docs.fairwinds.com/) 提供已弃用的[Kubernetes API](https://kubernetes.io/docs/concepts/overview/kubernetes-api/)的高级警告。随着 Kubernetes 更新了新的特性或改进了的实现选项，Kubernetes APIs 也会随之改变。有时候 API 的弃用和移除会让 Kubernetes 用户大吃一惊；Pluto 帮助您定期检查您的资源和集群中不推荐使用的 API，这些 API 可能会在 Kubernetes 的未来版本中被删除。 [Trivy](https://aquasecurity.github.io/trivy/v0.35/) 既有开放的扫描器，又有开放的数据库，它可以在能发现问题的地方寻找安全问题和目标。Trivy 可以扫描的目标包括容器映像、文件系统、Git 存储库(远程)、虚拟机映像、Kubernetes 和 AWS。这些开源工具可以帮助您避免已知的错误，让您的 Kubernetes 集群保持运行。

对于基于角色的访问控制，我们还开发了一个叫做 [RBAC 管理器](https://rbac-manager.docs.fairwinds.com/introduction/) 的开源工具。它可以帮助您简化创建集群角色所需编写的代码量，并将新角色绑定到用户或用户组。

## 实施您的 Kubernetes 安全策略

在单个集群上安装开源项目并使项目保持最新是相当简单的，然而，在数十或数百个集群上这样做更具挑战性。在大型 Kubernetes 环境中，考虑采用治理软件来帮助您从 CI/CD 到生产操作这些 Kubernetes 安全策略。[fair winds Insights](https://www.fairwinds.com/insights)帮助您提高安全性和合规性风险的可见性，优化成本，并为策略和安全性设置防护栏。

随着 2022 年接近尾声，你如何看待 2023 年的 Kubernetes 安全？我们知道 DevOps 团队对于确保 Kubernetes 的安全至关重要，但是您准备好迎接 2023 年 Kubernetes 安全策略的发展了吗？这三个策略会让你在新的一年里有一个稳固的基础，并且比过去更接近发展战略。

### 观看网络研讨会[Kubernetes 2023 年安全战略](https://www.fairwinds.com/cj-webinar-kubernetes-security-strategies-for-2023-reg) 立即点播，了解有关 Kubernetes 安全战略、治理和护栏的更多信息。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)