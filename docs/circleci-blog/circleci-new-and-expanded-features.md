# 新的和扩展的特性

> 原文：<https://circleci.com/blog/circleci-new-and-expanded-features/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

CircleCI 为 2016 年开了一个好头，我们与 CircleCI for OSX 和 [CircleCI Enterprise](/enterprise/) 推出并扩展了我们的产品。我们的团队在过去的这个季度自豪地发布了新产品，但我们也一直在努力发布新功能、升级和错误修复，这些都是我们的用户[引起我们注意的](https://discuss.circleci.com/)。

![New and Expanded Features](img/a89af32612b7460554b03c5332c5e925.png)

这个季度博客将花一些时间来强调我们引以为豪的功能。如果你等不及再等三个月的功能回顾，请在我们的[变更日志](/changelog/)上实时更新。如果你有功能请求或者想要报告一个错误，你可以在[讨论](https://discuss.circleci.com/)上这样做。我们很高兴收到您的来信！

## 这是最新消息

**一级造影机支架**

自从我们的标记构建 API 发布以来，我们现在能够支持 Phabricator 一级。要了解更多关于如何用 Circle 设置 Phabricator 的信息，你可以[点击这里](/docs/1.0/phabricator/)。这个特性请求部分是由 Phabricator 集成在我们的[讨论论坛](https://discuss.circleci.com/t/compatibility-with-phabricator/183/17)上的受欢迎程度驱动的。

**选择“执行环境”的能力**

现在你可以选择使用哪个 Ubuntu 映像来构建你的项目。我们在“项目设置”下添加了一个名为“执行环境”的新部分。您可以在 Ubuntu 12.04 或 Ubuntu 14.04 图像之间进行选择。如果您正在构建 OS X 项目，您可以在“执行环境”设置页面上的“构建 OS X 项目”部分下启用它。

**每个项目的见解**

您现在可以查看每个回购构建状态和构建性能。点击了解更多信息[。](/blog/announcing-circleci-per-project-insights/)

**API 检索特定分支的最新构建工件**

我们添加了功能来检索给定项目分支上最近完成的、成功的或失败的构建工件。点击了解更多信息[。这也部分是因为它在我们的](/docs/1.0/api/#build-artifacts/)[讨论论坛](https://discuss.circleci.com/t/referencing-latest-build-artifacts/931)上的受欢迎程度。

**标签构建 API**

我们现在支持带注释的和轻量级的标签。现在可以在 POST 请求的 JSON 主体中提供“tag”作为键。点击阅读更多[。](/docs/1.0/api/#new-build/)

**标记构建现在支持构建参数**

现在，您甚至可以对标记的构建使用 build_parameters。这使得它与其他构建触发 API 保持一致。点击阅读更多[。](/docs/1.0/api/#new-build/)

**支持[跳过配置项]或[跳过配置项]**

您现在可以使用“[skip ci]”和“[ci skip]”，因为我们已经开始支持这两种功能。点击阅读更多[。](/docs/1.0/skip-a-build/)

**Python 符号链接行为改变**

我们已经更新了我们对 Python 虚拟环境的处理。之前，我们在您的项目根目录中创建了一个指向系统“venv”目录的符号链接。对于 2016 年 3 月 11 日之后创建的项目，我们将不再这样做。如果您需要覆盖每个项目或每个组织的新默认值，请发电子邮件至 support@circleci.com联系我们。

**仅管理员权限**

我们添加了逻辑来限制任何非管理 Github 用户修改项目设置。您可以为您的组织或组织内的特定项目启用此功能。GitHub 成员仍然可以使用诸如重建、通过 SSH 重建和无缓存重建等功能。如果你想启用这个功能，请联系 support@circleci.com。

**新的环境变量**

我们添加了两个新的环境变量，`CIRCLE_REPOSITORY_URL`和`CIRCLE_BUILD_URL`。这些变量在您的构建中是可用的，并且可以轻松访问您在 GitHub 上的 repo 的 URL 和在 CircleCI 上的构建的 URL。这对于向自动化部署添加元数据非常方便。你可以在我们的文档的[环境变量一节中读到更多关于我们的环境变量的内容。](/docs/1.0/environment-variables/#build-details/)

**测试摘要现在显示 Junit 格式 XML 解析错误**

我们现在在“Test Summary”选项卡下显示 JUnit 格式的 XML 输出解析错误。如果构建生成了一个无效的 Junit 格式的 XML，我们将显示一个带有文件名和错误发生的行号的警告。

**浏览器无响应问题**

我们已经修复了许多导致构建页面时浏览器无响应的问题。我们目前正在显示控制台输出的前 400，000 行，并为您提供下载完整日志的能力。

**Maven 依赖缓存**

Maven 构建现在在依赖阶段使用`dependency:go-offline`而不是`dependency:resolve`，所以它们应该在运行测试之前将所有依赖保存到缓存中

我们希望你喜欢 CircleCI 的所有新功能！如果有你想看的功能，在[上添加一个请求，讨论](https://discuss.circleci.com/c/feature-requests)，如果你想加入我们的测试程序，内圈，你将在我们的最新功能公开之前尝试它们。如果您有任何问题要问我们，我们希望在 support@circleci.com[听到您的回答。](mailto:support@circleci.com)