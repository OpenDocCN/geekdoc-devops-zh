# 头盔与 Kustomize |马具的比较

> 原文：<https://www.harness.io/blog/helm-vs-kustomize>

Kubernetes 生态系统中处理配置和包管理的两个工具:Kustomize 和 Helm。在这篇文章中了解他们是如何比较的！

Kubernetes 生态系统仍然相对较新。随着 Kubernetes 于 2014 年夏天发布 GitHub，并于 2015 年夏天发布第一个 1.0 版本，许多人对其速度和进步仍记忆犹新。像任何应用程序基础设施一样，随着大量工作负载开始放在 Kubernetes 上，操作任务变得非常重要。

Kubernetes 提出了资源的概念，这些资源通常由基于 YAML 的清单来描述；例如，deployment.yaml。如果第一次使用 Kubernetes，您可能会部署一个简单的单一映像，如 NGINX，并且可以在线性清单中描述一个部署，例如 one deployment.yaml。由于 Kubernetes 是可插拔的，并且您可以非常容易地更改 Kubernetes 的观点，因此对于任何实质性的工作负载，部署资源(yaml 文件)的数量都会增加。

随着需要部署的清单数量(例如，在微服务架构中)和要维护和部署到的 Kubernetes 集群数量的双重增长，这很可能需要集群与集群之间的差异(例如，网络堆栈)，组织很容易因清单支持这一点而超支。输入两个工具来处理 Kubernetes 生态系统中的配置和包管理:分别是 Kustomize 和 Helm。

## 什么是 Kustomize？

Kustomize 是一个用于 Kubernetes 生态系统的配置管理工具。配置管理作为一门核心学科，专注于维护跨环境的一致性。随着 Kubernetes 引入新的范例，以及计算资源的转移变得更加短暂，Kubernetes 集群/环境的数量继续扩大。拥抱短暂的基础设施，即使是 Kubernetes 集群本身也可以回收和重建。Kubernetes 的优点和挑战在于，Kubernetes 可以将应用程序基础设施所需的操作任务编成代码，这往往会增加 Kubernetes 清单文件的长度和范围。

