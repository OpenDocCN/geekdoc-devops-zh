# 你现在需要知道的关于 Kubernetes 所有权的一切

> 原文：<https://www.fairwinds.com/blog/everything-you-need-to-know-about-kubernetes-ownership-right-now>

 今天，Kubernetes 服务所有权是软件交付和运营团队对他们支持的服务拥有完全“所有权”的一种新兴方式。在这个模型中，所有权跨越了从软件设计和开发到生产环境中的部署，并最终管理软件的日落。这种责任的转变极大地提高了应用程序的速度、可靠性、成本和安全性。

也就是说，并不总是清楚什么样的组织需要这种程度的组织转变。随着组织的成长，他们经常发现推动应用程序和服务的交付通过一个操作关口是一项挑战，如果不是不可能的话。但是，随着 DevSecOps 方法的价值不断增长，团队现在正在寻找在他们的工作流中实现这种转变的方法。作为 DevSecOps 的推动者， [Kubernetes 服务所有权](/kubernetes-service-ownership-whitepaper)使这一根本性的变化得以发生。

## 关于 Kubernetes 服务所有权，我需要了解什么？

了解 Kubernetes 环境的配置哪里不符合行业标准和最佳实践非常重要。以部署为例。它们可能看起来工作得很好，即使没有简单的活性和就绪性探测，或者没有资源请求和限制设置。但事实是，忽略这些配置设置最终会导致灾难。

关于安全性，发现 Kubernetes 部署何时被过度许可至少是一个挑战。事实上，许多团队*确实*过度使用容器和集群，主要是因为让某些东西工作的最简单的方法是提供 root 访问权限——这意味着完全的管理权限。这样，配置会给 Kubernetes 环境带来重大的安全风险。

另一方面，当适当的治理到位时，Kubernetes 服务所有权可以帮助组织以更低的风险更快地交付代码，尤其是具有多个团队和集群的企业。对于一个运行 40 个集群的公司来说，实现良好的服务所有权可能意味着知道谁负责修补漏洞和安全故障之间的差别。这种所有权的明确指示还确保了正确的团队或开发人员将检查集群配置并在需要时应用补丁。服务所有权可以帮助每个开发团队成功地在 Kubernetes 上获得应用程序。

## 谁需要这种类型的服务所有权？

尽管服务所有权的好处很有价值，但它不一定是每个企业的正确模式。例如，只有一个应用程序、一个 DevOps 工程师和一个小型开发团队的小型创业公司可能不需要服务所有权。通常，操作仍然可以拥有所有的部署配置，不管它们是否运行在 Kubernetes 上。

但是随着组织的成长，对服务所有权这样的模型的需求也在增长。在运行数十个集群的企业中，运营团队管理每个应用程序的所有部署配置的“所有权”并不罕见。这就是 Kubernetes 服务所有权能够提供真正价值的地方，因为它使开发人员能够在一个集中的团队之外掌控他们的应用程序的配置方式。虽然这通常发生在运行集中式 Kubernetes 平台的大型组织中，但是有效所有权的模型可以应用于许多业务实例。在拥有数百甚至数千个集群的组织中，如果没有某种服务所有权，扩展几乎是不可能的。

一个不太常见的场景包括开发团队获得完整的全栈所有权。虽然这通常发生在小团队中，但是公司本身可以是大的或小的。事实上，65%的公司在生产中使用 Kubernetes。根据最近 2021 年的一项研究，拥有超过 500 名开发人员的企业更有可能(78%)在生产中运行所有(或大多数)容器化的工作负载。

## 如何启用 Kubernetes 服务所有权？

Fairwinds Insights 通过简化复杂性和实现全面的服务所有权，统一了开发、安全和运营。为了帮助团队克服文化挑战并接受服务所有权，Insights 促进了:

*   **支持** —开发团队可以了解其应用中适当的安全性和效率配置，这不仅仅是一个运营问题，因为开发团队现在知道适当的配置是什么样的。
*   **可靠性** —服务所有者可以使用最佳实践指南配置 Kubernetes，即使他们并不十分了解 Kubernetes，也能确保快速、可靠的应用并避免停机。
*   **持续改进** —团队可以通过从 CI/CD 到生产过程中集成服务所有权来持续改进 Kubernetes 的使用方式，实际上可以从交付到生产过程中阻止中断的部署。

[fair winds Insights](https://www.fairwinds.com/?utm_source=adwords&utm_medium=ppc&utm_term=fairwinds%20insights&utm_campaign=Branded&hsa_cam=9424392662&hsa_mt=p&hsa_ver=3&hsa_src=g&hsa_ad=419723939660&hsa_net=adwords&hsa_tgt=kwd-1393288901891&hsa_acc=8748715703&hsa_grp=95380032853&hsa_kw=fairwinds%20insights&gclid=CjwKCAjwtfqKBhBoEiwAZuesiKj03PqQSfnHcBHQyqYrQjOXX1x4OeUSya9tLBWzLGZr4qmveUzQfRoCXpAQAvD_BwE)通过提供所有集群的仪表板视图，帮助团队了解导致安全和合规性风险的错误配置，并通过集成漏洞扫描减少漏洞管理所需的时间，从而为开发运维团队提供对 Kubernetes 环境的可见性。它还通过识别错误配置和漏洞，并将所有权分配给负责解决这些问题的个人或团队，来帮助团队解决管理 Kubernetes 的一些更具挑战性的方面。

对使用 Fairwinds Insights 感兴趣吗？免费提供！点击此处了解更多信息。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)