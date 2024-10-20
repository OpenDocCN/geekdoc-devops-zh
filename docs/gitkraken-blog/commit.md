# Git Commit -如何 Git Commit |学习 Git

> 原文：<https://www.gitkraken.com/learn/git/commit>

术语提交是 Git 作为版本控制系统的基础。在学习如何执行 Git commit 命令以及如何在 Git 中执行与提交相关的其他操作之前，首先理解什么是提交是很重要的。

什么是 Git 提交？

## 在 Git 中，提交是您的回购在特定时间点的快照。

为了帮助进一步理解什么是 Git 提交，我们需要回顾一下您的`Working Directory`和`Staging Directory`以及文件更改是如何反映在您的 Git 存储库中的。

把你的工作目录想象成你“进行中”的工作区域；这里，创建或修改的文件还没有反映在您的 Git repo 中。对工作目录中的文件所做的更改只存在于本地计算机上。

Git 阶段文件

为了将工作目录中的更改应用到 Git 存储库中，您必须首先将它们暂存在暂存目录中。

## 在这里，您的更改可以通过执行 Git commit 保存在 repo 中。

为了将工作目录中的更改应用到 Git 存储库中，您必须首先将它们暂存在暂存目录中。

现在，每个 Git 提交将代表您的 repo 在该时间点的快照，并且您的所有提交将汇集在一起形成您的存储库的历史。

Git 工作流

传统上，Git 工作流包括以下步骤:

在你的工作目录中进行修改

**暂存目录中的暂存更改**

**提交变更**以将它们应用到您的 Git 存储库中

## Git 工作流

