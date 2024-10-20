# 如何撤销 Git 合并？Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/undo-git-merge>

它发生在我们最好的人身上。您刚刚进行了代码更改，提交了它们，并将这些更改合并到您的主分支中。*乐喘气！事后你意识到你不应该做那件事。*

当这种情况发生时，您将需要一种方法来让您的主分支返回到它以前的状态。谢天谢地，Git 有这样的工具。

我们将首先展示如何使用跨平台 GitKraken Git GUI 撤销 Git 合并，然后讨论如何使用命令行撤销 Git 中的合并。

您过去是否进行过不恰当的合并，并且不确定如何修复它？GitKraken 使撤销合并和解决冲突变得直观，所以你不用太担心。

## 如何使用 GitKraken 撤销 Git 合并？

使用 GitKraken Git 客户端撤销 Git 合并非常简单且直观，因为您可以在同一个 UI 中查看 repo 中的每个分支。很容易识别何时发生了不适当的合并，并且只需点击几下就可以撤销。[立即下载#1 免费 Git GUI】并继续阅读以了解如何撤销 Git 合并。](https://www.gitkraken.com/download)

在 GitKraken 的中心图中，只需双击您的更改被错误合并的分支。这将[检验 Git 分支](https://dev2.gitkraken.com/learn/git/problems/switch-git-branch)。

在图形中查找上一个提交，或您希望重置为的另一个提交。然后从上下文菜单中选择`Reset <branch name> to this commit` > `Hard - discard all changes`。

现在，如果您已经将想要撤销的合并变更推送到您的远程存储库中，您可以右键单击合并提交并从上下文菜单中选择`Revert commit`。

然后会询问您是否想要立即创建一个提交。

一旦进行了新的提交以恢复您的合并更改，您可以通过从上下文菜单中选择`Push`将提交推送到远程存储库。

***GitTip:全面了解[如何在 Git](https://www.gitkraken.com/learn/git/problems/revert-git-commit) 中恢复提交，以及如何安全地修改历史记录。***

如果您刚刚开始使用 Git，撤销合并和恢复提交可能听起来很可怕，但是 GitKraken 使复杂的 Git 过程变得简单而安全。

## 如何在命令行中撤销 Git 合并？

与 GitKraken 不同，如果您已经将更改推送到遥控器，那么在 CLI 中撤销 Git 合并可能会有点麻烦。如果新的变更被添加到目标分支，这个过程也会变得复杂，因为您已经意识到需要撤销您的合并。GitKraken Git GUI 以清晰的布局提供了关于您的回购历史的详细可视上下文，使您更容易确定合并发生的时间，并允许您快速撤销合并，而不会冒进一步错误的风险。让看似复杂的撤销合并过程变得更容易、更安全:[现在就下载 GitKraken 它是免费的！](https://www.gitkraken.com/download)

在这个撤销 Git 合并的例子中，我们将回顾如何撤销 Git 合并的基本步骤，以及一些额外的步骤来处理更复杂的情况。

要在 CLI 中撤消 Git 合并，首先要检查您将更改合并到的分支。

```
git checkout <branch-name>
```

从这里开始，您将需要获得重置分支所需的提交的 ref。您将使用`git reflog`来完成这项工作。

```
git reflog show --all
```

在运行`git reflog`之后，您将识别您想要返回的提交，并复制提交 SHA。

此时，您需要验证您的合并提交还没有被推送到您的远程。如果是这种情况，您将使用`git reset`命令向前移动。

```
git reset --hard <merge-commit-sha>
```

***GitTip:如果您碰巧有任何未提交的更改，请确保在运行 Git reset 之前使用 [Git stash](https://dev2.gitkraken.com/learn/git/git-stash) 命令。***

## 如何在推送更改后撤销 Git 合并？

如果您已经将合并提交推送到远程存储库，您将需要进行新的提交来恢复更改。

```
git revert -m 1 <merge-commit-sha>
```

这将创建一个新的提交，该提交反转合并提交的更改。

使用传奇的 GitKraken Git 客户端，撤销 Git 合并的过程要简单得多，步骤也少得多—[Windows、Mac 和 Linux 免费下载](https://www.gitkraken.com/download)。