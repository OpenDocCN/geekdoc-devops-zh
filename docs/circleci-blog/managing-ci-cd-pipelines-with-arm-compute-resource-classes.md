# 使用 Arm 计算资源类管理 CI/CD 管道| CircleCI

> 原文：<https://circleci.com/blog/managing-ci-cd-pipelines-with-arm-compute-resource-classes/>

[Arm 处理器和架构](https://en.wikipedia.org/wiki/ARM_architecture)正变得广泛可用，因为开发团队将它们作为许多应用基础设施中的计算节点。需要以经济高效的方式运行微服务、应用服务器、数据库和其他工作负载的组织将继续转向 Arm 架构。需要基于 Arm 的计算的 CircleCI 客户已经可以使用自托管的 runners，但是我们认识到更多的客户将需要使用基于 Arm 的计算来构建、测试和部署他们的应用。为了让开发人员能够在 CI/CD 管道中基于 Arm 的实例上运行代码，而无需自行维护基础设施，我们为所有 CircleCI 用户添加了[新的基于 Arm 的资源类](https://circleci.com/docs/arm-resources/)。在本教程中，我将介绍新的 [Arm 资源类](https://circleci.com/docs/arm-resources/)，并演示如何在您的管道中使用它们来构建、测试和部署 Arm 应用程序。

## 先决条件

在开始学习本教程之前，您需要完成一些任务:

## Arm 计算资源类

本教程的以下部分将演示如何在基于 Arm 的执行器上配置和执行 CI/CD 管道，以及如何使用 [Terraform](https://www.terraform.io/) an 基础架构作为代码，基于 [AWS Graviton2](https://aws.amazon.com/ec2/graviton/) 计算节点创建、部署和销毁 [AWS ECS 集群](https://aws.amazon.com/ecs/)。

## 在 config.yml 中实现 Arm 计算

下面的管道配置示例显示了如何定义 Arm 资源类。

```
version: 2.1
orbs:
  node: circleci/node@4.2.0
jobs:
  run-tests:
    machine:
      image: ubuntu-2004:202101-01
    resource_class: arm.medium
    steps:
      - checkout
      - node/install-packages:
          override-ci-command: npm install
          cache-path: ~/project/node_modules
      - run:
          name: Run Unit Tests
          command: |
            ./node_modules/mocha/bin/mocha test/ --reporter mochawesome --reporter-options reportDir=test-results,reportFilename=test-results
      - store_test_results:
          path: test-results
      - store_artifacts:
          path: test-results          
  build_docker_image:
    machine:
      image: ubuntu-2004:202101-01
    resource_class: arm.medium
    steps:
      - checkout
      - run:
          name: "Build Docker Image ARM V8"
          command: |
            export TAG='0.1.<< pipeline.number >>'
            export IMAGE_NAME=$CIRCLE_PROJECT_REPONAME
            docker build -t $DOCKER_LOGIN/$IMAGE_NAME -t $DOCKER_LOGIN/$IMAGE_NAME:$TAG .
            echo $DOCKER_PWD | docker login -u $DOCKER_LOGIN --password-stdin
            docker push -a $DOCKER_LOGIN/$IMAGE_NAME
workflows:
  build:
    jobs:
      - run-tests
      - build_docker_image 
```

在这个代码示例中，`run-tests:`作业展示了如何指定一个机器执行器，并为它分配一个 Arm 计算节点资源类。`image:`键指定分配给执行器的操作系统。`resource_class:`指定使用哪个 [CircleCI 资源类](https://circleci.com/docs/configuration-reference/#resourceclass)。在这种情况下，我们使用的是`arm.medium`资源类类型，它支持管道在 Arm 架构和资源上执行和构建代码。`build_docker_image:`作业是使用`arm.medium`资源类构建支持 Arm64 的 Docker 映像的好方法，可以放心地部署到 Arm 计算基础设施，如 [AWS Graviton2](https://aws.amazon.com/ec2/graviton/) 。

```
 version: 2.1
  orbs:
    node: circleci/node@4.2.0
  commands:
    install_terraform:
      description: "specify terraform version & architecture to use [amd64 or arm64]"
      parameters:
        version:
          type: string
          default: "0.13.5"
        arch:
          type: string
          default: "arm64"
      steps:
        - run:
            name: Install Terraform client
            command: |
              cd /tmp
              wget https://releases.hashicorp.com/terraform/<<
                parameters.version >>/terraform_<<
                parameters.version >>_linux_<< 
                parameters.arch >>.zip
              unzip terraform_<< parameters.version >>_linux_<<
                parameters.arch >>.zip
              sudo mv terraform /usr/local/bin
  jobs:
    run-tests:
      machine:
        image: ubuntu-2004:202101-01
      resource_class: arm.medium
      steps:
        - checkout
        - node/install-packages:
            override-ci-command: npm install
            cache-path: ~/project/node_modules
        - run:
            name: Run Unit Tests
            command: |
              ./node_modules/mocha/bin/mocha test/ --reporter mochawesome --reporter-options reportDir=test-results,reportFilename=test-results
        - store_test_results:
            path: test-results
        - store_artifacts:
            path: test-results          
    build_docker_image:
      machine:
        image: ubuntu-2004:202101-01
      resource_class: arm.medium
      steps:
        - checkout
        - run:
            name: "Build Docker Image ARM V8"
            command: |
              export TAG='0.1.<< pipeline.number >>'
              export IMAGE_NAME=$CIRCLE_PROJECT_REPONAME
              docker build -t $DOCKER_LOGIN/$IMAGE_NAME -t $DOCKER_LOGIN/$IMAGE_NAME:$TAG .
              echo $DOCKER_PWD | docker login -u $DOCKER_LOGIN --password-stdin
              docker push -a $DOCKER_LOGIN/$IMAGE_NAME
    deploy_aws_ecs:
      machine:
        image: ubuntu-2004:202101-01
      resource_class: arm.medium
      steps:
        - checkout
        - run:
            name: Create .terraformrc file locally
            command: echo "credentials \"app.terraform.io\" {token = \"$TERRAFORM_TOKEN\"}" > $HOME/.terraformrc
        - install_terraform:
            version: 0.14.2
            arch: arm64    
        - run:
            name: Deploy Application to AWS ECS Cluster
            command: |
              export TAG=0.1.<< pipeline.number >>
              export DOCKER_IMAGE_NAME="${DOCKER_LOGIN}/${CIRCLE_PROJECT_REPONAME}"
              cd terraform/aws/ecs
              terraform init
              terraform apply \
                -var docker_img_name=$DOCKER_IMAGE_NAME \
                -var docker_img_tag=$TAG \
                --auto-approve
    destroy_aws_ecs:
      machine:
        image: ubuntu-2004:202101-01
      resource_class: arm.medium
      steps:
        - checkout
        - run:
            name: Create .terraformrc file locally
            command: echo "credentials \"app.terraform.io\" {token = \"$TERRAFORM_TOKEN\"}" > $HOME/.terraformrc
        - install_terraform:
            version: 0.14.2
            arch: arm64    
        - run:
            name: Destroy the AWS ECS Cluster
            command: |
              cd terraform/aws/ecs
              terraform init
              terraform destroy --auto-approve              
  workflows:
    build:
      jobs:
        - run-tests
        - build_docker_image
        - deploy_aws_ecs
        - approve_destroy:
            type: approval
            requires:
              - deploy_aws_ecs
        - destroy_aws_ecs:
            requires:
              - approve_destroy 
```

## 部署到 AWS ECS

上一节中的代码示例显示了如何在管道中利用 Arm 资源类。在这一节中，我将向您展示如何扩展代码来创建 AWS 资源，例如 [ECS 集群](https://aws.amazon.com/ecs/)。我将用底层的 [AWS Graviton2 EC2](https://aws.amazon.com/ec2/graviton/) 计算节点创建这些资源，使用 [Terraform](https://www.terraform.io/docs/cli-index.html) 和基础设施作为代码。

```
 version: 2.1
  orbs:
    node: circleci/node@4.2.0
  commands:
    install_terraform:
      description: "specify terraform version & architecture to use [amd64 or arm64]"
      parameters:
        version:
          type: string
          default: "0.13.5"
        arch:
          type: string
          default: "arm64"
      steps:
        - run:
            name: Install Terraform client
            command: |
              cd /tmp
              wget https://releases.hashicorp.com/terraform/<<
                parameters.version >>/terraform_<<
                parameters.version >>_linux_<< 
                parameters.arch >>.zip
              unzip terraform_<< parameters.version >>_linux_<<
                parameters.arch >>.zip
              sudo mv terraform /usr/local/bin
  jobs:
    run-tests:
      machine:
        image: ubuntu-2004:202101-01
      resource_class: arm.medium
      steps:
        - checkout
        - node/install-packages:
            override-ci-command: npm install
            cache-path: ~/project/node_modules
        - run:
            name: Run Unit Tests
            command: |
              ./node_modules/mocha/bin/mocha test/ --reporter mochawesome --reporter-options reportDir=test-results,reportFilename=test-results
        - store_test_results:
            path: test-results
        - store_artifacts:
            path: test-results          
    build_docker_image:
      machine:
        image: ubuntu-2004:202101-01
      resource_class: arm.medium
      steps:
        - checkout
        - run:
            name: "Build Docker Image ARM V8"
            command: |
              export TAG='0.1.<< pipeline.number >>'
              export IMAGE_NAME=$CIRCLE_PROJECT_REPONAME
              docker build -t $DOCKER_LOGIN/$IMAGE_NAME -t $DOCKER_LOGIN/$IMAGE_NAME:$TAG .
              echo $DOCKER_PWD | docker login -u $DOCKER_LOGIN --password-stdin
              docker push -a $DOCKER_LOGIN/$IMAGE_NAME
    deploy_aws_ecs:
      machine:
        image: ubuntu-2004:202101-01
      resource_class: arm.medium
      steps:
        - checkout
        - run:
            name: Create .terraformrc file locally
            command: echo "credentials \"app.terraform.io\" {token = \"$TERRAFORM_TOKEN\"}" > $HOME/.terraformrc
        - install_terraform:
            version: 0.14.2
            arch: arm64    
        - run:
            name: Deploy Application to AWS ECS Cluster
            command: |
              export TAG=0.1.<< pipeline.number >>
              export DOCKER_IMAGE_NAME="${DOCKER_LOGIN}/${CIRCLE_PROJECT_REPONAME}"
              cd terraform/aws/ecs
              terraform init
              terraform apply \
                -var docker_img_name=$DOCKER_IMAGE_NAME \
                -var docker_img_tag=$TAG \
                --auto-approve
    destroy_aws_ecs:
      machine:
        image: ubuntu-2004:202101-01
      resource_class: arm.medium
      steps:
        - checkout
        - run:
            name: Create .terraformrc file locally
            command: echo "credentials \"app.terraform.io\" {token = \"$TERRAFORM_TOKEN\"}" > $HOME/.terraformrc
        - install_terraform:
            version: 0.14.2
            arch: arm64    
        - run:
            name: Destroy the AWS ECS Cluster
            command: |
              cd terraform/aws/ecs
              terraform init
              terraform destroy --auto-approve              
  workflows:
    build:
      jobs:
        - run-tests
        - build_docker_image
        - deploy_aws_ecs
        - approve_destroy:
            type: approval
            requires:
              - deploy_aws_ecs
        - destroy_aws_ecs:
            requires:
              - approve_destroy 
```

这段代码扩展了原始的管道配置示例。您可能已经注意到，已经定义了一些新的工作。`deploy_aws_ecs:`、`approve_destroy:`和`destroy_aws_ecs:`作业是这个扩展配置中的新元素。在我深入研究它们之前，我将描述一下`commands:`和`install_terraform:`元素。

### install_terraform:命令

CircleCI 能够使用[管道参数](https://circleci.com/docs/pipeline-variables/#pipeline-parameters-in-configuration)封装和重用配置代码。`install_terraform:`命令是定义可重用管道代码的一个例子。如果您的管道重复执行特定的命令，我建议定义可重用的`command:`元素，以提供可扩展和集中管理的管道配置。`deploy_aws_ecs:`和`destroy_aws_ecs:`作业都执行 Terraform 代码，因此管道需要多次下载并安装 [Terraform cli](https://www.terraform.io/docs/cli-index.html) 。`install_terraform:`命令提供了宝贵的可重用性。

```
 commands:
    install_terraform:
      description: "specify terraform version & architecture to use [amd64 or arm64]"
      parameters:
        version:
          type: string
          default: "0.13.5"
        arch:
          type: string
          default: "arm64"
      steps:
        - run:
            name: Install Terraform client
            command: |
              cd /tmp
              wget https://releases.hashicorp.com/terraform/<<
                parameters.version >>/terraform_<<
                parameters.version >>_linux_<< 
                parameters.arch >>.zip
              unzip terraform_<< parameters.version >>_linux_<<
                parameters.arch >>.zip
              sudo mv terraform /usr/local/bin 
```

该代码块定义了`install_terraform:`可重用命令。`parameters:`键维护一个参数列表。参数`version:`和`arch:`分别定义了 Terraform CLI 版本和 CPU 架构。这些参数下载并在执行器中安装客户机。因为这个代码块代表一个`command:`元素，所以必须定义一个命令`steps:`键。在前面的例子中，`run:`元素执行相应的`command:`键。该键使用`<< parameter.version >>`和`<< parameter.arch >>`变量下载特定的 Terraform 客户端，以指定客户端版本号和 CPU 架构。[管道参数](https://circleci.com/docs/pipeline-variables/#pipeline-parameters-in-configuration)对于优化和集中管理管道配置中的功能非常有用。如果你想了解更多，你可以在这里得到所有的细节。

### 部署 _aws_ecs 作业

管道中定义的`deploy_aws_ecs:`作业利用基础设施作为代码来创建新的 Amazon ECS 集群。它包括所有必需的资源，如虚拟专用网络(VPC)、子网、路由表、应用程序负载平衡器和 EC2 自动扩展组。这项工作创建和调配部署和运行应用程序所需的所有基础架构。因为目标架构是 Arm，所以 AWS ECS 集群必须由 AWS Gravtion2 ECS 计算节点组成。这些节点将在之前的流水线作业中执行基于 Arm 的 Docker 应用映像构建。

```
 deploy_aws_ecs:
    machine:
      image: ubuntu-2004:202101-01
    resource_class: arm.medium
    steps:
      - checkout
      - run:
          name: Create .terraformrc file locally
          command: echo "credentials \"app.terraform.io\" {token = \"$TERRAFORM_TOKEN\"}" > $HOME/.terraformrc
      - install_terraform:
          version: 0.14.2
          arch: arm64    
      - run:
          name: Deploy Application to AWS ECS Cluster
          command: |
            export TAG=0.1.<< pipeline.number >>
            export DOCKER_IMAGE_NAME="${DOCKER_LOGIN}/${CIRCLE_PROJECT_REPONAME}"
            cd terraform/aws/ecs
            terraform init
            terraform apply \
              -var docker_img_name=$DOCKER_IMAGE_NAME \
              -var docker_img_tag=$TAG \
              --auto-approve 
```

这个代码块演示了如何使用我之前描述的`install_terraform:`命令。我们已经将`version:`参数设置为 0.14.2，将`arch:`参数设置为 arm64。最后的`run:`元素初始化 terraform，然后代码使用相应的参数执行一个`terraform apply`命令，这些参数传递在管道运行中创建的 Docker 图像名称和标记的值。完成后，该作业将创建应用程序，并将其部署到基于 AWS ECS Graviton2 的全功能集群中。

### 销毁 _aws_ecs 作业

我们在`deploy_aws_ecs`中创建了 AWS ECS 基础架构。`destroy_aws_ecs`作业显然执行相反的操作，以编程方式破坏所有创建的基础设施和资源。这是终止不必要的基础设施的最干净的方法。

```
 destroy_aws_ecs:
    machine:
      image: ubuntu-2004:202101-01
    resource_class: arm.medium
    steps:
      - checkout
      - run:
          name: Create .terraformrc file locally
          command: echo "credentials \"app.terraform.io\" {token = \"$TERRAFORM_TOKEN\"}" > $HOME/.terraformrc
      - install_terraform:
          version: 0.14.2
          arch: arm64    
      - run:
          name: Destroy the AWS ECS Cluster
          command: |
            cd terraform/aws/ecs
            terraform init
            terraform destroy --auto-approve 
```

在这个代码块中，除了最后一个`run:`元素之外，大部分作业定义与前一个相同。在这个元素中，我们发出了一个 Terraform 初始化和`terraform destroy`命令，正如所料，这个命令将销毁在前面的步骤中创建的所有资源。

### 工作流:批准 _ 销毁作业

我将讨论的最后一项是在配置示例的`workflows:`元素中找到的`approve_destroy:`。该作业是一种[手动批准类型](https://circleci.com/docs/workflows/#holding-a-workflow-for-a-manual-approval)，其中工作流将被有意停止并保持暂停状态，直到手动交互完成。在这种情况下，必须按下 CircleCI 仪表板上的按钮，以便执行`destroy-aws-ecs:`。如果没有这个批准作业，管道将自动触发销毁作业，并终止在以前的作业中创建的所有资源。在管道执行中需要手动干预或批准的情况下，批准类型的作业非常有用。

## 结论

CircleCI 以 Arm 计算节点的形式推出了支持 Arm 的执行器，让开发人员能够访问 Arm 架构的管道。在本教程中，我演示了如何将 CircleCI Arm 计算节点实现为管道执行器。我还展示了如何使用 Terraform 和 infrastructure 作为代码将应用程序部署到由 AWS Graviton2 EC2 节点支持的 AWS ECS 集群。本教程中的所有代码示例都可以在 GitHub 的 arm-executors repo 中找到，我强烈建议你去看看。我很想听到你的反馈、想法和意见，所以请发推特给我 [@punkdata](https://twitter.com/punkdata) 加入讨论。

感谢阅读！