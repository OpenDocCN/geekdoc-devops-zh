# 在 CI/CD 管道中构建 Kotlin 多平台项目

> 原文：<https://circleci.com/blog/building-kmm-on-cicd/>

Kotlin 是最通用的编程语言之一，这在很大程度上是因为 Kotlin 团队致力于将其引入尽可能多的平台。它是开发 Android 应用程序的主要语言，在 JVM 后端很流行。Kotlin 还提供了使用 Kotlin/Native 进行本地二进制编译的目标，以及通过 Kotlin/JS 进行 web 编译的目标。它最有前途的特性之一是能够针对多个平台进行编译。

这就是 [Kotlin 多平台移动(KMM)](https://kotlinlang.org/docs/mobile/getting-started.html) 的用武之地:它让你可以在 Android 和 iOS 应用中编写和重用相同的 Kotlin 代码。由于移动客户端通常以功能对等为目标，这是一种避免重复工作的聪明方法，我们都知道开发人员多么喜欢避免重复工作。相信我，我是一名开发人员，我知道这有多真实。

无论如何，回到科特林多平台。这是一种不同于跨平台工具的方法，如 Flutter 和 React Native，它们在一切之上提供统一的 UI。KMM 允许您编写通用业务逻辑，同时将用户界面和体验牢牢地保留在他们的本地平台领域中。在许多情况下，这种方法更适合应用程序用户，因此也更适合您的客户。

本文将向您展示如何开始在 [CI/CD 管道](https://circleci.com/blog/what-is-a-ci-cd-pipeline/)中构建 KMM 项目，并将其包含在您团队的开发工作流中。

## 跟随注意事项

本文假设读者理解 Kotlin，并且至少熟悉 Android 或 iOS 中的一个，最好是两个平台都熟悉。这篇文章不会教你 KMM。为此，请查看 Kotlin 站点上的现有教程之一，或者示例应用程序的自述文件。

另外请注意:KMM 目前是在阿尔法版本。技术和 API 在不断变化和发展，我们正在尽最大努力使示例保持工作和最新，但除了示例中的 API 版本之外，我们不提供任何保证。

## 在 CI/CD 管道中构建多平台项目

要在 CI/CD 环境中构建一个多平台项目，可以把它看作是为每个单独的目标平台构建不同的项目。我们的项目基于[科特林自己的多平台样本——一个 RSS 阅读器应用](https://github.com/Kotlin/kmm-production-sample)。它包含两个应用程序，以及一个以 Kotlin 库形式存在的共享 Kotlin 代码库。共享库保留在 Kotlin 中，用于在 Android 上编译，并向下编译为本机代码，以便在 ARM64 上运行，用于 iOS 目标。

Android 应用程序代码位于`androidApp`，用熟悉的 Kotlin 代码库编写。iOS 的 app 是用 Swift 写的，位于`iosApp`。要了解 KMM 是如何让一切工作的，请查看 KMM 官方文档[文档](https://kotlinlang.org/docs/mobile/home.html)和 Kotlin 的 GitHub repo 上的[样本项目](https://github.com/Kotlin/kmm-production-sample)。

## 建立一个基本的管道

用于 KMM 项目的最简单的 CI/CD 管道在单个工作流中包含两个作业，分别执行每个平台的构建。CircleCI 中的作业是一系列命令或步骤，它们在预定的环境中执行。这种环境是为不同平台构建应用程序的关键。对于 iOS，我们需要在 Mac 硬件上构建它，为 executor 传入`macos`。Android 更加宽松，但我们仍然可以选择一个包含所有 SDK 和其他内置内容的预构建环境。对于本教程，我们将使用由`android` orb 提供的`android` Docker 图像。

这些工作本身很简单，与本教程并不相关:检查代码，编译，可能运行一些测试，构建，甚至部署。作为本指南的一部分，我们将不关注工作。相反，我们将花一些时间为构建设置环境和工作流。如果你想了解更多关于为 Android 设置构建和测试工作的信息，你可以在[这篇文章](https://circleci.com/blog/building-android-on-circleci/)中读到。如果你想了解更多关于在 iOS 上设置构建和测试的信息，你可以在 CircleCI 文档中的[本教程中了解更多。](https://circleci.com/docs/ios-tutorial/)

```
version: 2.1

orbs:
  android: circleci/android@1.0.3

jobs:
  build-android:
    executor: android/android

    steps:
      - checkout
      - android/restore-build-cache
      - android/restore-gradle-cache
      - run:
          name: Run Android tests
          command: ./gradlew androidApp:testDebugUnitTest
      - android/save-gradle-cache
      - android/save-build-cache

  build-ios:
    macos:
      xcode: 12.4.0
    steps:
      - checkout
      - run:
          name: Allow proper XCode dependency resolution
          command: |
            sudo defaults write com.apple.dt.Xcode IDEPackageSupportUseBuiltinSCM YES
            rm ~/.ssh/id_rsa || true
            for ip in $(dig @8.8.8.8 bitbucket.org +short); do ssh-keyscan bitbucket.org,$ip; ssh-keyscan $ip; done 2>/dev/null >> ~/.ssh/known_hosts || true
            for ip in $(dig @8.8.8.8 github.com +short); do ssh-keyscan github.com,$ip; ssh-keyscan $ip; done 2>/dev/null >> ~/.ssh/known_hosts || true
      - run:
          name: Install Gem dependencies
          command: |
            cd iosApp
            bundle install
      - run:
          name: Fastlane Tests
          command: |
            cd iosApp
            fastlane scan

workflows:
  build-all:
    jobs:
      - build-android
      - build-ios 
```

### 构建 Android 应用程序

开始构建 Android 应用程序的最佳方式是使用 Android orb。这为您提供了一些内置的作业和命令，使构建变得更加容易。对于这个练习，我们将安装 Gradle 依赖项，并运行 Gradle 命令来运行单元测试。

Kotlin Multiplatform 带来的主要区别是该应用程序不是顶级项目。相反，它位于`androidApp`模块中。这个模块依赖于`shared`，所以我们需要调用带有`androidApp:`前缀的 Gradle 命令。

```
orbs:
  android: circleci/android@1.0.3

jobs:
  build-android:
    executor: android/android

    steps:
      - checkout
      - android/restore-build-cache
      - android/restore-gradle-cache
      - run:
          name: Run Android tests
          command: ./gradlew androidApp:testDebugUnitTest
      - android/save-gradle-cache
      - android/save-build-cache

      - store_artifacts:
          path: androidApp/build/outputs/apk/debug 
```

### 构建 iOS 应用程序

正如我们上面提到的，构建 iOS 应用程序需要安装了 XCode 的 Mac 硬件。对于`build-ios`作业，我们将使用`macos`执行器，将 xcode 版本作为参数传递。在我们的情况下是:`xcode: 12.4.0`。

不管您使用的是 CocoaPods 还是 Swift Package Manager，其余步骤都是通用的。您通常会安装一些依赖项，然后您可以使用浪子构建应用程序并运行测试。对于本教程，我们将只运行一些测试。

当我们使用浪子启动构建时，共享的 Kotlin 代码会自动构建为一个框架，所以这里没有什么要做的了。

```
jobs:
  ...
 build-ios:
    macos:
      xcode: 12.4.0
    steps:
      - checkout
      - run:
          name: Allow proper XCode dependency resolution
          command: |
            sudo defaults write com.apple.dt.Xcode IDEPackageSupportUseBuiltinSCM YES
            rm ~/.ssh/id_rsa || true
            for ip in $(dig @8.8.8.8 bitbucket.org +short); do ssh-keyscan bitbucket.org,$ip; ssh-keyscan $ip; done 2>/dev/null >> ~/.ssh/known_hosts || true
            for ip in $(dig @8.8.8.8 github.com +short); do ssh-keyscan github.com,$ip; ssh-keyscan $ip; done 2>/dev/null >> ~/.ssh/known_hosts || true
      - run:
          name: Install Gem dependencies
          command: |
            cd iosApp
            bundle install
      - run:
          name: Fastlane Tests
          command: |
            cd iosApp
            fastlane scan 
```

## 使用动态配置创建高级管道

每当有人提交到存储库时，这个示例管道可以很好地构建和测试您的 KMM 应用程序的两个目标平台。但是我们可以做得更好。移动开发的现实是，我们有专家专注于他们各自的平台。有些人喜欢并使用 Android，喜欢为 Android 制作伟大的 UX 和体验，对这个平台了如指掌。另一方面，也有同样专注于 iOS 的开发者。在公共代码库上工作总会有一些交叉。在大多数团队中，仍然会有专门从事某个平台的人。

开发进度通常以以下一种形式出现:

*   仅限 Android 前端代码库
*   仅限 iOS 前端代码库
*   由两个前端代码库使用的共享 KMM 代码库

对于前两个场景，只为正在工作的平台构建更有意义，并且节省时间、信用和效率。

为了优化这个流程，我们可以使用 CircleCI 的动态配置特性，这有助于我们更有效地编排这样的项目。动态配置允许您只构建应用程序中您已经更改的部分。

动态配置通过设置工作流工作，第一步是评估代码库，检测发生了什么变化(稍后将详细介绍)，然后才运行*真正的*工作流，即构建应用程序相关部分的工作流。

设置工作流程仍然使用 CircleCI 用户可能熟悉的`config.yml`。执行的工作流使用路径过滤 orb，它检测与指定分支相比的变化。这是完整的配置。

```
version: 2.1

setup: true

orbs:
  # the path-filtering orb is required to continue a pipeline based on
  # the path of an updated fileset
  path-filtering: circleci/path-filtering@0.0.2

workflows:
  select-for-build:
    jobs:
      - path-filtering/filter:
          base-revision: master
          config-path: .circleci/continue-config.yml
          mapping: |
            shared/.*|^(?!shared/.*|iosApp/.*|androidApp/.*).*  build-all     true
            androidApp/.*                                       build-android true
            iosApp/.*                                           build-ios     true 
```

以下是该配置的明细:

`setup:true`向 CircleCI 表示我们正在使用动态配置，这是管道运行的第一部分。设置工作流是此配置的唯一工作流。它只有一个作业`path-filtering/filter`，来自`path-filtering` orb。该作业需要几个参数:

*   `base-revision`表示要比较的分支
*   `mapping`决定比较哪些路径(稍后将详细介绍)
*   `config-path`指向`continue-config.yml`，这是该设置工作流程后要评估的下一个配置。在我们的例子中，我们指向一个静态文件，但是我们也可以预先以编程方式构造这个新的配置文件。

要使用`mapping`，您需要定义项目的路径，以便与基础版本中的代码库进行比较。每行有一条路径。然后，设置要传递给后续管道的管道参数及其值。所有这些都用空格隔开。

第 2 行和第 3 行相当简单:

```
androidApp/.* build-android  true 
```

在这种情况下，我们感兴趣的路径是`androidApp/`目录中的所有文件和子目录。这就是 Android 专用前端的代码库。流水线参数`build-android`被设置为`true`。

然而，第一个匹配行更复杂，因为正则表达式匹配器更长:

```
shared/.*|^(?!shared/.*|iosApp/.*|androidApp/.*).*  build-all  true 
```

它匹配两个目录中的路径，即我们的 Kotlin 多平台代码所在的目录，以及不在`shared/`、`iosApp/`或`androidApp/`目录中的任何内容，包括任何其他顶级文件。我们已经将管道参数`build-all`设置为`true`，这将触发两个平台的应用构建。

如`continue-config.yml`中所述，在映射评估了所有更改并设置了所有相关的管道参数后，CircleCI 停止该作业，并开始该动态工作流的第二部分。这是完整的文件。

```
version: 2.1

orbs:
  android: circleci/android@1.0.3

parameters:
  build-all:
    type: boolean
    default: false
  build-android:
    type: boolean
    default: false
  build-ios:
    type: boolean
    default: false

jobs:
  build-android:
    executor: android/android

    steps:
      - checkout
      - android/restore-build-cache
      - android/restore-gradle-cache
      - run:
          name: Build Android app
          command: ./gradlew androidApp:assembleDebug
      - android/save-gradle-cache
      - android/save-build-cache

  build-ios:
    macos:
      xcode: 12.4.0
    steps:
      - checkout
      - run:
          name: Allow proper XCode dependency resolution
          command: |
            sudo defaults write com.apple.dt.Xcode IDEPackageSupportUseBuiltinSCM YES
            rm ~/.ssh/id_rsa || true
            for ip in $(dig @8.8.8.8 bitbucket.org +short); do ssh-keyscan bitbucket.org,$ip; ssh-keyscan $ip; done 2>/dev/null >> ~/.ssh/known_hosts || true
            for ip in $(dig @8.8.8.8 github.com +short); do ssh-keyscan github.com,$ip; ssh-keyscan $ip; done 2>/dev/null >> ~/.ssh/known_hosts || true
      - run:
          name: Install Gem dependencies
          command: |
            cd iosApp
            bundle install
      - run:
          name: Fastlane Tests
          command: |
            cd iosApp
            fastlane scan

workflows:
  run-android:
    when:
      or:
        - << pipeline.parameters.build-android >>
        - << pipeline.parameters.build-all >>
    jobs:
      - build-android

  run-ios:
    when:
      or:
        - << pipeline.parameters.build-ios >>
        - << pipeline.parameters.build-all >>
    jobs:
      - build-ios 
```

这里没有包含`setup:true`行，这表明这是一个“标准”CircleCI 配置文件。它确实包含了带有我们在前一阶段定义的管道参数的`parameters`部分。

```
parameters:
  build-all:
    type: boolean
    default: false
  build-android:
    type: boolean
    default: false
  build-ios:
    type: boolean
    default: false 
```

要使用管道参数，我们首先需要在管道中定义它们。我们之前用过。现在您指定它们的类型和默认值:all `boolean`并设置为`false`。

```
workflows:
  run-android:
    when:
      or:
        - << pipeline.parameters.build-android >>
        - << pipeline.parameters.build-all >>
    jobs:
      - build-android

  run-ios:
    when:
      or:
        - << pipeline.parameters.build-ios >>
        - << pipeline.parameters.build-all >>
    jobs:
      - build-ios 
```

一旦定义了参数，我们就可以在过滤工作流时使用它们。与上一节所示的简单得多的配置不同，我们需要将作业划分到两个工作流中:

*   Android 的工作被命名为`run-android`
*   iOS 的作业名为`run-ios`

我们可以使用与逻辑操作符结合的`when`节来指定何时运行特定工作流的过滤器。对于 Android，它是一个`build-all`或`build-android`管道参数。对于 iOS，它是一个`build-ios`或`build-all`。

此工作流根据运行与工作流相关的作业的已更改文件有选择地运行。这种设置让您的团队更快、更有效地运作，同时仍然利用 KMM 的所有多平台功能。

# 结论

在本教程中，您已经学习了如何在 CircleCI 上建立构建 Kotlin 多平台移动项目的管道。它类似于构建任何其他项目，除了知道首先构建哪个平台。移动平台通常是单独开发的，因此我们使用 CircleCI 动态配置功能来构建应用程序开发人员当前正在开发的部分。

如果你对这篇文章有任何问题或建议，或者对未来的文章和指南有什么想法，请通过 [Twitter - @zmarkan](https://twitter.com/zmarkan) 或 [email 给我](mailto:zan@circleci.com)联系我。