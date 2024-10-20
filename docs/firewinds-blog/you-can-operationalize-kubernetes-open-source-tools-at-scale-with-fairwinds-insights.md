# 借助 Fairwinds Insights 大规模运营 Kubernetes 开源工具

> 原文：<https://www.fairwinds.com/blog/you-can-operationalize-kubernetes-open-source-tools-at-scale-with-fairwinds-insights>

 在 Fairwinds，我们在管理数十个组织的数百个 Kubernetes 集群方面有着悠久的历史。为了管理这种复杂性并让我们的客户保持在线和正常运营，我们需要一种方法来跟踪这些群集的健康状况，特别是在安全性、效率和可靠性方面。所以我们建立了 Fairwinds Insights 平台，这个平台可以运行不同的 Kubernetes 审计工具，并将结果汇总到一个仪表板中。

Fairwinds Insights 通过将一组可扩展的可信开源审计工具集成到一个平台中，提供了对 Kubernetes 环境的多集群可见性。Fairwinds Insights 贯穿整个开发生命周期，从持续集成(CI)到进入生产阶段。

> Fairwinds Insights 可供免费使用。你可以[在这里](/coming-soon)报名。

## 连续累计

对我们来说很重要的一点是，Insights 在开发过程的早期就发现了问题，因此我们可以阻止问题进入一个活跃的集群。因此，我们的第一步是将见解集成到基础设施即代码的持续集成(CI)流程中。当您将 Fairwinds 的见解添加到 CI 流程中时，您可以在开发流程的早期发现映像漏洞和 Kubernetes 错误配置。

Insights 可以扫描每个拉取请求中的更改，因此只要有安全、效率或可靠性问题，您就可以通知开发人员或阻止合并。为了找到这些问题，Insights 在 CI 中运行以下报告类型:

*   [Polaris](https://github.com/FairwindsOps/polaris) :最佳实践的配置验证
*   [Trivy](https://github.com/aquasecurity/trivy) :扫描 Docker 镜像寻找漏洞
*   [开放策略代理](https://www.openpolicyagent.org/):(又名 OPA)运行定制策略
*   [冥王星](https://github.com/FairwindsOps/pluto):检测废弃的资源

将 Insights 连接到您的 GitHub 存储库将帮助您充分利用 CI 集成，但是您仍然可以在没有 GitHub 的情况下使用 CI 特性。(如果您正在使用 Gitlab、Bitbucket 或其他 Git 主机，请告诉我们！)在我们的文档中了解关于[配置](https://insights.docs.fairwinds.com/run/ci/configuration/)的更多信息。

## 准入控制器

Fairwinds Insights 还可以作为[准入控制器](https://insights.docs.fairwinds.com/run/admission/about)运行。这将拒绝任何 Kubernetes 资源进入您的集群，如果他们不符合您的组织的政策。准入控制器增加了一层额外的保护，以防 CI 流程中出现问题，或者有人在没有使用基础设施即代码存储库的情况下向集群添加资源。Fairwinds 还通过自动化规则为准入控制器的细粒度定制提供了强大、灵活的解决方案。

## 洞察代理

最后，Insights 还提供了一个集群内代理来扫描集群内部已经存在的问题。代理每小时生成报告，并将数据发送回 Fairwinds Insights。用户可以启用或禁用几种不同的开源报告工具(如 Polaris、Trivy、 [Prometheus Collector](https://insights.docs.fairwinds.com/configure/reports/resource-metrics/) 和 [Goldilocks](https://www.fairwinds.com/blog/goldilocks-kubernetes-resource-requests) )，并可以使用报告中心独立配置它们。

调查结果和建议存储在一个位置，使运营商能够了解和控制多个 Kubernetes 集群，跟踪和优先处理问题，并监控 Kubernetes 工作负载的安全性和成本。

## OPA 和 Fairwinds 的见解

我们通过三种主要方式将 OPA 集成到 Fairwinds Insights 中:

1.  作为 CI/CD 挂钩，作为代码评审过程的一部分，审计基础设施即代码
2.  作为准入控制器(又名验证 Webhook)，它将阻止有问题的资源进入集群
3.  作为集群内代理，重复扫描有问题的资源

通过将 Fairwinds Insights 和 OPA 结合使用，组织可以主动将其 Kubernetes 集群与最佳实践和内部策略保持一致。此外，在开发和部署流程的每个步骤中运行 OPA 的能力有助于在问题进入集群之前尽早发现问题，从而更容易地在开发和运营之间进行交接。更好的是，Insights 可以采用相同的 OPA 策略，并将它们联合到所有三个上下文，以及您想要的任意多个集群。

## 最小化复杂性

Insights 平台使 DevOps 团队能够在应用从开发进入生产时发现并防止配置问题。通过与这些工具集成，Insights 帮助团队大规模运营开源。您可以通过阅读 [Insights 文档](https://insights.docs.fairwinds.com/run/agent/about)或免费使用它来了解更多信息！[拿过来。](/coming-soon)

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)