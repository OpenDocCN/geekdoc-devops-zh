# CloudBees 替代品和竞争对手|线束

> 原文：<https://www.harness.io/blog/cloudbees-alternatives>

你在寻找 CloudBees 的替代品吗？我们对 Cloudbees 备选方案进行了细分，以便您可以评估所有最佳方案。

作为一种工具，CloudBees 提供了广泛的解决方案，包括开源 Jenkins 的企业版、持续集成、持续交付、功能标志管理等——但在本文中，我们将重点关注 CI/CD 以及如何最好地做出购买决定。在一个充斥着夸大产品的公司的世界里，很难做出明智的决定，也很难找出最佳选择，所以我们为你做了这项工作。

## CloudBees CD 替代品

下面，我们重点介绍几个我们认为是 CloudBees 真正替代品的竞争对手。虽然 Jenkins 有很多优点，但它并不意味着是一个 CD 工具，而是依赖脚本和插件来扩展。我们知道许多工程专业人士与 Jenkins 有着私人联系，因为它已经上市十多年了，但它并不能满足许多现代云原生组织的需求。在这里，我们来看一下能够更清楚地解决 CD 业务需求的优秀产品。

### GitLab

GitLab 是一个完整的 DevOps 平台，提供源代码管理、CI/CD、安全性、价值流管理、GitOps 等等。它的起源是作为一个源代码管理工具和 Git 库，它绝对是这个领域的行业领导者。他们的持续交付工具有助于加速和有效地改进软件发布过程。

GitLab 支持所有主要的云提供商，包括 AWS、Azure 和 GCP。它们还支持容器编排工具，如 Kubernetes 和 ECS。他们最近发布了与 Terraform 的集成。就语言而言，GitLab 的目标是支持每一种语言，无论新旧——包括 PHP、go、Ruby。NET、Java、JS 等等。

GitLab 有一个漂亮的用户界面，但它缺乏易用性。利用这个平台肯定需要一个学习过程。他们没有任何持续验证功能，但可以与 Prometheus 等工具集成，然而，这是他们为提高可见性而集成的唯一工具。同样，它们也没有本机机密管理功能，但可以与 Hashicorp Vault 集成。

对于寻求具有企业级支持的 CI/CD 工具的大型组织来说，它可能是正确的解决方案。最后，由于 GitLab 是一个如此大的产品，如果您遇到问题，有许多社区支持，如论坛、文档、视频教程等。

#### 特征

*   SaaS 和内部
*   云原生和传统应用支持
*   地形整合
*   完整的 GitOps 功能
*   手动金丝雀部署

