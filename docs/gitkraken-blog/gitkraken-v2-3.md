# GitKraken v2.3:现在有 Git 挂钩了！

> 原文：<https://www.gitkraken.com/blog/gitkraken-v2-3>

你可能已经知道了，我们正在努力满足你最喜欢的 Git 客户端 GitKraken 忠实用户的需求和请求。2.3 版本实现了一个被广泛要求的特性，Axosoft 的每个人都很高兴在一个版本中看到它:Git hooks！

我们知道这是一个障碍，它阻止了一些用户在他们的团队中使用 GitKraken，而在某些行动中特定的功能是绝对必要的。随着 [Git 钩子支持](https://support.gitkraken.com/working-with-repositories/githooks/)，我们希望 GitKraken 现在结合了两个世界的精华:一个直观且简单易用的界面，以及 Git 钩子的超级用户功能。

## **什么是*Git 挂钩？***

 *钩子可以被定义为触发器。如果你熟悉 JavaScript，你可能以前用过钩子，以**事件**的形式。可以设置事件侦听器，以便在发生某些事件(例如，单击、点击)时触发自定义操作。那些事件可能被认为是“挂钩”，因为你是“挂钩”到它们中去做你需要做的事情。

WordPress 用户可能也对**动作钩子**的上下文中的钩子很熟悉。在呈现的页面或帖子中的某些点，会触发各种动作，程序员可以将自定义函数挂接到这些动作中，以便在呈现过程中的该点处理手头的信息。

Git 挂钩非常相似。它们允许用户创建定制的脚本，在 Git 进程中的特定时刻触发。GitKraken 不要求您在系统上安装 Git，所以直到现在，这种独立性意味着没有 Git 挂钩支持。但是，有了大量的血、汗和泪，v2.3 允许你用自己的方式来控制你的 Git 行为！

观看这个短片来了解 Git 挂钩，并了解 Git 挂钩在 GitKraken 中是如何工作的。

[https://www.youtube.com/embed/ZZgyILr-TjA?feature=oembed](https://www.youtube.com/embed/ZZgyILr-TjA?feature=oembed)

视频

## **git kraken 支持哪些钩子？**

在每个钩子下面是 GitKraken 调用该钩子的动作列表:

*   `pre-commit`:
*   `prepare-commit-msg`:
    *   犯罪
    *   改进
    *   Cherrypick
    *   合并
    *   壁球
    *   归还
*   `commit-msg`:
*   `post-commit`:
    *   犯罪
    *   改进
    *   Cherrypick
    *   合并解决
    *   归还
*   `pre-rebase`:
*   `post-checkout`:
    *   检验
    *   放弃更改(有选择地)
*   `post-merge`:
    *   合并(无冲突)
    *   快进
*   `post-rewrite`:
*   `pre-push`:
    *   推送分支
    *   推送标签
    *   删除远程分支
    *   删除远程标签

这就是 Git hooks。我们希望你喜欢在我们的行动中伸出你的触角！

So that’s Git hooks. We hope you enjoy getting your tentacles all up in our actions!

**地区日期设置**

## 另一个被广泛要求的特性是能够为提交设置特定区域的显示日期。你们可能不是本地人，可能有一些特定地区的约会方式。当你试图一目了然地解读日期时，查看另一种格式可能会产生不和谐的效果。

你猜怎么着？GitKraken 现在会想，我在哪里？并将根据您的系统语言环境相应地更新其日期格式。不客气！德里昂！比特舍恩！德·纳达！别提了！皮普皮普。就像你一样。

Well, guess what? GitKraken will now think to itself, *where am I?* and will update its date format accordingly, based on your system locale. You’re welcome! De rien! Bitte schön! De nada! Don’t mention it! Pip pip! As you were.

**新的入职体验**

## 现在比以往任何时候都更容易让你的团队在 GitKraken 安顿下来。V2.3 为首次用户引入了全新的入职界面。更容易看到在哪里设置首选项并开始使用回购。它还向用户介绍了我们的[*GitKraken*简介视频](https://support.gitkraken.com/#get-started)，它提供了 GitKraken 功能的 90 秒快速概述，我们的[支持网站](https://support.gitkraken.com/)，它提供了大量有用的文档，以及 [GitKraken Slack 社区](https://slack.gitkraken.com/)，在那里我们的用户可以互相帮助，帮助我们的团队改进 git kraken。

但是，还有一件事…

But, there is just one more thing…

**免费试用 git kraken Pro**

## 如果您一直想尝试 GitKraken Pro 的功能，如合并冲突输出编辑器、工作和个人使用的多个配置文件或 GitHub 企业集成，现在您有机会了！

只需点击

应用内按钮。在决定是否要升级到付费帐户之前，您将能够测试这些令人敬畏的功能长达 14 天！

button in-app. You’ll be able to test these awesome features for up to 14 days before deciding if you want to upgrade to a paid account!*