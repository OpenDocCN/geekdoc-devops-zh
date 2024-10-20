# Kubernetes 问题解决

> 原文：<https://www.fairwinds.com/blog/kubernetes-problem-solving>

 毫无疑问，运行 Kubernetes 时，你会花大量的时间来解决问题。我们的团队在许多不同的 Kubernetes 部署中解决了很多问题。以下是一些常见问题以及我们如何解决它们。

## 1.卵死亡荚

## 在 Kubernetes 集群中，内存的底层资源是有限的，当内存不足时，底层操作系统可以终止容器。根据进程对饥饿内存资源的使用是否超过请求以及优先级，对进程的终止进行排序。

为了避免问题，您需要设置内存请求和限制，以适应应用程序在其生命周期中的使用。如果您的内存限制太低，您将熟悉 OOMKilled 作为一种状态，它显示您的 Pod 因使用太多内存而被终止！但是如果你把你的限制设得太高，你会因为过度分配而浪费金钱。

理想情况下，您希望将内存请求和限制设置在“刚好够用”的最佳位置。您将需要检查所有 pod 以检查内存请求和限制。手动执行此操作会花费大量时间，并且容易出现人为错误。我们通过使用 [Goldilocks](https://github.com/FairwindsOps/goldilocks) 来解决这个问题，这是一个开源工具，可以帮助团队将资源分配给他们的 Kubernetes 部署，并正确地校准这些资源。

降低成本是 Kubernetes 的众多好处之一，您应该对预期成本有所了解。如果到了月底，你的账单显示费用飞涨，那么你可能会有问题。

**2\. Higher than expected costs**

要解决这个问题，您必须将建议成本与实际成本进行比较。许多组织将其 CPU 和内存请求和限制设置得太高，但不知道从哪里进行更改。如上所述，我们使用 Goldilocks 来审查请求和限额。不过，对于成本分析，我们使用[fair winds Insights](https://www.fairwinds.com/insights)，它吸收金发女孩数据，帮助我们区分哪些工作负载需要调整请求和限制。我们还使用 Cluster Autoscaler 来确保任何多余的节点在未使用时被删除，从而节省时间和金钱。

**3。知道什么时候更新**头盔

## 修补 Kubernetes 附加组件通常并不困难。另一方面，跟踪何时更新可能很困难。虽然 Kubernetes 定期按季度更新，但掌舵图的更新难以监控和预测。我们的 Fairwinds 团队使用 [Nova](https://www.fairwinds.com/blog/nova-monitor-helm-for-new-releases) ，这是一个开源的命令行界面，用于交叉检查您集群中运行的最新版本的舵图。Nova 会让您知道您运行的图表是否过期或被弃用，因此您可以确保您始终了解更新。如果使用 Fairwinds Insights，该工具可以接收 Nova 数据，并在有新更新时提供警报，而不必定期运行 Nova。

**4。处理突发流量**

## 无论你是突然被病毒感染还是遭到 DoS 攻击，Kubernetes 都提供自动缩放功能，因此你的应用不会失败。虽然这对于病毒式传播来说是件好事，但如果发生 DoS 攻击，那就非常糟糕了。

在您的应用以及服务网格或入口控制器中设置速率限制。这将防止任何单个机器使用太多的带宽。例如，nginx-ingress 可以限制每秒或每分钟的请求、有效负载大小以及来自单个 IP 地址的并发连接数。

*   运行负载测试，了解应用程序的伸缩性。您将希望在一个临时集群上设置您的应用程序，并对其进行流量处理。测试分布式 DoS 攻击有点困难，因为流量可能同时来自许多不同的 IP，但是有一些服务可以帮助解决这个问题。
*   利用 Cloudflare 等第三方服务来提供 DoS/DDoS 保护。
*   回到第一点，您需要确保您有正确的限制请求，以确保您的常规流量能够到达您的服务。

5.缺少标签

## 容器注册通常允许您在推送容器时覆盖标签。这种情况的一个常见例子是最近的标签，它甚至可以自动设置为匹配最后推送的标签。在生产中使用这个标签是一种危险的做法，因为它可能会出现这样的情况:您不知道您的容器中正在运行什么代码，并且最终可能会无意中在生产中运行多个版本的代码。为了帮助跟踪这样的问题，我们使用了 [北极星、](https://github.com/FairwindsOps/polaris/blob/master/docs/check-documentation/images.md) 另一个开源工具，它支持许多与 pods 指定的图像相关的检查。使用时，如果标签丢失，北极星将显示 `images.tagNotSpecified` 或 `images.pullPolicyNotAlways.`

对于那些没有资源来处理所有这些开源项目的人，我们已经将它们全部合并到 [Fairwinds Insights](https://www.fairwinds.com/insights) 中，这是一个配置验证工具，它将扫描您的集群，并检查可能会耗费您的资金、使您易受攻击或导致停机的配置。

对使用 Fairwinds Insights 感兴趣吗？免费提供！ [在此了解更多](https://www.fairwinds.com/coming-soon) 。

> 你可以通过在 GKE、AKS 或 EKS 上建立一个集群来检验和尝试它，看看它如何帮助解决 Kubernetes 的问题。

[![Managing Kubernetes Configuration Read the Whitepaper](img/34937a5ae51bc0ceb94a21b3fd2382d0.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/1ccd0525-c794-4194-8aad-fc9663bb9c5a)

[![Managing Kubernetes Configuration Read the Whitepaper](img/34937a5ae51bc0ceb94a21b3fd2382d0.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/1ccd0525-c794-4194-8aad-fc9663bb9c5a)