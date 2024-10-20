# GitHub Actions for Azure |自动化 for Azure

> 原文：<https://www.gitkraken.com/blog/github-actions-for-azure>

这篇文章是由客座作者 Nahla Davies 写的。Nahla 自 2010 年以来一直在软件领域工作，自 2019 年以来一直是一名技术作家。她曾在一家拥有 5000 家公司的体验式品牌机构担任首席程序员，该机构的客户包括三星、时代华纳、网飞和索尼。

Microsoft Azure 在开发人员和组织中广受欢迎，因为它具有可伸缩性、灵活性和可定制性。微软没有与之对抗，而是通过提供一种新的方式来自动化开发和部署 Azure 的 GitHub Actions，拥抱开源运动。

GitHub Actions 是一个工具，允许团队自动化软件开发工作流，与 Azure 管道和您的定制需求一起工作。开发人员可以在他们的存储库中创建工作流，从 GitHub 到 Azure 构建、测试、打包、发布和部署任何项目，而无需手动输入。这个过程有助于使开发和交付更加智能和高效，以便您的团队可以专注于开发优秀的代码，而不是处理重复的手动任务。

GitHub Actions 还能做什么？具体是怎么运作的？本指南将回答这些问题以及更多问题。

## GitHub 操作的核心元素

在开始使用 GitHub Actions for Azure 之前，尤其是如果你不熟悉[后端工作流自动化](https://www.waveapps.com/freelancing/full-stack-web-developer-portfolio)，你应该熟悉以下 GitHub Actions 语法。

*   工作流程
*   事件
*   乔布斯
*   行动
*   滑行装置

让我们更深入地了解一下这些元素的含义。

工作流程

工作流是运行一个或多个作业以简化测试、开发和交付的自定义自动化流程。它们是由存储库中的 YAML 文件定义的，只要被事件触发，这些文件就会运行。这些触发器可以是自动事件，也可以是手动触发器，甚至可以按照循环计划来定义。

### 每个存储库可以有多个工作流，为您的团队执行各种任务。一个工作流可以帮助构建和测试 pull 请求，另一个工作流可以在用户打开新问题时添加标签，而另一个工作流可以用于在发布自动可用时部署应用程序。

事件

当谈到 Azure 中的 [GitHub 动作时，事件就是触发工作流运行的东西。事件可以是您在存储库中指定的任何活动，用于触发工作流中的一组作业。当您选择的一个事件发生时，您触发的活动将根据您的独特配置来自 GitHub。](https://learn.microsoft.com/en-us/azure/developer/github/github-actions)

有数不清的事件可以用来执行重要的工作流，例如当用户打开一个问题、创建一个新的“拉”请求或向存储库提交一个提交时。事件也可以是时间表的形式。当达到某个日期、时间或发展里程碑时，您计划的工作流将运行。

### 滑行装置

在我们深入讨论之前，讨论跑步者是很重要的。运行程序是运行触发的工作流的服务器，每个运行程序一次只运行一个作业。GitHub 为 Azure 提供了 runners，这样每个工作流都在一个干净的虚拟机中运行。

对于更复杂的配置，您可以利用 GitHub 提供的更大的[滑道。或者，您也可以托管自己的跑步者来创建自定义配置。Runners 有点超出了本文的范围，但是它们在运行 GitHub 动作中起着至关重要的作用。](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)

乔布斯

### 作业是指在同一运行器上执行的工作流中的步骤。每个作业描述一个要执行的 shell 脚本或一个在事件触发工作流时运行的操作。一旦事件通知工作流开始，它将根据您的配置开始运行每个作业。换句话说，每一步都依赖于另一步，因此它们将按照依赖关系的顺序执行。

默认情况下，作业并行运行，因此您必须配置每个作业的依赖关系，以便在正确的时间执行操作。例如，假设您正在处理多个没有依赖关系的构建作业。这些作业将同时并行运行。但是，如果您有一个依赖于构建作业的打包作业，它将在其依赖项完成时运行。

行动

现在，我们来谈谈行动。GitHub Actions 是帮助开发团队执行频繁重复任务的定制应用程序。与其他常见的工作流自动化不同，GitHub 动作可以执行复杂的任务，以简化您的构建和部署过程。您可以为定制的体验编写自己的操作，也可以从 GitHub Marketplace 中选择操作。

### 对于 Azure 环境，操作可以做的事情有许多种可能性。例如，您可以使用 Actions 从 GitHub 获取 git 库，或者为您独特的构建环境建立最佳工具链。您甚至可以使用 GitHub 操作进行管理，比如为您的云提供商设置认证协议。

为 Azure 使用 GitHub 操作

现在你已经知道了 GitHub 动作的基础，让我们讨论一下如何在 Azure 中有效地使用它。当谈到 Azure 的操作时，你只受到项目需求的限制。您的团队可以通过多种方式利用 Azure 环境中的自动化操作。

让我们来看看 GitHub Actions 与 Azure 配合使用的几种方式:

### Actions

连接 GitHub Actions 和 Azure

将 GitHub Actions 连接到 Azure 所要做的就是使用一个服务主体。或者，您可以发布一个配置文件，通过 GitHub 连接到 Azure。每次使用 Azure 登录操作时，您都必须使用服务主体，但是您可以将它用于 Azure 的大多数操作，以简化您的任务。

## 从 Azure 到 GitHub 进行身份验证

您还可以使用 PowerShell 和 CLI 的 [Azure 登录，在 GitHub 操作工作流中与您的 Azure 资源进行交互。有两种不同的方法可供选择。您可以通过带有秘密的服务主体或 OpenID Connect 进行身份验证。](https://www.sqlshack.com/different-ways-to-login-to-azure-automation-using-powershell/)

让我们来看看 GitHub Actions 与 Azure 配合使用的几种方式:

将应用从 GitHub 部署到 Azure

### 通过应用服务部署中心，可以很容易地将应用从 GitHub 部署到 Azure。这种方法将根据您的 sack 自动生成一个工作流，并将其正确地提交到您的存储库中。或者，您可以使用手动工作流将应用程序从 GitHub 部署到 Azure。

将 GitHub Actions 连接到 Azure 所要做的就是使用一个服务主体。或者，您可以发布一个配置文件，通过 GitHub 连接到 Azure。每次使用 Azure 登录操作时，您都必须使用服务主体，但是您可以将它用于 Azure 的大多数操作，以简化您的任务。

将数据库从 GitHub 部署到 Azure

### 你也可以将数据库从 GitHub 部署到 Azure。可以使用操作将数据库部署到 Azure SQL、Azure MySQL 和 Azure Database for PostgreSQL。每个数据库都有自己的部署方法和操作，使部署过程变得快速而简单。

您还可以使用 PowerShell 和 CLI 的 [Azure 登录，在 GitHub 操作工作流中与您的 Azure 资源进行交互。有两种不同的方法可供选择。您可以通过带有秘密的服务主体或 OpenID Connect 进行身份验证。](https://www.sqlshack.com/different-ways-to-login-to-azure-automation-using-powershell/)

对 GitHub 操作使用变量替换

### 变量替换操作可用于替换配置和参数文件中的值。例如，您可以使用这些操作在工作流运行期间将类似于 [GitHub secrets](https://www.gitkraken.com/media/events/azure-spring-clean-2022) 的值插入到文件中。只要变量被定义为环境变量或者以其他方式可用，变量替换操作就可以在 Azure 上运行。

通过应用服务部署中心，可以很容易地将应用从 GitHub 部署到 Azure。这种方法将根据您的 sack 自动生成一个工作流，并将其正确地提交到您的存储库中。或者，您可以使用手动工作流将应用程序从 GitHub 部署到 Azure。

使用 GitHub 管理 Azure 策略

### GitHub Actions 也为团队提供了各种管理 Azure 策略的方法。例如，您可以使用 GitHub 操作从 Azure 导出 Azure 策略，通过 GitHub 将 Azure 策略作为代码进行管理，甚至触发 Azure 合规操作来简化开发生命周期的每个阶段。

你也可以将数据库从 GitHub 部署到 Azure。可以使用操作将数据库部署到 Azure SQL、Azure MySQL 和 Azure Database for PostgreSQL。每个数据库都有自己的部署方法和操作，使部署过程变得快速而简单。

使用 GitHub Actions 和 Azure 构建自定义虚拟机映像

### 如果你想要一个真正定制的体验，你可以创建一个工作流来使用 GitHub Actions for Azure 构建一个虚拟机镜像。为了加快 CI/CD 流程，您可以使用工作流中的工件创建自定义虚拟机映像，这些工件可以分发到共享映像库中。

变量替换操作可用于替换配置和参数文件中的值。例如，您可以使用这些操作在工作流运行期间将类似于 [GitHub secrets](https://www.gitkraken.com/media/events/azure-spring-clean-2022) 的值插入到文件中。只要变量被定义为环境变量或者以其他方式可用，变量替换操作就可以在 Azure 上运行。

如何用 GitHub 动作定制你的 Azure 工作流

### 使用 GitHub Actions for Azure 的好处之一是工作流的可能性几乎是无限的。您可以查看 GitHub Marketplace，找到要添加到工作流中的操作，使用存储库中的操作，或者引用 Docker Hub 上的容器。

以下是如何[找到并定制](https://docs.github.com/en/actions/learn-github-actions/finding-and-customizing-actions)动作:

### 从 GitHub Marketplace 添加操作

如果你刚刚开始使用 GitHub Actions for Azure，你的第一站应该是 GitHub Marketplace。在工作流编辑器中工作时，导航到边栏以查找新操作。您可以搜索特定操作或浏览特色操作和类别。您还会看到一个星级，以便您可以只找到要添加到 Azure 工作流中的最佳操作。

向存储库添加操作

## 如果您的存储库中已经有了一组操作，您可以简单地将操作添加到您在工作流编辑器中正在处理的任何内容中。你所要做的就是用这些语法路径之一来引用这个动作:‌ `{owner}/{repo}@{ref}`或`./path/to/dir`。

如果您想要的动作是在工作流文件之外的公共存储库中定义的，请使用以下语法引用该动作:`{owner}/{repo}@{ref}`。

以下是如何[找到并定制](https://docs.github.com/en/actions/learn-github-actions/finding-and-customizing-actions)动作:

引用 Docker Hub 上的容器

### 最后，您还可以[添加一个容器动作](https://www.gitkraken.com/gitkon/aws-cdk-github-actions-ci-cd-deployment)到您的工作流中。如果您在发布到 Docker Hub 的 Docker 容器映像中定义了一个动作，您应该使用以下语法引用该动作:`docker://{image}:{tag}`。

但是，在将任何操作引用到工作流文件之前，请确保在将代码引入到您的环境中之前验证代码是安全的。

将你的知识付诸行动

### 现在你已经有了一个关于 GitHub Actions for Azure 的速成班，是时候把它付诸实践了。您的团队有许多方法可以使用行动来加速开发和交付过程。首先看看我们在文章中提到的 GitHub Actions 对 Azure 的一些使用；然后，您可以开始定制自己的操作和工作流，以获得量身定制的 Azure 体验。

如果您的存储库中已经有了一组操作，您可以简单地将操作添加到您在工作流编辑器中正在处理的任何内容中。你所要做的就是用这些语法路径之一来引用这个动作:‌ `{owner}/{repo}@{ref}`或`./path/to/dir`。

如果您想要的动作是在工作流文件之外的公共存储库中定义的，请使用以下语法引用该动作:`{owner}/{repo}@{ref}`。

### 引用 Docker Hub 上的容器

最后，您还可以[添加一个容器动作](https://www.gitkraken.com/gitkon/aws-cdk-github-actions-ci-cd-deployment)到您的工作流中。如果您在发布到 Docker Hub 的 Docker 容器映像中定义了一个动作，您应该使用以下语法引用该动作:`docker://{image}:{tag}`。

但是，在将任何操作引用到工作流文件之前，请确保在将代码引入到您的环境中之前验证代码是安全的。

将你的知识付诸行动

## 现在你已经有了一个关于 GitHub Actions for Azure 的速成班，是时候把它付诸实践了。您的团队有许多方法可以使用行动来加速开发和交付过程。首先看看我们在文章中提到的 GitHub Actions 对 Azure 的一些使用；然后，您可以开始定制自己的操作和工作流，以获得量身定制的 Azure 体验。

Now that you’ve had a crash course on GitHub Actions for Azure, it’s time to put it into practice. There are many ways that your teams can use Actions to speed up development and delivery processes. Start by checking out some of the uses for GitHub Actions for Azure we mentioned here in the article; then, you can start customizing your own actions and workflows for a tailor-made Azure experience.