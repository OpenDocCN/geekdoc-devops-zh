# CircleCI server 3.x 简介:针对自托管 CircleCI 安装的企业级 Kubernetes-circle ci

> 原文：<https://circleci.com/blog/introducing-server-3.x/>

CircleCI server 3.x 现已面向所有 CircleCI 客户推出，旨在满足最严格的安全性、合规性和监管限制。这种自托管解决方案可以在繁重的工作负载下扩展，所有这些都在您的团队的私有网络上的 [Kubernetes](https://circleci.com/blog/getting-started-with-kubernetes-how-to-set-up-your-first-cluster/) 集群中，但具有 CircleCI 的动态扩展云体验。

提供卓越的现场服务意味着通过最新版本保持服务的更新。在幕后，我们一直在开发流程，使我们能够快速地将新功能和创新从我们的云产品中引入服务器，以便所有服务器客户都能充分利用 CircleCI。

## CircleCI server 3.x 的企业级安全性

软件开发中的网络安全比以往任何时候都更加重要。对一些人来说，这意味着保护云存储容器。对其他人来说，这意味着在防火墙后面加倍努力。Server 3.x 通过对 CircleCI 安装的端到端控制，使客户能够达到最严格的安全要求。客户在他们自己的控制平面中托管 CircleCI 服务和应用程序，将所有信息保存在他们的专用网络中。Server 3.x 还通过了顶级安全公司的第三方渗透测试。

## 强大的开发工具和功能

server 3.x 最令人兴奋的方面是，客户将能够比以前更快地访问最新的 CircleCI 功能:现在我们的自托管客户可以受益于与使用我们的云产品的客户相同的功能。这包括管道、orb、[矩阵作业](https://circleci.com/blog/circleci-matrix-jobs/)、预定工作流等等。这些只是已经可用的功能中的一部分——在未来的版本中，客户将可以使用其他增强功能，如适用于 macOS 的 runner 版本、observability dashboards、insights 和设置工作流。

## 行业领先的维护和监控

Server 3.x 允许客户使用 Grafana、Prometheus、Loki 等标准工具来监控他们的安装。团队还可以通过 Telegraf 集成他们自己的监控解决方案。

对于拥有内部 DevOps 和 [Kubernetes](https://circleci.com/blog/kubernetes-and-circleci-orbs-develop-your-project-not-your-deployment-pipeline/) 专业知识、拥有自己的数据中心或具有其他地方无法满足的高度定制需求的团队来说，最新的服务器版本是一个理想的解决方案。CircleCI 服务器可以在 Google Kubernetes 引擎(GKE)、Amazon Elastic Kubernetes 服务(EKS)或本地 Kubernetes 安装上运行。

要了解更多关于 server 3.x 的信息，请访问[我们的文档](https://circleci.com/docs/server-3-overview/)。如果您目前是 CircleCI server 的客户，请联系您的客户成功经理以了解任何问题或安排一次指导迁移。