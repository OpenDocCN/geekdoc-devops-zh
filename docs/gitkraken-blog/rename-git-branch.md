# 如何重命名 Git 分支？Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/rename-git-branch>

[https://www.youtube.com/embed/cEfoB8Hl2gY?feature=oembed](https://www.youtube.com/embed/cEfoB8Hl2gY?feature=oembed)

视频

## **重命名 Git 分支**

当使用您的 Git 库时，可能会有这样的时候，您希望重命名您正在使用的 Git 分支。首先，我们将介绍重命名本地 Git 分支，然后重命名远程 Git 分支；我们将使用跨平台 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 和 CLI 来回顾这个过程。

只需两次点击，GitKraken 就能让您快速重命名本地和远程分支机构。⬇️

## **如何用 GitKraken 给一个分支重新命名？**

让我们看看如何使用 GitKraken Git GUI 为 Windows、Mac 和 Linux 快速重命名 Git 分支。

## 如何在 GitKraken 本地重命名分支？

要使用 GitKraken 重命名本地分支，只需右击分支并从上下文菜单中选择`Rename`选项。

接下来，键入您想要的新分支名称并点击`Enter`。本地分支将被重命名。

## 如何在 GitKraken 中重命名远程分支？

要更新 GitKraken 中的远程 Git 分支，您必须通过右键单击分支并从上下文菜单中选择`Set Upstream`来更改上游。

接下来，输入新的分支名称并点击`Submit`。然后，您可以使用`Push`工具栏按钮，或者从上下文菜单中，将新的 Git 分支推送到远程。

最后，您必须通过右键单击旧的远程分支并从上下文菜单中选择`Delete origin/branch-name`来删除它。

## 如何在命令行中本地重命名 Git 分支？

当您想在本地重命名 Git 分支时，您可以使用带有`-m`选项的`git branch`命令来完成。

如果您想要重命名您已经签出的当前分支，您可以简单地传递您想要的新名称:

```
git branch -m <new-branch-name>
```

***GitTip:了解更多关于如何[结帐本地 Git 分支](https://www.gitkraken.com/learn/git/problems/switch-git-branch)的信息。***

## 如果你没有签出一个本地 Git 分支，你能重命名这个分支吗？

如果要重命名与已签出的当前分支不同的分支，可以使用相同的命令，但要传递分支的当前名称，后跟新的分支名称:

```
git branch -m <branch-name><new-branch-name>
```

## 如何重命名远程 Git 分支？

如果您要重命名的分支已经被推送到远程，上游分支不会因为您重命名了本地分支而改变。

您可以将新分支推送到远程，并使用带有`-u`(或`--set-upstream`)选项的`git push`命令更新上游:

```
git push origin -u <new-branch-name>
```

***GitTip:了解更多关于如何在 Git 中设置分支上游的[。](https://www.gitkraken.com/learn/git/problems/git-set-upstream-branch)***

现在，您只需使用带有`-d`(或`--delete`)选项的`git push`命令从远程删除旧分支:

```
git push origin -d <branch-name>
```

***GitTip:了解更多关于[如何在 Git](https://www.gitkraken.com/learn/git/problems/delete-remote-git-branch) 中删除远程分支。***

使用 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 重命名 Git 分支的过程要快得多，也更直观，并且可以避免错误地修改错误的数据。