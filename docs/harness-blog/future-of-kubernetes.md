# Kubernetes |线束的未来

> 原文：<https://www.harness.io/blog/future-of-kubernetes>

有了这么多的运输选择，Kubernetes 生态系统正在扩大。这不是没有竞争。

最后，博客系列的高潮！在第五部分之后，我们了解了将工作负载放入 Kubernetes 的[挑战。既然 Kubernetes 是一个快速发展的平台，那么 K8s 的未来会是怎样的呢？生态系统中的一大推动力是捕获传统上很难放在容器中和/或由 Kubernetes 编排的工作负载。由于应用程序和基础设施提供商一直在对其堆栈进行现代化改造，以便更好地应对容器的激增，我们已经达到了临界质量。Kubernetes 已经经历了从一个组织只是部署的平台到一个组织构建的平台的演变。随着越来越多的项目在 Kubernetes 上建立，甚至项目的节奏也在改变。](https://harness.io/blog/not-everything-fits-in-kubernetes/)

## 减慢滚动速度

截至 Kubernetes 1.15，这是我写这篇博客时的最新版本， [1.15 版本的口号是在平台和发布节奏方面的稳定性](https://devclass.com/2019/06/20/its-all-stability-and-extensibility-for-kubernetes-1-15/)。在 Kubernetes 的早期，似乎每隔几个月就会[发布一个 sub 或 minor 版本。这在快速发展的开源项目中是可以预料的。在子版本和次要版本之间，几乎是 API 不断变化，API 被弃用甚至被提升。作为个人解药，我参加了 1.9 [1.09]和 1.10 版本之间的](https://gravitational.com/blog/kubernetes-release-cycle/)[认证 Kubernetes 管理员](https://www.cncf.io/certification/cka/)考试，培训材料和考试之间大约有 5%的 API 不同。这意味着当我试图将 [KubeCTL](https://kubernetes.io/docs/reference/kubectl/overview/) 命令应用到我信任的[YAML](https://en.wikipedia.org/wiki/YAML)上时，我会得到验证错误，并且必须对 YAML 进行结构性修改。随着平台进入成熟的第一阶段，工作负载增加，随着平台暴露得越来越多，漏洞肯定会越来越多。最严重的漏洞之一发生在 2018 年，这是一次[权限升级](https://thenewstack.io/critical-vulnerability-allows-kubernetes-node-hacking/)，完成起来并不十分困难。社区和供应商迅速采取行动进行修补。Kubernetes 生态系统中最大的变化之一是使 Kubernetes 变得不那么普通。具有讽刺意味的是，2019 年 6 月，在允许这一点的[主要机制](https://kubernetes.io/blog/2019/06/20/crd-structural-schema/)之一中发现了一个漏洞。

## 您修改过的集群不再是普通的了

回到第五部分，对 Kubernetes 平台的批评是 Kubernetes 是通用的。Kubernetes 平台最大的变化之一是为您的应用程序定制平台。container [orchestrator 软件开发工具包(SDK)](https://thenewstack.io/the-rise-of-the-container-orchestrator-sdks/)已经有所增长。Kubernetes 和其他 orchestrators 如 [Apache Mesos](https://mesosphere.github.io/dcos-commons/) 提供软件开发工具包来构建你的应用程序。在这种转变之前，大多数人(可能仍然是大多数人)在 Kubernetes 上构建他们的应用程序，这意味着按原样使用 Kubernetes。随着沙子开始转移，人们开始在 Kubernetes 上构建他们的应用程序，这意味着修改 Kubernetes 以适应你的应用程序正在加速。最好的例子之一就是所有的发展成[的运营商](https://coreos.com/operators/)。几年前由 CoreOS 推出，运营商真的获得了很多动力。Kubernetes 运营商使 Kubernetes 非常注重应用/以应用为中心。操作员扩展并利用[控制器](https://kubernetes.io/docs/concepts/workloads/controllers/)和[自定义资源定义](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) (CRDs)。有了这种组合，Kubernetes 就有可能以非常特定的方式为您的应用和应用基础设施做出反应。为什么使用运营商的一个重要原因来自 Confluent (Apache Kafka)首席执行官的博客文章。她指出了几个要点，多亏了运营商，在 Kubernetes 上操作卡夫卡现在是精确的和规定的。虽然通过安装另一个控制器或 CRD，您的香草 Kubernetes 集群不再是香草。有一些安装步骤可以让您的新 CRDs 和控制器在集群中运行。让你的记忆回到第二部分的，包/配置经理如[掌舵](https://helm.sh/)、 [Kustomize](https://kustomize.io/) 和[卡皮坦](https://github.com/deepmind/kapitan)变得更加重要。随着我们的集群规模开始扩大，甚至可能跨越不同的基础设施提供商，即“混合云”，在不同的基础设施上利用 Kubernetes 仍然会受到分布式计算的[谬误](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing)的影响。

## 混合云

Kubernetes 仍然可以被视为分布式计算中最伟大的均衡器之一。理论上，如果您有一个 Kubernetes 集群在运行，那么您应该能够在另一个集群中部署您的应用程序，而不会有太多的停顿。Kubernetes 和 Kubernetes 领域的供应商正在努力实现更多的“集群的集群”方法。Kubernetes Federation 是 Kubernetes 项目的一个解决方案，旨在对多个集群进行更集中的管理。尽管有不同的/远程集群，特别是跨不同的基础设施提供商，Kubernetes 不会自动神奇地解决您的问题。良好的分布式系统原则适用。延迟不会因为你使用 Kubernetes 而减少。如果跨集群通信不是很强，Kubernetes 可以开始将节点标记为不健康，而实际上这些节点是健康的。随着工业赶上 Kubernetes 的力量，大众越来越容易获得分布式和健壮计算的力量。实际上，我们的应用程序的潮流正在上升。

## 水涨船高

即使是在 Kubernetes 早期传统上难以运行的项目，比如数据库，也变得越来越容易。供应商正在用这种新的设计范式来构建产品和平台。 [CockroachDB](https://www.cockroachlabs.com/campaigns/kubernetes/) 有一个很好的故事，讲述了他们在过去几年中为使 SQL 数据库在 Kubernetes 上可消费所做的工作。在浏览完博客系列后，再来看看 [CNCF 的风景](https://landscape.cncf.io/)，可以开始看到组织机构为拥抱新的设计范式所做的投资金额。云原生项目的数量被用作现代化/重新创建在 container orchestrator 世界中运行良好的平台的构建块。[集装箱存储接口(CSI)](https://github.com/container-storage-interface) 和[集装箱网络接口(CNI)](https://github.com/containernetworking/cni) 也有助于推动集装箱市场向前发展。在容器编排上运行工作负载的期望正在成为主流。当我们获得容器的好处时，技术机器总是向前移动。集装箱现在甚至开始面临竞争。

## 集装箱太重了吗？

Docker 从 dotCloud 引入荒野一年后([还记得第一部](https://harness.io/blog/what-is-kubernetes-container/)吗？)，亚马逊网络服务通过[AWSλ](https://aws.amazon.com/lambda/)向大众推出无服务器计算。如果一个容器需要几秒钟的时间来生成以允许应用程序开始运行，那么 Lambdas 在执行开始之前需要几秒钟。容器是短命的，也就是短暂的，但是无服务器的功能会随着每个请求而加速或减速；在您需要功能的时候，几乎完全使用您需要的功能。不要担心，如果 Kubernetes 是您的应用程序基础设施的一个组成部分，那么 [KNative 项目](https://knative.dev/)公开了无服务器实现的片段，因此您可以拥有由 Kubernetes 编排的无服务器基础设施。Kubernetes 不断成熟，生态系统不断扩大。工作负载肯定需要比无服务器调用更长的寿命，因此 Kubernetes 生态系统将存在一段时间。

## 展望未来

Kubernetes 生态系统继续扩展，供应商和组织仍在经历拥抱这项五年前的技术的旅程。如果你正在寻找这个博客系列的结尾，不幸的是，这本书并没有结束。幸运的是，马具会帮你度过难关。随着新的设计模式上线，可以确保您的管道在那里支持它们。作为额外的奖励，Harness 将举办一场[网络研讨会，讨论博客系列的所有六个部分](https://university.harness.io/introduction-to-kubernetes)。我们希望你能参加！干杯-拉维