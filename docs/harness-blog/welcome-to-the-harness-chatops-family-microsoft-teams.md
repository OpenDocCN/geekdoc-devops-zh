# 线束支持 ChatOps - Microsoft 团队|线束

> 原文：<https://www.harness.io/blog/welcome-to-the-harness-chatops-family-microsoft-teams>

欢迎来到 Harness 大家庭，微软团队！还有一个像 DevOps 一样的“ops”音素是[ChatOps](https://www.pagerduty.com/blog/what-is-chatops/)；或基于对话的开发/运营。ChatOps 越来越受欢迎，因为像 [Slack](https://slack.com/) 这样的交流平台已经深入我们的日常工程生活。一位团队领导曾经告诉我“如果它没有在空闲时发生，它就不会发生”，这表明了沟通平台作为记录系统的重要性。

随着微软 Lync 和 Skype 的不断发展，[微软团队](https://www.microsoft.com/en-us/microsoft-365/microsoft-teams/group-chat-software)是企业通信生态系统中的新成员。在[社区需求](https://community.harness.io/t/microsoft-teams-integration/213)的推动下，我们很高兴能够构建额外的原生功能来整合微软团队。

### 我们组队吧

要开始，您首先需要能够访问 Microsoft 团队。如果您没有帐户，您可以[向微软](https://www.microsoft.com/en-us/microsoft-365/microsoft-teams/free)注册一个对您有意义的层级；我正在使用免费层。

像往常一样，你可以继续关注博客和/或观看视频。

#### Microsoft 团队配置

我创建了一个名为“金丝雀队长”的团队和一个名为“金丝雀呼叫”的 T2 频道。我将把 Harness 生成的警报推送给“金丝雀的叫声”。

集成的重点是微软团队处理传入的 webhook 的能力。你可以很快建立一个，更多细节可以参考微软团队文档。添加 Incoming Webhook 应用可以通过进入 App - >搜索“web hooks”->Incoming web hook 来完成。

安装到微软团队时，您需要选择一个渠道。在我的例子中，“金丝雀的叫声”是我希望警报到达的通道。

命名网页挂钩。在我的情况下是“我的提醒”,然后单击创建。

将创建 webhook。确保复制将用于发布到频道的 URL。

#### 线束配置

要在 Harness 中启用 Microsoft Teams 通知，第一步是将 webhook 作为通知机制的一部分连接到一个 [Harness 用户组](https://developer.harness.io/docs/first-gen/firstgen-platform/security/access-management-howtos/users-and-permissions/)中。

持续安全->访问管理

您可以利用现有的用户组或创建一个新的用户组。在这里，我创建了一个名为“金丝雀队长”的新用户组。

朝着用户组设置的按钮，我们将编辑通知。

粘贴入站 Webhooks 应用程序生成的 webhook URL。

单击提交后，返回用户组列表。您将看到一个 Microsoft Teams 图标出现在通知类型列下。

有了这些连接，您就可以导航到[利用工作流](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/workflows/workflow-configuration/)并添加一个[通知策略](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/workflows/workflow-configuration/#notification-strategy)。我有一个简单的 [Kubernetes 部署](https://developer.harness.io/docs/first-gen/first-gen-quickstarts/kubernetes-quickstart/)，我用它作为例子。

您可以配置何时收到通知。对于这个例子，我选择了所有需要通知的可用条件。选择您已经连接到 webhook 的用户组。

单击提交后，通知策略将被关联。

现在是时候尝试一下了。单击部署按钮。

在新部署上单击提交。

部署将会运行。

您现在将在 Microsoft Teams 中收到通知。成功！

就这样，你现在融入了微软团队。

### 线束平台的灵活性

Harness 平台旨在协调自信地部署您的应用程序/平台所需的不同任务。建立信任的一部分是[提醒需要的各方](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/workflows/workflow-configuration/#notification-strategy)对部署谨慎的事件。值得一提的包括利用[的 Slack](https://developer.harness.io/docs/platform/notifications/send-notifications-using-slack/) 或[的 page duty](https://www.youtube.com/watch?v=YDyNj9EYiNk&feature=emb_logo)进行通知。Harness 平台将继续发展并适应您的需求；如果你有一个很酷的集成想法，请随时前往 [Harness 社区](https://community.harness.io/)，让我们知道你的想法或你一直在做的事情。像往常一样，如果你还没有报名，今天[报名](https://harness.io/try-continuous-delivery-as-a-service-for-free/)。

干杯！

拉维