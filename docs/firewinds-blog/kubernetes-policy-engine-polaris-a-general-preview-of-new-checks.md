# Kubernetes 策略引擎，北极星:新检查的一般预览

> 原文：<https://www.fairwinds.com/blog/kubernetes-policy-engine-polaris-a-general-preview-of-new-checks>

 [Polaris](https://polaris.docs.fairwinds.com/) 是 Kubernetes 的开源策略引擎，用于验证和修复 Kubernetes 资源。它包括 30 多种内置配置策略，并能够使用直观的 JSON 语法编写定制策略。

我们已经为 Kubernetes 策略引擎添加了额外的检查。我将简要地预览一下新的内置 Polaris 检查，它确定了人类或工作负载可能会过多访问 Kubernetes 的五个领域。在以后的博客文章中，我将深入总结这五种新检查。

## 1.ServiceAccount 令牌会自动装载

集群中运行的工作负载也可以访问与人类交互的相同的 Kubernetes API。工作负载可以被授予 Kubernetes 权限，允许检查不相关的 Kubernetes 资源，特别是如果它们共享一个 Kubernetes ServiceAccount。该检查检测自动拥有可用于访问集群的凭据的工作负载。这对于帮助防止对群集的未授权访问非常重要。

## 2.伴随网络策略

使用 Kubernetes NetworkPolicy 资源以及支持的容器网络接口(CNI)可以在工作负载和退出 Kubernetes 集群之间建立防火墙流量。这种访问控制有助于确保容器可以与所需的网络资源通信，并且不能到达其他网络。该检查确保每个工作负载都有一个命名空间 Kubernetes 网络策略。这一点很重要，因为防火墙工作负载内部通信提高了安全性。

## 3.强化 Linux 工作负载

默认情况下，工作负载可能比运行其应用程序所需的能力更强。调整这些权限可以最大限度地降低容器危害的范围、速度和影响。该检查确保工作负载使用 AppArmor、dropping Linux 功能、SELinux 或 seccomp 配置文件来授予工作负载运行所需的最低权限。这一点很重要，因为最小化工作负载的权限也会最小化攻击者获得其他工作负载或集群访问权限的能力。

## 4.执行或连接到 Pod

通过在现有容器内运行命令，可以执行额外的代码，从而允许对工作负载或集群进行更深入的访问。在生产集群中限制使用像“kubectl exec”或“kubectl debug”这样的命令是一个很好的做法。该检查检测允许执行或附加到容器的基于角色的访问控制(RBAC)。这一点很重要，因为限制运行其他命令的能力可以保护您的工作负载免受未经授权的访问或泄漏。

## 5.群集管理权限

授予对 Kubernetes 集群的管理访问权有助于您的团队在开始时更快地行动，但是这个决定扩大了潜在攻击者的机会和范围。通过检测允许集群管理权限的基于角色的访问控制(RBAC ),该检查有助于实现最低权限的安全原则。这一点很重要，因为应该谨慎授予集群管理权限，并在破碎玻璃的情况下使用。

如果这些新的[北极星](https://polaris.docs.fairwinds.com/)检查引起了你的兴趣，也许作为你的 [NSA Kubernetes 强化](https://www.fairwinds.com/kubernetes-nsa-hardening-insights)之旅的一部分，请留意未来的博客帖子，这些帖子将深入研究每一项检查。

寻找一个完整的 Kubernetes 治理平台？Fairwinds Insights 是免费的。 [今天就开始](https://www.fairwinds.com/coming-soon) 。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)