# 您的第一个 Kubernetes 集群部署|工具

> 原文：<https://www.harness.io/blog/kubernetes-cluster-deployment>

有了这么多的 Kubernetes 产品，让我们走一条阻力最小的路，让您的第一个集群嗡嗡作响。

既然我们已经了解了容器的用途，以及构成广受欢迎的容器编排器 Kubernetes 的重要部分，那么是时候构建一个小型集群了。Kubernetes 作为一个项目进展非常快，只有最近[发布的节奏](https://gravitational.com/blog/kubernetes-release-cycle/)慢了下来。

由于项目的流行和速度，在考虑 Kubernetes 供应商时，肯定有很多选择。供应商们试图用一系列关于运营和附加价值的处方来区分他们自己与“仅仅是另一个 Kubernetes 供应商/平台”的污名。

来自提供以下服务的公共云供应商的竞争非常激烈

[谷歌的 Kubernetes 引擎](https://cloud.google.com/kubernetes-engine/)和[亚马逊的弹性 Kubernetes 服务](https://aws.amazon.com/eks/)到[的平台即服务](https://harness.io/blog/what-is-paas/)供应商，如 Red Hat 的 OpenShift 和 Pivotal 的 PKS，争夺 mindshare 和你辛苦赚来的美元。

请随意浏览博客文章或视频，了解创建自己的 Kubernetes 集群的策略！我们现在将跳过一些操作项目，将重点放在您的第一个 K8s 部署上，不一定是管理平台。

## 观察和学习

一个关于在你的 Mac 上使用自制程序运行 Minikube 并运行本教程中的[命令的简短视频。](https://github.com/ravilach/harness/blob/master/minikubecommands) 

## Minikube -当地 Kubernetes

在我看来，在你的笔记本电脑上与 Kubernetes 互动的最快方式是 [Minikube](https://github.com/kubernetes/minikube) 。Minikube 自 2016 年夏天以来一直存在，并试图匹配 Kubernetes 的次要发布版本。
使用像[家酿](https://brew.sh/)这样的软件包管理器，在你的 Mac 上安装 Minikube 非常简单。你还需要虚拟化部分，即 [VirtualBox](https://www.virtualbox.org/) 。如果你的安装方法没有附带 VirtualBox，VB 是一个[快速安装](https://www.virtualbox.org/wiki/Downloads)。

您可以按照以下步骤从 Homebrew 安装 Minikube 和 VirtualBox。

*   从终端安装自制软件。
*   使用终端命令“*brew tap caskroom/木桶*”加入更多软件的木桶通道。
*   用*安装 VirtualBox“brew 木桶安装 virtualbox* ”。
*   用“*brew buck 安装 minikub* e”安装 Minikube/KubeCTL。
*   默认情况下，分配 4GB 内存。安装 minikube 后，您可以使用“ *minikube 配置设置内存 8128* ”提供更多资源，例如 8GB。
*   用“ *minikube start* ”启动 Minikube。
*   使用“ *minikube dashboard* ”启动您的 K8s 仪表盘。

有了这些步骤，您就可以开始使用 [KubeCTL](https://kubernetes.io/docs/reference/kubectl/overview/) 命令行界面了！

## 先有鸡还是先有蛋？

在第二部分中，我们讨论了与 Kubernetes 集群交互的机制。KubeCTL，Kubernetes 的命令行接口，是主要的机制之一。

如果这是您第一次安装 Minikube，那么最新版本的 KubeCTL 将与 Minikube 内部最新版本的 Kubernetes 相匹配，因为 KubeCTL 将与 Minikube Brew 安装程序一起安装。KubeCTL 和 Kubernetes 的版本需要匹配，否则将来可能会出现一些奇怪的 API 警告。

我倾向于先安装 KubeCTL，然后将我的 Kubernetes 集群连接到 KubeCTL。好消息是您可以有不止一个 [KubeCTL Context](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/) 这意味着您可以有不止一个集群。虽然对于这个例子来说，让 Minikube 安装程序来处理是非常好的，你可以专注于更难的东西，如 YAML。

## 一点 YAML 不会伤害任何人

[YAML](https://yaml.org/) 又名“另一种标记语言”是 Kubernetes 中使用的主要描述符语言。Kubernetes 最大的一个好处就是 Kubernetes 是声明性的；你只要说出你想要什么，其余的就由 K8 公司解决了。我们可以描述一个应用程序，比如说一个功夫金丝雀应用程序的端点。

如果您以前没有和 YAML 打过交道，请注意 YAML 是空间分离的，不一定依赖于数据结构。喘息标签 vs 太空战肆虐。对于命令行人员来说，我鼓励使用某种类型的 [linter](https://github.com/adrienverge/yamllint) (代码验证器)。

掌握 YAML 语法非常简单。Kubernetes 使用 YAML 地图和 YAML 列表来描述州/ a 部署。维基百科对 YAML 语法有很好的解释。在 Harness，我们将为您提供一只功夫金丝雀，让您继续学习。

## 功夫金丝雀，你的第一次部署！

我们可以用这个简单的[部署](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)作为例子。在 Kubernetes 中，部署是一个 [Pod](https://kubernetes.io/docs/concepts/workloads/pods/pod/) 和[副本集](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)的声明性/期望状态。部署的好处是能够整体管理部署；它可以删除、恢复等。

我把[功夫金丝雀](https://github.com/ravilach/harness/blob/master/kung-fu-canary.yaml)的部署贴在了 [GitHub](https://github.com/ravilach/harness/blob/master/kung-fu-canary.yaml) 上，供你复制粘贴。请随意下载用于练习。

让我们来分解一下这个辉煌的功夫-canary.yaml 的几个片段。我们定义这个特殊的资源/YAML 是一个部署。我们定义了我们的部署规范，即我们需要最小数量的副本，以及在有更新/升级的情况下应该做什么，并定义了关于如何执行[滚动更新](https://kubernetes.io/docs/tasks/run-application/rolling-update-replication-controller/)的策略。

Kubernetes 只对其声明的内容和解释的内容采取行动。[准备就绪和活性检查](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/)很重要。如果没有它们，Kubernetes 只会在发生诸如 [Sig-Term](https://www.gnu.org/software/libc/manual/html_node/Termination-Signals.html) 完全杀死一个容器的事件时重新启动/补充你的容器。准备就绪

活动检查可以提供更多关于运行中的应用程序的信息，以及在应用程序不健康时应该做什么。带着那只功夫金丝雀。YAML，开始部署！

## 一些 KubeCTL 命令

记住少数命令会让你在 K8s 世界里变得危险。让我们来看看 KubeCTL 应用、获取、描述和扩展。Kubernetes 文档确实有一个冗长的备忘单，但是我们可以在这里把重点放在基础上。

首先要使用的命令之一是“kubectl apply”。随着[应用](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#apply)命令，您正在执行您的可部署 YAML。我比我需要在 Kubernetes 中执行的大多数操作都更坚持，我把版本控制留在 YAML，等等。可以用"*kube CTL apply-f kung-fu-canary . YAML*"部署我们的功夫金丝雀[部署](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)。现在正在部署您的第一个部署！

您可以在终端中导航到 WebUI["*minikube dashboard*]以查看您的部署！

现在您已经完成了第一次部署，让我们来看看幕后发生了什么。使用 [Get](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#get) 命令，我们可以获得一个运行 Pod 的列表(还记得来自[第二部分](https://harness.io/blog/kubernetes-container-sprawl/)强大的 Pod)或者其他指定的资源

现在我们知道了我们的豆荚，我们可以更深入地看看它们。使用 [Describe](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe) 命令，您可以查看详细信息或资源，在本例中是我们的功夫金丝雀 Pod。从" *kubectl get pods* "列出的 pod 中，让"*ku bectl describe Pod _ id*"描述 Pod。看那可爱的州信息。

如果我们需要副本集或多或少的火力呢？我们的功夫金丝雀部署示例利用了三个副本。如果我们想把数量增加到四个呢？ [Scale](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#scale) 命令可以设置多个资源的新大小。只需运行"*ku bectl scale-current-Replicas = 3-Replicas = 4 deployment/kung-fu-canary*"即可从 3 个副本扩展到 4 个副本。

## 奖励命令:暴露

如果不能访问我们的应用程序，我们的应用程序将会非常孤独。暴露 T21 命令将创建一个服务来暴露你的应用程序。为了以最简单的方式公开我们的功夫-金丝雀部署，可以运行"*kubectl expose deployment kung-fu-canary-port = 80-type = load balancer*"。可以通过运行" *kubectl cluster-info* "找到您的集群的公共 IP 地址，这是主服务器在 Minikube 中运行的地方

您可以使用“*ku bectl describe service kung-fu-canary*”来检查您的新服务。记下节点端口。要访问您的 nginx 实例，可以欺骗并使用 Minikube 命令"*Minikube service kung-fu-canary*"，这将自动为您显示 public_ip:NodePort。

## 清除

一旦我们都完成了这个示例部署，就像一个好公民一样，我们可以清理我们所做的事情。

使用 [Delete](https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#delete) 命令，我们可以删除已经完成的部署。我们将我们的可部署包到一个部署中，这样就可以简单地运行"*kubectl delete deploy/kung-fu-canary*"可以验证您的 pods 已经消失，并且没有功夫 Pods 出现，或者我们信任的朋友没有返回任何资源。

如果您选择运行奖励命令，可以删除暴露功夫金丝雀的服务，该服务被方便地命名为功夫金丝雀，并带有"*kubectl delete Service kung-fu-canary*"可以验证服务是与“ *kubectl get services* ”和功夫-金丝雀不应该在那里。

最后，我们可以通过运行“ *minikube stop* ”来关闭我们忠实的老朋友 Minikube 如果你想从头开始或做更多的修改，可以运行 *minikube delete*

## 实际的 Kubernetes 星团呢？

在我们的下一部分，第四部分，我们将开始谈论 Kubernetes 集群本身。重要的是，把你的应用程序和 Kubernetes 平台分开。您的应用程序和 Kubernetes 集群都有不同的伸缩机制。

一个 TL；灾难恢复方面，大多数 Kubernetes 操作都是添加/减少节点、更换可插拔部件以及升级平台。和其他软件一样，这里没有什么神秘之处。

## 驾驭，增强您的 Kubernetes 部署

当我们剥开 Kubernetes 的洋葱皮时，Kubernetes 预成型的“魔术”归结为一个声明式驱动的系统。我们描述最终状态，Kubernetes 通过调度器和资源管理器功能努力提供我们所描述的内容。尽管完成更复杂的任务，比如 canary 发布或发布验证，需要几个精心编排的步骤和这些步骤的底层描述。

Harness 平台让这些项目变得异常简单。现在，凭借您的 Kubernetes 知识，您可以将[控制代理](https://ngdocs.harness.io/article/2k7lnc7lvl-delegates-overview)安装到 Kubernetes 环境中，[甚至是 Minikube](https://community.harness.io/t/minikube-setup-with-harness/22) ！请继续关注博客系列的第四部分，操作化。
-拉维