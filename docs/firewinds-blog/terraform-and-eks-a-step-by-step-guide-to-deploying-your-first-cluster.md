# Terraform 和 EKS:部署第一个集群的分步指南

> 原文：<https://www.fairwinds.com/blog/terraform-and-eks-a-step-by-step-guide-to-deploying-your-first-cluster>

 在一个由四部分组成的系列中，我们讨论了 Terraform 及其重要性，并通过[AK](/blog/getting-started-with-terraform-and-aks-a-step-by-step-guide-to-deploying-your-first-cluster)和 [GKE](/blog/how-to-use-terraform-with-gke-a-step-by-step-guide-to-deploying-your-first-cluster) 提供了 Terraform 的分步指南。现在我们穿越 EKS。

使用[基础设施作为代码](https://www.fairwinds.com/blog/why-infrastructure-as-code-kubernetes)来管理 Kubernetes 允许您在配置文件中声明基础设施组件，然后用于在各种云提供商中提供、调整和拆除基础设施。Terraform 是我们管理 Kubernetes 基础设施整个生命周期的首选工具。你可以在这里阅读关于 Terraform 的[好处](/blog/what-is-terraform-and-why-is-it-important)。

这篇博客提供了一步一步的指导，告诉你如何通过部署你的第一个以基础设施为代码的集群来开始使用 Terraform 和 EKS。

## 先决条件

*   具有管理员权限的 AWS 帐户
    *   创建访问密钥
    *   下载密钥文件
*   安装以下组件

## **步骤**

为项目创建一个类似 `terraform-eks.` 的目录接下来，用这个命令在目录中建立一个 ssh 密钥对: `ssh-keygen -t rsa -f ./eks-key.`

我们现在将设置几个 Terraform 文件来包含各种资源配置。第一个文件将被命名为 provider.tf。创建该文件并添加以下代码行:

```
provider "aws" {
  version = "~> 2.57.0"
  region  = "us-east-1"
} 
```

现在，创建一个名为 `cluster.tf.` 的文件，这将包括我们的虚拟网络、集群和节点池模块。首先，我们将添加一个局部变量块，其中包含一个可在不同模块中使用的集群名称变量:

```
locals {
  cluster_name = "my-eks-cluster"
} 
```

接下来，我们将使用 Fairwinds 的 AWS VPC 模块为集群设置网络。请注意，该模块被硬编码到一个/16 cidr 块和一个带有/21 cidr 块的子网。你可以在回购处了解更多关于该模块的信息:[https://github.com/FairwindsOps/terraform-vpc](https://github.com/FairwindsOps/terraform-vpc)。

```
module "vpc" {
  source = "git::https://git@github.com/reactiveops/terraform-vpc.git?ref=v5.0.1"

  aws_region = "us-east-1"
  az_count   = 3
  aws_azs    = "us-east-1a, us-east-1b, us-east-1c"

  global_tags = {
    "kubernetes.io/cluster/${local.cluster_name}" = "shared"
  }
} 
```

最后，我们将为集群本身添加模块。我们将实际使用一个社区支持的 Terraform AWS 模块:

```
module "eks" {
  source       = "git::https://github.com/terraform-aws-modules/terraform-aws-eks.git?ref=v12.1.0"
  cluster_name = local.cluster_name
  vpc_id       = module.vpc.aws_vpc_id
  subnets      = module.vpc.aws_subnet_private_prod_ids

  node_groups = {
    eks_nodes = {
      desired_capacity = 3
      max_capacity     = 3
      min_capaicty     = 3

      instance_type = "t2.small"
    }
  }

  manage_aws_auth = false
} 
```

一旦 `cluster.tf` 文件完成，通过运行 `terraform init.` 初始化 Terraform，Terraform 将生成一个名为 `.terraform` 的目录，并下载在`cluster.tf.`中声明的每个模块源。初始化将拉入这些模块所需的任何提供程序，在本例中，它将下载 aws 提供程序。如果进行了配置，Terraform 还将配置后端来存储状态文件。输出将如下所示:

```
$  terraform init
Initializing modules...

Initializing the backend...

Initializing provider plugins...
- Checking for available provider plugins...
- Downloading plugin for provider "local" (hashicorp/local) 1.4.0...
- Downloading plugin for provider "null" (hashicorp/null) 2.1.2...
- Downloading plugin for provider "template" (hashicorp/template) 2.1.2...
- Downloading plugin for provider "random" (hashicorp/random) 2.3.0...
- Downloading plugin for provider "kubernetes" (hashicorp/kubernetes) 1.11.3...

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary. 
```

成功初始化 Terraform 后，运行 `terraform plan` 查看将创建的内容:

```
$  terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.

module.eks.data.aws_partition.current: Refreshing state...
module.eks.data.aws_caller_identity.current: Refreshing state...
module.eks.data.aws_iam_policy_document.cluster_assume_role_policy: Refreshing state...
module.eks.data.aws_ami.eks_worker: Refreshing state...
module.eks.data.aws_ami.eks_worker_windows: Refreshing state...
module.eks.data.aws_iam_policy_document.workers_assume_role_policy: Refreshing state...

------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create
 <= read (data resources)

Terraform will perform the following actions:

  # module.eks.data.null_data_source.node_groups[0] will be read during apply
  # (config refers to values not yet known)
 <= data "null_data_source" "node_groups"  {
      + has_computed_default = (known after apply)
      + id                   = (known after apply)
      + inputs               = {
          + "aws_auth"        = ""
          + "cluster_name"    = "my-eks-cluster"
          + "role_CNI_Policy" = (known after apply)
          + "role_Container"  = (known after apply)
          + "role_NodePolicy" = (known after apply)
        }
      + outputs              = (known after apply)
      + random               = (known after apply)
    }

...

Plan: 59 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run. 
```

请注意，这段摘录已经过编辑，以缩减本文的篇幅。

如上例所示，Terraform 将添加 59 项资源，包括一个网络、子网(用于 pod 和服务)、EKS 集群和一个受管节点组。

计划通过验证后，通过运行 `terraform apply.` 进行最后一个验证步骤来应用更改，Terraform 将再次输出计划并在应用前提示确认。完成此步骤大约需要 15-20 分钟。

要与集群交互，请在终端中运行以下命令:

```
aws eks --region us-east-1 update-kubeconfig --name my-eks-cluster
```

接下来，运行 `kubectl get nodes` ，您将看到集群中的两个工作节点！

```
$  kubectl get nodes
NAME                           STATUS   ROLES    AGE     VERSION
ip-10-20-66-245.ec2.internal   Ready       3m16s   v1.16.8-eks-fd1ea7
ip-10-20-75-77.ec2.internal    Ready       3m16s   v1.16.8-eks-fd1ea7
ip-10-20-85-109.ec2.internal   Ready       3m15s   v1.16.8-eks-fd1ea7 
```

这就是我们关于使用 Terraform 在多个云提供商中设置 Kubernetes 集群的系列文章。如果不需要您的集群，请运行 terraform destroy。继续您的 Kubernetes 之旅，祝您愉快！

## 资源

[![Test Drive Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/c3aba2ed0914bca5d0eb3ed9e32fb0b8.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/6140ba18-2b30-4c40-a944-fa3c4961723f)