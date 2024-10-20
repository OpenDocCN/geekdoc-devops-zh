# 使用 CircleCI runner | CircleCI 运行私有云及内部作业

> 原文：<https://circleci.com/blog/run-private-cloud-and-on-premises-jobs-with-circleci-runner/>

CircleCI 发布了一个名为 [CircleCI runner](https://circleci.com/docs/runner-overview/) 的新功能。runner 特性增强并扩展了 CircleCI 平台的功能，使开发人员能够多样化他们的构建/工作负载环境。多样化的执行环境满足了我们在 [CircleCI runner 公告](https://circleci.com/blog/our-cloud-platform-your-compute-introducing-the-circleci-runner/)中提到的一些特定的边缘情况。例如，我们在金融和医疗保健等高度监管行业的一些较大客户必须满足合规性要求，以防止他们在云中运行一些工作负载。其他从事嵌入式系统或物联网工作的人需要在云中根本不存在的硬件上构建。我们构建了 runner，以便即使是具有最严格的安全性和合规性要求的客户也可以使用 CircleCI 满足其 100%的软件交付需求。在这篇文章中，我将介绍 CircleCI 流道，并演示如何在管道中使用流道。

## 先决条件

在开始使用跑步者之前，您需要完成一些任务:

## 运行节点平台

CircleCI runner [的当前产品在这些平台和操作系统上安装和运行](https://circleci.com/docs/runner-installation/index.html/#installation):

| 安装目标 | 目标值 |
| --- | --- |
| Linux x86_64 | 平台=linux/amd64 |
| Linux ARM64 | 平台=linux/arm64 |
| macOS x86_64 | 平台=达尔文/amd64 |

在本教程中，我们将使用 [Ubuntu 20.04](https://releases.ubuntu.com/20.04/) 和`platform=linux/amd64`平台。

## 供应 CircleCI 流道配置

在部署 runner 节点和处理作业之前，您需要创建名称空间和资源类，并生成 runner 令牌。这些操作在本教程的先决条件部分提到过，但是我将在这里更详细地介绍它们。

### 创建 CircleCI 命名空间

CircleCI 要求您在平台上创建一个[名称空间](https://circleci.com/docs/runner-installation/index.html/#authentication)，定义您在 CircleCI 平台中的唯一身份。这个名称空间也用于其他功能，如 CircleCI Orbs registry。如果您已经创建了一个名称空间，那么您可以前进到下一节。

使用 [CircleCI CLI 工具](https://www.terraform.io/docs/cli-index.html)创建新的名称空间:

```
circleci namespace create <your namespace> <vcs-type> <org-name> 
```

指定您的名称空间名称/值和您的 VCS/版本控制类型(github 等..).然后指定您的组织名称(通常是您的 GitHub 用户/组织名称)。

### 创建新的资源类

创建名称空间之后，您需要为您的跑步者创建一个新的资源类。资源类别允许您对相似的跑步者资源进行分组，并可用于对跑步者进行分类。例如，如果您希望使用 macOS 和 Linux 运行程序，您应该创建两个资源类，一个用于 macOS 资源，一个用于 Linux 资源。您还可以根据计算节点硬件资源(如 CPU、RAM、磁盘和网络功能)对资源类进行分类。运行以下命令创建新的 runner 资源类:

```
circleci runner resource-class create <my-namespace/resource-class> <description> 
```

第`<my-namespace/resource-class>`节要求您指定您的 CircleCI 名称空间，然后创建一个资源类名。在这个例子中:`punkdata/do-ub-linux-cpu4-16gbram`，名称空间是`punkdata`，`do-ub-linux-cpu4-16gbram`是资源类名。`do-ub-linux-cpu4-16gbram`是一个描述性的名称，代表使用 DigitalOcean 计算节点的 Ubuntu Linux 平台，该节点具有 4 个 CPU 和 16 GB RAM。如果需要，您可以通过填充`<description>`参数来给出关于资源类的更多细节。

### 创建跑步者令牌

使用 CLI 工具完成的最后一项任务是生成运行者令牌，运行者节点使用该令牌根据 CircleCI 平台进行身份验证。

**注意:** *令牌无法再次检索，因此请确保在创建后安全存放。*

通过运行以下命令生成运行者令牌:

```
circleci runner token create <my-namespace/resource-class> <nickname> 
```

和前面一样，指定您的`<my-namespace/resource-class>`参数值，然后为您的令牌指定一个描述性的、容易记忆的`<nickname>`值。

## CircleCI 转轮手动安装

CircleCI 运行人员使用手动安装过程，该过程详细说明了如何在相应的计算节点上安装和配置所需的软件。我鼓励您熟悉这个[手动流程](https://circleci.com/docs/runner-installation/index.html/#installation)。不过，在本教程中，我们将使用 Terraform 在 DigitalOcean 平台上提供 CircleCI 跑步者。

## CircleCI 转轮提供自动化

接下来，我将介绍我使用 Terraform 构建的自动化跑步者供应流程。从这个回购中获得的代码[是我们将用来提供 CircleCI 跑步者的代码。我希望您已经完成了先决条件一节中的步骤。您现在需要完成它们，以便完成本教程。](https://github.com/CircleCI-Public/blog-runner)

此[项目回购](https://github.com/CircleCI-Public/blog-runner)结构中的相关代码位于以下目录中:

```
runner-scripts/
  |_runner-agent-install
  |_runner-provisioner
terraform/
  |_do/
      |_main.tf
      |_variables.tf 
```

在下一节中，我将讨论 runner-agent_install 和 runner-provisioner 文件的内容。这些文件使一些手动转轮安装过程自动化。

### 跑步者脚本

`runner-scripts/`目录包含脚本文件，这些文件自动安装和提供运行器节点。这些脚本由代码和命令组成，封装了手动转轮安装过程中定义的各种脚本和命令。这些脚本无需修改即可用于安装和供应运行程序节点。在本教程中，我们将在 Terraform 代码中利用它们。这是这些文件的快速分类。

`runner-agent-install`脚本有一个“平台”参数，必须为其分配以下值之一:

*   平台=linux/amd64
*   平台=linux/arm64
*   平台=达尔文/amd64

必须首先执行该脚本来安装适当的运行程序代理。

`runner-provisioner`脚本提供并配置运行程序代理和底层操作系统服务。此脚本具有必须分配的参数:

*   `PLATFORM`需要是上面列出的受支持平台值之一
*   `RUNNER_NAME`标识跑步者节点；我建议使用节点的主机名
*   `RUNNER_TOKEN`是您在本教程中前几步生成的跑步者令牌

这个脚本自动执行运行器配置和操作系统级操作，比如用户配置和运行器相关的服务实现。在计算节点上运行这些脚本将产生一个功能完整的 CircleCI runner 节点，它可以在您的 CircleCI 配置中用作执行器。

接下来，我将讨论 Terraform 代码，它利用这些脚本将基础设施作为代码来提供 runner 节点。

### Terraform runner 供应器代码

让我花点时间浏览一下提供 runner 节点的 Terraform 代码。看看`terraform/do/`目录中的文件，从`variables.tf`文件开始。

`variables.tf`文件保存了 Terraform 项目使用的所有变量。该文件中指定的变量定义允许使用 Terraform CLI 传递值。下面是一个`variables.tf`文件的例子:

```
variable "do_token" {
  type        = string
  description = "Your DigitalOcean API token. See https://cloud.digitalocean.com/account/api/tokens to generate a token."
}

variable ssh_key_file {
  type        = string
  description = "The private ssh certificate in the user's .ssh/ dir -> $HOME/.ssh/id_rsa. Terrform uses this to access & run commands on node."
}

variable do_ssh_key {
  type        = string
  default     = "ariv3ra-ssh"
  description = "Specifies a Public SSH cert that exists in the DO Account Security section. See https://www.digitalocean.com/docs/droplets/how-to/add-ssh-keys/to-account/"
}

variable file_agent_install {
  type        = string
  default     = "../../runner-scripts/runner-agent-install"
  description = "Specifies the runner-agent-install script file path"
}

variable file_provisioner {
  type        = string
  default     = "../../runner-scripts/runner-provisioner"
  description = "Specifies the runner-provisioner script file path"
}

variable runner_platform {
  type        = string
  default     = "linux/amd64"
  description = "Defines the runner architecture platform choose one: [linux/amd64, linux/arm64, darwin/amd64]"
}

variable runner_name {
  type        = string
  description = "The host name of the CircleCI Runner which is also the Droplet name."
}

variable runner_token {
  type        = string
  description = "The CircleCI Runner token from the CLI"
}

variable "region" {
  type        = string
  description = "The region where the assets should be assigned to"
  default     = "nyc3"
}

variable "node_size" {
  type        = string
  description = "defines the size of the compute node"
  default     = "s-4vcpu-8gb"
}

variable "image_name" {
  type        = string
  description = "defines the DigitalOcean image name to install to compute node"
  default     = "ubuntu-20-04-x64"
}

variable droplet_count {
  type        = number
  default     = 2
  description = "The number of Droplet nodes you want to create"
} 
```

本例中列出的变量有`description`字段，告诉您每个变量指定的内容。使用这些变量，您可以为运行者节点计数、计算节点大小、运行者平台以及任何其他关于运行者或云提供商的相关数据赋值。在这种情况下，Terraform 代码适用于以数字海洋水滴的形式提供 runner 节点，这些节点本质上是计算节点。

接下来，看一下`main.tf`文件，它包含了这个项目的核心 Terraform 功能。下面是一个`main.tf`文件的例子:

```
terraform {

  required_version = ">= 0.13"

  required_providers {
    digitalocean = {
      source = "digitalocean/digitalocean"
    }
    local = {
      source = "hashicorp/local"
    }
  }
  # This backend uses the terraform cloud for state.
  backend "remote" {
    organization = "datapunks"
    workspaces {
      name = "dorunner"
    }
  }
}

provider "digitalocean" {
  token = var.do_token
}

data "digitalocean_ssh_key" "terraform" {
  name = var.do_ssh_key
}

resource "digitalocean_droplet" "dorunner" {
  count              = var.droplet_count
  image              = var.image_name
  name               = "${var.runner_name}-${count.index}"
  region             = var.region
  size               = var.node_size
  private_networking = true
  ssh_keys = [
    data.digitalocean_ssh_key.terraform.id
  ]
  connection {
    host        = self.ipv4_address
    user        = "root"
    type        = "ssh"
    private_key = file(var.ssh_key_file)
    timeout     = "3m"
  }

  #Upload runner agent install script
  provisioner "file" {
    source      = var.file_agent_install
    destination = "/tmp/runner-agent-install"
  }

  #Upload runner provisioner script
  provisioner "file" {
    source      = var.file_provisioner
    destination = "/tmp/runner-provisioner"
  }

  provisioner "remote-exec" {
    inline = [
      "export PATH=$PATH:/usr/bin",
      "sudo apt update",
      "sudo apt -y upgrade",
      "curl -sL https://deb.nodesource.com/setup_14.x | sudo bash -",
      "sudo apt install -y nodejs",
      "cd /tmp",
      "chmod +x /tmp/runner-agent-install",
      "chmod +x /tmp/runner-provisioner",
      "/tmp/runner-agent-install ${var.runner_platform}",
      "/tmp/runner-provisioner ${var.runner_platform} ${var.runner_name} ${var.runner_token}",
    ]
  }
}

output "runner_hosts and ip_addresses" {
  value = {
    for instance in digitalocean_droplet.dorunner :
    instance.name => instance.ipv4_address
  }
} 
```

这个文件由多个元素组成。我将把它们分开，并在每个代码片段后提供一个描述。

```
terraform {

  required_version = ">= 0.13"

  required_providers {
    digitalocean = {
      source = "digitalocean/digitalocean"
    }
    local = {
      source = "hashicorp/local"
    }
  }
  # This backend uses the terraform cloud for state.
  backend "remote" {
    organization = "datapunks"
    workspaces {
      name = "dorunner"
    }
  }
} 
```

这个片段指定了我们的目标平台提供商(DigitalOcean)。它还指定了维护项目状态的位置。在这种情况下，它位于您在先决条件部分创建的`dorunner` Terraform Cloud 工作空间中。

```
provider "digitalocean" {
  token = var.do_token
}

data "digitalocean_ssh_key" "terraform" {
  name = var.do_ssh_key
} 
```

这个片段指定了在 DigitalOcean 平台中配置的 DigitalOcean API 令牌和 SSH 密钥。使用在`variables.tf`文件中声明的变量指定这些值。

```
resource "digitalocean_droplet" "dorunner" {
  count              = var.droplet_count
  image              = var.image_name
  name               = "${var.runner_name}-${count.index}"
  region             = var.region
  size               = var.node_size
  private_networking = true
  ssh_keys = [
    data.digitalocean_ssh_key.terraform.id
  ]
  connection {
    host        = self.ipv4_address
    user        = "root"
    type        = "ssh"
    private_key = file(var.ssh_key_file)
    timeout     = "3m"
  } 
```

这个代码片段代表了实际创建和提供 runner 节点的自动化。您需要了解的主要因素:

*   `name`是现有变量的插值，生成唯一的运行节点主机名
*   `ssh_keys`使用 data.digitalocean_ssh_key 块从 DO 中检索公共 ssh 密钥
*   Terraform 需要访问权限才能在新创建的节点上执行命令。`private_key`指定与在`var.do_ssh_key`变量中指定的公共 ssh 密钥相对应的私有 ssh 密钥

```
 #Upload runner agent install script
  provisioner "file" {
    source      = var.file_agent_install
    destination = "/tmp/runner-agent-install"
  }

  #Upload runner provisioner script
  provisioner "file" {
    source      = var.file_provisioner
    destination = "/tmp/runner-provisioner"
  } 
```

这个代码片段将`runner_agent_install`和`runner_provisioner`上传到新创建的计算节点。这些文件将安装 CircleCI runner 代理，并将节点设置为 CircleCI runner 节点。

```
 provisioner "remote-exec" {
    inline = [
      "export PATH=$PATH:/usr/bin",
      "sudo apt update",
      "sudo apt -y upgrade",
      "curl -sL https://deb.nodesource.com/setup_14.x | sudo bash -",
      "sudo apt install -y nodejs",
      "cd /tmp",
      "chmod +x /tmp/runner-agent-install",
      "chmod +x /tmp/runner-provisioner",
      "/tmp/runner-agent-install ${var.runner_platform}",
      "/tmp/runner-provisioner ${var.runner_platform} ${var.runner_name} ${var.runner_token}",
    ]
  }
} 
```

这个代码片段在计算节点上远程执行命令。`inline`参数是要在计算节点上执行的命令列表。这些命令构成了 CircleCI runner 节点的配置过程。我想指出的是，这个模块有一些重要的命令:

*   `curl -sL https://deb.nodesource.com/setup_14.x | sudo bash -`下载 Node.js 版本 14
*   `/tmp/runner-agent-install ${var.runner_platform}`使用`var.runner_platform`值执行转轮安装脚本
*   `/tmp/runner-provisioner ${var.runner_platform} ${var.runner_name} ${var.runner_token}`使用多个变量(如平台、运行程序名称和令牌)执行运行程序配置脚本

```
output "runner_hosts_and_ip_addresses" {
  value = {
    for instance in digitalocean_droplet.dorunner :
    instance.name => instance.ipv4_address
  }
} 
```

最后，这个片段是 Terraform 输出，它遍历所有节点并打印主机名和相应的 IP 地址。当 Terraform 执行完成时，您将拥有功能齐全的 CircleCI runner 节点，可以开始处理您的 runner 管道工作负载。

## 创建 CircleCI 转轮节点

我将使用[示例代码 repo](https://github.com/CircleCI-Public/blog-runner) 来演示在 DigitalOcean 中创建 runner 节点。

在终端类型中:

```
cd terraform/do

terraform init 
```

前面的命令将初始化 Terraform 项目。下一个命令将执行 Terraform 创建过程:

```
terraform apply \
  -var "do_token=${DIGITAL_OCEAN_TOKEN}" \
  -var "runner_token=${RUNNER_TOKEN}" \
  -var "runner_name=dorunner" \
  -var "ssh_key_file=${HOME}/.ssh/id_rsa" \
  -auto-approve 
```

此 Terraform 命令指定 Terraform 变量的值。我建议将敏感数据分配给本地系统环境变量，以防止暴露，并在终端中工作时保持一致的行为。如果 Terraform 变量分配了`default`值，则这些值将被使用，而不是必需的。您可以通过简单地指定您想要覆盖的变量并提供要使用的新值来覆盖`default`变量值。

运行此命令后，您应该会看到类似如下的结果:

```
Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

Outputs:

runner_hosts_and_ip_addresses = {
  "dorunner-0" = "68.183.55.75"
  "dorunner-1" = "68.183.61.144"
} 
```

正如您在这里看到的，已经创建了两个单独的 DigitalOcean Droplet 节点，并将其配置为 CircleCI runner 节点。这些节点已经准备好开始处理管道工作负载。结果显示了节点的“主机名”和相应的 IP 地址，如果需要，您可以使用 SSH 来访问它们。

## 修改流道节点

现在您已经使用 Terraform 创建了 runner 节点，您还可以使用 Terraform 来管理这些节点。例如，我最初创建了两个 runner 节点来处理管道工作负载。如果事实证明两个节点不足以处理所有工作负载，您可以添加更多节点。如果需要，可以添加三个额外的 runner 节点来帮助处理工作负载。在这种情况下，您可以将`var.droplet_count`值调整为您需要的节点数量。为什么不将数量增加一倍，达到 4 个活动运行程序，这样我们就有足够的节点来处理工作负载了？下面是一个示例命令，您可以使用它来添加两个新的跑步者节点:

```
terraform apply \
  -var "do_token=${DIGITAL_OCEAN_TOKEN}" \
  -var "runner_token=${RUNNER_TOKEN}" \
  -var "runner_name=dorunner" \
  -var "ssh_key_file=${HOME}/.ssh/id_rsa" \
  -var "droplet_count=4" \
  -auto-approve 
```

我使用这个命令，通过在 apply 命令中将新的数量分配给 Terraform 变量`-var "droplet_count=4"`，将现有的流道节点数从 2 更改为 4。运行该命令后，您应该会看到如下结果:

```
Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

Outputs:

runner_hosts_and_ip_addresses = {
  "dorunner-0" = "68.183.55.75"
  "dorunner-1" = "68.183.61.144"
  "dorunner-2" = "64.225.21.96"
  "dorunner-3" = "64.225.21.95"
} 
```

在这些结果中，您可以看到添加了两个新的跑步者节点。现在总共有四个 runner 节点准备好处理工作负载。您可以使用这种方法减少所需的节点总数。在任何情况下，`droplet-count`变量都是节点总数的控制点。

## CircleCI runner 示例 config.yml

现在我们已经有了一些 CircleCI runner 节点，可以开始使用它们来处理 CI/CD 管道中的工作负载。我创建了一个示例 config.yml，演示了如何在管道作业中使用 CircelCI runner 节点:

```
version: 2.1

jobs:

  runner_build_test:
    machine: true
    resource_class: datapunks/dorunner
    steps:
      - checkout
      - run:
          name: Install npm dependencies
          command: |
            npm install
      - run:
          name: Run Unit Tests
          command: |
            ./node_modules/mocha/bin/mocha test/ --reporter mochawesome --reporter-options reportDir=test-results,reportFilename=test-results
      - store_artifacts:
          path: test-results

workflows:
  build_test_deploy:
    jobs:
      - runner_build_test 
```

这个示例显示了一个在 runner 节点上执行的简单作业。`runner_build_test:`任务展示了如何使用`machine:`和`resource_class:`键定义跑步者执行者。使用`namespace/resource_class_name`组合键分配`resource_class:`键。当指定`resource_class:`键的值时，必须使用该组合。该示例来自 Node.js 项目，要求 Node.js 安装在 runner 节点上，这发生在 Terraform 供应过程中。

既然我已经演示了如何在 config.yml 文件中实现和使用 runner，那么您应该很清楚如何在您选择的平台和云提供商上使用 Terraform 轻松地启动和运行 CircleCI runner 节点。作为奖励，我将在下一部分描述一些你可以用来帮助跑步者做好准备的技巧。

## CircleCI 跑步提示

CircleCI 跑步器仍然非常新，所以我想分享一些建议，你可能会发现在使用跑步器时有帮助:

*   **在转轮节点上安装软件**。Runner 必须在本地安装您的构建所依赖的所有软件:Node.js、git、gcc、python、Docker client 以及其他任何需要的软件。
*   **了解跑步者安全政策**。如果您在云提供商中运行节点，请确保您完全理解各个提供商内部安全性的复杂性。理解诸如安全组、基于角色的访问和防火墙配置之类的东西对于运行 runner 节点是至关重要的。
*   **给你的跑步者贴上补丁**。运行器本质上是计算节点，可以增强您当前的 CircleCI 管道。这些节点的管理完全由您的组织负责。这包括更新适当的软件/依赖项以及操作系统安全和漏洞补丁。维护最新的软件和安全的 runner 节点对于保护您的工作负载至关重要。
*   **了解跑步者的局限性**。在这里熟悉一下[目前跑步者的局限性](https://circleci.com/docs/runner-overview/#circleci-runner-limitations)，这样你就明白跑步者能做什么，不能做什么。

## 结论

CircleCI 平台处理所有的 CI/CD 处理，但是 CircleCI runners 是扩充您的 CI/CD 管道的一个好方法。如果您需要执行专门的处理，或者如果您有定制或规定的要求，流道会特别有用。仅在特定的用例中使用 runners，比如在本地运行作业的需求。当存在由于更严格的隔离而限制访问的基础设施时，或者如果您使用 CircleCI 不作为资源类提供的独特计算架构时，运行者可以提供帮助。

在这篇文章中，我简单介绍并演示了如何使用脚本和 Terraform 自动创建和提供 CircleCI runners。我还演示了如何定义管道中的`runner`作业。我很想听听你的反馈、想法和意见，所以请发推特给我 [@punkdata](https://circleci.com/blog/our-cloud-platform-your-compute-introducing-the-circleci-runner/) 加入讨论。

感谢阅读！