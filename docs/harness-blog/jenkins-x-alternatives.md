# 最佳 Jenkins X 替代品和竞争对手|线束

> 原文：<https://www.harness.io/blog/jenkins-x-alternatives>

你在寻找詹金斯 X 的替代品吗？我们分解了 Jenkins X 备选方案，以便您可以评估所有最佳选项。

啊，Jenkins X -经典的 Jenkins ' cooler，更 hip，rad 表弟(是的，我说 rad -欢迎回到 90 年代)。你会问，是什么让它变得如此酷？嗯:Jenkins X 是一个持续集成(CI)和持续部署(CD)平台，它是云原生的，运行在 Kubernetes 上，具有 CI 和 CD 管道自动化、GitOps、Terraform、secrets management、Tekton 管道，并且是开源的。

有些人可能想知道 Kubernetes 的好处是什么，我们会向您指出这一点:Jenkins X 不仅可以在 Kubernetes 上运行和扩展，而且它可以帮助您通过相对简单的设置将您的应用程序部署到 Kubernetes。每个作业通常在自己的容器中运行，特别运行，因此它可以放在集群中的任何位置，正如您在节点亲缘关系中所描述的那样。如果您有集群自动缩放器或类似的东西来增强集群的弹性，底层 kubelets 可以水平缩放。

所有这些都是说，与 OG Jenkins 相比，这是一个现代功能的宝库——当然也让各地的开发人员和工程团队的生活更加轻松。更少的工作，更少的设置问题，没有脚本，更少的配置。维护工时更少。插件更少。易于扩展。默认情况下，这是一个极好的消息。

然而，这是一个很大的“然而”，Jenkins X 仍然有不利的一面，如果你在这里，你显然认识到它们，并正在寻找不同的解决方案。在这篇博文中，我们将介绍一些肯定能满足你需求的 Jenkins X 替代方案——让你永远告别 jenkinsfile。

https://twitter.com/memenetes/status/1391860856057630721?s=20

## Jenkins X 要考虑的备选方案

### GitLab CI/CD

GitLab，最初是一个基于 Git 的源代码管理工具(像 GitHub 和 Bitbucket，供参考)，在他们的产品套件中引入了 CI/CD 解决方案。它们支持所有主要的云提供商，包括亚马逊网络服务(AWS)、微软 Azure 和谷歌云平台(GCP)。它们还支持容器编排工具，如 Kubernetes 和 ECS。在基础设施方面，GitLab 最近发布了一个 Terraform 集成。CI/CD 平台还在其企业级计划中提供了良好的治理和法规遵从性特性。

GitLab 有一个漂亮的用户界面，但它缺乏易用性。利用这个平台肯定需要一个学习过程。GitLab 没有任何部署验证功能，但可以与 Prometheus 等工具集成以实现可观察性。GitLab 也缺乏本地机密管理功能，但集成了 HashiCorp Vault。

至于 Jenkins X 的替代品，GitLab 是一个相当好的推荐，特别是如果 GitLab 已经是您的源代码管理器的选择。

#### GitLab 特性

*   SaaS 和内部
*   云原生和传统应用支持
*   地形整合
*   完整的 GitOps 功能
*   手动金丝雀部署
*   原始的 Linux 安装

### Codefresh

