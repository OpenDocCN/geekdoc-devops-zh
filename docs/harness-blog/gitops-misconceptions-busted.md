# 5 个 GitOps 错误概念被揭穿|装具

> 原文：<https://www.harness.io/blog/gitops-misconceptions-busted>

GitOps 仍然是新的，但它有巨大的优势。我们看到了围绕 GitOps 的一些误解，所以今天，我们要揭穿它们。

随着公司开始采用云原生技术来尽可能简化软件开发和部署，新方法应运而生。最著名的方法之一是 GitOps。随着 Kubernetes 成为容器编排的标准工具，GitOps 作为执行 Kubernetes 相关部署的默认方法越来越受欢迎。GitOps 仍然是新的，但它有巨大的优势。此外，我们看到一些 GitOps 的错误概念正在流传——所以今天，我们将打破它们。

## 1.GitOps 只是一个流行词

当 DevOps 这个词在 2009 年被创造出来的时候，它似乎只是另一个流行词。但是我们看到了过去十年的现实，几乎地球上的每家公司都想采用 DevOps 原则来实现他们加速软件交付过程的目标。我们提到 DevOps 是因为它是一个值得讨论的术语的很好的例子，而且由于所有的 GitOps 都是 DevOps，所以它更具有话题性。由于有些人想把 GitOps 当成另一个时髦词，让我们来探究一下为什么它值得大肆宣传。

GitOps 的接受度已经非常高了。在 5 月份的 Kubecon EU 2021 上，人们对 GitOps 主题的兴趣如此强烈，以至于 CD 会议(由 [CNCF](https://www.cncf.io/) 主持)不得不单独成立一个 [GitOps 会议](https://events.linuxfoundation.org/gitopscon-europe/)。

此外, [2021 DORA 报告的](https://cloud.google.com/blog/products/devops-sre/announcing-dora-2021-accelerate-state-of-devops-report)调查结果显示，GitOps 方法和原则对高绩效团队至关重要。[2021 年举办的以 GitOps 为中心的虚拟会议 ArgoCon](https://www.techstrongevents.com/argocon21/) 取得了巨大成功，全球有许多优秀的演讲者和与会者。此外，去年对关键词“ [GitOps](https://harness.io/blog/devops/gitops/) ”的搜索兴趣呈指数级增长。

Source: [Keywords Everywhere](https://keywordseverywhere.com/)

云计算领域广受尊敬的领导者凯尔西·海托华在推特上写道:“GitOps 是自配置即代码以来最好的东西。”最后，去年的 [AWS 集装箱安全调查](https://aws.amazon.com/blogs/containers/results-of-the-2020-aws-container-security-survey/)显示，64.5%的受访者已经使用 GitOps。所有这一切都表明 GitOps 不仅仅是一个时髦的词——它将留在这里，并准备主宰软件世界。

## 2.GitOps 仅用于与 Kubernetes 相关的部署

这是迄今为止所有 GitOps 误解中最大的一个。GitOps 不仅仅用于与 Kubernetes 相关的部署。GitOps 是一种适合任何基础设施自动化类型工作流的方法，从 Kubernetes 到云供应商，甚至是手工编写的 shell 脚本。使用 GitOps，您可以实现基础设施自动化和应用程序部署。此外，GitOps 可以应用于任何以声明性文本格式作为基础设施的东西，比如 [Terraform](https://harness.io/blog/devops/terraform-201-tutorial/) 。

GitOps 和 Kubernetes 就像是最好的朋友:他们在一起很好，但没有彼此也可以存在。通过保持 GitOps 的核心原则——保持 Git 作为事实的单一来源，这种方法可以应用于任何工作流。

## 3.GitOps 和 DevOps 是一样的

DevOps 是一种文化现象。这是一种通过增强协作和消除孤岛来弥合开发和运营团队之间差距的方法。GitOps 可以被视为 DevOps 的一个子集，它通过使用 Git 作为真实的单一来源来促进开发和运营之间的协作。DevOps 下有很多工具，Git 就是其中之一。简而言之，GitOps 简化了应用程序部署。

区分 DevOps 和 GitOps 是没有意义的，因为我们已经知道只有在 DevOps 道路上的公司采用 GitOps 来丰富和增强他们的开发过程。我们可以这样总结:**所有的 GitOps 都是 DevOps，但不是所有的 DevOps 都是 GitOps。**

## 4.GitOps 是关于使用 Git 的

GitOps 不仅仅是 Git。如果是这样的话，那么我们都已经在做 GitOps 了！在 GitOps 中，我们所有的配置文件和工件都存储在一个源代码控制库中。您需要一个 CD 工具，它在您的集群中托管一个类似代理的系统，以扫描实际和期望的状态。每当开发人员的提交发生漂移或更改时，代理会检测到这种更改，并提取它们以保持状态一致(实际的和期望的)。

GitOps 不止是 Git 它基于可靠的原则工作，例如单一的事实来源、声明性描述、检测偏差/差异、通过拉式请求的变更以及自动化变更批准。

## 5.GitOps 通过推和拉两种模式完成

GitOps 被一些人误解了，因为他们认为这都是关于使用 Git 的，如上所述。有些人还认为 [CI/CD](https://harness.io/blog/devops/what-is-ci-cd/) 已经使用了 GitOps 模型，但这也是不正确的。CI/CD 使用推模型，开发人员将代码推送到源代码管理系统(SCM ),后者触发 CI 服务器测试和构建代码。然后，应用程序的 Docker 映像被创建并被推送到 Docker 注册表。

这一切都是从推送过程开始的，这违背了 GitOps 的核心原则之一。在 GitOps 中，出于安全考虑，所有更改都是通过拉模型完成的。基于拉的方法是优选的，并且被认为是安全的，因为没有外部客户端可以访问集群；所有来自内部的更新都是可信的。不建议使用基于推送的方法，因为集群凭据位于构建系统内部。

## **结论:让我们把这些 GitOps 的误解放在一边**

GitOps 仍然是新的和不断发展的。它有可能扰乱云原生空间。如果你计划使用 Kubernetes，并且有一个可靠的软件交付目标，那么确保在你的工作流程中有 GitOps 而不是*只是*来自动化基础设施和应用部署。

开发人员和企业都喜欢 GitOps 的附加优势，例如增强的协作、简单的工作流程、改进的稳定性、合规性和弹性，可以自动化您的基础架构和部署。

我们希望上面列出的 GitOps 误解能够在您的 GitOps 之旅中为您指引正确的方向。别忘了今天就注册免费的安全带。