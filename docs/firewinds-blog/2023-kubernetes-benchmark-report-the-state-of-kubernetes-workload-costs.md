# 2023 年 Kubernetes 基准报告:Kubernetes 工作负载成本状况

> 原文：<https://www.fairwinds.com/blog/2023-kubernetes-benchmark-report-the-state-of-kubernetes-workload-costs>

 组织继续向云迁移。事实上，根据 Flexera 的 [2022 年技术支出脉搏](https://info.flexera.com/FLX1-REPORT-State-of-Tech-Spend-Thanks?revisit) ，65%的受访者将云和云迁移作为下一年的首要任务。对于大多数人(74%)来说，数字化转型比以往任何时候都更加重要，这一举措正推动着向云迁移的意愿不断增强。然而，管理云和 Kubernetes 成本仍然是大多数组织面临的持续挑战，根据 CNCF 的 FinOps for Kubernetes 报告，“[Kubernetes 成本监控不足或不存在导致超支](https://www.cncf.io/wp-content/uploads/2021/06/FINOPS_Kubernetes_Report.pdf) ”那么，在分析 Kubernetes 工作负载的效率时，这看起来像什么呢？

使用 2022 年的数据，其中包括超过 150，000 个工作负载，Fairwinds 分析了当前趋势以及它们与上一年的比较，从而创建了 2023 年 Kubernetes 基准报告 。虽然 Kubernetes 的采用持续增长，但最佳实践对许多组织来说仍然具有挑战性。不符合最佳实践会导致云成本超支、安全风险增加以及云应用和服务缺乏可靠性。

为了确保您的 Kubernetes 集群尽可能高效，您需要正确地设置资源限制和请求。例如，如果您在应用程序上将内存请求和限制设置得太低，Kubernetes 会因为您的应用程序违反了限制而终止它。另一方面，如果您将限制和请求设置得太高，就会过度分配资源，从而导致更高的云账单。

那么，当谈到 [成本效率](https://www.fairwinds.com/blog/youre-not-monitoring-kubernetes-cost-and-you-should-be) 时，组织的发展趋势是正确的吗？让我们深入数据找出答案。

## CPU 请求和限制

当设置 [CPU 请求和限制](https://www.fairwinds.com/blog/how-to-correctly-set-resource-requests-and-limits) 时，有一个好消息。基准数据显示，72%的组织仅将 10%的工作负载限制设置得过高。只有 1%的组织有 91=100%的工作负载受到设置过高的 CPU 限制的影响。

![Graph showing 72% of organizations are setting up to 10% of their workload limits too high and 94% of organizations are setting 0-10% of workload limits too low](img/e8aafe81b58038a35de76eceabce67ae.png)

就 CPU 限制设置过低而言，94%的组织仅将 0-10%的工作负载限制设置过低。

## 内存限制太高

类似于 2022 年发布的基准测试报告，对于近 50%的工作负载，组织的 [内存限制设置过高](https://www.fairwinds.com/blog/kubernetes-resource-limits) 。但是，今年受影响的工作负载百分比有所增加。当我们查看 2021 年收集的数据时，只有 3%的组织有 51-60%的工作负载受到影响。这一数字今年大幅增长。现在，30%的组织至少有 50%的工作负载受到内存限制设置过高的影响。这相当于浪费了大量的云资源。调整这些内存限制，使其与工作负载需求保持一致，这将有助于您进行控制，并减少膨胀的云账单。

## 内存限制太低

有趣的是，看起来许多组织(67%，比去年的 70%略有下降)仍然对至少 10%的工作负载设置过低的内存限制。虽然受影响的工作负载数量相对较少，但一定要记住，将这些内存限制设置得太低会降低集群的可靠性。同样，你可以 [为你的应用调整这些限制](https://www.fairwinds.com/blog/setting-rightsizing-kubernetes-workloads) ，以确保它们不会在压力下失败。额外的好处:适当地调整这些可以帮助你确保你没有浪费你的云资源。

![Graph showing 67% of organizations are setting memory limits too low on at least 10% of their workloads](img/f447bef4fe19417fe08d870dd3747097.png)

发生内存不足错误(OOMKills)。为了减少卵杀死的潜在影响，[fair winds Insights](https://www.fairwinds.com/insights)包括一个检测和报告卵杀死的工具。这样，你就知道什么时候发生了，这样你就能迅速做出反应。如果您选择将其设置为允许的话，该工具还可以增加内存以避免停机。

## 内存请求太低

不出所料，应用程序的可靠性变得非常具有挑战性，当内存请求设置得太低时，这种挑战会变得非常快。好消息是，对 59%的组织的工作负载的分析表明，这个问题只影响了他们工作负载的 10%左右，这与 2021 年的数据分析结果相似。

如果您希望避免与设置过高或过低的内存请求相关的效率(和可靠性)问题，请寻找分析使用情况并提出建议的工具，以帮助您适当地调整内存请求。[](https://www.fairwinds.com/goldilocks)Goldilocks 是一款开源工具，可以帮助你决定你需要做哪些改变。如果您正在跨多个团队运行多个集群，请尝试 Fairwinds Insights 平台。我们的 [自由层](https://www.fairwinds.com/blog/try-fairwinds-insights-kubernetes-governance-free-tier) 非常适合最多 20 个节点、两个集群和一个存储库的环境。

## 内存请求过高

在这方面，34%的组织对至少 10%的工作负载设置了过高的内存请求。不幸的是，82%的组织现在将工作负载的内存请求设置得过高，这是一个显著的增长。数据分析显示，与前一年相比，更多工作负载受到过高内存限制的影响，这是一种不受欢迎的趋势。

## 关于 Kubernetes 工作负载成本的要点

虽然大多数与效率相关的 Kubernetes 工作负载设置与上一年相比没有显著变化，但它们并没有像您希望的那样朝着积极的方向发展。2023 年已经成为有趣的一年，重点是更仔细地分析成本，关注在哪里以及如何调整云支出，以最低的支出实现最大的可靠性。随着越来越多的组织将更多的应用和服务转移到云，密切关注工作负载消耗的资源量非常重要。请阅读完整报告，了解有关这些结果的更多信息，并深入了解安全性和可靠性的趋势。

阅读 [完成 Kubernetes 基准报告](https://www.fairwinds.com/kubernetes-config-benchmark-report) 今天。