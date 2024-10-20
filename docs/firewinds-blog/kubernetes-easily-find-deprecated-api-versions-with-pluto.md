# Kubernetes 已解决:使用 Pluto 很容易找到已弃用的 API 版本

> 原文：<https://www.fairwinds.com/blog/kubernetes-easily-find-deprecated-api-versions-with-pluto>

 ### Pluto 是一个开源工具，可以帮助用户在他们的基础设施即代码仓库和 Helm 版本中轻松找到不推荐的 Kubernetes API 版本。

Kubernetes API 版本化方案对开发人员来说非常好。它允许 Kubernetes 团队轻松地向 alpha 和 beta API 路径发布新特性，并在经过实战测试后将它们升级到稳定路径。当这种情况发生时，旧版本[被弃用](https://kubernetes.io/docs/reference/using-api/deprecation-policy/#deprecating-parts-of-the-api)，最终将被删除。这引发了许多围绕 [Kubernetes 1.16 升级](/blog/kubernetes-1.18.0-is-out-now-what)的讨论，其中删除了多个不赞成使用的`Deployment`资源版本(除此之外，在这里阅读更多)。找到被否决的版本是很痛苦的，你将被阻止升级到 1.16，直到它们都被更新。我们决定编写一个开源实用程序，[冥王星](https://github.com/FairwindsOps/pluto)(以废弃的行星命名)，来解决这个问题。

## **背景**

kube-apiserver 非常灵活，不管请求中指定的 API 版本如何，它都会提供关于给定资源类型的相同信息。例如，在运行 Kubernetes 1.15.X 时，您可以运行以下命令来查看各种 API 版本的部署:

```
$ kubectl get deployments.v1beta1.apps rbac-manager -o yaml
apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: rbac-manager
...

$ kubectl get deployments.v1beta2.apps rbac-manager -o yaml
apiVersion: apps/v1beta2
kind: Deployment
metadata:
  name: rbac-manager
...

$ kubectl get deployments.v1.apps rbac-manager -o yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: rbac-manager
...

$ kubectl get deployments.v1beta1.extensions rbac-manager-o yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: rbac-manager
... 
```

您会注意到，所有这些命令都返回相同的对象，但是将`apiVersion`设置为您在请求中请求的值。这使得无法判断实际部署到服务器上的是哪个版本，从而在升级时导致问题。当您有很多应用程序部署到您的 Kubernetes 集群时(就像我们的许多客户端一样)，集群升级到 1.16 版本可能会中断部署过程，可能会跨越数百个存储库。我们需要一种提前提供这些信息的方法，以便在升级发生之前解决部署过程。

## **Pluto 识别弃用的 Kubernetes 文件**

进入冥王星。您可以使用 Pluto 扫描各种来源的废弃 API 版本，包括平面清单文件、来自`helm template`命令的 stdout，或者甚至直接与您的集群交互(如果您使用 Helm 进行部署的话)。例如，下面是使用 Helm 3 (Pluto 也支持 Helm 2)部署应用程序的集群扫描:

```
 $ pluto detect-helm --helm-version 3
KIND          VERSION        DEPRECATED   RESOURCE NAME
StatefulSet   apps/v1beta1   true         audit-dashboard-prod-rabbitmq-ha
Deployment    apps/v1        false        goldilocks-controller
Deployment    apps/v1        false        goldilocks-dashboard 
```

Pluto 告诉你你的`audit-dashboard-prod-rabbitmq-ha StatefulSet`是通过 Helm 用一个废弃的 API 部署的。现在，在 1.16 上部署之前，可以更容易地在图表存储库中定位和修复这个问题。

在升级到 1.16 之前，您可以使用 Pluto 验证在集群中运行的 Helm 图表、YAML 清单和 Helm 部署，并更新任何不推荐使用的 API。一旦冥王星通过，你就可以迁移到 1.16 版了。

Pluto 现在是开源的，将被更新以包括 Kubernetes 的未来版本和对资源 API 的任何弃用。我们正在努力增加功能，所以请打开问题，加入我们的 [Slack 频道](https://fairwindscommunity.slack.com/messages/pluto)，并做出贡献！

寻找一个完整的 Kubernetes 治理平台？Fairwinds Insights 是免费的。 [今天就开始。](https://www.fairwinds.com/coming-soon)

## 资源

[![Just Posted: Kubernetes Benchmark Report 2023](img/ac3f87548e14243ac3414b06e023028d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/df6709b9-c7f5-4c1a-8bf4-315f56b16325)