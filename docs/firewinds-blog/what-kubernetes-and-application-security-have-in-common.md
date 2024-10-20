# Kubernetes 和应用程序安全性的共同点是什么

> 原文：<https://www.fairwinds.com/blog/what-kubernetes-and-application-security-have-in-common>

 在过去的八年里，我花了大量时间帮助企业构建应用程序安全程序和开发安全产品，使开发人员和安全团队能够更轻松地完成共同的使命。

我通常将我过去的经验称为“测试左边的一切”，这意味着它主要集中在开发人员工具和持续集成(CI)上。也许你称之为 DevSecOps 的“DevSec”部分。然而，现在我已经加入了 [Fairwinds](/) ，我已经涉足了无所不包的持续部署(CD)和运营。我的新偏见受到了 Fairwinds 的核心使命的影响——帮助企业成功过渡到像 containers 和 Kubernetes 这样的云原生技术。

**核心任务继续**

随着 DevSecOps 的发展，最大的收获是看到了核心业务挑战的延续:加速开发速度，同时保持可靠性、可伸缩性和安全性。

即使在 Kubernetes 和 containers 的世界中，这两个业务目标仍然处于紧张状态。Kubernetes 正在通过技术采用生命周期，并开始成为主流解决方案。主流意味着取得平衡——提取足够的基础设施层，这样开发人员可以自由部署，而不会失去重要的治理和风险控制。

应用程序安全性比 Kubernetes 走得更远。我们知道这一点，因为它已经成为任何企业开发过程的重要部分。基于 SaaS 的平台已经成为将测试集成到 CI/CD 管道中的事实上的方法，而且还提供了 AppSec 项目所需的集中治理和报告。这种基于云的自助式完全集成方法使安全团队能够在不影响可见性的情况下与开发人员会面。

**历史重演**

Kubernetes 部署——管理无状态微服务如何在集群中运行——仍然会给开发、安全和运营带来麻烦。虽然开发团队创建集群和集成监控工具变得更加容易，但是配置和管理部署的过程仍然很复杂。

对于不熟悉 Kubernetes 甚至基础设施即代码的开发团队来说，很容易忽略部署配置的一些关键部分。例如，在没有准备就绪和活动探测的情况下，或者在没有资源请求和限制的情况下，部署可能看起来工作得很好。从安全的角度来看，当部署被授予超级用户访问权限时，这并不明显。

幸运的是，可以从应用程序安全领域学到一些经验。

Fairwinds 最近推出了两个开源项目——[fair winds Polaris](https://github.com/FairwindsOps/polaris)和[fair winds Goldilocks](https://github.com/FairwindsOps/goldilocks)——来应对这一挑战。Fairwinds Polaris 帮助工程师保持其部署清单符合最佳实践，检测与安全、网络、容器图像等相关的问题。Fairwinds Goldilocks 通过为 Kubernetes 部署推荐资源请求和限制(主要是 CPU 和内存设置)来节省工程时间。这两个开源工具都非常适合从开发到生产的交接，在发布之前为开发人员提供了一个反馈循环。

我们的 SaaS 平台[fair winds Insights](/insights)，旨在为企业解决“单一平台”需求。我们认为 Fairwinds Insights 是一种提供 Kubernetes 集群和部署配置可见性的重要方式，因此可以在多个团队之间轻松地对改进进行优先排序和规划。

Kubernetes 部署验证的新时代使开发团队能够快速行动，同时避免生产中代价高昂的安全性或可靠性问题。确保应用程序不仅安全地*构建*，而且安全地*部署*，这就是 DevSecOps 对于云原生应用程序开发如此重要的原因。让我知道你的经历和你的想法！

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)