# 使用 CircleCI 转轮进行本地测试| CircleCI

> 原文：<https://circleci.com/blog/using-runner-for-local-testing/>

> 在本文中，您将学习如何在本地机器上设置 CircleCI runner 代理来运行测试。您还将学习如何配置 CircleCI 管道，以便正确调用运行器。

许多开发团队从运行他们测试的本地构建盒(或者六个)开始他们的 CI/CD 之旅。例如，在我工作的几个移动团队中，我们有几个插有物理设备的 Mac Mini 盒子，用于运行本地 UI 和单元测试。最终，我们迁移到了基于云的解决方案，这给我们带来了更好的稳定性和许多新功能。但是迁移到云也意味着我们的本地硬件过时了。我们不得不依靠仿真，或者选择另一个基于云的设备场来实现。

有了 CircleCI 的 runners，你可以继续使用自己的设备进行测试，同时受益于 CircleCI cloud 中的其他任何东西。您还可以使用运行器进行测试，这些测试需要在您自己的基础设施上运行，用于安全关键的场景，或者当您正在开发自己的硬件时。

随着 [CircleCI 免费计划](/blog/new-cicd-free-plan-what-devs-get/)的推出，像跑步者这样的功能现在每个人都可以使用。在本文中，您将学习如何在本地机器上设置 CircleCI runner 代理来运行测试。您还将学习如何配置 CircleCI 管道，以便正确调用运行器。

## 先决条件

本教程假设您对 CircleCI 有所了解，并且了解如何配置管道来构建和测试软件项目。

我已经创建了一个显示流道配置的示例项目。

您还可以在示例项目的 CircleCI 仪表板中找到它的构建[。](https://app.circleci.com/pipelines/github/zmarkan-demos/android-testing-runner-example/7/workflows/8feabfe4-c942-4949-bf88-cfed8aca7615/jobs/30)

**注意** : *这个例子展示了一个 Android 应用程序和测试，使用了 macOS 上的 runner。然而，其背后的原则可以普遍适用:适用于 iOS 应用程序、定制硬件或不同基础设施上的 runner，如 Docker 容器、Windows、Linux 机器等。*

## 跑步者如何工作

runner 是安装在基础设施上的一个进程，它与 CircleCI 通信。运行者作为他们自己的资源类。您的`config.yml`仍然定义管道、作业和工作流，但是您没有使用 CircleCI 的基于云的资源，如容器和虚拟机，而是自己执行。

首先，在本地安装 runner 代理。这是一个后台运行的守护进程，检查来自 CircleCI 的提示。当 CircleCI 发出必须在运行程序上触发作业的信号时，代理将执行工作负载。它还可以访问与 CircleCI 云版本相同的凭证，因此它可以无缝运行，并可以从您的存储库中获取代码。

一旦作业完成，结果和任何工件都被发送回云服务。只有实际的作业执行是在您的基础设施上完成的。

## 履行

实施由以下过程组成:

*   在本地设置运行程序代理
*   从你的圈子中调用跑步者`config.yml`

### 设置跑步者代理

安装 runner 代理取决于您使用的平台。请参考 [CircleCI 文档](https://circleci.com/docs/runner-installation/)中的设置说明。

对于我的示例应用程序，我在 Mac 计算机上安装了 runner 代理。

**注意** : *这些步骤只是一个简要的概述，并不是完整的指南。*

设置流道的主要步骤是:

1.  在您的组织中注册一名新的跑步者
2.  按照适用于您的平台的说明安装运行程序代理
3.  启动 runner 守护进程，并确保它在系统启动时运行

使用`circleci` CLI 工具在您的组织中注册新的跑步者。如果这是您的第一个跑步者，您可能需要在该组织中创建一个名称空间。您将使用完全限定名引用 CircleCI 配置中的特定 runner。对我来说，那就是`zan_demos_namespace/mac_runner`；`mac_runner`是跑步者的名字，`zan_demos_namespace`是我使用的名称空间。

以下是一个命令示例:

```
$ circleci runner resource-class create zmarkan-demos-namespace/local-mac-runner "Local Mac OS Runner" --generate-token 
```

这个命令还将为这个 runner 设置一个新的 API 令牌，以便与组织中的 CircleCI API 一起使用。因为有了`--generate-token`标志，这才成为可能。请确保存储该令牌以备后用。

向 CircleCI 注册转轮后，按照文档中的[说明安装转轮代理。设置需要预装`curl`、`sha256sum`、`tar`和`gzip`等软件。可能需要其他软件。](https://circleci.com/docs/runner-installation/)

设置一些守护程序脚本，并传入这些值:

*   在上一步中创建的 API 令牌
*   跑步者名称空间
*   您指定的名称

这些项目的细节因您使用的平台而异。例如，在 Mac 上你需要创建一个`launchd.plist`并用`launchctl`启动它。在 Linux 上，这将是一个以`systemctl`开始的新服务。

## 从管道调用运行程序

一旦 runner 代理开始运行，您就可以从`.circleci/config.yml`文件中的一个作业调用它。确保用您的运行程序的名称空间和运行程序名称指定`machine`执行器类型和资源类。这里有一个例子:

```
 runner-test:
    machine: true
    resource_class: zmarkan-demos-namespace/local-mac-runner
    environment:
      JAVA_HOME: /Users/zmarkan/libs/jdk-11.0.2.jdk/Contents/Home
      ANDROID_SDK_ROOT: /Users/zmarkan/Library/Android/sdk
    steps:
      - checkout
      - run:
          name: Run connected check on local runner
          command: |
            ./gradlew connectedDebugAndroidTest 
```

在我的例子中，我有一些 Android 设备连接到我的 Mac 上，所以跑步者可以访问本地硬件来运行测试。

在使用 CircleCI 自己的基础设施时，我还需要传入一些现成的特定值，比如`JAVA_HOME`和`ANDROID_SDK_ROOT`。这是因为它们在 CircleCI runner 使用的特定 shell 中不容易访问。在大多数现代 MAC 电脑上，使用的外壳是 ZshCircleCI 默认运行 Bash。

剩下的步骤很简单，取决于您的应用程序和您想要执行的内容。在我的情况下，就是从回购中检查代码并运行测试。您还可以使用`store_artifacts`和`store_test_results`来处理测试并保存输出。然后，您可以在 CircleCI 云仪表板中看到输出。

## 结论

在本文中，您了解了如何设置一个本地 CircleCI 运行程序，以便在本地基础设施上执行 CircleCI 云工作负载。Runner 有许多使用案例，从安全性到在定制硬件上的执行，都具有基于云的 CircleCI 提供的相同 CI/CD 体验。

如果你对我接下来要讨论的话题有任何反馈或建议，请通过 [Twitter - @zmarkan](https://twitter.com/zmarkan/) 联系我。