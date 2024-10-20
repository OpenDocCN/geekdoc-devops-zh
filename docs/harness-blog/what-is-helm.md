# 头盔介绍:图表，部署和更多|装具

> 原文：<https://www.harness.io/blog/what-is-helm>

Helm 被设计成 Kubernetes 的第一个包经理。学习所有你需要知道的关于头盔的知识。

可能在您的 Kubernetes 集群启动并运行后安装的第一批包之一就是 [Helm](https://helm.sh/) 。Helm 是 Kubernetes 生态系统的坚定支持者，也是 Kubernetes 的软件包经理。如果您不熟悉 Helm，Helm 通过打包 Kubernetes 部署所需的所有资源，帮助用户获得更一致的部署。如果您习惯于编写软件包管理器指令，Helm 将遵循这些惯例中的大部分。

虽然像 Kubernetes 中的任何东西一样，发布/运营和开发任务正在合并，如果来自应用程序开发背景，Helm 将属于等式的发布侧。即使您没有编写 Helm 指令，Helm 也是许多软件供应商和项目在您的 Kubernetes 集群上安装资源的流行安装方法。随着 Kubernetes 的复杂性开始增加，Helm 在 2015 年第一次 [KubeCon](https://twitter.com/KubeCon_) 时被引入。

## 赫尔姆简史

2015 年对于 Kubernetes 来说是具有里程碑意义的一年。在加入社区一年多后，Kubernetes 终于发布了第一个正式版本，2015 年夏天发布了 1.0。由于 Kubernetes 被设计成可插拔的和“平台的平台”，因此需要开始编排多个 Kubernetes 配置文件。Helm 最初是由 Deis 开发的，Deis 是一家致力于帮助 Kubernetes 生态系统的公司，后来被微软收购。有趣的是，Deis 也是《Kubernetes 儿童指南》的原始赞助商之一。

Helm 从一开始就被设计成一个软件包管理器。Helm 目前正在进行第三次重现，Helm V3。它的第二个版本《圣盔 V2》获得了很多人的认可，该版本将于 2020 年夏天结束。随着项目的继续发展，Helm 现在当然有了更多的竞争和选择，但是该项目继续忠实于它的包管理根源和目标。

### 什么是头盔？

包管理并不是一个新概念。如果你以前用过 Linux 机器，软件包管理器，比如 [YUM](https://en.wikipedia.org/wiki/Yum_(software)) / [RPM](https://en.wikipedia.org/wiki/RPM_Package_Manager) 和 [APT](https://en.wikipedia.org/wiki/APT_(software)) ，对于软件包的安装和卸载至关重要。如果你不喜欢 Linux，在你的 Mac 上使用一个包管理器，比如 [Homebrew](https://brew.sh/) 或者 [Chocolatey](https://chocolatey.org/) for Windows，可以让安装软件，尤其是开发软件变得更加容易。

随着 Kubernetes 平台和生态系统的不断扩展，部署一个且只有一个 Kubernetes 配置文件(即:单个 YAML)不再是规范。在 Kubernetes 中，可以部署多个集群，编排多个资源。随着 YAML 数量的增加，在哪里存储这些 YAML 成为一个问题。进入 Helm 解决这些问题。

### 谁使用 Helm，为什么？

如果需要协调不止一个 Kubernetes 资源，并且有多个配置不同的集群，那么就有充分的理由利用 Helm。软件供应商和开源项目都可以通过提供 Helm 资源而受益，例如 Helm 存储库和图表，作为消费者将应用程序安装到 Kubernetes 集群中的一种方式。

Helm 使用一种叫做图表的打包格式。一个[掌舵图](https://helm.sh/docs/topics/charts/)是描述一组 Kubernetes 资源的文件集合。像其他基于惯例的包管理器格式一样，舵图遵循一个目录结构/树。舵图可以存档并发送到[舵图储存库](https://helm.sh/docs/topics/chart_repository/)。

在当前版本中，Helm 是一个安装在 Kubernetes 集群之外的客户端。它利用 kubectl 与 Kubernetes 集群进行连接和交互。Helm 将使用 kubectl 提供的连接信息。安装 Helm 客户端有几种方法，具体取决于您的操作系统。在装有家酿软件的 Mac 上，您只需运行 *brew install helm。安装头盔后，是时候享受它的好处了。*

## 使用 Helm 有什么好处？

使用 Helm 的最大好处与 CI 和 CD 目标非常相似:可重复性和一致性。有了 Kubernetes，安装的复杂性并没有消失；古老的计算机科学格言是正确的，你只是像算盘一样移动复杂性。将安装和卸载步骤视为一个软件是有好处的。

对于 Helm Charts 的消费者来说，这可能是在 Kubernetes 集群上使用和部署软件的一个快速通道。想开始尝试像普罗米修斯这样的新技术吗？你可以[手写](https://sysdig.com/blog/kubernetes-monitoring-prometheus/)安装步骤或者只是为普罗米修斯添加[头盔图表库，然后运行*头盔安装稳定/普罗米修斯*。](https://github.com/prometheus-community/helm-charts)

Helm packaging 不仅仅局限于软件供应商或开源项目。所有组织都安装和部署软件。随着对 Kubernetes 投资的继续，使用包管理器减少了编排包管理器中定义的步骤的重复和复杂性。为了在您的内部/定制应用程序中利用 Helm，需要创建一个 Helm 图表，并关联结构和值。

## 什么是舵轮图？

图表是 Helm 使用的主要格式。由于舵图是基于文件的，并遵循基于约定的目录结构，因此图表可以很容易地存储在图表存储库中。在 Kubernetes 集群中安装和卸载图表。像图像到容器的关系一样，图表的运行实例被称为发布。最后，Helm Chart 是一个将图表定义转换为 Kubernetes 清单的执行模板。

舵图在创建时必须有一个遵循[语义版本](https://semver.org/spec/v2.0.0.html)的版本号。Helm 图表可以引用其他图表作为依赖项，是任何软件包管理器的核心。Helm Charts 更高级的特性是[图表挂钩](https://helm.sh/docs/topics/charts_hooks/)和[图表测试](https://helm.sh/docs/topics/chart_tests/)，它们允许与一个版本的生命周期进行交互，并且能够分别针对一个图表运行命令/测试。关于舵图表的伟大之处在于，如果你正在学习，你可以探索图表的内容，因为它们是基于文件的。如果你正在创作你的第一张舵轮图，有四个主要部分需要关注。

### 分解舵图

[项目的入门指南](https://helm.sh/docs/chart_template_guide/getting_started/)是理解图表内容的绝佳资源。在 Github 上查看 Bitnami 的 MySQL 图表会让你对一个广泛使用的图表的结构和内容有一个很好的了解。

执行舵图所需的四个主要组成部分如下:

*   图表. yaml
*   A Values.yaml
*   其他图表的图表目录
*   模板(目录)

您的 [Chart.yaml](https://github.com/bitnami/charts/blob/master/bitnami/mysql/Chart.yaml) 需要描述图表内容的信息，包括名称和版本。Chart.yaml 中可以包含更多的信息，但这是最起码的。

在 [Values.yaml](https://github.com/bitnami/charts/blob/master/bitnami/mysql/values.yaml) 文件中，您可以定义由模板注入/解释的值。想象这是一个属性文件。结合模板，这些值将用于创建 Kubernetes 清单。

[模板](https://github.com/bitnami/charts/blob/master/bitnami/mysql/templates/serviceaccount.yaml)是使用头盔不可或缺的。模板目录是现有 Kubernetes 清单可以利用 Values.yaml 中的值的地方，如果需要生成资源，可以用模板生成附加的 yaml。如果模板中需要包含条件逻辑，Helm 会利用 Go 模板。如果你没有利用某种模板，Helm 可能不适合你这个制作人。

有了这些东西，Helm 就可以在你的图表上执行了。如果您在当前的 Helm V3 之前使用 Helm，Tiller 将是与您的 Kubernetes 集群进行交互所需的一部分。

## 什么是蒂勒？

如果你熟悉舵 V2，[舵杆](https://v2.helm.sh/docs/glossary/#tiller)是舵平台的一部分。它来自与谷歌部署管理器的项目集成的一部分，被设计成一个作业运行器，与 [Cloud Foundry 的 Bosh](https://www.bosh.io/docs/) 没有太大不同。简而言之，Tiller 是 Helm 的集群部分，它运行 Helm 的命令和图表。由于 Tiller 可以不受限制地访问 Kubernetes 集群，这成为了集群安全的一个痛处，Helm 的替代品也随之兴起。

## 头盔的替代品

包管理——以及现在的配置管理——是采用 Kubernetes 工作负载的组织快速需要的重要操作功能。赫尔姆当然面临着竞争，最普遍的竞争是随着 [Kustomize](https://harness.io/blog/welcome-to-the-harness-family-kustomize/) 的崛起，尽管有几个替代赫尔姆的选择取决于你的方法。每种选择都是在 Kubernetes 内部进行模板化的另一种方法。

### 草泽

根据这两个项目，Helm 和 [Kustomize](https://harness.io/blog/helm-vs-kustomize/) 是两种不同的方法。Kustomize 侧重于配置管理，而 Helm 侧重于包管理。Kustomize 广受欢迎的地方是，与赫尔姆·V2 相比，它不使用舵柄，而且从第一天起就利用了原生的 kubectl 命令。

### JSON nt/kson nt

Jsonnet 是一种模板语言和引擎。Jsonnett 有一个面向对象的模板方法，允许创建复杂的基于关系的模板。如果你需要复制某样东西，只需创建一个新的对象，支持模板就会接管。 [Ksonnnet](https://github.com/ksonnet/ksonnet) 是专门为 Kubernetes 设计的 Jsonnet 的一个分支，由 Heptio 创建。虽然，最近，Ksonnet 背后的团队不再支持这个项目。

### 斯卡福德

[Skaffold](https://skaffold.dev/) 是谷歌最新的 Kubernetes 管理项目之一。Skaffold 比 Jsonnet、Helm 甚至 Kustomize 更具包容性。因为 Skaffold 已经构建和部署了组件，所以它被设计成 Kubernetes 应用程序所需的一个配方。Skaffold 在部署阶段可以通过 [Helm](https://skaffold.dev/docs/pipeline-stages/deployers/helm/) 和 [Kustomize](https://skaffold.dev/docs/pipeline-stages/deployers/kustomize/) 进行插拔。

无论你为你的 Kubernetes 目标选择什么，Harness 已经覆盖了你。利用头盔很容易。

## 启动你的第一个头盔部署与线束

对于 Harness，开始部署第一个 Helm V3 非常简单。一个简单的方法是利用 Kubernetes 委托。在 [Kubernetes 快速入门](https://docs.harness.io/article/7in9z2boh6-kubernetes-quickstart)中描述了如何安装 Kubernetes 代理和连接 Kubernetes 集群。

按照下面的步骤为一个流行的发行版 Nginx 部署一个导航图。

### 添加舵库

第一步是增加一个[头盔库](https://docs.harness.io/article/0hrzb1zkog-add-helm-repository-servers)来驾驭。

要添加，请转到设置->连接器->工件服务器+添加工件服务器。

类型:Helm 仓库
显示名称:Bitnami Helm Repo
仓库 URL:https://charts.bitnami.com/bitnami

点击 Test 以验证位置，然后提交以添加存储库。

### 创建线束应用程序

Harness 运行 CD 抽象模型。线束应用程序是抽象模型的中心。

要创建新的线束应用程序，请设置->应用程序+添加应用程序。

创建一个名为“手提袋中的头盔”的新应用程序

一旦你点击了提交，你的画布就可以开始构建了。

### 创建线束环境

线束环境定义了您将要部署的位置。

要添加线束环境，请设置->手提袋中的头盔->环境+添加环境。

名称:测试环境
环境类型:非生产

单击 Submit 后，下一步是添加一个基础设施定义，定义目标基础设施，在本例中，它只是一个 Kubernetes 集群。

设置->手提袋中的舵->环境->测试环境+添加基础设施定义

名称:Helm Cluster
云提供商类型:Kubernetes Cluster
部署类型:Kubernetes
云提供商:<your _ Kubernetes _ Cluster>

单击 Submit 后，基础结构定义就完成了。

### 创建线束服务

您将要部署的是一个线束服务。在这里您可以链接到您想要部署的舵图。

要添加安全带服务，设置->手提袋中的头盔->服务+添加服务

名称:Bitnami_Nginx
部署类型:Kubernetes

点击提交后，在服务的右边，点击三个省略号来链接远程清单。

添加舵图作为远程清单。

清单格式:来自舵库的舵图
舵库:Bitnami 舵库
图名:bitnami/nginx
图版本:8.2.0【截止本帖最新】
舵版本:v3

点击提交，清单就会出现。

如果您不知道使用哪个版本的图表，您可以利用您机器上的 [Helm CLI](https://helm.sh/docs/intro/using_helm/) ，添加 Bitnami 存储库并搜索图表。

*头盔休息添加 bitnami https://charts . bitnami . com/bitnami
头盔搜索休息 bitnami/engine IX*

### 创建线束工作流

最后一步是定义如何使用[线束工作流](https://docs.harness.io/article/m220i1tnia-workflow-configuration)进行部署。

要创建一个新的工作流程，设置->手提篮中的头盔->工作流程+添加工作流程。

名称:部署 Nginx
工作流类型:滚动部署
环境:测试环境
服务:Bitnami_Nginx
基础架构定义:Helm 集群

点击提交后，工作流就完成了，该部署了。

### 展开你的舵图

有几种方法可以在 Harness 平台中部署。您可以返回到工作流，并单击右上角的 Deploy。另一种方法是通过左侧导航中的持续部署菜单。

连续部署->部署->开始新的部署。

直接执行工作流:选中
工作流:部署 Nginx

点击提交，你就上路了！

利用 Harness 平台，可以对 Kubernetes 集群进行大量的修改。

## 通过头盔和安全带持续交付

无论你的技术选择或 Kubernetes 拓扑结构，利用和掌舵一起更好。没有交付平台的 Helm 的局限性之一是 Helm 利用了您的 kubectl 上下文，因此集群之间存在管理切换。在 Harness 中，您甚至可以跨云提供商对您的分布式拓扑进行建模，以获得无缝体验。

关于赫尔姆的进一步阅读，请阅读[赫尔姆对库斯托米泽](https://harness.io/blog/helm-vs-kustomize/)。

干杯！

拉维