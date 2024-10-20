# GitLab CI |如何使用与 GitKraken Git GUI 的持续集成

> 原文：<https://www.gitkraken.com/blog/gitlab-ci>

这篇文章是一位客座作者写的。

## **配合 GitKraken 使用 git lab CI**

在当今的软件开发世界中，最受重视的实践之一是 CI，即持续集成。

持续集成是 CI/CD 管道的第一步，是整个 DevOps 思维和方法的推动者。竞争情报是现代软件开发的基础。鉴于这是正确安装 DevOps 的第一步，因此必须正确安装至关重要。

假设您有一个应用程序，它的代码在 GitLab 存储库中。开发人员每天都会推送代码变更，甚至一天多次。随着每个变更提交到应用程序的代码库，必须创建和测试一个新的构建。

涉及 GitLab CI 的最简单的持续集成流程之一包括以下阶段:

*   将更改推送到 [GitLab](https://www.gitkraken.com/integrations/gitlab) 。
*   构建应用——由 [GitLab runner](https://docs.gitlab.com/runner/) 执行的过程，包括将源代码打包到独立的软件工件中(我们将在本文后面提供一个带有 Docker 映像的例子)。
*   测试应用程序——由 GitLab runner 执行的过程，目的是在部署我们的应用程序之前验证我们的代码更改——在我们的例子中使用服务或工具 pylint。

## **定义 GitLab CI/CD**

我们想要实现的是一个零错误的稳定应用程序，同时遵守指导方针和代码遵从标准。

基本上，我们希望不断地构建、测试和部署迭代代码变更。再往下，我们可以看到整个 GitLab CI/CD 流程的高级图。

***趣闻: [GitLab 本身](https://gitlab.com/gitlab-org/gitlab)就是一个把持续集成作为软件开发方法#ouroboros 的项目的例子。***

再往下，我们有*连续交付*，这意味着不仅我们的应用程序在每次代码变更被推送到存储库时都被构建和测试，而且应用程序也必须被连续部署。然而，使用*连续投放*，我们手动触发部署，而不是自动触发的连续部署。

我们的目标是使部署可预测且稳定，无论是大规模分布式系统还是复杂的生产环境。 ***连续部署*** 下一步过去 ***连续交付。*** 部署测试过的变更不需要人工干预。

## **如何实施 GitLab CI**

从工具的角度来看，我们需要一个 GitLab repo 和 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 来演示一个适当的 GitLab CI/CD 流。

剧透:通过 GitKraken 的无缝集成和可视化提交图，实现 CI/CD 工作流要容易得多。你还在等什么？⬇️

## **GitLab CI yml 文件**

GitLab 持续集成的主要组件之一是。gitlab-ci.yml，它位于存储库的根目录中，包含 GitLab CI/CD 配置。

首先，我们来谈谈 [Git 仓库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)的结构。我们有自己的应用程序，在本例中，是用 Python 编写的 Prometheus exporter 基本上是一个公开 API 的应用程序。

我们存储库的布局包括:

*   app.py + requirements.txt(我们的应用和依赖项)
*   Docker file–包含将我们的应用程序打包成 Docker 映像所需的所有命令的文件
*   curl _ test . sh——我们的端点的一些基本测试
*   。git ignore–从 git 跟踪中排除某些文件
*   。git lab-ci . yml–描述我们的管道流程

正如我们之前提到的，我们需要对构建过程进行一些扩展。我们构建过程的核心是 [Dockerfile](https://docs.docker.com/engine/reference/builder/) ，它提供了将我们的应用程序打包成 Docker 映像的方法(命令)。构建使用 Dockerfile 文件和“上下文”。上下文是构建映像的目录中的一组文件；基本上，Docker 会通过读取 Docker 文件中的指令来自动构建映像。

GitLabs CI/CD 核心组件:

*   **管道**:持续集成、交付和部署的顶层组件。
*   **runner** : GitLab 使用不同服务器上的 runner 来实际执行一个管道中的作业；GitLab 提供了 runner 来使用，但是你也可以作为[runner](https://docs.gitlab.com/runner/install/)来运行你自己的服务器。
*   **作业**:定义做什么的基本配置组件(如编译或测试代码)。
*   **阶段**:定义何时运行作业。

简而言之，**作业**正由**运行者**执行，**阶段**提供了一种组织**作业**的方式(例如，如果多个作业在同一个阶段，它们被并行执行)。如果一个**阶段**中的所有**作业**成功，则**流水线**移动到下一个**阶段；**否则，一个作业失败，整个流水线都会失败。

下面是一个具体的例子，我们有三个阶段(构建、测试、部署)。在测试级阶段，有两个并行执行的测试作业，如果其中一个失败，管道将失败，部署阶段将不会执行。

## **从 GitKraken 触发 git lab CI**

gitlab-ci.yml 文件根据构成管道的作业描述了我们的工作流，后者是 GitLab CI/CD 的顶级组件。

当我们在 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 中添加一个. gitlab-ci.yml 文件到我们的存储库时，gitlab 会自动检测它，一个名为 GitLab Runner 的应用程序会运行阶段中定义的步骤。

下面我们可以看到。gitlab-ci.yml 文件显示在 GitKraken 的应用内文本编辑器中。

从一个高层次的角度来看，我们的管道有四个由运行者执行的主要任务，其中两个是并行执行的:test-code-job1 和 test-code-job2。

我们的目标是触发 GitLab CI 流程；这意味着直接从 GitKraken 构建/打包、测试并最终部署我们的应用程序。

使用 GitKraken 这样的工具，可视化 CI/CD 工作流更加直观，因此您的整个团队都可以做出贡献，无论他们的技能水平如何。

为了更好地查看我们的工作流，我们将使用**标签，**标签对于标记特定的部署和发布以供以后参考非常有用。

***GitTip:了解如何在 GitKraken 中使用标签，包括如何 [Git 推送标签](https://www.gitkraken.com/learn/git/problems/git-push-tag)以及如何[结帐 Git 标签](https://www.gitkraken.com/learn/git/problems/git-checkout-tag)。***

标签的创建和注释都是直接从 GitKraken 中完成的，所以不需要使用像`git tag -a v1.0 -m 'Version X.Y'`这样的命令。

Git 标签提供了将历史中的特定点——提交——标记为重要点并将其“标记”为发布版本的能力。

在我们推送我们的更改之后，我们可以看到构建过程已经被触发，因此，在 repository**Container Registry**选项卡下，我们可以看到我们的工件:带有所需标签的 Docker 图像。在 GitLab CI 中，每个项目都可以有自己的容器注册空间来存储图像。

根据前面提到的概念，我们可以看到用正确的工具发布新的代码变更(升级到**9.0**版本)是多么容易💻关于 CI/CD 最佳实践。

[https://www.youtube.com/embed/putJq1WZ9eU?feature=oembed](https://www.youtube.com/embed/putJq1WZ9eU?feature=oembed)

视频

## **准备好配合 GitKraken 使用 GitLab CI 了吗？**

正如所料，GitLab CI 工作流有多种风格/变化，但是一旦您有了一个稳定和清晰的工作流，您将能够在以后轻松地扩展它。

GitLab 使我们能够建立和发展健康的 CI/CD 实践，而 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 通过提供灵活性和简化整个流程，为整个生态系统带来附加值，使管理整个管道变得更加容易，并具有更好的可见性。

开始持续集成可能看起来势不可挡，但是在 GitKraken 的帮助下，您和您的团队将很快开始 CI/CD 工作流。