# 为您的 CI 管道|线束提供 3 个插件

> 原文：<https://www.harness.io/blog/3-plugins-to-consider-for-your-ci-pipeline>

在本帖中，我们将分享你应该考虑集成到你的持续集成(CI)管道中的 3 个无人机插件。

用于 CI/CD 的平台 Harness 为您的软件交付生命周期提供了企业级特性。我们很高兴地欢迎 Drone，一个[持续集成(CI)平台](https://harness.io/continuous-integration/)加入 Harness 家族，以扩展 Harness CI 功能。在以前的帖子中，我们已经分享了[如何开始使用无人机](https://harness.io/2020/08/your-first-harness-ci-installation/)。然而，在绿地或棕地环境中，在竞争情报平台上创建一个强大的竞争情报管道可能是一个挑战。在本帖中，我们将分享 3 个你应该考虑集成到你的[持续集成](https://harness.io/blog/what-is-continuous-integration/)管道中的无人机插件。

## 第一名:Sonarqube:

CI 管道是质量工程和保证的支柱，因此 CI 流程应该自动化，并且[将测试](https://harness.io/2020/08/testing-with-continuous-integration/)作为基石。

Sonarqube 是一个流行的开源代码质量管理平台。它提供代码分析和对安全漏洞和错误的洞察，如下面 SonarQube 仪表板的屏幕截图所示。

首先，您需要设置 SonarQube 平台和扫描仪。然后修改您的 pipeline yaml 配置，以利用[Drone sonar cube 插件](http://plugins.drone.io/aosapps/drone-sonar-plugin/)，如下所示。

#2:克莱尔

## 确保集装箱的安全是 DevOps 工程师和安全团队的首要任务。扫描集装箱图像是减少恶意软件和安全漏洞的一种做法。有各种开源和供应商解决方案可用，如 Clair、Dagda 和 Twistlock。

Clair 是一个开源项目，通过漏洞和安全扫描来保护容器图像。将容器扫描合并到您的 CI 管道中很容易，使用 Drone 的 Clair 插件(【http://plugins.drone.io/jmccann/clair/】T2)通过以下示例编辑您的管道配置。

#3:松弛

## CI 的最佳实践包括快速解决不完整的构建。通过 [Slack 插件](http://plugins.drone.io/drone-plugins/drone-slack/)启用该练习和交流。点击[这里](https://my.slack.com/services/new/incoming-webhook)获得一个进入你的 Slack 工作空间和频道的 webhook URL。然后使用下面的 yaml 示例修改管道配置:

在这里，我们使用 slack 插件作为帖子的个人资料图像，并发布到 dev slack 频道。

免费试用无人机

Get automated Drone notifications on Slack.

## 这篇博客文章分享了如何使用 [Harness CI / Drone](https://drone.io/) 在几分钟内构建强大的 CI 渠道。Drone 是一个开源的 CI 解决方案，您可以免费使用。如果你有兴趣通过构建自己的无人机插件来为开源无人机社区做贡献，请从这里开始:【https://github.com/drone/drone-plugin-starter。

这篇博客文章分享了如何使用 [Harness CI / Drone](https://drone.io/) 在几分钟内构建强大的 CI 渠道。Drone 是一个开源的 CI 解决方案，您可以免费使用。如果你有兴趣通过构建自己的无人机插件来为开源无人机社区做贡献，请从这里开始:【https://github.com/drone/drone-plugin-starter。