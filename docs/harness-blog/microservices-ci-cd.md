# Masters 微服务 CI/CD |线束

> 原文：<https://www.harness.io/blog/microservices-ci-cd>

了解 GoSpotCheck 如何使用 Harness 创建真正的连续输送管道。

## 关于 GoSpotCheck

[GoSpotCheck](https://www.gospotcheck.com/) 的实时执行管理平台使现场团队除了完成现场任务之外，还能够收集营销、销售和合规数据。

## 线束前的 CI/CD

过去，部署管道使用 CircleCI 编写脚本，以支持 GoSpotCheck 的许多 AWS ECS 微服务。

因此，这些管道非常复杂、有限，而且管理起来非常耗时，需要一名工程师在持续的支持下花费数周时间来创建。此外，管道不是不可知的，也不是跨云、栈和平台可移植的。

总体部署用户体验也很痛苦，对管道状态的了解有限，难以进行故障排除和调试。YAML 配置的历史也意味着管道变得脆弱；一个小小的打字错误就能让管道停止工作！

部署验证和运行状况检查由 QA 团队使用生产中的基本冒烟测试手动执行。根据微服务团队的不同，回滚可能需要 30 到 60 分钟。

我们的部署渠道非常复杂，并且不是云不可知的。

> 尼克·威尔逊| DevOps 领导| GoSpotCheck

## 引人注目的事件

随着 GoSpotTeam 变得更加云原生，利用 Go、containers 和 Kubernetes 等微服务技术，现有的部署流程变得过于复杂，难以管理。此外，它们缺乏跨不同云提供商的可移植性。就在这个时候，GoSpotTeam 决定评估线束[连续交付平台](https://harness.io/platform/continuous-delivery/)。

## 集成线束

线束成功集成了 GoSpotCheck 的技术和工具，包括:

*   去吧，码头工人和库伯内特斯
*   Heroku、谷歌云平台(GCP)和亚马逊网络服务(AWS)的多云
*   CI 的 CircleCI
*   用于基础设施供应的 HashiCorp Terraform
*   新遗迹，相扑逻辑，普罗米修斯进行监测和验证

## 今日 CI/CD

如今，使用 Harness，GoSpotCheck 能够在数小时内在云提供商和地区之间迁移多个应用程序(和微服务),而不会出现停机。

管道是不可知的、可移植的、动态的，只需要工程师或 DevOps 团队进行最少的维护。

现在创建部署管道需要几个小时，而以前的 CircleCI 工作流需要几天或几周。

[持续验证](https://harness.io/platform/continuous-delivery/continuous-verification/)和自动回滚使工程师不必了解服务配置和依赖关系，他们不需要担心何时或如何回滚。Harness 及其与 New Relic、Sumo Logic 和 Prometheus 的集成可以自动检测部署异常和回归。

开发团队可以轻松、实时地一起观察、管理和解决部署问题，并了解正在发生的事情。简而言之:Harness 为所有开发者提供了一个真正的[持续交付即服务](https://harness.io/platform/continuous-delivery/)平台。

线束实现了真正持续交付的承诺

> 尼克·威尔逊| DevOps 领导| GoSpotCheck

整个工程组织都接触到了 Harness，3-4 个敏捷团队每天都在积极地使用 Harness，这样他们就可以持续地交付成果。