# 什么是带舵图的 Argo CD？装具

> 原文：<https://www.harness.io/blog/what-is-argo-cd-helm-chart>

在本文中，我们将介绍如何使用带有舵图的 Argo CD 实现 GitOps。

应用程序部署的 GitOps 方法已经变得非常流行，因为它简单易用，并且为开发人员消除了许多瓶颈。使用开源项目 [Argo CD](https://github.com/argoproj/argo-cd) 和 [Helm Charts](https://helm.sh/) 来管理 Kubernetes 应用程序使 GitOps 变得简单。

在本文中，我们将介绍如何使用带有舵图的 [Argo CD](https://argo-cd.readthedocs.io/en/stable/) 实现 GitOps。使用这种方法，我们将展示如何部署一个简单的应用程序和舵图。

## 阿尔戈 CD 和舵图

GitOps 作为一种现代的[连续交付](https://harness.io/products/continuous-delivery) (CD)模型，在 Git 和部署应用程序的目标环境(例如 Kubernetes)之间使用 GitOps 代理。GitOps 还跟踪 Git 中发生的所有变化。

如果有任何更改，GitOps 代理会提取更改以匹配预期的配置。代理比较实际状态和期望状态。例如，当开发人员将 yaml 文件中的 pod 编号从 2 更改为 4 时，代理会比较这两种状态，当发现这种更改时，它会提取这些更改以使这两种状态(实际状态和期望状态)相同。当谈到一个经过深思熟虑的 GitOps 方法时，Argo CD 和 Kubernetes 是很好的一对。

当涉及到 GitOps 时，将 Argo CD 和舵图一起使用效果非常好。您可以使用 Helm 轻松地在 Argo CD 上部署应用程序。它们都是强大的云原生组合，帮助企业交付软件。Kubernetes 已经成为一个事实上的容器编排工具，每个组织都想采用它来加速应用程序的部署。但是，光靠 Kubernetes 是无法有所作为的；您应该采用类似 GitOps 的云原生方法，以确保您处于正确的道路上。

借助 Argo CD 和 Helm 等工具，您可以获得快速部署软件的好处。虽然 Argo CD 是一个真正的 GitOps 工具，但 Helm 是用来部署 Kubernetes 应用程序的。在您的软件开发生命周期中拥有这两者，您可以创造奇迹。

## 什么是 Argo CD？

Argo CD 是一个声明性的、基于 GitOps 的持续交付工具，可以帮助您从 Git 部署应用程序，管理它们的生命周期，并保持您的环境同步。通过[选择 Argo CD](https://harness.io/blog/software-deployment-tools) ，您可以让您的部署保持最新，并确保您的应用程序始终遵循所需的状态。Argo CD 允许您将应用程序及其环境定义为代码，然后在任何 [Kubernetes](https://kubernetes.io/) 集群上部署和更新它们。

## 什么是舵轮图？

虽然 Kubernetes 已经成为现代云原生企业事实上的容器编排工具，但学习曲线很高，因为它对一些人来说是一个复杂的工具。在 Kubernetes 上配置和部署应用程序的过程可能是一项艰巨的任务。幸运的是，有了 Helm 和 Helm Charts，现在可以自动化部署过程，并在 Kubernetes 上快速部署和管理应用程序。

Helm 是一个帮助您管理 Kubernetes 应用程序的开源工具。Helm Charts 是预先配置的 Kubernetes 资源包，可以快速轻松地部署。在 Helm 和 Helm Charts 的帮助下，开发人员可以简化他们的 Kubernetes 部署过程，并快速启动和运行他们的应用程序。

## 阿尔戈 CD 对赫尔姆

Argo CD 和 Helm 都是用于管理和部署云原生应用程序的流行开源工具，但它们有一些主要差异。让我们来看看 Argo CD 和 Helm 的三个主要区别。

### 1.应用程序部署

Argo CD 和 Helm 的第一个区别是它们部署应用程序的方式。Argo CD 是一个 CD 工具/平台，可用于将应用程序部署到多个环境中。Helm 是一个包管理器，可用于将应用程序部署到单个环境中。因此，Argo CD 更适合更复杂的部署，而 Helm 更适合简单的部署。

### 2.应用管理

第二个区别是他们管理应用程序的方式。Argo CD 是一个基于 GitOps 的工具，这意味着它使用 Git 作为真实的单一来源，并将其作为存储和跟踪应用程序配置和清单的焦点。这使得观察更加容易，如果需要，您可以回滚到以前的版本。Helm 将 Helm 图表视为管理和部署应用程序的焦点，并且必须手动跟踪更改。

### 3.缩放应用

第三个区别是他们如何扩展应用程序。Argo CD 设计为水平扩展，可以轻松部署到多个集群。使用 Helm，您只能部署到单个集群。Argo CD 非常适合大规模部署，Helm 非常适合小规模部署。

## 如何将 Argo CD 与舵图一起使用

现在我们已经看了 Argo CD 和舵，让我们探索如何使用 Argo CD 和舵图:

1.  第一步是启动并运行 Kubernetes 集群
2.  创建一个命名空间来存储所有 Argo CD 相关的部署。
3.  在本地计算机上安装 Argo CD CLI，以便与 Argo CD 实例进行交互并管理您的应用程序
4.  将 Argo CD 和 manifests 部署到您的 Kubernetes 集群上，并通过您喜欢的方法访问 Argo CD UI(我们将在教程中使用端口转发方法)
5.  使用用户名和密码从用户界面登录到 Argo CD(我们将在下面的教程中展示)
6.  下一步是在本地机器上安装 Helm 命令行界面(CLI)。这将允许你与你的舵图互动。
7.  一旦安装了 Helm CLI，您就可以连接到 Argo CD 实例，并将其配置为使用 Helm charts。
8.  最后，您可以使用 Argo CD 通过舵图部署您的应用程序。这将允许您快速轻松地管理和部署您的应用程序。

### 辅导的

对于本教程，唯一的先决条件是能够访问 Kubernetes 集群。您还可以使用 Minikube 或 Kind 来获得单节点集群。

首先，创建一个名称空间:

###### kubectl 创建名称空间 argocd

将清单文件应用于创建的命名空间:

###### ku bectl apply-n argocd-f https://raw。githubusercontent。com/Argo proj/Argo-CD/stable/manifests/install。YAML

确保所有的 pod 都在名称空间内正常运行:

###### kubectl get pods -n argocd

现在，让我们通过端口转发来公开我们的 ArgoCD 服务器 UI:

###### kubectl 端口转发 SVC/argocd-server-n argocd 8080:443

一旦你这样做了，你现在可以在 [https://localhost:8080/](https://localhost:8080/) 上看到 ArgoCD UI

现在，您需要用户名和密码登录。默认情况下，用户名为“admin ”,密码可以使用以下命令生成:

###### ku bectl-n argocd get secret argocd-initial-admin-secret-o JSON path = " { . data . password } " | base64-d；回声

您将得到一个输出，用作您的密码。一旦您有了用户名(admin)和密码，请登录 ArgoCD。

此外，确保您可以通过 CLI 在本地使用 Argo CD，因此安装 Argo CD CLI:

###### brew 安装 argocd

现在，您可以通过 CLI 登录 Argo CD 并与之对话:

###### argocd 登录本地主机:8080

它会要求您输入用户名和密码。设置两者，登录将会成功。

## 通过 Helm 将应用程序部署到 Argo CD

参考 [Argo CD 示例 repo](https://github.com/argoproj/argocd-example-apps) 通过 Helm 部署留言簿应用程序。

让我们在 ArgoCD 上创建一个应用程序。

###### argocd app 创建 helm-guest book-repo https://github.com/argoproj/argocd-example-apps.git-path helm-guest book-dest-server https://kubernetes . default . SVC-dest-namespace default

您应该会看到应用程序被创建:

###### 应用程序“helm-guestbook”已创建

让我们检查创建的应用程序的状态:

###### argocd 应用程序获取头盔-留言簿

‍

‍

您可以看到服务和部署都处于不同步状态。让我们使用以下命令同步应用程序:

###### argocd 应用程序同步舵-留言簿

您现在可以看到我们服务的健康状态是健康的，部署正在进行中。部署将需要一些时间，并变得健康。

您可以通过进入 UI 来验证 Argo CD 上创建的应用程序。

这里可以看到状态是 ***健康*** 和 ***同步。***

通过进行端口转发，您可以轻松地访问您部署的应用程序:

###### kubectl 左舷-前进 SVC/helm-留言簿 9090:80

让我们通过 [http://localhost:9090/](http://localhost:9090/) 访问我们的应用程序。

## 使用 Argo CD 部署舵图

让我们使用阿尔戈光盘部署舵图。

转到 Argo CD UI 中的设置并添加存储库。

添加以下详细信息并连接。

您应该会在列表中看到您添加的存储库。

现在，点击创建一个新的应用程序。我们将在 Argo 光盘上部署 NGINX 舵图。

请确保指定所需的详细信息。

一旦你点击创建，你应该能够在 Argo CD 仪表板上看到你的应用程序。

首先，应用程序会不同步。一旦我们同步了应用程序，它就会更新并显示为健康。

点击“同步”，然后点击“同步”

现在，如果你回去看看，你的应用程序应该显示健康和同步的状态。

### 使用线束 GitOps

Harness 提供了本机 GitOps 功能，允许您通过将源存储库中的 Kubernetes 清单与目标集群同步来部署服务。首先，通过在您的环境中安装 GitOps 代理来设置 Harness GitOps。接下来，在 Harness 中定义如何管理 GitOps 应用程序中的期望状态和目标状态。GitOps 代理执行应用程序中定义的同步操作，并对源和目标状态中的事件做出反应。

‍

使用您的一个 Kubernetes 集群来尝试这个[利用 GitOps](https://docs.harness.io/article/pptv7t53i9-harness-cd-git-ops-quickstart) 。

首先，你需要[注册一个免费的线束账户](https://app.harness.io/auth/#/signup/?module=cd&?utm_source=website&utm_medium=harness-developer-hub&utm_campaign=cd-plg&utm_content=get-started)并导航到 GitOps 选项卡。

从侧面菜单导航到部署> GitOps。

Harness 将代表您安装 Argo CD，并将 Argo CD 实例连接到 Harness。你只需要一个 Kubernetes 集群。现在，我们需要设置所需的东西来完成 GitOps，作为 Harness 的一部分。

导航至 GitOps >设置> GitOps 代理。

您将一个接一个地连接所有需要的 GitOps 设置，如下所示。

一旦连接并验证了所有东西，您就可以开始通过 Harness GitOps 部署应用程序了。以著名的留言簿为例，一旦部署完成，您应该会看到如下所示的 Harness GitOps 仪表板:

你可以跟随这个简单的 [Harness GitOps 教程](https://developer.harness.io/tutorials/deploy-services/first-gitops-example?utm_source=internal&utm_medium=social&utm_campaign=community&utm_content=pavan-gitops-article&utm_term=gitops)来看看通过 [Harness GitOps](https://docs.harness.io/article/w1vg9l1j7q-harness-git-ops-basics?utm_source=internal&utm_medium=social&utm_campaign=community&utm_content=pavans-gitops-article&utm_term=gitops) 部署应用程序是多么直观和简单。

### 让我们做 GitOps

GitOps 是一种现代软件交付方法，有着光明的未来。如果您正在处理与 Kubernetes 相关的部署，那么 GitOps 有许多好处。GitOps 专注于丰富的开发人员体验，可加快您的软件部署速度。结合 DevOps 最佳实践，GitOps 为您的软件交付插上翅膀。

寻找一个 GitOps 持续交付工具？[立即申请线束 CD 的演示](https://harness.io/demo/gitops)&GitOps-as-a-Service！