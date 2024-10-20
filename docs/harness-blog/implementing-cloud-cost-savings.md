# 实施您的首个云成本节约|利用

> 原文：<https://www.harness.io/blog/implementing-cloud-cost-savings>

想知道云成本管理建议告诉了你什么吗？了解如何实施云成本节约。

利用[云成本管理](https://harness.io/products/cloud-cost)的主要目标是允许将云成本信息从财务团队转移到工程团队，并实现其民主化。现在，作为一名工程师，您可以看到选择对您的云支出的影响。开始使用[云成本优化](https://harness.io/blog/cloud-cost-optimization/)非常简单，尤其是您可以试用云成本管理，看看是否有证据。

一旦云成本管理启动并运行，并且[建议](https://harness.io/blog/cloud-cost-management/introducing-cloud-cost-recommendations/)开始出现，您如何执行这些建议？如果您正在考虑试用云成本管理，或者在您的组织中使用该产品开始您的旅程，Harness [连续交付](https://harness.io/products/continuous-delivery)可以为您执行这些更改。

## 部署示例 Kubernetes 应用程序

您将从云成本管理监控中获得最大收益，并就实际应用程序/基础架构的发现提出建议。如果您还没有准备好，可以使用一个[示例应用程序](https://hub.docker.com/r/rlachhman/amazingapp)。这个样例应用程序与我们在讲授我们的[持续集成](https://github.com/ravilach/firstdrone)示例的最终形式——Docker 映像时使用的是同一个应用程序。

假设您已经完成了入门示例，下一步是部署应用程序。一种简单的部署方法是利用在同一个 UI 中的 Harness 连续交付。Harness Continuous Delivery 有一个[抽象模型](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#sort=relevancy),可以非常快速地建立和运行。

首先导航到设置，点击+添加应用程序。给个“贵 App”之类的名字。

接下来要配置的是要部署到的[环境](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#environments)。一个很好的例子就是您在云成本管理示例中利用的 Kubernetes 集群[例如，您部署了您的 Harness 委托的地方]。

设置->昂贵的应用->环境+添加环境。

创建后，您需要通过单击+Add Infrastructure Definition 创建一个基础架构定义来定义环境。云提供商类型为“Kubernetes 集群”，部署类型为“Kubernetes”。在浏览入门示例时，您已经部署了一个 Harness Kubernetes 委托，并设置了一个[云提供商](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#cloud_providers)。您可以选择已经提供的基础设施来选择您想要部署到的 Kubernetes 集群[例如 Harness Cloud Provider]。

当基础设施定义设置完成时，配置[服务](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#services)，这是您将部署的应用程序。

设置->昂贵的应用->服务+添加服务。部署类型为“Kubernetes”。

第一步是通过点击+Add Artifact Source 来定义工件源。线束预先与公共 Docker 集线器连接。[示例应用程序(如 Amazing App](https://hub.docker.com/r/rlachhman/amazingapp) )位于一个公共 Docker Hub 存储库中，可以使用 Docker 映像名 rlachhman/amazingapp 连接。

默认情况下，没有为 Amazing 应用程序定义容器资源规范。例如，您可以给出一个适中到较大的数量，您认为 [Kubernetes 资源限制和请求](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/)应该是多少。您可以通过点击 edit 并在 *Spec，Containers* 下添加容器资源约束来修改 deployment.yaml。

资源:
限制:
CPU:“1.5”
内存:“7Gi”
请求:
CPU:“1.5”
内存:“7Gi”

一旦你点击保存，就可以创建一个简单的[工作流程](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#workflows)来让这个神奇的应用进入野外。

设置->昂贵的应用->工作流+添加工作流。工作流类型是“滚动部署”，环境、服务和基础设施定义是在前面的步骤中创建的。

点击提交后，您就可以开始部署了。只需点击工作流程右上角的“部署”按钮，即可开始部署。选择您想要部署的构建/版本[Docker Tag],然后点击 Submit 进行部署。

在左侧导航到持续部署->部署，您可以看到您的部署。

应用程序退出后，在接下来的 24 小时内高枕无忧。如果这是一次全新的尝试，云成本管理将开始汇总和分析成本数据。

## 推荐信准备好了

大约一天后，您的云成本管理仪表板将会充满信息。在左侧导航到云成本管理，点击浏览器。

似乎我们高估了令人惊叹的应用程序所需的火力；作为这款令人惊叹的应用的作者，它应该拥有它所能得到的一切，对吗？

单击右上角的建议，我们可以看到我们过度配置的程度以及潜在的成本节约。

单击建议，我们可以看到适当的资源限制应该是多少，这比我们最初的预期要少得多。

单击“Workload”链接，我们可以看到实际资源与请求资源的历史数据，两者相差很大。

有了这些数据，我们所要做的就是根据资源变化重新部署应用程序。

## 节省开支

有了云成本管理数据，让我们继续进行这些资源更改。

设置->昂贵的应用->服务->“大得惊人的应用”。在 deployment.yaml 中，通过单击 Edit 重新修改资源限制和请求以匹配建议。

资源:
限制:
CPU:“25m”
内存:“250M”
请求:
CPU:“25m”
内存:“250m”

然后让 Harness 重新部署应用程序。要显示另一种部署方式，请导航至左侧导航，依次导航至连续部署和部署。单击“Start New Deployment”并选择您为示例创建的工作流。

点击提交，你现在就可以省钱了。

由于资源限制更低，您的集群可以自由扩展和/或放置更多工作负载。

## 现在就利用线束节省资金和资源

上面的例子可能比您生产中的应用程序更简单一些。节省资源甚至是敏捷性的关键。在创建这个例子的时候，对我来说一个意想不到的结果是在资源限制上咄咄逼人。如果您耗尽了 Kubernetes 集群，这可能会影响您部署另一个滚动部署。Kubernetes 调度程序要么需要调整，因为存在执行滚动部署所需的过剩[[max surge](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)，如果所有资源都被占用，甚至一个副本的激增也可能无法实现，这出乎意料地发生在我身上。

Harness 将在您的云计算之旅中与您合作，而这一旅程中的一个重要步骤/实现就是解决云计算成本问题。按需基础架构和云原生架构的优势支持下一代敏捷性。随着敏捷和容易的旋转项目，成本肯定会攀升。报名参加[云成本管理试用](https://harness.io/products/cloud-cost)并从今天开始普及云成本！