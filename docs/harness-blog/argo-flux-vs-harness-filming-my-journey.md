# 阿尔戈通量与驾驭-拍摄我的旅程|驾驭

> 原文：<https://www.harness.io/blog/argo-flux-vs-harness-filming-my-journey>

大约在 [KubeCon 北美 2019](https://harness.io/2019/11/kubecon-na-recap-cloud-native-mainstream/) 前后，两个目标相似的开源项目——[Argo](https://argoproj.github.io/argo-cd/)和[Flux](https://www.weave.works/oss/flux/)——决定结成合作伙伴关系，创建 Argo Flux。阅读 Intuit 的[新闻简报，Argo 来自被 Intuit 收购的](https://www.intuit.com/blog/technology/introducing-argo-flux/) [Applatix](https://applatix.com/) 。Flux 来自 Weaveworks 的产品，名为 Weaveworks Cloud，随着 Flux 的发展，它成为了自己的项目。

随着 GitOps 实践开始曝光，Argo、Flux 和 Jenkins X 成为协助 GitOps 咒语的工具。去年，我参加了一个[新堆栈制造商的播客](https://soundcloud.com/thenewstackmakers/what-is-gitops-and-why-it-might-be-the-next-big-thing)，与 Weaveworks 讨论 GitOps 的现状。我的观点不同于 Weaveworks 对 Flux 的严格定义，例如，你必须使用 Git、Kubernetes 和 infrastructure 作为代码。

拍摄的时候，项目还在融合，互相学习。没有一个项目或产品被称为“Argo Flux”——它更像是 Argo 或 Flux。两个项目的第一个共同之处是 [GitOps 引擎](https://github.com/argoproj/gitops-engine)。我必须选择 Argo 或 Flux 作为起点，我选择了 Argo。为了你自己的旅程，看一看 Argo 使用的[分类法。类似于我们的帖子潜入](https://argoproj.github.io/argo-cd/core_concepts/)[大三角帆](https://harness.io/2020/02/spinnaker-vs-harness-filming-my-journey/)和[詹金斯 X](https://harness.io/2020/03/jenkins-x-vs-harness-filming-my-journey/) ，我想看看阿尔戈；我有一个等待利用的[亚马逊 EKS](https://aws.amazon.com/eks/) Kubernetes 集群。对于基本安装，该视频大约 50 分钟长。我能够让 Nginx 从零部署到已部署，包括在视频结尾的大约 5 分钟内将一个[线束委托](https://docs.harness.io/article/m383u53mp1-connect-to-your-target-kubernetes-platform)安装到我的 EKS 集群中。

### 我的阿尔戈[和/或]变迁之旅

我花了两个晚上拍摄这个视频，这给了我一些离开镜头的时间来思考我在做什么。

### 方法学

对于许多人来说，部署映像是一个很好的起点。根据现有的开发方法，转向完全基于 GitOps 的方法可能会很困难。利用一个[拉取请求](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests)来开始或结束编排，对于你的项目应该如何构建，确实有一个学习曲线。如果你有一个绿地项目，这比现有的工件和过程更容易。

与我使用的其他 GitOps 开源工具相比，Argo 更容易安装。当我试图在 GitHub 中构建我的项目时，我确实遇到了一个[问题，这个问题是谷歌的回答框给我带来的，文档中没有明确说明如何实际开始。](https://github.com/argoproj/argo-cd/issues/1719) 

Argo like Flux 的目标部署是 Kubernetes。经过一番试验后，我不需要利用像 [Helm](https://helm.sh/) 或 [Kustomize](https://kustomize.io/) 这样的包/配置管理工具来强迫我的项目符合这些工具。

我预计的一些挑战将是你被 Kubernetes 启用和限制。看看其他的 Argo 项目，如 [Argo Rollouts](https://argoproj.github.io/argo-rollouts/) 预期会改变您的 Kubernetes 集群。根据 Argo 推出的意见，在您的部署中，YAMLs 将是推出步骤。我把我的步骤记录在下面，让你能够到达我的停留点。

### 我的脚步

# # Argo Flux Commands
# Install on K8s Cluster
kube CTL create namespace argocd
kube CTL apply-n argocd-f https://raw . githubusercontent . com/argoproj/Argo-CD/stable/manifests/Install . YAML
# Install CLI
brew tap argoproj/tap
brew Install argoproj/tap/argocd
# Expose Service
kube CTL patch SVC argocd-server cut-d '/'-F2
# PW:argocd-server-84 B4 bb 5d 5-npn 59
argocd 登录 dns.us-east-1.elb.amazonaws.com
# Update PW
argocd 账号更新-密码
# PW:ravi 1234
#获取并同步应用
argocd app 获取留言簿
argocd app 同步留言簿
#删除应用
argocd app 删除留言簿-级联

### 在 GitOps 之前、之中和之后，将平台带在身边

Harness 平台的一大吸引力在于，我们对您部署的基础设施并不感兴趣。您不必将 Git +基础设施作为符合 code +的 Kubernetes 集群来利用 Harness。最初投入利用是相当容易的，因为我们涵盖了关于如何生产和推广工件的大量基础设施和方法。

Harness 支持 GitOps 模型，其定义非常严格，您和您的组织都可以接受。利用 Git +基础设施作为代码+ Kubernetes 的三大优势当然是受支持的。不过，这里有一些值得思考的东西——如果你正在部署到[亚马逊 ECS](https://aws.amazon.com/ecs/) 或者部署一个[亚马逊 AMI](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html) 或者甚至一个虚拟机呢？

Harness 平台当然可以是一个基于约定的管道，用于协调所有建立信任的步骤，并部署到广泛的基础设施中。开始使用脊甲非常简单，[今天就报名参加试用](https://harness.io/try-continuous-delivery-as-a-service-for-free/)，准备好一会儿就开始摇滚吧。

干杯！

-拉维