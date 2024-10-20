# KubeCrash:云原生速成班

> 原文：<https://www.fairwinds.com/blog/kubecrash>

 [KubeCrash](https://www.kubecrash.io/) 是一个新的虚拟 KubeCon 同地活动，面向那些不能亲自参加 KubeCon 或处于“时区-左侧-后方”的人由五家使用 Kubernetes 开源工具的公司创建的 KubeCrash 提供了关于云原生技术的 KubeCon 级课程。没有供应商推介，只有令人敬畏的开源项目内容，如 Linkerd、cert-manager、CockroachDB、Pulumi 和 Fairwinds 的项目、 [北极星](https://polaris.docs.fairwinds.com/) 和 [金发女孩](https://goldilocks.docs.fairwinds.com/) 。

Kubernetes 是云托管应用程序开发的新标准，允许 DevOps 团队推动企业级云原生工具的技术选择。免费可用的开源解决方案通常是这些工具决策的主要来源。

[KubeCrash](https://www.kubecrash.io/) 为开发者、可靠性工程师、云安全专员、平台工程师提供半天的知识分享和虚拟学习环境。在这一系列专题讲座和研讨会中，直接向一些最受欢迎的开源项目的维护者学习。

> https://www.kubecrash.io/注册

## 期待什么

虚拟活动提供了一个充满精彩内容的时间表，这些内容来自维护一些云原生生态系统最受欢迎的开源项目的团队。该议程将涵盖以下方面的最新知识:实施可扩展的零信任、扫描工作负载以提高云原生安全性、使用服务网格确保跨多集群基础架构的高可用性，以及为多云部署提供“无服务器”服务。

## 日程安排

### 使用 cert-manager 实现 pod 内部通信的零信任身份——Jake Sanders，cert-manager 维护人员

现代云原生架构要求网络不可信，这就是内部工作负载快速推动 mTLS 和私有 PKI 使用的原因。Jetstack 的这个研讨会将演示如何使用 cert-manager 来发布、管理和轮换 mTLS 证书，允许用户在 Kubernetes pods 之间拥有经过严格验证的机器身份——所有这一切都不会让工作负载私钥离开节点内存！将此会话视为实现服务网格解决方案的前奏，使用 cert-manager 建立零信任环境(可能由信任域定义)，并加强点对点流量的安全性。

### 使用 Linkerd 的多集群故障转移—link erd 维护者 Eliza Weisman

跨集群故障转移是提高 Kubernetes 应用程序整体正常运行时间和可靠性的一个很好的方法。虽然整个集群的故障转移可以在全局入口层完成，但是单个服务的故障转移要稍微困难一些。在本专题讲座中，Linkerd 维护人员 Eliza Weisman 将介绍如何使用 CNCF 毕业的服务网格 Linkerd 来实现跨集群单个服务的流量故障转移。与会者将学习如何使用纯开源技术，以一种连贯和自动化的方式将服务网格指标、流量转移和跨集群通信结合起来，同时保留基本的安全保证，如相互 TLS。

### 使用 Polaris 和 Goldilocks 优化和保护 Kubernetes 工作负载— Rachel Sweeney、Fairwinds 和 Andy Suderman，Polaris 和 Goldilocks 维护人员

了解如何使用开源工具 Polaris 和 Goldilocks 扫描 Kubernetes 工作负载，以提高资源利用率和安全性。观看 Fairwinds 的 R&D 和技术总监 Andy Suderman 和 SRE 的 Rachel Sweeney 演示如何根据 Kubernetes 的安全性和效率最佳实践正确配置您的集群。

### 使用 Kubernetes 提供“无服务器”服务——丽莎-玛丽·南菲和吉姆·沃克，蟑螂实验室

在本次演讲中，蟑螂实验室团队成员将分享他们如何利用 Kubernetes 提供“无服务器”体验。无服务器承诺改变我们消费软件的方式。它使我们有可能只为我们使用的东西付费，并通过最大限度地减少资源消耗来帮助降低运营成本。无服务器架构需要对应用程序逻辑及其部署方式有独特的看法，这是逻辑和物理世界的结合。一种架构模式已经出现，在这种模式下，我们可以将短暂的计算与需要持久的服务分开来扩展。

### 多重云，单一部署:使用 Kubernetes 和 Pulumi 的云工程——亚伦·弗列尔和桂妮维亚·桑格，Pulumi

业务约束和客户请求通常要求团队在多个云提供商之间建立新的 Kubernetes 环境。当跨多个团队进行协调时，计算基础架构中日益增长的复杂性将为组织带来更高的运营成本。普鲁米工程师亚伦·弗列尔和桂妮维亚·桑格将通过使用普鲁米自动化 API 构建一个 CLI 来演示如何建立 Kubernetes 集群、部署应用程序和自动化运营任务。这些工具让每个工程师——从应用程序开发人员到站点可靠性工程师——都能成为云工程师。

## 5 月 17 日加入我们

任何人都可以加入这个 KubeCon，无论你是留在美洲还是熬夜开会。加入我们，时间为 5 月 17 日星期二，太平洋标准时间上午 9 点/中部标准时间上午 10 点/东部时间下午 12 点。了解由项目维护者领导的开源项目的更多信息，从涵盖现代云原生安全性的项目到改善开发人员体验的项目。[今天注册](https://www.kubecrash.io/)！