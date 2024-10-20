# 什么时候 Git rebase vs merge？Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/git-rebase-vs-merge>

在 Git 存储库中工作时，有时您需要将一个分支的变更合并到另一个分支。Git rebase 和 Git merge 都是可以用来完成这个任务的工具。

当比较 Git rebase 和 merge 时，您可以看到相关的好处和坏处。

Git Rebase 的优势:

*   创建更清晰的回购历史记录
*   获得可读性更好的提交图

Git 重定基数的风险:

使得重定基数过程中出现的冲突更难解决

*   Git 合并的好处:

保留完整的回购历史记录

更容易解决重定基数过程中出现的冲突

*   如果犯了错误，更容易撤销
*   Git 合并的风险:
*   如果您的存储库有许多合并的分支，可能会导致混乱的图形

因为在 Git 中进行合并的风险较小，所以如果您有疑问，最好使用 Git merge 而不是 Git rebase。

*   什么时候应该使用 Git merge？

比方说，我们有一个**主**分支，然后我们分支到一个**特性**分支，进行更多的变更。

在这个实例中，您可以通过执行 Git 合并，将来自您的**特性**分支的变更合并到您的**主**分支。

## 什么时候应该使用 Git merge？

***GitTip:逐步学习如何[使用命令行合并 Git 分支](https://www.gitkraken.com/learn/git/problems/merge-git-branch)，然后将体验与 GitKraken Git GUI 进行比较。***

什么时候应该使用 Git rebase？

如果您正在处理许多分支，并且需要合并来自不同分支的变更，那么您的提交图可能会变得很难阅读。

## 在这种情况下，重定基准可以作为合并的有用替代方案。Git rebase 允许一个更干净的图，因为它从一个分支获取提交，并将它们放到另一个分支。这通过将提交及其更改移动到目标分支上来改变图中的树结构。

如果您正在处理许多分支，并且需要合并来自不同分支的变更，那么您的提交图可能会变得很难阅读。

**Git Merge 与 gitkraken 中的 rebase**

无论您选择执行哪种操作，使用可视化 Git 客户端来管理合并文件更改的过程都会让您安心。与在终端中不同的是，使用 GitKraken 可以很容易地识别和解决由于合并或重定基础而产生的冲突。

## **走走走走走走走走走走走走走走走走走走走走**

GitKraken 使分支之间的变更合并过程变得快速而直观。您可以简单地将一个分支从中心图拖放到目标分支上，以启动合并。

**去 gitkraken 中的 rebase**

类似于如何启动 Git 合并，您将通过将一个分支拖放到目标分支上，从上下文菜单中选择`Rebase onto` 来启动 Git rebase 过程。

就像魔术一样，您将看到来自第一个分支的提交在目标分支之上被重新创建，让您确信您正确地执行了 rebase。

最精彩的部分？GitKraken 会自动提醒您在合并或重置过程中是否出现冲突，并让您有机会使用内部的[合并冲突编辑器](https://support.gitkraken.com/working-with-repositories/branching-and-merging/#merge-conflict-editor)完全控制解决问题。

别紧张。在 Git 中执行合并或重置时获得更多的信心和更多的控制，让强大的 GitKraken 袖手旁观来处理过程中出现的任何冲突。立即免费下载适用于 Linux、Mac 和 Windows 的跨平台 GitKraken Git 客户端。

就像魔术一样，您将看到来自第一个分支的提交在目标分支之上被重新创建，让您确信您正确地执行了 rebase。

最精彩的部分？GitKraken 会自动提醒您在合并或重置过程中是否出现冲突，并让您有机会使用内部的[合并冲突编辑器](https://support.gitkraken.com/working-with-repositories/branching-and-merging/#merge-conflict-editor)完全控制解决问题。

别紧张。在 Git 中执行合并或重置时获得更多的信心和更多的控制，让强大的 GitKraken 袖手旁观来处理过程中出现的任何冲突。立即免费下载适用于 Linux、Mac 和 Windows 的跨平台 GitKraken Git 客户端。