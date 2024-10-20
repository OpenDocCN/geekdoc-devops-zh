# 新的 Fairwinds Insights 发布:地形扫描

> 原文：<https://www.fairwinds.com/blog/new-fairwinds-insights-release-terraform-scanning>

 说到安全问题，时间是至关重要的。平台工程团队需要能够在部署过程中尽早发现问题，同时能够立即采取行动。

## 通过 Terraform 扫描更快地检测安全问题

最近的一个版本扩展了 Fairwinds Insights 的“基础设施即代码(IaC)”功能和通过地形扫描向左移动的安全思维。Insights 用户现在可以使用 IaC 扫描来检查 Terraform 文件中的配置问题，这些问题可能会使您的工作负载和云基础架构面临风险。

Terraform 扫描会根据一系列最佳实践检查您的文件，然后生成操作项目，例如:

*   配置您的亚马逊网络服务(AWS) EKS 集群来加密 Kubernetes 静态秘密数据。这可以防止访问 Kubernetes [etcd 数据库](https://etcd.io/) 的人访问 Kubernetes 的秘密数据。

*   将您的谷歌云平台(GCP) GKE 集群配置为自动修复节点。这将自动替换停止响应或磁盘空间不足的 Kubernetes 节点。

*   确保您的 AWS 或 GCP 存储桶不允许公众访问对象。这验证了 S3 或 GCS 存储桶不允许对其文件的自由访问。

通过在拉动请求阶段进行整合——无论是通过 Fairwinds 的[GitHub integration](https://insights.docs.fairwinds.com/installation/ci/github/)还是在您最喜欢的 [CI 平台](https://insights.docs.fairwinds.com/installation/ci/insights-ci-script/)——交付团队都会受益于即时反馈循环，因此他们可以更快地解决问题。此外，领导者可以使用 [策略执行](https://www.fairwinds.com/enforce-kubernetes-policy) 来根据扫描结果控制管道或合并请求。

对于已经使用了 [IaC 扫描](https://www.fairwinds.com/blog/why-infrastructure-as-code-scanning-matters-for-kubernetes-configuration) 的 Insights 用户来说，利用 Terraform 扫描很容易！要启用与 CI 集成的 Terraform 扫描，只需将 CI 脚本升级到最新版本。那些使用自动扫描不需要采取任何进一步的行动；此功能已经启用。

还没有使用 Fairwinds Insights？[试用我们的免费层](https://www.fairwinds.com/blog/get-started-with-fairwinds-insights-free-tier),适用于多达 20 个节点、两个集群和一个存储库的环境。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)