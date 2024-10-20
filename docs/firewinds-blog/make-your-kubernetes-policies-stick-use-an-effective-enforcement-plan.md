# 让你的 Kubernetes 政策坚持下去:使用有效的执行计划

> 原文：<https://www.fairwinds.com/blog/make-your-kubernetes-policies-stick-use-an-effective-enforcement-plan>

 随着团队超越他们的第一个 Kubernetes 试点，进入整个组织更广泛的部署，DevOps 团队的工作越来越困难。他们没有时间手动编写或审查进入其集群的每个 Dockerfile 和 Kubernetes 清单，这可能会导致安全漏洞、计算资源的过度消耗和嘈杂的工作负载。应对这些挑战的最简单的解决方案是实施策略模式。建立 Kubernetes 策略来加强安全性、效率和可靠性将 [为您的 DevOps 团队](/blog/kubernetes-policy-enforcement-to-enable-devsecops) 节省大量深夜页面和升级问题。

> 不确定如何创建有效的策略？阅读 [阅读此白皮书，了解更多](/kubernetes-policy-enforcement) ，包括如何建立要测量和跟踪的内容，以加强安全性、效率和可靠性。

## **Kubernetes 政策执行**

策略可以帮助您实施一致的标准，并通过避免错误配置和意外中断来帮助您的组织节省资金。虽然制定标准的和定制的策略很重要，但是如果策略没有得到执行，这也于事无补。虽然您的工程团队拥有最佳实践文档是件好事，但它们太容易被忘记或忽略。那么你如何执行你的 Kubernetes 政策呢？有三种方法可以让您的策略生效:

1.  开发内部工具

2.  部署开源

3.  选择策略驱动的配置验证平台

### **开发内部工具**

对于很多工程团队来说，这是一个正在进行的争论——[内部自建工具](https://www.softwareadvice.com/resources/build-vs-buy-software/) ，还是购买一些东西来解决问题？工程师喜欢开发他们自己的工具，但是很少值得这么做。构建自己的软件通常会导致其他项目的生产力损失，如果构建它的开发人员离开组织，就会失去支持，并且它通常会导致比已建立的解决方案功能更少。开发和维护自主开发的工具需要时间、金钱和资源，而您的组织可能更愿意将这些用于发展他们的业务。

### **部署开源工具**

有几个开源工具可以帮助您进行安全性、可靠性和效率配置。这里的团队在 [Fairwinds](/open-source-software) 贡献了 [Polaris](https://github.com/FairwindsOps/polaris) ，它识别 Kubernetes 部署配置错误，以及 [Goldilocks](https://github.com/FairwindsOps/goldilocks) ，它帮助您识别资源请求和限制的起点。 [Trivy](https://github.com/aquasecurity/trivy) ，来自 Aqua Security，是一款针对容器和其他工件的简单漏洞扫描器。此外，Kubernetes 社区有一个强大的创建配置策略的开放标准:开放策略代理(OPA)。这个开源的通用策略引擎统一了整个体系的策略实施。OPA 允许用户跨基础架构和应用程序设置策略，并且它也是内容感知的，因此管理员可以根据实际发生的情况做出策略决策。这些开源工具非常强大，尽管您也应该预料到您的团队将需要花费时间在每个集群上部署和管理每个工具。

### **选择策略驱动的配置验证平台**

使用一个平台，您的团队可以立即采取行动，修复不一致的地方，并在您的持续集成/持续开发(CI/CD)管道中执行策略。虽然这种选择有软件费用，但在构建自己的解决方案或独立运行开源工具时，开发、部署和管理不同工具所花费的时间会抵消这一费用。

> 寻找一个完整的 Kubernetes 治理平台？Fairwinds Insights 是免费的。今天就开始吧。

[fair winds Insights](https://www.fairwinds.com/insights)是一个策略驱动的 Kubernetes 配置验证平台。通过集成可信工具和协作工作流，我们团队的专业知识创建了一个单一的监控平台，该平台可以轻松管理多个集群的结果、推送通知、创建票证等。

## **避免跨多个集群和用户的安全事故、停机和不一致**

具有多个 Kubernetes 集群和多个用户的环境会引入不一致性，管理集群不一致性非常耗时、低效，并且容易出现人为错误。为了提高 Kubernetes 的安全性和可靠性，您需要从 CI/CD 渠道开始并贯穿整个生产过程的 Kubernetes 策略实施。

阅读本白皮书，了解哪些政策是必要的，以及如何制定和执行 Kubernetes 政策 。

[![New call-to-action](img/154350e87174c62c0f2d20e8a08d9722.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/397d414a-d137-4f8e-9cfc-f6f2363bf2ad)