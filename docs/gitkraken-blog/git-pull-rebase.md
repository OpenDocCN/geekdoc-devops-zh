# Git Rebase -什么是 Git Rebase？|学习 Git

> 原文：<https://www.gitkraken.com/learn/git/problems/git-pull-rebase>

什么是 Git rebase？Rebase 是 Git 中的一个动作，它允许你将一个 [Git 分支](https://www.gitkraken.com/learn/git/branch)的[提交](https://www.gitkraken.com/learn/git/commit)重写到另一个分支。本质上，Git rebase 是从一个分支删除提交，并将它们添加到另一个分支。

在本文中，我们将讨论以下与 Git rebase 命令相关的主题:

我们还有其他资源可以了解更多关于 Git rebase 的信息:

在 Git 中重置基可能是一种破坏性的行为。在 GitKraken Client 的帮助下，确保您拥有所需的可见性和控制力。

## **如何在命令行中** Git Rebase

要在终端中开始您的 Git rebase，您可能会从运行 [Git branch](https://www.gitkraken.com/learn/git/branch) 命令来查看您的本地分支列表开始。在下面的例子中，您可以看到当前检出的分支是`feature`，以及另外两个分支:`main`和`production`。

***如果你想在一个当前没有签出的分支上提交，你必须用`git checkout`命令切换到另一个分支。了解如何[检查一个远程 Git 分支](https://www.gitkraken.com/learn/git/problems/git-checkout-remote-branch)和一个[本地 Git 分支](https://www.gitkraken.com/learn/git/problems/switch-git-branch)。***

假设您想要将最近两次提交从`feature`分支重写到`main`分支。因为终端缺乏哪个提交存在于哪个分支的直接可视上下文，所以您将通过运行`git log`命令，然后运行`-2`来开始。

名为`Additional sentence structure changes`的提交是`feature`分支最初从`main`分支分支出来的地方。这将在 Git rebase 完成后改变，所以请注意。

为了继续 rebase，您将使用`git rebase`命令，后跟您的目标分支的名称——在本例中是`main`分支——将您的更改从一个分支移动到另一个分支。

至此，Git rebase 就完成了！Git 已经将来自`feature`分支的提交重写到了`main`分支上最近的提交。

您可以通过引用 GitKraken 客户端的提交图来验证 Git rebase 是否成功。提交图应该显示来自特性分支的提交已经转移到了主分支。

如果您选择 GitKraken CLI 作为您的终端，这就更容易了。因为 GitKraken 客户端结合了 CLI 的强大功能和方便的 GUI 视觉效果，开发人员可以实时看到您的更改如何影响存储库。[下载 GitKraken 客户端](https://www.gitkraken.com/git-client/try-free)亲自体验精简终端和桌面客户端的便利。

## 如何使用 GitKraken 客户端获取 Rebase

GitKraken 图形用户界面的最大好处之一是您的回购信息的显示方式。正如您在这里看到的，GitKraken 客户端在中央提交图中清晰地显示了您的提交历史。这给了你一个即时的快照，这样你就可以很快理解你想要重定基础的分支发生了什么。

在这个例子中，假设您有一个`feature`分支，您需要将变更合并到`dev`分支上。要在 GitKraken 中执行重设基础，只需将`feature`拖放到`dev`上，然后点击上下文菜单中的`Rebase feature onto dev`选项。

使用 GitKraken 客户端
只需点击 2 次即可进行 Git rebase

感觉你已经掌握了 Git rebase 的窍门？升级到 [Git interactive rebase](https://www.gitkraken.com/learn/git/problems/git-interactive-rebase) 。

不要在命令上浪费时间，把更多的时间用在…想做什么就做什么。今天免费下载跨平台 [GitKraken Git 客户端](https://www.gitkraken.com/git-client)。

Git Pull Rebase

## Git pull rebase 是一种将本地未发布的更改与远程上最新发布的更改相结合的方法。

假设您有一个包含未发布变更的项目主分支的本地副本，并且该分支是 origin/main 分支之后的一个提交。

Git 有两种方法来合并这种性质的更改:Git pull rebase 和 Git pull merge。

Git 有两种方法来合并这种性质的更改:Git pull rebase 和 Git pull merge。

Git Pull Rebase 与 Git Pull Merge

## 那么，Git 拉 rebase 和 Git 拉 merge 有什么区别呢？虽然这两个选项都将结合从您的远程获取的更改，但是在您的 Git 历史中，结果看起来会非常不同。

Git 拉合并是在 Git 中合并变更的默认方法，它会将未发布的变更与已发布的变更合并，从而导致合并提交。

另一方面，使用 Git pull rebase，未发布的更改将被重新应用到已发布的更改之上，并且不会有新的提交被添加到您的历史中。

记住这一点，您可以看到 Git pull rebase 通过删除不需要的合并提交，将会产生一个线性的、更清晰的项目历史。

我们将带您了解如何使用 CLI 和著名的跨平台 GitKraken 客户端执行 Git pull rebase。

我们将带您了解如何使用 CLI 和著名的跨平台 GitKraken 客户端执行 Git pull rebase。

如何在命令行中 Git Pull Rebase

## 要在 CLI 中执行 Git pull rebase，首先要导航到本地 repo 并执行以下命令:

`git pull --rebase`

如果 Git 在 rebase 中没有检测到任何冲突，您应该会看到消息:`Successfully rebased and updated refs/heads/main.`但是，如果 Git 检测到冲突，您未发布的更改将不会被应用。

如果 Git 在 rebase 中没有检测到任何冲突，您应该会看到消息:`Successfully rebased and updated refs/heads/main.`但是，如果 Git 检测到冲突，您未发布的更改将不会被应用。

如何使用 GitKraken 客户端获取 Rebase

## 要开始 GitKraken 客户端中的 Git pull rebase 过程，您将从选择顶部工具栏中`Pull`旁边的向下箭头▼开始。

要开始 GitKraken 客户端中的 Git pull rebase 过程，您将从选择顶部工具栏中`Pull`旁边的向下箭头▼开始。

从下拉菜单中选择`Pull (rebase)`。

(你可以点击那个选项旁边的圆圈，让`Pull (rebase)`成为你在 GitKraken 客户端拉取的默认操作。)

(你可以点击那个选项旁边的圆圈，让`Pull (rebase)`成为你在 GitKraken 客户端拉取的默认操作。)

仅此而已。说真的，就这么简单。

您可以看到该图现在是线性的，并且易于导航，无需任何合并提交！只需两次点击，您就可以在 GitKraken 中执行 Git pull rebase，并具有完整的可视上下文。

Git Rebase FAQ

问:Git Rebase 是做什么的？

答:Git rebase 从一个 Git 分支获取提交，并将它们添加到另一个分支。将此视为 Git 的“剪切-粘贴”操作可能会有所帮助。当您 Git rebase 时，您实际上是从一个分支中删除提交，并将它们添加到另一个分支中。

## 问:如何重定 Git 的基数？

### 答:要明确的是，Git 是一个版本控制软件，可以让你跟踪你的文件。Git rebase 是 Git 中可用的一个操作，它允许您在 Git 分支之间移动文件。有关如何 Git rebase 的分步说明，请参见以上章节，[如何在命令行中 Git Rebase](https://www.gitkraken.com/learn/git/git-rebase#How-to-Git-Rebase-in-the-Command-Line)或[如何在 GitKraken 客户端中 Git Rebase](https://www.gitkraken.com/learn/git/git-rebase#How-to-Git-Rebase-with-GitKraken-Client)。

答:Git rebase 从一个 Git 分支获取提交，并将它们添加到另一个分支。将此视为 Git 的“剪切-粘贴”操作可能会有所帮助。当您 Git rebase 时，您实际上是从一个分支中删除提交，并将它们添加到另一个分支中。

问:如何重定 Git 的基数？

### 答:要明确的是，Git 是一个版本控制软件，可以让你跟踪你的文件。Git rebase 是 Git 中可用的一个操作，它允许您在 Git 分支之间移动文件。有关如何 Git rebase 的分步说明，请参见以上章节，[如何在命令行中 Git Rebase](https://www.gitkraken.com/learn/git/git-rebase#How-to-Git-Rebase-in-the-Command-Line)或[如何在 GitKraken 客户端中 Git Rebase](https://www.gitkraken.com/learn/git/git-rebase#How-to-Git-Rebase-with-GitKraken-Client)。

A: To be clear, Git is a version control software that allows you to track your files. Git rebase is an action available in Git that allows you to move files between Git branches. For step-by-step instructions regarding how to Git rebase, see the above sections, [How to Git Rebase in the Command Line](https://www.gitkraken.com/learn/git/git-rebase#How-to-Git-Rebase-in-the-Command-Line) or [How to Git Rebase in GitKraken Client](https://www.gitkraken.com/learn/git/git-rebase#How-to-Git-Rebase-with-GitKraken-Client).