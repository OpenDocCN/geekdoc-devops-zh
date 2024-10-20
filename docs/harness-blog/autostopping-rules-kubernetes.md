# Kubernetes 集群的自动停止规则|线束

> 原文：<https://www.harness.io/blog/autostopping-rules-kubernetes>

利用智能云自动停止规则帮助自动管理您的资源，以确保它们仅在使用时运行，而不是在空闲时运行。

企业通常会在云上超支大约 35%。造成云成本账单冲击的最大因素之一是闲置资源以及未分配的资源。如果我们可以在闲置的云资源不被使用时将其关闭，然后在需要时将其重新打开，这不是很好吗？如果这可以自动化呢？

在这里，我们将引入 Harness 智能云自动停止规则来动态和自动地管理您的空闲资源。自动停止规则确保您的非生产资源仅在使用时运行，从不在空闲时运行。此外，它们允许您在完全协调的 spot 实例上运行工作负载，而不必担心任何 spot 中断。配置自动停止规则后，您将:

*   停止为忘记关闭的云资源付费。
*   将与非生产云资源相关的成本降低高达 70%。
*   停止为云浪费买单。

## 自动停止规则有何独特之处？

对于非生产工作负载，自动停止规则是一个动态且强大的资源编排器。以下是将自动停止规则整合到您的云资源中的一些最显著的优势:

*   自动检测空闲时间，并关闭(按需)或终止(现场)资源。
*   支持在完全协调的 spot 实例上运行工作负载，而无需担心 spot 中断。
*   静态预测空闲时间，尤其是工作时间。
*   允许访问停止或终止的机器，这在强制关机的情况下是不可能的。
*   停止没有计算优化的云资源，只有启动/停止操作。

