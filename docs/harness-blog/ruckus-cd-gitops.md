# 伸缩 Kubernetes 通过 GitOps | Harness 连续输送

> 原文：<https://www.harness.io/blog/ruckus-cd-gitops>

## 关于 Ruckus 网络

Ruckus Networks 正在通过弥合数字鸿沟和连接世界各地的人们来重新定义连通性。Ruckus 与渠道合作伙伴、原始设备制造商和战略合作伙伴合作，通过我们的接入点、交换机和云服务提供无处不在的连接。

## 从 Monolith 虚拟机迁移到 Kubernetes 微服务

在 Ruckus Networks，过去的 monolith 应用程序环境(例如开发、QA、试运行、生产)可能需要 30 分钟或更长时间才能启动，开发人员和开发团队需要等待。

因此，在生产中运行的服务的横向扩展和弹性成为一个主要挑战。

此外，部署管道是特定于服务的，很少甚至没有跨团队重用逻辑或配置。

因此，他们决定迁移到基于云原生 Kubernetes 的微服务架构，在该架构中，环境可以在几秒钟内完成配置，并且水平自动伸缩功能被原生嵌入运行时环境。

此次迁移的结果是，40 多个新的 Kubernetes 微服务都需要新的部署管道和 DevOps 团队的加入。Ruckus Networks 的云工程总监 Raj Zala 说:“开发一个新的 Kubernetes 应用程序可能需要我们的 DevOps 工程师一周的时间。

## 为持续交付构建还是购买？

为了解决这一入职挑战，Raj 和她的 DevOps 团队考虑了几种选择:

1.  手动编写每个 Kubernetes 管道的脚本——大约 40 周的工作量(每个微服务 1 周)
2.  构建一个 MVP CD 平台——大约 52 周的工作时间(2 名 DevOps 工程师 6 个月)
3.  购买商业连续交付解决方案

开发一个新的 Kubernetes 应用程序可能需要我们的 DevOps 工程师一周的时间。

> Rajeshwari Zala |云工程总监| Ruckus Networks

## 利用线束 GitOps 和管道即代码

[Harness](https://harness.io/) 的关键卖点之一是为开发人员和开发人员提供自助式持续交付能力，这意味着他们可以授权、自动化和扩展他们的软件交付流程，从开发一直到生产，而无需等待超过几分钟。

第二个优势是 DevOps 团队能够在其新的 Kubernetes 微服务和工程团队的产品组合中构建和重用管道。

Raj 说:“我们的 DevOps 团队现在在 Harness 中创建管道模板，开发人员用他们自己的服务参数实例化这些模板。
[Harness GitOps](https://developer.harness.io/docs/continuous-delivery/cd-gitops/harness-git-ops-basics/) 允许开发者在一个地方以代码的形式管理管道、服务、环境、Helm 和 Kubernetes 配置。这意味着所有管道都可以像任何 Git 存储库中的代码一样进行版本控制和管理。

我们现在可以在同一天而不是一周内使用新的 Kubernetes 微服务

> Rajeshwari Zala |云工程总监| Ruckus Networks

## 上市时间缩短 80%并节省 25 万美元

由于实施了 Harness，Ruckus Networks 获得了以下好处:

-新 Kubernetes 服务的入职时间从一周缩短到同一个工作日，缩短了 80%

DevOps 团队的入职工作从 38 周缩短到 38 天

-节省了 250，000 美元的开发运维成本，用于构建自己的 MVP CD 平台

-CD 流程的上市时间缩短了 92%，从 6 个月缩短到 2 周