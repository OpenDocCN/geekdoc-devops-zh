# 如何创建 Git 分支？Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/create-git-branch>

[https://www.youtube.com/embed/snxybJkFeUo?feature=oembed](https://www.youtube.com/embed/snxybJkFeUo?feature=oembed)

视频

在讨论如何使用 CLI 创建 Git 分支之前，我们将使用跨平台 GitKraken Git GUI 提供的可视化上下文来回顾一下更简单的过程。

在普通的工作流程任务上花更少的时间，比如创建新的分支，这样你就可以把精力放在困难的事情上。

## **如何在 GitKraken 创建分支机构？**

要在 GitKraken 中创建新的 Git 分支，只需右键单击任何分支或提交并选择`Create branch here`。

ProTip: GitKraken 会在分支创建后立即自动为您检出分支，因此您可以直接处理正确的文件。

或者，您可以使用`Cmd/Ctrl + P`启动 GitKraken 模糊查找器并键入“创建分支”。这将使您直接跳转到 create branch 字段，找到您当前已经签出的引用。

在终端管理分支机构可能会变得很麻烦。让 GitKraken 做组织工作，这样您就可以专注于代码。

## 如何在命令行中创建 Git 分支？

如果您使用终端，您将使用`git branch`命令，后跟您想要的分支名称，在您的存储库中创建一个 Git 分支。

它应该是这样的:

```
git branch feature-A
```

这将在您当前签出的引用上创建一个 Git 分支。

## 如何查看你的 Git 分支列表？

如果您没有使用 GitKraken，您将无法在主 UI 中看到您的存储库的所有分支。如果您正在使用 CLI，您可以运行`git branch`命令来查看您的本地分支列表，并确认您新创建的分支是否出现。

## 如何检验一个新的 Git 分支？

创建 Git 分支后，您可以使用 [Git checkout](https://www.gitkraken.com/learn/git/git-checkout) 命令，后跟分支名称来检验该分支。

```
git checkout feature-A
```

## 如何同时创建和签出 Git 分支？

一旦您熟悉了如何创建 Git 分支，您可能会喜欢使用一个命令创建新分支*和*检出分支的能力。

您可以将在 Git 中创建和检出分支的两个操作结合起来:

```
git checkout -b <branch name>
```

这将创建一个新的分支，并立即签出该分支。