# 使用 DevSecOps 监控管道安全性

> 原文：<https://circleci.com/blog/monitoring-pipeline-security-with-devsecops/>

这个博客是为那些已经实施了安全最佳实践，并且正在寻找如何继续监控安全的进一步信息的团队而写的。如果您正在寻找有关如何实施自动化安全实践的信息，请阅读我们的电子书《漏洞管理和开发与 CI/CD 合作》。

## DevSecOps 继续生产

如果您的团队已经将安全实践作为您的默认开发过程的一部分，那么保持监控是至关重要的。今天非常安全的代码明天可能会包含已知的安全漏洞。监控已经运行的软件，以及正在开发的代码。

你可以使用类似于 [Splunk](https://www.splunk.com/) 或者 [Prisma Cloud](https://www.paloaltonetworks.com/prisma/cloud) 这样的工具来完成这项工作。以所有相关方都能理解的格式自动生成您的报告。像 [Honeybadger](https://www.honeybadger.io/) 、 [Honeycomb](https://www.honeycomb.io/) 或 [LogDNA](https://logdna.com/) 这样的监控和日志工具可以提供很大的帮助——并且有 CircleCI orbs 可以让你快速地将它们集成到你的管道中。

当您在云环境中托管时，请确保检查该环境的监控工具。Azure 有应用洞察，AWS 有 CloudWatch 应用洞察。好好利用它们。它们可以跟踪恶意登录尝试、未经授权的访问以及来自应用程序的错误。

第三方工具通常通过使开始变得更容易、使其他团队可以访问监控、生成报告以及监控其他指标来增加价值。

## 修补软件

当您的工具报告漏洞时，尽快修补您的软件非常重要。更新可能会破坏软件，这包括来自开源项目和第三方供应商的更新。

为了限制打补丁的任何风险，请确保在打补丁过程中遵循合理的开发实践，包括 DevOps 原则，如[自动化单元测试和集成测试](https://circleci.com/resources/software-testing-devops-teams/)。尤其是集成测试，可以让您充满信心地修补软件，确信修复不会导致额外的问题。

自动化集成测试也将大大减少发布补丁的人力。如果您在团队之间共享标准化资产，您将确保所有团队都获得更新。

## 下一关:CircleCI 球体

如果你使用 CircleCI 作为你的 CI/CD 管道，你应该考虑使用 [CircleCI orbs](https://circleci.com/orbs/) 。orb 是 CircleCI 配置的可重用、可共享的开源包，支持许多第三方服务的即时集成，包括扫描仪服务等有价值的安全工具。

CircleCI 提供了许多漏洞扫描 orb，可以轻松地将漏洞扫描集成到您的管道中，只需花费最少的设置时间。借助 orbs，您可以获得一个保护管道的开箱即用的解决方案。

您将找到我们已经提到的工具的扫描器，如 Alcide、Snyk 和 Stackhawk，以及更多的扫描器，如:

*   [锚点](https://circleci.com/developer/orbs/orb/anchore/anchore-engine/)(用于图像)
*   [AWS 参数存储](https://circleci.com/developer/orbs/orb/anchore/anchore-engine)(用于管理和加载环境机密)
*   Checkmarx (用于静态和交互式应用程序安全测试)
*   [可能是](https://circleci.com/developer/orbs/orb/probely/security-scan)(用于扫描您的 web 应用程序的漏洞)
*   [秘密中枢](https://circleci.com/developer/orbs/orb/secrethub/cli)(为应用程序提供密码和密钥)
*   [SonarCloud](https://circleci.com/developer/orbs/orb/sonarsource/sonarcloud) (用于连续的代码质量扫描)

如果你想使用一个还没有 orb 的[安全扫描器](https://circleci.com/docs/orb-author/),你可以创建一个并把它推到开源的 CircleCI Orb 注册表中，贡献给社区。

## 与 DevSecOps 一起前进

将安全意识纳入 DevOps 实践的 DevSecOps 方法提供了一种利用 CI/CD 将漏洞扫描和管理添加到现有部署管道的战略方法。

随着时间的推移，您可以通过首先引入基本扫描来让开发团队习惯于 DevSecOps，然后随着时间的推移增加您扫描的漏洞的数量和类型来建立这一点。

漏洞管理只是 CI/CD 作为开发团队力量倍增器的一个领域。构建弹性系统允许团队以更少的时间和更低的风险交付高质量的代码。通过让您的 CI 渠道为您服务，您可以获得公司的关键优势和杠杆点。如果您准备好尝试一下，您可以在 [CircleCI](https://circleci.com/) 上创建一个完整的 DevSecOps 管道。

只是想了解更多关于安全的信息？阅读我们的电子书[漏洞管理和使用 CI/CD 的开发工具](https://circleci.com/resources/devsecops-ebook/)。