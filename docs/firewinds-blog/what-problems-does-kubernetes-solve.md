# Kubernetes 解决什么问题？

> 原文：<https://www.fairwinds.com/blog/what-problems-does-kubernetes-solve>

 本系列面向刚接触库伯内特和 GKE 的工程师。它提供了 Kubernetes 的基本概述、[定义](/blog/getting-started-with-kubernetes-architecture-basics-definitions)和[在 GKE](/blog/how-to-build-a-kubernetes-cluster-in-gke) 构建 Kubernetes 集群的快速入门，以及构建您的第一个多层 webapp 的研讨会。如果您正在寻找更深入的 Kubernetes 最佳实践和帮助，[请联系](/contact-us)。

在我们开始构建您的第一个 GKE 集群之前，了解一些关于 Kubernetes 的事情很重要。

## **什么是 Kubernetes？**

Kubernetes(K8s)是一个开源系统，用于自动化部署、扩展和管理容器化的应用程序。来源: [Kubernetes.io](https://kubernetes.io/)

## Kubernetes 是如何产生的？

2005 年，谷歌推出了博格系统。它开始是一个两三个人一起工作的小项目。这是一个大规模集群管理和资源调度系统，它引入了:

*   准入控制——允许在集群中调度什么工作
*   过度承诺的装箱——我们如何让多个系统和流程在单个节点上运行，而没有干扰
*   流程级资源隔离——如果在单个节点上调度一个容器，如何确保流程需求不会干扰另一个节点的流程需求

Kubernetes 把所有这些都变成了宣言。这意味着您可以获取需要调度的工作负载，在 YAML 文件中定义它们，将它们提交给 API，API 会告诉您它是否能够调度工作。

## **我们今天所知道的库伯内特人**

2014 年，谷歌推出了 Kubernetes 作为 Borg 系统的开源版本。2015 年，谷歌与 Linux 基金会联合成立了[云原生计算基金会](https://www.cncf.io/) (CNCF)。同年举行了第一届 KubeCon 活动，CNCF 开始了 Kubernetes 的季度发布周期。

## 【Kubernetes 解决什么问题？

### **用户期望应用和服务全天候可用** 

当您使用像 Kubernetes 这样的容器 orchestrator 时，您可以跨多台机器在不同时间调度节点或流程。这允许您丢失一个节点或进程，而不会中断服务的正常运行时间。

### **开发人员希望在一天内多次部署代码而不停机** 

如果您是系统操作员，开发人员希望您给他们一天多次部署代码的机会。Kubernetes 允许您在不停机的情况下实现滚动更新的智能流程和方案。

### **企业渴望更高效地利用云资源** 

您可以在一个节点上调度多个进程，而不是在一个全天候付费的云节点上运行一个进程。您的云节点还可以识别何时无法调度新流程并因此需要新资源，或者何时资源空闲并需要关闭。Kubernetes 提供了切换这种弹性的简单方法。

### **容错和自愈基础设施提高可靠性** 

Kubernetes 提供可靠性。如果一个容器或整个节点出现故障，Kubernetes 将在一个健康的节点上重新调度资源或单个进程。

### **在节点和容器(pod)范围内自动水平缩放** 

Kubernetes 允许您创建新的节点，并自动将它们添加到集群中。如果单个服务受到资源限制，Kubernetes 会检测到这一点，并调用新的实例来处理额外的负载。

[![Download the Kubernetes Best Practices Whitepaper](img/ff6b63b515c18edd13b80bc25f17c2de.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/e68d92d3-c876-4525-b775-6123e46c7212)