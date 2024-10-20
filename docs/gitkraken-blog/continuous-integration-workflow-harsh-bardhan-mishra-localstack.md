# 持续集成工作流程| GitKon 2022 | Harsh Bardhan Mishra，LocalStack

> 原文：<https://www.gitkraken.com/gitkon/continuous-integration-workflow-harsh-bardhan-mishra-localstack>

[https://www.youtube.com/embed/tgXSBa65Nzs?feature=oembed](https://www.youtube.com/embed/tgXSBa65Nzs?feature=oembed)

视频

### **为您的云构建自动化持续集成工作流&无服务器应用**

随着云&无服务器应用程序开发的兴起，工程师们遇到了本地开发的繁琐问题。当今的云开发缓慢、乏味且成本高昂，并且缺少模拟云服务的选项，这些云服务大多绑定到专有的云提供商。

这些挑战限制了工程师可用的选项，迫使他们依赖模拟测试工具来模拟可以在本地和持续集成(CI)管道上运行的本地云基础架构。

[LocalStack](https://github.com/localstack) 是一个**云服务模拟器**，它可以在你的本地机器或 CI 环境上的一个容器中运行，并让你运行你的**云**和**无服务器**应用程序，而无需连接到亚马逊网络服务帐户。

使用 LocalStack，您的应用程序所依赖的所有云资源都可以在本地获得。它允许您在 AWS 环境中运行自动化应用程序测试，而不需要昂贵的 AWS 开发人员帐户、缓慢的重新部署或来自远程连接的瞬时错误。

## **开始使用 LocalStack 持续集成工作流**

通过`pip`安装官方命令行界面就可以开始使用 LocalStack 了。您可以直接访问 [CLI](https://www.gitkraken.com/cli) 来创建您的 LocalStack 实例，将所有 AWS 服务包含在一个 Docker 容器中。

安装后，您可以通过官方 AWS CLI 与这些本地 AWS 服务进行交互，或者使用第三方集成，如 Terraform、CDK 等等。

## **本地堆栈&地形整合**

让我们看看 Terraform 与 LocalStack 的集成。有一个[示例 Terraform 脚本](https://github.com/HarshCasper/GitKon-2022-LocalStack)，它采用基于 NodeJS 的 Lambda 并将其部署为 AWS Lambda 函数 URL。

然后，这些函数 URL 可以作为 REST APIs 使用标准 HTTP 请求来触发。Terraform 是一个基础设施代码框架，它创建一个计划，然后应用该计划来创建资源。我们可以使用相同的 Terraform 配置在生产环境中部署 AWS，在本地或 CI 环境中部署 LocalStack。

要将 LocalStack 与 Terraform 的 AWS provider 一起使用，可以使用定义了本地 AWS 端点的`provider.tf`文件或 LocalStack 的`tflocal`脚本。

下面是应用 Terraform 配置在本地创建 Lambda 函数的样子:

正如您在上面的例子中所看到的，Terraform 使用 LocalStack 在本地创建了整个基础设施，只有一个运行中的 Docker 容器。现在，让我们回顾一下如何在持续集成提供者上运行它。

## **使用 GitHub 操作运行 LocalStack】**

所有重要的持续集成提供商都支持 LocalStack 来部署和测试您的 AWS 应用程序。工程师将 LocalStack 与 CI 提供者一起使用，以确保他们在模拟环境中进行测试，而不是调用实际的 AWS APIs。后者成本较高，理想情况下只应用于生产。

要开始使用像 GitHub Actions 这样的 CI 提供程序，您必须获取 LocalStack 映像并下载 LocalStack CLI。这里有一个[示例工作流](https://github.com/HarshCasper/GitKon-2022-LocalStack/blob/main/.github/workflows/ci.yml)，你可以在你的`.github/workflows`目录中使用它来开始测试你的 AWS 云和无服务器应用，而不需要使用任何真正的 API 来模拟你的堆栈。

GitKraken 的传奇 [Git 工具](https://www.gitkraken.com/)将帮助你提升你的持续集成工作流程。今天免费试用 GitKraken 工具:[桌面版 GitKraken 客户端](https://www.gitkraken.com/git-client)； [GitLens for VS 代码](https://www.gitkraken.com/gitlens)；和[吉拉的 Git 集成](https://www.gitkraken.com/git-integration-for-jira)。