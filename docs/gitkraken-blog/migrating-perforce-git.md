# 从 Perforce 迁移到 Git

> 原文：<https://www.gitkraken.com/blog/migrating-perforce-git>

源代码控制，也称为版本控制，是跟踪和管理软件代码变更的方法。源代码控制管理(SCM)系统提供了代码变更的运行历史，对于对相同文件进行变更的开发团队特别有帮助。

作为负责任的软件开发的一个重要方面，源代码控制帮助开发人员跟踪代码变更，查看完整的修订历史，并在需要时恢复到项目的先前版本。此外，版本控制使协作变得更加容易。

但不是所有的 SCMs 都是一样的；它们有自己的一套相关的好处和限制。当你得知 GitKraken 的团队中充满了痴迷于 Git 的开发人员时，你应该不会感到惊讶，因此我们提倡使用 [Git](https://git-scm.com/) ，一个开源的分布式版本控制系统(VCS)。

我们之前已经写过从 SVN 迁移到 Git 的文章，现在我们要面对的是 T2 的 Perforce，一个受游戏开发者和大公司欢迎的集中的 VCS。

Git 与 Perforce 比较

## Perforce 于 1995 年由 Perforce 软件公司前首席执行官 Christopher Seiwald 发布。整整十年后的 2005 年，Linux 之父 Linus Torvalds 创建了 Git。虽然这两个系统都是为了完成跟踪代码变更的庞大任务而引入的，但是它们在结构和功能上的显著差异是显而易见的。

Perforce 和 Git 处理软件项目的方式有根本的区别。Perforce 存储库可以保存成百上千个单独的软件项目，每个项目都有不同的分支模型，而 Git 存储库保存单个软件项目。

Perforce 和 Git 处理软件项目的方式有根本的区别。Perforce 存储库可以保存成百上千个单独的软件项目，每个项目都有不同的分支模型，而 Git 存储库保存单个软件项目。

迁移到 Git 的理由

## 有很多原因可以解释为什么一个开发人员或团队会试图将他们的版本控制系统从 Perforce 转换到 Git。

首先，Git 在跟踪代码变更方面做得更好，这是 SCM 的主要目的。Git 的方法更有用，因为它跟踪文件中的内容，而不仅仅是文件本身，更准确地反映了代码中的实际变化。

速度

Perforce 需要连接到服务器才能查看代码更改的历史记录。这不仅会减慢速度，而且随着每个新开发人员尝试访问服务器，情况会变得更糟，导致严重的瓶颈和部署软件项目的延迟。

### 相比之下，使用 Git，您可以在几秒钟内访问整个项目历史。在 Git 环境中，每个开发人员在他们的本地机器上都有一份代码副本。这消除了在工作流程中连接到集中式代码库的需求。

工作流灵活性

Git 更灵活的权限系统允许您的团队有更大的工作流可能性。例如，在 Perforce 中，您可能已经限制了分支的创建，这就要求开发人员在每次想要创建任务分支时都要请求许可。

在 Git 中，您可以只限制对主分支的推送访问，因此开发人员只需要在准备将代码实现到项目中时获得许可，而不是每次坐在计算机前时。

### 分支

在 Perforce 中，开发人员需要创建分支映射，这是 Git 中不需要的任务。这不仅需要开发人员花费时间和精力来做其他事情，而且还会扰乱原本高效的工作流，并产生不必要的数据。

Perforce 中的分支创建了数量惊人的元数据；这可能会导致大型部署的性能问题。相比之下，使用 Git，您可以有 100 个工作分支，但是只有一个实际存在于您的项目中的分支占用空间。而且，如果您想同时处理两个版本来比较结果，您可以克隆分支，执行一些代码更改，然后删除克隆而不会丢失任何东西。

*了解更多关于 Git 分支和[Git kraken](https://support.gitkraken.com/start-here/guide/#branching)中分支是如何工作的。*

最终，Perforce 分支对于大多数开发人员喜欢的工作流类型来说并不方便，并且最终不会产生经理和产品所有者期望的结果。

### 合并和解决冲突

Perforce 的合并算法过于复杂，而 Git 中合并的结果实际上是一个新的提交，包含所有的祖先数据。

许多 Perforce 用户抱怨该工具内置的自动解析功能的准确性，声称“[已经有过工作被破坏的报道](https://www.clearvision-cm.com/blog/git-vs-perforce-helix-whats-the-difference/)”Git 在合并冲突方面做得非常好。当提示您 Git 中存在冲突时，冲突实际上是存在的，而在其他时间，Git 会解决冲突，纠正并节省大量时间。

*看看为什么用户喜欢使用 GitKraken 解决 Git 中的合并冲突。*

Git 为各种新的工作流开辟了可能性，如 [Gitflow](/blog/gitflow) 、任务分支、分叉库等等。冒险在等着我们！

### 合作

从根本上说，作为源代码控制管理工具，Git 和 Perforce 都是基于协作的。它们的功能是允许一组开发人员处理一组共享的文件。协作是 Git 真正将 Perforce 从数字水域中剔除的一个点。

在 Perforce 中，您可以在团队之间共享文件，但是接收团队必须使用手动过程将他们想要实现的相关代码更改分离到他们的项目中。在 Perforce 中，分解文件提交是不可能的。

另一方面，在 Git 中，一个团队可以[精选](https://support.gitkraken.com/working-with-commits/cherrypick/)另一个团队的变更，并分离出他们想要保留的代码变更，而不必担心引入不需要的代码或部分实现的特性。

Perforce 的数据模型认为软件历史是单个服务器独有的，而 Git 有能力在任何地方克隆和共享历史。这使得跨公司边界使用 Git 进行协作成为可能，这是跨功能开发环境越来越普遍的需求。

团队入职

### 简单地说，每个人都在做这件事。根据[2019 Coding Sans State of Software Development 报告](https://codingsans.com/state-of-software-development-2019)，90%使用版本控制的 dev 选择 Git。如果你是一名招聘经理，你可以假设任何在过去五到十年中学习了他们的行业的工程师不仅已经知道如何使用 Git，而且*会希望*使用 Git。

Git 伴随着开源社区的热情和敏捷而来，并且正在快速发展以解决现实世界的问题(比如针对大文件存储的 Git LFS 的引入)。此外，任何开发人员都可以为项目贡献代码来改进某个特性或修复某个 bug。

*了解更多关于 [Git LFS](https://support.gitkraken.com/git-workflows-and-extensions/intro-and-requirements/) 以及 GitKraken 如何支持大文件存储的信息。*

费用

这一点会很快。Git 是免费的。就像完全免费一样，在任何时候，不管有多少人在一个项目上工作，也不管你的 Git repos 包含多少数据。

与之形成鲜明对比的是，据报道，Perforce 的每位用户费用为数百美元，最近的估计为 800 美元。Perforce 不再公布价格…这就足够了。当需要购买硬件时，大型团队的成本会变得更加昂贵。

### Team Onboarding

执行强制到 Git 的迁移

既然我们已经概述了将 Perforce 系统转换为 Git 的非常有说服力的论点，那么让我们进入如何完成这项任务的实质部分。

1.为改变做准备

在你开始信口开河之前，花些时间与你的团队交流即将到来的变化，并建立一个过渡期间的工作结构。

### 请记住，切换工具不仅与数据有关，也与开发人员有关。您可能希望做到一丝不苟，拥有安全导出和存储数据的程序，并且不希望暂停团队正在进行的每个项目。

专业提示:考虑一个团队接一个团队、一个项目接一个项目地迁移，并尝试在新项目开始时安排迁移时间。

投入时间和精力向您的团队传达向 Git 迁移的必要性，包括原因、方式和预期的最终结果。毕竟没有人喜欢改变。你肯定会遇到一些阻力；在你的组织中寻找那些能够向同事宣传并保持精神状态的 Git 拥护者。

每个人都需要时间来适应新的系统和工作流程，但这是值得的。进行小的、迭代的、有形的、易于理解的改变将有助于过程的成功和持续。

## 2.清理您的数据

这是删除任何不必要的分支的时候了；每个项目减少到一个主分支是理想的。这也是检查和删除任何不必要提交的二进制文件的好时机。

选择如何导出二进制文件要特别小心，因为它们在 Git 中的管理方式非常不同。虽然 Perforce 在二进制文件管理方面表现出色，但这是 Git 相形见绌的一点。简而言之，您不会想要将 Perforce 中的大量二进制 blobs 合并到 Git 中，因此相应地清理您的数据。 [Git LFS](https://support.gitkraken.com/git-workflows-and-extensions/intro-and-requirements/) 在这里可以派上用场。

### 3.导出您的数据

将数据从 Perforce 转移到 Git 有两种主要方法。您可以使用类似于 [Git Fusion](https://www.perforce.com/perforce/r15.3/manuals/git-fusion/) 的工具，它保存整个历史并将 Perforce 服务器的一部分提取到 Git 存储库中。这个选项需要最多的时间和精力，但是它保留了最多的历史。

或者，你可以选择重新开始。只需提取对应于相关项目的每个分支的头，并将其签入一个新的空 Git 存储库。虽然这是最简单、最直接的方法，但是如果您需要深入研究历史代码，您会希望保持旧的 Perforce 服务器完好无损。

***专业提示:您必须首先确保您的 Perforce 工作区已经用您想要的数据的正确视图定义。***

4.在 Git 中变得有条理

成功移动数据后，可以开始将用户和权限映射到 Git 托管服务，一次一个项目。

然而，将 Perforce 权限转换为 Git 托管服务(GitHub、Bitbucket、GitLab、Azure DevOps)的等效权限可能会很棘手。这是因为 Perforce 权限是出了名的精细和复杂，存在排除对单个文件的访问的风险。

### Perforce 复杂的许可方案是服务器陷入困境的首要原因；每次尝试访问服务器都会在复杂的数据结构上产生昂贵的计算。

考虑到 Git 结构:项目、回购和分支级别，您的项目负责定义更简单的权限集。享受众多新工作流程选项带来的荣耀。

5.健全性检查

在您对迁移是否成功进行苛刻的评估之前，设定现实的期望是很重要的。例如，您的数据传输极不可能在 Git 中产生一点一点的历史记录。这没关系。

### 完成数据导出和导入后，开始测试。在将 DevOps 管道从 Perforce 切换到 Git 之后，使用 CI/CD 验证来确保所有测试都通过。你还能部署吗？

下一步是什么？

在你把鲜血、汗水和泪水投入到一场迫在眉睫的迁徙之后，接下来会发生什么？好吧，你可以把花在 Perforce 上的钱拿去办一个 Git 派对。🤷🥳

要帮助您的团队使用 Git，请查看我们的[学习 Git 数据库](https://www.gitkraken.com/learn-git)，里面有各种经验水平的 Git 教程。

要在您的组织内获得扩展 Git 的帮助，请访问我们的 [Git 知识中心](https://www.gitkraken.com/knowledge-center)以获得关于使用您的托管服务成功扩展 Git 的信息: [GitHub](https://www.gitkraken.com/github) 、 [GitLab](https://www.gitkraken.com/gitlab) 、 [Bitbucket](https://www.gitkraken.com/bitbucket) 和 [Azure DevOps](https://www.gitkraken.com/azure-devops) 。

### 4.在 Git 中变得有条理

成功移动数据后，可以开始将用户和权限映射到 Git 托管服务，一次一个项目。

然而，将 Perforce 权限转换为 Git 托管服务(GitHub、Bitbucket、GitLab、Azure DevOps)的等效权限可能会很棘手。这是因为 Perforce 权限是出了名的精细和复杂，存在排除对单个文件的访问的风险。

Perforce 复杂的许可方案是服务器陷入困境的首要原因；每次尝试访问服务器都会在复杂的数据结构上产生昂贵的计算。

考虑到 Git 结构:项目、回购和分支级别，您的项目负责定义更简单的权限集。享受众多新工作流程选项带来的荣耀。

5.健全性检查

### 在您对迁移是否成功进行苛刻的评估之前，设定现实的期望是很重要的。例如，您的数据传输极不可能在 Git 中产生一点一点的历史记录。这没关系。

完成数据导出和导入后，开始测试。在将 DevOps 管道从 Perforce 切换到 Git 之后，使用 CI/CD 验证来确保所有测试都通过。你还能部署吗？

下一步是什么？

在你把鲜血、汗水和泪水投入到一场迫在眉睫的迁徙之后，接下来会发生什么？好吧，你可以把花在 Perforce 上的钱拿去办一个 Git 派对。🤷🥳

## 要帮助您的团队使用 Git，请查看我们的[学习 Git 数据库](https://www.gitkraken.com/learn-git)，里面有各种经验水平的 Git 教程。

要在您的组织内获得扩展 Git 的帮助，请访问我们的 [Git 知识中心](https://www.gitkraken.com/knowledge-center)以获得关于使用您的托管服务成功扩展 Git 的信息: [GitHub](https://www.gitkraken.com/github) 、 [GitLab](https://www.gitkraken.com/gitlab) 、 [Bitbucket](https://www.gitkraken.com/bitbucket) 和 [Azure DevOps](https://www.gitkraken.com/azure-devops) 。

For help getting your team onboarded using Git, checkout our [Learning Git database](https://www.gitkraken.com/learn-git), stocked full with Git tutorials for every experience level. 

For help scaling Git within your organization, visit our [Git Knowledge Center](https://www.gitkraken.com/knowledge-center) for information on successfully scaling Git with your hosting service: [GitHub](https://www.gitkraken.com/github), [GitLab](https://www.gitkraken.com/gitlab), [Bitbucket](https://www.gitkraken.com/bitbucket), and [Azure DevOps](https://www.gitkraken.com/azure-devops).