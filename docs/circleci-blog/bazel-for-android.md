# Bazel Android 项目的持续集成| CircleCI

> 原文：<https://circleci.com/blog/bazel-for-android/>

Bazel(发音像美味的药草:“bay-zell”)是由 Google 开发的通用构建工具。一些著名的公司，如 Twitter 和 Android 开源项目已经迁移到 Bazel。在本教程中，您将学习如何构建一个 Bazel Android 项目，并设置它与 CircleCI 的持续集成。我们将通过自动运行测试并生成一个二进制 APK 文件来结束本文。

除了书面指南，还有一个[工作样本项目](https://github.com/zmarkan/bazel-android-cicd-example)。样本项目也可以在 CircleCI 上的[查看。](https://app.circleci.com/pipelines/github/zmarkan/bazel-android-cicd-example)

### 关于示例项目

本教程的示例项目是一个用 Kotlin 编写的最小的 Android 应用程序，带有 Bazel 构建配置。项目应用程序具有应用程序二进制代码- `//app/src:app`以及使用 Robolectric - `//app/src:unit_tests`进行单元测试的构建目标。

## 先决条件

要完成本教程，您应该对现代 Android 开发、Kotlin、Gradle 和 Git 有所了解。你不需要对巴泽尔有任何经验。

## 为巴泽尔建立一个项目

要开始，您需要转到 GitHub，克隆示例项目，并检查设置。

示例项目是一个标准的、最小的 Android Gradle 应用程序，使用 Kotlin。它有一个项目文件和一个带有自己的`build.gradle`的`app`模块。在`app`目录中有一个公共的文件层级，有`main`、`test`和`androidTest`子目录，用于各种构建类型。

**注意:** *我在 Mac OS Catalina 上安装家酿软件时遇到了问题，因此您的里程数可能会有所不同。*

### 使用工作空间、构建和规则

开始使用 Bazel 需要两个文件- `WORKSPACE`和`BUILD`。`WORKSPACE`文件描述的正是你的工作空间。

`WORKSPACE`应该在引用所有其他资源的顶级目录中。它相当于顶层的`build.gradle`,在这里您可以指定在哪里找到依赖关系的存储库。

您在`WORKSPACE`文件中看到的第一行是:

```
load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive") 
```

`load`方法允许您访问该位置的其他脚本；在我们的例子中`http_archive`。使用这个方法从 web 上获取其他远程资源，比如来自 GitHub 的库版本。Bazel 的另一个很好的特性是用于验证下载文件完整性的`sha256`参数。

```
http_archive(
    name = "rules_android",
    urls = ["https://github.com/bazelbuild/rules_android/archive/v0.1.1.zip"],
    sha256 = "cd06d15dd8bb59926e4d65f9003bfc20f9da4b2519985c27e190cddc8b7a7806",
    strip_prefix = "rules_android-0.1.1",
) 
```

当然，因为 Skylark 是有效的 Python，变量和常量的工作方式与您预期的一样。

```
 RULES_JVM_EXTERNAL_TAG = "2.2"
RULES_JVM_EXTERNAL_SHA = "f1203ce04e232ab6fdd81897cf0ff76f2c04c0741424d192f28e65ae752ce2d6"

http_archive(
    name = "rules_jvm_external",
    strip_prefix = "rules_jvm_external-%s" % RULES_JVM_EXTERNAL_TAG,
    sha256 = RULES_JVM_EXTERNAL_SHA,
    url = "https://github.com/bazelbuild/rules_jvm_external/archive/%s.zip" % RULES_JVM_EXTERNAL_TAG,
)

load("@rules_jvm_external//:defs.bzl", "maven_install") 
```

您已经看到了如何使用`http_archive`加载新脚本，然后使用它们加载名为`maven_install`的新脚本。

### 获取依赖关系

在 Android 和 JVM 项目中，您通常从 Maven 存储库中获取依赖项。两个最常见的开源依赖库是 Maven Central 或 JCenter。对于特定于 Android 的依赖项，还有谷歌自己的 Maven 知识库。

Bazel 采用了一种熟悉的`maven_install`方法:

```
 maven_install(
    artifacts = [
        "androidx.core:core-ktx:1.2.0",
        "androidx.appcompat:appcompat:1.1.0",
        "androidx.fragment:fragment:1.0.0",
        "androidx.core:core:1.0.1",
        "androidx.lifecycle:lifecycle-runtime:2.0.0",
        "androidx.lifecycle:lifecycle-viewmodel:2.0.0",
        "androidx.lifecycle:lifecycle-common:2.0.0",
        "androidx.drawerlayout:drawerlayout:1.0.0",
        "androidx.constraintlayout:constraintlayout:1.1.3",
        "com.google.android.material:material:1.0.0",
        "org.jetbrains.kotlinx:kotlinx-coroutines-core:1.4.1",
        "org.jetbrains.kotlinx:kotlinx-coroutines-android:1.4.1",
        "junit:junit:4.+",

    ],
    repositories = [
        "https://maven.google.com",
        "https://jcenter.bintray.com",
    ],
    fetch_sources = True,
) 
```

`artifacts`参数包含所有的依赖项和它们的版本，`repositories`指定它们来自哪里。

为整个工作空间下载依赖项，但还不包括在应用程序中。在本教程的稍后部分，您会发现它们已经包含在内。

### 合并科特林

通读 Bazel 文档时，您会了解到 Kotlin 并没有得到该工具的官方支持。幸运的是，Kotlin 有一个官方的社区规则，它带来了编译特性。

目前，Bazel 插件还不完全支持 Kotlin 1.4。您可以引入 1.3.0 稳定版或 1.4.0 候选版。

```
 rules_kotlin_version = "legacy-1.4.0-rc4"
rules_kotlin_sha = "9cc0e4031bcb7e8508fd9569a81e7042bbf380604a0157f796d06d511cff2769"

http_archive(
    name = "io_bazel_rules_kotlin",
    urls = ["https://github.com/bazelbuild/rules_kotlin/releases/download/%s/rules_kotlin_release.tgz" % rules_kotlin_version],
    sha256 = rules_kotlin_sha,
)
load("@io_bazel_rules_kotlin//kotlin:kotlin.bzl", "kotlin_repositories", "kt_register_toolchains")

kotlin_version = "1.4.20"
kotlin_release_sha = "11db93a4d6789e3406c7f60b9f267eba26d6483dcd771eff9f85bb7e9837011f"
rules_kotlin_compiler_release = {
    "urls": [
    "https://github.com/JetBrains/kotlin/releases/download/v{v}/kotlin-compiler-{v}.zip".format(v = kotlin_version),
    ],
    "sha256": kotlin_release_sha,
}
kotlin_repositories(compiler_release = rules_kotlin_compiler_release)
kt_register_toolchains() 
```

您可以使用 Kotlin Bazel 规则附带的 Kotlin 编译器的捆绑版本。如果你喜欢使用其他的东西，你可以从 GitHub 上的 Kotlin 发布页面下载任何其他的编译器版本。

现在我们已经完成了你的 Kotlin Android 应用程序的 Bazel `WORKSPACE`，我们可以专注于单独的包和它的`BUILD`文件。

### 什么是 Bazel 包？

Bazel 应用程序称为目标，它们位于 Bazel 软件包中。Bazel 包是任何具有`BUILD`文件及其子目录的目录。也就是说，除非一个子目录包含自己的`BUILD`文件。在这种情况下，该特定的子目录将成为其自己的包。

Bazel 中的包在工作区内用双斜线`//`和它们的目录结构来寻址。我们的应用程序有一个单独的包:`//app/src`。那就是`BUILD`文件所在的地方。

### 在 Bazel 应用程序中使用目标

`BUILD`文件包含我们前面提到的`load`方法调用，以及`kt_android_binary`、`android_test`和`kt_android_library`调用。这些元素是 Bazel 应用程序中的目标。目标可以是接受输入并产生构建输出的任何东西。在我们的例子中，这可以是源代码，也可以是另一个目标。每个 Bazel 应用程序可以包含多个目标。

对于本教程，我们将使用`test`和`android_binary`目标。Android 二进制文件输出您的。apk 文件，并且`test`做测试。你可以在[巴泽尔文档](https://docs.bazel.build/versions/master/be/android.html)中找到这两者的文档。对于每个 Android 目标，您必须包括 Android 清单文件。

```
 android_binary(
    name = "my_bazel_app",
    manifest = MANIFEST,
    custom_package = PACKAGE,
    manifest_values = {
        "minSdkVersion": "21",
        "versionCode" : "2",
        "versionName" : "0.2",
        "targetSdkVersion": "29",
    },
    deps = [
        ":bazel_res",
        ":bazel_kt",
        artifact("androidx.appcompat:appcompat"),
    ],
) 
```

### 使用 Bazel 命令构建项目

要构建，使用`bazel build [target]`。`[target]`是您工作空间中完全合格的 Bazel 目标。对于本教程中的示例应用程序，目标是:`//app/src:app`，因此命令将是`bazel build //app/src:app`。

第一次构建可能需要一些时间，但是 Bazel 将缓存大多数依赖项和临时工件，因此未来的构建将会更快。

### 安装示例应用程序

所有最终构建工件都存储在`bazel-bin/app/src/main/app.apk`中。为了安装应用程序，Bazel 有一个方便的`mobile-install`命令:

```
bazel mobile-install //app/src:app 
```

该命令使用与您连接的设备相关的参数调用`adb install`。

## 与 CircleCI 一起建立 Bazel 项目

CircleCI 有许多 Android Docker 映像，这些映像提供了构建 Android 应用程序所需的一切。也就是*几乎*一切。Bazel 不是默认安装的，所以这将是我们的第一步。

Docker 镜像是基于 Debian Linux 的，所以你可以使用 Bazel 文档中的 [Ubuntu 安装说明。有两个步骤:](https://docs.bazel.build/versions/master/install-ubuntu.html)

1.  安装 Bazel apt 库
2.  用`apt install`安装 Bazel 本身

一个简单的`setup-bazel` CircleCI 命令就可以完成这项工作。

```
commands:
  setup-bazel:
    description: |
      Setup the Bazel build system used for building Android projects
    steps:
      - run:
          name: Add Bazel Apt repository
          command: |
            sudo apt install curl gnupg
            curl -fsSL https://bazel.build/bazel-release.pub.gpg | gpg --dearmor > bazel.gpg
            sudo mv bazel.gpg /etc/apt/trusted.gpg.d/
            echo "deb [arch=amd64] https://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
      - run:
          name: Install Bazel from Apt
          command: sudo apt update && sudo apt install bazel 
```

这个片段大部分是可复制、可粘贴和可重用的。您可能只是想为更加确定性的构建固定一个特定版本的 Bazel。我将在接下来的步骤和最终的示例项目中向您展示原因。

### 测试和构建 Bazel 目标

为了测试和构建 Bazel 目标，您需要分别使用`bazel test`和`bazel build`命令，为正确的目标传递合格的包和名称。在我们的例子中，这些是用于测试的`//app/src:unit_tests`，以及用于应用程序二进制文件的`//app/src:app`。

在本例中，我们在`setup-bazel`步骤之后立即构建它们。

```
jobs:
  build:
    docker:
      - image: circleci/android:api-29
    steps:
      - checkout
      - android/accept-licenses
      - setup-bazel
      - run:
          name: Run tests
          command: bazel test //app/src:unit_tests # Depending on your Bazel package and target
      - run:
          name: Run build
          command: bazel build //app/src:app # Depending on your Bazel package and target 
```

### 存储测试和构建工件

Bazel for Android 将所有测试输出存储在项目的`bazel-testlogs`目录中，将所有二进制输出存储在项目的`bazel-bin`目录中。

在我们的例子中，输出将采用与 Bazel targets - `src/app`相同的包结构。当您添加这些节时，CircleCI 会存储所有有用的输出:

```
 - store_test_results:
          path: ~/project/bazel-testlogs/app/src/unit_tests
      - store_artifacts:
          path: ~/project/bazel-testlogs/app/src/unit_tests
      - store_artifacts:
          path: ~/project/bazel-bin/app/src/app.apk
      - store_artifacts:
          path: ~/project/bazel-bin/app/src/app_unsigned.apk 
```

我们还存储了`app_unsigned.apk`,因为如果你想发布一个发布版本，你需要自己签名。你可以在安卓开发者门户上阅读更多关于[手动签名的信息。](https://developer.android.com/studio/publish/app-signing.html#sign-manually)

### 安装和使用特定的 Bazel 版本，以实现更具确定性的构建

当使用`apt install bazel`安装 Bazel 时，您安装的是*最新稳定的*版本。在本地机器上总是使用最新和最好的可能没问题，但是在 CI/CD 环境中，您可能希望在您的构建中有更多的确定性。

通过修改您的`apt install bazel`行来使用一个特定的版本，您可以确保一致地使用最新的版本:`apt install bazel-3.7.2`。您需要确保在所有后续调用中使用该特定版本。例如，`bazel-3.7.2 build ...`。

使用 Bazel 特定版本的一种方法是在您的`config.yml`中使用 CircleCI 可重用参数。示例项目使用了`build`作业中的参数:

```
jobs:
  build:
    parameters:
      bazel-version:
        description: "Pinned Bazel version Replace with your one"
        default: "bazel-3.7.2"
        type: string
    ...
    steps:
      - checkout
      - android/accept-licenses
      - setup-bazel:
          bazel-version: <<parameters.bazel-version>>
      - run:
          name: Run tests
          command: << parameters.bazel-version >> test //app/src:unit_tests # Depending on your Bazel package and target 
```

## 结论和下一步措施

我希望这篇教程能让你了解如何在你的 [CI/CD 管道](https://circleci.com/blog/what-is-a-ci-cd-pipeline/)中运行和构建 Bazel Android 应用程序。下一步可能是通过自动部署到测试服务来进一步扩展管道，甚至直接在应用商店分发应用。