# 不要忘记检查 Kubernetes Pod SecurityContext 中的 readOnlyRootFilesystem

> 原文：<https://www.fairwinds.com/blog/dont-forget-to-check-kubernetes-pod-securitycontext-for-readonlyrootfilesystem>

 在 Kubernetes 中运行安全的工作负载具有挑战性。Kubernetes 使工程师能够使用 **securityContext** 字段配置容器的安全特性。使用这个 Kubernetes 安全上下文来保护 pods 可以确保每个资源都拥有它所需要的权限，同时也避免了过度许可的陷阱。安全上下文在粒度级别上定义权限，始终遵循最小特权原则。也就是说，您可以使用安全上下文定义的许多安全规则也可以通过 pod 安全策略来配置，这不是同一个工具。

为什么两者都有？因为安全上下文是 pod 安全策略的替代品，而 pod 安全策略可用于为 Kubernetes 集群中运行的所有 pod 配置权限，所以与可应用于单个 pod 的安全上下文相比，安全上下文提供的控制粒度更小。

### 安全 Pod 策略

Kubernetes 中的 pod 安全策略是一个集群级资源，它控制 pod 规范的安全敏感方面。这些策略定义了运行系统将接受的 pod 所需的一组条件，以及相关字段的默认值。虽然[PodSecurityPolicies](https://kubernetes.io/docs/reference/generated/kubernetes-api/v1.23/#podsecuritypolicy-v1beta1-policy)通过启用准入控制器来实施，但是如果最初没有授权适当的策略，则不会在集群中创建 pods。

控制容器是否能够写入其文件系统的一个设置是 **readOnlyRootFilesystem** ，如果可能的话，您肯定希望启用该功能，以便为您的容器化工作负载提供额外的深度防御。如果攻击者或不良分子设法破坏集群， **readOnlyRootFilesystem** 将防止他们篡改应用程序或将可执行文件写入磁盘。

### 最佳实践

Kubernetes 安全最佳实践提供了关于如何为 pod 或容器配置 readOnlyRootFilesystem 的指南。是的，这个特性对于 Kubernetes 的安全性至关重要，但是如果用户没有部署一个将 **securityContext** 设置为 **readOnlyRootFilesystem** 的 pod，会发生什么呢？在理想的情况下，您的团队能够识别这个问题并应用适当的策略。最糟糕的情况是，您的 pod 被破坏和损害，这意味着需要立即识别未以只读方式运行的 pod。

### 具有洞察力的自动检查

手动检查每个 pod 的**安全上下文**有一个问题——很容易出现人为错误，更不用说这非常耗时。使用策略执行工具自动化该过程可以大大降低 Kubernetes 的安全风险。

[Fairwinds Insights](https://www.fairwinds.com/?utm_source=adwords&utm_medium=ppc&utm_term=fairwinds%20insights&utm_campaign=Branded&hsa_cam=9424392662&hsa_mt=b&hsa_ver=3&hsa_src=g&hsa_ad=563830028156&hsa_net=adwords&hsa_tgt=kwd-1494176964684&hsa_acc=8748715703&hsa_grp=95380032853&hsa_kw=fairwinds%20insights&gclid=Cj0KCQjwmPSSBhCNARIsAH3cYgaPoOyVSV-nbYaNz0HZU3dn674AJngFY8ikP4-XoMPLKLYf1_q2B4UaApowEALw_wcB) ，我们的 SaaS 治理平台为 Kubernetes 提供解决方案。作为一个策略驱动的配置验证平台，Insights 帮助负责 Kubernetes 的团队识别何时设置了不正确的安全上下文。Fairwinds Insights 的客户可以在流程的每个阶段实施这些护栏和反馈循环——在拉动请求、部署时，以及针对集群中已部署的工作负载。

> 您可以永远免费使用 Fairwinds Insights。 [拿到这里](https://www.fairwinds.com/coming-soon) 。

根据您组织的要求，Fairwinds Insights 会自动扫描集群，以检查缺失的安全上下文。这意味着您的团队可以节省定位特权容器的时间，这些时间可用于修复问题。

安装 Insights 代理后，您将在 10 分钟内收到结果。当**security context . readonlyrootfilesystem**不为真时，Insights 会给出警告。Fairwinds Insights 还确保在整个部署过程中执行策略，因此为每个 pod 设置了安全上下文。这一举措通过扫描从 CI/CD 到生产的配置，减少了安全事故。Insights 还致力于确保整个企业的所有团队都遵循 Kubernetes 安全最佳实践。

[![Fairwinds Insights is Available for Free Sign Up Now](img/90e93a941f22f2087c3a229a91ea6c10.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/d329e036-9905-4715-85b8-31a98b50623c)