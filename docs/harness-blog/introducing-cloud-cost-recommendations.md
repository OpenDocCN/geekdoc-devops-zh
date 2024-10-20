# 介绍云成本建议|利用

> 原文：<https://www.harness.io/blog/introducing-cloud-cost-recommendations>

云成本管理现在可以为您提供关于调整容器资源的建议。

Kubernetes 作为伟大的均衡器已经改变了我们定义应用基础设施的方式。只需描述/声明您在 YAML 的配置中需要什么，Kubernetes 就可以满足您的要求。

尽管通过一行 YAML 就可以轻松地调整您需要的资源，但有时我们容易为了安全而过度配置。基于容器的工作负载可能会变得非常昂贵，我们将详细介绍 [Kubernetes 是如何由于消费激增而变得昂贵的](https://harness.io/blog/why-kubernetes-becomes-expensive/)。

云成本管理的主要目标是让云成本和围绕使用的秘密大众化。作为一名工程师，您见过多少次在本地或开发环境之外运行的容器化工作负载的指标？这句话的讽刺之处在于，通常只有当你超过某个阈值时，你才能看到项目，而不是当你远低于某个阈值时。未充分利用的表现是更高的成本，这正是云成本管理的直接目的。现在，云成本管理可以推荐采取行动的最佳方式。

## 推荐在这里

我们非常高兴地宣布对我们的云成本管理产品[建议](https://ngdocs.harness.io/article/o75arkcg8i-workload-recommendations)的重大改进。云成本管理现在可以为您提供关于容器资源调优的建议。现在，在计算资源约束与投资回报之间的关系时，已经不再需要猜测；云成本管理实际上向您展示了潜在的成本节约。

如果没有云成本管理建议，通常情况下，与调整相关的数据可能会在组织混乱中丢失。需要有来自平台工程/运营团队的反馈回路，提醒未充分利用的部署。然后，需要向开发团队提供历史数据，以对潜在的资源减少做出判断。如果你正在阅读这些陈述，并想知道如何收集和传播这些信息，你肯定不是一个人。

深入研究这些建议，有两个服务质量(QoS)建议；[保证](https://kubernetes.io/docs/tasks/configure-pod-container/quality-service-pod/#create-a-pod-that-gets-assigned-a-qos-class-of-guaranteed)和[突发](https://kubernetes.io/docs/tasks/configure-pod-container/quality-service-pod/#create-a-pod-that-gets-assigned-a-qos-class-of-burstable)。

这两个 QoS 建议都是基于历史 CPU 和内存利用率数据计算的。该实现对 CPU 样本和内存峰值使用衰减直方图。保证资源按第 90 百分位计算。对于突发资源，下限和上限分别基于第 50 和第 95 个百分点。手动完成这一任务肯定不是一件有趣的事情，也不容易通过云成本管理来完成。

## 驾驭，您的云成本守护者

我们将继续精心设计和创新帮助您和您的组织节省云支出的方法。最近，我们还宣布能够通过 API 从 Harness 平台提取云成本信息。在云成本管理出现之前，管理您的云支出无疑是一件令人惊愕的事情。在 Harness，我们的使命是实现云支出的民主化，并在您消耗云资源的同时，帮助传播有价值的谨慎信息。敬请关注，并确保今天就报名参加[云成本管理](https://harness.io/products/cloud-cost/)。

干杯，

拉维