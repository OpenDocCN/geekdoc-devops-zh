# 如何有效实施| Harness

> 原文：<https://www.harness.io/blog/implement-ci-cd-efficiently>

让我们来探讨如何采用 CI/CD 来提高组织的效率和交付渠道。

DevOps 在软件开发团队中越来越受欢迎。考虑到以下好处，这并不奇怪:简化的工作流导致更快的发布频率和部署周期，更快的错误检测，在任何错误情况下的有效回滚，以及整体上更有生产力的团队。DevOps 工程师确保代码开发、测试和发布的流程和实践得到简化，以减少低效、风险和上市时间。结合[持续集成(CI)](https://harness.io/blog/what-is-continuous-integrationhttps://harness.io/blog/what-is-continuous-integration) 和[持续交付(CD)](https://harness.io/blog/what-is-continuous-delivery) ，DevOps 通过自动化与代码部署相关的手动任务，帮助 IT 组织简化开发流程。让我们来探讨如何采用 CI/CD 来提高您组织的效率和交付渠道。

## 连续累计

首先，我们来看看什么是 CI/CD，它的好处是什么。尽管这两个概念经常被放在一起讨论，但是它们在软件开发过程中各有自己的角色。

持续集成是自动构建和测试代码的过程。它需要一个 SaaS 构建服务(比如 Harness 托管的构建)或者一个[内部构建服务器](https://app.harness.io/auth/#/signup/?module=ci?utm_source=internal&utm_medium=social&utm_campaign=community&utm_content=replace&utm_term=get-started)，负责从源代码控制系统(比如 Git)获取代码，编译它，并测试它以确保它没有错误。这有助于通过确保错误/失误/缺陷一出现就被发现来保持软件项目的正常运行。如果一个构建被破坏了，它会暂停剩下的过程，直到它被修复。因此，构建过程更加简化。

## 持续交付和部署

连续交付自动化了发布过程。它确保代码总是处于可发布的状态。这个过程包括从构建系统中获取代码，并通过一个阶段化环境对其进行路由，以确保它适合发布。结果是一个完全自动化的部署过程，它可以由团队控制，也可以由团队外部的触发器触发(比如当开发人员向主分支推送或提交代码时)。CI 和 CD 的关键区别在于前者专注于构建代码，而后者专注于发布最终产品。

下图描述了 CI、CD 和[连续部署](https://harness.io/blog/continuous-deployment)之间的区别。

持续部署是 CI/CD 流程的最后也是最需要的阶段。这是团队可以零接触自动化部署代码的时候。这就是持续交付和部署的区别。当代码的部署通过人工干预完成时，这就是交付。然而，如果代码是在没有任何人工干预的情况下以自动化的方式部署的，这就叫做连续部署。

## 如何在您的组织中实施 CI/CD

在您的组织中实施 CI/CD 之前，您应该知道您为什么要实施它，目标是什么，以及您希望关注哪个指标。例如，DevOps 团队通常关注四个关键( [DORA](https://cloud.google.com/blog/products/devops-sre/using-the-four-keys-to-measure-your-devops-performance) )指标:

1.  *变更的交付时间*
2.  *改变失败率*
3.  *部署频率*
4.  *平均恢复时间*

您的 CI/CD 目标也可以很简单，从传统软件交付实践转移到云原生实践。这样，理解 DevOps 概念的基础变得非常重要。

如果您还没有，您可能需要采用一个版本控制系统，实现一个构建系统，并引入阶段环境。一旦这些基础工作就绪，您就可以开始 CI/CD 流程了:

1.  首先，您将从 CI 开始。这个过程确保代码经过测试，并且是干净的，随时可以部署。
2.  然后，您可以转移到 CD 部分，在那里您将能够将代码部署到阶段，并准备好供团队审查。
3.  一旦一切都被批准，您就可以将代码部署到生产环境中。

接下来，您需要评估不同的 CI/CD 工具，以了解哪种工具最适合您的组织。不幸的是，有太多的工具会让你们有些人不知所措。在本文中，我们将使用一个著名的 CI/CD 工具 Harness，通过一个简单的 hello world 应用程序来解释 CI/CD 是如何工作的。一个完善的 CI/CD 管道还可以让您的团队更容易地确定需要改进的地方，并采取措施降低系统中可能存在的风险。随着您的团队随着时间的推移而成长和变化，这也使得实现过程变更变得更加容易。

## 线束 CI/CD 教程

在本教程中，我们将只关注创建带有线束的 CI/CD 管道。我们也是 GitOps 的忠实粉丝，我们有 [GitOps-as-a-service](https://harness.io/blog/generally-available-harness-gitops-as-a-service) 可用于加速您的部署。

采用 CI/CDpipeline 的软件开发团队看到了更快的软件部署，风险更小，错误识别和修复更快。如果您是 CI/CD 的新手，似乎需要学习大量的新术语和流程。但是建立一个 [CI/CD 管道](https://docs.harness.io/article/3amcd8hn53-ci-pipeline-basics)比你想象的要容易。

CI/CD 管道有助于您的团队在问题到达最终用户之前，查明软件中可能出现问题的地方。此外，它允许您更频繁地发布更新，并在向用户发布之前花更少的时间测试单个组件或模块。Harness 是 CI/CD 领域的领导者，拥有无可挑剔的特性。今天，我们将向您展示如何通过简单的设置在几分钟内实施 CI/CD。

### 先决条件

*   免费[利用客户](https://app.harness.io/auth/#/signup/?utm_source=internal&utm_medium=social&utm_campaign=community&utm_content=pavan_cicd_article&utm_term=get-started)进行 CI/CD(内部)
*   Kubernetes 从任何云提供商处访问集群来部署我们的应用(您也可以使用 [Minikube](https://minikube.sigs.k8s.io/docs/start/) 或 [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/) 来创建单节点集群)。
*   Docker，最好是 [Docker 桌面](https://www.docker.com/products/docker-desktop/)
*   下载并安装 [Node.js](https://nodejs.org/en/download/)

首先，我们将创建一个简单的" *Hello World！*“node . js 中的应用，带有一个简单的测试用例。我已经创建了这个简单的“Hello World！”应用程序来使它更容易，并把它推到 GitHub。你可以[叉这个回购](https://github.com/pavanbelagatti/harness-ci-example)开始工作。

您在 repo 中看到的 Docker 文件将用于构建我们的应用程序，并将其作为映像推送到 Docker Hub。接下来，我们将构建映像，并使用命令将其推送到 Docker Hub，

###### docker buildx build-platform = Linux/arm 64-platform = Linux/amd64-t docker.io/<docker hub="" username="">/:<tag>-push-f ./docker 文件.</tag></docker>

一旦构建和推送成功，您可以通过您的 Docker Hub 帐户进行确认。

您可以在分叉的 repo 中看到 deployment.yaml 文件，它定义了部署 yaml 文件来帮助我们将应用程序部署到我们的 Kubernetes 集群。此时，确保您的 Kubernetes 集群已经启动并运行。

一旦一切就绪，就该建立一个线束帐户来做 CI/CD 了。创建一个免费的线束帐户和你的第一个项目。一旦您在 Harness 注册，您将获得新的 CI/CD 体验和功能。

添加所需的连接器、GitHub repo、Docker Hub 和 secrets(如果有)。Harness 中的 Delegate 是一个服务/软件，您需要在目标集群(在我们的例子中是 Kubernetes 集群)上安装/运行，以将您的工件、基础设施、协作、验证和其他提供者与 Harness Manager 连接起来。第一次设置线束时，安装线束委托。

选择持续集成模块，并添加必要的阶段和步骤，如下所示。

“测试”步骤设置如下:

“推送至 Docker 注册表”步骤如下:

接下来，设置部署管道。

在“服务”选项卡中添加所需的详细信息。

在“环境”选项卡中定义环境类型。

通过选择您喜欢的部署来制定执行策略。

保存所有内容并运行管道。

您可以看到 CI 和 CD 都按照指定的所有步骤逐一执行。

**恭喜你！**我们使用 Harness 平台成功构建并测试了应用程序代码，并将其部署到我们的 Kubernetes 集群上。

您可以使用命令 **kubectl** get pods 来确认这个部署

您可以在 deployment.yaml 文件中看到两个副本按照我们的规范运行。

此外，请访问您的 Kubernetes 仪表盘进行确认。由于我正在使用谷歌云(GCP)，我可以看到并确认有两个豆荚运行。

Harness platform 使开发人员可以通过利用不同的可用模块来简化他们的 SDLC。今天我们看到了 CI 和 CD 模块，到目前为止 Harness 总共有七个模块。

## CI/CD 和 DevOps

CI/CD 是任何 DevOps 战略的重要组成部分。它有助于自动化代码审查和测试过程，使团队更容易测试和部署软件。这也是创造持续改进文化的重要部分。同样重要的是要记住，CI/CD 不是灵丹妙药。奠定 [DevOps 文化](https://www.atlassian.com/team-playbook/examples/devops-culture)和方法论是最初的步骤。同样重要的是要记住，DevOps 下的这些流程和方法不是静态的；它们是不断发展的，应该根据需要进行调整以满足团队的需求。

如果您有兴趣了解有关使用 Harness 和智能软件交付的更多信息，[免费试用](https://app.harness.io/auth/#/signup)并查看我们的[开发者中心](https://developer.harness.io/)以获得更多分步教程、视频和参考文档。