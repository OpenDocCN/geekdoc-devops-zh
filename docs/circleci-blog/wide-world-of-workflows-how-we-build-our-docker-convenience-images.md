# 工作流程的广阔世界:我们如何构建 Docker 便利图像- CircleCI

> 原文：<https://circleci.com/blog/wide-world-of-workflows-how-we-build-our-docker-convenience-images/>

我们的工作流博客系列一直在探索工作流的一些主要功能集，包括[作业编排](https://circleci.com/blog/wide-world-of-workflows-job-orchestration/)、[多执行器支持](https://circleci.com/blog/wide-world-of-workflows-multi-executor-support/)和[控制流](https://circleci.com/blog/wide-world-of-workflows-control/)，重点介绍了 CircleCI 客户的开源示例，如 [Artsy](https://github.com/artsy/force) 、 [DataDog](https://github.com/DataDog/dd-trace-java) 、脸书( [React Native](https://github.com/facebook/react-native) 、谷歌([谷歌云](https://github.com/GoogleCloudPlatform/cloud-profiler-nodejs)和 [Mapbox](https://github.com/mapbox/mapbox-gl-js) 。

最后，我们想把焦点放在我们自己身上，谈谈我们如何在内部使用工作流。我们的大部分代码库都不是开源的，但是我们的 [circleci-images 库](https://github.com/circleci/circleci-images)是开源的——这是一个很好的例子，展示了您可以使用整个工作流工具集来完成什么。

### 什么是 circleci-images？

我们使用 circleci-images 来构建和部署我们的 [Docker 便利图像](https://circleci.com/docs/circleci-images/)，这些图像每天被使用数千次。

由于如此多的客户依赖于这些映像，我们使用扇入/扇出、具有不同执行环境的作业、`cron`调度、基于分支的过滤器和多个工作流等功能，构建了一个高效可靠的映像构建管道。

我们的 [config.yml 文件](https://github.com/circleci/circleci-images/blob/master/.circleci/config.yml)也大量使用了 [YAML 锚](https://circleci.com/blog/circleci-hacks-reuse-yaml-in-your-circleci-config-with-yaml/)，并且，作为我们部署过程的一部分，除了 circleci-images repo 本身之外，我们还整合了多个 GitHub 存储库，并最终将映像推送到 Docker Hub 上的 [staging](https://hub.docker.com/r/ccistaging/) 和 [production](https://hub.docker.com/r/circleci) 组织。

让我们深入探讨一下所有这些不同的部分是如何组合在一起的:circleci-images repo 中的源代码是如何作为 Docker 映像部署到 17 个不同的 Docker Hub 存储库中的，每个存储库中都有自己特定的映像变体集？更重要的是，我们为什么要这样建造它？

### circleci 影像之旅

在持续集成和交付管道中跟踪代码通常从一次提交开始。然而，对于计划的工作流，事情并不总是那么简单。在这种情况下，我们的 circleci-images config.yml 从解决两个不同的用例开始:
**1)我们希望构建和部署夜间映像。**

为什么？因为构建 Docker 映像通常需要引入许多上游依赖项，我们的映像当然也不例外。我们根据映像和变体在 Docker 映像中安装各种语言、框架、实用程序和应用程序，从常见的 CLI 工具如`curl`、`git`和`jq`；浏览器测试包，如 Chromedriver 或 PhantomJSDocker、Firefox 和 Chrome 等应用程序。如果该软件有新的补丁或稳定版本，我们希望尽快更新我们的便利映像，以便为我们的用户提供最安全、最稳定的平台。[预定工作流程](https://circleci.com/docs/workflows/#scheduling-a-workflow)让这一切成为可能。2)我们希望在每次提交时构建、测试和部署映像。

这是更典型的场景:每夜构建很重要，但获得代码变更的即时反馈也很重要。

为了适应这两种用例，我们在我们的 [config.yml](https://github.com/circleci/circleci-images/blob/master/.circleci/config.yml) 中使用了两种不同的工作流:一种夜间`build_test_deploy`工作流，它在世界协调时每天午夜运行，但只在 circleci-images 的`master`分支上运行

```
workflows:
  version: 2
  build_test_deploy:
    triggers:
      - schedule:
          cron: "0 0 * * *"
          filters:
            branches:
              only:
                - master 
```

—以及一个常规的`commit`工作流，为每个新的提交处理我们的控制流。然而，除了几个例外，这些工作流运行相同的一组作业，这是多工作流配置的一个重要特性:定义一个作业一次，您可以在任意多个工作流中重用它，这有助于保持您的 config.yml 简洁明了。

既然我们已经解决了夜间工作流场景，让我们继续关注从提交到部署的代码变更。对于给定的提交，我们运行几个初始作业，以编程方式为我们将要构建的所有映像变体生成 docker 文件，因此如果 shell 脚本出现问题，并让作业负责这项工作，工作流就在那里停止。

如果一切顺利，我们就展开下一组工作，做实际的形象塑造工作。这些作业利用了我们基于[分支的过滤](https://circleci.com/docs/workflows/#branch-level-job-execution)特性，因为我们只想在提交到`staging`或`master`分支时构建并推送 Docker 映像。同样，我们在这里使用 YAML 锚点，以避免在一个又一个作业中手动重复相同的工作流过滤器:

```
workflow_filters: &workflow_filters
  requires:
    - refresh_tools_cache
  filters:
    branches:
      only:
        - master
        - production
        - parallel 
```

每个形象塑造工作看起来或多或少都像这样:

```
 publish_image: &publish_image
    machine: true
    working_directory: ~/circleci-bundles
    steps:
      - checkout
      - run:
          name: Docker Login
          command: docker login -u $DOCKER_USER -p $DOCKER_PASS
      - run:
          name: Build and Publish Images
          command: |
            export COMPUTED_ORG=ccistaging
            if [[ "$CIRCLE_BRANCH" == "production" ]]
            then
              export COMPUTED_ORG=circleci
            fi
            export NEW_ORG=${NEW_ORG:-$COMPUTED_ORG}

            make -j $PLATFORM/publish_images
      - store_artifacts:
          path: "."
          destination: circleci-bundles 
```

这个 YAML 锚点允许我们重用我们的映像构建逻辑，只需传入一个不同的环境变量来指定我们在特定作业中构建哪个映像:

```
 publish_node:
    <<: *publish_image
    environment:
      - PLATFORM: node 
```

与传统的扇出/扇入工作流不同，在传统的扇出/扇入工作流中，可以扇出来运行测试，然后扇入来运行一个离散的部署作业或一组作业，这里我们的部署逻辑是在每个单独的映像构建作业中执行的[(通过 Make targets](https://github.com/circleci/circleci-images/blob/master/shared/images/build.sh) 处理的[)。虽然我们*在 Docker Hub 上托管*我们的便利图像，但我们在 CircleCI 上构建它们——我们发现这比使用 Docker Hub 构建每个图像的每个变体要快得多。](https://github.com/circleci/circleci-images/blob/17e60c886fac4d2625419deea7e33e153a640679/Makefile#L22)

正如你所看到的，我们最近更新了一些 Docker Hub 存储库——以`[circleci/golang]`(https://hub.docker.com/r/circleci/golang)为例——我们使用 Docker Hub 为每个存储库只构建了一个[示例映像](https://hub.docker.com/r/circleci/golang/builds)，这为我们提供了 Docker Hub 的[自动构建](https://docs.docker.com/docker-hub/builds)功能所提供的集成 [Dockerfile](https://hub.docker.com/r/circleci/golang/~/dockerfile/) 和 README 支持。

要构建所有这些图像，可以使用[远程 Docker 环境](https://circleci.com/docs/building-docker-images/)或[执行程序](https://circleci.com/docs/executor-types/#using-machine)。如果我们的映像构建作业有一些专门的逻辑，需要安装特定的依赖项或框架或语言，或者在构建之前运行一组特定的命令，那么 Remote Docker 可能是最佳选择，因为它允许我们使用 Docker 映像作为主要的构建容器，并将与 Docker 相关的命令发布到远程 Docker 环境。

然而，由于这些作业完全由 Docker 相关的命令组成，所以使用`machine` executor 更容易。这很好地说明了[多平台工作流](https://circleci.com/docs/executor-types/)的威力——为每项工作选择不同的执行环境为您提供了高水平的定制，从而实现了更加简化、优化的构建/测试/部署流程。

想深入挖掘吗？因为作为我们方便的图像构建过程的一部分使用的所有存储库都是开源的，所以您可以确切地看到它是如何工作的: