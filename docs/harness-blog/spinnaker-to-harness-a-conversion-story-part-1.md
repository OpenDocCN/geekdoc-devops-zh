# 从 Spinnaker 迁移到线束|线束

> 原文：<https://www.harness.io/blog/spinnaker-to-harness-a-conversion-story-part-1>

从 Spinnaker 迁移到 Harness 不是一对一的过程。它始于理解围绕每一个的需求和过程，然后进行跳跃！

当试图采用新的[方法](https://en.wikipedia.org/wiki/Methodology)时，任何人面临的最大挑战之一是弄清楚他们的旧方法相比如何。如果需要，旧方法的哪些部分可以转换，或者是否可能转换？对于那些喜欢旧方法的人来说，[用户采用](https://en.wikipedia.org/wiki/Adoption_(software_implementation))会是什么样的？要花多少努力才能把所有人和所有事都转变过来？

有些人会选择“冷火鸡”的方法，在这种方法中，他们一扫而过，彻底切断旧的。其他人将遵循"[快乐之路](https://en.wikipedia.org/wiki/Happy_path)"方法，其中唯一正式的和受支持的过程是新的过程，但是其他人可以选择构建和支持他们自己的过程，只要他们的团队根据某些预定义的度量是有生产力的。最后，其他人将追求“缓慢转换”的方法，他们将挑选某个小组或项目开始新的过程，然后随着时间的推移慢慢增加其他小组或项目，同时让每个人都知道未来会发生什么。

根据您的方法风格(新方法的采用没有一刀切)，最好的开始是理解新旧之间的设置和层次差异、新旧之间的转换过程，以及最重要的新旧之间的概念差异。

在这篇文章中，我们将讨论[三角帆](https://www.spinnaker.io/)和[挽具](https://harness.io/)之间的设置和等级差异。这是为了帮助读者理解阅读时需要付出的努力，以及为什么每件事并不总是一对一的过程。

## **三角帆**

首先要了解什么是 Spinnaker 体系结构，从基础设施的角度来看需要什么，设置和维护 Spinnaker 所涉及的维护，使其可以投入生产的安装过程，以及 Spinnaker 中的不同实体。

### **架构**

[Spinnaker 由 11+种不同的微服务组成](https://www.spinnaker.io/reference/architecture/)

#### **甲板**

#### **浇口**

*   API 网关
*   与 Front50、Igor、Echo、Orca、Clouddriver、Rosco 和 Kayenta microservices 对话
*   最低资源要求:约 250 万 CPU 和约 1Gi 内存

#### 虎鲸

*   编排引擎
*   与 Front50、Fita、Clouddriver、Rosco 和 Kayenta microservices 对话
*   最低资源要求:约 1 个 CPU 和约 4Gi 内存

#### **云驱动**

*   改变对云提供商的调用并索引/缓存所有部署的资源
*   与菲亚特微服务公司对话
*   最低资源要求:约 1 个 CPU 和约 4Gi 内存

#### **Front50**

*   应用程序、管道、项目和通知的元数据持久性
*   与菲亚特微服务公司对话
*   最低资源要求:约 250 万 CPU 和约 4Gi 内存

#### **罗斯科**

*   面向云提供商的不可变图像或图像模板
*   最低资源要求:约 250 万 CPU 和约 1Gi 内存

#### **伊戈尔**

*   持续集成工具集成和触发
*   与 Echo 微服务对话
*   最低资源要求:约 250 万 CPU 和约 1Gi 内存

#### **回音**

*   事件总线
*   与 Front50 和 Orca 微服务对话
*   最低资源要求:约 250 万 CPU 和约 1Gi 内存

#### **菲亚特**

#### **卡延塔**

*   金丝雀分析
*   最低资源要求:(至少约 1 个 CPU 和约 4Gi 内存，因为它需要 Orca，但也可能更多)

#### **升降索**

*   Spinnaker 配置服务
*   升降索 CLI 与升降索守护程序对话
*   最低资源要求:大约 200m CPU 和大约 2Gi 内存
*   **注意:**要让 Halyard 更新 Spinnaker，它会旋转一个[无头 Spinnaker 来更新你的 Spinnaker。](https://www.spinnaker.io/setup/install/environment/#distributed-installation)

这些服务中的每一个都有自己的版本，必须正确更新，通常由 Halyard 处理，因为不同微服务之间很少/没有向后兼容性。

Spinnaker 需要做的其他一些事情是为不同的微服务设置伸缩，为 Spinnaker 设置 HA/DR(以防它关闭),并使用您自己的度量或日志解决方案监视 Spinnaker/

### **安装流程**

要开始安装，Spinnaker 需要一个名为 Halyard 的单独的安装、配置和更新管理设备，该设备必须安装在一个单独的机器上，而不是安装 Spinnaker 的集群上。事实上，如果您想获得社区的支持，Spinnaker 的所有生产部署都需要 Halyard。

一旦 Halyard 安装完毕，您就可以开始设置您的帐户(云提供商)来部署您的应用程序。Spinnaker 做任何事情都需要这些云提供商。仅供参考，这是通过升降索，在用户得到 Spinnaker 实际上站起来。

添加云提供商/帐户后，您需要告诉 Halyard Spinnaker 需要安装在什么类型的环境中。像 Kubernetes 这样的分布式安装是[推荐用于生产](https://www.spinnaker.io/setup/install/environment/#distributed-installation)的方法，特别是因为处理流量需要扩展。

Spinnaker 下一个需要的部分是存储，用户将为 Spinnaker 设置一个外部存储系统来存储任何需要在升级后持久保存的数据。这些数据非常重要，建议用户选择具有冗余的存储选项。

在安装了 Halyard、设置了云提供者、选择了环境和分发类型、创建并连接了外部存储提供者之后，您现在可以安装 Spinnaker 并访问 UI 了。

虽然这是一般的安装过程，但是如果用户想要设置映像包，请备份配置、启用安全性、设置 CI 连接、启用监控等。

### **实体**

#### **应用**

Spinnaker 将一个[应用程序](https://www.spinnaker.io/concepts/#application)定义为一个集群的集合，而集群又是服务器组的集合。该应用还包括防火墙和负载平衡器。这意味着用户将要求他们的团队根据他们将要部署的环境减少他们的应用程序、服务或微服务。从长远来看，这将大大简化设置，因为管道是相当静态的。

#### **集群**

Spinnaker [集群](https://www.spinnaker.io/concepts/#cluster)是服务器组的手动分组，而不是 Kubernetes 集群。这是 Spinnaker 最初由网飞为 AWS AMIs 建造的结果。

#### **服务器组**

为了避免混淆，一个[服务器组](https://www.spinnaker.io/concepts/#server-group)标识了可部署的工件和工件的配置设置。同样，这是 Spinnaker 最初由网飞为 AWS AMIs 建造的结果。这对 Kubernetes 用户来说意味着服务器组是一组与工件相关的 pod。

#### **负载平衡器**

[负载平衡器](https://www.spinnaker.io/concepts/#load-balancer)是入口协议和端口范围，用于平衡服务器组之间的流量(带有可选的健康检查)。在 Kubernetes 的世界中，这将与入口和服务相关。

#### **防火墙**

[防火墙](https://www.spinnaker.io/concepts/#firewall)是网络流量接入。

#### **管道**

一个[管道](https://www.spinnaker.io/concepts/#pipeline)是 Spinnaker 的主要部署定义。将动作(阶段)或其他管道连接在一起。

#### **阶段**

一个[阶段](https://www.spinnaker.io/concepts/#stage)是 Spinnaker 中的任何动作，它需要一个管道来执行。通常，用户会将多个阶段串联在一起以完成部署。请务必注意，阶段是相对静态的，这意味着如果您创建了一个包含多个阶段的部署管道，并且希望该管道在多个环境中可用，那么您将需要为具有多个环境的每个服务克隆每个工作流。

### 管理要求

与 Spinnaker 相关的最后一个重要部分是与支持和维护解决方案以及解决方案的使用相关的管理需求。在许多尝试或使用 Spinnaker 进行 CD 流程的不同客户中，其中一位[最近分享了](https://harness.io/2020/03/datastax-upgrades-continuous-delivery-with-harness/)他们的整个团队致力于 Spinnaker 的管理工作。此外，一家专门从事 Spinnaker 的专业服务公司表示:

虽然开源平台在技术上是免费的，但是安装和管理 Spinnaker 有其自身的实际成本。Spinnaker 由十个子服务组成(加上额外的外部依赖，如 S3 和 Redis)。这些服务的初始设置、安装和实施可能是一项挑战，而平台的持续维护、升级和扩展都会消耗大量的注意力和资源。转移到微服务并大规模运行连续交付意味着使用许多其他开源服务，这也需要您的工程师的关注。如果您有三名工程师专门负责管理 Spinnaker(我们经常看到全球 2000 强公司有更多的工程师)，那么您每年在 Spinnaker 上花费 600，000 多美元是很容易的。

> [斯图·波斯伦斯 2019 年 4 月 15 日](https://www.armory.io/blog/what-value-does-armory-add-on-top-of-open-source-spinnaker/)

Spinnaker 有一个真实的拥有成本，这是大多数公司在开始时没有考虑的。假定开源软件的同义词是“免费”，但是隐藏的拥有成本可能很快成为难以承受的负担。

## **线束**

接下来要理解的是什么是 Harness 体系结构，从基础设施的角度来看需要什么，设置和维护 Harness 所涉及的维护，使其可以投入生产的安装过程，以及 Harness 中的不同实体。

### **建筑**

设置线束时，只需要考虑两个部分:[经理(线束云)和代理(你的 VPC)](https://developer.harness.io/docs/getting-started/learn-harness-key-concepts)

### **安装过程**

要开始安装过程，请转到您的 Harness 帐户，导航到设置中的 Harness Delegate 部分，下载代理，在所需位置运行安装命令。

### **实体**

#### **应用**

在 Harness 中，类似于 Spinnaker，一个[应用程序](https://docs.harness.io/article/bucothemly-application-configuration)是其他实体的逻辑分组。然而，Harness 中的应用程序是高度模板化的，使得应用程序可以在所有云提供商、环境和其他集成点之间共享。

#### **服务**

在 Harness 中，一个[服务](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/setup-services/add-service-level-config-variables/)是您的工件和基本配置。对于 Kubernetes，这将是核心工件(Docker 图像)和清单。此外，该服务旨在尽可能地模板化，以便它可以跨任何环境和任何工作流或管道使用。

#### **环境**

Harness 中的[环境](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/environments/environment-configuration/)是要部署到的基础设施的逻辑分组。了解环境的最简单方法是基于 RBAC 分组。如果我希望开发人员跨多个云或集群按需部署到开发环境，我将创建一个开发环境，指定开发基础设施，然后授予开发人员执行部署的权限。此外，该环境旨在尽可能地模板化，以便它可以跨任何服务和任何工作流或管道使用。

#### **基础设施定义**

Harness 中的[基础设施定义](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/environments/infrastructure-definitions/)正是如此；你如何定义你的基础设施？有时这与群集中的命名空间或整个群集有关，有时这是一组虚拟机/主机等。此外，基础设施定义旨在尽可能地模板化，以便它可以跨任何服务和任何工作流或管道使用。

#### **工作流程**

在 Harness 中，一个[工作流](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/workflows/workflow-configuration/)是部署定义。这可能是任何预部署、部署和部署后步骤，以及通知规则、失败策略和工作流变量。此外，工作流应该尽可能地模板化，以便它可以用于任何服务，跨任何环境，并且可以在管道中重用。

#### **管道**

Harness 中的[管道](https://developer.harness.io/docs/platform/templates/create-pipeline-template/)是将工作流和/或批准分组到逻辑部署流程中的想法。这可能是用户想要跨多个环境部署的情况，可能是依赖序列中的多个服务，或者甚至是为新的客户/环境部署一组服务。此外，管道旨在尽可能地模板化，以便它可以跨任何环境、任何服务使用，并重用工作流和批准。

#### **触发**

Harness 中的[触发器](https://developer.harness.io/docs/category/triggers)就像它听起来一样，它触发一个工作流或管道被执行。有许多不同类型的触发器，但触发器的主要好处之一是用户可以通过触发器将数据传递到服务、环境、工作流和/或管道中。

#### **基础设施供应商**

许多用户正在利用某种形式的基础设施即代码解决方案(即 Terraform 或 CloudFormation)。在 Harness 中，您可以在交付过程中利用 Terraform 或 CloudFormation 配置。这可能是供应新环境、创建短暂环境或引导环境。此外，[基础设施供应器](https://docs.staging-devharnessio.kinsta.cloud/article/o22jx8amxb-add-an-infra-provisioner)旨在尽可能地模板化，以便它可以跨任何环境和任何工作流使用。

#### **模板库**

随着用户引入更多的自动化测试，甚至需要一些 IaC 目前无法完成的额外引导步骤，Harness 让用户能够将这些脚本添加到[模板库](https://developer.harness.io/docs/category/templates)，对它们进行模板化，并根据需要在任何工作流中重用它们。

### 管理要求

与利用相关的最后一个重要部分是与支持和维护解决方案以及解决方案的使用相关的管理需求。应该假设没有完全不需要管理的解决方案，对于线束来说也是如此。对于一些知道如何导航线束的超级用户有一些要求，以及一些基础设施要求。然而，当客户有一个专门的客户成功团队时，培训和支持流程、设计流程，甚至故障排除或标签都很容易完成。以这种方式考虑利用:您有一个 100 多名专门的工程师团队在您的 CD 过程中工作，所以您不必这样做。请浏览这些令人惊叹的[客户成功案例](https://harness.io/category/customer-success/)以了解更多信息！

### **总结**

如果您已经完成了设置 Spinnaker、扩展 Spinnaker、升级 Spinnaker、维护和管理 Spinnaker 的过程，您会注意到 Harness 中的两个主要差异。首先，Harness 的设置、维护和升级过程比 Spinnaker 简单得多。第二，在 Spinnaker 中，如何构建交付流的思想更加面向雪花(在以后的文章中会有更多介绍)，而在 Harness 中，它旨在尽可能地模板化和可重用，放弃雪花需求。