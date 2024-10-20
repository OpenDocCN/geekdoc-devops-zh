# 如何在您的 Kubernetes 集群中采用 Istio

> 原文：<https://www.fairwinds.com/blog/how-to-adopt-istio-into-your-kubernetes-clusters>

最近，Istio 受到了很多关注。服务网格平台最近达到了 1.0 生产就绪里程碑。谷歌一直在戏弄[谷歌云上的托管 Istio 选项](https://cloud.google.com/istio/)。在 Kubernetes 集群中有足够的资源让它运行。

你可能已经经历了科技炒作周期的各个阶段:

1.  什么是服务网格？
2.  哇哦。服务网有点复杂。我真的需要一个吗？
3.  好的。我可以看到一些使用案例。完成这个“*Istio Up&15 分钟跑完*”演示需要多少天的时间？

现在，您已经实际上为您现有的 Kubernetes 托管应用程序设置了 Istio。那要比 15 分钟长得多。这篇文章将告诉你如何将 Istio 引入你的 Kubernetes 集群。

### Istio 到底是什么？

如果没有别的，你可能听说过 Istio 是一个服务网格。“服务网格”是一个奇特的术语，指的是处理一组连接服务之间常见通信挑战的工具。

在现实世界中，Istio 的架构建立在 Kubernetes 的基础上，增加了:

*   用于服务之间的识别、授权和加密通信的相互 TLS。
*   使用选择性白名单限制出站流量。
*   动态流量分布模式，如并发应用程序版本和渐进的金丝雀式部署。
*   通过断路器、重试处理、故障转移和对“混乱猴子”式故障注入测试的支持来提高弹性。
*   说明沟通模式和绩效的大量额外指标。

Istio 的架构通过在每个 pod 中运行一个单独的代理 sidecar 容器来实现这一切。一组核心服务在您的集群中运行，并与这些代理边盘通信，以实现上述功能。

### 电脑:给我安装一个 Istio

Kubernetes 上首选的 Istio 架构安装方法是[舵](https://helm.sh/)图。请注意， [Istio 掌舵图](https://github.com/istio/istio/tree/master/install/kubernetes/helm/istio)嵌入在 Istio Github 回购中。这是不可通过官方社区舵图表回购。

该图表安装了 Istio 运行所需的一组核心服务。它通过图表值提供了广泛的定制。它还支持升级到新的 Istio 版本。

在开始安装 Istio 之前，请继续阅读，了解一些在 Kubernetes 上安装和运行时特有的优势。

#### 你想如何在你所有的豆荚里安装边车代理？

你所有的豆荚都需要一个边车代理容器。Istio 可以将这个代理容器自动注入到新的 pod 中，而无需对部署进行任何更改。默认情况下该特性是启用的，但是它需要 Kubernetes 或更高版本。

您需要标记名称空间以启用边车注入:

```
$ kubectl create namespace my-app
$ kubectl label namespace my-app istio-injection=enabled
```

Istio chart 安装不会影响现有的 pod。在带标签的命名空间中启动的 pod 将接收 sidecar，而在其他命名空间中启动的则不会。这为安装 Istio 的核心服务和有选择地将 pod 转移到网格中提供了一条途径。

#### 您将如何将服务过渡到互助 TLS？

Mutual TLS 是一个有吸引力的安全特性，因为它限制了哪些服务可以相互通信，并且它对通信进行加密。但是，当您在转换服务时，它们需要与网格外部的资源进行通信，这可能会成为绊脚石。

为了简化这种转换，考虑[启用*许可*模式认证策略](https://istio.io/docs/tasks/security/mtls-migration/)。许可模式为服务启用明文 HTTP 和 mTLS 流量。该策略可以在网格策略中应用于集群范围，使用默认策略应用于每个命名空间，或者使用更细粒度的策略来针对单个服务。一旦您的应用程序完全迁移到 Istio，您就可以禁用许可模式并强制实施相互 TLS。

另一件要注意的事情是，典型的 Kubernetes HTTP 活跃度和就绪性探测在 mTLS only 模式下不起作用，因为探测来自服务网格之外。这些检查将在*许可*模式启用的情况下进行。一旦您取消*许可*模式，您将需要将探头转换为*执行*检查，以轮询 pod 内部的健康检查，或者在禁用 mTLS 的情况下在单独的端口上建立健康检查端点。

以下是创建网状策略的方法，该策略使整个集群的 MTL 处于许可模式:

```
cat <<EOF | kubectl apply -f -
apiVersion: “authentication.istio.io/v1alpha1”
kind: “MeshPolicy”
metadata:
  name: “default”
spec:
  peers:
  — mtls:
      mode: PERMISSIVE
 EOF
```

#### 您想在启用出口筛选的情况下开始吗？

默认情况下，Istio [会限制您的 pod 的出站流量](https://istio.io/docs/tasks/traffic-management/egress/)。这意味着，如果您的 pod 与网格外部的服务进行通信，如云提供商 API、第三方 API 或托管数据库，流量将被阻止。然后，您可以使用 [ServiceEntries](https://istio.io/docs/tasks/traffic-management/egress/#setting-route-rules-on-an-external-service) 有选择地启用对外部服务的访问。

从表面上看，出口过滤似乎是一个有吸引力的安全功能。然而，它的实现方式并没有提供真正的安全优势。正如 Istio 的一篇文章中所解释的，没有任何机制可以确保被过滤的 IP 与名称相匹配。这意味着只需在 HTTP 请求中设置一个主机头就可以绕过过滤。

此外，作为现有应用程序的起点，出口过滤可能很困难。或者，您可以使用[全局*包含入口*设置](https://istio.io/docs/tasks/traffic-management/egress/#calling-external-services-directly)在集群级别禁用出口过滤。通过将此设置为集群的内部 ip 空间，您将绕过 Istio 代理处理所有外部流量。一旦在 Istio 中运行了服务，您就可以识别外部服务，并在打开出口过滤之前构建所需的*服务条目*。

以下头盔安装将完全禁用所有吊舱的出口过滤。您应该将内部集群 IP CIDR 添加到 *includeIPRanges* 设置中，以便通过 Istio 将流量从 pod 路由到内部资源，同时完全绕过外部端点。使用这两种配置中的任何一种，您都不需要为访问外部端点定义任何 ServiceEntries。

```
$ helm upgrade install/kubernetes/helm/istio — install \
  --name istio — namespace istio-system \
  --set global.tag=”1.1.0.snapshot.1" \
  --set global.proxy.includeIPRanges=””
```

#### Istio 在设计上并不完全是 Kubernetes 本地的

毫无疑问，Istio 为 Kubernetes 上的部署提供了大量资源。有一个舵图表，其中有一系列子图表，用于安装大量与 CRD 相关的服务。有大量的文档和示例图表配置。然而，Istio 的目标是独立于平台。考虑到这一点，在 Kubernetes 上部署支持 Istio 的应用程序有一些明显的不同。

最有影响力的差异之一在于在 Istio 网格之外公开服务。一个典型的 Kubernetes 应用程序使用一个连接到入口控制器的入口来公开一个外部接口。相反，Istio 依赖于一个[网关对象](https://istio.io/docs/tasks/traffic-management/ingress/)来定义协议设置，比如端口和 TLS。网关对象与虚拟服务对象配对来控制路由细节。

这可能会影响您已经为入口对象(如域名和 TLS 证书管理)建立的模式。[外部 dns](https://github.com/kubernetes-incubator/external-dns) 项目最近引入了对 Istio 网关对象的[支持，使得从入口对象到自动域管理的过渡更加容易。](https://github.com/kubernetes-incubator/external-dns/releases/tag/v0.5.6)

*网关*的 TLS 证书故事仍然有些复杂。Istio 的 *IngressGateway* 不支持与 [cert-manager](https://github.com/jetstack/cert-manager) 兼容的多种证书，后者是一种从 Kubernetes 中自动提供和安装 TLS 证书的流行方式。证书更新需要滚动*ingress gateway*pod，这使得事情更加复杂。这使得在不按计划手动滚动 pod 的情况下，很难使用短期 Let 加密证书。即使使用静态证书，网关也会在启动时安装所有证书，因此为附加服务添加和删除证书也需要重新启动。共享一个网关的所有服务也可以访问所有其他服务的证书。

目前，在*网关*下开始使用 TLS 获取外部服务的最简单选项是:

1.  在类似 Amazon 弹性负载平衡器的地方终止网格外部的 TLS，并让 Amazon 的证书管理器管理证书。
2.  避免使用动态证书提供商，比如让我们在 Istio *IngressGateway* 上加密并安装多域或通配符 TLS 证书。

#### 在 Kubernetes 上找到通往生产级 Istio 的道路

现在您已经了解了 Istio 在 Kubernetes 上的一些优势，您可以继续在您的 Kubernetes 集群中安装 Istio 架构。这个帖子远没有提供生产级的配置。然而，它将使您的工作受到最小的干扰，以便您可以找到自己的工作生产配置之路。