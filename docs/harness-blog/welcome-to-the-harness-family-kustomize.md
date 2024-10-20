# 欢迎来到线束家族，Kustomize！装具

> 原文：<https://www.harness.io/blog/welcome-to-the-harness-family-kustomize>

在帮助实现 Kubernetes 内部的配置和包管理的竞赛中，Kustomize 和 Helm 是两个非常受欢迎的工具，分别在这些领域提供帮助。

在帮助实现 Kubernetes 内部的配置和包管理的竞赛中， [Kustomize](https://kustomize.io/) 和 [Helm](https://helm.sh/) 是两个非常受欢迎的工具，分别在这些领域提供帮助。我们最近欢迎了[头盔 V3](https://harness.io/blog/product-updates/welcome-to-the-harness-family-helm-v3/) 加入线束家族，我们也非常兴奋地欢迎 [Kustomize 加入线束家族](https://developer.harness.io/docs/continuous-delivery/onboard-cd/cd-quickstarts/kustomize-quickstart/)！

一个常见的比较是在赫尔姆和 Kustomize 之间。这两个项目有不同的目标；Kustomize 是为成为一名配置经理而设计的，而 Helm 并没有自称是一名配置经理。

### 为什么 Kustomize 产生喧嚣？

如果你在最新发布的 Helm aka [Helm V3](https://helm.sh/docs/) 之前沿着记忆之路走一趟，Helm 需要安装[舵柄](https://v2.helm.sh/docs/install/)和[舵柄客户端](https://v2.helm.sh/docs/using_helm/#installing-helm)。Kustomize 从早期就有能力被 [Kubernetes 的 CLI](https://kubernetes.io/blog/2018/05/29/introducing-kustomize-template-free-configuration-customization-for-kubernetes/) ， [KubeCTL](https://kubectl.docs.kubernetes.io/pages/app_management/introduction.html) 执行。作为一名配置经理，这一点很重要，因为想象一下一个新创建的 Kubernetes 集群启动并运行，开始利用 Kustomize 不需要在集群上安装额外的客户机。Harness 使在您的基础设施和部署中利用 Kustomize 变得轻而易举。

### 去 Kustomize 'ing

利用 Kustomize 文档是一个很好的起点。像往常一样，你可以观看视频和/或跟随博客。

#### Kubernetes 基础设施设置

在我的例子中，我将使用亚马逊 EKS，并在 EKS 集群中安装一个 Kubernetes 代理。

安装一个[Harness Kubernetes Delegate](https://developer.harness.io/docs/first-gen/continuous-delivery/kubernetes-deployments/connect-to-your-target-kubernetes-platform/)非常简单。登录[驾驭平台](https://app.harness.io/)，如果你还没有运行 Kubernetes 代理，进入设置- >驾驭代理。

在“下载代表”下，选择 Kubernetes YAML。

为线束代理提供一个名称，在这种情况下，我们可以将所有代理“kustomize”。

点击“提交”后，将会下载一个包含所有所需内容的 tar.gz。展开 tar.gz。

在展开的文件夹中，只需运行*ku bectl apply-f harness-delegate . YAML*即可启动并运行您的代理。如果您的 KubeCTL 没有连接到您的 EKS 集群，可以运行一个 helper 方法，用*AWS eks-region your-region update-kube config-name your-cluster-name 注入 [Kubeconfig](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) 。*

可以运行*ku bectl get pods-n Harness-Delegate*来获取线束代理的状态。

回到线束 UI，可以验证代理是否已连接。

一旦连接了委托，为了将 Git 位安装到 Harness Kubernetes CLI 中，我们可以创建一个[委托概要文件](https://docs.harness.io/article/nxhlbmbgkj-common-delegate-profile-scripts#git_cli)来安装 Git CLI。

设置->利用代理->管理代理配置文件+添加代理配置文件“安装 Git”

*apt-get 更新*
*#自动批准安装 Git*
*是| apt-get 安装 git*
*#检查 Git 安装*
*Git-version*

点击 submit 后，在委托中，点击 Profile -> Install Git

应该需要几分钟的时间来执行。您可以通过点击最后执行日期旁边的 View Logs 来查看概要文件执行日志，从而验证是否已经安装了 Git。

接下来，您可以将您的 Kubernetes 集群连接为一个[云提供商](https://developer.harness.io/docs/first-gen/firstgen-platform/account/manage-connectors/add-kubernetes-cluster-cloud-provider)。

设置->云提供商+添加云提供商

添加一个名为“EKS 集群”的云提供商，类型为 *Kubernetes 集群*。因为我们将 Harness 委托安装到了 EKS 集群中，所以我们可以从一个委托(在本例中是委托“kustomize ”)继承细节。

当您单击 Submit 时，Kubernetes 集群将可用于工作负载。

一旦集群连接起来，就该开始挖掘 Kustomize 部分了。

#### Kustomize 工作流程

Kustomize 的[线束文档](https://developer.harness.io/docs/continuous-delivery/cd-advanced/kustomize-howtos/use-kustomize-for-kubernetes-deployments/)是帮助您入门的绝佳资源。在这个例子中，你将利用 [Kustomize 的多基地例子](https://github.com/kubernetes-sigs/kustomize/tree/master/examples/multibases)展示环境变量的使用，例如开发/生产作为[覆盖](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/)。

接下来，您需要将 Kustomize GitHub 库连接到 Harness。

设置->连接器->源回购提供者+添加源回购提供者

添加*https://github.com/kubernetes-sigs/kustomize*的存储库细节，包括您的 GitHub 凭证，并指定 *master* 作为分支名称。

单击提交后，您的存储库将可用。

您现在可以创建一个[线束应用程序](https://docs.harness.io/article/bucothemly-application-configuration)来尝试 Kustomzie 集成。

设置-> +添加应用程序

创建一个名为“Hello World”的应用程序

单击 Submit 后，您就可以创建一个[线束服务](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/setup-services/add-service-level-config-variables/)。

设置-> Hello World ->服务+添加服务

创建一个名为“Multibases”的服务，部署类型为 *Kubernetes* 。

一旦你点击了提交，就该进行 Kustomization 配置了。在 Multibases 服务的 Manifests 部分，单击省略号->链接远程清单。

链接远程清单时，选择定制化配置。源存储库将是您之前链接的 GitHub 存储库。对于 kustomization 目录的路径，这将是*examples/multi base/$ { env . name }*。有了 *env.name* 的变量，我们可以指向一个特定的环境。

一旦你点击提交，清单应该已经链接。

当创建一个[线束环境](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/environments/environment-configuration/) t 时，我们可以模仿多基座示例结构。

您可以创建两个环境，“开发”和“生产”。

设置-> Hello World ->环境+添加环境

可以先加上“dev”。

单击提交后，您可以添加基础结构定义。

创建一个名为 *dev_eks* 的[基础设施定义](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/environments/infrastructure-definitions/)，作为云提供商类型 *Kubernetes* 集群，部署类型 *Kubernetes* ，并为您之前连接的 Kubernetes 集群选择云提供商。

单击提交后，您的基础结构定义就可用了。

对*产品*的线束环境重复该过程。

*产品*的基础设施定义。

现在您可以创建一个[利用工作流](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/workflows/workflow-configuration/)来运行您的 Kustomization 部署。

设置-> Hello World ->工作流+添加工作流

我们可以设置 Hello Kustomize 工作流。现在让工作流指向你的*开发*基础设施。

创建完成后，我们可以通过展开工作流变量部分并单击铅笔图标来添加一个[工作流变量](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/expressions/passing-variable-into-workflows/)。

创建一个名为 Environment 的工作流变量，并将 *dev，production* 添加到列表中，以模拟多基地示例和您的线束环境。

一旦你点击保存，现在你可以运行你的第一个例子。在工作流概述中，单击部署。

在 Start New Deployment 窗口中，选择 *dev* 作为环境值。

点击提交运行和繁荣！

您还可以弹出 [Kubernetes UI](https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/) 并查看正在创建/追加的[配置映射](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)。

祝贺你的第一次 Kustomize 部署！在 Harness，我们是您的合作伙伴。

### 马具——你在 Kubernetes 的伙伴

许多组织正在经历 Kubernetes 之旅。在 Harness，我们是 Kubernetes 内外工作负载的权威持续交付平台。我们的客户和社区都支持 Kustomize。随着 Kubernetes 生态系统的不断发展和成熟，Harness 将会出现。请随时带着工具[软件交付平台](https://harness.io/platform/)出去兜风，并立即注册！