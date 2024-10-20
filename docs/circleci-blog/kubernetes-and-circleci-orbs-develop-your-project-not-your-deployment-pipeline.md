# Kubernetes 和 CircleCI orbs:开发您的项目，而不是您的部署管道

> 原文：<https://circleci.com/blog/kubernetes-and-circleci-orbs-develop-your-project-not-your-deployment-pipeline/>

虽然微服务架构和容器的兴起加快了许多人的开发周期，但在生产中管理它们却带来了新的复杂性，因为团队需要考虑管理这些服务的负载平衡和分布。Kubernetes 是一个管理分布式容器化应用程序的强大工具，但是管理 Kubernetes 本身是一个复杂的过程，有一个陡峭的学习曲线。Kubernetes 解决方案不仅难以实现，而且难以更新和调试。

降低 Kubernetes 环境管理复杂性的一种方法是将其直接集成到 CI/CD 管道中。CircleCI orbs 只需几行代码，就可以轻松地将集成添加到 Kubernetes 和容器管理的其他工具和服务中。orb 是 CircleCI config 的可重用、可共享的开源包，支持这些服务的即时集成。使用 orbs，您可以获得一个现成的解决方案来管理管道中最重要的服务。

## 使用这些 orb 部署和管理 Kubernetes 服务和环境:

[Google Kubernetes 引擎](https://circleci.com/developer/orbs/orb/circleci/gcp-gke) **新**
部署 Kubernetes 集群，并在几秒钟内从您的 CI 渠道更新生产代码

[亚马逊弹性容器服务(EKS)](https://circleci.com/developer/orbs/orb/circleci/aws-eks) **新**
使用 AWS 上的 Kubernetes 部署、管理和扩展容器化应用

[Azure Kubernetes 服务](https://circleci.com/developer/orbs/orb/circleci/azure-aks) **全新**
借助 Azure Kubernetes 服务，运输速度更快、操作更轻松、扩展更自信

[Red Hat open shift](https://circleci.com/developer/orbs/orb/circleci/redhat-openshift)**new**
自动化 Kubernetes 应用程序的构建、部署和管理

[Kublr](https://circleci.com/developer/orbs/orb/kublr/kublr-api) **新**
在您的所有环境中集中部署、运行和管理大型企业的多集群 Kubernetes 部署

[赫尔姆](https://circleci.com/developer/orbs/orb/circleci/helm) **新**
用赫尔姆图表查找、分享和使用为 Kubernetes 构建的软件

Nirmata
通过 Nirmata API 跨开发、测试、试运行和生产环境管理 Kubernetes 应用

[VMware 代码流](https://circleci.com/developer/orbs/orb/vmware/codestream) **新**
发布更高质量的应用和更快的 IT 代码，同时降低运营风险

[DeployHub](https://circleci.com/developer/orbs/orb/deployhub/deployhub-orb)
自动化您工作流程的发布步骤，为您管道中的每个状态创建 100%可重复的部署流程

## 使用这些 orb 存储、管理和保护容器映像:

[Amazon EC2 Container Registry](https://circleci.com/developer/orbs/orb/circleci/aws-ecr)
使用 Amazon 轻松存储、管理和部署容器映像

[Google Container Registry](https://circleci.com/developer/orbs/orb/circleci/gcp-gcr)
使用 Google Cloud Registry 存储、管理和保护您的 Docker 容器映像

Docker Hub
创建、管理、构建和运送你团队的容器应用到任何地方

[Azure Container Registry](https://circleci.com/developer/orbs/orb/circleci/azure-acr)**new**
通过轻松存储和管理 Azure 部署的容器映像，简化 Kubernetes 容器开发

## 我们的合作伙伴在说什么:

VMware CMBU 产品营销主管 Ken Lee 表示:“对于 CI/CD 而言， [VMware 代码流](https://cloud.vmware.com/code-stream)和 [VMware 云组装](https://cloud.vmware.com/cloud-assembly) orb 用于创建 PKS 群集和自动化应用程序部署是一个很好的起点。

“CircleCI 和 Nirmata 为客户提供了跨任何基础设施无缝自动化其从开发、部署到运营和优化的容器化应用生命周期管理的能力。CircleCI 专注于加速大规模应用程序开发，而 Nirmata 则为客户提供了单一管理平台，用于在基于 Kubernetes 的多云环境中部署和管理这些应用程序。CircleCI 和 Nirmata 携手实现了企业业务敏捷性的承诺，同时显著降低了云原生应用生命周期管理的总拥有成本，”Nirmata 客户成功副总裁 Anubhav Sharma 说道。

“随着开发团队转向 Kubernetes，他们将开始脱离单片 CD 管道。CircleCI 正在通过 DeployHub orb 进一步增强这一能力，以支持微服务共享、配置和发布，”DeployHub 首席执行官兼联合创始人特雷西·拉冈说。

“Kublr 创建了一个 orb，使 CircleCI 用户能够在 Kubernetes 集群上自动构建、测试和部署应用程序。CircleCI 用户可以自动向 Kublr 平台认证，以查询和利用 Kublr API。CircleCI 作业将能够创建 kubernetes 集群，检查集群的状态，并检索集群配置文件，以便在 Kubernetes 上部署应用程序，”Kublr 首席技术官 Oleg Chunikhin 说。

## 你能做什么

你还想用 Kubernetes 做些别的什么事情吗？orb 是开源的，所以在一个现有的 orb 上增加功能只是获得你的 PR 批准和合并的问题。查看 [orbs 注册表](https://circleci.com/developer/orbs/orb/circleci/helm)中所有可用的 orbs。你有没有一个用例让你觉得与当前的 Kubernetes orbs 不同？你可以[自己创作一个](https://circleci.com/developer/orbs/orb/nirmata/nirmata)并贡献给社区。我们甚至发布了为 orb 创建自动化构建、测试和部署管道的最佳实践([第 1 部分](https://circleci.com/developer/orbs/orb/vmware/codestream)和[第 2 部分](https://circleci.com/developer/orbs/orb/deployhub/deployhub-orb))来帮助您。

借助微服务，您的团队可以利用第三方服务和操作系统，无需在内部开发通用工具和资源。有了 orb，您的团队只需要知道如何使用这些服务，而不需要知道如何集成或管理它们。