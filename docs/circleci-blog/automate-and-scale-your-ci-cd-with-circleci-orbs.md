# 使用 CircleCI orbs | CircleCI 自动化和扩展您的 CI/CD

> 原文：<https://circleci.com/blog/automate-and-scale-your-ci-cd-with-circleci-orbs/>

在过去两年半的时间里，作为 CircleCI 的解决方案工程师，我非常高兴能够与 CircleCI 的一些大客户一起工作，帮助他们将健康的 CI/CD 实践融入到他们的开发流程中。

领先的组织试图确保他们的应用程序是可伸缩的、可靠的和安全的。快速可靠地将产品运送给用户是获得竞争优势的必要条件。

是什么让公司能够快速可靠地运送产品？

对于 CircleCI 用户来说，这个问题的一个答案是 orbs。orb 正成为团队寻求简化具有相似需求的多个项目的 CI/CD 的首选加速器。orb 还使开发人员更容易、更快地掌握项目进度。

在这篇博客文章中，我们将介绍 CircleCI orbs 是什么、为什么以及如何使用，并为您提供一些关于如何在您自己的 CI/CD 实践中实施 orbs 的指导。

## 什么是球体？

orb 是一个指令列表，它允许您遵循成熟的逻辑，跨多个项目自动化一个特定的任务。它们帮助您最大限度地降低配置复杂性，同时充当软件包管理器，快速高效地集成您的软件和服务堆栈。orb 是可定制的，并且配备了[利用作业、命令和执行者](https://circleci.com/docs/using-orbs/#quick-start)来满足你对项目的独特需求的能力。

有公共和私有 orb，用户可以在具有相似用例的项目中采用。公共 orb 集成了流行的工具，可供使用 CircleCI 的广大开发人员使用。此外，我们提供私人 orb，仅限于同一组织内的用户。[自 2021 年初以来，我们的规模计划客户可以使用私人球体](https://circleci.com/blog/circleci-private-orbs/)，现在我们的付费计划的所有客户都可以使用。

## 公共球体与私有球体

**公共球体**

*   任何 CircleCI 用户都可以在自己的配置中使用公共 orb。
*   CircleCI orbs 是开源的，因此创建或使用公共 orb 是向其他开发人员支付的一种方式。发布的 orb 可以在我们的[开发者中心](https://circleci.com/developer/orbs)找到。

**私人宝珠**

*   私有球体只在内部发布。公司可能会选择创作私有 orb，因为他们不想让竞争对手知道他们在开发过程中使用了某种工具。
*   私有 orb 不会出现在 CircleCI 开发人员中心，您组织之外的人不能查看或使用它们，也不能在不属于您组织的管道中使用它们。

关于 orb 最令人兴奋的事情之一是，你可以使用 [orb 开发工具包](https://circleci.com/docs/orb-author/#orb-development-kit)自己创建公共和私有 orb。

值得注意的是，私有球体并不意味着被用作秘密管理工具。任何可能被视为“秘密”的信息，比如 API 密钥、身份验证令牌和密码，都不应该作为参数值直接输入。CircleCI 的 [orb 开发最佳实践](https://circleci.com/docs/orbs-best-practices/?section=configuration)有更多关于这方面的信息。出于存储秘密的目的，我们强烈建议使用上下文和环境变量，你可以在这里阅读关于[的内容。](https://circleci.com/docs/contexts/)

## 不要多此一举

在管道中使用 orb 可以节省大量时间。在[这个例子](https://circleci.com/docs/orb-intro/?section=configuration#benefits-of-using-orbs)中，创建和采用 orb 来测试节点应用程序意味着只使用项目配置文件中的`test`作业(已经预先配置好),而不是实际创建作业并概述测试节点应用程序的步骤。这不仅节省了开发人员配置 CI/CD 管道的时间，而且有助于使配置符合不要重复自己(DRY)的最佳实践，这意味着您不必每次编写代码时都重新发明轮子。

企业客户有成千上万的 CI/CD 管道，这意味着需要匹配配置文件。使用 orb 可以帮助这种规模的公司确保他们的管道和配置文件在规模上保持可靠和高效。

## orbs 如何保护你的应用安全

orb 对于建立和维护健康的 CI/CD 渠道至关重要。对于希望跨多种安全工具部署和测试应用程序的团队来说，它们也非常有用。CircleCI 已经与几个安全组织合作，他们的合作伙伴 orb 可以用来测试你的应用程序。

只需调用这些 orb，根据您的应用程序测试它们，并收集关于安全漏洞的有意义的数据见解，以确保质量控制。[通过“安全”过滤合作伙伴 orb](https://circleci.com/developer/orbs?filterBy=certifiedAndPartner&query=&category=Security)以查看所有 CircleCI 安全合作伙伴 orb。

## 为什么应该使用球体？

orbs 的最终目标是开发人员可以更容易地使用配置文件，而不是感觉被它消耗掉了。简单地能够调用 orb 允许开发人员将他们的注意力转移到构建作业和工作流上，而不是花时间配置代码。

凭借直观的设计和框架，orb 可以通过配置和版本控制轻松维护。这超越了优化——orb 减少了构建时间和信用消耗，我敢说，提高了开发人员的理智。

无论它们是公共的还是私有的，orb 都是确保 CI/CD 流程的速度、可靠性和安全性的最佳方式。要查看公共 orb 或开始创作自己的 orb，请查看 CircleCI [orb 开发工具包](https://circleci.com/docs/orb-author/#orb-development-kit)。