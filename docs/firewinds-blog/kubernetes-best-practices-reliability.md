# Kubernetes 可靠性最佳实践

> 原文：<https://www.fairwinds.com/blog/kubernetes-best-practices-reliability>

 许多 Kubernetes 的采用者试图利用早于云原生应用程序和 Kubernetes 的现有基础设施自动化技术，如 Puppet、Chef、Ansible、Packer 和 Terraform。以非云本机方式使用这些工具不会产生最佳结果。例如，使用配置管理工具来构建容器映像会为您的应用程序部署增加不必要的时间和复杂性，这会影响您的灵活性和恢复能力。

通过正确的配置，Kubernetes 的可靠性变得更加容易实现。随着您采用容器、Kubernetes 和云原生架构，您使用前 Kubernetes 工具的程度和方式可能会发生变化。

## Kubernetes 可靠性最佳实践

Kubernetes 环境中的可靠性等同于稳定性、简化的开发和操作以及更好的用户体验。

> 在 Kubernetes 中，很容易配置错误。部署正确的配置以确保稳定性、简化的开发和操作以及更好的用户体验。
> 
> **[点击发微博](https://ctt.ac/dFhHB)**

在 Kubernetes 环境中，通过正确的配置，可靠性变得更加容易实现。在这里，我推荐四个 Kubernetes 最佳实践技巧来提高可靠性。你可以在这里阅读[我的完整推荐。](https://www.fairwinds.com/kubernetes-best-practices-comprehensive-white-paper)

### 秘诀 1:拥抱短暂

使用云原生架构来帮助拥抱容器和 Kubernetes pods(应用程序容器的运行实例)的短暂本质。两个例子包括:

1.  使用服务发现来帮助用户和其他应用程序访问您的应用程序。Kubernetes pods(运行容器)的数量会随着您的应用程序扩展以满足需求而变化，服务发现提供了一种无需知道它们的位置就可以访问这些 pods 的方法。
2.  不要试图修改现有的容器，而是从其容器映像中提取应用程序配置，并通过 CI 管道构建和部署新的容器映像。容器是短暂的，在应用程序容器中运行配置管理软件会增加不必要的复杂性和开销。关于从容器映像中分离应用程序配置的更多信息，请参见我们的 [How to Kube 视频。](https://www.youtube.com/watch?v=K-KQwBsbaGo&feature=youtu.be&utm_content=115660200&utm_medium=social&utm_source=twitter&hss_channel=tw-3002222549)

### 技巧 2:避免单点故障

Kubernetes 通过提供冗余组件来帮助提高可靠性，并使跨云中多个节点和多个可用性区域(AZs)调度应用程序容器成为可能。使用反亲缘关系或节点选择来帮助将您的应用程序分布在 Kubernetes 集群中，以实现高可用性。

节点选择允许您根据标签定义集群中哪些节点有资格运行您的应用程序。标签通常代表节点特征，如带宽或特殊资源，如 GPU。

反相似性允许您基于标签的存在，进一步约束不允许您的应用程序运行的节点。这使得您的应用程序容器不能在同一节点上运行，或者不能与同一应用程序的其他组件在同一节点上运行。点击阅读更多容错建议[。](https://www.fairwinds.com/kubernetes-best-practices-comprehensive-white-paper)

### 技巧 3:设置资源请求和限制

对 CPU 和内存的资源请求和限制是 Kubernetes 调度程序完成工作的核心。如果允许单个 pod 消耗所有的节点 CPU 和内存，那么其他 pod 和潜在的 Kubernetes 组件将会缺乏资源。对 pod 消耗设置限制将通过防止 pod 消耗节点上的所有可用资源来增加可靠性(这被称为“噪音邻居问题”)。

### 技巧 4:使用活跃度和就绪性探测器

默认情况下，Kubernetes 将立即开始向应用程序容器发送流量。通过设置健康检查来提高您的应用程序的健壮性，健康检查会告诉 Kubernetes 您的应用程序 pods 何时准备好接收流量，或者它们是否变得没有响应。参见[我对设置这些探头](https://www.fairwinds.com/kubernetes-best-practices-comprehensive-white-paper)的建议。

如果您想对构建可靠的集群进行更深入的分析，请查看我们的 [Kubernetes 最佳实践](https://www.fairwinds.com/kubernetes-best-practices-comprehensive-white-paper)。您还可以阅读更多关于[安全性](https://www.fairwinds.com/blog/kubernetes-best-practices-for-security)和[效率](/blog/kubernetes-best-practice-efficient-resource-utilization)的最佳实践。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)