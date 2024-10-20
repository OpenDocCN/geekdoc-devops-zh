# 通过 Harness | Harness 使用 Github 动作

> 原文：<https://www.harness.io/blog/using-github-actions-with-harness>

了解 GitHub Actions 和 Harness 如何在几分钟内帮助您完成到 Kubernetes 的 CI/CD。

GitHub Actions 是一种执行开发工作流的自动化方式。这些通过 YAML 文件定义的工作流存储在您的 GitHub 存储库中，位于。github/workflows 目录。您可以通过外部事件、预定事件或 GitHub 存储库事件(例如对 GitHub 存储库或问题创建的 push 或 pull 请求)以三种方式触发这些工作流。如果你想了解更多关于 GitHub 动作的信息，包括这个特性的使用限制，请看这个文档[这里](https://help.github.com/en/actions)。

使用 GitHub 动作的一种方法是创建持续集成(CI)工作流，该工作流构建和测试用应用程序的编程语言编写的项目。GitHub Actions 还能够将工件发布到 GitHub 包或另一个包托管提供商，如 Docker Hub。有对 GitHub 动作的 Dockerfile 支持，以及利用其他开发者构建的动作的能力。

这篇博文将这些 GitHub Actions 特性与 Harness 平台结合使用，以持续交付 Kubernetes 部署。

## 预备动作！

如果你想一步一步地跟随这个指南，你可以在这里找到现在公开的应用代码[库。否则，您可以使用自己的应用程序代码和 docker 文件。](https://github.com/tiffanyjachja/tic-tac-toe)

要开始 GitHub 操作，通过选择 GitHub 存储库的 Actions 选项卡创建一个工作流。

如果选择“自己设置工作流”选项，GitHub 会创建一个 main.yml 文件，其中包含。github/workflows 目录为您提供了一个您可以编辑的模板化工作流。

这里，我在 CI 工作流中创建了一个“build”和“create_dockerfile”作业。这些作业是工作流中的连续步骤。推送提交会将此工作流触发到存储库。任何失败的步骤都会导致工作流在失败时退出。

我的 create_dockerfile 作业需要我的 Docker 帐户的凭据。要定义这些密码，请转到“设置”、“密码”,然后添加新密码。

另外，请注意，您的 Docker 凭证必须有权从您的 Docker Hub 推送和提取 Docker 图像。

一旦现在触发你的 GitHub Actions CI 工作流，并查看工作流内每个步骤的细节。

在这里，我在 Docker Hub 上的图像存储库有我的工作流中指定的名为 tictactoe 的新推送的 Docker 图像。

## 驾驭你的交付

现在是时候用 Harness 将新工件部署到 Kubernetes 环境中了。

在“设置”下，确保您的环境中安装了一个线束委托，并且它当前正在运行。在连接器下，工件服务器确保您已经用有效的 Docker 凭证添加了工件服务器。

在 Setup 下创建一个名为 tictactoe 的新应用程序。使用此应用程序视图创建部署 tictactoe 应用程序的服务、环境和工作流。

这里，我的应用程序部署了一个名为 tictactoe 的服务。为了从我的公共 Docker Hub 存储库中提取 Docker 映像，我需要前面定义的适当的源服务器和相应的存储库信息。

在为部署指定了目标 Kubernetes 环境之后，创建一个 Kubernetes 部署工作流。在这里，我使用 minikube 环境来部署最新的工件。

当您准备好部署工件时，点击 deploy 按钮并查看应用程序部署状态。

创建部署工作流非常简单，不到 3 分钟就可以完成。这个视频是使用 Harness 部署 tictactoe 工件的分步指南。

在 Harness 中创建工作流触发器也非常简单。这里，我触发了“部署到 minikube”工作流，条件是有一个新的 tictactoe 工件(docker 映像)准备好被部署。

## 掌握您的开发工作流程

Github Actions 允许开发人员为各种应用程序框架、运行时和语言触发软件开发工作流。这篇博客发布了如何在 GitHub Actions 中建立一个持续集成工作流来构建一个可部署的工件。然后，我们使用 Harness 在不到 3 分钟的时间内将工件连续交付到 Kubernetes 环境中。[免费试用 Harness CD](https://app.harness.io/auth/#/signin)开始交付独特的开发人员工作流程。