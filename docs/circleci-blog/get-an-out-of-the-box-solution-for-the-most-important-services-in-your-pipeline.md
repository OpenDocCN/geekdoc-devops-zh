# Kubernetes 管理的微服务| CircleCI

> 原文：<https://circleci.com/blog/get-an-out-of-the-box-solution-for-the-most-important-services-in-your-pipeline/>

整体架构是现代应用规模和复杂性持续增长的结果。当应用程序很小的时候，诸如消除 bug 或添加新功能、更新 UI 或 UX，或者实现额外的安全性等事情都是由同一个团队完成的。将会有一个中央存储库，在一个地方存放满足所有业务需求的所有代码。随着应用程序的增长，不同的职责开始被分配给具有特定角色和功能的各个团队。这些单独的团队有特定的角色，但是他们仍然需要在同一个集中的代码库上操作，这导致了跨团队的依赖性和代码不能合并的可能性。微服务的引入有望通过将应用程序分成独立的服务来消除团队之间的交叉工作，每个服务提供一个特定的功能。这种架构最有益的方面是快速扩展、增加可靠性和更快的开发时间。

## 管理您的微服务

容器用于在微服务之间提供必要的隔离层。随着应用程序的复杂性及其在云中的服务之间的交互的增长，为控制多个服务如何在多个容器中运行而开发的编排工具成为了一种需求。截至目前，Kubernetes 是唯一一个可以从亚马逊(EKS)、微软(AKS)和谷歌(GKE)这三大云服务提供商那里获得的容器编排解决方案。由于这些项目提供的灵活性、可靠性和透明性，越来越依赖云服务来部署应用程序的同时，OSS 的使用也在增加。Kubernetes 是开源标准组织 CNCF 最受欢迎的项目，并且已经证明是编排工具的领导者。

微服务方法为您的架构带来的好处是有代价的。一个应用程序可能需要很多很多服务，包括来自第三方和/或 OSS 项目的专有服务。单独来说，这些服务可能更容易管理，或者在第三方和 OSS 的情况下，可能由其他人来管理它们，但是所有这些服务的行为和操作的互联方式可能非常复杂。Kubernetes 解决方案不仅难以实现，而且难以更新和调试。这就是您选择使用哪种 CI 工具如此重要的原因。您需要一个 CI 工具，它允许您将 Kubernetes 与您已经使用的所有服务一起使用。

## 面临一个共同的问题

使用 CircleCI 的 orbs，您可以为您管道中最重要的服务获得开箱即用的解决方案。orb 是 CircleCI config 的可重用、可共享的开源包，支持这些服务的即时集成。它们允许针对常见问题的众包解决方案。我们这里关注的是执行主要操作的 orb，比如部署到 GKE，但是其他的也允许发送关于您的项目的定制 Slack 通知。关键的好处是，您可以访问所有这些服务，而不必自己学习集成它们。要在登录 GCP、构建和部署 Docker 映像，然后将该映像部署到 GKE 集群之后向 USERID1 发送自定义 Slack 通知，只需通过导入 [Slack](https://circleci.com/developer/orbs/orb/circleci/slack) 和[GCP-GKE](https://circleci.com/developer/orbs/orb/circleci/gcp-gke)orb 来更新它们的`.circleci/config.yml`:

```
orbs: 
    slack: circleci/slack@x.y.z
    gke: circleci/gcp-gke@x.y.z 
```

然后运行`gke/publish-and-rollout-image`后的`job`中的`slack/notify`命令，并带有所需的参数:

```
jobs:
  - build
  - gke/publish-and-rollout-image:
      deployment: k8s-deployment-name
      container: container-name
      image: image-name
  - slack/notify:
      mentions: ‘USERID1’
      message: ‘deployment to GKE’ 
```

虽然一个精明的高级 DevOps 工程师可能能够将此功能编写到他们选择的 CI 工具的配置中，但 CircelCI 的 orbs 消除了这种开发，并以一个正在工作并经过测试的社区支持的集成来取代它。`slack/notify`命令本身超过 125 行代码。整个松弛球体几乎有 500 行。没有一个使用 CircleCI 的团队需要自己编写代码。

CircleCI 球体是使用 Kubernetes 的最简单方法。你是只想安装`kubectl`还是`kops`？ [Kubernetes orb](https://circleci.com/developer/orbs/orb/circleci/kubernetes) 可以帮你安装。您是否使用 Kubernetes 部署到 GKE？在上面的例子中，我展示了如何用 [GCP-GKE 球体](https://circleci.com/developer/orbs/orb/circleci/gcp-gke)轻松实现这一点。也许你正在使用 Helm 来部署你的 Kubernetes。我们有[头盔球](https://circleci.com/developer/orbs/orb/circleci/helm)来做这件事。无论您如何使用 Kubernetes，CircleCI 是唯一可以让您快速启动并运行的 CI 工具，这样您就可以将时间花在开发项目上，而不是部署管道上。

## 你能做什么

你还想从当前的 Kubernetes orb 中看到其他的特性吗？由于 orb 是开源的，您可以通过批准和合并您的 PR 来向现有 orb 添加功能。你也可以制造一个问题，看看是否有社区支持开发。你有没有一个用例让你觉得与当前的 Kubernetes orbs 不同？您可以[自己创作一个](https://circleci.com/blog/how-to-make-an-easy-and-valuable-open-source-contribution-with-circleci-orbs/)并将其贡献给社区。我们甚至发布了为 orb 创建自动化构建、测试和部署管道的最佳实践([第 1 部分](https://circleci.com/blog/creating-automated-build-test-and-deploy-workflows-for-orbs/)和[第 2 部分](https://circleci.com/blog/creating-automated-build-test-and-deploy-workflows-for-orbs-part-2/))来帮助您。查看 [orbs 注册表](https://circleci.com/developer/orbs)中所有可用的 Orbs。

微服务允许您的团队利用第三方服务和 OSS，消除了内部开发这些常用工具和资源的需要。有了 orb，您的团队只需要知道如何使用这些服务，而不需要知道如何集成或管理它们。