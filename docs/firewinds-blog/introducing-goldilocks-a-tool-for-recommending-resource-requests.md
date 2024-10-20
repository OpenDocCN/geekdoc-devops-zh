# Goldilocks:一个推荐资源请求的开源工具

> 原文：<https://www.fairwinds.com/blog/introducing-goldilocks-a-tool-for-recommending-resource-requests>

 Fairwinds 的客户最常向我提出的一个问题是“我们如何知道如何设置我们的资源请求和限制？”今天我们开源了一个工具，希望能在这方面给大家一点帮助。Goldilocks 是一个 Kubernetes 控制器，它提供了一个指示板，给出了关于如何设置资源请求的建议。

为了提供建议，我们使用了  [垂直吊舱](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler) (VPA)。VPA 控制器堆栈包含一个建议引擎，它会考虑到您的 pod 的当前资源使用情况，以便提供指导。VPA 的主要目标是实际上为你设置这些，但我们目前对它如何做到这一点并不满意；更具体地说，我们喜欢运行水平吊舱自动缩放，这并不适合 VPA。相反，我们只是使用 VPA 提供的推荐引擎，就如何设置您的资源请求和限制向您提供良好的建议。

我们利用 VPA 推荐引擎的方式很简单。我们在集群中运行一个控制器，监视标有  `goldilocks.fairwinds.com/enabled=true`的名称空间。在这些启用的名称空间中，我们查看每个部署对象并创建相应的 VPA 对象。VPA 设置为  `Mode: Off` ，并不实际修改您的资源请求和限制；它只是坐在那里，提供一个建议。这本身就很酷，但是为了查看这些推荐，你必须使用  `kubectl` 来查询每一个 VPA 对象。对于大中型部署，这可能会非常繁琐。这就是仪表板的用武之地。

“金发女孩仪表板”直观地展示了 VPA 建议。现在，您可以访问集群中的服务并查看两种类型的建议，这取决于您希望部署的 QoS 等级。仪表板如下所示:在

*Goldilocks Dashboard Preview*

如果你现在想自己安装金发女孩，请前往 https://github.com/FairwindsOps/goldilocks寻求指导。否则继续阅读，看看一些常见问题。

## 我如何安装金发女孩？

安装金发女孩最简单的方法是使用我们的  [舵轮图](https://github.com/FairwindsOps/charts/tree/master/stable/goldilocks)。您可以通过运行以下命令来实现:

```
helm repo add fairwinds-stable https://charts.fairwinds.com/stable
helm install --name goldilocks --namespace goldilocks --set installVPA=true fairwinds-stable/goldilocks
```

这将安装 VPA(来自他们的回购)、金发姑娘控制器和金发姑娘仪表板。

现在您可以通过端口转发访问仪表板:

```
kubectl -n goldilocks port-forward svc/goldilocks-dashboard 8080:80
```

其他选项可在  [图表](https://github.com/FairwindsOps/charts/tree/master/stable/goldilocks) 和  [金发女孩储存库](https://github.com/FairwindsOps/goldilocks)中找到。

## 如果我想从仪表板中排除容器该怎么办？

这是我们对金发女孩的第一次升级，主要是因为 sidecar 代理模式。我们经常看到每个单元都有一个边车的集群，例如 Istio 或 Linkerd2 边车。您可能不想看到这些容器的资源建议，因为我们可能控制这些值，也可能无法控制这些值。有两种方法可以排除这些容器。

第一种方法是全局的，可以通过运行带有标志  `--exclude-containers "istio-proxy,linkerd-proxy"`的仪表板来设置。这是在舵图安装中默认设置的。

第二种方法是使用部署本身的标签来排除每个部署的容器:  `goldilocks.fairwinds.com/exclude-containers=linkerd-proxy,istio-proxy`。

## 这个 QoS 类别是什么？

如果您还不熟悉它，QoS Class 指的是设置资源请求和限制的不同方式。有两个是我们关心的，还有一个几乎不应该使用。你可以在 Kubernetes 文档中阅读关于它们的详细信息  [，但是我将在这里对它们进行总结。](https://kubernetes.io/docs/tasks/configure-pod-container/quality-service-pod/)

**保证** —这是我最喜欢的 QoS 类别，因为它通常适用于最稳定的 Kubernetes 集群。在这个类中，您将您的资源请求和限制设置为完全相同的值，这保证了容器所请求的资源在它被调度时是可用的。

**可突发** —这意味着您的资源请求低于您的限制。本质上，调度程序将使用请求将 pod 放在一个节点上，但是随后 pod 可以使用更多的资源，直到它被终止或限制为止。

**BestEffort** —不设置任何资源请求或限制。我们不建议你这么做。

Goldilocks 仪表板将为您提供有保证的和可突发的 QoS 类别的建议。这些是由 VPA 建议字段中的不同值生成的。在保证类中，我们建议您将您的请求和限制设置为 VPA“目标”字段。通常，我们将该值与水平 Pod 自动缩放器一起使用，以允许我们的应用程序进行缩放。突发类推荐来自 VPA 下界和上界油田。您可以在他们的回购中了解更多关于来自 VPA [的推荐人的信息。](https://github.com/kubernetes/autoscaler/blob/master/vertical-pod-autoscaler/pkg/recommender/README.md#implmentation)

### 感谢阅读

如果您有问题(或有如何改进的想法！)我们希望收到您的来信。联系我们的最佳方式是通过  [Github](https://github.com/FairwindsOps/goldilocks) 上的请求或问题。感谢阅读！