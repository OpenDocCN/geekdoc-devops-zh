# 加速 AWS 云迁移|利用

> 原文：<https://www.harness.io/blog/aws-cloud-migration>

Beachbody 通过利用持续交付避免了与云迁移相关的难题。

## 关于海滩

Beachbody 是一家领先的健身、营养和减肥项目提供商，拥有超过 2300 万客户和 10 亿美元的总销售额。Beachbody 是美国最受欢迎的健身和减肥解决方案的创造者，包括 [P90X 系列](https://www.beachbody.com/product/fitness_programs/p90x.do)、疯狂、FOCUS T25、21 天修复、Body Beast、PiYo 和 Hip Hop Abs。

## 迁移到 AWS、Kubernetes & Lambda

在过去的一年里，Beachbody 已经将他们的许多内部应用重构为运行在亚马逊网络服务(AWS)上的微服务和 T2 功能。2019 年，另有 90 个 API 服务计划加入并迁移到公共云。

## Jenkins 管道-管理复杂且成本高昂

过去，Beachbody 的开发人员和开发工程师会使用詹金斯管道来部署服务。随着时间的推移，这一过程变得极其困难，需要花费大量的基础架构成本和时间来升级 Jenkins 平台并对其进行故障排除。“我们的詹金斯管道是手动的，脆弱，管理成本高，”Beachbody 的技术运营、开发和 SRE 高级总监陈方灿说。Bob 说:“Jenkins 的新云原生服务可能需要我们的 DevOps 团队 3 天时间。【Jenkins 的部署速度在非生产环境中是每天一次，在生产环境中是每两周一次。生产部署通常由 6-8 个人平均进行大约 2 小时，使用 [Dynatrace APM](https://www.dynatrace.com/) 和 [Elastic](https://www.elastic.co/) 堆栈进行手动验证(运行状况检查)需要 30-60 分钟。Bob 和他的团队也研究了 Spinnaker 等开源解决方案，但估计需要几个月的时间来设置，并且需要 2-3 名全职工程师来长期管理。

我们的 Jenkins 管道是手动的，易损坏，管理成本高。

> 陈方灿| devo PS | Beachbody 的高级总监

## 利用线束实现云原生连续交付

今天， [Har](https://harness.io/) n [ess](https://harness.io/) 帮助 Beachbody 在大约 100 个云原生服务及其整个 DevOps 工具链中实现软件交付自动化:

*   GitHub、Jenkins 和 TravisCI 负责[持续集成(CI)](https://harness.io/blog/what-is-continuous-integration/)
*   用于云基础设施、容器编排和无服务器的 AWS、Terraform、Kubernetes 和 Lambda
*   用于监控的 Dynatrace APM 和弹性堆栈

开发人员拥有自助服务[连续交付(CD)](https://harness.io/platform/continuous-delivery/) 功能，可以抽象所有云原生服务、工件、工具和技术。使用 Harness GitOps 将管道作为代码进行模板化、版本控制和管理。HashiCorp Terraform 脚本也集成到每个管道阶段，因此环境是一致的，并在需要时动态供应/停用。Bob 说:“新的 Kubernetes 和 Lambda 服务现在只需 2 小时，而不是 3 天。此外，Harness 与 Dynatrace APM 和 Elastic stack 集成，开箱即用，使用无监督的机器学习来自动验证部署，以检测任何时序性能指标或日志事件中的异常/回归。“有了 Harness，我们的 QA 和测试现在完全自动化，只需不到 5 分钟，”Bob 说。因此，Beachbody 已经能够端到端地自动化其部署管道，因此环境、测试、部署和验证可以在几分钟内完成，而不是几小时或几天。

借助 Harness，我们可以使用无监督的机器学习在 Dynatrace 和 Elastic 之间实现自动化部署验证。

> 陈方灿| devo PS | Beachbody 的高级总监

## 确保安全性和合规性

在 Beachbody 为工程师提供自助部署功能需要关注安全性和合规性。工程师通过 Harness OKTA SSO 集成进行身份验证，并通过定义明确的[基于角色的访问控制(RBAC)](https://harness.io/blog/product-updates/harness-delivers-rbac-for-continuous-delivery/) 系统进行授权/限制，该系统提供了跨服务、环境和团队的职责和权限分离。Harness 还提供了一个[完整的审计跟踪](https://harness.io/blog/continuous-delivery/harness-audit-trails/)，让用户可以在服务受到影响时即时了解谁更改了什么。这使得鲍勃和他的团队可以在几秒钟内确定撞击的原因。此外，Harness 保留了跨所有环境的所有服务版本的记录，因此团队可以报告哪些软件工件被部署到特定的集群。

## 将部署时间和工作量减少 90%

Beachbody 从实施线束持续交付中看到了以下业务优势和投资回报:

*   **加速 AWS 云迁移**从 1 年缩短到 3 个月
*   **3 个月内生产部署速度翻倍**
*   **QA 测试时间减少 92%** 从大约 1 小时减少到不到 5 分钟
*   **部署工作量减少 90%** 从 7 人 90 分钟减少到 3 人 20 分钟
*   **部署验证时间减少 78%** 从 45 分钟减少到 10 分钟
*   **将申请入职时间缩短 92%** 从 3 天缩短至 2 小时
*   **由于不支持 OSS CD 解决方案，延期开发运维成本为 600，000 美元**