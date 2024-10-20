# 集群运行状况检查:可靠性

> 原文：<https://www.fairwinds.com/blog/cluster-health-checkup-reliability>

 一个站点可靠性工程师非常关心 Kubernetes 环境中的可靠性，你可能已经从职位名称中猜到了。可靠性对软件组织来说很重要，因为它提供了稳定性和更好的用户体验。此外，它使在组织中工作的操作员和开发人员的工作变得更加容易。

在 Kubernetes 环境中，可靠性可能很容易获得，但是如果配置不正确，它就会成为一个深远的目标。在 Fairwinds，我们做了很多事情来确保我们的 Kubernetes 集群对我们的客户来说是可靠和稳定的。这可能涉及对应用程序更改以及群集配置更改的建议。在接下来的几篇文章中，我将概述其中的一些内容，以及 [Fairwinds Insights](/insights) 可能会有所帮助。

## **资源请求和限制**

对 CPU 和内存的资源请求和限制是 Kubernetes 调度程序能够很好地完成工作的核心。

当调度程序决定应该在哪个节点上调度一个 pod 时，它会查看该 pod 的资源请求，然后将该 pod 放置在将提供它所请求的资源的节点上。这确保了 pod 能够运行它需要运行的任何工作负载。如果未设置资源请求，或者设置不正确，这可能会导致 pod 被放置在资源不足的节点上，并且工作负载不会尽可能高效地运行(或者可能根本不运行)。

限制用于防止 pod 在消耗一个节点上的所有可用资源时干扰其他 pod。这被称为“噪音邻居问题”如果允许单个 pod 消耗所有节点 CPU 和内存，则其他 pod 将没有可用的资源。如果你对一个豆荚能消耗的东西设置限制，那么你就能阻止这种事情发生。

## 需要关于资源请求和限制的帮助吗？

[Fairwinds Insights](/insights) 可以使用一款名为 [Goldilocks](https://github.com/FairwindsOps/goldilocks) 的开源工具来帮助你进行这些设置。Goldilocks 使用由 [垂直窗格自动缩放器](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler) 提供的建议，在 Insights 中创建建议新资源请求和限制的行动项目。它通过创建一个 VPA 对象来实现这一点，该对象反过来查看您的工作负载的历史内存和 CPU 使用情况来确定建议。

> 对使用 Fairwinds Insights 感兴趣吗？免费提供！点击了解更多[。](/coming-soon)

一旦设置好资源请求和限制，就可以开始为应用程序和集群启用自动伸缩了。接下来，我们将讨论 Fairwinds Insights 如何帮助自动缩放。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)