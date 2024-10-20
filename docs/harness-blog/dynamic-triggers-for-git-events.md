# git 事件管理中的动态触发器|管理

> 原文：<https://www.harness.io/blog/dynamic-triggers-for-git-events>

本文概述了 Harness 中可用的动态触发器，并根据 GitHub webhooks 中标记的事件重点介绍了这些触发器的工作方式。这有助于开发人员根据他们的用例来定制触发器。

触发器是一个关键术语，用来表示自动化部署或启动管道构建的步骤。如下表所示，摘自 CD。Foundation 的[通用术语映射](https://github.com/cdfoundation/sig-interoperability/blob/main/docs/tools-terminology.md#cicd-tools-and-technologies)，trigger 几乎被所有可用的 CI/CD 工具使用。

‍

同样，在部署中使用 in harness 来基于各种 git 事件(即 PUSH、CREATE & ALL)自动化 CI/CD 管道，从而可用于基于这些事件自动触发管道。

## 我们为什么需要它？

触发器支持事件驱动的 CI/CD，并支持每次提交构建和/或部署到目标环境的实践。

## 触发器是如何基于各种 git 事件的？

可以自动触发管道来响应 Git 事件。例如，当 Git repo 上发生拉请求或推事件时，可以执行 CI 或 CD 管道。

触发器在特定的预定义条件下工作，这有助于形成一套完整的标准，以便根据给定源中的更改来触发管道。在利用中，我们根据 3 个 git 事件对条件进行了分类:

1.  拉取请求
2.  问题注释
3.  推

这进一步归因于

例如:

*   如果源/目标分支名称与模式匹配，则执行管道。
*   如果为 Git repo 中特定目录的文件更改发送事件，则执行管道。这在使用 monorepo (mono repository)时非常有用。它确保只有特定的管道被触发以响应变化。

## 让它与 GitHub web-hooks 一起工作:

为了配置 GitHub web-hooks，harness 使用 [events](https://docs.github.com/en/rest/activity/events) API 来选择哪些事件将发送有效载荷。

以下是 Harness for triggers 中描述的有效负载条件示例:

`payloadConditions: - key: operator: Contains value: tags - key: operator: Contains value: main`

以下资源可用于获取有关如何触发管道以响应与您在[线束触发器](https://docs.harness.io/article/rset0jry8q-triggers-reference)中设置的特定负载条件相匹配的 Git 事件的更多信息，还可用于定制 Github 负载条件的触发器:

1.  [使用 Git 事件触发管道](https://docs.harness.io/article/hndnde8usz-triggering-pipelines)
2.  [使用 Git 事件有效负载条件触发管道](https://docs.harness.io/article/10y3mvkdvk-trigger-pipelines-using-custom-payload-conditions)

## 需要进一步的帮助吗？

欢迎在 [community.harness.io](https://community.harness.io/c/harness/7) 或[提出问题，加入社区 slack](https://join.slack.com/t/harnesscommunity/shared_invite/zt-y4hdqh7p-RVuEQyIl5Hcx4Ck8VCvzBw) 与我们的工程师在特定于产品的渠道中聊天，如: