# 基础设施即代码，第 3 部分:使用 CI/CD 和 Terraform | CircleCI 自动化 Kubernetes 部署

> 原文：<https://circleci.com/blog/learn-iac-part3/>

本系列向您展示如何开始使用基础设施即代码(IaC)。目标是通过教程和代码示例帮助开发人员加深对 IaC 的理解。

在这篇文章中，我将演示如何创建[持续集成和部署(CI/CD)](https://circleci.com/continuous-integration/) 管道，这些管道自动化了本系列的[第 1 部分](https://circleci.com/blog/learn-iac-part1/)和[第 2 部分](https://circleci.com/blog/learn-iac-part02/)中涉及的[地形](https://www.terraform.io/) IaC 部署。下面是我们将在这篇文章中完成的事情的快速列表:

1.  为项目构建一个新的 [CircleCI .config.yml 文件](https://circleci.com/docs/configuration-reference/)
2.  配置新的[作业](https://circleci.com/docs/configuration-reference/#jobs)和[工作流](https://circleci.com/docs/configuration-reference/#workflows)
3.  自动执行 Terraform 代码以创建 [Google Kubernetes 引擎(GKE)集群](https://cloud.google.com/kubernetes-engine)并部署应用程序

**注意:** *在你开始学习这部分教程之前，确保你已经完成了第 1 部分的[先决条件部分中的所有操作。](https://circleci.com/blog/learn-iac-part1/)*

我们将从快速解释什么是 CI/CD 开始，并回顾本系列教程的前两部分。然后你就可以开始了解[这个代码报告中包含的](https://github.com/datapunkz/learn_iac) [CircleCI .config.yml 文件](https://circleci.com/docs/configuration-reference/)。

## 持续集成和持续部署

CI/CD 管道帮助开发人员和团队自动化他们的构建和测试过程。CI/CD 创建了有价值的反馈循环，提供了软件开发过程的接近实时的状态。CI/CD 自动化还提供了一致的流程执行和准确的结果。这有助于优化这些流程，并有助于提高速度。用 CI/CD 简化开发实践正在成为团队中的普遍实践。理解如何集成和自动化重复的任务对于构建有价值的 CI/CD 管道至关重要。

在[第 1 部分](https://circleci.com/blog/learn-iac-part1/)和[第 2 部分](https://circleci.com/blog/learn-iac-part02/)中，我们使用 [Terraform](https://www.terraform.io/) 创建了一个新的 GKE 集群和相关的 Kubernetes 对象，用于部署、执行和服务应用程序。这些地形命令是从我们的终端手动执行的。这在你开发 Terraform 代码或者修改它的时候很有效，但是我们希望自动执行这些命令。有许多方法可以使它们自动化，但是我们将把重点放在如何从 CI/CD 管道中完成。

## 什么是 CircleCI 管道？

[CircleCI pipelines](https://circleci.com/docs/concepts/#pipelines) 是在您启动项目工作时运行的全套流程。管道包含您的工作流程，进而协调您的工作。这都是在项目配置文件中定义的。在本教程的下一节中，我们将定义一个 CI/CD 管道来构建我们的项目。

### 在 CircleCI 建立项目

在我们开始为这个项目构建一个`config.yml`文件之前，我们需要将这个项目添加到 CircleCI 中。如果你不熟悉这个过程，你可以在这里使用[设置 CircleCI 指南。一旦您完成了**设置 CircleCI** 部分，就此停止，以便我们可以配置](https://circleci.com/docs/getting-started/#setting-up-circleci)[项目级环境变量](https://circleci.com/docs/env-vars/#setting-an-environment-variable-in-a-project)。

### 项目级环境变量

此管道中的某些作业将需要访问身份验证凭据，以便在目标服务上执行命令。在本节中，我们将定义一些作业所需的凭证，并演示如何将它们作为项目级环境变量输入到 CircleCI 中。对于每个变量，在**名称**字段中输入`EnVar Name:` 值，在**值**字段中输入凭证。以下是我们的管道将需要的凭证及其值的列表:

一旦上面所有的环境变量都就位，我们就可以开始在`config.yml file`中构建我们的管道。

### CircleCI config.yml

[config.yml](https://circleci.com/docs/configuration-reference/) 文件是您定义要处理和执行的 CI/CD 相关作业的地方。在本节中，我们将为我们的管道定义作业和工作流。

在编辑器中打开`.circleci/.config.yml`文件，删除其内容并粘贴以下代码:

```
version: 2.1
jobs: 
```

`version:`键指定运行该管道时要使用的平台特性。`jobs:`键代表我们将为该管道定义的单个[作业](https://circleci.com/docs/configuration-reference/#jobs)的列表。接下来，我们将创建管道将要执行的作业。

### 作业运行测试:

我鼓励您熟悉本 [CircleCI 参考文档](https://circleci.com/docs/configuration-reference/)中的特殊按键、功能和特性，这将有助于您获得平台经验。下面是我们将要讨论的工作中每个关键点的概述和解释。

*   [**docker:**](https://circleci.com/docs/jobs-steps/#steps-overview) 是一个键，代表我们的作业将要执行的运行时
    *   [**图像:**](https://circleci.com/docs/jobs-steps/#steps-overview) 是一个键，表示 Docker 容器要用于这个作业
*   [**步骤:**](https://circleci.com/docs/jobs-steps/#steps-overview) 是一个键，代表一个作业期间运行的可执行命令的列表或集合
    *   [**签出:**](https://circleci.com/docs/configuration-reference/#checkoutt) 是一个键，是一个特殊的步骤，用来将源代码签出到配置好的路径
    *   [**运行:**](https://circleci.com/docs/configuration-reference/#run) 是用来调用所有命令行程序的一个键
        *   [**名称:**](https://circleci.com/docs/configuration-reference/#run) 是一个按键，表示在 CircleCI 界面中要显示的步骤的标题
        *   [**命令:**](https://circleci.com/docs/configuration-reference/#run) 是一个代表命令通过 shell 运行的键
    *   [**store _ test _ results:**](https://circleci.com/docs/configuration-reference/#store_test_results)是一个键，表示一个特殊的步骤，用于上传和存储一个构建的测试结果
        *   [**path:**](https://circleci.com/docs/configuration-reference/#store_test_results) 是指向包含 JUnit XML 或 Cucumber JSON 测试元数据文件子目录的目录的路径(绝对路径，或相对于您的`working_directory`)
    *   [**store_artifacts:**](https://circleci.com/docs/configuration-reference/#store_artifacts) 是一个键，表示在 web 应用程序中或通过 API 存储可用性工件(例如日志、二进制文件等)的步骤
        *   [**路径:**](https://circleci.com/docs/configuration-reference/#store_artifacts) 是用于保存作业工件的主容器中目录的路径

CI/CD 的一个有价值的好处是能够对新编写的代码执行[自动化测试](https://en.wikipedia.org/wiki/Test_automation)。它通过在每次修改代码时对代码执行测试来帮助识别代码中已知和未知的错误。

我们的下一步是在`config.yml`文件中定义一个新任务。将以下内容粘贴到文件中:

```
 run_tests:
    docker:
      - image: circleci/node:12
    steps:
      - checkout
      - run:
          name: Install npm dependencies
          command: |
            npm install --save
      - run:
          name: Run Unit Tests
          command: |
            ./node_modules/mocha/bin/mocha test/ --reporter mochawesome --reporter-options reportDir=test-results,reportFilename=test-results
      - store_test_results:
          path: test-results 
```

这是我们刚刚添加的内容的明细。

*   **docker:** 和 **image:** 键指定了我们在这项工作中使用的执行者和 docker 图像
*   **命令:`npm install --save`** 键安装应用程序中使用的应用程序依赖项
*   **名称:运行单元测试**执行自动化测试，并将它们保存到名为`test-results/`的本地目录中
*   **store_test_results:** 是一个特殊的命令，它将`test-results/`目录结果保存并固定到 CircleCI 中的构建中

这项工作作为一个单元测试功能。它有助于识别代码中的错误。如果这些测试中的任何一个失败，整个管道构建都将失败，并提示开发人员修复错误。目标是通过所有的测试和作业。接下来，我们将创建一个构建 Docker 映像的作业，并将其推送到 [Docker Hub registry](https://hub.docker.com/) 。

### 工单-构建 _docker_image

在本系列的第 2 部分中，我们手工创建了一个 Docker 映像，并将其推送到 Docker Hub 注册中心。在这项工作中，我们将使用自动化来完成这项任务。将该代码块追加到`config.yml`文件中:

```
 build_docker_image:
    docker:
      - image: circleci/node:12
    steps:
      - checkout
      - setup_remote_docker:
          docker_layer_caching: false
      - run:
          name: Build Docker image
          command: |
            export TAG=0.2.<< pipeline.number >>
            export IMAGE_NAME=$CIRCLE_PROJECT_REPONAME            
            docker build -t $DOCKER_LOGIN/$IMAGE_NAME -t $DOCKER_LOGIN/$IMAGE_NAME:$TAG .
            echo $DOCKER_PWD | docker login -u $DOCKER_LOGIN --password-stdin
            docker push $DOCKER_LOGIN/$IMAGE_NAME 
```

这项工作相当简单。你已经遇到了它使用的大多数 CircleCI YAML 键，所以我将直接跳到`name: Build Docker Image`命令块。

*   `export TAG=0.2.<< pipeline.number >>`行定义了一个本地环境变量，该变量使用 [pipeline.number](https://circleci.com/docs/configuration-reference/#using-pipeline-values) 值将 Docker 标签值与正在执行的管道号相关联
*   `export IMAGE_NAME=$CIRCLE_PROJECT_REPONAME`定义了我们将在命名 Docker 图像时使用的变量
*   `docker build -t $DOCKER_LOGIN/$IMAGE_NAME -t $DOCKER_LOGIN/$IMAGE_NAME:$TAG .`使用我们之前设置的项目级变量和我们指定的本地环境变量的组合来执行 Docker 构建命令
*   `echo $DOCKER_PWD | docker login -u $DOCKER_LOGIN --password-stdin`验证我们的 Docker Hub 证书以访问平台
*   `docker push $DOCKER_LOGIN/$IMAGE_NAME`将新的 docker 映像上传到 Docker Hub 注册表

这些应该看起来和感觉起来都很熟悉，因为这些命令与您在第 2 部分中手动运行的命令完全相同。在本例中，我们添加了环境变量命名位。接下来，我们将构建一个作业来执行构建 GKE 集群的 Terraform 代码。

### 作业- gke_create_cluster

在这项工作中，我们将自动执行在`part03/iac_gke_cluster/`目录中找到的 Terraform 代码。将这个代码块附加到`config.yml`文件，然后保存它:

```
 gke_create_cluster:
    docker:
      - image: ariv3ra/terraform-gcp:latest
    environment:
      CLOUDSDK_CORE_PROJECT: cicd-workshops
    steps:
      - checkout
      - run:
          name: Create GKE Cluster
          command: |
            echo $TF_CLOUD_TOKEN | base64 -d > $HOME/.terraformrc
            echo $GOOGLE_CLOUD_KEYS | base64 -d > $HOME/gcloud_keys
            gcloud auth activate-service-account --key-file ${HOME}/gcloud_keys
            cd part03/iac_gke_cluster/
            terraform init
            terraform plan -var credentials=$HOME/gcloud_keys -out=plan.txt
            terraform apply plan.txt 
```

在这个代码块中需要注意的一点是执行器 Docker 图像`image: ariv3ra/terraform-gcp:latest`。这是我创建的一个安装了 [Google SDK](https://cloud.google.com/sdk/docs/quickstarts) 和 [Terraform CLI](https://www.terraform.io/) 的图像。如果我们不使用它，我们将需要在这个作业中添加安装步骤，以便每次安装和配置工具。`environment: CLOUDSDK_CORE_PROJECT: cicd-workshops`键也是一个重要的元素。这设置了我们稍后将执行的`gcloud cli`命令所需的环境变量值。

代码块中使用的其他元素:

*   `echo $TF_CLOUD_TOKEN | base64 -d > $HOME/.terraformrc`是解码`$TF_CLOUD_TOKEN`值的命令，它创建 Terraform 访问相应 Terraform 云工作空间上的状态数据所需的`./terraformrc`文件
*   `echo $GOOGLE_CLOUD_KEYS | base64 -d > $HOME/gcloud_keys`是解码`$GOOGLE_CLOUD_KEYS`值的命令，它创建了`glcoud cli`访问 GCP 所需的`gcloud_keys`文件
*   `gcloud auth activate-service-account --key-file ${HOME}/gcloud_keys`是一个命令，它授权使用我们之前解码并生成的`gcloud_keys`文件访问 GCP

其余的命令是带有`-var`参数的`terraform cli`命令，这些命令指定并覆盖在各自 Terraform `variables.tf`文件中定义的变量的`default`值。一旦`terraform apply plan.txt`执行，这个作业将创建一个新的 GKE 集群。

### Job - gke_deploy_app

在这项工作中，我们将自动执行在`part03/iac_kubernetes_app/`目录中找到的 Terraform 代码。将此代码块追加到 config.yml 文件，然后保存它:

```
 gke_deploy_app:
    docker:
      - image: ariv3ra/terraform-gcp:latest
    environment:
      CLOUDSDK_CORE_PROJECT: cicd-workshops
    steps:
      - checkout
      - run:
          name: Deploy App to GKE
          command: |
            export CLUSTER_NAME="cicd-workshops"
            export TAG=0.2.<< pipeline.number >>
            export DOCKER_IMAGE="docker-image=${DOCKER_LOGIN}/${CIRCLE_PROJECT_REPONAME}:$TAG"
            echo $TF_CLOUD_TOKEN | base64 -d > $HOME/.terraformrc
            echo $GOOGLE_CLOUD_KEYS | base64 -d > $HOME/gcloud_keys
            gcloud auth activate-service-account --key-file ${HOME}/gcloud_keys
            gcloud container clusters get-credentials $CLUSTER_NAME --zone="us-east1-d"
            cd part03/iac_kubernetes_app
            terraform init
            terraform plan -var $DOCKER_IMAGE -out=plan.txt
            terraform apply plan.txt
            export ENDPOINT="$(terraform output endpoint)"
            mkdir -p /tmp/gke/ && echo 'export ENDPOINT='${ENDPOINT} > /tmp/gke/gke-endpoint
      - persist_to_workspace:
          root: /tmp/gke
          paths:
            - "*" 
```

下面是这个职务代码块的重要元素和一些我们以前没有讨论过的新元素。

*   定义一个变量，该变量保存我们将要部署到的 GCP 项目的名称。
*   `gcloud container clusters get-credentials $CLUSTER_NAME --zone="us-east1-d"`是一个从我们在上一个作业中创建的 GKE 集群中检索`kubeconfig`数据的命令。
*   `terraform plan -var $DOCKER_IMAGE -out=plan.txt`是一个命令，覆盖在各个地形`variables.tf`文件中定义的各个变量的`default`值。
*   `export ENDPOINT="$(terraform output endpoint)"`将 Terraform 命令生成的输出`endpoint`值赋给一个本地环境变量，该变量将被保存到一个文件中，并持久保存到 [CircleCI 工作区](https://circleci.com/docs/configuration-reference/#persist_to_workspace)。然后可以从[附属的 CircleCI 工作区](https://circleci.com/docs/configuration-reference/#attach_workspace)取回、附加并用于后续工作。

### 作业- gke_destroy_cluster

这项工作是我们将为此管道建立的最后一个。这将基本上摧毁我们在以前的 CI/CD 工作中建立的所有资源和基础设施。作为测试的一部分，短暂的资源被用于冒烟测试、集成测试、性能测试和其他类型的测试。当不再需要这些构造时，执行 destroy 命令的作业对于删除它们非常有用。

在这项工作中，我们将自动执行在`part03/iac_kubernetes_app/`目录中找到的 Terraform 代码。将此代码块追加到 config.yml 文件，然后保存它:

```
 gke_destroy_cluster:
    docker:
      - image: ariv3ra/terraform-gcp:latest
    environment:
      CLOUDSDK_CORE_PROJECT: cicd-workshops
    steps:
      - checkout
      - run:
          name: Destroy GKE Cluster
          command: |
            export CLUSTER_NAME="cicd-workshops"
            export TAG=0.2.<< pipeline.number >>
            export DOCKER_IMAGE="docker-image=${DOCKER_LOGIN}/${CIRCLE_PROJECT_REPONAME}:$TAG"            
            echo $TF_CLOUD_TOKEN | base64 -d > $HOME/.terraformrc
            echo $GOOGLE_CLOUD_KEYS | base64 -d > $HOME/gcloud_keys
            gcloud auth activate-service-account --key-file ${HOME}/gcloud_keys
            cd part03/iac_kubernetes_app
            terraform init
            gcloud container clusters get-credentials $CLUSTER_NAME --zone="us-east1-d"            
            terraform destroy -var $DOCKER_IMAGE --auto-approve
            cd ../iac_gke_cluster/
            terraform init
            terraform destroy -var credentials=$HOME/gcloud_keys --auto-approve 
```

该作业代码块的重要元素是`terraform destroy -var credentials=$HOME/gcloud_keys --auto-approve`命令。该命令执行 Terraform 命令，该命令销毁分别在`part03/iac_gke_cluster`和`part03/iac_kubernetes_app/`目录中使用 Terraform 代码创建的所有资源。

现在，我们已经定义了管道中的所有[作业](https://circleci.com/docs/configuration-reference/#jobs)，我们准备创建 [CircleCI 工作流](https://circleci.com/docs/configuration-reference/#workflows)，它将协调作业在管道中的执行和处理方式。

## 创建 CircleCI 工作流

我们的下一步是创建[工作流](https://circleci.com/docs/configuration-reference/#workflows)，它定义了如何执行和处理作业。将工作流视为作业的有序列表。您可以使用工作流指定何时以及如何执行这些作业。将此工作流代码块附加到`config.yml`文件:

```
workflows:
  build_test:
    jobs:
      - run_tests
      - build_docker_image
      - gke_create_cluster
      - gke_deploy_app:
          requires:
            - run_tests
            - build_docker_image
            - gke_create_cluster
      - approve-destroy:
          type: approval
          requires:
            - gke_create_cluster
            - gke_deploy_app
      - gke_destroy_cluster:
          requires:
            - approve-destroy 
```

这个代码块代表我们管道的[工作流](https://circleci.com/docs/configuration-reference/#workflows)定义。这个街区的情况是这样的:

*   `workflows:`键指定一个工作流元素
*   `build_test:`表示该工作流的名称/标识符
*   `jobs:`键代表在`config.yml`文件中定义的要执行的任务列表

在此列表中，您可以指定要在此管道中执行的作业。这是我们的工作清单:

```
 - run_tests
      - build_docker_image
      - gke_create_cluster
      - gke_deploy_app:
          requires:
            - run_tests
            - build_docker_image
            - gke_create_cluster
      - approve-destroy:
          type: approval
          requires:
            - gke_create_cluster
            - gke_deploy_app
      - gke_destroy_cluster:
          requires:
            - approve-destroy 
```

`run_tests`、`build_docker_image`和`gke_create_cluster`工作流[作业](https://circleci.com/docs/configuration-reference/#jobs-1)在[中并行或并发运行](https://circleci.com/docs/configuration-reference/#jobs-1)，不像`gke_deploy_app:`项目有一个`requires:`键。默认情况下，作业是并行运行的，因此您必须使用一个`requires:`键和一个在开始指定的作业之前必须完成的作业列表，通过它们的作业名称明确要求任何依赖项。把`requires:`键想象成建立在其他工作成功的基础上。这些键允许您分割和控制管道的执行。

`approve-destroy:`项指定具有[手动批准步骤](https://circleci.com/docs/configuration-reference/#type)的作业。它需要人工干预，必须有人批准执行工作流作业列表中的下一个作业。下一个作业`gke_destroy_cluster:`依赖于执行前正在完成的`approval-destroy:`作业。它会破坏管道中先前执行的作业所创建的所有资源。

## 完整的. config.yml 文件

完整的`config.yml`基于这个帖子，可以在`.circleci/`目录下的[项目代码报告](https://github.com/datapunkz/learn_iac)中找到。它在这里供您回顾:

```
version: 2.1
jobs:
  run_tests:
    docker:
      - image: circleci/node:12
    steps:
      - checkout
      - run:
          name: Install npm dependencies
          command: |
            npm install --save
      - run:
          name: Run Unit Tests
          command: |
            ./node_modules/mocha/bin/mocha test/ --reporter mochawesome --reporter-options reportDir=test-results,reportFilename=test-results
      - store_test_results:
          path: test-results
      - store_artifacts:
          path: test-results
  build_docker_image:
    docker:
      - image: circleci/node:12
    steps:
      - checkout
      - setup_remote_docker:
          docker_layer_caching: false
      - run:
          name: Build Docker image
          command: |
            export TAG=0.2.<< pipeline.number >>
            export IMAGE_NAME=$CIRCLE_PROJECT_REPONAME
            docker build -t $DOCKER_LOGIN/$IMAGE_NAME -t $DOCKER_LOGIN/$IMAGE_NAME:$TAG .
            echo $DOCKER_PWD | docker login -u $DOCKER_LOGIN --password-stdin
            docker push $DOCKER_LOGIN/$IMAGE_NAME
  gke_create_cluster:
    docker:
      - image: ariv3ra/terraform-gcp:latest
    environment:
      CLOUDSDK_CORE_PROJECT: cicd-workshops
    steps:
      - checkout
      - run:
          name: Create GKE Cluster
          command: |
            echo $TF_CLOUD_TOKEN | base64 -d > $HOME/.terraformrc
            echo $GOOGLE_CLOUD_KEYS | base64 -d > $HOME/gcloud_keys
            gcloud auth activate-service-account --key-file ${HOME}/gcloud_keys
            cd part03/iac_gke_cluster/
            terraform init
            terraform plan -var credentials=$HOME/gcloud_keys -out=plan.txt
            terraform apply plan.txt
  gke_deploy_app:
    docker:
      - image: ariv3ra/terraform-gcp:latest
    environment:
      CLOUDSDK_CORE_PROJECT: cicd-workshops
    steps:
      - checkout
      - run:
          name: Deploy App to GKE
          command: |
            export CLUSTER_NAME="cicd-workshops"
            export TAG=0.2.<< pipeline.number >>
            export DOCKER_IMAGE="docker-image=${DOCKER_LOGIN}/${CIRCLE_PROJECT_REPONAME}:$TAG"
            echo $TF_CLOUD_TOKEN | base64 -d > $HOME/.terraformrc
            echo $GOOGLE_CLOUD_KEYS | base64 -d > $HOME/gcloud_keys
            gcloud auth activate-service-account --key-file ${HOME}/gcloud_keys
            gcloud container clusters get-credentials $CLUSTER_NAME --zone="us-east1-d"
            cd part03/iac_kubernetes_app
            terraform init
            terraform plan -var $DOCKER_IMAGE -out=plan.txt
            terraform apply plan.txt
            export ENDPOINT="$(terraform output endpoint)"
            mkdir -p /tmp/gke/
            echo 'export ENDPOINT='${ENDPOINT} > /tmp/gke/gke-endpoint
      - persist_to_workspace:
          root: /tmp/gke
          paths:
            - "*"
  gke_destroy_cluster:
    docker:
      - image: ariv3ra/terraform-gcp:latest
    environment:
      CLOUDSDK_CORE_PROJECT: cicd-workshops
    steps:
      - checkout
      - run:
          name: Destroy GKE Cluster
          command: |
            export CLUSTER_NAME="cicd-workshops"
            export TAG=0.2.<< pipeline.number >>
            export DOCKER_IMAGE="docker-image=${DOCKER_LOGIN}/${CIRCLE_PROJECT_REPONAME}:$TAG"            
            echo $TF_CLOUD_TOKEN | base64 -d > $HOME/.terraformrc
            echo $GOOGLE_CLOUD_KEYS | base64 -d > $HOME/gcloud_keys
            gcloud auth activate-service-account --key-file ${HOME}/gcloud_keys
            cd part03/iac_kubernetes_app
            terraform init
            gcloud container clusters get-credentials $CLUSTER_NAME --zone="us-east1-d"            
            terraform destroy -var $DOCKER_IMAGE --auto-approve
            cd ../iac_gke_cluster/
            terraform init
            terraform destroy -var credentials=$HOME/gcloud_keys --auto-approve
workflows:
  build_test:
    jobs:
      - run_tests
      - build_docker_image
      - gke_create_cluster
      - gke_deploy_app:
          requires:
            - run_tests
            - build_docker_image
            - gke_create_cluster
      - approve-destroy:
          type: approval
          requires:
            - gke_create_cluster
            - gke_deploy_app
      - gke_destroy_cluster:
          requires:
            - approve-destroy 
```

## 结论

恭喜你！您刚刚完成了本系列的第 3 部分，并通过构建一个新的使用 [Terraform](https://www.terraform.io/) 执行 IaC 资源的`config.yml`文件提升了您的体验。这篇文章解释并演示了`config.yml`文件中的一些关键元素以及与 CircleCI 平台相关的内部概念。

在这个系列中，我们涵盖了许多概念和技术，如 Docker、GCP、Kubernetes、Terraform 和 CircleCI，并包含了一些使用它们的实践经验。我们还介绍了如何连接您的项目以使用 CircleCI，并利用 Terraform 代码在目标部署环境中测试您的应用程序。本系列旨在增加您对重要的 DevOps 概念、技术以及它们如何协同工作的了解。

我鼓励你自己和你的团队去尝试；更改代码，添加新的 Terraform 提供程序，以及重新配置 CI/CD 作业和管道。使用您所学到的知识和团队提出的其他想法的组合，互相挑战以完成发布目标。通过实验，你会学到比任何博客文章都多的东西。

感谢您关注本系列。希望你觉得有用。请随时在 Twitter [@punkdata](https://twitter.com/punkdata) 上寻求反馈。

以下资源将帮助您扩展知识: