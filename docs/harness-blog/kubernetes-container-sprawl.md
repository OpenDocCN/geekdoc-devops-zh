# Kubernetes 容器蔓延是新的虚拟机蔓延|利用

> 原文：<https://www.harness.io/blog/kubernetes-container-sprawl>

容器蔓延是新的虚拟机蔓延。对于所有这些容器，我们需要一个解决方案来帮助大规模运行容器。欢迎来到 Kubernetes！

容器蔓延是新的虚拟机蔓延这一陈词滥调被证明是非常正确的。与物理服务器相比，当人们开始迁移到虚拟机时，启动虚拟机比购买物理服务器要快得多。因此，人们开始更加频繁地进行资源调配，很快就出现了过多的虚拟机。快进到容器革命，为应用程序进程旋转容器以开始加载通常需要几秒钟。从虚拟机时代开始，这种无序扩张就被放大了。随着图像和运行容器数量的激增，我们在本系列的第一部分中了解到, [Kubernetes 容器](https://harness.io/blog/what-is-kubernetes-container/)是不可变的，因此当您做出更改时，会创建新的图像和新的容器。我们有大量的容器，不仅要应对规模和可靠性，现在还要应对更多的变化。

## 祝你五岁生日快乐！

一个正在塑造我们今天的分布式系统的解决方案就是 Kubernetes，也就是 K8s。2014 年 6 月在[GitHub](https://github.com/kubernetes/kubernetes)上发布的 Kubernetes 在过去的几年里是最受欢迎的开源项目之一，直到最近才被 [TensorFlow](https://www.tensorflow.org/) 等机器学习包超越。Kubernetes 的血统可以追溯到两个大型谷歌平台，这两个平台对于谷歌实现今天的网络规模至关重要。Google Omega 和后来的 Google Borg 是开创性的集群管理解决方案，解决了影响 Kubernetes 项目的调度和资源管理问题。

## 调度程序+资源管理器= Kubernetes

Kubernetes 提供了一个处方来帮助解决两个基本的分布式系统问题。根据调度器的定义，调度器必须回答“运行多少”和“当我们没有那么多要运行时会发生什么”。根据资源管理器的定义，同样，资源管理器必须回答“运行到哪里”。将调度器和资源管理器结合在一起，您就有了一个现代的容器编排器。假设您在没有使用 orchestrator 的情况下开始了您的容器之旅。因为集装箱是被制造成死的，所以确定运行集装箱的最小和最大数量是一个很难解决的调度问题。此外，由于我们的基础架构变得更加分散，找到合适的位置来运行您的工作负载可能会很有挑战性。幸运的是，Kubernetes 在这里，我们可以与 Kubernetes 集群和一些 YAML 互动。要理解 Kubernetes 集群是如何组合在一起的，有几个重要部分可以分几个部分来描述。

## Kubernetes 积木

Kubernetes 的基础可以被描述为一个从主人到工人的模型。一个主人控制 n 个工人。就这么简单！虽然您可能会对 Kubernetes 提供的上百个包/平台感到困惑，但为什么我只列出了两个呢？Kubernetes 的设计是可插拔的，因此大多数部件都可以更换，从而改变了对您的 Kubernetes 集群如何解决问题的看法。数百个包/平台通常被部署/插入到主服务器或工作服务器中。管理集群主要有两种方式。有 [Web UI](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) ，有一个命令行界面【CLI】叫做 [KubeCTL](https://kubernetes.io/docs/reference/kubectl/overview/) 。这些接口通常与主设备对话。这位 [Kubernetes 大师](https://kubernetes.io/docs/concepts/)可以被看作是这次行动的主脑。许多重要服务的所在地，如[调度器](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-scheduler/)和 [API 服务器](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-apiserver/)。主服务器也是 etcd 的所在地，etcd 用于存储所有的配置和关于 Kubernetes 集群的重要状态信息。如果没有一些[工作节点](https://kubernetes.io/docs/concepts/architecture/nodes/)，你还没有一个 Kubernetes 集群。Worker Nodes 顾名思义就是您的工作将被执行的地方。您的应用程序位于这些节点上。除了您的应用程序之外，节点的数量也是成比例的。Worker 节点还包含帮助容器运行的部分，例如 [Docker 引擎](https://docs.docker.com/engine/)。几年前当我学习 Kubernetes 时，Worker 节点中的 [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod/) 的概念对我来说是一个难点，因为我不理解它的用途。我以前知道的唯一的豆荚是咖啡豆荚，我会用我的克里格战斗。Kubernetes 中非咖啡意义上的 pod 被设计为驻留在 Worker 节点中的容器的逻辑分组。这与那些通常拥有不止一个 [Kubernetes 容器](https://harness.io/blog/what-is-kubernetes-container/)【再次回到我们系列的第一部分】并且可以在[副本集](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)中伸缩每个 Pod 的应用程序有关。不算太差！你会发现 Kubernetes 是非常可配置的。有一些工具可以帮助我们跟踪并使我们的 Kubernetes 体验更具可重复性。

## 赫尔姆是什么鬼东西？

人们在 Kubernetes 集群上安装的第一个工具通常是名为 [Helm](https://helm.sh/) 的包管理器，这是第一个为 Kubernetes 设计的包管理器。赫尔姆研究的是[图表](https://helm.sh/docs/developing_charts/)的概念。Helm Chart 用更简单的术语描述了部署应用程序或模板所需的一组 Kubernetes 资源。自从 Helm 于 2015 年 10 月发布以来(一年后变更为 Kubernetes 项目)，Kubernetes 内部肯定有很多不同的配置管理方法。对于今天的配置管理方法，值得一提的是 [Kustomize](https://kustomize.io/) 和 [Kapitan](https://github.com/deepmind/kapitan) 。

## 到处都是 Kubernetes

随着 Kubernetes 生态系统的扩展和提供 Kubernetes 服务的提供商数量的增加，K8s 作为一个运行时变得越来越普遍。从使用 [Minikube](https://kubernetes.io/docs/tutorials/hello-minikube/) 【潜行巅峰到第三部分】在您的桌面上本地运行，到运行 Kubernetes 的云主机和[平台即服务](https://harness.io/blog/what-is-paas/)提供商，您有许多选择。毋庸置疑，Kubernetes 可能是混合云的重要推动者之一。尽管良好的分布式系统原则适用，并且都是关于工人和潜在的主人之间的延迟。对于如何扩展和链接绝望的集群有很多意见，这本身就可能是一个冗长的系列。

## Harness 和 Kubernetes——一起更好

Harness 为您的 Kubernetes 投资提供了一个强大的编排层。Harness 是在 Kubernetes 时代建立的，在 [Kubernetes 部署](https://developer.harness.io/docs/continuous-delivery/cd-advanced/cd-kubernetes-category/kubernetes-deployments-overview/)中提供了大量的覆盖面。我们的文档中有一个[优秀的 Helm 示例](https://docs.harness.io/article/zmrvylwqds-docker-kubernetes-and-helm)供您在 Kubernetes 之旅中参考。请继续关注第三部分，我们将打开 k8 并进行一些部署。拉维