# 什么是 Git 流|如何使用 Git 流|学习 Git

> 原文：<https://www.gitkraken.com/learn/git/git-flow>

更新日期:2022 年 6 月 17 日

[Git flow](https://nvie.com/posts/a-successful-git-branching-model/) 是一种流行的 [Git 分支策略](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy)，旨在简化发布管理，由软件开发人员 Vincent Driessen 于 2010 年推出。从根本上说，Git 流包括将你的工作隔离到不同类型的 [Git 分支](https://www.gitkraken.com/learn/git/branch)。在本文中，我们将介绍 Git flow 工作流中的不同分支，[如何在 GitKraken 客户端中使用 Git flow](https://www.gitkraken.com/learn/git/git-flow#how-to-use-git-flow)，并简要讨论另外两种 Git 分支策略，GitHub flow 和 GitLab flow。

Git 流工作流

## 在 Git 流工作流中，有五种不同的分支类型:

[主](https://www.gitkraken.com/learn/git/git-flow#main-branch)

1.  [开发](https://www.gitkraken.com/learn/git/git-flow#develop-branch)
2.  [功能](https://www.gitkraken.com/learn/git/git-flow#feature-branch)
3.  [发布](https://www.gitkraken.com/learn/git/git-flow#release-branch)
4.  [热修复](https://www.gitkraken.com/learn/git/git-flow#hotfix-branch)
5.  [热修复](https://www.gitkraken.com/learn/git/git-flow#hotfix-branch)

其他 Git 分支策略

## [GitHub 流量](https://www.gitkraken.com/learn/git/git-flow#github-flow)

[GitLab 流程](https://www.gitkraken.com/learn/git/git-flow#gitlab-flow)

Git 流程图

*定制图片的灵感来自[“一个成功的 Git 分支模型”](https://nvie.com/posts/a-successful-git-branching-model/)中的 Vincent Driessen。*

## Git 流程图

GitKraken 客户端使配置和定制您的 Git 流设置变得快速而简单。

Git 流:主分支

*请注意:主枝俗称“主”；我们特意决定避免使用这个过时的术语，而是选择使用“main”。*

Git 流工作流中主要分支的目的是包含可以发布的生产就绪代码。

在 Git 流中，主分支在项目开始时创建，并在整个开发过程中维护。可以在各种提交时标记分支，以表示代码的不同版本或发布，其他分支将在经过充分审查和测试后合并到主分支中。

## Git 流:开发分支

开发分支是在项目开始时创建的，并在整个开发过程中进行维护，它包含预生产代码，以及正在测试的新开发的功能。

新创建的特性应该基于开发分支，然后在准备好进行测试的时候合并回来。

Git 流:支持分支

当使用 Git flow 进行开发时，有三种类型的支持分支具有不同的预期目的:特性、发布和修补程序。

## Git 流:特性分支

特性分支是 Git 流程工作流中最常见的分支类型。向代码中添加新功能时会用到它。

当处理一个新的特性时，您将从开发分支开始一个特性分支，然后当特性完成并被适当地评审时，将您的更改合并回开发分支。

Git 流:发布分支

## 在准备新的产品发布时，应该使用发布分支。通常，在发布分支上执行的工作涉及到收尾工作和特定于发布新代码的小错误，代码应该与主开发分支分开处理。

Git 流:修补程序分支

在 Git 流中，hotfix 分支用于快速处理主分支中的必要变更。

### 修补程序分支的基础应该是您的主分支，并且应该合并回主分支和开发分支。将来自 hotfix 分支的更改合并回 develop 分支是至关重要的，这样可以确保下次发布主分支时修复仍然有效。

特性分支是 Git 流程工作流中最常见的分支类型。向代码中添加新功能时会用到它。

GitKraken 客户端如何使用 Git 流

现在我们已经介绍了什么是 Git flow，让我们来看看如何在 GitKraken 客户端中使用 Git flow。

### 要开始在 GitKraken 客户端中使用 Git flow 工作流，请执行以下步骤:

导航至顶部工具栏中的`Preferences`

在左侧面板中选择`Gitflow`并设置您偏好的分支命名约定

### 选择，然后点击绿色的`Initialize Gitflow`按钮

在 Git 流中，hotfix 分支用于快速处理主分支中的必要变更。

## 现在，当您退出首选项时，您会在左侧面板的顶部看到一个 Gitflow 部分。从这里，

GitKraken 客户端将帮助您启动和完成功能、发布和修复分支。

要开始在 GitKraken 客户端中使用 Git flow 工作流，请执行以下步骤:

1.  导航至顶部工具栏中的`Preferences`
2.  例如，大多数开发人员可能会点击“Feature ”,然后命名并创建他们的特性分支:
3.  选择，然后点击绿色的`Initialize Gitflow`按钮

然后，当您的功能分支被批准后，您可以使用 Gitflow 面板关闭功能分支。这将自动将特性分支合并到开发中，或者，您可以选择在开发中重新设置特性分支的基础。

现在，当您退出首选项时，您会在左侧面板的顶部看到一个 Gitflow 部分。从这里，

免费尝试使用 Git flow 和 GitKraken 客户端。

Git 流常见问题

问:什么是 Git 流？

答:Git flow 是一个 Git 分支策略，创建它是为了使发布新版本的软件更容易。

问:如何安装 Git flow？

答:Git flow 是一个 Git 分支策略，因此不是一个可以下载的工具或软件。开发人员可以*使用* Git 流来组织他们的 Git 分支和开发过程，以便更有效地发布版本。

其他 Git 工作流

虽然 Git flow 自 2010 年问世以来人气飙升，但就连 Driessen 自己也承认，它可能不是每个开发团队或环境的最佳 Git 工作流，正如他在 2020 年 3 月的[“反思笔记”](https://nvie.com/posts/a-successful-git-branching-model/)中所示。

*“Web 应用程序通常是连续交付的，而不是回滚的，并且您不必支持软件的多个版本。这不是我 10 年前想到的软件案例。”*

有两个比 Git 流更简单的 Git 工作流，可以允许连续交付。

***GitTip:了解更多关于 [Git 分支策略](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy)，包括 GitHub flow 和 GitLab flow 的深入细节和工作流程图。***

## **GitHub 流量**

### 比 Git flow 更简单的是， [GitHub flow](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy#github-flow-branch-strategy) 非常适合小型团队和不需要支持多个版本的 web 应用程序或产品。由于其简单性，GitHub flow 工作流允许持续交付和持续集成。

当然，也有相关的风险需要考虑。例如，缺乏专门的开发分支使得这个工作流程更容易在生产中出现错误。

GitLab Flow

### 与 GitHub flow 工作流相比， [GitLab flow](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy#gitlab-flow-branch-strategy) 也比 Git flow 更简单，更有组织性和结构化。

GitLab flow 根据您的情况引入了环境分支，比如生产和预生产，或者发布分支。

稍加修改，GitLab flow 可以支持版本化发布和持续交付。

## 其他 Git 工作流

GitKraken 客户端 Git 流入门

在其核心，Git 流有助于更好地组织您的工作。将它与 Git 客户机的视觉能力结合起来，将您的工作流程提升到一个新的水平。

GitKraken 客户端支持 Git 流，并允许您在配置过程中根据自己的喜好定制分支名称和其他细节。这对于跨开发团队的标准化过程特别有帮助，以便更有效地协作。了解关于在 GitKraken 客户端中使用 [Git flow 的更多信息。](https://support.gitkraken.com/git-workflows-and-extensions/git-flow/)

***GitTip:了解更多关于 [Git 分支策略](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy)，包括 GitHub flow 和 GitLab flow 的深入细节和工作流程图。***

**GitHub 流量**

## 比 Git flow 更简单的是， [GitHub flow](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy#github-flow-branch-strategy) 非常适合小型团队和不需要支持多个版本的 web 应用程序或产品。由于其简单性，GitHub flow 工作流允许持续交付和持续集成。

当然，也有相关的风险需要考虑。例如，缺乏专门的开发分支使得这个工作流程更容易在生产中出现错误。

GitLab Flow

与 GitHub flow 工作流相比， [GitLab flow](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy#gitlab-flow-branch-strategy) 也比 Git flow 更简单，更有组织性和结构化。

## GitLab flow 根据您的情况引入了环境分支，比如生产和预生产，或者发布分支。

稍加修改，GitLab flow 可以支持版本化发布和持续交付。

GitLab flow introduces environment branches, such as production and pre-production, or release branches, depending on your situation.

GitKraken 客户端 Git 流入门

在其核心，Git 流有助于更好地组织您的工作。将它与 Git 客户机的视觉能力结合起来，将您的工作流程提升到一个新的水平。

## GitKraken 客户端支持 Git 流，并允许您在配置过程中根据自己的喜好定制分支名称和其他细节。这对于跨开发团队的标准化过程特别有帮助，以便更有效地协作。了解关于在 GitKraken 客户端中使用 [Git flow 的更多信息。](https://support.gitkraken.com/git-workflows-and-extensions/git-flow/)

At its core, Git flow helps better organize your work. Combine that with the visual power of a Git client to take your workflow to the next level.

GitKraken Client supports Git flow and allows you to customize branch names and other details to your liking during the configuration process. This can be particularly helpful for standardizing processes across development teams for more effective collaboration. Discover more about using [Git flow with GitKraken Client.](https://support.gitkraken.com/git-workflows-and-extensions/git-flow/)