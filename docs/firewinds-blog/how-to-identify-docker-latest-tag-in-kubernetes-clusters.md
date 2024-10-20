# 如何识别 Docker:Kubernetes 集群中的最新标签

> 原文：<https://www.fairwinds.com/blog/how-to-identify-docker-latest-tag-in-kubernetes-clusters>

 当你的团队用新功能更新一个应用程序或解决错误时，你将把最新的图像推送到你的 Docker 注册表。在 Kubernetes 中，Docker 的最新标签默认应用于没有指定标签的图像。但是，不指定图像的特定版本会导致各种各样的问题:

*   基础映像可能包含意外的更改，每当提取最新的映像时，这些更改都会破坏您的应用程序。
*   对图像的多个版本重复使用相同的标签会导致同一集群中的不同节点具有图像的不同版本，即使标签是相同的。
*   最新的标签很容易被意外覆盖，并可能导致您将开发分支投入生产。

> Kubernetes 文档建议:“注意:在生产环境中部署容器时，应该避免使用:latest 标记，因为很难跟踪哪个版本的映像正在运行，也很难正确回滚。”

## 识别 Docker:Kubernetes 集群中的最新标签

应定期确定哪些集群正在使用 Docker 最新标签，以避免出现问题。使用 Kubernetes 配置验证平台，只要识别出最新的标签，您就会收到警报。然后，您的团队可以轻松地修复清单，帮助避免应用程序出现问题(停机、错误等)。

> Fairwinds Insights 社区版永远免费使用。在此 注册 [试用完整版 30 天。在 GKE、阿拉斯加或 EKS 进行测试，或者在测试集群上运行。](https://insights.fairwinds.com/auth/register/)

Fairwinds Insights 能够在您的整个部署流程中执行 Kubernetes 策略，因此从 CI/CD 到生产，没有默认的最新标签。虽然这篇博客文章讨论了如何识别违规工作负载，但 Fairwinds Insights 也有助于通过扫描 CI 中的基础架构代码，并在 Kubernetes API 前作为准入控制器运行，从一开始就防止这些工作负载进入您的集群。

通过舵图 [创建账号](/insights-pricing) ，创建集群 [安装代理](https://www.youtube.com/watch?v=QYwNmtJc5no) 可以免费试用。一旦安装了 Fairwinds Insights 代理，您将在 5-10 分钟内获得结果。您可以轻松检查 [imagePullPolicy 是否设置为 Always](/blog/kubernetes-how-to-ensure-imagepullpolicy-set-to-always) 以及其他可靠性问题，如 [缺少就绪或活动探测器](https://www.fairwinds.com/blog/how-to-identify-missing-readiness-probes-in-kubernetes) 。