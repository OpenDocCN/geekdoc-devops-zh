# 什么是 KSPM？Kubernetes 安全态势管理简介

> 原文：<https://www.fairwinds.com/blog/kubernetes-security-posture-management>

 随着 Kubernetes 的采用持续增长，我们已经看到 Kubernetes 的规模和复杂性增加，我们也看到了 Kubernetes 专业知识的短缺，特别是 Kubernetes 安全专家。为了帮助采用 [Kubernetes](https://kubernetes.io/) 的组织了解他们在成熟度方面处于什么位置(因为有很多东西需要学习)，我们最近推出了 [Kubernetes 成熟度模型](/kubernetes-maturity-model)，它强调了在采用 Kubernetes 时要经历的一些重要活动。同样重要的是要记住，Kubernetes 仍然是一项年轻的技术，尽管 Kubernetes 本身及其[生态系统](https://www.cncf.io/)正在迅速成熟。为了应对管理 Kubernetes 安全的挑战，出现了一个新的类别，称为 Kubernetes 安全态势管理(KSPM)。在 VMWare 的 2020 年 Kubernetes 状况报告中，他们显示 59%的受访者在生产中运行 Kubernetes，其中 20%拥有 50 个或更多群集。

*来源:* [*VMWare 的《库伯内特 2020 年的状态》*](https://k8s.vmware.com/state-of-kubernetes-2020/)

为什么关注 KSPM？本质上是因为很容易忽略安全性，或者认为它是默认内置的。当组织开始使用 K8s 时，他们的主要目标通常是快速部署应用程序，他们很少考虑所涉及的安全性问题。当主要目标是启动并运行以获得敏捷性并加快上市时间时，它可能会在云原生安全性方面留下真正的缺口。

## 云本机安全性

云原生基础设施不仅仅是在云中运行应用。这也是关于你如何建造它们。云原生技术(微服务、容器和 Kubernetes)使组织能够在现代动态云环境中构建和运行可扩展的应用，并帮助公司在云原生架构上构建和运行应用。这一转变有助于他们更快地将创新推向市场，并满足不断变化的客户需求和期望。

“云原生不在于您在哪里运营，而在于您如何运营。”–Joe Beda，VMware 首席工程师，Kubernetes 的联合创始人

虽然好处很多，但云原生也使得从一开始就将安全性构建到您的环境和应用程序中变得比以往任何时候都更加重要。与传统的基础设施不同，你不能在游戏后期增加安全性(虽然这也不太好)。从一开始就构建安全性意味着你必须确保拥有[正确且一致的配置](/manage-kubernetes-configuration-security-efficiency-reliability)，你已经从一开始就将你的 Kubernetes 环境设置为安全的(默认情况下它是不安全的)，并且你的团队遵循[最佳实践](/blog/kubernetes-best-practices-for-security)。

## Dev + Sec + Ops + Kube？

然而现实是，你的开发人员(可能)不是 Kubernetes 专家，你 ***不要*** ***想要*** 他们成为 Kubernetes 专家。您希望开发人员专注于编写代码和构建应用程序。这就是为什么向他们提供他们需要的信息(并且只提供他们需要的信息)非常重要的原因，这些信息与他们的应用相关，可以帮助他们做出正确的决策。同时，从安全性和法规遵从性的角度来看，在安全和运营团队之间就如何设置适当(或不适当)的策略提供高水平的可见性也很重要。我们经常谈论 DevSecOps，但是安全团队仍然经常被排除在 K8s 对话之外。不值得羡慕的结果是 Sec 团队必须努力工作才能赶上 K8s 的对话和需求。

## Kubernetes 的配置很复杂

配置 K8s 涉及许多不同的参数，由许多不同的用户设置，这造成了许多混乱和不一致。虽然处理配置可能会令人困惑，但一些基本的安全原则是相同的。例如，根据适用于 Kubernetes 集群和用户的最小特权原则，遵循限制权限的策略有助于最小化安全风险。挑战在于在 Kubernetes 集群中一致地实施这些原则和政策。创建标准化和定制的策略是重要的第一步，为您的环境创建最佳实践也至关重要，但要真正让这些策略坚持下去，[您需要实施它们](/blog/make-your-kubernetes-policies-stick-use-an-effective-enforcement-plan)。自动实施您的策略有助于您的组织避免安全事故、停机和跨多个群集和用户的不一致。

## KSPM: Kubernetes 安全态势管理

那么，到底什么是 Kubernetes 安全态势管理(KSPM)？与自动化安全云基础架构配置的云安全状态管理(CSPM)类似，KSPM 自动化安全 Kubernetes 基础架构配置。在整个流程中集成 KPSM 非常重要，这样您的应用程序和容器化的工作负载从一开始就可以安全部署..如果一开始就让安全团队参与进来，他们可以帮助编写安全的配置，了解配置问题，在开发过程的早期提供相关的安全反馈，并确保开发人员获得可操作的相关补救信息，以便他们可以更轻松地解决问题。

从一开始就应用策略有助于您防止问题发生，并通过让您团队中的更多人员(包括开发、安全和运营人员)实施安全性，而无需他们通过手动试错找出正确的方法，从而实现大规模的一致安全性。KPSM 支持安全的基础架构，同时支持快速上市，从开始到整个 SDLC 降低安全性和合规性风险。

Fairwinds Insights 可帮助您保护 Kubernetes 集群配置，使用策略自动应用最低权限访问，持续扫描和保护容器以发现和修复新的漏洞，并保持符合外部标准。Fairwinds Insights 还提供了数十种开箱即用的策略，您可以快速应用这些策略，在集群群、单个集群或单个命名空间或工作负载之间实施保护。该软件还可以接受为与开放策略代理一起使用而创建的任何现有的基于减压阀的策略。我们基于帮助各种规模的组织成功部署 Kubernetes 的长期经验创建了 Fairwinds Insights，它是利用我们的[开源工具](/open-source-software)和从构建安全、可靠和高效的实施中获得的来之不易的知识构建的。

> 您可以永远免费使用 Fairwinds Insights。 [拿到这里](https://www.fairwinds.com/coming-soon) 。

[![Fairwinds Insights is Available for Free Sign Up Now](img/90e93a941f22f2087c3a229a91ea6c10.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/d329e036-9905-4715-85b8-31a98b50623c)