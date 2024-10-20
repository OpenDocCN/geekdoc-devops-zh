# kops 102 -在顺风航道上部署 Kubernetes 的内部观察

> 原文：<https://www.fairwinds.com/blog/kops-an-inside-look-at-deploying-kubernetes-on-aws-the-fairwinds-way>

 在上一篇 kops 帖子中，  [kops 101](http://blog.reactiveops.com/kops-the-kubernetes-deployment-game-changer) ，我介绍了 kops 是什么以及为什么它是专业级 Kubernetes 安装的正确选择。这一周，我将介绍如何用 kops 建立 Kubernetes。

我想分享一个在 AWS 上带有  [kops](https://github.com/kubernetes/kops) 的 Fairwinds 式[Kubernetes](https://kubernetes.io/)配置的例子。它包含了一个未记录的特性，DevOps 社区可能会觉得非常有用。在我们谈得太远之前，我还想介绍一下[风向](/)方式的想法。一个持续的试错过程使我们的团队能够以一种深思熟虑的、可重复的方式为创建基础设施设定最佳实践。这意味着我们有自己的观点并支持它。

就这样，让我们开始吧。

用 kops the Fairwinds 方法创建 Kubernetes 集群有两个步骤/组件:

1.  使用[terra form](https://www.terraform.io/)在  [亚马逊 Web 服务](https://aws.amazon.com/) (AWS)上创建虚拟私有云(VPC)。
2.  让 kops 处理所有其他事情，这意味着在同一个 VPC 中添加额外的网络资源，启动组成集群的实例并验证它是否工作。

## 步骤 1:配置您的 VPC

我们使用 Terraform，一款由  [哈希公司](https://www.hashicorp.com/)开发的开源基础设施管理和供应工具，来创建我们的基本 VPC 和网络布局。我们总是使用经过测试和验证的相同初始结构。我们创建的 VPC 旨在托管 Kubernetes 和我们可能希望 Kubernetes 与之交互的任何其他基于云的资源，如托管数据库、缓存和消息队列。

简单介绍一下我们的 VPC 能为您带来什么:

*   多可用性区域(AZ)(通常为 3 个)
*   每个区域 4 组子网–1 组面向公众，1 组用于管理功能、私人工作和私人生产
*   每个 AZ 个 AWS 管理的 NAT 网关，私有子网中的资源可以通过该网关访问外部互联网

VPC 足够灵活，几乎可以应对我们范围内的任何情况，而且我们对其进行了精心设计，因此不会出现单点故障。

当然，你可以让 kops 为你创建 VPC。这是默认模式，公平地说，这绝对是一种有吸引力的工作方式，特别是因为我们已经设计了 kop 来自动对您的基础架构做出明智的选择。然而，在 [Fairwinds](/) ，我们认识到 Kubernetes 只是高度复杂的生态系统中的一环。因此，我们用 Terraform 搭建舞台，然后让 kops 将 Kubernetes 放在舞台上。

## 第二步:让 kops 发挥它的魔力

现在我们已经有了 VPC 和基本的网络资源，我们可以进入特定于 Kubernetes 的配置了。

要从我们的 Terraform VPC 设置进入工作集群，请使用以下四个命令:

```
kops create -f cluster_spec.yml 
kops create secret --name $CLUSTER_NAME sshpublickey admin -i $SSH_KEY_PATH 
kops update cluster $CLUSTER_NAME # Sanity check the output. Make sure that kops is 
only making the changes you expect. 
kops update cluster $CLUSTER_NAME --yes
```

这真的是我们部署集群所需的全部吗？是也不是。虽然这可能不是您部署第一个集群的方式，但是一旦您浏览了几次，并且了解了集群规范的工作原理，这将是定义和创建集群的一个很好的方式。

因此，让我们花点时间来讨论集群规范。集群规范是 kops 的一个基本概念。不是所有的 Kubernetes 集群都有一个集群规范，但是所有 kops 创建的集群都有。集群规范是 kops 创建的 yaml 文档，它用代码定义了 kops 部署和管理集群所需的一切。它存在于你的州立商店里，对于 AWS 来说是在 S3。如果您有一个集群规范，您就有了创建一个 Kubernetes 集群所必需的蓝图，这就是为什么我们只需要几个命令就可以创建一个 Kubernetes 集群(如上所示)。

如果您有一个使用 kops 构建的工作集群，并且您想要查看或保存集群规范，那么应该这样做:

```
kops get cluster ${CLUSTER_NAME} -o yaml --full > cluster.yml 
kops get ig nodes -o yaml > nodes.yml 
kops get ig masters -o yaml > master.yml 
… 
Merge the .yml files and put “---” between them into one large cluster_spec.yml See 
cluster_spec-example.yml
```

与 kop 互动的方式有很多种，但这里我要强调 kop 一个相对不为人知的特性——通过  `kops create -f`直接创建你的集群。此功能旨在镜像  `kubectl create -f`，用于创建和配置所有类型的 Kubernetes 资源。在这里，kops 一次性获取整个集群的规范，推断出任何未指定的值，然后创建您的集群。如果您正在编写集群部署的脚本或模板，这将非常有用(参见 cluster_spec-template.yml.j2，了解如何开始考虑开发模板的一些想法)。

用 kops 创建集群的更广为人知的方法是使用一个脚本来包装包含在  `deploy.sh`中的命令行标志。如果您运行  `deploy.sh`，它将创建一个基本的集群规范，然后您可以立即部署该规范，或者通过  `kops edit cluster`进行编辑以定制您的集群配置。最后可以用  `kops update cluster`部署。但是对于那些习惯于完全自动化的剧本、食谱或其他自动化的基础设施管理方法的人来说，我上面描述的确实是交互式的，并且花费了操作员大量的时间。如果您正在进行任何定制(我们做了很多)，它也引入了出错的机会。那就是模板化和  `kops create -f` 进来的地方。

对于开发一致的、可重复的基础设施，我的建议是最初手工交互地创建几次集群，并密切关注集群规范如何响应您的更改。到那时，您将有一个满足您需求的基本规范，并且您可以使用它作为构建更多集群的模板。

请注意，我们将集群规范配置为在安全的私有拓扑中启动 Kubernetes，这意味着主节点和节点位于私有子网中。kops 创建了与私有子网 1:1 的“公用”子网。实用程序子网托管 Kubernetes API 服务器的弹性负载平衡器(ELB)、Kubernetes 中启动的任何外部服务的 elb 以及 bastion 主机(如果使用的话)。

对于用 kops 创建的 RO 风格的 Kubernetes 集群，我们更喜欢让 kops 在我们现有的 VPC 中完全管理自己的配置(NAT 网关除外)。我们发现，让 kops 控制网络、安全组和实例，可以让 it 部门优化对基础架构的升级和其他更改的管理。对于 NAT 网关，我们指导 kop 使用我们最初作为 VPC 创建的一部分生成的网关，因为它们是可扩展的和冗余的——并且相当昂贵。即使我们在 VPC 中有多个集群，我们也可以继续获得私有拓扑的好处，而不会增加相关的网络成本。

如果  `kops update cluster`一切顺利，几分钟之内您就可以运行  `kops validate cluster $CLUSTER_NAME` ，并且知道您的集群已经准备就绪。

这种配置看起来很复杂。那么，我们为什么要这样工作呢？

简而言之，Terraform 非常擅长打基础。它塑造了景观，并为 Kubernetes 创造了一个休息的地方。反过来，kops 是专门为 Kubernetes 打造的。它是可预测的、快速的，可以处理所有的群集资源调配，并且比任何其他工具都更好地更新和管理群集。当你同时使用 Terraform 和 kops 时，你就拥有了完成手头任务的最佳工具。

以下是示例脚本、程序和规范的链接:【https://github.com/geojaz/ro-intro-to-kops-blog】T2