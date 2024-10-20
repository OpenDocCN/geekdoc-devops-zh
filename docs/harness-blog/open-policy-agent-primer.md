# 开放策略代理初级|线束

> 原文：<https://www.harness.io/blog/open-policy-agent-primer>

保护您最新的 Kubernetes 船舶，开放政策代理简化跨服务授权。用线束安装不到 5 分钟。

随着容器和微服务的蓬勃发展，我们当然要处理越来越大的分布式系统。随着我们在冗余和性能 SLA 方面的努力，拓扑结构将继续增长。很快我们的一个终点变成了许多；即使是同样的服务。

身份验证和授权是应用程序的内在需求。不幸的是，所有人和所有事都不可信，所以我们想出了限制和控制访问的机制。

有时，在一个服务中对授权进行更改(比如出于调试目的)会要求部署先增加权限，然后再减少权限。

[开放策略代理](https://www.openpolicyagent.org/) (OPA)努力成为服务不可知的授权中心。如果没有像开放策略代理这样的机制，每次需要更改授权时，您都必须在微服务拓扑中部署多个服务；你当然有适合你的工作。

### 授权财源

当您谈论应用程序中的访问控制时，您通常是在谈论[认证和](https://medium.com/datadriveninvestor/authentication-vs-authorization-716fea914d55)授权的动态组合。开放策略代理关注授权部分。

认证关注的是你是谁，授权关注的是你能访问什么。来自 Styra 的 Torin Sandall 在 T2 做了很好的工作，他在去年的开源峰会上谈到授权需要改变多少次。

即使对于受信任的用户，例如工程师，也存在需要增加您在应用程序和应用程序基础架构中的访问权限的情况。调试/模仿用户是一个很好的例子，说明了授权的改变是如何累积的。不同的服务可能使用不同的授权提供者。当您看一看最小公分母时，开放策略代理非常适合必须做出的授权决策，从而集中这些决策。

### 输入 OPA

开放政策代理是 CNCF 在 CNCF 成熟的孵化项目。OPA 将策略决策与策略执行分离开来。OPA 有几个关键术语需要熟悉。第一个是您提供给 OPA 的数据，它在 JSON 中。数据是决策的基础。第二个是查询输入，也在 JSON 中。查询输入指导决策，例如需要做出什么决策。最后是[减压阀](https://www.openpolicyagent.org/docs/latest/#rego)中的策略，这是 OPA 的一种声明性语言。策略是需要执行的逻辑。

当执行基于 OPA 的决策时，您的调用系统或进程需要某种方法来解析答案。例如，如果 Jane Q Devops 有权访问员工薪金，作为响应列表的一部分，该薪金可以是“真/假”或“Jane Q Devops”。开始使用 OPA，一个很好的资源是[减压阀游乐场](https://play.openpolicyagent.org/)，在那里你可以快速模拟数据和政策。在 Kubernetes 集群上使用 OPA 是轻而易举的事情。

### 您的首次 OPA 安装

在本教程中，我们将安装 [OPA 的看门人](https://github.com/open-policy-agent/gatekeeper)，这是一个部署在 Kubernetes 上的策略控制器，目前每个项目需要 1.14+集群。喜欢总是可以跟随视频和/或博客帖子。

在 Kubernetes 集群中安装 Harness 委托的第一步非常简单。

设置->管理代理->下载代理+YAML Kubernetes

将代表命名为“opa”。

点击提交下载。展开 tar.gz。

在 delegate 文件夹中，运行 ku bectl apply-f harness-delegate . YAML 来安装代理。

几分钟后，您的马具代表就可以使用了。

连接 Kubernetes 集群很简单，尤其是在 Kubernetes 集群内部运行一个 Harness 委托。

设置->云提供商+添加云提供商

添加一个名为“opa_eks_cluster”的 Kubernetes 集群。请确保从选定的代理[opa]继承集群详细信息。

单击 Submit 后，您的 Kubernetes 集群就被添加了。

添加集群后，您可以部署可安装网关守护设备。

您需要链接 GateKeeper 项目的 GitHub 存储库。

设置->连接器->来源回购提供者+添加来源回购提供者

连接到项目[[https://github.com/open-policy-agent/gatekeeper](https://github.com/open-policy-agent/gatekeeper)]的 Git 存储库中。您需要输入您的 GitHub 凭据。

接下来，您将创建一个[线束应用程序](https://docs.harness.io/article/4o7oqwih6h/preview#applications)来完成这项工作。

设置-> +添加应用程序“OPA101”

添加应用程序后，Kubernetes 集群中的 wire 作为一个[线束环境](https://docs.harness.io/article/n39w05njjv-environment-configuration)。

设置-> OPA101 ->环境+添加环境

添加一个名为“opacluster”的环境。

添加一个基础设施定义，将您的 Kubernetes 集群与+ Add Infrastructure Definition 链接起来。

将部署类型定义为 Kubernetes[而不是 Helm]。目前，OPA 看门人掌舵图是一个需要舵柄的掌舵 V2 图。挽具能够[在不使用舵杆的情况下展开舵 V2 海图](https://docs.harness.io/article/2aaevhygep-helm-quickstart)。

基础设施定义完成后，是时候连接一个[线束服务](https://docs.harness.io/article/eb3kfl8uls-service-configuration)，这将是 OPA 安装。

设置-> OPA101 ->服务+添加服务

添加一个名为“opa_install”的新服务。

点击提交后，您可以链接到 OPA GateKeeper 存储库中的掌舵图。在服务中间，单击省略号链接远程清单。

可以链接“来自源库的舵图”的远程清单。[[https://github . com/open-policy-agent/gate keeper/tree/master/chart/gate keeper-operator](https://github.com/open-policy-agent/gatekeeper/tree/master/chart/gatekeeper-operator)。分支将是“主”，路径将是“图表/看门人-操作员”。

验证清单看起来不错。

接下来，基于“opa_install”服务创建一个[线束工作流](https://docs.harness.io/article/m220i1tnia-workflow-configuration)。

设置-> OPA101 ->工作流+添加工作流

创建名为“opa_install”的线束工作流，作为带有服务“opa_install”的滚动部署。

利用工作流设置，是时候部署 OPA 了。单击蓝色的部署按钮。

准备安装 OPA。

在你的 OPA 之旅中观察和繁荣。

Harness 平台通过一致的方式部署新的想法和方法，使得采用新技术变得更加容易。

## 驾驭:范式转变中的伙伴

科技唯一不变的是会有变化。开放策略代理代表了一种新的方法来解决越来越多的问题。Harness 平台是专为满怀信心地部署新方法而构建的。像 Harness 一样，作为开放政策代理领域的领导者，Styra 是一家由不寻常的风险投资支持的公司。两人都是各自领域的梦想家。今天就可以随意报名参加[装具试用](https://harness.io/try-continuous-delivery-as-a-service-for-free/)。我们也希望听到您在[线束社区](https://community.harness.io/)中关于 OPA 的工作。

干杯！

拉维