好了，现在我们已经回答了什么是 Git 提交的问题，让我们深入了解如何使用跨平台 GitKraken Git GUI 进行 Git 提交，以及您可以在 CLI 中使用 [Git 提交命令](https://www.gitkraken.com/learn/git/commands#git-commit-commands)执行的相关操作。

1.  在你的工作目录中进行修改
2.  你每天在 Git 中提交多少次？GitKraken 将把这个过程加快 2.5 倍🤯。不相信我们？你自己试试。⬇️

3.  如何在 GitKraken 中提交

让我们回顾一下使用 GitKraken 可以轻松执行的许多操作，包括如何添加、修改、删除等等。

在 GitKraken 中，当您修改、添加、删除或重命名存储库中的任何文件时，您的工作进展或 WIP 将显示在图形的顶部。

当您在 GitKraken 中单击 WIP 节点时，所有受影响的文件都会在右侧的提交面板中列出。此外，GitKraken 会在每个文件旁边显示一个彩色编码图标来表示变更类型:增加、修改、更名或删除。

你每天在 Git 中提交多少次？GitKraken 将把这个过程加快 2.5 倍🤯。不相信我们？你自己试试。⬇️

## 这非常有用，因为它为您提供了存储库中文件的即时上下文；不需要运行命令！

GitKraken 中的暂存文件

要在 GitKraken 中暂存文件，只需将鼠标悬停在右侧提交面板中的文件名上，然后点击`Stage File`。

当您在 GitKraken 中单击 WIP 节点时，所有受影响的文件都会在右侧的提交面板中列出。此外，GitKraken 会在每个文件旁边显示一个彩色编码图标来表示变更类型:增加、修改、更名或删除。

GitKraken 提供的一个命令行所没有的优点是只能存放文件的单行或大块内容。您可以通过查看文件的差异来实现这一点。

这非常有用，因为它为您提供了存储库中文件的即时上下文；不需要运行命令！

在 GitKraken 中查看文件差异

## 要在 GitKraken 中查看文件的差异，只需在右侧提交面板中单击一个文件。从这里，您可以选择暂存或丢弃单独的代码行或代码块。

要在 GitKraken 中暂存文件，只需将鼠标悬停在右侧提交面板中的文件名上，然后点击`Stage File`。

放弃 GitKraken 中的更改

您可以通过在提交面板上右键单击文件并选择`Discard changes`来放弃在 GitKraken 中对文件所做的所有更改。

GitKraken 提供的一个命令行所没有的优点是只能存放文件的单行或大块内容。您可以通过查看文件的差异来实现这一点。

## 此外，您还可以通过单击“提交”面板左上角的垃圾箱图标🗑来放弃对 WIP 中所有文件所做的所有更改。

要在 GitKraken 中查看文件的差异，只需在右侧提交面板中单击一个文件。从这里，您可以选择暂存或丢弃单独的代码行或代码块。

## 如果你在 GitKraken 中不小心丢弃了什么东西呢？

使用 GitKraken 这样的工具来管理项目的最大好处之一就是能够快速撤销或恢复错误的操作。GitKraken 只需点击一下就能让它变得完全神奇。

如果您错误地放弃更改或文件，只需单击顶部工具栏中的`Undo`按钮即可恢复放弃。

此外，您还可以通过单击“提交”面板左上角的垃圾箱图标🗑来放弃对 WIP 中所有文件所做的所有更改。

在 GitKraken 中添加 Git 提交消息

当您准备好在 GitKraken 中提交您的临时更改时，请确保在单击按钮提交之前，在提交消息字段中键入提交摘要和描述。也可以使用键盘快捷键`Cmd` / `Ctrl` + `Enter`。

## 如果你在 GitKraken 中不小心丢弃了什么东西呢？

如果您错误地放弃了一个更改或文件，只需点击顶部工具栏中的`Undo`按钮即可恢复放弃。

提交更改后，WIP 节点将被转换为提交。在这里，您可以单击您的提交来查看从这个特定时间点开始的更改。

## 用 GitKraken 恢复 Git 提交

类似于你如何恢复你因失误而放弃的东西，GitKraken 使用神奇的`Undo`按钮使[撤销 Git 提交](https://www.gitkraken.com/learn/git/problems/revert-git-commit#undo-git-commit)变得快速而简单。

***GitTip:了解更多关于[如何恢复 Git 提交](https://www.gitkraken.com/learn/git/problems/revert-git-commit)，包括丢弃或修改历史的其他选项。***

提交更改后，WIP 节点将被转换为提交。在这里，您可以单击您的提交来查看从这个特定时间点开始的更改。

在 GitKraken 中修改 Git 提交

## 假设您在 Git 提交消息中犯了一个错误，需要进行更正。您可以通过从中心图中选择提交来修改 GitKraken 中最近提交的提交消息。从这里，单击提交消息开始编辑文本，然后单击`Update Message`保存您的更改。

如果你意识到上一次犯了一个错误，这可能是一个救命稻草。

***GitTip:了解更多关于[如何恢复 Git 提交](https://www.gitkraken.com/learn/git/problems/revert-git-commit)，包括丢弃或修改历史的其他选项。***

提交速度提高 2.5 倍🏎💨与命令行相比，使用 GitKraken。说真的。自己测试一下。⬇️

## Git Status

如果您没有使用功能强大的 GitKraken Git 客户端来可视化您的回购历史，您总是可以在 CLI 中运行`git status`命令来检查哪些文件已经暂存并且存在于您的暂存目录中。

如果你意识到上一次犯了一个错误，这可能是一个救命稻草。

在这个例子中，在执行了`git status`命令之后，您可以看到除了另外两个文件之外，`trial-activation.md`文件也被修改了。

提交速度提高 2.5 倍🏎💨与命令行相比，使用 GitKraken。说真的。自己测试一下。⬇️

如何在命令行中 Git 提交

## 为了在 CLI 中提交这些更改，您需要使用`git add`命令，后跟文件的名称来暂存这些更改。

如果您没有使用功能强大的 GitKraken Git 客户端来可视化您的回购历史，您总是可以在 CLI 中运行`git status`命令来检查哪些文件已经暂存并且存在于您的暂存目录中。

这将暂存`trial-activation.md`文件。啊，但是你还没说完呢！您仍然需要通过执行`git commit`命令将更改保存到您的存储库中。

如何添加 Git 提交消息？

要将 Git 提交消息添加到您的提交中，您将使用`git commit`命令，后跟`-m`标志，然后用引号将您的消息括起来。添加 Git 提交消息应该如下所示:

## 如何修改 Git 提交？

现在，如果你能从上面的例子中看到，最后一个词有一个错别字。那么，如何更改 Git 提交消息呢？

```
git add trial-activation.md
```

为了[修改 Git 提交](https://www.gitkraken.com/learn/git/problems/git-commit-amend)，您将使用`git commit`命令，后跟`--amend`，然后是修改后的 Git 提交消息。它应该是这样的:

## 我可以使用 Git commit 命令添加多个修改过的文件吗？

如果您的工作目录中有许多修改过的文件，那么一次添加一个文件会很费时间。通过使用`git commit`命令后跟`-a`标志来加速该过程。这将把工作目录中所有修改或删除的文件添加到当前提交中。它应该是这样的:

```
git commit -m “Add an anchor for the trial end sectionnn.”
```

准备好用更简单的方式在 Git 中提交代码更改了吗？

## GitKraken Git GUI 将通过其简单的用户界面和清晰的组织结构，让您更有信心在 Git 中暂存、保存和提交文件更改。

现在，如果你能从上面的例子中看到，最后一个词有一个错别字。那么，如何更改 Git 提交消息呢？

为了[修改 Git 提交](https://www.gitkraken.com/learn/git/problems/git-commit-amend)，您将使用`git commit`命令，后跟`--amend`，然后是修改后的 Git 提交消息。它应该是这样的:

```
git commit --amend “Add an anchor for the trial end section.”
```

## 我可以使用 Git commit 命令添加多个修改过的文件吗？

如果您的工作目录中有许多修改过的文件，那么一次添加一个文件会很费时间。通过使用`git commit`命令后跟`-a`标志来加速该过程。这将把工作目录中所有修改或删除的文件添加到当前提交中。它应该是这样的:

```
git commit -a -m “Update trial-activate page with changes from release."
```

## 准备好用更简单的方式在 Git 中提交代码更改了吗？

GitKraken Git GUI 将通过其简单的用户界面和清晰的组织结构，让您更有信心在 Git 中暂存、保存和提交文件更改。