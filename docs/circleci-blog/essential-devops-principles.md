# 2021 年 DevOps 基本原则| CircleCI

> 原文：<https://circleci.com/blog/essential-devops-principles/>

## DevOps 成功工程团队的原则和实践

欢迎阅读 CircleCI 指南，了解领导高绩效开发团队的基本原则。在本指南中，您将找到相关资源来帮助您的团队培养 DevOps 文化，从而做出明智的决策并最大限度地提高运营效率。

许多团队现在正在转向 DevOps，这是有充分理由的:使用 DevOps 实践有助于团队更好地响应市场变化。他们可以更快、更安全地部署代码，更不用担心中断生产。

但是切换到 DevOps 并不是一个非此即彼的命题。是的，DevOps 是一种将运营知识带入开发的思维模式，但它也提供了一整套流程、方法和使用工具的方式，使其与众不同。

## 在购买所有工具之前，培养一个 DevOps 团队环境

DevOps 是一系列问题解决过程中的最新一个，每个过程都带有一个充满工具的数字车库:CI/CD 系统、测试框架、监控工具和安全审计工具等等。但是在采用工具之前应用 DevOps 思维模式是必要的。

#### 在[转移到 DevOps](https://circleci.com/blog/what-tools-do-you-need-to-do-devops/) 之前，确保您的团队在以下方面保持一致:

1.  **每个人都在同一页上**关于您试图通过这种转变实现的目标。每个人都应该在你试图解决的问题上达成一致，并且在痛点上保持一致。
2.  **从小处着手。**不要试图一夜之间把整个组织打造成一个模范 DevOps 团队。相反，从一个团队开始，看看流程改变在你的组织中是否有效。
3.  **经常测量。**在您开始任何改进计划之前，获取您当前所处位置的准确指标(即我们的开发周期需要 X 时间)。一旦你实施了改变，你就可以衡量它们的有效性。
4.  不要试图一次自动化所有的事情。关于开发运维的一个误解是，所有基础架构配置和配置管理都必须自动完成。这被称为“[基础设施，代码为](https://circleci.com/blog/an-intro-to-infrastructure-as-code/) (IaC)。”IaC 实践旨在支持大规模应用。在自动化复杂的部署场景之前，尝试自动化应用程序的构建和测试。

有关 DevOps 文化和工具的更多信息，请参见:[转移到 DevOps:您真正需要什么工具？](https://circleci.com/blog/what-tools-do-you-need-to-do-devops/)或下载[devo PS 文化的基石](https://circleci.com/resources/devops-culture-ebook/)电子书。

关于代码形式的基础设施的更多信息，请参见:[我如何使用基础设施作为代码？](https://circleci.com/blog/how-do-i-iac/)

## 获得管理层对开发运维的认可

让您的经理对新的 DevOps 工具说“是”可能是一项挑战。使用以下提示来获得新工具的批准:

1.  **强调问题，而不是工具。**在任何对话开始之前，我们需要确定行动项目。没有哪个经理希望召开没有可操作性见解的会议。
2.  使用相关数据来支持你的需求。开发人员是生成数据的专家，所以在你的提问中包含一些具体的统计数据。
3.  从你团队中最不可能的粉丝那里获得认同。重要的是，要让那些将受到影响的人和那些可能对新工具有意见的人参与进来。
4.  **演示工具并邀请提问。**询问您的团队是否看到引入该工具可能会带来的任何问题。

欲知更多关于接近管理层的信息和建议，请阅读:[让你的经理对 DevOps 工具说“是”](https://circleci.com/blog/getting-your-manager-to-say-yes-to-devops-tools/)。

## 使用 DevOps 提高安全性

CI/CD 管道是您的技术堆栈的一个关键部分，您的基础架构可以访问许多不同的资源，从开发和生产环境到分析密钥和代码签名证书。您的管道可以访问的资源越多(安全机密、专有代码、数据库等。)，保持 CI/CD 系统的安全就越重要。

[如何利用 DevSecOps](https://circleci.com/blog/security-best-practices-for-ci-cd/) 保护 CI/CD 管道？我们建议实施以下三种解决方案中的安全最佳实践:

1.  **安全管道配置** -可以使用您的 CI/CD 管道配置来降低发生安全问题的可能性。(查看 [CircleCI orbs](https://circleci.com/blog/devsecops-and-circleci-orbs-security-focused-ci-cd-best-practices/) )。
2.  **代码和 Git 历史分析**——花时间识别已经提交到代码库中的秘密，以便您可以停用和替换它们。
3.  [**安全策略实施**](https://circleci.com/blog/three-rules-for-turning-devops-into-devsecops/)——某些安全方面无法基于已知漏洞进行静态检查，而是特定于您的特定公司，因此需要作为策略进行编码。

要了解关于采用 DevSecOps 方法的更多信息，请下载本电子书，[CI/CD 安全和 DevSecOps](https://circleci.com/blog/security-best-practices-for-ci-cd/) 终极指南。

## 为你的团队设定有意义的开发目标

当谈到团队成功时，找到正确的 [DevOps 指标来衡量](https://circleci.com/blog/engineering-metrics/)是至关重要的。虽然没有每个团队都应该向往的通用标准，但我们的数据和我们在我们的平台上看到的 2020 年 DevOps 软件趋势表明，团队有合理的基准来设定目标。

*了解如何[用工程团队的四个关键基准来衡量开发运维的成功](https://circleci.com/blog/how-to-measure-devops-success-4-key-metrics/)*

最终，你测量基线并对这些指标进行增量改进的能力比追求“理想”更有价值。

下载 [2020 年软件交付状态:面向工程团队的数据支持基准](https://circleci.com/resources/2020-state-of-software-delivery/)了解您和您的团队如何在未来扩大您的软件交付。在这里下载[的报道](https://circleci.com/resources/2020-state-of-software-delivery/)。