# 我如何使用基础设施作为代码？- Terraform 和 Pulumi | CircleCI

> 原文：<https://circleci.com/blog/how-do-i-iac/>

基础设施即代码(IaC)是通过机器可读的定义文件管理和供应云和 IT 资源的流程，是现代[持续集成](https://circleci.com/continuous-integration/)管道的一部分。IaC 使组织能够使用现代 DevOps 工具调配、管理和销毁计算资源。IaC 通过在代码中静态定义和声明这些资源，然后通过代码部署和动态维护这些资源来实现这一点。

在最近与开发人员的交流中，我意识到许多人要么不知道 IaC，要么知道但因为升级太麻烦而放弃了最初的努力。在这篇文章中，我将讨论 IaC 的好处和一些目前可用的工具选项，并简要演示如何使用它们，希望能够简化向使用 IaC 的过渡。我将重点介绍当今业界使用的两个最流行的 IaC 工具: [HashiCorp 的 Terraform](https://www.terraform.io/) 和[的 Pulumi](https://www.pulumi.com/) 。

## 先决条件

在你开始之前，你需要有这些东西:

## 创建 Google 云项目证书

您将需要创建 [Google Cloud 凭证](https://github.com/GoogleCloudPlatform/community/blob/master/tutorials/getting-started-on-gcp-with-terraform/index.md#getting-project-credentials)，以便使用 IaC 工具执行管理操作。

*   转到[创建服务帐户密钥页面](https://console.cloud.google.com/apis/credentials/serviceaccountkey)
*   选择默认服务帐户或创建一个新帐户，选择 JSON 作为密钥类型，然后单击**创建**
*   将这个 JSON 文件保存在`~/.config/gcloud/`目录中，并将文件重命名为`cicd_demo_gcp_creds.json`。这对**稍后在容器中启用谷歌云 CLI 非常重要**

## 创建 Pulumi API 令牌

您将需要获得一个 Pulumi API 令牌来将您的项目状态存储在 Pulumi 的后端云中。

*   前往[app.pulumi.com/](https://app.pulumi.com/)
*   点击页面左上方的**选择一个组织**并选择您的账户
*   点击**设置**
*   点击页面左侧的**访问令牌**
*   点击页面右侧的**新建访问令牌**
*   点击**创建**并保存创建的新 API 令牌。这将用于稍后初始化您的 Pulumi 项目

## Docker pull IaC101 Docker 图像

至此，所有先决条件都已完成，您已经准备好为本次研讨会提取 docker 映像了。在终端中键入以下命令:

```
docker pull ariv3ra/iac101 
```

现在您应该在本地拥有了`ariv3ra/iac101` docker 映像。您可以验证 buy running `docker images`，您应该会看到结果中列出的图像。

## 码头运行 IaC101 集装箱

现在我们将基于`ariv3ra/iac101`图像创建一个新的 Docker 容器，其中包含所有的 IaC 工具和代码。

### 安装/。配置/gcloud/目录

在运行新容器之前，您需要到您的`~/.config/gcloud/`目录的绝对路径。在我的 MacOS 机器上，我的绝对路径是`/Users/angel/.config/gcloud/`。确保获得本地机器上`gcloud`目录的绝对文件路径。

### 运行带有装载的 IaC101 容器

在终端运行中运行这个命令，但是一定要用本地`glocud/`目录的实际绝对路径替换`<your absolute path here>`位:

```
docker run -it --name iactest --mount type=bind,source=<your absolute path here>.config/gcloud/,target=/root/.config/gcloud/ ariv3ra/iac101 
```

### IaC101 容器正在运行

运行前面的`docker run`命令后，您的 IaC101 容器将启动并运行，您的终端 shell 将您放入运行容器中。从现在开始，您在终端 shell 中运行的每个命令都将在 Docker 容器中执行，直到您手动`exit`该容器。该容器安装了 Terraform 和 Pulumi CLI 工具，以及在各自工具中创建基础设施的示例代码。容器项目文件映射如下。

```
projects/
|_ terraform/gcp/compute/   # Contains the Terraform code files
|_ pulumi/gcp/compute/      # Contains the Pulumi code files 
```

## 哈希公司地形

HashiCorp Terraform 是一款开源工具，用于安全高效地构建、更改和管理基础设施。Terraform 可以管理现有的服务提供商以及定制的内部解决方案。

配置文件描述了在 Terraform 上运行单个应用程序或整个数据中心所需的组件。Terraform 生成一个执行计划，描述它将如何达到期望的状态，然后执行它来构建所描述的基础设施。随着配置的改变，Terraform 能够确定改变了什么，并创建可应用的增量执行计划。

Terraform 可以管理的基础设施包括低级组件，如计算实例、存储和网络，以及高级组件，如 DNS 条目、SaaS 功能等。

### 使用 Terraform 调配基础架构

让我们从使用 Terraform 代码在 GCP 提供一些资源开始。`terraform/gcp/compute/`中的`main.tf` 是定义我们的基础设施的代码。在该文件中，我们将创建一个新的计算实例，该实例将安装并运行打包在 Docker 容器中的 Python Flask 应用程序。Terraform 代码还将创建一些防火墙规则，允许公众通过端口 5000 访问应用程序。

要对此进行配置，请在终端中运行以下命令:

```
cd ~/project/terraform/gcp/compute/ 
```

### 初始化地形

在`~/project/terraform/gcp/compute/`中运行该命令:

```
terrform init 
```

您将看到类似于下面的结果。

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

Terraform 有一个命令，允许您在不实际执行任何操作的情况下，模拟运行和验证您的 Terraform 代码。该命令名为`terraform plan`,它还绘制了 Terraform 将针对您现有的基础设施执行的所有操作和更改。在终端运行中:

```
terraform plan 
```

您将看到类似这样的结果。

```
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # google_compute_firewall.http-5000 will be created
  + resource "google_compute_firewall" "http-5000" {
      + creation_timestamp = (known after apply)
      + destination_ranges = (known after apply)
  }

    # google_compute_instance.default will be created
  + resource "google_compute_instance" "default" {
      + can_ip_forward       = false
      + cpu_platform         = (known after apply)
      + deletion_protection  = false
      + guest_accelerator    = (known after apply)
      + id                   = (known after apply)
      + instance_id          = (known after apply)
      + label_fingerprint    = (known after apply)
      + labels               = {
          + "container-vm" = "cos-stable-69-10895-62-0"
        }
      + machine_type         = "g1-small"
  }
  Plan: 2 to add, 0 to change, 0 to destroy. 
```

如您所见，Terraform 将根据`main.tf`文件中的代码为您创建新的 GCP 资源。

### 地形应用

现在，您已经准备好创建新的基础设施并部署应用程序了。在终端中运行以下命令:

```
terraform apply 
```

Terraform 将提示您确认您的命令。键入 yes 并按回车键。

```
Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes 
```

Terraform 将构建您的基础设施，完成后不久，您将在 GCP 上安装并运行一个应用程序。请注意，Terraform 完成后，应用程序将需要 3-5 分钟才能上线。这不是一个即时的过程，因为后端系统正在进行供应和上线。

一旦完成，您将看到类似这样的结果。

```
Outputs:

Public_IP_Address = 34.74.75.17 
```

`Public_IP_Address`值是你的应用程序运行的 IP 地址，所以如果你打开浏览器，输入这个数字加上`:5000`端口地址(看起来像这个`34.74.75.17:5000`)，你会看到你的应用程序在 GCP 运行。

### 地形破坏

既然您已经证明了您的 Google Compute 实例和您的应用程序可以工作，那么运行`terraform destroy`命令来销毁您在本教程中创建的资产。你可以让它继续运行，但要知道，在谷歌云平台上运行任何资产都是有成本的，你要为这些成本负责。谷歌为其免费试用注册提供了慷慨的 300 美元信用，但如果你让资产运行，你可以很容易地吃掉它。由你决定，但是运行`terraform destroy`会终止任何运行的资产。

接下来，我将演示如何使用 [Pulumi](https://www.pulumi.com/) 执行基础设施供应。

## Pulumi SDK

Pulumi SDK 是一个开源框架，用于定义和部署云应用和 IaC。Pulumi 使开发人员能够用他们喜欢的语言编写代码，如 TypeScript、JavaScript、Python 和 Go。这实现了云应用和基础设施的现代方法，而无需学习另一种 YAML 或 DSL 方言。这开启了抽象和重用，以及对您喜欢的 ide、重构和测试工具的访问。掌握一个工具链和一套框架，轻松地转向任何云(AWS、Azure、GCP 或 Kubernetes)。

### 使用 Pulumi 配置基础架构

您已经体验了使用 Terraform 构建基础设施，现在您将学习如何使用 Pulumi 配置和部署新的基础设施。Pulumi 示例将使用 Pulumi 代码和工具创建相同的 GCP 资源。Pulumi 使您能够用编程语言定义基础设施，这提供了很大的灵活性。在这个例子中，我们使用 [Python](https://www.python.org/) 作为我们的语言。`~/project/pulumi/gcp/compute/`目录中的`__main.py__`文件是我们将要定义基础设施的地方。

确保您的 Pulumi API 令牌可用。在接下来的章节中，您会用到它。

Pulumi 示例可以在`~/project/pulumi/gcp/compute/`目录中找到。运行以下命令切换到 Pulumi 示例目录:

```
cd ~/project/pulumi/gcp/compute/ 
```

### 为 pulumi 安装依赖项

因为我们使用 Python 定义 IaC 规范，所以我们需要首先安装 Pulumi Python SDK 依赖项。在终端中运行以下命令:

```
pip install -r ../requirements.txt 
```

### 使用 Pulumi 预览

Pulumi 有一个名为`pulumi preview`的预演命令。此命令显示现有堆栈更新的预览，现有堆栈的状态由现有状态文件表示。新的期望状态是通过运行 Pulumi 程序并从其结果对象图中提取所有资源分配来计算的。然后，将这些分配与现有状态进行比较，以确定必须进行哪些操作才能达到期望的状态。实际上不会对堆栈进行任何更改。

在终端中运行以下命令:

```
pulumi preview 
```

运行该程序后，您将看到以下结果，提示您输入之前创建的 Pulumi API 令牌。将令牌粘贴到终端中。粘贴令牌后，点击 enter。

```
Manage your Pulumi stacks by logging in.
Run `pulumi login --help` for alternative login options.
Enter your access token from https://app.pulumi.com/account/tokens
    or hit <ENTER> to log in using your browser                   : 
```

**注意:** *将 API token 粘贴到终端时，不会显示任何值。出于安全目的，它将保持不可见。*

可能会提示您在终端中选择一个堆栈。如果是，选择`dev`选项，然后点击回车。您将看到类似这样的结果。

```
Previewing update (dev):
     Type                     Name                        Plan
 +   pulumi:pulumi:Stack      compute-dev                 create
 +   ├─ gcp:compute:Network   network                     create
 +   ├─ gcp:compute:Address   workshops-infra-as-code101  create
 +   ├─ gcp:compute:Instance  workshops-infra-as-code101  create
 +   └─ gcp:compute:Firewall  firewall                    create

Resources:
    + 5 to create 
```

您刚刚启用了对 Pulumi 后端云的访问，它会跟踪您的基础设施的状态。现在您已经准备好运行一些代码了。

### Pulumi up

要执行 Pulumi 代码，请使用命令`pulumi up`。该命令创建或更新堆栈中的资源。通过运行当前的 Pulumi 程序并观察所有的资源分配来产生一个资源图，可以计算出目标栈的新的期望目标状态。然后，将该目标状态与现有状态进行比较，以确定必须进行哪些创建、读取、更新和/或删除操作，从而以破坏性最小的方式实现期望的目标状态。之后，该命令记录堆栈新状态的完整事务快照，以便以后可以再次增量更新堆栈。

在终端运行中:

```
pulumi up 
```

选择`yes`选项并按下回车键。Pulumi 应用程序将会执行，很快您将拥有一个完整的服务器，在 GCP 上运行应用程序。您将看到类似这样的结果。

```
Updating (dev):
     Type                     Name                        Status
 +   pulumi:pulumi:Stack      compute-dev                 created
 +   ├─ gcp:compute:Network   network                     created
 +   ├─ gcp:compute:Address   workshops-infra-as-code101  created
 +   ├─ gcp:compute:Firewall  firewall                    created
 +   └─ gcp:compute:Instance  workshops-infra-as-code101  created

Outputs:
    external_ip       : "34.74.75.17"
    instance_meta_data: {
        gce-container-declaration: "spec:\n  containers:\n    - name: workshops-infra-as-code101\n      image: ariv3ra/workshops-infra-as-code101:latest\n      stdin: false\n      tty: false\n  restartPolicy: Always\n"
    }
    instance_name     : "workshops-infra-as-code101"
    instance_network  : [
        [0]: {
            accessConfigs    : [
                [0]: {
                    natIp              : "34.74.75.17"
                    network_tier       : "PREMIUM"
                }
            ]
            name             : "nic0"
        }
    ]

Resources:
    + 5 created

Duration: 58s 
```

在这些结果中，您会看到一个`external_ip`键。它的值是在端口 5000 上公开的面向公众的应用程序的 IP 地址。就像前面的 Terraform 示例一样，您可以在浏览器中访问应用程序。请记住留出几分钟时间，以便后端系统可以将所有内容上线。

### 普鲁米毁灭

Pulumi 有一个命令可以终止所有名为`pulumi destroy`的云资源。此命令按名称删除整个现有堆栈。当前状态是从工作区的相关状态文件中加载的。运行完成后，该堆栈的所有资源和相关状态都将消失。

在终端运行`pulumi destroy`，当提示**永久**销毁创建的 GCP 资源时，选择`yes`选项。

## 摘要

恭喜你！您刚刚升级，现在已经有了使用现代 IaC 工具 [Terraform](https://www.terraform.io/) 和 [Pulumi](https://www.pulumi.com/) 向 GCP 供应和部署应用程序的经验。以下资源将帮助您扩展知识: