# 使用 Saffire 消除作为 Kubernetes 单点故障的注册表

> 原文：<https://www.fairwinds.com/blog/eliminate-the-registry-as-a-single-point-of-kubernetes-failure-using-saffire>

 你遇到过这种情况吗？你正在经营一个由[法兰绒](https://github.com/flannel-io/flannel)作为 [CNI](https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/) 的 [kops](https://kops.sigs.k8s.io/) 集群，而 [Quay](https://quay.io) 经历了一整天的停电。突然，你的自动伸缩 Kubernetes 集群这台运转良好的机器嘎然而止。新节点从不加入集群，其他随机服务无法启动。

您已经成为 Kubernetes 集群中为数不多的单点故障的牺牲品:容器注册。这看起来似乎是一件奇怪的事情，要编写一个完整的工具来解决，但在 [Fairwinds](https://fairwinds.com) 这个问题让我们非常头疼，并在我们为客户运行的几十个集群中浪费了时间。如果今天它不是码头和法兰绒，明天它将是另一个集群关键服务。

## Saffire 简介

Saffire 是一个在集群中运行的控制器，它会监视在提取底层映像时遇到问题的 pod。当它找到这些窗格时，它将尝试修补拥有该窗格的对象，并用您指定的替换对象替换图像规范。因此，在映像拉取失败后的几秒钟内，您的工作负载就可以在从不同的注册表拉取的映像上愉快地运行了。

### 它是如何工作的？

Saffire 创建了一个您可以部署的自定义资源定义，它指定了被认为与该名称空间中的映像等效的注册表列表。再加上我的一个朋友兼同事写的一个名为 [controller-utils](https://github.com/FairwindsOps/controller-utils) 的包，Saffire 可以找到发生故障的 pod 的顶层控制器，并用新的图像源修补它们。

### 这难道不需要我做些计划吗？

是的！你将不得不同时把你所有的图像推送到两个存储库。你可能想知道为什么 [Fairwinds](https://fairwinds.com) 选择这种方法，而不是像拉通缓存或镜像我们所有图像的集群内注册表这样的东西。这里有几个原因:

1。我们希望避免运行我们自己的基础架构来解决这个问题。

我们一直在寻找一种 Kubernetes 本地方法，不需要我们运行长寿命、高维护的基础设施。

2。我们希望尽可能利用现有的服务。

Google Container Registry (GCR)、Amazon Elastic Container Registry(ECR)、Quay 和 Docker Hub 都是很棒的服务，它们相对来说很少出现宕机。有了 Saffire，你仍然可以使用这些服务，只是你不再依赖单一的服务。

## 
名字怎么回事？

## 在 Fairwinds，我们喜欢上了以太空为主题的名字。当我们最初将[命名为北极星](https://github.com/FairwindsOps/polaris)时，我们陷入了不断困扰库伯内特社区的航海主题。但是北极星让我们在海洋和太空之间架起了一座桥梁。从那以后，我们创造的所有工具都是以太空为主题的。

## Saffire 指的是美国宇航局为了测试火在宇宙飞船和空间站上的表现而进行的一系列任务。这似乎是一个合适的名字，一个在我们的 Kubernetes 环境中减轻火灾的工具。

## 
希望你喜欢

## 前往[Saffire](https://github.com/FairwindsOps/saffire)的 Github 仓库，如果您有任何问题，请告诉我们！

[![Fairwinds Open Source Contributions](img/7d97f9831d48f7363d5ce22f38471699.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/a23b9631-ae62-4d66-b1e9-55078f2a0481)