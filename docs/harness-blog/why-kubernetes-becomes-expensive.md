# 为什么 Kubernetes 变得昂贵-允许消费飙升|线束

> 原文：<https://www.harness.io/blog/why-kubernetes-becomes-expensive>

Kubernetes 允许通用工作负载放置，因此 Kubernetes 的成本和消耗可能会飙升。从 Harness 中了解这种情况是如何发生的，以及如何应对。

阿波罗 11 号导航计算机只有 32KB 的内存来运行所有这些源代码，那是在 20 世纪 60 年代。快进到 21 世纪 20 年代，你的智能手表的电量会成倍增加，所以我们应该一直用手表发射火箭。随着计算机变得越来越强大，我们对资源的消耗也在增加，例如[Writh/Page 的](https://en.wikipedia.org/wiki/Wirth%27s_law)定律。

对于每一层抽象和泛型，我们都以敏捷的名义增加了开销。进入 Kubernetes，它是现代软件生态系统中最伟大的均衡器之一；应用程序和基础设施团队可以对应用程序需要部署和运行的内容有一个共同的描述。

大部分(如果不是全部的话)与运营和基础设施相关的需求可以在一个或几个 [YAML](https://en.wikipedia.org/wiki/YAML) 文件中描述，一旦你有了一个等待 Kubernetes 集群，你就可以开始比赛了。纵观今天更大的 [Kubernetes](https://harness.io/2020/02/a-developers-guide-to-deploying-to-kubernetes/) 生态系统，你有很多方法可以获得集群。

从使用 [Minikube](https://community.harness.io/t/minikube-setup-with-harness/22) 进行本地开发，到利用公共云供应商之一，如 [EKS](https://aws.amazon.com/eks/) 和 [GKE](https://cloud.google.com/kubernetes-engine) ，到利用[平台即服务](https://harness.io/blog/what-is-paas/)，如[open shift](https://www.openshift.com/)；库伯内特星团就在眼前。虽然 Kubernetes 是一项相对较新的技术，并且在企业中仍在不断发展，但 Kubernetes 中的低工作负载规模壁垒已经在过去几年中消除了一些组织保护措施。

### 库伯内特之前的时代

显然，自阿波罗计划以来，计算机领域发生了巨大的变化，所以我们不会回到那么久以前。回顾 2014 年以前的科技时代，以目前的创新速度来看，Kubernetes 似乎是一个永恒的时代(祝[6 岁生日快乐](https://enterprisersproject.com/article/2020/6/kubernetes-birthday-6-facts) K8s！).再过几年，Kubernetes 工作负载的技术采用曲线将成为主流，让我们考虑一下 Kubernetes 之前的范式是什么样的。

拥有一个分布式应用程序当然不是什么新发明；在 JAVA 生态系统中，自 21 世纪初以来，集群一直围绕着 T1 发展。在本地测试群集功能或多个节点是一项挑战。最有可能的情况是，根据技术堆栈，必须降低节点大小，以便在您的计算机上安装两个节点，并且对于网络堆栈，增加/偏移所需的端口，以便不会发生冲突。

在本地测试之后，走向上层环境/生产，如果你作为一个开发人员必须添加一个应用服务器的另一个节点，比如说 [JBoss Wildfly](https://wildfly.org/) ，会怎么样？首先，您不会添加额外的节点，这是组织安全措施的一部分。最有可能的情况是，您的另一个团队中有一名中间件/平台工程师负责管理应用服务器集群。这是因为添加节点的步骤[并不简单](https://docs.wildfly.org/20/Admin_Guide.html#Domain_Setup)，很可能会产生应用服务器供应商的许可成本，并需要额外的虚拟或物理基础设施。

从应用服务器到消息代理，这些神话般的 JEE 集群管理员会像系统工程师监控基础设施虚拟机一样密切关注 JAVA 虚拟机。中间件工程师的工作中有一部分是性能调优和容量规划/优化，以定期修剪或扩展集群。

从历史上看，由于 JAVA/JEE 基础设施和工作负载受到领域专家的密切关注，因此重构基础设施不成问题。不过，如果你是我，不希望你宝贵的基础设施被拿走，我在我曾经工作过的一家银行写了下面的脚本，以确保我的 WebSphere 的特定[单元](https://www.ibm.com/support/knowledgecenter/SS7K4U_8.5.5/com.ibm.websphere.zseries.doc/ae/tagt_watch_cell.html)不会被重组。

这足以让中间件工程团队看到我的单元是活动的，因为存在与 WebSphere 相关的许可成本。今天的情况如何，你可以在我关于云成本基础知识的[网络研讨会](https://harness.io/webinar-cloud-cost-101/)中了解云成本的基础知识。随着范式向 Kubernetes 转变，拥有另一个应用程序副本就像 YAML 的一行代码一样简单。

### 进入 Kubernetes -越多越好

需要立即扩展您的应用吗？使用 Kubernetes 不用担心，你可以在一行 YAML 中增加或减少[副本数量](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)或者一个简单的“*kubectl scale-replicas = 3rs/foo*命令。像魔术一样，一旦集群上有可用空间，Kubernetes 作为资源管理器和调度器将尽快满足您的请求。

与没有 Kubernetes 时扩展工作负载的感觉相比，增加副本的数量就像在公园散步一样容易。没有讨厌的中间件工程师来审问你，并提交几张票供审查和进一步讨论；只要点燃 [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) 然后嘭。有了这种级别的敏捷性，应用程序团队可以更快地扩展其工作负载。过去会发生的调整因为扩展仍然很耗时，一旦能够依赖 Kubernetes，调整就会退居二线。

如果利用 Kubernetes 作为基础设施，您仍然需要基础设施来运行集群。Kubernetes 本身将耗尽可用资源。随着工作负载需求的增加，一种常见的方法是扩展 Kubernetes 集群。资源越多，成本越高。

随着越来越多的工作负载转向 Kubernetes，控制已经从拥有 Kubernetes 的开发团队转移到确保集群正常运行的平台工程团队。可能会有适用于团队的配额和退款，但它们要宽松得多，以允许扩展。通常，工作负载调优将由应用团队负责，集群调优将由平台工程团队负责。

### 优化工作负载还是优化集群，或者两者都是？

Kubernetes 与其他需要维护和管理的软件没有什么不同；您不会让您的操作系统运行而不更新和调优，直到时间结束。

从平台工程的角度来看，构建 Kubernetes 集群来支持您组织的工作负载是一个迭代过程。理解 Kubernetes 相对于物理或虚拟硬件的容量需求变得更加困难，因为将工作负载放在您所珍视的 Kubernetes 集群上的团队有合理的弹性预期。作为平台的提供者，寻找工作负载的团队将是难题的一部分。在优化集群时，优化工作负载可能比集群范围内的更改更容易。

应用团队拥有支持他们想法的应用足迹。降低开销和提高性能是软件工程师构建下一组特性的内在要求。应用程序团队通常会创建 Docker 映像和 Kubernetes 部署 YAMLs，其中包含资源规范。作为一名构建容器化工作负载的软件工程师，我接受的训练是假设容器中 100%的资源总是用于设计安全。

在运行我们的虚拟线束大学时，假设我们有几十个并发用户，并且知道[容器资源限制](https://www.deliver-better.com/post/resource-management-in-kubernetes)，我构建了一个大型集群来处理潜在的最大值。我觉得我有点低，担心达到容量，所以我准备了一个自动缩放器来添加额外的 Kubernetes 工作节点。

看一下上面的数字，即使在峰值使用时，我们甚至没有达到集群容量的 10%;说到过度杀戮。为了提高效率，可以选择调整工作负载和/或集群。

### 调整工作负载+位置

一个应用程序的[四个体液](https://www.nlm.nih.gov/exhibition/shakespeare/fourhumors.html)是存储、计算、网络和内存。通过减少一个或多个体液，您正在减少集群的负担，从而可以减少集群的大小或有能力在当前集群中放置更多的工作。

减少一个资源请求/限制是减少工作量的主要方法。说起来容易做起来难，可能会有很多因素，例如技能组合的可用性。如果应用程序/服务没有处于积极的开发中，如果没有开发支持或适当的指标来支持调整，这种类型的调整是有风险的。通常，资源请求/限制的大小是根据一些基准或基线来确定的。

[云成本管理显示实际资源、请求资源和资源限制]

年龄也是一个因素，针对 Kubernetes 的较新的专门构建的应用程序倾向于利用较新的架构和平台，而不是提升并转移到容器中，在容器中的限制曾经是在传统的应用服务器中。

更短暂和/或更有弹性的工作负载可以从调整放置策略中受益。放置策略就是 Kubernetes 如何将您的工作负载放置在集群中。蒂芙尼有一篇关于 d [不同自动缩放和放置策略](https://harness.io/blog/kubernetes-containers-cost/)的精彩博文，你可以加以利用。

另一方面，如果您的工作负载继续对集群造成负担，例如，在一个集群级自动缩放器上执行，如[Horizontal Pod auto caller](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/)(HPA)，您将耗尽资源。在这种情况下，您需要在集群级别采取措施，例如添加额外的节点/调整工作节点，以减少触发 HPA 的压力之一。

### 优化集群

强大的 Kubernetes 是一个工人节点模型，其中主节点进行编排，工人节点位于 [kubelets](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/) 居住的地方，例如工作发生的地方。在集群级别，可以进行从策略到 kubelet 的各种级别的调优，一直到机器级别。

您的 Kubernetes 节点必须在某种操作系统上运行，无论是物理的还是虚拟的，而且很可能该操作系统是一种 Linux。在 Linux 世界中，Kubernetes 的一个常见做法是禁用将内存从 RAM 交换到磁盘的[交换](https://opensource.com/article/18/9/swap-space-linux-systems)。在更现代的 Kubernetes 发行版中，如果 kubelet 确定 swap 仍然启用，那么 kubelet 服务实际上不会启动。系统工程师在 Linux 机器上进行的性能调优仍然会让您的工作负载更高效地运行。

通常为较大的集群保留，您可以超越使用[污点和容忍度](https://kubernetes.io/docs/concepts/scheduling-eviction/taint-and-toleration/)给出节点亲缘关系，您实际上可以通过使用 [kube-scheduler 阈值](https://kubernetes.io/docs/concepts/scheduling-eviction/scheduler-perf-tuning/)影响放置分数的计算方式来修改调度行为。对于一些组织来说，深入了解 Kubernetes 如何安排工作负载可能超出了范围，

对于实际的集群大小，在[之前的博客文章](https://harness.io/2020/06/optimizing-kubernetes-cluster-costs/)中，我介绍了从 Kubernetes 集群中删除和清空节点的命令。即使是小型集群，您的成本也可能会快速增加。

### 成本会如何增加

还记得在 Kubernetes 出现之前的一节中，在本地机器上测试分布式应用程序吗？非生产规模和测试无疑是 Kubernetes 之前的一个痛点。很有可能在今天利用 Kubernetes 时，在 Kubernetes 中测试大规模分布式架构的能力很简单。

具有讽刺意味的是，非生产工作负载可能会占用与实际生产应用程序一样多甚至更多的资源。让多个环境或名称空间支持多个较低的环境可能是一项成本高昂的工作。提供一个通用平台来快速、一致地扩展工作负载的本质意味着更高的成本。

因为平台是通用的，所以“中间件工程师之死”和对工作负载的深入了解已经随着 Kubernetes 而改变。如今，中间件/平台工程师的职责是确保平台正常运行，例如 Kubernetes，但平台内部的工作负载本身不是他们的责任。如果集群负担过重，平台工程师就有工作要做，他们需要确定成本和证明工作负载合理性的最佳方式。云成本管理消除了这一挑战。

### 进入云成本管理-与线束合作

[云成本管理](https://harness.io/platform/cloud-cost-management/)允许您跟踪应用程序和集群级别的使用情况，了解集群支出的金额。由于 Harness 是一个部署记录系统，因此无需在 Kubernetes 中添加标签即可完成。

云成本管理还能够将变更/部署等事件与其成本关联起来。借助云成本管理，可以在一个简洁的位置查看历史趋势或影响事件。

具有云成本管理功能的 Harness 平台是您关注和解决优化 Kubernetes 工作负载问题的一站式平台。Harness 将帮助您与合作伙伴一起降低云成本。如果你还没有看过[云成本管理](https://harness.io/platform/cloud-cost-management/)的运行，请随意[要求演示](https://harness.io/platform/cloud-cost-management/)。

干杯，

拉维