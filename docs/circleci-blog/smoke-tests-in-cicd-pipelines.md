# CI/CD 管道中的烟雾测试

> 原文：<https://circleci.com/blog/smoke-tests-in-cicd-pipelines/>

这里有一个困扰许多开发团队的常见情况。您通过 CI/CD 管道运行一个应用程序，所有测试都通过了，这很好。但是，当您将它部署到一个活动的目标环境中时，该应用程序并不像预期的那样运行。你不能总是预测当你的应用被实时推送时会发生什么。解决办法？冒烟测试旨在通过运行覆盖应用程序的关键组件和功能的测试用例来尽早揭示这些类型的故障。它们还可以确保应用程序在部署的场景中按预期运行。当实现时，冒烟测试通常在每个应用构建上执行，以在进入更广泛和耗时的测试之前验证基本但关键的功能通过。冒烟测试有助于创建对软件开发生命周期至关重要的快速反馈循环。

在这篇文章中，我将演示如何在 CI/CD 管道的部署阶段添加冒烟测试。冒烟测试将测试部署后应用程序的简单方面。

## 用于烟雾测试的技术

这篇文章将引用以下技术:

## 先决条件

这篇文章依赖于我上一篇文章[中的配置和代码，使用基础设施作为代码](https://circleci.com/blog/automate-releases-from-pipelines-using-infrastructure-as-code/)从您的管道中自动发布。完整的源代码可以在[这个回购](https://github.com/datapunkz/orb-pulumi-gcp)中找到。

## 从烟雾测试中获得最大收益

冒烟测试非常适合暴露意外的构建错误、连接错误，以及在新版本部署到目标环境后验证服务器的预期响应。例如，一个快速、简单的冒烟测试可以验证一个应用程序是可访问的，并且使用预期的响应代码(如`OK 200`、`300`、`301`、`404`等)进行响应。本文中的例子将测试部署的应用程序是否用一个`OK 200`服务器代码进行响应，还将验证默认页面内容是否呈现预期的文本。

## 运行 CI/CD 管道，不进行烟雾测试

让我们来看一个示例管道配置，它被设计来运行单元测试、构建和推送 Docker 映像到 Docker Hub。该管道还使用[基础设施作为代码](https://circleci.com/blog/an-intro-to-infrastructure-as-code/) ( [Pulumi](https://www.pulumi.com/) )来提供新的 Google Kubernetes 引擎(GKE)集群，并将该版本部署到该集群。这个管道配置示例没有实现冒烟测试。请注意，如果您运行这个特定的管道示例，将会创建一个新的 GKE 集群，它将一直存在，直到您手动运行`pulumi destroy`命令来终止它创建的所有基础设施。

**注意:** *不终止基础设施将导致意外成本。*

```
version: 2.1
orbs:
  pulumi: pulumi/pulumi@2.0.0
jobs:
  build_test:
    docker:
      - image: cimg/python:3.8.1
        environment:
          PIPENV_VENV_IN_PROJECT: 'true'
    steps:
      - checkout
      - run:
          name: Install Python Dependencies
          command: |
            pipenv install --skip-lock
      - run:
          name: Run Tests
          command: |
            pipenv run pytest
  build_push_image:
    docker:
      - image: cimg/python:3.8.1
    steps:
      - checkout
      - setup_remote_docker:
          docker_layer_caching: false
      - run:
          name: Build and push Docker image
          command: |       
            pipenv install --skip-lock
            pipenv run pip install --upgrade 'setuptools<45.0.0'
            pipenv run pyinstaller -F hello_world.py
            echo 'export TAG=${CIRCLE_SHA1}' >> $BASH_ENV
            echo 'export IMAGE_NAME=orb-pulumi-gcp' >> $BASH_ENV
            source $BASH_ENV
            docker build -t $DOCKER_LOGIN/$IMAGE_NAME -t $DOCKER_LOGIN/$IMAGE_NAME:$TAG .
            echo $DOCKER_PWD | docker login -u $DOCKER_LOGIN --password-stdin
            docker push $DOCKER_LOGIN/$IMAGE_NAME
  deploy_to_gcp:
    docker:
      - image: cimg/python:3.8.1
        environment:
          CLOUDSDK_PYTHON: '/usr/bin/python2.7'
          GOOGLE_SDK_PATH: '~/google-cloud-sdk/'
    steps:
      - checkout
      - pulumi/login:
          version: "2.0.0"
          access-token: ${PULUMI_ACCESS_TOKEN}
      - run:
          name: Install dependencies
          command: |
            cd ~/
            pip install --user -r project/requirements.txt
            curl -o gcp-cli.tar.gz https://dl.google.com/dl/cloudsdk/channels/rapid/google-cloud-sdk.tar.gz
            tar -xzvf gcp-cli.tar.gz
            echo ${GOOGLE_CLOUD_KEYS} | base64 --decode --ignore-garbage > ${HOME}/project/pulumi/gcp/gke/cicd_demo_gcp_creds.json
            ./google-cloud-sdk/install.sh  --quiet
            echo 'export PATH=$PATH:~/google-cloud-sdk/bin' >> $BASH_ENV
            source $BASH_ENV
            gcloud auth activate-service-account --key-file ${HOME}/project/pulumi/gcp/gke/cicd_demo_gcp_creds.json
      - pulumi/update:
          stack: k8s
          working_directory: ${HOME}/project/pulumi/gcp/gke/
workflows:
  build_test_deploy:
    jobs:
      - build_test
      - build_push_image
      - deploy_to_gcp:
          requires:
          - build_test
          - build_push_image 
```

这个管道将新的应用程序版本部署到一个新的 GKE 集群，但是我们不知道在所有这些自动化完成之后，应用程序是否真正启动并运行。我们如何发现应用程序是否已经在这个新的 GKE 集群中部署并正常运行？冒烟测试是在部署后快速、轻松地验证应用程序状态的好方法。

## 如何编写冒烟测试？

第一步是开发测试用例，定义验证应用程序功能所需的步骤。确定您想要验证的功能，然后创建场景来测试它。在本教程中，我有意描述一个非常小的测试范围。对于我们的示例项目，我最关心的是验证应用程序在部署后是否可访问，以及提供的默认页面是否呈现预期的静态文本。

我更喜欢列出我想要测试的项目，因为这符合我的开发风格。大纲显示了我在开发此应用程序的冒烟测试时考虑的因素。下面是我如何为这个冒烟测试开发测试用例的一个例子:

*   什么语言/测试框架？
*   该测试应在何时执行？
    *   创建 GKE 集群后
*   会考什么？
    *   测试:应用程序部署后是否可访问？
        *   预期结果:服务器以代码`200`响应
    *   测试:默认页面是否呈现文本“欢迎使用 CI/CD”
    *   测试:默认页面是否呈现文本“版本号:”
*   测试后操作(无论通过还是失败都必须发生)
    *   将测试结果写入标准输出
    *   摧毁 GKE 集群和相关基础设施

我的测试用例大纲(也称为测试脚本)对于本教程来说是完整的，并且清楚地显示了我对测试感兴趣的内容。在这篇文章中，我将使用一个基于 bash 的开源冒烟测试框架来编写冒烟测试，这个框架由 [asm89](https://github.com/asm89) 开发，名为 [`smoke.sh`](https://github.com/asm89/smoke.sh) 。对于您自己的项目，您可以用您喜欢的任何语言或框架编写冒烟测试。我选择了`smoke.sh`,因为它是一个易于实现的框架，并且是开源的。现在让我们探索如何使用`smoke.sh`框架来表达这个测试脚本。

## 使用 smoke.sh 创建冒烟测试

框架的文档描述了如何使用它。下一个示例代码块展示了我如何使用在[示例代码的 repo](https://github.com/datapunkz/orb-pulumi-gcp) 的`test/`目录中找到的`smoke_test`文件。

```
#!/bin/bash

. tests/smoke.sh

TIME_OUT=300
TIME_OUT_COUNT=0
PULUMI_STACK="k8s"
PULUMI_CWD="pulumi/gcp/gke/"
SMOKE_IP=$(pulumi stack --stack $PULUMI_STACK --cwd $PULUMI_CWD output app_endpoint_ip)
SMOKE_URL="http://$SMOKE_IP"

while true
do
  STATUS=$(curl -s -o /dev/null -w '%{http_code}' $SMOKE_URL)
  if [ $STATUS -eq 200 ]; then
    smoke_url_ok $SMOKE_URL
    smoke_assert_body "Welcome to CI/CD"
    smoke_assert_body "Version Number:"
    smoke_report
    echo "\n\n"
    echo 'Smoke Tests Successfully Completed.'
    echo 'Terminating the Kubernetes Cluster in 300 second...'
    sleep 300
    pulumi destroy --stack $PULUMI_STACK --cwd $PULUMI_CWD --yes
    break
  elif [[ $TIME_OUT_COUNT -gt $TIME_OUT ]]; then
    echo "Process has Timed out! Elapsed Timeout Count.. $TIME_OUT_COUNT"
    pulumi destroy --stack $PULUMI_STACK --cwd $PULUMI_CWD --yes
    exit 1
  else
    echo "Checking Status on host $SMOKE... $TIME_OUT_COUNT seconds elapsed"
    TIME_OUT_COUNT=$((TIME_OUT_COUNT+10))
  fi
  sleep 10
done 
```

接下来，我将解释这个 smoke_test 文件中发生了什么。

### smoke_test 文件的逐行描述

让我们从文件的顶部开始。

```
#!/bin/bash

. tests/smoke.sh 
```

这个代码片段指定了要使用的 Bash 二进制文件，还指定了要导入/包含在`smoke_test`脚本中的核心`smoke.sh`框架的文件路径。

```
TIME_OUT=300
TIME_OUT_COUNT=0
PULUMI_STACK="k8s"
PULUMI_CWD="pulumi/gcp/gke/"
SMOKE_IP=$(pulumi stack --stack $PULUMI_STACK --cwd $PULUMI_CWD output app_endpoint_ip)
SMOKE_URL="http://$SMOKE_IP" 
```

这个片段定义了将在整个`smoke_test`脚本中使用的环境变量。以下是每个环境变量及其用途的列表:

*   `PULUMI_STACK="k8s"`由 Pulumi 用来指定 Pulumi 应用程序堆栈。
*   `PULUMI_CWD="pulumi/gcp/gke/"`是 Pulumi 基础设施代码的路径。
*   `SMOKE_IP=$(pulumi stack --stack $PULUMI_STACK --cwd $PULUMI_CWD output app_endpoint_ip)`是 Pulumi 命令，用于检索 GKE 集群上应用程序的公共 IP 地址。该变量在整个脚本中被引用。
*   `SMOKE_URL="http://$SMOKE_IP"`指定 GKE 集群上应用程序的 url 端点。

```
while true
do
  STATUS=$(curl -s -o /dev/null -w '%{http_code}' $SMOKE_URL)
  if [ $STATUS -eq 200 ]; then
    smoke_url_ok $SMOKE_URL
    smoke_assert_body "Welcome to CI/CD"
    smoke_assert_body "Version Number:"
    smoke_report
    echo "\n\n"
    echo 'Smoke Tests Successfully Completed.'
    echo 'Terminating the Kubernetes Cluster in 300 second...'
    sleep 300
    pulumi destroy --stack $PULUMI_STACK --cwd $PULUMI_CWD --yes
    break
  elif [[ $TIME_OUT_COUNT -gt $TIME_OUT ]]; then
    echo "Process has Timed out! Elapsed Timeout Count.. $TIME_OUT_COUNT"
    pulumi destroy --stack $PULUMI_STACK --cwd $PULUMI_CWD --yes
    exit 1
  else
    echo "Checking Status on host $SMOKE... $TIME_OUT_COUNT seconds elapsed"
    TIME_OUT_COUNT=$((TIME_OUT_COUNT+10))
  fi
  sleep 10
done 
```

这个片段是所有奇迹发生的地方。这是一个`while`循环，一直执行到条件为真或者脚本退出。在这种情况下，循环使用一个`curl`命令来测试应用程序是否返回一个`OK 200`响应代码。因为这条管道正在从头开始创建一个全新的 GKE 集群，所以在我们开始冒烟测试之前，需要完成 Google 云平台中的一些事务。

*   GKE 群集和应用程序服务必须启动并运行。
*   用 curl 请求的结果填充`$STATUS`变量，然后测试`200`的值。否则，循环会将`$TIME_OUT_COUNT`变量增加 10 秒，然后等待 10 秒来重复`curl`请求，直到应用程序做出响应。
*   一旦集群和应用启动、运行并响应，`STATUS`变量将产生一个`200`响应代码，其余的测试将继续进行。

在`smoke_assert_body "Welcome to CI/CD"`和`smoke_assert_body "Version Number: "`语句中，我测试了被调用的网页上的欢迎文本和版本号文本。如果结果为假，测试将失败，这将导致管道失败。如果结果为真，那么应用程序将返回一个`200`响应代码，我们的文本测试将产生`TRUE`。我们的冒烟测试将通过并执行`pulumi destroy`命令，该命令终止了为这个测试用例创建的所有基础设施。由于不再需要该集群，它将终止在该测试中创建的所有基础设施。

这个循环还有一个`elif` (else if)语句，用于检查应用程序是否超过了`$TIME_OUT`值。`elif`语句是[异常处理](https://en.wikipedia.org/wiki/Exception_handling)的一个例子，它使我们能够控制意外结果发生时会发生什么。如果`$TIME_OUT_COUNT`值超过`TIME_OUT`值，则`pulumi destroy`命令被执行并终止新创建的基础设施。然后,`exit 1`命令会使您的管道构建过程失败。不管测试结果如何，GKE 集群都将被终止，因为除了测试之外，这个基础设施真的没有存在的必要。

## 向管道添加烟雾测试

我已经解释了冒烟测试的例子和我开发测试用例的过程。现在是时候将它集成到上面的 CI/CD 管道配置中了。我们将在`deploy_to_gcp`作业的`pulumi/update`步骤下添加一个新的`run`步骤:

```
 ...
      - run:
          name: Run Smoke Test against GKE
          command: |
            echo 'Initializing Smoke Tests on the GKE Cluster'
            ./tests/smoke_test
            echo "GKE Cluster Tested & Destroyed"
      ... 
```

这个片段演示了如何将`smoke_test`脚本集成并执行到现有的 CI/CD 管道中。添加这个新的运行块可以确保每个管道构建都将在一个活动的 GKE 集群上测试应用程序，并提供应用程序通过所有测试用例的验证。您可以确信，当部署到测试的目标环境(在本例中是一个 Google Kubernetes 集群)时，特定版本将正常运行。

## 包扎

总之，我已经讨论并展示了在 CI/CD 管道中使用冒烟测试和基础设施作为代码来测试目标部署环境中的构建的优势。在应用程序的目标环境中测试应用程序，有助于深入了解应用程序在部署后的行为。将冒烟测试集成到 CI/CD 管道中增加了应用程序构建的另一层信心。

如果您有任何问题、评论或反馈，请随时在 Twitter 上 ping 我 [@punkdata](https://twitter.com/punkdata) 。

感谢阅读！