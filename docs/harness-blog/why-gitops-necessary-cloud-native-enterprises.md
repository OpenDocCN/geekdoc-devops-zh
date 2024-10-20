# 为什么 GitOps 对于云原生企业是必要的|利用

> 原文：<https://www.harness.io/blog/why-gitops-necessary-cloud-native-enterprises>

让我们讨论一下 GitOps 及其优势，然后将 Argo 与 Kubernetes 集群集成，以实现改变。

GitOps 是一种使用 Git 部署和管理软件应用程序的方法。它也被称为“代码操作”或“代码驱动的操作”，它使用 DevOps 的原则来简化整个组织的软件更新。它基本上使用了一个涉及 Git 和 Kubernetes 的协作软件开发模型。

您还可以将 GitOps 定义为 Git 和 Ops 的组合，这是一种使用操作原理来管理版本控制系统的实践。当我们谈论版本控制时，它主要是关于存储库中的代码。GitOps 通过在您的版本控制系统上应用操作原则而更进一步。

## 什么是 GitOps？

术语“GitOps”最初是由 Weaveworks 的工程师发明和推广的，并作为一组最佳的云原生实践与来自 Weaveworks 的工具一起呈现给 DevOps 世界，以帮助开发人员通过 Git 操作复杂的 Kubernetes 工作流。

GitOps 是部署软件的云原生实践，其中 Git 被用作系统中发生的任何部署资源的单一事实来源。每当任何新部署推出时，开发人员都需要通过 Git 定义一切。通过这种方法，自动化部署变得更加容易，并有助于提高可观察性。这提高了 DevOps 团队内部的部署信心，并提高了整体开发人员的工作效率。

GitOps 要求通过 Git 声明性地定义资源，这样您就可以轻松地维护资源的状态。它鼓励在最少或没有人工干预的情况下自动化部署，并在发生意外情况时快速回滚到之前的状态。

GitOps 使开发人员能够将基础设施代码推入环境存储库中，并注意到一个变化。GitOps 对软件和基础设施的环境进行必要的更改，然后将其进一步移入 CI/CD 管道。

GitOps 的工作有四个基本原则:

1.  *以 YAML 的形式，以声明的形式定义整个系统。*
2.  *使用 Git 作为事实的单一来源，并在 Git 中对规范的期望系统/环境状态进行版本控制。*
3.  *自动批准对所需状态的更改。*
4.  *采用基础设施作为代码(IaC)并确保正确性。*

### 简单的 GitOps 工作流程

GitOps 的基本工作原理是让 Git 成为真理的源泉，包括把一切都移到代码中，把一切都存储和维护在 Git 中。说到部署，利用操作符以声明的方式部署 Git 和 Yaml 中配置的内容。由于所有开发人员对 Git 都很友好，GitOps 简化了他们复杂的工作流程。

所以当涉及到 Kubernetes 时，应用程序代码、容器图像和所有相关的清单文件都将存储在 Git 中，任何更改都是通过 Git 作为单一的真实来源进行的。

## 练习 GitOps 的方法

GitOps 可以有两种部署策略:**推送和拉取管道**。它们之间的区别在于我们确保部署环境与所需基础架构相匹配的方式。

### 推动管道战略

CI/CD 工具在这里起着至关重要的作用，许多人使用这种策略，将源代码和部署清单文件存储在一个存储库中。每当有新的更新发生时，就会触发构建管道。管道创建容器映像并将最近的更改推送到环境中。

### 拉动式管道战略

容器映像和声明性配置(以 YAML 格式编写)更改从集群内运行的 CD 引擎中的集群内部被拉入集群。

## GitOps 的优势

*   通过采用 GitOps 方法，DevOps 团队可以轻松处理灾难恢复，并在发生任何灾难性事件时顺利管理灾难恢复。
*   通过 Git 强大的加密和正确性来跟踪集群中的任何变化，从而提高可观察性。
*   对任何了解 Git 的人来说都是透明和直接的，因为它利用声明性配置来描述每个过程。
*   GitOps 尽可能轻松地实现连续和频繁的部署，而无需管理一堆工具，因为 Git 中发生的一切都是一个真实的来源。
*   复杂的 Kubernetes 升级、部署和特性可以通过 GitOps 更有效地管理。
*   因为每一个动作都会通过 Git 在 GitOps 中被追踪，所以可观察性将会很容易。
*   GitOps 通过在整个工程团队中标准化 GitOps 工作流程来提高生产率。

## DevOps vs. GitOps

开发运维工作流包括:

*   基于规定的模型
*   与帮助云相关-本机原则
*   依赖 CI/CD 渠道作为推动创新和自动化的主要工具
*   灵活。做 DevOps 有很多种方法，途径也很开放。

