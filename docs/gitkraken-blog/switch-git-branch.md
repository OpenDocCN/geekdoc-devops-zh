# 如何切换 Git 分支？Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/switch-git-branch>

当您使用 Git 存储库中的分支时，经常会出现切换到另一个分支的需求。你可能需要在另一个分支上执行工作，或者帮助队友进行代码审查。

在 Git 中，切换操作也被称为“签出”。为了使用终端切换到另一个 Git 分支，您将使用`git checkout`命令。

首先，让我们看看如何使用 checkout 命令在本地 Git 分支之间切换。

### 如何在本地 Git 分支之间切换？

首先运行`git branch`命令来查看您的本地分支列表，这样您就知道您必须使用哪个分支，以及您当前已经签出了哪个分支。

接下来，您将使用`git checkout`命令来检出或者切换到一个不同的本地分支。它应该是这样的:

```
$ git checkout <name-of-branch-you-want-to-switch-to>
```

### 如何在 Git 中签出远程分支？

按照类似的过程，您将签出远程分支，以便切换到新的分支。

参见[如何在 Git](https://www.gitkraken.com/learn/git/problems/git-checkout-remote-branch) 中签出远程分支的流程。

### 如何切换 Git 分支名称？

现在，如果你正在寻找如何改变一个 Git 分支的名称，你将会想要回顾我们的参考资料[如何重命名一个 Git 分支](https://www.gitkraken.com/learn/git/problems/rename-git-branch)。

### **如何使用 GitKraken 切换 Git 分支？**

让我们比较一下在 Git 客户端的帮助下切换 Git 分支的体验。GitKraken 提供的一个巨大而明显的好处是能够快速方便地查看您的本地和远程分支机构列表。不需要记住名字，也不需要花时间调出你的 Git 分支列表。

在 GitKraken 的左侧面板或中央图形中，您只需双击或右键单击您希望切换到的分支的名称，并从上下文菜单中选择`Checkout`。

此外，您可以使用键盘快捷键`Cmd/Ctrl + P`打开 GitKraken Fuzzy Finder，输入“checkout ”,然后输入您希望切换到的分支名称。

简化切换 Git 分支的过程并优化您的工作流程。下载适用于 Windows、Mac 和 Linux 的跨平台 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 。