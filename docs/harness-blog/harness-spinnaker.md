# 为 CI/CD |线束选择 Spinnaker 上的线束

> 原文：<https://www.harness.io/blog/harness-spinnaker>

### **关于数据税**

[DataStax](https://www.datastax.com/) 是基于 Apache Cassandra 构建的高度可扩展、高度可用的云原生 NoSQL 数据平台的幕后公司。DataStax 让用户和企业可以自由地在全球范围内的任何云中运行数据，而不会出现停机和锁定。

### **开发人员自助服务功能分支**

由 Frank Moley 领导的云服务和基础设施团队负责为 DataStax Astra 工程师提供自助式功能分支部署能力，以加快速度和上市时间。

DataStax Astra 是一个建立在 Apache Cassandra 基础上的数据库即服务，旨在运行在任何地方、任何云上、任何数据中心以及任何可能的组合中。

我们需要一个解决方案，让我们能够专注于实现管道，而不是管理提供管道的平台。

> 弗兰克·莫利|工程经理|数据税

### **操作 CD 三角帆的挑战**

在过去的一年中，Frank 的团队在他们的特性分支计划之前，已经自托管和自管理了开源项目 Spinnaker。

“最终，管理 Spinnaker 的开销对团队来说变得不可行，”Frank 说。“我们需要一种解决方案，让我们能够专注于实施管道，而不是管理提供管道的平台。”

此外，在进行更改时，对失败的部署进行调试和故障排除通常需要 4-5 个小时。了解和信任特定集群和环境上运行的服务版本变得越来越困难。

DataStax CD 需求从基本的容器化 Kubernetes 部署转变为动态特性分支部署，工程师可以在 Kubernetes 集群中部署到他们自己的名称空间。

使用 Spinnaker，除了实现 canary 部署和自动回滚(估计需要几周的工作)之外，加入新的 Astra 团队/服务可能需要几个小时。

Spinnaker 不太适合我们关于特性分支的 CD 需求。

> 弗兰克·莫利|工程经理|数据税

### **即插即用带线束连续交货**

利用[持续交付即服务](https://harness.io/platform/continuous-delivery/)使 DataStax Astra 团队能够专注于实施管道，而不是管理提供这些管道的 CD 平台。

GitOps 和 GitSync 功能与管道模板相结合，意味着部署可以作为代码进行管理，具有可重复的功能分支流程和开发团队的快速入职。

与 [HashiCorp Terraform](https://harness.io/blog/product-updates/cloudformation-and-terraform-support/) 的本机集成意味着动态基础架构供应，以及 AWS secrets manager 与部署管道的无缝集成，意味着可以完全消除对 Jenkins 的依赖。

我们不需要对现有的 CI/CD 流程做任何更改，线束是即插即用的，开箱即用。

> 弗兰克·莫利|工程经理|数据税

如今，利用 Harness 对部署进行故障排除只需不到一分钟的时间，而且部署执行过程非常透明。

[审计跟踪](https://harness.io/blog/continuous-delivery/harness-audit-trails/)也意味着团队可以在几秒钟内了解并信任任何集群或环境上运行的服务版本。

Franks 团队还希望在几天内实施[金丝雀部署](https://harness.io/blog/blue-green-canary-deployment-strategies/)和自动回滚，以便他们可以在其环境中执行渐进式交付。

有了 Harness，我们能够减少 20-30%的工程工作量。

> 弗兰克·莫利|工程经理|数据税

### **跨 25-30 名工程师持续交付的最佳实践**

利用 Harness，Frank 的团队取得了以下成果:

*   DataStax Astra 团队在 25-30 名工程师中实施了最佳实践连续交付
*   减少了与管理旧 CD 平台相关的工程工作量(20-30%的工程时间),这比支付线束成本还多
*   将新服务的启动时间从几个小时缩短到几分钟
*   将部署故障排除时间从 1 小时减少到 1 分钟以内，减少了 98%
*   将实施 canary 部署和自动回滚的时间从几周缩短到几天