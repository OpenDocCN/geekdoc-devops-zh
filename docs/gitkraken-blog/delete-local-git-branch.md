# Git 删除本地分支|如何在 Git 中删除本地分支

> 原文：<https://www.gitkraken.com/learn/git/problems/delete-local-git-branch>

更新日期:2022 年 7 月 20 日

其核心是，Git 提供的分支模型旨在帮助您避免将不稳定的代码合并到主代码库中。大多数 Git 工作流和 [Git 分支策略](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy)要求开发人员在开发过程的某个时候删除 [Git 分支](https://www.gitkraken.com/learn/git/branch)；通常是在一个分支机构并入主分支机构之后。

删除 Git 分支通常被认为是良好的[存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)卫生，因为它加快了您的回购性能，并确保只有最新和必要的信息包含在您的项目中。

在本文中，我们将介绍如何使用 CLI 和 [GitKraken 客户端](https://www.gitkraken.com/git-client)删除本地分支。

## 使用 CLI & GitKraken 客户端查看您的 Git 分支

在删除本地 Git 分支之前，您需要获得想要删除的分支的确切名称。

要通过 [CLI](https://www.gitkraken.com/cli) 访问项目中本地 Git 分支的完整列表，请键入`git branch`并点击`Enter`。终端将返回 Git repo 中所有本地分支的列表。

我们在这个例子中使用了 GitKraken Client，但是需要注意的是，如果您使用的是 GitKraken Client 以外的终端，您将只能看到终端本身包含的信息。

使用 GitKraken Client，您可以将 CLI 的速度和效率与 GUI 中提供的便捷视觉效果结合起来。这意味着您可以利用内置的终端，并且只需浏览一下客户端极其强大的提交图，就可以轻松地可视化您的 Git 分支。提升您的工作流程，[免费试用 GitKraken 客户端](https://www.gitkraken.com/git-client/try-free)！

*了解有关查看您的* [*Git 分支列表*](https://www.gitkraken.com/learn/git/problems/git-branch-list) 、*的更多信息，包括如何从 CLI & GitKraken 客户端查看您的本地和远程 Git 分支。*

一旦您确定了想要删除的 Git 分支的确切名称，您就可以实际删除该分支了。

## Git 使用 CLI 删除本地分支

要使用终端删除本地 Git 分支，请运行以下命令:

`git branch -d <branch name>`.

请记住，如果您使用的是 GitKraken Client 以外的终端，您将不会立即看到 Git 分支已经从存储库中正确删除的确认。相反，您必须再次运行`git branch`来查看您的 Git 分支列表，并确保想要的 Git 分支已经被删除。

## Git 使用 GitKraken 客户端删除本地分支

正如我们已经提到的，使用 GitKraken 客户端来可视化您的存储库有很大的好处。

如果您想使用 [GitKraken 客户端](https://www.gitkraken.com/git-client)Git 删除一个本地分支，只需在左侧面板或中央图形中右键单击一个分支名称并选择`Delete <branch name>`。

Git 误删了一个本地分支？不要害怕。你可以随时使用 GitKraken 客户端顶部工具栏中的神奇按钮`Undo`,让它看起来像从未发生过的错误。

想在 Git 中快速简便地删除本地分支吗？免费下载 [GitKraken 客户端](https://www.gitkraken.com/git-client)。

*一旦你习惯了删除本地 Git 分支，通过学习* [*如何使用 CLI & GitKraken 客户端删除远程 Git 分支*](https://www.gitkraken.com/learn/git/problems/delete-remote-git-branch) *来继续提高你的 Git 技能。*

Git 删除本地分支常见问题

## 问:为什么我不能删除一个本地 Git 分支？

### 答:如果您签出了一个本地分支，Git 不允许您删除该分支。如果您在删除 Git 分支时遇到问题，请确保您签出的分支不是您想要删除的分支。

答:如果您签出了一个本地分支，Git 不允许您删除该分支。如果您在删除 Git 分支时遇到问题，请确保您签出的分支不是您想要删除的分支。

问:我如何删除带有未合并变更的本地分支？

### 答:要删除包含未合并变更的本地 Git 分支，您需要运行:

`git branch -D <branch name>`

这告诉 Git 你对删除这个分支是认真的。但是要注意！使用`-D`标志通常会使数据很容易丢失，所以要小心使用。

这告诉 Git 你对删除这个分支是认真的。但是要注意！使用`-D`标志通常会使数据很容易丢失，所以要小心使用。