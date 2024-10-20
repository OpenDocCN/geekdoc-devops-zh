# Kubernetes DevOps 提示 5:为什么将 imagePullPolicy 设置为 Always 比您想象的更有必要

> 原文：<https://www.fairwinds.com/blog/kubernetes-devops-tip-5-why-setting-imagepullpolicy-to-always-is-more-necessary-than-you-think>

 这篇博客将讨论围绕 Kubernetes 容器的 Pull 策略的更好实践的需求，该策略是使用 pod 规范中的 imagePullPolicy 字段设置的。当imagepulpolicy设置为 Always 时，用户可以确保每次启动 Kubernetes pod 时都部署最新版本的映像。

## 依靠缓存版本

依赖缓存版本的 Docker 图像会引入安全漏洞，因为 Kubernetes 会试图使用缓存版本的图像，而不验证它来自哪里。如果攻击者能够替换缓存的图像，并且 imagePullPolicy 被设置为 IfNotPresent ，那么 Kubernetes 将很乐意运行您放在节点上的任何图像。此外，不使用 Always pull 策略会导致每个节点上运行的映像发生变化。

例如，许多项目会发布带有图像标签 1 、 1.2 和 1.2.3 的版本 1.2.3 。关于 Kubernetes 配置，可以指定标签 1.2 ，因此当 1.2.4 发布安全补丁时，它会在下次重新安排或重启 pod 时自动流入。但是，如果用户没有指定imagepulpolicy:Always，那么 Kubernetes 集群将继续使用缓存的 1.2 标签，该标签仍然指向 1.2.3 。

为了解决这个问题，Kubernetes 使用了 imagePullPolicy 。根据 [Kubernetes 文档](https://kubernetes.io/docs/concepts/configuration/overview/#container-images) 和配置最佳实践，当 imagePullPolicy 设置为 Always 时，kubelet(也称为主“节点代理”)将查询容器映像注册表，以便在每次启动容器时将名称解析为映像摘要。

如果 kubelet 有一个包含本地缓存的精确摘要的容器图像，它就使用它的缓存图像。否则，kubelet 下载(或提取)带有解析摘要的图像，使用该图像启动容器。这里需要注意的是，将 imagePullPolicy 设置为 Always 不会绕过本地容器缓存机制，它只是验证缓存是否与上游源匹配。这意味着这种配置几乎没有缺点。

## 通过洞察力获得正确的设置

您可以通过确保将imagepulpolicy设置为 Always 来避免图像问题。当然，您的团队可以手动检查这个策略，但是这项工作会占用应用程序开发的时间、精力和资源。相反，您可以使用配置验证软件，如[fair winds Insights](https://www.fairwinds.com/?utm_source=adwords&utm_medium=ppc&utm_term=fairwinds%20insights&utm_campaign=Branded&hsa_cam=9424392662&hsa_mt=b&hsa_ver=3&hsa_src=g&hsa_ad=524170558549&hsa_net=adwords&hsa_tgt=kwd-1494176964684&hsa_acc=8748715703&hsa_grp=95380032853&hsa_kw=fairwinds%20insights&gclid=Cj0KCQjwl7qSBhD-ARIsACvV1X1g_8YMyD-_sAolWXtzaxdIFne7GApsVaI8szm73UGjQTqQrA4HS_UaApEMEALw_wcB)，来自动检查您的 imagePullPolicy 。

为了解决这个问题，Insights 会在未指定标签时触发警告，或者如果imagepulpolicy未正确设置为 Always 。然后，您的团队可以修复清单，避免应用程序停机、错误等问题。

Fairwinds Insights 还可以贯穿您的整个部署流程，这意味着从 CI/CD 到生产，始终会验证 imagePullPolicy 。

Fairwinds Insights 将通过扫描 CI 中的基础架构代码，并在 Kubernetes API 前作为准入控制器运行，从一开始就阻止这些工作负载进入您的 Kubernetes 集群。

有兴趣尝试见解吗？它可以免费使用。你可以在这里报名。安装 Fairwinds Insights 代理后，您将在 5-10 分钟内找到结果。您可以轻松地检查是否有 [超过许可的容器](https://www.fairwinds.com/blog/how-to-identify-over-permissioned-containers) ，以及其他可靠性问题，如缺少 [准备就绪或活性探测器](https://www.fairwinds.com/blog/how-to-identify-missing-readiness-probes-in-kubernetes) 。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png) ](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)