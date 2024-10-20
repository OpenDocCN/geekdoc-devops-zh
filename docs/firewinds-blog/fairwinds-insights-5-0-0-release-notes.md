# Fairwinds Insights 5.0.0 发行说明

> 原文：<https://www.fairwinds.com/blog/fairwinds-insights-5-0-0-release-notes>

 在六月和七月，我们都在努力开发我们的 Fairwinds Insights 5.0.0 版本。5.0.0 版本包括与 Kubernetes 安全性、策略和治理相关的新更新。我们还在 6 月份的 4.4.0 中添加了一些很棒的新功能，我在这篇文章中也提到了。其他最近的更新包括一个漏洞用户界面，在吉拉创建票的能力，等等！了解有关我们版本中的新特性和附加功能的更多信息。

*如果您需要持续的*[*【Kubernetes】安全*](http://fairwinds.com/kubernetes-security) *监控、策略执行以及治理合规性和优化成本的能力，* [*尝试洞察*](/fairwinds-insights-demo) *。*

## 费尔温德见解 5.0.0 版本

### SAML 功能-测试版

我们很高兴地宣布，Insights 现在提供了安全断言标记语言(SAML)功能！[如果您想试用或在我们的文档中了解更多关于](/contact-us) [SAML 功能的信息](https://insights.docs.fairwinds.com/configure/management/membership/#single-sign-on-beta)，请联系我们。

### 在行动项目表中保存视图

现在，您可以在“行动项目”( action items)表格中保存过滤后的视图。使用表格顶部的星号按钮，并为视图指定一个名称。您以后将能够访问该视图，而无需重新选择过滤选项。

### 导航滚动条

为了提高 Insights 的可用性，现在在导航栏的存储库和集群选项中有一个滚动条。

### 改进了 UI 中的错误消息处理

我们改变了 Insights 处理错误消息的方式。现在可以显式取消通知了。Insights 现在还会显示更长时间的错误消息，以便您通读信息。

### 修复了修复错误

我们的团队解决了一个问题，即 [Polaris](/polaris) 行动项目的信息在修复下不显示。

### 修正了行动项目的错误

有一个超级用户在 kube 系统中看不到行动项目的 bug，现在已经解决了。我们还修复了一个 bug，在这个 bug 中，一些动作项对于非管理员来说是隐藏的。

## Fairwinds Insights 4.4.0 版本

### 更好的报告中心同步

大多数 Insights 用户将他们的 insights-agent 安装编码为基础设施代码，而不是复制和粘贴 Insights UI 中提供的 [Helm](https://helm.sh/) 安装指令。有了这个更新，如果你在你的头盔安装上做了改变，用户界面会自动合并这些改变。

### 按票据创建排序/过滤

您现在可以根据票证是否已创建来对操作项进行排序和过滤。

### 离线报告停止显示准入控制器

#### *准入控制器的修复*

我们已经解决了准入控制器中的一些小问题:

*   重复的行动项目不再触发 400 响应
*   准入控制器不定期运行，因此无法自动检测它是否离线或只是安静。为解决该潜在问题，您将不再收到准入控制器离线的通知。

## 尝试最新版本的 Fairwinds Insights

对使用 Fairwinds Insights 感兴趣？免费提供！ [在这里报名。](https://www.fairwinds.com/coming-soon)

要了解如何使用这些功能并了解最新的 [Fairwinds Insights，请随时查看最新的发行说明](https://insights.docs.fairwinds.com/release-notes/)。

你们中的一些人可能已经在使用我们的开源项目了。我们最近推出了一个开源用户组，我们一直希望有更多的人加入我们！[在这里](/open-source-software-user-group)注册 Fairwinds 开源用户组，并加入我们九月份的下一次聚会。

[![Download Kubernetes Best Practices Whitepaper](img/c38df324d5163c7ccc9c6b998a78ad26.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/cb39a009-a458-4282-9211-41e010cb3376)