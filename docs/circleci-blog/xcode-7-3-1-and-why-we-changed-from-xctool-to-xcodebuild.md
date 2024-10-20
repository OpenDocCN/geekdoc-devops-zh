# Xcode 7.3.1，以及我们为什么从 xctool 换成 xcodebuild - CircleCI

> 原文：<https://circleci.com/blog/xcode-7-3-1-and-why-we-changed-from-xctool-to-xcodebuild/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

在过去的几天里，我们用最新的 Xcode 7.3.1 更新了 OS X 构建映像。此外，默认情况下，我们在容器中包含了更高版本的 Carthage 和 fastlane。这将有助于您了解最新的开发工具。

![xcode image.png](img/e3ea21397362ab21a0ae7f5dc71f1b4b.png)

您可能还注意到，默认情况下，OS X 项目现在是用`xcodebuild`而不是`xctool`构建的。随着`xctool`的开发放缓，以及对它的构建支持即将移除，正如 [`xctool`自述文件](https://github.com/facebook/xctool#features)中所解释的:

> 注意:不推荐使用 xctool 支持构建项目，并且不会更新以支持 Xcode 的未来版本。对于简单的需求，我们建议迁移到 xcodebuild(带有 xcpretty ),对于更复杂的需求，我们建议迁移到 xcbuild。xctool 将继续支持测试(见上文)。

我们决定使用苹果提供的`xcodebuild`来执行所有必要的构建动作。

这种变化通常不会影响构建过程或构建的性能。`xcodebuild`在你的代码上执行与`xctool`完全相同的动作。

我们的选择是由大量客户在使用`xctool`时获得的构建行为的不一致性所激发的——我们平台的一些细节可能会阻止我们的用户利用`xctool`提供的一切。

如果您仍然希望在您的项目中使用`xctool`，我们非常欢迎[在您的`circle.yml`](https://circleci.com/docs/1.0/ios-builds-on-os-x/#build-commands) 中指定。

虽然我们不再使用`xctool`作为默认的构建命令，但我们很高兴看到脸书团队将他们的努力集中在更快、更灵活和跨平台的 [xcbuild](https://github.com/facebook/xcbuild) 上——我们期待看到`xcbuild`与`xcodebuild`在功能上旗鼓相当。我们非常感谢开发和支持`xctool`的团队，它是一个节省了很多开发者时间的工具，我们相信新的`xcbuild`将继续为世界带来更好的 Xcode 体验。

如果您有任何对 CircleCI 用户社区有益的见解，或者想分享您在测试中使用“xcodebuild”与“xctool”的经验，请在[讨论 CircleCI](https://discuss.circleci.com/tags/xcode) 上发表意见。