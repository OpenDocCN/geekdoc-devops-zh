# 如何获得推动力| Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/git-push-force>

Git push 命令获取您在本地机器上所做的更改，并更新您的[远程存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-git-remote)以反映这些更改。开发人员使用这个命令来更新他们的远程存储库，以便与项目合作者共享最准确的 Git 历史。

Git push 是更新远程存储库的首选命令。Git push 不仅接受您的本地更改并将它们添加到远程，而且不会覆盖远程先前保存的任何 Git 历史。

但是，在某些情况下，您需要覆盖远程历史记录。这就是 Git push force 的用武之地，您可能听说过它被称为“Git force push”或“force pushing”。

请继续阅读，了解如何在您的工作流中有效、安全地利用 Git force pushing。在本文中，我们将介绍如何使用 [GitKraken 客户端](https://www.gitkraken.com/git-client)强制推送，首先在 [CLI](https://www.gitkraken.com/cli) 中，然后在 Git GUI 中。

GitKraken 客户端通过撤销/重做按钮等功能，最大限度地降低了 Git 中可能存在的危险操作的风险，如强制推送。希望您的工作流程更加安全？⬇️

## **Git Push Force 是如何工作的？**

与 Git push 不同，`git push –force`不要求您的本地存储库与远程存储库完全同步。恰恰相反，Git push force 使远程存储库与本地存储库相匹配。

### **Git 推力的风险**

Git push force 会覆盖远程存储库，以完全匹配您运行该命令时本地存储库的样子。这意味着在运行 Git push force 之前，您需要确保您的本地存储库完全是最新的，包含来自远程的最新更改，否则您将面临丢失提交的风险。

例如，假设一个团队成员将新的变更推送到一个遥控器上，而您忘记了提取它们。因为您未能提取这些变更，所以它们不会反映在您的本地存储库中。在这种情况下，如果您执行 Git push force，您将用本地 repo 的副本替换远程存储库，实际上删除了团队成员的工作。

使用 Git push force 仍然没有确保在运行命令之前获取最新的更改那么简单。可能您的一个或多个团队成员正在基于旧的提交历史处理变更。如果你在这种情况下强行推动，如果你没有将团队成员的贡献融入到你当前的工作中，这可能会使他们变得过时。

如果你决定要保留你的项目的历史，但仍然想对你的远程回购做一个小的改变，考虑使用`git revert <commit>`用[恢复你的提交](https://www.gitkraken.com/learn/git/problems/revert-git-commit)。这不会像强制推送那样改变项目的历史，并且可以帮助您撤销与代码库冲突的提交。

## **什么时候应该使用 Git 推力？**

在一些情况下，使用 Git push force 是最好的做法，尽管可能会有负面影响。

您可能需要在 Git 中强制推送的一些常见示例包括:

*   在你在本地对一个分支进行 Git rebase 之后
*   在[挤压已经被推到远程的提交](https://www.gitkraken.com/learn/git/git-squash)之后
*   在敏感数据被意外推送到远程设备并需要删除后

如果您是 Git 新手，并且对使用终端感到不确定，GitKraken CLI 会根据您的活动为 Git 命令提供自动建议，甚至会提供它们的简要描述。

## **用命令行给出推力**

虽然您可以在任何终端中遵循相同的基本步骤来强制推送，但我们将使用强大的 [GitKraken CLI](https://www.gitkraken.com/cli) 来回顾这个过程。

要访问 [GitKraken 客户端](https://www.gitkraken.com/git-client)中的 CLI，点击顶部工具栏中的`Terminal`按钮。

您将希望从使用`git fetch --all`从遥控器获取任何更改开始。执行一个 [Git 获取](https://git-scm.com/docs/git-fetch)从远程获取任何还没有应用到本地 repo 的更改，并添加它们。

虽然这一步对于执行强制推送不是必需的，但是这是一个很好的做法，因为它允许您验证最近没有推送任何您不想覆盖的内容。

接下来，您应该执行一个[交互式重置](https://www.gitkraken.com/learn/git/problems/git-interactive-rebase)来挤压您的提交。这种实践促进了更干净的远程存储库，并使您的团队成员更容易找到您的变更并与之交互。要执行交互式重置基础和压缩一组提交:

1.  使用`git log`找到您将开始工作的基础提交的 SHA。
2.  运行`git rebase -i <SHA>`开始交互式重置基础。
3.  使用像 Vim 这样的文本编辑器程序，选择您想要压缩的提交。
4.  使用`Escape`键退出文本编辑器，然后键入`:wq`。
5.  最后，基于所有文件编写一个全面的提交消息，然后使用`Escape`和`:wq`完成。

作为一般的最佳实践，您可能还想运行`git status`来确认您的本地分支是否处于预期状态。运行 [Git status](https://git-scm.com/docs/git-status) 将返回索引文件和当前 HEAD commit 之间的差异，以及索引文件和工作树之间的差异。

一旦您采取了这些步骤来为 Git push force 创建一个更安全的环境，您现在就可以运行下面的命令来强制 push 到远程存储库:

`git push --force`

从 CLI 考虑的另一个安全措施是使用`git push force-with-lease`。如果使用此标志发现对远程数据库所做的更改没有反映在您的本地存储库中，则它将无法强制推送。

回想一下我们过去的例子。如果一个团队成员已经对远程进行了更改，而您没有提取这些更改，那么单独运行 Git push force 将会覆盖您的团队成员的贡献。但是，如果使用 lease 运行 Git push force，那么 force push 将会失败，因为它识别出您需要从远程获取更改。如果自上次拉动后遥控器没有发生任何变化，强制推送将继续进行。

从 CLI 考虑的另一个安全措施是使用`git push force-with-lease`。如果使用此标志发现对远程数据库所做的更改没有反映在您的本地存储库中，则它将无法强制推送。

**使用 Git GUI 获得推力**

就像如果您要使用 [CLI](https://www.gitkraken.com/cli) ，在 Git GUI 中执行强制推送之前，您会希望先执行一个获取操作。您可以在 [GitKraken 客户端](https://www.gitkraken.com/git-client)中通过选择顶部工具栏中`Pull`按钮右侧的小`^` 箭头来完成此操作。从随后的下拉菜单中，选择`Fetch All`。如前所述，首先执行这个操作有助于降低意外覆盖工作的风险。

## **使用 Git GUI 获得推力**

就像如果您要使用 [CLI](https://www.gitkraken.com/cli) ，在 Git GUI 中执行强制推送之前，您会希望先执行一个获取操作。您可以在 [GitKraken 客户端](https://www.gitkraken.com/git-client)中通过选择顶部工具栏中`Pull`按钮右侧的小`^` 箭头来完成此操作。从随后的下拉菜单中，选择`Fetch All`。如前所述，首先执行这个操作有助于降低意外覆盖工作的风险。

一旦您对本地回购感到满意，您可以使用`Push`按钮将您的更改推送到遥控器。使用 GitKraken 客户端强制推送时，会出现一个横幅，有以下选项:`Pull (fast forward if possible)`、`Force Push`或`Cancel`。

注意:`Pull (fast forward if possible)`获取远程分支上的任何更新，并尝试快进或移动本地分支，以指向与远程分支相同的提交。如果不能快进，将执行 [Git 合并](https://www.gitkraken.com/learn/git/git-merge)。`Cancel`将取消推送。

在下面的例子中，有几个提交应该被压缩。为此，使用`command/ctrl`从图中多选所需的提交，然后选择`Squash 3 commits`。请注意，本地主分支现在在历史上与远程主分支不同。

注意:`Pull (fast forward if possible)`获取远程分支上的任何更新，并尝试快进或移动本地分支，以指向与远程分支相同的提交。如果不能快进，将执行 [Git 合并](https://www.gitkraken.com/learn/git/git-merge)。`Cancel`将取消推送。

在下面的例子中，有几个提交应该被压缩。为此，使用`command/ctrl`从图中多选所需的提交，然后选择`Squash 3 commits`。请注意，本地主分支现在在历史上与远程主分支不同。

既然本地主分支指向了期望的提交，那么是时候 Git force push 这个更改来从远程历史中删除其他提交了。为此，在 GitKraken 客户端的顶部工具栏中选择`Push`。然后，系统会提示您以下消息:

既然本地主分支指向了期望的提交，那么是时候 Git force push 这个更改来从远程历史中删除其他提交了。为此，在 GitKraken 客户端的顶部工具栏中选择`Push`。然后，系统会提示您以下消息:

选择`Force Push`，最后再次选择`Force Push`确认。因为这是一个潜在的破坏性行动， [GitKraken 客户端](https://www.gitkraken.com/git-client)给你机会仔细检查，确保你想继续进行。

选择`Force Push`，最后再次选择`Force Push`确认。因为这是一个潜在的破坏性行动， [GitKraken 客户端](https://www.gitkraken.com/git-client)给你机会仔细检查，确保你想继续进行。

您可以通过检查本地和远程主分支是否指向同一个提交，以及其他不需要的提交是否已被删除，来确认强制推送是否成功。

**团队的远程回购健康和 Git**

基于我们已经介绍的概念，很容易理解为什么保持一个干净的远程存储库如此重要。当与团队合作时，需要一个干净的远程存储库。有了这么多的接触点和贡献者，建立清晰的存储库标准并与您的合作者尽可能多地促进项目可见性是至关重要的。

## GitKraken 客户端的 Git for Teams 功能会在您和团队成员处理同一个文件时提醒您，以便您可以避免潜在的合并冲突，深度链接允许您与团队快速共享项目的特定部分。GitKraken Client 让团队协作变得无缝和简单，这样你就可以专注于编写令人惊叹的代码。

基于我们已经介绍的概念，很容易理解为什么保持一个干净的远程存储库如此重要。当与团队合作时，需要一个干净的远程存储库。有了这么多的接触点和贡献者，建立清晰的存储库标准并与您的合作者尽可能多地促进项目可见性是至关重要的。

想要帮助您的团队改善他们的远程存储库的健康状况，避免合并冲突，并提高生产力吗？GitKraken 客户端可以帮助您完成所有这些任务，甚至更多。立即下载，向改善团队协作和成功迈出第一步。

想要帮助您的团队改善他们的远程存储库的健康状况，避免合并冲突，并提高生产力吗？GitKraken 客户端可以帮助您完成所有这些任务，甚至更多。立即下载，向改善团队协作和成功迈出第一步。