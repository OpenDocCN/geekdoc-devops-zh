# Android 项目的持续集成| CircleCI

> 原文：<https://circleci.com/blog/building-android-on-circleci/>

CircleCI 在 Android 开发人员中很受欢迎，原因有几个:它可以快速入门，快速执行具有高并行性的构建，(无论是本机、跨平台还是多平台)，甚至支持使用我们的 Android 机器映像直接从 CircleCI 运行 Android 模拟器。

本文将向您展示如何在 CircleCI 平台上为一个示例项目构建和测试 Android 应用程序。完整的源代码可以在 GitHub-CircleCI-Public/Android-testing-circle ci-examples 上找到[，你可以在相应的 circle ci 项目](https://github.com/CircleCI-Public/android-testing-circleci-examples)中找到工作管道[。](https://app.circleci.com/pipelines/github/CircleCI-Public/android-testing-circleci-examples)

为了从本文中获得最大收益，您应该

*   Android 开发的工作知识
*   熟悉 Android 工具和测试框架
*   能够使用 Gradle 构建系统

## CircleCI 为 Android 提供了什么

我们为您提供了一些构建 Android 的工具:

*   Docker 图像
*   Android 机器图像
*   安卓 orb

### 图像对接器

Docker 图像是团队使用的主要执行者。它运行在 Docker 容器中，因此启动非常快，并且包含了开始构建 Android 项目所需的一切。预装工具的完整列表可以在文档中找到[。这些图片还附带了预装 Node.js、Android NDK 和浏览器工具的变体。](https://circleci.com/developer/images/image/cimg/android)

### 机器图像

变化最大的是[新安卓机镜像](https://circleci.com/developer/machine/image/android)。这是一个虚拟机映像，所以比上面提到的 Docker 映像花费的时间多一点，但它包含了构建 Android 应用程序可能需要的所有工具，包括 SDK、Google Cloud tools for Firebase、 [Python](/features/python/) 、Node。JS 和浪子。然而最大的变化是对嵌套虚拟化的支持，它允许你在这个机器映像中运行 Android 模拟器。

Android 机器映像支持嵌套虚拟化，这允许在 CircleCI 作业中运行 x86 模拟器。如果你从平台早期就开始为 Android 开发，你可能记得模拟器非常慢。然后 x86 模拟器开始支持英特尔的 Hyper-V 虚拟机管理程序，这让模拟器直接使用主机的资源。像这样直接使用资源使得模拟器能够运行得更加流畅和快速。

很长一段时间，支持仅限于我们的本地硬件。对 CircleCI 的有限支持降低了模拟器的速度，并使其难以运行 UI 测试。嵌套虚拟化解决了这一问题，它为可大规模复制的 CI/CD 环境增加了对快速仿真的支持，使所有的好处都变得可用。

### 安卓 Orb

最后是 CircleCI Android orb，它提供了作为执行者对两个映像的轻松访问，以及安装 SDK、运行模拟器、缓存、测试等方便的任务和命令。你可以在 orb 文档[的链接](https://circleci.com/developer/orbs/orb/circleci/android)中阅读更多关于 orb 的内容。

## 介绍 Android CI 演示项目

我已经创建了一个[演示项目](https://github.com/zmarkan/android-testing-circleci-examples)，这是谷歌自己的 Android 测试 Codelab 源代码的一个分支。Codelab 是一个简单的待办事项列表管理器应用程序，包含单元测试和工具测试的组合，有和没有 UI。该项目还可以在 CircleCI 上观看[。如果在 Android Studio 中打开，一定要切换到`Project`视图。项目视图允许您查看我们将要查看的`.circleci/config.yml`文件。](https://app.circleci.com/pipelines/github/zmarkan/android-testing-circleci-examples)

该项目有一个单独的`app`模块，带有默认的`debug`和`release`变量。`test`目录包含单元测试，而`androidTest`目录包含仪器测试。包括了完整的 CircleCI 配置，我将在本文中引用它。配置位于`.circleci/config.yml`。

## 为示例 Android 应用程序构建和测试 CI

我们的 CI 流程必须做到三点:

1.  在虚拟机中运行单元测试
2.  在模拟器上运行检测测试
3.  构建应用程序的发布版本

我们将同时运行单元测试和仪器测试。如果两种类型的测试都成功了，我们就可以组装发布版本了。我们还将添加一个条件，如果新的工作已经提交到“主”分支，并且只有在从 23 到 30 的每个 Android 版本上成功的模拟器测试之后，才继续发布版本*。当我们的构建满足这个条件时，我们将有坚实的保证，我们的应用程序将在许多不同的 Android 设备上工作。*

 ***注意:** *正如我之前提到的，从这一点开始的一切都将引用`.circleci/config.yml`中的 CircleCI 配置文件。*

### 使用 CircleCI Android orb

创建我们的 [CI 管道](https://circleci.com/blog/what-is-a-ci-cd-pipeline/)的第一步是使用 Android orb，它包含许多已经为您预先编写好的步骤。最新版本可在 [orb 文档](https://circleci.com/developer/orbs/orb/circleci/android)中获得。

定义 orb 后，脚本指定这些作业:

*   `unit-test`
*   `android-test`
*   `release-build`

还包括一个工作流程`test-and-build`。

```
version: 2.1

orbs:
  android: "2.1.2"

jobs:
  unit-test:
    ...
  android-test:
    ...
  release-build:
    ...
    # We'll get to this later

workflows:
  test-and-build:
    ...
  # We'll get to this later 
```

### 运行单元测试

我们的`unit-test`作业包含一个来自 Android orb 的执行程序:`android/android-docker`。如上所述，它在 Docker 容器中运行，启动速度极快。

在第`steps`节中，有一些事情需要注意。首先，我们运行`android/restore-gradle-cache` Gradle cache 帮助存储我们的依赖项并构建工件。在运行测试命令之后，我们还运行`android/save-gradle-cache`和`android/save-build-cache`来确保后续的构建通过使用缓存运行得更快。这些都来自 Android orb，如前缀`android/`所示。

我们还可以通过将`find-args`参数传递给`restore-gradle-cache`来修改缓存的内容。在这个开源项目中可以找到一个很好的例子[。](https://github.com/owntracks/android/blob/59f13afa93a27904e739493d96af64ab36f783eb/.circleci/continue-config.yml#L17-L44)

主要的构建步骤发生在`android/run-tests`命令中，我们在`test-command`参数中调用`./gradlew testDebug`。这个命令运行`debug`构建变体的所有单元测试。另外，这个命令带有一个默认的重试值，可以帮助您避免测试失败。

```
jobs:
  unit-test:
    executor:
      name: android/android-docker
      tag: 2022.08.1
    steps:
      - checkout
      - android/restore-gradle-cache
      - android/run-tests:
          test-command: ./gradlew testDebug
      - android/save-gradle-cache
      ... 
```

### 在模拟器上运行 Android 测试

我们的工作是使用新的 Android 模拟器功能。该作业使用与`build`作业相同的 Android 机器执行器。所有后续作业都将使用同一个执行器，缓存恢复步骤也是一样的。

模拟器要求我们在机器映像上运行，所以我们指定`android/android-machine`作为执行器。为了缩短构建时间，我们将`resource-class`指定为 xlarge。您也可以在`large`执行器上运行它，但是我发现资源的缺乏会导致构建时间变慢。我建议你自己做一些实验，为你的项目找到合适的执行者。

下一步更短；我们只需要`android/start-emulator-and-run-tests`命令。这个命令确实做到了它所说的:它启动指定的模拟器并运行测试。我们再次传入`test-command`作为参数(它使用了`android/run-tests`命令)。`system-image`是完全合格的 Android 模拟器系统映像。我们目前将我们的 Android 机器映像捆绑回 SDK 版本 23。你可以使用`sdkmanager`工具安装更多。

```
jobs:
  android-test:
    executor:
      name: android/android-machine
      resource-class: xlarge
    steps:
      - checkout
      - android/start-emulator-and-run-tests:
          test-command: ./gradlew connectedDebugAndroidTest
          system-image: system-images;android-30;google_apis;x86
      ... 
```

您可以使用 Android orb 中指定的命令手工编写所有模拟器设置步骤的代码:`create-avd`、`start-emulator`、`wait-for-emulator`和`run-tests`。

### 存储测试结果

测试不仅仅是运行测试，因为我们可以从 CircleCI 仪表板中准确地看到什么失败了，从而获得很多价值。为此，我们可以使用 CircleCI 平台内置的`store_test_results`函数，它将显示我们通过(或失败)的构建。

单元测试和仪器测试的步骤略有不同，因为它们各自的 Gradle 任务将测试存储在不同的位置。对于单元测试，测试将在`app/build/test-results`中进行，对于仪器测试，测试将在`app/build/outputs/androidTest-results`中进行。

我们建议您创建一个新的`Save test results`步骤，它使用`find`工具，将所有测试结果文件复制到一个公共位置——比如`test-results/junit`,然后从那里存储它。还要确保将`when: always`参数添加到步骤中，这样无论上面的测试是成功还是失败，该步骤都会运行。

### 存储单元测试

对于这个项目，我们希望基于 XML 的结果。调用`store_artifacts`使得这些结果可以在 CircleCI 平台上解析。如果您愿意，也可以存储 HTML 输出。

```
...
- android/run-tests:
  test-command: ./gradlew testDebug
- run:
    name: Save test results
    command: |
        mkdir -p ~/test-results/junit/
        find . -type f -regex ".*/build/test-results/.*xml" -exec cp {} ~/test-results/junit/ \;
    when: always
- store_test_results:
    path: ~/test-results
- store_artifacts:
    path: ~/test-results/junit 
```

### 存储仪器测试

不同测试类型有不同的 regex 命令。存储测试结果的其余步骤与单元测试示例相同。

```
- android/start-emulator-and-run-tests:
        test-command: ./gradlew connectedDebugAndroidTest
        system-image: << parameters.system-image >>
    - run:
        name: Save test results
        command: |
          mkdir -p ~/test-results/junit/
          find . -type f -regex ".*/build/outputs/androidTest-results/.*xml" -exec cp {} ~/test-results/junit/ \;
        when: always
    - store_test_results:
        path: ~/test-results
    - store_artifacts:
        path: ~/test-results/junit 
```

### 制作发布版本并存储工件

对于这个项目，我们需要存储我们正在构建的应用程序。为了完成流程的这一部分，我们使用了`release-build`作业，它有两个需要注意的部分。一个`run`步骤调用`./gradlew assembleRelease`。然后，`store_artifacts`指向建成的 APK 所在的位置。对于一个完整 CI/CD 项目，您可以签署并上传 APK 到一个测试版发行服务。你甚至可以和浪子一起在 Play Store 上发布。但是，这两种活动都超出了本文的范围。

```
jobs:
  ...
  release-build:
    executor:
      name: android/android-machine
      resource-class: xlarge
    steps:
      - checkout
      - android/restore-gradle-cache
      - android/restore-build-cache
      - run:
          name: Assemble release build
          command: |
            ./gradlew assembleRelease
      - store_artifacts:
          path: app/build/outputs/apk/release 
```

### 同时在多个 Android 版本上运行测试

Android 平台一直在发展，每个版本都有自己的缺陷和独特之处。我相信你已经遇到了其中的一些。避免不同版本 Android 之间不确定行为的一个好方法是尽可能多的测试它们。CircleCI 作业矩阵功能使在多个版本上运行作业变得更加容易。

要使用职务矩阵，请向要在多个版本上测试的职务添加一个参数。我们的`android-test`作业使用`system-image`参数来完成这项工作。我们还为参数指定了一个默认值，因此您可以在没有它的情况下运行作业。

要使用该参数，通过将它放在成对的尖括号- `<< >>`中来指定需要它的值的位置:

```
 android-test:
    parameters: # this is a parameter
      system-image:
        type: string
        default: system-images;android-30;google_apis;x86
    executor:
      name: android/android-machine
      resource-class: xlarge
    steps:
      - checkout
      - android/start-emulator-and-run-tests:
          test-command: ./gradlew connectedDebugAndroidTest
          system-image: << parameters.system-image >> # parameter being used 
```

要将值传递给工作流中的参数，请使用我们正在调用的作业的`matrix`参数。

```
workflows:
  jobs:
  ...
  - android-test:
      matrix:
        alias: android-test-all
        parameters:
          system-image:
            - system-images;android-30;google_apis;x86
            - system-images;android-29;google_apis;x86
            - system-images;android-28;google_apis;x86
            - system-images;android-27;google_apis;x86
            - system-images;android-26;google_apis;x86
            - system-images;android-26;google_apis;x86
            - system-images;android-26;google_apis;x86
            - system-images;android-26;google_apis;x86
      name: android-test-<<matrix.system-image>> 
```

### 通过缓存使每个运行更快

除了构建我们的应用程序、安装依赖项和运行测试，我们通常还希望利用缓存。这让我们更少重复做任务。Android 尤其是 Gradle 为此提供了一个极好的机制-[Gradle 构建缓存](https://docs.gradle.org/current/userguide/build_cache.html)。

这让您可以跳过应用程序的预构建部分。

### 选择何时运行什么

在开发期间，我们创建许多提交，应该鼓励开发人员尽可能频繁地将它们推送到远程存储库。但是，这些提交中的每一个都创建了一个新的 CI/CD 构建，我们可能不需要为一个提交在所有可能的设备上运行每个测试。另一方面，如果`main`分支是我们主要的事实来源，我们希望尽可能多的对它进行测试，以确保它总是如预期的那样工作。我们可能希望只在`main`上启动发布部署，而不是在任何其他分支上。

我们可以使用 CircleCI 的`requires`和`filters`节来定义适合我们流程的工作流。我们的`release-build`岗位`requires`那个`unit-test`岗位。这意味着`release-build`被放入队列中，直到所有的`unit-test`任务都已通过。只有这时`release-build`作业才会运行。

我们可以使用`filters`来设置逻辑规则，以便只在特定的分支或 git 标签上运行特定的作业。例如，我们的`main`分支有一个过滤器，它用一个完整的仿真器矩阵运行`android-test`。在另一个例子中，`release`构建只在`main`上触发，并且只有当它的两个必需的作业成功运行时才触发。

```
workflows:
  test-and-build:
    jobs:
      - unit-test
      - android-test: # Commits to any branch - skip matrix of devices
          filters:
            branches:
              ignore: main
      - android-test: # Commits to main branch only - run full matrix
          matrix:
            alias: android-test-all
            parameters:
              system-image:
                - system-images;android-30;google_apis;x86
                - system-images;android-29;google_apis;x86
                - system-images;android-28;google_apis;x86
                - system-images;android-27;google_apis;x86
                - system-images;android-26;google_apis;x86
                - system-images;android-25;google_apis;x86
                - system-images;android-24;google_apis;x86
                - system-images;android-23;google_apis;x86
          name: android-test-<<matrix.system-image>>
          filters:
            branches:
              only: main 
      - release-build:
          requires:
            - unit-test
            - android-test-all
          filters:
            branches:
              only: main # Commits to main branch 
```

## 改进您的 Android 应用 CI 流程

我在本文中描述的步骤只是一个开始。有很多方法可以提高流量。以下是一些想法:

### 构建一次，运行多次

如果你是一个有经验的 Android 开发者，你会知道`connectedAndroidTest`总是从一开始就运行整个构建过程。使用 CircleCI，我们可以一次构建整个应用程序和测试，将工件传递给后续作业，并简单地在模拟器上运行测试。这个过程可能会在每次作业运行时节省几分钟的构建时间。

为此，在每个模拟器运行中添加三个命令行步骤:安装应用程序、安装测量应用程序和运行测量测试。对于我们的应用程序，我们将输入:

```
adb install app/build/outputs/apk/debug/app-debug.apk  
adb install app/build/outputs/apk/androidTest/debug/app-debug-androidTest.apk  
adb shell am instrument -w com.example.android.architecture.blueprints.reactive.test/androidx.test.runner.AndroidJUnitRunner 
```

确实，这段代码将输出发送到命令行。这不是 Gradle 提供的整洁的测试输出。如果你想让输出看起来更好，这个 StackOverflow 主题可能对你有帮助。

### 在真实设备上运行

仿真器是一种工具，但是真实世界的设备通常(可悲地)非常不同。没有什么可以取代真实世界的设备测试。为此，你可以使用在云中提供真实设备的解决方案，比如 Sauce Labs 或 Firebase Test Lab。

### 部署到 beta 测试人员，并发布应用程序

这个构建和测试示例项目只是更广泛的 CI/CD 故事的一部分。您可以进一步发展您的 CI/CD 流程。例如，您可以将应用程序的新版本自动发布到 Play Store。CircleCI 有一些指南可以帮助你。本教程是一个很好的起点。浪子官方文件是另一个有用的资源。

## 结论

在本文中，我描述了用 CircleCI 构建一个 Android 应用程序，并对其进行测试。对于我们的示例项目，我们使用单元测试和 Android 平台模拟器矩阵，使用新的 CircleCI Android orb 和机器映像。我们还谈到了如何存储和显示测试结果，以及如何编排工作流来运行测试，这不会给我们提供 Gradle 提供的整洁的测试输出，所有输出都在一部分设备上。

如果你想让我探讨另一个 Android 主题，请告诉我，无论是在文章中还是在直播中。在 [Twitter - @zmarkan](https://twitter.com/zmarkan/) 上联系我。*