您还可以使用[自动停止仪表板](https://ngdocs.harness.io/article/ehmi6kiynl-autostopping-dashboard)在一个简单直观的界面中查看您创建的所有自动停止规则的摘要。

在自动停止规则之前，只有资源调度程序处理类似的问题。然而，它们带来了各种各样的仿制品:

*   不可能静态预测空闲时间。
*   团队不能访问停止的机器。
*   最小优化。

## 自动停止规则的好处

以下是采用自动停止规则的一些最重要的优势:

*   可衡量的云账单减少，实际节省 70%或更多。
*   自动检查以确保没有闲置资源运行，也没有云浪费发生。
*   没有因人为错误导致费用超支的机会。
*   一旦资源被使用，就不需要记得关闭或终止它们。
*   初始设置后，不需要进一步的手动操作。
*   简化了与当前基础架构供应技术的集成。

## 对 K8s 集群使用自动停止规则

现在，您对自动停止规则的功能和独特性有了更好的理解。让我们探索一下自动停止规则如何帮助您处理 K8s 集群。

自动停止规则支持:

*   EKS (AWS)
*   GKE (GCP)
*   AKS(天蓝色)

创建自动停止规则的步骤很简单:

首先，选择您想要使用自动停止规则(AWS、GCP 或 Azure)管理的工作负载正在其中运行的云帐户。

其次，定义自动停止规则。在这里，您还可以指定资源的空闲时间。这是自动停止规则在停止空闲资源之前等待的时间。

第三，选择要使用此规则管理的资源。您还可以指定高级配置，例如，在两个或多个自动停止规则之间添加依赖关系。当您希望一个规则根据其流量激活一个或多个规则时，这很有用。

第四，更新将应用于集群的 Kubernetes 自动停止规则的资源定义 YAML。

YAML 文件中的规范与带有附加元数据的 Kubernetes 入口/非入口相同。这些是自动停止规则将为您的服务创建的入口/非入口配置。

**启用自动停止的入口示例:**

' API version:ccm.harness.io/v1
kind:autostopping rule
元数据:
name: testK8s
命名空间:default
批注:
harness.io/cloud-connector-id: AWS test 3
nginx.ingress.kubernetes.io/configuration-snippet: ' more _ set _ input _ headers ' autostopping rule:default-testK8s "；'
spec:
idleTimeMins:15
hideProgressPage:false
ingress:
rules:
-host:<替换为您的域名，例如 qa.harness.com>
http:
paths:
-path:/
path type:前缀
后端:
服务:
名称:<替换您的服务名，例如 test > 【T23

**启用自动停止的非入口示例:**

API version:ccm.harness.io/v1
kind:AutoStoppingRule
元数据:
名称:< postgres >
命名空间:< dev-poc >
注释:
harness.io/cloud-connector-id:<connector _ id>
spec:
idleTimeMins:<40>
workloadName:<postgres>
工作负载类型:【T53

## 自动缩放与自动停止规则

自动缩放是一种自动缩放资源以满足不断变化的需求的功能。使用[集群自动缩放器](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler)和[卡尔平特](https://karpenter.sh/)等工具进行自动缩放。

当满足下列条件之一时，Cluster autoscaler 会自动调整 Kubernetes 群集的大小:

*   由于资源不足，有些 pod 无法在集群中运行。
*   群集中有一些节点长期未得到充分利用，它们的 pod 可以放在其他现有节点上。

根据谷歌云，[集群自动缩放器](https://cloud.google.com/kubernetes-engine/docs/concepts/cluster-autoscaler)基于每个节点池工作。使用群集自动缩放配置节点池时，需要指定节点池的最小和最大大小。

Cluster autoscaler 通过在节点池的底层计算引擎管理的实例组(MIG)中添加或删除虚拟机(VM)实例来自动增加或减少节点池大小。Cluster autoscaler 根据在该节点池的节点上运行的 pod 的资源请求(而不是实际资源利用率)做出这些扩展决策。它会定期检查单元和节点的状态，并采取措施:

*   如果因为节点池中没有足够的节点而无法安排单元，那么 cluster autoscaler 会将节点添加到节点池的最大大小。
*   如果节点未得到充分利用，并且即使节点池中的节点较少，也可以调度所有单元，则 cluster autoscaler 会将节点删除到节点池的最小大小。如果在一段超时时间(当前为 10 分钟)后节点无法正常清空，则该节点将被强制终止。

您可能已经发现集群自动缩放器有自己的一套[复杂性](https://cloud.google.com/kubernetes-engine/docs/concepts/cluster-autoscaler#how_cluster_autoscaler_works)和[限制](https://cloud.google.com/kubernetes-engine/docs/concepts/cluster-autoscaler#limitations)。

但是，自动停止规则可以最大程度地节省云成本。对于非生产工作负载，它是一个动态且强大的资源编排器。我们已经讨论了它的独特性和功能。

以下示例显示了如何根据定义的规则处理和自动停止 K8s 集群中的空闲资源。通过自动停止闲置资源来查看资源的累计节省。

Harness 的自动停止解决方案还意识到，在某些情况下，您可能需要您的资源启动并运行，而不管已定义的规则。您可以使用规则配置 ECG 心跳代理和/或固定时间表来提供解决方案。

## 心电图

通过使用自动停止规则，您可以在空闲时自动关闭云资源，然后在需要时重新打开它们。此外，您可以定义自动停止规则在停止空闲资源之前应该等待的分钟数。

默认情况下，自动停止规则会侦听 HTTP/HTTPS 流量。另一方面，资源可以处理长期运行的后台作业，例如机器学习(ML)。在这种情况下，仅使用网络流量来检测资源空闲可能不是最佳策略。因此，您可以使用自动停止规则配置一个名为 ECG 的心跳代理。

此外，您可以假设 ECG 是规则的事件发射器或监听器。它将发送已配置规则的使用记录。

ECG 附带以下预装观察器:

为您的规则配置度量或进程观察器。度量观察器允许您在达到指定的度量阈值时发送心跳信号。进程监视器监视是否存在符合给定条件的进程。

**指标举例:**

#当度量大于或等于配置的阈值
T5【度量】
cpu = "40"
memory = "5Gb "时，将发送心跳

**流程示例:**

#找到符合条件的进程时会发送心跳

【process】
condition = " python * "

## 固定时间表

自动停止规则也支持固定计划。在某些情况下，您不希望您的资源减少或增加。例如，每周五下午 5 点，您希望 ABC 资源停止运行。自动停止规则使这变得非常简单。您可以为 ABC 资源安排停机时间。在此期间，无论定义了什么规则，都将强制关闭资源。此外，您可以用同样的方式指定资源的正常运行时间。

## 开始管理闲置的云资源

下一个合乎逻辑的问题是如何开始使用这种智能和自动化的云成本管理解决方案:

*   简单地[注册一个演示](https://harness.io/products/cloud-cost/)，一个线束专家将带你开始线束云成本管理。
*   如果您想了解更多关于如何为 K8s 资源创建自动停止规则的信息，请查看这些[文档](https://ngdocs.harness.io/article/1r80jdz2f9-create-autostopping-rules-for-kubernetes)。