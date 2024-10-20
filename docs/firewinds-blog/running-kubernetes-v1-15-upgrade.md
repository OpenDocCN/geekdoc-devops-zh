# 运行 Kubernetes 版？可能是时候升级了

> 原文：<https://www.fairwinds.com/blog/running-kubernetes-v1-15-upgrade>

 最近，亚马逊网络服务(AWS)分享了一篇关于[计划用亚马逊弹性 Kubernetes 服务(亚马逊 EKS)升级 Kubernetes](https://aws.amazon.com/blogs/containers/planning-kubernetes-upgrades-with-amazon-eks/?nc1=b_rp)的帖子，部分原因是他们停止支持 Kubernetes 1.15，而在 2 月份，亚马逊 EKS 发布了对 Kubernetes 1.15 的支持。Kubernetes 继续更新——最新版本于 4 月 19 日发布，即[1.21](https://kubernetes.io/docs/setup/release/notes/)。(之前的版本是 1.20 版，发布于 2020 年 12 月 8 日——这是“[最热门的版本](https://kubernetes.io/blog/2020/12/08/kubernetes-1-20-release-announcement/)”)

总而言之，Kubernetes 进展很快——这是一个复杂的开源项目，正在迅速成熟。我们自己的[开源项目](/open-source-software)也与 K8s 密切相关，并且它们是活跃的和持续更新的。我们最近[成立了一个开源用户组](/open-source-software-user-group)，因此我们可以邀请更多来自更广泛的开源社区的贡献者来贡献想法和改进。

## 没有版本是永恒的...

预计将停止对任何软件的旧版本的支持；不可能一直支持所有发布的软件版本。旧产品的停产只是产品生命周期的一部分，它使公司能够继续开发和支持其产品的活动(和更安全)版本。不过，如果你还没有准备好，转换到新版本会很难，有时甚至很难知道你正在使用的软件产品的版本。

## 你的集群里有什么？

如果你正在使用亚马逊 EKS，但你不确定你是否在使用 1.15 版，我们可以通过我们的北极星、洞察或 Nova 开源项目来帮助你解决这个问题。弄清楚你正在使用什么以及你是否需要很快升级是很重要的，因为如果你仍然在 AWS EKS 上使用 K8s v.1.15，你将无法再创建这个版本的新集群。根据亚马逊的说法，最终所有集群都将更新到 1.16 版。

使用较新的(受支持的)Kubernetes 版本总是一个好主意，因为旧的、不受支持的版本不会获得安全性、稳定性或特性补丁。特别是对于从 1.15 到 1.16 的过渡，引入了一些您需要了解的 API 弃用。最好是对升级进行规划，以便在上线之前有时间测试任何更改，并确保您的应用程序和服务仍能正常工作。

## 规划您的升级

亚马逊分享的帖子中有很多有用的信息，关于[计划用亚马逊 EKS](https://aws.amazon.com/blogs/containers/planning-kubernetes-upgrades-with-amazon-eks/?nc1=b_rp) 升级 Kubernetes，包括与 1.16 中的 [API 弃用相关的信息，以及可以帮助你检查升级到 1.16 后会崩溃的东西的开源工具。我们创建了我们的开源项目，](https://kubernetes.io/blog/2019/07/18/api-deprecations-in-1-16/) [Pluto](https://github.com/FairwindsOps/pluto) ，以帮助用户在他们的基础设施即代码仓库和 Helm 版本中找到不推荐的 Kubernetes API 版本。如果您在升级过程中需要更具体的帮助，或者您对一个跨团队和集群提供一致性的平台感兴趣，以简化您的 Kubernetes 部署，[联系](/fairwinds-insights-schedule-demo) —我们很乐意提供帮助。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)