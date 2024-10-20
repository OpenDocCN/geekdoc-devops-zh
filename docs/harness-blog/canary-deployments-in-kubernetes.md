# Kubernetes | Harness 的金丝雀部署和操作

> 原文：<https://www.harness.io/blog/canary-deployments-in-kubernetes>

在库伯内特生态系统中，沙地上的一些线条变得模糊不清。

在我们讨论了第三部分 [Kubernetes 集群部署](https://harness.io/blog/kubernetes-cluster-deployment/)之后，我们现在在 Kubernetes 的世界中变得越来越危险。我们对在平台上做基本动作有很好的理解。如果你喜欢冒险，你可以拿到学习资料，甚至报名参加[认证的库伯内特考试【CKA】](https://www.cncf.io/certification/cka/)，继续你的冒险之旅。在 [Kubernetes](https://kubernetes.io/) 生态系统中，沙地上的一些线条变得模糊不清。某些功能是委托给 Kubernetes 平台，还是由正在部署的应用程序负责？搅浑水，更有甚者，有热门技术正在增强可插拔的 Kubernetes 功能，如[入口控制器](https://kubernetes.io/docs/concepts/services-networking/ingress/)选项，如 [Gloo](https://gloo.solo.io/) 和 [Kong](https://konghq.com/) 。自举问题又开始起作用了；我们可以使用入口控制器来增强我们的部署，但是将它们放在我们的平台上(例如 Kubernetes 基础设施的变化)需要适当注意将变化传播到 Kubernetes 平台本身。我以为 Kubernetes 应该很简单！

## Kubernetes 艰难的方式

就像在数学考试中展示你的工作或者在软件工程面试中展示白板一样，学习如何在没有任何帮助方法的情况下创建 Kubernetes 集群可以让你非常熟悉这个平台。《艰难之路》的典型例子是凯尔西·海塔尔的《艰难之路》通读这些步骤非常有帮助，尤其是在添加节点和查看通信是如何连接的时候，因为在 Kubernetes 的任何旅程中必然会发生的一个操作是添加和减去 [Kubernetes Worker 节点](https://github.com/kelseyhightower/kubernetes-the-hard-way/blob/master/docs/09-bootstrapping-kubernetes-workers.md)。理解了这些步骤，人们就可以想象出几种使用自动化工具扩展集群的自我或社区规定的方法。不过，我要说的是:“艰难之路”可能会因为这么多供应商/项目希望让人们轻松进入 Kubernetes 而逐渐消亡。

## 请给我添加一个 K8s 节点

当然，除了 Kelsey 的基本示例之外，还有更多添加工作节点的方法。来自供应商的早期增值是扩展和收缩您的 Kubernetes 集群的更加简化和灵活的方式。记住:Kubernetes 仍然是一个需要管理的基础设施。公共云和[平台即服务](https://harness.io/blog/what-is-paas/)供应商，根据您从他们那里获得的服务类型，使添加节点变得容易。下面是一些可以添加工作节点的主要供应商的列表。他们中的大多数都有一些警告，例如必须使用他们的 CLI/SDK 之一创建集群，或者集群的潜在最大大小。

用上面的列表添加一个 Kubernetes 节点是相当困难的。关于集群规模的现代复杂性，例如何时扩展 Kubernetes 集群，仍然有待解释。添加 K8 的节点和基础架构来支持这些节点可能需要几分钟时间，并且很好地掌握需要扩展集群的事件本身就是一门科学。仍然需要良好的容量规划规程。只需几个节点，我们就可以开始将实际的应用程序部署到 Kubernetes 中。

## Kubernetes -始终部署

在后台，Kubernetes 中的容器一直在没有管理员干预的情况下旋转。这是 [Kubernetes 调度器](https://harness.io/blog/kubernetes-container-sprawl/)(还记得第二部分中的内容)的核心优势，它在短暂的 [Kubernetes 容器](https://harness.io/blog/what-is-kubernetes-container/)(还记得第一部分中的内容)死亡的情况下，强制执行规定数量的[副本](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)。我的同事写了一篇关于不同类型的[部署策略](https://harness.io/blog/blue-green-canary-deployment-strategies/)的优秀博客文章。两个突出的发布策略是金丝雀发布和蓝色/绿色(或者选择你喜欢的颜色，例如红色/黑色)。虽然蓝/绿的概念是拥有完全相同的生产环境，也就是实际运行在生产环境中的 100%以上的基础架构，以最大限度地降低风险并实现众所周知的转变。对于您的 Kubernetes 集群来说，扩展或拥有额外的蓝/绿容量成本高昂。从纯粹的部署策略来看，金丝雀部署可以被视为更复杂，因为你要分开一个百分比，例如翻转开关两次(或者更多，如果是增量金丝雀)，但你可以用比蓝/绿更少的火力完成。

## 让我看看 K8s 中的一些应用程序——不会变得更容易

我见过的关于在 Kubernetes 部署金丝雀的最全面的指南之一是 dock bit2017 年的这个[帖子。当你阅读这篇文章时，你可能会开始注意到文章持续了一段时间，你的手指厌倦了滚动。这不是偶然的；有几个移动的部分需要协调，并围绕每个部分制定策略。随着 Kubernetes 生态系统开始扩展并变得更加精细，决策变得更加复杂。提醒一下，canary 的目的是为需要访问应用程序的客户提供无缝服务；一个 URL/URI，集群中可能有两个最终版本。*负载平衡器- >【金丝雀(标签)+稳定(标签)】*的策略是通过协调](https://medium.com/google-cloud/kubernetes-canary-deployments-for-mere-mortals-13728ce032fe)[部署](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)和[删除](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete)来实现的，这将有助于实现金丝雀发布。

### 标签和选择器

我们需要在 Kubernetes 中引入一些变化/新的部署。我们如何区分不同的部署？在我们之前的[功夫金丝雀的例子中。YAML](https://github.com/ravilach/harness/blob/master/kung-fu-canary.yaml) 部署，比方说我们准备引进一些变化。我们需要向 Kubernetes 指明这是一种不同的方式；如果我们只是重新部署相同的配置，只是增加 Docker 映像，Kubernetes 将尽快替换每个已知的 [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod/) ，我们只是追求金牌，而不是分阶段的方法。幸运的是，我们有[标签和](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)选择器。当创建我们的对象时，我们可以添加标签元素来帮助区分彼此。标签可以包含您想要添加的任何种类的细节/元数据；对于金丝雀来说，这可能是指定某个环境，甚至将某个特定的部署标记为金丝雀。我们可以创建部署的另一个版本，这一次带有标签。由于我们部署的是金丝雀，我们需要的副本数量会更少；我们从 3 升到了 1。

现在，将被用作金丝雀的部署有了正确的标签，我们需要实际引入金丝雀并最终翻转开关的操作，如[服务](https://kubernetes.io/docs/concepts/services-networking/)或[入口](https://kubernetes.io/docs/concepts/services-networking/ingress/)负载平衡器可以使用选择器对该标签进行操作。要将您的新部署引入到 Kubernetes 负载平衡器循环中，您可以展开 Kubernetes 负载平衡器的选择器，以包含新标签，或者确保标签是通用的，并且跨版本排列，如“应用程序名称”

### 我们在哪里翻跟头？

自从 Dockit 博客以来，Kubernetes 中的入口选项有所增加。[入口](https://kubernetes.io/docs/concepts/services-networking/ingress/#what-is-ingress)是从 Kubernetes 集群外部到 Kubernetes 集群内部的通信。负载平衡可以采取许多形状和形式；当用户请求在互联网上竞争时，[第四级](https://www.nginx.com/resources/glossary/layer-4-load-balancing/) - > [第七级](https://www.nginx.com/resources/glossary/layer-7-load-balancing/)负载平衡很容易发生在 Kubernetes 集群之外。然而，在您的集群内部，可能有其他项目正在运行，比如像 [Istio](https://istio.io/) 这样的服务网格。我们是将金丝雀负载平衡逻辑委托给[服务网格路由规则](https://istio.io/blog/2017/0.1-canary/)还是其他什么地方？在入口层用 Gloo 这样的[项目代替](https://scott.cranton.com/canary-deployments-with-solo.html)[临时负载平衡器](https://kubernetes.io/docs/concepts/services-networking/#loadbalancer)怎么样？如您所见，我们有很多选择来决定在哪里匹配开关。随着将我们的集群通信连接在一起的新方法变得流行，我们肯定会有更多的选择。有了所有的技术和准备，我们可以有一只成功的金丝雀；尽管就金丝雀而言，成功与否仍然证明了价值。

### 当“船”撞上风扇

哦，不，我们的小金丝雀没有在煤矿中存活下来——我是说 K8s 星团。一个常用的命令是 [Rollout](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#rollout) 命令，这是 K8 用来部署资源的计划。比如，在《T2》原著中的功夫-金丝雀。YAML 从第三部分，我们设置了三个副本集。要快速浏览一次展示，您所要做的就是再次部署功夫金丝雀部署。如果当前有此运行，可以用“*kubectl delete deploy/kung-fu-canary*删除”。我们可以使用"*ku bectl apply-f kung-fu-canary . YAML*进行重新部署，并使用"*ku bectl Rollout status deployment/kung-fu-canary*查看部署状态

假设我们想做一些改变。不使用 [Nginx](https://www.nginx.com/) ，我们想使用 [Apache HTTPD](https://httpd.apache.org/docs/2.4/programs/httpd.html) 并从五个副本开始。

我们将其保存为[功夫-金丝雀 _ 更新。YAML](https://github.com/ravilach/harness/blob/master/kung-fu-canary_update.yaml) 。现在，我们可以使用"*ku bectl apply-f kung-fu-canary _ update . YAML*来部署更新，并再次使用"*ku bectl Rollout status-w deployment/kung-fu-canary*来观看首次展示

我们可以通过“ *kubectl 首次展示历史部署/功夫金丝雀*”来查看首次展示历史，并看到我们添加了另一个版本。

哦，不，我们决定更喜欢 nginx，所以让我们用"*kubectl rollout undo deployment/kung-fu-canary*"来触发回滚

恭喜，您刚刚经历了一个基本的回滚示例。我们只有一个简单的部署，不需要担心多个临时的部分。幸运的是，Harness 能帮上忙。

## 带着挽具，挥动魔棒

有了所有这些选项，而且选项的数量似乎还在增加，我们怎么能只关注金丝雀功能的工作呢？别担心，驾驭[软件交付平台](https://harness.io/platform/)就在这里！线束可以很容易地使 [Kubernetes 金丝雀](https://docs.harness.io/article/wkvsglxmzy-kubernetes-canary-workflows)成为你的整体[连续交付管道](https://harness.io/blog/ci-cd-pipeline//)的一部分。即使你的观点倾向于 Istio 和流量分配，Harness 也会支持你。请继续关注第五部分，我们将了解为什么今天我们所有的工作负载都不在 Kubernetes 中。拉维