# 2023 年你需要的四大 Kubernetes 开源工具

> 原文：<https://www.fairwinds.com/blog/top-4-kubernetes-open-source-tools-you-need-in-2023>

 认为 2022 年即将过去，而我们中的许多人正在为 2023 年的下一步做准备，这是很疯狂的。新年快到了，是时候考虑一下 2023 年你需要哪些 Kubernetes 开源工具了。

Gartner 预测，到 2027 年，超过 50%的企业将使用行业云平台来加速其业务计划。随着我们迈向 2023 年，我们将通过采用云原生技术、容器和 Kubernetes 来体验这种加速。对于那些采用 Kubernetes(或加速其使用)的团队，有四个开源工具可以帮助您创建和管理治理策略，使您的集群更稳定、更安全、更高效。

## 北极星:开源策略引擎

[北极星](https://www.fairwinds.com/polaris) 是 Kubernetes 的一个开源策略引擎，安装简单，可以作为仪表板、命令行界面(CLI)工具或准入控制器运行。北极星可以给你显示一个分数，这个分数与通过和未通过检查的数量有关。您可以快速查看错误、可能对集群健康有危险的事情、警告和通过检查。Polaris 会根据 Kubernetes 的最佳实践给你一个总分和相应的等级。

![Cluster overview in Polaris showing checks and cluster health](img/ff71b7a7d28f4038dec6475afad12757.png)

你可以查看不同的支票。例如，Polaris 表明您应该为您的部署制定一个 pod 中断预算。如果您不确定这是什么意思，您可以单击问号并转到文档页面获取更多信息。当您点击时，您可以看到缺少分销预算描述的窗格。

我们在 Polaris 中内置了许多这样的检查，记录在可靠性、效率和安全性下。这些检查都是 Kubernetes 的最佳实践，Fairwinds 团队已经学习、遵循了这些实践，现在向我们所有的客户推荐这些实践。您还可以向 Polaris 添加自定义检查，以查找与您的环境或其他组织需求相关的特定内容。

在配置部分，您可以看到检查列表及其严重程度。

有三种不同的级别可供选择:

*   **忽略:**这些甚至不会显示在仪表板上

*   **警告:**在仪表板中显示为警告

*   **危险:**这些也显示在仪表板上，但也可以被我们的验证准入网络挂钩阻止

一旦你在 Polaris 中完成了检查，修复了所有的警告并加入了豁免，你就可以开始阻止违反这些规则的东西进入你的集群。您可以将它们标记为“危险”，并在准入请求级别阻止它们。 [查看完整教程](https://www.fairwinds.com/blog/how-to-use-polaris-to-identify-kubernetes-security) 。

### 自定义策略示例:图像注册表的自定义检查。

如果您想要求所有图像都来自特定的注册表列表，您可以在 Polaris 中编写一个策略来强制执行。Polaris 策略是用 JSON 模式编写的。如果你进入 Polaris 库，你可以看到所有的默认检查都在 YAML 文件的 checks 文件夹中。如果您查看容器的 image 属性，您可以指出它必须匹配这些字符串模式匹配中的任何一个。如果其中任何一项不匹配，就会在 Polaris 仪表盘上弹出警告。

### 北极星:吊舱破坏预算中的豁免

cert manager 的 pod 中断预算就是一个很好的例子。证书管理器是此群集中的单个 pod 控制器。它不需要运行多次，因为它是作为协调循环运行的。如果它下降或消失了一点点，它会回来处理它的业务。您可以通过转到“控制器名称”并设置规则来为 cert manager 添加豁免，使其不受缺少 pod 中断预算规则的约束。保存后，重新运行引用该值文件的 Helm 安装程序，并在该名称空间中更新 Polaris。那么证书管理器将不再触发北极星警告。

## 金发女孩:确定资源需求和限制

Polaris 发送关于设置资源请求和限制的报告。使用 Kubernetes 的每个人都意识到需要遵循最佳实践来设置 CPU、内存和工作负载的资源请求和限制。然而，这样做可能是一个挑战，因为要正确地做它，你需要大量的信息，包括指标(在合理的时间跨度内收集的)、你的高低点的流量信息，以及更多的数据点。这需要手动完成大量工作，这也是 Fairwinds 创造了[](https://goldilocks.docs.fairwinds.com/)金发女孩来帮你完成这些工作的原因。

开源工具 Goldilocks 通过设置适当的 CPU/内存来帮助用户优化 Kubernetes 的资源。它需要你安装一个 [公制服务器](https://github.com/kubernetes-sigs/metrics-server) 和 [垂直 Pod 自动缩放器](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler) ，这两个都是标配。(大多数人已经在他们的集群中安装了它们。)垂直 Pod 自动缩放器有三个组件:推荐器、更新器和准入控制器。您可以使用他们的舵图安装公制服务器，以便安装垂直 Pod 自动缩放器；我们也有 VPA 的舵手图，因为没有官方的。默认情况下，我们的舵图只安装推荐器和更新器。

建议程序从度量服务器获取度量，然后使用这些度量向 VPA 对象提出建议，这些对象是为您附加到 VPA 的部署创建的。Goldilocks 使用这些价值来提出建议，或者根据 VPA 向你展示这些价值。你可以在这里 阅读 [调整 Kubernetes 工作负载的完整教程。](https://www.fairwinds.com/blog/setting-rightsizing-kubernetes-workloads)

### 如何将金发女孩与您的工作负载联系起来

如果您希望 Goldilocks 为您的工作负载创建 VPA 对象，您需要用一个特定的标签来标记名称空间。如果您从一个纯普通的安装开始，有一小段文本告诉您需要运行哪个命令来手动标记您的名称空间。一旦标记了名称空间，就需要刷新仪表板。这就是向 Goldilocks 添加工作负载是多么容易。现在部署在该演示命名空间中的所有内容都将被提取，并有一个与之关联的 VPA。

使用垂直 Pod 自动缩放器，如果您安装了更新程序，更新程序可以根据来自推荐者的信息垂直缩放您的 Pod 上的资源。如果将这些设置为禁用，它不会自动为您缩放。您可以向您的名称空间传递另一个标签，以将更新模式设置为 auto。

### 建议

来自推荐者的推荐在我们如何推荐 Kubernetes 的资源限制和 Goldilocks 中的请求方面起着作用。类似于你在北极星中看到的，金发女孩感觉到有一个名称空间。基于此，它向您展示:

![Namespace details - CPU and memory requests and limits](img/344aa609e5ebc62666bf9bfbf3446408.png)

您可以看到当前的设置，以及对于保证服务质量和突发服务质量的建议。对于这些打印出一些 YAML，然后你可以复制并粘贴到你的舵图表。

Goldilocks 包括一个术语表，解释了有保证的服务质量、可突发的服务质量之间的区别，以及 Kubernetes 在面临资源压力时如何处理您的工作负载。Goldilocks 建议是设置资源请求和限制的起点，您可以根据它们在流量模式方面的工作方式进行测试和调整。

## Nova:查找过时或废弃的头盔图

[Nova](https://nova.docs.fairwinds.com/) 帮你找到过时或弃用的舵图。它会扫描您的集群以获取 Helm 图表的更新，如果您没有使用 Helm，还会扫描容器图像。这对于跟踪您希望尽可能保持最新的加载项、证书管理器和外部 DNS 非常有用。有许多安全修补程序和稳定性修补程序需要保持更新，以维护集群的安全性和稳定性。如果没有 Nova 这样的工具，跟踪这一点可能会非常困难。

Nova 使用起来很简单。默认情况下，Nova 将以 JSON 格式输出。如果你传递了 format table 标志，它会给你一个小表，显示你的版本名图表，名称，命名空间，和 Helm 版本。

![Nova report of Helm charts](img/02bbb1f02b81300bfea989c630007405.png)

如果你看上面的截图，你可以看到 Nova 给你的信息:

*   Install 是集群中的图表版本

*   最新版本是 Nova 已知的最新图表版本

*   Old 是一个布尔值，表示您的版本是否是旧的

*   弃用是一个标志，可以设置为弃用，以指示将来不应使用舵图

Nova 通过拉动 [工件中枢](https://artifacthub.io/) 来获得关于图表的不同版本以及它们是否被否决的信息。要记住的一件事是，你可以分叉图表，有时这些分叉图表会出现在工件中心。Nova 无法看到您的版本来自哪个上游图表。记住这一点很重要。Fairwinds 做了一些匹配和评分，试图减轻这种情况，但 Nova 引用的可能是不同于您正在使用的分叉。

你可以在这里 阅读一篇 [完整 Nova 教程。](https://www.fairwinds.com/blog/find-outdated-helm-charts-with-nova)

### Nova 扫描集装箱图像

Nova 还可以找到并扫描集群中运行的容器。扫描将向您显示当前(已安装)的版本，无论它是旧的，最新的主要版本，最新的次要版本，以及最新的补丁版本。这为您提供了有关更新内容的更多信息。

![Nova container images report](img/dea431d5c506b60ed6c52e80f374e548.png)

例如，您可能不担心将您的附加组件更新到最新的补丁版本，甚至是最新的次要版本。使用 Nova，您可以线性或精细地决定如何处理插件修补和升级。Nova 还提供了更多的配置选项供您尝试。

## Pluto:查找废弃的资源

当我们在 [Kubernetes 1.16](https://kubernetes.io/blog/2019/09/18/kubernetes-1-16-release-announcement/) 升级中遇到挑战时，Fairwinds 的团队编写了 [冥王星](https://pluto.docs.fairwinds.com/) 。Pluto 可以帮助您在 Kubernetes 集群中找到不推荐的和删除的 API 版本。1.16 中删除了所有旧的部署扩展 V1、Beta1、API 版本，因此，如果您周围有许多旧的 YAML，或者您正在部署旧的舵图，其中包含不赞成使用的 API 版本，那么更新这些版本将非常困难。我们需要一种方法来告诉我们的客户他们正在使用的东西已经被弃用或删除。

Kubernetes API 服务器自动将 API 版本从一个版本转换到下一个版本。这使得就地升级变得容易，但是在完成升级后，它会破坏您部署到集群的能力。我们希望我们的客户明白，与其简单地阻止他们更新集群，

Pluto 是另一个 CLI 工具，所以当我们运行 Pluto 时，它会给出一个表格输出，其中包括:

*   对象列表

*   它们是什么种类的

*   版本

*   API 版本

*   替换它的 API 版本

*   它是否已被删除

*   是否已被弃用

[观看冥王星](https://www.fairwinds.com/blog/find-deprecated-and-removed-apis) 的视频概述(附文字稿)。![Pluto table output showing API versions](img/7dc3c4e20e7651c402817128eb6aa1e1.png)

### 冥王星探测 API 资源命令

感谢一位社区贡献者，我们现在有了 detect API resources 命令，该命令查找 Kubernetes 添加到一些资源中的注释，该注释包含上次应用的 YAML 的字符串副本。如果将 CTL 应用于 YAMLs，那么注释就会被设置。Pluto 可以检测您不赞成使用的 API 版本，您可以在 CI 中使用它来阻止构建或使构建失败，如果人们试图部署不赞成使用或删除的东西，

## Kubernetes 为您的云原生之旅提供开源工具

如果您运行所有这四个 Kubernetes 开源工具，修复它们找到的所有东西，并遵循它们提供的建议，您应该在 Kubernetes 之旅中处于一个更好的位置。如果你不想安装所有这些工具，自己运行它们，编写自己的 CI/CD 代码，部署到你所有的集群中，[fair winds Insights](https://www.fairwinds.com/insights)就是我们的商业 SaaS 平台。它结合了这四个开源工具以及更多的工具，并添加了更多的功能，将操作项报告到一个仪表板中。

> 你可以免费使用 Fairwinds Insights，永远 [在这里了解更多](https://www.fairwinds.com/coming-soon) 。

洞察可以帮助您将 Kubernetes 的安全性、成本、合规性和防护措施落实到位，以确保 Kubernetes 为您的组织正常工作。

观看网上研讨会点播，获得关于 [这四个 Kubernetes 开源工具](https://www.fairwinds.com/kube-clinic-kube-gov-strat-11-09-reg) 如何工作的演示。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)