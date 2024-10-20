# Terraform 201:它是什么，教程和更多|线束

> 原文：<https://www.harness.io/blog/terraform-201-tutorial>

在本文中，我们将介绍 HashiCorp Terraform，这是一个基础设施即代码(IAC)工具，它加速了云计算世界中开发人员和工程团队的工作。

在本文中，我们将介绍 HashiCorp Terraform，这是一个[基础设施即代码(IAC)](https://harness.io/blog/devops/infrastructure-as-code/) 工具，它加速了云计算世界中开发人员和工程团队的工作。与 AWS CloudFormation 一样，HashiCorp Terraform 使您能够使用代码定义基础架构的理想状态，并将这些更改部署到您的 AWS 帐户、Google Cloud 项目和其他解决方案中(稍后将详细介绍)。让我们深入一些核心用例、概念和一些需要注意的警告。

## 地形教程

### 地形状态

首先，让我们谈谈 Terraform 如何跟踪它所管理的基础设施。有人可能会问，“Terraform 如何知道需要做出哪些改变？”

通常情况下，亚马逊 S3 或谷歌云存储桶被用作 Terraform 所管理内容的真实来源。对于大多数项目，不建议使用本地状态，因为您的基础结构状态记录可能会在磁盘故障期间丢失。

如果您选择使用亚马逊 S3，强烈建议在您指定为“远程状态”的存储桶上启用 S3 对象版本控制，以防将来出现任何问题。

Terraform 命令，如` **terraform plan** `、` **terraform apply** `，以及伤脑筋的` **terraform destroy** `将利用远程状态下记录的内容，同时 terraform 将理想状态(您编写的 terraform 代码)与现实进行协调，从您的代码正在与之交互的云提供商或 SaaS API 的角度来看。

### Terraform 核心概念

既然我们已经从较高的层次上介绍了 Terraform 远程状态，那么让我们来讨论一下存在于其中的资源的类型，以及当他们对基础设施代码进行更改时，如何操作状态。

#### Terraform 文件扩展名

以下是一些原生 Terraform 文件扩展名的简要介绍:

***。tf**
正如您将在下面看到的，扩展名为. tf 的文件被处理为“基础设施声明”文件。Terraform 对这些文件的命名相当灵活，尽管建议遵循 HashiCorp 的[标准模块文档](https://www.terraform.io/docs/language/modules/develop/structure.html)。这样做将确保新工程师尽快加入您的 IAC Git 库。

***。tfvars**
遵循标准的模块文档约定，根模块的变量在 variables.tf 中定义，它作为模块的默认值。假设默认情况下，您希望 RDS 备份保留时间设置为 7 天，但是在生产环境中，您希望保留 14 天。

下面是 variables.tf 对象定义变量的样子:

变量" web app _ rds _ backup _ retention _ days " {
description = "保留 RDS 备份的天数"
type = Number
default = 7
}

要覆盖该值，您需要创建一个 production.tfvars 或类似名称的文件，并添加一行，如下所示:

webapp _ rds _ 备份 _ 保留期 _ 天数=14

***.tf.json** 虽然 Terraform 有自己的本地语言(HCL)，但它也支持基于 JSON 的配置。

***。tpl**
不赘述，tpl 文件是模板文件，可以由 Terraform 以优雅且可重用的方式进行插值。Terraform 提供了遍历所提供的 list()并动态生成 IAM 策略资源块等功能。如果你感兴趣的话，这里有一些关于这个主题的进一步阅读材料。

#### 部署更改

要在当前工作目录中部署(或应用)更改，我们可以使用如下命令。根据您是否正在考虑使用 Terraform 工作空间或环境，您可以将 tfvars 文件作为` **terraform apply** '的一部分传入。

#拉出任何模块并连接到后端远程状态
terraform init

#可选，因为 terraform 首先应用计划并提示批准
terraform 计划

#协调现实 vs 状态 vs cwd 代码
terraform 应用

值得注意的是，terraform init 通常需要在第一次在目录中应用之前调用，或者如果对根模块所依赖的模块进行了修改。

如果您的团队有多名工程师将为您的 Terraform 代码库做出贡献，请考虑使用 Atlantis 或 HashiCorp 的 Terraform Cloud 等解决方案，以一致且优雅的方式协调您的基础架构部署。

#### 捕捉语法错误和格式

当用其他语言编码时，你有多少次在 CI 中推送代码却看到林挺检查失败？对我来说太多次了！

嗯，有了 Terraform，我们可以在本地对我们的代码进行一些抽查，以确保它处于 CI 的良好状态。以下是您可以运行的几个命令:

1.  格式化您的代码
2.  捕捉语法错误
3.  验证我们的假设

#缩进和完整性检查代码
#写入 STDOUT 的任何文件名都已格式化
terraform fmt

#验证 cwd 中的 terraform 语法
terraform 验证

#### 提供者/扩展性

你以前可能听说过有人使用 Terraform 来管理他们的 AWS、Google Cloud 和 Azure 基础设施。这可以立即引发这样的想法:“他们如何及时跟上所有最新的 API 更新？”

名为 Terraform 的出色的基础设施代码工具的基础是无数的子组件，它们处理与每个特定云提供商的交互。HashiCorp 称这些为 Terraform 供应商。

重点介绍一些常用的流行提供程序，以及供您阅读的文档链接:

AWS- [https://registry。terra 形态。io/providers/hashi corp/AWS/latest/docs](https://registry.terraform.io/providers/hashicorp/aws/latest/docs)
Google-[https://registry。terra 形态。io/providers/hashi corp/Google/latest/docs](https://registry.terraform.io/providers/hashicorp/google/latest/docs)azure RM-[https://registry。terra 形态。io/提供商/哈希公司/azure RM/最新](https://registry.terraform.io/providers/hashicorp/azurerm/latest)

为了开始在 AWS 帐户中创建资源，您需要在 providers.tf 中添加一个提供者块，如下所示:

提供商" AWS " {
version = " ~>2 . 54 . 0 "
region = " us-east-2 "
}

在上面的例子中，Terraform 和 AWS SDK 将通过通常的 AWS 凭证提供者链过程来查找 AWS 凭证以验证请求。在这种情况下，我们退回到使用~/中的[default]配置文件。AWS/凭据。

如果您需要将您的资源部署到多个客户或地区，我们可以为提供商起别名，并挑选出我们希望在这些配置环境中部署的特定 Terraform 资源。这里一个很好的用例是在另一个 AWS 帐户托管的 Route53 区域中创建记录。

#### 数据源——想想“读者”

数据源的概念非常有趣，也是一个经常争论的话题。这里增加的基本价值是，您可以让 Terraform“查询”您选择的特定资源的提供者。

在完美的世界中，你的 AWS、GCP 或 Azure 云环境中的每一个资源都将在一个中心位置进行管理，即在 Terraform 中。

事实上，在大多数情况下，老牌公司都会实施一种混合基础架构管理模式。例如，您的 AWS 帐户中的 VPC 可能是手动或通过 CloudFormation 创建的，但您希望使用 Terraform 将资源部署到所述 VPC 中。

记住这个 VPC 场景，在 Terraform 中，您可能需要为特定 VPC 内的子网标识专用子网 id。当然，您可以添加一个 Terraform 输入变量，并在 prod.tfvars 中手动提供覆盖列表，但是这个新时代完全是关于动态基础设施的！

为了完成动态查找，我们可以像这样使用“ **aws_subnet_ids** ”数据源，并简单地传入 VPC 的 ID 和一个明确标识私有子网的标记。确定您所考虑的数据源的文档范围是很重要的，因为筛选字段有一些限制。下面是上述查找在 HashiCorp 配置语言中的样子:

变量" vpc_id" {}

数据" AWS _ subnet _ ids " " private " {
VPC _ id = var . VPC _ id

tags = {
subnet _ type = " private "
}
}

关于这个数据源以及其他数据源的更多信息，请查看 HashiCorp 最新的[文档](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/subnet_ids)。

#### 资源——思考“创造者”

资源，资源，资源——这很容易是 Terraform 的面包和黄油，你将最大程度地利用它们。

想要在您的 AWS 帐户中创建新的 EC2 实例吗？这将与 [aws_instance 资源](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)一起使用，就像这样:

资源" AWS _ instance " " web " {
ami = " ami-12 E4 cf D5 "
instance _ type = " T2。微"
tags = {
Name = "基本 EC 2 实例示例"
}
}

想在 Datadog 中创建新的监视器吗？使用 Datadog Terraform 提供程序，您可以将`[**Datadog _ monitor**](https://registry.terraform.io/providers/DataDog/datadog/latest/docs/resources/monitor)`资源部署到您的 Datadog 帐户中。

想要在您的开发或生产租户中创建新的 Auth0 应用程序吗？没问题，有 Terraform 提供者和相应的资源支持！

我认为 Terraform 的可集成性是最大的卖点之一。它不仅仅是一个基础设施代码解决方案；它能够管理大量 SaaS/PaaS 解决方案中的资源！通常，这些外部系统和我们托管应用程序的基础设施之间存在依赖关系。

#### 模块-地形分类

现在我们已经介绍了模块的构建块:数据源、资源和输入变量，让我们来谈谈如何使用 Terraform 模块保持我们的 Terraform 代码干燥和可重用！

首先，值得指出的是，在 Terraform 0.13 之前，模块有一些特别令人遗憾的限制。没有可用于模块的计数参数。作为副作用，您会在 Github 等上发现大量的模块，这些模块实现了大量的“enablement”变量来控制模块中每个单独资源的创建。我过去称这些为“管道”变量，因为我们从调用代码中传递它们(你想在那里利用模块)。这使得使用模块变得很麻烦，因为您需要大量的变量来控制基本的创建行为。

好吧，如果你刚刚开始使用 Terraform，我很高兴地宣布，情况不再是这样了！

人们可以把 Terraform 模块看作是 Terraform 的类。假设您经常将一个特定的架构部署到您的 AWS 环境中，它由一个 RDS 实例和 Route53 记录组成。我们可以将所有内容封装到一个模块中，而不是在您想要部署的每个 Terraform 根模块中单独调用每个资源。和通常的根模块一样，我们定义 Terraform 输入变量、提供者及其版本需求、资源，甚至数据源！

您可能会想，“这听起来很酷，但是我必须自己编写所有的自定义内容吗？”

幸运的是，像大多数开源社区一样，有大量的预构建模块。将模块库从 GitHub 分支到您的企业组织中可能是有意义的，但是还有另一个选择:Terraform registry。

在你创建一个复杂的模块之前，仔细阅读 Terraform 注册中心的社区支持的预建模块是值得的。如果您确实创建了一个定制模块，请考虑将其分解到一个定制的 g it 存储库中，或者至少分解到一个具有适当划分的目录结构的通用“terraform-modules”存储库中。

版本控制你的 Terraform 代码和模块(真的有什么区别吗？)对于使我们能够快速行动并能够在事情不按计划进行时审计变更是至关重要的。我强烈推荐使用[语义版本控制](https://semver.org/)来保持事情正常。关于这个主题，最后一个要分享的想法是(这有时会在软件开发中引起共鸣):考虑其他可能想要使用你的模块的工程师。避免在小版本或补丁版本中进行破坏性更改。 [Terraform-docs](https://github.com/terraform-docs/terraform-docs) 还可以通过自动更新模块中的 README.md 来帮助维护，作为 CI 的一部分或预提交挂钩。

#### 函数-基本库函数

正如大多数其他语言一样，Terraform 提供了一个标准函数库，您可以使用它对输入变量和任何其他需要传递的数据进行修改。在本文中，我们将介绍一些日常可能会用到的常用函数。

下面是一个使用 format()函数连接 KMS 键描述的示例:

locals {
key _ description = format("用于%s 环境的加密密钥"，var.environment)
}

变量"环境" {
description = "环境的名称。"
type = string
}

resource " AWS _ kms _ key " " fintech " {
description = local . key _ description
}

您可能遇到的另一个常见场景是 JSON 被期望作为资源的参数。让我们看看如何为 SQS 队列适当地编码重驱动策略，因为那里需要 JSON。

资源" AWS _ SQS _ queue " " my-queue " {
name = " example-TF-queue "
message _ retention _ seconds = 86400
receive _ wait _ time _ seconds = 10
redrive _ policy = jsonencode({
deadLetterTargetArn = var . dlq _ arn
maxReceiveCount = 5
})
}

有关使用 Terraform 提供的内置函数的更多信息，请查看 Hashicorp 的[函数文档](https://www.terraform.io/docs/language/functions/index.html)。我强烈推荐查看一下[的 IP 网络功能](https://www.terraform.io/docs/language/functions/cidrsubnet.html)，当动态分割你的 VPC 子网中的 IP 地址时，它真的会派上用场。

#### 输出

有时，将所有资源放在一个单一的 Terraform 根模块中会导致 terraform 应用过于缓慢，因此许多工程团队逻辑上将他们的 Terraform 代码划分到多个目录中。考虑 RDS 实例和 ECS 服务任务定义的例子，其中包括环境变量。你可能希望这些彼此隔离，因此，这就是 Terraform 输出发挥作用的地方。

输出可以让你...从一个 Terraform 状态输出数据，然后将其导入另一个状态。这对于 RDS 主机名、VPC 对象等等来说非常方便。较新版本的 Terraform 现在允许您导出整个模块，而不仅仅是原始数据类型、列表、地图等。

### 结论

如您所见，Terraform 提供了丰富的功能来支持部署、维护和与您的基础设施交互。仔细考虑如何将资源分组到不同的根模块中，你会发现 Terraform 非常容易使用和维护。试着让你的 Terraform 代码尽可能的简洁，并且在处理一大群贡献者的时候利用自动化来处理应用

这就结束了我们对 Terraform 的 201 研究。我们希望这对您有所帮助，如果您有任何问题或意见(或者甚至希望我们做一个 Terraform 301！)请伸手。不要忘记通过[今天预订您的演示](https://harness.io/demo)来查看 Harness 的强大 Terraform 集成。

马库斯