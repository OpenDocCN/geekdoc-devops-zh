# -= ytet-伊甸园字幕组=-翻译:粒粒尘紫月猫姐校对

> 原文：<https://www.gitkraken.com/blog/gitkraken-v5-0>

我们一直像冰原狼一样工作，为 GitKraken Git 客户端提供一些令人难以置信的新特性。我们希望您和我们一样对这些新功能和改进的性能感到兴奋。

我们带给你:所有释放的母亲！

[https://www.youtube.com/embed/K1mbGn0Af8U?feature=oembed](https://www.youtube.com/embed/K1mbGn0Af8U?feature=oembed)

视频

***获取 GitKraken Git 客户端的最新版本:***

## **交互式重置基础**

互动 rebase 是~~来~~ …这里。

举起你的圣杯！

现在，当您将一个分支拖放到 rebase 时，您将看到一个交互式 rebase 选项。

单击打开`Interactive Rebase`视图后，您可以为每个提交选择一个操作:

*   在左侧，您可以选择、改写、挤压或丢弃提交。
*   使用`up`和`down`键来导航提交。
*   双击提交以改写。
*   拖放以上下移动提交。
*   使用键盘快捷键来执行所有这些操作！

一旦你做出选择，点击`Start Rebase`按钮；交互式基础将开始，图形将更新以反映您的更改。*这是一个如此简单高效的过程，你会觉得自己像拿着剑的塔斯的布蕾妮。*

## **访问交互式 Rebase 的其他方式**

您还可以右键单击图中的提交来访问其子提交的交互式 rebase 选项。

此外，当您右键单击提交时，现在有以下选项:

*   编辑提交消息。
*   放弃提交。
*   移动提交。
*   或者选择多个提交来挤压，而不需要头提交。

**G.P.G.**

## 不，你耳朵里没有龙鳞… GitKraken 现在支持 GPG 提交签名。*倒红酒！*

不确定提交签名是什么或它需要什么？查看我们与 GPG [签署的承诺支持文档](https://support.gitkraken.com/git-workflows-and-extensions/commit-signing-with-gpg/)以了解更多信息。

简而言之，GPG 提交签名有助于防止其他用户欺诈性地使用您的别名进行提交，并且是公司为了遵守更严格的安全合规性标准而更加频繁地强制执行的事情。毕竟，真正的北境之王只有一个。

在您的计算机上安装和配置 GPG 后，您必须生成一个 GPG 密钥。这可以在你的 GitKraken 帐户中轻松完成。一旦您有了自己的唯一密钥，您就可以与您的远程主机服务共享它(或者配置 GitKraken 来为您做这件事)，并开始使用您的密钥签署提交。

使用您的 GPG 密钥签署提交将允许其他用户验证该提交确实是由您做出的，而不是某个冒充您的人。

需要帮助安装 GPG 吗？查看我们支持文档的[部分](https://support.gitkraken.com/git-workflows-and-extensions/commit-signing-with-gpg/),获取分步指南。

需要帮助安装 GPG 吗？查看我们支持文档的[部分](https://support.gitkraken.com/git-workflows-and-extensions/commit-signing-with-gpg/),获取分步指南。

**验证 GitKraken 中的提交**

## 完成安装 GPG 的步骤后，选择您的密钥，并配置 GitKraken 使用您的密钥签署提交；您所做的所有提交都将在提交消息中包含一个绿色徽章，注明提交为`Verified`。您可以在 GitKraken 的右侧提交面板中查看此徽章。

类似地，您可以通过检查其他用户的绿色徽章来验证在您的项目中工作的其他用户的合法性。只有同时满足以下两个条件时，图标才会显示为绿色:

您有其他用户的公共 GPG 密钥。

*   用户的 GPG 密钥是可信的。
*   如果在没有 GPG 密钥的情况下签署提交，则不会出现图标。如果 GPG 无法验证或找到签名密钥，或者签名密钥已过期或被吊销，图标将显示为灰色。这将有助于你清除叛徒。

**更快的抓取速度**

在一个有多个远程设备的存储库上获取数据现在快多了。那感觉就像是在龙上飞行。

## **更快的抓取速度**

**增加了欢迎屏幕上的选项**

布拉佛斯。我们增加了克隆一个库到 GitKraken 欢迎界面的选项。现在去化妆吧！

## **增加了欢迎屏幕上的选项**

**获取内存泄漏已修复**

我们的团队已经解决了`Fetch`操作可能出现的内存泄漏问题。瓦里斯和他的密语者将不得不去别处寻找他们的秘密。

## **获取内存泄漏已修复**

**Azure DevOps 抓取**

在不使用集成的情况下从 Azure DevOps 存储库获取数据时，GitKraken 将不再抛出`Fetching pull requests failed`错误消息。

## *我们知道谎言会导致什么……*

在不使用集成的情况下从 Azure DevOps 存储库获取数据时，GitKraken 将不再抛出`Fetching pull requests failed`错误消息。

*我们知道谎言会导致什么……*