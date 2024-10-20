# 在 Kubernetes 实施 CI/CD 管道的最佳实践

> 原文：<https://www.fairwinds.com/blog/best-practices-for-implementing-ci-cd-pipelines-in-kubernetes>

 在 Fairwinds，我们对如何在 Kubernetes 和 Docker 中实现 CI/CD 管道采取了固执己见的方法。在以下部分，我将概述我们观点的指导原则，然后根据 [Kubernetes 最佳实践](/kubernetes-best-practices-comprehensive-white-paper)展示我们使用的典型 CI/CD 工作流程。我还将分享一些代码，如果您愿意，可以尝试一下。

这些原则没有一个是全新的或惊天动地的，但它们的结合在 Kubernetes 中创造了一个可重复的、可持续的、安全的 CI/CD 管道，受到了我们许多客户的喜爱。

## CI/CD 指导原则

### 容器应该是不可变的

Docker 允许你在推送容器时重写标签。一个常见的例子是最新的标签。这是一种危险的做法，因为它可能会出现您不知道容器中正在运行什么代码的情况。相反，我们采用所有标签都是不可变的方法，因此我们将它们绑定到一个不可变的唯一值上，这个值可以在我们的代码库中找到——提交 ID。这直接将容器与构建它的代码联系起来，消除了我们确切知道容器中有什么的疑问。

### 释放被测试的容器

根据第一个原则，我们已经部署到一个 staging/dev/QA 环境中，然后经过测试(希望如此)的不可变容器应该与部署到生产环境中的容器完全相同。这消除了在部署到生产环境中时可能会发生变化的顾虑。我们通过使用 git 标签触发产品发布并使用标签的提交 ID 发布容器来处理这个问题。

### 将秘密和配置放在容器之外

因为容器应该被认为是我们构建过程中不可变的工件，所以配置永远不应该被“烘焙”配置应该存储在 Kubernetes 集群中的一个 [configmap](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) 中，而秘密应该存储在 Kubernetes [秘密](https://kubernetes.io/docs/concepts/configuration/secret/)中，你猜对了。这允许在不改变容器本身的情况下，根据环境配置容器的部署。

### 带舵展开

在 Fairwinds，我们使用 [Helm](https://helm.sh/) 模板化所有部署到 Kubernetes 的 yaml。这允许多个环境及其配置的简单配置。此外，它使得跟踪逻辑分组或“发布”变得更加容易

### 使用基于 Git 的工作流

所有 CI/CD 管道都应该由 Git 操作触发。这符合正常的开发人员工作流程，并使开发人员更容易访问管道。此外，它可以很好地与前面的不变性概念(与提交 ID 相关联)一起工作。

### 在 Docker 内部构建

我们强烈反对使用一位前同事所称的“手工制作的机器”,这种机器包含各种随机的依赖项和工具。如果你在容器中构建*，你可以控制代码中所有的依赖项和工具，永远不用担心哪个“构建者”在运行你的管道。Docker 的[多级构建](https://docs.docker.com/develop/develop-images/multistage-build/)使这一点变得容易，同时也使您部署的运行时映像尽可能小。*

### 利用缓存

Docker 图像可以被缓存和重用。我们建议尽可能多地使用这个特性，以便将构建时间保持在最短。应该将缓存映像推送到存储库，或者应该使用 CI/CD 系统的缓存功能来最大限度地提高效率。

### 奖励:做 Kubernetes 安全和配置扫描

您的 CI/CD 管道是捕捉容器和部署代码中的安全和配置问题的绝佳机会。我们使用 [Fairwinds Insights](https://fairwinds.com/insights) 在我们的管道中执行几种不同的检查，如[北极星](https://github.com/FairwindsOps/polaris)一个由 Fairwinds 和 [Trivy](https://github.com/aquasecurity/trivy) 开发的开源项目。

> 对使用 Fairwinds Insights 感兴趣吗？免费提供！ [在此了解更多](https://www.fairwinds.com/coming-soon) 。

### CI/CD 管道示例

我将一步一步地展示我们将为客户建立的正常渠道。它考虑了前一节中的所有原则，并将提供一条“用 X 个简单的步骤”从代码到产品的路径

过程是这样的:

1.  制作一个特征分支
2.  推动那个分支并打开一个 PR——它被构建并部署到一个短暂的名称空间，您可以用它来测试变更。迭代直到它准备就绪
3.  将功能分支合并到主功能——这将触发一个构建和一个到临时环境的部署
4.  一旦试运行环境达到生产就绪状态，标记发布
5.  使用为试运行而构建的相同容器，将标签部署到生产中，但是使用生产配置

在这个过程中，我们可以看到几个“循环”这些是反馈循环，可用于提高质量、测试代码等。我们的目标是使这些变得小而快，这样我们就可以发布可测试和可管理的代码。

### 前进，制造管道

如果你喜欢我们在 Fairwinds 做 CI/CD 的方式，你可以像我们一样拥有自己的管道！我们使用一组 bash 脚本来完成所有这些工作。我们只需要创建一些配置文件，我们得到的设置与本文中的非常相似。回购被称为 [rok8s-scripts](https://github.com/reactiveops/rok8s-scripts) (读作“火箭脚本”)，和往常一样，PRs 被接受。

[![Free Download: Kubernetes Best Practices Whitepaper](img/b53e4ee22b6ef19bc06a035649ea1dc6.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/4f92c7e1-1646-4985-9a0a-b1091903dddb)

*最初发布于 2019 年 3 月 6 日下午 1:02:00，更新于 2021 年 2 月 23 日*