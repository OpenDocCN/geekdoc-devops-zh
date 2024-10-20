# 选择 Spinnaker 上的线束，以确保连续交付|线束

> 原文：<https://www.harness.io/blog/secure-continuous-delivery>

相对于 Spinnaker，Relativity 选择了 Harness，从而大幅提高了安全性。

## 关于相对论

[相对论](https://www.relativity.com/)制作软件，帮助用户组织数据，发现真相，并据此采取行动。他们的电子发现平台被全球数千家组织用来管理大量数据，并在诉讼、内部调查和法规遵从性项目中快速发现关键问题。

## 通过 Azure 微服务加快开发速度

Relativity 首席系统工程师 Corey Wagehoft 领导了一项内部计划，旨在通过从传统的遗留 windows 服务迁移到在 Microsoft Azure 中运行的基于容器的微服务来降低成本和提高开发速度。

Corey 说:“过去的部署通常需要 8 个小时，并且由 3 名工程师组成的团队手动协调。“此外，我们有 50%的变更失败率，这降低了内部信心并导致停机。”

过去，部署需要 8 个小时和 3 名工程师。

> Corey Wagehoft |首席系统工程师|相对论

Corey 知道 Relativity 需要转变其部署渠道，以便简化 Azure 云中微服务的部署。他的团队还需要在整个迁移过程中支持传统 Windows 服务的方法。

管理用户数据意味着 Corey 和他的团队有严格的安全要求，这限制了可以搭载的工具类型。“我们有一种在内部开发自己工具的文化倾向，”Corey 说。

## 评估三角帆和线束

Corey 的团队首先研究了 Spinnaker，但很快发现开源解决方案不适合他们的需求。

“为了获得 Spinnaker 的部分功能，我们需要一名工程师为该项目工作一整年，”Corey 说。

我们需要一名专门的工程师来完成 Spinnaker 的部分功能。

> Corey Wagehoft |首席系统工程师|相对论

Relativity 在其整个工具栈中使用 Linux 和容器，而 Spinnaker 缺乏原生的微软集成。Corey 还确定 Spinnaker 需要进行大量调整，以符合相对论的安全标准。

经过短暂的评估，Corey 的团队选择了[Harness](https://harness.io/)CD as a-Service，原因如下:

*   除了 Azure 微服务之外，还能够支持原生 Windows 服务
*   与 Azure Key Vault 进行本机集成，用于机密管理
*   通过线束委托架构实现安全通信
*   CD 即服务与需要专用工程资源的自我管理解决方案

## 使用安全带确保连续交付

仅在 3 个月内，Corey 和他的团队就能够使用 Harness 将部署速度提高 30 倍。

Harness 与 Relativity 的整个工具链集成，包括用于 CI 的 Jenkins、用于基础架构配置的 HashiCorp Terraform、用于 APM 的 New Relic、用于日志分析的 Splunk 和用于基础架构监控的 Prometheus。

我们花在部署上的努力完全白费了。脊甲展开的时候我可以休息一下。

> Corey Wagehoft |首席系统工程师|相对论

他的团队还利用 Harness Pipeline 模板创建黄金标准，以便开发团队可以以可重复的方式一致地部署，无论工件是 Windows 服务还是 Azure 微服务。

Harness 与 Azure Key Vault 的本机[集成(在 Harness 试用期间相对论提出的一项功能要求)使相对论能够保持 ISO 和 SOC 2 合规性，同时也为团队获得 FED ramp 认证做好了准备。](https://developer.harness.io/docs/platform/Security/azure-key-vault)

最后，团队利用[持续洞察](https://harness.io/platform/continuous-delivery/continuous-insights/)帮助他们围绕管道流程、部署速度、变更失败率和 MTTR 做出数据驱动的决策。

借助 Harness，我们的工程团队利用自助 CD 成功地将部署时间从 8 小时缩短到了几分钟。

> Corey Wagehoft |首席系统工程师|相对论

## 变更失败率降低 80%

利用 Harness，Corey 的团队取得了以下成果:

*   将部署时间缩短 98%，从 8 小时缩短到几分钟
*   将部署工作量减少 98%,从 3 名全职工程师 8 小时的工作量减少到 1 名工程师几分钟的工作量。
*   将变更失败率降低 80%，从 50%降至 10%。
*   节省了 150，000 美元，而不是需要专门的工程资源来管理 Spinnaker