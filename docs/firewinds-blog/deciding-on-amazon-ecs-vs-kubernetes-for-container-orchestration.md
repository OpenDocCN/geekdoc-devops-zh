# 决定 Amazon ECS 与 Kubernetes 的容器编排

> 原文：<https://www.fairwinds.com/blog/deciding-on-amazon-ecs-vs-kubernetes-for-container-orchestration>

 说到容器编排，Kubernetes 已经成为在生产中管理和运行容器化工作负载的事实上的标准。“开放性”是 Kubernetes 超越竞争对手的主要原因之一，主要是受其开源根源的驱动。Kubernetes 带来了一个蓬勃发展的开源生态系统，它利用了该平台的模块化设计。几乎每一个云提供商都将软件集成为本地服务，建立了一个被称为“托管 Kubernetes”的新类别。大玩家包括 AWS Elastic Kubernetes Service(EKS)、Azure Kubernetes Service (AKS)和谷歌云的 Kubernetes Engine (GKE)产品。

然而，像 AWS 这样的一些云提供商提供了额外的容器编排选项。亚马逊的弹性容器服务(ECS)就是一个例子，它是 AWS 平台的原生产品。在本帖中，我们将阐明 ECS、Kubernetes 和推荐方法的优缺点。

## **了解 AWS ECS 和 AWS Fargate**

AWS ECS 有两种风格:由 Fargate 驱动的 ECS 和由 EC2 驱动的 ECS。

由亚马逊 EC2 compute 提供支持的传统 ECS 于 2015 年推出，旨在方便地在云上运行 Docker 容器。传统 ECS 为您的容器提供了对 EC2 计算选项的底层控制。这种灵活性意味着您可以选择运行工作负载的实例类型。它还将您与其他用于监视和记录 EC2 实例活动的 AWS 服务联系起来。

另一方面，Fargate 于 2017 年发布，作为一种无需管理底层 EC2 计算即可运行容器的方法。相反，Fargate 会自动计算所需的 CPU 和内存需求。如果您需要快速启动并运行工作负载，并且不想计算或找出底层计算选项，那么 Fargate 通常是一个不错的选择。

## 给定这些选项，ECS 在什么情况下有意义？

*   您的工作负载很小，对其他服务没有太多依赖
*   您希望您的工作负载是短暂的，只需要运行一段时间。
*   你看不到显著的架构变化
*   您有一个容器化的应用程序，需要快速启动并运行

比较这两种服务时，传统的 ECS 提供了最大的灵活性。一种可能的迁移途径是从 Fargate 开始，然后在您需要更高的运营敏捷性时迁移到传统的 ECS。

## **您是否已经超越了 ECS？**

也就是说，在产品路线图和客户需求的驱动下，大多数公司的应用和服务都在增长。在这些情况下，ECS 可能无法满足您的容器编排需求。考虑以下业务情况:

*   *云成本和资源效率变得越来越重要:*如果您希望扩展工作负载并让它们持续运行，根据您使用的服务类型，ECS 可能会成为一个昂贵的选择。例如，Fargate 将底层计算抽象化，这意味着您在微调可能正在运行的实例类型方面控制较少。
*   *您正在增加云中运行的应用和服务的数量:*随着应用的发展，您的架构也在发展。就高效扩展和运营工作负载所需的资源而言，ECS 可能变得过于有限。

例如，Fargate 服务的启动时间可能会很慢，主要是因为您将计算决策留给了 Amazon，以支持更简化的操作体验。

## **退出计划**

当您有少量服务时，在 ECS 上开始是非常好的，但当您需要灵活性和控制力时，请确保您有一个到另一个 PaaS 的退出计划，如 EKS 或 Kubernetes。

*   *您不希望局限于一家云提供商:*有时，您的首席财务官可能会要求您评估其他云提供商的成本节约措施。或者，您可能希望利用 AWS 之外的同类最佳服务。在任何情况下，您可能都希望能够灵活地选择您所选择的云提供商，尤其是随着云计算层随着时间的推移变得更加商品化。根据 [2018 年 CNCF 调查](https://www.cncf.io/blog/2018/08/29/cncf-survey-use-of-cloud-native-technologies-in-production-has-grown-over-200-percent/)，42%的受访者使用云原生技术实现云可移植性。
*   *您希望变得更加敏捷:*云原生技术能够实现更高的速度和更快的上市时间。容器有助于传递部分商业价值。然而，如果没有一个健壮的容器编排策略，您将冒着组织中多个团队采用不同模式的风险——这可能会在未来造成复杂性。

如果以上任何一种情况与你有关，那么考虑从 ECS 毕业到更现代的平台如 Kubernetes 的策略。在我们的[下一篇文章中，了解更多关于 Kubernetes 的好处和摆脱 ECS 的选择:托管 Kubernetes 与亚马逊 ECS 的好处](/thoughts/the-benefits-of-managed-kubernetes-vs-amazon-ecs)

[![Managing Kubernetes Configuration Read the Whitepaper](img/34937a5ae51bc0ceb94a21b3fd2382d0.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/1ccd0525-c794-4194-8aad-fc9663bb9c5a)