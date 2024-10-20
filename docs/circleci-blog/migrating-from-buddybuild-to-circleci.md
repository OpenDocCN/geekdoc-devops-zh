# 从 buddybuild 迁移到 CircleCI - CircleCI

> 原文：<https://circleci.com/blog/migrating-from-buddybuild-to-circleci/>

在被苹果收购后，昨天 buddybuild 宣布他们将停止为 Android 版本和免费 iOS 计划提供服务。祝贺 buddybuild 团队，尽管这个消息让一些团队为他们的移动构建寻找一个替代的 CI 系统。我们将这种比较放在一起，以帮助团队决定 CircleCI 是否是适合他们的工具。

## CircleCI 和 buddybuild 如何比较？

|  | 绕圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈圈 | buddybuild |
| 移动支持(macOS、Android) | ✅ | ✅ |
| Non-mobile project support | ✅ | ❌ |
| 为 macOS 和 Android 构建隔离 | ✅ | ✅ |
| 支持开源项目 | ✅ | ✅ |
| 自动上传到 iTunes Connect | ✅ | ✅ |
| 在同一工作流程中运行 macOS 和 Linux 作业 | ✅ | ❌ |
| 工作流的手动批准步骤 | ✅ | ❌ |
| UI 中的配置 | ❌ | ✅ |
| 没有 GitHub / Bitbucket 访问权限的合作者 | ❌ | ✅ |

## CircleCI 和 buddybuild 的主要区别是什么？

### git 中存储的配置文件

buddybuild 的配置完全通过他们的 web 用户界面完成，而 CircleCI 将配置存储在 git 存储库中的配置文件中。将配置作为文件存储在存储库中的好处是:

*   更好的审计跟踪。查看谁对您的配置进行了更改，并在 GitHub 和 Bitbucket 上并排比较新旧配置。
*   轻松恢复以前的配置版本。既然配置存储在 git 中，就很容易恢复以前版本的配置文件。
*   将配置复制到新的存储库。如果您有多个存储库需要配置，您可以在它们之间复制配置文件，并在每个存储库中调整细节。

当您注册 CircleCI 时，我们会为您提供一个配置文件模板，您可以在您的项目中使用它。我们在本文档中提供了当前最佳实践配置以供参考[。](https://circleci.com/docs/testing-ios/#basic-setup)

你可以在 CircleCI [这里](https://github.com/CircleCI-Public/circleci-demo-ios)找到一个使用 fastlane 的示例 iOS 应用程序。

### 使用 fastlane 进行代码签名、测试和部署

buddybuild 为代码签名、运行测试命令和部署应用程序提供了内置机制。CircleCI 不提供任何内置机制，相反我们依赖于使用 [fastlane](https://fastlane.tools/) ，这是一个用于构建、测试和发布 iOS 和 Android 应用的开源工具。

我们选择依赖 fastlane，因为 fastlane 和相关工具背后的社区在跟踪苹果和其他供应商实现的改进和变化方面做得很好。这确保您在与 Apple 构建工具和第三方服务进行交互以进行部署时，能够获得最一致的构建体验。

我们建议使用 fastlane 运行您的测试，导出您的应用程序，并部署到 TestFlight(以及其他测试服务)和 iTunes Connect。请[查看此文档部分，了解在 CircleCI](https://circleci.com/docs/testing-ios/#using-fastlane) 上使用 fastlane 的示例。你可以在这里找到关于通过快速通道匹配[设置代码签名的说明。你可以在这里](https://circleci.com/docs/ios-codesigning/)找到更详细的快车道文件[。](https://docs.fastlane.tools/)

### 相同配置的 iOS 和 Android

使用 CircleCI 工作流，可以在一个配置文件中包含 iOS 和 Android 配置。这在构建 React 原生项目时尤其有用。

使用像 Danger 和 swiftlint 这样的 iOS 开发工具的项目也可以利用这一点:例如，如果 swiftlint 没有通过，您可以提前使工作流失败，或者并行运行 iOS 测试和 Danger。

您将在这里找到一个在工作流[中将 swiftlint 和 Danger 作为单独作业运行的示例。请看一下在 CircleCI](https://circleci.com/docs/testing-ios/#sample-configuration-with-multiple-executor-types-macos--docker) 上构建 iOS 和 Android 应用的[示例 React Native 项目。](https://github.com/CircleCI-Public/circleci-demo-react-native)[工作流程文档](https://circleci.com/docs/workflows/)显示了所有可用的工作流程选项。

### 试试 CircleCI

[阅读我们的 iOS 文档以了解更多信息](https://circleci.com/docs/testing-ios/)或查看我们的[circle ci 示例 iOS 项目](https://github.com/CircleCI-Public/circleci-demo-ios)。准备好开始建造了吗？[报名并立即开始](https://circleci.com/signup/)。