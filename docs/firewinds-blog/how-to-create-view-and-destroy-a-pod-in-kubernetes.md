# 如何在 Kubernetes 中创建、查看和销毁 Pod

> 原文：<https://www.fairwinds.com/blog/how-to-create-view-and-destroy-a-pod-in-kubernetes>

 *2023 年 2 月 28 日更新*

在我们的 How-to-Kube 系列中，我们想从介绍 pod 基础知识开始。与服务、卷和名称空间一样，pod 是一个基本的 Kubernetes 对象。pod 是在作为一个单元的同一物理或虚拟机上调度的一个或多个容器的集合。当您创建 pod 的声明时，您可以用位于 pod 内部的任意数量的容器来定义它。

pod 还在内部共享一个网络——每当在 pod 内的所有容器之间调度一个 pod 时，就共享一个私有网络。他们还可以共享文件系统卷。与 Docker 使用- **volumes-from** 类似，当在一个 pod 中运行多个容器时，这与 Kubernetes 的概念相同。您可以在 pod 内共享临时或写入时复制样式的存储。

通常，您不会直接创建一个 pod——相反，您将创建一个更高级别的对象，如包含 pod 规范的 Deployment 或 StatefulSet(见下文)。

## **Kubernetes 部署**

部署是对 pod 的抽象。它允许您在 pod 上拥有额外的功能和控制，以说明您希望跨节点运行多少个 pod 实例，或者您是否希望定义滚动更新策略(例如，一次仅滚动一个 pod，中间等待 30 秒)。这使您能够根据自己的需求控制部署，以便在启用新流程和淘汰旧流程时实现零停机。

部署提供以下功能:

*   提供更高级别的 pod 抽象(定义一组相同的 pod)
*   可以扩展 pod 的副本以满足需求
*   负责创建、更新和删除窗格
*   可以向前或向后滚动

在这里，您可以看到这个部署称为“webapp”。它希望存在两个副本。

在这篇文章中，您将学习如何使用 nginx 图像在 Kubernetes 中创建一个 pod，查看描述 pod 的 YAML，然后删除您创建的 pod。我们将使用 Minikube 工具，使您能够在您的笔记本电脑或计算机上  [运行单节点 Kubernetes 集群](https://www.fairwinds.com/developer-hub/making-kubernetes-fully-operational) 。

> 要获得更多关于 Kubernetes 入门的帮助，请阅读我们为刚接触 Kubernetes 和 GKE 的工程师准备的系列文章。它提供了 Kubernetes 、[架构基础和定义](https://www.fairwinds.com/blog/getting-started-with-kubernetes-architecture-basics-definitions)的[基本概述，以及构建 Kubernetes 集群和](https://www.fairwinds.com/blog/what-problems-does-kubernetes-solve)[构建您的第一个多层 webapp](https://www.fairwinds.com/blog/how-to-deploy-multi-tiered-web-application-with-kubernetes) 的快速入门。

## 如何在 Kubernetes 中创建 Pod

首先，你需要  [启动一个 Kubernetes 集群](/blog/how-to-build-a-kubernetes-cluster-in-gke)(在 GKE)。一旦您进入了 Kubernetes 沙盒环境，确保您已经连接到了 Kubernetes 集群，方法是在命令行中执行 kubectl get nodes 来查看终端中的集群节点。如果成功了，您就可以创建和运行 pod 了。

要使用 nginx 映像创建 pod，运行命令ku bectl run nginx-image = nginx-restart = Never。这将创建一个名为 **nginx** 的 pod，与 Docker Hub 上的 [nginx 映像](https://hub.docker.com/_/nginx/)一起运行。 通过设置标志 - restart=Never 我们告诉 Kubernetes 创建一个单独的 pod，而不是一个部署。

一旦您按下 enter 键，pod 将被创建。您应该看到 pod/nginx 创建的出现在终端中。

## 如何查看 Pod

您现在可以运行命令 kubectl get pods 来查看您的 pod 的状态。要查看 pod 的整个配置，只需在您的终端中运行 kubectl describe pod nginx 。

终端现在将显示 pod 的 YAML，从名称 nginx、其位置、Minikube 节点、开始时间和当前状态开始。您还将看到关于 nginx 容器的深入信息，包括容器 ID 和图像所在的位置。

如果您一直滚动到终端的底部，您将看到 pod 中发生的事件。在本教程中，您将看到 pod 已经启动、创建，nginx 映像被成功提取并被分配给 Minikube 中的这个节点。

[![Related Resource: Kubernetes Best Practices Whitepaper](img/312bd1689890eb3c36d7b20b763f0e1c.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/73982414-9e2a-405e-8776-4b8369f49814)

## 摧毁吊舱

删除 pod 的操作很简单。要删除您创建的 pod，只需运行 kubectl delete pod nginx 。在按 Enter 键之前，请务必确认要删除的 pod 的名称。如果您已经成功完成了删除 pod 的任务， pod nginx deleted 会出现在终端中。

Pods 是理解 Kubernetes 对象模型的重要单元，因为它们代表了应用程序中的流程。在大多数情况下，pod 作为一种间接的方式来管理 Kubernetes 中的容器。在更复杂的用例中，pod 可能包含多个需要共享资源的容器，充当容器管理的中心位置。

> 要了解更多 Kubernetes 最佳实践，[获取本指南](/kubernetes-best-practices-comprehensive-white-paper)。它将带您了解您将面临的许多问题，以及如何配置 Kubernetes 以避免错误。

## 如何实施 Pod 策略

Kubernetes 的一个大问题是缺乏可见性和跨多个集群和开发团队的一致策略执行。当你开始你的 Kubernetes 之旅时，你应该考虑 [Kubernetes 护栏](/enforce-kubernetes-policy) -你将如何让你的团队安全地使用 Kubernetes？尽早这样做将确保您不会在没有为 Kube 配置建立内部标准的地方引入配置漂移。在您玩游戏时，查看一些 Kubernetes 安全注意事项:

1.  [检查 Pod SecurityContext 的 readOnlyRootFilesystem](https://www.fairwinds.com/blog/check-kubernetes-pod-securitycontext-for-readonlyrootfilesystem)

2.  [kubernets 如何:确保 imagePullPolicy 设置为 Always](https://www.fairwinds.com/blog/kubernetes-how-to-ensure-imagepullpolicy-set-to-always)

3.  [如何识别超限集装箱](https://www.fairwinds.com/blog/how-to-identify-over-permissioned-containers)

4.  [如何识别 Kubernetes 中缺失的就绪探测器](https://www.fairwinds.com/blog/how-to-identify-missing-readiness-probes-in-kubernetes)

5.  [为什么修复 Kubernetes 配置不一致对于多租户和多集群环境至关重要](/blog/why-fixing-kubernetes-configuration-inconsistencies-is-critical-for-multi-tenant-and-multi-cluster-environments)

## Kubernetes 针对安全性、可靠性和资源请求的最佳实践

正如您应该考虑护栏一样，您也应该考虑 Kubernetes 的最佳实践。通过在学习 Kubernetes 的同时学习最佳实践，您将能够充分评估该平台并对其进行扩展。

开源项目 Polaris 运行各种 Kubernetes 最佳实践检查，以确保 pod 和控制器配置正确。使用这个开源项目，您可以评估 Kubernetes 并避免将来出现问题。

开始使用 Kubernetes 时经常出现的另一个问题是如何确定应用程序的规模。开源项目， [Goldilocks](https://goldilocks.docs.fairwinds.com/) ，让你调整你的应用程序，让它“恰到好处”它帮助您确定资源请求和限制的起点。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)