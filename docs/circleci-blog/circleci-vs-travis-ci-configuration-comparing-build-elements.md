# 将 Travis CI 配置转换为 CircleCI | CircleCI

> 原文：<https://circleci.com/blog/circleci-vs-travis-ci-configuration-comparing-build-elements/>

CircleCI 构建管道在一个`.circleci/config.yml`文件中定义。此配置文件不同于其他 CI/CD 服务，如 Travis CI。有兴趣将他们的代码库和/或开源项目构建配置迁移到 CircleCI 的开发人员有时会因为不了解他们当前的 CI/CD 服务配置和 CircleCI 的构建配置之间的差异而却步。在这篇文章中，我将比较 Travis CI 构建配置元素和它们的 CircleCI 对应元素，并尽可能帮助翻译。由于 Travis CI 和 CircleCI 是非常不同的系统，所以并不是所有的构建元素都可以在 CircleCI 中使用，反之亦然，但是我希望本指南可以帮助那些希望了解它们如何相似和不同的人。下表列出了 Travis CI 构建元素及其 CircleCI 对等元素(如果适用)。

## 摘要

我希望这篇文章可以作为开发人员的指南，帮助他们理解 Travis CI 配置和兼容的 CircleCI 构建配置之间的区别。上面列出的参考表显示了常见的 Travis to CircleCI 配置元素，对于有兴趣迁移 CI/CD 管道以在 CircleCI 平台上进行构建、测试和部署的 Travis CI 用户来说，这是一个很好的起点。关于从 Travis CI 迁移到 CircleCI 的实际例子，请查看我们的[文档，其中包括一个示例翻译](https://circleci.com/docs/migrating-from-travis/)。

如果你被卡住了，你可以通过[社区](https://discuss.circleci.com/)论坛联系 CircleCI 社区。