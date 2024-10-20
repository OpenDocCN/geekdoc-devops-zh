# GitKraken 转到 GUI v7.7:转到团队

> 原文：<https://www.gitkraken.com/blog/gitkraken-v7-7>

准备好在全新的水平上与您的团队协作。

虽然 Git 在跟踪随时间的变化并让我们深入了解过去方面令人惊叹，但它在帮助您了解其他人当前正在做什么变化或谁被分配在任何给定项目的代码部分工作方面并不出色。无需切换应用程序，就能更好地了解团队中其他人正在积极工作的内容，这不是很好吗？我们完全同意！

宣布 [GitKraken v7.7](https://support.gitkraken.com/release-notes/current/#version-770) 。

[https://www.youtube.com/embed/VoNrFyzeKkU?feature=oembed](https://www.youtube.com/embed/VoNrFyzeKkU?feature=oembed)

视频

GitKraken 的新团队特性将使您团队的 Git 工作流程变得更加安全和透明。剧透警告:你将知道谁在哪个文件中工作，并在潜在的合并冲突发生之前得到警告！

## **为团队带来前所未有的收获**

使用 Git 的一个最好的理由是，您可以在本地拥有您需要的所有代码，允许独立工作。这一基本原则允许任何规模的分布式团队在几天甚至几年的时间内就各种项目进行协作。使用 Git 的最大缺点之一是当你和你的队友在同一个文件中编辑了相同的行时，会遇到合并冲突，这种情况在你尝试 Git pull 之前可能不会发现。当这些冲突发生时，开发人员不得不从构建功能转向戴上他们的侦探帽，开始整理历史，以发现谁做了哪些改变，哪个版本的现实是最正确的。吉拉和其他项目跟踪软件可以有所帮助，如果团队预先知道哪些特定文件将被哪些票据所触动。但是实际上，这并不是软件开发的工作方式。我们需要的是一种方法，在我们到达 [Git 合并冲突](https://www.gitkraken.com/learn/git/tutorials/how-to-resolve-merge-conflict-in-git)点之前，让众所周知的右手看到左手在做什么。

这也是 GitKraken 引入团队的原因。这是我们寻求将 Git 与团队协作工具结合使用的第一个重要步骤。我们设想这样一个世界，在这个世界中，全面的团队可见性赋予新型结对编程和指导以力量，同时极大地改进项目管理工作流程。我们很自豪地推出我们的最新产品，并对我们知道它能为您和您的同事做什么感到兴奋。让我们仔细看看新功能。

### **组织团队成员以获得更好的可视性**

团队合作始于知道你在和谁合作。知道谁应该在一个里程碑上工作有助于整个组织更快和更透明地前进。

拥有 [Pro](https://www.gitkraken.com/pricing) 和[企业云](https://www.gitkraken.com/gitkraken-enterprise)账户的 GitKraken 用户现在可以在他们的组织内创建和管理团队。从用户的简档设置中，用户现在可以看到所有关联组织的选项卡。从这个角度来看:

*   所有团队成员都可以查看组织中其他成员的列表，以及他们所属的任何团队中的成员。
*   管理员可以创建和管理团队。
*   管理员还可以邀请、删除或更改组织中成员的角色。

### **Git 团队协作功能**

知道谁在团队中很棒，但是能够看到其他人在做什么才是真正的游戏改变者。现在，第一次，您可以对您的队友正在处理的分支，甚至是特定的文件有一个感觉。这将有助于在合并冲突出现之前就避免它们。

GitKraken 团队视图允许用户:

*   在左侧面板中查看团队成员列表。
*   查看团队成员当前正在处理的文件和分支。
*   避免合并冲突。如果您和另一个团队成员都修改了同一个文件，我们会用警告图标⚠️.提醒您

你在 GitKraken 与越多的团队成员合作，你获得的生产力收益就越多。当您知道谁在做什么，并且可以避免合并冲突时，您团队的 Git 工作流会变得更加安全！

### **按作者或团队过滤图表**

当您执行代码审查或监控项目进度时，快速查看谁提交了哪些代码是非常有利的。使用新的图表过滤，只要您启用了 Author 列，用户现在就可以轻松地按团队和/或协作者过滤提交图表，以突出显示单个团队成员提交的工作。

## **新 Git 拉取请求过滤**

与团队一起工作通常意味着分支，并最终要求项目的维护者引入您的变更。随着整个团队的贡献者发出拉请求，监视所有潜在的变化会很快变得势不可挡。有了新的 GitKraken pull 请求过滤，您可以快速找到问题的核心。

拉请求过滤装置:

默认情况下包括以下过滤器，以帮助用户关注最重要的 PRs:

*   我的拉取请求
*   所有拉取请求

有一些特定于某些主机提供商的选项，比如 GitHub pull 请求:

分配给我

*   等待我的审查
*   除了默认过滤器之外，用户还可以设置自定义过滤器。现在可以轻松搜索所有过滤后的拉取请求。

对于那些使用持续集成和部署管道的用户，新的状态图标现在将指示 CI/CD 构建状态，以及拉请求的请求状态，包括“所有检查通过”、“构建失败”和“检查通过但审查待定”。

In addition to the default filters, users can set up custom filters. And all filtered pull requests can now be easily searched.

**直接从拉式请求视图复制**

当审查和测试来自 PR 的代码时，可能需要在本地复制一个代码片段来进行测试。在这个版本中，我们允许您直接从 GitKraken 中的 pull request 视图进行复制，从而使这个过程变得更加简单。

## **Copy Directly from the Pull Request View**

**新的和改进的 GitHub 动作模板**

GitHub 动作是将 CI/CD 和其他自动化功能构建到您的工作流程中的一种方式。越来越多的团队利用这些工作流来改进和定制他们的代码审查、分支管理和问题分类工作。随着 GitKraken 的新发布，我们正在改进和扩展内置模板，以帮助您更好地利用 GitHub 操作。

## **New and Improved GitHub Actions Templates**

GitHub Actions are a way to build CI/CD and other automations into your workflow. A growing number of teams take advantage of these workflows to improve and customize their code reviews, branch management, and issue triaging work. With the new release of GitKraken, we are improving and expanding the built-in templates to help you better leverage GitHub Actions.

**自动聚焦表单提交**

对于许多开发人员来说，让手指离开键盘最多是一种不便。我们听到了你的声音并调整了我们的界面。现在，任何表单提交都将成为焦点，使得使用 tab 和 enter 键导航和提交更加方便。

## **Automatically Focused Form Submissions**

团队合作让梦想成真🎉

我们很高兴推出 GitKraken 的这一版本，并将其视为我们所有人迈向更富有成效的未来的一步。正如一位伟大的思想家曾经说过的那样"*个人对集体努力的承诺——这是团队工作、公司工作、社会工作、文明工作的基础。*

## 使用适用于 Windows、Mac、& Linux 的[GitKraken Git GUI](https://www.gitkraken.com/git-client)7.7 版，让您的整个团队兴奋不已，并提高工作效率。

通过避免合并冲突来节省解决合并冲突的时间！GitKraken 的新团队功能会在两个团队成员处理同一个文件时通知您。

Get your whole team excited and more productive with v7.7 of the [GitKraken Git GUI](https://www.gitkraken.com/git-client) for Windows, Mac, & Linux.

Save time resolving merge conflicts by avoiding them all together! GitKraken’s new team features notifies you when two team members are working on the same file.