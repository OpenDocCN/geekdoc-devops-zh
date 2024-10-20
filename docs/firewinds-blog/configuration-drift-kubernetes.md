# Kubernetes 中的配置漂移——它是什么，为什么重要

> 原文：<https://www.fairwinds.com/blog/configuration-drift-kubernetes>

 大多数组织都是从一个应用程序开始试验的。一旦他们通过了一个成功的试点并接受了 Kubernetes，公司可能会建立几十个集群来支持几十个团队。对于使用 Kubernetes 部署多个应用程序的大中型公司来说，这也意味着开发和运营团队也在采用它，通常是在自助服务模式中。当您在构建和部署许多不同集群的过程中有许多用户时，确保一致、安全地部署应用程序并满足适当的资源需求就变得非常困难。

## 什么是配置漂移

配置漂移是指在一个环境中，[基础设施](/blog/why-infrastructure-as-code-kubernetes)中运行的集群随着时间的推移变得越来越不同，这通常是由于对单个集群进行手动更改和更新。设置一致的自动化集群供应流程有助于确保集群在创建时是[一致的](/blog/why-fixing-kubernetes-configuration-inconsistencies-is-critical-for-multi-tenant-and-multi-cluster-environments)，但不会阻止在该集群或其他集群上发生更改。对配置参数的更改可能由开发团队、运营团队或开发团队完成。

## 为什么配置漂移很重要

当您开始操作大量手动部署且配置不一致的集群时，几乎可以肯定您的容器和集群之间的配置会有差异。这使得识别不一致并纠正它们变得相当困难；与配置漂移相关的重大负面后果包括:

*   [**安全漏洞:**](/kubernetes-security) 错误配置可能导致权限提升、易受攻击的映像、来自不可信存储库的映像或以 root 身份运行的容器。
*   [**效率问题:**](/kubernetes-cost-optimization) 当工作负载过度调配或过时的工作负载未得到清理时，成本可能会攀升。
*   [**可靠性风险:**](/blog/kubernetes-best-practices-reliability) 扩展不足或扩展过于频繁会导致您的应用或服务停机。

试图手动跟踪配置偏差并修复错误配置非常容易出错，并且会很快导致运营团队花费太多时间来跟踪问题。

## 跟踪您的指标的工具

为了跟踪基本的[指标](/blog/kubernetes-best-practices-monitoring-alerts)，大多数公司需要准备好工具，以便了解他们正在运行的 Kubernetes 版本，以及支持 Kubernetes 的关键系统软件的版本，如入口控制器、证书管理、DNS 等等。能够找到并看到所有软件版本信息非常重要，因为这有助于您的组织将所有软件的*升级到最新的稳定版本，从而帮助您避免技术债务。你不希望运行旧版本的 Kubernetes，特别是因为旧版本的 Kubernetes 和你正在运行的插件可能不安全，增加了你遭受网络攻击的风险。*

 *## 不一致的负面后果

配置漂移还会导致许多不一致，这看起来可能没有那么糟糕，但会对您的升级过程产生重大影响。当集群的配置不一致时，随着时间的推移，运行 Kubernetes 会变得更加昂贵，因为这意味着您需要单独研究每个升级路径。这可能会增加升级过程的大量时间，并导致时间和运营资源的大量浪费。当您能够拥有一致的基础架构时，这意味着您可以一次性研究您的升级和修补方案，并在多个环境中统一应用它。

## 多云环境中的配置漂移

较大的公司开始考虑[多云](/blog/how-to-operate-kubernetes-in-a-multi-cluster-multi-cloud-world)场景，这使他们能够利用不同云提供商的优势。这对于 Kubernetes 来说不成问题，因为 Kubernetes 可以在多个云上使用。Kubernetes 的好处在于，它为跨所有这些云运行基础设施提供了一致的 API。当您试图一致地应用策略并从这些不同的云提供商处获取有关您的集群状态的信息时，挑战就来了。对于 DevOps 团队来说，手动管理和洞察跨多个云和集群的配置漂移极具挑战性。

## Fairwinds 的多集群和多云计算管理

[Fairwinds Insights](/insights) 的重要功能之一是它支持多云用例。这有助于工程和开发团队更有效地管理多用户、多集群和多云环境，因为它支持许多组织正在考虑的多云部署，而不会失去全局管理配置的能力。为了保持部署的一致性，即使您跨多个云进行部署，将所有安全性、效率和可靠性信息汇总到一个位置也很重要，这样运营和安全团队就可以从一个视图管理所有配置，并降低配置偏差中固有的风险。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)*