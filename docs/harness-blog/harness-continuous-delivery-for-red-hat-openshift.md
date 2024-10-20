# 用于 Red Hat OpenShift |线束的线束连续交付

> 原文：<https://www.harness.io/blog/harness-continuous-delivery-for-red-hat-openshift>

借助 Red Hat OpenShift 支持，利用线束连续交付，在几分钟内完成管道部署。通过一步一步的教程了解如何操作。

Red Hat OpenShift 是一个全面的企业级应用程序平台，为带有 Kubernetes 的容器而构建。对于购买了 Red Hat Enterprise Linux 的客户来说，这是一种运行和管理基于容器的应用程序的简单方法。您可以将 Red Hat OpenShift 视为 Pivotal Cloud Foundry 的替代产品，这意味着它是应用程序和底层基础架构或云提供商之间的抽象层。

标准的红帽 OpenShift 架构看起来是这样的:

## 线束连续交付适合哪里？

Harness 符合上述应用生命周期管理 CI/CD 绿盒，特别关注于[连续交付](https://harness.io/products/continuous-delivery/)。

我们的一个财富 500 强大客户请求 OpenShift 支持，以便他们能够在利用 [Helm](https://harness.io/blog/what-is-helm/) 和纯 Kubernetes 编排的团队之外，支持更多的开发团队利用持续交付。

## 我以为 OpenShift 已经有管道了...

在幕后，OpenShift 管道基本上就是 Jenkins 管道:

OpenShift 管道让您可以控制在 OpenShift 上构建、部署和推广您的应用程序。结合使用 Jenkins 管道构建策略、Jenkinsfiles 和 OpenShift 领域特定语言(DSL)(由 OpenShift Jenkins 客户端插件提供)，您可以为任何场景创建高级构建、测试、部署和提升管道。

> OpenShift

Jenkins 管道基本上是由客户编写他们自己的部署外壳脚本(称为“作业”)构建的，并按顺序缝合在一起，以表示各阶段的部署管道。

Harness 不需要客户使用 shell 脚本和作业为每个应用/服务编写部署管道，而是使用其模板(也称为智能自动化)来自动化这一过程。

可以把 Jenkins 管道想象成需要数百个社区插件的硬编码、脆弱的管道，而 Harness 代表动态、灵活的管道，带有开箱即用的支持插件。

点击此处查看更详细的 [Jenkins 与 Harness 对比](https://harness.io/blog/comparing-harness-vs-jenkins/)。

## 我以为 OpenShift 已经有了蓝绿色/淡黄色部署…

是的，OpenShift 通过引导 Kubernetes pods 之间的流量来实现蓝绿和淡黄色部署。然而，Canary 部署被视为标准的滚动部署，因此没有验证或 Canary 实际控制每个部署阶段。

Harness 提供了一个名为持续验证的新概念，它使用 AI 和无监督的机器学习自动验证所有部署和类型(基本、多服务、蓝/绿、金丝雀、滚动……)。
Harness 与您所有的 APM 工具(AppDynamics、New Relic、dyna trace……)和日志分析工具(Splunk、Sumo Logic、ELK……)集成，并且可以自动 a .)验证性能和质量，b .)在发现异常或回归时回滚到先前的工作版本。

## 利用 OpenShift 支持，在几分钟内完成部署流水线

要构建 OpenShift 部署管道，只需遵循以下五个步骤:

### 1.创建新服务

设置>应用程序>创建服务

只需为您的**工件库**添加一个**线束连接器**，并将工件源链接到您在线束中创建的每个服务。

### 2.创建新的云提供商和环境

设置>云提供商>添加

接下来，我们需要设置我们的 OpenShift **云提供商**，选择“Kubernetes Cluster”作为类型，并输入 url、凭证和 Kubernetes 服务帐户令牌细节(见下文)。

这允许 Harness 查询 OpenShift Kubernetes 引擎，并检索部署所需的所有集群配置。

一旦您配置了 OpenShift 云提供商，您就可以基于您的云提供商帐户中已经存在的 OpenShift Kubernetes 集群创建**环境**

### 3.创建新的部署工作流

设置>应用程序>创建工作流

要将**服务**部署到**环境**中，您需要**工作流。**管理工作流预配置了几种发布策略(蓝/绿、淡黄色、滚动、...)并且可以是动态的，以便您可以为您的部署逻辑参数化所有输入。可以为许多服务和环境提供简单的部署工作流模板，而不是为每个服务/环境组合都需要一个工作流。

下面是一些截图，展示了从头开始创建 canary 部署是多么简单。

在部署了你的容器应用程序之后，你可以在一个工作流中选择你的验证策略。这是我们的无监督机器学习分析来自应用性能监控(APM)和日志分析工具的时间序列指标和非结构化数据的地方。

### 4.创建新管道

设置>应用程序>创建管道

最后，您可以将您的部署工作流附加到给定的**管道**。大多数 Harness 客户的每个应用都有一个管道，其中有几个阶段代表了[环境](https://harness.io/blog/deployment-environments/)(开发、QA、试运行、生产等。)他们的服务工件必须被提升。

下面是一个简单的 4 阶段管道的例子，展示了 Harness 如何在开发、QA 和生产环境之间通过人工批准来提升代码:

### 5.创建新触发器

设置>应用程序>创建触发器

最后，为了执行我们的 OpenShift **管道，**我们可以创建一个**触发器**，它将在给定的条件或事件下触发(例如，新的构建、一天中的时间、webhook)。

下面的 Webhook **触发器**将执行我的 OpenShift 管道。

web hook 的细节由线束自动生成:

如果您想要参数化任何触发器或管道输入，Harness 还提供 Curl 命令。
就是这样！以上过程应该不超过 5-10 分钟。

今天就报名参加线束连续交付的[免费试用](https://app.harness.io/auth/#/signup/?module=cd)。

干杯，
史蒂夫。