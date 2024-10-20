# NSA 强化指南:利用 Fairwinds Insights 锁定网络访问

> 原文：<https://www.fairwinds.com/blog/nsa-hardening-guide-locking-down-network-access-with-fairwinds-insights>

 在我们的第二篇博客中，涵盖了 [NSA Kubernetes 强化指南](https://media.defense.gov/2021/Aug/03/2002820425/-1/-1/0/CTR_Kubernetes_Hardening_Guidance_1.1_20220315.PDF) ，我们将讨论无数种方式[fair winds Insights](https://www.fairwinds.com/insights)——以及 [开源](https://www.fairwinds.com/open-source-software) 和云原生技术——可以帮助您的组织遵守这些重要的建议。虽然我们的第一篇文章介绍了 Insights 增强和支持 pod 安全性的功能，这是云环境中所有企业都应该关注的问题，但我们的第二篇文章将重点关注网络分离和强化。

想要立刻得到所有美国国家安全局的建议？下载我们最新的白皮书

[**满足 NSA Kubernetes 硬化准则的步骤。**](https://www.fairwinds.com/kubernetes-nsa-hardening-insights)

## 寻址网络

集群网络是 Kubernetes 概念的核心。必须考虑吊舱、容器和外部服务之间的通信。默认情况下，Kubernetes 资源不是孤立的，如果集群受到威胁，就无法防止横向移动或升级。资源分离和加密是限制数字攻击者在 Kubernetes 集群内活动和升级的有效方法。

总体而言，美国国家安全局建议使用网络政策和防火墙来分离和隔离资源，保护控制平面，以及加密流量和静态敏感数据。让我们看看我们的 Kubernetes 治理软件解决这些关键领域的不同方法，包括遵守所有措施的最佳方法。

**围绕网络分离，NSA 有哪些建议？**

> NSA:使用防火墙锁定对控制平面节点的访问。控制平面是 Kubernetes 的核心，允许用户在集群中查看容器、安排新的 pod、读取机密和执行命令。由于这些敏感功能，控制平面应该受到高度保护。Kubernetes API 服务器不应该暴露给互联网或不可信的网络。

保护控制平面访问的最简单方法是使用托管的 Kubernetes 服务，如 GKE 或 EKS。这些解决方案抽象出控制平面，防止其配置错误。

如果您必须管理自己的控制平面节点，请使用 VPC 和网络访问控制列表(NACL)来控制对节点的访问。我们建议通过与您的控制平面节点位于同一 VPC 中的 SSH 堡垒来控制对控制平面的访问。

> NSA:使用 RBAC 锁定对控制平面节点的访问。 默认情况下启用，RBAC 是一种用于根据组织内个人的角色来控制对集群资源的访问的方法。RBAC 可用于限制对用户和服务帐户的访问。

在库伯内特，RBAC 是一个棘手的概念。Fairwinds 提供了两个开源项目来帮助管理 RBAC:

*   [【RBAC】管理器](https://github.com/FairwindsOps/rbac-manager) 提供了一个更简单的权限调配接口。
*   [](https://github.com/FairwindsOps/rbac-lookup)RBAC 查找允许用户精确地确定谁有权访问什么。

虽然每个组织都有不同的需求，并对如何配置 RBAC 做出不同的设计决策，但利用角色和集群角色为不同的角色提供不同的访问级别是势在必行的。这有助于遵守最小特权原则，还有防止诚实错误的额外好处。

Fairwinds Insights 提供了一个界面，用于审核每个集群中的 RBAC，显示特别危险的角色和集群角色。

> **NSA:对控制平面组件和节点使用单独的网络。** 管理员应主动限制攻击面，将工作节点与不需要与工作节点或 Kubernetes 服务通信的其他网段分开。

网络分段是限制攻击爆炸半径的一个很好的方法。特别是，将面向用户的应用程序与控制平面分开非常重要，这样可以防止攻击者逃离应用程序环境并获得对整个集群的访问权限。

我们建议在您的 VPC 中为不同类型的工作节点使用不同的子网，特别是将需要控制平面访问的 nginx-ingress 或 cert- manager 等工具从面向用户的应用程序中分离出来。像 AWS 的安全组这样的概念可以用来限制节点组之间的通信。

借助 Fairwinds Insights，您可以在 Kubernetes 集群中运行 kube-hunter，以更好地了解哪些节点可以访问控制平面。为了了解这些节点对控制平面的访问权限，我们建议将 kube-hunter 配置为在应用程序节点上运行。

> **NSA:配置控制平面组件，以使用 TLS 进行经过认证的加密通信。**etcd 后端数据库存储状态信息和集群机密。它是一个关键的控制面板组件，获得 etcd 的写访问权限可以让 cyber actor 获得整个集群的 root 访问权限。

默认情况下，大多数 Kubernetes 部署都会加密网络流量。无论您是使用 EKS 或 GKE 这样的托管 Kubernetes 服务，还是使用 kOps 这样的工具来管理自己的控制平面，默认配置都将使用 TLS 来加密与控制平面的通信。

与往常一样，确保你使用的是最新版本的 Kubernetes 和 kOps 之类的工具。

> **NSA:静态加密 etcd，使用单独的 TLS 证书。**etcd 后端数据库是一个关键的控制平面组件，也是控制平面中最重要的安全部分。

同样，通过使用 GKE 或 EKS 这样的托管 Kubernetes 服务，您可以避免担心您的控制平面配置。但是，如果您确实需要管理控制平面节点，请确保加密底层卷，并设置唯一的 TLS 证书，以便与 etcd 进行通信。

Fairwinds Insights 可以在您的每个集群中运行 kube-bench，它会根据 CIS 基准运行许多自动检查。其中一个检查(检查 2.1)将确保您已经用适当的证书文件和密钥文件配置了 etcd。如果没有，Fairwinds Insights 将创建一个行动项目来解决问题。

> **NSA:设置网络策略隔离资源。** 网络策略控制 pod、名称空间和外部 IP 地址之间的流量。默认情况下，没有网络策略应用于 pod 或命名空间，导致 pod 网络中的进出流量不受限制。

虽然 Kubernetes 提供了内置的网络策略资源，但它的范围相当有限。它只能在 L3/L4 限制流量，例如基于 IP 地址，这使得它在实践中很难使用。

我们建议使用像 Linkerd 这样的开源服务网格。该服务可以捕获 L7 语义，允许更具可读性、可维护性和细粒度的网络策略。Linkerd 还在 Pod 级别而不是 CNI 级别执行其策略，这更接近于“零信任”的理想状态。

## 网络加固的后续步骤

如果您有兴趣了解更多有关见解如何帮助您的组织实现新的 NSA 准则的其他基准，包括如何创建明确的拒绝网络策略并将所有敏感信息加密到 Kubernetes Secrets 中，请阅读我们的新白皮书， [满足 NSA Kubernetes 强化准则的步骤](https://www.fairwinds.com/kubernetes-nsa-hardening-insights) 。此外，这些信息将告诉您如何在部署期间避免错误配置并实施推荐的措施和缓解措施。

我们的 NSA 白皮书还提供了关于满足所有 NSA 建议的信息，而不仅仅是 pod 安全性和网络加固。它包括对其他领域的详细讨论，如身份验证和授权、审计日志、威胁检测和其他应用程序安全实践。随着集装箱化世界的不断发展，越来越明显的是，强大的纵深防御方法是最小化风险、最大化效率和创新的最佳方式。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)