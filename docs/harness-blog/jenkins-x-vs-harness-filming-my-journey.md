# 詹金斯 X vs 马具-拍摄我的旅程|马具

> 原文：<https://www.harness.io/blog/jenkins-x-vs-harness-filming-my-journey>

哦船另一个比较！詹金斯 X vs 马具。你不必成为 GitOps 专家来利用 Harness。不要打双学曲线。

Jenkins 是过去十年 DevOps 运动中最有影响力的工具之一。来自[哈德森](https://en.wikipedia.org/wiki/Hudson_(software))的詹金斯已经迎来了与大众的持续融合。尽管权威人士可能会把詹金斯称为“ [DevOps 胶带](https://www.reddit.com/r/devops/comments/90be66/does_jenkins_suck_in_general_or_just_for_some/)”，意思是如果你有詹金斯，你就要勾选 DevOps。随着 DevOps 运动的不断成熟，DevOps 肯定不是一种工具，而是人、过程和技术的顶峰。

Jenkins 平台生态系统会像任何流行的软件平台一样继续发展。大约一年前，我对詹金斯不同版本的新版本进行了评论。虽然我从来没有尝试过[詹金斯 X](https://jenkins-x.io/) 直到现在。类似于我们围绕[三角帆 vs 马具](https://harness.io/blog/spinnaker-vs-harness-filming-my-journey/)所做的视频之旅，一双新鲜的眼睛第一次期待利用詹金斯 X。

我对 Jenkins 非常熟悉，但还没有亲身体验过 Jenkins X 中描述的更严格的[gitop](https://harness.io/blog/what-is-gitops/)概念/定义。为自己了解 Jenkins X 做准备，在开始之前先看看 [Jenkins X 概念](https://jenkins-x.io/docs/concepts/)。我有一个简单的目标，就是在等待的亚马逊 EKS Kubernetes 集群上部署来自 Docker Hub 的 T4 Nginx 映像。视频时长 90 多分钟。在经历了一些复杂的安装之后，我不得不降级我机器上的 Helm 和 kubectl 版本，我挥舞着白旗。在视频结束时，我能够在大约 5 分钟内将 Nginx 从零部署到部署，包括将一个[线束委托](https://developer.harness.io/docs/first-gen/continuous-delivery/kubernetes-deployments/connect-to-your-target-kubernetes-platform/)安装到我的 EKS 集群中。

### 让我们去詹金斯 X'in

事不宜迟，下面是视频。是的，我穿了一件绿色衬衫参加我们团队的虚拟圣帕特里克节庆祝活动。

### 方法学

在分布式系统/平台工程领域待了一段时间后，测试过程中最容易实现的就是部署 Nginx。我很喜欢使用亚马逊 EKS 的 Kubernetes 集群，因为我可以用 [EKSCTL](https://eksctl.io/) 快速旋转一个集群。

我有能力根据需要增加 AWS 资源，并且作为一名管理员，确保 [IAM 角色](https://aws.amazon.com/iam/)等就位。使用答案框(又名谷歌)寻找答案，并利用来自 [Jenkins X 项目](https://jenkins-x.io/docs/)和 [CloudBees](https://docs.cloudbees.com/docs/cloudbees-jenkins-x-distribution/latest/) 的文档。

我最初在 MacBook Pro 上运行 [Helm](https://helm.sh/) V3 和 [kubectl](https://kubernetes.io/docs/reference/kubectl/kubectl/) 1.17.4 时遇到了一些困难。根据 JX 的安装程序，我需要降级到 V2 头盔和低于 1.17.0 的 kubectl 版本，这个版本迟早会成熟。我遇到的第二个障碍是我的 EKS 集群名有大写字母，因为我是[骆驼案](https://en.wikipedia.org/wiki/Camel_case)的粉丝。JX 安装程序脚本会用一个小写的集群名来更改 *jx-requirements.yml* 。使用小写集群名称， [AWS CLI](https://aws.amazon.com/cli/) 无法找到集群，因为 AWS EKS CLI 区分大小写。

不过这些障碍都被克服了，因为我是从零开始，并且拥有对我的 AWS 帐户、本地机器和 GitHub 库的管理员级访问权限。让我着迷的是严格的 GitOps 模型的学习曲线。如果我正在做的项目是一个绿地项目，并且我们专门构建了这个项目来很好地塑造一个 GitOps 模型，那么更多的移动部分将是有意义的。

詹金斯 X 项目正在改变，利用 Tekton 作为管道引擎，这对我来说是新闻。我没有尝试更困难的部署，例如金丝雀部署，以减少詹金斯 X 和现在泰克顿的学习曲线。谢天谢地， [CloudBees 最近创建了一个测试版 UI，这样我就可以在快速启动中看到所有正在运行的管道步骤。列出我的步骤，让你知道我在詹金斯 X 旅程中的位置。](https://docs.cloudbees.com/docs/cloudbees-jenkins-x-distribution/latest/user-interface/install)

### 我的脚步

在你启动资源之前，确保看一下项目中的 [Jenkins X 组件](https://jenkins-x.io/docs/concepts/components/)和[图](https://jenkins-x.io/docs/concepts/diagram/)。我尽我所能地阅读了《T4 X 入门指南》和《快速入门指南》，之前从未看过该指南。如果你和我一样，有新版本的 Helm 和 kubectl，我会提供一些辅助工具。还包括创建和删除 EKS 集群时的 EKSCTL 参数。

# Jenkins X Steps
# CLI Install for Mac OS
brew Install Jenkins-X/JX/JX
# Get Installables
JX boot
# Configure AWS Cloud Provider for Install
# https://Jenkins-X . io/docs/getting-started/setup/boot/clouds/Amazon/
# JX-requirements . yml
cluster config:
Provider:eks
cluster:
cluster name:deploybash _ profile
helm init
#删除 KubeCTL 并安装早期版本[1 . 17 . 0 之前版本]
# https://www . Ben pickles . com/articles/72-downgrading-KubeCTL-with-home brew
# https://github . com/kubernetes/KubeCTL/releases
brew uninstall-ignore-dependencies KubeCTL
curl-LO https://storage . Google API s . com/kubernetes-release/release/kube CTL/usr/local/bin/kube CTL
kube CTL version-client
# Reset kube config
AWS eks-region us-east-1 update-kube config-name deploy-to-me
kube CTL get ns
# Run JX boot
CD Jenkins-x-boot-config/
JX boot
#生成 Git Bot 令牌
https://github.com/settings/tokens/new?scopes=repo，read:user，read:org，user:email，write:repo_hook，delete _ repo
# Install Jenkins X UI
# https://docs . cloud bees . com/docs/cloud bees-Jenkins-X-distribution/latest/user-interface/Install
JX add app JX-app-UI-version 0 . 1 . 49
# Merge PR
https://github.com/repo/environment-deploy-to-me-dev/pull/1
# Launch quick start
JX create quick start-f http
-节点 2 \
-节点-最小 1 \
-节点-最大 3 \
-节点-ami 自动
eks CTL delete cluster-name = deploy-to-me-region = us-east-1

### 展望未来

在视频的最后，我决定启动一个快速的 [Harness 工作流](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/workflows/workflow-configuration/)，并在我们的 Harness 社区版中直接执行该工作流。我是 Jenkins 为 DevOps 运动所做工作的忠实粉丝，很高兴能得到 Jenkins X。随着实践和范式围绕 GitOps 不断发展，任何新工具的复杂性都将存在。当看到像 GitOps 这样的新范例时，在工具和方法上对抗学习曲线是双重打击，这使得学习更加困难。

观看一个 [JavaZone 视频](https://vimeo.com/362768726)和通读一篇 [DZone 文章](https://dzone.com/articles/progressive-delivery-with-jenkins-x-automatic-cana)关于 Jenkin X 的金丝雀方法感觉被限制在堆栈中，我对我的 Jenkins X 安装的信心没有全部尝试。在 Harness，我们采用多种方法，例如，开启和关闭 GitOps 成熟度模型，以便为您带来持续交付，无论您处于旅程的哪个阶段。您不必利用最新的 [CNCF](https://www.cncf.io/) 堆栈来从线束中获取价值。使用 Harness [软件交付平台](https://harness.io/platform/)简化您的交付工作，立即注册！

干杯，

-拉维