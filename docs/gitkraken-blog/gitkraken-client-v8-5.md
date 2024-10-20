# GitKraken 客户端 8.5 版:Azure DevOps 工作区支持

> 原文：<https://www.gitkraken.com/blog/gitkraken-client-v8-5>

团队合作对于任何任务都是至关重要的，无论你是在挑战一个银河帝国，还是在 DevOps 管道中推进你的代码。我们在 GitKraken 工作区中增加了对 Azure DevOps 存储库的支持，这是为了帮助更多各种规模的团队高效工作。我们认为你会同意，GitKraken 客户端 v8.5 和 Azure DevOps 工作区的力量是强大的！

[https://www.youtube.com/embed/DJhsfHyL_m8?feature=oembed](https://www.youtube.com/embed/DJhsfHyL_m8?feature=oembed)

视频

保持 Git force 的光明面，帮助您的团队使用 GitKraken Client v8.5 更有效地协作

## GitKraken 工作区支持 Azure DevOps Repos

我们很高兴地宣布 [GitKraken Workspaces](https://www.gitkraken.com/blog/gitkraken-client-v8-2) 现在支持 [Azure DevOps repositories](https://azure.microsoft.com/en-us/services/devops/repos/) ，让更多的开发人员可以获得我们强大的多库项目功能的所有好处。

团队喜欢使用 GitKraken 工作空间，它允许你在一个标签页中收集和访问组成一个项目的所有相关的回购协议。您可以从一个视图中看到所有的拉请求，关于单个存储库的信息，甚至对这些存储库采取行动，所有这些都不需要离开 GitKraken 客户端中的工作流。工作区为您节省了在多个屏幕和文件夹之间切换来管理多存储库项目的大量时间。

每天都有成千上万的开发人员依靠 [Azure DevOps](https://azure.microsoft.com/en-us/blog/introducing-azure-devops/) 来帮助他们协作开发代码，构建和部署他们的应用程序。这个单一的平台集合了 CI/CD 管道、项目管理、包管理、测试，当然还有 Git 存储库托管的工具。现在选择 Azure DevOps 也意味着您可以访问 GitKraken 工作区的强大优势，从而更有效地进行协作。

## GitKraken CLI `gk`命令中增加了交互式基础

有时候，你只需要[改变一个分支](https://www.gitkraken.com/learn/git/git-rebase)的基础。GitKraken 客户端用户喜欢我们的交互式 rebase 工具，但直到现在，它还需要使用鼠标和 GUI。

现在，使用 GitKraken CLI(您可以在 repo 选项卡或 terminal 选项卡中访问终端),您可以通过使用`gk rebase`命令启动交互式重置基础，并为您想要重置基础的提交提供 refs。

## 新的 Git 合并冲突解决选项

Git 中的合并冲突发生在我们大多数人身上。虽然遇到冲突并不有趣，但 GitKraken 客户端的合并冲突编辑器可以帮助您快速解决冲突。现在，使用 GitKraken Client 解决冲突更快、更容易！

当右键单击提交面板中显示的任何冲突时，您会看到两个新选项:

*   “采用当前”——这将应用来自当前检出的分支的更改来解决冲突。
*   “接受传入”——这应用来自传入分支的变更来解决冲突。

在提交面板中右键单击冲突后，只需选择您喜欢的合并冲突策略，GitKraken 客户端将应用指定的更改。这将自动暂存现在准备提交的结果修改文件，所有这些都不需要打开合并冲突编辑器。

## GitKraken 工作空间改进

与团队共享工作空间

### 与组织中所有合适的人共享工作空间变得更加容易。团队与组织中所有合适的人共享工作空间变得更加容易。团队工作区使您能够向组织中需要访问组存储库的任何特定团队授予访问权限。在创建工作区时，您可以决定与一个团队或多个团队共享工作区，或者您可以随时将任何个人工作区转换为团队工作区。

新存储库详细信息图标

单击 GitKraken 工作区中列出的任何 repo 的名称，现在将在 repo 选项卡中打开该 repo，而不只是显示存储库的详细信息。您仍然可以通过单击存储库详细信息图标从工作区选项卡访问相同的存储库信息

在任何存储库列表的右侧。

### 新存储库详细信息图标

单击 GitKraken 工作区中列出的任何 repo 的名称，现在将在 repo 选项卡中打开该 repo，而不只是显示存储库的详细信息。您仍然可以通过单击存储库详细信息图标从工作区选项卡访问相同的存储库信息

使用 GitHub 搜索语法在工作区中搜索 PRs

用户现在可以使用 [GitHub 的搜索语法](https://docs.github.com/en/search-github/searching-on-github/searching-issues-and-pull-requests)在工作区中搜索与回购相关的拉请求。这个语法允许您在搜索中使用限定符来将查询集中到特定的字段，比如用`is:`来指定`issue`或`pr`，或者使用限定符`author:`来快速查找某个团队成员打开的 PRs。

例如，要在 pull 请求的正文中查找术语“GitHub ”,可以使用限定搜索`GitHub in:title`。有很多选项可供选择；更多信息参见 [GitHub 的文档](https://docs.github.com/en/search-github/searching-on-github/searching-issues-and-pull-requests)。

### 使用 GitHub 搜索语法在工作区中搜索 PRs

用户现在可以使用 [GitHub 的搜索语法](https://docs.github.com/en/search-github/searching-on-github/searching-issues-and-pull-requests)在工作区中搜索与回购相关的拉请求。这个语法允许您在搜索中使用限定符来将查询集中到特定的字段，比如用`is:`来指定`issue`或`pr`，或者使用限定符`author:`来快速查找某个团队成员打开的 PRs。

工作区提取请求过滤

现在，随着[拉取请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)过滤的引入，在 GitKraken 工作区中找到您正在寻找的 PRs 比以往任何时候都更容易。您现在可以通过以下方式进行筛选:

“由我打开”，仅显示由您打开的 PRs。

### “有风险”，显示任何未起草且已打开超过 7 天的 PRs。

“按存储库”将视图限制为工作区内的单个存储库。

*   “由我打开”，仅显示由您打开的 PRs。
*   注意:这些新过滤器将只适用于托管在 GitHub、GitHub Enterprise、T2、GitLab 和 GitLab Self Managed 上的存储库。“按存储库”也将与 Azure DevOps 一起工作。我们将在未来增加额外的支持。
*   团队功能的改进

我们改善了在 GitKraken 客户端管理团队的体验。以下是我们帮助组织利用团队功能的最新方法。

注意:这些新过滤器将只适用于托管在 GitHub、GitHub Enterprise、T2、GitLab 和 GitLab Self Managed 上的存储库。“按存储库”也将与 Azure DevOps 一起工作。我们将在未来增加额外的支持。

创建新团队时添加新团队成员

GitKraken 客户端允许你按照你认为合适的方式组织你的团队，并且添加团队成员的过程变得更加简单了！当您在 GitKraken Client 中创建新团队时，您可以在创建过程中添加所有需要的团队成员，帮助您更快地组织起来。

## 按用户名对团队视图排序

一眼就能看出谁在你的团队中。现在，您的团队成员将出现在左侧面板中的`TEAMS`下，按用户名列出。

随着您的团队的成长，teams 视图将帮助您更好地保持组织和协作，让您了解谁在哪个分支上工作，并在任何可能的合并冲突发生之前提醒您。

### 创建新团队时添加新团队成员

GitKraken 客户端允许你按照你认为合适的方式组织你的团队，并且添加团队成员的过程变得更加简单了！当您在 GitKraken Client 中创建新团队时，您可以在创建过程中添加所有需要的团队成员，帮助您更快地组织起来。

准备好实现更好的协作

这些**就是**您正在寻找的 Git 客户端特性。我们很自豪能够为 Azure DevOps 工作区和使用 Git 管理多个团队的组织提供更好的支持。无论你的开发者联盟是大是小，GitKraken Client 都可以帮助你更容易、更安全地使用 Git，一起完成更多的工作！

### 我们知道你会成为 Windows、Mac 和 Linux 版 GitKraken 客户端 8.5 版的粉丝，不管你喜欢什么颜色的光剑。

一眼就能看出谁在你的团队中。现在，您的团队成员将出现在左侧面板中的`TEAMS`下，按用户名列出。

您不需要旅行到很远很远的地方去获得最好的 Git 客户机。你可以从银河系的任何一个角落免费安装 GitKraken 客户端。

## 准备好实现更好的协作

这些**就是**您正在寻找的 Git 客户端特性。我们很自豪能够为 Azure DevOps 工作区和使用 Git 管理多个团队的组织提供更好的支持。无论你的开发者联盟是大是小，GitKraken Client 都可以帮助你更容易、更安全地使用 Git，一起完成更多的工作！

我们知道你会成为 Windows、Mac 和 Linux 版 GitKraken 客户端 8.5 版的粉丝，不管你喜欢什么颜色的光剑。

您不需要旅行到很远很远的地方去获得最好的 Git 客户机。你可以从银河系的任何一个角落免费安装 GitKraken 客户端。