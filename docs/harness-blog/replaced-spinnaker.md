# 更换 Spinnaker 并节省 275，000 美元|线束

> 原文：<https://www.harness.io/blog/replaced-spinnaker>

## 关于 MakerBot

MakerBot 相信每个人都有一个创新者。作为 3D 打印的全球领导者，它通过为 3D 打印过程的每个阶段提供有效的解决方案，树立了可靠性和易用性的标准。从一开始，它就为所有行业的用户重新定义了 3D 打印的可能性。

## 开源 CI/CD 困境

MakerBot 的软件交付过程让 Erik Ahrend 夜不能寐。作为首席云架构师，Erik 维护 MakerBot 的 Spinnaker 管道。Erik 每天花三个小时来解决部署问题，并且经常在半夜被叫醒来修复损坏的部署。Erik 每年花费的时间总计约为 118，000 美元。

Spinnaker pipelines 和 API 创建和编辑所需的高级知识。Erik 是唯一一个有足够经验解决部署问题的人。MakerBot 雇佣 Armory 来帮助管理他们的管道。不幸的是，MakerBot 的部署问题依然存在。

部署在上传映像和投入生产之间有一个无法解释的 48 小时延迟。大多数开发人员发现手动更新 Helm 比等待 Spinnaker 更容易。Erik 需要修正和简化 MakerBot 的软件交付过程。

我们希望降低复杂性，增加开发人员的幸福感

> Erik Ahrend |首席云架构师|MakerBot

## 自定义治理的成本

除了简化部署，Erik 还需要增加功能。管道缺乏与 APM 工具(如 DataDog)的基本集成，这减慢了验证过程。管道也缺乏管理。没有 RBAC 控制或审计跟踪，Eric 担心 MakerBot 应用程序的安全性。

Erik 计划花 6 个月的时间编写这两个特性，并将它们添加到 MakerBot 的管道中。该项目至少需要两名开发人员，MakerBot 为此花费了 15 万美元。

意识到眼前的高昂成本，Erik 决定寻找一家能够提供开箱即用解决方案的公司。

为了实现我们的交付目标，我们需要指导和支持

> Erik Ahrend 首席云架构师|MakerBot

### **利用 CI/CD 提供无价的信心**

埃里克求助于[马具](https://harness.io/platform/)寻求解决方案。

我们的开发人员应该得到最好的工具，利用这一点。

> Erik Ahrend |首席云架构师| MakerBot

Harness 创造了一种自助式部署文化。开发人员能够将 Harness 的管道作为代码复制到他们需要的任何新管道中，并且 Erik 不必每天花 3 个小时照看部署。Harness 还为 MakerBot 提供了先进的 RBAC 控制、数据狗和吉拉集成，以及模板化的金丝雀部署。这些功能减少了 MakerBot 的安全顾虑，增加了部署信心。

使用 Harness 仅 4 个月，Makerbot 每天部署约 15 次，故障率非常低，仅为 8%。最重要的是，Harness 能够为 Erik 提供一些急需的空间。

我不必担心部署，它们就这样发生了。

> Erik Ahrend |首席云架构师| MakerBot