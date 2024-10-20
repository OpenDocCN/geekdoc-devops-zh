# 如何让你的开发者成为坏蛋

> 原文：<https://www.fairwinds.com/blog/how-to-make-your-developers-badass-with-kubernetes-and-devops-as-a-service>

 *在* [*上一篇博文*](http://blog.reactiveops.com/is-kubernetes-overkill) *中，我考察了为什么许多公司对拥抱 Kubernetes 犹豫不决，以及它的复杂性实际上如何有利于未来的增长。在这里，我将展示如何利用 Kubernetes 和 Fairwinds 的力量让您的开发人员变得强大。*

在 [*Badass:让用户牛逼*](https://www.amazon.com/Badass-Making-Awesome-Kathy-Sierra/dp/1491919019/ref=sr_1_1?s=books&ie=UTF8&qid=1502989320&sr=1-1&keywords=Badass%3A+Making+Users+Awesome) ，[Kathy Sierra](http://seriouspony.com/)问为什么一个产品在竞品拥有同等定价、促销和感知质量的情况下销量超过其他产品。答案与用户有关。用户不在乎你，也不在乎你的产品；他们关心的是你的产品是否让他们变得更。

[Kubernetes](http://kubernetes.io/) 让开发者可以做以前做不到的牛逼事情。您的工程师不想管理您的基础架构。他们想成为坏蛋。他们很难找到、吸引和留住，所以为什么不给他们想要的呢？您将拥有更快乐的开发人员、更轻松/更快速的部署、更少的停机时间&以及更快的恢复时间。Kubernetes 提供了更好的开发者/用户体验。

## **获得更灵活的 Heroku 式体验**

[Heroku](https://www.heroku.com/) 以其极其简单的平台即服务起步而闻名于世。Heroku 的界面并不复杂:没有人认为 Heroku 过分。如果有什么不同的话，大多数对此事有意见的人认为它太局限了。然而在幕后，Heroku 是极其复杂的。他们的成功部分源于用户看不到这种复杂性。

需要明确的是，技术上的权衡并不是在运营复杂的高度复杂的 Kubernetes 环境中，而是在不复杂的情况下拥有出色的开发人员用户体验，以及拥有出色的开发人员用户体验。如果这是一个交易，Kubernetes 就不会存在。 **权衡实际上是在 Kubernetes 这样的东西和简单地做错事之间，Kubernetes 使做错事变得困难而做对的事变得容易。**

人们很容易被复杂性吓倒。很容易求助于 [EC2](https://aws.amazon.com/ec2/) 或 [Docker Swarm](https://docs.docker.com/engine/swarm/) ，在那里容易的事情是琐碎的，而困难的事情是不可能的。然而，重要的是要记住一定的复杂性是必不可少的。

## **选择开发运维即服务，以便有人为您管理复杂性**

如果你自己实现 Kubernetes，你需要了解整个 Kubernetes 世界。你是怎么安装的？(Kubernetes 没有安装程序。)应该用什么工具？(有十几种可能的工具，选择哪个安装程序并不容易。)假设您选择了 kops 这样的安装工具，您如何确保您的基础架构是可重复的？在发生灾难时，您如何确保您可以在不丢失任何东西的情况下在其他地方启动您的基础架构？(您需要自动化所有这些工作，以便在必要时可以重新创建集群。)

这种学习非常耗时，对你没有好处，也不会给你的企业增加价值。你会有最糟糕的技术债务(基础设施债务和编程代码债务)，而且你永远也不会还清。

你不需要了解哪些 API 在未来的版本中被弃用，也不需要知道调用哪些 API。你不需要了解库伯内特 YAML 的来龙去脉。作为我们开发运维即服务方法的一部分，我们通过管理大量 Kubernetes YAML、管理所有持续集成/持续交付(CI/CD)并为您提供一个简单的界面，让您远离这种复杂性。Kubernetes 是以 Heroku 建筑为核心的建筑。在 Fairwinds，我们用 Kubernetes 为您做与 Heroku 相同的事情，除了我们定制 Kubernetes 以满足您特定的工作流程要求。

有了合适的团队，Kubernetes 的复杂性就很容易管理。在 Fairwinds，我们通过编写使 Kubernetes 更易于使用的开源工具来实现规模经济和投资回报。 [DevOps 即服务是我们的业务](http://blog.reactiveops.com/why-should-you-outsource-devops) ，开源的 Kubernetes 工具是我们的业务模式。

**我们的目标？照顾与您的架构相关的每一个细节，因此您不必担心它。**

## **本质的复杂性造就了一个糟糕的解决方案**

Kubernetes 很复杂。但是当别人为你管理它时，这种复杂性是值得的。选择一个 DevOps 即服务提供商，为您提供您的开发人员想要的强大 Kubernetes 系统，同时消除所有相关的复杂性。

记住，当你不知道自己不知道的事情时，很容易选择走自己的路。开始使用其他编排工具很容易——事实上太容易了，以至于这些工具最终会限制您所能做的事情，就像 Heroku 一样。入门的容易掩盖了复杂性。最终你会发现你错过了什么。当您准备好体验我们基于 Kubernetes 的 DevOps 即服务方法时(在您的第二次或第三次竞技表演后)，您会想知道没有它您是如何生活的。

一旦你开始使用 Kubernetes，前途无量。您可以获得出色的 Kubernetes 用户体验，这也是您想要它的原因，而且您不必担心构建 Kubernetes 基础架构所涉及的无聊问题。

80%的财富 500 强企业使用 Kubernetes，这可能并不奇怪。但是故事并没有就此结束。中型公司使用 Kubernetes。十人创业公司用 Kubernetes。Kubernetes 不仅仅面向企业。对于各种规模的前瞻性公司来说，这是一笔真正的交易。

那么，如何确保 Kubernetes 迁移成功呢？ [选择合适的伴侣](http://blog.reactiveops.com/how-to-choose-the-right-devops-as-a-service-partner) 而不是自己动手。

但是不要相信我们的话。听听范多尔的迈克尔·图辛斯基怎么说。正如他所说，“费尔温斯让我们远离了库伯内特的复杂性。我们看到的只是一个愉快的 CI/CD 界面，我们完全不用担心。我们根据具体的业务挑战做出了明智的选择，选择了正确的工具 Kubernetes 和正确的团队合作伙伴 Fairwinds。”