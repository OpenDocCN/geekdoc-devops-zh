# Kubernetes 如何:确保 imagePullPolicy 设置为 Always

> 原文：<https://www.fairwinds.com/blog/kubernetes-how-to-ensure-imagepullpolicy-set-to-always>

 依赖缓存版本的 Docker 映像可能会成为一个安全漏洞。默认情况下，如果图像尚未缓存在试图运行它的节点上，将会提取该图像。这可能会导致每个节点上运行的映像发生变化，或者可能提供一种无需直接访问 ImagePullSecret 即可访问映像的方法。

例如，您可能会故意覆盖一个特定的标记，并希望您的工作负载引入这些更改。更具体地说，许多项目将发布带有图像标签 1 的版本 1.2.3 、 1.2 和 1.2.3 。在 Kubernetes 配置中，您可以只指定标签 1.2，这样当 1.2.4 发布了一个安全补丁时，它会在下次重新安排或重启 pod 时自动流入。但如果不指定imagepulpolicy:Always，集群将继续使用缓存的 1.2 标签(仍然指向 1.2.3 )。

为了克服这个问题，Kubernetes 使用了 imagePullPolicy 。当设置为 Always(imagepulpolicy:Always)，“每次 ku elet 启动一个容器时，ku elet 都会查询容器映像注册中心，将名称解析为映像摘要。如果 kubelet 有一个容器图像，其中包含本地缓存的精确摘要，则 kubelet 使用其缓存的图像；否则，kubelet 下载(拉取)带有已解析摘要的图像，并使用该图像启动容器。来源: [Kubernetes 文档](https://kubernetes.io/docs/concepts/configuration/overview/#container-images) 。

## 确保 imagePullPolicy 设置为“始终”

通过确保将 imagePullPolicy 设置为 Always，避免图像出现问题。您的团队当然可以手动检查该策略，但这需要花费您的应用程序的时间和精力。相反，使用像 [Fairwinds Insights](/insights) 这样的配置验证软件来自动检查你的 imagePullPolicy 。Fairwinds Insights 会在未指定标签或未将 imagePullPolicy 设置为 Always 时触发警告。然后，您的团队可以轻松地修复清单，帮助避免应用程序出现问题(停机、错误等)。

Fairwinds Insights 还可以贯穿您的整个部署流程，因此从 CI/CD 到生产， imagePullPolicy 始终是指定的。虽然这篇博客文章讨论了如何识别违规工作负载，但 Fairwinds Insights 也有助于通过扫描 CI 中的基础架构代码，并在 Kuberenetes API 前作为准入控制器运行，从一开始就防止这些工作负载进入您的集群。

通过舵图 [创建账号](/insights-pricing) ，创建集群 [安装代理](https://www.youtube.com/watch?v=QYwNmtJc5no) 可以免费试用。一旦安装了 Fairwinds Insights 代理，您将在 5-10 分钟内获得结果。您可以轻松检查[超过许可的容器](/blog/how-to-identify-over-permissioned-containers)以及其他可靠性问题，如 [缺少就绪或活性探测器](https://www.fairwinds.com/blog/how-to-identify-missing-readiness-probes-in-kubernetes) 。