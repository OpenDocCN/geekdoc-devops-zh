# Drops Jenkins -每年节省 18 万美元，部署速度提高 3 倍|线束

> 原文：<https://www.harness.io/blog/drop-jenkins-deploy-faster>

BetterCloud 是一个领先的 SaaS 管理平台，它淘汰了 Jenkins，每年节省 18 万美元。了解他们是如何利用马具做到这一点的。

## **关于**

BetterCloud 是领先的 SaaS 管理平台(SMP ),使 IT 专业人员能够发现、管理和保护数字工作场所中不断增长的 SaaS 应用堆栈。随着 SaaS 集成生态系统的不断扩大，Zoom、沃尔玛和 Square 等数千家前瞻性组织现在依靠 BetterCloud 来自动化其云应用程序组合的流程和策略。

## **高成本的精英性能**

BetterCloud 使用 Jenkins 和定制脚本构建了一个复杂的软件部署解决方案。他们每天近 25 次向 Kubernetes 部署 300 个微服务。通俗地说，BetterCloud 实践了真正的持续软件交付。但是代价是什么呢？

泰勒·多尔蒂(Taylor Daugherty)负责监督 BetterCloud 的交付管道团队。随着 Taylor 的团队加入更多微服务，他们遇到了可扩展性问题。Jenkins 要求 Taylor 和另一名团队成员将 100%的时间用于故障排除和维护。这相当于每年大约 18 万美元的维护工作。泰勒开始质疑他的团队如何处理更多的微服务，因为他们已经达到了最大容量。

“我们的 Jenkins 管道由数千行 Groovy 脚本组成，我们 100%的时间都在维护它们。”

> Taylor Daugherty |现场可靠性工程经理| BetterCloud

每次需要加入新的服务时，Jenkins 脚本都需要更新。有时，Taylor 不得不请求其他团队的资源介入并帮助满足需求。

“有时我们不得不把产品工程师从他们的工作中抽出来，帮助我们解决管道问题”

> Taylor Daugherty |现场可靠性工程经理| BetterCloud

随着泰勒探究詹金斯的功能，其他裂缝开始出现。

每个软件部署都需要启动一个新的 Jenkins 代理，这导致每次部署增加 30 秒。Jenkins 管道只能执行简单的滚动部署，这意味着 BetterCloud 无法在生产中安全地测试新功能。对管道的任何更改都需要预定的生产部署冻结，每次都会中断大约 15 次部署。

必须有一个成本更低的解决方案。

## **没有压力的精英表现**

BetterCloud 转向利用增加部署速度，而无需维护工作。

Harness 帮助将生产部署数量增加到每天 80 个，同时将维护和故障排除工作量减少到原来的 10%，从每周 80 小时减少到 8 小时。这相当于部署速度提高了 3 倍，每年维护成本降低了 18 万美元。

Harness 还提供了现成的部署策略，如 canary deployment，BetterCloud 使用它来安全地增加推向生产的功能数量。Canary 部署允许用户以安全的方式测试和迭代，方法是缓慢地向用户群的子集发布变更。

“Harness 帮助我们测试新功能，并提高了我们的创新水平。”

> Taylor Daugherty |现场可靠性工程经理| BetterCloud

新员工可以轻松使用所有这些新功能。例如，站点可靠性工程师 Wesley Williams 在入职一周之后，就可以放心地部署使用了。用户大约需要一个月的时间来熟悉 Jenkins 管道。根据韦斯利的说法，Harness 正在彻底改变他的工作方式。

“拥有可重复、快速且稳定的部署渠道将帮助我们超越竞争对手”

> Wesley Williams |现场可靠性工程师| BetterCloud

Harness 正在帮助 BetterCloud 更快地向客户交付更好的代码。注册免费试用 Harness，看看连续交付有多快多容易。