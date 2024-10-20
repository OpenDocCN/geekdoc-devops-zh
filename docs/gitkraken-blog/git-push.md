# 了解如何 Git 推送| Git 推送本地分支到远程分支

> 原文：<https://www.gitkraken.com/learn/git/git-push>

Git push 命令将本地更改上传到您的远程存储库。

通常，当使用 Git 时，您的代码既存在于您计算机上的本地存储库中，也存在于服务器上的一个或多个存储库中。我们称存储在服务器上的回购文件为“[远程文件](https://www.gitkraken.com/learn/git/git-remote)”。

Git push 会将 [Git commits](https://www.gitkraken.com/learn/git/commit) 从您的本地存储库上传到您的远程存储库，比如存储在 GitHub 或 GitLab 上的 repos。Git push 通常用在开发工作流中，使本地更改可以在远程访问，以便其他协作者可以获取或提取最新的项目历史。

运行 Git push 不会覆盖您的原始文件。Git 知道哪些提交已经存在于上游分支上，并且将只上传从您的本地存储库中推送的新更改。

在本文中，我们将介绍使用 Git push 及其相关操作的来龙去脉，包括如何连接到远程存储库，设置默认上游、删除远程分支的利弊，以及如何正确使用 Git push force。

喜欢从 CLI 推送您的更改吗？GitKraken Client 允许您利用终端的速度，同时在同一个窗口中提供可视化提交图。

## **Git 推送的好处**

您希望将提交推送到远程有两个主要原因:

1.  对您的存储库和分支进行远程备份。大多数 Git 托管服务本质上充当你的回购的“云备份”。如果你的计算机出了什么问题，你可以放心，你的代码不会永远消失。
2.  与他人共享您的代码。无论是与您的开发团队共享代码，还是向开源社区公开共享代码，对于开发人员来说，远程推送都是一种共享和协作项目的便捷方式。其他人可以克隆或派生您的 repo 来查看您的代码并做出更改。根据您使用的 Git 主机的不同，谁可以访问您的远程设备会有不同的配置。

## **连接到远程存储库**

开始一个项目有两种主要的方法。您可以[克隆一个现有的存储库](https://www.gitkraken.com/learn/git/git-clone)或者创建一个新的 [Git 存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)。克隆存储库不需要您连接到远程，因为它会自动为您创建一个远程。

然而，如果您在本地机器上使用 GitKraken 客户端的终端标签中的`git init`或者通过简单地从 GUI 中选择`Start a local repo`按钮创建了一个新的存储库，您将需要手动连接到一个远程服务器以便上传和共享您的项目。

GitKraken 客户端可以在各种流行的托管服务上添加远程存储库，包括 [GitHub](https://www.gitkraken.com/integrations/github) 、 [GitLab](https://www.gitkraken.com/integrations/gitlab) 、 [Bitbucket](https://www.gitkraken.com/integrations/bitbucket) 和 [Azure DevOps](https://www.gitkraken.com/integrations/azure-devops) 。

GitKraken 客户端集成了许多主要的存储库托管服务，因此您可以花更少的时间尝试连接存储库，而将更多的时间用于编写令人惊叹的代码。

要使用 [GitKraken 客户端](https://www.gitkraken.com/git-client)中的 GUI 将您的本地项目连接到远程项目，导航到左侧面板，悬停在`Remote`上并选择右侧的`+`图标。接下来，在顶部选择你想要的远程托管服务。最后，选择`Create remote and push local refs`。屏幕左下方将出现一条提示信息“已成功创建回购”。

您也可以使用 URL 添加 GitKraken 客户端的现有遥控器。在以这种方式添加远程之前，您需要在您的托管站点上创建一个远程存储库，这样您就可以访问项目的 URL。

项目的 URL 通常会显示在您首选的托管网站的显著位置，便于访问。要添加现有遥控器，请在 GitKraken 客户端中执行以下步骤:

1.  从您的存储库托管站点复制 URL 并返回到 GitKraken 客户端
2.  导航到左侧面板，悬停在`Remote`上，选择右侧的`+`图标
3.  选择 URL 并输入项目名称
4.  将 URL 粘贴在标有`Pull URL`和`Push URL`的部分下
5.  选择`Add remote`按钮

您还可以使用带有`remote add`命令的 [Git CLI](https://www.gitkraken.com/cli) 将您的本地存储库连接到远程服务器。要访问 GitKraken 客户端中的 CLI 选项卡，请选择位于主工具栏右侧的`Terminal`按钮。

您的屏幕应该看起来像这样:

GitKraken 客户端 CLI 界面，终端在顶部，GUI 提交图在底部。

现在导航到您的托管站点，使用 HTTPS 或 SSH 复制远程回购的 URL。下面的例子展示了 GitHub 中 URL 的例子。

*GitHub 页面，您可以在其中复制远程存储库的 URL*

现在您已经有了所有必要的信息，可以将本地 repo 连接到新创建的 remote 了。要连接这些回购，请使用:

`git remote add origin <url of remote>`

在 CLI 中，您可以通过运行`git remote`来仔细检查遥控器是否已连接。您应该看到终端在运行该命令后返回了`origin`。这意味着您已经成功地将本地项目连接到了远程存储库。

## **Git 将本地分支推送到远程**

为了将更改推送到您的遥控器，您需要首先对您的本地存储库进行更改。为了让这些变更处于可以被推送的状态，您需要[暂存或添加](https://git-scm.com/docs/git-add)它们，然后提交这些变更。

如果您的本地存储库在远程存储库的“前面”,也就是说本地存储库上的更改还没有反映到远程存储库中，那么您可以验证您确实准备好将更改推送到远程存储库了。在 GitKraken 客户端中，您可以从 GUI 的中央提交图中看到您的本地项目是否在远程项目之前。

*GitKraken 客户端提交图*

要使用 CLI 查看本地是否领先于远程，您需要运行`git status`。在这种情况下，您可以看到本地在远程之前，因为终端返回了消息:“您的分支在`origin/main`之前 1 次提交。”

*GitKraken 客户端 CLI*

现在，您已经准备好将您的更改推送到远程，让我们看看如何使用 GitKraken 客户端的 GUI 和 CLI 进行 Git push。

使用 GitKraken 客户端的 GUI，有 3 种方法可以推动您的更改:

检查所需的分支，然后选择顶部工具栏中的`Push`按钮。

使用带有键盘快捷键`command/ctrl + P`的命令面板，然后输入“Push”。

1.  右键单击中心图的分支，并从上下文菜单中选择`Push`。
2.  使用带有键盘快捷键`command/ctrl + P`的命令面板，然后输入“Push”。
3.  右键单击中心图的分支，并从上下文菜单中选择`Push`。

要使用 GitKraken 客户端的 CLI 推送更改，请从顶部工具栏选择`Terminal`按钮打开终端选项卡。[通过使用`git checkout <branch name>`检查您想要推送变更的分支](https://www.gitkraken.com/learn/git/problems/git-checkout-remote-branch)。要 Git 推送您的更改，请使用以下命令:

`Git push <remote name> <branch name>`

要使用 GitKraken 客户端的 CLI 推送更改，请从顶部工具栏选择`Terminal`按钮打开终端选项卡。[通过使用`git checkout <branch name>`检查您想要推送变更的分支](https://www.gitkraken.com/learn/git/problems/git-checkout-remote-branch)。要 Git 推送您的更改，请使用以下命令:

`Git push <remote name> <branch name>`

**设置默认上游分支**

在将变更推送到远程时，有两种截然不同的开发人员工作流。一些开发人员更喜欢[设置一个默认的上游分支](https://www.gitkraken.com/learn/git/problems/git-set-upstream-branch),而其他人则更喜欢手动选择每次将他们的更改推送到哪个分支。

### 一些开发人员选择设置默认上游分支的原因是，它消除了推送过程中的一个步骤。他们不必每次都选择将提交推送到哪个远程分支，而是在本地分支和远程分支之间创建一种关系，我们称之为设置默认上游，告诉 Git 将相关本地分支上的所有更改推送到连接的远程分支。

如果您想要使用 GitKraken 客户端的 GUI 设置上游，只需导航到左侧面板，将鼠标悬停在您想要设置上游的本地分支上，单击省略号图标，然后选择`Set Upstream`。

一些开发人员选择设置默认上游分支的原因是，它消除了推送过程中的一个步骤。他们不必每次都选择将提交推送到哪个远程分支，而是在本地分支和远程分支之间创建一种关系，我们称之为设置默认上游，告诉 Git 将相关本地分支上的所有更改推送到连接的远程分支。

*为分支选择省略号时可用的下拉选项*

要使用 GitKraken 客户端的内置 CLI 设置上游分支，请在 push 命令中使用`--set-upstream`标志。例如，它可能看起来像这样:

`Git push --set-upstream origin main`

这将建立一个默认的上游分支，Git 将自动从这个远程分支向前推和拉。

`Git push --set-upstream origin main`

这将建立一个默认的上游分支，Git 将自动从这个远程分支向前推和拉。

这让我们想到了另一个学派:不要设置默认上游。这些开发人员选择手动输入分支名称，每次他们将提交上传到远程时，他们都希望将分支名称推送到该分支。这个过程被一些人认为是一个更有意图的过程，它促进了干净的远程存储库，避免了设置和切换上游带来的麻烦。

如果您不想设置默认的上游，您可以简单地继续使用前面例子中的 push 命令。要么使用 GUI 中的一个推送选项，如`Push`按钮，要么使用命令行中的`git push <remote name> <branch name>`。

这让我们想到了另一个学派:不要设置默认上游。这些开发人员选择手动输入分支名称，每次他们将提交上传到远程时，他们都希望将分支名称推送到该分支。这个过程被一些人认为是一个更有意图的过程，它促进了干净的远程存储库，避免了设置和切换上游带来的麻烦。

GitKraken 客户端中神奇的自动建议功能将填充 Git push 命令的所有可用选项，但是如果您更喜欢使用 GUI，您可以通过拖放来实现快速推拉！

**删除远程分支**

当您开始与各种本地和远程存储库交互时，您可能会遇到需要[删除远程 Git 分支](https://www.gitkraken.com/learn/git/problems/delete-remote-git-branch)的情况。例如，要升级一个干净的存储库，您可能需要删除一个合并到主分支中的远程功能分支。因为远程存储库应该是您和您的团队维护的最准确的工作副本，所以删除远程分支应该小心处理。确保您和您的团队已经进行了有效的沟通，并同意删除有问题的分支。

## 使用 GitKraken 客户端中的 GUI，您可以通过简单地导航到左侧面板的`Remotes`部分下的`right-clicking`远程分支，然后选择`Delete <branch name>`来删除远程分支。

当您开始与各种本地和远程存储库交互时，您可能会遇到需要[删除远程 Git 分支](https://www.gitkraken.com/learn/git/problems/delete-remote-git-branch)的情况。例如，要升级一个干净的存储库，您可能需要删除一个合并到主分支中的远程功能分支。因为远程存储库应该是您和您的团队维护的最准确的工作副本，所以删除远程分支应该小心处理。确保您和您的团队已经进行了有效的沟通，并同意删除有问题的分支。

*右键点击分支后的下拉选项*

也许与直觉相反，远程上的分支是用 CLI 上的 Git push 命令删除的。要使用 CLI 删除远程分支，请导航到终端并使用:

`git push -d <remote> <remote branch name>`

也许与直觉相反，远程上的分支是用 CLI 上的 Git push 命令删除的。要使用 CLI 删除远程分支，请导航到终端并使用:

*删除 GitKraken 客户端终端中的 Git 分支“How-to-example”*

**什么是 Git 推力？**

[强制推送](https://www.gitkraken.com/learn/git/problems/git-push-force)是 Git push 命令的变体，是用本地历史覆盖存储在远程存储库中的提交历史的有效方法。使用 Git push force 时应该非常小心，因为它可能会产生意想不到的后果，包括丢失提交。

## 虽然存在强制推送的可接受场景，但它们代表了规范的例外。强制推送不应该成为您工作流的一个正常部分，因为它可能会彻底改变您的远程存储库，这可能会给您和您的团队带来重大问题。

在 GitKraken 客户端的 GUI 中启动强制推送时，会出现一个带有以下选项的横幅:`Pull (fast forward if possible)`、`Force Push`或`Cancel`。如果您 100%确定要强制推送，只需选择`Force Push`选项。您需要再次选择`Force Push`进行确认。这是 GitKraken 采取的另一项安全措施，以帮助您避免无意的力推。

虽然存在强制推送的可接受场景，但它们代表了规范的例外。强制推送不应该成为您工作流的一个正常部分，因为它可能会彻底改变您的远程存储库，这可能会给您和您的团队带来重大问题。

*使用 GitKraken 客户端的 GUI 强制推送*

在 GitKraken 客户端的 CLI 中使用 Git push force 同样简单。导航到顶部工具栏中的终端选项卡。点击进入终端，输入`git push –force`。

*使用 GitKraken 客户端的终端强制推送*

**强制推租**

使用[Git push-force-with-lease](https://git-scm.com/docs/git-push#Documentation/git-push.txt---no-force-with-lease)是一种利用强制推送的方法，但它也为你提供了更多的安全保障。`--force-with-lease`标志告诉 Git，只有在远程设备上有任何您尚未拉取或获取到本地存储库的更改时，才强制推送。如果 Git 检测到远程分支上的更改将会丢失或被覆盖，那么强制推送将会失败。

## **强制推租**

GitKraken 客户端使运行复杂的动作，如力推，安全和容易。另外，autosuggest 特性提供了 Git 命令功能的简要描述，因此您可以更放心地进行编码。

**让你的代码和你自己更进一步**

不管你是新手还是 Git 老手，知道如何有效地使用 Git push 可以让你开始为项目做贡献。能够对团队的存储库做出有意义的贡献是非常令人鼓舞的。GitKraken Client 在此帮助您将令人惊叹的代码推向世界各地的项目。

## **让你的代码和你自己更进一步**

不管你是新手还是 Git 老手，知道如何有效地使用 Git push 可以让你开始为项目做贡献。能够对团队的存储库做出有意义的贡献是非常令人鼓舞的。GitKraken Client 在此帮助您将令人惊叹的代码推向世界各地的项目。