就 CI/CD 平台而言，Codefresh 在几个领域达到了目标——即当它涉及到现代特性时。他们似乎击中了所有的流行语，像[管道作为代码](https://harness.io/blog/devops/pipeline-as-code/)，GitOps 2.0，[自动预览环境](https://codefresh.io/docs/docs/testing/automatic-preview-environments/)，Kubernetes -哦，我的天！

这个基于云的原生容器平台拥有强大的治理功能，如 RBAC、审计跟踪、SAML SSO、通过加密的内置秘密管理(或者，如果你是第三方的粉丝，可以使用 HashiCorp Vault)等等。

此外，Codefresh 还可以集成所有主要的云和 Git 提供商，包括吉拉、Helm、Argo 和 GitHub Actions。谈到可观察性，Codefresh 用[蜂巢](https://www.honeycomb.io/)来处理。

对 Codefresh 最大的担忧是资源消耗和停机时间(即使是部分的)。浏览他们的状态页面，有相当多的[中断](https://status.codefresh.io/uptime?page=1)。至于资源消耗，[客户评论](https://www.g2.com/products/codefresh-codefresh/reviews)指出，由于 Codefresh 消耗了相当数量的资源，因此检查计划限额很容易。然后，他们需要支付日常费用，如果你预算紧张，这可不是什么好事。

#### Codefresh 功能

*   稳健的治理
*   小型团队的免费计划
*   Saas 和内部部署
*   用于内部安装的 Codefresh 转轮
*   完整的 GitOps 功能
*   预建管道模板

### 马具

Harness 是首要的商业和企业级[连续交付解决方案](https://harness.io/platform/continuous-delivery/)，它非常强大，通过确保一种自助式、简单、高效的软件交付方法，为资源丰富或有限的团队而构建。

无论您的 Kubernetes 集群是在 GCP、Azure、AWS，甚至是自主开发/自托管的，Harness 都为您提供了将您的[掌舵图](https://harness.io/blog/continuous-delivery/what-is-helm/)部署到任意多个集群的能力。

Harness 基于内置或定制的部署模板自动生成所有部署脚本——听到了吗，Jenkins？没错，没有脚本！它还拥有基于机器学习的部署验证，很快将成为自己的模块，名为[持续验证](https://harness.io/platform/continuous-delivery/continuous-verification/)，在[部署](https://harness.io/blog/continuous-verification/blue-green-canary-deployment-strategies/)后监控你的应用程序的异常。有关持续验证的更多信息以及一些先睹为快的截图，请访问我们关于[持续验证的重要性](https://harness.io/blog/continuous-verification/importance-of-continuous-verification/)的文章。

Harness 拥有与 Datadog、New Relic 和 AppDynamics 等可观测性平台的深度集成。基础架构供应器通过 CloudFormation 和 Terraform 的强大集成提供。此外，Harness 与吉拉和斯诺完美集成，用于问题跟踪。它还非常关注法规遵从性和治理，拥有细粒度的 RBAC、集成的机密管理、权限和审计跟踪。

完整的 Harness 软件交付平台具有持续交付、持续集成、功能标志、持续验证和[云成本管理](https://harness.io/platform/cloud-cost-management/)模块，允许您按需构建、测试、部署和验证。Harness 还通过收购无人机对开源社区进行了大量投资，你可以今天[下载](https://docs.drone.io/)。最后，Harness 现在完全支持 GCP、微软 Azure 和 AWS——在我们最近的[公告](https://devops.com/harness-empowers-developers-with-cloud-agnostic-end-to-end-software-delivery-platform/)中了解更多关于我们对厂商不可知的承诺。

#### 线束特征

*   提供免费增值选项
*   SaaS 和内部
*   持续验证，金丝雀部署
*   具有命令行界面的完整 GitOps 功能
*   RBAC、SSO 和秘密管理
*   加速指标、仪表板和报告
*   快速启动允许在 15 分钟内完成管道

## 我们不推荐的 Jenkins X 替代品

### 大三角帆

Spinnaker 最初由网飞创建，是一个 CD 平台，只是有太多的问题需要我们推荐。我们在我们的 [Spinnaker Alternatives](https://harness.io/blog/continuous-delivery/spinnaker-alternatives/) 帖子中讨论了它们，但是概括一下:它仅是内部部署的，缺乏本机机密管理，不提供传统的应用程序支持，并且被称为设置和配置的“噩梦”。对于一个不提供原生 CI，只提供 CD 的平台来说，这似乎不值得付出努力。

### 云蜂

Cloudbees 是基于 Jenkins / Jenkins X 构建的，这取决于您使用的是他们的哪种解决方案，它实际上只是一个升级版本，具有一些额外的功能和企业支持。所以，如果你在这里，你在寻找詹金斯 X 的替代品，这些不是你要找的机器人。往前走。

## 从 Jenkins X 迁移到 Harness

是时候改变了吗？您是否正在寻找简单的自助式软件交付解决方案？Harness' [软件交付平台](https://harness.io/platform/)包含用于持续集成、持续交付和[云成本管理的模块](https://harness.io/platform/cloud-cost-management/) -即将推出功能标志和持续验证！为什么[不安排一次带吊带的演示](https://harness.io/platform-demo-request-lp)，看看我们是否适合您的开发人员和工程团队？