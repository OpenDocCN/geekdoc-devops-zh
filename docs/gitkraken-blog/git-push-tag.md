# 你如何得到推送标签？Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/git-push-tag>

在我们开始讨论如何推送 Git 标签之前，让我们快速回顾一下 Git 中标签的基础知识。

## 什么是 Git 标签？

Git 标签是对一个 [Git 存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)的历史中的特定提交的引用。例如，在 Git 中，标签通常用于标记发布，但是当您想要引用提交而不需要使用提交散列时，它们也很有用。

在 Git 推送标签之前，您必须创建标签。我们将回顾使用跨平台 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 创建和推送 Git 标记的过程，然后在 CLI 中浏览该过程。

GitKraken 的易于阅读的提交图将使您更深入地了解您的分支和标签，并使推送更改到您的远程安全，快速，非常容易。

## **如何在 GitKraken 中创建 Git 标签？**

使用 GitKraken 可视化您的存储库的美妙之处在于能够立即洞察您的分支和提交。中央 UI 按照时间顺序显示您提交的所有内容，这样您就可以确切地知道任何给定项目中正在发生什么。

要在 GitKraken 中创建 Git 标记，您将从中间的图中找到您想要标记的 [Git 提交](https://www.gitkraken.com/learn/git/commit),并简单地右键单击该提交。然后从下拉菜单中选择选项`Create tag here`。

接下来，您将在 commit 旁边的框中键入 Git 标记的名称，然后按 enter 键。就是这样！

## 如何在 GitKraken 中创建带注释的 Git 标签？

GitKraken 还允许您使用相同的过程创建带注释的标签。只需右键单击您想要标记的提交并选择`Created annotated tag here`。

这将在图的顶部打开一个对话框，允许您输入一条与 Git 标签相关的消息。

当您将鼠标悬停在中央图形中的标签上时，您的注释将显示为工具提示。

## 如何推送标签？

创建 Git 标签后，您将能够将标签推送到您的遥控器。

默认情况下，Git 标记存储在本地机器上，并且在运行 Git push 命令时不会被推送。

## 如何在 GitKraken 中推送标签？

要在 GitKraken 中推送标签，只需右键单击 Git 标签并选择`Push <tag-name> to origin`。

现在，因为您已经让 Git 推送了您的标签，所以每当有人在您的远程存储库上执行 [Git pull](https://www.gitkraken.com/learn/git/tutorials/what-is-git-pull) 时，它就会被拉下来。只需两次点击，你就完成了。💥

有了 GitKraken，你只需点击两下就可以将标签推送到遥控器上。加快工作流程中较小任务的速度会带来巨大的生产力优势。给你更多的时间做…无论你想要什么。

## 如何在命令行中创建 Git 标签？

如果没有 GitKraken 的中心图提供的直观上下文，您将不得不在 CLI 中运行一个命令来查看您的提交历史。

`git log --pretty=oneline`

在本例中，运行该命令将显示存储库中有两个提交。

要 Git 标记最近的提交，您将使用以下命令:

`git tag <tag-name>`

在这个例子中，标签名称将是`this-is-a-tag`。

现在，如果您想在 CLI 中标记一个旧的提交，您将需要在命令的末尾添加一部分提交散列。让我们使用`48dad`向本例中的`Initial commit`添加一个 Git 标签。

要包含带有 Git 标记的消息，可以使用`-a`标志创建一个带注释的标记，然后添加带有`-m`标志的消息。

`git tag -a <tag-name> -m “tag-message”`

## 如何使用 Git show tag 命令？

要验证您的 Git 标记是否被正确注释，您可以运行以下命令:

`git show <tag-name>`

这个命令应该提供以下信息来验证 Git 标签上的注释。

## 如何在命令行中推送标签？

要在终端中推送标签，您将使用以下命令。在本例中，`this-is-a-tag`是您的标签的名称。

`git push origin this-is-a-tag`

## 如何推送所有标签？

要 Git 将您本地拥有的所有标签推送到您的远程存储库，您将使用以下命令:

`git push origin --tags `

GitKraken Git GUI 提供了对提交、分支和 Git 标签的更多可见性，使得在需要时可以快速轻松地找到标签并将其推送到您的远程设备。使用 GitKraken 的开发人员加快了日常 Git 操作，并使更高级的任务，如 [Git interactive rebase](https://www.gitkraken.com/learn/git/problems/git-interactive-rebase) ，安全而直观。