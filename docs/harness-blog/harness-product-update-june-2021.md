# 线束产品更新| 2021 年 6 月|线束

> 原文：<https://www.harness.io/blog/harness-product-update-june-2021>

它来了，它走了，它征服了！当然，我们谈论的是{无脚本}-这是多么令人惊奇的两天啊，充满了知识、伟大的演讲者、富有洞察力的专家小组和震撼行业的公告！

它来了，它走了，它征服了！当然，我们谈论的是{无脚本}-这是多么令人惊奇的两天啊，充满了知识、伟大的演讲者、富有洞察力的专家小组和震撼行业的公告！如果你错过了，我们为第一天和第二天的[准备了一些有用的回顾，一定会让你兴奋不已！我们也有一个方便的时髦的](https://harness.io/blog/unscripted/unscripted-2021-day-one-recap/) [YouTube 播放列表](https://www.youtube.com/watch?v=qGue06BQNZU&list=PLXsYHFsLmqf2RcjVlPcapsTyBAsCTz8X7)，所有的会议都可以观看。请阅读下面的内容，了解我们在 2021 年 6 月的产品更新中的更多信息！

## 新功能

### 治理 CI 企业

Harness 已经进入 CI 领域，在其平台上添加了 CI 模块。Harness 从其开源对手 Drone 那里获得了最佳概念，并添加了一些新功能，通过使开发人员更容易构建和测试代码来增强开发人员的体验。新特性包括容器化步骤、可视化管道构建器、YAML 代码编辑器、GitOps 功能、细粒度 RBAC，以及 CI 解决方案中首次出现的测试智能。Test Intelligence 使用 ML 来选择确认触发构建的代码更改的质量所需的测试，并以最佳顺序对测试进行排序，以提高故障检测率。在我们的博客文章中了解更多关于测试智能和 CIE 的许多好处。

*Visual Pipeline Builder*

*GitOps*

*Fine-Grained RBAC*

*Test Intelligence*

### 线束特征标志

随着线束特征标志的引入，线束现在在特征标志空间中有一个产品模块。特性标志使工程团队能够以更低的风险更快地交付特性。它专为开发人员打造，具有简化的开发人员体验，包括可视化管道构建器和自动化功能验证。附加功能包括:布尔和多元标志；跨用户、组、区域和基础设施的灵活定位；流和轮询架构支持；RBAC；和审计跟踪。Harness Feature Flags 是在 Feature Flags 空间中独特设计的，易于上手，可以快速获得您的第一面旗帜。在我们的[功能标志](https://harness.io/blog/product-updates/introducing-harness-feature-flags/)帖子中了解更多信息。

*Feature Flags!*

### 利用 CCM -云自动停止

Harness 现在支持主动云成本管理，其最新添加的云自动停止功能可将与非生产环境相关的云计算浪费减少高达 75%。Harness 可以主动管理您的云计算资源，以便它们在空闲时自动关闭，并在检测到资源流量时重新打开。它还在 spot 实例上提供了完整的编排——这是“设置它，然后忘记它”的最佳状态。在我们的[云自动停止](https://harness.io/blog/product-updates/cloud-autostopping/)帖子中了解更多信息。

*Cloud AutoStopping*

### 利用 Github Webhook 触发器控制 CD 支持秘密

这允许用户使用秘密来认证传入的 GitHub webhooks，以确保 webhooks 来自可信的来源。即使一个非预期的用户可以访问线束触发器的 webhook URL，该用户也不能在 Github 中设置它，因为它也需要秘密值。这提高了触发器的安全性。

### 利用 CD 粒度访问控制进行工作流部署

Harness 添加了回滚工作流的新权限。这允许用户将服务回滚到上次成功部署的版本，或者为正在运行的工作流启动回滚，即使他们没有执行工作流的权限。

## 哈斯林大学

**新增模块和课程:**

我们有**新的**课程，现在在[线束大学](https://university.harness.io)提供。

*   **线束产品概述**
*   **Terraform 101** :线束解决方案架构师**波格丹一世·卡塔纳**带领我们了解 Terraform 的基础知识——起源、资源提供者、命令、状态、输出、变量和插值。
*   **按需网络研讨会:**概念和基础、持续集成、持续交付、云成本管理、持续验证、安全性、功能标志。

**脊甲化活:**

**2021 年 7 月 19 日、20 日**:线束基础知识，太平洋标准时间上午 6-9 点

2021 年 7 月 21 日，22 日:线束管理局，太平洋标准时间上午 6-9 点

要注册 7 月及以后的活动，我们的现场活动日历是[这里是](https://university.harness.io/calendar)。

## 报刊上值得注意的提及

*   **Tiffany Jachja，技术传道者，TNS 和 DevOps.com 特稿:**
*   DevOps.com: [过去与现在:DevOps 文化的演变](https://devops.com/then-and-now-the-evolution-of-devops-culture/)
*   TNS: [当 CI 遇上 CD:放心交付](https://thenewstack.io/when-ci-meets-cd-deliver-with-confidence/)
*   **Ravi Lachmann，技术布道者，DevOps.com 特稿:**
*   DevOps.com: [实现持续集成的现代化](https://devops.com/modernizing-continuous-integration/)
*   **CIE、特征标志和云自动停止公告:**
*   DevOps.com: [Harness 采用 ML 来简化 DevOps 工作流程](https://devops.com/harness-employs-ml-to-streamline-devops-workflows/)
*   ZDNet: [利用更新平台加快软件交付，降低云成本](https://www.zdnet.com/article/harness-updates-platform-to-speed-up-software-delivery-lower-cloud-costs/)
*   TechTarget: [利用特征标志设置线束诱饵 CI/CD，云自动停止](https://searchitoperations.techtarget.com/news/252502582/Harness-baits-CI-CD-set-with-feature-flags-cloud-auto-stop)
*   SD Times: [利用测试智能、特征标志等更新平台](https://sdtimes.com/softwaredev/harness-updates-platform-with-test-intelligence-feature-flags-and-more/)

## 即将举行的虚拟活动

**这完全不是虚拟事件**，但你应该在 7 月 8 日和我们一起观看《黑寡妇》!我们将在美国举办一个私人的预发行活动-你所需要做的就是注册，我们会给你两张票-如果你真的很好的话，可能会更多！我们在美国有 11 个地点。[现在注册](https://harness.io/event-black-widow-viewing)！

## 即将举行的网络研讨会

## 展望未来

很难不想继续回顾六月，因为我们已经发布了这么多新的东西…但是你知道他们怎么说蜜蜂的:他们总是很忙。在接下来的几周/几个月里，你会遇到以下情况:

*   发布部署事件——Harness 将能够在 webhook URL 上发布管道部署事件。用户将能够在事件上设置仪表板，以运行对其部署的分析。

*   通过 API 批准/拒绝线束批准-线束用户将能够通过 API 批准或拒绝批准。这将使他们能够通过脚本实现包含线束批准的部署，而无需通过线束 UI 导航。

*   部署冻结窗口增强-线束现在将允许用户设置每天，每周，每月等定期冻结窗口。基础。此外，用户将能够在服务上设置冻结窗口。这样，将有一个新的许可，允许在冻结期间部署。

*   AWS CloudFormation 的功能支持——用户现在可以定义和使用 AWS CloudFormation 在提供新堆栈时提供的“- capabilities”功能。功能允许用户定义这个堆栈能做什么，这是一种确认堆栈将在用户环境中创建和操作敏感资源的形式。

*   预处理和从任何存储库源获取舵图——用户将能够定义一种方法，说明 Harness 如何获取用户的舵图和值 yaml 以进行部署。该特性的一个常见用例是，需要从特定的来源生成或获取一些秘密，或者组织拥有自定义的 yaml 模板机制，希望利用这些机制。这些舵图可以从任何来源获取，只要代理可以与该来源通信。

*   将 Deployment.yaml 导出为变量——用户将能够获取 Harness 在部署期间生成的 manifest-dry-run.yaml，并将该文件传递给第三方系统。这允许用户针对他们的 yaml 运行策略或检查，以确保清单配置符合组织标准。

*   头盔图作为一个神器来源(经验修补)-对于我们的用户谁尝试了新的头盔图功能，我们想**谢谢你！**我们很高兴收到您的来信并收集您的反馈。我们正在努力通过为用户提供更直观的用户界面来为 Kubernetes 和 Helm 服务配置他们的 Helm 图表，从而使体验变得更好。改进后的体验包括对 Helm 和 Kubernetes 部署的 API 支持，并将 Helm Chart 作为可部署单元。我们将投票为新的舵图表发布到用户舵回购和部署它！

*   HashiCorp Vault 基于 HashiCorp Vault 代理的身份验证-用户将能够使用 HashiCorp Vault 代理(除了应用程序角色和令牌之外)对 HashiCorp Vault 进行身份验证。基于 Vault 代理的身份验证消除了用户将 HashiCorp Vault 凭据存储在 Harness 中的需要。此外，还可以使用存储库代理来管理存储库令牌续订，而不是在 Harness 中进行管理。

*   代理令牌轮换-用户将能够通过替换其默认代理令牌来进一步保护其代理与线束管理器之间的通信。线束使用委托令牌来加密线束委托和线束管理器之间的通信。用户现在可以创建自定义委托令牌，并将这些自定义令牌与其一个或多个委托一起使用，而不是使用默认委托令牌。此外，用户可以根据其治理策略撤销委托令牌，并用新的自定义令牌替换撤销的令牌。

*   对本地 Helm 的定制远程清单支持——多个客户要求能够在运行部署之前获取链接到 Harness 服务的 Helm 图表，这允许他们在部署之前运行某种形式的预处理。这将很快推出。

*   支持在没有模板化值文件的情况下部署的选项——Harness 用户将能够使用他们现有的 Helm manifest 文件，其中包含他们自己的定制 go-templating，并且能够进行 Kubernetes 部署。

*   优雅地处理云形成状态- Harness 现在可以优雅地处理云形成状态。这将有助于部署和回滚。