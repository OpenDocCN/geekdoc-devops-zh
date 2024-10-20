# 使用 Git 度量和 GitKraken 提高团队效率

> 原文：<https://www.gitkraken.com/gitkon/git-metrics-gitkraken>

[https://www.youtube.com/embed/37LLPw5e68c?feature=oembed](https://www.youtube.com/embed/37LLPw5e68c?feature=oembed)

视频

当我们在 GitKraken 中谈论 Git Velocity 时，我们指的是一组 Git 指标&围绕您的活动的数据，这些数据可以帮助您和您的队友更有效地使用 Git 以及相互合作。

当您的整个团队都在使用 GitKraken Git 客户端时，您将更加协调一致地工作，以提高整体生产力并改善开发人员的体验。

Git Velocity 的目标是实现两个结果:更频繁地发布和更少的 bug。

如果你不熟悉 DevOps 研究和评估(DORA)报告，它们是来自 Google 的一系列报告，这些报告在六年的时间里调查和分析了数千个开发团队，以了解哪些行为将高绩效开发团队与低绩效开发团队区分开来。

来自 GitLab 的高级开发人员布道者 Brendan O'leary 在 2021 年 [GitKon Git 大会](https://gitkon.com/)上分享了他对 [DORA metrics](https://www.gitkraken.com/gitkon/dora-metrics) 的想法。

如果你想更深入地探索研究的各个方面，强烈推荐阅读 Nikole Forsgren、Gene Kim 和 Jez Humble 的《加速》。

**朵拉指标**

让我们从帮助团队设定目标和更好地执行的 4 个关键 DORA 指标开始。在 GitKraken，我们坚信 [Git 是一项团队运动](https://www.gitkraken.com/git-client/team-features)，并认为访问这些指标将帮助您发布更高质量的代码，并更频繁地这样做。

## 这 4 个测量值是:

吞吐量:代码被成功推送到生产环境的频率。

变更的前置时间(周期时间):投入生产所需的时间。

变更失败率:导致生产失败的部署的百分比。

平均恢复时间:从生产故障中恢复需要多长时间。

*   Throughput: How often code is successfully pushed to the production environment.
*   **Git 度量在 gitkraken】中的应用**
*   在 GitKraken，我们正在探索将这些类型作为 Git 指标提供给开发团队的方法。
*   这是我们为内部测试和跟踪我们团队的 Git 速度而构建的系统的早期视图。目标是定期监控 Git 指标，并尽早暴露瓶颈。

## **Git Metrics in GitKraken**

这些 Git 指标由连接到 GitKraken Git 客户端的存储库提供支持。登录到贵组织的 GitKraken 帐户后，将要求您选择您的存储库托管在哪个提供商上。目前只支持 GitHub，但未来有计划支持其他提供商(GitLab、Azure DevOps、BitBucket)。

在连接并提供 PAT 之后，您将能够选择您想要跟踪的存储库。通过分析存储库活动，Git 客户端可以提供基于实际代码变更的 Git 分析，而不是来自项目管理工具的任意问题。

一旦选择了存储库，一个简单的仪表板显示过去 30 天的周期时间和吞吐量指标。最终，您将能够通过日期范围、团队，甚至特定的存储库组来分析和过滤这些 Git 指标，以比较您的速度是如何在您的发布和项目中受到影响的。

您还可以深入每个指标，分析影响这些数字的具体变化。例如，通过周期时间，您可以查看已扫描的[拉取请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)的列表，并轻松识别哪些请求花费的时间比预期的长。

GitKraken Git 客户端的用户也可以期待 GitKraken 项目更好的回购管理。项目将是存储库的集合，您可以在 Git 客户机中创建这些存储库来帮助您和您的团队跨多个存储库更有效地工作。您可以从单个视图中查看每个存储库的当前签出分支的状态，并更新、打开和更改它们中的任何一个。

You can also drill into each metric and analyze the specifics of the changes that are influencing the numbers. For example, with Cycle Time, you may view the list of [pull requests](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git) that were scanned and easily identify which took longer than expected. 

Gitkraken 项目还将允许您在一个位置查看项目中所有存储库的请求和问题，而不是在选项卡之间来回切换。

因为这些功能处于设计和开发的早期阶段，GitKraken 团队希望得到关于我们如何构建 Git velocity 服务的行为、方向和方法的反馈。

如果你或你的团队想问问题或参与测试和提供反馈，请发电子邮件到 feedback@gitkraken.com，在主题行写上 Git velocity。

Gitkraken Projects will also allow you to see pull requests and issues for all repositories in the project in one location, instead of bouncing around between tabs. 

Because these features are in the early stages of design and development, the GitKraken team would love feedback regarding the behavior, direction and approach to how we are building the Git velocity service. 

If you or your team want to ask questions or be involved in testing and providing feedback, please email us at [feedback@gitkraken.com](mailto:feedback@gitkraken.com) with Git velocity in the subject line.