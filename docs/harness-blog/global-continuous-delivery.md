# 利用线束实现全球连续交付|线束

> 原文：<https://www.harness.io/blog/global-continuous-delivery>

Linedata 节省了 50 万美元的开发成本。找出你也能做到的方法。

## 关于 Linedata

[Linedata](https://www.linedata.com/) 在 20 个办事处的 1300 名员工为资产管理和信贷行业提供全球人性化的技术解决方案和服务。Linedata 总部位于法国，2017 年收入为 1.79 亿欧元。Linedata 资产管理 AMP 平台内的软件交付涵盖 4 个业务团队，约有 50 名开发人员/QA 工程师，分布在全球 5 个地区。

## 通过微服务进行新一代资产管理

Linedata 正在转变为基于的模块化下一代资产管理平台(AMP)。运行在公共云中的 NET core 微服务。作为这一转变的一部分，内部连续交付(CD)计划被创建来提高开发速度，加强开发和 QA 之间的反馈循环，并缩短上市时间。

## 部署脚本无法扩展

过去，部署是由 QA 工程师使用 Terraform 和定制 bash 脚本手动执行的。每个环境的典型部署需要 1 个小时。此外，部署验证还需要 3-4 名 QA 工程师花费 2 小时(分钟)在世界各地进行手动移交/批准，因为在每个环境中都推广了候选版本。鉴于团队之间的地理位置和时区差异，这种手动部署过程意味着开发人员和 QA 工程师之间的反馈循环可能需要几天时间。

在公共云中迁移到微服务架构意味着 Linedata 当前的部署流程无法扩展。工程和 QA 需要一种新的方法来实现[连续交付](https://harness.io/platform/continuous-delivery/)，于是自助式光盘计划诞生了。

## 通过自助 CD 增强团队能力

Linedata 的 DevOps 总监 Andrey Budzar 表示:“我们希望为我们的工程和业务团队提供自助式持续交付。

“加强工程和业务之间的反馈回路将为我们的客户提供最大价值，”Andrey 说。

Linedata 的愿景是从他们的持续集成(CI)工具 TeamCity 触发和自动化部署管道。开发人员可以使用 CI webhooks 来启动一个自动化的管道，在他们的开发、QA、试运行和生产环境中推广、测试和验证新的构建。

我们希望为我们的工程和业务团队提供自助服务

> Andrey Budzar |总监 DevOps | Linedata

## 利用持续交付即服务

在实施 Harness 的几周内，Andrey 和他的团队成功地为开发、QA 和业务推出了一个新的自助式 CD 平台。“我们在实施的几周内就实现了切实的利益，”Andrey 说。

如今，Linedata 在其所有环境中拥有[自动部署管道](https://harness.io/platform/continuous-delivery/smart-automation/)，以及[自动验证](https://harness.io/platform/continuous-delivery/continuous-verification/)来测试新版本和发布候选版本。Harness 现已成功集成到 Linedata 的以下技术体系中:

*   码头工人。网络核心微服务。NET 4。X
*   亚马逊网络服务——EC2、ECS 和 Fargate
*   CI 团队精神
*   Atlassian 吉拉管道票务和审批
*   用于自动化测试的 Resharper、xUnit、WhiteHat、Selenium 和 LoadRunner

我们在实施的几周内就实现了切实的收益

> Andrey Budzar |总监 DevOps | Linedata

## 开发运维成本节省 50 万美元

借助 Harness，Linedata 通过脚本和开发运维工程师团队手动扩展连续交付，消除了产生 50 万美元开发运维成本的需求。跨 5 个地理位置的工程、业务和运营团队现在能够自助连续交付，最大限度地实现协作并提供卓越的业务价值。

Linedata 还发现部署时间减少了 80%，验证/测试时间减少了 88%。

最终结果是加快了开发和业务的反馈循环，缩短了上市时间，并为 Linedata 的客户提供了更好的产品体验。