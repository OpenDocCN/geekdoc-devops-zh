# 如何将 Git 推送到远程分支| Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/git-push-to-remote-branch>

您是否希望将本地分支推送到远程？将本地 [Git 分支](https://www.gitkraken.com/learn/git/branch)推送到[远程](https://www.gitkraken.com/learn/git/tutorials/what-is-git-remote)会用本地分支上的所有提交更新远程分支。推送是为了让其他人可以在远程设备上访问本地更改以获取或提取。

在本文中，您将学习如何使用 [GitKraken 客户端](https://www.gitkraken.com/git-client)将 Git 推送到远程分支，首先在 GitKraken CLI 中，然后在 Git GUI 中查看该过程。

如果您是 Git 新手，管理和更新您的远程项目可能会很棘手。幸运的是，GitKraken Client 通过清晰地可视化您的本地和远程分支使它变得容易，因此您确切地知道您在哪里推动变化。

## **如何使用 CLI 推送至远程分支机构**

虽然您可以在任何终端中按照相同的基本步骤将 Git 推送到远程分支，但是我们将使用强大的 [GitKraken CLI](https://www.gitkraken.com/cli) 来回顾这个过程。

在 GitKraken 客户端，可以通过顶部工具栏的`Terminal`按钮访问 GitKraken CLI。

在从 CLI 运行 Git push 操作之前，您应该首先使用`git status`。运行 [git status](https://git-scm.com/docs/git-status) 将返回索引文件和当前 HEAD commit 之间的差异，以及索引文件和工作树之间的差异。这是一个很好的方式来仔细检查您的工作，并确保您准备好将您的本地更改推送到远程。

在下面的例子中，您可以看到本地主分支在远程主分支 *origin/main* 之前，或者包含一个额外的提交。这意味着您的计算机上本地包含的文件或更改在您的远程计算机上找不到，需要被推上来。

此外，您可以看到消息:" *nothing to commit，working tree clean* "这表明没有未提交的工作要推送到远程分支。

当您对本地分支的状态感到满意时，是时候将您的更改推送到远程分支了。

如果您已经为这个分支设置了上游远程，那么 Git push 命令是您需要运行的唯一操作。设置默认上游分支的好处是，它消除了推送过程中需要您每次选择要推送到哪个分支的步骤。如果这是您选择遵循的流程，并且您已经设置了一个上游分支，只需运行`git push`将您的本地分支推到远程。

因为本例中的 [Git 存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)正在使用 HTTPS，所以我们还必须填写远程用户名和密码来进行身份验证。一旦用户名和密码被接受，所有本地提交都被推送到远程分支。

然而，重要的是要注意，许多开发人员宁愿不设置默认分支，而宁愿指定他们每次都将推送到哪个分支。一些人认为这是一个更加谨慎的过程，可以促进干净的远程存储库，避免设置和切换上游带来的麻烦。

如果您属于不设置上游的阵营，您可以简单地告诉 Git 您每次推送时想要使用哪个远程和分支，使用:

`git push <remote> <branch-name>`

## **如何使用 Git GUI 将分支推送到远程**

要在 GitKraken 客户端中推送分支，请勾选所需的分支，然后只需选择工具栏中的`Push`按钮。

或者，还有两种方法可以让 Git 推送远程分支:

1.  使用带有键盘快捷键`command/ctrl + P` 的命令面板，然后输入“Push”。
2.  右键单击中心图的分支，并从上下文菜单中选择`Push`。

在下面的例子中， [GitHub 集成](https://www.gitkraken.com/integrations/github)被用作通过 HTTPS 与遥控器一起工作的认证。这使得你不必每次在 GitKraken 客户端与远程回购程序交互时都输入你的 GitHub 帐户信息。

GitKraken 客户端用分支名称旁边的笔记本电脑图标来标识本地文件。在本例中，您可以看到本地分支“main”有一个名为“Update README.md”的提交，该提交尚未推送到远程。

ProTip:在推送之前，确保没有 WIP 节点表明仍然需要提交更改。

当您准备好推送时，从顶部工具栏中选择`Push` 按钮。一旦推送完成，本地和远程分支将指向相同的提交，GitKraken 客户端将显示一条*推送成功*的消息。

还没有上游集吗？别担心，GitKraken 的客户会支持你的。GitKraken 客户端将提示您，并询问您希望将本地更改推送到哪个远程和分支。

正如您在这个例子中所看到的，默认情况下，您的本地分支的名称是自动填充的，这样以后在您的远程存储库中就很容易找到。它可以保持不变，或者您可以选择输入一个新的远程分支名称。选择`Submit`完成推送。

## **如何将分支推送到不同的远程上游**

有时，您将需要 Git push 到一个当前没有设置为上游的远程分支。如果这是一种特殊情况，您只想将本地分支推送到特定的远程分支一次，只需将本地分支拖放到所需的远程分支上，然后从自动生成的菜单中选择 push。

如果您想将默认上游更改为不同的远程分支，只需右键单击您想要的分支并选择`Set Upstream`。

## **饭桶** **饭桶推之前先拉**

与他人协作时，最好的做法通常是在推送之前执行 [Git pull](https://www.gitkraken.com/learn/git/problems/pull-remote-git-branch) 。这将使用其他人推送到远程分支的任何更改来更新本地分支。如果远程上存在与本地更改冲突的更改，则可以在本地解决这些更改，以避免在远程上产生冲突。

正如任何犯过这种错误并过早推进的人都知道的那样，试图解开并调试[合并冲突](https://www.gitkraken.com/learn/git/tutorials/how-to-resolve-merge-conflict-in-git)可能是一件非常头疼的事情，需要你和你的团队投入大量的时间。GitKraken Client 可以帮助你避免这个错误，其他人也喜欢它，因为它有非常直观的[团队 Git](https://www.gitkraken.com/blog/git-for-teams)功能。当多个团队成员同时处理文件时，提交图左侧的团队视图会提供警告。此外，交互式 [GitHub pull request](https://www.gitkraken.com/learn/git/problems/github-pull-requests) 管理和各种问题跟踪集成只是 GitKraken Client 受到全球数万家组织信任的部分原因。

想要帮助您的团队花更少的时间修复繁琐的问题，花更多的时间编写惊人的代码吗？尝试 GitKraken 客户端的 Git for Teams 特性来提高您的工作效率！

使用直观的 GitKraken 客户端使得 Git 对于初学者来说易于理解，对于 Git 专家来说更加强大。GitKraken Client 具有拖放功能、传奇的 UX 和强大的可视化工具，可以将您的 Git 工作流提升到一个新的水平。