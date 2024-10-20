# 在 GitOps Repos | Harness 中构建代码的方法

> 原文：<https://www.harness.io/blog/gitops-repo-structure>

当通过 GitOps 最佳实践进行管理时，声明性的、不可变的和持续协调的基础设施带来许多好处。以下是管理这些管道中使用的代码的四种方法。

实施 [GitOps](https://harness.io/blog/devops/gitops/) 实践将把你的软件交付管道带到下一个层次。[声明性的](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/declarative-config/)、[不可变的](https://www.cncf.io/online-programs/immutable-infrastructure-in-the-age-of-kubernetes/)，以及持续[调和的](https://github.com/open-gitops/documents/blob/v1.0.0/GLOSSARY.md#reconciliation)基础设施在通过 [GitOps 最佳实践](https://harness.io/blog/devops/gitops-best-practices/)进行管理时，会带来许多[好处](https://harness.io/blog/devops/gitops-benefits/)。多年来，我已经帮助许多开发团队构建和改进了他们的 GitOps 工作流。在这篇博客中，我将分享管理这些管道中使用的代码的四种方法。

“GitOps”的“Ops”部分指的是配置代码，或者将[基础设施称为代码](https://harness.io/blog/devops/infrastructure-as-code/) (IaC)。软件依赖于由此代码管理的资源来运行。在 [Git](https://git-scm.com/) 仓库中管理这种配置有很多好处。通常这些代码的结构是事后才想到的，这会导致将来的重大重构。

## 一个存储库中的应用程序和基础架构代码

第一个示例在同一个存储库中管理应用程序代码和基础结构代码。单个[长寿](https://medium.com/@peter.hozak/long-lived-git-branches-survival-guide-f3b1028d21d)分支存在([主](https://www.theserverside.com/feature/Why-GitHub-renamed-its-master-branch-to-main))。

### 例子

下面是一个 Node.js 项目，应用程序代码在根目录下，YAML 文件在 kubernetes 目录下。对 development.yaml 的更改适用于开发环境，对 production.yaml 的更改适用于生产环境。

### 利益

*   同一个存储库中的基础设施代码和应用程序代码将所有内容的版本保持在一起。不需要将多个存储库之间的点连接起来，以在某个时间点再现应用程序和配置的状态。
*   对于开发者来说，一个存储库意味着更少的上下文切换。开发人员在对基础设施代码进行更改时，不需要更改存储库。

### 缺点

*   没有[特权分离](https://harness.io/blog/devops/pipeline-security-devsecops-catalyst/)。能够访问存储库的开发人员将能够更改应用程序和基础设施代码。

一些组织要求应用程序和基础设施代码分离。下面的例子都在它们自己的存储库中管理应用程序和基础设施代码。这改进了权限分离，因为每个 Git 存储库都可以设置自己的用户权限。

## 独立的基础架构存储库，多个分支机构

您可能熟悉 Git 分支工作流，如 [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) 。最近，随着基于主干的开发越来越流行，git flow[已经失宠](https://georgestocker.com/2020/03/04/please-stop-recommending-git-flow/)。有充分的理由避免在您的 Git 存储库中有多个长期存在的分支。然而，在某些情况下，多个长寿命分支仍然值得考虑。

### 例子

下面是一个[掌舵](https://helm.sh/)图表库，有两个长期的分支，开发和生产。变更总是起源于开发分支。推广到生产需要[将](https://git-scm.com/docs/git-merge)开发融合到生产中。开发环境使用开发分支中的 development-values . YAML[values](https://helm.sh/docs/chart_template_guide/values_files/)文件。生产环境使用生产分支中的 production-values.yaml 值文件。

### 利益

*   在促进环境之间的变化时，[配置漂移](https://harness.io/blog/devops/infrastructure-as-code/#consistency)的风险较低。合并分支可以确保不会遗漏任何更改。
*   改进了开发和生产变更之间的权限分离。比如 GitHub 支持分支[保护规则](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/defining-the-mergeability-of-pull-requests/managing-a-branch-protection-rule)。存储库的所有者可以控制哪些用户可以提交到分支。

### 缺点

*   对于您的基础设施代码来说，这是一条“单车道”。开发分支中的变更可以阻止生产变更(没有[精选](https://git-scm.com/docs/git-cherry-pick)期望的变更)。

## 基于目录的独立基础架构存储库

现在，让我们考虑一个只有一个长期分支(main)的存储库。每个环境都有自己的目录。

### 例子

下面是一个 [Terraform](https://harness.io/blog/devops/terraform-201-tutorial/) 存储库，有单独的开发和生产目录。对开发的更改使用开发目录中的 development . TF vars[TF vars](https://www.terraform.io/language/values/variables#variable-definitions-tfvars-files)文件。对生产的更改使用生产目录中的 production.tfvars tfvars 文件。

### 利益

*   在开发目录中所做的更改不会影响生产目录。

### 缺点

*   环境之间配置偏差的风险增加。理解目录之间的差异对开发人员来说是一个沉重的负担。
*   没有特权分离。用户可以对开发和生产环境进行更改。

## 多个基础架构存储库，每个环境一个

让我们考虑一种方法，其中每个[环境](https://harness.io/blog/continuous-delivery/deployment-environments/)都有自己专用的存储库。每个存储库都有一个单独的长期分支(main)。

### 例子

这是一个 Terraform 项目的例子，其中开发和生产是分开的。对开发的更改使用开发存储库中的 development.tfvars tfvars 文件。对生产的更改使用生产存储库中的 production.tfvars tfvars 文件。

### 利益

*   最高级别的特权分离。您的 Git 主机在存储库级别提供的关于用户/组访问的任何特性都将对您可用。只有需要对生产进行更改的用户才能将更改提交到生产存储库。
*   更容易建立新环境或迁移现有环境。创建新环境时，创建一个新的存储库。不需要与现有的存储库集成来构建新的环境。

### 缺点

*   环境之间配置偏差的风险更高。理解存储库之间的差异对开发人员来说是一个沉重的负担。

## 结论

根据我的经验，每个环境一个存储库是管理 GitOps 代码的最[经得起未来考验的](https://en.wikipedia.org/wiki/Future-proof)方法。特权和环境分离的好处超过了潜在的缺点。如果您决定将来不需要分离，您可以将多个存储库合并为一个。好消息是，无论您选择哪种方法， **Harness 的产品套件都支持它们。**

无论您是使用 [Harness CI](https://harness.io/products/continuous-integration) 构建、测试和发布工件，使用 [Harness CD](https://harness.io/products/continuous-delivery) 进行部署，还是使用 [Harness GitOps](https://harness.io/gitops-beta/) (目前处于测试阶段)将您的管道提升到下一个级别，我们都会为您提供帮助！此外，每个线束管道都可以利用围绕[治理](https://harness.io/blog/continuous-delivery/ci-cd-pipeline-governance/)、[混沌工程](https://harness.io/blog/product-updates/introducing-harness-chaos-engineering/)等的高级特性。

来看看 Harness 如何帮助加速您的 GitOps 之旅。注册一个 [14 天的免费试用](https://harness.io/pricing)并按照我们的 [Kubernetes CD 快速入门](https://ngdocs.harness.io/article/knunou9j30-kubernetes-cd-quickstart)指南将应用程序部署到您的集群。如果你想找一个有导游的旅程，请[预订一个演示](https://harness.io/demo/)。

我们很乐意回答您在我们的[论坛](https://community.harness.io/)、[社区 Slack](https://harnesscommunity.slack.com/) 或即将到来的 [Harness &无人机用户组](https://www.meetup.com/harness/)虚拟聚会上的任何问题。