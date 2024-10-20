# 泰克顿 vs 马具 CD -拍摄我的旅程|马具

> 原文：<https://www.harness.io/blog/tekton-vs-harness>

Ravi 拍摄了他学习 Tekton 教程的过程。让我们看看他做得怎么样！

紧随西雅图 KubeCon 2018 之后，[连续交付基金会](https://www.linuxfoundation.org/press-release/2019/03/the-linux-foundation-announces-new-foundation-to-support-continuous-delivery-collaboration/)于一年多前的 2019 年 3 月成立，Harness 是其成员之一。宪章项目是我自己熟悉的 Spinnaker 和 Jenkins/Jenkins X 的项目。虽然还有一个我以前从未听说过的项目是 [Tekton](https://github.com/tektoncd) ，我非常好奇想了解更多关于这个项目的信息。使用答案框，也就是谷歌，这个项目非常环保，甚至搜索“Tekton”也不会很容易找到这个项目。直到今天， [Tekton 网站](https://tekton.dev/)仍在建设中，这个项目已经进行了一年。

快进到今天，这个项目还在继续发展。虽然 Tekton 现在看起来更像是一个引擎，例如为 OpenShift 管道甚至 Jenkins X 管道提供动力。我用一双新的眼睛通过使用 Tekton 本身进行了一次练习。目标类似于我们围绕 Spinnaker 和 Jenkins X 进行的其他比较，如果我们有一个映像，例如 Nginx，我们如何部署到一个等待的 Kubernetes 集群，在我的例子中，是亚马逊 EKS。

这段视频大约有一个小时长，我浏览了一下 [Tekton 教程](https://github.com/tektoncd/pipeline/blob/master/docs/tutorial.md)。这些线路似乎在泰克顿一起工作；我的底线是创建从源到图像的构建步骤。当您自己查看 Tekton 时，我推荐 IBM Tekton 教程，它很好地列出了 Tekton 的概念。在视频结束时，我能够在大约 5 分钟内将 Nginx 从零部署到部署，包括将一个 Harness 委托安装到我的 EKS 集群中。

## “泰克托尼克”视频

事不宜迟，下面是视频。与其他视频相比，新的一天没有过去，也不需要换球衣。

## 方法学

对于许多人来说，部署映像是一个很好的起点。根据现有的开发方法，转向基于 [GitOps](https://harness.io/blog/what-is-gitops/) 的方法可能很困难。具有适当布线的 Tekton 可以遵循严格的 GitOps 模型，或者一旦生成了适当的 Tekton 对象并在每个管道中就位，就可以脱离。

利用 Tekton，您需要创建几个对象。熟悉一个 [Tekton 任务](https://github.com/tektoncd/pipeline/blob/master/docs/tasks.md)和一个 [Tekton 流水线](https://github.com/tektoncd/pipeline/blob/master/docs/pipelines.md)，两者都有需要执行的运行对象。因为 Tekton 利用了[定制资源定义](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)，所以你需要跟踪这些对象。

我选择用亚马逊 EKS 在 AWS 上运行我的 Kubernetes 集群。从 Tekton 设置开始需要一个持久的区域，我可以在亚马逊 S3[快速旋转，因为我是我账户的管理员。因为我没有利用源到图像或其他持久 Tekton 管道步骤，所以 S3 桶从未被写入。](https://aws.amazon.com/s3/)

随着 Tekton 向引擎方向发展，一旦你有了 Tekton，你就有了 Tekton 所拥有的一切。当时，我在这个项目中找不到任何现成的权限，访问控制是 Harness 平台的一个支柱。

最后，我启动了 Harness 平台，很快就成功了。

这在一定程度上要归功于 Harness CD 抽象模型，在这个模型中，装载移动部件很容易，装配管道也很快。

相比之下，下面是基本 Tekton 管道所需的布线。

## 我的泰克顿台阶

##Tekton 命令

# Install tek ton
ku bectl apply-filename https://storage . Google APIs . com/tek ton-releases/pipeline/latest/release . YAML

# Monitor Installation
ku bectl get pods-namespace tek ton-pipelines-watch

# Config Map for S3-tek ton-storage . YAML
API version:v1
kind:Secret
metadata:
name:tek ton-metadata
https _ Validate _ certificates = True
-
API version:v1
kind:CONFIG Map
metadata:
name:CONFIG-artifact-bucket
namespace:Tekton-pipelines
data:
location:S3://Tekton storage
bucket . service . account . secret . name:Tekton-storage
bucket . service . account . key:BOTO-CONFIG
bucket . service .
#创建任务-nginx-Task . YAML
API version:tekton.dev/v1beta1
种类:任务
元数据:
名称:nginx-task
规格:
步骤:名称:第一张
图片:nginx
#应用任务
kubectl 应用-f nginx-Task . YAML

#描述任务
tkn 任务描述 nginx-任务

#验证任务运行
tkn 任务运行描述 nginx-任务运行
tkn 任务运行日志 nginx-任务运行

#将任务包装在管道中-Pipeline . YAML
API 版本:tekton.dev/v1beta1
种类:管道
元数据:
名称:nginx-管道
规格:
任务:
-名称:nginx-管道-任务
任务
名称:nginx-Pipeline

#应用管道运行
ku bectl Apply-f Pipeline-Run . YAML

#获取管道日志
tkn 管道运行日志 nginx-Pipeline-Run-1-f

#获取管道计划
tkn 管道运行描述 nginx-Pipeline-Run-1

# # # # #助手方法
#创建 EKS 集群

-nodes-min 1 \
-nodes-max 5 \
-node-ami auto

# Delete Cluster
eks CTL Delete Cluster-name = The-tek tons-region = us-east-1

# Add K8s Dashboard
kubectl apply-f https://raw . githubusercontent . com/kubernetes/Dashboard/v 2 . 0 . 0-rc6/AIO /登录

## 马具平台，你的信心引擎

Harness 平台是专门构建的，以基于约定的方式接受广泛的工作负载和信心建立步骤。Harness 平台允许平台以一种易于消费的方式编排许多方法，从而可以增强您的持续交付目标。

能够查看几个工具，如 [Spinnaker](https://harness.io/blog/spinnaker-vs-harness-filming-my-journey/) 、 [Argo](https://blog.harness.io/blog/continuous-delivery/argo-flux-vs-harness-filming-my-journey/) ，以及现在的 Tekton，这些工具需要大量的工程专业知识。在 [DeliverConf](https://www.infoq.com/news/2020/03/reimagining-cicd-pipelines/) ，最近的一次谈话谈到了作曲的需要；当你需要的时候，将你的管道分解成你所需要的，这也是代码繁重的解决方案所暗示的。其中的难点在于将您的可部署性和基础设施耦合起来，以便在正确的时间拥有正确的组件来执行部署。这非常类似于设计您正在部署的实际软件；管理旅程中的功能。

我们对线束平台采取了不同的方法。我们已经开发了一个易于使用的界面，作为基于约定的部署和部署模式的记录系统。今天就报名参加[线束试用](https://app.harness.io/auth/#/signup)吧，把自己从为自己的平台搭建平台中解放出来。

干杯，

-拉维