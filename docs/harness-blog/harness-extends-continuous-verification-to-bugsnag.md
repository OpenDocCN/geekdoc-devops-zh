# 线束将持续验证扩展到 Bugsnag |线束

> 原文：<https://www.harness.io/blog/harness-extends-continuous-verification-to-bugsnag>

可以将 Bugsnag 看作是对您的部署管道的质量测试或验证，您希望在您部署代码的每个环境中运行它。

[Bugsnag](https://www.bugsnag.com/) 分析应用程序运行时的事件，特别是错误和异常，这样您可以在几秒钟内调试堆栈跟踪。可以将其视为应用程序可靠性或稳定性监控。数据收集的范围不如 Splunk、Sumo Logic 和 ELK stack，但在诊断数据(堆栈跟踪、面包屑、会话参数)方面更深入。

应用程序开发人员、DevOps 工程师和 sre 通常使用这样的工具来监控和测试他们的本地构建、沙箱和环境。

## 为什么要将 Bugsnag 集成到您的部署管道中？

可以将 Bugsnag 看作是对您的部署管道的质量测试或验证，您希望在您部署代码的每个环境中运行它。

简单地说，我刚刚部署的代码是否给构建或工件带来了任何新的错误或异常？
如果是，Harness 可以标记并显示在部署环境中引入的新错误/异常，终止管道并自动回滚到上一个工作版本。如果没有，线束部署管道可以继续提升代码，直到它到达生产中的客户。

我们的客户之一，[Build.com](https://www.build.com/)请求 Bugsnag 支持，以便他们能够将我们的持续验证扩展到他们的开发、qa、试运行和生产[环境](https://harness.io/blog/deployment-environments/)中的 New Relic 和 Sumo 逻辑之外。

## 线束故障持续验证

这是一个针对您的部署工作流和管道的 Bugsnag 验证的截图:

因此，现在，当您部署新代码时，Harness 将立即连接到您的 Bugsnag 服务器，并将机器学习算法应用于您应用程序的数据集。异常和回归将被自动标记(如上面红色所示),并能够在您的部署环境中查看所有堆栈跟踪。工具之间不再有上下文切换！

利用 ML 算法可以标记由部署引入的新错误或异常，以及错误/异常频率的任何变化。

利用 Harness，每个部署工作流可以运行的验证次数没有限制，因此可以使用所有您喜欢的工具，如 AppDynamics、New Relic、Dynatrace、Datadog、Splunk、Sumo Logic、ELK 和 now Bugsnag，同时验证性能、质量和安全性。

## 配置 Bugsnag 连接器

只需转到**设置>连接器>添加连接器**

现在输入连接器的 **Bugsnag API URL、认证令牌**和**名称**:

您的 Bugsnag 连接器现已配置完毕:

您现在可以向任何部署工作流添加 Bugsnag 验证:

只需点击“添加验证”并从 Bugsnag 连接器对话框中选择应用程序，这将确保 Harness 具有适合您的应用程序部署的正确上下文:

我们的许多客户使用验证来支持 canary 部署阶段。

例如，金丝雀阶段 1:向 10%的基础设施部署新代码，如果没有引入新的错误或异常，则进入金丝雀阶段 2:向 50%的基础设施部署新代码，依此类推。

## 今天就开始

你可以报名免费试用[线束](https://app.harness.io/auth/#/signup)和[障碍](https://www.bugsnag.com/)。

干杯，
史蒂夫。
@伯顿说