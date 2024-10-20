# 如何:您的第一次 Istio 部署|利用

> 原文：<https://www.harness.io/blog/istio-service-mesh-deployment>

这艘船是什么服务网？你能装运所说的网或者用所说的网装运吗？在我们的博客文章中了解如何部署您的第一个 Istio 应用程序！

Istio 去年很受欢迎。我不能 100%确定 Istio 是什么，但我知道我需要两个 Istio；一个用于展示，一个在科技会议上亮相，比如 T2 的 CNCF kube con T3。

玩笑归玩笑，如果你对[服务网格](https://harness.io/blog/what-is-a-service-mesh/)的知识有所欠缺，不要担心。我们是来帮忙的。随着我们的应用程序变得越来越分布式，并采用更多的微服务架构，服务网格技术正在蓬勃发展。

## 不断变化的景观

不久以前，网络团队拥有您的应用程序和内部/外部世界之间的连接和路由。当我们有一些可预测的上下文要路由到时，网络团队可以有足够的时间来修改规则(还记得为防火墙规则提交一张传票或添加一个新的虚拟 IP 吗？).看看下面我在一个关于应用程序网络复杂性的演示中提出的对玉米卷的要求。

用[虚拟机](https://en.wikipedia.org/wiki/Virtual_machine) :
请求传统的旧日玉米饼

借助网络堆栈支持过去:

随着应用程序开始变得越来越分散，我们对网络团队的依赖也变得越来越长，因为端点的动态部分开始悄悄进入。

随着 [Kubernetes](https://kubernetes.io/) 、[无服务器](https://harness.io/blog/serverless-and-harness/)和公共云的出现，我们快速前进到了今天；资源总是在加速旋转和减速旋转。我们当然不可能为每个端点都制作一张票。路线和终点一直在变化，因为系统决定了我们需要多少火力(例如自动缩放)。

现代的玉米卷要求有[λ](https://aws.amazon.com/lambda/)和 [Redis](https://redis.io/) :

借助网络堆栈支持现代社会:

随着开发团队和协调者制造出比以往更多的端点，需要有一种技术来确保通信畅通，从而欢迎服务网格。

## Hello Mesh

随着不断变化的分布式体系结构将我们从整体结构中解放出来，服务网格技术诞生了。 [Solo.io](https://www.solo.io/service-mesh) ，服务网格编排的领导者，将服务网格定义为:“一个服务网格从应用网络(它应该如何相互对话)中抽象出应用的业务逻辑(服务做什么)”。让您的应用程序参与到服务网格中可以抽象出网络复杂性(尽管这种复杂性必须存在，请继续关注第二部分)。

就像任何流行的技术一样，在服务网格中也有选择。像 Kubernetes 一样，媒体数量最多的是 [Istio](https://istio.io/) 。不过值得一提的是 [LinkerD](https://linkerd.io/) 、[Hashi ' s consult](https://www.consul.io/)，甚至像 AWS 这样的公有云厂商也有 [AWS App Mesh](https://aws.amazon.com/app-mesh/) 。让我们浏览一下您的第一个 Istio 部署，看看到底是怎么回事。

## 你的第一次 Istio 部署

部署 Istio 和利用 Istio 的应用程序很容易，只需要几个部分。我们可以使用我们信任的朋友 [Minikube](https://kubernetes.io/docs/setup/learning-environment/minikube/) ，通过 Istio 的示例应用 [book info](https://istio.io/docs/examples/bookinfo/) 。阅读 Minikube 上的 [Istio 文档，我们确实需要给 Minikube 分配更多的资源。](https://istio.io/docs/setup/platform-setup/minikube/)

我们还将使用 Istio 的命令行实用程序 [istioctl](https://istio.io/docs/reference/commands/istioctl/) 。

像往常一样，你可以观看视频或浏览博客文章的其余部分。让我们完成第一次 Istio 部署吧！

抓取[一个 Istio 发行版](https://istio.io/docs/setup/)，在这个博客的时候最新的是 1.3.1:

【https://git . io/get atest | Instituto _ version = 1。3 .1 sh-

下载文件后，您可以按照提示将 istioctl 添加到您的路径中。或者可以将 CD 放入 istioctl 文件夹，从那里进行布线。

*cd istio-1.3.1
导出路径= $ PWD/bin:$ PATH*

让我们启动一个 Minikube 实例。如果你以前没有使用过 Minikube，你可以利用[家酿](https://brew.sh/)来安装/升级。

*酿造桶安装 minikube* 或*酿造桶更新 minikube*

安装 Minikube 后:
*Minikube start-memory = 8192-CPU = 4*

通过启动仪表板进行验证。

*minikube 仪表盘*

也用 istioctl 验证。

*istioctl 验证-安装*

通过 istioctl 验证后，我们可以安装 Istio 下载中包含的 Istio 演示。

*立方申请-f 安装/立方结构/Instituto 演示。YAML*

我们可以通过查看适当的服务是否已经启动来验证安装。

*kubectl 获取 svc -n istio-system*

现在 Istio 正在运行，为[自动边车注射](https://istio.io/docs/setup/additional-setup/sidecar-injection/#automatic-sidecar-injection)配置[默认名称空间](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)。越来越接近了！

*kubectl 标签名称空间默认 istio-injection=enabled*

现在安装图书信息应用程序。

ku bectl apply-f samples/book info/platform/kube/book info。YAML

让我们用一个 [Istio 网关](https://istio.io/docs/concepts/traffic-management/#gateways)让应用程序在 Kubernetes 集群之外可访问。

kube CTL apply-f samples/book info/networking/book info-gateway。YAML
kube CTL 获得网关

现在，我们需要设置[入口 IP 和端口](https://istio.io/docs/tasks/traffic-management/ingress/ingress-control/#determining-the-ingress-ip-and-ports)并设置网关 URL。

*export INGRESS _ HOST = $(minikube IP)*

export INGRESS _ PORT = $(ku bectl-n istio-system get service istio-Ingres gateway-o JSON path = ' { . spec . ports[？(@.name=="http2")]。节点端口} ')

*export SECURE _ INGRESS _ PORT = $(ku bectl-n istio-system get service istio-Ingres gateway-o JSON path = ' { . spec . ports[？(@.name=="https")]。节点端口}')*

*导出网关 _ URL = $入口 _ 主机:$入口 _ 端口*

最后，我们来获取网关 URL。这里我得到 192 . 168 . 99 . 100:31380
*printenv GATEWAY _ URL*

我们可以去<Gateway _ URL>/product page 看看应用。就这样，你在用 Istio 做饭！

Istio 的一个重要特性是允许 Istio 处理[目的地规则](https://istio.io/docs/concepts/traffic-management/#destination-rules)。book info 示例应用程序附带了几个方便的例子供我们学习。

kube CTL 应用-f 样本/图书信息/网络/目的地-规则-全部。YAML
kube CTL 获取目的地规则-o YAML

有了这个，你就有了一个 Istio 环境和应用程序可以玩了。像任何一个好的网民一样，我们做完了就清理一下吧。Istio 项目通过发布一个 cleanup.sh 使这变得简单。

*/istio-1。3 .1/samples/bookinfo/platform/kube/clean up。sh*

可以用 *minikube 删除*终止 minikube。

## 与线束啮合

我们希望您在学习如何进行第一次 Istio 部署的过程中获得了乐趣！服务网格继续流行。在 Harness，我们将为您提供帮助。服务网格正在使开发团队的网络规则民主化。将目标规则/流量拆分作为管道的一部分进行导入。

请继续关注第二部分，我们将讨论服务网格带来的挑战。我不是网络工程师，大部分挑战是将网络复杂性引入应用程序开发领域。

干杯！
-拉维