# 2023 年 Kubernetes 基准报告:您的 Kubernetes 工作负载相比如何？

> 原文：<https://www.fairwinds.com/blog/2023-kubernetes-benchmark-report>

 [Kubernetes](https://kubernetes.io/) 的采用正在增长，随着组织越来越多地使用 [](https://kubernetes.io/) it 来自动化容器化应用的部署、管理和扩展，更多的团队开始关注其工作负载的可靠性、安全性和成本效益。去年，Fairwinds 基于对超过 100，000 个 Kubernetes 工作负载的评估，创建了一份新的 Kubernetes 基准报告，旨在帮助组织更好地了解他们的容器配置，并将他们的结果与同行的结果进行比较。今年，Fairwinds 根据 2022 年的数据(超过 150，000 个工作负载)更新了 [基准报告](https://www.fairwinds.com/kubernetes-config-benchmark-report) ，并与上一年的数据进行了比较，以分析过去一年的情况如何改善(或没有改善)。

## 2023 年 Kubernetes 最关心的是什么？

Kubernetes 安全性、成本效益和可靠性仍然是 [云原生用户](https://www.cncf.io/) 最关心的问题。您的开发团队现在想知道的是，他们如此努力地建立他们的 Kubernetes 环境，并努力正确地配置他们的工作负载，与您在云原生生态系统中的同行相比，您的配置看起来如何。

根据基准数据，人们仍然没有将 Kubernetes 配置为符合 [最佳实践](https://www.fairwinds.com/blog/intro-kubernetes-best-practices) 。不幸的是，这转化为不必要的风险、云成本超支，甚至(潜在的)由于糟糕的应用程序性能而失去客户。最新的 Kubernetes 基准测试报告可以帮助您更好地了解将时间和金钱用于改进配置的最佳途径。

## DevOps 队寡不敌众

数百家组织正在使用[fair winds Insights 平台](https://www.fairwinds.com/insights) ，在 2022 年运行超过 15 万个工作负载。通过根据可靠性、安全性和成本效率指标评估基准测试集群的结果，我们可以看到情况实际上变得更糟，而不是更好。2021 年的数据显示，许多结果只有不到 10%的工作负载受到影响，但今年所有核心问题都发生了变化。为什么会这样呢？

随着 Kubernetes 使用的扩展，DevOps 管理新团队引入的潜在配置风险变得更加困难。换句话说，他们寡不敌众——Kubernetes 的采用速度超过了他们应用必要的策略和控制的能力。我们需要通过自动化 [Kubernetes 治理](https://www.fairwinds.com/blog/what-is-kubernetes-governance) 和 [护栏](https://www.fairwinds.com/kubernetes-guardrails-explained-reg) 来帮助他们，而不是依赖手动流程并试图引用和交叉检查策略并强制执行它们。

无论您是在使用云原生技术开发应用和服务，还是在构建基础架构的开发运维团队和平台工程团队中，您都需要一个高性能、安全且经济高效的平台。

我们今年再次将 [基准报告](https://www.fairwinds.com/kubernetes-config-benchmark-report) 分为三个板块:可靠性、安全性、成本效率。您在哪些方面落后了，如何改进您的 Kubernetes 配置？

## 可靠性

Kubernetes 的可靠性会受到六个关键配置问题的影响，其中许多问题很难解决。不容易知道为每个应用程序分配什么值，导致没有限制、限制不足，或者在测试期间限制设置得太高且从未纠正。今年，只有大约 17%的组织为超过 90%的工作负载设置了内存请求和限制，与去年的 41%相比大幅下降。同样，许多组织缺少活性和准备就绪探测器，依赖于 Docker 映像 的 [缓存版本，并且缺少 CPU 请求和限制。今年新增，我们现在检查只有一个副本的部署，因为副本部署有助于保持容器的稳定性和高可用性。](/blog/kubernetes-basics-tutorial-how-to-set-image-pull-policy-to-always)

## 安全性

我们在今年的报告中评估了九种不同的安全相关 Kubernetes 配置，不仅显示了组织去年的表现，还显示了今年的一些严重波动。2021 年，42%的组织关闭了大部分工作负载的不安全功能，但令人惊讶的是，今年只有 10%的组织关闭了这些功能。最新报告显示，33%的组织有超过 90%的工作负载使用不安全的功能运行。

另一个重要的安全设置是[readOnlyRootFilesystem](https://www.fairwinds.com/blog/dont-forget-to-check-kubernetes-pod-securitycontext-for-readonlyrootfilesystem)，它防止容器写入其文件系统。在最新的报告中，在安全性方面有另一个重要的转变，这表明组织可能没有意识到需要覆盖 Kubernetes 中不安全的默认设置。该报告还显示了容器特权运行能力的一些令人惊讶的变化。本报告还涵盖了允许容器以 root 用户身份运行、受映像漏洞影响的工作负载、过时的舵图、过时的容器映像和过时的 API 版本。

## 成本效率

当谈到 [成本效率](https://www.fairwinds.com/kubernetes-cost-optimization) 时，似乎大多数组织在设置 CPU 请求和限制时都做得不错。很少有人将工作负载限制设置得过高，将这些限制设置得过低的人就更少了。在成本效率方面需要改进的地方包括更仔细地设置内存限制-许多人将内存限制设置得太高，30%的组织看到至少 50%的工作负载受到影响，从而浪费了大量的云资源。在某些情况下，相当多的人将内存限制设置得太低，这会降低集群的可靠性，并可能导致应用程序失败。该报告还强调了内存请求设置过低或过高的地方，有多少工作负载受到影响，以及如何将这些请求调整到合适的大小。

## Kubernetes 基准测试报告的关键要点

容器和 Kubernetes 技术可以为企业带来巨大的价值，但是如果不了解这些基本配置以及如何适当地设置它们，就很难驾驭 Kubernetes 固有的复杂性。该报告有助于您更好地了解您的集群缺陷，在哪里进行投资，以及如何配置 Kubernetes 以产生积极的业务影响。通过查看这些数据，您可以了解并做出调整，以改进您的 Kubernetes 部署，并确保您的应用程序和服务在未来一年尽可能可靠、安全和经济高效。

阅读 [完整的 Kubernetes 基准报告](https://www.fairwinds.com/kubernetes-config-benchmark-report) 今天。

[![Just Posted: Kubernetes Benchmark Report 2023](img/ac3f87548e14243ac3414b06e023028d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/df6709b9-c7f5-4c1a-8bf4-315f56b16325)