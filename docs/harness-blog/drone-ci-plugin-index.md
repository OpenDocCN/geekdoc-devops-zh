# 介绍新的无人机 CI 插件索引|线束

> 原文：<https://www.harness.io/blog/drone-ci-plugin-index>

我们新设计的插件市场在这里！阅读所有相关内容。

今天，我们很兴奋地在 [plugins.drone.io](https://plugins.drone.io) 发布全新设计的无人机 CI 插件索引！

插件索引列出了提交给[drone-plugin-index](https://github.com/drone/drone-plugin-index)GitHub 库的官方插件(由 Harness 开发)和社区支持的插件。通过这种重新设计，所有插件都可以很容易地按类别和预期用途进行过滤，从而更容易为您的管道步骤找到正确的插件。当然，外挂不仅限于无人机 CI，在[驾驭 CI](https://harness.io/products/continuous-integration) 中也可以使用！经过重新设计，您现在将看到用于无人机配置项和线束配置项的示例 YAML 语法。

无人机 CI 插件是 Docker 容器，执行预定义的任务，并被配置为您的[管道](https://docs.drone.io/pipeline/overview/)中的[步骤](https://docs.drone.io/pipeline/docker/syntax/steps/)。通过抽象出不必要的复杂性，他们可以显著减少执行 CI 管道所需的重复和脚本数量。如果你能建立一个 Docker 镜像，你就能建立一个无人机 CI 插件！

为 [Go](https://docs.drone.io/plugins/tutorials/golang/) 和 [Bash](https://docs.drone.io/plugins/tutorials/bash/) 都提供了插件教程。你也可以观看这个关于[构建你的第一个无人机插件](https://youtu.be/JJgkX9ZYPpY)的演示，在最近的 [Harness &无人机用户组](https://www.meetup.com/harness/) meetup 上发表，或者阅读我们最近的博客文章[如何为 CI](https://harness.io/blog/continuous-integration/write-first-plugin-for-cie/) 编写你的第一个插件。

有关将插件与线束配置项一起使用的更多信息，请参见[在配置项中运行无人机插件](https://ngdocs.harness.io/article/fjagoj8mez-run-a-drone-plugin-in-ci)。