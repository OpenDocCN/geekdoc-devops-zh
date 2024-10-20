# 要考虑的最佳 Spinnaker 替代品和竞争对手|线束

> 原文：<https://www.harness.io/blog/spinnaker-alternatives>

您是否正在寻找持续交付解决方案？我们分解了 Spinnaker 备选方案，以便您可以评估所有最佳选项。

正如你们许多人可能知道的，Spinnaker 是网飞早在 2015 年内部构建他们自己的[持续交付工具](https://harness.io/blog/continuous-delivery-tools/)的著名成果。2015 年末，他们将该工具作为开源的一部分发布，并于 2017 年发布了 Spinnaker 的第一个版本。

Harness 对 Spinnaker 及其对[连续交付](https://harness.io/blog/what-is-continuous-delivery/)的贡献深表敬意；这是第一个接受挑战的开源项目。

像许多开源 CD 工具一样，它也有缺陷。用户很乐意尝试 Spinnaker，因为它的入门门槛很低，尤其是对于那些不想编码的人。不幸的是，对许多人来说，他们意识到 [Spinnaker 可能还不够](https://harness.io/use-cases/spinnaker/)。

这不会是对所有[连续交付平台](https://harness.io/platform/continuous-delivery/)的客观评估，除非我们分析 Spinnaker 的优点和缺点。

### 三角帆的优点

**开源:**这大概是使用 Spinnaker 最大的 pro 了。拥有一个自由的框架(有限制)可以给内部团队很多开发的自由，让他们在如何构建他们的持续交付平台上更有创造性。

**多云部署:** Spinnaker 在多云环境中很有用，它还可以与 Kubernetes 集成。

**基础设施供应商:** Spinnaker 在很长一段时间内没有提供与 Terraform 的任何集成。最近，他们推出了一个平台整合，让你可以利用它。

### 三角帆的缺点

**缺乏对传统应用程序的支持:** Spinnaker 被设计为完全云原生的，不支持 Java 或. NET 中的应用程序。

**缺乏机密管理:**在网络安全需求空前高涨的今天，Spinnaker 没有提供内置的机密管理工具。当然，也有可用的第三方集成，但是缺乏本地工具仍然是一个不利因素。

**仅本地:** Spinnaker 不提供 SaaS 选项，仅限于寻求本地解决方案的用户。

## 我们的评估方法

在选择适合您需求的正确 CD 工具时，需要考虑许多不同的因素。我们希望为您提供尽可能多的信息和足够的资源，以便您做出明智的决定。考虑到这一点，我们对以下工具进行了评估:

*   用户体验、分析和报告
*   平台支持和集成
*   持续交付概念和能力
*   [部署策略](https://harness.io/blog/blue-green-canary-deployment-strategies/)
*   部署验证和运行状况检查
*   与主要云提供商的集成
*   自动回滚功能
*   微服务部署流程控制和影响分析
*   测试自动化功能
*   安全性
*   支持

根据我们的分析，我们提供了前 6 个 Spinnaker 备选方案，我们认为在您探索下一个连续交付平台时值得一试。

## 三角帆的替代品

### 线束 CD

Harness 为寻求强大而简单的平台的资源紧张的团队提供了业界第一个商业和企业级连续交付解决方案，避免了通过开源工具进行繁重工作的需要。

Harness 基于内置或定制的部署模板自动生成所有部署脚本。它还具有基于机器学习的[连续验证](https://harness.io/platform/continuous-delivery/continuous-verification/)，确保前后性能和功能。它与标准监控系统深度集成，通知(警报)可以发送到您组织选择的聊天工具，如 Slack 或 Hipchat。

Harness 还非常关注法规遵从性和治理，拥有细粒度的 RBAC、集成的机密管理等等。您还可以创建关于安全性、性能和质量的合规性规则，然后在您的 [CD 管道](https://harness.io/blog/ci-cd-pipeline//)中自动执行这些规则，并获得记录流程每一步的完整审计跟踪。

Harness [软件交付平台](https://harness.io/platform/)具有持续交付、持续集成和云成本管理模块，允许您按需构建、测试、部署和验证。Harness 还通过收购[无人机](https://drone.io/)对开源社区进行了大量投资。最后，Harness 现在完全支持谷歌云平台、微软 Azure 和 AWS。

#### 特征

*   SaaS 和内部
*   云原生应用支持
*   完整的 GitOps 功能
*   RBAC 和秘密管理
*   加速指标和报告

*查看更深入的*[*Spinnaker vs .*](https://harness.io/spinnaker-vs-harness/)*对比。*

### 阿尔戈光盘

2018 年，Intuit 收购了一家名为 Applatix 的公司，这是 Argo 的原始创造者。从那时起，他们推出了 3 个新项目:阿尔戈 CD，阿尔戈推出，和阿尔戈事件。

Argo CD 是 Kubernetes 的开源 GitOps 连续交付解决方案。它自动化了 Kubernetes 的部署，并支持多种配置管理工具。它提供自动化的连续部署和 SDLC 管理。

Argo CD 的成功之处在于它的 GitOps 功能。在定义应用程序的状态时，它使用 Git 存储库作为事实的来源。Argo CD 提供了任何 Kubernetes 资源的基于 GitOps 的声明性部署，包括 Argo 事件、服务和跨多个 Kubernetes 集群的部署。

Argo CD 的现代功能可能会成为一个障碍-最大的障碍是 Argo CD 的自动部署能力。它可能需要改变部署是如何触发的:阿尔戈光盘“手表”你的码头和舵图 repos，它可以自动部署时，变化被发现。这对高级工程师来说可能是一件很棒的事情，但初学者将会有一个很大的学习曲线。

Argo CD 绝对是更高级的 CD 工具。此外，Argo CD 缺乏本地机密管理，而是依赖于第三方集成，如 HashiCorp Vault。最后，你需要集成一个[持续集成工具](https://harness.io/blog/continuous-integration-tools/)，比如 Harness CI 或 CircleCI，因为 Argo CD 不能解决 CI 问题。

#### 特征

*   仅限内部部署
*   广泛的 Kubernetes 支持
*   完整的 GitOps 功能
*   库伯内特 RBAC
*   用于洞察和报告的 REST API

*查看更深入的*[*Argo CD vs .*](https://harness.io/learn/developer/devops-tools/argocd-vs-harness/)*对比。*

### GitLab

Gitlab 是一个完整的 DevOps 平台，提供源代码管理、 [CI/CD](https://harness.io/blog/what-is-ci-cd/) 、安全性、价值流管理、GitOps 等等。它的起源是作为一个源代码管理工具和 Git 库。他们的持续交付工具有助于加速和有效地改进软件发布过程。他们有一个端到端的 DevOps 解决方案，这使他们成为一个灵活的管道管理系统。

GitLab 支持所有主要的云提供商，包括 AWS、Azure 和 GCP。它们还支持容器编排工具，如 Kubernetes 和 ECS。他们最近发布了与 Terraform 的集成。

GitLab 有一个漂亮的用户界面，但它缺乏易用性。利用这个平台肯定需要一个学习过程。他们没有任何持续验证能力，但可以与 Prometheus 等工具集成，然而，这是他们为提高可见性而集成的唯一工具。

同样，它们也没有本机机密管理功能，但可以与 Hashicorp Vault 集成。但是，他们的企业级计划中确实有良好的治理和法规遵从性特性。

#### 特征

*   SaaS 和内部
*   云原生和传统应用支持
*   地形基础设施供应商
*   完整的 GitOps 功能
*   手动金丝雀部署

*查看更深入的*[*git lab vs . Harness*](https://harness.io/gitlab-cd-vs-harness/)*对比。*

### Azure DevOps

早在 2005 年(是的，15 年前)，微软 Azure 就开始了对 DevOps 的初始投资，目的是为希望以高速度进行软件变更的软件开发团队提供帮助。

2018 年末，他们宣布了 Azure DevOps，其中包括 CI/CD、报告、工件、Git repos、测试等各种工具。他们有各种各样的插件和扩展，比如 Slack，可以和平台一起工作。

作为一个 CD 工具，Azure DevOps 非常灵活，并提供可定制的管道。Azure DevOps 管道的一个(相当大的)缺点是在设置和配置过程中需要大量的脚本。但是，它们有管道模板和触发器来改进设置过程。

Azure DevOps 为 Azure、Kubernetes 和基于 VM 的资源提供支持。他们的 GitOps 功能仅用于管道，不用于发布。它们缺乏任何验证能力，回滚是使用插件实现的。您可以查看软件交付度量，例如构建历史和部署状态，但是仅限于确定部署速度。

#### 特征

*   SaaS 和内部
*   云原生和传统应用支持
*   有限的基础设施提供者
*   有限的 GitOps 功能
*   粒状 RBAC 和 SSO

*查看更深入的*[*Azure devo PS vs . Harness*](https://harness.io/learn/developer/devops-tools/azure-devops-vs-harness/)*对比。*

### 章鱼部署

Octopus Deploy 是一个持续的交付工具，它与您的构建服务器协同工作，以管理发布和自动化部署。从功能角度来看，它包含了大量的部署策略选项和良好的支持，并且可以与大多数持续集成工具/服务器集成。

从可用性的角度来看，它相对容易导航，这对于新的开发团队来说非常有吸引力。这些功能相对简单，例如，没有 YAML。他们推出了代码配置功能，以在 Git 存储库中提供版本控制。

简单性可以从提供完成工作的基本工具转变为缺少高级开发团队完成任务所需的特性的工具。没有可观察性功能，并且该工具缺乏有用的报告和度量。

在持续交付领域，他们绝对是一个值得关注的后起之秀。他们有超过 450 个集成，并且非常专注于为几乎没有开发经验的团队提供低代码工具。他们有很好的文档和支持，并根据客户需求提供定期更新。

#### 特征

*   SaaS 和内部
*   云原生和传统应用支持
*   部署工作流的手动配置
*   手动回滚
*   基本 RBAC 和单点登录

*查看更深入的*[*Octopus Deploy vs . Harness*](https://harness.io/octopus-deploy-vs-harness/)*对比。*

### 军械库

军械库建立在 Spinnaker 之上，但是通过军械库的许多模块拥有扩展的功能。不幸的是，它是一个付费平台，所以如果您正在寻找 Spinnaker 的开源替代方案，这不是您的正确选择。

扩展的军械库模块包括可观察性、秘密管理、作为代码的管道和扩展的治理的功能。因为这是 Spinnaker 企业版，所以您还可以获得支持和培训，这是 Spinnaker 用户经常提到的不足之处。

因为它是在 Spinnaker 上构建的，所以它仍然会遇到开源平台上遇到的类似问题。高昂的维护成本和困难的可用性使得任何地方都需要 2-4 名全职工程师来保持平台的运行——这包括初始设置和配置，以及持续的维护。军械库三角帆也缺乏灵活性。

所有这些加在一起，是一个昂贵的解决方案，我们不确定这个价格是否值得开源 Spinnaker 的可扩展性。

#### 特征

*   企业支持
*   与 Spinnaker 同步
*   治理和机密管理
*   管道作为代码
*   地形整合

## 您可能听说过的其他工具

在您寻找下一个[软件交付平台](https://harness.io/platform/)时，您可能会听到许多其他可用的工具。在很大程度上，这些工具被归入 CI/CD 类别，但主要以 CI 为中心。

### 詹金斯

Jenkins 是另一个流行的开源平台，许多人认为它是流行的 Spinnaker 替代平台。它主要是一个 CI 工具，需要大量的脚本和辛劳才能扩展到 CD，而且还有 [Jenkins 的替代品](https://harness.io/blog/best-jenkins-alternatives/)你也应该考虑。Jenkins 在 CI/CD 领域越来越老了，我们相信您使用上面列出的其他工具会更好。

*查看* [*马具 vs*](https://harness.io/jenkins-vs-harness/)*的更深入对比。*

### 切尔莱西

CI/CD 领域中另一个经常提到的工具是 CircleCI。顾名思义，他们的专长是作为 CI 工具的[持续集成](https://harness.io/blog/what-is-continuous-integration/)，尽管他们也有一些 CD 工具的功能。

*查看* [*无人机 vs CircleCI*](https://harness.io/circleci-vs-drone/) *更深入的对比。*

## 从 Spinnaker 迁移到线束

如果您正在努力交付您的软件或者构建您的软件交付解决方案，那么是时候考虑像 Harness 这样的 SaaS 解决方案了。您正在寻找 Spinnaker 的替代品，所以请确保您投资了一个可以像您一样扩展的工具。利用[软件交付平台](https://harness.io/platform/)将持续集成、持续交付和[云成本管理](https://harness.io/platform/cloud-cost-management/)的模块打包在一起。使用 Harness 按需构建、测试、验证和部署-立即开始您的 Harness[免费试用](https://harness.io/free-trial/)。