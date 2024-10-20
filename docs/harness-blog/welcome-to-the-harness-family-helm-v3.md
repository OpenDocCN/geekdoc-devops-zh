# 线束连续交付支持 Helm V3 |线束

> 原文：<https://www.harness.io/blog/welcome-to-the-harness-family-helm-v3>

欢迎来到马具家族，圣盔 V3！你的无舵方式在这艘船上很受欢迎。在马具社区的推动下，我们回答。

自从几年前在首届 KubeCon 上被介绍以来， [Kubernetes](https://kubernetes.io/) 、 [Helm](https://helm.sh/) 的实际包装经理已经经历了一次演变。赫尔姆的最新版本是去年底发布的[赫尔姆 V3](https://helm.sh/blog/helm-3-released/) 。

过去反对赫尔姆的一个论点是一个名为[蒂勒](https://v2.helm.sh/docs/using_helm/#installing-helm)的高架[库本内特斯吊舱](https://kubernetes.io/docs/concepts/workloads/pods/)用于行刑。尽管 Helm 项目确实考虑到了所有的问题，并重新设计了 Helm V3，不再依赖于 Tiller。

Harness 最近在我们的平台中发布了对 Helm V3 的支持。让我们看看如何利用您的第一个 Helm V3 部署。不过，如果你对赫尔姆不熟悉，我们可以沿着记忆之路走一走。

## 舵更新器

这里有很多与头盔相关的资源可以利用。我们已经完全打破了[掌舵](https://harness.io/blog/what-is-helm/)并主持了[最近的网络研讨会](https://harness-1.wistia.com/medias/ext8gfgpje)让你变得危险。虽然作为一个[*TL；博士*](https://en.wikipedia.org/wiki/Wikipedia:Too_long;_didn%27t_read) Helm 是 Kubernetes 类似于[家酿](https://brew.sh/)、 [RPM](https://en.wikipedia.org/wiki/RPM_Package_Manager) 、[巧克力](https://chocolatey.org/)等的包管理器。Helm 使用[图表](https://helm.sh/docs/topics/charts/)和[模板](https://helm.sh/docs/topics/chart_best_practices/templates/)来处理约定概念。

在之前的博客系列中，我们利用了一个[亚马逊 EKS 集群](https://aws.amazon.com/eks/)作为我们的 Kubernetes 端点，并演示了如何使用 [EKSCTL](https://eksctl.io/) 来设置一个集群，并在您的机器上安装 [KubeCTL](https://kubernetes.io/docs/tasks/tools/install-kubectl/) 。我们将再次利用 EKS。虽然您可以将这个例子移植到您喜欢的 Kubernetes 端点。Helm V3 简化了所需的步骤。

## 您的第一个 Helm V3 部署

利用 Helm V3 简化了操作步骤，因为我们不再需要部署 Tiller。

喜欢总是可以跟进博客和/或观看视频。

### Kubernetes 基础设施

我有一个名为“手提袋中的头盔”的空 EKS 星团

你需要安装一个[线束代理](https://developer.harness.io/docs/getting-started/learn-harness-key-concepts/)。在这种情况下，我们可以在 EKS 的 Kubernetes 集群中安装一个 Harness 委托。这是简单的可以下载所需的 Kubernetes YAML 和申请。

导航到线束平台，然后设置->线束代理->下载代理-> Kubernetes YAML。

出现提示时，给代理一个名称“helm ”,然后单击提交。

单击提交后，将会下载一个 TAR.GZ。打开焦油包装。

在解压后的文件夹中，安装 *harness-delegate.yaml* 和*ku bectl apply-f harness-delegate . YAML*

几分钟后，Kubernetes 代表的名字将变为“helm-*”

一旦线束代理启动，您可以利用代理概要文件[在代理上安装 Helm V3 客户端](https://helm.sh/docs/intro/install/)。在代表页面上，单击管理代表个人资料+添加代表个人资料。

使用 Helm 项目本身的脚本创建一个显示名称为“Helm V3 Install”的委托概要文件。

*#安装舵*

*curl-fsSL-o get _ helm。sh https://raw。githubusercontent。com/helm/helm/master/scripts/get-helm-3*

*chmod 700 get_helm.sh*

*./get _ helm。sh* 

单击“提交”后，您可以在代理人上运行代理人配置文件。导航到配置文件并选择“Helm V3 安装”。

一旦执行了配置文件，现在就可以在线束代理上运行 Helm 了。

接下来，您需要将 EKS 集群作为一个 [Kubernetes 环境](https://developer.harness.io/docs/getting-started/learn-harness-key-concepts/)进行连接。这可以通过进入设置- >云提供商+添加云提供商
来实现

可以通过添加类型 *Kubernetes Cluster* 和名称 *kubernetes_cluster* 来填写关于您的 Kubernetes Cluster 的详细信息。

单击“提交”后，就可以将群集部署到其中了。

### 安全带和头盔工作流程

随着 Kubernetes 项目的方式，时间获得更多的驾驭组合。

进入设置- >应用程序+添加应用程序，创建一个新的[线束应用程序](https://docs.harness.io/article/bucothemly-application-configuration)。我们可以称这个应用程序为“手提袋中的头盔”。

创建完成后，让我们连接我们在博客系列的第一部分【https://charts.bitnami.com/bitnami】中使用的[头盔库](https://developer.harness.io/docs/first-gen/firstgen-platform/account/manage-connectors/configuring-artifact-server/)。要添加，请转到设置- >连接器- >工件服务器+添加工件服务器。我们将添加显示名称为 *Bitnami Helm Repo* 的 *Helm Repository* 类型。

单击提交后，Helm 存储库将可用。

现在，您可以通过导航回应用程序来添加一个[服务](https://developer.harness.io/docs/category/add-services)。设置- >手提袋中的头盔- >服务+添加名为“Bitnami_Nginx”的服务。这将是*头盔*的部署类型，我们将启用头盔 V3。

点击提交后，现在可以通过向下滚动到 UI 中间并单击“添加图表规范”来连接图表规范。

让我们利用原始博客系列的第一部分中的类似细节。图表名称将为“bitnami/nginx”，图表版本将为“5.1.4”。

随着时间的推移，如果您想要最新版本的图表，您可以利用您的本地机器(如果您安装了 Helm)来查找最新版本，方法是通过运行*Helm repo add Bitnami https://charts.bitnami.com/bitnami*添加 Bitnami Helm 库，并通过运行*Helm search repo Bitnami/nginx*进行搜索。

点击提交，您的图表规格将会出现。

现在是时候给你的舵轮图一个家了。进入设置- >手提袋中的头盔- >环境+添加环境，设置[驾驭环境](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/environments/environment-configuration/)。在这种情况下，添加 *Dev* 作为*非生产*环境。

单击 Submit 后，使用+Add Infrastructure Definition 在 UI 中间添加一个基础结构定义。命名为“Helm_Hand_Basket ”,云提供商为 *Kubernetes* 集群，部署类型为 *Helm* 。选择您作为云提供商接入的 Kubernetes 集群。

单击“提交”后，基础架构定义就会出现。

接下来，您将通过进入设置- >手提篮中的头盔- >工作流+添加工作流来创建[线束工作流](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/workflows/workflow-configuration/)。添加一个名为“nginx_helm”的工作流。

点击提交后，您可以在部署阶段下添加一个阶段

将服务 *Bitnami_Nginx* 添加到工作流阶段。

单击提交后，通过添加发布名称来完成 Helm 部署步骤。

称这次发布为“我的头盔发布”。

接下来，您可以通过转到 Setup->Helm in a hand basket->Pipelines+Add Pipeline 创建一个[线束管道](https://developer.harness.io/docs/platform/templates/create-pipeline-template/)来将工作流包装在管道中。添加一个名为“Helm_Pipeline”的管道。

单击提交后，添加一个管道阶段。

添加名为“Go for Gold”的执行步骤。

一旦你点击了提交，现在是时候运行你所有的努力工作，并开始一个线束部署。现在是时候开始你的[挽具展开](https://developer.harness.io/docs/first-gen/continuous-delivery/helm-deployment/helm-workflows/)了。可以导航到持续部署- >开始新的部署。

一旦你点击提交，看看奇迹发生！

可以登录到您的 Kubernetes UI，在默认的[名称空间](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)中查看您的发布。

完美和欢迎掌舵 V3！

## 带着安全带向前

Harness 平台简化了对您当前和未来管道至关重要的技术的采用。在 Harness，我们一直在集成新的平台。如果没有我们支持的平台，我们仍然通过几种方法使集成变得容易，以便与线束管道集成。如果您还没有机会检查线束[软件交付平台](https://harness.io/platform/)，请随意尝试一下。用头盔做些很酷的事？我们很乐意在 [Harness 社区](https://community.harness.io/)上听到您的意见。