相比之下，GitOps 工作流:

*   宣言的
*   重点关注云原生和微服务应用原则
*   Git 是创新和自动化的主要工具
*   严格。遵循一个特定的程序，并具有一定程度的正确性，或者什么是或不是 GitOps

**DevOps 管道和 GitOps 管道**

## 带绳索和挽具的陀螺

GitOps 是一种云原生方法，其中整个软件交付过程都以 Git 的使用为中心。在本教程中，我们将看到将 Argo 与 Kubernetes 集群集成并以自动化方式交付更改是多么容易。

**先决条件:**

**教程:**

我们将使用谷歌云创建一个集群。

首先，我们需要创建一个 Kubernetes 集群来托管我们的 ArgoCD

接下来，我们需要创建 ArgoCD 名称空间，并将所有清单文件安装到我们创建的集群上。

###### kubectl 创建名称空间 argocd

下载 Argo CD CLI

###### brew 安装 argocd

是时候访问 Argo CD API 服务器了。我们可以通过使用以下命令将其作为服务类型的负载平衡器来实现这一点。

###### kubectl 修补程序 SVC Argo CD-server-n Argo CD-p ' { " spec ":{ " type ":" load balancer " } } '

返回到您的集群和“服务和入口”选项卡，查看外部负载平衡器。

打开链接，您应该能够访问 ArgoCD UI。

现在，您需要一个用户名和密码。默认用户名是“admin”，密码可以使用下面的命令生成。

###### ku bectl-n argocd get secret argocd-initial-admin-secret-o JSON path = " { . data . password } " | base64-d；回声

一旦您提供了登录凭证，您应该会看到您的 ArgoCD 设置。

## 玩 ArgoCD GitOps:派生 Git Repo

让我们来看一个回购的例子，比如这里找到的这个:[https://github.com/argoproj/argocd-example-apps](https://github.com/argoproj/argocd-example-apps)

一旦你叉样本回购，去你的 Argo 光盘设置，并创建一个新的应用程序。添加相关细节。

一旦应用程序被创建，它在 ArgoCD UI 中显示为不同步，为了变成同步，我们需要点击“同步”按钮，应用程序显示为健康。

现在，只要回到你的分叉例子回购和改变一些东西。假设您将 guestbook-ui-deployment.yaml 副本从 1 更改为 2。接下来会发生什么？应用程序不同步，你需要再次点击“同步”按钮，你应该看到所有东西都通过拉策略自动更新。

您可以通过转到您的集群并查看新的更改来验证这些更改。

您现在可以看到两个副本正在运行。

## 带吊带的 GitOps

首先，你需要[注册一个免费的线束账户](https://app.harness.io/auth/#/signup/)并导航到 GitOps 选项卡。

从侧面菜单导航到部署> GitOps。

Harness 将代表您安装 Argo CD，并将 Argo CD 实例连接到 Harness。你只需要一个 Kubernetes 集群。现在，我们需要设置所需的东西来完成 GitOps，作为 Harness 的一部分。

导航至 GitOps >设置> GitOps 代理。

您将一个接一个地连接所有需要的 GitOps 设置，如下所示。

一旦连接并验证了所有东西，您就可以开始通过 Harness GitOps 部署应用程序了。以著名的留言簿为例，一旦部署完成，您应该会看到如下所示的 Harness GitOps 仪表板。

你可以跟随这个简单的 [Harness GitOps 教程](https://developer.harness.io/tutorials/deploy-services/helm-argocd-gitops-k8s/)来看看通过 [Harness GitOps](https://docs.harness.io/article/w1vg9l1j7q-harness-git-ops-basics) 部署应用程序是多么直观和简单。

## GitOps 和复杂的应用程序部署

我们刚刚看到了如何使用 GitOps 方法将更改部署到我们的应用程序中。GitOps 已经在云原生空间获得了发展势头，开发人员喜欢这种方法，因为它易于设置。此外，由于 Git 是使用的主要工具，学习曲线显著缩短。自动化是快速可靠的软件交付的关键，GitOps 方法在这里派上了用场。

虽然 GitOps 可能不是云原生一切的答案，但我确信这种方法将很快在软件交付领域占据显著地位。GitOps 通过使用每个开发人员都知道的简单工具 Git，强调单点真理方法。与 Kubernetes 等云原生工具一起使用，GitOps 可以成为实现真正数字化转型的绝佳途径。GitOps 将继续存在，它的未来是光明灿烂的。实现一个合适的 DevOps 过程需要很多努力。幸运的是，GitOps 是来帮忙的。

想了解更多关于 GitOps 如何加速应用部署的信息吗？[今天和我们聊聊](https://harness.io/demo/next-gen-cd)！