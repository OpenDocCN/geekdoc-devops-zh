# NSA Kubernetes 强化指南概述

> 原文：<https://www.fairwinds.com/blog/nsa-kubernetes-hardening-guide>

 本月早些时候，美国国家安全局(NSA)和网络安全与基础设施安全局(CISA)在 2021 年 8 月发布了 1.0 版本的 Kubernetes 硬化指南，并在 2022 年 3 月根据行业反馈对其进行了更新(1.1 版本)。Kubernetes 硬化指南的最新版本于 2022 年 8 月发布，其中包含一些更正和澄清。1.2 版本概述了对 [硬化 Kubernetes 集群](https://media.defense.gov/2021/Aug/03/2002820425/-1/-1/1/CTR_KUBERNETES%20HARDENING%20GUIDANCE.PDF) 的若干建议。该指南概述了一种强大的纵深防御方法，以确保当攻击者危及您的集群时，爆炸半径尽可能小。NSA Kubernetes 强化指南包括以下建议:

1.  扫描 Linux 容器映像和容器，查找漏洞或错误配置。

2.  以尽可能低的权限运行容器和单元。使用为以非根用户身份运行应用程序而构建的容器。如果可能的话，运行具有不可变文件系统的容器。

3.  使用技术控制来实施最低安全级别，例如防止特权容器、拒绝经常被利用来突破的容器功能(hostPID、hostIPC、hostNetwork、allowedHostPath)、拒绝作为根用户执行的容器(或允许提升到根用户)、强化应用程序以防止利用。

4.  使用网络隔离和加固来控制危害可能造成的损失。这包括锁定对控制平面节点的访问，并为控制平面组件和节点使用单独的网络，限制对 Kubernetes etcd 服务器的访问(etcd 应通过 API 服务器进行管理)，配置控制平面组件以使用传输层安全(TLS)证书来确保经过身份验证的加密通信，对静态 etcd 进行加密，设置网络策略以隔离资源，创建明确的拒绝访问策略，并将凭证和敏感信息加密到 Kubernetes Secrets 中。

5.  使用防火墙来限制不必要的网络连接和加密，以保护机密性。不要将 Kubernetes API 服务器暴露给互联网或不可信的网络。

6.  使用强认证和授权来限制用户和管理员的访问，以及限制攻击面。确保禁用匿名登录，并为基础架构团队、服务帐户、开发人员、用户和管理员创建基于角色的访问控制([【RBAC】](https://kubernetes.io/docs/reference/access-authn-authz/rbac/))策略。

7.  使用日志审计，以便管理员可以监控活动并对潜在的恶意活动发出警报。启用审核日志记录，并确保在节点、发布或容器失败时日志可用。在环境中配置日志记录，包括集群度量日志、集群应用程序接口(API)审计事件日志、Pod seccomp 日志、应用程序  日志和存储库审计日志。将日志聚集在群集外部的位置，并实施日志监控和警报系统。

8.  您的安全策略应该要求您定期检查所有 Kubernetes 设置，并使用漏洞扫描来帮助确保适当考虑风险并应用安全补丁。遵循应用程序安全最佳实践，包括按需升级、及时应用安全补丁和更新、定期执行漏洞扫描和渗透测试，以及从环境中删除未使用的组件。

## 为什么 NSA 强化指南对 Kubernetes 社区很重要？

组织正在采用 Kubernetes 并交付云原生应用程序，在亚马逊网络服务(AWS)、Azure 和谷歌云平台等云提供商上部署应用程序和服务。Kubernetes 开箱即用并不安全。默认情况下，部署到集群的所有设备都能够与其他设备进行通信。Pod 安全性非常重要，因为 pod 可以以 root 用户身份运行，即使您禁用了特权模式，群集中的攻击者仍然可以启用大量 pod 设置来获得底层节点的 root 用户访问权限。

不幸的是，有一个非常标准的过程来攻击你花了这么多时间的集群。考虑许多可能的攻击媒介:使用泄漏的 kubeconfig 或泄漏的云凭证、供应链攻击、暴露的仪表板或利用已知的安全漏洞。然后，攻击者可以在您的集群中执行他们自己的一些代码，提升权限，然后掩盖他们的踪迹。所有这些行为发生的同时，也创造了一个持久的立足点，使攻击者能够窃取机密和数据，消耗计算资源以获取经济利益，或实现任何数量的其他结果。

随着架构变得越来越复杂，保护您的微服务免受攻击变得越来越困难。您知道您的 Kubernetes 集群中的每个 pod 是否都禁用了[主机路径](https://kubernetes.io/docs/concepts/policy/pod-security-policy/)吗？通过您的集群的网络流量如何 pods 能够与控制平面组件或相邻的名称空间通信吗？这些小细节是将攻击的爆炸半径限制在单个容器内、让您的集群受到威胁、甚至让您的整个云环境受到威胁之间的区别。如果攻击者逃离 pod，使用节点的云凭据获得对您帐户的访问权限，并找到更多方法来提升权限，就会发生这种情况。

从内部威胁的角度来看， [RBAC](https://github.com/FairwindsOps/rbac-manager) 对于确保有权访问您的集群的每个人和应用程序都拥有适当级别的权限至关重要。管理 RBAC 配置文件，甚至了解配置的内容，都是一项艰巨的任务。在集群内部活动的恶意内部人员可以使用现有权限或通过利用已知漏洞来提升其权限，这就是为什么管理 RBAC、监控和修补容器中的已知漏洞以及审查服务的安全权限至关重要。

## 我如何实施美国国家安全局/CISA·库伯内特公司强化指南中的建议？

云本地计算基金会继续为 Kubernetes、Prometheus 和 Grafana 等项目提供开源中心。使用开源软件，如 Fairwinds 的 [【北极星】](https://www.fairwinds.com/polaris) ，是检查服务配置的第一步。借助 Polaris，您将获得 Kubernetes 安全状况的即时快照。Polaris 还提供了其他最佳实践建议，帮助您发现 Kubernetes 的错误配置并避免问题。

虽然 Polaris 改进了 Kubernetes 的部署，但 NSA 加固指南提倡采用更持续的方法来保护 Kubernetes 的安全。幸运的是，这些建议中有几个可以使用[fair winds Insights](https://www.fairwinds.com/insights)来实现，它将 Polaris 与其他开源工具相结合，提供了一个简化 Kubernetes 复杂性并降低风险的平台。

借助 Fairwinds Insights，DevOps 团队将能够持续运行 Polaris，并集成从 CI/CD 到生产的多种开源安全工具。Fairwinds Insights 补充道:

*   **【RBAC】监控:** 标记任意高度宽松的配置文件

*   **配置扫描:** 确保部署的工作负载和 pod 没有过度使用 Kubernetes 的安全特性

*   **策略实施和防护:** 在 CI/CD 和准入控制器环境中建立您的最低安全要求。这将自动防止使用集群的下游团队的安全错误配置

*   **运行时容器漏洞扫描:** 识别哪些部署了容器的容器出现高严重性 CVEs，或者最近变得易受攻击

如果您有兴趣了解更多信息，请点击 [试试 fair winds Insights](/insights-pricing)看看它如何帮助您强化 Kubernetes 环境。

[![Steps to Meeting NSA Kubernetes Hardening Guidelines  How to comply with NSA’s recommendations using Fairwinds Insights, open source and cloud native technologies](img/8972892aec3f4935ca3ce07b3077605b.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/abedc766-9d54-4068-8387-d11bb1fa97c7)

最初发布于 2021 年 8 月 10 日，更新于 2023 年 1 月 23 日