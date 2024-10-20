# 开发人员不再需要为持续集成和交付|利用而牺牲

> 原文：<https://www.harness.io/blog/speed-isolation-developer-experience-ci-cd>

开发者不必满足于更低的价格。虚拟机使持续集成和交付比以往任何时候都更容易。

但是这些管道，尽管有很多好处，也有隐藏的成本和代价。不久前，你必须从以下三个好处中选择两个:速度、隔离和简单易用。使用遗留的开发工具，不可能实现这三个目标。同样的问题也适用于持续集成(CI)系统。

## 权衡:有可能拥有一切吗？

### 速度

如果你想要快速管道，你将使用轻量级、可靠、可扩展的 CI 系统，该系统在容器上运行，并且可以被容易地供应和杀死(遵循[牛与宠物的类比](http://cloudscaling.com/blog/cloud-computing/the-history-of-pets-vs-cattle/))。这种设置将允许您的 CI 系统以最小的资源成本运行快速构建。毕竟，这个构建系统是短暂的，不可改变的。这就是 Harness CI 的设计目的，因为它是由无人机驱动的。

### 隔离

但是，比方说，如果你为[高度监管的行业](https://harness.io/case-studies/secure-continuous-delivery)或政府工作而优化**安全性**，那么你可能会更加关注隔离。如果您的软件交付工作的主要驱动力是安全性，那么您不会介意为您的 CI 提供动力的基础设施需要更长的供应时间，只要它具有更高级别的隔离和对更安全的低级内核特性的访问。但是这种场景肯定比前者更麻烦、更慢，而且从一开始就不适合开发人员。在这两种情况下，脚本编写、管道维护和更多的日常工作是不可避免的——而且很糟糕。本质上，这些权衡是不可避免的。

### 开发者体验

Source: [@ibuildthecloud on Twitter](https://twitter.com/ibuildthecloud/status/1461001774647504898)

Darren Shepherd 强调了许多工具的一个关键难点:可用性。在某些情况下，开发人员的经验是构建软件交付管道的主要驱动力。在其他容器上运行嵌套或并行任务可能需要为容器提供 root 或管理员权限。这种情况不会没有风险和摩擦。除此之外，其他以安全为中心的用例需要访问虚拟机管理程序或运行时的更深层。任何给定容器中的可用运行时都是不够的。更让开发者头疼的是价格问题——具体来说，当涉及多个独立工具时,[管理云成本](https://harness.io/blog/cloud-cost-management-tools)。

在 Harness，我们努力让这些权衡成为过去。不仅如此，我们还想让 CI 流程对所有开发人员来说都是互补和愉快的。无论速度是主要驱动因素，安全性是首要考虑因素，还是您希望使用工具来优雅地解决任务，Harness 都有一个面向开发人员的工具。

## 不再需要权衡取舍:在任何地方的虚拟机上实现快速 CI

虚拟机是虚拟化革命的基础。它们仍然是隔离计算机和充分利用裸机的绝佳方式。虚拟机在任何地方运行，包括在边缘和公共云中，并且它们通过在其中运行完整的系统映像来实现。AWS 率先通过其弹性计算云(EC2)提供虚拟机。谷歌云提供类似的服务，名为 Compute Engine，微软 Azure 也有虚拟机。虚拟机不像容器那样容易调配和删除，但仍然比物理机更容易调配。

### 利用虚拟机上的支持

Harness CI 目前在 AWS 上的[虚拟机上运行，带有 Docker、](https://docs.harness.io/article/z56wmnris8-set-up-an-aws-vm-build-infrastructure) [Linux/arm64](https://portal.productboard.com/harness-io/4-harness-continuous-integration/c/72-support-for-linux-arm64-builds-on-k8s-cluster?utm_medium=social&utm_source=portal_share) 或 [MacOS](https://docs.harness.io/article/d79v3d2uwv-define-a-mac-os-build-infrastructure) 映像和 GCP 虚拟机。这些是目前 CI 支持的所有[基础设施选项](https://docs.harness.io/category/rg8mrhqm95-set-up-build-infrastructure)线束:

*   赢在 K8s 上
*   码头搬运工
*   蔚蓝的

这个列表将继续扩大，所以如果你想了解新的线束功能和用例，请订阅我们的 [YouTube](https://www.youtube.com/c/Harnessio/featured) 和 [Twitch](https://www.twitch.tv/harnesscicd) 频道，我们在那里举办各种节目，如无人机和线束 CI 办公时间或 CodeAbout 会议。我们希望通过提供关于 [CI/CD 最佳实践](https://harness.io/blog/continuous-delivery/ci-cd-best-practices/)的信息，帮助我们的用户充分利用 CI。

## 利用 CI:交付速度、隔离和开发人员体验

在 Harness，我们提供最好的开发人员体验，不管您做出什么选择或有什么限制。我们希望您的软件交付工作富有成效并且令人愉快。与许多开源 CI 工具不同， [Harness CI](https://harness.io/products/continuous-integration-b) 是一个企业级的轻量级 CI 系统。它是通过其现代软件交付平台实现 DevOps 增长的发射台。这意味着 Harness CI 能够弹性扩展以满足资源需求高峰，并在空闲时收缩。像平台的其他部分一样，它的设计考虑了效率。

这就是构建云原生 CI 系统的好处:它完全适应弹性和声明性基础设施的效率。许多[开发者既喜欢容器又喜欢 Kubernetes](https://survey.stackoverflow.co/2022/#technology) 的事实证明，当我们在 2021 年 8 月推出 CI 模块时，这是一个好的决定。

Harness CI 由无人机提供动力，因此它可以在你需要的任何地方大规模运行。此外，它还旨在使使用它令人难忘。体验的一部分是矩阵构建。开发人员的日常生活变得更加容易，尤其是如果他们与产品提供的不同循环功能相结合的话。有了这个特性，开发人员可以在一个地方一次性地对复杂的管道进行更改，从而简化构建的定义。以前，他们必须在文件的不同位置进行更改。矩阵构建有效地减少了难以编辑或更改的重复和冗余的管道定义。

当开发人员不想长时间等待他们的构建完成时，测试并行化就派上了用场。测试并行化通过让开发人员为给定的作业设置并行性，并保持其对于测试之外的用例的可扩展性，有助于驾驭 CI 体验。例如，安全扫描、代码林挺和工作负载生成测试是能够从并行运行中获益的其他潜在用例。

Harness CI 通过其旗舰功能 Test Intelligence 管理测试的方式，是我们希望促进测试驱动开发的采用的方式。运行您的测试套件不应该是痛苦或无聊的。它应该是渐进的，并在相关时提供反馈。测试智能是语言特定的，它现在支持微软的。网芯。

[立即免费试用 Harness CI！](https://app.harness.io/) ‍