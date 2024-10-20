# Astro 简介:管理 Kubernetes 部署中的 Datadog 监视器，以提高工作效率和集群性能

> 原文：<https://www.fairwinds.com/blog/introducing-astro-managing-monitors-in-a-dynamic-environment-0>

 ### 

### “Astro 利用新的 Kubernetes-native 模式，帮助工程师以声明方式管理 Datadog 监视器，以适应这些环境的短暂性。我们很高兴能与 Fairwinds 合作，并支持他们努力使这一工具惠及整个社区。”

### —迈克尔·格斯滕哈伯，Datadog 产品管理总监

![Artboard 2](img/61487c422b56a780a9187ca52f87ecf9.png)

在 Kubernetes 环境中，我们看到的挑战之一是反映集群和工作负载状态的准确监控管理。监控往往是事后才想到的: a s 工作负载变化 ， 很少进行监控更新。现有的工具依赖于手工配置监控状态 、 ，这给 SRE 团队带来了很大的麻烦。它还增加了可用性和性能风险，因为监视器可能不存在或不够准确，无法触发 KPI(关键 性能指标)的变化。 由于未检测到的 问题 和/或 不正确的 监视器设置 导致的传呼机噪音，这可能导致 SLA 违约。

To tackle this problem, Fairwinds has introduced a new open source software project called Astro. Astro is a Kubernetes operator that watches objects in your cluster for defined patterns and manages Datadog monitors based on this state.

#### Astro 提供了三个关键元素来大大简化 Datadog 监视器管理:

*   #### **Datadog 监视器的自动化生命周期管理:**给定配置参数，实用程序将自动管理 Kubernetes 集群中所有相关对象的已定义监视器。随着对象的变化，监视器会更新以反映该状态，从而消除了手动配置的需要。

*   #### **逻辑绑定对象之间的关联:** Astro 能够管理给定名称空间内所有对象的监视器。这确保了显示器配置之间的更大一致性。

*   #### **将 Kubernetes 对象中的值模板化到受管监视器中:**受管 Kubernetes 对象中的任何数据都可以插入到受管监视器中。这将生成更多信息性和情境化的警报，从而更容易对问题进行分类。

[![Try Astro Today](img/73fb89b760e7e4f19527ea76256ae988.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/ae582ca9-6cdc-4ca6-b29c-bac1250db55a)

## **入门**

您可以通过两个简单的步骤在集群中运行 Astro:配置您想要的监视器，并部署 helm chart。

监视器通过一个或多个 yaml 配置文件进行配置。 这些文件可以是本地文件路径或通过 URL 远程访问。 这些配置文件会定期重新加载，因此您所做的任何更改最终都会传播到您的显示器上。下面是一个配置示例:

In this example, a monitor would be created for any Kubernetes deployments that have the annotations listed in match_annotations (in this case, astro/owner: astro).  This particular monitor will alert you when a deployment has no pods available. Astro uses go templating and you can insert any variables from the Kubernetes object *or* from the cluster_variables section of your config file into monitors. Specific details about the config file syntax is available in Astro’s [readme](https://github.com/FairwindsOps/astro/blob/master/README.md).Once your config file is in place, you’re ready to deploy with helm! The helm chart is available [on GitHub](https://github.com/FairwindsOps/charts/tree/master/incubator/astro). If you’re installing with Reckoner, a tool to declaratively install and manage multiple Helm chart releases, here’s a snippet to install Astro:
In case you haven’t tried Reckoner, it’s available [on GitHub](https://github.com/fairwindsops/reckoner).

##  **欣赏天文**

现在，每次使用 astro/owner 注释创建部署时，astro 都会自动为它创建一个 Datadog 监视器，如果它关闭了，我们会收到一个警报。如果没有 Astro，我们将不得不依靠工程团队手动创建这些监视器，这既耗时又容易出错。

Astro 是一个伟大的 开源项目 ，它可以用来保持您的显示器同步，防止因显示器配置错误而中断，并对抗寻呼机疲劳。我们希望您喜欢自动化管理您的 Datadog 监视器！

