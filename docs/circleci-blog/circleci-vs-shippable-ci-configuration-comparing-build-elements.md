# 将可发货配置转换为 CircleCI | CircleCI

> 原文：<https://circleci.com/blog/circleci-vs-shippable-ci-configuration-comparing-build-elements/>

CircleCI 构建管道在一个`.circleci/config.yml`文件中定义。这个[配置文件](https://circleci.com/docs/configuration-reference/)不同于其他 CI/CD 服务，比如 Shippable。有兴趣将他们的代码库和/或开源项目构建配置迁移到 CircleCI 的开发人员有时会因为不了解他们当前的 CI/CD 服务配置和 CircleCI 的构建配置之间的差异而却步。

在这篇文章中，我将比较可发布的构建配置元素和它们的 CircleCI 对应物，并尽可能地帮助翻译。由于 Shippable 和 CircleCI 是非常不同的系统，并不是所有的构建元素都可以在 CircleCI 中使用，反之亦然，但是我希望本指南可以帮助那些希望了解它们如何相似和不同的人。如果你现在准备从可发货转变，开始在 CircleCI [这里](https://circleci.com/signup/?shippable)上构建。

下表列出了可发货的构建元素及其 CircleCI 等效元素(如果适用)。

## 摘要

我希望这篇文章可以作为开发者的指南，帮助他们理解可发布配置和兼容的 CircleCI 构建配置之间的区别。上面列出的参考表显示了常见的可发货到 CircleCI 配置元素，对于有兴趣迁移 CI/CD 管道以在 CircleCI 平台上构建、测试和部署的可发货用户来说，这是一个很好的起点。

准备好从可发货切换了吗？在 CircleCI [这里](https://circleci.com/signup/?shippable)开始建造。此外，如果你遇到困难，你可以通过[社区](https://discuss.circleci.com/)论坛联系 CircleCI 社区。