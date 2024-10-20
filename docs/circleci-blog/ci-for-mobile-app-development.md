# 面向移动应用开发的| CircleCI

> 原文：<https://circleci.com/blog/ci-for-mobile-app-development/>

测试是软件开发的关键部分。测试移动应用程序可能比测试服务器或基于网络的应用程序更困难，因为要检查应用程序的功能，应用程序必须在实际的 Android 或 iOS 设备上运行，或者在模拟器中运行。

幸运的是，通过使用[持续集成(CI)](/continuous-integration/) 工具自动化您的移动应用程序测试，这个过程可以变得更加容易、更加高效、更加一致。

让我们回顾一下如何设置移动应用程序进行测试，并探索如何使用 [CI 管道](/blog/what-is-a-ci-cd-pipeline/)自动化构建和测试过程。

## 如何创建带有测试的移动应用程序

Android Studio 和 Xcode (iOS)的所有新项目都支持创建测试就绪的移动应用程序。每个 IDE 都提供了一种内置的方式来添加和运行测试，更具体地说，就是添加单元测试。

对于 Android 应用程序，Android Studio 创建了一个包含新项目的目录(/src/test/java/),我们可以在其中编写测试作为 JUnit 4 测试类的实现。我们还需要在`build.gradle`文件中配置几个依赖项。你可以参考[这份谷歌指南](https://developer.android.com/training/testing/unit-testing/local-unit-tests)，了解更多关于如何创建一个带测试的安卓应用的详细说明。

对于 iOS 应用程序，Xcode 会在项目方案中包含一个测试目标，并在您创建新项目时为单元测试提供存根方法。我们可以在这个目标中编写测试，然后通过选择 Product > Test 来运行它们，或者通过选择 Product > Perform Action > Run Test Methods 来选择并运行它们的子集。看看苹果关于使用单元测试的指南[和关于使用 Xcode 测试的指南](https://developer.apple.com/library/archive/documentation/ToolsLanguages/Conceptual/Xcode_Overview/UnitTesting.html)和[以获得更多信息。](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/testing_with_xcode/chapters/01-introduction.html#//apple_ref/doc/uid/TP40014132-CH1-SW1)

一个移动应用 CI 解决方案将能够构建这些项目，并通过简单的配置运行其测试。

## 测试和自动化移动应用

看看[测试类型](https://circleci.com/blog/testing-methods-all-developers-should-know/)，测试一个移动应用程序发生在几个[隔离测试环境](https://circleci.com/blog/path-to-production-how-and-where-to-segregate-test-environments/):

*   **单元测试**——对我们部分代码的小规模测试，不需要在设备上运行，可以在构建机器上运行。
*   **集成测试** -可以与本地设备功能(如地理定位)交互的高级测试，需要在设备或模拟器上运行。
*   **功能和 UI 测试** -测试以确保应用程序运行和功能正常，并且必须在设备或模拟器上运行，如场景导航。
*   **性能测试** -测量应用性能指标的压力测试。

作为配置 CI 管道的一部分，我们可以自动化构建代码的过程，并对提交到存储库的每个新代码运行这些测试。CI 通常作为云上的服务器或本地机器上的服务器运行，它构建并运行配置的命令或任务。

使用单元测试，我们可以将 Android Studio 与 JUnit 一起使用，或者将 Xcode 与 XCTest 一起使用，或者，如果我们愿意，也可以使用其他标准的 Java 和 ObjC/Swift 测试框架，因为它们可以直接在构建机器上运行。

对于其他需要在设备或模拟器上运行的测试，像 [Appium](http://appium.io/) 这样的工具可以很容易地创建 UI 自动化之类的应用测试。

例如，通过将这些测试与 CircleCI 集成，我们可以将项目存储库添加到 CircleCI 应用程序，并设置一个`config.yml`文件来定义从存储库中提取最新代码的步骤，构建它，然后部署并运行移动应用程序和/或其测试，从而集成并自动化我们的构建和测试流程。

配置文件示例如下所示:

```
# .circleci/config.yml
version: 2.1
jobs:
  build-and-test:
    macos:
      xcode: 11.3.0
    environment:
      FL_OUTPUT_DIR: output
      FASTLANE_LANE: test
    steps:
      - checkout
      - run: bundle install
      - run:
          name: Fastlane
          command: bundle exec fastlane $FASTLANE_LANE
      - store_artifacts:
          path: output
      - store_test_results:
          path: output/scan 
```

我们鼓励在 CircleCI 上使用[浪子](https://fastlane.tools/),因为它简化了构建、测试和部署过程的设置和自动化，并且适用于 iOS 应用程序和 Android 应用程序。阅读[用浪子](https://circleci.com/blog/deploy-flutter-android/)构建和部署 Flutter 应用程序，了解更多关于使用 Fastalne 和 CIrcleCI 的细节。

并且通过使用 [CircleCI orbs](https://circleci.com/orbs/) ，特别容易上手移动测试自动化。我们可以添加 orb，它们是配置元素的可共享包，直接在 CI 管道中使用，而不是花费大量时间试图找出如何集成不同的服务并指定作业和命令。[宝珠目录](https://circleci.com/developer/orbs)给了我们很多选择。

## 移动应用安全问题

除了简化作为移动开发过程一部分的测试之外，CI 还有助于缓解关键的安全问题。

看看这些 CI 可以解决潜在安全问题的例子:

*   **凭证和认证** -在团队或组织中，将帐户密码和权限安全地限制给管理员是很重要的。运行敏感命令或任务(如使用私有证书进行代码签名或使用 API 密钥上传构建)将仅限于 CI 服务器。
*   **数据加密** -个人用户信息和密码需要小心处理，因此确保这些数据在设备和网络上始终加密的测试将有助于通过代码更改来保护我们的用户。
*   保持以前的错误和问题不被重新引入我们的代码的最好方法之一是创建一个自动测试来检查每个构建的场景。
*   **错误处理** -移动应用崩溃和内存缓冲区限制可能是安全漏洞的来源，或者至少是糟糕的用户体验。测试优雅的错误处理可以减轻这些问题。
*   **设备功能和权限** -测试可以帮助确保我们的应用程序正确使用本机设备功能，如麦克风或摄像头，并且仅在用户希望被记录的正确应用程序场景中使用，绝不会超出该上下文。这可能与应用程序获得用户保存的照片或文件的访问权限类似。

## 通过 CI 部署移动应用

最后，一旦我们的移动应用程序经过测试并准备好发布，CI 也可以通过将其直接发布到应用程序商店来提供帮助。

从高层次的角度来看，CI 任务可以获取最新版本的代码，增加版本号，构建项目源代码以供发布，然后将打包的应用程序直接上传到谷歌 Play 商店或苹果应用商店。

我们建议在这个场景中使用 Git 分支，并指定一个发布分支，这样我们就可以将它与开发分支分开，并且只有当代码从开发分支合并到发布分支时，才允许 CI 将构建上传到商店。

使用 CircleCI 和浪子可以轻松配置移动应用程序部署。对于 iOS 应用程序，我们可以提供对 App Store Connect 和浪子配置的访问，其中包含构建和上传应用程序发布版本的步骤。浪子还可以使用它的屏幕截图和 frameit 操作在元数据处理过程中生成屏幕截图。

按照 CircleCI 文档中的步骤，在 iOS 上配置移动应用部署[，在 Android](https://circleci.com/docs/deploying-ios/) 上配置移动应用部署[。](https://circleci.com/docs/language-android/#deploying-to-google-play-store)

## 移动开发入门

我们刚刚浏览了使用 CI 自动化我们的移动应用测试所涉及的步骤的高级视图，正如我们所看到的，这并不困难。以正确的方式开始移动应用项目是有意义的:从一开始就内置测试和 CI。

有了 CircleCI，自动化应用测试和部署变得很容易。更好的是，开始不需要任何成本，如果你的项目是开源的，你可以申请一个[特别计划](https://circleci.com/open-source/)，每月有免费的构建积分。

在我们结束之前，这里有一些有用的资源，可以帮助您开始 CircleCI 的自动化移动应用程序测试之旅:

祝您测试和自动化您的移动应用程序好运！