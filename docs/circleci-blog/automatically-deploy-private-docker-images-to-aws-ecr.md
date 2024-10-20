# 自动将私有 Docker 映像部署到 Amazon ECR - CircleCI

> 原文：<https://circleci.com/blog/automatically-deploy-private-docker-images-to-aws-ecr/>

## 使用 CircleCI orbs 构建 Docker 映像并将其推送到 Amazon ECR

本文介绍了如何使用 CircleCI orbs 构建生产级 Docker 映像并将其推送到 Amazon 弹性容器注册中心(ECR)进行存储。熟悉本文中使用的工具和技术是首选，但不是必需的。以下是常用术语和技术的简要总结:

*   **Docker:** 基于容器构建应用的软件平台。
*   **容器:**一种虚拟化方法，将应用程序的代码、配置和依赖项打包成构建块，以实现一致性、效率、生产力和版本控制。
*   容器映像:一个自包含的软件，其中包含运行应用程序所需的一切:代码、工具和资源。
*   **容器映像库:**命名和相关容器映像的集合，通常提供相同应用程序或服务的不同版本，由它们的标签标识。
*   **Docker image registry:** 是一种存储集装箱图像的服务，由第三方托管，或者作为公共/私人注册中心，如 Docker Hub、AWS (ECR)、GCP (GCR)、Quay 等。它们简化了从开发到生产的工作流程。
*   **容器编排:**容器编排就是管理容器的生命周期，尤其是在大型的动态环境中。

让我们深入了解一下 Amazon ECR，它是什么，以及 CircleCI 如何帮助您充分利用这项服务。

## 什么是亚马逊 ECR？

亚马逊 ECR 是一个完全托管的私有 Docker 容器注册中心，开发者可以很容易地存储、管理和部署 Docker 容器映像。亚马逊 ECR 与亚马逊弹性容器服务([亚马逊 ECSe](https://aws.amazon.com/ecs/) )和亚马逊弹性 Kubernetes 服务([亚马逊 EKS](https://aws.amazon.com/eks/) )无缝集成。亚马逊 ECR 也可以和其他云厂商一起使用。

**你为什么会想使用像亚马逊 ECR 这样的私有 Docker 注册中心？**

*   在许多情况下，分离开发、测试和生产注册是可取的。
*   它是完全托管的，因此无需运营自己的容器存储库或担心底层基础架构的扩展。
*   很安全。具有扫描功能和基于角色的访问控制的私有容器注册表提供了更多的安全性、治理(IAM)和高效管理。它通过 HTTPS 传输您的容器图像，并自动加密您的静态图像。
*   在运行容器的系统附近运行注册中心可以减少部署延迟并减少网络中断的风险。

开发人员需要使用注册表来存储应用程序开发过程中创建的图像。[持续集成](https://circleci.com/continuous-integration/)管道连接构建容器图像，并将这些工件推入亚马逊 ECR。想象一个管道，其中您推送一个 commit，该 commit 触发 CircleCI 上的一个构建，该构建将一个新图像推送到 Amazon ECR 中。然后，您的注册中心可以启动 webhook 并触发部署。所有这些都不需要人工做任何事情。注册中心使得这种完全自动化的管道更加容易。放在注册表中的容器映像可以在开发的不同阶段使用。在几个步骤中，使用 [CircleCI orbs](https://circleci.com/orbs/) ，我们将封装一个简单的 Node.js 应用程序，构建映像，并将其推送到 Amazon ECR。

## 设置 Amazon ECR

从 AWS 管理控制台中，选择 IAM。此服务允许您管理对 AWS 资源的访问。

![](img/7af1d17a249218fc100d3509557e52d3.png)

创建角色/用户。在这个例子中，我把我的名字叫做`ci-cd-ecr`，但是任何任意的名字都可以。

![](img/3b04aad7715f19b284c6d18b92e8730e.png)

接下来，我们需要使用一个`AWS_ACCESS_KEY`和一个`AWS_SECRET_KEY`以编程方式访问我们的 Amazon ECR 服务。点击新创建的用户，进入**你的安全凭证**页面。在这里，您将能够创建新的访问密钥。请妥善保存密钥，因为我们将使用它从 CircleCI 访问 Amazon ECR 服务。密钥的格式如下:

```
AWS_ACCESS_KEY_ID=AKIAIOSFODNN7EXAMPLE
AWS_SECRET_ACCESS_KEY=wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY 
```

![](img/ae96f51abe4d3ca6fa7666e101ab74c2.png)

接下来，从**策略**部分创建一个策略，并附加前面创建的`ci-cd-ecr`角色。该政策赋予了亚马逊 ECR 的完全访问权限。CircleCI orb 使用我们新创建的`ci-cd-ecr`角色，将完全访问我们的 Amazon ECR 服务，包括创建图像存储库(如果它们不存在的话)。

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "ecr:*",
                "cloudtrail:LookupEvents"
            ],
            "Resource": "*"
        }
    ]
} 
```

为用户生成访问密钥和秘密密钥，并将它们存储在安全的地方。在设置 CircleCI 管道示例时，我们将需要它们。

## 使用 aws-ecr orb 设置 CircleCI 管道

这个演示的代码可以在 GitHub [这里](https://github.com/daumie/circleci-ecr-orb-demo)找到。CircleCI 配置文件可以在`.circleci/`文件夹中找到。正如您将看到的，使用 orbs，我们仅使用 18 行配置就完成了构建一个映像并将其推送到 Amazon ECR！要使用 orbs，我们需要使用 CircleCI 版。它是支持球体的版本。aws-ecr orb 预装了以下命令:

*   建立形象
*   标记图像(使用`HEAD == CIRCLE_SHA1`的 Git 提交散列)
*   登录亚马逊 ECR
*   创建一个 Amazon ECR repo(如果不存在)
*   将图片推送到亚马逊 ECR

以下是我们管道的完整配置:

```
version: 2.1

