# 同时部署到阿拉斯加、EKS 和 GKE |线束

> 原文：<https://www.harness.io/blog/deploying-to-aks-eks-gke>

随着基础架构复杂性的增加，您的部署复杂性也会增加。在这个例子中，学习如何轻松地跨多个 Kubernetes 提供者进行部署。

Kubernetes 被看作是你构建下一代平台的平台。Kubernetes 并非没有运营复杂性，这就是为什么许多组织希望公共云供应商在其上运行他们的 Kubernetes 工作负载。Azure Kubernetes 服务( [AKS)](https://azure.microsoft.com/en-us/overview/kubernetes-on-azure/) 、Amazon Elastic Kubernetes 服务([【EKS】](https://azure.microsoft.com/en-us/overview/kubernetes-on-azure/))和 Google Kubernetes 引擎([【GKE】](https://cloud.google.com/kubernetes-engine)在过去几年中得到了大幅增长和采用。

当组织设计分布式系统时，不要把所有的鸡蛋放在一个篮子里是一个设计考虑。利用多个公共云供应商是一种流行的方法。与非常具体的云服务不同，Kubernetes 确实超越了公共云供应商；您在一个集群上的工作负载应该能够在另一个集群上工作。随着基础架构复杂性的增加，您的部署复杂性也会增加。在这个例子中，学习如何轻松地跨多个 Kubernetes 提供者进行部署。

像往常一样，关注博客和/或观看视频。

## 设置您的实例

为了利用这个例子，我们需要在三个公共云供应商中创建 Kubernetes 集群。对我自己来说，我对 AWS 最熟悉，在我以前的角色中使用 AWS 最多。在 Azure 和 GCP 旋转资源对我来说是第一次。如果你还没有的话，第一步就是在 AWS 、 [Azure](https://portal.azure.com/#home) 和 [GCP](https://cloud.google.com/cloud-console) 中设置账户。

## 亚马逊 EKS 供应

我最喜欢的供应 EKS 集群的工具是 [EKSCTL](https://eksctl.io/) 。如果你使用的是 Mac，安装 EKSCTL 最简单的方法就是利用[自制软件](https://formulae.brew.sh/formula/eksctl)。运行以下命令将启动 EKS 集群。

#Create EKS 集群 eksctl Create cluster \-name captain-canary-eks \-version 1.18 \-region us-east-2 \-node group-name standard-workers \-node-type T3 . xlarge \-nodes 2 \-nodes-min 1 \-nodes-max 3 \-node-ami auto

配置完成后，运行 kubectl get nodes 来验证连接性和集群状态。

没有了 EKS，让我们转向 AKS。

## Azure AKS 供应

前往 Azure 门户网站并登录。可以导航到+创建资源，然后 Kubernetes 服务。[微软文档](https://docs.microsoft.com/en-us/azure/aks/kubernetes-walkthrough-portal)有一个很棒的入门指南，可以让你的第一个 AKS 集群启动并运行。

通过配置向导运行后，您的 AKS 集群就启动并运行了。

为了完善这三家公共云供应商，最后可以构建一个 GKE 集群。

## 谷歌 GKE 供应

在你的浏览器上多一个标签，进入[谷歌云控制台](https://console.cloud.google.com/)，然后登录。导航到 Kubernetes 引擎，然后+创建集群。如果这是你的第一个 GKE 集群，[谷歌文档](https://cloud.google.com/kubernetes-engine/docs/quickstart)有一个很棒的入门指南。

配置完成后，您的 GKE 集群就可以正常运行了。

随着集群的出现，下一步是开始利用强大的功能。

## 线束准备

让我们给海因斯打个电报，让他代表我们行事。我们将利用[管理代表](https://docs.harness.io/article/igftn7rrtg-delegate-installation-overview)，例如管理部署在每个集群内部的工作节点。一个好的分布式系统设计考虑是利用不同的云提供商的代表来实现可用性。根据每个云提供商的要求，通过管理资源交互的 IAM/服务主体/云 IAM 角色，可以将一个 Harness 委托部署到其他 Kubernetes 集群。

首先，导航至[线束网络用户界面](https://app.harness.io/)并登录。如果你没有马具账号，你可以[免费注册一个](https://harness.io/try-continuous-delivery-as-a-service-for-free/)。

## 线束委托安装

我们将依次在 EKS、阿克苏和 GKE 安装线束代理，因为我的机器是通过 CLI 连接到 EKS 的，而不是阿克苏或 GKE。不过，如果您已经为其他云提供商之一连接了 CLI，那么您可以按照任何顺序安装 Harness 委托。以下说明显示了如何从每个云提供商重新注入 [kubeconfig](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) ，这样您就可以在本地机器上使用终端。

## EKS 代表装置

安装线束代理非常简单，导航到设置，然后安装代理。选择 Kubernetes YAML，并可以为该代表命名，如“eks-delegate”。

下载完成后，展开 tar.gz。

在展开的文件夹中，有一个 readme 文件，其中包含安装 Harness Delegate 的命令，该文件是 ku bectl apply-f Harness-Delegate . YAML。

如果您首先安装了另一个云提供商 CLI，您可以随时重新注入 EKS 集群 kube config by AWS eks-region your-region update-kube config-name cluster-name，例如 AWS eks-region us-east-2 update-kube config-name captain-canary-eks。

过了一会儿，Harness 委托将被连接到 Harness 中，并可以在 UI 中进行验证。

随着第一个利用委托的方式，我们可以重复一个类似的过程为 AK 和 GKE。

## AKS 委托安装

我们需要 kubectl 访问 AKS 集群。最简单的方法是让 Azure CLI 注入 kubeconfig 上下文。如果您尚未安装，请安装 Azure CLI。

注入 kubeconfig 的命令是:

az aks get-credentials-resource-group my resource group-name myaks cluster # Azure 命令 brew update & & brew install Azure-cliaz loginaz aks get-credentials-resource-group ravi group-name captain-canary-Azure kubectl get 节点

一旦 kubectl 连接好，就可以从 Setup -> Install Delegate 返回并重新下载一个线束代理。选择 Kubernetes YAML，并可以给出一个名称“aks-delegate”。

使用 ku bectl apply-f harness-delegate . YAML .*展开 tar.gz 并安装到您的 AKS 集群中*

*稍后验证现在有两个线束代理存在。*

*下一步是对 GKE 集群执行类似的流程。*

## *GKE 代表装置*

*按照 EKS 和 AKS 的类似流程，让 Google Cloud CLI 注入 kubeconfig 将是启动和运行 kubectl 访问的最简单方法。如果你还没有安装 Google Cloud CLI，最简单的方法就是[使用自制](https://formulae.brew.sh/cask/google-cloud-sdk)。如果在我写这篇博客的时候你有一个 IPV6 ISP，你可能需要在谷歌云 SDK 安装期间[禁用 IPV6](https://stackoverflow.com/questions/44087310/google-cloud-sdk-install-on-os-x-gcloud-components-list-failed-to-fetch-compo) 。*

*Google Cloud 给你 gcloud CLI [命令来执行](https://cloud.google.com/kubernetes-engine/docs/how-to/cluster-access-for-kubectl#using-gcloud-init)来更新 UI 中的 kubeconfig。可以导航到谷歌云控制台，点击 GKE 集群上的“连接”来执行命令。*

*谷歌云命令注入 kubeconfig:*

*g cloud container clusters get-credentials cluster-name-zone your-zone-project your-project # GCP 命令 networksetup -setv6off Wi-Fibrew 木桶安装 Google-cloud-SDK _ PYTHON = " $(brew-prefix)/opt/PYTHON @ 3.8/lib exec/bin/PYTHON " source " $(brew-prefix)/cas kroom/Google-cloud-SDK/latest/Google-cloud-SDK/path . bash . Inc " source " $(brew-prefix)/cas kroom/Google-cloud-SDK/PYTHON*

*一旦 kubectl 连接好，返回并从 Setup -> Install Delegate 重新下载一个线束代理。选择 Kubernetes YAML，并可以给出一个名称“gke-delegate”。*

*使用*ku bectl apply-f harness-delegate . YAML .*扩展 tar.gz 并安装到您的 GKE 集群中*

*安装最后一个线束委托后，您可以验证所有三个委托都在线束 UI 中。*

*困难的事情解决了之后，是时候添加要部署的 Kubernetes 集群，并创建一个要部署到所有三个集群的集群工作流了。*

## *线束 Kubernetes 布线*

*Harness 有一个 [CD 抽象模型](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts)的概念，其中资源和步骤被抽象掉。Kubernetes 集群属于[云提供商](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#cloud_providers)抽象范畴，需要建立三个云提供商，每个集群一个。由于 Harness Kubernetes 代理可以从它们被部署到的地方继承细节，因此将 Kubernetes 集群连接到 Harness 很容易。*

*导航至设置->云提供商+添加云提供商。选择 Kubernetes 作为类型。对于 EKS 群集，可以显示名称为“eks-cluster ”,并且可以从“eks-delegate”继承详细信息。*

*单击测试进行验证，然后单击下一步提交。*

*添加后，EKS 集群将显示在列表中。*

*AKS 和 GKE 集群也有类似的流程。*

*添加 AKS 集群:*

*添加 GKE 集群:*

*添加两者后，AKS 和 GKE 集群都将作为云提供商提供服务。*

*Kubernetes 连接完成后，最后，我们需要创建一个工作流来部署所有的优点。*

## *驾驭工作流程*

*如果您熟悉 Harness，那么任何部署的命脉都是一个 [Harness 应用程序](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#applications)，它包含了您的部署所需的所有抽象。创建应用程序，可以导航到设置+添加应用程序。可以给出“大库伯内特”这个名字。*

*接下来，我们将所有三个 Kubernetes 环境与一个[线束环境](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#environments)连接在一起。*

*设置->大 Kubernetes ->环境+添加环境。给它起名叫“杂交库伯内特”。*

*接下来，我们可以添加[基础设施定义](https://docs.harness.io/article/v3l3wqovbe-infrastructure-definitions)，每个集群一个。要设置 EKS，可以将云提供商的名称“EKS”命名为“Kubernetes 集群”，部署类型为“Kubernetes”。确保将云提供商与在上述步骤中连接的“eks-cluster”相匹配。*

*冲洗并重复 AKS 和 GKE 集群。*

*下一步是定义[线束服务](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#services)，例如您将要部署什么。您可以通过进入设置- > Grand Kubernetes - >服务+添加服务来创建服务。让我们用部署类型“Kubernetes”来部署 Nginx。*

*创建好服务后，在 Nginx 图像的位置进行连接。公共码头枢纽工程良好。*

*点击+Add Artifact Source，利用默认的源服务器[Harness Docker Hub]和“library/nginx”作为镜像。*

*单击 submit 后，Nginx 就可以部署了。*

*接下来，我们将创建三个[利用工作流](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#workflows)或步骤，每个步骤对应一个 Kubernetes 环境。*

*设置->大 Kubernetes ->工作流+添加工作流。将工作流命名为“部署 EKS”，工作流类型为“滚动”，环境为“混合 Kubernetes”，服务为“Nginx”，基础架构定义为“EKS”。*

*点击提交后，工作流将被保存。让我们冲洗一下，并为 AKS 和 GKE 集群重复一遍。*

*导航回设置->大 Kubernetes ->工作流程，并添加另外两个工作流程。*

*AKS 工作流程:*

*GKE 工作流程:*

*点击提交后，您应该会看到所有三个工作流。*

*为了将这些工作流缝合在一起，我们可以创建一个[线束管道](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#pipelines)来顺序执行这些工作流；线束管道有几个执行模型，但为了简单起见，我们将利用顺序模型。*

*设置-> Grande Kubernetes ->管道+添加管道*

*我们可以为每个 Kubernetes 环境/提供商添加一个管道阶段，以展示管道的威力。点击“添加管道阶段”。*

*定义一个步骤名为“EKS”的执行步骤[取消选中自动生成名称]，并将执行工作流设置为“部署 EKS”。*

*点击提交后，对 AK 和 GKE 重复这个过程。*

*AK:*

*GKE:*

*有了 Kubernetes 提供者，您应该有一个三级管道。*

*现在您已经准备好跨三个 Kubernetes 提供商进行部署了！*

## *观看魔术*

*有几个地方可以开始部署，例如直接从您创建的管道开始。虽然可以在左侧导航到持续部署->部署->开始新部署。选择管道，然后选择刚刚创建的应用程序和管道。选择一个 Nginx 标签[版本]。*

*点击提交并观看魔术！*

*就这样，您成功地部署了三个主要公共供应商的 Kubernetes 平台。*

## *带着马具继续旅程*

*在 EKS、阿拉斯加和 GKE 部署可能是一个老生常谈的例子。Harness 平台功能强大且易于使用，可帮助您和您的组织实现云现代化目标。欢迎[今天就报名参加线束平台](https://app.harness.io/auth/#/signup)。*

*干杯！*

*拉维*