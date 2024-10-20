# 如何解决 Git |高级 Git 教程中的合并冲突

> 原文：<https://www.gitkraken.com/learn/git/tutorials/how-to-resolve-merge-conflict-in-git>

#### **************高级饭桶教程**************

[https://www.youtube.com/embed/MzpW-k66XE8?feature=oembed](https://www.youtube.com/embed/MzpW-k66XE8?feature=oembed)

视频

GitKraken 的应用内合并冲突工具将允许您执行更高级的 Git 操作，而没有相关的风险-您的解决方案只需点击几下鼠标。

## **Git 合并冲突**

观看这个高级 Git 教程视频，了解更多关于 Git 中的合并冲突以及它们何时发生的信息。

当 Git 无法自动解决两次提交之间的代码差异时，因为对同一行代码有冲突的更改，合并冲突就发生了。当[合并 Git 分支](https://www.gitkraken.com/learn/git/problems/merge-git-branch)、[重定分支](https://www.gitkraken.com/learn/git/problems/git-rebase-branch)或挑选提交时，Git 中的合并冲突可能发生。

了解如何与 Git 沟通以解决合并冲突，并继续进行您的 [Git 合并](https://www.gitkraken.com/learn/git/git-merge)、 [Git rebase](https://www.gitkraken.com/learn/git/git-rebase) 或 [Git cherry pick](https://www.gitkraken.com/learn/git/cherry-pick) 。

## **什么是 Git 合并冲突？**

当在 Git 中工作时，用户可以通过一个称为合并的操作来合并来自两个不同分支的提交。除非有冲突的变更集，否则文件会自动合并(即提交以不同的方式更新同一行代码)。

当 Git 无法自动解决两次提交之间的代码差异时，就会发生合并冲突。

当代码中的所有更改发生在不同的行或不同的文件中时，Git 将在没有您的帮助下成功合并提交。

然而，当同一行上有冲突的变更时，就会发生“合并冲突”，因为 Git 不知道保留哪些代码，丢弃哪些代码。

## 什么时候会发生 Git 合并冲突？

当合并分支、重定分支或挑选提交时，可能会发生合并冲突。

如果 Git 检测到冲突，它会突出显示冲突区域，并询问您希望保留哪个代码。

一旦您告诉 Git 您想要的代码，您就可以保存文件，并继续进行合并、rebase 或 cherry pick。

无论您的合并冲突是发生在合并、重定基础还是挑选过程中，GitKraken 都会引导您快速轻松地解决冲突。

## 如何用 GitKraken 解决 Git 中的合并冲突？

虽然命令行中的合并冲突可能会令人生畏，但使用 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 却是小菜一碟。

假设我们有两个分支修改了同一个文件中的同一行。当您拖放执行合并时，GitKraken 会检测到合并冲突，并通知您它需要您的帮助。

单击查看冲突以查看冲突文件的列表。然后，当您单击一个文件时，它将打开合并工具，向您显示两个分支之间的冲突更改，并在底部显示输出视图。您可以选中您希望保留的每一行代码的复选框，或者一次选择一行代码。

您甚至可以在输出框中键入内容，进一步微调您的代码。一旦您对更改感到满意，点击保存退出合并工具，并继续下一个冲突的文件。解决所有冲突文件后，您可以继续进行合并。

准备好在 Git 中更容易地解决合并冲突了吗？跨平台 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 拥有您在项目和组织中安全扩展 Git 所需的健壮特性。