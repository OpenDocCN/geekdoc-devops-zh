# 受监控的基础设施并不好玩...但这很关键

> 原文：<https://www.fairwinds.com/blog/monitored-infrastructure-is-not-fun-but-its-critical>

 [缩放](http://blog.reactiveops.com/the-importance-of-scaling-infrastructure-the-right-way)和[自动化](http://blog.reactiveops.com/why-automated-infrastructure-is-key-and-how-it-saves-time)只有在详细监控到位时才起作用。监控是至关重要的，但是很多公司都没有时间去做。监控可能很复杂，考虑风险管理很困难(也不好玩)。

什么会出错？很多。这个想法会让人不知所措。

## 奠定基础

一个建议是从小处着手。要有一个适当监控的基础设施，从基线开始会有所帮助，然后每当发生一些事情时，您可以根据需要添加一个新的监控端点。例如，  [AWS 停运](https://aws.amazon.com/message/41926/)，就是一次很好的监控演练。每当一家公司或该公司的客户注意到应用程序存在问题时，就有机会将未来的监控端点整合到基础架构中。

**对于服务器实例，您可以做一些基本的检查:**

1.  我的实例启动了吗？( [Kubernetes](https://kubernetes.io/) 可以为你监控服务器状态。)
2.  我的实例/服务器是否有足够的资源——磁盘空间、CPU 使用率和 RAM 使用率。(Kubernetes 也可以帮你监控这些资源。)
3.  我的所有集群节点可以互相通信吗？

能够区分基础设施问题和应用程序问题非常重要。因此，您需要网络和应用程序监控。将服务器日志和集群日志放在一个集中的日志记录位置，这与记录应用程序出现的所有错误一样重要。

## 基础设施建设得当

虽然自动化和监控的重要性已经得到了很好的理解，但是并不总是能够实现足够的自动化和监控。另一方面，扩展是大多数公司都会犯的错误。因为他们不明白如何适当地扩展，所以公司经常用硬件来解决问题。这样的解决方案既不便宜也不高效。

将注意力转向 Kubernetes 的公司很快了解到它的扩展能力以及充分利用资源带来的相关节约。这些都是很大的卖点。Kubernetes 可以更快地运行您的应用程序，并且易于部署，但它还有其他功能:它可以帮助您以最有利的方式安排和使用您的资源。

集装箱化，尤其是 Kubernetes，吸引了许多公司。但是，如果这些公司试图在内部实现 Kubernetes，他们往往最终会雇用人员，并让这些资源承担学习复杂新技术的耗时任务。(然后他们希望这些人会留下来。)这就是有经验的 DevOps 合作伙伴的用武之地，他们可以帮您减轻负担，降低成本。

总之，Kubernetes 可以为您照顾您的基础设施[自动化](http://blog.reactiveops.com/why-automated-infrastructure-is-key-and-how-it-saves-time) 和监控需求。合适的 DevOps 合作伙伴可以实施战略解决方案，以确保您的扩展、自动化和监控完全按照预期方式运行。在合作伙伴的帮助下，您可以确保您的 Kubernetes 基础设施运行正常。