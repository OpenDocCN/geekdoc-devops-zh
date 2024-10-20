# GoNoGo 在升级前检查 Kubernetes 附加组件

> 原文：<https://www.fairwinds.com/blog/gonogo-checks-kubernetes-add-ons>

 在 GKE 或 AKS 这样的服务上部署第一个 Kubernetes 集群非常简单，但是当你想实际使用 Kubernetes 时，附加组件就变得很重要。

附加组件扩展了 Kubernetes 的功能。一些例子包括[度量服务器](https://github.com/kubernetes-sigs/metrics-server)、[证书管理器](https://cert-manager.io/)、[集群自动缩放器](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler)。与任何软件一样，附加组件的挑战在于它们需要升级。

每次需要进行附加升级时，都需要进行大量检查，以确保最新版本与您的集群兼容，并且没有重大更改。这个过程非常耗时，尤其是当您管理多个集群时。

这就是 Fairwinds 发布其最新开源工具 [GoNoGo](https://github.com/FairwindsOps/gonogo/tree/main) 的原因，该规范可用于定义然后发现与 Helm 一起安装的附加组件是否可以安全升级。

## GoNoGo 如何工作

监控附加升级是管理集群的一部分。在本例中，我们将讨论证书管理器的升级。SRE 将意识到即将到来的或新的证书管理器升级。SRE 需要通过查看文档和发行说明来确定是否需要升级。

GoNoGo 依靠用户用 Helm 管理和升级附加组件。一旦您决定要进行升级，您将在一个名为 bundle spec 的 YAML 文件中绘制一些字段。这些字段将用于设置参数，这些参数将根据集群和舵图信息进行评估，以确定该插件的升级可信度。

例如:

*   您可能有一个只能在 Kubernetes 及以上版本上运行的版本

*   您可能希望检查集群中的某些 API 是否可用

*   您可能希望检查您的清单是否指定了适用于您要升级到的版本的 API 版本

所有这些信息将被捆绑在一个 YAML 文件中，并输入到 GoNoGo。

然后运行 GoNoGo，并带有一个指向这个包文件的标志。GoNoGo 会将您的捆绑包规范中的附加组件列表与您的集群中部署的 Helm 版本进行比较，并针对已经成功部署到您的集群的附加组件运行您指定的检查。然后，GoNoGo 将使用图表清单、上游 repo 和集群清单的组合来运行您的包规范中的各种检查。

## 捆绑包中的 OPA 和 JSON

在您的包中，您还可以设置定制的 OPA 检查，在清单中查找注释和字段之类的东西。GoNoGo 将运行 OPA 检查舵图的单个 YAML 文件，以及您在“resources”键下的包中指定的任何对象。

它还运行 json 模式验证。GoNoGo 可以根据存储在 repo 中的上游 values.schema.json 文件进行验证，或者您可以提供自己的内嵌模式。例如，您的架构可以指定它将检查 imagePullPolicy 是否设置为 Always。

## 为什么要用 GoNoGo？

虽然您仍然需要监视附加升级、查看文档并签入规范中的更改，但 GoNoGo 可以帮助您节省手动查看集群中所有对象的时间。

例如，如果 cert-manager 不同意一个注释，您将需要查看将要从中提取的上游舵图，并检查舵图是否已经更新了它的清单。你还需要进入你的集群，做一个导航`get manifest`，逐个查看你所有的清单，确保它们也有正确的注释。GoNoGo 为你做这个。因为 GoNoGo 会自动完成这项工作，所以在升级过程中丢失文件/人为错误的风险更小。

附加升级的最大风险是，因为它们是在集群范围内使用的，如果出现问题，它们可能会对您的集群产生深远的影响。影响范围将取决于附加组件。对于 Metric Server，可能没什么大不了的。但是，如果由于没有正确指定入口类名而错过了 nginx 入口插件，它可能无法传递流量，从而导致不必要的停机。类似地，如果您有一个损坏的 cert-manager 安装，那么当您试图解决这个问题时，您将无法为工作负载生成新的 cert。因此，虽然群集可能没有关闭，但它可能会影响服务。

如果您正在升级附加组件，请查看 [GoNoGo](https://github.com/FairwindsOps/gonogo/tree/main) 。

### 资源