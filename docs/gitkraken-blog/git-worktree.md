# 如何使用 Git Worktree |添加、列出、删除

> 原文：<https://www.gitkraken.com/learn/git/git-worktree>

Git worktree 命令允许您同时检查和处理多个 Git 分支。现在，你会在什么情况下使用这个动作？

想象一下，您正在对一个项目进行大量的更改，其中有多个新的依赖项，这些依赖项是由各种 WIP 更改引入的。如果您突然不得不在另一个分支中处理一个修补程序，会发生什么？对许多人来说，正常的工作流程是使用 [Git stash](https://www.gitkraken.com/learn/git/git-stash) 保存你当前的工作，签出热修复分支，结束工作，然后重新签出你原来所在的分支，弹出你的 stash。如果这听起来没有效率，Git 的开发者同意你的观点。这就是 Git worktree 的用武之地。

您可以简单地添加一个新工作树条目，并将目录更改为所需的分支，而不是存储和检出流程。

例如，假设您正在主分支中处理一个项目，但是需要测试和批准应用在特性分支中的变更。使用 Git worktree，您只需告诉 Git 在当前工作目录之外的不同目录中签出特性分支，并切换到该目录；从这里，您可以做您的工作，并回到原来的目录，找到所有正在进行的工作，就像您离开时一样。

[https://www.youtube.com/embed/s4BTvj1ZVLM](https://www.youtube.com/embed/s4BTvj1ZVLM)

VIDEO

## **Git 分支机构审查**

在我们了解如何在 GitLens 中为 VSCode 和命令行使用 Git worktree 之前，让我们先快速复习一下 [Git 分支](https://www.gitkraken.com/learn/git/branch)。

在 Git 中，分支是指向一个特定提交的指针，而提交是您的存储库在特定时间点的快照。您的分支指针随着您的每次新提交而移动。

默认情况下，Git 一次只在一个分支上跟踪和移动指针。您用 [Git checkout](https://www.gitkraken.com/learn/git/git-checkout) 选择的分支被认为是您的“工作树”。

就像森林中的一棵树一样，Git worktree 可以同时有几个分支，这正是 Git worktree 命令允许你做的。

为了保持有序，Git worktree 将每个分支放在文件系统中不同的指定文件夹中。要在工作树上的多个分支之间移动，您只需更改目录来处理所需的分支。

GitLens 可以帮助您保持组织性，并跟踪您的任务和项目。免费解锁 VS 代码中 Git 的全部功能！

[Install GitLens for VS Code](vscode:extension/eamodio.gitlens)

## **Git 工作树概念**

Git worktree 是一个强大的命令，有许多选项。出于我们的目的，我们将只关注分支的工作，因为这是最常见的用例。我们将使用以下命令浏览 Git 工作树示例:

*   Git 工作树添加
*   Git 工作树列表
*   Git 工作树删除

参见[官方 Git 文档](https://git-scm.com/docs/git-worktree)了解更多关于利用 Git 工作树的其他方法的信息。

参见[官方 Git 文档](https://git-scm.com/docs/git-worktree)了解更多关于利用 Git 工作树的其他方法的信息。

**Git 工作树添加**

## Git 工作树在引用特定的提交对象(如分支)时，将始终指向文件系统上的一个文件夹。为了同时处理多个检出的分支，您需要将每个分支作为一个新的工作树添加到 Git 工作树中。使用 Git 工作树添加时，您可能会遇到 4 种情况:

向与分支同名的目录中添加一个新的工作树(最常用的方法)

1.  将一个新的工作树添加到一个不同名称的目录中作为分支
2.  [创建一个新的 Git 分支](https://www.gitkraken.com/learn/git/problems/create-git-branch)并将一个新的工作树添加到与该分支同名的目录中
3.  创建一个新的分支，并将一个新的工作树添加到一个与新分支名称不同的目录中

4.  创建一个新的分支，并将一个新的工作树添加到一个与新分支名称不同的目录中

我们将在本文后面的 Git 工作树示例中介绍如何做到这些。无论您选择哪种路径，每个目录都只能有一个工作树，这一点很重要。当你创建新的工作树时，目标目录**必须**为空，否则，命令将失败，你将得到一个错误信息。

我们将在本文后面的 Git 工作树示例中介绍如何做到这些。无论您选择哪种路径，每个目录都只能有一个工作树，这一点很重要。当你创建新的工作树时，目标目录**必须**为空，否则，命令将失败，你将得到一个错误信息。

**Git 工作树列表**

## 为了查看哪些分支当前作为工作树是活动的，您需要告诉 Git 用 Git worktree list 命令列出它们。首先列出主工作树，然后是每个链接的工作树。

输出细节包括每个工作树的路径、提交散列和该条目中当前检出的分支的名称。

为了查看哪些分支当前作为工作树是活动的，您需要告诉 Git 用 Git worktree list 命令列出它们。首先列出主工作树，然后是每个链接的工作树。

输出细节包括每个工作树的路径、提交散列和该条目中当前检出的分支的名称。

**Git 工作树移除**

## 为了让您的 Git 工作树保持有序，您需要不时地删除条目。无论 worktree 条目是如何添加的，您都将以相同的方式删除它们:通过指定要删除的文件夹。并且您不需要担心在该文件夹中指定分支名称。

如果某个特定文件夹中有未提交的变更，Git 默认不会允许你删除它。您可以通过使用`--force`选项来覆盖此限制。和使用`-f`一样，在覆盖 Git 的安全预防措施之前，要小心并确保知道自己在做什么。

为了让您的 Git 工作树保持有序，您需要不时地删除条目。无论 worktree 条目是如何添加的，您都将以相同的方式删除它们:通过指定要删除的文件夹。并且您不需要担心在该文件夹中指定分支名称。

如果某个特定文件夹中有未提交的变更，Git 默认不会允许你删除它。您可以通过使用`--force`选项来覆盖此限制。和使用`-f`一样，在覆盖 Git 的安全预防措施之前，要小心并确保知道自己在做什么。

**GitLens 工作树**

## VS 代码的 GitLens 中的 Worktrees 是一个 GitLens+特性，需要 GitLens+或 GitLens+ Pro 帐户。GitLens+用户可以利用公共存储库上的工作树，而 GitLens+ Pro 用户可以使用公共和私有的所有存储库上的工作树。你可以在这里阅读更多关于 [GitLens+功能的信息。默认情况下，可以在 VS 代码侧边栏的源代码控制菜单下查看 GitLens 中的](https://www.gitkraken.com/gitlens/plus-features) 工作树。您也可以通过使用 VS 代码命令面板`⇧⌘P`并键入`GitLens: Show Worktrees view`来打开 Worktrees 菜单。

VS 代码的 GitLens 中的 Worktrees 是一个 GitLens+特性，需要 GitLens+或 GitLens+ Pro 帐户。GitLens+用户可以利用公共存储库上的工作树，而 GitLens+ Pro 用户可以使用公共和私有的所有存储库上的工作树。你可以在这里阅读更多关于 [GitLens+功能的信息。默认情况下，可以在 VS 代码侧边栏的源代码控制菜单下查看 GitLens 中的](https://www.gitkraken.com/gitlens/plus-features) 工作树。您也可以通过使用 VS 代码命令面板`⇧⌘P`并键入`GitLens: Show Worktrees view`来打开 Worktrees 菜单。

Worktrees 只是你可以用 GitLens+解锁的众多强大功能之一。你还在等什么？

Worktrees 只是你可以用 GitLens+解锁的众多强大功能之一。你还在等什么？

[Sign up for GitLens+](https://app.gitkraken.com/register?product=gitlens&referrer=gitlens)

**使用 GitLens 添加 Git 工作树**

## 在这个 Git 工作树示例中，我们将回顾如何使用 GitLens 工作树添加工作树条目，涵盖所有 4 种可能的情况。

要开始使用 GitLens 工作树添加工作树条目，只需点击工作树菜单中的+图标。这将打开命令面板，并要求您选择将哪个现有提交用作新工作树条目的基础。

**提交式**

关于这个奇怪的术语“有点犯浑”。Commit-ish 指的是最终指向 Git 提交对象的 Git 标识符，如标签、分支或单个提交。例如，当引用提交图的顶端时，您可以通过完整的 40 个字符的散列号、截断的 7 个字符的散列号、标记名或“头”来引用特定的提交。所有这些方法最终都指向同一个引用。
出于我们的目的，我们将只对分支机构名称使用这个术语，因为这是最常见的用例。但是请记住:即使在分离的 HEAD 状态下，您也可以添加单独的提交。更多信息参见[官方 Git 文档](https://git-scm.com/docs/git-worktree)。

一旦选择了基本分支(最常见的)、标签或[提交](https://www.gitkraken.com/learn/git/commit)，系统将提示您选择一个文件夹，作为工作树条目的基本文件夹。记住，每个工作树条目都需要有自己的空文件夹来完成这个过程。

### **提交式**

关于这个奇怪的术语“有点犯浑”。Commit-ish 指的是最终指向 Git 提交对象的 Git 标识符，如标签、分支或单个提交。例如，当引用提交图的顶端时，您可以通过完整的 40 个字符的散列号、截断的 7 个字符的散列号、标记名或“头”来引用特定的提交。所有这些方法最终都指向同一个引用。
出于我们的目的，我们将只对分支机构名称使用这个术语，因为这是最常见的用例。但是请记住:即使在分离的 HEAD 状态下，您也可以添加单独的提交。更多信息参见[官方 Git 文档](https://git-scm.com/docs/git-worktree)。

一旦您在本地文件系统中选择了基本文件夹，您将在命令面板中看到 4 个选项。

让我们仔细看看 VS 代码命令面板下拉菜单中的 4 个创建选项，当您添加一个新的工作树条目时，它显示在上图中。

在 GitLens 中，如何使用 Git worktree add 来添加与工作目录同名的现有分支？

选择选项:`Create Worktree for branch <branch-name>`将在与分支同名的文件夹中创建工作树条目。该分支的完整路径将位于一个新文件夹中，其目录名取自原始文件夹，并附加了术语`.worktrees`。

例如，假设您想要基于分支`dev`创建一个工作树。假设你在一个名为`vscode-gitlens`的目录中工作，并且选择了下一个最高的目录作为新工作树条目的文件夹`..`的基础。您的新工作树条目将位于相对于您当前工作目录的`../vscode-gitlens.worktrees/dev`。

### 在 GitLens 中，如何使用 Git worktree add 来添加与工作目录同名的现有分支？

如何使用 GitLens 创建一个与工作目录同名的新分支？

从命令面板下拉菜单中选择第二个选项:`Create a New Branch and Worktree from branch <branch-name>`将向您显示命令面板中的一个新选项，以命名新的分支。新分支的首次提交将是您在上一步中指定的分支上的最新提交。

### 如何使用 GitLens 创建一个与工作目录同名的新分支？

命名新分支后，GitLens 将在与新分支同名的文件夹中创建新的工作树条目。该分支的完整路径将位于一个新文件夹中，其目录名取自原始文件夹，并附加了术语`.worktrees`。

例如，假设您正在创建一个工作树条目，并希望基于您在上一步中选择的`dev`分支创建一个名为`demo-worktrees-branch`的新分支。我们还假设在一个名为`vscode-gitlens`的目录中工作时，您选择了下一个最高的目录作为新工作树条目的文件夹`..`的基础。在这种情况下，您的新工作树条目和分支将位于相对于您当前工作目录的`../vscode-gitlens.worktrees/demo-worktrees-branch`。

如何使用 Git worktree add 来添加一个与 GitLens 的工作目录名称不同的现有分支？

到目前为止，您已经使用与分支相同的名称命名了目标目录。虽然这是命名目录最常见、最不容易混淆的方式，但 Git 允许您为文件夹取一个不同于分支名称的名称。

如果您从命令面板下拉菜单中选择第三个选项:`Create Worktree (directly in folder) for branch <branch-name>`，您将基于您在目标文件夹中指定的分支创建一个新的工作树条目。

例如，假设您正在从`insiders`分支创建一个工作树条目，并希望将它放在一个名为`vscode-gitlens-demo-1`的目录中。我们还假设您已经选择了下一个最高目录作为新工作树条目的文件夹的基础，`..`。您的新工作树条目将位于相对于您当前工作目录的`../vscode-gitlens-demo-1`。

注意:要使用此选项，您必须在文件部分步骤中选择一个空文件夹(该过程中的步骤 2)。如果文件夹不为空，您将会看到一条错误消息:命令将会失败。

### 如何使用 Git worktree add 来添加一个与 GitLens 的工作目录名称不同的现有分支？

到目前为止，您已经使用与分支相同的名称命名了目标目录。虽然这是命名目录最常见、最不容易混淆的方式，但 Git 允许您为文件夹取一个不同于分支名称的名称。

如果您从命令面板下拉菜单中选择第三个选项:`Create Worktree (directly in folder) for branch <branch-name>`，您将基于您在目标文件夹中指定的分支创建一个新的工作树条目。

例如，假设您正在从`insiders`分支创建一个工作树条目，并希望将它放在一个名为`vscode-gitlens-demo-1`的目录中。我们还假设您已经选择了下一个最高目录作为新工作树条目的文件夹的基础，`..`。您的新工作树条目将位于相对于您当前工作目录的`../vscode-gitlens-demo-1`。

GitLens Worktrees 仍然会在 worktree 条目列表中显示 commit-ish 的名称，不管你给文件夹起了什么名字，这样很容易跟踪你添加了哪个分支、标签或 commit-ish。

**如何使用 Git worktree add 用不同于 GitLens 的工作目录的名字创建一个新的分支？**

从命令面板下拉菜单中选择第四个选项:`Create a New Branch and Worktree (directly in folder) from branch <branch-name>`将向您显示命令面板中的一个新选项，以命名新的分支。新分支的首次提交将是您在上一步中指定的分支上的最新提交。

### **如何使用 Git worktree add 用不同于 GitLens 的工作目录的名字创建一个新的分支？**

例如，假设您正在创建一个工作树条目，您首先创建一个名为“feature-demo”的新分支，您希望将它放在名为`gitlens-feature-demo`的目录中。让我们假设你在一个名为`vscode-gitlens`的目录中工作，并且选择了下一个最高的目录作为新工作树条目的文件夹`..`的基础。在这种情况下，相对于您当前的工作目录，您的新工作树条目将位于`../gitlens-feature-demo`。

注意:要使用此选项，您必须在文件部分步骤中选择一个空文件夹(该过程中的步骤 2)。如果文件夹不为空，您将看到一条错误消息，并且命令将失败。

例如，假设您正在创建一个工作树条目，您首先创建一个名为“feature-demo”的新分支，您希望将它放在名为`gitlens-feature-demo`的目录中。让我们假设你在一个名为`vscode-gitlens`的目录中工作，并且选择了下一个最高的目录作为新工作树条目的文件夹`..`的基础。在这种情况下，相对于您当前的工作目录，您的新工作树条目将位于`../gitlens-feature-demo`。

**用 GitLens 给出工作树列表**

GitLens UI 最好的一点是，你永远不需要运行命令来查看工作树条目列表，你只需要查看`Source Control`菜单的`Worktrees`部分。

## **用 GitLens 给出工作树列表**

现在就启动 GitLens+特性，让可视化工作树和 Git 的其余部分变得更加容易！

**用 GitLens 移除工作树**

要用 GitLens 删除工作树条目，只需从列表中右击或按住 alt 键，并选择`Delete Worktree…`选项。

[Sign up for GitLens+](https://app.gitkraken.com/register?product=gitlens&referrer=gitlens)

## **用 GitLens 移除工作树**

选择`Delete Worktree…`选项后，GitLens 会从命令面板打开`Confirm Delete Worktree`菜单。你可以选择`Delete Worktree`或者`Force Delete Worktree`。强制删除工作树也将删除任何未提交的更改。

**如何通过命令行使用 Git 工作树列表**

如果您使用的是 [Git CLI](https://www.gitkraken.com/cli) ，您将无法立即看到您的活动 Git 工作树，因此您需要运行 Git 工作树列表命令来查看您的工作树条目列表。

`git worktree list`

输出详细信息包括:

## 每个工作树条目的路径

工作树条目中签出的分支的提交哈希

当前为工作树条目签出的分支的名称

**如何通过命令行使用 Git work tree Add**

*   就像用 GitLens 添加新的工作树条目一样，在 CLI 中使用 Git 工作树时，有 4 种可能的场景可供选择。让我们遍历每个 Git 工作树示例。
*   工作树条目中签出的分支的提交哈希
*   **如何在命令行中使用与工作目录同名的 Git worktree add 来添加现有分支？**

`git worktree add path/to/folder/<existing-branch-name>`

## **如何通过命令行使用 Git work tree Add**

就像用 GitLens 添加新的工作树条目一样，在 CLI 中使用 Git 工作树时，有 4 种可能的场景可供选择。让我们遍历每个 Git 工作树示例。

**如何在命令行中使用不同于工作目录的名称对现有分支使用 Git worktree add？**

### 虽然这个命令看起来与前面的命令非常相似，但是请注意文件路径和分支名称之间空格的使用。

`git worktree add path/to/folder/ <existing-branch-name>`

`git worktree add path/to/folder/<existing-branch-name>`

**如何使用 Git worktree add 用命令行创建一个与工作目录同名的新分支？**

### 您可以通过使用`-b`标志告诉 Git 创建一个新的分支。默认情况下，它将基于您当前签出的分支创建新的分支。

`git worktree add -b <new-branch-name> path/to/folder/<new-branch-name>`

### 通过在命令末尾指定您的选择，您还可以选择从另一个分支、标记或提交创建一个新分支。

`git worktree add -b <new-branch-name> path/to/folder/<new-branch-name> <existing-branch-to-use-as-base>`

您可以通过使用`-b`标志告诉 Git 创建一个新的分支。默认情况下，它将基于您当前签出的分支创建新的分支。

`git worktree add -b <new-branch-name> path/to/folder/<new-branch-name>`

**如何使用 Git worktree add 用不同于工作目录的名字用命令行创建一个新的分支？**

`git worktree add -b <new-branch-name> path/to/folder/`

通过在命令末尾指定您的选择，您还可以选择从另一个分支、标记或提交创建一个新分支。

`git worktree add -b <new-branch-name> path/to/folder/<new-branch-name> <existing-branch-to-use-as-base>`

**如何通过命令行使用 Git work tree Remove**

### 要使用 CLI 删除 Git 工作树条目，您需要指定要删除的文件夹。如果分支名称和文件夹名称不同，则不需要命名分支。

`git worktree remove path/to/folder/`

`git worktree add -b <new-branch-name> path/to/folder/`

如果该工作树的文件夹中有未提交的更改，remove 命令将失败。要克服这一点，可以使用 force 选项:`-f`。和使用`--force`一样，在覆盖 Git 的安全预防措施之前，要小心并确保知道自己在做什么。

## **如何通过命令行使用 Git work tree Remove**

要使用 CLI 删除 Git 工作树条目，您需要指定要删除的文件夹。如果分支名称和文件夹名称不同，则不需要命名分支。

`git worktree remove path/to/folder/`

准备好用更简单的方法来管理 Git 工作树了吗？

你的时间是宝贵的，更不用说你的精神能量了。为什么要花时间去记忆或手动跟踪现有的工作树条目呢？为什么要不断运行 Git worktree list 命令？让 [GitLens for VS Code](http://gitkraken.com/gitlens) 为您组织所有这些，并将其呈现在一个漂亮且易于理解的 UI 中。

如果该工作树的文件夹中有未提交的更改，remove 命令将失败。要克服这一点，可以使用 force 选项:`-f`。和使用`--force`一样，在覆盖 Git 的安全预防措施之前，要小心并确保知道自己在做什么。

## 准备好用更简单的方法来管理 Git 工作树了吗？

你的时间是宝贵的，更不用说你的精神能量了。为什么要花时间去记忆或手动跟踪现有的工作树条目呢？为什么要不断运行 Git worktree list 命令？让 [GitLens for VS Code](http://gitkraken.com/gitlens) 为您组织所有这些，并将其呈现在一个漂亮且易于理解的 UI 中。