在我们的产品对比页面中查看更深入的 [GitLab 与 Harness](https://harness.io/gitlab-cd-vs-harness/) 对比。

### Azure DevOps

2018 年，Azure DevOps 诞生了——随之而来的是各种用于 CI/CD、报告、工件、Git repos、测试等的工具。他们有各种各样的插件和扩展来支持这个平台。

作为一个 CD 工具，Azure DevOps 非常灵活，并提供可定制的管道。Azure DevOps 管道的一个(相当大的)缺点是在设置和配置过程中需要大量的脚本。但是，它们有管道模板和触发器来改进设置过程。

Azure DevOps 为 Azure、Kubernetes 和基于 VM 的资源提供支持。他们的 GitOps 功能仅用于管道，不用于发布。它们缺乏任何验证能力，回滚是使用插件实现的。您可以查看软件交付度量，例如构建历史和部署状态，但是仅限于确定部署速度。

不出意料，微软和谷歌最近与 Codefresh 合作，成为推荐的第三方 CI/CD 解决方案。随着最近对 GitHub 的收购，微软现在正在销售两款竞争产品:GitHub Action 和 Azure DevOps。我们不确定这对 ADO 意味着什么，但是微软的 Sasha Rosenbaum 已经声明他们[正朝着只有一个](https://medium.com/devops-cloud-it-career/azure-devops-or-github-c83fe1eced4d)的方向发展。要记住的事情！

#### 特征

*   SaaS 和内部
*   云原生和传统应用支持
*   有限的基础设施提供者
*   有限的 GitOps 功能
*   粒状 RBAC 和 SSO

查看更多深度产品评论 [Azure DevOps vs. Harness](https://harness.io/azure-devops-vs-harness/) 对比。

### 线束 CD

Harness 是首要的商业和企业级[连续交付解决方案](https://harness.io/platform/continuous-delivery/)，它非常强大，通过确保一种自助式、简单、高效的软件交付方法，为资源丰富或有限的团队而构建。

无论您的 Kubernetes 集群是在 GCP、Azure、AWS，甚至是自主开发/自托管的，Harness 都为您提供了将您的[掌舵图](https://harness.io/blog/continuous-delivery/what-is-helm/)部署到任意多个集群的能力。

Harness 基于内置或定制的部署模板自动生成所有部署脚本。它还拥有基于机器学习的部署验证，很快将成为自己的模块，名为[持续验证](https://harness.io/platform/continuous-delivery/continuous-verification/)，在[部署](https://harness.io/blog/continuous-verification/blue-green-canary-deployment-strategies/)后监控你的应用程序的异常。有关持续验证的更多信息以及一些先睹为快的截图，请访问我们关于[持续验证的重要性](https://harness.io/blog/continuous-verification/importance-of-continuous-verification/)的文章。

Harness 拥有与 Datadog、New Relic 和 AppDynamics 等可观测性平台的深度集成。基础架构供应器通过 CloudFormation 和 Terraform 的强大集成提供。此外，Harness 与吉拉和斯诺完美集成，用于问题跟踪。它还非常关注法规遵从性和治理，拥有细粒度的 RBAC、集成的机密管理、权限和审计跟踪。客户获得的支持水平在软件供应商中是无与伦比的。

完整的 Harness 软件交付平台具有持续交付、持续集成、功能标志、持续验证和[云成本管理](https://harness.io/platform/cloud-cost-management/)模块，允许您按需构建、测试、部署和验证。Harness 还通过收购无人机对开源社区进行了大量投资，你可以今天[下载](https://docs.drone.io/)。最后，Harness 现在完全支持 GCP、微软 Azure 和 AWS——在我们最近的[公告](https://devops.com/harness-empowers-developers-with-cloud-agnostic-end-to-end-software-delivery-platform/)中了解更多关于我们对厂商不可知的承诺。

#### 特征

*   提供免费增值选项，完整的平台即服务功能
*   SaaS 和内部
*   持续验证，金丝雀部署
*   具有命令行界面的完整 GitOps 功能
*   RBAC、单点登录、机密管理和审计跟踪
*   加速指标、仪表板和报告
*   快速启动允许在 15 分钟内完成管道

查看更深入的 [CloudBees 与 Harness](https://harness.io/cloudbees-electric-cloud-vs-harness/) 对比。

## CloudBees CI 替代方案

下面，我们重点介绍几个竞争对手，我们认为它们是 CloudBees 的真正持续集成替代方案。这些竞争对手长期以来在竞争情报行业获得了巨大的市场份额，并且在谈到持续集成工具时一直在进行讨论。

### 切尔莱西

CircleCI 在业内受到高度评价，因为它帮助开发人员安全可靠地推动成功的绿色建筑。该工具专注于测试所有提交的代码变更，已经被很多知名公司使用。与 Concourse CI 一样，构建配置位于一个文件中，必须添加该文件才能运行管道。拥有这些配置文件，并让它们存在于源代码控制管理中，允许开发人员在每个分支的基础上更改构建。

CircleCI 中的每个构建运行在一个干净的 Linux 容器中，可以通过 SSH 直接访问该容器。这允许快速和高级的调试来找出管道失败的原因。

CircleCI 有一个名为 Orbs 的资源，用于重用特定的工作流和管道配置。该资源使编写和定制 CircleCI 的配置变得简单，因为它们遵循可重用的配置特性，该特性允许开发人员定义参数化的配置元素，并在整个项目配置文件中重用这些元素。

CircleCI 可以做很多事情。你可以使用许多不同的语言( [Ruby、Python、PHP、Node 和 Java](https://circleci.com/docs/2.0/tutorials/?section=examples-and-guides)——哦，天哪！)、框架和依赖项，甚至还有一个用于定制集成的 API。它还提供了内置的 Docker 容器支持和 iOS 支持。尽管它提供了所有这些功能，但它目前确实缺乏更有经验的用户所需要的安全性和治理，并且内置的集成和插件列表并不广泛。就 Concourse CI 备选方案而言，CircleCI 在那里相当不错。

### 特征

*   不需要脚本
*   容器和云-原生
*   传统应用支持
*   GitOps 功能
*   集装箱化管道和插件

查看更深入的 [CircleCI 与无人机](https://harness.io/circleci-vs-drone/)对比。

### 无人机(线束 CI)

Harness 致力于开源社区，并计划保持他们的企业定价模式一致，以及保持无人机完全开源。今年 3 月，Harness 发布了无人机 2.0 的一些新功能。这次更新包括一个新的外观和感觉，增强了开发人员的体验，因为有图形来描述构建时间，图表和管道可视化。像 Concourse CI 一样，Drone 可以与 GitHub 和 Bitbucket 集成，也可以与其他源代码控制管理工具集成。它作为 GitOps 进程运行，并寻找一个 [yaml 文件](https://docs.drone.io/yaml/)进行配置。例如，它们都可以通过 GitHub 进行认证，但 Drone 提供了一个用户管理屏幕。

Drone 是容器原生的，因此所有构建都是隔离的，所有扩展都是标准化的。它也有大约 150 个容器化插件！这使得定制变得容易，并增加了工具的可扩展性。如果用户正在寻找的插件不存在，他们可以创建一个，这就像编写 bash/shell 脚本一样简单。为了让其他人可以使用它，用户可以通过发送一个 pull 请求将其添加到无人机注册网站。这就是开源的力量。

Drone 还提供了一个付费版本，该版本扩展了该工具，并为您的所有支持需求提供企业支持。

如果您正在寻找完整的 CI/CD 解决方案，Harness 将全力支持您！利用带来了持续部署、持续验证、云成本管理和[功能标志](https://harness.io/blog/feature-flags/build-or-buy-feature-flags/)(现已推出！)–以及持续集成企业。Harness 是首屈一指的商业和企业级[连续交付解决方案](https://harness.io/platform/continuous-delivery/)，功能强大得令人难以置信，它通过确保自助式、简单、智能的软件交付方法，为大型企业或小型商店而构建——所有这一切都是为了让您获得最佳体验。

### 特征

*   自助服务、智能和简单
*   不需要脚本
*   GitOps 功能
*   集装箱化管道和插件
*   轻量级可扩展性和基础设施
*   内置机密管理

查看更深入的 [Cloudbees 与无人机](https://harness.io/cloudbees-vs-drone/)对比。

### 我们不推荐替代者:詹金斯

Jenkins 是最著名的开源、自我管理的自动化工具，用于持续集成和持续部署。因为它是开源的，所有人都可以免费使用，并且有一个庞大的社区，这导致了额外的支持、文档和功能。因为这个社区，Jenkins 有超过 1800 个插件可用，这增加了工具的灵活性。插件的数量之多使得定制变得容易，并使 Jenkins 能够集成开发所需的几乎任何工具。

Jenkins 有一个必须添加的文件，以便管道运行。在这种情况下，它被称为 Jenkinsfile。这个文件需要大量的脚本。创建 Jenkins 管道需要一个学习过程，因为 groovy 语法并不总是最容易操作的。Jenkins 还缺少 [GitOps](https://harness.io/blog/devops/what-is-gitops/) 特性，因为没有一个集中的位置来保存用于版本控制的 Jenkins 文件。

谈到安装，詹金斯是相当容易的。这个 CI 工具是一个独立的 Java 程序，与平台无关。它适用于大多数主流操作系统，如 Linux、windows 和 macos，也可以作为普通安装程序和 war 文件使用。

虽然 Jenkins 肯定有很多好处，主要是因为我们上面提到的可扩展性，但事实是这是一个十年前的工具，不是为云原生而构建的——它肯定不适合敏捷团队。虽然是的，可扩展性非常好，但不得不依赖脚本和插件本身并不是一件好事。首先，插件作者经常放弃他们的插件(让你去寻找一个新的插件或者自己写)，插件会产生依赖链，插件会引入安全漏洞，插件升级和更新会增加相当大的工作量，等等。在一个如此依赖于额外扩展性的开发环境中推荐一个产品，我们会感到不舒服。

查看更深入的[詹金斯 vs 无人机](https://harness.io/jenkins-vs-drone/)对比。

## 从云蜂迁移到马具

是时候改变了吗？您是否正在寻找简单的自助式软件交付解决方案？‘harness’[软件交付平台](https://harness.io/platform/)打包了用于持续集成、持续交付、[云成本管理](https://harness.io/platform/cloud-cost-management/)和[特征标志](https://harness.io/platform/feature-flags/)的模块。为什么不[安排一次带吊带的演示](https://harness.io/platform-demo-request-lp)，看看我们是否适合您的开发人员和工程团队？