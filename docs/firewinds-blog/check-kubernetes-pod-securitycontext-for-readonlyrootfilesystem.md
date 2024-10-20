# 检查 Kubernetes Pod SecurityContext 中的 readOnlyRootFilesystem

> 原文：<https://www.fairwinds.com/blog/check-kubernetes-pod-securitycontext-for-readonlyrootfilesystem>

 Kubernetes pod 安全策略支持围绕 pod 创建和更新的细粒度控制。 `[securityContext](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/)` 定义了一组 pod 运行时的约束条件。

`readOnlyRootFilesystem`是一个控制容器是否能够写入其文件系统的设置。这是在发生黑客攻击时最希望启用的功能——如果攻击者进入，他们将无法篡改应用程序或将外来可执行文件写入磁盘。

[Kubernetes 安全最佳实践](https://kubernetes.io/docs/tasks/configure-pod-container/security-context/) 提供关于配置 pod 或集装箱的 `ReadOnlyRootFilesystem` 的指导。因此，虽然该功能对于 Kubernetes 安全性至关重要，但如果您的用户没有部署将`securityContext`设置为 `readOnlyRootFilesystem?` 的 pod，那么会发生什么呢？最好的情况是，您的团队发现了这一点并应用了策略，最坏的情况是您的 pod 被黑客攻击。可能最好识别那些没有以只读方式运行的 pod。

## **自动检查 notReadOnlyRootFilesystem**

手动检查每个 pod 的 `securityContext` 容易出现人为错误且耗时。使用策略执行工具自动化这一过程有助于降低 Kubernetes 的安全风险。

Fairwinds Insights 是一个策略驱动的配置验证平台(社区版免费使用)，允许负责 Kubernetes 的团队识别何时设置了不正确的安全上下文。

> Fairwinds Insights 永远免费使用。搞定这里的[](https://www.fairwinds.com/coming-soon)。

Fairwinds Insights 是一款 SaaS 解决方案，它会根据您的要求自动扫描集群，以检查缺失的安全上下文。您的团队节省了识别和跟踪特权容器的时间，并能够利用这些时间来补救问题。

一旦安装了 Fairwinds Insights 代理，您将在 5-10 分钟内获得结果。当 `securityContext.readOnlyRootFilesystem` 不真实时，Fairwinds 的见解会提供警告。您还可以使用 Fairwinds Insights 来确保在整个部署过程中执行策略，以便为每个 pod 设置安全上下文。这样，通过从 CI/CD 到生产扫描您的配置，您将降低安全事故的风险。策略驱动的配置验证平台确保 Kubernetes 安全最佳实践在组织范围内得到遵循。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)