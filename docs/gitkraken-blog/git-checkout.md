# Git 签出-签出分支、提交和标签|学习 Git

> 原文：<https://www.gitkraken.com/learn/git/git-checkout>

Git checkout 命令告诉 Git 您希望您的更改应用到哪个分支或提交。

现在，在我们开始讨论如何在 GitKraken Git 客户端和命令行中进行 Git 检验之前，让我们先快速复习一下 [Git 分支](https://www.gitkraken.com/learn/git/branch)和 [Git 提交](https://www.gitkraken.com/learn/git/commit)。

在 Git 中，分支是指向一个特定提交的指针，而提交是您的存储库在特定时间点的快照。

您的分支指针随着您的每次新提交而移动。

在 GitKraken 中，您可以快速识别分支和相关提交，使签出变得轻而易举。

Git 检验分支

## 如果要对尚未签出的分支进行更改，首先需要签出该分支。

***GiTip:学习如何 [Git 结账一个远程分支](https://www.gitkraken.com/learn/git/problems/git-checkout-remote-branch)以及如何[在本地分支](https://www.gitkraken.com/learn/git/problems/switch-git-branch)之间切换。***

检出 Git 分支将更新您的存储库文件，以匹配分支指向的任何提交的快照。从这里开始，分支指针将继续跟随分支上的每个新提交。

Git 签出提交

也有可能 Git checkout 提交。这将更新您的存储库文件，以匹配您签出的提交的快照。这对于在不创建新分支的情况下审查旧的提交非常有用。

## GitTip:如果你想了解更多关于[如何 Git checkout 一个 commit](https://www.gitkraken.com/learn/git/problems/git-checkout-commit) 的知识，我们可以指导你。

Git Checkout Tag

类似地，如果你想回顾过去的代码版本，比如最近发布的版本，你可以签出一个标签。

***GitTip:了解更多关于[如何用 GitKraken 和命令行检查 Git 标签](https://www.gitkraken.com/learn/git/problems/git-checkout-tag)的信息。***

## Git 分离头

注意这一点很重要:检查 Git 提交或 Git 标签会将头点移动到提交而不是分支。这将使您的存储库处于所谓的分离状态。

当您在 Git 中处于分离的 HEAD 状态时，您不能提交对分支的更改。

Git 结帐的好处

## Git checkout 对于将更改应用到分支非常有用，但是如果您想要检查某个行为是何时引入的，它对于检查旧的提交也很有用。

注意这一点很重要:检查 Git 提交或 Git 标签会将头点移动到提交而不是分支。这将使您的存储库处于所谓的分离状态。

你如何用 GitKraken 结账？

无论您是想使用 Git checkout 命令来检查分支、提交还是标记，GitKraken 都可以让整个过程变得简单直观，并提供整个存储库的完整可视上下文。

## 厌倦了浪费时间搜索提交散列或标记名吗？GitKraken 可以帮忙。

Git checkout 对于将更改应用到分支非常有用，但是如果您想要检查某个行为是何时引入的，它对于检查旧的提交也很有用。

你如何用 GitKraken 结账？

## 在这个例子中，我们将回顾如何使用 Git checkout 命令来签出一个远程分支。

由于 GitKraken 主用户界面提供的视觉优势，您可以查看左侧面板中列出的所有远程分支机构。

要在 GitKraken 中签出远程分支，只需双击或右键单击左面板或中央图中的分支名称，然后选择`Checkout`。

## 你如何用 GitKraken 结账？

或者，您可以使用 GitKraken 模糊查找器，方法是键入“checkout ”,然后键入您想要检查的分支名称。

由于 GitKraken 主用户界面提供的视觉优势，您可以查看左侧面板中列出的所有远程分支机构。

如何用 GitKraken 检查提交？

GitKraken 在中央图表中按时间顺序显示您的存储库的提交列表。要在 GitKraken 中检查一个提交，右键单击中央提交并选择`Checkout this commit`。

## 你如何用 GitKraken 签出标签？

在 GitKraken 中，标签在中央图形中用🏷标签图标。这使得快速和容易地找到您的回购标签。

在中间的图表中，找到您想要签出的标签，然后右键单击。然后，选择`Checkout this commit`以分离头部状态检出标签。

## 你如何用 GitKraken 签出标签？

但是，如果您想将标签签出为分支，该怎么办呢？您可以遵循完全相同的过程，但是在中间图形或左侧面板中右键单击目标标记后选择`Create branch here`。

只需点击两次，您就可以在 GitKraken 中签出分支、提交或标记。🤯

如何在命令行中签出一个分支？

如果您正在使用 CLI，您将无法立即看到您的本地分支，因此您将从运行 Git branch 命令来查看您的本地分支列表开始。这也将显示您当前已经签出的分支。

接下来，您将运行 Git checkout 命令，后跟您希望切换到的本地分支的名称。

## 如何在命令行中检查提交？

为了在 CLI 中检查提交，您将需要提交散列。您可以通过运行 Git log 命令来获得提交散列。

要检验之前的提交，您将使用 Git checkout 命令，后跟提交散列。

```
git checkout <name-of-branch>
```

## 如何在命令行中签出一个标签？

与 GitKraken 不同，在 Git kraken 中，您可以通过在中心图中查找标记图标来轻松定位您的标记，您必须运行 Git tag 命令来查看您的存储库的标记列表。

有了标记名之后，可以在 Git 分离头状态下签出标记，或者将标记作为分支签出。如果您想在分离的 HEAD 状态下签出标签，您将运行:

```
git checkout <commit hash>
```

如果您想将 Git 签出标记作为分支，您将运行:

你的时间和精力是宝贵的；为什么要花时间去记忆或查找远程分支、提交散列或标记名呢？让传说中的跨平台 GitKraken Git GUI 为您组织所有这些，并以一个漂亮且易于理解的图形呈现您的存储库。

有了标记名之后，可以在 Git 分离头状态下签出标记，或者将标记作为分支签出。如果您想在分离的 HEAD 状态下签出标签，您将运行:

```
git checkout <tag name>
```

如果您想将 Git 签出标记作为分支，您将运行:

```
git checkout -b <branch name> <tag name>
```

你的时间和精力是宝贵的；为什么要花时间去记忆或查找远程分支、提交散列或标记名呢？让传说中的跨平台 GitKraken Git GUI 为您组织所有这些，并以一个漂亮且易于理解的图形呈现您的存储库。