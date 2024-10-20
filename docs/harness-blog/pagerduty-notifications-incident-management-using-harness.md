# 使用线束|线束为事件管理设置寻呼责任通知

> 原文：<https://www.harness.io/blog/pagerduty-notifications-incident-management-using-harness>

了解如何将 PagerDuty 事件管理工具与 Harness 集成，以便清楚地了解您的 CI/CD 管道事件。

使用 Harness 为事件管理设置寻呼责任通知

如今，每个组织都有一个工程师团队努力工作以保持系统正常运行，他们必须创建处理不可避免的事件的流程。即使您有最好的开发人员和站点可靠性工程师(sre ),管理和解决这些事件也需要时间和精力。然而，与所有事情一样，如果您有正确的工具和策略，事件响应管理起来会容易得多。每个组织都需要将同类最佳的[持续集成](https://www.harness.io/products/continuous-integration)和[持续交付](https://www.harness.io/products/continuous-delivery) (CI/CD)平台与最佳事件管理工具相结合。

如果您是一名 IT 专业人员，感觉您的团队一直在努力应对警报和事件，您可能会觉得事件管理是您团队的主要摩擦点。本文将展示如何将事件管理工具[page duty](https://www.pagerduty.com)与 Harness 集成，以清晰地查看您的 CI/CD 管道事件。

## 什么是事件管理？

[事件管理](https://www.gartner.com/en/information-technology/glossary/it-incident-management)是识别、调查和解决影响服务可用性的事件的过程。事件管理流程从中断或事件开始，这些中断或事件会触发来自监控工具或操作人员手动检测的警报。然后，事件经理评估对客户和业务的影响，然后根据问题的严重性、受影响的对象以及解决问题需要多长时间来决定如何应对。事件管理是[开发运维](https://www.harness.io/blog/sre-vs-devops)的关键组成部分。它包括为响应事件并减少其对业务运营的影响而采取的步骤。

CI/CD 管道是管理软件开发过程的一系列步骤。它包括自动化测试、CI 和[部署](https://www.harness.io/blog/blue-green-canary-deployment-strategies)。管道旨在缩短开发代码并将其部署到生产环境中的时间。在 CI/CD 管道中的某个位置配置事件管理可以完善您的管道，因为它有助于您监控事件并立即获得通知。

## 为什么选择事件管理？

事故管理非常重要，因为它有助于组织维护其系统和服务的可用性、可靠性和安全性。通过快速识别和解决事故，组织可以主动最大限度地减少事故造成的停机时间，并确保继续满足客户的需求和期望。事件管理是成功的开发运维战略不可或缺的一部分，因为它有助于确保及时、高效且经济高效地解决任何问题或事件。

事件管理不仅有助于开发人员，也有助于安全团队通过增强的安全性和合规性来填补任何安全缺口。此外，它还监控应用程序和服务的任何错误或关闭。如果发生故障，事件管理工具会提醒负责的团队成员。然后，通过团队决定的策略集，有问题的应用程序或服务将被立即恢复。因此，事故管理工具在您的 CI/CD 渠道中至关重要。

## 事故管理策略

成功的事件管理策略应该包括自动化、协作和通信工具。此外，事件管理包括尽可能快速高效地响应、解决和记录任何[服务中断或故障](https://www.harness.io/blog/delivering-reliability-through-sre-practices)。事故的范围从小的软件故障到大的系统故障，一个深思熟虑的策略对于确保系统的稳定性和安全性是必要的。

实施正确的事件管理策略可以帮助开发运维团队掌控问题并确保服务连续性。DevOps 团队可以快速高效地响应任何事件，确保系统保持稳定和安全。有了事件管理，工程组织可以节省大量资金，并避免一些紧张的情况。

为了举例说明事件管理策略可能是什么样子，以下是四个高级步骤:

1.  监控 CI/CD 渠道，以识别可能出现的任何问题。当检测到问题时，应该生成警报，以便通知相关的团队成员。
2.  收到警报后，对问题进行分类，以确定其严重性和影响。如果问题被认为很严重，应该上报给相应的团队进行进一步的调查和解决。
3.  尽快解决问题，并逐步记录成功的实施。
4.  需要就此事件召开后续会议，以确保所有问题都得到解决，系统恢复正常运行。

这是一个事故管理策略的简化示例。实际上，您会希望根据您的[服务级别协议(SLA)](https://www.harness.io/blog/introduction-service-level-objective-slo-management)、[服务级别目标(SLO)](https://www.harness.io/blog/introduction-service-level-objective-slo-management)、[服务级别指标(SLIs)](https://www.harness.io/blog/introduction-service-level-objective-slo-management) 和[错误预算](https://www.harness.io/blog/slo-error-budgets)来定制您的方法。

## 什么是页面责任？

PagerDuty 是一个基于云的 IT 事故管理工具，有助于通知和协调整个组织的事故响应。以下是 PagerDuty 的一些特性和优点:

*   帮助确定事件的紧急程度，并相应地将其分配给工程师，以便他们采取适当的措施
*   当事件有更新时，提供实时通知
*   允许用户分配任务并跟踪其进度
*   启用特定事件的通知

## 教程:如何使用 Harness 发送寻呼机工作警报

在本教程中，我们将演示如何在管道执行期间检测到事件的情况下发送 PagerDuty 警报。

当开发人员向[源代码控制管理(SCM)](https://www.harness.io/blog/harness-source-code-manager-scm) 系统提交代码时，如 [GitHub](https://github.com/) ，CI/CD 工具将构建并测试代码中的任何错误配置、错误或漏洞。然后代码被部署到目标环境，比如 Kubernetes。在这里，我们将配置警报通知，以便当不成功的部署发生时，管道将通知 PagerDuty 来警告开发人员和负责采取进一步行动的团队。

从克隆代码到持续集成，再到将其部署到目标环境并提醒负责人员，利用是整个设置的核心。

### 教程先决条件:

‍ ***注意:*** 内置通知是 Harness 平台的一项功能，因此，它们可以应用于任何阶段类型。

### 线束设置

如果你是马具新手，你需要有一个免费的[马具账号](https://app.harness.io/auth/#/signup)。

接下来，登录您的 Harness 帐户并选择 [CD](https://www.harness.io/products/continuous-delivery) 模块。建立一个简单的管道来部署应用程序。我们将使用 Kubernetes 集群来部署我们的示例应用程序。

‍

为您的项目添加所需的连接器，比如您的 GitHub 存储库(我们使用一个[示例应用程序](https://github.com/pavanbelagatti/Simple-Node-App)；可以分叉使用)和一个委托。 [Harness Delegate](https://developer.harness.io/docs/platform/Delegates/get-started-with-delegates/delegates-overview) 是您在本地网络或 VPC 中运行的服务，它通过 Harness 连接您的所有工件、基础设施、协作、验证和其他提供者。

一旦设置了项目和部署应用程序的管道，就该设置通知了。在控制面板的右侧，您会看到一个通知图标。单击图标并设置您的事件通知策略。

正如我们上面讨论的，我将向您展示如何集成页面责任通知。

添加通知。

安装程序将询问您希望收到什么事件的通知。我只是为任何部署设置通知，也就是说，对于应用程序/服务的任何成功部署，都会生成通知。

接下来，我们将选择 PagerDuty 作为通知方法。

一旦您选择了 **PagerDuty** 作为您的通知工具，您需要在下一步中提供 PagerDuty 键。

要获得寻呼机工作密钥，你需要进入你的[寻呼机工作](https://www.pagerduty.com/home/)账户。

### 设置寻呼机工作

在[page duty](https://www.pagerduty.com/)上创建一个帐户，并提供一个服务名以继续。

在下一步中，您将设置集成。转到“全部”选项卡并搜索线束，然后选择如下所示的线束。

下一步将询问您希望如何获得通知。我把我的电子邮件地址。参见下面由 PagerDuty 发送的电子邮件示例。

接下来，转到服务并单击服务目录以查看您的服务配置。您应该会看到您添加的服务。

点击管理，然后点击查看集成查看您的密钥。

集成密钥是我们需要的。复制并保存它。

### 向线束添加寻呼任务

回到 Harness 应用程序，将密钥添加到您的寻呼机工作通知选项卡。

测试您的连接是否成功。

保存更改并应用它们。然后，转到您的线束仪表板，运行管道。

在成功部署我们的应用程序后，我们现在应该在 PagerDuty 上看到事件通知，并收到一封电子邮件响应。

‍

同样，您可以自定义您的工作流程和事件，指派人员调查事件，等等。PagerDuty 集成可以帮助您的支持和工程团队跟踪正在发生的事情，以便尽快解决问题。

## 事件管理对于 CI/CD 至关重要

一旦您部署了您的服务，您需要知道发生了什么，并向您的团队报告不同的事件。拥有事故管理工具是任何 CI/CD 平台的重要补充，可确保一切按预期进行。PagerDuty 是众多可以与 Harness 集成的通知工具中的一个很好的例子，将您的事件管理提升到了一个新的水平。

如需更多线束产品教程，请查看[线束开发中心](https://developer.harness.io/)，了解更多关于[线束 CD 特性](https://www.harness.io/product-features/cd-gitops)，或[立即免费试用](https://app.harness.io/auth/#/signup/?module=cd)。