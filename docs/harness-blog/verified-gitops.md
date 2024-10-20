# 关于经验证的 GitOps | Harness，您需要了解的一切

> 原文：<https://www.harness.io/blog/verified-gitops>

GitOps:一个好的开始，但我们如何才能让它变得更好？输入经过验证的 GitOps。GitOps 中的验证可能是一项挑战，请向 Harness 学习！

这是经过验证的 GitOps 的完整概述。我们将从概述 [GitOps](https://harness.io/blog/what-is-gitops/) 的优点和缺点开始。接下来，我们将更深入地了解 GitOps 实际上是什么样子。最后，我们将回顾一下什么是经过验证的 GitOps 以及它在实践中的样子。

## GitOps 概述

[集装箱化](https://www.webopedia.com/TERM/C/containerization.html)！[云原生](https://en.wikipedia.org/wiki/Cloud_native_computing)！ [CI/CD](https://harness.io/blog/what-is-ci-cd/) ！其他营销流行语！

令人惊讶的是，大多数技术概念是如何开始和发展的，从工程师开始，最终成为最佳实践，导致营销和销售术语，然后管理层在他们的团队中实施最佳实践，然后导致其他技术概念来解决存在的问题或差距。这是一个恶性循环，它定义了 OOP、函数式编程、云计算、虚拟化以及其他许多东西是如何流行起来的。

一个技术概念在被正式命名之前就已经被实践了。很长一段时间以来，使用 git 在源代码管理解决方案中保留某种东西的编码版本的想法一直是应用程序开发的一部分。

最终，用户开始利用声明性语言在代码中表示基础设施，以便使供应基础设施更加可伸缩、可重复和可靠。这导致了术语进入市场，当 Weaveworks 创建了一个工具，它允许您利用用于[Kubernetes](https://kubernetes.io/)([Kubernetes Specs](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/)、 [Helm](https://helm.sh/) 、 [Kustomize](https://kustomize.io/) 、 [KSonnet](https://ksonnet.io/) )的声明性应用程序代码，并确保代码中的内容在集群中被准确地表示。

这为 [SDLC](https://en.wikipedia.org/wiki/Systems_development_life_cycle) 流程带来了一些主要好处，特别是允许用户在[灾难恢复](https://en.wikipedia.org/wiki/Disaster_recovery)场景中快速启动新集群。

## GitOps 故障

让我们快速分解一下 GitOps 目前的样子:

整个过程从工程师对他们的应用或基础设施规范代码进行更改开始，然后通过提交( [Pull 请求](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests))同步到他们的 git repo，然后，根据 GitOps Sync 的方法，要么是 [webhook](https://en.wikipedia.org/wiki/Webhook) 推送更改，要么是代理/ [操作员](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/)将更改拉送到应用或基础设施。这使得 git repo 成为事实的来源，而应用程序/基础设施是 git repo 的反映。

## GitOps 的优势

GitOps 有一些显著的优势，尤其是在之前的基础上:

### 利用开发人员本地工具

因为开发人员已经将 [Git](https://git-scm.com/) 用于他们的应用程序代码，甚至可能是基础设施代码，允许他们在 git repo 中处理部署规范使得培训和使用变得容易。

### 自动恢复

因为基础设施和应用程序规范代码存储在 git repo 中，所以如果一个集群宕机，一切都可以很快恢复，因为它已经存在于代码中。

### 凭证管理

由于工程师已经被要求[将](https://en.wikipedia.org/wiki/Authentication)认证到他们的 git repo 中，该认证和[授权](https://en.wikipedia.org/wiki/Authorization)也可以用于部署。

### Git 责备和版本控制

如果应用程序配置和基础设施的声明性代码存储在 git 中，那么您就拥有了代码的原生版本，这允许根据需要进行恢复，并且能够找出是谁进行了更改。

### 更容易的知识转移

由于 git 的所有内容都存在于一个目录结构[中，所以很容易看出这些内容与其他应用程序或文件的关系。](https://www.siteground.com/tutorials/git/directory-structure/)

## GitOps 中的问题

### 自动部署

在 git repo 更新时自动部署到基础设施或应用程序对于任何高于开发的环境来说都不是一个好的过程。

对此的解决方案是在每次回购之间制造一个[空气间隙](https://en.wikipedia.org/wiki/Air_gap_(networking))，并在批准后手动将变更推上回购链。

### 违约

默认使用 Helm/Kustomize/Kubernetes 规范，让这些文件决定部署什么和如何部署，几乎不可能添加暂停、批准、测试等内容。因为没有办法在本地表示这些需求。

对此的解决方案是建立前面提到的空隙模型，它允许您通过实现手动暂停从一个环境部署转移到另一个环境部署。

### 工具使用

当您开始设置 git repo 以最好地与 GitOps 流程集成时，除了直观地了解正在部署的内容及其当前状态之外，使用 GitOps 工具几乎毫无意义。

额外的工具可能代表 GitOps 的垂直定义。

### 技术理解

需要对应用程序、基础架构和体系结构的高级技术理解，以确保 git repo 为 GitOps 流程做好准备，这使任何不精通底层技术的人都感到陌生。

唯一的解决办法是引进更多拥有先进知识的人，并在内部积累部落知识。

### 仅 Kubernetes

当前面向 GitOps 的工具只支持 Kubernetes。

如果公司正在使用其他工具(无服务器、其他容器编排工具、传统应用程序等)。)然后他们需要设置多个工具和流程。

### 自动回滚

自动回滚会导致配置漂移。

如果正在使用的解决方案提供自动回滚，那么与 git repo 相比，基础架构或应用程序中的配置将是以前的版本。

潜在的解决方法是不进行自动回滚，而是在出现问题时发送通知，要求最终用户恢复 git repo 中的更改，然后同步更改。

## 高级别 Gitops 摘要

能够编写基础设施和应用程序规范非常有用。不需要部落知识就能拥有可重复配置的能力不仅对 GitOps 有帮助，这也是像 Kubernetes 这样的技术如此受欢迎和被大量使用的主要原因之一！

新员工的加入也变得很容易，因为配置代码是声明性的，易于阅读，允许那些有权访问 Git repo 的人很容易看到环境的预期外观。

然而，即使有这些好处，GitOps 也有明显的缺点，这使得它很难在企业范围内正确实现。

## 在实践中验证 GitOps

GitOps 进程旨在当用户对存储在 [git repo](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository) 中的配置代码进行更改时启动，然后在 git repo 和环境之间执行[自动同步](https://en.wikipedia.org/wiki/Synchronization_(computer_science))。这使得 git repo 成为最终[环境](https://en.wikipedia.org/wiki/Deployment_environment)中应该是什么的真实来源。

## 实践中 GitOps 的开始

此时，围绕 GitOps 的一切都是假设的，但实际情况是怎样的呢？

要开始您的 GitOps 进程，有三个先决条件:

### Kubernetes 载货清单

首先，您需要一个工作版本 Kubernetes 清单。

*虚拟代码中的库斯托米泽*

### 簇

一个已经设置好的[集群](https://kubernetes.io/docs/reference/glossary/?all=true#term-cluster)或[基础设施即代码](https://en.wikipedia.org/wiki/Infrastructure_as_code)来设置一个经过测试和批准的集群。

*在* [*Minikube*](https://kubernetes.io/docs/tutorials/hello-minikube/) 上的集群设置

### 解决办法

使您能够将 git repo 与集群同步的解决方案。

*vs code*[*RunOnSave*](https://github.com/pucelle/vscode-run-on-save)*扩展*

GitOps 的想法是让 git repo 成为事实的来源，并在文件被提交到存储库时自动同步(自动部署)到端点(集群)。在这个例子中，保存文件[模拟](https://www.merriam-webster.com/dictionary/emulate)将文件提交给 repo，然后[触发](https://developer.harness.io/docs/category/triggers)自动同步(您可以在 git repo 中为此运行类似于 [GitHub 动作](https://github.com/steebchen/kubectl))。

*原* [*草切*](https://kustomize.io/) *文件*

*更改文件并保存*

*kustomize 文件与集群的自动同步*

*自动同步的结果*

这是 GitOps 的一个相当基本的设置，其中的要点是当文件被更改时自动同步到集群。GitOps 的另一部分是一个[设备](https://en.wikipedia.org/wiki/Computer_appliance)，它检查集群中的内容和 git repo 中的内容，以确保两者相等。

这通常被视为一个[操作符](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/)，它在集群中的内容和 git 中的内容之间进行内部审计。也可以有一个外部解决方案来完成与操作员相同的过程。

这里没有显示的是 GitOps 的基础设施即代码过程，但是很少有甚至没有实现这一功能的解决方案，尤其是在考虑完整的上下文时。

就基础设施即代码而言，任何解决方案所能达到的最大程度都是基础设施配置的改变(即[服务](https://kubernetes.io/docs/concepts/services-networking/service/)、[配置图](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/)、[秘密](https://kubernetes.io/docs/concepts/configuration/secret/)、[入口](https://kubernetes.io/docs/concepts/services-networking/ingress/)等)。).

另一个选择是将预先批准和测试的基础设施代码存储在 git repo 中，这将允许更容易地完成灾难恢复过程，或者仅仅是 T2 扩展基础设施的愿望。

## GitOps 在实践中的优势

GitOps 的一些好处在上一篇博文中已经讨论过了。这个列表将具体说明从实际演练中可以看到的好处。

其中一个好处是，通过自动同步触发部署的能力是一个非常简单而强大的过程。通过使用 git repo，工程师们将会有一个更熟悉的过程，而 GitOps 将会是这个过程的扩展。

另一个好处是大多数团队已经有了批准的过程，[代码检查](https://developer.github.com/v3/checks/)，[特性分支](https://www.atlassian.com/git/tutorials/comparing-workflows/feature-branch-workflow)等等。GitOps 也可以利用这一点。

本演练的最后一个好处是，用户可以通过了解他们放在 git repo 中的内容在集群中是活动的而获得信心。然而，这就是缺点开始发挥作用的地方。

## GitOps 在实践中的缺陷

GitOps 最明显的缺点之一是 git repo 是真实的来源。因此，如果在 git repo 中发生了配置错误，部署仍然会进行，但是错误可能是毁灭性的:

*错误配置的映像导致集群中的部署问题*

想象一下，如果错误配置是部署的名称或副本[计数？整个集群都可能被摧毁。更糟糕的是，当实时流量影响集群时，解决方案是什么？](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)

考虑到集群中其他相关资源的所有潜在问题，此时您不能[回滚](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/#rolling-back-a-deployment)。更好的办法是建立一个全新的集群，把有问题的那个赶走。

如果 YAML 看起来是正确的，但是 Kubernetes 没有识别出来，那会怎么样呢？你可以有一个应该发生的部署，但是它实际上并没有通过，然后在没有[反馈循环](https://itrevolution.com/the-three-ways-principles-underpinning-devops/)的情况下会有失败(Kubernetes 在提到[错误消息](https://learnk8s.io/troubleshooting-deployments)时并不是最好的)。

另一个主要问题是回滚过程。回滚的真相来源是什么？是 git 回购吗？集群？一个人？例如:

*部署的名称现已更改*。

新部署以错误的名称显示，我们希望回滚。代码恢复为旧名称，并再次保存，结果如下:

*恢复部署名称*

*新部署生效，旧部署仍在*

如果用户认为回滚实际上会删除部署及其资源，那么当他们实际查看集群时，就会猛然惊醒。

另一个突出的问题是，每一步都与部署过程相关([测试](https://en.wikipedia.org/wiki/Software_testing)、[批准](https://support.atlassian.com/jira-service-desk-cloud/docs/set-up-an-approval-stage-for-a-request/)、[变更请求](https://en.wikipedia.org/wiki/Change_request)等)。)不能成为这个自动化过程的一部分(你不能在没有得到[超级黑客](https://kubernetes.io/docs/concepts/workloads/controllers/jobs-run-to-completion/)的情况下向你的 Kubernetes 清单添加测试部分)。

因此，GitOps 现在要求那些负责部署过程的人仅仅作为部署者来利用 GitOps 过程，实质上消除了登录集群并自己运行 [*kubectl*](https://kubernetes.io/docs/reference/kubectl/overview/) 命令的需求。然后，随着部署过程的每个额外部分进入范围，一切又变得非常手动。

GitOps 的最后也是最重要的缺点是先决条件。集群必须[锁定](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)，以便通过 git repo 集中所有潜在变更，并避免[配置漂移](https://dzone.com/articles/configuration-drift)对流程产生负面影响。

此外，要利用 GitOps，一切都必须已经存在。那些想要实现 GitOps 的人需要有一个全功能的 git repo，经过全面测试的 Kubernetes 清单，经过良好调整的 Kubernetes 集群。

作为 Kubernetes 专家的一个或一组用户，在 GitOps 范围之外对 Kubernetes 流程进行故障排除和维护(您无法通过使用 GitOps 流程来真正测试新的部署管道是如何工作的。

您必须首先在外部测试它们，然后在通过审查后将它们导入到流程中)。

## 实践中的 GitOps 总结

GitOps 有一些非常好的做法，应该坚持；将应用程序、基础设施和应用程序配置的编码版本存储在 git repo 中，以便进行协作和版本控制。

然而，git repo 作为 Kubernetes 部署的真实来源，对于那些希望拥有跨生产和整个企业的可靠流程的人来说，并不是最佳场景。

经验证的 gitop 可能是每个公司的最佳流程，解决方案的前景使该公司能够真正利用经验证的 gitop。

## 超越吉托普

当我们最初讨论 GitOps 时，我们开始讨论不同的概念如何从基层开始，然后发展到最初的概念被遗忘，营销团队使用这些术语来增加他们网站的流量。我们想尝试打破循环，在草根过程之外开创一个概念，把最初的目的和概念拿到世界里，才可以淡化！

众所周知，世界上并不是每家公司都只使用 Kubernetes，这意味着当涉及到非 Kubernetes 产品时，只使用 Kubernetes 的解决方案并没有多大帮助。

此外，当 git repo 更新时，没有公司愿意让其生产部署[完全自动化。相反，每个人都有某种形式的](https://www.freecodecamp.org/news/what-i-learned-during-production-deployment-fe037a6ee4db/)[测试、检查、批准、验证等](https://en.wikipedia.org/wiki/Software_deployment#Deployment_activities)，这是流程的一部分。

而且理由很充分！你能想象有人将[复制品](https://en.wikipedia.org/wiki/Kubernetes#Replica_Sets)的数量从 2 个增加到 20 个，然后它被自动合并和部署吗？

经验证的 GitOps 实践想要做的是[整理](https://en.wikipedia.org/wiki/Declarative_programming)流程的每一部分和交互。在 CI/CD 的情况下，不仅仅是代码本身存在于 git repo 中，还有[获取代码到 repo](https://en.wikipedia.org/wiki/Commit_(version_control)) 的过程、[代码检查](https://en.wikipedia.org/wiki/Static_program_analysis)将发生在 git repo 上、批准、测试、工件的[构建、标签和](https://en.wikipedia.org/wiki/Build_automation)[变更管理](https://en.wikipedia.org/wiki/Change_management)等等。

## 分解已验证的 GitOps

让我们快速分析一下经过验证的 GitOps 是什么样的:

这似乎更像是典型的过程，对吗？这是因为 Verified GitOps 的目标不是引入一个简单的部署工具，而是从头到尾创建一个编码的、可重复的过程，这允许可伸缩性和安全性，因为一切都应该是自动化的(或接近自动化)。

## 经验证的 GitOps 的更多优势

经验证的 GitOps 有一些明显的优势:

1.  利用流程中每个人都已经熟悉的工具。
2.  这一过程不仅仅延伸到 Kubernetes 和部署
3.  过程的每个部分都考虑到了简单的可伸缩性和可靠性
4.  安全性现在可以对整个过程有更好的控制、输入和可审计性

然而，经过验证的 GitOps 有一些不太明显的优势:

1.  每个部分都可以放在 [RBAC](https://en.wikipedia.org/wiki/Role-based_access_control) 下
2.  [新特性、功能和人员的入职](https://en.wikipedia.org/wiki/Onboarding)变得极其容易
3.  如果实施正确，增加/删除/交换部分流程不会对流程的其余部分产生任何明显的连锁反应
4.  [固执己见](https://medium.com/@stueccles/the-rise-of-opinionated-software-ca1ba0140d5b)和[可达成一致](https://dl.acm.org/doi/pdf/10.1145/1506216.1506221)可以根据公司希望如何实施该流程来协调

## 验证 GitOps 的更多问题

尽管这个过程听起来很棒，但也有一些缺点:

1.  不是每个人都能马上实施的
2.  工程中有多少不同的过程或过程的变体？
3.  有多少流程将被强制执行，允许多少变化？
4.  谁来维护流程？
5.  目前[市场上没有工具](https://harness.io/blog/cost-of-implementing-ci-cd/)可以完成完全验证的 GitOps。那你是做什么的？
6.  你组合工具吗？
7.  你购买和建造吗？
8.  你只是建造吗？

## 经过验证的 GitOps 设计

既然我们已经讨论了经验证的 gitop 的一些好处和问题，在实现经验证的 gitop 之前，有一些重要的决定:配置将如何设计？

配置设计的第一个主要决策是构建设计的语言风格。一种声明性的语言风格是最好的选择，但是应该使用哪种标记语言呢？YAML ？ [JSON](https://en.wikipedia.org/wiki/JSON) ？ [XML](https://en.wikipedia.org/wiki/XML) ？

根据组织中当前标记语言的使用情况做出决定是很重要的，但也要根据公司不可避免的发展方向做出决定。不要选择 XML，因为在过去的 10 年里，公司的每个工程师都生活在那里。

相反，选择一些既可读又容易组合的东西(对你来说仍然可能是 XML)。请记住，无论做什么选择，都会有培训要求；这包括间歇用户、日常用户、高级用户和流程维护人员。

## 验证 GitOps 的方法

下一个重要的决定是你是采用单一文件方法，文件引用方法，还是混合方法。

### 单线方法

*   顾名思义，这种方法将整个过程的所有声明性语言组合在一个长文件中(因此称为 monofile)。如果你回头看上面的图表，想象每一个步骤、变量、配置需求等等。按顺序排列在一个文件中。
*   利益
*   阅读整个过程并理解正在发生的事情是非常容易的
*   新员工入职很简单，因为他们只需通读文件，就能了解什么会发生，什么不会发生
*   不需要额外的编排文件
*   问题
*   它可以变得很长，很快。根据验证的 GitOps 过程由多少部分组成，您可能需要解析数百行声明性语言。
*   任何需要支持的变化都会导致另一个单丝，不管这个变化是大是小。

### 文件引用方法

*   这与单一文件方法相反，在单一文件方法中，流程的每个部分都包含在自己的文件中，然后其他文件将引用这些文件。
*   好处:
*   需要非常小的配置文件
*   易于阅读和理解
*   通过制作单个文件的副本，可以轻松处理流程中不同部分的更改和变化，而不会对流程的其余部分产生重大影响
*   问题:
*   可能会变得非常复杂，需要检查多个文件才能了解应该如何处理信息流
*   需要流程编排文件来拼凑流程的不同部分
*   如果要求人们浏览文件结构，他们很难加入

### 混合工艺

*   这种方法是文件参考和单文件方法的结合。这种方法的实际设计取决于最终用户，但建议大多数是单丝。此外，对于将被引用的片段，单丝中引用的主要部分将是需要被覆盖的变量(如果有的话)。
*   好处:
*   比文件引用方法需要解析的文件更少
*   不需要任何编排文件
*   小的改变和变化不如单丝方法有影响力
*   问题:
*   仍然有一些文件引用会导致点击以获取上下文的兔子洞
*   主文件可能会变得很长，这取决于混合方法的实现方式
*   需要一个用户界面来理解这种设置。它不像单线电影那样易读。

## 已验证的 GitOps 摘要

对 CI/CD 流程的每个部分进行编码的能力对于适应未来的业务至关重要。不需要部落知识的可重复和可扩展的配置不仅有用，这也是 Kubernetes 或 [Terraform](https://www.terraform.io/) 等技术如此受欢迎和大量使用的主要原因之一！

与 GitOps 相比，Verified GitOps 不仅仅局限于 Kubernetes，它让工程团队能够真正理解他们当前流程的所有错综复杂之处。尽管存在上述缺点，但对于任何拥有 CI/CD 业务的公司来说，Verified GitOps 都是最佳选择。