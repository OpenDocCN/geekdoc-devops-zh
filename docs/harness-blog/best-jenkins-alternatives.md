# 开发人员和开发人员团队的最佳 Jenkins 替代方案| Harness

> 原文：<https://www.harness.io/blog/best-jenkins-alternatives>

你在努力寻找詹金斯的替代品吗？我们提供了一个最佳 Jenkins 备选方案的列表，它将改进软件交付。

有两个潜在的原因，你点击了一篇名为詹金斯替代品的文章。也许你有一个破碎的软件交付过程，每天需要几个小时的 Jenkins 维护，或者也许你刚刚开始在你的公司建立第一个 CI/CD 管道，你听说 Jenkins 是一个噩梦。不管怎样，你来对地方了。让我们为詹金斯找一个对你有用的替代品。

## **工程师为什么离开詹金斯**

十年前，Jenkins 彻底改变了公司处理软件部署的方式。但是十年后， [Jenkins 变老了](https://itnext.io/jenkins-is-getting-old-2c98b3422f79) -它没有为云原生工具堆栈提供最佳解决方案。管理 Jenkins 就像管理整个应用程序一样。必须有人创建和维护詹金斯管道。必须有人来维护和支付公司的詹金斯服务器。有人需要一个詹金斯博士来解决失败的部署。天哪，难怪你在寻找詹金斯的替代品！这里有一些其他人也这样做的例子。

### **Jenkins 是一个笨重的持续集成解决方案**

这里有一个真实的例子来说明为什么寻找詹金斯替代品是正确的事情。[融水用詹金斯](https://harness.io/customers/case-studies/meltwater-scaled-pipelines-with-harness/)进行持续整合。Jenkins 缺乏自动缩放等关键功能，迫使开发人员等待完成构建。向该公司的 Jenkins 管道添加自动扩展似乎是不可能的，因为所有的依赖项都需要同时更新。关键功能不断被推迟，开发人员开始对 Jenkins 失去更多信心。

“更新詹金斯是一件如此麻烦的事情，以至于你最终会几个月甚至几年都不去更新。”- Jim Sheldon，Meltwater 首席软件工程师

Jenkins pipeline 的采用率下降到了 15%,开发人员开始主动寻找更好的解决方案。

“团队正以最快的速度逃离詹金斯”——Jim Sheldon，Meltwater 首席软件工程师

### **詹金斯不是为连续交货而生的**

Beachbody 使用 Jenkins 进行连续交付，但是 Jenkins 在他们的部署过程中制造了瓶颈。每个生产部署需要六名开发人员花两个小时来完成。这一巨大的承诺意味着每隔一周就要进行部署。Jenkins 需要大量的基础设施来运行，并且需要一个专门的工程师团队来升级和排除管道故障。

“我们的詹金斯管道是手动的，脆弱且管理成本高。”——比奇博迪 DevOps 高级总监陈方灿

[当 Beachbody 试图迁移到 Kubernetes](https://harness.io/customers/case-studies/aws-cloud-migration/) 时，整个 DevOps 团队花了 3 天才将一项服务部署到 Jenkins。整个项目估计需要一整年。

## **詹金斯 CI 替代品**

持续集成是构建和测试工件的过程。CI 工具与源代码库集成，如 [GitHub](https://github.com/) (或 GitHub Enterprise)和 [Bitbucket](https://bitbucket.org/) 来构建可部署的工件。当评估持续集成 Jenkins 替代方案时，您需要考虑几个因素。首先，您需要考虑解决方案的可伸缩性。创建新管道容易吗？在内部需求高的时候，可以使用自动缩放器来增加实例吗？其次，您需要考虑托管解决方案的成本。您需要建立多少基础架构来内部管理您的 CI 实例？第三，你要考虑工程维护工作。有多少工程师将致力于更新和排除 CI 管道故障？

### **1。线束 CI**

设计用于在几分钟内下载、安装和运行， [Harness CI 简单而强大](https://harness.io/products/continuous-integration/)。下载后，Harness CI 持续集成服务器将可以与任何源代码控制系统、任何平台和任何语言一起工作。该产品的直观特性允许在短的 YAML 文件中创建管道，而不是数百行脚本。每个管道步骤都在运行时自动下载的独立 Docker 容器中执行。由于内置的自动缩放器，这些简单的管道可以处理任何扔给它们的东西。该工具的简单性也使得添加新插件变得容易，几乎不需要任何维护工作。与 Jenkins 的 2GB 内存和 Gitlab 的 8GB 内存相比，Harness CI 占用的内存不到 100MB。所有这些都转化成了一个极易维护的高度可扩展的工具。

### **2 .循环〔t1〕**

CircleCI 专为速度和可扩展性而打造。CircleCi 中的一切都是用版本控制系统构建的，因此几乎不需要维护。CircleCI 还通过帮助用户调试潜在的管道问题来减少故障排除工作。“Orb”系统允许第三方集成和插件设置没有任何麻烦。团队可以最大限度地提高构建管道的效率，并通过可定制的 RAM 和 CPU 提高可伸缩性。这些特性，加上 CircleCI 易于共享的配置，创建了一个标准化的 CI 解决方案，组织中的任何人都可以使用。

### **3。特拉维斯 CI**

[Travis CI](https://travis-ci.org/) 是首个 CI 即服务解决方案，专为开源项目而设计。Travis CI 是一种简化的体验，易于设置和使用。配置是使用 YAML 文件创建的，Travis CI 允许用户对他们的构建执行各种各样的测试。Travis CI 缺乏其他 CI 工具的许多企业级功能(考虑安全性和治理)，但这反过来意味着 Travis CI 几乎不需要维护。

### **詹金斯没有比詹金斯更好的选择**

警惕与 Jenkins 同时发布的其他 CI 工具。像[TeamCity](https://www.jetbrains.com/teamcity/)(JetBrains)和 [Bamboo](https://www.atlassian.com/software/bamboo) 这样的工具仍然需要手动配置才能正常工作。你必须为每一个[松弛](https://slack.com/)、 [Git](https://github.com/) 、亚特兰大[吉拉](https://www.atlassian.com/software/jira)等等编写定制脚本。整合。

## **詹金斯光盘替代品**

连续交付是将工件部署到生产中的过程。此流程包括问答、内部批准、非生产部署、生产部署、验证步骤和回滚。真正的 CD 解决方案通过关注这些流程步骤来描述 CI 工具。Jenkins 经常使用自定义脚本来执行 CD 功能。事实上，几年前，这是你唯一的选择。不再是了。现代 CD 解决方案让用户无需定制脚本即可创建管道，并轻松连接到现代工具堆栈。在选择 Jenkins CD 替代品时，您需要注意功能的对等性和易用性，因为困难的工具将很难获得内部采用。

### **1。线束 CD**

[Harness Continuous Delivery](https://harness.io/products/continuous-delivery/)是一款 SaaS 解决方案，提供先进的开箱即用 CD 功能。Harness CD 通过零定制脚本自动创建部署管道。除了标准的 canary 部署，Harness 还提供了高级治理功能，如 RBAC、审计跟踪和 SSO。一旦部署完成，Harness 的持续验证将使用机器学习来检测任何部署异常，并在出现问题时自动回滚。Harness 有一个易于理解的用户界面，但用户也可以选择使用 GitOps 来运行一切。首次使用 Harness 的用户可以在半天内设置和部署他们的第一个管道。

### **2。阿尔戈 CD**

[Argo CD](https://argoproj.github.io/argo-cd/) 使用 GitOps 技术自动化 Kubernetes 部署。Argo CD 支持多种配置管理工具、SSO 集成和 web hook 集成，为开发人员提供了一种简单而有效的部署方法。Argo 通过使应用程序定义、配置和环境具有声明性和版本控制性，减少了管理工作量。Argo 的既定目标是使应用程序的部署自动化、可审计且易于理解。

### 3 .GitLab

[GitLab](https://about.gitlab.com/) 让发布在整个 DevOps 生命周期中变得“无聊”。GitLab CI 直接与 GitLab CD 连接，没有太多空间混搭工具。这个紧密缠绕的包通过简化审查、准备和生产部署管道降低了维护成本。他们的方法通常导致更快的发布周期，这反过来降低了每次部署的规模和复杂性。如果一家公司想要完全标准化其 DevOps 工具链，那么 GitLab 提供了一站式解决方案。

### **不比詹金斯好的詹金斯替代品**

当心像 [Spinnaker](https://spinnaker.io/) (网飞)和 [GoCD](https://www.gocd.org/) 这样的工具。这两个开源平台都需要和 Jenkins 一样多的手动设置和维护。请记住:目标是减少您必须在内部完成的管理工作。

## **使用线束扩展到詹金斯管道之外**

Jenkins 一度是市场上最先进的 CI/CD 工具。但是詹金斯在上个十年开始时就被释放了。从那以后，云的采用增加了，软件开发生命周期也发生了巨大的变化。詹金斯没有跟上过去十年的创新。如果您要使用 Jenkins 替代方案，请找到一个不需要自己的开发团队的现代解决方案。让其他人来构建和管理[最佳 CI/CD 工具](https://harness.io/free-trial/) -今天就开始免费试用 Harness 吧。

如果你还没有准备好迈出这一步，请继续阅读我们的 [CI/CD 购买指南](https://harness.io/learn/ebooks/buyers-guide-for-ci-cd-ebook/)，这是一本免费的电子书，比较了大量 CI、CD 和 CI/CD 供应商。