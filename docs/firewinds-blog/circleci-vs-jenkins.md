# CI/CD 支持:CircleCI vs Jenkins

> 原文：<https://www.fairwinds.com/blog/circleci-vs-jenkins>

 在 Fairwinds，每当我们开始与一家新公司合作时，我们都会推荐 [CircleCI](https://circleci.com/) 作为其 CI/CD 渠道。有两个主要原因:

1.  我们的客户选择与我们合作，以降低 Kubernetes 的复杂性并获得专业知识，从而专注于他们的核心业务技术。出于同样原因，我们推荐 CircleCI。

2.  CircleCI 是在其前辈(如 Jenkins)多年反复试验的基础上构建的一个完善的工具。

打磨工具，进化出

**TL；詹金斯博士是强大的，但高维护。CircleCI 的 UX 更好，没有保养**。

Jenkins 是一个流行的 CI/CD 开源工具。实际上是一场甲骨文之争后哈德森的[分叉。Jenkins 是围绕一个插件系统设计的，该系统允许第三方扩展来添加功能。然而，这是一把双刃剑。一方面，它允许用户创建非常独特的功能，甚至尝试一些想法。另一方面，这种额外的功能使得配置和维护变得更加困难，并且产生了不连贯的用户体验(UX)。这可以从彻底改造 Jenkins 用户体验的项目](https://en.wikipedia.org/wiki/Jenkins_(software))[蓝海](https://www.jenkins.io/doc/book/blueocean/#blue-ocean-overview)中看出。使用 Jenkins，公司花费大量时间维护工具本身和插件，占用了核心业务技术的时间和资源。

进入 CircleCI。它创建于 2011 年，因此建立在观察最初 CI/CD 使用案例中哪些可行、哪些不可行的经验之上。它不仅从 Jenkins 那里汲取了思想，还从它之前的其他 CI/CD 工具中汲取了思想。像 Jenkins 这样的工具委托给插件的所有核心 CI/CD 功能都内置在 CircleCI 中，支持更干净的配置和更好的 UX。然而，它并没有止步于此，对于类似插件的功能，CircleCI 提供了与许多服务的集成，如 GitHub，AWS，GCP 和 Kubernetes。另外，CircleCI 有 orb，它们是“您在构建中使用的 CircleCI 配置的可共享包”。在 Fairwinds，我们提供一个名为 [rok8s-scripts](https://circleci.com/orbs/registry/orb/fairwinds/rok8s-scripts) 的球体😉有了 CircleCI，用户花在维护上的时间更少了，更多的时间用于他们的业务。

## 维护复杂性降低

**TL；詹金斯博士需要安装和维护。这意味着运营团队将有额外的工作。CircleCI 可以在云中运行，几乎不需要维护。**

要使用 Jenkins 首先你需要把它安装在某个地方。这意味着您需要在您的 Kubernetes 集群中提供或安装一个服务器。无论哪种方式，这都意味着您需要采取额外的步骤来确保安装是生产就绪的，然后维护它。然而，它并没有停止在这里，安装后您需要配置詹金斯，以满足您的需求。插件将需要安装，配置和维护。最后，做完这一切后，Jenkins 准备好创建管道了。使用 CircleCI，只需注册即可开始创建管道。CircleCI 负责安装和管理。此外，如果用例要求 CircleCI 托管在公司自己的基础设施上，这是可以做到的。运行托管的 CircleCI 当然需要维护，但是设置仍然更简单，您仍然可以从更好的 UX 中受益。

## CI/CD 支持

我们的 Fairwinds 团队专注于 Kubernetes 支持。这就是用户需要的帮助，以减少复杂性并实现 Kubernetes 的最佳实践。它有助于用户避免反复试验以节省时间，提供生产级 Kubernetes 基础设施，使用户可以专注于他们的应用，并提供对高技能 sre 的即时访问。

作为我们对 Kubernetes 支持承诺的一部分，我们也致力于推荐最佳的工具。这意味着 CircleCI 支持 CI/CD。

[![Free Download: Kubernetes Best Practices Whitepaper](img/b53e4ee22b6ef19bc06a035649ea1dc6.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/4f92c7e1-1646-4985-9a0a-b1091903dddb)