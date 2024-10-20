# 介绍私人球体| CircleCI

> 原文：<https://circleci.com/blog/circleci-private-orbs/>

## 在你的组织中私有共享配置:私有 orb 现在在 CircleCI 上

两年前，CircleCI 推出了开源的 orbs。有了这些 orb，全球各地的开发人员已经能够简化他们的整个开发过程，并缩短他们的生产时间。在此期间，已经创建了 2，000 多个 orb，每月有 10，000 多个独特的组织在使用它们，它们已经集成到近 1，800 万个 CI/CD 管道中。

*不熟悉宝珠？*orb 是 YAML 配置的可重用包，帮助开发人员自动化重复的过程，加快项目设置，并使其易于与第三方工具集成。

开发人员正在利用我们的[公共注册表](https://circleci.com/developer/orbs)上可用的各种 orb 来帮助自动化否则将是手动的、耗时的、重复的过程。例如，下面是一些我们最常用的 orb:

*   [Slack orb](https://circleci.com/developer/orbs/orb/circleci/slack) 为开发人员的 CI/CD 管道实现基于事件的通知
*   CircleCI 合作伙伴 [Codecov 的 orb](https://circleci.com/developer/orbs/orb/codecov/codecov) 提供跨应用的测试覆盖
*   社区开发的 [React Native orb](https://circleci.com/developer/orbs/orb/react-native-community/react-native) 构建并测试 React Native 项目

## CircleCI 的 orbs 的下一步是什么？

我们的许多客户已经要求 CircleCI 的团队为他们提供一种在多个项目之间私有共享配置的方法，这是他们组织独有的。我们听了，现在，我们很兴奋地宣布我们发布了[私人球体](https://circleci.com/docs/orb-intro/#private-orbs)。

这些组织通常有许多他们必须管理的回购，因此标准化配置并能够在项目间私下共享它是一个重要的优先事项。所有 CircleCI 计划为您的组织提供无限数量的私人球体。

## CircleCI 私人宝珠在哪里有用？

私有 orb 为开发人员提供了更多的隐私、效率和跨团队的协作。这对于在医疗保健、金融和其他具有高治理和法规遵从性标准的行业中工作的团队尤其有用。

### CircleCI 私人球体如何工作

私有 orb 不会出现在公共注册表中，只有您组织内的人员可以访问它们。具有读或写权限的用户只能查看和管理私有 orb，前提是他们在您的组织中经过身份验证。

通过查看 CircleCI 的“组织设置”选项卡中的专用“orb”页面，经过身份验证的用户可以快速轻松地查找和管理您的组织创建的所有现有 orb。管理员和 orb 作者可以直接从这个页面快速重复或更新有用的配置。

## 如何创作一个私人球体

要创建新的私有 orb，请使用 [CircleCI 本地 CLI](https://circleci.com/docs/local-cli/) 工具。管理员可以完全控制他们的团队可以使用哪些 orb，因为只有管理员有权使用 [orb 开发工具包](https://circleci.com/docs/orb-author/?section=configuration#orb-development-kit)或[手动 orb 创作流程](https://circleci.com/docs/orb-author-validate-publish/)创建、发布和更新私有 orb。

看看下面的视频，了解如何创作一个私人球体:

## 获得最佳 CI/CD 体验

除了私有 orb 和 Runner， [Scale plan](https://circleci.com/pricing/) 还包括同时运行多个构建的无限并发性，以及用于执行计算密集型应用的强大资源。这些功能使您的团队能够为您的最终用户构建令人难以置信的应用程序体验。

我们付费计划的管理员现在可以创建、发布和维护他们的私人球体。关于如何在组织内的多个项目间开始私有共享配置的更多细节，请参考我们的 [orbs 文档](https://circleci.com/docs/orb-intro/#private-orbs)。

要了解更多关于私有 orb 如何提高隐私、效率和组织内部共享的信息，请联系我们的[客户成功团队](https://support.circleci.com/hc/en-us)或阅读我们的[讨论帖子](https://discuss.circleci.com/t/private-orbs-are-here/39176)。