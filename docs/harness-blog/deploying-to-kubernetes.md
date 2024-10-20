# 部署到 Kubernetes | Harness 的开发者指南

> 原文：<https://www.harness.io/blog/deploying-to-kubernetes>

这里是 Kubernetes 上的 411 作为开发人员将应用程序部署到容器环境的术语和概念。

在 Harness，我们喜欢完成任务。事情直到交付才算完成。因此，为了帮助你实现这一点，这里有一个快速的 4-1-1，告诉你开发人员在 Kubernetes 上部署你的应用需要知道些什么。

## 进入容器:

**容器**打包您的应用程序和运行它所需的一切。这包括任何应用程序依赖项或文件以及应用程序的运行时环境。

容器，顾名思义，就是便携的。您可以在不同的环境和不同的基础设施中旋转容器，而不会听到“嗯，它在我的机器上工作。”这也是容器和虚拟机之间的主要区别。VM 就像船，集装箱就像船。你可以把船放在船上，但是你不能把船放在船上。

[VMs vs Containers](https://www.redhat.com/en/topics/containers/whats-a-linux-container)

容器是图像的运行实例。一个**容器映像**包含应用程序的源代码、库和依赖项。如果您的应用程序使用 ssh，那么您应该确保容器映像安装了 ssh。图像就像模板，您可以向它们添加层，以向您的容器添加附加功能。

容器图像存储在公共或私有存储库的注册表中，如 [Docker Hub](https://www.docker.com/products/docker-hub) 。通常，您从存储库中下载一个映像，或者构建自己的容器映像，并使用它来启动容器。

容器技术的生态系统继续增长。Docker 和 [Buildah](https://buildah.io/) 是两种帮助构建轻量级容器的技术，可以让你的应用程序在整个组织中有效地伸缩。以下是来自 [TechBeacon](https://techbeacon.com/enterprise-it/30-essential-container-technology-tools-resources-0) 的 30 种不同集装箱技术的列表。

## 装运集装箱:

Kubernetes 是一个用于编排和管理容器的工具。容器平台还有助于构建、部署和管理您的容器。在这样的生态系统中，当使用容器时，您需要知道一些额外的事情。

[Kubernetes Architecture](https://thenewstack.io/kubernetes-an-overview/)

[Pods](https://kubernetes.io/docs/concepts/workloads/pods/pod/) 将您的容器分组，以允许它们在您的基础设施上运行。**吊舱**是由 Kubernetes 创建和管理的最小可部署单元。Kubernetes 将分配 pods 在由 Kubernetes 管理的名为**节点**的机器上运行。

pod 可以包含一个容器(这是最常见的用例)，也可以包含多个容器。部署到同一个箱中的集装箱被称为**边车**集装箱。sidecar 模式确保容器在 pod 级别共享同一组资源。

一个[达蒙塞特](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/)部署经常被考虑并与边车模式相比较。一个 **DaemonSet** 是一个 Kubernetes 资源，它确保一个 pod 实例在集群中的所有节点上运行。DaemonSets 模式比 sidecar 模式消耗更少的资源，因为每个节点只有一个实例。

因为您有充当容器包装器的 pod，所以您可以使用 Kubernetes [**服务**](https://cloud.google.com/kubernetes-engine/docs/concepts/network-overview#services) 使 pod 可寻址。您可以使用 configure your container platform 来指定构建映像、部署和运行应用程序的方式。

## 之后会发生什么？

有几种方法可以确保您的应用程序运行良好。**健康检查**提供活性检查和就绪检查功能。活性探测器检查容器是否正在运行。准备就绪探测器确定容器是否准备好服务请求。准备就绪探测器可以配置为进行 HTTP 检查。通过这种方式，您将知道服务已经准备好接收流量。检查您的 [Kubernetes 文档](https://kubernetes.io/docs/concepts/workloads/pods/pod-lifecycle/#container-probes)中可用的探针。

我最后的指导是关于性能的。在节点上运行的每个容器消耗 [**计算资源**](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/#meaning-of-cpu) 。如果您注意到性能或功能下降，请确保您的 pod 有足够的分配的 **CPU 资源和内存资源**单元。每个 pod 在一个节点上的内存和 CPU 消耗都是有限的。在某些情况下，如果 pod 超过了内存限制，它们就会被终止。资源疏忽的一个典型例子是 Jenkins 的一个实例执行构建很慢，因为它的 CPU 限制是 500m CPU 或半个内核。

## 结论

技术的未来将包括构建在 Kubernetes 之上的易于使用的工具和平台，供开发人员快速开发和部署代码。考虑到这一点，这篇博文旨在以开发人员的身份在容器环境中开发和部署应用程序时，解决 Kubernetes 的术语和概念。

我期待分享更多 Kubernetes 概念和容器原生应用开发(CNAD)内容，包括[十二因素应用](https://12factor.net/)和[秘密](https://kubernetes.io/docs/concepts/configuration/secret/)管理。如果你想了解 Harness 如何将应用程序部署到 Kubernetes，请点击[这里](https://harness.io/kubernetes/)。同时，继续编码！