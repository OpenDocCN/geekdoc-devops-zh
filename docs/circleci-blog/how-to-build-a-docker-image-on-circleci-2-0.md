# 在 CircleCI 上建立 Docker 形象

> 原文：<https://circleci.com/blog/how-to-build-a-docker-image-on-circleci-2-0/>

在 CircleCI 中，开发人员可以自由组合任意图像，如乐高积木，以创建他们首选的 [CI](https://circleci.com/continuous-integration/) 容器环境。比如 CircleCI 原生支持 [Docker](https://www.docker.com/) 。您可以将应用程序作为 [Docker 映像](https://circleci.com/docs/circleci-images/)进行构建、推送和部署。CircleCI 使用 Docker 容器中的 Docker 构建 Docker 映像，

在本帖中，我将简要描述[如何在 CircleCI 中构建 Docker 图像](https://circleci.com/docs/building-docker-images/)，包括[图像层](https://circleci.com/docs/docker-layer-caching/)缓存。

## TL；速度三角形定位法(dead reckoning)

在项目的根目录中，在名为`.circleci`的目录中创建一个`config.yml`:

```
version: 2
jobs:
  build:
    working_directory: /app
    docker:
      - image: docker:17.05.0-ce-git
    steps:
      - checkout
      - setup_remote_docker
      - run:
          name: Install dependencies
          command: |
            apk add --no-cache \
              py-pip=9.0.0-r1
            pip install \
              docker-compose==1.12.0 \
              awscli==1.11.76
      - restore_cache:
          keys:
            - v1-{{ .Branch }}
          paths:
            - /caches/app.tar
      - run:
          name: Load Docker image layer cache
          command: |
            set +o pipefail
            docker load -i /caches/app.tar | true
      - run:
          name: Build application Docker image
          command: |
            docker build --cache-from=app -t app .
      - run:
          name: Save Docker image layer cache
          command: |
            mkdir -p /caches
            docker save -o /caches/app.tar app
      - save_cache:
          key: v1-{{ .Branch }}-{{ epoch }}
          paths:
            - /caches/app.tar
      - run:
          name: Run tests
          command: |
            docker-compose -f ./docker-compose.test.yml up
      - deploy:
          name: Push application Docker image
          command: |
            if [ "${CIRCLE_BRANCH}" == "master" ]; then
              login="$(aws ecr get-login)"
              ${login}
              docker tag app "${ECR_ENDPOINT}/app:${CIRCLE_SHA1}"
              docker push "${ECR_ENDPOINT}/app:${CIRCLE_SHA1}"
            fi 
```

根据项目的不同，一些细节可能会有所变化。如果您想亲自尝试这些步骤，我准备了一个[示例项目](https://github.com/builtinnya/circleci-2.0-beta-docker-example)，您可以使用这个配置文件。

该项目使用 [Express](https://expressjs.com/) 在 node.js 中编写，并简单地返回“Hello World”，用 [Jest](https://facebook.github.io/jest/) 和 [supertest](https://github.com/visionmedia/supertest) 进行测试。为了将 Docker 图像(ECR)推送到 [Amazon EC2 容器注册表](https://aws.amazon.com/jp/ecr/)，我使用了一个 [Terraform](https://www.terraform.io/) 脚本。

在每一节之后，我将提供它是如何工作的一个细目分类。

## 顶层结构

```
version: 2
jobs:
  build:
    working_directory: /app
    docker:
      ...
    steps:
      ... 
```

在 CircleCI 中，我们可以自由定义每个作业的*步骤*，包括存储库签出和缓存相关的处理。这给了我们很大的自由，不仅对于 CI 容器环境，而且对于步骤。所有细节都可以在[官方文件](https://circleci.com/docs/)中找到。

## 执行者坞站

```
docker:
  - image: docker:17.05.0-ce-git 
```

在本节中，我们定义前面提到的 CI 环境。这个环境是我们的步骤将被执行的地方。

我们想要的是一个安装 Docker 并有 Git 的 Docker 映像。这些要求通过使用`docker:17.05.0-ce-git`来满足，它是一个[官方 Docker 图像](https://hub.docker.com/_/docker/)。

当一个图像有后缀“-git”时，意味着 git 是预装的。通过使用这个映像，您可以确保您始终使用最新的 [Docker 客户端](https://circleci.com/docs/building-docker-images/)。

## 检验

```
steps:
  - checkout 
```

第一步，`checkout`，是检查源代码的特殊步骤；这将被下载到由`working_directory`指定的目录。

## 设置远程停靠站

```
- setup_remote_docker 
```

这一步可以帮助你避免 Docker-in-Docker 问题。事实上，我们正在设置一个与 CI(或*主*)容器隔离的[环境，然后使用远程主机的 Docker 引擎。](https://circleci.com/docs/building-docker-images/)

## 安装所需的库

```
- run:
    name: Install dependencies
    command: |
      apk add --no-cache \
        py-pip=9.0.0-r1
      pip install \
        docker-compose==1.12.0 \
        awscli==1.11.76 
```

在这里，我们安装了 [Python](https://www.python.org/) 、 [pip](https://pip.pypa.io/en/stable/) 、 [Docker Compose](https://docs.docker.com/compose/) ，以及 [AWS CLI](https://aws.amazon.com/cli/) 。

在实际项目中，我建议提前在映像中安装这些依赖项。

## 构建和缓存 Docker 图像

```
- restore_cache:
    keys:
      - v1-{{ .Branch }}
    paths:
      - /caches/app.tar
  - run:
    name: Load Docker image layer cache
    command: |
      set +o pipefail
      docker load -i /caches/app.tar | true
  - run:
    name: Build application Docker image
    command: |
      docker build --cache-from=app -t app .
  - run:
    name: Save Docker image layer cache
    command: |
      mkdir -p /caches
      docker save -o /caches/app.tar app
  - save_cache:
    key: v1-{{ .Branch }}-{{ epoch }}
    paths:
      - /caches/app.tar 
```

这是这篇文章的核心。基本上，我们正在做以下工作:

1.  当有以`v1-{{ <branch name> }}`为后缀的缓存时，CircleCI 会将你的目录恢复到`/caches/app.tar`。`app.tar`是以前构建的 Docker 映像文件。

2.  当`/caches/app.tar`存在时，Docker 将加载它，允许我们重用以前构建的图像。

3.  当你[构建一个 Docker 映像](https://circleci.com/docs/building-docker-images/)时，你需要指定`--cache-from=<image name>`。

4.  我们将保存我们在`/caches/app.tar`中构建的 Docker 映像。

5.  最后，我们缓存`/caches/app.tar`以便在下一次构建中重用它。我们使用`v1-{{ <branch name> }}-{{ <Unix epoch time> }}`作为[缓存键](https://circleci.com/docs/caching/)。

我们必须这样做的原因是因为远程 Docker 引擎默认情况下不做层缓存。虽然有一个[函数来执行这个层缓存](https://circleci.com/docs/docker-layer-caching/)，但是我们必须请求 CircleCI 支持来启用 2.0 公测版中的缓存特性。这也有可能成为正式版本中的付费功能。

您可以参考示例项目的[构建历史](https://circleci.com/gh/builtinnya/circleci-2.0-beta-docker-example/tree/master)来查看通过缓存图像层实际可以获得多少速度提升。例如，[版本#12](https://circleci.com/gh/builtinnya/circleci-2.0-beta-docker-example/12) 有缓存，而[版本#13](https://circleci.com/gh/builtinnya/circleci-2.0-beta-docker-example/13) 没有缓存。在这个例子中，我们看到速度增加了大约 22 秒。

## 运行测试

```
- run:
    name: Run tests
    command: |
      docker-compose -f ./docker-compose.test.yml up 
```

对于这个项目，我用 Docker Compose 运行测试，并且测试只在应用程序容器中运行。如果您需要一个数据库容器，使用 Docker Compose 很容易设置一个。

## 推送 Docker 图像

```
- deploy:
    name: Push application Docker image
    command: |
      if [ "$ {CIRCLE_BRANCH}" == "master" ]; then
        login="$(aws ecr get-login)"
        ${login}
        docker tag app "${ECR_ENDPOINT}/app:${CIRCLE_SHA1}"
        docker push "${ECR_ENDPOINT}/app:${CIRCLE_SHA1}"
      fi 
```

仅为主分支(在本例中称为“主”)构建 Docker 映像并将其推送到 ECR 存储库。我们从之前安装的 AWS CLI 获取登录信息。在一个真实的项目中，您通常会在推送映像后使用`ecs-deploy`进行部署。

## 结论

CircleCI 允许您使用远程 Docker 引擎来构建 Docker 映像。即使在我的例子中构建速度的提高很小，我仍然知道缓存 Docker 图像层是可以做到的。根据项目的不同，构建速度的提高可能会更大。

阅读更多信息:

Naoto Yokoyama 是一名自由职业的全栈工程师。你可以在 [Twitter](https://twitter.com/builtinnya) 、 [GitHub](https://github.com/builtinnya) 或者他们的[博客](http://lambdar.hatenablog.com/)上与他们取得联系。