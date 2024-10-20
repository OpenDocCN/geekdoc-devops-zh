# 宣布在 CircleCI - CircleCI 上推出 OS X 和 Android 支持

> 原文：<https://circleci.com/blog/announcing-ios-and-android-support/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

我们的客户要求移动应用测试和部署支持已经有一段时间了，今天[我们已经做了一些事情](https://circleci.com/execution-environments/)。我们收购了 Distiller，一家专注于 OS X 测试和部署的公司，这是我们的首次收购！我们已将他们的技术和专业知识融入 CircleCI，现在您可以使用它来测试您的移动应用程序！

## 我们为什么要做移动？为什么用蒸馏器？

移动应用程序中的错误是毁灭性的。web 应用程序中的错误并不简单，但至少你可以马上找到解决方法。移动漏洞通常会持续几周或几个月，并依赖于令人讨厌的外部因素，如应用商店和用户升级。在复杂的应用程序提交和移动部署过程中，错误很容易被引入，这进一步加剧了这种情况。让一些关键的错误配置或愚蠢的错误进入生产，降低你的应用商店评级，让你的客户失望，让你损失金钱，这是非常容易的。

作为一家承诺让每个人都能轻松测试和发布高质量软件的公司，我们知道我们必须为此做些什么。成千上万依赖我们测试和部署其后端和基于浏览器的应用程序的开发人员中，许多人也有移动需求，因此这是我们进入的一个明显方向。

我们非常幸运地与 Distiller 相遇，Distiller 是一家完全认同我们让测试和部署变得非常简单的愿景的公司。他们有我们需要的 OS X 专业知识，他们了解生态系统，而且他们是 Clojurians！

## 到目前为止的故事…

我们一直在努力与运行 Android 和 OS X 构建的客户一起工作，他们有各种各样的工作流和需求(比如构建从 Haskell 生成的 OS X 项目的人！)我们已经用 Android 构建所需的所有工具、SDK 和库增强了我们的 Linux 容器，并且已经在一个月前的私人测试版中提供了 OS X 支持。从今天开始，我们完全致力于为移动应用提供[最好的持续集成和交付工具。](https://circleci.com/execution-environments/)

我们已经看到了一些早期用户的反馈。使用 CircleCI 测试 OS X 和安卓应用的基础设施总监劳伦特·劳法斯特(Laurent Raufaste)说，“随着我们扩大产品规模，我们的重点一直是保持频繁的小发布流，CircleCI 让我们有信心做到这一点。” [Trunk Club](https://www.trunkclub.com/) 的工程副总裁 Michael Cruz 说:“CircleCI 让我们的技术团队有信心每天在众多平台上快速一致地构建、测试和部署。”

## 入门指南

现有的 CircleCI 用户应该有宾至如归的感觉——只需使用我们普通的 UI 和基于 YAML 的配置。Android 构建运行在我们的标准 Linux 构建容器中。要使用能够构建 OS X 项目的 OS X VM，只需在项目设置>实验设置中启用实验设置。

如果你是 CircleCI 的新手，不要担心，它仍然超级容易设置！只需[注册](https://circleci.com/execution-environments/)，跟踪一个项目，如有必要，启用如上所述的 OS X 构建，你的第一个构建就会开始。我们将自动推断基于 Ant 或 Gradle 的 Android 项目的构建操作，在 OSX 虚拟机中，我们将使用`xcodebuild`命令行工具检测并构建您的项目。

更多信息请见 [Android](https://circleci.com/docs/1.0/android/) 或 [OS X](https://circleci.com/docs/1.0/ios-builds-on-os-x/) 的完整文档，如果您需要任何帮助，请不要犹豫[联系我们](mailto:sayhi@circleci.com)！

## 更多即将到来

这只是我们计划的移动支持的开始。我们很高兴能很快推出更多的功能和细节。目前 CircleCI 的客户会注意到，我们的 OSX 虚拟机的能力有几个限制因素——请参见[文档](https://circleci.com/docs/1.0/ios-builds-on-os-x/#constraints-on-os-x-based-builds)了解更多详细信息。我们非常高兴能够从更多的用户那里得到反馈，因为我们已经推出了移动服务，所以请通过 sayhi@circleci.com[联系我们，让我们知道您的想法以及我们还应该为您做些什么！](mailto:sayhi@circle.com)

[讨论黑客新闻](https://news.ycombinator.com/item?id=8762784)