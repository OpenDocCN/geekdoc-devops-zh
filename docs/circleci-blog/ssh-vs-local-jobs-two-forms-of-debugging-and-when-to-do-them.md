# SSH 访问-本地构建| CircleCI

> 原文：<https://circleci.com/blog/ssh-vs-local-jobs-two-forms-of-debugging-and-when-to-do-them/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

“它在我的机器上工作，但在生产中中断了”是工程师们经常遇到的问题。CircleCI 1.0 用户发现通过使用 SSH 访问来调试这样的构建失败非常有用，我们现在已经为运行在我们的 [2.0 平台](https://circleci.com/docs/)上的构建启用了 SSH 访问。

此外，对于 2.0，我们引入了 CircleCI CLI，它可以帮助用户在本地重现 CircleCI 环境。该功能允许用户在其本地环境中快速迭代和调试问题。

## 我应该使用哪一个？

## 通过 CircleCI CLI 进行本地构建

如果您希望保持代码库的完整性，并且需要快速修复问题或调试配置语法，那么通过 CircleCI CLI 在本地运行构建可能是您的正确选择。

我们已经看到用户提交他们的配置并运行几次构建来验证他们的 YAML 语法。您可以使用`circleci build`在本地机器上快速试验和验证您的语法。一旦配置通过验证，您就可以提交它。

如果您需要一种简单的方法来解决容器配置问题，您可能还希望使用本地版本。本地构建允许您快速重试数据库配置或验证服务是否设置正确。

最后，对于测试项目，您可以克隆 repo，安装 CircleCI CLI 并运行`circleci build`。这是在与您的团队相同的环境中快速启动和运行的好方法。

## SSH 访问

对许多 CircleCI 用户来说，通过 SSH 跨网络测试构建已经成为一个流行且有价值的特性。对构建的 SSH 访问允许您通过 SSH 会话访问执行环境，以排除构建故障、访问日志文件、分析正在运行的进程等等。您可以直接 SSH 到一个失败的构建中，看看哪里出错了。在每次构建之后，您可以选择启用 SSH 访问进行重建。当机器运行时，你将能够 SSH 到它，并发现问题。

## 我如何使用这些？

对于 SSH 访问您的作业，您可以[遵循本指南](https://circleci.com/docs/ssh-access-jobs/)。

要在本地运行 CircleCI，请按照本指南中的[安装 CircleCI CLI。](https://circleci.com/docs/local-jobs/)