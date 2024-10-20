# GitOps 最佳实践和您需要遵循的 Kubernetes 护栏

> 原文：<https://www.fairwinds.com/blog/gitops-best-practices-and-the-kubernetes-guardrails-you-need>

 GitOps 是一个大家都在谈论的流行语，这个词是由 Weaveworks 在 2017 年创造的。它通过拉请求 方法使用 [操作来定义和管理网络、基础设施、应用程序代码和 GitOps 管道。GitOps 是一种管理由 Kubernetes 支持的云原生系统的方法。](https://www.weave.works/blog/gitops-operations-by-pull-request)

## 什么是 GitOps

Weaveworks 有很多很酷的开源工具，并且已经在 Kubernetes 领域存在了一段时间。根据 Weaveworks 网站 上的一个故事 [，那里的团队投入了大量精力来改善他们的 Kubernetes 运营，方法是创建不同的中转区，设置多个可靠性区域，并创建内置监控和安全性。在 2016 年的某个时候，一名工程师告诉团队，他正在推出一项可能会消除一切的变革。确实如此。但是因为他们拥有 Git 配置文件中描述的一切，所以他们能够在不到 45 分钟的时间内恢复他们的系统。](https://www.weave.works/blog/the-history-of-gitops)

大量工作投入到构建他们的系统中，以使其能够如此快速地恢复，这使他们能够确定使快速恢复成为可能的因素。就在那时，他们提出了 GitOps 的概念。在开源分布式版本控制系统 [Git](https://git-scm.com/) 中，他们列出了操作他们的 Kubernetes 系统需要遵循的原则——该系统基于系统外部存在的系统模型，使整个系统的操作自动化。

GitOps 在 2018 年被全行业采用，2020 年 11 月，亚马逊、Codefresh、GitHub、微软和 Weaveworks 创建了 GitOps 工作组，该工作组现在是一个 [开放 CNCF 社区项目](https://opengitops.dev/about/) 。这个小组的目标是定义什么是 GitOps，并使它与供应商无关。

## GitOps 的四个主要原则

1.  **【GitOps】是陈述性的，** 意思是强调事实而不是说明。就像 Kubernetes 一样，你告诉它应该是什么，而不是怎么做。

2.  **GitOps 使用 Git 作为真理源** 并且 Git 真理源是版本化的和不可变的。这是 GitOps 的好处之一。

3.  **GitOps 专注于自动拉取机制。** 在我们的标准流程中，您在您的 [CI 管道](https://www.fairwinds.com/blog/best-practices-for-implementing-ci-cd-pipelines-in-kubernetes) 中完成所有的构建和测试；在这里，您可以使用命令将所有内容推送到您的集群。GitOps 的哲学是，你的集群应该是 ***拉动*** 那些变化本身，你不应该去推动它。

4.  **你的集群不断被调和** 。它利用控制循环来关注事情应该如何发展，并使您的集群保持这种状态，这很像 Kubernetes 本身的内部结构，它完全是关于监视事情状态与您希望事情发展的方式的控制器。

在 Fairwinds，我们在内部使用 GitOps 来管理基础设施的附加组件，如 cert-manager、ingress-nginx 和 external-dns。我们使用 [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) ，这是市面上少数几种不同的 GitOps 工具之一。您在集群中部署 ArgoCD，并将其配置为监视特定的 repo，然后将该 repo 中的内容与特定的一个或多个集群进行协调。它监视集群，然后协调集群以保持这种状态，这在 Git 中。

## 支持 GitOps 的工具

### 计算者

Fairwinds 构建和使用的一个开源工具是 [【计算器】](https://reckoner.docs.fairwinds.com/#requirements) ，这是一个命令行助手，用于[Helm](https://github.com/helm/helm)创建一个声明性语法来管理单个位置的多个发布，并允许从 git 提交/分支/发布安装图表。

清算者允许你在一个 YAML 文件中声明一大堆头盔释放。每个版本都是一个舵图，以及相应的值。在 YAML 文件中，你也可以告诉计算者生成一个 ArgoCD 应用程序清单。ArgoCD 应用程序中可用的大多数选项，如 autosync，在 Reckoner 中也可用。

然后，Reckoner 在 manifest 目录中创建一个文件夹结构，每个应用程序有一个文件夹，还有一个包含所有 ArgoCD 应用程序的文件夹。然后，您可以创建一个应用程序的应用程序，指向该文件夹并同步应用程序。因此，您唯一需要手动管理的是定义您的应用程序的单个 ArgoCD 应用程序清单。

当您创建一个新的应用程序时，它会同步到 ArgoCD 中，然后继续同步该应用程序。模板化到每个文件夹中的清单是应该应用到集群的声明性状态，因此当您对其进行更改时，您可以在 Git pull/merge 请求中看到更改。通过模板化这些清单，您可以在更改进入集群之前获得将要发生的完全不同的确切更改。这种增加的透明度为您提供了您需要的所有信息，而不是您必须在舵图或 ArgoCD 舵应用程序中搜索的更有限的信息。

清单还允许您对 CI/CD 系统中的 YAML 进行静态分析和运行 CI/CD 检查。您还应该创建一个检查来验证计算器模板是否被正确执行。它验证没有 Git 更改，也没有手动修改 YAML。这支持 GitOps 原则，即确保只有一个源，其中没有未跟踪的更改。

### 立方结构改革

您还需要运行一个[kube conform](https://github.com/yannh/kubeconform)来检查您的模板化清单，因为虽然 Helm 图表可能会产生一些 YAML，但 YAML 可能没有根据 Kubernetes 的模式进行验证。Helm 不会在 Helm 模板上这样做，因为它无权访问集群。Kubeconform 是一个 Kubernetes 清单验证器；它获取所有清单，并根据 Kubernetes 存储库中的模式对它们进行验证。kubeconform 可以做的另一件事是创建一个 JUnit 文件，您可以将它发送到您的 CI/CD 系统，让它显示您的每个单独测试的结果。

### 北极星

采用 GitOps 时，重要的是将正确的护栏放置到位，并遵循 Kubernetes 的最佳实践，以确保您构建的安全性和稳定性。 [北极星](https://www.fairwinds.com/polaris) 帮你做到。这是另一个可以在 CI/CD 中运行的 Fairwinds 开源工具。它检查 [Kubernetes 最佳实践](https://www.fairwinds.com/blog/intro-kubernetes-best-practices) ，特别是与安全性、效率和可靠性相关的最佳实践。Polaris 包括 30 多种内置配置策略，并将验证它们，甚至修复 Kubernetes 资源，以确保遵循配置最佳实践。Polaris 检查整个存储库和清单集的配置问题，这有助于您与 GitOps 最佳实践保持一致。

## 费尔温德见解

您可以使用[fair winds Insights](https://www.fairwinds.com/insights)获得完整的存储库报告，该报告将在一个地方向您显示清单目录中发现的所有问题，而不是为每个工具编写单独的检查。它包括:

*   一份 [琐碎的](https://github.com/aquasecurity/trivyhttps:/github.com/aquasecurity/trivy) 报告，显示其发现:漏洞、错误配置、秘密、容器中的软件材料清单、Kubernetes、代码库、云等等。

*   一份显示配置问题的报告。

*   A[Pluto](https://pluto.docs.fairwinds.com/)关于代码仓库和 Helm 版本中弃用的 Kubernetes API 版本的报告。

*   An[Open Policy Agent(OPA)](https://www.openpolicyagent.org/)报告，这样您就可以通过减压阀应用您自己的策略，将它们应用到您的 YAML，并将其添加到您的支票中。

您可以打开“自动扫描细节”来自动查看这些报告，并根据调查结果创建票据。您还可以使用吉拉集成为发现创建标签，并将它们分配给需要修复它们的团队。Fairwinds Insights 为多达 20 个节点、两个群集和一个存储库的环境提供了一个免费层 。

## GitOps +立方结构护栏

通过遵循这些 GitOps 最佳实践，并使用工具来符合 Kubernetes 最佳实践并实现 guardrails，您可以确保您的 GitOps 流程是安全的、可靠的和可维护的。这将帮助您保持 Kubernetes 环境按预期运行，同时确保所做的任何更改都是有效的，不会导致意外问题。

观看网络研讨会，更好地了解 GitOps 如何工作，以及这些开源工具如何帮助您实现符合 GitOps 最佳实践的 [Kubernetes 护栏](https://www.fairwinds.com/kube-clinic-gitops-12-07-reg) 。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)