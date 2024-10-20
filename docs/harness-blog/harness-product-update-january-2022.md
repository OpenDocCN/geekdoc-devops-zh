# 线束产品更新| 2022 年 1 月|线束

> 原文：<https://www.harness.io/blog/harness-product-update-january-2022>

产品更新 2022 年 1 月:查看我们的最新动态，了解产品功能、活动、网络研讨会、HarnessU 等方面的更新。

希望每个人都能享受假期。在 Harness，我们工作，我们玩耍，我们放松，然后我们回去努力工作，做公司需要快速安全地构建和交付软件的所有事情。老实说，这是我们喜欢的，所以感觉不太像工作。

随着 Harness Community Edition 的发布，我们在一月份发布了一个大型公告...[线束发射源可用连续发射](https://harness.io/blog/harness-launches-source-available-continuous-delivery/)

Harness editions from the pricing page.

## 新功能

### **词**

*   **支持 AWS EC2 Windows & Linux 实例作为** [**构建基础设施**](https://ngdocs.harness.io/article/ia5dwx5ya8-set-up-a-kubernetes-cluster-build-infrastructure) **-** 这带来了一种新的功能，可以直接在基于 Windows 和 Linux 的虚拟机上、Docker 容器中或直接在主机上运行构建。这在需要直接运行 Docker 命令时也很有用，因为在 K8S 中运行 Docker 命令需要特权模式，而不是在虚拟机上运行。
*   **支持。用户现在可以使用[测试智能](https://ngdocs.harness.io/article/vtu9k1dsfa-test-intelligence-concepts)。Net 框架应用程序，以优化和减少他们的单元测试执行时间。**

The benefits of using Test Intelligence - unique to Harness CI Enterprise.

### **CD**

*   [**设置连接器时跳过验证**](https://ngdocs.harness.io/article/sjjik49xww-kubernetes-cluster-connector-settings-reference#credential_validation)**-**我们为 ServiceNow 和 Artifactory 连接器引入了一个复选框“跳过验证”。选中此框将允许用户在创建/更新过程中跳过凭据验证。如果该框保持未选中状态，将遵循默认行为。

*   **与**[**Terraform providers**](https://ngdocs.harness.io/article/boug6e884h-terraform-provisioning-with-harness)一起使用承担角色——现在，您可以通过假设代理人有权访问您的 terra form，而无需提供内联凭据，来与 terra form providers 一起使用承担角色。在特性标志后面提供新的功能:terra form _ AWS _ CP _ authentic ation。

*   **IRSA 凭证通过 GQL** 设置您的云提供商——现在您可以使用 IRSA 凭证通过[graph QL API](https://ngdocs.harness.io/article/f0aqiv3td7-api-quickstart)设置您的云提供商。

### **特征标志**

Feature flag management via pipeline.

*   **公共 API**——用户现在可以使用 API 以编程方式创建、编辑和管理他们系统中的特性标志。除了使用 UI，团队还可以利用 API 来自动化一些他们通常会做的特性标志管理工作。以更快的速度工作，同时以您喜欢的方式工作。
*   [**用于单一接入点的代理服务**](https://ngdocs.harness.io/article/q0kvq8nd2o-relay-proxy)**——对于喜欢只使用一个开放连接器的客户，他们可以使用代理服务通过单一接入点连接多个服务器或流。对于企业来说，它创建了在中断情况下的标志状态恢复能力，并允许团队在有空气间隙的环境中工作。**

### ****云成本管理****

*   ****[**自动停止规则的固定计划**](https://ngdocs.harness.io/article/wzr5tz0ero-auto-stopping-rules) **-** 用户现在可以计划正常运行时间或停机时间的固定窗口，而不考虑资源的活动或闲置。它很容易设置，可以为每个规则配置多个固定窗口。它消除了 DevOps 团队维护额外脚本来处理这些场景的需要。****
*   ******Azure 虚拟机库存管理** -用户现在可以使用专用的 OOTB BI 仪表板管理 [Azure](https://ngdocs.harness.io/article/v682mz6qfd-set-up-cost-visibility-for-azure) 虚拟机的库存，其中包括成本、利用率指标和相关标签。****

## ****哈斯林大学****

******新模块和课程:******

****我们现在有 [**线束大学**](http://university.harness.io/) 的**新课程。******

****[**驾驭安全基础**](https://university.harness.io/path/harness-security-foundations) :该路径涵盖各种安全主题，包括代表安全、基于角色的访问控制、认证和授权、审计和治理、秘密管理以及安全和驾驭的最佳实践。****

****[**部署验证**](https://university.harness.io/deployment-verification) :本课程涵盖部署验证概念以及如何在 Harness 中设置您的验证步骤。****

****[**应用架构**](https://university.harness.io/application-architectures) :本模块将介绍不同类型的应用架构以及从单片到微服务和容器的演变。****

******驾驭大学直播:******

******从今年开始，我们将提供持续集成和功能标志现场培训！******

*   ****2022 年 2 月 14 日和 15 日:线束基础知识，太平洋标准时间上午 6 点到 9 点。****
*   ****2022 年 2 月 16 日:线束管理，太平洋标准时间上午 6 点到 10 点。****
*   ****2022 年 2 月 17 日:利用持续集成和功能标志，太平洋标准时间上午 6 点到 9 点****

****要注册 2 月份及以后的活动，请查看我们的 [**现场活动日历**](https://university.harness.io/calendar) 。****

## ****治理社区****

******社区论坛:******

******聚会:******

****要注册即将到来的 Harness 社区聚会，请访问[https://meetup.com/harness](https://meetup.com/harness)。****

## ****报刊上值得注意的提及****

## ****即将举行的活动****

*   ****开发者周- 2 月 2 日-2 月 4 日****
*   ****2017 年 2 月的绍姆堡顶级高尔夫球场****
*   ****西方 2022 年 2 月 15 日至 2018 年 2 月****
*   ****16 年 2 月 2 日，纳多·达拉斯****
*   ****落基山网络空间研讨会 2 月 21 日至 2 月 24 日****
*   ****威士忌品尝 2/24****
*   ****行政品酒会 2/24****

## ****即将举办的网络研讨会和新的点播内容****

## ****展望未来****

****在接下来的几周/几个月里，你会遇到以下情况:****

******CI******

*   ******支持。Net Core -** 客户现在可以通过他们的。Net 核心应用程序，以优化和减少他们的单元测试执行时间。****
*   ****支持**CI 步骤的自签名证书******
*   ******OSX 支持**——能够在 OSX 机器上运行构建，在亚马逊就像直接在物理机器上一样。****
*   ****插件卡 -允许插件通过可视化的方式来扩展无人机的用户界面。这个特性提供了一个对构建中发生的事情的高级洞察的中心位置，并允许开发人员更快地识别 bug 和缺陷。该功能计划添加到 Q2 2021 的线束平台中。****

******CD******

*   ******外壳脚本超时错误将由新的故障策略**处理——如果外壳脚本超时，您将能够拥有一个单独的故障策略，另一个用于其余的故障。这将是在一个功能标志后面。****

*   ******工作流事件的事件规则-** 我们正在增强我们的事件规则，以前仅支持管道事件，现在也支持工作流事件(工作流开始、工作流结束、工作流暂停和工作流继续)。如果是直接工作流执行或工作流作为管道的一部分执行，我们将在两种情况下发送这些事件。这将是后面同一个功能标志——APP _ TELEMETRY****

*   ******支持阶段增强后回滚供应器-** 作为 MVP 的一部分，我们在工作流&步骤级别**的阶段**后为故障策略中的新选项**回滚供应器提供了支持。**当手动干预正在等待由您触发的手动回滚的动作&时，我们也在扩展。这将是在相同的功能标记之后-roll back _ PROVISIONERS _ AFTER _ PHASES****

*   ******Github webhook secret 对触发器是强制性的-** 随着这一变化，将有一个应用程序级别的设置，通过它您可以强制所有 Github 触发器包含 webhook secret，否则调用将失败。这将是特性标志的背后——GITHUB _ web hook _ authentic ation****

*   ******步骤名称的表达式-** 通过此更改，您将能够在步骤中使用表达式来访问步骤名称。****

*   ******内联主机支持目标到特定主机** -通过这一更改，您将能够向特定主机提供在目标中的基础架构定义中不存在的主机。您还可以选择提供一个引用主机列表的表达式。这将是 FF - DEPLOY_TO_INLINE_HOSTS 的幕后推手****

*   ******支持非容器 Azure 应用服务** -有了它，你将能够从 ARTIFACTORY、NEXUS、JENKINS、AZURE_ARTIFACTS 等工件源在 Azure Web 应用上部署 war、nuget、zip。这将在 FF - AZURE_WEBAPP_NON_CONTAINER 的后面****
*   ******采用退避策略的云形成操作** -我们将为所有 CF 操作支持指数退避策略。这将是可配置的，通过设置一些帐户级别的默认值。****

*   ******SSH 服务的实例同步**——我们将为 SSH 部署(PDC，AWS & Azure)提供实例同步，通过它，服务仪表板将包含这些服务的实例的实时值。****

*   ******已弃用-force from terra form Destroy for version 15**-对于 Terraform version 15，-force 选项已弃用。我们也在利用这一点。****

******特征标志******

*   ******新的 SDK******
*   ****红宝石****
*   ****Xamarin****
*   ******获得永久免费和团队计划** -用户将能够永久免费使用功能标志。如果他们需要扩大规模，有一个团队计划以合理的成本将他们带到 Harness 平台上。****
*   ****目标管理 UX 升级 -我们已经从客户那里得到反馈，关于旗帜的目标管理可以做得更好。我们已经根据这些反馈采取了行动，并将在下个月推出一项重大的 UX 改进。****

******云成本管理******

*   ******自动停止摘要页面增强功能** -通过这些增强功能，用户将有各种选项来搜索、排序和过滤自动停止规则。您将能够排序规则的储蓄，名称或最后一次活动。此外，您可以使用名称、主机名或云提供商进行搜索和过滤。****
*   ******自动停止自定义排除-** 用户将能够排除某些类型的流量，使其无法启动与自动停止规则相关的资源。如果检测到这些被排除的流量请求，实例将保持停止状态。****
*   ******工作负载建议增强功能-** 这些增强功能允许用户在提出建议时向历史利用率指标添加缓冲区。此外，您还可以为 Kubernetes pods 指定 QoS(服务质量)是有保证的还是可突发的。****
*   ******节点建议增强-** 此更新将提供与使用的历史 CPU/内存利用率指标相似的缓冲区，同时提出类似于工作负载建议的建议。它将提供指定首选节点实例系列的能力。****

****咻，那是很多，但是等着看我们在二月份完成了什么吧。****