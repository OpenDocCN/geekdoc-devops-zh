# 为什么你需要 Kubernetes 安全政策的执行

> 原文：<https://www.fairwinds.com/blog/addressing-kubernetes-security-vulnerabilities-with-policy-enforcement>

 保护 Kubernetes 是一个大话题，许多供应商都在解决这个问题。我们看到的安全挑战之一不仅仅是漏洞，而是执行策略来防范漏洞和其他安全问题。容器或底层 Kubernetes 基础设施中的错误配置可能会产生问题。

考虑一下我们看到的一些安全挑战，以及为什么需要执行 Kubernetes 策略。

## 应用程序漏洞

根据 DZone 的调查，78%的公司现在在 OSS 上运行部分或全部业务(2010 年上升到 42%)。而建立在开源项目上的 Kubernetes 社区肯定更高。问题是这些开源工具中的已知漏洞可能会被无意中注入到应用程序或容器中。

工程团队需要扫描容器的能力，以识别具有已知漏洞的 CVEs 和/或 OSS 组件/版本。然后，开发人员需要升级或修补这些组件来解决漏洞。

如果存在已知的漏洞，制定策略来防范这些漏洞是非常重要的。然而，执行是许多团队失败的地方。如何确保在不断变化的动态环境中应用所有容器策略？使用[策略驱动的配置验证](https://www.fairwinds.com/insights)可以帮助识别哪里存在可能暴露 CVE 的错误配置。

## 平台漏洞

同样，底层 Kubernetes 集群和附加组件中也可能存在漏洞。需要不断地扫描和监控基础设施以发现新的漏洞，并根据需要修补以解决问题。

## 适当的权限

黑客使用的一种常见攻击手段是利用能够访问超出他们实际需要的系统资源的用户或服务，例如利用权限提升、“根”访问等。基于角色的访问控制(RBAC)可以强制实施最小特权的概念，即只授予对用户或服务所需资源的访问权，而不是更多。然而，要知道 Kubernetes 部署是否被超级用户过度许可，需要负责安全的团队手动检查每个 pod，以检查错误配置的部署。这个过程受益于贯穿整个开发生命周期的自动检查，以确保授予正确的特权。

## 入口/出口控制

当应用程序服务与应用程序内部或外部的其他资源进行通信时，还必须采取适当的安全措施来管理入站和出站通信。策略决定了允许哪些数据到达哪里，以及允许哪些服务相互通信。与 RBAC 类似，最佳实践是为网络和权限建立一个“零信任模型”,使通信只在需要的地方发生。这些政策必须实施。在 Kubernetes 中，策略即代码是最好的选择，但是面临的挑战是如何检查策略是否已经应用到每个集群。同样，如果没有自动化，这是一个耗时且容易出错的过程。

## 证书管理

SSL 证书用于加密网络流量，以保护传输的数据。这些证书需要轮换/更新/管理，以确保数据得到正确加密。

在 Kubernetes 中，cert-manager 作为一系列部署资源在集群中运行。它利用 `CustomResourceDefinitions` 来配置认证机构和请求证书。这种定制应该对照策略进行检查，以确保 `CustomResource` 具有从特权到能力等所有正确的安全检查。

## Kubernetes 通过 Fairwinds Insights 实施安全策略

为了应对 Kubernetes 政策执行方面的挑战，我们开发了 [Fairwinds Insights](https://www.fairwinds.com/insights) 。Fairwinds Insights 基于我们的开源工具，包括[北极星](https://www.fairwinds.com/polaris)，是一个策略驱动的 Kubernetes 配置验证平台。它确保在整个开发生命周期中，根据安全策略和其他最佳实践检查容器和 pod。这意味着用户不会意外地将群集暴露给 CVE，权限与策略一致，并且整个环境遵守策略。Fairwinds Insights 不仅使用 Kubernetes 策略执行来提高安全性，还为用户提供了效率和可靠性优势，以便集群能够适当扩展以避免停机，并控制成本。

您可以在自己的集群中免费体验 Fairwinds Insights！点击了解更多[。](/coming-soon)

[![Address Kubernetes Policy Enforcement with Fairwinds Insights Free Trial](img/c7d1bab0fbdcf4906582b7502017e792.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/962d56bc-4a0a-4243-85ef-9bb92f499f18)