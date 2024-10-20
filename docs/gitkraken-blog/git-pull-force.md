# 快去拉克兰

> 原文：<https://www.gitkraken.com/learn/git/problems/git-pull-force>

Git pull 命令允许您从另一个 repo 或本地分支获取数据并与之集成。

在您的工作流程中，有时您可能希望强制覆盖本地分支的历史记录和文件，以便与远程分支相匹配。虽然 Git 有办法做到这一点，但实际上您不会使用 Git pull 命令。

在讲述如何在命令行中完成相同的事情之前，我们将带您了解如何使用跨平台 GitKraken Git GUI 强制执行 pull 来覆盖您的本地分支历史。

## **你为什么会想要 Git 拉力？**

在您的工作流中可能会出现一些情况，鼓励您强制覆盖您的本地分支。

比方说，您的遥控器上的历史记录与您的本地副本相比发生了巨大的变化；简单地进行 Git 拉取可能会产生非常混乱的历史、代码冲突，或者两者都有。另一方面，你可能对你的本地回购协议做了许多现在已经无关紧要的改变；也许你和一个团队成员错误地在同一个分支上处理一个 bug，他们先解决了它。

简而言之，如果你不关心我们的本地副本的历史，而只想尽快用你的遥控器让你的回购“恢复速度”，你会想要执行 Git pull force。

如果没有 GitKraken 提供的可视化上下文，在覆盖本地更改时知道您的目标是哪个远程分支可能是一个挑战。

## 如何用 GitKraken 获得拉力？

在这个 Git pull force 的例子中，假设您有一个分支的本地副本，它在远程版本的后面。这是中心 GitKraken 图中的样子:

假设您只想用远程分支所做的任何更改来覆盖本地分支。您不希望执行 Git pull，因为这样一来，您的本地副本将同时包含您不希望看到的本地历史和远程更改。

Git pull 动作实际上是另外两个 Git 命令的系列:一个 Git fetch 后面跟着一个 [Git merge](https://www.gitkraken.com/learn/git/git-merge) 。

在这个用例中，Git merge 操作是阻止 Git pull 操作以期望的方式执行的部分；它实际上是将远程分支合并到本地分支，而不是覆盖更改，保留了两个组的*更改。这也可以为一些龌龊的 [饭桶合并](https://www.gitkraken.com/learn/git/tutorials/how-to-resolve-merge-conflict-in-git)的冲突打开大门。😱*

GitTip:查看 Git pull 和 Git fetch 之间的 [差异，理解每个命令是如何工作的。](https://www.gitkraken.com/learn/git/problems/git-pull-vs-fetch)

那`git pull --force`呢？虽然看起来这个命令会产生一个 Git pull force，但是将`--force`标志添加到 Git pull 命令实际上只是将标志传递给了`git fetch`，使其成为一个 Git fetch force。

所以这实际上会产生与单独使用`git pull`相同的结果。

要强制您的本地分支被 GitKraken 中的远程覆盖，您将采取以下步骤:

1.fetch——你可以通过点击顶部工具栏中方便的``Fetch``按钮在 GitKraken 中执行 Git fetch。

您也可以在 GitKraken 中通过右键单击左侧面板中的遥控器来执行 Git 获取。

2.直接在图形中双击远程 ref。

3.出现提示时，选择 GitKraken UI 顶部的``Reset Local to Here``。

嘣！💥任务完成！您已经使用 GitKraken Git 客户端强制覆盖了您的本地历史。是的，真的很简单。

您可以通过注意到您的本地分支与您的中心图中的远程历史相匹配来验证操作是否成功执行。

GitKraken 在覆盖 Git 中的本地罚款和更改时，提供了许多优于命令行的好处，包括更好地可视化表示正在执行的操作和分支。

强制覆盖本地更改可能是一个危险的操作。GitKraken 的一键撤销/重做保护开发者免于常见错误。

## 如何在命令行中获得拉力？

在 CLI 中强制本地分支被远程覆盖的过程类似。

1.  您将从执行 Git 获取开始。

2.接下来，您需要为您想要从中提取更改的远程分支找到提交 SHA:

`git rev-parse <remote-branch-name>`

(`git rev-parse`比跑`git log <remote-name>`或`git log <branch-name>`快)

3.然后您将使用`git reset --hard`,后跟遥控器的当前提交 SHA。这会将您的本地分支重置为远程分支。

🚨警告🚨`git reset --hard`命令被认为是高度危险的，因为它将破坏本地分支上的所有工作，包括已提交和未提交的更改。因此，在执行此操作之前，一定要确保您希望放弃本地分支的更改。

GitKraken 包括破坏性操作的验证提示，如 Git 硬重置，因此您可以避免灾难性的错误。

4.此时，因为您缺少 GitKraken 图提供的可视化上下文，所以您需要验证您的本地分支和远程分支是否指向相同的提交。

`git diff <local-branch-name> <remote-branch-name>`

如果它们*而不是*指向同一个提交，您将会看到类似这样的内容:

有时候，如果你离遥控器太远，在本地覆盖你的修改会更快。借助于用于 Windows、Mac、& Linux 的跨平台 [GitKraken Git 客户端](https://www.gitkraken.com/git-client)，强制覆盖本地更改并不一定是令人生畏或具有破坏性的。