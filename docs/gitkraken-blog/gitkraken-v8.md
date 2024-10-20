# GitKraken Git 客户端 v8 - GitKraken CLI 预览和深度链接

> 原文：<https://www.gitkraken.com/blog/gitkraken-v8>

在 GitKraken，我们知道每个开发人员都希望专注于交付最好的项目。花费时间和精力在工具之间切换，或者在钻研代码时努力保持专注，这些都是我们希望最小化的事情。这正是促使我们在 [GitKraken v8.0 版本](https://support.gitkraken.com/release-notes/current/#v8-0-0)中引入令人难以置信的新功能集的原因！

[https://www.youtube.com/embed/PrjLwxsivLg?feature=oembed](https://www.youtube.com/embed/PrjLwxsivLg?feature=oembed)

视频

免费下载 GitKraken 有史以来最强大的版本！

## **吉塔拉克 CLI 预览**

Git 用户的世界通常是一个分裂的房子。在一个阵营中，开发人员严格生活在 Git 命令行中，不想离开，而另一方面，开发人员更喜欢图形用户界面或图形用户界面。多年来，这种分歧已经引起了很多争论，双方的争论也确实随着时间的推移而有所改善。GUI 体验的强大之处在于它的历史可视化和拖放功能，它与 Git CLI 不断发展的功能集并行发展。这实际上是一种非此即彼的情况，直到现在……

随着 GitKraken v8.0 的发布，我们前所未有地团结了 Git 用户，推出了 GitKraken CLI 预览版。现在，用户真的可以两全其美，从同一个 Git 客户端获得 Git 可视化的好处和 Git 命令行的全部功能。

### **具有实时同步可视化功能的 Git 终端**

当新用户第一次打开 GitKraken 时，你会有两种选择:打开 GitKraken GUI 或者 GitKraken CLI。

现有用户只需点击顶部工具栏中的新终端选项卡。这将在一个新的终端标签中打开您的存储库，同时也打开传说中的 GitKraken 提交图。

通过 CLI 与您的本地开发环境进行交互，并且仍然可以获得 GitKraken 以其可视化而闻名的好处，比如丰富多彩的提交图、差异视图、历史和责任视图。

GitKraken CLI 用户可以使用 GitKraken `gk`命令更快地深入项目历史。这个 CLI 程序为您提供了一种在 Git 终端的上下文中与 GitKraken 提交图以及 GitKraken 著名的其他 Git 可视化交互的方法。

命令:

*   `gk  graph`打开或关闭图形。
    *   添加`top`、`bottom`、`left`或`right`改变位置。例如，`gk graph right`将视图设置为分屏。
*   `gk diff`命令快速查看提交之间发生了什么变化。
*   `gk history`命令显示你的文件历史视图。
*   显示文件或提交中发生了什么变化，以及是谁做出了这些变化。

您还可以通过打开`Preferences`下的`Terminal`设置来自定义您的 GitKraken CLI 体验。这允许以下终端选项卡自定义:

*   字体选择。对此设置的更改将仅适用于新的终端选项卡。
*   字体大小。
*   启用自动完成和自动建议。
*   打开新面板时默认的可视化面板位置:顶部、右侧、底部或左侧。
*   默认情况下显示可视化面板。这只在打开新标签页时适用。
*   终端主题。

**自动建议和自动完成 Git 命令**

## 没有比一个空白的 Git 终端窗口等待你输入非常具体的命令更可怕的 UI 了。不要害怕，只要开始在 GitKraken CLI 中输入，就会出现自动建议来帮助你完成你的 [Git 命令](https://www.gitkraken.com/learn/git/commands)！

对于`gk`和`git`命令都是如此。例如，当你开始输入一个`git`命令时，你会看到:

没有比一个空白的 Git 终端窗口等待你输入非常具体的命令更可怕的 UI 了。不要害怕，只要开始在 GitKraken CLI 中输入，就会出现自动建议来帮助你完成你的 [Git 命令](https://www.gitkraken.com/learn/git/commands)！

对于`gk`和`git`命令都是如此。例如，当你开始输入一个`git`命令时，你会看到:

您的新内置助手将帮助您在 Git CLI 的世界中导航。除了建议如何完成 Git 命令，auto-suggest 特性实际上还会提示您可以从每个操作中得到什么。它甚至可以在您需要应用参数时帮助建议和解释参数，如`-a`或`-M`。[学习 Git](https://www.gitkraken.com/learn/git) 从未如此简单或直观！

您的新内置助手将帮助您在 Git CLI 的世界中导航。除了建议如何完成 Git 命令，auto-suggest 特性实际上还会提示您可以从每个操作中得到什么。它甚至可以在您需要应用参数时帮助建议和解释参数，如`-a`或`-M`。[学习 Git](https://www.gitkraken.com/learn/git) 从未如此简单或直观！

免费获得有史以来最好的 Git 终端体验！

**深度链接**

当处理队列中的问题或 [Git pull 请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)时，开发人员需要能够轻松打开问题创建者引用的任何代码。你越快找到你需要处理的代码，你就能越快解决问题。

## 手动打开一个 [Git 库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)并寻找一个特定的 [Git 分支](https://www.gitkraken.com/learn/git/branch)或 Git 标签需要时间，但这也需要稍微转移一下注意力，这可能会导致各种各样的分心。

GitKraken 致力于为团队改善 [Git 工作流程，以增强协作并提高生产率。这就是为什么我们一直在努力给你们带来 8.0 版本的深度链接。

你现在可以链接到一个特定的 Git 库，commit，branch，或者 tag，并在你的 issues，pull requests，Slack，email，或者任何交流平台上分享那个链接。当有人点击这个链接时，它会自动打开 GitKraken 到确切的存储库，并将视图聚焦在特定的提交、分支或指定的标签上。](http://gitkraken.com/teams)

让我们看一个在 GitKraken 中使用分支深度链接的深度链接示例:

GitKraken 致力于为团队改善 [Git 工作流程，以增强协作并提高生产率。这就是为什么我们一直在努力给你们带来 8.0 版本的深度链接。

你现在可以链接到一个特定的 Git 库，commit，branch，或者 tag，并在你的 issues，pull requests，Slack，email，或者任何交流平台上分享那个链接。当有人点击这个链接时，它会自动打开 GitKraken 到确切的存储库，并将视图聚焦在特定的提交、分支或指定的标签上。](http://gitkraken.com/teams)

让我们看一个在 GitKraken 中使用分支深度链接的深度链接示例:

**命令面板**

在 GitKraken v8.0 中，模糊查找器已被重命名为命令面板，现在更容易被发现。这个功能可以节省大量的时间！将手指放在键盘上，键入`Ctrl` / `Cmd` + `P`，或者点按工具栏中的新魔棒图标以打开命令调板。从这里，您可以快速执行各种 Git 操作。

## **命令面板**

在 GitKraken v8.0 中，模糊查找器已被重命名为命令面板，现在更容易被发现。这个功能可以节省大量的时间！将手指放在键盘上，键入`Ctrl` / `Cmd` + `P`，或者点按工具栏中的新魔棒图标以打开命令调板。从这里，您可以快速执行各种 Git 操作。

**改进的标签页体验**

利用我们强大的标签功能访问多个存储库。由于在标签栏中添加了一个新的下拉列表，现在导航变得更加容易。只需单击您想要返回的选项卡，您就可以回到您的工作流程中。所有标签现在都有工具提示，显示回购的名称和路径，让你更容易跟踪你在哪里。

## **改进的标签页体验**

利用我们强大的标签功能访问多个存储库。由于在标签栏中添加了一个新的下拉列表，现在导航变得更加容易。只需单击您想要返回的选项卡，您就可以回到您的工作流程中。所有标签现在都有工具提示，显示回购的名称和路径，让你更容易跟踪你在哪里。

**一个 Git 客户端来管理所有客户端**

我们很自豪也很兴奋地发布 [GitKraken](https://www.gitkraken.com/git-client) 的下一步进化。我们希望有了 GitKraken CLI，命令行用户和 GUI 爱好者可以像以前一样走到一起，进行协作。

## 不管你是 Git 的新手，还是在人类时代之前就已经使用这个工具~~有一段时间了，你都会爱上 GitKraken 的 8.0 版，适用于 Windows、Mac、&Linux。~~

我们很自豪也很兴奋地发布 [GitKraken](https://www.gitkraken.com/git-client) 的下一步进化。我们希望有了 GitKraken CLI，命令行用户和 GUI 爱好者可以像以前一样走到一起，进行协作。

立即免费获取 GitKraken！这是一个 Git 客户端来管理它们。

立即免费获取 GitKraken！这是一个 Git 客户端来管理它们。