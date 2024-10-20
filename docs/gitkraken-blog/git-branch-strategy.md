# 最好的 Git 分支策略是什么？| Git 最佳实践

> 原文：<https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy>

Git 和其他版本控制系统让软件开发人员能够跟踪、管理和组织他们的代码。

特别是，Git 帮助开发人员与队友合作编写代码；将提交和分支等强大的功能与特定的原则和策略相结合，有助于团队组织代码并减少管理版本所需的时间。

当然，每个开发人员和开发团队都是不同的，都有独特的需求。这就是 Git 分支策略的用武之地。

我们将讨论三种相当流行的 Git 分支策略，每种策略都有各自的优点。最精彩的部分？这些工作流都不是一成不变的，可以而且应该进行修改以适应您的特定环境和需求。

*请注意:这些原始策略中有许多都是指“主”分支，但我们选择使用“主”来代替。*

* * *

无论您选择哪种分支策略，GitKraken 都可以通过预测合并冲突检测和应用内拉请求等功能，实现与 Git 的强大、更简单、更安全的协作。

Git 流分支策略

## Git 流分支策略背后的主要思想是将你的工作隔离到不同类型的分支中。总共有五种不同的分支类型:

主要的

*   发展
*   特征
*   释放；排放；发布
*   修补程序
*   Git 流程中的两个主要分支是*主*和*开发*。有三种类型的支持分支，具有不同的预期目的:*特性*、*发布*和*热修复*。

Git 流:利弊

Git 流分支策略有很多好处，但是也带来了一些挑战。

## **Git 流量的好处:**

各种类型的分支使组织工作变得简单而直观。

系统开发过程允许有效的测试。

发布分支的使用允许您轻松地、持续地支持产品代码的多个版本。

1.  各种类型的分支使组织工作变得简单而直观。
2.  **Git 流的挑战:**
3.  根据产品的复杂程度，Git 流程模型可能会过于复杂，并减缓开发过程和发布周期。

由于开发周期长，Git flow 在历史上不支持持续交付或持续集成。

Git 流与 GitKraken

