# 如何在 CircleCI 上导入项目环境变量

> 原文：<https://circleci.com/blog/new-on-circleci-import-project-environment-variables/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

共享项目环境变量的能力是我们最需要的功能之一，现在所有 CircleCI 用户都可以使用。

以前，每个新项目都需要手动添加环境变量，如 API 密钥。现在，组织管理员用户可以从组织内的其他项目一次性导入环境变量。这应该会减少建立新项目时的痛苦。

以下是如何从构建仪表板中查找和导入项目环境变量:

 </blog/media/2016-07-09-env-variables-screencast.mp4> 

你可以在这里阅读更多关于环境变量和[如何在 CircleCI 1.0 上使用它们，在这里](https://circleci.com/docs/1.0/environment-variables/)阅读 CircleCI 2.0 上的[。](https://circleci.com/docs/env-vars/)

如今，导入指的是简单的一次性复制粘贴功能。我们正在努力进行迭代，以尊重与用户和组相关的更多权限，允许环境变量的作业级更改，并保持组织级同步。如果您对此类功能感兴趣，您的反馈将是无价的- [请访问讨论！](https://discuss.circleci.com/t/organisation-wide-environment-variables/103/24?u=dominikgm)