# 将自动化安全性和测试集成到您的 CI/CD 管道|线束中

> 原文：<https://www.harness.io/blog/integrating-automated-security-testing-ci-cd-pipeline>

我们将向您展示如何使用 Harness 平台将您选择的安全工具、套件和框架集成到您的持续集成和交付(CI/CD)管道中。

安全性不再是事后的想法，它是我们[持续集成和交付(CI/CD)管道](https://harness.io/blog/container-pipelines)不可或缺的一部分。虽然 [DevOps 实践](https://harness.io/blog/gitops-vs-devops)更关注敏捷性和速度，但 [DevSecOps](https://harness.io/blog/pipeline-security-devsecops-catalyst) 确保安全性紧密嵌入交付渠道。竞争非常激烈，客户快速要求新功能。决定新软件和特性发布给客户的速度的是组织的技能和他们使用的工具。

由于有如此多的工具，决定如何将安全性集成到您的 DevOps 工具链中有时会让人不知所措。今天，我们将向您展示如何使用 Harness 平台将您选择的安全工具、套件和框架集成到您的 CI/CD 管道中。

## CI/CD 管道中的安全性

CI/CD 是一个过程，在这个过程中，一旦进行新的提交，就自动构建和部署代码。该过程通常包括构建代码、运行测试用例、执行静态代码分析，以及最终部署应用程序。有许多工具运行在 CI/CD 管道之上，帮助公司自动化他们的工作流程。为了在您的 CI/CD 管道中自动化安全测试，您将需要一种自动化的方法来对您的代码运行安全扫描。这可以通过将不同的安全工具集成到您的 CI/CD 管道中来实现。

### CI/CD 管道中的自动化手动测试

手动测试是一个耗时的过程，并且不可扩展。虽然在每次发布之前做一些手动测试是个好习惯，但是只要有可能，就应该自动化。测试可以通过以下步骤实现自动化:

1.  设置一个反映生产环境的测试环境。一旦设置好环境，您就可以开始添加测试用例来执行您的测试。
2.  接下来，您需要确定需要测试什么。您可以使用[风险分析](https://www.codingninjas.com/codestudio/library/risk-analysis-in-software-development)来识别代码中需要关注的区域。风险分析是发现代码漏洞、稳定性和性能问题，如果有的话。风险分析帮助您对测试进行优先级排序，并确保您不会错过任何关键的测试用例。
3.  一旦您准备好了您的测试用例，您需要在自动化测试之前手动设置您的测试数据。一旦测试数据准备好了，您就可以将测试工具集成到您的 CI/CD 管道中。
4.  另一个选择是，在“构建和测试”阶段集成不同的测试工具和套件，CI 工具将运行所有的测试并构建代码。选择一个包含这些集成能力和定制测试能力的现代工具是非常重要的。这就是 Harness 这样的平台帮助组织应对安全和漏洞威胁的地方。

### 测试工具和参数

并行运行多个安全性测试是一个很好的实践，我们将在本教程中演示这一点。让我们看一些我们将要使用的测试套件和工具的例子。

*   可断言的
*   Snyk
*   Pm2
*   索纳库贝
*   浏览器堆栈
*   AppSignal

让我们一个一个来看看它们是什么。

### 可断言的

Assertible 确保您的 API 和网站的正常运行时间和可用性，以及您的数据的正确性。

### Snyk 试验

Snyk 在您的应用程序中发现开源漏洞和许可证问题。

### Pm2 测试

PM2 是 Node 的生产流程经理。带有内置负载平衡器的 js 应用程序。它允许您永远保持应用程序的活力，在不停机的情况下重新加载应用程序，并简化常见的系统管理任务。

### 索纳库贝

Sonarqube 报告您的应用程序的代码质量。

### 浏览器堆栈

BrowserStack 对您的应用程序进行跨浏览器测试。

### AppSignal

AppSignal 对您的应用程序进行错误跟踪和性能监控。

Harness 集成了目前可用的超过 40 种最流行的应用程序安全扫描器。

## 使用线束 CI 的自动化测试

Harness 与所有主要工具集成得很好，并且可以通过在 CI/CD 的不同阶段选择各种工具和平台来相应地定制管道。类似地，当涉及到测试时，CI(在我们的例子中是 Harness CI)工具可以与不同的测试集成在一起，以找到 bug 和漏洞。让我们来看看如何用上述所有安全测试配置 Harness CI。

首先派生示例 Node.js 应用程序:[https://github.com/pavanbelagatti/harness-ci-example](https://github.com/pavanbelagatti/harness-ci-example)

接下来，注册[线束平台](https://app.harness.io/auth/#/signup/?utm_source=internal&utm_medium=social&utm_campaign=community&utm_content=pavan_security_testing_article&utm_term=get-started)免费试用，选择 [CI 模块](https://harness.io/products/continuous-integration?utm_source=internal&utm_medium=social&utm_campaign=community&utm_content=pavan_article_nov&utm_term=get-started)。

创建项目并配置“**构建&测试**阶段。

在“ ***下构建&测试*** ，你可以配置你不同的定制测试套件。使用“**运行**步骤进行如下配置。

‍

您可以使用“**Run”**步骤配置所有不同的测试套件。一旦配置了所有的测试，保存并运行管道。所有的测试都可以很容易地配置，只需点击一下鼠标就可以运行管道。您应该看到管道的成功执行和所有测试的通过。如果发现任何漏洞、缺陷或错误，管道不会继续前进，而是自己停在那里。

‍

你可以看到所有的测试都通过了。

## 将 DevSecOps 和 CI/CD 结合在一起

CI/CD 是一个帮助团队更快、更有效地构建、测试和部署代码的过程。由于工程师必须执行手动测试，因此出错的可能性更大。随着自动化成为 DevOps 最佳实践的中心，执行安全测试自动化的工具更受青睐。毫无疑问，安全测试是 CI/CD 的一个关键部分，并且可以自动化以节省时间和精力。此外，如果操作正确，还可以降低部署不安全代码的风险。

许多工具可以帮助您自动化 CI/CD 管道安全性测试。它们包括漏洞扫描器、代码覆盖分析器、代码审查工具、静态代码分析工具、故障后工具等。您所需要的只是一个像 Harness 这样的具有 CI/CD 和安全测试功能的平台，以确保您的部署 100%安全。

如果您有兴趣了解有关使用 Harness 和智能软件交付的更多信息，[免费试用](https://app.harness.io/auth/#/signup)并查看我们的[开发者中心](https://developer.harness.io/)以获得更多分步教程、视频和参考文档。