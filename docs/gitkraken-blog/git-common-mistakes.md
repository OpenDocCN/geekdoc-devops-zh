# 7 个(致命的)常见 Git 错误以及如何修复它们

> 原文：<https://www.gitkraken.com/blog/git-common-mistakes>

Git 是一个版本控制系统，被世界上绝大多数开发人员使用。Git 是由 Linux 大师 Linus Torvalds 开发的，自 2005 年以来已经对公众开放，并使开发人员的生活变得更加容易。

有了 Git，团队工作和文件协作变得更加容易，它有助于更快地开发软件产品。不再强制团队中的每个程序员处理不同的文件；他们可以在同一时间处理相同的文件，最终， [Git 合并](https://www.gitkraken.com/learn/git/git-merge)他们的更改而不影响其他人。Git 帮助开发人员跟踪他们代码库的版本，并有效地与不同的团队成员协作，以实现软件产品的无错误交付。

当然，在这些情况下协作， [Git 合并冲突](https://www.gitkraken.com/learn/git/tutorials/how-to-resolve-merge-conflict-in-git)可能会发生。使用健壮的 [Git 工具](https://www.gitkraken.com/)，像 [GitKraken 客户端](https://www.gitkraken.com/git-client)，可以把你团队的 Git 工作流程带到下一个层次。当两个团队成员同时处理同一个文件时，预测性合并冲突警报等功能会通知您，以便您可以在冲突发生之前先发制人地避免冲突。

虽然 Git 很常见，但它有许多功能——不仅仅是准备、提交和推送对远程 Git 库的更改。Git 实际上非常强大，但是许多开发人员不知道如何利用它提供的所有特性。

很多人害怕在 Git 中犯错误，因为他们没有很好地理解它。每天，开发人员都会对代码库进行大量的修改，因此很难跟踪每一个修改并有效地提交它们。

同时处理多项任务也很容易出错。如果您是一名在 Git 中犯常见错误的开发人员，本文将教您如何不惊慌地修复它们。另一方面，如果你是一名工程经理，想要雇佣[远程开发人员](https://esparkinfo.com/hire-remote-developers.html)，这是与你的团队分享的一个很好的资源，这样他们就能理解这些错误和解决方案。

1.意外删除文件

## 在 Git 中意外删除文件是很常见的错误。例如，在进行功能升级时，开发人员通常认为有些文件是不需要的，在这种预期下，他们会在文件的需求到期之前将其删除。

如果您不小心删除了 Git 中的一个文件，您可能会认为自己犯了一个不可挽回的错误。但是不用担心；Git 对文件非常慷慨。它总是跟踪存储库中添加或删除的文件。这意味着您只需一个 [Git 命令](https://www.gitkraken.com/learn/git/commands)就可以取回您的文件。或者，你可以使用 [GitKraken 客户端](https://www.gitkraken.com/git-client)中神奇的撤销按钮。

“@GitKraken 刚刚在一次丢弃所有后救了我。”–[@ QiMata](https://twitter.com/QiMata/status/1472000085852622850)

如果您不小心删除了 Git 中的一个文件，您可能会认为自己犯了一个不可挽回的错误。但是不用担心；Git 对文件非常慷慨。它总是跟踪存储库中添加或删除的文件。这意味着您只需一个 [Git 命令](https://www.gitkraken.com/learn/git/commands)就可以取回您的文件。或者，你可以使用 [GitKraken 客户端](https://www.gitkraken.com/git-client)中神奇的撤销按钮。

为了更好地理解，我们假设，例如，您已经从 Git 存储库中删除了 404.html 文件。要取回文件，您可以在您选择的终端中键入以下命令。

`git checkout HEAD 404.html`

一旦您运行了上面的 [Git checkout](https://www.gitkraken.com/learn/git/git-checkout) 命令，您将获得您的存储库的 404.html 文件的最新提交版本。如果您使用 Git status 命令检查存储库的状态，您将看到 404.html 不再位于 deleted files 下。现在可以在代码编辑器中对其进行修改。

GitKraken 客户端为用户提供了一个 CLI 和 GUI，因此开发者可以为他们的工作流程选择最好的工具。不再需要手动检查您的回购状态；GitKraken 客户端的图形会随着操作的完成而自动更新，因此您可以立即确认事情是否按预期运行。

2.推送未完成的代码

作为一名开发人员，您经常会最终致力于需要极快交付的重要功能，或者困扰用户并需要快速解决的生产 bug。在这种高压力的情况下，您可能会忘记切换您的工作 [Git 分支](https://www.gitkraken.com/learn/git/branch)，并最终将文件提交到主分支。

将未完成的代码推送到远程主分支可能会对团队工作流的 CI/CD 和部署部分造成严重破坏。

如果您最近[将您的代码推送到一个远程主分支](https://www.gitkraken.com/learn/git/problems/git-push-to-remote-branch)，如果您将远程分支的先前版本与您的本地分支同步，您可以恢复这些更改。您可以使用下面的命令获取远程分支的旧版本。

## `git reset - -hard <remote_branch>/<branch>@{1}`

在这个上下文中使用的 [Git reset](https://www.gitkraken.com/learn/git/git-reset) 命令将使用主分支的本地历史的最后工作版本来重置和更新您的远程分支。如果您想保护这个分支免受这样的直接提交，您也可以在您的远程 Git 存储库中使用下面的设置，并将其与本地同步。

`git config --system receive.denyNonFastForwards true`

3.可怜的 Git 提交消息

编写好的提交消息对于拥有一个每个人都能理解的更好的代码库是必不可少的。提交消息对于任何查看代码的人来说都是不言自明的。诸如“已修复”、“已编辑”等消息。，没有任何附加的上下文就没有任何作用。

许多开发人员在快速工作时犯了添加糟糕的提交消息的错误，最终，他们希望他们已经编写了更好的提交消息，不仅仅是为了他们的团队成员，也是为了他们自己。

好消息是 Git 允许您随时编辑提交消息。要编辑提交消息，使用下面的 [Git commit amend](https://www.gitkraken.com/learn/git/problems/git-commit-amend) 命令:

`git commit --amend`

在终端中键入上述命令后，按 enter 键。这将打开一个文本编辑器，您可以在其中轻松编辑最后一条提交消息。

## 4.意外删除整个 Git 分支

在一个分支上工作很长时间，最后，在没有合并的情况下不小心删除了它，这听起来是开发人员最大的恐惧。但这并不像人们想象的那么重要。您只需从终端发出几个命令就可以恢复该分支。

为了完美地恢复分支，首先需要找到切换到被删除分支的提交。为此，可以使用 Git reflog 命令

获取提交和详细信息的所有日志条目。一旦您找到了提交，您可以使用下面的命令来恢复您删除的分支，这样您就可以合并并将其推送到远程分支。

`git checkout -b <branch-name> <commit-id>`

在上面的命令中，`branch-name`是您删除的分支名称，`commit-id`是您从 Git reflog 结果中确定的提交。

5.错误的 Git 提交

## 提交时出错是一个常见的 Git 错误，但也可能是毁灭性的。例如，您的提交可能会给您的代码库带来问题，或者与另一个变更发生冲突。

如果错误通过了部署阶段，这些错误会对生产构建造成不可估量的损害。但是如果你及早发现错误，你可以采取措施来补救。

通过使用下面的 [Git revert commit](https://www.gitkraken.com/learn/git/problems/revert-git-commit) 命令恢复到旧的提交来修复错误的提交:

`git revert <commit-id>`

在上面的命令中，`commit-id`是旧提交的提交 id，它具有更好、更稳定的代码版本。

6.Git 提交太多

Git 习惯于增量地跟踪代码库中的变更，但有时人们最终会进行太多的提交。这通常发生在你尝试新事物的时候，当事情开始起作用时，你在整个过程中不断地提交。

最后，当实验成功，并且您准备好提交到主分支时，您可能会留下许多带有不太清楚的提交消息的提交。这些提交会让其他开发人员很难理解你的代码。

在这种情况下，您可以求助于从终端压缩提交，然后将您的更改推送到远程。下面是将多个提交压缩为一个的过程。

## 5.错误的 Git 提交

首先，使用下面的命令签出到您想要合并的分支:

`git checkout <branch-name>`

接下来，使用以下命令:

`git merge - -squash <branch-name-to-be-merged>`

一旦执行了上面的两个命令，就会有一些需要解决的合并冲突。

7.忘记添加文件了

忘记添加文件是另一个常见的 Git 错误，可能会带来麻烦。有时您可能有不同的目录，如果您忘记在提交中添加一个重要的文件，您可能会得到一个糟糕的代码库。

要快速修复这个错误，可以使用下面的 Git add 命令:

## `git add <missed-file>`

执行上述命令后，使用:

`git commit - -amend`

这将把文件添加到最后一次提交。

减少常见的 Git 错误

Git 是一个强大的工具，但是你只能通过[学习更多关于 Git](https://www.gitkraken.com/blog/best-git-training) 的知识来利用它的所有好处。没什么好害怕的。Git 很复杂，但是一旦你[学会了 Git](https://www.gitkraken.com/learn/git) ，你就不会对错误感到恐慌，并且会对你的工作流程更有信心。

不要担心犯这些常见的 Git 错误——您可以用几个命令来修复它们。

1.  首先，使用下面的命令签出到您想要合并的分支:

GitKraken 客户端帮助所有技能水平的开发人员更好地理解 Git 操作，实现更自信的工作流，减少错误。今天就升级到世界上最流行的 Git 客户端——GUI 和终端！

2.  接下来，使用以下命令:

`git merge - -squash <branch-name-to-be-merged>`

3.  一旦执行了上面的两个命令，就会有一些需要解决的合并冲突。

7.忘记添加文件了

## 忘记添加文件是另一个常见的 Git 错误，可能会带来麻烦。有时您可能有不同的目录，如果您忘记在提交中添加一个重要的文件，您可能会得到一个糟糕的代码库。

要快速修复这个错误，可以使用下面的 Git add 命令:

`git add <missed-file>`

执行上述命令后，使用:

`git commit - -amend`

这将把文件添加到最后一次提交。

减少常见的 Git 错误

Git 是一个强大的工具，但是你只能通过[学习更多关于 Git](https://www.gitkraken.com/blog/best-git-training) 的知识来利用它的所有好处。没什么好害怕的。Git 很复杂，但是一旦你[学会了 Git](https://www.gitkraken.com/learn/git) ，你就不会对错误感到恐慌，并且会对你的工作流程更有信心。

不要担心犯这些常见的 Git 错误——您可以用几个命令来修复它们。

## GitKraken 客户端帮助所有技能水平的开发人员更好地理解 Git 操作，实现更自信的工作流，减少错误。今天就升级到世界上最流行的 Git 客户端——GUI 和终端！

Git is a powerful tool, but you can only leverage all of its benefits by [learning more about Git](https://www.gitkraken.com/blog/best-git-training). There’s nothing to fear. Git is complex, but once you’ve [learned Git](https://www.gitkraken.com/learn/git), you won’t panic about mistakes and will feel more confident in your workflow. 

Don’t worry about making these common Git mistakes–you can fix them with a few commands. 

GitKraken Client helps developers of all skill levels better understand Git actions, enabling a more confident workflow with less mistakes. Level up today with the most popular Git client in the world – with a GUI and terminal!