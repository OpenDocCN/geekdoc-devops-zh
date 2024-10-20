# 常见的 Kubernetes 错误配置漏洞

> 原文：<https://www.fairwinds.com/blog/kubernetes-misconfigurations>

 保护 Kubernetes 中的工作负载是整个集群安全性的一个重要部分。总体目标应该是确保容器以尽可能小的特权运行。这包括避免权限提升、不作为根用户运行容器，以及尽可能使用只读文件系统。

由于疏忽或缺乏经验，安全漏洞可能会溜进生产中。交付的速度与关键的安全保护措施经常是不一致的，因为团队试图平衡工程的速度与安全的反应速度。这种平衡行为会导致混乱的 Kubernetes 配置和不必要的风险。如果开发人员由于缺乏经验或疏忽而错误配置工作负载，就会出现问题。

我们最近的 [Kubernetes 配置基准报告](/kubernetes-config-benchmark-report)告诉我们，并不是所有的组织都已经在 Kubernetes 环境的正确配置上找到了稳定的立足点，他们甚至还没有达到一半。

如果没有被发现和解决，小的错误配置会产生大的安全漏洞。例如，一个常见的漏洞是容器运行时具有比所需更多的安全权限，例如根级别的访问权限。在特定的配置下，容器可以提升它自己的特权。因为配置不是默认建立的，所以有安全意识的团队需要显式地设置它们。

不幸的是，单个应用程序开发人员经常忽略每个工作负载的安全配置。例如，通常更容易过度授权一个具有 root 访问权限的部署来让某些东西工作。强迫个体贡献者设计他们自己的安全配置几乎确保了不一致性和错误。

## 什么是安全错误配置？

Kubernetes 的安全错误配置经常发生在配置还没有被设置的时候。这通常是因为最快的生产方式是通过设置阶段。一些示例安全配置包括 hostIPCSet、privilegeEscalationAllowed、insecureCapabilities 或 privilegeEscalationAllowed。点击查看完整列表[。](https://polaris.docs.fairwinds.com/checks/security/)

## 避免错误配置部署的提示

避免错误配置需要几样东西。首先，DevOps 团队需要定期扫描 Kubernetes 集群，以确保它们配置正确。通常，组织使用电子表格手动完成这项工作，然后交给开发人员进行修改。不幸的是，这些变化并不总是发生，因此循环不断重复。避免错误配置的首要技巧是从开发到生产自动连续扫描集群。这样做有助于避免这些错误配置进入任何生产环境。

更好的是，使用准入控制器来防止错误配置可以帮助开发人员更好地配置 Kubernetes。准入控制器可以是 CI/CD 过程的一部分，有助于在检查生产中的误配置之前扫描集装箱。任何问题都会在投入生产之前通知开发人员。这有助于开发人员修复他们自己的问题，并解放开发人员，使他们不再像 Kubernetes 服务台那样行事。

## 常见的 Kubernetes 安全错误配置

那么，如何快速、主动地识别 Kubernetes 的安全错误配置以防止安全风险呢？根据我们的经验，有八种常见的 Kubernetes 安全错误配置会导致易受攻击的部署。

不识别和解决这些安全设置配置可能会产生负面的业务后果。例如，如果一个[容器以 root](/blog/how-to-identify-kubernetes-pods-running-root) 身份运行，但不一定需要这种级别的访问权限，那么一个恶意容器就有权限窃取数据或对系统造成其他损害。

| **配置** | **严重性** | **描述** |
| security.hostIPCSet | 危险 | 配置 hostIPC 属性时失败。 |
| security.hostPIDSet | 危险 | 配置 hostPID 属性时失败。 |
| security . notreadonlyrootfilesystem | 警告 | 当 security context . readonlyrootfilesystem 不为 true 时失败。 |
| security.privilegeEscalationAllowed | 危险 | 当 security context . allowprivilegescalation 为 true 时失败。 |
| security.runAsRootAllowed | 危险 | 当 securityContext.runAsNonRoot 不为 true 时失败。 |
| security.runAsPrivileged | 危险 | 当 securityContext.privileged 为 true 时失败。 |
| 安全.不安全能力 | 警告 | 当 securityContext.capabilities 包含此处列出的功能之一[时失败](https://github.com/FairwindsOps/polaris/blob/master/checks/insecureCapabilities.yaml) |
| 安全.危险能力 | 危险 | 当 securityContext.capabilities 包含此处列出的功能之一[时失败](https://github.com/FairwindsOps/polaris/blob/master/checks/dangerousCapabilities.yaml) |

为了解决这些常见的 Kubernetes 安全威胁，我们的团队构建了一个开源工具 [Polaris](https://www.fairwinds.com/polaris) ，它可以检查 Kubernetes pods 和容器的 securityContext 属性中的配置。北极星终结的地方就是 Fairwinds Insights 的起点。

[Fairwinds Insights](https://www.fairwinds.com/insights) 是一款配置验证工具，通过审计工作负载和验证配置的弱点、容器漏洞和错误配置的部署，提供对组织的 Kubernetes 安全状况的可见性。

> 对使用 Fairwinds Insights 感兴趣吗？免费提供！ [在此了解更多](https://www.fairwinds.com/coming-soon) 。

Fairwinds Insights 不仅提供调查结果，还保存所有集群结果的历史记录，并提供补救指导，从而实施北极星检查。Fairwinds Insights 允许您跟踪安全性、效率和可靠性问题并确定其优先级，跨团队协作，并在应用程序从开发进入生产时应用最佳实践。

了解有关 Fairwinds Insights 的更多信息或[获取关于如何管理 Kubernetes 配置以提高安全性、效率和可靠性的论文](https://www.fairwinds.com/manage-kubernetes-configuration-security-efficiency-reliability)。

[![Download Now](img/3992d90d5c64e188a10cbb0110bd5e83.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/c7afd087-bcd4-4009-a300-9aeadce17975)