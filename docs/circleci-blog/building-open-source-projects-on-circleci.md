# 在 CircleCI 上构建开源项目

> 原文：<https://circleci.com/blog/building-open-source-projects-on-circleci/>

现代的界面、与 GitHub 和 Bitbucket 的一步集成以及免费的构建容器使 CircleCI 成为各种规模的许多组织的首选 CI 工具。很多人不知道的是，我们专门为开源项目提供了额外的资源和设置。

## 额外的构建容器

CircleCI 上的所有组织(用户和公司)都有一个 linux 容器和 1000 分钟的构建时间来运行他们的项目。包括专有代码。百分百免费。尽管这可能很棒，但我们提供的开源项目甚至更多！一个正在 CircleCI 上构建的开源项目获得了**三个额外的构建容器**，总共 4 个。更好的是，这些构建容器没有分钟限制，所以您可以想构建多少就构建多少。这是免费提供的价值 150 美元的 CircleCI 服务。

多个构建容器允许您做两件事情中的一件。利用 CircleCI 的并行性，一次构建多个拉请求(PR ),或者更快地构建单个 PR。不管怎样你都赢了。

**注意:**如果构建在 Linux(我们的默认平台)上的项目是 GitHub 或 Bitbucket 上的“公共”项目，它们将自动接收额外的容器。不过，我们不会冷落 macOS 用户。如果你正在 macOS 上构建一个开源项目，并想利用我们的服务，请联系 billing@circleci.com。

## 有用的功能和设置

**公共构建页面** -您的构建页面可以是公共的，以允许核心贡献者、合作者和一般社区对您的项目透明。

私有环境变量(Private environment variables)——许多项目最终需要 API 令牌、SSH 密钥、密码或其他东西来构建所有的依赖项，或者部署到其他地方。Private envars 允许您安全地存储这些变量，即使您的项目可以被组织外部的成千上万的用户看到和贡献。

**高级设置- >构建分叉拉取请求** -这将决定是否构建从分叉库打开的 PRs。这样做的主要好处是快速有效地测试用户的贡献，在你有机会看之前就告诉你他们的公关是否好。一个可能的缺点是大量的或者分叉的 PRs，或者制作不良的 PRs 会使您的构建容器忙碌起来，而不是处理您自己的代码。

**高级设置- >从分叉拉取请求**中传递秘密给建筑——这个设置对于**安全**来说**极其重要**。当这个打开时，之前提到的那些私有 envars，AWS & Heroku 凭证，甚至存储在 CircleCI 上的 SSH 密钥都将对任何创建一个 fork 并为您的项目打开一个 PR 的人可用。这个设置的好处是需要这些变量的构建将能够通过，这总是一个好消息。然而，严重的缺点是允许某人，任何人，简单地编辑您的 CircleCI 配置，将您的 SSH 密钥打印到终端，现在就可以访问您的服务器。

**高级设置- >仅构建拉取请求** -该设置意味着仅构建拉取请求和项目的默认分支(通常是主分支)。这对于有大量提交活动的项目非常有用，有助于减少将要运行的构建数量。

## CircleCI 已经在建设社区项目

CircleCI 上已经有大量大大小小的开源项目正在建设中。这里有几个例子:

*   [**React**](https://github.com/facebook/react)——脸书基于 JavaScript 的 React 是用 CircleCI(以及其他)构建的。
*   [**Vue**](https://github.com/vuejs/vue)——一个构建用户界面的渐进式框架。
*   [**Calypso**](https://github.com/Automattic/wp-calypso)——为 WordPress.com 供电的下一代 webapp。
*   [**Angular**](https://github.com/angular/angular)——另一个建立在包括 CircleCI 在内的多个提供者之上的 JavaScript 框架。
*   [**fastlane**](https://github.com/fastlane/fastlane) -一款适用于 Android 和 iOS 的自动构建工具。
*   [**纱**](https://github.com/yarnpkg/yarn)——一个快速、可靠、安全的另类 npm 客户端。

加入 [CircleCI 讨论](https://discuss.circleci.com)分享您的项目，告诉我们如何更好地支持开源社区。