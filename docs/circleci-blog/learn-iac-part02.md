# 基础设施即代码，第 2 部分:使用 Terraform | CircleCI 构建 Docker 映像并部署到 Kubernetes 集群

> 原文：<https://circleci.com/blog/learn-iac-part02/>

本系列向您展示如何开始使用基础设施即代码(IaC)。目标是通过教程和代码示例帮助开发人员加深对 IaC 的理解。

在这篇文章中，我将演示如何为应用程序创建一个 [Docker 映像](https://docs.docker.com/get-started/overview/#docker-objects)，然后将该映像推送到 [Docker Hub。](https://hub.docker.com/)我还将讨论如何使用 [HashiCorp 的 Terraform 创建 Docker 映像并将其部署到](https://www.terraform.io/) [Google Kubernetes Engine (GKE)集群](https://cloud.google.com/kubernetes-engine)。

下面是我们将在这篇文章中完成的事情的快速列表:

1.  构建新的 [Docker 映像](https://docs.docker.com/get-started/overview/#docker-objects)
2.  将新的 Docker 映像推送到 [Docker Hub 注册表](https://hub.docker.com/)
3.  使用[地形](https://www.terraform.io/)创建一个新的 [GKE 集群](https://cloud.google.com/kubernetes-engine)
4.  使用 [Terraform Kubernetes 提供者](https://www.terraform.io/docs/providers/kubernetes/)创建一个新的 [Terraform Kubernetes 部署](https://www.terraform.io/docs/providers/kubernetes/r/deployment.html)
5.  摧毁所有使用[地形](https://www.terraform.io/)创造的资源

**注意:** *在你开始学习这部分教程之前，确保你已经完成了第 1 部分的[先决条件部分中的所有操作。](https://circleci.com/blog/learn-iac-part1/)*

我们的第一个任务是学习如何基于包含在[代码报告中的 Node.js 应用程序构建一个](https://github.com/datapunkz/learn_iac) [Docker 镜像](https://docs.docker.com/get-started/overview/#docker-objects)。

## 建立码头工人形象

在[之前的帖子](https://circleci.com/blog/learn-iac-part1/)中，我们使用 [Terraform](https://www.terraform.io/) 创建了一个新的 GKE 集群，但该集群不可用，因为没有部署任何应用程序或服务。因为 Kubernetes (K8s)是一个容器编排器，应用和服务必须打包到 [Docker 镜像](https://docs.docker.com/get-started/overview/#docker-objects)中，然后这些镜像可以衍生出 [Docker 容器](https://docs.docker.com/get-started/overview/#docker-objects)来执行应用或服务。

[Docker 镜像](https://docs.docker.com/get-started/overview/#docker-objects)是使用 [`docker build`命令](https://docs.docker.com/engine/reference/commandline/build/)创建的，你将需要一个[Docker 文件](https://docs.docker.com/engine/reference/builder/)来指定如何构建你的 Docker 镜像。我将讨论 Dockerfiles，但首先我想解决[。dockerignore 文件](https://docs.docker.com/engine/reference/builder/#dockerignore-file)。

### 是什么？dockerignore 文件？

`.dockerignore`文件排除了与其中声明的模式相匹配的文件和目录。使用该文件有助于避免不必要地将大型或敏感的文件和目录发送到守护程序，并有可能将它们添加到公共映像中。在这个项目中，`.dockerignore`文件排除了与 Terraform 和 Node.js 本地依赖相关的不必要的文件。

### 了解 Dockerfile 文件

Docker 文件对于构建 Docker 映像至关重要。它指定了如何构建和配置映像，以及向其中导入什么文件。Dockerfile 文件是动态的，因此您可以用不同的方式完成许多目标。对 Dockerfile 功能有一个扎实的了解是很重要的，这样您就可以构建功能性映像。这是这个项目的代码报告中包含的 docker 文件的细目分类。

```
FROM node:12

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
COPY package*.json ./

RUN npm install --only=production

# Bundle app source
COPY . .

EXPOSE 5000
CMD [ "npm", "start" ] 
```

`FROM node:12`行定义了一个要继承的图像。构建映像时，Docker 从父映像继承。在这种情况下，它是从 Docker Hub 获取的`node:12`图像，如果它不在本地的话。

```
# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
COPY package*.json ./

RUN npm install --only=production 
```

这个代码块定义了`WORKDIR`参数，它指定了 Docker 映像中的工作目录。`COPY package*.json ./`行将任何与包相关的文件复制到 Docker 映像中。`RUN npm install`行安装在`package.json`文件中列出的应用程序依赖项。

```
COPY . .

EXPOSE 5000
CMD [ "npm", "start" ] 
```

这个代码块将所有文件复制到 Docker 映像中，除了在`.dockerignore`文件中列出的文件和目录。`EXPOSE 5000`行指定为这个 Docker 映像公开的端口。`CMD [ "npm", "start" ]`行定义了如何开始这个图像。在这种情况下，它正在执行该项目的`package.json`文件中指定的`start`部分。该`CMD`参数是默认的执行命令。现在您已经理解了 Dockerfile 文件，您可以使用它在本地构建一个映像。

### 使用 Docker 构建命令

使用 [`docker build`](https://docs.docker.com/engine/reference/commandline/build/) 命令，Dockerfile 根据其中定义的指令构建一个新的映像。在构建 Docker 映像时，需要记住一些命名约定。如果您计划共享图像，命名约定尤其重要。

在我们开始构建图像之前，我将花点时间来描述如何给它们命名。Docker 图像使用由斜杠分隔的名称组件组成的[标签](https://docs.docker.com/engine/reference/commandline/tag/)。因为我们将把图像推送到 [Docker Hub](https://hub.docker.com/) ，所以我们需要在图像名称前加上我们的 Docker Hub 用户名。对我来说，那就是`ariv3ra/`。我通常会在后面加上项目的名称，或者图像的有用描述。这个 Docker 镜像的全名将会是`ariv3ra/learniac:0.0.1`。`:0.0.1`是应用程序的版本标签，但是您也可以用它来描述图像的其他细节。

一旦你有了一个好的、描述性的名字，你就可以建立一个形象。以下命令必须从项目 repo 的根目录中执行(确保将`ariv3ra`替换为*您的* Docker Hub 名称):

```
docker build -t ariv3ra/learniac  -t ariv3ra/learniac:0.0.1 . 
```

接下来，运行以下命令查看机器上的 Docker 映像列表:

```
docker images 
```

这是我的输出。

```
REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
ariv3ra/learniac        0.0.1               ba7a22c461ee        24 seconds ago      994MB
ariv3ra/learniac        latest              ba7a22c461ee        24 seconds ago      994MB 
```

### Docker 推送命令

现在我们已经准备好[将这张图片](https://docs.docker.com/engine/reference/commandline/push/)推送到 Docker Hub 并公开发布。Docker Hub 需要授权才能访问服务，所以我们需要使用 [`login`命令来认证](https://docs.docker.com/engine/reference/commandline/login/)。运行以下命令登录:

```
docker login 
```

在提示中输入您的 Docker Hub 凭据以授权您的帐户。每台机器只需要登录一次。现在你可以推送图片了。

使用您的`docker images`命令中列出的图像名称，运行以下命令:

```
docker push ariv3ra/learniac 
```

这是我的输出。

```
The push refers to repository [docker.io/ariv3ra/learniac]
2109cf96cc5e: Pushed 
94ce89a4d236: Pushed 
e16b71ca42ab: Pushed 
8271ac5bc1ac: Pushed 
a0dec5cb284e: Mounted from library/node 
03d91b28d371: Mounted from library/node 
4d8e964e233a: Mounted from library/node 
```

现在，您在 Docker Hub 中有了一个 Docker 映像，并准备好部署到 GKE 集群。将您的应用程序部署到新的 Kubernetes 集群的所有工作都已就绪。下一步是使用 Terraform 构建 Kubernetes 部署。

## 使用 Terraform 部署 Kubernetes

在本系列的第 1 部分中，我们学习了如何使用 Terraform 创建一个新的 Google Kubernetes 引擎(GKE)集群。正如我前面提到的，该集群没有提供任何应用程序或服务，因为我们没有向它部署任何应用程序或服务。在这一节中，我将描述使用 Terraform 部署 [Kubernetes 部署需要什么。](https://www.terraform.io/docs/providers/kubernetes/r/deployment.html)

Terraform 有一个 [Kubernetes 部署](https://www.terraform.io/docs/providers/kubernetes/r/deployment.html)资源，允许您定义一个并执行一个 Kubernetes 部署到您的 GKE 集群。在第 1 部分[中，我们使用`part01/iac_gke_cluster/`目录中的 Terraform 代码创建了一个新的 GKE 集群。在本文中，我们将分别使用`part02/iac_gke_cluster/`和`part02/iac_kubernetes_app/`目录。`iac_gke_cluster/`是我们在第 1 部分中使用的相同代码。我们将在这里结合`iac_kubernetes_app/`目录再次使用它。](https://circleci.com/blog/learn-iac-part1/)

### Terraform Kubernetes 提供商

我们之前使用 Terraform [Google 云平台提供商](https://www.terraform.io/docs/providers/google/index.html)创建了一个新的 [GKE 集群](https://cloud.google.com/kubernetes-engine)。Terraform 提供者是特定于 Google 云平台的，但它仍然是引擎盖下的 Kubernetes。因为 GKE 本质上是一个 Kubernetes 集群，我们需要使用 [Terraform Kubernetes 提供者](https://www.terraform.io/docs/providers/kubernetes/)和 [Kubernetes 部署资源](https://www.terraform.io/docs/providers/kubernetes/r/deployment.html)来配置和部署我们的应用到 GKE 集群。

## 地形代码文件

`part02/iac_kubernetes_app/`目录包含这些文件:

*   providers.tf
*   变量. tf
*   main.tf
*   部署. tf
*   services.tf
*   输出. tf

这些文件维护我们用来定义、创建和配置 Kubernetes 集群应用程序部署的所有代码。接下来，我将分解这些文件，让您更好地理解它们的作用。

### 细分:providers.tf

在`provider.tf`文件中，我们定义了将要使用的 Terraform 提供者: [Terraform Kubernetes 提供者](https://www.terraform.io/docs/providers/kubernetes/)。`provider.tf`:

```
provider "kubernetes" {

} 
```

此代码块定义了将在此 Terraform 项目中使用的提供程序。`{ }`块是空的，因为我们将用不同的进程处理[认证需求。](https://www.terraform.io/docs/providers/google/guides/using_gke_with_terraform.html#interacting-with-kubernetes)

### 细分:变量. tf

这个文件应该看起来很熟悉，并且类似于第 1 部分`variables.tf`文件。这个特殊的文件只指定了这个 Terraform Kubernetes 项目使用的输入变量。

```
variable "cluster" {
  default = "cicd-workshops"
}
variable "app" {
  type        = string
  description = "Name of application"
  default     = "cicd-101"
}
variable "zone" {
  default = "us-east1-d"
}
variable "docker-image" {
  type        = string
  description = "name of the docker image to deploy"
  default     = "ariv3ra/learniac:latest"
} 
```

此文件中定义的变量将在项目文件的代码块中用于整个 Terraform 项目。所有这些变量都有`default`值，可以在执行代码时通过在 CLI 中定义它们来更改。这些变量为 Terraform 代码增加了急需的灵活性，并允许重用有价值的代码。这里需要注意的一点是，`variable "docker-image"`默认参数被设置为我的 Docker 图像名称。将该值替换为您的 Docker 图像的*名称。*

### 细分:main.tf

`main.tf`文件的元素以`terraform`块开始，它指定了 [Terraform 后端](https://www.terraform.io/docs/backends/index.html)的类型。Terraform 中的一个“后端”决定了如何加载状态，以及如何执行一个操作，如`apply`。这种抽象支持非本地文件状态存储和远程执行等。在这个代码块中，我们使用了`remote`后端。它使用 Terraform 云，并连接到您在第 1 部分文章的[先决条件部分创建的`iac_kubernetes_app`工作空间。](https://circleci.com/blog/learn-iac-part1/)

```
terraform {
  required_version = "~>0.12"
  backend "remote" {
    organization = "datapunks"
    workspaces {
      name = "iac_kubernetes_app"
    }
  }
} 
```

### 细分:deployments.tf

接下来是对`deployments.tf`文件中语法的描述。该文件使用 [Terraform Kubernetes 部署资源](https://www.terraform.io/docs/providers/kubernetes/r/deployment.html)来定义、配置和创建将我们的应用程序发布到 GKE 集群所需的所有 Kubernetes 资源。

```
resource "kubernetes_deployment" "app" {
  metadata {
    name = var.app
    labels = {
      app = var.app
    }
  }
  spec {
    replicas = 3

    selector {
      match_labels = {
        app = var.app
      }
    }
    template {
      metadata {
        labels = {
          app = var.app
        }
      }
      spec {
        container {
          image = var.docker-image
          name  = var.app
          port {
            name           = "port-5000"
            container_port = 5000
          }
        }
      }
    }
  }
} 
```

是时候回顾一下代码元素了，以便更好地理解正在发生的事情。

```
resource "kubernetes_deployment" "app" {
  metadata {
    name = var.app
    labels = {
      app = var.app
    }
  } 
```

这个代码块指定了 Terraform Kubernetes 部署资源的使用，它为 Kubernetes 定义了我们的部署对象。`metadata`块用于为 Kubernetes 服务中使用的参数赋值。

```
 spec {
    replicas = 3

    selector {
      match_labels = {
        app = var.app
      }
    }

    template {
      metadata {
        labels = {
          app = var.app
        }
      }

      spec {
        container {
          image = var.docker-image
          name  = var.app
          port {
            name           = "port-5000"
            container_port = 5000
          }
        }
      }
    }
  } 
```

在 resources `spec{...}`块中，我们指定想要三个 [Kubernetes pods](https://kubernetes.io/docs/concepts/workloads/pods/pod/) 在集群中运行我们的应用程序。`selector{...}`块代表[标签选择器](https://kubernetes.io/docs/concepts/overview/working-with-objects/labels/#label-selectors)。这是 Kubernetes 的一个核心分组原语，允许用户选择一组对象。

资源`template{...}`块中有一个`spec{...}`块，它有一个`container{...}`属性块。该块具有定义和配置部署中使用的容器的参数。从代码中可以看出，这是我们定义 pod 的 Docker `image`(我们想要使用的图像)和容器的`name`的地方，它应该出现在 Kubernetes 中。这也是我们定义在容器上公开的`port`的地方，它将允许对正在运行的应用程序的入口访问。这些值来自同一文件夹中的`variables.tf`文件。Terraform Kubernetes 部署资源能够执行非常健壮的配置。我鼓励您和您的团队尝试其他一些属性，以便更广泛地熟悉这个工具。

### 细目:services.tf

我们已经创建了一个 [Terraform Kubernetes 部署资源](https://www.terraform.io/docs/providers/kubernetes/r/deployment.html)文件，并为这个应用程序定义了我们的 Kubernetes 部署。剩下一个细节来完成我们的应用程序的部署。我们正在部署的应用程序是一个基本的网站。和所有的网站一样，它需要是可访问的才是有用的。此时，我们的`deployments.tf`文件指定了使用 Docker 映像部署 Kubernetes pod 的指令以及所需的 pod 数量。我们的部署缺少一个关键要素:Kubernetes 服务。这是一种将运行在一组 pod 上的应用程序作为网络服务公开的抽象方式。使用 Kubernetes，您不需要修改应用程序来使用不熟悉的服务发现机制。Kubernetes 为一组 pods 提供它们自己的 IP 地址和一个 DNS 名称，并且可以在它们之间进行负载平衡。

在`services.tf`文件中，我们定义了一个 [Terraform Kubernetes 服务](https://www.terraform.io/docs/providers/kubernetes/r/service.html)。它将连接 Kubernetes 元素，以提供对集群中 pods 上运行的应用程序的入口访问。这里是`services.tf`文件。

```
resource "kubernetes_service" "app" {
  metadata {
    name = var.app
  }
  spec {
    selector = {
      app = kubernetes_deployment.app.metadata.0.labels.app
    }
    port {
      port        = 80
      target_port = 5000
    }
    type = "LoadBalancer"
  }
} 
```

此时，描述一下`spec{...}`块和其中的元素可能会有所帮助。`selector{ app...}`块指定了一个在`deployments.tf`文件中定义的名称，并表示部署资源中元数据块的`label`属性中的`app`值。这是一个重用相关资源中已经分配的值的例子。它还提供了一种机制来简化重要的值，并为像这样的重要数据建立一种形式的引用完整性。

`port{...}`块有两个属性:`port`和`target_port`。这些参数定义了服务将侦听应用程序请求的外部端口。在本例中，它是端口 80。`target_port`是我们的 pod 正在监听的内部端口，即端口 5000。该服务将所有流量从端口 80 路由到端口 5000。

这里要回顾的最后一个元素是`type`参数，它指定了我们正在创建的服务的类型。Kubernetes 有三种[服务](https://kubernetes.io/docs/concepts/services-networking/service/#publishing-services-service-types)。在这个例子中，我们使用的是`LoadBalancer`类型，它使用云提供商的负载均衡器对外公开服务。外部负载平衡器路由到的 NodePort 和 ClusterIP 服务是自动创建的。在这种情况下，GCP 将创建并配置一个`LoadBalancer`，它将控制流量并将其路由到我们的 GKE 集群。

### 细分:output.tf

Terraform 使用[输出值](https://docs.docker.com/get-started/overview/#docker-objects)返回 Terraform 模块的值，该模块在运行`terraform apply`后向子模块提供输出。这些输出用于向父模块公开其资源属性的子集，或者在 CLI 输出中打印某些值。`output.tf`块我们使用输出值来读出集群名称和我们新创建的负载平衡器服务的入口 IP 地址等值。这个地址是我们可以访问托管在 GKE 集群上的应用程序的地方。

```
 output "gke_cluster" {
    value = var.cluster
  }

  output "endpoint" {
    value = kubernetes_service.app.load_balancer_ingress.0.ip
  } 
```

## 初始化地形零件 02/iac_gke_cluster

现在您已经对我们的 Terraform 项目和语法有了更好的理解，您可以开始使用 Terraform 配置我们的 GKE 集群了。将目录更改为`part02/iac_gke_cluster`目录:

```
cd part02/iac_gke_cluster 
```

在`part02/iac_gke_cluster`中，运行以下命令:

```
terrform init 
```

这是我的输出。

```
Initializing the backend...

Successfully configured the backend "remote"! Terraform will automatically
use this backend unless the backend configuration changes.

Initializing provider plugins...
- Checking for available provider plugins...
- Downloading plugin for provider "google" (hashicorp/google) 3.31.0...

The following providers do not have any version constraints in configuration,
so the latest version was installed.

To prevent automatic upgrades to new major versions that may contain breaking
changes, it is recommended to add version = "..." constraints to the
corresponding provider blocks in configuration, with the constraint strings
suggested below.

* provider.google: version = "~> 3.31"

Terraform has been successfully initialized! 
```

这太棒了！现在我们可以创建 GKE 集群了。

### 地形应用零件 02/iac_gke_cluster

Terraform 有一个命令，允许您在不实际执行任何操作的情况下，模拟运行和验证您的 Terraform 代码。该命令名为`terraform plan`，它还绘制了 Terraform 将针对您现有的基础设施执行的所有操作和更改。在终端中，运行:

```
terraform plan 
```

这是我的输出。

```
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.
------------------------------------------------------------------------
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
  ... 
```

Terraform 将根据`main.tf`文件中的代码为您创建新的 GCP 资源。现在，您已经准备好创建新的基础设施并部署应用程序了。在终端中运行以下命令:

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

Terraform 将在 GCP 建立新的 Google Kubernetes 引擎集群。

**注意** : *集群需要 3-5 分钟才能完成。这不是一个即时的过程，因为后端系统正在进行配置并使其上线。*

在我的集群完成之后，这就是我的输出。

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

接下来，我们将使用`part02/iac_kubernetes_app/`目录中的代码来创建一个 Kubernetes 部署和附带的`LoadBalancer`服务。

### Terraform 初始化 part02/iac_kubernetes_app/

我们现在可以使用`part02/iac_kubernetes_app/`目录中的代码将我们的应用程序部署到这个 GKE 集群。使用以下命令将目录更改为目录:

```
cd part02/iac_kubernetes_app/ 
```

在`part02/iac_kubernetes_app/`中，运行此命令初始化 Terraform 项目:

```
terrform init 
```

这是我的输出。

```
Initializing the backend...

Successfully configured the backend "remote"! Terraform will automatically
use this backend unless the backend configuration changes.

Initializing provider plugins...
- Checking for available provider plugins...
- Downloading plugin for provider "kubernetes" (hashicorp/kubernetes) 1.11.3...

The following providers do not have any version constraints in configuration,
so the latest version was installed.

To prevent automatic upgrades to new major versions that may contain breaking
changes, it is recommended to add version = "..." constraints to the
corresponding provider blocks in configuration, with the constraint strings
suggested below.

* provider.kubernetes: version = "~> 1.11"

Terraform has been successfully initialized! 
```

### GKE 集群凭据

使用 Terraform 创建`google_container_cluster`后，需要对集群进行身份验证。您可以使用 [Google Cloud CLI](https://cloud.google.com/sdk/docs/quickstarts) 来配置集群访问，并生成一个 [kubeconfig](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/) 文件。执行以下命令:

```
gcloud container clusters get-credentials cicd-workshops --zone="us-east1-d" 
```

使用这个命令，`gcloud`将生成一个使用`gcloud`作为认证机制的 kubeconfig 条目。该命令使用`cicd-workshops`值作为集群名称，该名称也在`variables.tf`中指定。

### Terraform 应用 part02/iac_kubernetes_app/

最后，我们准备使用 Terraform 将我们的应用程序部署到 GKE 集群。执行以下命令:

```
terraform plan 
```

这是我的输出。

```
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.
------------------------------------------------------------------------
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create
Terraform will perform the following actions:
  # kubernetes_deployment.app will be created
  + resource "kubernetes_deployment" "app" {
      + id = (known after apply)
      + metadata {
          + generation       = (known after apply)
          + labels           = {
              + "app" = "cicd-101"
            }
          + name             = "cicd-101"
          + namespace        = "default"
          + resource_version = (known after apply)
          + self_link        = (known after apply)
          + uid              = (known after apply)
        }
  ... 
```

Terraform 将根据`deployment.tf`和`services.tf`文件中的代码为您创建新的 GCP 资源。现在，您可以创建新的基础设施并部署应用程序。在终端中运行以下命令:

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

Terraform 将为您构建新的 Kubernetes 应用程序部署和相关的负载平衡器。

**注意** : *集群需要 3-5 分钟才能完成。这不是一个即时的过程，因为后端系统正在进行配置并使其上线。*

在我的集群完成之后，这就是我的输出。

```
Apply complete! Resources: 2 added, 0 changed, 0 destroyed.

Outputs:

endpoint = 104.196.222.238
gke_cluster = cicd-workshops 
```

该应用程序现在已经部署。`endpoint`值和输出是集群`LoadBalancer`的公共入口的 IP 地址。它还表示您可以访问应用程序的地址。打开网络浏览器，使用`output`值访问应用程序。将会出现一个网页，内容为“欢迎使用 CircleCI 学习 CI/CD 101！”。

### 使用地形摧毁

您已经证明您的 Kubernetes 部署工作正常，并且将应用程序部署到 GKE 集群已经过成功测试。你可以让它继续运行，但要知道，在谷歌云平台上运行任何资产都是有成本的，你要为这些成本负责。谷歌为其免费试用注册提供了慷慨的 300 美元信用，但如果你让资产运行，你可以很容易地吃掉它。

运行`terraform destroy`将终止您在本教程中创建的任何正在运行的资源。

运行以下命令销毁 GKE 集群。

```
terraform destroy 
```

请记住，上面的命令只会销毁`part02/iac_kubeernetes_app/`部署，您需要运行以下命令来销毁在本教程中创建的*所有*资源。

```
cd ../iac_gke-cluster/

terraform destroy 
```

这将破坏我们之前创建的 GKE 集群。

## 结论

恭喜你！您已经完成了本系列的第 2 部分，并且通过构建和发布新的 Docker 映像，以及使用基础设施作为代码和平台将应用程序配置和部署到 Kubernetes 集群，提升了您的体验。

继续本教程的第 3 部分,您将学习如何使用 CircleCI 将所有这些令人惊叹的知识自动化到 CI/CD 管道中。

以下资源将帮助您扩展知识: