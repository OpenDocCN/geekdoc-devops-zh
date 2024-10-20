# 介绍建议 API:以编程方式寻找潜在的成本节约|利用

> 原文：<https://www.harness.io/blog/recommendations-api>

有了推荐 API，现在有了收集 Harness CCM 提供的见解和建议的编程方式。

Harness 的[云成本管理(CCM)平台](https://harness.io/platform/cloud-cost-management/)是一个强大的平台，旨在节省您在公共云中的成本。合理确定资源大小并最终装箱是增加密度和降低成本的关键。随着时间的推移，跟踪使用情况并了解趋势可能是一项数学练习，并保持对指标高保真度的跟踪。[利用 CCM 建议](https://harness.io/blog/introducing-cloud-cost-recommendations/)允许通过围绕 Kubernetes 工作负载提出的谨慎[建议](https://developer.harness.io/docs/cloud-cost-management/use-cloud-cost-management/ccm-recommendations/home-recommendations/)来消除繁重的工作。

随着推荐 API 的引入，现在有了编程方式来收集 Harness CCM 提供的见解和建议。通过一个 [GraphQL](https://harness.io/blog/graphql-harness-your-way/) 查询，您可以检索 CCM 正在观察的每个 Kubernetes 集群的结果。

## 按 API 列出的 Kubernetes 建议

实施[您的第一个建议](https://harness.io/blog/implementing-cloud-cost-savings/)，收集 CCM 提供的见解可以让 CCM 观察到的每个工作负载受益。[建议 API](https://developer.harness.io/docs/first-gen/cloud-cost-management/cost-explorer-apis/workload-recommendations-api/) 是一种在 Kubernetes 集群中收集见解并在整个企业中扩展成本节约的好方法。

通过进入 Setup - > Harness API Explorer，利用 [API Explorer](https://developer.harness.io/docs/first-gen/firstgen-platform/techref-category/api/harness-api-explorer/) ，您可以执行所需的 GraphQL 查询。在制定查询时，您需要决定希望在查询中接收多少百分比的采样，然后过滤适当的结果作为查询变量。

如果您不熟悉如何进行采样，采样使用实际的 CPU 和内存利用率，并使用直方图方法进行分析。深入研究建议 UI 有助于直观地了解您希望从建议 API 中获得的抽样百分比。

通过调整利用率采样，您可以调整建议，使其更倾向于成本优化的工作负载或性能优化的工作负载。通过这种方式，您不仅仅是信任一个合理调整的建议，您还可以自己定义建议中包含哪些数据，从而节省进行数据验证和提出替代建议的时间和精力。

*显示百分位数的建议用户界面。*

### 执行您的第一个建议 API 查询

[线束文档](https://developer.harness.io/docs/first-gen/firstgen-platform/techref-category/api/harness-api)提供了一个优秀的 GraphQL 查询来帮助您。唯一需要优化的两个部分是查询中抽样的百分比和查询变量中要过滤的数据。

可以将百分比“ *p50* ”元素更改为另一个采样百分比。

查询:

query($ filters:[工作负载过滤器]，$limit: Int！){
k 8 sworkloadverences(filters:$ filters，limit:$limit){
节点{
预计节省量
命名空间
工作负荷名称
集群名称
工作负荷类型
容器重新推荐{
容器名称
当前{
请求{
名称
数量
}
限制{
名称
数量
}
} 【T19

使用查询变量，您可以添加/删除/修改过滤器以显示更多或更少的结果。在本例中，删除了特定集群的过滤器，因此所有具有[部署](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)的集群都进入默认的[命名空间](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)。可以很容易地删除名称空间过滤器，以便显示所有集群中的所有部署。

查询变量:

{
" filters ":[
{
" namespace ":{
" operator ":" EQUALS "，
"values": ["default"]
}，
" workload type ":{
" operator ":" IN "，
" values ":[" Deployment "]
}
}
，
【limit】:1000【

可以在 API Explorer 中执行查询和查询变量。

设置->线束 API 浏览器。

*执行推荐 GraphQL 查询。*

返回建议后，您可以看到不同采样百分位数的建议。

结果:

{
" data ":{
" k8sworkloadverences ":{
" nodes ":[
{
" estimatedSavings ":11.96，
"namespace": "default "，
"workloadName": "cv-demo "，
" Cluster name ":" Example Harness K8s Cluster "，
"workloadType": "Deployment "，
" container recommendations ":[
{

【推荐】:{
【请求】:【
{
【名称】:【cpu】，
【数量】:【25m】
}，
{
【名称】:【内存】，
【数量】:【297m】
}
，
【限制】:
{
【名称】:【内存】，
。
【数量】:【25m】
}，
{
【名称】:【内存】，
【数量】:【258m】
}
]
}
}
}
}，
{
【estimatedSavings】:null，
【命名空间】:【默认】，
“默认”。
{
【名称】:【内存】，
【数量】:【250m】
}
，
【限制】:【
{
【名称】:【内存】，
【数量】:【250m】
}
]
}，
【p50】:{
【请求】:【
。

下一步是对工作量采取行动。

## 如何合理调整 K8s 工作负载

有了返回的建议，简单地调优工作负载的资源限制和请求就可以通过 Harness 实现。我们有一个详细的博客，围绕[实施云成本节约](https://harness.io/blog/implementing-cloud-cost-savings/)的步骤。虽然只是用更新的资源限制/请求修改清单，并通过 Harness 重新部署。

之前:

之后:

随着工作负载的调整，下一步将是调整和优化 Kubernetes 集群本身。

## 如何调整 K8s 集群的大小

纵观所有的见解，想象下一步是否会在重新创建集群时调整集群的大小。

例如，如果使用 AWS EKS，节点机器类型(例如什么 EC2 实例类型)是一个关键部分。对于这个例子，我通常使用一个 [t3.xlarge](https://aws.amazon.com/ec2/instance-types/t3/) ，它有 4 个 CPU 和 16g 内存。

虽然你的操作系统和 Kubernetes 有开销。假设工作节点可以访问所有 4 个 CPU 份额和所有 16gb 内存，但实际情况并非如此。

### K8s 需要多少开销？

Kubernetes worker 节点有一些运行开销。您运行的每个 pod 都有开销，并且每个节点上都有沉没守护进程成本。查看一个 worker 节点上没有工作负载的基线，您可以看到如果 Kubernetes 在开销后可以访问 100%的可用资源时的可用资源量。

通过运行 *free -m* ，进入一个空的 worker 节点:

如您所见，这个节点只有 14.2 GB 的空闲内存。如果您有两个各有 8 GB 资源的 pod，这里只能放置一个。

在空工作节点上运行 Top:

在空闲的任何给定时间，kubelet(节点代理)和 docker/container 守护进程之间占用了 1-3%的 CPU。

### 许多小豆荚或几个大豆荚

了解放置容量的实际可用性是关键。Pod 规模取决于工作负载。拥有更多小型吊舱相对于更少大型吊舱有什么好处吗？根据工作负载，如果较小的单元需要扩展和隔离，那么使用较小的单元是一个好主意。尽管每个 Pod 本身都有占用可用容量的开销。

Kubernetes 在工作负载规模和集群规模方面肯定不是一劳永逸的。像任何系统和软件一样，优化和调优是一个持续的过程。Kubernetes 的好处是能够更快地重新创建，从而允许迭代。

## 重建适当大小的 Kubernetes 集群

合理调整 Kubernetes 集群的规模有两个方面:平衡按需放置的集群容量可用性与询问每个工作负载和应用所有者的使用情况。

平台工程师的任务是提高工程效率，并为 Kubernetes 集群提供足够的容量。虽然如果对工作负载放置和违反直觉的配额进行严格控制，价值主张会开始减弱，但 Kubernetes 提供的自助服务功能会转变为旧的虚拟机模式。

在进行讨论和做出调整决策时，数据和分析至关重要。随着时间的推移，利用 CCM 的建议可以消除实际使用中的猜测。

通过汇总不同工作负载的建议，使用类似 [EKSCTL](https://eksctl.io/) 的工具重新创建 Kubernetes 集群，只需调整节点类型和/或节点数量就可以节省成本。

#创建 EKS 集群
eksctl 创建集群\
-name big-money-Cluster \
-版本 1.19 \
-region us-east-1 \
-node group-name standard-workers \
-node-type T3 . xlarge \
-nodes-min 1 \
-nodes-max 3 \
-node-ami auto \
-ssh-access \
-ssh-ssh-

通过基于 Harness CCM 建议的谨慎调整，您已经走上了增加密度和节约成本的道路。

## Harness，您的云成本合作伙伴

在 Harness，我们的使命是实现云支出的民主化，并在您消耗云资源的同时，帮助传播有价值的谨慎信息。

不仅仅限于 Kubernetes，Harness CCM 还可以帮助跟踪和可视化跨多个资源的公共云支出，这些资源也可以通过 API 提取[。](https://developer.harness.io/docs/first-gen/cloud-cost-management/cost-explorer-apis/ce-cost-explorer-apis/)

*线束 CCM 仪表板*。

敬请关注，并确保今天就报名参加[云成本管理](https://harness.io/platform/cloud-cost-management/)的演示。

干杯，

拉维