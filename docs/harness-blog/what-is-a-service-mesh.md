# 什么是服务网格？装具

> 原文：<https://www.harness.io/blog/what-is-a-service-mesh>

服务网格提供了四个支柱。服务网格能够连接您的服务、保护通信、充当控制平面，并且能够观察和被观察。

在过去的几年中，一种叫做服务网格的网络技术在云原生生态系统中越来越受欢迎和采用。与任何新技术一样，服务网格解决特定的问题。随着应用/应用层向微服务架构发展，或者正在使用微服务架构构建，端点(您的服务)的数量也在增加。容器化应用的兴起和 Kubernetes 的流行无疑为服务网格技术投资让路。

随着具有更多端点和容器的短暂性的体系结构的双重变化，网络复杂性急剧增加。随着 Kubernetes 不断旋转容器，为虚拟机或物理机提供永久地址的日子已经一去不复返了。您的服务仍然需要与内部和外部世界进行通信，拥有一个服务网格可以带来一个控制平面和功能来帮助处理现代网络复杂性。

## 服务网格做什么？

服务网格是一个专用的基础设施层，它提供了四个支柱。服务网格能够连接您的服务、保护通信、充当控制平面，并且能够观察和被观察。

### 连接

随着容器化微服务所需的网络蔓延和重新平衡，让服务相互通信以及与外部世界通信可能是一个挑战。通过为不在永久基础设施上的服务提供路由和连接，服务网格可以成为通信的管道。因为是编程实现，所以可以通过流量转移来实现网络转移技术，如蓝绿色或金丝雀部署。

### 安全的

对于容器化的微服务，保持一致的安全通信也是一个挑战。由于所有的移动部件和远程调用，确保流量具有一致的加密是服务网格的一个好处。远程调用可以触发几个下游远程调用来完成微服务模型中的请求。

### 控制

有了服务网格，你也就有了控制平面。这里的一个关键支柱是基于策略的控制。通过基于策略的控制，可以对流量进行实时和基于规则的处理。某些流量可以被标记/负载平衡到不同的端点，并且可以实现更高级的负载平衡方案，例如断路模式。

### 观察

对于应用程序团队来说，网络级的跟踪和追踪曾经是一个谜，因为这一职责被委托给了网络团队。通过跟踪和追踪应用流量和调用，您可以了解很多关于应用堆栈的信息。有了服务网格，就可以记录和检查服务流量，甚至是聚合服务流量。端到端监控、分布式跟踪、指标和日志记录现在都可以在服务网格级别使用。

## 为什么需要服务网格？

随着两条战线的崛起，组织和平台工程团队正在采用更加分布式的微服务模型，并支持企业中日益增长的多语言和框架。需要服务网格来支持跨无数语言和微服务的通信。为短暂/中转基础设施创建并硬编码 DNS 条目将是一项不间断的工作，尤其是随着需要支持的服务数量的增加。

## 什么时候需要服务网格？

像任何技术一样，这“取决于”组织的需求和拥有服务网格带来的好处。对于大多数调用都在运行的应用程序内部的整体架构(即:方法调用)，将服务网格作为整体架构的门面没有多大意义。如果堆栈中只有几个同构的端点(即:用相同的语言和相似的应用程序基础设施编写的)，使用服务网格将是大材小用。

