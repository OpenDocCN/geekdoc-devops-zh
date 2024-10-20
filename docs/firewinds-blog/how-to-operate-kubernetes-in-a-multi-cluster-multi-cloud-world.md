# 在多集群、多云计算环境中运营 Kubernetes

> 原文：<https://www.fairwinds.com/blog/how-to-operate-kubernetes-in-a-multi-cluster-multi-cloud-world>

 当跨多个云帐户操作多个 Kubernetes 集群时，如何在最大限度地减少意外更改的同时提高工作效率？在这篇文章中，我将分享一些最佳实践和开源工具，Fairwinds 的团队认为这些对 Kubernetes 从业者很有价值。您还可以观看一个附带视频的[来观看解说视频。](https://youtu.be/Btbqir4UAA8)

## 库贝特尔

在[安装 kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) 时，Kubernetes 命令行界面为 Bash 或 Zsh 配置 shell 完成。这允许您按 Tab 来完成 kubectl 命令和 Kubernetes 资源名称，比如部署或 pod。确保安装了 bash-completion 先决条件包。上面指向 kubectl 安装文档的链接包括设置其 shell 完成的细节。

```
laptop:~$ kubectl get pod [tab]app-7f5fd6f95c-[tab twice]
app-7f5fd6f95c-bnkwj app-7f5fd6f95c-lg9pg
laptop:~$ kubectl get pod app-7f5fd6f95c-b[tab]nkwj 
```

上面的例子使用 tab 来帮助完成一个 kubectl get pod 命令。因为两个 pod 共享相同的“7f...5c”字符串，按 tab 键两次会显示两个窗格。然后，我键入“b”作为决胜符，并再次按 tab 来完成我想要的 pod 的名称。

也可以用 tab 键补全我可能会忘记的命令名，比如 kubectl api-resources。

### 在集群和名称空间之间切换

没有什么比意外地在不同的 Kubernetes 集群中进行更改更能影响您一天的轨迹了——“哦，不，我以为我在 staging 中删除了那个负载平衡器服务！”

kubectl 使用的 Kubernetes 客户机配置将访问参数分组到上下文中。每个上下文都指一个集群、一个用户和当前的默认名称空间。在一个 Kubernetes 客户机配置文件中可能有多个上下文，分别代表不同的临时和生产 Kubernetes 集群。

kubectl 命令可用于在单个 Kubernetes 客户端配置(KubeConfig)文件中定义的多个上下文之间切换，如关于多集群访问的 Kubernetes [文档中所述。然而，](https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/) [kubectx 和 kubens](https://github.com/ahmetb/kubectx) 工具使得在上下文之间切换和设置当前名称空间变得更加容易。对于没有 Bash 或 ZSH 的微软 Windows 用户来说，这些工具直接支持 Windows，并且在 Golang 中进行了相对较新的重写。

### 分离 Kubernetes 上下文

为每个环境使用单独的 KubeConfig 文件，需要更明确的操作来使用特定的上下文或集群。将 KUBECONFIG 环境变量设置为要使用的 KubeConfig 文件。默认情况下，这可以避免同时访问所有集群。给 Kubernetes contexts 起一个有意义的名字，并在 shell 提示符下显示当前的上下文也是有帮助的——下面将详细介绍。

### Extend Kubectl

kubectl 命令可以使用插件来扩展——要了解更多关于如何增强 kubectl 命令行的技巧，请查看我们关于 [kubectl 插件和 krew 插件管理器](https://www.fairwinds.com/blog/help-improve-your-kubectl-command-with-plugins)的博文，并查看这些 Krew 插件:

*   [RBAC-查找](https://github.com/FairwindsOps/rbac-lookup) -轻松查找与任何用户、服务帐户或组相关的角色和集群角色
*   谁能显示哪些主题拥有 RBAC 权限
*   [证书管理器](https://cert-manager.io/docs/usage/kubectl-plugin/) -帮助管理由证书管理器群集附加组件管理的资源

## 管理不同版本的工具

升级是一种持续的生活方式，您可能有不同版本的 Kubernetes、Helm 或其他基础设施管理工具，如 Terraform。理想情况下，[版本的 kubectl 与](https://kubernetes.io/docs/setup/release/version-skew-policy/)版本的 Kubernetes API 相匹配，团队中的每个人都使用相同版本的 Helm 来管理集群插件或应用程序。一个版本管理器，如 [asdf](https://asdf-vm.com/#/) ，管理命令行工具的多个版本，以及 Python、Node 或 Ruby 等语言。asdf 工具版本文件可以包含在项目或存储库目录的顶部，以便根据当前工作目录配置使用哪个版本的工具。asdf 工具使用[插件](https://asdf-vm.com/#/plugins-all)来支持许多工具和语言。

```
laptop:~$ asdf plugin add kubectl
laptop:~$ asdf list all kubectl
…..
1.19.2
1.19.3
1.20.0-alpha.1
1.20.0-alpha.2
…..
laptop:~$ asdf install kubectl 1.19.3
Downloading kubectl from https://storage.googleapis.com/kubernetes-release/release/v1.19.3/bin/darwin/amd64/kubectl
laptop:~$ cd /path/to/project/directory
laptop:~$ asdf local kubectl 1.19.3
laptop:~$ cat .tool-versions
kubectl 1.19.3 
```

上面的例子为 kubectl 添加了 asdf 插件，列出了可用的 kubectl 版本，安装了 1.19.3 版本，并配置了一个项目目录以使用该版本的 kubectl。

Asdf 还支持使用环境变量来配置使用哪个版本的工具，如果您的工作流有助于改变您的环境而不是依赖于当前的工作目录。例如，将 ASDF _ 库 ECTL_VERSION 环境变量设置为“1.19.3”。

## 多个云帐户

有时，有必要检查 Kubernetes 节点的计算实例，这可能跨越多个环境，这些环境可能使用不同的云帐户、项目或订阅。 [AWS](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-envvars.html) 、 [Google](https://cloud.google.com/sdk/gcloud/reference/config) 和 [Azure](https://docs.microsoft.com/en-us/cli/azure/azure-cli-configuration) 的云命令行接口支持使用环境变量控制配置。这些环境变量可以使用 [direnv](https://direnv.net/) 工具基于您当前的工作目录进行更改，该工具在您“cd”到一个目录时设置环境变量。

### 亚马逊网络服务

使用 aws configure - profile …命令为每个环境配置一个配置文件，并通过- profile 命令行参数或 AWS_PROFILE 环境变量使用该配置文件。

```
laptop:~$ aws configure --profile production
AWS Access Key ID [None]: xxxxx
AWS Secret Access Key [None]: yyyyy
Default region name [None]: us-east-2
Default output format [None]: text
laptop:~$ aws --profile production s3 ls
…..
laptop:~$ export AWS_PROFILE=production # Use this profile while the variable is set 
```

### 谷歌云

使用 g cloud config configuration s create 命令为每个环境创建一个单独的配置。使用 gcloud config set 命令来设置 Google 帐户和项目等属性。通过 CLOUDSDK_ACTIVE_CONFIG_NAME 环境变量指定哪个配置是活动的。

```
laptop:~$ gcloud config configurations create qa
Created [qa].
Activated [qa].
laptop:~$ gcloud config set account ivan@fairwinds.com
Updated property [core/account].
laptop:~$ gcloud config set project qa                
Updated property [core/project].
WARNING: You do not appear to have access to project [qa] or it does not exist.
laptop:~$ export CLOUDSDK_ACTIVE_CONFIG_NAME=qa # Use this configuration while the variable is set
laptop:~$ gcloud auth login
…..
laptop:~$ gcloud auth application-default login # Optional, if using tools like Terraform 
```

### 蔚蓝色的云

为每个环境的命令行配置文件使用单独的目录。设置 AZURE_CONFIG_DIR 环境变量，然后使用 az login 和 az account set 命令为该环境配置 AZURE 命令行。

```
laptop:~$ export AZURE_CONFIG_DIR=~/infra/test/.azure
laptop:~$ az account show # demonstrates this is a new config
Please run 'az login' to setup account.
laptop:~$ az login
…..
laptop:~$ az account set --subscription xxxxx 
```

## 我在哪里？在你的提示中反映事情

快速灵活的 [Starship](https://starship.rs/) 提示符定制工具可以在您的 shell 提示符中显示 Kubernetes 和云信息，以帮助了解哪个 Kubernetes 集群和云帐户是活动的。 [Kubernetes 组件](https://starship.rs/config/#kubernetes)显示当前的上下文，也可以显示 KubeConfig 文件中定义的当前名称空间。还有 [AWS](https://starship.rs/config/#aws) 和 [gcloud](https://starship.rs/config/#gcloud) 组件来包含关于那些云的信息。[环境变量](https://starship.rs/config/#environment-variable)和[定制命令](https://starship.rs/config/#custom-commands)组件可用于提供额外的上下文，包括活动 Azure 订阅。

请看随附的视频，及其[示例星舰配置](https://github.com/FairwindsOps/how-to-kube/blob/master/blog-multiple-clusters-and-clouds/starship.toml)。我们希望这有助于您跨多个云操作 Kubernetes 集群。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)