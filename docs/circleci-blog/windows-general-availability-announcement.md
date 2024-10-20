# Windows CI - Windows 支持| CircleCI

> 原文：<https://circleci.com/blog/windows-general-availability-announcement/>

在 CircleCI，我们帮助公司加速创新。虽然 2019 年 StackOverflow 开发者调查参与者中有一半以上是为 Windows 构建的，但我们根本没有为 Windows 社区提供服务。

在过去的几年里，我们对构建 Windows 项目越来越感兴趣。从。NET Core 1.0 和 2016 年 Visual Studio 代码的发布，到即将发布的 WSL2 和 unification。网芯和。2020 年，一些最令人兴奋的开发人员工具创新正在微软生态系统中发生。今天，我们很高兴地告诉大家，Windows 支持现已在 CircleCI 上普遍提供。

## 支持 Windows 作业

我们的核心功能现在可用于 Windows 环境。缓存、工作空间、审批作业和上下文等功能，以及同样强大的支持和漂亮的用户界面，现在都可以用于 Windows 作业。Windows 作业基于虚拟机，并提供完全的构建隔离。每个新作业都使用一个干净的环境，这个环境是及时创建的，一旦作业运行完毕就会被销毁。这确保了在 CI 环境中构建的可再现性以及代码、数据和秘密的安全性。CircleCI 上的 Windows 环境还支持基于 Docker 的 Windows 工作流的 Docker Engine - Enterprise。

现在，您可以在一个工作流程中使用 Windows、Linux 和 macOS 作业。在这个版本中，CircleCI 是构建跨平台应用程序的标准 CI 解决方案。现在，所有绩效计划客户都可以使用 Windows 作业。点击了解有关我们绩效计划[的更多信息。](https://circleci.com/pricing/)

## 你能做什么

想知道执行环境到底是什么样的？前往 [CircleCI for Windows 文档页面](https://circleci.com/docs/hello-world-windows/)。

您是现有客户吗？您需要帮助转向性能定价吗？点击联系我们的销售团队[。](https://circleci.com/contact-us/)

您是新客户吗，您想在 CircleCI 上试用 Windows 吗？点击[这里](https://github.com/CircleCI-Public/circleci-demo-windows)观看演示项目。

我们期待着根据您的反馈改进我们对 Windows 的支持。如果你有想法，请在以下地方与我们分享:

*   要报告我们的 Windows 支持中的错误，请前往我们的讨论网站[这里](https://discuss.circleci.com/c/bug-reports)。
*   要建议 Windows 支持的新功能，请在我们的[创意门户](https://ideas.circleci.com/)中提交新创意。
*   我们也想知道你对 CircleCI 上的 Windows 支持的总体想法。请填写[这张](https://docs.google.com/forms/d/17pvPu4WbbjGtHoyoHs3Nw6L8DbhQPRvbKPhJxP_Y-Ek/viewform?edit_requested=true)表格，并附上您想要分享的任何反馈。我们的一位产品经理将很快回复您。

在我们改进 Windows 解决方案的同时，我们也期待微软生态系统的进一步创新。我们的 Windows 路线图包括在 Windows 上使用 Linux 容器，以及为移动开发预装依赖项的容器。如果您的团队迫切需要获得额外的功能，请在这里告诉我们[。](https://ideas.circleci.com/)