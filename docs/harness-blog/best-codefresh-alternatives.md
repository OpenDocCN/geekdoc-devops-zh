# 值得考虑的最佳 Codefresh 替代方案| Harness

> 原文：<https://www.harness.io/blog/best-codefresh-alternatives>

寻找 Codefresh 的替代品？我们会保护你的。无论您只是在寻找一个 CI 工具、一个 CD 工具，还是一个完整的 CI/CD 平台，我们都会在这里为您提供最佳选择。

Codefresh 是 Kubernetes 应用程序的头号 GitOps 自动化平台，作为一种持续集成和持续部署工具，帮助软件团队构建强大的管道，受到数千家公司的信任。

Codefresh 确实在一些领域脱颖而出——对治理的关注非常强烈，包括 SAML SSO、审计跟踪、基于角色的权限(RBAC——基于角色的访问控制),以及称为 Secret Storage 的秘密管理的本机集成。Codefresh 还拥有完全容器化的体验，每个步骤都可以跨管道重用——没有像 Jenkins 这样的 CI 工具会带你经历的[依赖地狱](https://harness.io/blog/continuous-delivery/dependency-plugin-hell-jenkins/)。可以使用预构建的管道步骤集成；或者，可以灵活地使用任何 docker 图像。

但是——如果你在这里，那是因为你在寻找 Codefresh 的替代品。我们将把它分为持续集成工具、持续部署工具和完整的 CI/CD 平台。

## 要考虑的持续集成备选方案

### 切尔莱西

CircleCI 在业内受到高度评价，因为它帮助开发人员安全可靠地推动成功的绿色建筑。该工具专注于测试所有提交的代码变更，已经被很多知名公司使用。与 Travis CI 一样，构建配置位于一个文件中，必须添加该文件才能运行管道。拥有这些配置文件，并让它们存在于源代码控制管理中，允许开发人员在每个分支的基础上更改构建。

CircleCI 中的每个构建运行在一个干净的 LXC 容器中，该容器可以直接放入。这允许快速和高级的调试来找出管道失败的原因。此外，在每个构建容器中，都有预安装的包，包括最流行的数据库、语言和框架。这使它变得容易，因为容器已经准备好了。如果有没有包含的配置元素，CircleCI 有一个资源叫做 Orbs(把 Orbs 想象成插件)。该资源使编写和定制 CircleCI 的配置变得简单，因为它们遵循可重用的配置特性，该特性允许开发人员定义参数化的配置元素，并在整个项目配置文件中重用这些元素。

CircleCI 可以做很多事情。您可以在构建中使用许多不同的语言、框架和依赖项，甚至还有一个用于定制集成的 API。它还提供了内置的 Docker 支持和 iOS 支持。尽管它提供了所有这些功能，但它目前确实缺乏更有经验的用户所需要的安全性和治理，并且内置的集成和插件列表并不广泛。然而，CircleCI 让软件工程师很容易就进入状态——在一个小的学习曲线之后。

这个解决方案不是开源的。我们之所以提到这一点，是因为下面的许多选择都是。CircleCI 确实提供了一个免费的计划，但是对于较大的组织来说还不够——仅仅是个人项目，或者是刚刚开始 CI/CD 之旅的小企业。

### 雄蜂

早在 2012 年，Brad Rydzewski 就创立了一个名为[无人机](https://www.drone.io/)的开源项目。Brad 创建了 Drone，为工程师提供了一个容器原生、可扩展且易于使用的自助式 CI 平台。无人机蓬勃发展，在 DockerHub 上获得了超过 1 亿次点击，5 万活跃用户，275+贡献者，在 GitHub 上获得了 22.5k 颗星。Harness 于 2020 年 8 月收购了 Drone，以进一步履行其为工程师和 DevOps 团队简化软件交付的使命。

当 Harness 收购无人机时，它承诺永远保持开源。Harness 最近通过大规模发布重申了其对开源解决方案的投资，其中添加了更时尚的界面、新的可视化管道构建器、治理和安全功能以及实时调试工具。虽然这个功能丰富的版本是免费的，但也有一个付费版本的无人机，它提供了企业支持和更多的集成和功能。附加功能包括秘密管理选项，自动缩放，自定义插件，等等。

无人机为这样一个轻量级的工具提供了强大的动力。它功能丰富，每个功能都有许多选项/集成，允许您选择自己喜欢的第三方供应商。它拥有一个面向初学者的安装程序:安装如此简单，5 分钟就能完成，入门门槛相当低。不需要脚本——每个管道都是代码配置，每个步骤都在自己的容器中运行，插件也是容器化的，确保零插件依赖性。

## 要考虑的连续交付替代方案

### 阿尔戈光盘

Argo CD 使用 GitOps 技术自动部署 Kubernetes。Argo CD 支持多种配置管理工具、SSO 集成和 web hook 集成，为开发人员提供了一种简单而有效的部署方法。Argo 通过使应用程序定义、配置和环境具有声明性和版本控制性，减少了管理工作量。Argo 的既定目标是使应用程序的部署自动化、可审计且易于理解。

建立在 Kubernetes 之上，Argo CD 提供了一个现代的持续部署。这是一个非常可定制的自助式工具，它提供了 [waves](https://argoproj.github.io/argo-cd/user-guide/sync-waves/) ，因此您可以将更多的部署步骤推进到 Kubernetes 中，从而消除了对 Jenkins 中 CD 的需求。此外，它还提供多集群和多租户功能、RBAC、单点登录、审计跟踪、使用 [Kustomize 和 Helm](https://harness.io/blog/devops/helm-vs-kustomize/) 进行模板化、通过 web 钩子进行金丝雀部署等等。

### GoCD

GoCD 是一个由全球软件公司/咨询公司 ThoughtWorks 赞助的开源工具。这个工具拥有过多的特性，比如作为代码的管道、原生的 Kubernetes 集成、弹性代理、JSON 和 YAML 支持以及一个现代化的接口。然而，它是一个较新的工具——因此，有经验的用户可能不喜欢缺少更高级的功能。除此之外，还有相当数量的插件可供使用——没有像 Jenkins 这样强大的插件，但足以增强工具的可扩展性。GitHub 上有 6k 个明星，超过 120 个贡献者，还有一个活跃的社区，它不是一个可以嘲笑的工具。它有助于软件部署顺利进行。

GoCD 似乎击中了现在所有的热门词汇:作为代码的管道、Kubernetes 和 GitOps -哦，我的天！围绕该工具还有一个非常活跃的社区，所以文档问题在这个开源解决方案中并不是大问题。该工具本身也相当可靠，它以并行步骤运行管道，是自助式的，支持大多数语言和框架。

## CI/CD 平台

### GitLab CI/CD

GitLab，最初是一个基于 Git 的源代码管理工具(像 GitHub 和 Bitbucket，供参考)，在他们的产品套件中引入了 CI/CD 解决方案。它们支持所有主要的云提供商，包括亚马逊网络服务(AWS)、微软 Azure 和谷歌云平台(GCP)。它们还支持容器编排工具，如 Kubernetes 和 ECS。在基础设施方面，GitLab 最近发布了一个 Terraform 集成。

使用 GitLab 的一个主要动机是它的易用性和基本的内置功能。一切都可以在一个地方。单个应用程序可以处理从源代码控制管理到发布和监控的整个 DevOps 生命周期。集成和权限只需要一个应用程序。在 GitLab 中，它为您提供了更深入的见解，如存储库中使用的编程语言、代码覆盖历史以及每月、每天和每小时的提交次数。这使用户能够看到 CI/CD 流程，并对其有更透彻的了解。

CI/CD 平台还在其企业级计划中提供了良好的治理和法规遵从性特性。GitLab 有一个漂亮的用户界面，但它缺乏易用性。利用这个平台肯定需要一个学习过程。

就 Travis CI 替代品而言，GitLab 是一个相当好的推荐，尤其是如果 GitLab 已经是您选择的源代码管理器的话。这不是一个开源的解决方案。与 CircleCI 类似，它提供了一个基本计划，以及一个利用更多功能的企业计划。

### 马具

Harness 是首要的商业和企业级[连续交付解决方案](https://harness.io/platform/continuous-delivery/)，它非常强大，通过确保一种自助式、简单、高效的软件交付方法，为资源丰富或有限的团队而构建。

无论您的 Kubernetes 集群是在 GCP、Azure、AWS，甚至是自主开发/自托管的，Harness 都为您提供了将您的[掌舵图](https://harness.io/blog/continuous-delivery/what-is-helm/)部署到任意多个集群的能力。

Harness 基于内置或定制的部署模板自动生成所有部署脚本——听到了吗，Jenkins？没错，没有脚本！它还有基于机器学习的部署验证，很快将成为自己的模块，名为[持续验证](https://harness.io/platform/continuous-delivery/continuous-verification/)，在[部署](https://harness.io/blog/continuous-verification/blue-green-canary-deployment-strategies/)后监控你的应用程序的异常。有关持续验证的更多信息以及一些先睹为快的截图，请访问我们关于[持续验证的重要性](https://harness.io/blog/continuous-verification/importance-of-continuous-verification/)的文章。

Harness 拥有与 Datadog、New Relic 和 AppDynamics 等可观测性平台的深度集成。基础架构供应器通过 CloudFormation 和 Terraform 的强大集成提供。此外，Harness 与吉拉和斯诺完美集成，用于问题跟踪。它还非常关注法规遵从性和治理，拥有细粒度的 RBAC、集成的机密管理、权限和审计跟踪。除了本机 secrets manager，Harness 还提供了与第三方供应商的集成，如 HashiCorp Vault、Amazon Secrets Manager、Google Secret Manager、AWS Key Management Service、Google Cloud Secret Manager、CyberArk 和 Azure Key Vault。

完整的 Harness 软件交付平台具有持续交付、持续集成、功能标志、持续验证和[云成本管理](https://harness.io/platform/cloud-cost-management/)模块，允许您按需构建、测试、部署和验证。Harness 还通过收购无人机对开源社区进行了大量投资，你可以[今天](https://docs.drone.io/)下载。最后，Harness 现在完全支持 GCP、微软 Azure 和 AWS——在我们最近的[公告](https://devops.com/harness-empowers-developers-with-cloud-agnostic-end-to-end-software-delivery-platform/)中了解更多关于我们对厂商不可知的承诺。

最后，Harness 不是开源的，但是它提供了不同层次的计划来满足您的需求。

## 我们不推荐的替代品

和往常一样，你会读到一些产品，坦白地说，它们已经过了黄金期，我们不推荐。

### 詹金斯

Jenkins 是最著名的开源、自我管理的自动化工具，用于持续集成和持续部署。因为它是开源的，所有人都可以免费使用，并且有一个庞大的社区，这导致了额外的支持、文档和功能。由于这个社区，Jenkins 有超过 1800 个插件可用，这增加了工具的灵活性。

谈到安装要求，Jenkins 非常简单。这个 CI 工具是一个独立的 Java 程序，与平台无关。它适用于大多数主流操作系统，如 Linux、Windows 和 Mac，也可以作为普通安装程序和 war 文件使用。

我们通常将 Jenkins 放在这个列表中，因为它已经有十年的历史了，不是云原生的，并且坦率地说，它过于依赖插件来扩展它的功能。首先，插件作者经常放弃他们的插件(让你去寻找新的插件或者自己写)，插件会产生依赖链，插件会引入安全漏洞，等等。我们不愿意推荐一个如此依赖于额外扩展性的产品。它还被设计成一个持续集成工具，DevOps 工程师/软件工程师必须依靠脚本来将其扩展到持续部署中。

### 大三角帆

Spinnaker 最初由网飞创建，是一个 CD 平台，只是有太多的问题需要我们推荐。我们在我们的 [Spinnaker Alternatives](https://harness.io/blog/continuous-delivery/spinnaker-alternatives/) 帖子中讨论了它们，但是概括一下:它只在内部部署，缺乏本地机密管理，不提供传统的应用程序支持，并且被称为设置和配置的“噩梦”。对于一个不提供原生 CI，只提供 CD 的平台来说，这似乎不值得付出努力。

## 从 Codefresh 迁移到 Harness

是时候改变了吗？您是否正在寻找简单、可扩展且智能的软件交付解决方案？Harness' [软件交付平台](https://harness.io/platform/)包含用于持续集成、持续交付和[云成本管理](https://harness.io/platform/cloud-cost-management/)的模块，即将推出功能标志和持续验证！为什么[不安排一个带 Harness 的演示](https://harness.io/platform-demo-request-lp)，看看我们是否适合您的开发团队和工程师？