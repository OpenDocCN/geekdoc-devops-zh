# 利用持续集成和吉拉| Harness 实现 CI 工作流标准化

> 原文：<https://www.harness.io/blog/harness-jira-integration>

Harness 内置了与吉拉的集成，本文将通过一个简单的设置和示例演示它们如何协同工作。

[持续集成(CI)](https://harness.io/blog/continuous-integration/what-is-continuous-integration/) 和[持续交付(CD)](https://harness.io/blog/continuous-delivery/what-is-continuous-delivery/) 在任何组织的开发运维之旅中都占据了重要位置。对于大多数公司来说，这一切都始于建立一个持续集成平台，该平台与问题跟踪工具紧密集成。Atlassian 的[吉拉](https://www.atlassian.com/software/jira)平台已经被许多行业广泛采用。Harness 有一个请求，今天，我们将通过一个简单的设置和示例来看看 [Harness CI](https://harness.io/products/continuous-integration) 和吉拉是如何协同工作的。

虽然开发人员在一个组织中从事不同的项目，并不断地为他们做 CI，但他们需要一种标准化的方法来确保整个工作流按预期运行。吉拉批准使得开发人员、经理和代码审查人员可以轻松地理顺 CI/CD 工作流。

在本教程的第一阶段，让我们构建、测试并把我们的工件推送到 Docker 注册中心。由于 Harness CI 与语言无关，因此您可以用任何语言编写应用程序来构建工件。线束 CI 的 DSL 是 YAML。

我们将连接到您的 Git 存储库并选择要构建的分支，一旦完成，Harness CI 将通过 [Harness Test Intelligence](https://harness.io/blog/continuous-integration/test-intelligence/) 运行测试，这是 Harness CI 模块中的一项功能，它使用机器学习来自动选择需要运行的测试。测试智能只指定相关测试的子集，跳过其余的，而不是运行所有的测试，这可能需要几个小时。测试智能非常智能，它以最佳顺序排列测试，以提高故障检测率。这是一项强大的功能，也是 CI 解决方案领域的游戏规则改变者。

一旦测试通过 Test Intelligence 运行，Harness CI 就会构建并将构建映像推送到任何容器存储库(例如，Docker Hub)。然后，您可以通过 Harness UI Pipeline Studio 编辑您的管道，这样可以很容易地进行试验，并快速查看工作情况。

内含的 YAML 编辑器提供了类似 IDE 的体验，使编辑管道和实际理解事情变得容易。它还具有自动完成和模式验证功能，这是防止人为错误的额外措施。

## 辅导的

首先，我们初始化。在这里，所有需要的东西都将被设置。

我们克隆拉取请求。

在这个阶段，单元测试将贯穿测试智能。这有助于您获得关于代码质量的快速反馈，并通过帮助快速失败和在开发阶段早期找到 bug 来减少必要的工作。

最后，我们构建映像并将其推送到 Docker Hub。

您可以访问 Docker Hub，检查映像是否按照 CI 管道中的建议进行了推送。

## 吉拉一体化

与吉拉的线束集成支持您的所有票务需求。

您可以通过两种方式使用吉拉审批:

1.  向任何 CD 或批准阶段添加吉拉批准阶段
2.  添加包括吉拉创建、吉拉审批和吉拉更新步骤的吉拉审批阶段。

下面是部署阶段后吉拉创建、批准和更新的一个简单示例。

单击添加阶段。

添加批准阶段。

选择吉拉并设置舞台。您还可以从执行阶段单独添加创建和更新阶段。

然后，您将看到以下屏幕。填写所需的详细信息。

以下是填写详细信息后自动创建吉拉机票的一些示例:

您可以为批准标准-条件或 JEXL 表达式使用任何指定的方法。我想指出的是，问题的关键是使用一个表达式，一个变量是在前面的阶段设置的。在本教程中，我们使用 JEXL 表达式作为审批标准。您不应该使用 JEXL 作为审批标准。条件应自动提示您输入问题字段和状态。

保存并运行管道。它一直运行到批准阶段，并等待管道的批准。

批准者现在应该在吉拉收到通知，他们应该能够批准或拒绝管道。

批准后，检查日志。您应该看到这样的效果:

完整的步骤如下所示:

## 结论

利用 Harness，我们可以轻松地集成吉拉和自动化任务，以帮助开发、运营和测试团队协作，减少管理和跟踪组织中的任务和问题的额外负担。

我们很乐意帮助您的团队变得更加高效。[请求您的演示](https://harness.io/demo/)，[下载免费试用](https://app.harness.io/auth/#/signup/?module=ci)，或者访问 Atlassian marketplace 的[Harness](https://marketplace.atlassian.com/vendors/1221408/harness-inc)，看看 Harness 如何帮助您实现软件交付过程的现代化。