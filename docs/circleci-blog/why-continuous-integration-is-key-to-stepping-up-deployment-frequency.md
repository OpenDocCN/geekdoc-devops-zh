# 为什么 CI 是提高部署频率的关键| CircleCI

> 原文：<https://circleci.com/blog/why-continuous-integration-is-key-to-stepping-up-deployment-frequency/>

在 [2009 年 O'Reilly Velocity 大会](https://www.youtube.com/watch?v=LdOe18KhtT4)上，当时担任 Flickr 工程经理的 John Allspaw 和 Paul Hammond 给[做了一个演讲](https://www.slideshare.net/jallspaw/10-deploys-per-day-dev-and-ops-cooperation-at-flickr)，讲述了让 Flickr 的 DevOps 组织每天部署 10 次以上的最佳实践。那个著名的演示引发了很多关于每日部署的最佳构建数量以及如何增加部署频率的讨论。

但是，正如我们在之前关于[工作流程交付周期](https://circleci.com/blog/continuous-integrations-impact-on-lead-time/)的帖子中解释的那样，数量和速度并不总是等同于质量。部署代码是必不可少的，事实上，高性能的 DevOps 团队比他们低性能的同行更频繁地部署代码。然而，没有神奇的部署数量可以保证你在一个高性能的团队中；部署的数量取决于您正在开发的软件。

*在这篇文章中学习如何[发现和监控团队成功的工程指标](https://circleci.com/blog/engineering-metrics/)。*

正如我们在最近的报告中所分享的，[CI 的数据驱动案例:实践中 3000 万个工作流揭示了什么关于 devo PS](https://circleci.com/resources/data-driven-ci/)，部署频率，以及其他关键指标，如交付周期和变更失败率，可以通过[持续集成](https://circleci.com/continuous-integration/)和持续交付(CI/CD)来改进。使用来自 CircleCI 的 3000 万个工作流的运行数据，我们发现这些数字支持来自 [2019 年 DevOps 报告](https://cloud.google.com/devops/state-of-devops/)等来源的关键 DevOPs 指标。在这篇文章中，四部分系列的第二部分，我们将深入探讨部署频率。

## 部署频率和随意部署能力之间的联系

部署频率表明有多少离散的工作单元正在通过开发管道。在我们的报告中，我们测量了团队开始工作流的频率。当您采用 CI/CD 时，您会得到自动化，它甚至允许自动部署复杂的代码库。这对提高部署频率至关重要。在自动化软件管道中，像补丁这样的东西几乎可以立即提供给用户，并像其他更新一样经过严格的测试。

部署频率的重要之处在于，它可以作为团队随意部署能力的指标。如果您的团队对其 CI 渠道有信心，并且能够自动化部署，他们就不必处理手动验证代码，这是更频繁部署的一大绊脚石。使用自动化，可以在每次提交时执行测试，失败的构建通知允许对失败的即时可见性。通过快速向您提供错误信息，您的 DevOps 团队可以更快地推出变更。

为了了解观察到的开发行为与行业标准相比如何，我们查看了 2019 年 6 月 1 日至 8 月 30 日期间观察到的超过 3000 万个工作流的 CircleCI 数据。工作流代表:

*   每天运行 160 万个作业
*   超过 40，000 个组织
*   超过 150，000 个项目

以下是我们的发现:

*   50%的开发运维组织每天在所有项目中启动六个工作流
*   在每个项目级别，50%的项目每天都有三个工作流启动
*   在第 95 百分位，这些数字上升到每个项目 74 个工作流，所有项目 250 个工作流

鉴于 Allspaw 和 Hammond 10 年前关于部署频率的陈述的高姿态，我们本应期望看到今天的许多团队以非常高的频率部署。相反，我们没有看到很多团队每天部署数十次或更多次。虽然这种行为确实存在，但它是例外，而不是规则。

## 从我们的数据中有什么收获？

尽管我们没有看到很多组织达到每天 10+部署的神奇数字，但我们可以观察 CI/CD 对部署频率的影响。根据 DevOps 2019 报告的[加速状态:](https://cloud.google.com/devops/state-of-devops/)

*   精英小组一天部署几次
*   高绩效团队部署在任何地方，每天一次到每周一次
*   低绩效团队每月部署一次，每六个月部署一次

这些发现与我们的运行数据非常吻合，数据显示 CircleCI 用户平均每天运行六个工作流。**由于使用 CircleCI 的 DevOps 团队可以在任何需要的时候进行部署，因此部署的数量成为组织选择的反映，而不一定是高性能的反映**。测试和集成代码的频率，以及团队在遇到错误时的恢复速度，是高性能 DevOps 团队比部署频率更好的指标。

## 为什么 CI 帮助您始终如一地构建最好的软件

我们的数据显示:

*   使用 CircleCI 的团队非常快:80%的工作流程不到 10 分钟就完成了
*   使用 CircleCI 的团队保持在流程中并保持工作进展:50%的恢复发生在不到一个小时内
*   CircleCI 上 25%的故障在 10 分钟内恢复
*   CircleCI 上 50%的故障可以一次恢复

虽然我们仍然在研究 CircleCI 是否能使团队变得高绩效——或者高绩效团队是否只是养成了选择 CircleCI 的习惯——我们可以充满信心地说，致力于持续集成实践的团队可以持续地、高速地生产出最好的软件。

我们还可以自信地说，我们知道实施 CI 并不容易，但简单地启动这一过程将会带来好处，即使您不会每天部署一千次或在基础架构上投资数百万。

在接下来的两篇关于我们工作流数据的博文中，我们将分享对更多关键 DevOps 指标的见解:[平均恢复时间](https://circleci.com/blog/feedback-loops-the-key-to-improving-mean-time-to-recovery/)和[变更失败率](https://circleci.com/blog/what-does-the-change-fail-rate-tell-us-about-high-performing-teams/)。如果你错过了，这是我们关于[交付周期](https://circleci.com/blog/continuous-integrations-impact-on-lead-time/)的第一篇文章。