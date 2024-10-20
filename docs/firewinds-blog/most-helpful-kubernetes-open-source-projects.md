# 我们使用的最有帮助的 Kubernetes 开源项目

> 原文：<https://www.fairwinds.com/blog/most-helpful-kubernetes-open-source-projects>

 Kubernetes 本身就是最好的开源贡献之一——这就是为什么我们 Fairwinds 的团队 100%致力于这个项目和社区。

我们的 SREs 团队致力于帮助企业最大限度地受益于 Kubernetes。我们使用许多开源工具，并贡献项目，以确保组织能够运行安全、可靠和可扩展的 Kubernetes 基础设施。

我们询问了我们的销售代表他们最喜欢的开源项目。在这里，我们列出来，以帮助您的 Kubernetes 之旅。

## **nginx-ingress+cert-manager+外部 dns 的组合**

ingress-nginx 是 Kubernetes 的入口控制器，使用 nginx 作为反向代理和负载平衡器。

[cert-manager](https://github.com/jetstack/cert-manager) 是 Kubernetes 的一个附加组件，用于自动管理和发布来自各种发布源的 TLS 证书。它将确保证书有效并定期更新，并尝试在到期前的适当时间续订证书。

[ExternalDNS](https://github.com/kubernetes-sigs/external-dns) 将公开的 Kubernetes 服务和入口与 DNS 提供商同步。它使得 Kubernetes 资源可以通过公共 DNS 服务器被发现。像 KubeDNS 一样，它检索资源列表(服务、入口等)。)来确定所需的 DNS 记录列表。然而，与 KubeDNS 不同，它本身不是 DNS 服务器，而只是相应地配置其他 DNS 提供商——例如 AWS Route 53 或 Google Cloud DNS。

nginx-ingress+cert-manager+external-DNS 的组合允许系统管理员自动完成一些历史上最困难或最烦人的任务。

## **舵**

[Helm](https://helm.sh/) 帮助您管理 Kubernetes 应用程序——Helm 图表帮助您定义、安装和升级最复杂的 Kubernetes 应用程序。

我们使用 Helm 安装内部应用程序和第三方应用程序，但真正喜欢它安装第三方应用程序。

## **清算者+舵手**

计算者是由费尔温德 SRE 团队创建的 helm 命令行助手。该实用程序以多种方式增加了 Helm 的功能:

*   创建声明性语法，以便在一个位置管理多个版本
*   允许从 git 提交/分支/发布安装图表

计算者为 Kubernetes 团队节省时间和头痛。

## **kubectl-ns**

[**kubectl-ns**](https://github.com/juanvallejo/kubectl-ns) 允许你通过 kubectl 快速查看或更改当前名称空间。它非常具体，每次使用只节省一点时间。但是因为我们的团队每天使用它很多次，所以这是一个巨大的努力与价值的比率。[库贝克斯](https://github.com/ahmetb/kubectx)和[库本斯](https://github.com/ahmetb/kubectx/blob/master/kubens)是你可以使用的类似工具。

## **kubetails**

kubetails 是 bash 脚本，它使您能够将来自多个 pod 的日志聚合(跟踪)到一个流中。这与运行“kubectl logs -f”是一样的，只是针对多个 pod。

## 船尾

[船尾](https://github.com/wercker/stern)允许你在 Kubernetes 上跟踪多个吊舱和吊舱中的多个集装箱。每个结果都用颜色编码，以便更快地调试。该查询是一个正则表达式，因此可以很容易地过滤 pod 名称，并且您不需要指定确切的 id(例如省略部署 id)。如果一个 pod 被删除，它将从尾部移除，如果添加一个新的 pod，它将自动添加尾部。当一个 pod 包含多个容器时，Stern 也可以对所有容器进行尾部处理，而不必对每个容器进行手动处理。只需指定容器标志来限制要显示的容器。默认情况下，所有容器都会被监听。

## **kube-capacity**

kube-capacity 是一个简单的 CLI，它提供了 Kubernetes 集群中资源请求、限制和利用率的概述。它试图将 kubectl top 和 kubectl describe 输出的最佳部分结合到一个易于使用的集中于集群资源的 CLI 中。

## **金发女孩**

Fairwinds 的开源工具 Goldilocks 是一个 Kubernetes 控制器，它提供了一个仪表盘，给出了如何设置资源请求的建议。为了提供建议，我们使用[垂直 Pod 自动缩放器](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler) (VPA)。VPA 控制器堆栈包含一个建议引擎，它会考虑到您的 pod 的当前资源使用情况，以便提供指导。VPA 的主要目标是实际上为你设置这些，但我们目前对它如何做到这一点并不满意；更具体地说，我们喜欢运行水平吊舱自动缩放，这并不适合 VPA。相反，我们只是使用 VPA 提供的推荐引擎，就如何设置您的资源请求和限制向您提供良好的建议。你可以在我们的博客上了解更多关于[金发女孩的信息。](https://www.fairwinds.com/news/introducing-goldilocks-a-tool-for-recommending-resource-requests)

作为我们对 Kubernetes 社区承诺的一部分，Fairwinds 的 SREs 团队已经开发了许多[开源工具](/open-source-software)。

[![Learn more about Fairwinds Open source tools](img/7082525820e14a08115382e83ce5de51.png)](https://www.fairwinds.com/open-source-software)