作为一个[实现](https://github.com/kubernetes/community/blob/master/contributors/design-proposals/architecture/declarative-application-management.md)，Kustomize 是一个声明性的模板引擎，它基于重构 Kubernetes 清单的概念。基本 Kubernetes YAMLs 与库定制化配置(覆盖和库定制化文件)相匹配，并根据这些文件中定义的逻辑进行重构/替换。由于大量的应用程序都指向 Kubernetes 并维护集群本身，Kustomize 有助于实现跨 Kubernetes 资源和部署到 Kubernetes 的应用程序的配置管理。

### Kustomize 结构

Kustomize 提出了一个“在哪里、做什么以及如何”的概念来重构特定的 Kubernetes 清单。要重构/更改的“位置”是基本清单，例如 deployment.yaml。要更改的“内容”是要更改的 yaml 的覆盖或小片段，例如副本数量、卷装载等。“如何”改变是 kustomization/config 文件。

### Kustomize 文件

如果您以前没有使用过 Kustomize，假设您已经有了 Kubernetes 清单(一个部署/服务/etc/。yaml)作为基础，Kustomize 文件(Kustomize . YAML)将是 Kustomize 逻辑所在的位置。匹配逻辑可以在特定的 Kubernetes 资源上工作，例如特定的基本文件，但是更动态地可以匹配那些基本文件内的特定 Kubernetes [标签](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/)和[注释](https://kubernetes.io/docs/concepts/overview/working-with-objects/annotations/)。

### Kustomize 的优点

使用 Kustomize 的当代比较落在备选方案上，即不使用包/配置管理器的现状，并将 Kustomize 与赫尔姆 V2 进行比较。

使用模板引擎/配置管理解决方案消除了为特定变更保持多个清单的负担。

现状将是在源代码控制中保留清单中的每一个更改，并且仅仅基于文件名很难区分哪些细微的差别适用于 Kubernetes 中的什么环境。

Kustomize 可以由 Kubernetes 命令行界面(CLI)从 1.14 版开始运行。与需要在 Kubernetes 集群上使用名为 Tiller 的特权舱的圣盔 V2 相比，Kustomize 可以在没有集群依赖性的情况下启动并运行。

### Kustomize 的缺点

像任何技术一样，在实施和采用时都有一个学习曲线和挑战。根据组织的不同，模板化的好处也可能是一种阻碍。Kustomize 没有提供太多现成的约定，而是严格遵循模板引擎设计。

最终用户可能无需显式的模板代码就可以更改基本清单中的任何内容。通过根据文件、标签或注释进行匹配，最终用户必须更详细地了解将会发生什么变化以及在哪里发生变化。制作拙劣的基本 YAMLs 可能没有容易访问的注释/选择器来匹配 Kustomize 文件。与 Helm 相比，Kustomize 不是一个完整的生态系统(例如，一个存储库格式本身),它依赖于 SCM，就像 Git 存储库一样。

## 什么是头盔？

Helm 被视为 Kubernetes 生态系统事实上的包管理解决方案。软件包管理器专注于安装和卸载软件包。类似于 Linux 世界中的[YUM](https://en.wikipedia.org/wiki/Yum_(software))/[RPM](https://en.wikipedia.org/wiki/RPM_Package_Manager)/[APT](https://en.wikipedia.org/wiki/APT_(software))，或者分别在你的 Mac/Windows 机器上使用[home brew](https://brew.sh/)/[Chocolatey](https://chocolatey.org/)，Helm 简化了软件安装生命周期。

Helm 最初由 [Deis](https://blogs.microsoft.com/blog/2017/04/10/microsoft-acquire-deis-help-companies-innovate-containers/) 于 2015 年开发，并在首届 Kubecon 上展示。Helm 被组织和供应商广泛采用，作为一种在 Kubernetes 集群上加载和卸载软件的机制。它也是一种存储格式，允许快速共享/分发 Helm 资源。有几个开放社区托管 Helm 存储库，供应商/项目可以拥有特定的 Helm 存储库。

### 舵结构

舵运行了舵图和舵模板的概念。当模板与值结合时，模板将根据变量/值生成有效的 Kubernetes 清单。

### 舵图

图表是 Helm 使用的主要格式。由于舵图是基于文件的，并遵循基于约定的目录结构，因此图表可以很容易地存储在图表存储库中。在 Kubernetes 集群中安装和卸载图表。像图像到容器的关系一样，图表的运行实例被称为发布。最后，Helm Chart 是一个将图表定义转换为 Kubernetes 清单的执行模板。

舵图在创建时必须有一个遵循[语义版本](https://semver.org/spec/v2.0.0.html)的版本号。Helm 图表可以引用其他图表作为依赖项，是任何软件包管理器的核心。Helm Charts 更高级的特性是[图表挂钩](https://helm.sh/docs/topics/charts_hooks/)和[图表测试](https://helm.sh/docs/topics/chart_tests/)，它们允许与一个版本的生命周期进行交互，以及分别针对一个图表运行命令/测试的能力。

### 舵的优点

利用 Helm 的好处与软件包管理人员从 Kubernetes 生态系统中获得的好处相同。通过允许用户创建、共享和分发安装/卸载软件的指令的生态系统和惯例，软件的消费肯定会增加。

包含一个约定和存储库，舵轮图的创建者可以对消费者实施模板和标准。DevOps 或平台工程团队可以向内部客户提供更多完整的图表。

### 头盔的缺点

大量的反对意见都指向了 V2 头盔和舵柄的引入(以及后来在第三版头盔中的移除)。舵柄是舵平台的一部分。它来自与谷歌部署管理器的项目集成的一部分，被设计成一个作业运行器，与 [Cloud Foundry 的 Bosh](https://www.bosh.io/docs/) 没有太大不同。简而言之，Tiller 是 Helm 的集群部分，它运行 Helm 的命令和图表。由于 Tiller 可以不受限制地访问 Kubernetes 集群，这成为了集群安全性的一个痛处。

赫尔姆 V2，直到赫尔姆 V3 发布前的最后版本之一，都是以明文形式在 Kubernetes [ConfigMaps](https://kubernetes.io/docs/concepts/configuration/configmap/) 中存储秘密或敏感信息。这可能对 Helm 的内部使用有意义，但随着 Helm 图表开始走向公共存储库，这种做法需要改变。

最后，对于简单的部署来说，Helm 可能是多余的。Helm 带来的复杂性，以及作为一个发布经理本身，是一个额外的复杂导航层。因为组织希望对如何部署/发布进行标准化，所以为了一致性，将无需 Helm 即可轻松完成的项目迁移到 Helm 是一项艰巨的任务。

## 我还应该看哪些 Kubernetes 模板工具？

使用 Kustomize 或 Helm 的两个主要替代方法是 Jsonnett 和 Skaffold。

### JSON nt/kson nt

Jsonnet 是一种模板语言和引擎。Jsonnett 有一个面向对象的模板方法，允许创建复杂的基于关系的模板。如果你需要复制某样东西，只需创建一个新的对象，支持模板就会接管。 [Ksonnnet](https://github.com/ksonnet/ksonnet) ，由 [Heptio](https://tanzu.vmware.com/content/blog/welcoming-heptio-open-source-projects-to-vmware) 创作，是 Jsonnet 的一个分支，专门为 Kubernetes 设计。截至最近，Ksonnet 背后的团队不再支持该项目。

### 斯卡福德

Skaffold 是谷歌最新的 Kubernetes 管理项目之一。Skaffold 比 Jsonnet、Helm 甚至 Kustomize 更具包容性。因为 Skaffold 已经构建和部署了组件，所以它被设计成 Kubernetes 应用程序所需的一个配方。Skaffold 在部署阶段可通过 [Helm](https://skaffold.dev/docs/pipeline-stages/deployers/helm/) 和 [Kustomize](https://skaffold.dev/docs/pipeline-stages/deployers/kustomize/) 进行插拔。

## 利用 Harness 改进 Kubernetes 部署

无论您选择 Kubernetes 部署/模板方法，Harness 都会支持您。即使您不是专家，或者您正在设计将被整个组织利用的管道，Harness 也会使这一过程更加简单。

*模板化 Kustomize 环境，以便用户可以选择部署哪个环境。*

Harness 广泛支持 Helm 和 Kustomize。“Harness”抽象模型允许部署模板控制下的应用程序(例如 Helm 或 Kustomize 部署)和外部模板控制。

*一次成功的带挽具的 Kustomize 展开。*

随意注册一个[驾驭账户](https://app.harness.io/auth/#/signup/)，开始你的 Kubernetes 之旅吧！

干杯，

拉维