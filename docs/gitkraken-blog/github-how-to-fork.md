# GitHub 如何派生|了解如何派生 GitHub 存储库

> 原文：<https://www.gitkraken.com/learn/git/problems/github-how-to-fork>

在 Git 中，派生存储库意味着在一个远程托管服务(比如 GitHub)上，在您的个人帐户下创建一个存储库的副本，无论是公开存储还是秘密存储。

例如，如果您要分叉 [VS 代码 GitHub 库](https://github.com/Axosoft/vscode-gitlens)，新的副本将存储在`https://github.com/<Your-github-account-name>/vscode-gitlens`。

派生存储库意味着您可以对代码进行任何想要的更改，而不会影响原始项目。您可以选择通过[拉请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)流程与原始回购共享这些更改。

新的分叉副本仍然连接到原始存储库，并被视为原始“上游”存储库的“下游”。如果需要，对原始上游存储库的任何更改都可以很容易地应用到下游存储库。

这个连接允许下游副本通过使用 [GitHub pull 请求](https://www.gitkraken.com/learn/git/problems/github-pull-requests)来建议对上游存储库进行更改。例如，如果您想要修复在一个存储库中发现的一个 bug，您应该分叉该存储库，修复您的副本上的问题，然后启动一个 pull 请求，请求上游存储库所有者将您提议的更改合并到原始存储库中。

有时候，一个存储库被分叉成充当你自己想法的起点，这在开源软件的世界里很常见。

GitKraken 与 GitHub 的强大集成使得克隆、派生、添加远程和管理拉请求变得快速而简单。此外，开发人员可以充分利用 GUI 和 CLI。

## **如何用 GitKraken CLI 插入 GitHub**

使用 [Git CLI](https://www.gitkraken.com/cli) 在 GitHub 中派生一个存储库:

首先，导航到您想要在 GitHub.com 上分叉的 [Git 库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)。

在那里，点击存储库的 GitHub 页面右上角的`Fork`按钮。

恭喜你。现在，您的 GitHub 帐户中有了一个存储库的副本。

## **在 GitHub 中使用分叉库**

既然 fork 已经存在，您将希望确保您的存储库本地副本可以访问原始的和分叉的 repo。

首先，通过键入以下命令，使用 Git CLI 和 SSH 在您的本地机器上执行原始存储库的 [Git 克隆](https://www.gitkraken.com/learn/git/git-clone):

`git clone git@github.com:Axosoft/vscode-gitlens.git`

这一步将把原始存储库设置为`origin` Git remote。

现在，通过键入以下命令，将分叉的 GitHub 存储库作为新的 [Git remote](https://www.gitkraken.com/learn/git/git-remote) 添加到本地:

`git remote add GitHub-user-name git@github.com:GitHub-user-name/vscode-gitlens.git`

记得在这两个地方都用你实际的 GitHub 用户帐户名替换`GitHub-user-name`。

现在，您已经设置了一个本地存储库，其中有两个连接的远程存储库，即原始的 repo 和 GitHub 上的 fork。

## **从一个分叉的 GitHub 仓库中拉出**

如果您想从分叉的 GitHub 存储库中提取更改，请键入:

`git pull GitHub-user-name <branch-name>`

如果要从原始存储库中获取并合并更改，请键入:

`git pull origin <branch-name>`

## **推送到分叉的 GitHub 库**

您现在可以通过键入:
将更改推送到分叉的 GitHub 存储库

`git push GitHub-user-name <branch-name>`

如果允许访问，您可以通过键入以下内容将更改直接推送到原始存储库:

`git push origin <branch-name>`

使用 GitKraken，添加和管理分叉 GitHub 库的过程简单而直观，因此您可以快速设置并为您的项目做出贡献。我们知道你的时间很宝贵。

## **如何将 GitHub 与 GitKraken Git 客户端相结合**

如果您想使用 GitKraken Git 客户端派生 GitHub，您将首先克隆您希望派生的 repo。

要开始克隆 GitHub repo，请导航至`Repository Management` → `Clone`。您可以使用 GitHub URL 进行克隆，并且可以选择要克隆到本地机器的哪个位置。

成功克隆 GitHub 存储库后，您可以在 GitKraken 的中央 UI 中的提交图左侧的远程面板中看到 repo。

接下来，右键单击遥控器并从上下文菜单中选择`Fork and Add Remote`。

这就够了！为了验证您已经成功地分叉了 GitHub 存储库，您可以在左侧面板中的原始 remote 下方看到一个新的本地分叉。

## **GitHub 克隆 vs 分叉**

虽然派生和克隆 GitHub 存储库都是原始存储库的副本，但重要的是要注意这些概念的主要区别。克隆存储库会创建存储库的本地副本，并自动在 GitHub 上设置一个到该存储库的 Git remote。如果这个项目属于其他人，您将不能将任何变更推回到原来的存储库中。另一方面，派生一个 GitHub 存储库会创建一个您自己的存储库的在线副本，可以作为远程使用。您可以根据需要推和拉这个存储库，除了您的副本之外，您还可以从您派生的原始回购中获取更改。

## **GitHub 分叉 vs 分支**

用最简单的话来说，fork 是一种一次性复制整个存储库的方法，而 branch 是一种在存储库中工作的方法。

一个 [Git 分支](https://www.gitkraken.com/learn/git/branch)是一种在一个项目的同一个副本中安全地进行一组新的提交的方法，在您准备好之前不需要对主分支进行修改。实际上，分支只是一个指向提交的新指针，它会随着您所做的每个新提交而移动，并且只有在一个共同的祖先提交上进行提交时才会在图中出现分叉。

一个分支可以包含很多分支，你可以像更新主分支一样从上游更新每个分支。常见的做法是将新的工作放在一个特性分支中，并在尝试更新原始上游存储库时从该分支创建一个拉请求。

## **如何解除对 GitHub 库的封锁**

GitHub 中的 Unforking 不是一个常见的操作，需要 GitHub 支持团队的直接协助。您需要访问 [GitHub 支持请求页面](https://support.github.com/request)并选择“附加、分离或重新路由分叉”选项。虚拟助手将指导您完成余下的过程。

如果你准备好让 GitHub 库的分叉过程变得更容易、更快、更具可见性和可控性，请下载 GitKraken Git 客户端 T1，它现在有 GUI 和 T2 CLI T3，今天就可以免费下载。