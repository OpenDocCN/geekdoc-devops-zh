# Kubernetes 基础教程:为什么&如何将图像拉取策略设置为“总是”

> 原文：<https://www.fairwinds.com/blog/kubernetes-basics-tutorial-how-to-set-image-pull-policy-to-always>

 Kubernetes 的入门似乎有点让人不知所措，因为虽然它提供了大量的灵活性和可伸缩性，但这些好处也伴随着复杂性。从一些基本的 [开始 Kubernetes 教程](https://training.fairwinds.com/) 可以帮助确保你不犯一些最常见的错误。我们在 Fairwinds 遵循的 Kubernetes 最佳实践之一是将 [imagePullPolicy](https://www.fairwinds.com/blog/kubernetes-devops-tip-5-why-setting-imagepullpolicy-to-always-is-more-necessary-than-you-think) 设置为“始终”

## 为什么您需要确保您的 imagePullPolicy 设置为“始终”

使用图像的缓存版本似乎更快更容易。不检查更新版本可以节省时间，并且您可能认为坚持以前的工作方式没有风险。毕竟，图像需要多久更新一次？比你想象的更频繁。 [漏洞](https://www.cve.org/About/Overview) 每天都有介绍、发现、披露。您正在使用的图像的缓存版本可能会过期几天、几周甚至几个月。

当您依赖于文档或图像的缓存版本时，这些旧版本会在您的环境中引入安全漏洞，因为 Kubernetes 会自动尝试使用图像的缓存版本，而不验证它来自哪里或是否需要更新。这些漏洞可能来自不安全的库或其他已导入容器映像的依赖项。这些漏洞可能会对您环境的其余部分构成风险。

## Kubernetes kube let 命令

[Kubelet](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/) 是一个在 Kubernetes 集群的每个节点上运行的进程。它根据指示在节点上创建、销毁和更新 pod 及其容器。kubelet 管理由 Kubernetes 创建的容器，并遵循一组 PodSpecs，这是一个描述 pod 的 YAML 或 JSON 对象。基本上，kubelet 确保 PodSpecs 中描述的容器运行正常。当 kubelet 需要创建一个新的容器时，它会引用 PodSpec 来决定如何做。

## 立方图像提取政策选项

Kubernetes 有三个 imagePullPolicy 选项:

1.  当 imagePullPolicy 设置为 Always 时，Kubernetes 总是从存储库中提取图像

2.  当 imagePullPolicy 设置为 IfNotPresent 时，Kubernetes 仅在图像不存在于节点上时提取图像，但它将运行您放置在节点上的任何图像

3.  当 imagePullPolicy 设置为 Never 时，Kubernetes 不拉图像；但是，如果图像存在于本地，kubelet 将尝试启动容器

对于 Kubernetes 来说，策略选项都是确定您是否想要拉一个策略的有效方法。如果您将 imagePullPolicy 设置为“Always ”,并且 kubelet 在本地缓存了一个具有相同摘要的容器映像，那么它将使用缓存的映像创建一个新的容器。否则，kubelet 下载带有已解析摘要的图像，并使用该图像启动容器。请注意，将 imagePullPolicy 设置为“Always”不会绕过本地容器缓存机制。它只是验证缓存中的图像是否与上游源匹配。这意味着这种配置几乎没有缺点。

如果您试图找出您的集群中正在运行哪些容器以及每个容器正在运行哪些版本， [Nova](https://nova.docs.fairwinds.com/) 是一个开源工具，它将向您显示当前安装的版本，无论是旧版本、最新的主版本、最新的次版本还是最新的补丁版本。

## 了解如何检查 imagePullPolicy 设置的容器

您可能想知道如何辨别您的 imagePullPolicy 设置。

1.  要检查容器的 imagePullPolicy 设置，可以使用 Docker 命令行； `run the command `docker inspect [container-name]`` ，显示您的集装箱配置

2.  在配置中，您可以看到“ImagePullPolicy”设置，它显示了容器的策略

3.  如果你想改变一个容器的镜像拉取策略，你可以使用 ``docker run`` 命令，这允许你在启动容器时设置一个特定的策略

## 如何将 imagePullPolicy 设置为“始终”

如果您不想检查每个容器的 imagePullPolicy 设置，可以使用 Kubernetes 治理软件，如[fair winds Insights](https://www.fairwinds.com/insights)来自动检查策略。因为这是一个 [Kubernetes 最佳实践](https://www.fairwinds.com/blog/why-arent-you-following-these-5-kubernetes-best-practices) ，所以它已经内置到见解中，便于您查看。Fairwinds Insights 会在未指定该标签或未将其设置为“Always”时触发警告，以便您可以轻松修复清单。您可以在 Action Items 下的 Insights 中找到有关 imagePullPolicy 设置为 always 的更多信息。

## 修复 imagePullPolicy 的业务优势

如果您已经阅读了本文，并且看到了能够跨多个集群和团队检查 imagePullPolicy 的技术价值，那么也请考虑一下业务优势。团队需要确保安全。确保您使用的映像没有漏洞，有助于防范不安全和不可靠的应用程序。

浏览 [快速教程](https://www.youtube.com/watch?v=ZCcwX09NFfw) 如何使用 Fairwinds Insights 将 imagePullPolicy 设置为“始终”。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)