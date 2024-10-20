# AWS EC2 - AWS ECR | CircleCI

> 原文：<https://circleci.com/blog/aws-ecr-auth-support/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

*TL:DR；* **CircleCI 2.0 现在支持直接从 Docker executor 向 AWS EC2 容器注册中心(ECR)认证。这意味着您可以使用 ECR 中的私有 Docker 映像作为您的构建映像。[查看文件](https://circleci.com/docs/configuration-reference/#docker--machine-executor)。**

CircleCI 2.0 带来了原生 Docker 支持。一个项目可以构建在 2.0 之上，使用一个公共 Docker 映像作为执行环境。精心制作一个轻量级的 CI 环境，根据您的项目的确切需求进行定制，并且可以对其进行快照以便反复使用，这已经变得很平常了。用户喜欢拥有这种能力，但表示他们也希望支持私有图像。不久之后，`auth`键被引入来支持登录到 Docker 注册表，以获取私有映像作为您的执行环境:

```
jobs:
  build:
    docker:
      - image: acme-private/private-image:321
        auth:
          username: mydockerhub-user  # can specify string literal values
          password: $DOCKERHUB_PASSWORD  # or project UI environment variable reference 
```

这对于支持标准`docker login`凭证的注册管理机构(即 Docker Hub)来说非常有效。如果您有 Docker 注册中心的用户名和密码，并且这是您所需要的，那么您就很好。输入 AWS 的 ECR。

[AWS ECR](https://circleci.com/docs/1.0/continuous-deployment-with-aws-ec2-container-service/) 提供 Docker 注册服务，但不提供适当的`docker login`凭证。相反，根据 [AWS CLI 文档](http://docs.aws.amazon.com/AmazonECR/latest/userguide/ECR_AWSCLI.html)，您需要运行`aws ecr get-login`，这将生成一个带有临时登录凭证的`docker login` shell 命令。这些凭据仅持续 12 小时，因此不适合在 CI 环境中使用。

我们现在增加了专门针对 AWS ECR 的内置支持。您可以通过以下三种方式之一从 ECR 开始使用私有映像:

1.  使用 [CircleCI AWS 集成](https://circleci.com/docs/deployment-integrations/#aws-deployment)设置您的 AWS 凭证。
2.  使用标准的 [CircleCI 私有环境变量](https://circleci.com/docs/env-vars/#adding-environment-variables-in-the-app)设置您的 AWS 凭证。
3.  使用`aws_auth`在`.circleci/config.yml`中指定您的 AWS 凭证:

```
version: 2
jobs:
  build:
    docker:
      - image: account-id.dkr.ecr.us-east-1.amazonaws.com/org/repo:0.1
        aws_auth:
          aws_access_key_id: AKIAQWERVA  # can specify string literal values
          aws_secret_access_key: $ECR_AWS_SECRET_ACCESS_KEY  # or project UI envar reference 
```

选项 2 和 3 实际上是相同的，只是选项 3 允许您为凭证指定任何想要的变量名。当您对不同的基础设施有不同的 AWS 凭证时，这就很方便了。例如，假设您的 SaaS 应用运行速度更快的测试，并在每次提交时部署到暂存基础架构，而对于 Git 标签推送，我们在部署到生产之前运行全面的测试套件:

```
version: 2
jobs:
  build:
    docker:
      - image: account-id.dkr.ecr.us-east-1.amazonaws.com/org/repo:0.1
        aws_auth:
          aws_access_key_id: $AWS_ACCESS_KEY_ID_STAGING
          aws_secret_access_key: $AWS_SECRET_ACCESS_KEY_STAGING
    steps:
      - run:
          name: "Every Day Tests"
          command: "testing...."
      - run:
          name: "Deploy to Staging Infrastructure"
          command: "something something darkside.... cli"
  deploy:
    docker:
      - image: account-id.dkr.ecr.us-east-1.amazonaws.com/org/repo:0.1
        aws_auth:
          aws_access_key_id: $AWS_ACCESS_KEY_ID_STAGING
          aws_secret_access_key: $AWS_SECRET_ACCESS_KEY_STAGING
    steps:
      - run:
          name: "Full Test Suite"
          command: "testing...."
      - run:
          name: "Deploy to Production Infrastructure"
          command: "something something darkside.... cli"

workflows:
  version: 2
  main:
    jobs:
      - build:
          filters:
            tags:
              only: /^\d{4}\.\d+$/
      - deploy:
          requires:
            - build
          filters:
            branches:
              ignore: /.*/
            tags:
              only: /^\d{4}\.\d+$/ 
```

在 CircleCI 2.0 上享受使用来自 [AWS ECR](https://circleci.com/docs/1.0/continuous-deployment-with-aws-ec2-container-service/) 的私人图像。始终注意验证您没有从映像或私有 envars 的构建输出中泄漏任何敏感信息。