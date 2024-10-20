# 移动和浏览器测试:设置简单、管理方便、信心增强| CircleCI

> 原文：<https://circleci.com/blog/mobile-and-browser-testing-on-circleci-simple-setup-easy-management-increased-confidence/>

测试是开发生命周期中最重要的部分之一，但是对于拥有在许多不同设备或浏览器上运行的应用程序的公司来说，这是非常棘手的。此外，编写测试可能需要大量的时间投入，这对试图快速进入市场的团队来说是一个挑战。

这就是为什么许多团队转向 CI/CD 来自动化和简化测试。尽管如此，编写大量有价值的测试可能很困难。为了帮助解决这一挑战，我们与各种测试工具合作，为 CircleCI 用户创建预构建的集成。这些浏览器和移动测试集成允许用户在测试创建和管理上节省时间和精力。

我们广泛的列表[orb](https://circleci.com/orbs/)帮助团队以更少的努力建立可靠的测试。如今市场上有如此多的测试工具和理念，我们希望给予团队选择最适合他们的工具所需的灵活性。最终，这些预先构建的集成是为了帮助团队快速、轻松地建立复杂的测试，这样他们就可以专注于开发。

查看我们的 web 和移动测试平台，直接从您的 CI/CD 渠道帮助促进和管理浏览器和移动环境测试:

## 浏览器测试集成

| Orb 链接 | Orb 描述 |
| --- | --- |
| [柏树](https://circleci.com/developer/orbs/orb/cypress-io/cypress) | 运行 Cypress.io 端到端浏览器测试，无需花费时间配置 CircleCI。这个 orb 在 Cypress 仪表板上记录结果，并以并行模式进行负载平衡测试。 |
| [酱实验室](https://circleci.com/developer/orbs/orb/saucelabs/sauce-connect) | 自动设置内置的 http 代理，允许轻松访问防火墙后的开发/qa 站点。代理在构建开始时作为后台任务启动，在构建结束时结束连接。 |
| [雨林 QA](https://circleci.com/developer/orbs/orb/rainforest-qa/rainforest) | 在每次提交时集成并运行 RainforestQA 测试用例。 |
| [LambdaTest](https://circleci.com/developer/orbs/orb/lambdatest/lambda-tunnel) | 通过在 CI/CD 管道中集成 LambdaTest，在 2000 多种浏览器和操作系统上执行自动跨浏览器测试。 |
| [目录](https://circleci.com/developer/orbs/orb/katalon/katalon-studio) | 利用 Katalon 和 CircleCI 之间的无缝集成，在 CI/CD 中为任何浏览器运行自动化测试。Katalon orb 允许使用强大的端到端测试框架进行跨浏览器和跨平台测试。 |
| [酸](https://circleci.com/developer/orbs/orb/happo/happo) | 通过 Happo.io 将截图测试直接集成到您的 CI/CD 管道中。 |
| [氧气](https://circleci.com/developer/orbs/orb/cloudbeat/oxygen) | 使用 Oxygen 作为 CircleCI 构建流程的一部分，运行集成和端到端测试。 |
| [幽灵探长](https://circleci.com/developer/orbs/orb/ghostinspector/test-runner) | 在 CircleCI 项目上执行 Ghost Inspector 浏览器测试。 |
| [Testim](https://circleci.com/developer/orbs/orb/testimio/runner) | 在代码提交或合并时运行 Testim 端到端、跨浏览器 UI 测试，无需花费时间配置 CircleCI。Testim orb 允许团队从人工智能稳定的测试中受益，而无需离开他们的 CircleCI 工作流。 |
| [PreFlight](https://circleci.com/developer/orbs/orb/preflight/test-runner) | 在您的 CircleCI 版本中，跨不同的环境、浏览器和屏幕大小执行印前检查的自动化 UI 测试。 |

## 移动测试集成

| Orb 链接 | Orb 描述 |
| --- | --- |
| [酱实验室](https://circleci.com/developer/orbs/orb/saucelabs/sauce-connect) | 自动设置内置的 http 代理，允许轻松访问防火墙后的开发/qa 站点。代理在构建开始时作为后台任务启动，在构建结束时结束连接。 |
| [雨林 QA](https://circleci.com/developer/orbs/orb/rainforest-qa/rainforest) | 在每次提交时集成并运行 RainforestQA 测试用例。 |
| [目录](https://circleci.com/developer/orbs/orb/katalon/katalon-studio) | 通过 Katalon 和 CircleCI 之间的无缝集成，在 CI/CD 中创建和运行移动测试。Katalon orb 支持在所有操作系统和环境上使用 Appium 兼容的测试框架进行测试。 |
| [现在安全](https://circleci.com/developer/orbs/orb/nowsecure/ci-auto-orb) | 将 CircleCI 工作流与 NowSecure orb 集成，以自动启动每个移动应用版本的安全性和隐私测试。 |
| [元宵节](https://circleci.com/developer/orbs/orb/genymotion/genymotion-saas) | 启动 Genymotion Android 虚拟设备，通过 ADB 连接，并在 Genymotion 云 SaaS 上停止设备，以进行移动自动化测试(web 和本地应用程序)。 |
| [氧气](https://circleci.com/developer/orbs/orb/cloudbeat/oxygen) | 使用 Oxygen 作为 CircleCI 构建流程的一部分，运行集成和端到端测试。 |
| [Testfairy](https://circleci.com/developer/orbs/orb/testfairy/uploader) | 将 Android 和 iOS 应用程序上传到 TestFairy，并轻松分发给用户。 |

阅读更多关于[移动应用开发持续集成](https://circleci.com/blog/ci-for-mobile-app-development/)的信息。

> “CircleCI 非常适合移动开发团队的独特移动需求。它们提供了关键功能，不仅能够实现高效的开发生命周期，还能完成 Apple AppStore 和 Google Play 提交流程。我们的移动客户在其 DevSecOps 流程中利用 CircleCI 和我们的 NowSecure orb 来确保内置安全性。”Brian C. Reed，NowSecure 首席移动官

> “CircleCI 的 orb 概念使我们能够轻松地为用户提供无缝的 CI/CD 执行流程。通过最低的设置要求，Katalon 用户能够扩展他们的测试范围，实现整体 CI/CD 管道，并为他们的项目利用强大的测试框架。”Dzung Ngo，产品副总裁，Katalon。

> “Testim 很高兴与 CircleCI 合作，帮助现代开发团队更快地进行测试和发布。这种集成消除了开发和测试过程之间的摩擦，从而提高了软件质量并加快了交付速度。”Testim 创始人兼首席执行官柳文欢·鲁宾

好奇想了解更多？请访问 [Orb 注册表](https://circleci.com/developer/orbs)或查看这些点播网络研讨会: