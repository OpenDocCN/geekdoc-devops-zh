# 数周内实现按需部署|利用

> 原文：<https://www.harness.io/blog/openbank-on-demand-deployments>

了解 OpenBank(桑坦德银行的数字银行)如何在 AWS 云迁移期间支持工程师按需部署

[Openbank](https://www.openbank.es/en),[桑坦德集团](https://www.santanderbank.com/us/personal)的数字银行，拥有业内最完整、最灵活、最敏捷的数字银行平台之一。它是世界上首批使用基于云的 It 基础设施的银行之一，并提供全套产品(储蓄、贷款、投资……)，以及定制的全年全天候客户服务。

Openbank 是一个现代的云原生微服务和无服务器应用程序，运行在 Amazon Web Services (AWS)上。Openbank 团队发现，在数十个微服务、开发团队和云环境中扩展他们现有的 CI/CD 平台变得过于复杂和难以管理。

## 线束前的 CI/CD

像许多组织一样，Openbank 通过扩展他们的[持续集成(CI)平台](https://harness.io/platform/continuous-integration/)，历史性地建立了他们自己的[持续交付(CD)管道](https://harness.io/blog/ci-cd-pipeline//)。随着他们的微服务和 lambda 架构的增长，管理这些复杂的管道依赖变得脆弱且难以管理。

具体来说，缺乏对部署期间和部署后的部署依赖关系、状态和运行状况的了解变得非常具有挑战性。“我们的詹金斯管道工作太难管理和部署了，”架构和 Openbank 负责人 Javier Ros 说。"我们不相信詹金斯会永远做正确的事。"

Openbank 努力追求卓越的技术，利用我们在软件交付方面的优势。

> Javier Ros |建筑主管| Openbank

传统上，部署由运营部门集中管理，需要 3 到 15 名工程师花费 4 到 6 个小时。部署验证和运行状况检查可能需要 2-3 个小时，具体取决于服务。回滚也是手动的，可能需要几个小时来检索和同步正确的工件、配置和环境设置。

## 通过自助式光盘增强开发人员的能力

Openbank 的 CI/CD 愿景是实现部署民主化，并通过自助式持续交付平台[增强其开发团队](https://harness.io/blog/continuous-delivery/scaling-continuous-delivery-in-your-org/)的能力。
Javier 和他的团队负责寻找创新的解决方案，以实现从运营到开发的“左移”。就在这个时候，哈维尔偶然发现了马良。

Javier 对自助式连续交付的要求如下:

*   “按钮”和事件驱动(如 webhook)部署管道
*   提供与其 AWS 云堆栈(EC2、ECS、Lambda、CodeDeploy、KMS 和 CloudFormation)的深度集成
*   提供跨所有应用、微服务和部署的实时洞察(单一控制台)
*   通过金丝雀部署减少生产事故并提高代码质量
*   使用现有的监控堆栈 New Relic、Splunk 和 CloudWatch 自动验证部署
*   提供部署和云抽象，以便在数小时内完成云迁移

Harness 允许我们的开发人员在不与运营部门沟通的情况下部署他们自己

> Javier Ros |建筑主管| OpenBank

Openbank 团队在最初的演示后不到 24 小时就开始了对 Harness 的评估，并在几天内开始运行。

## 将部署时间和工作量减少多达 95%

在实施 Harness 的几周内，Openbank 团队使用 Harness 中的全自动 canary 部署工作流，将平均部署时间减少了 75%,从 4-6 小时减少到 1 小时。

现在，开发人员可以在不到 1 小时的时间内部署他们自己的代码，而不是由 3 到 15 名运营工程师照看部署。根据服务和团队的不同，生产率提高了 66%到 95%。开发团队现在可以通过真正的自助服务功能自行完成日常部署。

部署验证和回滚现在完全自动化，使用线束'[连续验证，](https://harness.io/platform/continuous-delivery/continuous-verification/)直接与 New Relic、Splunk 和 AWS CloudWatch 集成。回滚时间减少了 96%，从 2-3 小时减少到 5-10 分钟，具体取决于服务。

对 Openbank 来说，最终结果是他们可以提供卓越的客户体验。Openbank 不仅每天向客户提供创新，而且在任何部署失败的情况下，他们都以最小的停机时间或影响来实现这一目标。

想试驾一下 Harness 吗？[注册免费试用](https://harness.io/try-continuous-delivery-as-a-service-for-free/)。