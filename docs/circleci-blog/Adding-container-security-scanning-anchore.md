# 用 Anchore | CircleCI 进行集装箱安全扫描

> 原文：<https://circleci.com/blog/Adding-container-security-scanning-anchore/>

Docker 让开发人员能够大规模地简化应用程序的打包、存储和部署。随着软件开发团队越来越多地使用容器技术，保护这些映像变得很有挑战性。由于容器映像成为开发生命周期的常规部分，对这些工件的安全检查需要被编织到自动化管道中，并成为测试的标准化最佳实践。

## 使用锚定保护容器图像

当我们转向构建容器映像时，持续集成起着至关重要的作用。容器图像本质上是不可变的。意思是，一旦我们建立了一个图像，它是不变的。如果我们想对容器内运行的应用程序进行任何更改，我们将使用代码更改构建另一个映像，然后部署新的、更新的容器。这减少了开发和测试时间，并且非常适合 CI。

CI 的核心部分之一是自动运行测试。当我们在 CI 管道中构建容器映像时，我们应该使用能够对构建工件进行适当级别分析的工具。 [Anchore](https://anchore.com/) 是一种对集装箱图像进行静态分析的服务，并应用用户定义的可接受的策略，以允许自动化的集装箱图像验证和认证。这意味着 Anchore 用户不仅可以深入了解映像中包含的操作系统和非操作系统包，还可以围绕工件及其内容创建治理。

## 在 circleci 构建中集成锚定扫描

在下面的例子中，我们将介绍如何将 Anchore 扫描集成到 CircleCI 构建中。

注意:这些例子利用了 anchore/anchore-engine @ 1 . 0 . 0[orb](https://circleci.com/developer/orbs/orb/anchore/anchore-engine)。此外，它们需要一个 circleci 目录和`config.yml`文件。

## 将公共图像扫描作业的锚定扫描添加到 CircleCI 工作流

如果你需要扫描一个公共图像，这里有一个例子，我们使用 orb 扫描`anchore-engine:latest`公共图像。

```
version: 2.1
orbs:
  anchore-engine: anchore/anchore-engine@1.0.0
workflows:
  scan_image:
    jobs:
      - anchore/image_scan:
          image_name: anchore/anchore-engine:latest
          timeout: '300' 
```

## 将私有图像扫描作业的锚定扫描添加到 CircleCI 工作流

如果你需要扫描一个私有图像，这里有一个例子，我们使用 orb 扫描 anchore-engine:private Docker 注册表中的最新图像。

```
version: 2.1
orbs:
  anchore-engine: anchore/anchore-engine@1.0.0
workflows:
  scan_image:
    jobs:
      - anchore/image_scan:
          image_name: anchore/anchore-engine:latest
          timeout: '300'
          private_registry: True
          registry_name: docker.io
          registry_user: "${DOCKER_USER}"
          registry_pass: "${DOCKER_PASS}" 
```

## 将锚点图像扫描添加到容器构建管道作业中

在这里，我们从连接的存储库构建一个 Docker 映像，然后扫描该映像。作为扫描的一部分，我们已经将`analysis_fail: True`添加到配置中。当此项设置为 true 时，还将根据 anchore-engine 附带的默认策略包对分析的图像进行评估。如果策略评估的结果失败，管道将在这一步失败。

```
version: 2.1
orbs:
  anchore: anchore/anchore-engine@1.0.0
jobs:
  local_image_scan:
    executor: anchore/anchore_engine
    steps:
      - checkout
      - run:
          name: build container
          command: docker build -t ${CIRCLE_PROJECT_REPONAME}:ci .
      - anchore/analyze_local_image:
          image_name: ${CIRCLE_PROJECT_REPONAME}:ci
          timeout: '500'
          analysis_fail: True 
```

## 结论

通过上面的例子，我们可以看到如何无缝地将这些扫描添加到我们的 CircleCI 配置中，以便获得容器图像静态分析的好处。

请记住一个小提示:我们明确建议使用 Anchore 策略检查来获得更多的控制，以控制您希望或不希望哪些图像进入管道的下一步。这里的使用情形是，组织通常希望能够围绕正在构建的映像创建治理，并且只允许将合规的映像提升并推送到受信任的注册中心。

要了解更多信息，请注册参加我们即将于 2018 年 12 月 4 日举办的网络研讨会: [Anchore + CircleCI:集装箱安全与合规](https://www2.circleci.com/circleci-anchore-webinar.html)。

* * *

Jeremy Valance 曾担任过 IT 解决方案的管理、设计、开发、实施和测试等多个职位。他目前在 Anchore 担任高级解决方案架构师。