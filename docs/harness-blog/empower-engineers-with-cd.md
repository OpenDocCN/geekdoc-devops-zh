# 通过 Harness | Harness 为 700 名工程师提供支持

> 原文：<https://www.harness.io/blog/empower-engineers-with-cd>

Advanced 在两个月内获得了 10 倍的投资回报。找出方法。

## 关于高级

[Advanced](https://www.oneadvanced.com/) 是英国第三大商业软件和服务提供商，营业额为 2 . 54 亿英镑，拥有 16，000 名客户和 2，200 名员工。Advanced 提供以企业和市场为中心的解决方案，让客户重新想象什么是可能的，在他们的领域进行创新，并改善英国数百万人的生活。

## AWS 迁移–CD 的触发器

与许多其他组织一样，Advanced 正在将其传统的内部部署应用程序迁移到公共云，更具体地说是 AWS。拥有 100 多个不同的产品/团队和大约 700 名软件工程师，今年早些时候启动了一项内部 CI/CD 计划，为所有开发团队提供一个自助式持续交付平台，帮助他们提高速度和缩短上市时间。

## 临时连续交付无法扩展

在过去，Advanced 的典型部署管道需要几个月的时间，使用 Jenkins、AWS CloudFormation、Octopus Deploy、Puppet、SSM 和 Bash 脚本的组合来构建。“管道需要 DevOps 工程师和团队维护几千行傀儡代码，”Advanced 的开发和 DevOps 经理 Martin Reynolds 说。

此外，这些管道可能需要 3 到 30 个小时才能完成，并且需要许多人工干预。部署运行状况检查和验证也是手动的，从 2 名工程师 2 小时的小版本到 4-5 名工程师 1 天的大版本。

“很难通过特设管道实现连续输送规模，”Martin 说。“那时，我们决定在内部启动一项新的 CI/CD 计划，目标是为所有开发人员提供持续交付自助服务。”

正是在这一点上，马丁斯团队评估了[线束](https://harness.io/)平台。

## 带线束的 CD 即服务

如今，Harness 帮助 100 多个产品团队、700 多名软件工程师、6 个数据中心和多个技术堆栈/工具实现高级自动化软件交付，包括:

*   面向公共云的 AWS EC2、ECS、EKS 和 Lambda
*   用于持续集成(CI)的 Jenkins、GitHub 和 JFrog Artifactory
*   用于监控的 AppDynamics、CloudWatch 和 Elastic/ELK 堆栈

“詹金斯构建它，Harness 部署它，”马丁说，“Harness 使我们的部署变得容易。”

詹金斯建造它，线束部署它。Harness 使我们的部署变得容易。

> Martin Reynolds | DevOps 经理|高级

Harness 为高级软件工程师和 DevOps 团队提供了以下功能:

*   直观的[自助 CD 平台](https://harness.io/platform/continuous-delivery/)加速跨团队入职
*   [全自动金丝雀部署](https://harness.io/blog/build-canary-deployment/)降低推广风险
*   [部署的自动验证](https://harness.io/continuous-delivery/continuous-verification/)使用机器学习对来自 AppDynamics、CloudWatch 和 Elastic/ELK stack 的性能和质量数据进行基准测试
*   不符合性能/质量基准的部署的自动回滚
*   DevOps 对部署管道的模板化
*   开发团队重复使用管道模板

## 用马具升级詹金斯

Harness 不仅自动化了 Advanced 内部的软件交付，还帮助自动化了像 Jenkins 这样的工具的升级。

例如，Advanced 的 DevOps 团队每周会花 2-3 个小时用补丁和升级来升级 Jenkins。利用 Harness，这一过程是自动化的，只需 15-20 分钟，每周为 DevOps 工程师节省几个小时。

## 仅在 2 个月内获得 10 倍的投资回报

第一批 10 个应用程序团队和 100 个服务在短短 2 个月内就与 Harness 一起上线了，其中许多在第一天就启用了。

部署管道现在只需要 DevOps 工程师几个小时就可以创建/模板化，几乎不需要维护，而持续维护则需要几个月。开发人员为他们的特定服务和环境重用这些管道模板，而不必每次都重新创建轮子，因此减少了他们的工作和上市时间。

“我们看到了立竿见影的效果，”马丁说。“我们的一个教育团队发现，19 项服务的部署时间从 3 小时减少到 1 分钟；另一个团队的部署时间从 30 小时减少到不到 30 分钟。”
总体而言，Harness 帮助 Advanced 将平均部署时间从 2 天减少到 2 小时，减少了 88%,开发团队的入职时间从 2 天减少到 90 分钟，减少了 90%。“据保守估计，我们在部署和之前的 CI/CD 流程上节省了 50-60%的总开发和工程时间，”Martin 说。

据保守估计，我们在之前的 CI/CD 流程中节省了 50-60%的总开发和工程时间。

> Martin Reynolds | DevOps 经理|高级