1.  传奇的跨平台[GitKraken Git GUI](https://www.gitkraken.com/git-client)for Windows，Mac，& Linux 有助于在高层次上简化和可视化 Git，并支持 Git 流分支策略。
2.  由于开发周期长，Git flow 在历史上不支持持续交付或持续集成。

Git 流与 GitKraken

## 要用 GitKraken 初始化 Git 流，打开 repo，然后导航到`Preferences` → `Gitflow`来设置您的首选分支命名约定。GitKraken 将帮助您启动和完成特性、发布和修补程序分支。

GitKraken 提供了令人难以置信的 [GitHub 集成](https://www.gitkraken.com/integrations/github)、 [GitLab 集成](https://www.gitkraken.com/integrations/gitlab)、 [Bitbucket 集成](https://www.gitkraken.com/integrations/bitbucket)和 [Azure DevOps 集成](https://www.gitkraken.com/integrations/azure-devops)，使其更容易与托管库一起工作。

GitKraken 使大大小小的团队都能够利用 Git 的真正力量，让您更清楚地了解谁在什么时候做什么，这样您就可以避免冲突并保护您的代码。

GitHub 流量分支策略

GitHub 流分支策略是一个相对简单的工作流，允许较小的团队，或者不需要支持多个版本的 web 应用程序/产品，来加速他们的工作。

在 [GitHub 流](https://githubflow.github.io/)中，主分支包含您的生产就绪代码。

其他分支，特性分支，应该包含新特性和 bug 修复的工作，并且在工作完成并被适当评审后，将被合并回主分支。

GitHub 流程考虑

[Learn more about Git team features](/git-client/team-features)

在使用 GitHub 流分支策略时，有六个原则[你应该遵守，以确保你维护好代码。](https://githubflow.github.io/)

## 主分支中的任何代码都应该是可部署的。

为新工作从主分支创建新的描述性命名的分支，例如`feature/add-new-payment-types`。

将新工作提交给本地分支机构，定期将工作推送到远程机构。

要请求反馈或帮助，或者当您认为您的工作已经准备好合并到主分支中时，请打开一个[拉动请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)。

在您的工作或功能被审阅和批准后，它可以被合并到主分支中。

## 一旦您的工作被合并到主分支中，就应该立即进行部署。

GitHub 流:利弊

1.  与其他 Git 分支策略一样，GitHub flow 也有一些亮点和不足。
2.  *受 [GitHub 流程指南启发的自定义图像。](https://guides.github.com/introduction/flow/)*
3.  要请求反馈或帮助，或者当您认为您的工作已经准备好合并到主分支中时，请打开一个[拉动请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)。
4.  **GitHub 流的好处**
5.  在本帖中我们讨论的三个 Git 分支策略中，GitHub 流是最简单的。
6.  由于工作流的简单性，这种 Git 分支策略允许连续交付和连续集成。

这种 Git 分支策略非常适合小型团队和 web 应用程序。

## 与其他 Git 分支策略一样，GitHub flow 也有一些亮点和不足。

**GitHub 流的挑战**

这种 Git 分支策略无法同时支持生产中的多个版本的代码。

缺乏专门的开发分支使得 GitHub flow 在生产中更容易出现 bug。

GitLab 流量分支策略

1.  在其核心，GitLab 流分支策略是一个明确定义的工作流。虽然类似于 GitHub 流分支策略，但主要区别在于根据情况添加了环境分支——即生产和预生产——或发布分支。
2.  就像其他两个 Git 分支策略一样， [GitLab flow](https://docs.gitlab.com/ee/topics/gitlab_flow.html) 有一个主分支，其中包含可以部署的代码。然而，这些代码并不是发布版本的真实来源。
3.  在 GitLab 流程中，feature 分支包含新功能和 bug 修复的工作，当它们完成、审查和批准后，将被合并回主分支。

**GitHub 流的挑战**

在发布周期中使用 GitLab 流程

1.  GitLab 流分支策略适用于两种不同类型的发布周期:
2.  版本化发布:每个发布都有一个基于主分支的相关发布分支。Bug 修复应该首先合并到主分支中，然后再精选到发布分支中。

*自定义图像灵感来自 [GitLab 流程](https://about.gitlab.com/blog/2014/09/29/gitlab-flow/)中的图像。*

## 在其核心，GitLab 流分支策略是一个明确定义的工作流。虽然类似于 GitHub 流分支策略，但主要区别在于根据情况添加了环境分支——即生产和预生产——或发布分支。

2.连续发布:生产分支被用来包含部署就绪的代码，所以当代码准备好发布时，它被合并到生产分支中。

在 GitLab 流程中，feature 分支包含新功能和 bug 修复的工作，当它们完成、审查和批准后，将被合并回主分支。

**git lab 流程的优势**

与 Git 流分支策略相比，GitLab 流更简单。

## GitLab 流比 GitHub 流分支策略更有组织性和结构化。

稍加修改后，GitLab flow 可以允许连续交付和版本化发布。

1.  *自定义图像灵感来自 [GitLab 流程](https://about.gitlab.com/blog/2014/09/29/gitlab-flow/)中的图像。*

**git lab 流程的挑战**

GitLab 流不是最简单的 Git 分支策略。

GitLab 流不是最结构化的 Git 分支策略，这可能导致混乱的协作。

**git lab 流程的优势**

可视化提交和分支的能力将使您和您的团队对您的存储库的结构有一个新的理解，使日常的 Git 操作更加直观。

1.  GitLab 流比 GitHub 流分支策略更有组织性和结构化。
2.  那么，哪个是最好的 Git 分支策略呢？
3.  最终，哪个 Git 分支策略是最好的取决于您和您的团队的环境、产品和您的特定开发需求。

没有一种通用的 Git 分支策略，无论您最终选择哪一种，您都可以通过进一步的修改来优化它。

使用 GitKraken 开始使用 Git flow。免费下载 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 。

1.  GitLab 流不是最结构化的 Git 分支策略，这可能导致混乱的协作。
2.  GitLab flow is not the most structured Git branching strategy which can lead to messy collaboration.

可视化提交和分支的能力将使您和您的团队对您的存储库的结构有一个新的理解，使日常的 Git 操作更加直观。

The ability to visualize your commits and branches will give you and your team a newfound understanding of your repository’s structure, making daily Git actions more intuitive.

[Make Git Easier with GitKraken](/git-client/easy-git-features)

那么，哪个是最好的 Git 分支策略呢？

## 最终，哪个 Git 分支策略是最好的取决于您和您的团队的环境、产品和您的特定开发需求。

没有一种通用的 Git 分支策略，无论您最终选择哪一种，您都可以通过进一步的修改来优化它。

使用 GitKraken 开始使用 Git flow。免费下载 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 。

Get started with Git flow using GitKraken. Download the [GitKraken Git GUI](https://www.gitkraken.com/git-client) for free.