# 迎接 Kubernetes 多集群管理的挑战

> 原文：<https://www.fairwinds.com/blog/challenges-kubernetes-multi-cluster-management>

 无论您是希望分离安全边界、隔离可怕的配置更改的影响、扩展更接近客户的应用程序、实现数据主权，还是追求多云梦想，您的生活中都可能有多个 [Kubernetes 集群](https://kubernetes.io/)。在这篇文章中，我将描述一些有助于成功管理多个集群的特征和工具。

## 对标准和流程的依赖

随着 Kubernetes 集群的增加，遵循标准和拥有可重复的流程对于产生一致的行为和维护秩序变得更加重要。

这种标准的一个例子是安装了相同版本的入口或 DNS 控制器的所有集群。附带的流程记录或编纂了休息良好的工程师关于如何安全升级这些控制器的多种观点。

标准和流程将随着您的基础架构的扩展而调整，您需要支持云、并非在所有地区都可用的服务或新架构之间的差异。有时会觉得维护标准和流程的额外负担阻碍了进步，但是一旦您达到逃逸速度，这些习惯会帮助您的基础架构发展得更快更远。保持标准的一部分包括适应变化。

*   Fairwinds 开源工具 [Pluto](https://github.com/FairwindsOps/Pluto) 帮助您检测 Kubernetes API 版本何时被[否决](/blog/running-kubernetes-v1-15-upgrade)或不再可用。例如，升级到 Kubernetes 1.16 可能会破坏您升级应用程序的能力，如果那些应用程序的清单没有提前升级的话— Pluto 帮助您了解您的 Kubernetes 清单或舵图需要如何与 Kubernetes API 一起发展。
*   类似地，Fairwinds [Nova](https://github.com/FairwindsOps/Nova) 是一个开源工具，它将舵版本与已知的舵库进行比较，并在您安装了过时的图表或过期版本时通知您。

## 政策

策略有助于防止对您的 Kubernetes 集群和工作负载进行非标准的更改。在开发或工程工作流的早期，策略的实施应该在必要的地方提供防护栏。例如，如果由于不安全的配置而不允许 Kubernetes 部署，理想情况下，它应该在持续集成期间失败，而不是在应用程序被部署到 Kubernetes 时失败。

*   Fairwinds [Polaris](https://github.com/FairwindsOps/polaris) 是一款开源工具，通过 CI/CD 集成和 Kubernetes 准入控制器将您的 Kubernetes 工作负载与最佳实践进行比较。准入控制器在 API 级别阻止资源部署到 Kubernetes 集群。Polaris 允许使用 JSON Schema 定义自己的标准，这与 Kubernetes API 规范相匹配。自定义 Polaris 检查的示例包括不允许从非标准注册表中提取图像，或者要求 Kubernetes 部署还伴随有 Pod 中断预算。

使用[开放策略代理](https://www.openpolicyagent.org/)或类似工具可以实现更具体的策略。例如，Kubernetes 资源的标签标准可以帮助您跟踪您的平台是如何被消费的，并简化功能分支的部署和清理。标签可用于指示 CI/CD 应在何处部署应用程序、开发人员访问开发环境所需的 RBACBindings、网络访问或何时清理短暂的开发环境。标签只有到处存在才可靠。通过用减压阀语言(开放策略代理使用的语言)编写更具表达性的策略，策略可以强制将特定标签设置为一定范围的值。

## 能见度

被动或短期调整可能会迷失在集群的海洋中。这里有一些工具可以帮助您发现车队中的意外偏差。

*   Fairwinds [Polaris](https://polaris.docs.fairwinds.com/) 提供集群内工作负载的仪表板视图，以及它们与最佳实践的关系。
*   Fairwinds [Goldilocks](https://github.com/FairwindsOps/Goldilocks) 通过在建议模式下使用 vertical-pod-autoscaler，帮助您获得“恰到好处”的 Pod 资源请求和限制，在仪表板中可视化。
*   Fairwinds [RBAC Lookup](https://github.com/FairwindsOps/rbac-lookup) 是一个命令行工具，它可以方便地找到向 Kubernetes 认证的用户、组或服务帐户的角色。

帮助管理多个仪表板，并提供一个统一的地方来管理来自这些优秀工具的结果...有一个应用程序可以解决这个问题。:)

## 费尔温德见解

[Insights](/insights) 将包括上述工具在内的优秀开源工具的结果聚集到一个视图中，这允许您跨多个集群评估和实施您的标准。Insights 使用 Polaris 和开放策略代理为您的部分或全部集群提供集中策略管理。无论您是希望针对 Insights 提出的问题发出松弛警报还是创建故障单，您的工程师都可以跟踪他们的修复情况，而无需经常返回 Insights 仪表盘。

> Fairwinds Insights 可供免费使用。你可以在 这里 [报名。](https://www.fairwinds.com/coming-soon)

跨团队和云管理多个 Kubernetes 集群不可避免地会引入不一致性，这会导致错误并耗费时间和生产力。我们的开源工具解决了其中的许多挑战，但是为了更大的规模和更好的管理，请查看 Fairwinds Insights。

[![Join the Fairwinds Open Source User Group today](img/8ab607311768483f3bb5136a75381d4b.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/b163554e-b5ef-4f40-a053-03afe6ecbee6)