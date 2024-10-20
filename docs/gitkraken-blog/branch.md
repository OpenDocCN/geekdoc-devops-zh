# Git 分支-如何分支|学习 Git

> 原文：<https://www.gitkraken.com/learn/git/branch>

在 Git 中，关于分支有一些常见的误解。例如，有些人假设一个分支是一组提交，或者一个分支必须在 Git 图中分叉。

事实上，分支只是一个指向特定提交的指针。

分支指针随着您所做的每个新的提交而移动，并且仅当在公共祖先提交上进行提交时才在图中分叉。

与其他版本控制系统相比，Git 提供的分支模型是轻量级的，有助于保护您不将不稳定的代码合并到主代码库中，并让您有机会在合并到主分支之前清理您的历史。

* * *

## 如何创建一个 Git 分支？

在 Git 中创建新的分支既快又简单。

您可以从提交历史中的任何提交创建 Git 分支；如果您想要从项目中的上一点开始进行更改，这可能非常有用。

如果您想使用终端创建一个 Git 分支，您将使用`git branch`命令，后跟您想要的分支名称。这将在您当前签出的引用上创建一个 Git 分支。

了解更多关于[如何在 Git](https://www.gitkraken.com/learn/git/problems/create-git-branch) 中创建分支，包括如何使用一个命令同时创建和签出您的新分支。

## 如何重命名 Git 分支？

您可以随时轻松地重命名 Git 分支。

要使用终端在本地重命名 Git 分支，您将使用`git branch -m`后跟所需的新分支名称。

但是，如果您试图重命名已经推送到远程的分支，您将需要将新分支推送到远程，并使用带有`-u`(或`--set-upstream`)选项的`git push`命令更新上游。

参见分步[如何重命名 Git 分支](https://www.gitkraken.com/learn/git/problems/rename-git-branch)，包括如何重命名您没有签出的本地 Git 分支。

## 如何删除 Git 分支？

如果您决定不再需要它，可以删除一个 Git 分支来清理您的存储库。

要使用终端删除 Git 分支，您需要使用`git branch -d`命令以及您想要删除的本地分支的名称。了解更多关于如何在本地删除 Git 分支的信息。

要删除 Git 中的远程分支，您实际上要使用`git push`命令。了解更多关于如何在 Git 中删除远程分支的信息。

## 如何切换 Git 分支？

随着 Git 存储库中分支数量的增长，很可能会有一天，您将同时处理多个任务，并且需要能够从一个分支转移到另一个分支。

为了[切换到 Git](https://www.gitkraken.com/learn/git/problems/switch-git-branch) 中的另一个分支，您必须使用`git checkout`命令来签出该分支。

## **用 GitKraken 在 Git 中分支**

现在，让我们回顾一下当您使用豪华的 GitKraken Git 客户端来可视化您的存储库时，分支是如何工作的。

当您有了历史的可视化表示时，理解 Git 中分支的基础就容易多了。

在这个例子中，我们使用了两个 Git 分支:`dev`和`production`；为了开始一个新特性的工作，我们需要从`dev`分支的最近提交中创建一个特性分支。

## **如何用 GitKraken 创建 Git 分支？**

要使用 GitKraken 创建 Git 分支，右键单击目标 commit 并从菜单中选择`Create branch here`。

## **如何用 GitKraken 重命名 Git 分支？**

要使用 GitKraken 重命名 Git 分支，只需右键单击分支名称并选择`Rename [branch name]`。

## **如何用 GitKraken 删除 Git 分支？**

要删除 GitKraken 中的 Git 分支，在图中右键单击分支名称并选择`Delete [branch name]`。

### 准备好用更简单的方法在 Git 中进行分支了吗？

GitKraken Git 客户端通过其图形用户界面使 Git 更快、更直观。