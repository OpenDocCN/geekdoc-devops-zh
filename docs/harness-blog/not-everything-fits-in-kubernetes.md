# 并非所有东西都适合 Kubernetes | Harness

> 原文：<https://www.harness.io/blog/not-everything-fits-in-kubernetes>

在这个系列的第四部之后，我们在 Kubernetes 的世界里变得相当危险；我们了解 Kubernetes 中一些广泛使用的操作任务。很明显，下一步是将我们拥有的每一个应用程序都装进 Kubernetes，并开始给我们的供应商打电话要求部署。YAML:所以我们 100%都在库伯内特经营。当你开始你的 Kubernetes 之旅时，你会很快发现自信地利用一个快速移动的平台需要大量的思考；就在几年前，这还是意料之中的事情，现在正在发生变化。用三个项目符号总结博文(看到一个趋势？):

*   什么最有效:在 Kubernetes 时代构建的无状态工作负载。
*   什么效果好:在 Kubernetes 的现代构建的有状态/无状态工作负载。
*   不太好用的地方:Kubernetes 时代没有构建的有状态/可集群工作负载。

我父亲会说，当彩票头奖变得非常大时，“如果你没有彩票，你就没有机会”。在 Kubernetes 的世界里，“如果你没有一个容器，你就没有机会”。

## 你仍然需要一个容器(99%确定是 Docker)

