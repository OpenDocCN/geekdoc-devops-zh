# 金发女孩 3.0.0 的新功能

> 原文：<https://www.fairwinds.com/blog/new-in-goldilocks>

 我们最近发布了 Goldilocks 的 3.0.0 版本，有很多非常大的变化。我想花一点时间展示哈里森·卡茨在过去几个月里为实现这一目标所做的辛勤工作。

## 什么是金发女孩？

如果你正在阅读这篇文章，而你还没有听说过 [Goldilocks](https://www.fairwinds.com/blog/introducing-goldilocks-a-tool-for-recommending-resource-requests) ，它是一个工具，允许你查看关于设置 Kubernetes 部署的资源请求和限制的建议。它通过管理垂直 pod 自动缩放器(VPA)的控制器来实现这一点，并提供一个总结这些 VPA 中包含的建议的仪表板。 [GitHub](https://github.com/FairwindsOps/goldilocks)

## **什么变了？**

我使用的大多数集群都在 10-50 个节点的范围内，只有十几个名称空间。这意味着，在最初测试和实现 Goldilocks 时，我没有大量的数据来筛选，然后将它们呈现给仪表板。Harrison 发现，在拥有数百个名称空间和数百个垂直 Pod 自动缩放(VPA)对象的大型集群中，仪表板将需要很长时间才能加载，有时甚至根本无法加载。

为了解决这个问题，金发姑娘不得不经历许多变化。最值得注意的是，从事物的前端来看，您现在只看到一个名称空间列表。您可以一次查看所有名称空间，也可以深入查看各个名称空间。

我们在 3.0.0 中做的最后一个重大改变不是金发女孩本身，而是我们部署金发女孩的方式。在旧图表中，[vertical-pod-auto scaler](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler)(VPA)可以通过钩子安装，这些钩子调用 VPA repo `hack `目录中的 bash 脚本。这导致了许多不同的问题，并且没有选项来配置垂直 pod 自动缩放器。对于这个版本，我们创建了一个 [VPA 图表](https://github.com/FairwindsOps/charts/tree/master/stable/vpa)，你可以使用它来安装 VPA 控制器和必要的资源。该图表允许定制您的 VPA 安装。

## **流程**

Goldilocks 是我用 Go 编写的第一个大项目。因此，当我回头看代码时，我会质疑我做某些事情的方式以及许多代码的组织方式。我最初写它是为了让我自己的生活变得更容易，当金发女孩得到如此多的反馈和采纳时，我真的很惊讶。

因此，当 Harrison 找到我们，告诉我们他和他的同事们已经解决了围绕大型集群的许多主要问题时，我欣喜若狂。然后，我们开始了一长串的拉式请求、审查、修改和相互等待。总的来说，对我们的一个开源项目做出如此大的外部贡献是一次美妙的经历。很高兴看到公司给他们的工程师时间和空间来为他们认为有价值的开源项目做贡献。

## **结论**

Goldilocks 3.0 在具有大量名称空间的大型集群中表现更好，可以查看单个名称空间的资源建议，并且安装 VPA 更加简单。

Goldilocks 代码库中正在发生大量伟大的事情，我很高兴看到它的未来走向。再次感谢哈里森的辛勤工作和耐心。

## **资源**

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)