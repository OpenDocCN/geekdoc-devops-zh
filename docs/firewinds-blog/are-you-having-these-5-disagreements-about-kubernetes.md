# 你对 Kubernetes 有这 5 个不同意见吗？你应该害怕！

> 原文：<https://www.fairwinds.com/blog/are-you-having-these-5-disagreements-about-kubernetes>

 随着我们迈向 2023 年，这是一个思考如何使用技术堆栈的好时机，尤其是 Kubernetes。许多组织在 2020 年加入了 Kubernetes，促使全球疫情更多地采用云。如果你是他们中的一员，在将更多的应用程序迁移到云上之前，你可能已经从一个试验项目开始了，并且你可能已经不再回头了。现在事情已经稍微稳定下来，这是一个更仔细地思考你如何使用 Kubernetes 的好时机，并对你在匆忙中可能错过的这五个分歧(或讨论)进行一些思考。

## 1.让您的 CEO 访问您的 Kubernetes 环境

如果你有一个人的组织，或者如果你是自由职业者，并且是一个 Kubernetes 顾问，你可能应该能够访问你的 Kubernetes 环境。在几乎所有其他情况下，这可能是个坏主意。

尤其是在小型创业公司中，权限可能是一个非常敏感的话题。您可能已经开始与公司中的每个人一起构建应用程序或服务，并部署到 Kubernetes。但从长远来看，这是不可持续的。也许你的首席执行官是前首席技术官，喜欢插手这个游戏，但随着公司的发展，这真的不太理想。作为首席执行官，你不需要插手基础设施的每一个细节，包括 Kubernetes。许多 CTO 也不需要这种级别的访问权限。

