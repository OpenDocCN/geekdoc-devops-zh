# Kubernetes 控制器基础知识

> 原文：<https://www.fairwinds.com/blog/kubernetes-controllers>

 如果你对 Kubernetes 不熟悉，你首先要熟悉的概念之一就是控制器。大多数 Kubernetes 用户不直接创建 Pods 相反，他们创建一个部署、CronJob、StatefulSet 或其他控制器来为他们管理 pod。

## **控制器基础知识**

首先，这里有一些控制器的基础知识。Kubernetes 文档使用了一个例子，控制器就像你的恒温器。刻度盘的位置是它想要的状态，当前的温度是它的实际状态，恒温器不断地加热或散热以保持两者同步。这就是 Kubernetes 控制器的工作方式——它是一个循环，监视集群的状态，并根据需要进行更改，始终保持您想要的状态。

控制器可以跟踪许多对象，包括:

*   哪些工作负载正在运行，在哪里运行
*   这些工作负载可用的资源
*   围绕工作负载行为的策略(重启、升级、容错)

当控制器注意到实际状态和期望状态之间的差异时，它将向 Kubernetes API 服务器发送消息，以进行任何必要的更改。

## **控制器的类型**

在我们基本的 Kubernetes 概念中，我们强调了许多 Pod 控制器，包括以下内容。然后，我们将深入研究四个最常用的控制器。

*   replica set——replica set 创建一组稳定的单元，所有单元都运行相同的工作负载。你几乎不会直接创建它。
*   部署-部署是在 Kubernetes 上获取应用程序的最常见方式。它维护了一个具有所需配置的副本集，以及一些用于管理更新和回滚的附加配置。
*   stateful set-stateful set 用于管理具有持久存储的有状态应用程序。Pod 名称是永久性的，在重新计划时会保留(app-0、app-1)。存储保持与替换机架相关联，当删除机架时，卷仍然存在。
*   作业-作业创建一个或多个短期 pod，并期望它们成功终止。
*   cron job——cron job 按计划创建作业。
*   daemon set-daemon set 确保所有(或一些)节点运行一个 Pod 的副本。随着节点添加到集群中，单元也会添加到其中。随着节点从集群中移除，这些 pod 将被垃圾收集。常见于 CNI、监控代理、代理等系统进程。

## **复制集**

副本集是一组没有唯一标识的多个相同的 pod。复制集旨在满足两个要求:

*   容器是短暂的。当他们失败时，我们需要他们的吊舱重新启动。
*   我们需要运行一定数量的 pod。如果一个被终止或失败，我们需要新的豆荚被激活。

副本集确保指定数量的 Pod 副本在任何给定时间运行。尽管您可能永远不会直接创建副本集，但这是保持 Kubernetes 基础设施正常运行的重要部分。

## **部署**

像复制集一样，部署是一组没有唯一标识的多个相同的 pod。但是，部署比复制集更容易升级和修补。用户可以配置推出新版本部署的策略，从而可以在最短的停机时间内推出变更。

例如，我们可以选择滚动更新策略。然后，当我们更新部署时，将会在现有副本集旁边创建第二个副本集，随着新副本集中更多的 pod 准备就绪，一些副本集将会从旧副本集中删除，直到旧副本集为空并且可以删除。我们甚至可以设置像 [maxUnavailable 和 maxSurge](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-update-deployment) 这样的参数来控制这种切换的机制。

## **状态设置**

虽然部署管理无状态应用程序，但有状态集对于需要以下一项或多项功能的应用程序很有价值:

*   稳定、唯一的网络标识符
*   稳定、持久的存储
*   有序、优雅的部署和扩展
*   有序的自动滚动更新

StatefulSet 为它管理的每个 Pod 保留一个唯一的标识。每当它需要重新安排那些豆荚的时候，它使用相同的身份。

当运行 Cassandra、MongoDB、MySQL、PostgreSQL 或任何其他利用持久存储的工作负载时，建议使用 StatefulSets。它们可以在伸缩和更新事件期间帮助维护状态，对于维护高可用性特别有用。

## **工作**

作业是短期工作负载，可用于执行单个任务。作业将简单地创建一个或多个 pod，并等待这些 pod 成功完成。工作有助于做以下事情:

*   运行数据库迁移
*   执行维护任务
*   旋转原木

作业还可以并行运行多个 pod，为您提供一些额外的吞吐量。

## **CronJob**

CronJobs 只是按照用户定义的计划运行作业。您可以使用 [cron 语法](https://en.wikipedia.org/wiki/Cron)提供调度，CronJob 控制器将在每次调度需要时自动创建一个作业。

CronJob 控制器还允许您指定可以并发运行多少个作业，以及保留多少个成功或失败的作业用于日志记录和调试。

* * *

## **总结**

Kubernetes 控制器允许您在集群中运行和管理您的应用程序，并且是您需要理解的核心概念之一，以便在 Kubernetes 中获得成功。

当然，在您拥有一个生产就绪的集群之前，您还需要更多。如果您正在寻找创建一个完全可操作的 Kubernetes 集群，您可以[查看我们关于主题](https://www.fairwinds.com/blog/making-kubernetes-fully-operational)的博客帖子。或者，如果您正在学习 Kubernetes，请查看我们的 [How to Kube](https://www.fairwinds.com/blog/tag/how-to-kube) 系列，该系列展示了 Kubernetes 在概念、工具、安全性等方面的最佳实践。

[![Fairwinds Insights is Available for Free Sign Up Now](img/90e93a941f22f2087c3a229a91ea6c10.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/d329e036-9905-4715-85b8-31a98b50623c)