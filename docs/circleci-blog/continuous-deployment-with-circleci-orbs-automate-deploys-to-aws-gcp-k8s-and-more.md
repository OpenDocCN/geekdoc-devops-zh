# CircleCI orbs 的持续部署

> 原文：<https://circleci.com/blog/continuous-deployment-with-circleci-orbs-automate-deploys-to-aws-gcp-k8s-and-more/>

为了提高上市速度，开发团队已经开始从自动化他们的测试转移到 DevOps 成熟度的下一个阶段——自动化他们的部署。虽然这可能看起来是一个令人生畏的步骤，但我们已经构建了一组新的 orb，专门用于通过向团队的配置中添加几行预打包的代码来帮助团队进行这一大转变。通过简化自动化部署过程，团队可以更快地发布新功能，并利用尖端技术，而不必彻底检查现有基础架构。

自动化部署的团队可以更快地将代码推向市场。那么为什么不是每个团队都简单地自动部署呢？有几个关键的障碍阻止团队设置他们的 CI/CD 管道来自动部署。首先，代码覆盖率。对他们的代码没有信心的团队不会——也不应该——自动化他们的部署。为了解决这个问题，我们将特定于测试的 orb 放在一起，以帮助团队确定他们的代码覆盖率以及他们的测试在哪里缺乏。查看我们关于测试驱动开发的[帖子](https://circleci.com/blog/how-to-test-software-part-ii-tdd-and-bdd/)，了解更多关于如何编写最有用的测试的想法。

一旦团队对他们的代码覆盖率有信心，他们通常会在实际自动化部署的时候陷入困境，因为不是所有的 CI/CD 工具都允许您部署到现代服务，如 AWS ECS、AWS Serverless 或 Google Cloud Run。而且，当您必须自己编写这些集成时，可能需要几个小时的开发时间。我们设计了部署球来克服这个障碍。您可以使用 orb 直接从 CI/CD 管道部署到任何测试或生产环境，设置过程只需几分钟而不是几小时。每个 orb 都是专为满足现代团队的需求而设计的——提供无缝、开箱即用的解决方案来集成尖端技术，并帮助团队达到 DevOps 成熟度的新水平。

了解如何使用 orb 自动部署到流行的服务:

### 使用这些 orb 部署到云服务:

安装 Heroku CLI 并将应用程序部署到 Heroku。

[AWS CodeDeploy](https://circleci.com/developer/orbs/orb/circleci/aws-code-deploy)
将应用程序部署到 AWS CodeDeploy。

[亚马逊 ECS](https://circleci.com/developer/orbs/orb/circleci/aws-ecs)
支持 EC2 和 Fargate 启动类型和蓝/绿部署。

[Cloud Foundry](https://circleci.com/developer/orbs/orb/circleci/cloudfoundry)
向 Cloud Foundry 推送部署应用。

[AWS SAM 无服务器](https://circleci.com/developer/orbs/orb/circleci/aws-sam-serverless)
利用 AWS 无服务器应用程序模型，在 CircleCI 上构建、测试和部署您的 AWS 无服务器应用程序。

[Google Cloud Run](https://circleci.com/developer/orbs/orb/circleci/gcp-cloud-run)
构建无状态映像并将其部署到 Google Cloud Run，作为无服务器应用程序。

### 使用这些球体部署到 Kubernetes:

[OpenShift (RedHat)](https://circleci.com/developer/orbs/orb/circleci/redhat-openshift)
自动化 Kubernetes 应用的构建、部署和管理。

[Helm](https://circleci.com/developer/orbs/orb/circleci/helm)
用 Helm Charts 查找、分享、使用为 Kubernetes 打造的软件。

[Kublr](https://circleci.com/developer/orbs/orb/kublr/kublr-api)
在您的所有环境中集中部署、运行和管理大型企业的多集群 Kubernetes 部署。

当您运行 Kubernetes 集群时，在开发的早期检测漏洞和错误配置漂移。

[谷歌云 GKE](https://circleci.com/developer/orbs/orb/circleci/gcp-gke)
部署 Kubernetes 集群，并在几秒钟内从您的 CI 渠道更新生产代码。

有了 Azure Kubernetes 服务，Azure AKS
运输速度更快、操作更轻松、扩展更自信。

亚马逊 EKS
在 AWS 上使用 Kubernetes 部署、管理和扩展容器化应用。

[DeployHub](https://circleci.com/developer/orbs/orb/deployhub/deployhub-orb)
自动化您工作流程的发布步骤，为您管道中的每个状态创建 100%可重复的部署流程。

[Fairwinds](https://circleci.com/developer/orbs/orb/fairwinds/rok8s-scripts)
提供了一个用 Docker 和 Kubernetes 构建 GitOps 工作流的框架。

[VMware 代码流](https://circleci.com/developer/orbs/orb/vmware/codestream)
发布更高质量的应用和更快的 IT 代码，同时降低运营风险。

在 CircleCI 上使用 Kubernetes 的工具集合。

[Rafay Systems](https://circleci.com/developer/orbs/orb/rafaysystems/rafay)
使用 Rafay Systems 平台部署 CircleCI 的 Kubernetes 集群。

### 浏览我们的其他部署资源:

[Pulumi](https://circleci.com/developer/orbs/orb/pulumi/pulumi)
支持预览对 Pulumi 堆栈所做的任何云基础架构更改。

作为 CircleCI 工作流程的一部分，将您的应用程序部署到 Convox 平台。

[Spinnaker](https://circleci.com/developer/orbs/orb/circleci/spinnaker)
使用 Spinnaker 简化部署。

[realMethods](https://circleci.com/developer/orbs/orb/realmethods/appgen)
生成 MPV 质量的应用程序，可立即部署到 CircleCI。

[Quali](https://circleci.com/developer/orbs/orb/quali/cloudshell-colony)
将 CloudShell Colony 集成到您的 CI/CD 管道中。您可以使用可用的构建任务从任何蓝图创建一个沙箱，开始您的测试，并在完成后结束沙箱。

[Pantheon](https://circleci.com/developer/orbs/orb/pantheon-systems/pantheon)
将 Drupal 和 WordPress 代码推送到 Pantheon Dev 和 Multidev 环境中。

在 CI/CD 过程中，隐藏代码中对特性标志的所有引用。

[Salesforce](https://circleci.com/developer/orbs/orb/circleci/salesforce-sfdx)
针对 CircleCI 的 Salesforce SFDX CLI 集成。为您的 Salesforce 集成轻松创建 CI/CD 渠道。

### 我们的合作伙伴在说什么

谷歌云混合伙伴关系主管 Rayn Veerubhotla 表示:

> “通过推出这些新的[orb]，CircleCI 使开发人员能够进一步简化他们在云运行上的体验，最终帮助企业更快地为他们的客户带来新的服务和产品。”

Convox 的联合创始人 Cameron Gray 描述了他构建部署 orb 的经历:

> “与 CircleCI 合作建造 Convox orb 既简单又强大。借助 Convox CircleCI orb，我们的客户能够构建应用程序、运行测试并部署到运行在多个云上的 Kubernetes 集群，所有这些都只需一个工作流程，这将为用户节省时间来做最重要的事情。”

### 你能做什么

对于部署，您还想做一些 orb 中没有的事情吗？orb 是开源的，所以在一个现有的 orb 上增加功能只是获得你的 PR 批准和合并的问题。查看 [orbs 注册表](https://circleci.com/developer/orbs)中所有可用的 orbs。您是否有一个用例，您觉得它与当前的部署 orb 集有所不同？你可以[自己创作一个](https://circleci.com/blog/how-to-make-an-easy-and-valuable-open-source-contribution-with-circleci-orbs/)并贡献给社区。我们甚至发布了为 orb 创建自动化构建、测试和部署管道的最佳实践([第 1 部分](https://circleci.com/blog/creating-automated-build-test-and-deploy-workflows-for-orbs/)和[第 2 部分](https://circleci.com/blog/creating-automated-build-test-and-deploy-workflows-for-orbs-part-2/))来帮助您。

为了在您的部署过程中建立信心，开始使用 orb 来测试代码覆盖率，轻松地与第三方工具集成，并自动部署到您首选的测试或生产环境。与其花时间自己开发集成和复杂的部署流程，不如使用 orb 轻松地自动化部署。

[开始在 CircleCI 建造部署球](https://circleci.com/developer/orbs)。