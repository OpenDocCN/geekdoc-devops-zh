# 使用插件帮助改进您的 Kubectl 命令

> 原文：<https://www.fairwinds.com/blog/help-improve-your-kubectl-command-with-plugins>

您可以使用插件向 [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) Kubernetes 命令行工具添加特性。创建`kubectl`子命令来添加定制功能，同时保持与 Kubernetes 的紧密交互。请继续阅读，了解更多关于 kubectl 插件的信息，在哪里与社区共享插件，以及您可能想在自己的 kubectl 中添加哪些新的有用插件。

## 这些插件是什么？

可以使用 [kubectl 插件](https://kubernetes.io/docs/tasks/extend-kubectl/kubectl-plugins/)来扩展`kubectl`命令，这些插件是路径中以 *kubectl-* 开头的可执行文件(kubectl 后跟一个破折号)。您可以使用任何编程或脚本语言，并且不需要库或预加载。

插件文件的名称决定了 kubectl 命令。包含破折号(-)表示空格，下划线(_)表示破折号。例如，`kubectl hello world`子命令将在`kubectl-hello-world`文件中执行，而`kubectl hello world at-night`子命令将在`kubectl-hello-world-at_night`文件中执行。

Kubectl 使用可用时间最长的插件，并将剩余的命令行参数传递给插件。例如，如果您运行命令`kubectl gke create regional`，kubectl 将首先查找可执行文件`kubectl-gke-create-regional`，然后是`kubectl-gke-create`，以此类推。如果第一个可用的插件是`kubectl-gke`，参数`create regional`将被传递给插件。

您可以通过创建一个更具体的插件来将命令添加到现有的插件中。使用上面的`kube-gke`示例，您可以通过创建`kubectl-gke-create-zonal`来添加`kubectl gke create zonal`命令，而不是向`kubectl-gke`插件添加功能。

您不能使用插件来覆盖或添加子命令到内置的 kubectl 命令中。您不能使用`kubectl create catastrophe`插件，因为`create`已经是一个 kubectl 命令。

在您的路径中找到的第一个插件将由 kubectl 使用。命令显示所有可用的插件，包括路径中重复插件的警告，或者不可执行的插件。

目前 [kubectl shell 自动补全](https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion)不包括选项卡补全建议中的插件，正如[在这个特性请求](https://github.com/kubernetes/kubernetes/issues/74178)中提到的。

## 与他人共享插件

共享 kubectl 插件的两种常见方式是 kubectl-plugin Github 主题和 Krew kubectl 插件管理器。

### Kubectl-plugin Github 主题

你可以用主题对 Github 库进行分类，以帮助其他人发现它。 [kubectl-plugins Github 主题](https://github.com/topics/kubectl-plugins)包含 48 个库，其中一些包含多个 kubectl 插件。每个插件作者都有自己的安装和升级过程，从编译自己的二进制文件到下载一个 shell 脚本。

将你的插件 Github 库添加到这个主题是一个好主意，即使你使用 Krew(如下所述)来共享你的插件。

### Krew 插件管理器

[Krew](https://github.com/GoogleContainerTools/krew) 是一个针对 kubectl 插件的插件管理器，旨在简化多个操作系统上的插件发现、安装、升级和移除。您可以通过运行`kubectl krew search ...`来搜索可用的插件，或者通过运行`kubectl krew upgrade`来升级所有新安装的插件。

目前不可能将一个插件固定到你的首选版本来防止它被升级。您可以通过从先前提交到 [krew-index](https://github.com/GoogleContainerTools/krew-index) 存储库中获取清单文件，并对`kubectl krew install`命令使用`--manifest=FileName.yaml`选项来安装旧版本的插件。

Krew 已被 Kubernetes SIG-CLI 接受，中央索引可能会演变为类似于 [Helm Hub](https://hub.helm.sh/) 的目录索引，正如在本[提案中所提到的，将 Krew 作为 SIG CLI 子项目](https://groups.google.com/forum/#!topic/kubernetes-sig-cli/9fWm_aZ-AC8)。

#### 血液样本呢

安装`git`，然后遵循 [Krew](https://github.com/GoogleContainerTools/krew) Github 页面上的安装说明。下面是一个使用 Krew 搜索安装 kubectl 插件的例子:

我将使用`krew info`命令来读取关于资源容量插件的更多信息，然后使用`krew install`命令来安装它。

这非常简单——我不需要下载、解压存档，或者验证任何校验和。我碰巧安装在 Linux 上，但这个插件也适用于 Mac OS X。现在我可以在我相对较空且无聊的集群上使用资源容量插件:

#### 向 Krew 贡献插件

[Krew 命名指南](https://github.com/GoogleContainerTools/krew/blob/master/docs/NAMING_GUIDE.md)给出了很好的建议，让插件更容易被`krew search`命令发现，并避免宽泛或过载的术语。例如，命名一个插件`kubectl-gke-login`或`kubectl-gke-ui`，而不是更一般的名字`kubectl-login`或`kubectl-ui`。

Krew 插件开发者指南描述了如何创建一个插件*。与 Krew 一起使用的 zip* 或 *.tar.gz* 文件，以及 *plugin.yaml* 清单文件，其中包含您所支持的操作系统的信息、校验和以及到您的插件的下载链接。最后一步是创建一个 [krew-index](https://github.com/GoogleContainerTools/krew-index) pull 请求，将您的 *plugin.yaml* manifest 文件添加到 krew 索引中——一旦您的 pull 请求被接受，任何使用 Krew 的人都可以使用您的插件。

接受新插件到 [krew-index](https://github.com/GoogleContainerTools/krew-index) repo 有一些挑战，如这个 [tiller-init pull 请求](https://github.com/GoogleContainerTools/krew-index/pull/87)所示。前面提到的 Krew 对 SIG-CLI 的捐赠包括改进插件索引过程的计划。

## 让插件共享开始

是时候分享你的插件了，不管你是使用 Git 库， [Krew](https://github.com/GoogleContainerTools/krew) ，还是在这样的博客中。说到分享。。。

### 发现有问题的 Pod 中断预算

显示允许少于一次中断的 [pod 中断预算](https://kubernetes.io/docs/tasks/run-application/configure-pdb/)-这些可以防止 Kubernetes 节点在维护期间被[耗尽](https://kubernetes.io/docs/tasks/administer-cluster/safely-drain-node/)。这个插件需要`jq`命令。

### 列出带有池和版本的 Google 容器引擎节点

当 Google Container Engine (GKE)集群正在升级时，用节点池的名称和 Kubernetes Kubelet 的版本列出节点会很有用。

## 感谢您的阅读

感谢您的阅读！如果你喜欢这篇文章，或者对上面的 kubectl 插件有建议，请联系 Twitter 上的 [@IvanFetch](http://www.twitter.com/ivanfetch) 。