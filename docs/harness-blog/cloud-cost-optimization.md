# 云计算成本优化入门|利用

> 原文：<https://www.harness.io/blog/cloud-cost-optimization>

如今，您可以利用云成本管理来实现无风险旋转，从而消除围绕云成本优化的挑战。从今天开始获得洞察力和节省！

随着[云成本管理](https://harness.io/platform/cloud-cost-management/)的推出，以及现在的云成本管理试用，每个人都有机会接触到云成本优化。如果您对如何开始感到好奇，即使您是 Harness 平台的新手，我们也能满足您的需求。

在这个例子中，我利用了 Amazon Elastic Kubernetes 服务，并可以访问 AWS 中的计费模块。让我们开始着手进行云成本管理。像往常一样，你可以关注博客和/或观看视频。

### 入门-注册线束

如果你还没有，注册一个[线束账户](https://harness.io/try-continuous-delivery-as-a-service-for-free/)。对于这个例子，我们将利用 Harness CD 平台的免费社区版。

一旦你[登录到](https://app.harness.io)你的账户，你会看到一个 Harness 仪表盘。

利用 Harness 要做的第一件事是安装一个 [Harness 委托](https://developer.harness.io/docs/platform/delegates/get-started-with-delegates/delegate-installation-overview/)，它是一个代表您执行操作的 worker 节点。

切换齿轮很快，我有一个等待[亚马逊 EKS 集群](https://aws.amazon.com/eks/)。所以谨慎的驾驭委托应该是 Kubernetes 委托。

回到 Harness 平台，安装委托很简单。

设置->线束代理->安装代理。选择 Kubernetes YAML，并给出一个像“保持低成本”的名称。

下载完 TAR 后，展开 TAR，然后进入展开的文件夹。

在 Delegate 文件夹中，有一个包含 KubeCTL 指令的自述文件。运转

*ku bectl apply-f Harness-Delegate . YAML*将安装线束代理。

几分钟后，可以在代理 UI 上验证线束代理已连接。

利用代表离开后，让我们准备好开始被云成本管理观察的 EKS 集群。

### Kubernetes 集群准备

云成本管理使用 [Kubernetes Metric Server](https://docs.aws.amazon.com/eks/latest/userguide/metrics-server.html) 作为数据点。根据您的 Kubernetes 提供者的不同，在您实例化一个集群时，它可能会也可能不会为您安装。就 EKS 而言，默认情况下不存在这种情况。

您可以通过运行"*kube CTL apply-f https://github . com/Kubernetes-sigs/metrics-Server/releases/download/v 0 . 3 . 6/components . YAML*"来安装 Kubernetes Metric Server。

可以通过运行“ *kubectl top nodes* ”来快速测试 Kubernetes 度量服务器是否正在运行。

准备好您的 Kubernetes/EKS 集群后，是时候启动您的云成本管理试验了。

### 激活您的云成本管理试用

激活[云成本管理试验](https://developer.harness.io/docs/first-gen/cloud-cost-management/new-to-ccm-get-started-with-a-trial/setup-ce-harness-editions/)非常简单。在左侧导航窗格中导航。点击“持续效率”，可以报名参加平台内部的云成本管理试用。

一旦你点击开始免费试用，可以开始在配置布线。如果您需要回到设置，可以导航回持续效率->设置。

随着 Kubernetes 代表运行布线在 EKS 集群是轻而易举的。从云成本管理设置菜单中选择 Kubernetes 集群。

分两步将 Kubernetes 集群添加到 Harness 平台。首先将 Kubernetes 集群命名为“Big Money Cluster”，单击 Test，然后单击 Next。

单击下一步后，在持续效率启用菜单上，选中启用持续效率。

单击 Submit，您就可以监控 Kubernetes 的使用情况了。

云成本管理需要 2-3 个小时来完成其初始评估。当云支出数据可用时，您将收到一封电子邮件。

如果存在现有工作负载，也将开始报告使用数据。不过，如果这是您第一次与 Kubernetes 集群交互，或者第一次浏览这个示例，您可以很容易地用 Harness CD 播种一些工作负载。

### 播种 Kubernetes 工作负载

如果您的 Kubernetes 集群像示例中我的集群一样是空的，那么利用 Harness CD 来[部署一个示例工作负载](https://github.com/harness-training-dept/KubernetesCDWorkshop)是一个很好的下一步。Harness 在一个 [CD 抽象模型](https://developer.harness.io/docs/first-gen/starthere-firstgen/harness-key-concepts/)上工作，该模型是 Harness 应用程序的基础。

通过转到以下页面添加[线束应用](https://developer.harness.io/docs/getting-started/learn-harness-key-concepts/):

设置->应用程序+添加应用程序

可以添加一个名为“不贵 App”的应用。

一旦您点击提交，就可以开始填写抽象模型的各个部分。

第一部分是建立一个[线束环境](https://developer.harness.io/docs/getting-started/learn-harness-key-concepts/)来部署。

设置->不贵的应用->环境+添加环境。创建一个名为“沙箱”的非生产环境。

我们可以让 Harness 知道，作为“沙箱”环境的一部分，我们刚刚用一个[基础设施定义](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/environments/infrastructure-definitions/)连接了 Kubernetes 集群。

点击+添加基础设施定义。可以将定义“K8s 集群”命名为 Kubernetes 集群类型，并将成为 Kubernetes 部署。

单击 Submit，现在是时候通过创建一个[线束服务](https://developer.harness.io/docs/getting-started/learn-harness-key-concepts/#services)来定义您实际部署的内容了。

设置->不贵的应用->服务+添加服务

一旦点击添加服务，可以添加一个名为“Nginx”的 Kubernetes 服务。

一旦在服务中，可以为 Nginx 连接一个工件源，这将是公共 Docker Hub 添加工件源。线束预先与公共 Docker 集线器连接，作为电源。

连接“library/nginx”作为工件源并点击 Submit。

一旦点击 Submit，您的图像就被连接到默认的脚手架上，这对于这个例子来说已经足够了。

在您部署之前，将需要创建一个 [Harness 工作流](https://developer.harness.io/docs/getting-started/learn-harness-key-concepts)来指导 Harness 如何处理您的应用程序。

设置->不贵的应用->工作流+添加工作流

添加一个名为“Deploy Nginx”的工作流作为滚动部署。选择之前创建的环境、服务和基础结构定义。

单击提交后，您就可以开始部署了。

单击部署。选择一个标签，如“最新”并点击提交。

几分钟后，您无畏的 Nginx 将被部署到您的 K8s 集群！

完成第一次部署后，您也可以坐下来等待最初的 2-3 小时播种期。

### 了解您的第一个集群成本明细

一旦您收到数据准备就绪的电子邮件，就该回到云成本管理了。

返回到 Harness 平台，然后在左侧导航中转至云成本管理。预算和浏览器链接将被激活。可以点击 Explorer 来查看您的第一组 Kubernetes 集群支出。

仔细看看“大钱集群”的效率，我们可以看到云成本管理如何分解 [Kubernetes 容器使用](https://harness.io/blog/kubernetes-containers-cost/)。

总的来说，肯定有一些方法可以调优这个集群。对于我们的示例工作负载和利用代表，我们过度配置，如果这是一个长期运行的集群，我们可以调整 Kubernetes 集群的大小并重新部署工作负载。对我自己来说，我为 AWS 上的团队维护了几个其他服务，我们也可以将云成本管理连接到 AWS 本身。

### 获得更多资金——云提供商布线

我的 Kubernetes 工作负载成本只是云成本管理所能提供的一部分。您可以获得更多关于将云提供商计费 API 接入云成本管理的见解。这将真正有助于云成本优化。

您的 AWS 帐户必须是[组织](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_manage_create.html)的一部分。可以在 AWS 组织控制台中进行验证。

查看[文档](https://developer.harness.io/docs/cloud-cost-management/onboard-with-cloud-cost-management/set-up-cloud-cost-management/set-up-cost-visibility-for-aws/)和/或导航到持续效率- >设置- >连接到您的 AWS 帐户，从 AWS 导出使用数据需要几个步骤。云成本管理向导将引导您前进。

第一步是在您的 AWS 帐户中创建一个成本和使用报告。可以启动 AWS 控制台或利用云成本管理中的按钮。

在 AWS 控制台中，单击创建报告。

可以给出当前报告的名称，例如“my-cur-report”。确保选择包括资源 id 和自动刷新。

点击下一步后，创建一个 S3 存储桶来存储当前报告。

可以给新桶起个名字。虽然 [S3 桶](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-s3-bucket-naming-requirements.html)在整个可用性区域必须是唯一的，但是我的桶名“bigmoney-cur-bucket”不会是唯一的，因为我正在利用。

点击“下一步”后，默认策略就可以了。回到交付选项菜单，将报告路径前缀改为“ce”，将时间粒度改为每小时，将报告版本改为覆盖现有报告，将 GZIP 改为压缩。

点击查看并完成。一旦设置完成，您的第一个 CUR 将在 24 小时内由 AWS 交付。

同时，可以将新的报告和存储区连接到云成本管理设置中。

下一步是授予云成本管理对 CUR 报告的访问权限，以分析其中的内容。这将通过身份和访问管理[IAM]角色来完成。

通过点击启动模板按钮，您将进入 AWS 云形成模板创建向导，例如快速创建堆栈。

默认值就可以了。确认并创建堆栈。

云形成堆栈将被创建。在堆栈的输出部分，复制 CrossAccountRoleARN 值。

ARN 值和您的帐户名称将是您需要连接到云成本管理 AWS 设置向导的最后两个连接。

完成后，您应该已经连接了 CUR 和 IAM 细节。

单击保存并继续，退出确认提示，现在一切就绪。

云成本管理需要 24 小时来分析您的云支出。你现在已经开始省钱了！

### 线束平台——提升您的体验

云成本管理是 Harness 平台中的一个支柱。云成本管理的起源是支持和解决我们自己的[财务运营](https://www.finops.org/what-is-finops/)挑战。正如我们在 [2020 年关于 SaaS 云支出的报告](https://harness.io/saas-cloud-spend-survey-2020/)中所报告的，控制云成本对于各种规模的组织来说都是一项挑战。借助云成本管理和新的试用能力，证据就在布丁中。请务必[注册](https://harness.io/platform/cloud-cost-management/)，立即开始节省云成本！

您想了解更多关于其他工具的信息吗？我们制定了要考虑的顶级[云成本管理工具](https://harness.io/blog/cloud-cost-management-tools/)。