# 第一个 Kubernetes 集群的安全基础

> 原文：<https://www.fairwinds.com/blog/security-basics-for-your-first-kubernetes-cluster>

 所以你建立了一个集群。也许你已经部署了一些应用程序。你对库伯内特的事情很感兴趣。现在你做什么？

接下来可以做的事情很多。你可以安装一堆工具来让你的生活变得更轻松(无耻地塞给我的[上一篇文章](https://medium.com/uptime-99/kubernetes-202-making-it-fully-operational-7416e4bb15ab))。您可以开始部署应用程序并在生产中运行它们。或者，你可能在想“我到底要如何保护我刚刚放在云中的这个庞大复杂的东西呢?*!？！?"这是一个有效的问题，我将尝试通过一系列易于遵循的实用步骤来回答这个问题。*

 *免责声明:我并没有写一个关于保护集群安全的完整指南，也没有声称这些信息是完美的。这是一个起点，一种让大脑活跃起来的方式，一种开启全面安全计划之门的方式。

### GKE

我完全在 AWS 中开始了我的 Kubernetes 之旅，并认为它是 Bees Knees。然后，我发现了 GKE(谷歌 Kubernetes 引擎)，并迅速改变了我的想法。这两个平台都很棒，也有各自的缺点，但如果你刚刚起步，GKE 是一个很好的地方。

GKE 最新的默认设置相当不错，而且还在不断改进，但是在设置集群时，您应该选择一些设置，这样您以后就能够做一些安全的事情。

#### VPC 本地集群

首先要做的是打开 VPC 本地网络配置。这在 GKE 是相对较新的，很快将成为默认设置。它将集群放在 VPC 中，就像在 AWS 中一样。只差一个复选框，完全值得一试:

GKE VPC-Native Option

#### 集群访问

完成后，我将浏览一组与集群网络相关的复选框。第一个在这里:

GKE Enable Master Authorized Networks

当你看到这里时，你可能会想“这个家伙不久前不是告诉我们 IP 白名单不是一种安全形式，我们应该停止这样做吗？”你在这一点上基本上是对的，我倾向于认为这一步基本上是不必要的。然而，作为更大的安全态势的一部分，这个设置可能是一个好主意。该设置将对主节点(和 Kubernetes API)的访问限制在一组特定的 IP 地址。这并不意味着这些 IP 地址上的任何人都可以神奇地访问您的 API，因为他们仍然需要通过 Kubernetes 进行身份验证才能做任何事情。它让你安心，如果 Kubernetes(见[CVE-2018–1002105](https://github.com/kubernetes/kubernetes/issues/71411))出现严重漏洞，你有一点喘息的机会来修复它。这自然会对公共 CI/CD 工具和远程工作人员的可用性产生影响，但是如果您的集群的所有预期流量都来自单个网络内部，那么请务必打开“启用主授权网络”并将其设置为该 IP CIDR 块。

#### 网络策略

我们要考虑的第三个复选框是*启用网络策略:*

![](img/21459716d05989769d0e658091af3874.png)

GKE Enable Network Policy

这个盒子听起来很棒，事实也的确如此。它支持名为“Calico”的网络策略实现，允许您创建 Kubernetes [网络策略](https://kubernetes.io/docs/concepts/services-networking/network-policies/)。反过来，这提供了严格限制集群中哪些东西可以与其他东西对话的能力。它并没有带来很多现成的好处，但它打开了通往安全世界的大门。如果你计划做高级网络策略，那么在开始的时候选中这个框会省去你一些麻烦。

#### 禁用旧身份验证

最后一组框控制 Kubernetes API 的身份验证:

GKE Settings Security Section — Auth

很快，默认选项就足够了，但在此之前，您需要取消选中*启用遗留认证*、*发布客户端证书*和*启用遗留授权*。这将禁用基本身份验证，并强制所有用户使用 GCP 身份验证来访问群集。此外，Google 默认启用 RBAC，以便我们可以控制谁可以访问群集中的内容。稍后我将更详细地介绍这一点。

### AWS (kops)

默认情况下，kops 在 VPC 内部创建一个集群，其中节点位于专用子网中，主节点位于公共子网中，ELB 用于 API 访问，ssh 仅在内部可用。总的来说，这是一个很棒的网络配置，不需要太多改变。在这一点上向 kops 开发者脱帽致敬。但是，在创建集群时需要添加一些设置，以最大限度地提高安全性。

#### 集群访问

首先，您可以启用一个可以访问主服务器的 IP 白名单(参见 GKE 部分了解我对此的想法)。您可以在 kops 集群 yaml 中通过添加这个珍闻并替换您的 IP CIDR 块来做到这一点:

```
spec:
 kubernetesApiAccess:
 - 203.0.113.0/24
```

#### 禁用匿名身份验证

您可以做的另一个简单的改变是关闭 kubelet 的匿名认证。从 [Kops 安全](https://github.com/kubernetes/kops/blob/master/docs/security.md#kubelet-api):

```
spec:
 kubelet:
 anonymousAuth: false
```

#### 网络策略

如果您想启用网络策略，可以通过更改网络覆盖来实现。Calico 或 Canal 都是支持网络政策的好选择。我不会在这里详细介绍它，但是 kops 有关于这个主题的大量文档。

#### 启用 RBAC

接下来，您需要确保在集群中启用了 RBAC。这是新 kops 集群中的默认设置，但是值得检查一下集群规范:

```
spec:
 authorization:
 rbac: {}
```

#### 启用审核日志记录

您想要的最后一个设置是打开审计日志记录。GKE 默认给你这个，但是在 kops 中我们必须自己添加。在 [Fairwinds](/) 时，我们使用来自 [Kubernetes 文档](https://kubernetes.io/docs/tasks/debug-application-cluster/audit/)的以下配置:

### 集群安全性

一旦您使用所有这些选项构建了集群，您还可以做更多的事情来增强您的安全状况。

#### 使用 RBAC

现在启用了，就用吧。内置策略可能有用，但您也应该考虑自己的策略。此外，无论何时安装舵图，都要确保非常常见的`rbac.enabled: true`设置处于开启状态。这将创建特定于该应用程序的 RBAC 对象。去看看他们，看看他们做了什么。

使用 [RBAC 管理器](https://github.com/reactiveops/rbac-manager)简化角色绑定和集群角色绑定的创建。这将使您能够创建一个简单的 CRD，将用户/组/服务帐户绑定到角色/集群角色。

最后，当你深入 RBAC 的杂草中，无论如何也想不出为什么有人能或不能接触到某样东西时，使用 [RBAC 查找](https://github.com/reactiveops/rbac-lookup)来查找。

![](img/69333e376fb852f5733482f952c29502.png)

Brief Demo of rbac-lookup — [https://asciinema.org/a/222327](https://asciinema.org/a/222327)

#### 自由使用名称空间

引用一位同事的话，“名称空间很便宜。”使用它们来分离基础设施工具和应用程序之类的东西。这允许您使用 RBAC 轻松限制访问，并限制应用程序的范围。当您决定创建网络策略时，我也会让它变得更容易。

### 集装箱安全

最后，我们需要保护我们在集群中运行的工作负载，因为这些工作负载将暴露给我们的(潜在的敌对)用户。在 Kubernetes 中运行容器时，有许多默认或常见的东西提供了不太理想的安全性。这里有一个你可以开始做的事情的非详尽列表。

#### 不要在每个容器中运行多个进程

Docker 通用最佳实践建议每个容器使用一个进程，这仅仅是出于可用性和大小的原因，实际上这也是一个很好的安全实践。它使您的攻击面保持较小，并限制了潜在漏洞的数量。

#### 不要以特权身份运行容器

以特权身份运行容器或 pod 使其能够对主机进行修改。这是一个很大的安全问题，只要不去做就很容易缓解。

#### 不要以根用户的身份运行容器

默认情况下，容器作为 root 用户在容器内运行。这很容易避免使用 pod 规范来设置高编号的 UID。

```
spec:
 securityContext:
 runAsUser: 10324
```

#### 不要使用默认的功能列表

默认情况下，Docker 运行具有大量 Linux 功能的容器，其中许多功能您的应用程序可能并不需要。你可以在[源代码](https://github.com/moby/moby/blob/master/oci/defaults.go#L14-L30)中看到它们。默认情况下，以下配置将删除所有 Linux 功能，允许您只添加应用程序实际需要的特定功能:

```
spec:
 containers:
 - name: foo
 securityContext:
 capabilities:
 drop:
 - ALL
```

#### 扫描容器中的漏洞

已知漏洞占违规的很大一部分。使用工具来扫描您的容器，然后缓解它们。 [Anchore](https://anchore.com/) 、 [Clair](https://github.com/coreos/clair) 和 [Quay](https://quay.io/) 是我最喜欢的工具，但是还有其他的。

#### 看看 KUBSEC。超正析象管(Image Orthicon)

这个工具将分析您的部署 yaml，并告诉您应该进行哪些更改，以提高这些 pod 的安全性。它甚至给你一个分数，你可以用它来建立一个最低标准。该分数包含了我上面概述的所有最佳实践，以及其他一些最佳实践。

下面是一个得分为 9 的部署示例:apiVersion: apps/v1

### 不要停在这里

一直都有关于 Kubernetes 和安全的新东西出现。去读一读，试一试。就在最近，我在 CNCF 博客上读到了[一篇文章，这篇文章很好地概述了安全最佳实践，并提醒了我一些我们应该做得更好的事情。安全是一个复杂的多层网络，不能用一种工具来解决，它永远不会“完成”，所以请不要在阅读本文后停止关心它。](https://www.cncf.io/blog/2019/01/14/9-kubernetes-security-best-practices-everyone-must-follow/)*