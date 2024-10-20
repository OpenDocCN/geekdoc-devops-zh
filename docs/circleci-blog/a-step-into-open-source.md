# 迈向开源的一步——circle ci

> 原文：<https://circleci.com/blog/a-step-into-open-source/>

*编者按:本帖最初发表于 2014 年 8 月 28 日，为了准确和全面，于 2018 年 11 月 29 日更新。*

我们创建 CircleCI 是为了帮助开发团队提高效率。作为其中的一部分，我们长期以来一直专注于生产 web 应用程序的工具，CircleCI 的大部分特性集旨在让团队更快地交付代码。

这意味着我们没有专注于为开源存储库构建特性，我们的客户在与合作者共享构建结果方面有困难，甚至求助于使用不同的服务。长期以来，您一直要求我们为公共项目提供更好的支持，今天我们很高兴地宣布了这方面的一些功能。

## 细节

每个用户和 GitHub 组织总共有[四个免费的 linux 容器](https://circleci.com/docs/oss/#overview)(每年价值 2400 美元)用于开源项目。通过公开您的项目，这将自动为您启用。我们还为 macOS 开源项目免费提供 Seed 计划(1 倍并发)。要获得该计划，请联系 billing@circleci.com 的。这些项目现在也可以进行未经认证的 API 访问，因此徽章不再需要特定用途的 API 密钥。另一个对开源项目来说很棒的特性是用于公共项目的[公共构建页面](https://circleci.com/gh/circleci/circleci-docs)，这样你的合作者就不必登录来查看构建页面。对于启用了 GitHub 检查的组织中的项目，协作者还可以在 GitHub UI 中看到 CircleCI 作业和工作流的状态。阅读我们在 [GitHub Checks](https://circleci.com/blog/see-the-status-of-your-circleci-workflows-in-github/) 上的博文，了解更多关于使用这个特性的信息。

为了解决开源项目开发人员经常遇到的凭证和设置的具体问题，我们引入了大量的[特性和设置](https://circleci.com/docs/oss/#features-and-settings-for-open-source-projects):

*   **私有环境变量**允许您安全地存储公共项目的秘密，包括 API 令牌、SSH 密钥和密码。
*   **仅构建拉取请求**以减少 CircleCI 构建的数量。默认情况下，CircleCI 从每个分支构建每个提交，这对于开源项目来说往往过于激进。在项目的**高级设置**中打开和关闭该设置。
*   **从分叉的存储库构建 pull 请求**如果您允许从分叉的存储库获得 pull 请求，那么可以在任何手工评审之前运行测试。该选项可以在您的项目**高级设置**中启用。
*   **将秘密从分叉的拉请求传递到构建**如果你觉得[与任何分叉你的项目并打开拉请求的人共享秘密](https://circleci.com/blog/managing-secrets-when-you-have-pull-requests-from-outside-contributors/)。默认情况下，CircleCI 隐藏四种类型的配置数据:环境变量、部署密钥和用户密钥、添加到 CircleCI 的无密码私有 SSH 密钥以及 AWS 权限和配置文件。在项目的**高级设置**中打开和关闭该设置。

*注意:如果你还没有，你需要在**项目设置>高级设置>免费开源**中启用这个项目。从现在开始创建的所有公共项目都将自动进行此设置。*

## 现已上市

如果你已经是 CircleCI 的客户，你会看到开源版本和其他版本一样:它在同一个应用程序中，在同一个仪表板中列出，使用相同的 API。

我们的所有功能都面向开源客户:

*   我们的并行性帮助您的测试套件比构建时间增长得更快。
*   我们有一个很好的内置机制来保存和上传[构建工件](https://circleci.com/docs/artifacts/)。
*   当您的测试失败时，您可以 [SSH 到我们的构建中](https://circleci.com/docs/ssh-access-jobs/)来找出哪里出错了。:)
*   我们的基础设施会自动扩展，因此您不会因为其他人的 而经历任何繁忙时间或排队时间 ***。***

如果您正在构建一个更大的开源项目，并且需要更多的资源，请让我们知道如何帮助您！我们将重复这一点，所以在 CircleCI 上建立你的 OSS repos，让我们知道你的想法！