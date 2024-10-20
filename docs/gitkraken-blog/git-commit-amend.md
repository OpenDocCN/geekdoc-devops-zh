# 如何修改 Git 提交消息| Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/git-commit-amend>

我们都可以理解这种情况:您刚刚提交了更改，却发现在 Git commit 消息中拼错了一些内容。或者，您可能需要对另一个文件进行更改，该文件确实应该是提交的一部分。

当然，你可以让那句“teh”滑过去，或者只是用你忽略的改变再提交一次，但是来吧，你可以做得更好！幸运的是，Git 为如何[撤销 Git 提交](https://www.gitkraken.com/learn/git/problems/revert-git-commit#undo-git-commit)提供了几个选项，包括[如何恢复提交](https://www.gitkraken.com/learn/git/problems/revert-git-commit)，但是我们将先使用 GitKraken Git GUI，然后使用命令行来演练如何修改 Git 提交。

使用 GitKraken
修改您的最后一次提交和编辑提交消息既快速又简单

## 如何在 GitKraken 中修改提交消息？

使用 GitKraken 编辑您最近提交的消息非常简单。从中央图形中选择一个提交后，只需单击右侧提交面板顶部的消息开始编辑，然后单击`Update message`保存您的更改。

## 你如何修改你在 GitKraken 的最后一次犯下的错误？

在 GitKraken 中向最后一次提交添加新的更改的过程非常简单。不需要记住或运行任何命令。

暂存更改后，选中提交消息旁边的`Amend`旁边的框，然后单击`Amend Previous Commit`。在提交更改之前，您还可以在这里轻松编辑 Git 提交消息。

## 如何在命令行中修改 Git 提交？

是一个 Git 命令，专门用来保持语法的完整性和库的整洁。它将允许您编辑 Git 提交消息或更改您上次提交的内容。

## 如何在命令行中修改 Git 提交消息？

以下方法将使用更新的消息创建一个新的提交，替换以前的提交。要在命令行中更改 Git 提交消息，您将运行以下命令:

```
git commit --amend -m “new commit message”
```

在 GitKraken 中，您可以简单地从中央图中选择一个提交来查看相关的提交消息，而在终端中，您的可见性要低得多。如果您想在 CLI 中编辑之前看到 Git commit 消息，您可以去掉`-m`标志，只需键入:`git commit --amend`。

但是请记住，使用这种方法需要在 VIM 中编辑提交消息，所以您需要键入`i`进入`INSERT`模式来更改消息，然后键入`esc`退出`INSERT`模式，然后键入`:wq`来保存您的更改并退出。

与 GitKraken 相比，在 CLI 中编辑 Git commit 消息至少需要四个额外的步骤。但是，嘿，谁在数呢。😉

## 如何在命令行中修改你的最后一次提交？

要将提交修改为仅包括 CLI 中的新更改，您首先需要将工作目录中的任何更改转移到新提交中。

要修改 Git 提交以包含新的更改而不更新提交消息，您将运行:

```
git commit --amend --no-edit
```

如果您想更改上次提交的内容并编辑 Git 提交消息，您可以传入`-m`标志而不是`--no-edit`。

```
git commit --amend -m “new commit message”
```

借助适用于 Windows、Mac 和 Linux 的跨平台 [GitKraken Git client](https://www.gitkraken.com/git-client) ,为您的工作流程增加更多灵活性并修改 Git 提交，无论您是编辑 Git 提交消息还是修改上次提交。