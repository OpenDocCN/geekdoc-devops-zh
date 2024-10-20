# 如何合并一个 Git 分支？Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/merge-git-branch>

在 Git 中合并分支对于将一个分支中的更改合并到另一个分支中，以及保存历史非常有用。

在这个例子中，我们将把一个 Git 分支与 master 合并。假设您有一个主分支，其中有一些变化。然后分支到一个特性分支，并进行额外的修改。

要合并特征分支，您需要首先使用检出主分支

```
git checkout master
```

***GitTip:需要帮助？查看如何[在本地检出 Git 分支](https://www.gitkraken.com/learn/git/problems/switch-git-branch)和如何[检出远程 Git 分支的分步过程。](https://www.gitkraken.com/learn/git/problems/git-checkout-remote-branch)***

然后使用该命令

```
git merge feature
```

然后，您将把来自特性分支的更改合并到主文件中，将来自特性分支的所有更改添加到主文件中。除了将所有的更改合并到 master 上之外，这还为您提供了历史的准确表示。

## **如何合并 GitKraken 中的一个 Git 分支？**

为了使这个过程更加简单，您可以使用 Git 客户端，比如 [GitKraken](https://www.gitkraken.com/git-client) ，来可视化 Git 中的分支过程。

下面是一个使用 GitKraken Git GUI 的例子

在这个例子中，我们将再次合并一个 Git 分支和 master 分支。您有一个带有变更的主分支，但是您已经分支到一个特性分支中进行额外的变更。

您将从检查一个分支开始。要在 GitKraken 中做到这一点，只需双击中央图中的主分支，或者右键单击该分支并选择`Checkout master`。

## **如何在 GitKraken 中将 Git 分支与 master 合并？**

在您签出主分支之后，您可以将您的特性分支拖放到中心图中的主分支上，或者右键单击特性分支，并从上下文菜单中选择`Merge feature into master`。

## **解决 GitKraken 的合并冲突**

在 Git 中，如果在两个分支之间对同一个文件或文件的某一行进行了竞争性的更改，就会发生合并冲突。

在命令行中，解决合并冲突可能会很复杂，而且压力很大。相比之下， [GitKraken 合并冲突编辑器](https://support.gitkraken.com/working-with-repositories/branching-and-merging/#merge-conflict-editor)清楚地为您可视化冲突文件，因此您可以看到代码中存在差异的地方。

使用 GitKraken 的合并工具时，每个冲突部分都会有一个附带的复选框。选中某个部分的复选框会将其添加到底部的输出部分。这使您可以在上下文中看到可用的选项，以决定如何最好地继续。

## **Git 中的主分行与主分行**

“主人”这个称呼已经过时了；我们建议尽可能使用“main”来代替。

在 [GitKraken](https://www.gitkraken.com/git-client) 中设置你的默认分支机构名称既快速又简单。当您在 GitKraken 中初始化一个新的 Git 存储库时，或者从`Preferences`菜单中，您可以这样做。

借助 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 的强大功能，使 Git 中的合并分支更加容易和直观，并对您的工作流程更有信心。