# Git 回复提交| Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/revert-git-commit>

开发人员使用 Git revert commit 通过“向前更改”来撤销过去的错误从本质上讲，开发人员使用 Git revert commit 来撤销过去的错误，方法是创建一个“相等但相反”的提交，以抵消目标提交或一组提交的影响。大多数情况下，如果您在团队中工作，Git revert 是用来纠正提交的最佳命令，因为它不太可能影响团队成员的工作。

在本文中，我们将介绍如何使用终端、 [GitKraken 客户端](https://www.gitkraken.com/git-client)和 [GitLens for VS 代码](https://www.gitkraken.com/gitlens)来恢复 Git 中的提交。我们还将看看开发人员用来撤销错误的其他 Git 命令和工作流，以及如何根据您的用例来确定使用哪个命令。

[如何使用终端执行 Git 还原提交](#revert-using-terminal)

[如何使用 GitKraken 客户端执行 Git 还原提交](#revert-using-gitkraken-client)

[如何对 VS 代码使用 GitLens 执行 Git Revert 提交](https://www.gitkraken.com/learn/git/problems/revert-git-commit#revert-using-gitlens)

[去重置](#git-reset)

[Git Rebase](#git-rebase)

[Git 修正](#git-amend)

[https://www.youtube.com/embed/w4hvcI_4WNQ?feature=oembed](https://www.youtube.com/embed/w4hvcI_4WNQ?feature=oembed)

视频

无论您更喜欢使用 GUI 还是 CLI，GitKraken Client 都提供了两个世界的最佳选择，通过给您更多的控制，使恢复提交更快、更容易、更安全。

## 在终端中恢复 Git 提交

要使用终端撤销 Git 提交，首先需要识别要撤销的提交的惟一提交 ID 或 SHA。要查找目标提交的提交 ID，请运行以下命令:

`git log`

这将显示您的提交列表以及每个提交的唯一 ID。接下来，`copy`您想要恢复的提交的提交 ID。

这将显示您的提交列表以及每个提交的唯一 ID。接下来，`copy`您想要恢复的提交的提交 ID。

现在运行`git revert <commit ID>`。这将创建一个新的提交来否定您指定的提交。

**Git 使用终端回复多个提交**

您还可以恢复一系列 Git 提交。为此，请运行以下命令:

### **Git 使用终端回复多个提交**

`git revert <older commit ID>..<newer commit ID>`

旧的提交应该首先出现，然后是新的提交。这将恢复这两个提交，以及它们之间的任何提交。

旧的提交应该首先出现，然后是新的提交。这将恢复这两个提交，以及它们之间的任何提交。

使用 GitKraken 客户端进行 Git 还原提交

[](https://www.gitkraken.com/wp-content/uploads/2022/12/Screen-Shot-2022-12-15-at-11.58.28-AM.png)

使用 GitKraken 客户端的可视化上下文，只需点击两下就可以恢复 Git 提交。

## 要使用 GitKraken 客户端恢复提交，只需在中央图中右键单击要恢复的提交，并从上下文菜单中选择`Revert commit`。

然后将询问您是否要立即提交更改；在这里，您可以选择保存恢复的提交，或者选择`No`进行额外的代码更改或更改 Git 提交消息。

要使用 GitKraken 客户端恢复提交，只需在中央图中右键单击要恢复的提交，并从上下文菜单中选择`Revert commit`。

然后将询问您是否要立即提交更改；在这里，您可以选择保存恢复的提交，或者选择`No`进行额外的代码更改或更改 Git 提交消息。

很简单，对吧？无需记住复杂的提交 id 或经历额外的步骤。准备好尝试了吗？[下载 GitKraken 客户端](https://www.gitkraken.com/download)并测试恢复 Git 提交有多容易。

**Git 使用 GitKraken 客户端回复多个提交**

如果您在 GitKraken 客户端中恢复多个提交，您将需要一次恢复一个，并且您应该按照从最新到最早的顺序这样做。这将减少引入冲突的机会。

### **Git 使用 GitKraken 客户端回复多个提交**

使用 gittens 进行 git 还原提交

如果你是一个 VS 代码用户， [GitLens](https://www.gitkraken.com/gitlens/try-free) 使得恢复提交变得很容易。

## 要使用 GitLens 恢复 Git 提交，请完成以下操作:

[在 VS 代码](http://vscode.dev)中打开您的回购

从侧边栏中选择`Source Control`

1.  导航至`COMMITS`部分
2.  `Right-click`在您想要恢复的提交上
3.  选择`Revert Commit`选项
4.  `Right-click`在您想要恢复的提交上
5.  选择`Revert Commit`选项

您也可以通过命令面板键入`>GitLens: Git Revert`或从提交细节的快速选择菜单访问该选项。

[](https://www.gitkraken.com/wp-content/uploads/2022/03/gitlens-revert-commit-sidebar-1.png)

您也可以通过命令面板键入`>GitLens: Git Revert`或从提交细节的快速选择菜单访问该选项。

准备好试试了吗？[安装 GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens) 让您的任务和项目井然有序。

[](https://www.gitkraken.com/wp-content/uploads/2022/03/gitlens-revert-commit-hover.png)

撤销提交的附加命令和工作流

当使用 Git 库时，即使是最勤奋的开发人员也会时不时地遇到错误。但是修复这些错误的过程可能会因每个案例的情况而有所不同。此外，如果您不注意如何撤销这些错误，您可能会丢失工作或弄乱团队成员的工作。

## 幸运的是，Git 提供了几个工具，允许您撤销由提交引入的错误。与任何其他工具箱一样，在使用每个工具之前，了解它们的用途、优势和相关风险是非常重要的。

因此，让我们看看开发人员用来撤销错误的其他一些常见 Git 命令。

幸运的是，Git 提供了几个工具，允许您撤销由提交引入的错误。与任何其他工具箱一样，在使用每个工具之前，了解它们的用途、优势和相关风险是非常重要的。

使用 GitKraken 客户端快速撤销

GitKraken 客户端使用顶部工具栏中的 [`undo`和`redo`按钮](https://help.gitkraken.com/gitkraken-client/undo-and-redo/)轻松撤销/重做以下操作:

## 检验

犯罪

*   抛弃
*   删除分支
*   移除遥控器
*   将分支重置为提交
*   需要注意的是 GitKraken 客户端的撤销按钮只会撤销你最近的 Git 操作。在最近一次 Git 操作之后撤销任何操作都需要使用 Git revert、Git reset 或 Git rebase。
*   **去重置**

Git revert 使用前向更改来撤销提交，而 Git reset 的操作正好相反。

Git reset 是一种及时返回到特定提交的方法，并且将您的活动位置重置为分支提交历史中所选的提交。

然而，正如科幻电影所描绘的那样，改变历史进程会带来各种各样的副作用。例如，如果您返回到一个带有重置的提交，您传递的所有提交可能会进入一个悬空状态，它们存在，但没有任何东西引用它们。此外，如果您执行“硬”复位，您可能会丢失没有正确备份的本地工作。

### 如果您需要对不是上次提交的提交进行修改，最好的解决方案是通过恢复旧的提交来创建新的提交。作为 Git 的最佳实践，您应该避免做任何需要您强制推送——并重写历史——您的主要分支的事情。

仍然决定执行 Git 重置吗？这里有一个[指南来帮助你尽可能少的重启](https://www.gitkraken.com/learn/git/git-reset)。

**Git Rebase**

现在，假设您想通过回到过去来撤销 Git 提交，但是您不想完全重置历史。

您不是在错误提交后放弃提交，而是希望再次应用它们，并逐个提交地处理更改的历史记录的影响。

对于那些想要对历史修订过程进行更多手动控制的人，Git 提供了交互式 rebase 工具。使用交互式 rebase，您将看到一个提交列表，该列表显示了到您的分支历史上的指定点为止的提交。从这一点来看，每次提交都有不同的选项:

**Pick:** 您可以将提交保留在历史记录中

### **删除:**从历史记录中删除提交

**挤压:**将提交与其之前的提交合并

**改写:**更改提交消息

获得关于[如何执行 Git rebase](https://www.gitkraken.com/learn/git/problems/git-interactive-rebase) 的分步说明。

*   **Git 修正**
*   如果您碰巧在提交后立即发现了错误，您可以使用 Git amend 命令快速撤销错误。也许您在最后一次提交时忘记了存放文件，或者在提交消息中出现了拼写错误，或者甚至在代码中出现了错误，但是还没有提交任何其他内容。
*   如果你想修改历史中的最后一次提交，你可以用修改来修改 Git 提交。例如，您可以通过更改 Git 提交消息或描述来修改之前的提交，或者您甚至可以暂存您忘记包含的文件。
*   类似地，如果您之前提交的代码中有错误，您可以修复错误，甚至在修改之前添加更多的更改。Git amend 是一个单次提交撤销工具，它只影响分支历史中的最后一次提交。

与 Git 重置和 Git 重置一样，修改是一个历史重写过程，或者说是一个逆向变化，而不是一个正向变化。在幕后，Git amend 创建了一个全新的提交，并用新的提交替换您的上一次提交。因此，它应该受到同等程度的关注；一旦您将提交推送到共享分支中，修改该提交并推送到修改后的变更可能会对处理该提交的任何人产生副作用。

### 改变未来的最佳方式

撤销 Git 提交很像时间旅行的现代描述，每个 Git 命令都允许您以独特的方式改变和影响历史。由您决定哪个命令适合您的用例，并从那里应用它。旅行愉快！

让我们做一个最后的总结。

**Git 回复**

不要重写你的回购历史

消除指定提交的影响

## 通常在与合作者一起工作时，用于“撤消”命令的最佳命令

**去重置**

将您的活动位置重置为所选的提交，可能会删除后面的所有提交

要求您使用 Git 强制推送

会给你的队友带来麻烦

*   **Git Rebase**
*   本质上是移动提交并保留一些提交历史
*   使用交互式 rebase 工具，您可以手动选择每次提交时要做的事情，包括拾取、丢弃、挤压、重绕

**Git 修正**

“重写历史”命令

*   只能在您最近一次提交时执行
*   可用于更改提交消息、添加文件、调整代码等。
*   会给你的队友带来麻烦

GitKraken 客户端使恢复 Git 提交的过程变得简单，风险也小得多。帮你的工作流程一个忙，今天就试试 GitKraken 客户端吧。

**Git Rebase**

*   本质上是移动提交并保留一些提交历史
*   使用交互式 rebase 工具，您可以手动选择每次提交时要做的事情，包括拾取、丢弃、挤压、重绕

**Git 修正**

“重写历史”命令

*   只能在您最近一次提交时执行
*   可用于更改提交消息、添加文件、调整代码等。
*   Can be used to change a commit message, add a file, adjust code, etc.

GitKraken 客户端使恢复 Git 提交的过程变得简单，风险也小得多。帮你的工作流程一个忙，今天就试试 GitKraken 客户端吧。

GitKraken Client makes the process of reverting a Git commit simple and with far less risk. Do your workflow a favor and try GitKraken Client today.