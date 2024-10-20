# GitKraken 与 Git 扩展|查看 Git 客户端的比较

> 原文：<https://www.gitkraken.com/compare/gitkraken-vs-git-extensions>

有时候，选择正确的工具来解决具有挑战性的问题与首先知道如何解决它们一样重要。开发者工具应该满足个人和团队复杂且不断增长的需求，当然这也适用于 Git 工具。

如果您还没有使用 [Git 客户端](https://www.gitkraken.com/git-client)来帮助您可视化和管理您的存储库，您可能需要考虑一些工具的相关好处，如 GitKraken 客户端，它提供了预测合并冲突检测和解决，一次分组和克隆多个回购的能力，[拉请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)管理，等等。

如果您正在评估在 GitKraken 客户端和 Git 扩展之间使用哪个 Git 客户端，那么您来对地方了。本文将引导您了解 GitKraken 与 Git 扩展之间的主要区别，帮助您决定哪种 Git GUI 适合您。

## **比较 Git 扩展和 GitKraken 客户端**

GitKraken Client 是同类工具中唯一提供 Git 增强的终端和 GUI 的工具，让开发人员可以随心所欲地工作。

## **支持 Mac 和 Linux**

### GitKraken 客户端✅ | Git 扩展❌

GitKraken 客户端目前支持在所有流行的操作系统上本地执行该应用程序，包括:Windows、MacOS 和各种 Linux 发行版，如 Ubuntu、Debian 或 RHEL，仅举几例。

另一方面，Git 扩展在 MacOS 或 Linux 系统上没有本地支持，并且平台的维护目前只集中在 Windows OS 上。如果你想在 Mac 或 Linux 上运行 Git 扩展，你将需要使用 Mono，并且你将无法运行该软件的最新版本(通过 Mono，Mac/Linux 仅支持 v2.5 或更旧版本)。

## **用户界面元素**

### GitKraken 客户端👍Git 扩展👎

GitKraken Client 的团队投入巨资为 Git 创建了一个用户界面，它对于高级用户来说足够强大，对于完全的初学者来说足够直观。这个漂亮的设计只是已经健壮且功能丰富的 Git 客户端上的一颗樱桃。

GitKraken Client 中精心设计的提交图让您一眼就能清楚地了解您的 [Git 存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)结构:分支、提交、作者、提交消息、日期/时间、远程存储库、标签、问题、团队成员活动等等都整齐地组织到一个简单的 UI 中。这使得开发人员能够轻松地访问他们需要的特性，并在他们不需要时隐藏它们。

用户可以通过顶部工具栏、拖放操作与他们的存储库进行交互，通过命令面板输入 Git 命令，甚至可以在终端使用提交图作为参考点，使用 [GitKraken CLI](https://www.gitkraken.com/cli) 进行工作。

如果你正在合作一个项目，头像就像一个视觉标记，让你知道谁在什么时候提交了修改。分支标签上的图标显示它们属于哪个遥控器和 PRs。如果您需要与特定提交相关的更多信息，您只需单击图表中的提交即可显示提交细节的详细视图，例如右侧面板中的 [Git 提交消息](https://www.gitkraken.com/learn/git/best-practices/git-commit-message)。

Git 扩展还为用户提供了一个提交图形视图和一个可定制的垂直分割面板，其中包含所选提交的详细信息和视图，如提交消息、差异或错误。

Git 扩展中的 UI 有点混乱不堪，有些用户报告说，由于任何时候都会显示大量的信息，它“[妨碍了他们的工作流程](https://sourceforge.net/projects/gitextensions/reviews/)”。

## **连接到托管的 Git 存储库**

### GitKraken 客户端👍Git 扩展👎

Git Extensions 要求用户使用 PuTTy 之类的工具手动生成一个 [SSH 密钥](https://www.gitkraken.com/learn/git/tutorials/how-git-ssh-works)，它允许您将密钥保存到系统上的一个文本文件中，以便连接到 GitHub 上的托管存储库。这也需要您手动将公钥添加到 GitHub 配置文件中。

GitKraken 客户端让这个过程变得简单快捷，只需点击一个按钮，你就可以连接你的 [GitHub](https://www.gitkraken.com/integrations/github) 、 [GitLab](https://www.gitkraken.com/integrations/gitlab) 、 [Bitbucket](https://www.gitkraken.com/integrations/bitbucket) 和 [Azure DevOps](https://www.gitkraken.com/integrations/azure-devops) 账户。您可以轻松生成一个公私密钥，该密钥将直接从 GitKraken 客户端自动链接到您的帐户。

GitKraken 客户端不仅兼容前面提到的所有最流行的托管版本控制系统，还兼容自托管和企业托管服务，如 GitHub Enterprise、GitLab 自托管、Bitbucket Server 和 Azure DevOps。

虽然 Git 扩展也允许您克隆和使用托管存储库，但是对于初学者来说，设置过程要困难得多，也更加繁琐。

## **管理多个档案**

### GitKraken 客户端✅ | Git 扩展❌

如果您有与非工作 Git repos 相关联的个人项目，您可以通过创建多个[概要文件](https://support.gitkraken.com/start-here/profiles/)轻松地将它们从 GitKraken 客户端中的工作存储库中分离出来。这对于在一个托管服务上拥有多个账户的开发者和团队来说也是有益的。

单独的配置文件存储不同的应用程序首选项和 Git 配置设置，这使得切换上下文和控制 Git 客户端如何与您合作变得更加容易，反之亦然。

另一方面，Git 扩展不提供创建多个概要文件的能力。

## **命令面板**

### GitKraken 客户端✅ | Git 扩展❌

GitKraken 客户端为开发人员提供了一种简单而强大的方法，可以使用命令面板快速执行 Git 命令。

用户可以简单地开始输入以执行命令，如打开 repo、查看文件历史、打开 GitKraken 设置等等。

Git 扩展没有提供命令面板。

## **团队回购管理工作区**

### GitKraken 客户端✅ | Git 扩展❌

如果你在一个团队中工作或者每天与人合作，GitKraken Client 为用户提供了创建[工作空间](https://support.gitkraken.com/working-with-repositories/workspaces/)的能力。该功能使用户能够将 repos 分组在一起，并同时跨多个存储库采取行动。

在 GitKraken Client 中对存储库进行分组可以使团队的入职过程更加顺畅。当一个新的团队成员加入时，他们可以很容易地从同一个位置访问他们需要工作的所有存储库——非常方便！

工作区还提供了一次从所有或多个存储库中克隆或获取变更的能力，进一步提高了您的团队的工作流生产率。

跟踪指定工作区内所有存储库中的提取请求，过滤分配给您的请求，或者自动标记由于不活动而处于“危险”状态的请求。

虽然是为团队设计的，但工作区也可以由单独的开发人员使用，也是简化回购管理和组织的一种方式。

相比之下，Git 扩展不提供分组和共享多个回购的特性。

GitKraken Client 不断设计和实现更多的团队协作功能，如同时分组和共享多个 repos 的工作区。

## **团队视图和协作工具**

### GitKraken 客户端✅ | Git 扩展❌

GitKraken Client 提供了一系列基于团队的协作工具和功能，组织可以使用这些工具和功能来帮助最大限度地提高生产力和改善沟通。

如果你已经在 GitKraken 客户端建立了一个团队，你会在提交图旁边看到一个`TEAMS`视图。在这里，您可以找到关于团队成员正在处理的存储库和文件的有价值的信息。

当两个团队成员同时处理同一个文件时，GitKraken 客户端会在团队视图中显示一个警告图标。这使您能够在潜在的[合并冲突](https://www.gitkraken.com/learn/git/tutorials/how-to-resolve-merge-conflict-in-git)中保持领先，并在问题发生之前避免问题。

## **问题跟踪集成**

### GitKraken 客户端👍Git 扩展👎

GitKraken Client 通过集成最流行的问题跟踪器:吉拉、GitLab 问题和 GitHub 问题，为开发人员、项目经理和团队领导提供了跨平台高效工作的能力。

在设置了与 GitKraken 客户端的问题跟踪集成后，您将能够在左侧面板中看到您的问题的预览。通过单击某个问题，您可以编辑、添加受托人、添加评论等，而无需离开客户端。

此外，新问题可以直接在 GitKraken 客户端中创建，在那里它们将与问题跟踪器实时同步，帮助团队减少文档时间，使开发人员的生活更轻松。

当处理一个特定的问题时，GitKraken 客户端提供了从该问题创建一个新分支的选项，因此您可以直接处理需要您立即关注的任务。

Git 扩展在这方面非常有限，只提供了通过插件显示吉拉提交提示的选项。

## **Git 增强型终端**

### GitKraken 客户端✅ | Git 扩展❌

喜欢使用命令行而不是 GUI 的开发人员可以使用 GitKraken 客户端中的终端来执行 Git 操作。

GitKraken Client 以键盘优先、Git 增强的终端体验增加了传统的 CLI 体验。自动建议和自动完成命令和语法工具提示有助于提高工作流程速度，并且无需记忆复杂的 [Git 命令](https://www.gitkraken.com/learn/git/commands)。

除了您已经熟悉的 Git 命令，使用 GitKraken Client 的开发人员还可以通过***【GK】命令*** 访问一系列功能，让您可以访问终端旁边的不同视图，包括:提交图、比较、责任和历史视图。

相比之下，Git 扩展不提供终端接口，希望使用 CLI 执行 Git 命令的开发人员会退出应用程序。

## **Git 合并冲突解决**

### GitKraken 客户端👍Git 扩展👎

GitKraken 客户端为开发人员提供了解决合并冲突的能力，而无需移动外部工具，直接在应用程序内的专门视图中。当检测到合并冲突时，应用程序会向您发出警告，并指导您完成解决问题的步骤。

当检测到冲突时，您可以在右侧面板中点击受影响的文件，在 GitKraken 客户端中打开*合并冲突*视图。从这里，您可以决定想要保留哪些代码块或文件。在底部甚至有一个输出视图，这样你就可以确切地看到你的改变将如何影响文件。

如果您仍然喜欢在工作流程中使用外部工具，您仍然可以在 GitKraken 客户端中配置不同的默认合并冲突工具，方法是导航到`Preferences` → `General`并在不同的可用选项之间进行选择:kdiff3、araxis、P4 merge…仅举几例。

Git 扩展还允许您配置外部工具来查看文件差异和解决合并冲突。您可以从流行的解决方案中进行选择，如:kdiff3、diffmerge、tortoisediff 等。该工具还为用户提供了在应用程序的合并冲突对话框中选择更改的能力。

## **1-点击撤销&重做**

### GitKraken 客户端👍Git 扩展👎

错误发生；这是生活的现实。幸运的是，如果您使用 GitKraken 客户端，由于 GUI 中的撤销/重做快捷方式很容易访问，Git 中的错误风险要小得多。

至于用户可以撤销/重做的操作，该列表涵盖了所有主要的 Git 操作，例如:签出、提交、放弃、删除分支、移除远程以及将分支重置为提交。

另一方面，Git 扩展只允许用户撤销最后一次提交。

## **Git 扩展 vs GitKraken 客户端**

我们喜欢开源 Git 扩展社区用他们的项目开发的东西。然而，如果你是一名开发人员，在使用 Git 时寻求绝对最佳的用户体验，并且你意识到不断地在不同应用之间移动会如何影响你的速度和性能，那么是时候尝试一下 [GitKraken 客户端](https://www.gitkraken.com/git-client)了。

如果你是一个团队的一员，希望跨不同的存储库进行协作，通过像吉拉这样的问题跟踪器来跟踪 bug 和 epics，或者对他们的队友正在做的事情有详细的了解，你也将从 GitKraken Client 的[面向团队的特性](https://www.gitkraken.com/blog/git-for-teams)中受益匪浅。