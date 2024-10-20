# NSA 强化指南:Fairwinds Insights 根除不良 Pod 安全性的三种方式

> 原文：<https://www.fairwinds.com/blog/three-ways-fairwinds-insights-can-root-out-poor-pod-security>

 美国国家安全局发布了一套严格的准则来强化您的 Kubernetes 集群。这份 66 页的文档概述了一种强大的深度防御方法，以最大限度地降低违规的可能性，并确保在攻击者设法渗透到您的 Kubernetes 集群的情况下，爆炸半径保持尽可能小。但更棘手的部分是学习 ***如何*** 这些准则，现在被认为是行业最佳实践的黄金标准，可以通过使用类似[fair winds Insights](https://www.fairwinds.com/insights)的 SaaS 解决方案、开源工具和其他云原生技术来实现。

这个致力于 pod 安全的博客开启了一个由五部分组成的系列，重点关注新的 [NSA Kubernetes 强化指南](https://media.defense.gov/2021/Aug/03/2002820425/-1/-1/0/CTR_Kubernetes_Hardening_Guidance_1.1_20220315.PDF) ，包括如何在任何运行容器化工作负载的组织中优化网络隔离、身份验证、授权、审计日志记录和威胁检测等关键安全领域。[](https://media.defense.gov/2021/Aug/03/2002820425/-1/-1/0/CTR_Kubernetes_Hardening_Guidance_1.1_20220315.PDF)

作为遵守 NSA 最佳实践的实用指南，本博客系列将讨论从业者如何利用 Fairwinds Insights，结合其他同类最佳的商业和开源软件，有效且经济地遵守 NSA 的严格建议。

> 想要立刻得到所有美国国家安全局的建议？下载我们最新的白皮书， [满足 NSA Kubernetes 强化准则的步骤。](https://www.fairwinds.com/kubernetes-nsa-hardening-insights)

## **围绕 pod 安全，NSA 有哪些建议？**

> ***NSA:以非根用户身份使用构建的容器运行应用。*** 具体来说，NSA 建议使用为非根用户运行应用程序而构建的容器。默认情况下，许多容器服务作为特权根用户运行，这意味着应用程序经常作为根用户在容器内执行，尽管不需要特权执行。通过使用非根容器或无根容器引擎来防止根执行限制了容器危害的影响。

Fairwinds Insights 与 [北极星](https://www.fairwinds.com/polaris) 集成，后者是一个用于验证 Kubernetes 配置的流行开源项目。这个开源工具带有一个内置的检查，用于检测允许以 root 用户身份运行的容器。通过采用北极星 via Fairwinds Insights，用户可以精确地看到集群中的哪些工作负载有权以 root 用户身份运行。此外，您可以使用 Insights 在 CI/CD 管道中运行相同的策略，确保基础架构代码更改不会引入能够以 root 身份运行的新资源。

一旦您锁定了在所有适用工作负载中以 root 用户身份运行的能力，我们建议您在 Insights 许可控制器中打开 Polaris，它可以为以 root 用户身份运行的工作负载提供最强的防御。

对于真正需要 root 访问权限的工作负载，可以配置 Insights 以允许特定豁免。为此，我们强烈建议使用允许列表，并在默认情况下拒绝以 root 用户身份运行。您可以根据名称空间、标签、注释、集群名称等来调整洞察力以做出决策。

> ***NSA:用不可变的文件系统运行容器。*** 默认情况下，容器被允许在它们自己的上下文中不受限制地执行。在容器中获得执行权限的攻击者可以在容器中创建文件、下载脚本和修改应用程序。Kubernetes 可以锁定容器的文件系统，从而防止许多利用后的活动。

同样，Polaris 通过对可写文件系统的内置检查解决了这一问题，该检查集成到 Fairwinds Insights 中，其方式与上述对以 root 身份运行的容器的检查相同。在 Insights 集群内代理中启用这一开源工具，用户可以检测哪些工作负载当前能够写入本地文件系统，哪些已经锁定。Insights CI/CD 插件还有助于防止任何允许工作负载修改文件系统的代码更改。

准入控制器功能提供了强有力的防御，防止违反此策略的任何新资源被添加到集群中。 如果某些特定的工作负载确实需要修改文件系统的能力，Insights 应该配置一个允许列表，默认情况下拒绝可写文件系统。这些允许列表可以基于名称空间、标签、注释、集群名称等等来构建。

> **NSA:扫描集装箱图像寻找可能的漏洞。** 除了使用可信存储库来构建容器，图像扫描也是确保部署的容器安全的关键。在整个容器构建工作流程中，应扫描图像以识别已知的漏洞。

Insights 还与 [Trivy](https://insights.docs.fairwinds.com/technical-details/reports/trivy/) 集成，这是另一个扫描所有容器图像以发现已知漏洞的开源工具。Trivy 检查图像，将其与大型 CVE 数据库进行比较，并按照严重程度列出所有问题。Insights 在构建时(即 CI/CD 期间)和运行时扫描容器。这一点很重要，因为通常在 映像被扫描并部署到您的环境后会宣布 *，这意味着成功通过 CI 和准入步骤的容器现在正在您的集群中运行，并且已知易受攻击。Insights Agent 可以检测这些情况，并向您的安全和运营团队发出警报。*

NSA 还建议实施一套“可信存储库”,以防止工作负载部署不可信的容器。Fairwinds Insights 与 [OPA](https://github.com/open-policy-agent/opa) 集成，允许用户创建自定义策略，精确指定允许哪些存储库。这些 OPA 策略可在 CI/CD 和 Insights 准入控制器中实施，并可用于使用集群内代理扫描所有现有资源的违规情况。

> 点击 [这里](https://www.fairwinds.com/blog/here-is-an-overview-of-our-open-source-projects-for-kubernetes) 阅读我们所有的开源项目！

## **免费使用 Fairwinds 洞见**

不要相信我们的话——今天就使用 的见解 ,了解您的组织如何以更少的恶化和总体成本达到 pod 安全的黄金标准。免费提供！[点击此处了解详情。](/coming-soon)