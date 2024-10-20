# 使用 Snyk 和 Harness | Harness 查找并修复管道中的漏洞

> 原文：<https://www.harness.io/blog/snyk-harness-vulnerabilities>

创建您的 Snyk 和线束帐户并开始！跟随我们的逐步教程。

当 DevOps 在 10 多年前出现时，主要焦点是弥合开发和运营团队之间的差距。这是通过在设计、构建、测试和部署应用程序的过程中引入自动化来实现的。

随着开发团队继续更快更频繁地交付产品，安全团队发现很难跟上。通常，它们会成为交付渠道中的瓶颈。由于这个原因，在 DevOps 过程中尽早引入安全性并接受 DevSecOps 文化变得越来越重要。

将 Snyk 以开发人员为中心的安全平台集成到 Harness 的统一交付管道工作流中，确保安全性和合规性测试是每个版本的一部分。这允许您防止具有易受攻击的依赖项和代码的应用程序进入生产环境。使用像 Snyk 和 Harness 这样的现代工具，您可以通过 CI/CD 管道找到、修复和补救，并减轻业务风险，而不会影响您快速发布软件的能力。

## Snyk 入门

Snyk 是一个开发者安全平台，它使团队能够轻松地发现、优先处理和修复代码、依赖项、容器和作为代码的[基础设施中的安全漏洞。它直接集成到开发工具、工作流和自动化管道中。](https://harness.io/blog/infrastructure-as-code/)

在业界领先的应用程序和安全智能的支持下，Snyk 将安全专业知识放入任何开发人员的工具包中。

开始使用 Snyk 非常简单，只需访问 [snyk.io](http://snyk.io/signup) 并注册一个免费帐户。

## 线束入门

Harness 是业界第一个使用 AI 来简化您的 DevOps 流程的软件交付平台——CI、CD、功能标志、云成本管理等等。

Harness 使您能够使用可重用的模板在几分钟内构建复杂的部署，从而节省时间和压力。使用 Harness，您可以在几分钟内建立管道。

开始使用 Harness 非常简单，只需前往 [harness.io](https://app.harness.io/auth/#/signup) 并设置一个免费试用帐户。

## 防止漏洞通过构建和部署过程

演示 Snyk 和 Harness 组合功能的最佳方式是通过一个演练，欢迎您自己设置和运行。在本例中，我们使用 Snyk 来演示如何通过将自动化 Snyk 测试添加到线束工作流中来防止漏洞通过构建和部署过程。

我们使用下面的 GitHub 库:[https://github.com/mansong1/springbootemployee-api](https://github.com/mansong1/springbootemployee-api)

如果你想继续下去，你将需要以下:

1.  [**Snyk 账号**](http://snyk.io/signup)
2.  [**驾驭账号**](https://app.harness.io/auth/#/signup)

完成以下步骤后，本教程最有意义:

如果您已经完成/复习了上述内容，那么您已经学会了如何设置一个线束 CI 工作流。

## 在线运行 Snyk 测试的七个步骤

### 步骤 1:创建 Snyk API 令牌机密

您需要一个 Snyk API 令牌来执行 Snyk CLI 测试。对于企业客户，可以通过 Snyk App 设置[服务账号](https://docs.snyk.io/features/integrations/managing-integrations/service-accounts)并创建令牌。对于非企业 Snyk 帐户，包括我们的免费 Snyk 层，您可以使用主[用户令牌](https://docs.snyk.io/features/snyk-cli/install-the-snyk-cli/authenticate-the-cli-with-your-account)。

接下来，按照这些步骤，在 Harness 中创建一个 secret 来存储 Snyk API 令牌。

### 步骤 2:创建新的管道

1.  首先，点击 **+Project** ，在 Harness 中创建一个新项目

2.  一旦你创建了你的项目，转到**持续集成**模块来创建一个新的管道。

### 步骤 3:添加代码库

如果这是管道中的第一个 CI 阶段，请在 CI 阶段设置中启用克隆基本代码。如果您有一个包含 CI 阶段的现有管道，请单击“代码库”。参见[编辑代码库配置](https://ngdocs.harness.io/article/ota4xj59le-run-a-script-in-a-ci-stage)。

### 步骤 4:定义构建场基础结构

在 CI Stage 基础结构中，定义代码库的构建场。

参见 [Kubernetes 集群构建基础设施设置](https://ngdocs.harness.io/article/ia5dwx5ya8-set-up-a-kubernetes-cluster-build-infrastructure)。

### 步骤 5:配置运行 Snyk 测试

在 CI 执行中，单击添加步骤，然后单击运行。

运行步骤在容器映像上执行一个或多个命令。

Snyk 提供了多个容器图像，这些图像包装了 Snyk CLI，并带有针对不同语言的相关工具。在这个例子中，我们使用的是 Maven 版本，因为在这个示例应用程序中使用的就是它。

有关配置运行步骤的步骤设置，请参见[运行步骤设置](https://ngdocs.harness.io/article/1i1ttvftm4-run-step-settings)并添加您的 Snyk API 令牌。

现在，您可以在运行步骤配置的命令部分引用该环境变量。

### 步骤 6:运行管道

现在，您可以运行您的管道了。

1.  单击保存并发布。
2.  单击运行。将出现管道输入设置。
3.  在 CI 代码库中，单击 Git 分支。
4.  在 Git 分支中，输入代码库所在分支的名称，例如 master。

5.  单击运行管道。

### 步骤 7:查看管道执行

当工作流运行时，Snyk **测试**步骤失败，因为项目包含易受攻击的依赖项。我们也可以点击**控制台视图**切换来查看完整的报告。

注意:为了更好地控制您的测试，您可以将 severity-threshold 标志传递给 Snyk test 命令，其中包含一个受支持的选项(低|中|高|关键)。使用此标志，将只报告所提供级别或更高级别的漏洞

整个管道可作为 **YAML** 也。

在构建中，单击更多选项( **︙** )并选择编辑管道。

点击 **YAML** 。

你可以看到整个管道作为 YAML(见下面截图)。您可以编辑管道中的任何内容并再次运行它。

管道:
阶段:
-阶段:
类型:CI
规格:
基础设施:
类型:kubernetesidirect
规格:
connectorRef: org。GKE
名称空间:线束构建
执行:
步骤:
-步骤:
类型:运行
规格:
连接器引用:组织。docker hub
image:SNYK/SNYK-CLI:1 . 745 . 0-maven-3 . 5 . 4
命令:|-
snyk 配置集 api=$SNYK_TOKEN
snyk 测试
privileged:true
env variables:
SNYK _ TOKEN:<+secrets . getvalue(" org。SNYK_TOKEN") >
资源:
限制:
内存:1Gi
cpu: "1.0"
名称:SNYK _ Test
clone codebase:true
名称:CI
标识符:CI
属性:
CI:
code base:
repoName:spring boot employee-API
connector:org。github
build:<+input>
project identifier:Snyk
or identifier:default
name:Snyk Test
identifier:Snyk _ Test
描述:测试本地项目漏洞。

## CI/CD 工作流程

除了扫描应用程序的依赖关系，漏洞还会根据所用容器的选择而潜入。除此之外，云资源的错误配置也是用来访问云数据和服务的最常见的云漏洞。

在本例中，我们将在 Kubernetes 上构建和部署我们的应用程序，然后使用 Snyk 对其进行监控。

要构建和推动一个容器，只需遵循这些[步骤](https://ngdocs.harness.io/article/8l31vtr4hi-build-and-upload-an-artifact)。添加 Snyk 容器扫描步骤类似于上面的 Snyk 测试步骤。

要将容器部署到我们的 Kubernetes 集群，请参见这里的。在我们的 CD 管道中，我们将在此之前执行 Kubernetes 清单的 Snyk 测试。添加 Snyk IaC 测试步骤是通过 [Shell 脚本](https://ngdocs.harness.io/article/k5lu0u6i1i-using-shell-scripts)步骤完成的。

在 Snyk IaC 步骤中执行的脚本如下:

RM-RF springboot employee-API | | true

git 克隆 https://github.com/mansong1/springbootemployee-api
CD springboot employee-API

SNYK _ TOKEN =<+secrets . getvalue(" org。SNYK_TOKEN") >
snyk 配置集 api=$SNYK_TOKEN
snyk iac 测试清单/<+infra . namespace>/templates/*。yaml

完整的 CI/CD 工作流如下所示:

和以前一样，这条管道可以看作是 YAML，位于这个 [GitHub repo](https://github.com/mansong1/springbootemployee-api/blob/master/harness/.harness/SpringBoot_Employee_API.yaml) 中。

Snyk 与 Kubernetes 集成，使您能够导入和测试正在运行的工作负载，并识别它们的相关映像和配置中的漏洞。导入后，Snyk 会继续监控这些工作负载，随着新映像的部署和工作负载配置的变化，识别其他安全问题。

通过我们的 Harness CD 工作流程进行的成功部署显示在 Snyk 应用程序上，如下所示:

## 现在试试吧

在选择技术平台时，重要的是选择一个将开发者的需求放在解决方案中心的平台。它还应该侧重于补救问题，而不仅仅是报告问题。采用开发人员优先方法的平台可以集成整个管道的安全性，帮助多个不同的利益相关方(如开发人员、sec 和运营团队)获得一个整体视图。这有助于植入一种相互开发合作的心态，并确保消费者手中的软件更加安全。

非常感谢你加入我们的教程。别忘了今天试试 [Snyk](https://app.snyk.io/login) 和[线束](https://app.harness.io/auth/#/signup)！

*本文由帕斯·阿皮塞拉(Snyk)和马丁·安松(Harness)合作撰写。*

Martin 是 Harness 商业销售组织的解决方案工程师，致力于帮助客户加快软件生命周期的构建和交付。在 Harness 之前，Martin 担任过各种工程职位，带来了云计算、自动化工具、DevOps 实践等方面的丰富知识。在业余时间，他喜欢航海、素描和杂耍。

*Pas Apicella 是 Snyk 的首席解决方案工程师 APJ，在 Snyk 平台上工作，帮助客户保护他们的云原生应用程序，并在满足业务需求的同时实现 DevSecOps 文化。他为 OSS 项目贡献了代码，并在时间允许的情况下不断地撰写关于云原生架构的博客。他毕业于墨尔本理工大学，获得了计算机科学学士学位，此后曾在 Snyk、Elastic、Pivotal、VMware、Oracle 和 IBM 等公司担任过各种职务。*