# 为 Backstage | Harness 引入 Harness CI/CD 插件

> 原文：<https://www.harness.io/blog/introducing-harness-ci-cd-plugin-for-backstage>

借助 Harness CI/CD Backstage 插件，开发人员可以看到 Harness 管道在 back stage 内部的执行情况。

今天，我们将为 Backstage 发布 Harness CI/CD 插件。该插件是开源的，是将 Harness 与 Backstage 集成的众多插件之一。

[后台](http://backstage.io/)是一个搭建开发者入口的开放平台。Backstage 由一个集中的软件目录提供支持，恢复基础设施的秩序，并使产品团队能够快速交付高质量的代码，而不会影响自主性。一些后台用例包括:

*   构建内部开发人员门户
*   面向各种自动化工作流的开发人员自助服务中心
*   根据公司的最佳实践选择和创建新的软件组件
*   专注于他们拥有的服务，并在一个位置查看相关信息，如构建、部署、警报和文档

借助 Harness back stage 开源插件，开发人员可以在后台看到 Harness 管道的执行情况。此外，开发人员可以在软件目录中的服务主页旁边查看后台的最新执行情况。

CI/CD 插件的第一个版本允许您将 Backstage 上的服务与 Harness 项目连接起来。该插件显示了项目内部管道最近的执行情况。对于每次执行，您可以看到状态、提交和时间等详细信息。如果执行失败，还可以重新运行管道。还有一种方法是使用右上角的过滤器在管道中进行搜索。如果您的项目有大量管道，您可以指定使用哪些管道在 Backstage 上显示执行。您可以在[插件库](https://github.com/harness/backstage-plugins/tree/main/plugins/harness-ci-cd)上的安装说明中找到更多配置选项。

这仅仅是开始。我们将继续发布更多插件，以将[利用平台](https://harness.io/products/platform)与 Backstage 集成，用于与功能标记、管理云成本和混沌工程相关的用例。我们还将在 [Apache 2.0](https://www.apache.org/licenses/LICENSE-2.0) 许可下启动该项目，因此欢迎开源社区随时做出贡献。我们相信贡献不仅仅是通过提交代码来完成的，还可以通过建议功能和就我们如何改进给出诚实的反馈来完成。欢迎所有反馈！

如果你使用 Harness 和 Backstage，现在就试试我们的 CI/CD 插件。前往 [GitHub 库](https://github.com/harness/backstage-plugins)。