# 如何用 Packer 和 CircleCI 工作流构建不可变的基础设施

> 原文：<https://circleci.com/blog/how-to-build-immutable-infrastructure-with-packer-and-circleci-workflows/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

HashiCorp [最近宣布](https://www.hashicorp.com/blog/hashicorp-terraform-enterprise-general-availability)他们将放弃 Atlas，并将 Terraform Enterprise 作为独立产品提供给客户。在本帖中，我们将概述如何使用 Packer 和 CircleCI 复制您的 Atlas 管道，并举例说明 Packer 作业配置、AMI 生成、使用 Terraform 管理变更以及在 S3 存储工件。在 CircleCI，我们使用 AMIs 作为不可变基础设施的一部分。能够在我们的持续部署管道中的单点将配置和变更管理应用到 AMI 是非常有益的。不可变的基础设施允许我们以尽可能快且可证明的方式纵向扩展以满足需求——如果我们的扩展请求必须等待供应过程完成，我们将不得不过度供应以处理峰值。

一旦您有了以这种方式生成的 ami，您现在也可以通过使用 Terraform(或 Chef、Puppet 等)等工具将您的网络基础设施移入代码中而受益。).让您的基础设施以代码的形式存在，可以让它受益于作为持续集成和部署管道的一部分的变更管理。

在这个过程的每一步，我们都应该以可审计和可再现的方式进行变更，以生成现在可以根据需要进行测试、验证和部署的工件。管道步骤不再需要同步或紧密耦合。我们还获得了改变管道中任何工具的能力，只要它匹配它生成的任何工件的需求。

既然我们已经讨论了为什么要这样做，让我们来看看如何做。在我们的示例中，我们将引用这些工具:

*   Docker，它允许您测试和微调每个打包定义
*   Packer，它允许您从一个源配置创建多个机器映像
*   AWS 命令行(或类似的)允许您部署新 AMI 的实例

我们将有一个基本 AMI 和一个雪花 AMI，它们的源是基本 AMI。我们还将为两者生成清单 json 文件，并将它们推送到 S3。

我们假设您对 Packer 有一定的了解，并且至少已经阅读了 [HashiCorp Packer 入门](https://www.packer.io/intro/getting-started/install.html)文档。

基本 AMI 的封隔器配置相当标准，您可以在 https://github.com/CircleCI-Public/circleci-packer.看到完整的配置

下面的例子有几个项目可以使调试和测试您的打包程序配置更容易，例如，我们使用一个`inline` provisioner 进行循环，直到我们看到云初始化阶段完成，否则我们可能会在操作系统完全初始化之前安装到操作系统上。

我们还定义了`user`变量，包括`ami_sha`变量，它将从 Packer 配置的 Git SHA 中获取值。这由 [tag_exists()](https://github.com/CircleCI-Public/circleci-packer/blob/master/scripts/common.sh#L2) shell 函数用来检查正在生成的 AMI 是否已经存在。

```
 "provisioners": [
  {
     "inline": [
       "while [ ! -f /var/lib/cloud/instance/boot-finished ]; do echo 'Waiting for cloud-init...'; sleep 1; done"
     ],
     "type": "shell"
   },
   {
     "scripts": [
       "./base/tasks/baseline.sh",
       "./base/tasks/cleanup.sh",
       "./base/tasks/debug.sh"
     ],
     "type": "shell"
   }
 ],
 "variables": {
   "ami_name": "baseline-ubuntu-1604",
   "ami_base": "ami-aa2ea6d0",
   "ami_sha":  "{{env `SHA`}}"
 } 
```

供应的实际工作由三个任务脚本控制，它们为 Ubuntu 准备 APT 环境，将区域设置为 UTC(唯一的真实时区)，然后执行始终存在的`apt-get upgrade`。因为我们在基线 AMI *和*中执行这些基本任务，所以当基线 AMI 发生变化时，我们的依赖 AMI 会自动生成，我们的下游 AMI 的任务脚本可以针对该 AMI，这改善了我们的干燥(不要重复自己)态度。通过在我们的 Packer 配置中包含一个`Docker`构建器，我们还从 Packer 中获得了不仅生成 AMI 而且生成 Docker 容器的能力。

我们可以更深入地研究 Packer 提供的不同旋钮和杠杆，但他们的文档在这方面做得很好，所以我宁愿深入研究 build.sh 脚本中的“粘合”代码，它使菊花链 ami 成为可能。

您可能已经知道，我们使用 Git SHA 作为我们的标记来识别对 AMI 的打包器配置的更改是否会生成 AMI。在`common.sh`中，我们有 [get_base_ami()](https://github.com/CircleCI-Public/circleci-packer/blob/master/scripts/common.sh#L31) ，它以两种方式寻找基本 ami。首先，它将查看我们是否可以在先前运行生成的 manifest.json 文件中找到它，或者它将在 AWS AMI 列表中查找 Git SHA。如果找到了 AMI，我们可以从 AMI 的标签中寻找 CircleCI 工件 ID。

这样，当基础 AMI 的 CircleCI 工作流中的变化触发了我们的依赖 CircleCI 工作流步骤时，我们能够检索生成的基础 AMI，并将其插入到我们的 Packer 构建中。

Packer 是一个非常灵活的工具，可以通过 CircleCI 工作流的配置定义针对多个环境，这种组合为我们不可变的基础架构创建了一个持续集成和部署管道。通过在 bash 脚本中拥有我们 AMI 一代的“业务逻辑”,可以沿着持续集成管道进行管理，我们已经满足了 AMI 一代的可审计和可自动化变更管理流程的需求。

我希望这个公认简单的例子有助于指出这个工具组合如何将一个重要的过程从繁琐的手动转移到 DevOps 任务列表的自动部分。