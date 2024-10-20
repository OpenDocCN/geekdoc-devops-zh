# 开源项目:NOVA 3.1.0，不再仅仅面向 Helm 用户！

> 原文：<https://www.fairwinds.com/blog/open-source-project-nova-not-just-for-helm-users-anymore>

 到目前为止， [Fairwinds Nova](https://github.com/fairwindsops/nova) 只是一个释放头盔的工具。虽然这在过去几年对我们和其他用户非常有用，但我们认识到并不是每个人都使用 Helm (gasp)！因此，我们开始为 Kubernetes 用户提供额外的功能来检测他们集群中的过期版本。

## 容器更新建议

从 Nova 3.1.0 开始，您现在可以使用名为 containers 的新标志运行这个开源工具。该特性将检查 Kubernetes 集群中的所有容器映像，并在有新版本可用时通知您。

此外，新的 Nova 将向您展示三种不同的更新图像的方法。我们提供绝对最新版本、最新次要版本和最新补丁版本。让我们来看看它的实际应用:

$ nova 查找-容器

容器名称当前版本旧最新最新次要最新补丁

==============                                            ===============        ===     ======           =============          =============

quay.io/jetstack/cert-manager-controller 1 . 5 . 0 版真实版 1.8.0 版 1.8.0 版 1.5.5 版

quay.io/jetstack/cert-manager-cainjector 1 . 5 . 0 版真实版 1.8.0 版 1.8.0 版 1.5.5 版

quay.io/jetstack/cert-manager-webhook 1 . 5 . 0 版真实版 1.8.0 版 1.8.0 版 1.5.5 版

坞站. io/bitnami/外部 DNS 0 . 10 . 2-debian-10-r0 true 0 . 11 . 1 0 . 10 . 2-debian-10-r0 . 10 . 2

这里，我们运行的是 cert-manager v1.5.0，我们有多个更新选项。如果我们只想更新最新的补丁版本，我们可以转到 1.5.5 版。如果我们想要完整的最新版本，也可以使用 1.8.0 版。

## 已知限制

试图寻找容器图像的新版本的最大问题是，并不是所有的容器都在它们的图像标签中使用实际的 [semver](https://semver.org/) 版本。容器图像标签在技术上可以包含任何东西，有些模式非常常见。

例如，如果您有一个基于 alpine 的映像和一个基于 ubuntu 的映像，您可以用 v0.0.0-alpine 来表示它们。当试图比较版本时，这个决定会导致一些真正的问题。有了这个新功能，我们尽最大努力让版本参与进来，我们不能保证什么。所以，一定要小心！

[![Find Outdated or Deprecated Helm Charts Across Multiple Clusters and Teams](img/0e2544b6359c936ae89fd12418473c8a.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/f3b05cfd-929e-4ad5-b12f-8182c3e3157b)