# CircleCI orbs 提供了一种与 AWS 服务集成的简单方法

> 原文：<https://circleci.com/blog/circleci-for-aws-deep-integration-security-and-flexibility/>

希望将 AWS 服务与其 CI/CD 管道相集成的团队不应该为了建立一致的工作流而费尽周折。这正是我们创建各种 AWS orbs 的原因，这些 orb 为构建和测试代码、创建和推送工件以及将您的应用程序部署和更新到您的 AWS 帐户提供简单的开箱即用的解决方案。

为了向 AWS 用户提供一套全面的 orb，我们还专门为 AWS 的新无服务器应用程序模型(SAM)添加了一个新的 orb，这是 CI 管道和 AWS Lambda 之间的首次简化集成。现在，您可以使用我们的 orb 在 CI/CD 管道上自动测试更新的 Lambda 函数，然后再向用户发布，而不是手动操作您的系统来测试您的 Lambda 代码。

我们的 AWS orbs 套件提供了一个高效的解决方案，可以节省您自己设置集成的时间。例如，您需要编写大量的脚本来在大多数 CI 平台上构建和推送 Docker 映像。使用我们的 ECR orb(目前已有超过 1，100 家组织使用),您只需六行预打包的配置就可以实现同样的功能。看到如此多的团队从我们当前的 AWS 集成中受益，令人振奋。此外，我们还在不断扩展和增强我们的 AWS orbs，以支持最受欢迎的使用案例，并根据客户的要求集成其他 AWS 服务。

查看我们最新的 AWS orbs，帮助将 AWS 服务与您的 CI/CD 渠道集成:

### 使用这些 orb 部署到 AWS:

[AWS-SAM-无服务器](https://circleci.com/developer/orbs/orb/circleci/aws-serverless) -利用 AWS 无服务器应用程序模型，在 CircleCI 上构建、测试和部署您的 AWS SAM 无服务器应用程序。

[ECR](https://circleci.com/developer/orbs/orb/circleci/aws-ecr)——构建图像，并将它们推送到亚马逊弹性容器注册中心(Amazon ECR)。

[ECS](https://circleci.com/developer/orbs/orb/circleci/aws-ecs) -部署并更新亚马逊弹性容器服务(Amazon ECS)

[EKS](https://circleci.com/developer/orbs/orb/circleci/aws-eks) -为 Kubernetes(亚马逊 EKS)部署和更新亚马逊弹性容器服务。

[CodeDeploy](https://circleci.com/developer/orbs/orb/circleci/aws-code-deploy) -将应用程序部署到 AWS CodeDeploy。

### 通过这些 orb 与 AWS 服务集成:

CLI -安装并配置 AWS 命令行界面(awscli)。

[S3](https://circleci.com/developer/orbs/orb/circleci/aws-s3) -使用这套工具与亚马逊 S3 合作。要求:bash。

[AWS 系统管理器参数存储库](https://circleci.com/developer/orbs/orb/circleci/aws-parameter-store) -加载 AWS 系统管理器参数存储库密钥作为环境变量。

### 你能做什么

你还想用 AWS 做一些 orb 中没有的事情吗？orb 是开源的，所以在一个现有的 orb 上增加功能只是获得你的 PR 批准和合并的问题。查看 [orbs 注册表](https://circleci.com/developer/orbs)中所有可用的 orbs。你是否有一个用例，你觉得它与当前的 AWS orbs 集不同？你可以[自己创作一个](https://circleci.com/blog/how-to-make-an-easy-and-valuable-open-source-contribution-with-circleci-orbs/)并贡献给社区。我们甚至发布了为 orb 创建自动化构建、测试和部署管道的最佳实践([第 1 部分](https://circleci.com/blog/creating-automated-build-test-and-deploy-workflows-for-orbs/)和[第 2 部分](https://circleci.com/blog/creating-automated-build-test-and-deploy-workflows-for-orbs-part-2/))来帮助您。

要将 CI/CD 集成到您的 AWS 基础设施中，请让您的团队利用第三方服务并消除内部开发的需要。有了 orb，您的团队只需要知道如何使用这些服务，而不需要知道如何集成或管理它们。开始用 AWS 球体在 CircleCI 上建造。