# 牧场主里约 vs 马具-拍摄我的旅程|马具

> 原文：<https://www.harness.io/blog/rancher-rio-vs-harness>

牧场主 Rio 是由 T2 牧场主 T3 开发的微型 PaaS T1。两年前在[被引入野外](https://www.slideshare.net/cyberblackvoom/whats-rio-112779732)，牧场主 Rio 正在被开发，以帮助在 [Kubernetes 平台](https://kubernetes.io/)中消除复杂性。Kubernetes 作为一个平台有一个我称之为可插入的观点，如果你不喜欢关于某个功能如何工作的观点，你可以通过一个新的提供商来改变这个观点。例如，如果您不喜欢[入口控制器](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/)，请切换到另一个提供商，如 [Istio](https://istio.io/) 或 [Traefik](https://github.com/containous/traefik) 。

有了所有的选择，你会发现自己涉水通过几乎不断变化的意见，因为 Kubernetes 生态系统移动相当快。在本地使用你的网络浏览器访问你编写的应用程序似乎很容易，但是在 Kubernetes 的世界里，你需要通过几个层次来展示你的服务和增加方法的复杂性，例如,[服务网格](https://harness.io/2020/02/service-mesh-fundamentals/)增加了基础设施的复杂性。输入 [Rancher Rio](https://github.com/rancher/rio) 让你起床，开始完成看似简单的目标，就像轻松完成工作一样。

Harness 平台和 Racher Rio 当然有不同的目标。我真的想用一双全新的眼睛去看看牧场主里奥。Harnes 是一个平台，旨在协调众多基础设施和提供商之间的信心建设步骤。Rancher Rio 旨在简化您在 Kubernetes 的启动和运行工作。让我们来看看牧场主里奥。

### 去里约热内卢

Rancher Rio 的安装过程非常简单。类似于我们围绕 [Spinnaker](https://harness.io/2020/02/spinnaker-vs-harness-filming-my-journey/) 和 [Jenkins X](https://harness.io/2020/03/jenkins-x-vs-harness-filming-my-journey/) 所做的比较，安装和部署工作负载的基本目标。

虽然牧场主 Rio 确实更关注实际的 Kubernetes 部署，但它将属于一个有助于实现持续部署的伟大工具，而不是像 Harness 一样的持续交付。当你自己看的时候，一定要访问项目页面的[工作原理](https://rio.io/)部分。我喜欢 Rancher Rio 的一点是，它可以方便地访问您的终端，并且能够站起来启动 [LinkerD UI](https://linkerd.io/) 。

展望建立信任的步骤，牧场主 Rio 采用了一种基于[拉请求的方法](https://github.com/rancher/rio/blob/v0.6.0/docs/continuous-deployment.md)，即 [PR](https://help.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests) 是核心资源或流程编排。这将 Rancher Rio 定位为一个部署引擎，但是您需要告诉 Rancher Rio 您希望您的部署在何时何地移动，并且您需要构建一个复杂的[拉动请求](https://www.atlassian.com/git/tutorials/making-a-pull-request)。

转到一个线束示例，为了实现自动化和成功的部署，我们需要接触的不仅仅是[源代码控制](https://harness.io/2020/03/how-to-use-version-control-systems-to-deliver-better/)。利用也可以帮助协调发布策略和活动。

并非所有部署都是同等创建的。拥有像[金丝雀发布](https://harness.io/2019/07/continuous-delivery-102/)这样的发布策略是一种安全机制，可以确保对发布的信心。金丝雀版本是线束平台的原生版本。下图显示了一只金丝雀没有被提升，原因是自动判断呼叫风险太大，无法通过。这是由设计决定的，因为线束是基于约定的。

目前，即使是在里约热内卢的牧场主手动制作一只金丝雀，也只是模仿在 T21 精心制作一只金丝雀的步骤。

### 命令

这些命令中的大多数都可以在 [Rancher Rio QuickStart](https://github.com/rancher/rio#quick-start) 中找到。文档中有一个[第二快速入门](https://github.com/rancher/rio/blob/v0.6.0/docs/quick-start.md)展示了更多关于从[docker 文件](https://docs.docker.com/engine/reference/builder/)构建的内容。我利用 [EKSCTL](https://eksctl.io/) 让[亚马逊 EKS](https://aws.amazon.com/eks/) 集群启动并运行。

##Rio 命令
#创建 EKS 集群
eksctl 创建集群\
-名称 rio-grande \
-版本 1.15 \
-地区 us-east-1 \
-节点组-名称 standard-workers \
-节点类型 t3.xlarge \
-节点 2 \
-节点-最小 1 \
-节点-最大 3 \
-node-ami auto
#删除 EKS 集群
eksctl 删除 Cluster-name = Rio-grande-region = us-east-1
#安装 Rio
# https://github . com/rancher/Rio #快速启动
#下载
curl-sfL https://Get . Rio . io | sh-
#设置
rio 安装
#验证 Pods
Rio-n Rio-系统 Pods

### 吊带，可持续输送

Harness 平台是专为实现您的持续交付目标而构建的。将[持续部署与](https://harness.io/2020/01/continuous-delivery-vs-continuous-deployment/)持续交付相比较，持续交付协调了安全和成功部署所需的所有建立信任的步骤。想象这是“连续的决策”,在 SDLC 过程中建立信心，并自动做出决定。请随意观看[驾驭测试](https://harness.io/try-continuous-delivery-as-a-service-for-free/)以便今天就开始，不要忘记报名参加我们即将推出的[虚拟驾驭大学](https://harness.io/events/university-virtual)。

干杯！

-拉维