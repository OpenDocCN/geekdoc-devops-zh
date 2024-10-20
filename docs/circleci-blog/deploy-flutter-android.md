# 使用浪子| CircleCI 构建和部署 Flutter 应用程序

> 原文：<https://circleci.com/blog/deploy-flutter-android/>

[Flutter](https://flutter.dev/) 是 Google 提供的一个工具包，用于构建跨平台应用，包括 Android、iOS 和 web 应用。随着越来越多的应用程序使用 it 和 Dart(底层语言)编写，考虑开发人员如何将他们的代码打包并交付给用户变得越来越重要。利用 CircleCI 和浪子，这是一个相对简单的过程，可以自动化。

按照以下指南，可以安装并创建您的第一个应用程序:

默认的“hello world”应用程序就足够了。我们不会过多地处理应用程序本身。将其推送到 GitHub 上的一个存储库中。

确保您已设置好使用 Google Play 或 Apple App Store 发布应用程序，每个应用程序的步骤会有所不同，因此请务必遵循各个提供商概述的步骤。

阅读更多关于[移动应用开发持续集成](https://circleci.com/blog/ci-for-mobile-app-development/)的信息。

## 机器人

当发布 Android 应用时，在实现浪子之前有一些要求需要满足。需要一个“上传密钥”,这样谷歌才能在接受应用之前验证其真实性。设置的步骤可以在[这里](https://flutter.dev/docs/deployment/android)找到。建议在构建 Android 应用程序时使用 AppBundles，因为它允许 Google 为不同的平台构建单独的 apk，同时保持文件大小不变。记下您保存`key.jks`文件的位置。虽然它可以存在于您的主目录中，但是在配置`key.properties`文件时，将它保存到项目中一个被 g it 忽略的目录中会很方便。

本指南中的两个东西需要作为[环境变量](https://circleci.com/docs/env-vars/)添加到 CircleCI 中:`key.properties`文件和`key.jks`文件。这将允许[持续集成](https://circleci.com/continuous-integration/)管道正确构建和签署应用程序。因为一个文件需要换行，而另一个文件是二进制文件，所以建议在将它们作为环境变量添加之前对它们进行 base64 编码，以保留数据。

您可以通过运行以下命令对它们进行编码:

```
cat <file> | base64 -w 0 # -w 0 will ensure no newlines are present. 
```

然后，可以在作业过程中使用以下命令重新添加这些文件:

```
echo "$ENV_VARIABLE_NAME" | base64 --decode > <file> 
```

在创建 Android 设置时，您还需要设置一个浪子服务帐户。你可以在这里阅读更多关于[的内容。请务必将您的服务帐户 JSON 文件放在手边。我们将很快将其添加到管道中。还要注意，命令应该从您的 Flutter 项目下的`android`目录中运行。](https://docs.fastlane.tools/getting-started/android/setup/)

添加浪子所需的服务帐户可以通过添加服务帐户 JSON 文件内容作为`SUPPLY_JSON_KEY_DATA`环境变量来完成。这不要与`SUPPLY_JSON_KEY`混淆，后者应该是文件的文件路径。

## Fastlane

现在我们的环境变量已经就位，我们可以使用 Flutter 构建一个签名的`.aab`文件，是时候告诉浪子如何为我们做这件事了。我们从安卓开始。打开`android/fastlane/Fastfile`会显示一些构建 Android 应用的默认设置，但它们对我们的用例不起作用。我们在用颤动建造。

首先，我们来定义一个新的“车道”。这是浪子被调用时将执行的一组指令。目前，我们将把我们的应用推向“测试”频道，而不是生产。

```
desc "Deploy a new beta build to Google Play"
lane :beta do

end 
```

这是一个车道的基本结构。接下来，我们要获取一个内部版本号，这是一个递增的数字，用作应用程序版本的参考(不同于面向用户的 semver)。

将以下内容添加到车道:

```
 build_number = number_of_commits() 
```

这确保了`build_number`是对 Git 存储库进行提交的次数。这也可以在 Flutter 用来构建包的`pubspec.yaml`文件中手动递增。

接下来，我们将做建筑。我们需要转到父目录，运行几个 shell 命令来获取我们的包，清理以前的构建(如果有的话)，然后用我们的构建号构建 appbundle。

```
 Dir.chdir "../.." do
    sh("flutter", "packages", "get")
    sh("flutter", "clean")
    sh("flutter", "build", "appbundle", "--build-number=#{build_number}")
  end 
```

最后，我们可以发送到测试版频道的应用商店。

```
 upload_to_play_store(track: 'beta', aab: '../build/app/outputs/bundle/release/app.aab') 
```

最后，你的车道看起来会像这样:

```
desc "Deploy a new beta build to Google Play"
lane :beta do
  build_number = number_of_commits()
  Dir.chdir "../.." do
    sh("flutter", "packages", "get")
    sh("flutter", "clean")
    sh("flutter", "build", "appbundle", "--build-number=#{build_number}")
  end
  upload_to_play_store(track: 'beta', aab: '../build/app/outputs/bundle/release/app.aab')
end 
```

继续保存文件，然后运行`fastlane beta`。它将逐步运行，为您构建应用程序并推送应用程序。

现在，我们将对 iOS 做同样的事情，但是将一些命令替换为它们各自的 iOS 命令。这应该在`ios/Fastlane`目录中完成。

```
desc "Deploy a new build to Testflight"
lane :beta do
  Dir.chdir "../.." do
    sh("flutter", "packages", "get")
    sh("flutter", "clean")
    sh("flutter", "build", "ios", "--release", "--no-codesign")
  end
  build_ios_app(scheme: "MyApp")
  upload_to_testflight
end 
```

**注:**app 是两次搭建。第一个构建基于最新的 Flutter 工具链，而第二个构建构建将上传到 Testflight 的实际的`.ipa`文件。

## 现在，有了 CircleCI

现在我们已经准备好了一切，是时候编写 CircleCI 配置了，这样分支的推送就可以转化为新的部署。

### ios

对于 iOS 的部署，CircleCI 已经有相关文档了！设置带颤振的浪子展开的步骤类似于此处[和此处](https://circleci.com/docs/ios-codesigning/)和[概述的步骤。](https://circleci.com/docs/deploying-ios/#app-store-connect)

### 机器人

谈到要选择的 Docker 图像，有两个选项。你可以构建并推送 Flutter 项目使用的 Docker 镜像，在这里找到[，或者你可以拉`gmemstr/flutter-fastlane-android:29.0`，它构建在一个现有的 CircleCI Android Docker 镜像之上并安装 Flutter 和浪子，避免在运行时安装它的额外开销。](https://github.com/flutter/flutter/blob/master/dev/ci/docker_linux/Dockerfile)

还要确保前面提到的环境变量被添加到您的项目中。即:

*   `PLAY_STORE_UPLOAD_KEY`-base64 编码的`key.jks`文件
*   `PLAY_STORE_UPLOAD_KEY_INFO`-base64 编码的文件，包含密码和其他关于上传签名密钥的信息
*   `SUPPLY_JSON_KEY_DATA` -您的 Google 服务帐户用于验证的 JSON 字符串

在您的项目根目录中，添加一个`.circleci`文件夹，并在该文件夹中添加一个`circleci.yml`文件。将以下配置添加到新创建的文件中:

```
version: 2.1

executors:
  android-flutter:
    docker:
      - image: gmemstr/flutter-fastlane-android:29.0
    environment:
      TERM: dumb
      _JAVA_OPTIONS: "-Xmx2048m -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap"
      GRADLE_OPTS: '-Dorg.gradle.jvmargs="-Xmx2048m"' 
```

上面的代码片段定义了一个执行器，由于 CircleCI 2.1 中使用了可重用配置和 [orbs](https://circleci.com/orbs/) ，我们可以在以后的配置中重用这个执行器。它还包括一些环境变量，以确保 Java 在构建应用程序时不会消耗我们所有的内存。接下来，我们将定义希望运行命令的作业。

```
jobs:
  beta_deploy:
    executor: android-flutter
    steps:
      - checkout
      - run: echo "$PLAY_STORE_UPLOAD_KEY" | base64 --decode > key.jks
      - run: echo "$PLAY_STORE_UPLOAD_KEY_INFO" | base64 --decode > android/key.properties
      - run: cd android && fastlane beta 
```

最后，我们将定义我们的工作流，它将只在新代码被推送到`beta`分支时运行作业。

```
workflows:
  deploy:
    jobs:
      - beta_deploy:
          filters:
            branches:
              only: beta 
```

最终，您的配置将如下所示:

```
version: 2.1

executors:
  android-flutter:
    docker:
      - image: gmemstr/flutter-fastlane:latest
    environment:
      TERM: dumb
      _JAVA_OPTIONS: "-Xmx2048m -XX:+UnlockExperimentalVMOptions -XX:+UseCGroupMemoryLimitForHeap"
      GRADLE_OPTS: '-Dorg.gradle.jvmargs="-Xmx2048m"'
jobs:
  beta_deploy:
    executor: android-flutter
    steps:
      - checkout
      - run: echo "$PLAY_STORE_UPLOAD_KEY" | base64 --decode > key.jks
      - run: echo "$PLAY_STORE_UPLOAD_KEY_INFO" | base64 --decode > android/key.properties
      - run: cd android && fastlane beta

workflows:
  deploy:
    jobs:
      - beta_deploy:
          filters:
            branches:
              only: beta 
```

[在 CircleCI](https://circleci.com/docs/projects/) 上设置您的项目。一旦你点击**设置项目**，你将被带到一个文本编辑器，允许你从上面复制配置。点击**开始建造**。现在，对 beta 分支的每个新提交都将导致部署。