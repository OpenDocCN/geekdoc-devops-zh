# Git 拉远程分支|学习如何在 Git 中从远程分支拉

> 原文：<https://www.gitkraken.com/learn/git/problems/pull-remote-git-branch>

假设您的本地分支已经过时，您需要从远程分支获取更改，以便让本地分支跟上速度。

为了从您的远程获取这些更改，或者换句话说，将更改下载到您的本地分支，您将执行 Git pull。

在幕后，Git pull 实际上是一个 Git fetch，后跟一个 [Git merge](https://www.gitkraken.com/learn/git/git-merge) 。Git pull 只是在一个步骤中执行这两个操作的捷径。

在展示 [Git pull](https://www.gitkraken.com/learn/git/tutorials/what-is-git-pull) 如何在 CLI 中工作之前，让我们回顾一下如何使用跨平台 GitKraken 客户端 Git pull 远程分支。

“我使用@GitKraken Client，因为我可以集中精力完成工作，而不是试图记住命令和想象分支是如何连接的。”–[@ palmiak _ FP](https://twitter.com/palmiak_fp/status/1418573620637487104)

## 如何在 GitKraken 客户端中获取远程分支？

使用极其强大的 [GitKraken 客户端](https://www.gitkraken.com/git-client)的可视化辅助，从远程 Git 分支提取更改非常简单。

在本例中，我们将从远程分支获取更改，并使本地分支加速。

GitKraken Client 简化了您的工作，因为默认情况下，GitKraken Client 每分钟自动从您的远程存储库获取更新。如果您只喜欢手动提取，您可以从`Preferences ->` `General`菜单中更改此设置。

如果 GitKraken 客户端没有自动获取更改，只需点击顶部工具栏中的`Pull`按钮，并从下拉菜单中选择`Fetch`选项。这将为您当前签出的分支获取远程。

如果您想要获取相关的变更并将其合并到您的本地分支中，那么您可以选择一个合并策略供 GitKraken 客户端实现。

您可以看到，当您想要在 GitKraken 客户端中提取远程分支时，有多个选项可供选择。

GitKraken 客户端允许您轻松选择执行带有快进功能的[Git pull](https://support.gitkraken.com/working-with-repositories/pushing-and-pulling/#pull-fast-forward-if-possible)、 [Git pull fast forward only](https://support.gitkraken.com/working-with-repositories/pushing-and-pulling/#pull-fast-forward-only) 或 [Git pull rebase](https://support.gitkraken.com/working-with-repositories/pushing-and-pulling/#pull-rebase) 。不需要记住或键入任何命令！

GitKraken 的易于阅读的提交图将帮助初学者和高级用户可视化分支结构，这样您就可以准确地知道谁在什么时候做了什么代码更改，甚至是您的分支和远程上的活动。

## 如何在命令行中获取远程分支？

如果您正在使用一个终端来学习 Git，比如 [GitKraken CLI](https://www.gitkraken.com/cli) ，您将从下面的命令开始:

```
git pull
```

Git 拉式原点 Main

执行 Git 拉取的一个最常见的例子是使用命令:

`git pull origin main`

为什么 Git pull origin 主命令在例子中如此常见？在 Git 中，您为本地存储库添加的第一个远程被默认命名为`origin`。对于许多存储库来说，只有一个远程集合，所以`origin`是最流行的远程名称。

正如`origin`是默认的远程名称一样，“main”是当前关于如何称呼主工作分支的行业标准。在一些较老的文档和存储库中，你可能会看到它被标记为`master`分支，使得命令 Git pull origin master 成为标准，将其重命名为`main`。你可以在这里阅读更多关于为什么“main”是当前默认设置的信息。

## 
不要求具有称为`origin`的遥控器或称为`main`的分支。一些 Git 工作流完全消除了这些命名约定，更喜欢像`dev`、`staging`和`production`这样的术语，正如你在本页示例中看到的 repo。

尽管注意为什么这如此普遍很重要，但更重要的是要认识到所有的 Git 拉取都遵循通用格式:`git pull <remote-name> <branch-name>`，而不管任何特定的命名约定。

无论您做出什么决定，您都可以在您的 [Git 配置设置](https://git-scm.com/docs/git-config)中设置 Git 使用的默认远程名称以及默认分支命名策略。

仅 Git 拉快进

如果不需要合并，Git 将快进您的本地分支。这意味着您的本地分支现在将指向来自远程分支的最新提交，而不会合并。但是，如果因为需要合并而无法快速前进，将改为执行合并。

***GitTip:了解更多关于如何[合并一个 Git 分支](https://www.gitkraken.com/learn/git/problems/merge-git-branch)，包括如何将一个 Git 分支与 main 合并。***

无论您做出什么决定，您都可以在您的 [Git 配置设置](https://git-scm.com/docs/git-config)中设置 Git 使用的默认远程名称以及默认分支命名策略。

仅 Git 拉快进

## 如果您只想引入无冲突的更改，而不想执行合并，您可以选择仅使用快进来提取分支。

现在，如果您希望只使用快速转发来提取分支，那么您可以将`--ff-only`标志附加到`git pull`命令上。

```
git pull --ff-only
```

Git Pull Rebase

或者，如果您更喜欢在合并文件更改时执行 [Git rebase](https://www.gitkraken.com/learn/git/git-rebase) ，您可以选择从您正在拉取的远程 Git 分支中对提交进行 rebase，而不是合并它们。

您可以使用`--rebase`标志来实现这一点。

现在，如果您希望只使用快速转发来提取分支，那么您可以将`--ff-only`标志附加到`git pull`命令上。

Git Pull Rebase

你有没有尝试过 [Git 交互式 rebase](https://www.gitkraken.com/learn/git/problems/git-interactive-rebase) ？使用 GitKraken 客户端执行复杂的 Git 操作(如 rebase 和交互式 rebase)是多么容易，您一定会大吃一惊！此外，您将简化更简单、更常见的操作过程，比如拉一个远程分支。

## 您可以使用`--rebase`标志来实现这一点。

你有没有尝试过 [Git 交互式 rebase](https://www.gitkraken.com/learn/git/problems/git-interactive-rebase) ？使用 GitKraken 客户端执行复杂的 Git 操作(如 rebase 和交互式 rebase)是多么容易，您一定会大吃一惊！此外，您将简化更简单、更常见的操作过程，比如拉一个远程分支。

```
git pull --rebase
```

你有没有尝试过 [Git 交互式 rebase](https://www.gitkraken.com/learn/git/problems/git-interactive-rebase) ？使用 GitKraken 客户端执行复杂的 Git 操作(如 rebase 和交互式 rebase)是多么容易，您一定会大吃一惊！此外，您将简化更简单、更常见的操作过程，比如拉一个远程分支。

Have you ever attempted a [Git interactive rebase](https://www.gitkraken.com/learn/git/problems/git-interactive-rebase)? You will be blown away by how easy it is to perform complex Git actions, such as rebase and interactive rebase, with the GitKraken Client! Plus, you will streamline the process for easier, but more common, actions, like pulling a remote branch.