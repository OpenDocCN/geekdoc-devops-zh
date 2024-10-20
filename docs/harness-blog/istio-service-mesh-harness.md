# 如何维护 Istio 维修网与线束|线束

> 原文：<https://www.harness.io/blog/istio-service-mesh-harness>

是时候开始运送您的新尖牙 Istio 服务网了。利用马具增强您的体验！不要害怕尝试。

正如我们在[第一部分](https://harness.io/blog/istio-service-mesh-deployment/)和[第二部分](https://harness.io/blog/continuous-delivery/what-is-a-service-mesh/)中了解到的，服务网格技术有助于引入最新一代的工作负载，但也给之前没有直接处理网络复杂性的团队带来了负担。就像任何改变我们如何构建弹性服务的范式的新软件类别一样，令人生畏的运营需求的复杂性可能会使采用变得黯然失色。不要害怕朋友，马具是来帮忙的。

## 服务网格采用备忘单

在创新和保持光明之间平衡收音机刻度盘是许多从业者在技术世界中经历的平衡。在一个例子或快乐之路中利用[服务网](https://harness.io/blog/continuous-delivery/what-is-a-service-mesh/)是一个很好的学习练习。尽管当我们跨越鸿沟进入实际工作负载时，复杂性肯定会出现。在 [Istio](https://istio.io/) 一个极其强大的模式是[交通转移](https://istio.io/docs/tasks/traffic-management/traffic-shifting/)。能够将百分比/权重应用于服务，例如流量分流模式中的 v1 和 v2 服务，为金丝雀部署打开了大门。如果我们回忆起我们的 Kubernetes 系列，尝试在没有任何帮助的情况下部署 Kubernetes Canary,确实需要几个手动步骤。通过利用线束[软件交付平台](https://harness.io/platform/)，这些复杂性变得更加简单。

## 带线束的增压器

我们最有可能触及流量转换规则的时间是在部署期间。编排一组 [KubeCTL](https://kubernetes.io/docs/reference/kubectl/overview/) 和 [IstioCTL](https://istio.io/docs/reference/commands/istioctl/) 命令，维护配置，以及为失败进行设计，例如为那些任务进行回滚，当然需要适当的计划和思考。在我们的[流量管理](https://developer.harness.io/docs/first-gen/continuous-delivery/kubernetes-deployments/set-up-kubernetes-traffic-splitting/)支持下，Harness 平台允许您从编排和故障复杂性中解脱出来，专注于规则和结果本身。喜欢总是可以看视频或关注博客帖子。

首先，我们需要确保在 Kubernetes 集群上安装了 Istio。可以参考第一部分关于如何安装一个[快速 Istio 安装](https://istio.io/docs/setup/install/kubernetes/)。第二件事是我们有一些图片，我们从 Harness 部署到 Kubernetes 上。在下面的例子中，我们将使用 [Nginx](https://nginx.org/en/) 前往等待 [Minikube](https://github.com/kubernetes/minikube) 的目的地。假设 Kubernetes 集群正确连接到了 Harness，我们可以将[流量分割步骤](https://developer.harness.io/docs/first-gen/continuous-delivery/kubernetes-deployments/set-up-kubernetes-traffic-splitting/)添加到 Harness 工作流中。如果这是您的第一个[线束工作流程](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/workflows/workflow-configuration/)，不用担心，我们会在这里创建一个新的。作为基础，我创建了一个名为“My K8s”的[线束应用程序](https://docs.harness.io/article/bucothemly-application-configuration)，以及一个名为“nginx_k8s”的[线束服务](https://developer.harness.io/docs/category/add-services)，它只是 nginx 通过 [Docker Hub 拉过来的。](https://hub.docker.com/)

导航至设置->您的应用->工作流，然后“+添加工作流”。我们将把我们的新工作流程称为“Istio Canary”。

接下来，我们可以将部署阶段添加到我们的工作流中。我正在利用一个预先存在的“nginx_k8s”服务，我们将把它部署到 Minikube。

我们可以使用“+ Add Phase”将流量拆分添加到验证阶段，然后添加到 Kubernetes 列下。

可以设置流量分割的权重。可能取决于需要多少 Canary 交互，可以有多个拆分。在这里，我利用 20/80 的分裂。

让我们回到“nginx_k8s”的线束服务定义。Harness 可以用作 UI 来填充需要发送进行配置的项目。在下面的 manifest 部分，让我们添加另一个名为 istio.yaml 的文件，该文件包含一个基本的 Istio [目的地规则](https://istio.io/docs/reference/config/networking/v1alpha3/destination-rule/#DestinationRule)和[虚拟服务](https://istio.io/docs/reference/config/networking/v1alpha3/virtual-service/)。Harness 能够代表您应用 YAMLs，我们将利用这一点来实现“DestinationRule”和“VirtualService”。要做到这一点，请将鼠标悬停在文件夹上，然后用圆点选择“+添加文件”并添加一个 istio.yaml。

在 [istio.yaml](https://github.com/ravilach/harness/blob/master/istio.yaml) 中，我们为 istio 目的地规则和 Istio 虚拟服务添加[基本配置](https://developer.harness.io/docs/first-gen/continuous-delivery/kubernetes-deployments/set-up-kubernetes-traffic-splitting/)。我们将有[工作流变量](https://developer.harness.io/docs/first-gen/continuous-delivery/kubernetes-deployments/workflow-variables-expressions/)，因此 Harness 可以注入所需的信息。

[Sample istio.yaml](https://github.com/ravilach/harness/blob/master/istio.yaml) 点击保存，一切就绪。最后，可以运行您新创建的管道，并看到您的流量分裂的行动。持续部署- >开始新的部署

点击提交并观看魔术！

我们的 20/80 分成。

## 马具是你最好的朋友！

现在你在 Istio 和服务网格世界是危险的，不要害怕参加服务网格 dip。线束[连续交付平台](https://harness.io/platform/continuous-delivery/)的灵活性和稳健性可以帮助您和您的团队以更快的速度采用更新的技术。Harness 是您的合作伙伴，将新技术作为您的[持续交付渠道](https://harness.io/blog/ci-cd-pipeline//)的一部分来运营。干杯！拉维