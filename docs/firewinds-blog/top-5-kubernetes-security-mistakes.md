# 发现你(可能)犯的五大 Kubernetes 安全错误

> 原文：<https://www.fairwinds.com/blog/top-5-kubernetes-security-mistakes>

 毫不夸张地说，云原生技术正在彻底改变组织开发和交付应用的方式。随着组织越来越多地采用微服务和容器，许多组织转向 Kubernetes 进行容器编排。Kubernetes 控制云应用和微服务的资源分配和流量管理，为全天候运行应用提供关键能力。K8s 支持自动扩展、自动恢复等功能。虽然 Kubernetes 的好处令人印象深刻，但许多组织都在为五个常见的 Kubernetes 安全错误而努力。你的组织知道吗？

## Kubernetes 对组织的挑战

Kubernetes 很复杂，在团队[对他们的 Kubernetes 环境获得信心](/kubernetes-maturity-model/phase-4-build-confidence)之前，需要大量的学习和实践。如果你刚刚起步，你可能缺乏成功启动 ***安全*** Kubernetes 环境所必需的工具、流程和经验。不仅如此，在开发、运营和安全团队中必须进行相当大的文化变革，因为 Kubernetes 和 containers 提供了一种部署应用程序的新方法。这些变化意味着，当组织采用微服务、容器和 Kubernetes 来开发和部署应用程序时，运营和安全团队会质疑应用程序和数据是否安全。

## 云环境中的安全性

在云原生模型中，许多传统的安全工具和流程不再是正确的选择，同时，容器会产生新的盲点和攻击面。获得您需要的跨容器和集群的可见性带来了额外的挑战。在新的范例中，开发人员可能会发现现在有必要对一些新的安全挑战负责，这是大多数开发人员不习惯并且可能不愿意承担的角色。

那么大多数组织最常犯的 Kubernetes 安全错误是什么呢？

*   **授予对主机节点的访问权限—** 授予应用程序管理员级别的访问权限很容易，但这会增加您遭受攻击的风险。
*   **假设运营团队与安全团队保持一致—** Kubernetes 提供了许多配置选项，这些选项提供了安全团队需要理解的大量灵活性 **—** 和复杂性 **—** 。
*   **运行具有已知漏洞的容器—** Kubernetes 使用容器来交付应用程序，但许多团队并不知道这些容器中可能暴露的已知漏洞。
*   **期望使用本地控件实现默认安全—** 虽然 Kubernetes 确实提供了本地安全特性，但许多特性在默认情况下并未启用。

这五个错误可以通过在开发和生产环境中持续扫描集群来避免。识别他们是成功的一半。学习如何识别错误并补救它们。要开始提高 Kubernetes 的安全性，请详细了解您可能犯的五大错误，并获取修复它们所需的信息。[阅读白皮书](https://www.fairwinds.com/top-five-kubernetes-security-mistakes-0-0)。

寻找一个完整的 Kubernetes 治理平台？Fairwinds Insights 是免费的。今天就开始吧。

[![The Top 5 Kubernetes Security Mistakes You Are Probably making](img/9133465fd4e699bbd4f6fdac84ce8018.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/eae00e98-7ed3-42f8-8ea9-885755ea79ac)