orbs:
  aws-ecr: circleci/aws-ecr@6.7.0

workflows:
  build_and_push_image:
    jobs:
      - aws-ecr/build-and-push-image:
          account-url: AWS_ECR_ACCOUNT_URL
          aws-access-key-id: AWS_ACCESS_KEY_ID
          aws-secret-access-key: AWS_SECRET_ACCESS_KEY
          create-repo: true
          dockerfile: Dockerfile
          path: .
          region: AWS_REGION
          repo: circleci-ecr-orb-demo
          tag: "$CIRCLE_SHA1" 
```

## 为 Amazon ECR 设置环境变量

您可以在配置中看到几个需要设置的环境变量:

*   `AWS_ECR_ACCOUNT_URL` -存储映射到 AWS 帐户的 Amazon ECR 帐户 URL 的环境变量，例如{ awsAccountNum } . dkr . ECR . us-west-2 . Amazon AWS . com
*   `AWS_ACCESS_KEY_ID` -我们之前创建的`ci-cd-ecr` IAM 角色的 AWS 访问键 id。
*   `AWS_SECRET_ACCESS_KEY` -我们之前创建的 ci-cd-ecr IAM 角色的 AWS 密钥。将此设置为您将设置来保存此值的环境变量的名称，即 AWS_SECRET_ACCESS_KEY
*   `AWS_REGION` -您的 ECR 资源将位于的 AWS 区域。

**注意:** *你不必设置`$CIRCLE_SHA1`，因为它是所有 CircleCI 项目中可用的默认变量。这是当前构建的最后一次提交的`SHA1`散列。使用 Git 提交散列让您能够跟踪容器中的内容。理想情况下，它允许我们将容器追溯到其 Docker 映像，然后追溯到映像中包含的 Docker 文件和代码。在自动化执行环境中，这将带我们回到导致 Docker 映像构建的提交。*

上面的配置示例使用了`$CIRCLE_SHA1`来设置`tag:`。或者，您可以在您的构建中使用 CircleCI 构建号(`$CIRCLE_BUILD_NUM`)作为标签。

![](img/ca00bb075716ce85334c145af14deb31.png)

上面的`dockerfile:`命令指定了 docker 文件的路径。我们的演示报告使用以下 docker 文件:

```
# Set the base image to use for subsequent instructions
FROM node:alpine

# Add metadata to an image 
LABEL app="simple-node-application"
# Directive to set environmental variables key to value pair
ENV NPM_CONFIG_LOGLEVEL warn

# Set the working directory for any subsequent ADD, COPY, CMD, ENTRYPOINT, 
# or RUN instructions that follow it in the Dockerfile
WORKDIR /usr/src/app

# Copy files or folders from source to the dest path in the image's filesystem.
COPY package.json /usr/src/app/
COPY . /usr/src/app/

# Execute any commands on top of the current image as a new layer and commit the results.
RUN npm install --production

# Define the network ports that this container will listen on at runtime.
EXPOSE 3000

# Configure the container to be run as an executable.
ENTRYPOINT ["npm", "start"] 
```

CircleCI 和 Amazon ECR 都配置好之后，您就可以开始构建映像并将它们推送到存储库了。

## 仅用 18 行配置就将 Docker 图像推送到 Amazon ECR

仅用 18 行配置，我们就能够构建 Docker 映像并将其推送到 Amazon ECR。CircleCI orbs 通过将预构建的命令、作业和执行器导入 CircleCI 配置文件来节省时间，并提供与云服务和其他工具的轻松集成。

* * *

Dominic Motuka 是 Andela 的 DevOps 工程师，在 AWS 和 GCP 支持、自动化和优化生产就绪部署方面拥有 4 年多的实践经验，利用配置管理、CI/CD 和 DevOps 流程。

[阅读多米尼克·莫图卡的更多帖子](/blog/author/dominic-motuka/)