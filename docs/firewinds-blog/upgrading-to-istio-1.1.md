# 升级到 Istio 1。

> 原文：<https://www.fairwinds.com/blog/upgrading-to-istio-1.1>

Istio 最近发布了其流行服务 mesh 的 1.1.1 版本，进行了一些更改和改进。毫无疑问，您应该从早期版本升级，以利用 Kubernetes 集群中的这些改进。但是升级并不简单——Istio 的服务网几乎影响了集群通信的每个方面！除了升级 Istio 的控制面板，每个边车都必须升级。在接下来的部分中，我将概述一些应该计划的主要变化，以及在集群中升级 Istio 可以采取的步骤。

## 有什么变化

如果你是一个 helm 用户，需要注意的最大变化是 CRD(自定义资源定义)现在由一个名为`istio-init`的单独的 helm 图表安装。这个新图表应该在控制面板升级之前安装，否则升级可能会失败。为了简化安装，添加了配置文件，它们是用于不同需求的舵值文件。

### 活性和准备就绪探测

在以前的版本中，当启用 mTLS 时，就绪性和活性探测一直是一个棘手的问题，因为 kubelet 没有 istio 颁发的证书来与服务通信。这使得使用 http 和 tcp 检查变得不可能。版本 1.1 引入了一个探针重写，可以将活跃度探针请求重定向到引导代理。然后，pilot 代理将请求重定向到 pod，允许 kubelet 在没有 istio 颁发的证书的情况下发送请求。换句话说，您可以再次使用 http/tcp 活跃度和就绪性探测！

### mTLS

如果您正在使用注释来覆盖 mTLS 策略，我很抱歉地说，这将不会继续下去。这已被一种用身份验证策略和目标规则替代的新方法所取代。使用策略和 DestinationRule 对象提供了更多的灵活性，但是在升级过程中您需要记住这一点。 ****

**边车**

在过去，很难配置超出开箱即用配置的代理边车。引入了一个新的 sidecar 资源，允许您配置代理将接受来自(入口和出口)的流量的端口和协议。该资源是动态的，您可以在一个名称空间中指定工作负载的子集来应用配置。
 ****

**入口**

在我看来最令人兴奋的变化是对 istio ingress 的弃用。这为更多的本地 Kubernetes 支持铺平了道路，随着这一变化，您可以使用 Kubernetes ingress 的可选支持。这允许您使用入口对象来定义入口点，并且您可以使用一个新的控制器来动态加载和旋转外部证书，包括 LetsEncrypt。在 [Istio 1.1 发行说明](https://istio.io/about/notes/1.1/)页面上可以详细查看这些变更以及其他一长串变更。

## 升级过程

升级通常是通过备份 istio CRDs，安装新的`istio-init`舵图，用舵升级控制平面，然后升级所有的边车来完成的。

备份您的 CRD！

备份现有的 CRDs 是必要的，以防安装过程中出现问题。Kubectl 让这变得非常简单:

**安装 Istio 初始化器图表**

一旦备份了 CRDs，就该开始安装 istio 图表了。这些都可以在 Istio repo 中获得，所以我们需要首先使用下面的命令将其克隆到我们的工作站。所有适用的舵图都将在这个 repo 的`install/kubernetes/helm`子目录中。

如果你正在使用计算者，你很幸运，因为它可以从 git repo 安装图表。计算者是一个由[费尔温德](/)开发的舵的命令行助手，它使用 YAML 语法在一个文件中安装和管理多个舵图表。

我们需要安装的第一个图表是名为`istio-init`的 Istio 初始化器图表。它管理 istio 需要的所有 CRD。将这个图表安装到您已经安装了 Istio 的同一个名称空间是一个很好的做法。在这个例子中，我们在`istio-system`名称空间中有 Istio。

`helm install install/kubernetes/helm/istio-init --name istio-init --namespaceistio-system`

如果您正在使用 Reckoner，可以将此片段添加到您的课程中。yml:

然后，可以用命令`reckoner plot course.yml --heading istio-init`安装图表。

安装图表后，运行几个作业来创建 CRD。在继续之前，您需要确保它们都已完成。您可以通过`watch`和`kubectl` : `watch -d "kubectl get job -n istio-system | grep istio-init-crd"`来监控状态

**升级控制平面**流程的下一步是升级 Istio 控制平面。在你继续之前，有几个新的舵图表值需要注意:

一旦您的图表值配置完毕，您就可以使用 helm: `helm upgrade istio install/kubernetes/helm/istio --namespace istio-system -f <path-to-values>.yml`升级版本了

这里有一个例子片段，你可以用它来升级 Istio。可以使用命令
`reckoner plot course.yml --heading istio`升级图表。

**升级边车**

升级控制面板后，所有使用 istio 的应用程序仍将使用旧版本的 sidecar 代理。随着豆荚被重新装载，他们将得到更新的边车。通过自然的 pod 重启来接收更新是完全可以接受的，但是您可能想要考虑触发所有部署的滚动更新，以使它们都在相同的 sidecar 版本上。Istio 建议通过编辑每个启用了 istio 的部署规范中的普通字段来触发更新。这个代码片段通过向每个部署添加注释来实现这一点:

## 祝你好运！

享受升级后的 Istio 安装，它改进了对活性/就绪性探测的支持，更新了证书处理，并在 Kubernetes 生态系统中提供了更大的灵活性！