另一方面，您的开发人员确实需要了解他们正在部署的环境以及它是如何工作的。您的开发团队对 Kubernetes 的理解越好，他们在编写部署到该环境中的代码时就会做得越好。这将帮助他们部署更稳定的应用程序。它还可以帮助您的团队将 Kubernetes 安全性整合到开发过程中，并将其进一步左移，使您的应用程序更加安全。由于您的开发人员了解应用实际运行的环境，这将改善他们的编码方式，并支持一种 [Kubernetes 服务所有权](https://fairwinds.medium.com/how-the-fair-winds-of-better-kubernetes-security-will-blow-you-safely-home-ca4de3104294) 方法。就谁应该——谁不应该——访问您的 Kubernetes 环境(以及为什么)进行深思熟虑的讨论，是在您的组织中提高 [Kubernetes 成熟度](https://www.fairwinds.com/blog/introduction-kubernetes-maturity-model) 的重要一步。

## 2.将所有服务部署到您的 Kubernetes 环境中

一些组织采取的方法是只使用内部[](https://www.fairwinds.com/blog/make-your-kubernetes-policies-stick-use-an-effective-enforcement-plan)构建的服务和工具，然后他们可能希望将所有这些东西放入 Kubernetes。这在一些大型组织中可能行得通，比如网飞或谷歌，他们的工作产生了很多开源工具，这对开源社区来说非常好。然而，一般来说，使用最适合这项工作的工具是最有意义的。如果服务或工具不是你的核心竞争力，不是让你的公司赚钱的东西，你可能不应该花很多时间自己构建、维护和运行它。那会分散你的注意力，而你应该做的是让你的组织更加成功。

已经有很多解决方案来运行您的数据库、队列和电子邮件服务。他们有经验丰富的大型团队作为后盾。你可能应该使用那些专门构建的服务，因为它们有效。例如，在 Kubernetes 集群中运行的 MySQL 和 Amazon 使用[【RDS】](https://aws.amazon.com/rds/)运行关系数据库的方式有很大的不同，因为这是该团队专注于做好的事情——快速、轻松地在云中设置、操作和扩展关系数据库。如果您发现自己正在解决新的问题，这样您就可以构建它，以便它更好地为您试图构建的东西工作，那么将该服务部署到您的 Kubernetes 环境中可能是合适的。对于绝大多数公司来说，尤其是中小型公司，这很少有意义。

## 3.让您的开发人员负责*所有事情* —一直到生产

当你雇佣一个新的开发者时，你要做的第一件事不是让他们访问 ***所有的*** ，希望如此。让开发人员对他们的应用程序、工作负载服务等拥有[](https://www.fairwinds.com/cloud-native-service-ownership)所有权肯定有好处。实际上，您的开发人员最了解需要如何配置，代码如何工作，以及部署后它们将如何工作。但是为了让开发人员更有效率，你还需要额外的工具来帮助他们完成这个过程。

众所周知，Kubernetes 可能是一个复杂的环境，它需要大量的培训来帮助开发人员快速高效地使用它，或者您需要使用工具来帮助他们。工具可以将 [正确的护栏放置在](https://www.fairwinds.com/kubernetes-guardrails-explained-reg) 的位置，以便开发人员在将服务部署到 Kubernetes 时能够遵循 [的最佳实践。](https://www.fairwinds.com/kubernetes-best-practices-comprehensive-white-paper)

如果你雇佣一个什么都能做的工程师——他们知道 Kubernetes、observability、CI/CD、前端和后端开发以及数据库，并让他们拥有从生产到销售的所有东西——那将是相当不寻常的。但是，如果你有一个人，每个人都去找他们，作为唯一知道如何解决(所有)问题的人，他们将很难做好每一件事。相反，如果你 [让你的开发人员](https://www.fairwinds.com/blog/5-ways-to-make-your-dev-teams-love-you) 部署并拥有他们自己的服务，直到生产，他们可以负责他们真正理解的所有部分，而其他人可以负责以合理和负责的方式帮助他们做到这一点。

## 4.让您的 CTO 保留对各个容器中 SSH 的访问

如果你让你的 CTO [SSH](https://en.wikipedia.org/wiki/Secure_Shell) 进入生产中的单个容器，请停止。在生产环境中，应该没有人能够执行容器。Kubernetes 的目标是创建不可变的基础设施，其中组件被替换而不是改变。如果你是:

*   编写发行版或从头开始的安全容器
*   部署不臃肿的容器
*   在您的集群中使用良好的工具来实现可观察性
*   将您的日志运送到某个地方

那么您根本不需要 SSH 到生产容器中。

请记住，Kubernetes 并不是让一台机器永远健康地运行。在 Kubernetes 中，你可以使用 [Kube control](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/) 登录或杀死一个 pod 并旋转另一个 pod。你没有理由担心每个单独的容器。这是 Kubernetes 促成的范式转变的一个非常重要的部分。在过去，太多的故障排除过程都涉及到将一台机器 [关闭再打开](https://www.youtube.com/watch?v=nn2FB1P_Mn8) 。嗯，Kubernetes 会自动为您做这些，以保持事情顺利运行。在一些组织中，由于组织的规模以及您自己独特的部署和环境，您可能需要能够 SSH，这就是为什么这是一个重要的讨论或争论。然而，对于大多数组织来说，您不需要 CTO 来保持对单个容器的 SSH 访问。

## 5.忘记您的云成本

传统观点认为，你不需要担心你的云成本，声称它总是比拥有自己的基础设施便宜。你可能处于这样一个位置:你的销售非常好，你的应用和服务根据需要进行扩展，并且你的云成本随着你的收入一起上升是可以接受的。但是，了解您的云支出成本仍然很重要。

了解您的足迹的单位成本(无论是按集群、按客户、按收入还是其他衡量标准)非常重要。虽然很难达到这样的粒度级别，尤其是在共享的多租户环境中，但是您需要知道单位成本何时增加，以及增加的速度是否合适。

云的魅力在于它很容易增加新的工作负载，但获得可见性却不容易。您能判断您是否有流氓工作负载吗？有人黑进去启动 [bitminer](https://www.bankrate.com/investing/what-is-bitcoin-mining/) 了吗？获得一个可以显示您的 Kubernetes 集群、工作负载、名称空间等的成本的工具，对于帮助您了解您的支出以及您如何 [在 Kubernetes](https://www.fairwinds.com/kubernetes-cost-optimization) 中支出(并帮助您调整集群规模)至关重要。

## 解决 Kubernetes 挑战

无论您是 Kubernetes 新手还是已经使用了很长时间，您都知道这是一个提供了很多灵活性和可伸缩性的复杂软件。尽管如此，还是很容易忘记停下来想想你是如何设置的，或者谁可以访问什么。您应该进行我在这篇文章中概述的讨论，因为 Kubernetes 的使用和部署很少是非黑即白的。几乎任何事情都没有正确或错误的答案。因为它与您的 Kubernetes 环境有关，无论这项技术有多热门，它被广泛采用，每个人都在使用 Kubernetes，请记住，并不是 Kubernetes 中的所有东西总是一个好主意。

观看网上研讨会 [关于 Kubernetes 你应该有的 5 个分歧以及如何解决](https://webinars.containerjournal.com/5-disagreements-you-should-be-having-about-kubernetes-and-how-to-solve-them) 欣赏比尔·莱丁汉姆(CEO)、肯德尔·米勒(技术布道者)、伊莉莎·赫伯特(工程运营副总裁)和安迪·苏德曼(首席技术官)之间有趣的讨论。