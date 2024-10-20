# Git 合并-合并分支以合并更改|学习 Git

> 原文：<https://www.gitkraken.com/learn/git/git-merge>

在 Git 中工作时，merge 动作用于将一个分支的更改合并到另一个分支，比如 Git merge to master。

合并也有助于保存你的回购历史。特别是在比较 [Git merge 和 rebase](https://www.gitkraken.com/learn/git/problems/git-rebase-vs-merge) 时，合并保留了更完整的历史，并且更容易撤销错误。

在介绍如何在 CLI 中使用 Git merge 命令之前，我们将先介绍如何使用传奇的跨平台 GitKraken Git 客户端进行合并。

使用 GitKraken
更快地合并您的分支并进行更多控制

## **在 GitKraken 怎么合并？**

GitKraken 作为 Git GUI 提供的最大好处之一是能够快速可视化存储库中的所有分支，并通过拖放操作直观地管理这些分支。

在这个例子中，我们有一个 dev 分支，它包含了我们希望合并到 production 分支中的变更。要开始 GitKraken 中的 Git 合并分支过程，您可以简单地将 dev 分支拖放到 production 分支上。

现在，如果你还没有签出 dev 分支，GitKraken 会问你是否想先签出这个分支。看，告诉过你这是直觉。😉继续前进…

在您将带有变更的分支拖放到您的目标分支上之后，您可以从上下文菜单中点击`Merge dev into production`选项。如果没有冲突，合并提交将会完成，并且您的所有更改将会合并到生产分支中。

## 在 GitKraken，你如何解决合并冲突？

现在，如果你遇到一个 [Git 合并冲突](https://www.gitkraken.com/learn/git/tutorials/how-to-resolve-merge-conflict-in-git)，GitKraken 会暂停合并并立即通知你发现了一个文件冲突。

更好的是，GitKraken 的合并冲突工具将指导您完成解决过程，为您提供决定保留和放弃哪些更改所需的所有信息。

在主 GitKraken UI 右侧的提交面板中，单击任何冲突文件进入 GitKraken 合并冲突工具。

每个冲突部分都会出现一个相应的复选框。选中某个部分的复选框会将其添加到合并冲突工具底部的输出部分。这允许您在决定如何最好地继续之前，看到所有可用选项的完整上下文。

一旦您为每个文件冲突做出了选择，您就可以继续保存和完成 Git 合并。

揭示黑暗世界与恶魔的合并冲突

## 如何在命令行中进行 Git 合并？

如果您使用 CLI 来合并 Git 中的变更，典型的工作流将通过运行`git status`命令来检查您的特性分支上的任何未决变更。

在这个例子中，有一些更改需要提交，所以让我们从这里开始。您将从暂存文件开始:

```
git add <file name>
```

后跟一个提交:

```
git commit -m “<commit message>”
```

***Git 提示:如果你是 Git 新手，在进入合并之前，先熟悉一下[如何提交变更](https://www.gitkraken.com/learn/git/commit)。***

现在我们已经提交了我们的变更，我们准备将特性分支合并到开发分支中。

但是在我们可以在命令行中进行 Git merge 之前，我们必须检查我们希望将我们的更改合并到其中的目标分支，在本例中是`dev`。

***GitTip:不确定如何检查一个 Git 分支？学习[如何切换本地分行](https://www.gitkraken.com/learn/git/problems/switch-git-branch)，以及[如何结账远程分行](https://www.gitkraken.com/learn/git/problems/git-checkout-remote-branch)。***

在这个例子中，命令行告诉我们 dev 分支落后了。在这种情况下，您将希望在进行合并之前执行 Git pull 来获得最新的更新。

```
git checkout dev
```

在这个例子中，命令行告诉我们 dev 分支落后了。在这种情况下，您将希望在进行合并之前执行 Git pull 来获得最新的更新。

***GitTip:学习如何[拉一个远程 Git 分支](https://www.gitkraken.com/learn/git/problems/pull-remote-git-branch)来保持你的本地分支是最新的。***

好了，现在您已经准备好进行 Git 合并了。在命令行中，您将使用 Git merge 命令，后跟包含您的更改的分支。

接下来，您将看到一个提示，询问合并提交描述。在这里，您可以编辑默认的提交消息，或者通过保存并关闭消息编辑器来完成命令行中的合并。

```
git merge feature
```

接下来，您将看到一个提示，询问合并提交描述。在这里，您可以编辑默认的提交消息，或者通过保存并关闭消息编辑器来完成命令行中的合并。

在 Git 中获得对合并的更多控制，永远不用担心会产生无法解决的合并冲突。GitKraken 是获得更好的合并体验的途径；今天就免费下载传说中的用于 Windows、Mac、& Linux 的[跨平台 Git GUI](https://www.gitkraken.com/git-client) 。

在 Git 中获得对合并的更多控制，永远不用担心会产生无法解决的合并冲突。GitKraken 是获得更好的合并体验的途径；今天就免费下载传说中的用于 Windows、Mac、& Linux 的[跨平台 Git GUI](https://www.gitkraken.com/git-client) 。