如果我们把我们的知识回溯到几周前的第一部分，我们已经了解了这个容器是什么。作为复习，容器是不可变的(不是为改变而设计的)和短暂的(会死亡)。这种设计模式相对较新，但现代云原生平台和应用程序基础架构考虑了这种模式。大多数利用 Kubernetes 的人都使用某种 [Docker 图像](https://docs.docker.com/v17.09/engine/userguide/storagedriver/imagesandcontainers/)和 [Docker 引擎](https://docs.docker.com/engine/)来编排。还有其他的容器格式，如 RKT 的[和 Mesos 的](https://coreos.com/rkt/docs/latest/using-rkt-with-kubernetes.html)等，但是这些都被 Kubernetes + Docker 的组合所掩盖。下一个合乎逻辑的划分是棕地开发和绿地开发。建造新东西的好处是你可以为新的范例设计。与现有的又名棕色地带，你可以感觉到你是在一个圆孔中安装一个方钉。对于现有的应用程序，你当然可以进行一些调查，看看你是否能把你的应用程序归档。甚至在编写应用程序的语言/运行时方面，也有很多东西与你作对。JAVA 已经经历了一次转变，运行时间被更新以更好地考虑容器限制。有了 Kubernetes，拥有应用程序的多个实例就像增加副本数量一样简单。一些应用程序被设计为一次只能运行一个，并且没有考虑到分布式计算。

## 分布式计算的谬误

JAVA 的发明者詹姆斯·高斯林和彼得·德意志提出了分布式计算的八大谬误。总结一下这些谬误，有很多人在做一些看起来总是可用的事情和资源，比如带宽/网络是有限的。随着多个团队部署到 Kubernetes，我们都在集体学习，真的会加剧这些谬误。开发一个分布式系统有一系列独特的挑战，Kubernetes 不会自动神奇地解决它们，比如无延迟的谬论。Kubernetes 对我们来说是一个很好的平台，但这只是对分布式系统应该如何的一种解释。在 Kubernetes 范式之前构建的分布式系统很难嵌入到 Kubernetes 集群中。

## 我的集群需要 um a (Kubernetes)集群？

在 2015-2016 年期间，Kubernetes 遇到的第一批工作负载是无状态应用程序。尽管大多数企业应用程序需要管理状态，并且是有状态的。如果您不熟悉什么是有状态应用程序和无状态应用程序，请将无状态应用程序想象为天气应用程序，将有状态应用程序想象为银行应用程序。什么端点告诉我们天气并不重要，例如，我们可以请求“邮政编码为 94105 的地区的天气如何”并从任何节点获得相同的结果；那是些甜蜜的[等幂](https://en.wikipedia.org/wiki/Idempotence)！转移资金是另一回事。涉及到[分布式事务](https://en.wikipedia.org/wiki/Distributed_transaction)，端点需要知道其他节点在做什么，例如复制。我们可以向 Ravi 请求“金钱”,一个节点满足了这个请求，然后必须以某种方式将该信息保存到集群的其他节点，否则 Ravi 可能会支付两次。集群和复制对于应用程序来说并不陌生。如果你使用的是 JAVA 栈，[应用服务器](https://www.javaworld.com/article/2075019/j2ee-clustering--part-1.html)已经支持了近二十年。将 Kubernetes 排除在外，对于不久前还必须集群在一起的 JAVA 应用程序，应用服务器/内存解决方案将使用 [UDP](https://en.wikipedia.org/wiki/User_Datagram_Protocol) / [多播](https://en.wikipedia.org/wiki/Multicast)让成员节点找到彼此。让你的 [Kubernetes Worker 节点](https://kubernetes.io/docs/concepts/architecture/nodes/)相互允许 UDP 是一个考验。应用程序[服务器已经更新](https://medium.com/@andrevcf/wildfly-10-kubernetes-cluster-dee7d4d377c6)和工具，如[Weave works](https://www.weave.works/oss/net/)的 Weave Net，但这些都是生态系统中的新成员。我们可以看看更现代的解决方案，[阿帕奇卡夫卡](https://en.wikipedia.org/wiki/Apache_Kafka)。诞生于 LinkedIN 的 Kafka 是一个流媒体通讯平台，只比 Kubernetes 早两年。Kafka 背后的公司 Confluent 在过去的四年里一直在努力让他们的平台能够在 Kubernetes 上被大众消费。他们当然面临着集群机制和复制机制的挑战，行业和 Kubernetes 项目需要时间来构建解决方案。集群和复制只是有状态战斗的一半。第二部分是状态最终存在的地方，持久层。

## 害虫的持久性

在 Docker bloom 早期，人们开始关注存储问题。很明显，一旦容器死亡/恢复，容器内的挂载和卷将不再持久。一个好的设计应该具有在容器之外的存储卷，并且可以从外部管理。随着集装箱的激增，尤其是它们来来去去，确定存储已经成为一个挑战。在 Kubernetes 的早期，建议是需要持久性的平台(比如数据库)不能在 Kubernetes 上运行。具有非常具体的文件系统要求的高度专业化的数据库很可能运行在由 Kubernetes 管理的专门构建的集群上，甚至运行在裸机上。Docker 和 Kubernetes 都在发展，以更好地支持外部卷。像 [Portworx](https://portworx.com/use-case/kubernetes-storage/) 这样的专用解决方案正在帮助解决集装箱存储难题。[容器存储接口](https://github.com/container-storage-interface/spec) (CSI)也可以归因于增强存储选项和与容器生态系统的互操作性。随着生态系统的所有增强，收音机表盘似乎指向泛型。

## Kubernetes 是通用的

Kubernetes 要成为一名成功的编排者，就必须能够编排大量的工作负载，并捕获 Kubernetes 需要的最大工作量。减去[活跃度和准备就绪检查](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/)，在涉猎 Kubernetes 之后，你开始看到标志性的[宠物 vs 牛](https://medium.com/@Joachim8675309/devops-concepts-pets-vs-cattle-2380b5aab313)类比在起作用。如果你的牛生病了，买一只不像你心爱的宠物的新的；因此，更换一个不健康的容器比愈合。扩展领域有所增强，其中的[水平 Pod 自动缩放器](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) [HPA]允许利用容器资源来驱动 Pod 副本。HPA 是一个很好的机制，但是如果不加以检查，很容易耗尽您的集群容量。作为博客系列第六部分的一个提示，Kubernetes 生态系统中增加和扩展了一些新功能，使 Kubernetes 更具应用感知性/针对性。到目前为止，我们仍然在讨论 Kubernetes 可以采取行动的泛型。

## 集群感知和响应时间

对于 Kubernetes 来说，对故障或扩展触发器等事件采取行动确实需要一点时间来响应。水平 Pod 自动缩放器只知道给 HPA 的是什么。回到[第四部分](https://harness.io/2019/07/kubernetes-series-part-4-6-canary-operations-in-kubernetes/)，扩展您的 Kubernetes 集群非常重要。增加/减少工作节点应该被认为是一项常见的任务。在某些情况下，水平窗格自动缩放器将无法放置工作，您将最终无法满足或拒绝资源请求。因此，您需要扩展集群，这需要时间。这就是为什么[应用性能监控](https://www.appdynamics.com/cloud-monitoring/kubernetes-monitoring/)【APM】解决方案如此重要，以帮助在更具体的环境中预测何时触发扩展事件。非常大的 Kubernetes 集群有另一个集群管理器形式的额外系统帮助。在我所谓的“光明会星团”中，世界上一些最大的星团通常在 Kubernetes 后面还有一层管理星团的层。 [Google Omega](https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/41684.pdf) 、[微软 Apollo](https://www.usenix.org/system/files/conference/osdi14/osdi14-paper-boutin_0.pdf) 和 [Apache Mesos](https://people.eecs.berkeley.edu/~alig/papers/mesos.pdf) 都是“集群中的集群”管理者的例子。如果你点击其中的一个链接，你会看到一些关于分布式系统的冗长的学术论文。这些平台旨在协调其他平台；回到你的计算机科学大学时代，使用两级调度器。您的工作负载可能不值得为您的 Kubernetes 集群投资两级调度器或集群管理器，但是不要害怕，从某个地方开始吧。

## 增量是你的朋友

当你为未来设计应用和平台时，了解 Kubernetes 的挑战是很重要的。不要气馁，Kubernetes 是一个将分布式计算的力量带给大众的平台。任何一种技术变革的通用方法都是增量方法。从唾手可得的果实开始，逐步建立自信。从应用程序中最容易编排的无状态部分开始。可以了解什么是[dockering](https://docs.docker.com/compose/)以及如何扩大和缩小流量。随着旅程的继续，可以考虑引入具有不同层次的持久性需求的有状态应用程序。将一些工作负载部署到 Kubernetes，而将一些工作负载不部署到 Kubernetes 是意料之中的事情。

## 驾驭——跨越鸿沟

Harness 是帮助弥合异构基础设施中持续交付鸿沟的完美平台。无论您是第一次启动 Kubernetes，还是准备在 Kubernetes 上完成所有工作，Harness 都会支持您。有了 Harness，您可以很容易地将[管道分割到部署](https://harness.io/2019/08/migrating-clouds-with-harness-continuous-delivery/)的地方，这样您就可以部署到 Kubernetes 和非 Kubernetes 目的地。请继续关注最后一章，第六部分，Kubernetes 将走向何方。拉维