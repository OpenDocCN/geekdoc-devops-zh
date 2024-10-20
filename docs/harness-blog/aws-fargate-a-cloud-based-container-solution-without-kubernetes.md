# 利用 Harness | Harness 简化 AWS Fargate 部署

> 原文：<https://www.harness.io/blog/aws-fargate-a-cloud-based-container-solution-without-kubernetes>

集装箱难题并不容易解决。我们刚刚在一个特定的容器编排工具 [Kubernetes](https://kubernetes.io/) (K8s)上进行了一个由六部分组成的旅程。在这六个部分中，我们讨论了 K8s 的谱系、观点和一些用例；但是少了一块。我们并没有过多提及 orchestrator 和图像/容器所在的实际基础设施。构建支持集群基础设施本身的基础设施是一项复杂的任务；例如，运行您自己的 Kubernetes 集群的 AWS 基础设施。 [AWS Fargate](https://aws.amazon.com/fargate/) 希望通过 AWS 的处方，消除管理集群技术和运行集群的基础设施的复杂性。Fargate 提供了一个服务来编排和打包几个底层的 AWS 服务，以消除复杂性。

## Kubernetes 是否过于复杂？

构建容器映像只是等式的一部分。在花了一些时间整理你的申请后，是时候向世界展示你的努力了。至少你需要一个位置来执行 [Docker 运行](https://docs.docker.com/engine/reference/run/)或者你选择的容器运行时。利用 [Kubernetes](https://kubernetes.io) 是一个设计和架构决策。在公共云供应商的基础上运行您自己的 Kubernetes 集群不仅对 Kubernetes 本身来说是一个学习曲线，而且确保基础架构对 Kubernetes 集群来说是谨慎的。费力地处理实例类型、扩展组、存储和网络规则会占用创新的宝贵周期。很有可能需要一些不同的 Kubernetes 环境。从本地桌面到低级环境，在将应用程序发布之前，在这些环境中构建和测试应用程序。预计将有资源维护 Kubernetes 集群和 Kubernetes 的专业知识。AWS 拥有[弹性 Kubernetes 服务(EKS)](https://aws.amazon.com/eks/) ，这是 AWS 的托管 Kubernetes 服务。维护和管理 Kubernetes 集群，尤其是大规模的集群，可能非常困难。最好的证明是，EKS 的 AWS 仍然运行旧版本的 Kubernetes，而不是最新版本；在我写这篇博客的时候， [Kubernetes 1.15](https://kubernetes.io/blog/2019/06/19/kubernetes-1-15-release-announcement/) 已经发布，EKS 最新支持的版本是 1.13.7 。为了消除 Kubernetes 的复杂性，Fargate 并不基于 Kubernetes；这可能会令人感到惊讶，如果今天要建造什么东西的话。

## Fargate 不基于 Kubernetes-Hello 任务

当我在 2017 年的 [AWS re:Invent 上第一次听说 Fargate 时，我立即假设 Kubernetes 在组合中的某个位置，并一直在 Fargate 材料中寻找 K8s 标志。这是 AWS 提供的继](https://aws.amazon.com/blogs/aws/aws-fargate/)[弹性集装箱服务(ECS)](https://aws.amazon.com/ecs/) 和 EKS【或者第四，如果你算上[弹性豆茎](https://aws.amazon.com/elasticbeanstalk/)】之后的第三个集装箱服务，我很惊讶 Kubernetes 没有更靠前和居中。不管你喜欢它还是讨厌它，自从 2015 年夏天发布以来，弹性容器服务(ECS)就是 Fargate 背后的命脉，因为 Fargate 是 ECS 上的[发布类型](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/launch_types.html)。有关于 EKS 在法盖特的谈话，但是目前两者之间还没有本土的融合。当利用 ECS 时，任务定义是一个主要概念。Fargate 和 ECS 的资源定义语言是任务定义。通过填充[任务定义参数](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definition_parameters.html)，创建[任务定义 JSON](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/create-task-definition.html) 非常简单。如果利用 Fargate，可以使用的任务定义是有限制的，这在[中有很好的记录](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/AWS_Fargate.html)。

任务定义可以直接在 Harness 中创建。当利用几个 AWS 容器产品中的一个时，当然有选择和意见。

## 建设者有选择；纯 ECS vs EKS vs 法盖特

在 2018 年的 [AWS re:Invent](https://reinvent.awsevents.com/) 期间，AWS 开始大量使用“建设者”一词。[互联网上充斥着关于开发商 vs .建筑商的辩论](https://www.zdnet.com/article/aws-says-so-long-developers-and-hello-builders/)，但是 AWS 创造了期待即时基础设施的新一代人。如果你以前没有在 [EC2](https://aws.amazon.com/ec2/) (又名纯 ECS)、EKS 或 Fargate 上使用 ECS，一个很好的学习策略是从最新的开始，然后向后学习。AWS 的 Abby Fuller 有一个很棒的视频深入介绍了 Fargate 提供的内容。以此为基准，您可以看到 EC2 上的 ECS 和 Fargate 之间的差异，Totalcloud 的文章对此进行了很好的解释；基本上 EC2 上的 ECS 比 Fargate 有更宽的控制平面和更少的规定。如果 Kubernetes 是你的毒药，那么 EKS 是你今天的选择，或者利用 Kubernetes 供应商部署到 AWS。

可以选择 ECS 类型的内置线束平台。[/caption]在 Fargate 之旅的早期与客户合作，这是 Fargate 很快变得昂贵的一个常见问题。与任何 AWS 服务一样，在控制云成本时，了解服务的定价和扩展方式至关重要。[这个由变革代理人制作的视频](https://www.youtube.com/watch?v=HoXEyXIf6_U)是我见过的最好的分解三种服务成本的视频。

Fargate ECS 集群示例。像任何现代云供应商一样，当然有选择。创建支持选择的渠道对于现代组织来说至关重要。

## 鱼与熊掌不可兼得

Harness 平台的最大优势之一是灵活性。想象一下，您的管道可以支持多种基础设施部署模式。线束具有一级 [ECS 支持](https://docs.harness.io/article/08whoizbps-ecs-deployments-overview)，线束代理可以部署在 [Fargate 发射类型](https://docs.harness.io/article/wrm6hpyrjl-harness-ecs-delegate#fargate_specs_and_delegates)上。

Harness Fargate 管道示例。[/caption]我们可以浏览一个由我们的销售工程师之一 [Greg Kroon](https://www.linkedin.com/in/gregory-kroon-0a92022/) 创建的[简单而优雅的示例](https://www.linkedin.com/posts/gregory-kroon-0a92022_harness-ecs-aws-activity-6563943618892140544-1ngq)。在这个 Fargate 部署示例中，我们利用 Harness 的本机能力来部署到 ECS。

如果您尝试在多个地方运行您的容器，Harness 平台可以轻松地提供在 [Kubernetes](https://docs.harness.io/article/wkvsglxmzy-kubernetes-canary-workflows) 、[EC2 上的 ECS 和 Fargate 中自信地部署/回滚所需的管道。](https://docs.harness.io/article/7qtpb12dv1-ecs-blue-green-workflows)随着更多云服务和协调器的出现，Harness 将会为您提供信心，让您相信您的管道将会是强健的。