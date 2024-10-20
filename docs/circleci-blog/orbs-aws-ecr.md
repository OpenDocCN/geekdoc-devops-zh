# 使用 CircleCI AWS ECR orb 构建 CI/CD 管道

> 原文：<https://circleci.com/blog/orbs-aws-ecr/>

CircleCI 最近发布了一款名为 [orbs](https://circleci.com/blog/announcing-orbs-technology-partner-program/) 的新产品，旨在让你在 CircleCI 上快速启动并运行。现在，您可以轻松地将您的 DevOps 工具与我们的技术合作伙伴提供的可信 orb 相集成。

在这篇文章中，我将演示和解释 [AWS ECR Orb](https://circleci.com/developer/orbs/orb/circleci/aws-ecr) 及其在 CircleCI 配置文件中的用法，利用 [CircleCI 工作流](https://circleci.com/docs/workflows/)，并将图像推送到 [Docker Hub](https://hub.docker.com/) 和指定的 AWS ECR。

## 什么是 CircleCI 球体？

orb 是 CircleCI 配置的包，可以跨项目共享。orb 允许您创建一个作业、命令和执行器的捆绑包，它们可以相互引用，可以导入到 CircleCI 构建配置中，并在它们自己的名称空间中调用。orb 在 CircleCI 注册，使用 semver 模式表示修订。CircleCI orbs 由 [CircleCI Orbs Registry](https://circleci.com/developer/orbs) 托管和维护，该注册中心是 CircleCI Orbs 的集中存储库。

## 先决条件

在你开始之前，你需要有这些东西:

完成所有先决条件后，就可以继续下一部分了。

## 配置 CircleCI 项目环境变量

我们项目的 CI/CD 管道将 Docker 映像部署到多个容器存储库，因此您需要在 [CircleCI 项目设置](https://circleci.com/docs/env-vars/#setting-an-environment-variable-in-a-project)中设置一些环境变量:

*   点击左侧菜单中 CircleCI 仪表板上的**添加项目**
*   在项目列表中找到并点击项目名称，点击右侧的**设置项目**
*   点击右上方 CircleCI 仪表盘中的**项目重心**按钮
*   在**构建设置**部分，点击**环境变量**
*   点击**添加变量**

在`Add an Environment Variable`对话框中，你将定义这个构建所需的几个环境变量。下面是必须定义的环境变量列表:

*   名称:`AWS_ECR_ACCOUNT_URL`值:[您的 AWS ECR 注册表 URL](https://docs.aws.amazon.com/AmazonECR/latest/userguide/Registries.html)
*   名称:`AWS_ACCESS_KEY_ID`值:`Your AWS IAM Account's Access Key ID`
*   名称:`AWS_SECRET_ACCESS_KEY`值:`Your AWS IAM Account's Secret Access Key`
*   名称:`DOCKER_LOGIN`值:`Your Docker Hub User Name`
*   名称:`Docker_PWD`值:`Your Docker Hub Password`

正确设置这些环境变量对于构建成功完成至关重要。在继续下一节之前，请确保正确设置了这些变量及其值。

## 使用 CircleCI AWS ECR Orb

下面是定义这个项目管道的`config.yml`文件。在接下来的部分中，我将解释这个`config.yml`文件的各种元素。

```
version: 2.1

orbs:
  aws-ecr: circleci/aws-ecr@0.0.4

workflows:
  build_test_deploy:
    jobs:
      - build_test
      - docker_hub_build_push_image:
          requires:
            - build_test
      - aws-ecr/build_and_push_image:
          region: us-east-1
          account-url: ${AWS_ECR_ACCOUNT_URL}
          repo: ${CIRCLE_PROJECT_REPONAME}
          tag: ${CIRCLE_BUILD_NUM}
          requires:
            - build_test
jobs:
  build_test:
    docker:
      - image: circleci/python:2.7.14
    steps:
      - checkout
      - run:
          name: Setup VirtualEnv
          command: |
            virtualenv helloworld
            . helloworld/bin/activate
            pip install --no-cache-dir -r requirements.txt
      - run:
          name: Run Tests
          command: |
            . helloworld/bin/activate
            python test_hello_world.py
  docker_hub_build_push_image:
    docker:
      - image: circleci/python:2.7.14
    steps:
      - checkout
      - setup_remote_docker:
          docker_layer_caching: false
      - run:
          name: Build and push Docker image to Docker Hub
          command: |
            echo 'export TAG=0.1.${CIRCLE_BUILD_NUM}' >> ${BASH_ENV}
            echo 'export IMAGE_NAME=${CIRCLE_PROJECT_REPONAME}' >> ${BASH_ENV}
            source ${BASH_ENV}
            docker build -t ${DOCKER_LOGIN}/${IMAGE_NAME} -t ${DOCKER_LOGIN}/${IMAGE_NAME}:${TAG} .
            echo ${DOCKER_PWD} | docker login -u ${DOCKER_LOGIN} --password-stdin
            docker push ${DOCKER_LOGIN}/${IMAGE_NAME} 
```

### 指定工作流和 orb

```
orbs:
  aws-ecr: circleci/aws-ecr@0.0.4 
```

`orbs:`键指定一个 orb 将在这个管道中使用。`aws-ecr:`键定义了配置中使用的内部名称。`circleci/aws-ecr@0.0.4`值指定并关联由`aws-ecr:`键使用和引用的实际 orb。这些 orb 语句可以被认为是其他语言和框架中的 import 语句。

```
workflows:
  build_test_deploy:
    jobs:
      - build_test
      - docker_hub_build_push_image:
          requires:
            - build_test
      - aws-ecr/build_and_push_image:
          region: us-east-1
          account-url: ${AWS_ECR_ACCOUNT_URL}
          repo: ${CIRCLE_PROJECT_REPONAME}
          tag: ${CIRCLE_BUILD_NUM}
          requires:
            - build_test 
```

### 工作流定义

`workflows:`键指定了由构建作业和 orb 组成的工作流列表。该段指定了一个称为`build_test_deploy:`的工作流

```
jobs:
  - build_test
  - docker_hub_build_push_image:
      requires:
        - build_test 
```

在这个片段中，指定了一个`jobs:`键，列出了在这个管道中执行的所有作业和 orb。该列表中的第一个作业是`build_test`，它没有作业依赖性。

### Orb 定义

工作流列表中的下一个任务`docker_hub_build_push_image:`指的是稍后将讨论的任务，但是请注意`requires:`键，它指定`docker_hub_build_push_image:`任务依赖于`build_test`任务的通过。否则，整个构建将会失败，并且工作流中的剩余作业将不会执行。

```
- aws-ecr/build_and_push_image:
    region: us-east-1
    account-url: ${AWS_ECR_ACCOUNT_URL}
    repo: ${CIRCLE_PROJECT_REPONAME}
    tag: ${CIRCLE_BUILD_NUM}
    requires:
      - build_test 
```

上面的部分显示了指定 AWS ECR Orb 执行的`aws-ecr/build_and_push_image:`键。在本例中，AWS ECR Orb 的参数需要分配给[内置环境变量](https://circleci.com/docs/env-vars/#built-in-environment-variables)的值。有关此 Orb 的更多详细信息，请参见 [AWS ECR Orb 文档](https://circleci.com/developer/orbs/orb/circleci/aws-ecr)。这个特定的 AWS ERC Orb 依赖于在执行这个 Orb 的`build_and_push_image`方法之前成功完成的`build_test:`作业。

### 作业定义

我已经讨论了配置中的`workflows:`和`orbs:`元素，它们是对该配置的主`jobs:`元素中定义的作业的引用。以下代码段显示了在此配置语法中定义的所有作业。

```
jobs:
  build_test:
    docker:
      - image: circleci/python:2.7.14
    steps:
      - checkout
      - run:
          name: Setup VirtualEnv
          command: |
            virtualenv helloworld
            . helloworld/bin/activate
            pip install --no-cache-dir -r requirements.txt
      - run:
          name: Run Tests
          command: |
            . helloworld/bin/activate
            python test_hello_world.py
  docker_hub_build_push_image:
    docker:
      - image: circleci/python:2.7.14
    steps:
      - checkout
      - setup_remote_docker:
          docker_layer_caching: false
      - run:
          name: Build and push Docker image to Docker Hub
          command: |
            echo 'export TAG=0.1.${CIRCLE_BUILD_NUM}' >> ${BASH_ENV}
            echo 'export IMAGE_NAME=${CIRCLE_PROJECT_REPONAME}' >> ${BASH_ENV}
            source ${BASH_ENV}
            docker build -t ${DOCKER_LOGIN}/${IMAGE_NAME} -t ${DOCKER_LOGIN}/${IMAGE_NAME}:${TAG} .
            echo ${DOCKER_PWD} | docker login -u ${DOCKER_LOGIN} --password-stdin
            docker push ${DOCKER_LOGIN}/${IMAGE_NAME} 
```

在上面的片段中，定义了两个作业`build_test:`和`docker_hub_build_push-image:`，演示了原始配置语法。`build_test:`作业实例化一个 Python 容器，安装应用程序依赖项，并运行应用程序的单元测试。

`docker_hub_build_push_image:`作业设置用于命名和标记图像的环境变量，基于 Docker 文件构建 Docker 图像，并将构建的图像推送到 Docker Hub。

这整个配置演示了如何定义和实现工作流、orb 和作业，它们在配置语法上提供了健壮和强大的功能。成功执行这个管道后，您应该在 Docker Hub 和 AWS ECR 存储库中得到一个经过测试的 Docker 映像。

## 摘要

如您所见，与 AWS ECR Orb 定义相比,`jobs:`段更加冗长。orb 旨在封装功能，提供一致性并减少管道配置中的重复代码量，从而减少冗长的配置语法。

使用 orbs，您可以在团队和项目之间共享您首选的 CI/CD 设置，并且只需几行代码就可以轻松集成工具和第三方解决方案。开发社区的成员可以编写 orb 来解决常见问题并帮助管理配置。在常见用例中共享和重用 orb 使团队能够解决有趣的、独特的问题，使他们的业务与众不同，同时为社区中的其他人提供加速 CI/CD 工作流的解决方案。

我期待 orb 的未来发展，以及它们给 CI/CD 管道带来的令人敬畏的流线型功能。如果您浏览 orb 注册表，没有找到符合您需要的功能的 orb，那么我鼓励您[创作您自己的定制 orb](https://circleci.com/docs/orb-intro/#authoring-your-own-orb) 。请注意，目前 CircleCI Orb 注册表上托管的所有 Orb 都是开放的，可供公众使用。注册中心目前还不支持私有 orb，但是这个特性已经在 orb 的路线图上了，将来会提供。

## 参考

要了解有关 orb、工作流和 CircleCI 的更多信息，请使用下列参考资料: