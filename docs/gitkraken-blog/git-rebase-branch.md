# 你如何改变一个分支的基础？Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/git-rebase-branch>

[Git rebase](https://www.gitkraken.com/learn/git/git-rebase) 动作有助于将一个分支的变更合并到另一个分支上，并有助于创建更清晰的回购历史，尤其是在比较 [Git rebase 和 merge](https://www.gitkraken.com/learn/git/problems/git-rebase-vs-merge) 时。

GitTip:如果你在寻找如何[合并 Git 分支](https://www.gitkraken.com/learn/git/problems/merge-git-branch)，我们有另一个页面。

在回顾如何在命令行中对分支进行 Git 重新基础化之前，我们将通过跨平台 GitKraken Git GUI 对分支进行重新基础化。

*虽然其他教程通常会展示如何将分支重设为 master，但我们选择使用分支名称:`main`。*

下面是一个使用 GitKraken Git GUI 的例子

## 如何用 GitKraken 给树枝打基础？

使用直观的 GitKraken Git 客户端来重定 Git 分支的基础非常简单，并且允许您更清楚地看到您想要重定基础的分支的情况。

只需将 GitKraken 中的一个分支拖放到您想要作为新基础的分支上，然后从上下文菜单中选择`Rebase <source branch name> onto <target branch name>`。

或者，您也可以右键单击中央图形中的分支来访问相同的菜单选项。

## **如何用 GitKraken 重新设定远程分支的基础？**

在命令行中，如果目标分支不是最新的，就需要手动更新，与命令行不同，GitKraken 会为您保持远程分支的最新状态。GitKraken 中的 rebase 过程完全相同，无论您是否更新了 base 分支。

## 你如何解决与 GitKraken 的 rebase 冲突？

如果你的重置导致文件冲突，GitKraken 会立即提醒你冲突，并为你提供一个在应用程序中解决冲突的工具，无需上下文切换。

GitKraken 的合并解决工具将冲突文件并排呈现在您面前，您签出的分支在左边，您的目标分支在右边。底部还有一个输出部分。

选中一行或一段代码旁边的框，将其添加到输出中。这使您能够清楚地看到您的选择，并做出最佳决定，保留哪些更改，放弃哪些更改。

一旦冲突得到解决，您就可以通过单击按钮继续您的 rebase。

## 如何在终端中改变分支的基础？

要使用命令行重建 Git 分支的基础，首先要检出包含要重写到目标分支的变更的分支。在这个例子中，假设您想要将`feature`分支重新放在`main`分支的基础上。您将通过运行以下命令开始:

```
git checkout feature-branch
```

```
git rebase main
```

Git 应该会显示如下消息:

```
Successfully rebased and updated refs/head/feature-branch
```

这告诉我们`feature-branch`上的最后一次提交已经被更新。

## 如何在终端中对远程分支进行重置？

现在，如果对目标分支的远程进行了更改，您将需要从该远程分支获取最新的更改。

GitTip:如果你刚开始在偏远的分支机构工作，不要害怕。我们已经有了你学习如何[拉远程 Git 分支](https://www.gitkraken.com/learn/git/problems/pull-remote-git-branch)所需的一切。

在这个例子中，你的目标分支仍然是`main`；首先执行一个 Git pull 来从遥控器获取更改。

```
git checkout main
```

```
git pull origin main
```

在拉动完成后，您通过检查您的`hotfix`分支，然后返回到`main`来继续前进。

```
git checkout hotfix
```

```
git rebase main
```

## 如何解决终端中的 Git rebase 冲突？

有时，尝试重定 Git 分支的基础可能会导致冲突的更改，需要在操作完成之前解决这些冲突。当 Git 检测到冲突的更改时，它会在错误的提交处暂停 rebase。

不像在 GitKraken 中，解决冲突只需点击一下鼠标，当在 CLI 中工作时，您没有足够的上下文来立即识别冲突代码存在于何处。您将不得不离开终端，在您首选的外部编辑器中打开冲突文件，以决定您想要保留哪些代码，以及想要丢弃哪些代码。

完成并保存您的更改后，您可以通过运行`git add`命令后跟文件名来暂存它们。接下来，您应该使用`git commit -m`为您的解析创建一个提交，后跟您的 Git 提交消息。

```
git commit -m “my merge resolution commit message”
```

如果冲突确实得到了解决，您可以使用以下命令继续进行基础变更:

```
git rebase --continue
```

或者，如果您无法解决冲突，或者决定不希望继续进行重新调整，您可以使用:

```
git rebase --abort
```

这将取消 Git rebase 操作，您的分支将回到开始 rebase 之前的状态。

使用面向 Windows、Mac、& Linux 的 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 跨平台重建 Git 分支，使其具有更好的可视性和更大的信心。