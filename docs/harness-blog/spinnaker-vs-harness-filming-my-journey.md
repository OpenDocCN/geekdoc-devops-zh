# 三角帆 vs 马具-拍摄我的旅程|马具

> 原文：<https://www.harness.io/blog/spinnaker-vs-harness-filming-my-journey>

从我的旅程中学习如何让三角帆 vs 马具离开地面。观看视频以比较 Spinnaker 项目安装到控制工作流程。

当人们考虑连续交付解决方案时，大多数时候人们会交叉购买 [Spinnaker](https://spinnaker.io/) rendition 与 Harness。在技术上，我们当然可以像猫和狗的争论一样站在哪一边【我有一只[西伯利亚雪橇犬](https://en.wikipedia.org/wiki/Siberian_Husky)；你可以看到我站在哪一边】。有一段时间我想做的事情是通过 [Spinnaker 管道](https://spinnaker.io/concepts/pipelines/)运行一两条管道，并为自己学习。

我非常熟悉 Spinnaker 项目和这个项目的目标，但是我从来没有尝试过自己安装这个工具。我决定拍摄一段视频，记录我自己在计时的同时尝试完成一个安装和基本管道。(虽然进行了大约四分之一，我决定停止花时间，因为我必须查找命令——只是基本的 [Linux](https://en.wikipedia.org/wiki/Linux) 管理东西，我忘记了，必须在视频拍摄期间查找。)

这段视频大约有两个小时长，是在一个晚上和一个早上拍摄的。我的目标是将典型的 Nginx 映像部署到 T2 亚马逊 EKS T3 集群中。不幸的是，过了一段时间后，我无法在 Spinnaker 中创建系统帐户，并抛出了白旗。我能够让 Nginx 从零部署到已部署，包括在视频结尾的大约 5 分钟内将一个[线束委托](https://developer.harness.io/docs/first-gen/continuous-delivery/kubernetes-deployments/connect-to-your-target-kubernetes-platform/)安装到我的 EKS 集群中。

### 视频

事不宜迟，下面是视频。(是的，我确实连续两天穿同一件套衫。)

### 方法学

在分布式系统/平台工程领域已经有一段时间了，现在测试过程中最容易实现的就是部署 Nginx。我很喜欢使用亚马逊 EKS 的 Kubernetes 集群，因为我可以用 [EKSCTL](https://eksctl.io/) 快速构建一个集群。

作为一名管理员，我有能力根据需要增加 AWS 资源，并确保[我的角色](https://aws.amazon.com/iam/)等。已经就位。我使用答案框(又名谷歌)寻找答案，并利用来自 [Spinnaker 项目](https://spinnaker.io/guides/)和[军械库](https://www.armory.io/)的文档。

我在早期遇到了一些挑战，我需要改变我计划安装 Spinnaker 的方式。我最初的目标是在我的 EKS 集群中安装 Spinnaker，但是我的 [Halyard](https://spinnaker.io/reference/halyard/) 配置中出现了一些错误，它将 AWS 作为提供者，并且不喜欢我的 EKS 集群。所以，我在我的升降索 EC2 [Ubuntu](https://ubuntu.com/) 机器上安装了 Spinnaker。

在我能够启动并运行 Spinnaker UI 之后， [Armory](https://docs.armory.io/spinnaker/armory-spinnaker-quickstart-1/) 拥有比 OSS Spinnaker 项目更好的文档，用于我部署到亚马逊 EKS 的特定目标。

在视频的第二个小时左右，我抛出了白旗，因为我无法让需要代表我执行部署的系统帐户显示在 Spinnaker UI 中。工具提示建议联系我的 Spinnaker 管理员，也就是我自己— fail。尝试了一段时间后，我有点累了。你可以在视频和下面看到我的步骤。

### 我的脚步

在您开始之前，请确保先了解一下 [Spinnaker 架构](https://spinnaker.io/reference/architecture/)以及您需要安装和交互的活动部件。我尽我所能浏览了军械库 [Spinnaker 安装流程](https://spinnaker.io/setup/install/)和 [AWS 快速入门指南](https://docs.armory.io/spinnaker/armory-spinnaker-quickstart-2/)，之前从未见过它们。

我用 Ubuntu LTS 18.04 创建了一个 [AWS EC2](https://aws.amazon.com/ec2/) 实例，最终打开了端口。在我的本地机器 MacBook Pro 上，我将 [AWS CLI](https://aws.amazon.com/cli/) 和 [KubeCTL](https://kubernetes.io/docs/tasks/tools/install-kubectl/) 连接到我的 EKS 集群。

可能不必要的步骤是根据这篇[博文](https://blog.spinnaker.io/exposing-spinnaker-to-end-users-4808bc936698)打开[门](https://github.com/spinnaker/gate)和[甲板](https://github.com/spinnaker/deck)。

##Spinnaker 命令
#Ubuntu Box - EC2 公共 IP: your-ip
ssh -i ~/。ssh/your-key . PEM Ubuntu @ your-IP
# Get Halyard Install for Ubuntu
curl-O https://raw . githubusercontent . com/spinnaker/Halyard/master/Install/debian/Install Halyard . sh
# Install Halyard on Ubuntu
sudo bash Install Halyard . sh
# Get Cloud Formation templates on Local Machine
curl-O https://d 3079 GX vs 8 ayeg . cloudfront . net/templates
sudo adduser some user
pw:some password
#连线一个 Hal 提供者以检查安装选项
hal 配置提供者 aws 帐户添加您的帐户\
-account-id xxxx \
-assume-role role/spinnaker managed
#在我的情况下本地安装
hal 配置部署编辑类型 localdebian
# #不要设置 https://github.com/spinnaker/spinnaker/issues/4554 地区
#配置存储
hal 配置存储 S3 编辑\

-secret-access-key xxxxx
# Set Version
Hal config Version Edit-Version 1 . 18 . 3
# Restart
sudo Hal deploy apply
# Tunnel
sudo Hal deploy connect
# Edit IPs
Hal config security ui Edit-override-base-URL https://yourIP:9000
Hal config security API Edit-override-base-URL https://yourIP:8084【T31
export ROLE _ NAME = ROLE/Spinnaker-Managed-ROLE
# Add Provider
Hal config Provider AWS ACCOUNT Add $ { AWS _ ACCOUNT _ NAME } \
-ACCOUNT-ID $ { ACCOUNT _ ID } \
-assume-ROLE $ { ROLE _ NAME } \
-regions us-east-1
# Enable
Hal config Provider AWS Enable
sudo Hal Deploy apply
sudo system CTL daemon-reload【T49 /spinnaker-tools create-service-account
#复制本地 KubeConfig-SA 文件
scp -i ~/。ssh/your-key . PEM kube config-sa Ubuntu @ your-IP:/Home/Ubuntu
#这个地点是 TBD，MV 很成功但是不得不搬回 Ubuntu Home
sudo MV kube config-sa/Home/some user/。哈尔/。secret
#将 KubeConfig 添加到 Halyard
hal 配置提供者 kubernetes 帐户添加 kubeconfig-sa-eks \
-提供者-版本 v2 \
-kube Config-file/home/Ubuntu/kube Config-sa \
-only-spinnaker-managed true
# #出现错误
hal 配置提供者 kubernetes 帐户删除 kube Config-sa-eks
#重新部署【T67Hal/config
# EC2 NetStat for UI 和 API
sudo NetStat-tul pn | grep 9000
sudo NetStat-tul pn | grep 8084

### 展望未来

在视频的最后，我决定启动一个快速的 [Harness 工作流](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/workflows/workflow-configuration/)，并在我们的 Harness 社区版中直接执行该工作流。我去寻找帮助，以采取 SaaS 自定进度的 Spinnaker 试驾，并不能找到一个现成的双重检查我的工作。在[中间层的 DC 操作系统](https://docs.d2iq.com/mesosphere/dcos/services/spinnaker/0.3.0-1.9.2/)中可能有一个，但那需要 DC 操作系统的专业版。你可以免费注册一个线束账户，然后执行和我一样的[线束工作流程](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/workflows/workflow-configuration/)，时间和我差不多。

干杯！

-拉维