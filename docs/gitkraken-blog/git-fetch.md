# 什么是 Git Fetch |远程分支和错误问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/git-fetch>

Git 变得如此流行的原因之一是因为它使任何规模的团队都能够在代码上进行协作。当使用 Git 时，用户可以 [Git 将](https://www.gitkraken.com/learn/git/git-push)更改推送到他们本地存储库的共享副本，称为 [Git remote](https://www.gitkraken.com/learn/git/git-remote) 。当新的合作者执行 [Git 克隆](https://www.gitkraken.com/learn/git/git-clone)时，他们可以拉下这些集体变更——所有相关 [Git 提交](https://www.gitkraken.com/learn/git/commit)的总和——但是如果有人只想拉下选定的变更呢？

如果有人已经克隆了一个远程，但是想要获得其他人提交的新更改，并将这些更改合并到他们的本地 repos 中，他们可以执行 Git 获取。在本文中，我们将详细回顾 Git fetch 命令，并回顾如何在命令行和 [GitKraken 客户端](https://www.gitkraken.com/git-client)中执行该操作

“@GitKraken 让我的生活轻松多了！在分支之间切换，压缩提交，很好的概述代码中的变化等等。👌"–[@ @ mjovanc](https://twitter.com/mjovanc/status/1554397486244036609)

[https://www.youtube.com/embed/uEEcw1s_wWk?feature=oembed](https://www.youtube.com/embed/uEEcw1s_wWk?feature=oembed)

视频

## 什么是 Git Fetch？

Git fetch 是 Git 中的一个命令，它执行两个不同的任务。首先，Git fetch 从特定的远程分支下载所有提交，在本地更新远程跟踪分支。同时，Git 更新一个名为 FETCH_HEAD 的特殊文件，该文件跟踪下载的更新来自哪里以及涉及哪些提交 sha。

### **远程跟踪分支**

Git 通过远程跟踪分支跟踪本地 [Git 分支](https://www.gitkraken.com/learn/git/branch)与任何远程存储库的不同步程度。Git 根据远程跟踪分支的内容，统计本地 repo 来自远程 repo 之前或之后的提交次数。超前意味着本地分支中存在远程分支中不存在的变化。另一方面，滞后意味着远程上的更改在相应的本地分支上还不存在。

您可以通过执行以下命令查看一个存储库的所有远程跟踪分支:
`git branch -a`

Git 是异步工作的，这意味着每个人都在本地机器上处理回购的完整副本。与 SVN 或其他版本控制系统不同，使用 Git，没有真正的中央存储库可以连接。Git 不会通过互联网与远程 repos 保持连接，并假设您将从远程服务器手动下载提交。同时，Git 需要为您提供一种方法来检查这些更改，以防您不想在本地应用它们。这就是 Git fetch 的本质。

当您执行一个 Git 获取时，远程分支上的所有提交首先被下载到远程跟踪分支，允许您在将更改应用到本地分支之前查看提交的内容。你可以用几种不同的方法来做到这一点。

查看本地下载的变更示例的第一种方法是[Git check out](https://www.gitkraken.com/learn/git/git-checkout)remote tracking 分支。这将使你处于分离的头部状态，意味着头部没有指向局部分支的末端。你可以看到所有的修改，复制代码，或者[创建一个新的 Git 分支](https://www.gitkraken.com/learn/git/problems/create-git-branch)，所有这些都不会影响本地分支。

另一个可以快速向您展示发生了什么变化的选项是运行一个 [Git diff](https://www.gitkraken.com/learn/git/git-diff) ，在您执行一个 Git fetch 之后，它将本地分支与远程跟踪分支进行比较。我们将在本文的后面用命令行[和 GitKraken 客户端展示这样的例子。](https://www.gitkraken.com/cli)

### **Git FETCH_HEAD**

每当运行 Git init 时，都会创建一个./git 文件夹，保存帮助 Git 做它应该做的事情的密钥。每个文件夹中都有一个名为 FETCH_HEAD 的特殊文件。FETCH_HEAD 文件跟踪最近获取的所有分支，以及远程上该特定分支上存在的最新提交的提交 SHA。

这个文件有一个特殊的功能，因为它可以在一个 [Git 合并](https://www.gitkraken.com/learn/git/git-merge)中使用。在这种情况下，Git 将为 FETCH_HEAD 中可用于合并的所有分支解析合并请求。当执行 [Git 拉取而不仅仅是 Git 获取](https://www.gitkraken.com/learn/git/problems/git-pull-vs-fetch)时，这将自动发生。

#### **用 FETCH_HEAD** 从多个远程获取

您可以在 FETCH_HEAD 中引用来自多个远程仓库的分支的多个版本。默认情况下，每次运行 Git fetch 都会覆盖整个 FETCH_HEAD 文件。

然而，通过 Git fetch 使用`--append`选项，或者仅仅使用`-a`，Git 将会把这些额外的获取分支添加到 FETCH_HEAD 文件中。运行一个`git merge FETCH_HEAD`将合并来自不同远程的所有提交，所有这些都在一个步骤中完成！💥

跟踪回购的所有遥控器并不复杂。免费下载 GitKraken 客户端，现在就可以可视化并更好地了解您的存储库！

## **Git 获取远程分支**

正如您已经读到的，Git fetch 被设计为将更改下载到本地计算机上的远程跟踪分支。但是 Git fetch 如何知道您打算从哪个分支和哪个远程下载更改，特别是当不止一个远程可用时？最佳实践是在运行 Git fetch 时指定我们想要的远程和分支。

如果在没有其他选项或子命令的情况下运行`git fetch`，Git 将假设您想要从默认的上游和当前检出的任何分支获取数据。分支的默认上游通常是第一个创建的分支，通常是最初克隆代码的上游。如果你检查。git/config 文件，您可以很快看到每个分支与什么上游相关联。

通过明确声明 Git 获取的目标是哪个远程和分支，可以确保从正确的源代码中获取更新的代码。

如果你有很多分支和很多遥控器，依靠`git fetch –all`可以节省大量时间。`--all`选项告诉 Git 尝试从所有可能的远程获取任何具有本地不存在的变更的分支。

## **错误:无法打开。git/FETCH_HEAD 权限被拒绝**

有时，当您对 FETCH_HEAD 执行 Git fetch 或 Git merge 时，您的 CLI shell 将返回一个错误，说明:错误:无法打开。git/FETCH_HEAD 权限被拒绝。

发生这种情况的原因有很多，但最常见的是因为您没有正确的用户权限来更新或访问 FETCH_HEAD 文件。

要解决这个错误，请确保您以对正在使用的 Git 存储库具有读写权限的用户身份登录。或者，你可以修改文件权限，或者用`chmod` 允许更多用户[访问，或者用`chown`将文件所有权更改为你自己。](https://www.gitkraken.com/blog/shell-scripting)

## **如何在命令行中获取数据**

在执行 Git 获取之前，最好先看看哪些 Git 远程可用于 repo。要查看所有可用的遥控器，请使用命令:

`git remote -v`

在上面的例子中，有三个可用的远程位置来下载变更:`origin`，存储库最初被克隆到本地机器上；`gitkraken`，它是 GitLens 产品所有者更新的上游；以及`gitlab`，它是在[GitLab.com](https://about.gitlab.com/)上的回购副本。

**在 CLI 中使用 Git Fetch**

要对当前签出的分支执行 Git 提取，从默认上游存储库下载更改，请使用以下命令:

### `git fetch`

在上面的例子中，Git 从远程`origin`为分支`fetch-example`下载了一个变更。如果没有要下载的更改，Git fetch 命令将不会产生任何输出，因为没有什么可以显示。

`git fetch`

**在 CLI 中使用 Git 获取远程分支**

要定位特定遥控器上的特定分支，请使用以下命令:

`git fetch <remote> <branch>`

确保将`<remote>`替换为您想要的遥控器名称，将`<branch>`替换为您想要的分支名称。

在上面的例子中，Git 从远程`origin`为分支`fetch-example`下载了一个变更。如果没有要下载的更改，Git fetch 命令将不会产生任何输出，因为没有什么可以显示。

### **在 CLI 中使用 Git 获取远程分支**

上例中，遥控器是`gitlab`，分支是`fetch-example`。该命令将更新远程跟踪分支:`gitlab/fetch-example`，并更新 FETCH_HEAD。

现在，您可以使用以下命令在本地合并所有更改:

`git merge FETCH_HEAD`

上例中，遥控器是`gitlab`，分支是`fetch-example`。该命令将更新远程跟踪分支:`gitlab/fetch-example`，并更新 FETCH_HEAD。

**如何使用 GitKraken 客户端获取数据**

使用 [GitKraken 客户端](https://www.gitkraken.com/git-client)来完成这个动作的一个最好的部分就是这个工具会自动为你执行获取！

与 Git CLI 不同，GitKraken Client 可以保持与各种远程设备的连接，并在后台以每分钟一次的频率为您执行 Git 获取。您可以在 GitKraken 客户端的 Git commit 图中看到远程跟踪分支的下载更改，如下所示。

## 要将更改应用到本地 repo，只需在提交图上双击要应用更改的远程跟踪分支条目。GitKraken 客户端将要求您确认您想要“将本地重置到此处”点击`Reset Local to Here`按钮，GitKraken 客户端将执行合并。

使用 [GitKraken 客户端](https://www.gitkraken.com/git-client)来完成这个动作的一个最好的部分就是这个工具会自动为你执行获取！

与 Git CLI 不同，GitKraken Client 可以保持与各种远程设备的连接，并在后台以每分钟一次的频率为您执行 Git 获取。您可以在 GitKraken 客户端的 Git commit 图中看到远程跟踪分支的下载更改，如下所示。

要调整 GitKraken 客户端执行自动获取的频率，导航至`Preferences` → `General`。
显示的第一个选项是`Auto-Fetch Interval`。您可以将其调整为每分钟一次，最多 60 分钟一次。您可以通过将频率设置为`0`来禁用自动抓取功能。

要将更改应用到本地 repo，只需在提交图上双击要应用更改的远程跟踪分支条目。GitKraken 客户端将要求您确认您想要“将本地重置到此处”点击`Reset Local to Here`按钮，GitKraken 客户端将执行合并。

GitKraken Client 可以让您轻松地看到您的存储库中正在发生的事情，并从多个远程位置了解最新的变化！像自动取货这样的功能比单独在终端上工作更方便、更省心。

**GitKraken 客户端…就是这样 Fetch**

GitKraken Client 使远程存储库的工作变得简单，无论有多少其他合作者在更新代码。这个传说中的 [Git 工具](https://www.gitkraken.com/)通过自动获取功能使更新变得更加简单，查看代码变化就像点击右边面板上的文件名一样简单。

这只是 GitKraken Client 在与他人合作时，让您作为开发者的生活变得更美好的众多方式之一。[今天就下载并开始使用 GitKraken 客户端](https://www.gitkraken.com/download)，传说中的跨平台 Git 客户端！

GitKraken Client 可以让您轻松地看到您的存储库中正在发生的事情，并从多个远程位置了解最新的变化！像自动取货这样的功能比单独在终端上工作更方便、更省心。

## **GitKraken 客户端…就是这样 Fetch**

GitKraken Client 使远程存储库的工作变得简单，无论有多少其他合作者在更新代码。这个传说中的 [Git 工具](https://www.gitkraken.com/)通过自动获取功能使更新变得更加简单，查看代码变化就像点击右边面板上的文件名一样简单。

这只是 GitKraken Client 在与他人合作时，让您作为开发者的生活变得更美好的众多方式之一。[今天就下载并开始使用 GitKraken 客户端](https://www.gitkraken.com/download)，传说中的跨平台 Git 客户端！