根据[新堆栈](https://thenewstack.io/when-you-do-and-dont-need-a-service-mesh/)的说法，随着技术堆栈变得更加异构，终端数量激增，服务网格可以解决多个网络和运营挑战。服务网络的四个支柱(连接、保护、控制和观察连接性)投资于一个中心点更有意义。

## 如何实现服务网格？

有几个服务网格项目和提供商。同样，Kubernetes 是杰出的容器编制者，Istio 是杰出的服务网格。服务网格有几个拓扑需要考虑，比如[边车代理](https://istio.io/latest/docs/reference/config/networking/sidecar/)，以及其他几个服务网格提供者，比如 [LinkerD](https://linkerd.io/) / [浮力](https://buoyant.io/)、[咨询](https://www.consul.io/)、 [Solo](https://www.solo.io/) 和 [AWS 应用网格](https://aws.amazon.com/app-mesh/)。

要开始使用服务网格，需要安装控制器/控制平面。然后，在应用节点和控制器之间需要有一个通信机制。和其他任何技术一样，第一天不需要煮海洋。人们可以将注意力集中在控制器安装和第一个代理(或注入)上，以处理服务网格的基本流量/目的地规则。随着信心和技能的增长，人们可以开始扩展到服务网格的其他支柱，并扩展规则集的复杂性。不过，如果你现在就开始，Kubernetes 空间中最受欢迎的服务网格是 Istio。

## 什么是 Istio 服务网格？

Istio 成立于 2018 年夏天，是一个开源项目，是 IBM、谷歌和 Lyft 的联合合作伙伴关系。在 Istio 之前，如果你正在寻找一个服务网格，那么路线是利用几个网飞 OSS 项目，如 [Zuul](https://github.com/Netflix/zuul) 、 [Ribbon](https://github.com/Netflix/ribbon) 和 [Hystrix](https://github.com/Netflix/Hystrix) 。Istio 用服务网格功能扩展了 Envoy 代理项目。

Istio 有四个主要组成部分。这三个非特使组件正朝着管理和配置的单一分布发展。

### 使者

[Envoy](https://istio.io/latest/docs/ops/deployment/architecture/#envoy) 是主要的 sidecar 代理，允许控制层和数据层之间的应用程序通信。Envoy 支持故障注入、动态服务发现和 TLS 终止等功能。

### 飞行员

[Pilot](https://istio.io/latest/docs/reference/commands/pilot-agent/) 负责跨服务网格的特使实例的生命周期。Pilot 有助于动态生成特使特定的配置。

### 活版盘

[Galley](https://github.com/istio/istio/tree/master/galley) 将配置 YAML 转换为 Istio 本机理解的格式。简而言之，Galley 为 Istio 提供了配置管理。

### 城堡

Citadel 跨服务网格管理密钥和证书。除了飞行员和厨房，Citadel 的功能也被新的 istiod 打包。

无论您处于 Istio 或 Service Mesh 旅程的哪个阶段，开始使用线束都是轻而易举的事情。

## Istio 服务网和线束

Istio 中一个非常强大的模式是[流量转移](https://istio.io/docs/tasks/traffic-management/traffic-shifting/)。能够在流量分流模式中对服务应用百分比/权重(例如:服务的 v1 和 v2)为[金丝雀部署](https://harness.io/blog/blue-green-canary-deployment-strategies/)打开了大门。

在部署过程中，您很可能会碰到流量转换规则。编排一组 [KubeCTL](https://kubernetes.io/docs/reference/kubectl/overview/) 和 [IstioCTL](https://istio.io/docs/reference/commands/istioctl/) 命令，维护配置，以及为这些任务设计故障(回滚)当然需要适当的计划和思考。具有[流量管理](https://developer.harness.io/docs/first-gen/continuous-delivery/kubernetes-deployments/set-up-kubernetes-traffic-splitting/)支持的 Harness 平台允许您远离编排和故障复杂性，只关注规则和结果本身。

### 创建线束应用程序和服务

用一个名为“nginx_k8s”的线束服务创建一个名为“My K8s”的新线束应用程序，这只是 nginx 通过 [Docker Hub 得到的。](https://hub.docker.com/)

#### Nginx 工件布线

Docker 图像名称“library/nginx”

### 创建线束工作流

创建新的线束工作流。

导航至设置->您的应用->工作流，然后“+添加工作流”我们将把我们的新工作流程称为“Istio Canary”

#### 将部署阶段添加到工作流中

接下来，您可以将部署阶段添加到您的工作流中。在这个例子中，我们利用了一个预先存在的“nginx_k8s”服务，我们将把它部署到 Minikube。

#### 将流量分流添加到工作流中

接下来，使用 Kubernetes 列下的“+添加阶段”将流量分割添加到验证阶段。

#### 配置流量分割权重

设置流量分割的权重。根据需要多少 canary 交互，它可以有多个拆分。在这里，我利用 20/80 的分割。

### 添加目的地规则

回到“nginx_k8s”的线束服务定义 Harness 可以用作 UI 来填充需要发送进行配置的项目。

在下面的 manifest 部分，让我们添加另一个名为 istio.yaml 的文件，该文件包含一个基本的 Istio [目的地规则](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/#DestinationRule)和[虚拟服务](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/)。Harness 能够代表您应用 YAMLs，我们将利用这一点来实现“DestinationRule”和“VirtualService”

为此，将鼠标悬停在文件夹上，然后选择“+ Add File”并添加一个 istio.yaml。

#### 修改 istio.yaml

在 [istio.yaml](https://github.com/ravilach/harness/blob/master/istio.yaml) 中，添加 istio 目的地规则和 Istio 虚拟服务的[基本配置](https://developer.harness.io/docs/first-gen/continuous-delivery/kubernetes-deployments/set-up-kubernetes-traffic-splitting/)。

[样本 istio.yaml](https://github.com/ravilach/harness/blob/master/istio.yaml)

API version:networking.istio.io/v1alpha3
kind:destination rule
metadata:
批注:
staging-devharnessio.kinsta.cloud/managed: true
name:{ { . values . name } }-destination rule
spec:
host:{ { . values . name } }-SVC
traffic policy:
load balancer:
simple:RANDOM
-
API version:networking.istio.io/v1alpha3
kind:virtual service
元数据:
批注

然后，单击保存。

### 运行您的服务网格部署

导航到持续部署->开始新部署

点击提交后，我们可以看到流量分流的应用。

放大:

至此，您的第一个服务网格已经在 Harness 的帮助下部署到所有人了！

## 服务网格的常见问题

在之前的网络研讨会中，有一些围绕服务网格提出的问题。

### 什么是软件开发中的服务网格时代？

有一个正在进行的“左移”心态的运动(将更多的决策和责任转移给开发团队)。网络一直被视为传统的基础设施角色，需要很长的准备时间来修改/扩展网络规则。随着容器/短暂基础设施和微服务的蓬勃发展，网络拓扑已经从绝对的转变为动态的/基于规则的。服务网格技术允许对基于业务逻辑/规则的网络规则和操作进行编码，以便由软件工程师进行修改。

### 微服务架构中的服务网格是什么？

服务网格是一个网络基础设施，可以被开发团队修改。在网络堆栈中提供连接性、安全性、控制和可观察性，以及服务如何与内部和外部世界通信。随着端点和微服务的增加，服务网格将开始在架构中扮演更重要的角色。

### 服务网格只用于微服务吗？

使用服务网格和学习曲线是有开销的。微服务确实暗示自己是服务网格的更好候选者。如果您的应用程序使用 HTTP/HTTP2/gRPC，那么您的应用程序很容易成为服务网格的一部分。服务传播不仅仅是微服务模式。

### Kubernetes 是服务网格吗？

Kubernetes 本身并不是一个服务网格，尽管随着 Kubernetes 作为一个平台和集群的兴起，服务网格变得流行起来。

## 利用线束简化您的连续交付管道

无论您在[连续交付](https://harness.io/blog/what-is-continuous-delivery/)或服务网状旅程的哪个阶段，Harness 都会与您携手合作。Harness 有一个完整的端到端软件交付平台，可以带你从想法到生产。如果你还没有一个脊甲账号，今天就可以在[注册](https://app.harness.io/auth/#/signup/)！

干杯！

拉维