# 在 CircleCI - CircleCI 上用纱线取代 NPM

> 原文：<https://circleci.com/blog/install-and-use-yarn-the-npm-replacement-on-circleci/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

Yarn 是一个开源的 JavaScript 包管理器。CircleCI 可以缓存 Yarn 和它安装的包。这可能会加快构建速度，但更重要的是，可以减少与网络连接相关的错误。

要了解更多关于如何在 CircleCI 构建中安装 Yarn 的信息——包括完整的示例——请阅读我们的文档。

Yarn 是谷歌、脸书、Exponent 和 Tilde 合作的成果，于 2016 年 10 月发布。要了解更多关于纱线的信息，你可以查看他们的[回购](https://github.com/yarnpkg)或脸书上的[公告。](https://code.facebook.com/posts/1840075619545360/yarn-a-new-package-manager-for-javascript/)