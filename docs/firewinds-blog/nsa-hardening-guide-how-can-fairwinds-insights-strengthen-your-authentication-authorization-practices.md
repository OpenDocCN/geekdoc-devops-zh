# NSA 强化指南:Fairwinds Insights 如何加强您的身份验证和授权实践？

> 原文：<https://www.fairwinds.com/blog/nsa-hardening-guide-how-can-fairwinds-insights-strengthen-your-authentication-authorization-practices>

 如果你还没有翻阅美国国家安全局最近的 [Kubernetes 强化指南](https://media.defense.gov/2021/Aug/03/2002820425/-1/-1/0/CTR_Kubernetes_Hardening_Guidance_1.1_20220315.PDF) 来了解更多关于当今 Kubernetes 和云原生技术的最佳实践，不要担心——我们已经覆盖了你。在关于这些新兴标准的五篇系列文章的第三部分中，我们将讨论认证和授权的所有内容，包括 *如何*[fair winds Insights](https://www.fairwinds.com/insights)，我们的 Kubernetes 治理平台，它可以帮助您的组织满足 NSA 的严格建议。

**ICYMI** : NSA 加固指南: [利用 Fairwinds Insights 锁定网络访问](https://www.fairwinds.com/blog)

**ICYMI:** NSA 加固指南: [三种方式的 Fairwinds 见解可以根除糟糕的 Pod 安全性](https://www.fairwinds.com/blog/three-ways-fairwinds-insights-can-root-out-poor-pod-security)

## 认证的重要性

作为限制访问群集资源的主要机制，身份验证和授权的最佳实践至关重要。如果 Kubernetes 集群配置错误(这种情况经常发生),坏人可以(而且确实会)扫描众所周知的 Kubernetes 端口，访问集群的数据库或在没有身份验证的情况下进行 API 调用。是的，支持多种用户身份验证机制，但默认情况下不启用。

NSA 已经就登录程序、特定政策的制定和强身份认证的需求提出了几项重要建议，所有这些都通过 Fairwinds Insights 的功能得到了具体解决。请关注我们，我们将概述我们的 Kubernetes 治理软件可以帮助您的组织遵守这些现代实践的不同方式，这些实践现在被认为是 Kubernetes 所有权的黄金标准。

## NSA 对认证和授权有什么建议？

> **NSA:禁用匿名登录** 。 匿名请求是指不会被其他已配置的身份验证方法拒绝的请求，并且不会绑定到任何个人用户或 Pod。启用匿名请求可能会允许网络参与者在未经身份验证的情况下访问群集资源。

Kubernetes 为用户提供了多种向集群进行身份验证的方法，并且有内置的用户，如 system:unauthenticated 和角色，如*system:public-info-viewer*，它们允许公共访问健康和版本 API 等内容。

在自管理集群上，可以通过*-anonymous-auth*标志禁用匿名登录。该流程可以像 GKE 一样 [更多地参与托管服务。](https://cloud.google.com/kubernetes-engine/docs/how-to/hardening-your-cluster)

Fairwinds Insights 可以通过其 kube-bench 集成，帮助确保在您的集群上设置 - anonymous-auth 标志。具体来说，check 4.2.1 将确保此标志设置为 false，如果不是，则引发一个操作项。

**想一下子得到所有 NSA 的推荐？下载我们的白皮书:** [**满足 NSA Kubernetes 强化指南的步骤**](https://www.fairwinds.com/kubernetes-nsa-hardening-insights)

> **NSA:使用强用户认证** 。 管理员必须实施认证方法或将认证委托给第三方服务。Kubernetes 假设独立于集群的服务管理用户认证。

获得正确的集群身份认证对于安全性和必须与集群交互的工程师的日常工作效率都至关重要。Kubernetes 在这里提供了许多不同的选项。 如果您正在使用 GKE 或 EKS 等托管 Kubernetes 提供商，我们建议您使用云提供商的内置身份和访问管理(IAM)。例如，Kubernetes 团队提供了[aws-iam-authenticator](https://github.com/kubernetes-sigs/aws-iam-authenticator)，用于通过现有的 AWS 组和角色管理集群访问。

如果您没有使用托管的 Kubernetes 提供商，或者如果 IAM 不是一个选项，我们建议您使用 OpenID Connect (OIDC)以及像 Google Workspace 这样的 SSO 提供商。

> NSA:创建具有独特角色的 RBAC 政策。 基于角色的访问控制(RBAC)，默认启用，是一种基于组织内个人角色来控制对集群资源访问的方法。RBAC 可用于限制用户帐户和服务帐户的访问。

一旦用户能够使用上述身份验证方法连接到集群，您将需要设置 RBAC，以确保他们拥有完成工作所需的权限，同时仍然遵循最小权限原则。

我们强烈建议设置与贵公司具体工作描述相关的角色和集群角色。例如，您可能有一个开发人员角色，可以查看日志和状态；允许在应用程序命名空间中进行更改的 SRE 角色；和被授予广泛访问权限的管理员角色。

[RBAC 经理](https://github.com/FairwindsOps/rbac-manager) 可以帮助用更友好的语法创建 RBAC 配置文件，而 Fairwinds Insights 提供了一个仪表板，用于审计 RBAC 配置，显示具有高级访问权限的角色和集群角色。

## 更好认证的步骤&授权

如果您想了解有关 Fairwinds Insights 如何帮助您的企业遵守 NSA 最新准则的更多信息，请阅读我们最新的白皮书， [满足 NSA Kubernetes 强化准则的步骤](https://www.fairwinds.com/kubernetes-nsa-hardening-insights) 。

我们的 NSA 白皮书还提供了关于满足所有 NSA 建议的信息，而不仅仅是那些与身份验证和授权相关的信息。该白皮书详细讨论了其他领域，如 pod 安全、网络访问、审计日志、威胁检测和其他应用程序安全实践。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)