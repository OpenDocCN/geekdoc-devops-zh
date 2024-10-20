# GitHub SEO | git kon 2022 | Greg Bulmash，西雅图 CoderDojo

> 原文：<https://www.gitkraken.com/gitkon/github-seo-greg-bulmash>

[https://www.youtube.com/embed/9R9K_isJulY?feature=oembed](https://www.youtube.com/embed/9R9K_isJulY?feature=oembed)

视频

SEO，或搜索引擎优化，对一些人来说是一个肮脏的短语，但这通常是因为他们不明白让你的内容更容易找到和把它塞进人们的喉咙之间的区别。这可能是因为他们看到的 SEO 侧重于后者。

有一些实现 SEO 的方法实际上对读者是有帮助的，而不是恶意的。这里有一些技巧，可以让你在 GitHub 上的内容更容易被 GitHub SEO 发现。

如果你在 GitHub 上托管你的回购，考虑使用 GitKraken 客户端来增强你的项目历史的可见性，并且更有信心执行高级 Git 操作。

## **使用 GitHub 组织**

例如，假设您有一个包含您个人项目的个人 GitHub ID，但是您也加入了一个志愿者团体。您可以为您的志愿者小组创建一个新的 [GitHub 组织](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/about-organizations)，而不是在您的个人帐户中为您的志愿者小组创建一个存储库。

GitHub 上的组织就像个人 id，但它们可以被更好地命名，因为它们不是个人句柄。而且你可以“拥有”不止一个。因为微软拥有 GitHub，所以让我们用他们著名的例子公司之一 Contoso 来进一步说明这一点。

假设您是 Contoso 的 OSS 社区经理，您需要分享一些指南和代码示例。您可以使用以下命名结构:

`github.com/contoso (organization)`

`  |- Guides (repo)`

`|- Using_Contoso_Widgets (folder)`

`|- Samples (repo)`

`    |- Contoso_Widgets_SDK (folder)`

但是，如果有人在 GitHub 上搜索，甚至在 GitHub 上的 Contoso 组织的顶层搜索，例如“Contoso Widgets ”,它可能不会注册任何结果，原因很简单，因为名称中没有包含“Widgets”和“Contoso”的存储库。

在这种情况下，您可能会收到一条消息:“我们找不到任何与搜索`org:contoso contoso widgets.`匹配的结果”

更好的策略是使用以下命名结构:

`github.com/contoso-guides (organization)`

`|- Using_Contoso_Widgets (repo)`

`github.com/contoso-samples (organization)`

`  |- Contoso_Widgets_SDK (repo)`

## **GitHub SEO 的好处**

实现 GitHub SEO 最佳实践有一些快速的好处。

1:您现在在存储库名称中使用搜索词*，而不是在子文件夹的名称中。这使得内容更容易找到，因为它会显示在主要搜索中。*

2:谷歌的算法通过文件夹的深度对内容(在一定程度上)进行加权。

*   把它放在四个文件夹的深度。
*   把它放在三个文件夹的深度。

这不是一个巨大的差异，但谷歌将更快地索引内容，并认为它与“contoso widgets”更相关，从而使它更有可能出现在谷歌搜索结果的更高位置。

但是你可能会问:“如果客户不在一个组织下，他们如何找到从指南到 SDK 的路，反之亦然？”

这是一个很好的问题，但你可以很容易地通过链接自述文件到相关的回购协议来解决这个问题，而不仅仅是作为底部的一个要点。每当您在指南中提到 SDK 时，请链接到 SDK repo。

## **开始使用 GitHub SEO**

如你所见，这些都不是黑帽 SEO 这不是打败竞争对手的一招。这些良好的实践有助于确保更多的人在 GitHub 上搜索您分享的内容时能够轻松找到它。所以下次你打算在 GitHub 上分享东西的时候，考虑在你的架构中使用组织。

将 GitHub repos 与 GitKraken Client 集成在一起再简单不过了，一旦完成，您将体验到强大的功能，如交互式拉请求管理、GitHub 问题连接等等。