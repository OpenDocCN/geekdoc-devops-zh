# 如何设置正确的 Kubernetes 资源限制

> 原文：<https://www.fairwinds.com/blog/kubernetes-resource-limits>

 Kubernetes 是一个动态系统，可以自动适应您的工作负载的资源利用率。

Kubernetes has two levels of scaling:

1.  水平 Pod 自动缩放器(HPA)——每个单独的 Kubernetes 部署都可以使用水平 Pod 自动缩放器(HPA)自动缩放。HPA 监控部署中各个单元的资源利用率，并根据需要添加或删除单元，以将每个单元的资源利用率保持在指定的目标范围内。
2.  集群自动伸缩——通过观察集群的资源利用率来处理集群本身的伸缩，并自动向集群添加或删除节点。如果您的资源请求设置不正确，集群自动缩放器将很难完成它的工作。它依赖于调度器来知道一个 pod 不适合当前节点，并且它还依赖于资源请求来确定添加一个新节点是否允许 pod 运行。

Kubernetes 支持这两种扩展操作的一个关键特性是能够为您的工作负载设置特定的资源请求和限制。通过设置合理的 Kubernetes 请求并限制每个 pod 使用多少 CPU 和内存，您可以确保平稳的应用程序性能并最大限度地利用您的基础设施。

## 正确设置 Kubernetes 资源限制

CPU 和内存的资源请求和限制是 Kubernetes 调度程序能够很好地完成工作的核心。如果允许一个单元消耗所有的节点 CPU 和内存，那么其他单元将会缺乏资源。

正确设置 Kubernetes 资源请求和限制非常重要。对应用程序设置太低的限制会导致问题。例如，如果你的内存限制太低，Kubernetes 一定会因为你违反了它的限制而杀死你的应用程序。同时，如果你把你的限制设置得太高，你会因为过度分配而自然地浪费资源，这意味着你最终会有更高的账单。

## 寻找缺失的 Kubernetes 资源限制

您的第一步是确定任何缺失的 Kubernetes 资源限制。Fairwinds 提供了一个开源工具 [北极星](https://polaris.docs.fairwinds.com/checks/efficiency/) 来检查丢失的请求和内存限制。当用户管理几个集群时，Polaris 运行良好。

如果您负责管理跨许多团队的多个集群，您可能希望查看 Fairwinds Insights，它可以在集群之间一致地应用 Polaris。

一旦发现缺少 Kubernetes 资源限制和请求，就需要对它们进行设置。但是如前所述，将它们设置得太低或太高都是一个问题。

## 如何设置 Kubernetes 资源限制

说“只设置 Kubernetes 资源限制”很容易，但是知道如何设置它们以便你的应用程序不会崩溃或者你浪费钱是很重要的。开源工具 [【金凤花](https://goldilocks.docs.fairwinds.com/) ，帮助你确定 Kubernetes 资源请求和限制的起点。

通过在推荐模式下使用 kubernetes vertical-pod-auto scaler，您可以在我们的每个应用程序上看到资源请求的建议。该工具为命名空间中的每个工作负载创建一个 VPA，然后查询它们的信息。

同样，如果您想在多个团队和集群中应用这一点，除了提供更多 Kubernetes 成本洞察之外，使用 Fairwinds Insights 还可以帮助您。最新功能 [Kubernetes 成本分配](https://www.fairwinds.com/blog/you-can-now-accurately-measure-application-costs-on-kubernetes-using-your-aws-bill) ，允许您为为集群供电的节点配置每小时混合价格。有了这些信息，并使用 Prometheus 指标跟踪实际的 pod 和资源利用率，您就可以生成工作负载的相对成本。这一举措有助于跟踪您的云支出，同时还能确定哪些功能会影响云成本。

总的来说，最重要的是你要合理确定资源限制。不要被抓到，把他们摆正！

您可以通过我们的 [沙盒环境](https://insights.fairwinds.com/orgs/centaurus/clusters/alpha-staging/overview) 游览 Fairwinds Insights，阅读我们的 [文档](https://insights.docs.fairwinds.com/first-steps/cost-efficiency/#viewing-workload-costs) 或[随时免费使用](/coming-soon) [](https://www.fairwinds.com/fairwinds-insights-trial)。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)