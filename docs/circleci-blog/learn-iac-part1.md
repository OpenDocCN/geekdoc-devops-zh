# 基础设施代码第 1 部分:用 Terraform | CircleCI 创建一个 Kubernetes 集群

> 原文：<https://circleci.com/blog/learn-iac-part1/>

本系列向您展示如何开始使用基础设施即代码(IaC)。目标是通过教程和代码示例帮助开发人员加深对 IaC 的理解。

基础设施即代码(IaC)是现代[持续集成](https://circleci.com/continuous-integration/)管道不可或缺的一部分。它是使用机器可读的定义文件管理和调配云和 It 资源的过程。IaC 通过在代码中静态定义和声明计算资源，为组织提供了创建、管理和销毁这些资源的工具。

以下是本系列将涵盖的主题:

在本帖中，我将讨论如何使用 [HashiCorp 的 Terraform](https://www.terraform.io/) 来供应、部署和销毁基础设施资源。在我们开始之前，您需要在目标云提供商和服务中创建帐户，如[谷歌云](https://cloud.google.com/free)和 [Terraform 云](https://app.terraform.io/signup/account)。然后你可以开始学习如何使用 Terraform 创建一个新的 [Google Kubernetes 引擎(GKE)集群](https://cloud.google.com/kubernetes-engine)

## 先决条件

在开始之前，您需要准备好这些东西:

这篇文章使用了[这个回购](https://github.com/datapunkz/learn_iac)的`part01`文件夹中的代码。不过首先，你需要创建 GCP 凭证，然后 Terraform。

## 创建 GCP 项目凭据

GCP 凭证将允许您使用 IaC 工具执行管理操作。要创建它们:

1.  转到[创建服务帐户密钥](https://console.cloud.google.com/apis/credentials/serviceaccountkey)页面
2.  选择默认服务帐户或创建一个新帐户
3.  选择 JSON 作为键类型
4.  点击**创建**
5.  将这个 JSON 文件保存在`~/.config/gcloud/`目录中(可以重命名)

## HashiCorp Terraform 是如何工作的？

HashiCorp Terraform 是一款开源工具，用于安全高效地构建、更改和管理基础设施。Terraform 可以管理现有的服务提供商以及定制的内部解决方案。

Terraform 使用配置文件来描述运行单个应用程序或整个数据中心所需的组件。它生成一个执行计划，描述它将做什么来达到期望的状态，然后执行它来构建该计划所描述的基础结构。随着配置的变化，Terraform 会确定发生了哪些变化，并创建可用于更新基础架构资源的增量执行计划。

Terraform 用于创建、管理和更新基础设施资源，如物理机、虚拟机、网络交换机、容器等。Terraform 可以管理的组件包括低级基础架构组件，如计算实例、存储和网络，以及高级组件，如 DNS 条目和 SaaS 功能。

几乎任何基础设施类型都可以在 Terraform 中表示为资源。

### 什么是 Terraform 提供商？

提供者负责理解 API 交互和公开资源。提供商可以是 IaaS(阿里云、AWS、GCP、微软 Azure、OpenStack)、PaaS(比如 Heroku)或者 SaaS 服务(Terraform Cloud、DNSimple、Cloudflare)。

在这一步中，我们将使用 Terraform 代码在 [GCP](https://cloud.google.com/free) 提供一些资源。我们想编写 Terraform 代码，定义并创建一个新的 [GKE 集群](https://cloud.google.com/kubernetes-engine)，我们可以在本系列的第 2 部分中使用它。

为了创建一个新的 [GKE 集群](https://cloud.google.com/kubernetes-engine)，我们需要依靠 [GCP 提供商](https://www.terraform.io/docs/providers/google/index.html)与 GCP 进行交互。一旦定义和配置了提供者，我们就可以在 GCP 上构建和控制地形[资源](https://www.terraform.io/docs/configuration/resources.html)。

### 什么是 Terraform 资源？

[资源](https://www.terraform.io/docs/configuration/resources.html)是地形语言中最重要的元素。每个资源块描述一个或多个基础设施对象。基础设施对象可以是虚拟网络、计算实例或更高级别的组件，如 DNS 记录。一个资源块声明了一个给定类型(`google_container_cluster`)的资源，它有一个给定的本地名称，比如“web”。该名称用于从同一 Terraform 模块的其他地方引用该资源，但它在模块范围之外没有任何意义。

## 理解 Terraform 代码

既然您对 Terraform [提供者](https://www.terraform.io/docs/providers/google/index.html)和[资源](https://www.terraform.io/docs/configuration/resources.html)有了更好的理解，那么是时候开始深入研究代码了。Terraform 代码保存在目录中。因为我们使用的是 [CLI 工具](https://www.terraform.io/docs/cli-index.html)，所以您必须从代码所在的根目录中执行命令。对于本教程，我们使用的 Terraform 代码位于`part01/iac_gke_cluster`文件夹[这里](https://github.com/datapunkz/learn_iac)。该目录包含这些文件:

*   providers.tf
*   变量. tf
*   main.tf
*   输出. tf

这些文件代表了我们将要创建的 GCP 资源基础结构。这就是地形的过程。您可以将所有的 Terraform 代码放在一个文件中，但是一旦语法变得越来越多，管理起来就会变得越来越困难。大多数 Terraform 开发人员为每个元素创建一个单独的文件。这里是每个文件的快速分解，并讨论每个文件的关键元素。

### 细分:providers.tf

`provider.tf`文件是我们定义将要使用的云提供商的地方。我们将使用 [google_container_cluster 提供者](https://www.terraform.io/docs/providers/google/index.html)。这是`provider.tf`文件的内容:

```
provider "google" {
  # version     = "2.7.0"
  credentials = file(var.credentials)
  project     = var.project
  region      = var.region
} 
```

该代码块在闭包`{ }`块中有参数。`credentials`块指定您之前创建的 GCP 凭证的 JSON 文件的文件路径。注意，参数值以`var`为前缀。`var`前缀定义了 [Terraform 输入变量](https://www.terraform.io/docs/configuration/variables.html)的用法，用作 Terraform 模块的参数。这允许在不改变模块自己的源代码的情况下定制模块的各个方面，并且允许在不同的配置之间共享模块。在配置的根模块中声明变量时，可以使用 CLI 选项和环境变量来设置它们的值。当您在子模块中声明它们时，调用模块将在模块块中传递值。

### 细分:变量. tf

`variables.tf`文件指定了这个 Terraform 项目使用的所有输入变量。

```
variable "project" {
  default = "cicd-workshops"
}

variable "region" {
  default = "us-east1"
}

variable "zone" {
  default = "us-east1-d"
}

variable "cluster" {
  default = "cicd-workshops"
}

variable "credentials" {
  default = "~/.ssh/cicd_demo_gcp_creds.json"
}

variable "kubernetes_min_ver" {
  default = "latest"
}

variable "kubernetes_max_ver" {
  default = "latest"
} 
```

该文件中定义的变量将在整个项目中使用。所有这些变量都有`default`值，但是这些值可以在执行 Terraform 代码时通过 CLI 定义来更改。这些变量为代码增加了非常必要的灵活性，并使重用有价值的代码成为可能。

### 细分:main.tf

`main.tf`文件定义了我们的大部分 GKE 集群参数。

```
terraform {
  required_version = "~>0.12"
  backend "remote" {
    organization = "datapunks"
    workspaces {
      name = "iac_gke_cluster"
    }
  }
}

resource "google_container_cluster" "primary" {
  name               = var.cluster
  location           = var.zone
  initial_node_count = 3

  master_auth {
    username = ""
    password = ""

    client_certificate_config {
      issue_client_certificate = false
    }
  }

  node_config {
    machine_type = var.machine_type
    oauth_scopes = [
      "https://www.googleapis.com/auth/logging.write",
      "https://www.googleapis.com/auth/monitoring",
    ]

    metadata = {
      disable-legacy-endpoints = "true"
    }

    labels = {
      app = var.app_name
    }

    tags = ["app", var.app_name]
  }

  timeouts {
    create = "30m"
    update = "40m"
  }
} 
```

以下是对从`terraform`块开始的`main.tf`文件的每个元素的描述。该块指定了[地形后端](https://www.terraform.io/docs/backends/index.html)的类型。Terraform 中的一个“后端”决定了如何加载状态，以及如何执行一个操作，如`apply`。这种抽象支持非本地文件状态存储和远程执行。在这个代码块中，我们使用的是使用 Terraform Cloud 的`remote`后端，它连接到您在先决条件部分创建的`iac_gke_cluster`工作区。

```
terraform {
  required_version = "~>0.12"
  backend "remote" {
    organization = "datapunks"
    workspaces {
      name = "iac_gke_cluster"
    }
  }
} 
```

下一个代码块定义了我们将要创建的 [GKE 集群](https://cloud.google.com/kubernetes-engine)。我们还使用了`variables.tf`中定义的一些变量。`resource`块有许多用于在 GCP 上供应和配置 GKE 集群的参数。这里的重要参数是`name`、`location`和`Initial_node_count`，它们指定了将组成这个新集群的计算资源或虚拟机的初始总数。我们将从该集群的三个计算节点开始。

```
resource "google_container_cluster" "primary" {
  name               = var.cluster
  location           = var.zone
  initial_node_count = 3

  master_auth {
    username = ""
    password = ""

    client_certificate_config {
      issue_client_certificate = false
    }
  }

  node_config {
    machine_type = var.machine_type
    oauth_scopes = [
      "https://www.googleapis.com/auth/logging.write",
      "https://www.googleapis.com/auth/monitoring",
    ]

    metadata = {
      disable-legacy-endpoints = "true"
    }

    labels = {
      app = var.app_name
    }

    tags = ["app", var.app_name]
  }

  timeouts {
    create = "30m"
    update = "40m"
  }
} 
```

### 细分:output.tf

Terraform 使用一种叫做[的输出值](https://docs.docker.com/get-docker/)。这些函数返回 Terraform 模块的值，并为子模块提供输出。子模块输出向父模块公开其资源属性的子集，或者在运行`terraform apply`后在 CLI 输出中打印某些值。以下代码示例中显示的`output.tf`块输出值，以读出集群名称、集群端点以及敏感数据等值，这些值由`sensitive`参数指定。

```
 output "cluster" {
  value = google_container_cluster.primary.name
}

output "host" {
  value     = google_container_cluster.primary.endpoint
  sensitive = true
}

output "cluster_ca_certificate" {
  value     = base64decode(google_container_cluster.primary.master_auth.0.cluster_ca_certificate)
  sensitive = true
}

output "username" {
  value     = google_container_cluster.primary.master_auth.0.username
  sensitive = true
}

output "password" {
  value     = google_container_cluster.primary.master_auth.0.password
  sensitive = true
} 
```

## 初始化地形

现在我们已经介绍了 Terraform 项目和语法，您可以开始使用 Terraform 配置 GKE 集群了。将目录切换到`part01/iac_gke_cluster`文件夹:

```
cd part01/iac_gke_cluster 
```

在`part01/iac_gke_cluster`中，运行以下命令:

```
terraform init 
```

您的输出应该如下所示:

```
root@d9ce721293e2:~/project/terraform/gcp/compute# terraform init

Initializing the backend...

Initializing provider plugins...
- Checking for available provider plugins...
- Downloading plugin for provider "google" (hashicorp/google) 3.10.0...

* provider.google: version = "~> 3.10"

Terraform has been successfully initialized! 
```

### 使用地形预览

Terraform 有一个命令，允许您在不实际执行任何操作的情况下，模拟运行和验证您的 Terraform 代码。这个命令叫做`terraform plan`。该命令还显示了 Terraform 将针对您现有的基础设施执行的所有操作和更改。在终端中，运行:

```
terraform plan 
```

输出:

```
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # google_container_cluster.primary will be created
  + resource "google_container_cluster" "primary" {
      + additional_zones            = (known after apply)
      + cluster_ipv4_cidr           = (known after apply)
      + default_max_pods_per_node   = (known after apply)
      + enable_binary_authorization = false
      + enable_intranode_visibility = (known after apply)
      + enable_kubernetes_alpha     = false
      + enable_legacy_abac          = false
      + enable_shielded_nodes       = false
      + enable_tpu                  = (known after apply)
      + endpoint                    = (known after apply)
      + id                          = (known after apply)
      + initial_node_count          = 3
      + instance_group_urls         = (known after apply)
      + label_fingerprint           = (known after apply)
      + location                    = "us-east1-d"
  }....
Plan: 1 to add, 0 to change, 0 to destroy. 
```

Terraform 将根据`main.tf`文件中的代码为您创建新的 GCP 资源。

### 地形应用

现在，您可以创建新的基础设施并部署应用程序。在终端中运行以下命令:

```
terraform apply 
```

Terraform 将提示您确认您的命令。键入`yes`并按回车键。

```
Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes 
```

Terraform 将在 GCP 建立新的 GKE 集群。

**注意** : *集群需要 3-5 分钟才能完成。这不是一个即时的过程，因为后端系统正在进行资源调配并使其上线。*

在我的集群完成之后，这是我的输出:

```
Apply complete! Resources: 1 added, 0 changed, 0 destroyed.

Outputs:

cluster = cicd-workshops
cluster_ca_certificate = <sensitive>
host = <sensitive>
password = <sensitive>
username = <sensitive> 
```

新的 GKE 集群已经创建，并显示了`Outputs`结果。请注意，标记为敏感的输出值在结果中用`<sensitive>`标记屏蔽了。这确保敏感数据受到保护，但在需要时可用。

### 使用地形摧毁

现在您已经证明您的 GKE 集群已经成功创建，运行`terraform destroy`命令来销毁您在本教程中创建的资产。你可以让它继续运行，但是要知道，在 GCP 上运行任何资产都是有成本的，你要对这些成本负责。谷歌为其免费试用注册提供了慷慨的 300 美元信用，但如果你让资产运行，你可以很容易地吃掉它。这取决于你，但是运行`terraform destroy`将会终止任何正在运行的资产。

运行以下命令销毁 GKE 集群:

```
terraform destroy 
```

## 结论

恭喜你！您刚刚完成了本系列的第 1 部分，并通过使用 IaC 和 [Terraform](https://www.terraform.io/) 将 Kubernetes 集群配置和部署到 GCP，提升了您的体验。

继续本教程的[第 2 部分](https://circleci.com/blog/learn-iac-part02/)。在第 2 部分中，您将学习如何为应用程序构建 Docker 映像，将该映像推送到存储库，然后使用 Terraform 将该映像作为容器部署到使用 Terraform 的 GKE。

这里有一些资源可以帮助你扩大知识面: