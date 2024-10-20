# 云计费-寻找成本的挑战|利用

> 原文：<https://www.harness.io/blog/cloud-billing>

云计费 API 应该为我们提供洞察力。虽然你的云账单只是变成了 JSON。云成本管理可以解决这个难题！

在我们的公共云之旅中，我们有能力快速、有计划地增加资源。尽管这些资源肯定不是免费的。由于公共云为我们的应用基础架构带来了极大的灵活性，理论上，获得我们工作负载的清晰简明的计费详细信息应该很容易。尽管当我们深入研究我们的账单时，特别是在多种工作负载和团队的情况下，我们开始看不到确切的工作负载成本。

各大公有云厂商都有云计费 APIs 尽管如果以可视化格式查看您的账单，JSON 格式的账单可能不会为您提供更多见解。了解您的支出的第一项是查看您的账单，根据组织的不同，账单细节可能会对您隐藏。

### 从哪里获取您的云账单

主要的云提供商都为您提供某种计费仪表板。

您的账单/总账单可能会按照某种层级进行计算。微软 Azure [计费管理文档](https://docs.microsoft.com/en-us/azure/cost-management-billing/manage/view-all-accounts)提供了关于如何对多个账户进行计费并将其汇总到一个层次结构中的见解。

尽管我们可以看一个更简单的例子。下面是我的 AWS 法案后，运行我们的虚拟线束大学之一。我可以估计，每次我们运行该类时，我们会在云上花费大约 200 美元。

我如何计算成本是相当初级的；看一下前一天和后一天的花费，希望我的账户里没有其他人有变化。云账单很像你的信用卡账单，它们告诉你发生了什么，但不告诉你事情发生的原因。“发生了什么”这一事实的挑战是具有挑战性的，尤其是如果你没有很好地掌握所有移动部件和计费复杂性，这是你的云账单难以理解的一个原因。

### 为什么云账单很难理解？

在寻求改变时，我在 2014 年初面试了亚马逊网络服务的解决方案架构师[我最终去了红帽]。当时面试过程的一部分是为一家假公司创建一个样本架构和堆栈。你得到了一张 AWS 凭证，用来创建、运行和解释你所创建的东西，这样就不会产生费用。

为了不惹怒我现在的雇主，我创建了自己的 AWS 账户。由于对 AWS 不熟悉，我能够创建一个新颖的架构，并演示为什么我做出了某些选择。尽管我对 AWS 的账单并不熟悉，当时我的公司并没有向工程师透露，我的个人账户也是我第一次与账单打交道。

当面试过程结束时，我想我关闭了所有的服务，终止了所有的基础设施。几个月过去了，我的优惠券用完了，然后小额账单开始打到我的信用卡上。我每月要支付几美元，当我登录控制台时，没有任何服务出现。

我用 AWS 开了一张票，那进入了以太网，所以它是由我自己来决定这或完全终止我的帐户。这个账单对我来说相当神秘。最终我被收费的是我的弹性 IP，它被旋转起来，但没有随着我为采访创建的一个项目被删除。几个月后，我不知所措，正要与美国运通公司争论信用卡费用。

当你开始使用新的令人兴奋的服务时，你会收到这些服务的账单。有时，当您终止服务时，所有项目都不会被清除。天下没有免费的午餐，下图提醒你所付出的一切。

提供计费维度的一种方法是查询您的云提供商计费 API，因为计费 API 应该是计费可视化所基于的。

### AWS 开单 API 查询示例

当然，随着更多的工作负载被放在公共云上，过去几年已经有所改进，因此需要更强大的计费 API。AWS 提供了一个如何利用 [AWS Cost Explorer](https://docs.aws.amazon.com/aws-cost-management/latest/APIReference/API_GetCostAndUsage.html#API_GetCostAndUsage_Examples) 制定查询的例子。由于需要安全签名，利用 AWS 命令行界面[CLI]或 SDK 将是一种更直接的查询方式。CLI 或 SDK 之外的 HTTP 方法需要[制定一个签名](https://docs.aws.amazon.com/general/latest/gr/sigv4_signing.html)，这个签名需要一个规范请求的散列，例如 CLI 和 SDK 在幕后为你做什么。

还记得上一节缺少免费的午餐吗？查询 AWS Cost Explorer API】会产生成本,在我写这篇博客的时候是每个 API 调用 0.01 美元。当构建您的查询时，需要确保您得到了您需要的内容，例如，粒度，或者如果您计划在调用后进行处理，可能会非常宏观。

开始使用 [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) 非常简单。在我的 macOS 上，我用一个简单的“ *brew install awscli* ”来利用[家酿](https://formulae.brew.sh/formula/awscli)进行安装。

安装完成后，您可以运行" *aws configure* "然后输入您的访问和密钥，如果您想要您的默认 aws 区域。输入这些项目后，您现在就可以使用 AWS CLI 开始比赛了。

通过“ *aws ce <表达式>* ”调用 [AWS 成本管理器命令](https://docs.aws.amazon.com/cli/latest/reference/ce/get-cost-and-usage.html)。制定成本浏览器查询可以遵循以下格式，并利用 filters.json 获得更具体的见解。

###### AWS ce get-cost-and-usage \
-time-period Start = 2020-06-01，End = 2020-06-30 \
-granularity MONTHLY \
-metrics "混合成本" "非混合成本" "使用数量"
-group-by Type = DIMENSION，Key=SERVICE Type=TAG，Key = Environment \
-filters file://filters。JSON

###### 使用 filters.json

###### {
" Dimensions ":{
" Key ":" SERVICE "，
"Values": [
"为 Kubernetes 提供的亚马逊弹性容器服务"
]
}
}

对我自己来说，具有挑战性的部分是确保我拥有作为维度的适当的 AWS 服务和潜在的相应基础设施/服务。我想查询 EKS 支出，其中包括对底层 EC2 基础设施的收费，该查询并不代表更现实的支出。您可以开始看到挑战从哪里开始增加，特别是需要一种无需复杂标记即可关联支出的方法。这正是我们旨在通过云成本管理解决的问题。

### 云成本管理——更好的方式

回到 Harness 大学的例子，减少支出的最直接方法是减少您需要的基础架构数量。说起来容易做起来难；如果我们超过利用率阈值，我们会收到警报，工作节点池的自动缩放组会触发，但如果我们未充分利用，则会触发相反的警报，而不是发出唧唧声，从而让过度配置继续存在。

为了查看我们集群上的压力，我利用了 [Kubernetes Metric Server](https://docs.aws.amazon.com/eks/latest/userguide/metrics-server.html) 并运行“ *kubectl top nodes* ”来查看集群中的压力。我在这件事上被过度部署了。

尝试将使用中的资源与应用程序支出相关联是很困难的，因为将这些指标汇总和关联在一起很困难。进入[云成本管理](https://harness.io/platform/cloud-cost-management/)，我能够在一个地方看到使用和支出信息。

通过缩小粒度，云成本管理使我们能够获得跨众多应用/集群的企业级支出控制面板。

Harness 平台的功能不断增强，不仅是一个持续交付平台，还是一个可以帮助降低云成本的平台。

### 与马具搭档

Harness 已经准备好与您合作，帮助您进一步实现持续交付和云成本效益目标。将 Harness 作为您的部署的记录系统有助于关联这些部署/运行的应用程序，并有助于跟踪这些应用程序的成本。在我的例子中，您可以简单地[连接 AWS](https://developer.harness.io/docs/cloud-cost-management/onboard-with-cloud-cost-management/set-up-cloud-cost-management/set-up-cost-visibility-for-aws/) 来利用云成本管理，从而快速获得深刻的见解，而不是钻取 AWS 成本浏览器。欢迎[今天就注册](https://harness.io/try-continuous-delivery-as-a-service-for-free/)一个线束账户，[给我们喊一声](https://harness.io/contact/)来看看云成本管理。

干杯，

拉维