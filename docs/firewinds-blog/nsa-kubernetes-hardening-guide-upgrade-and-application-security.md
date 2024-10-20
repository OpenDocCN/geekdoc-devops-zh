# NSA Kubernetes 强化指南:升级和应用安全

> 原文：<https://www.fairwinds.com/blog/nsa-kubernetes-hardening-guide-upgrade-and-application-security>

 我们的 NSA Kubernetes 强化指南系列已经查看了 [pod 安全](https://www.fairwinds.com/blog/three-ways-fairwinds-insights-can-root-out-poor-pod-security)、[网络访问](https://www.fairwinds.com/blog/nsa-hardening-guide-locking-down-network-access-with-fairwinds-insights)、[认证和授权](https://www.fairwinds.com/blog/nsa-hardening-guide-how-can-fairwinds-insights-strengthen-your-authentication-authorization-practices)、[审计日志记录和威胁检测](https://www.fairwinds.com/blog/nsa-kubernetes-hardening-guide-audit-logging-and-threat-detection-overview)。在本系列的最后一部分中，我们将讨论升级和应用程序安全实践。

[NSA Kubernetes 强化指南](https://media.defense.gov/2021/Aug/03/2002820425/-1/-1/0/CTR_Kubernetes_Hardening_Guidance_1.1_20220315.PDF)旨在帮助组织加强其容器和 Kubernetes 的安全性，以最大限度地降低违规的可能性，并确保如果攻击者确实渗透到您的集群，爆炸半径将尽可能小。在这里，我们将讨论安全补丁和更新、漏洞扫描以及删除集群中未使用的组件。

## 及时应用安全补丁和更新

> 安全是一个持续的过程，及时安装补丁、更新和升级至关重要。具体的软件组件因具体的配置而异，但整个系统的每一部分都必须尽可能保持安全。这包括更新 Kubernetes、虚拟机管理程序、虚拟化软件、插件、运行环境的操作系统、运行在服务器上的应用程序、组织的持续集成/持续交付(CI/CD)管道的所有元素以及环境中托管的任何其他软件。

保持 Kubernetes 和所有底层组件和容器映像最新可能是一项全职工作(甚至是多项全职工作)。拥有合适的工具有助于减少相关的劳动和工程工作。

[Fairwinds Insights](//www.fairwinds.com/insights) 是一个帮助实施安全护栏的 Kubernetes 治理平台，它提供了两种服务:

*   Nova 扫描环境中任何过时或废弃的头盔版本。它将为每个有可用更新的版本显示一个操作项。

*   [Trivy](https://aquasecurity.github.io/trivy/v0.28.1/) 扫描环境中每个容器的漏洞(无论是操作系统还是安装在上面的软件),并在有补丁可用时推荐升级。

此外，确保 Kubernetes 本身保持最新也很重要，因为每个版本的支持期只有一年左右。

## 执行定期漏洞扫描和渗透测试

> 管理员应定期检查，以确保其系统的安全性符合当前的网络安全最佳实践。应对各种系统组件执行定期漏洞扫描和渗透测试，以主动寻找不安全的配置和零日漏洞。

Kubernetes 集群不是一个静态的基础设施。这是一个不断发展的生态系统，每天都会自动应用新的应用程序和更新。组织必须结合使用自动化和手动渗透测试来定期扫描他们的 Kubernetes 环境中的漏洞。

默认情况下， [Fairwinds Insights](//www.fairwinds.com/insights) 每小时运行一次其组件，并且可以配置为每分钟运行一次。此功能确保在发现新漏洞时尽快提出问题。

除了上面列出的 Trivy、 [Polaris](https://polaris.docs.fairwinds.com/) 和 kube-bench 集成之外，Insights 还提供了 kube-hunter 集成，它可以自动探测集群的网络漏洞和攻击者可能利用的其他安全问题。

## 从环境中卸载并删除未使用的组件

> 当管理员部署更新时，他们还应该从环境和部署管道中卸载任何旧的、未使用的组件。这种做法将有助于减少攻击面，并降低未使用的工具留在系统中以及过时的风险。

确保陈旧和未使用的部署不会停留在您的 Kubernetes 环境中，这对于安全性和一般卫生都很重要。不幸的是，Kubernetes 没有一种通用的方法来检查集群中资源的陈旧性。有一个[未解决的问题](https://github.com/kubernetes/kubernetes/issues/12430)，尽管有些事情可以逐案检查。

Fairwinds Insights 有两个可定制的 OPA 策略，用于检测已有一定天数未更新的导航图和部署，这极大地帮助了检测陈旧资源所涉及的辛劳和手动工作。

* * *

Kubernetes 治理和安全平台 Fairwinds Insights 可以帮助完成 NSA 的许多最重要的指导方针。利用 Fairwinds Insights，结合其他同类最佳的商业和开源软件，可以帮助组织遵守 NSA 的建议。

[![Steps to Meeting NSA Kubernetes Hardening Guidelines  How to comply with NSA’s recommendations using Fairwinds Insights, open source and cloud native technologies](img/8972892aec3f4935ca3ce07b3077605b.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/abedc766-9d54-4068-8387-d11bb1fa97c7)