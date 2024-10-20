# 缺陷——创新和持续交付管道中不可避免的问题

> 原文：<https://www.harness.io/blog/bugs-inevitable-in-innovation-and-your-continuous-delivery-pipeline>

很明显，这不是一个 bug，而是一个特性。

很明显，这不是一个 bug，而是一个特性。这是我在软件开发中最喜欢的一句话，我需要偶尔说这句话来取笑我遇到的错误。随着开发和运营团队之间的界限变得更加模糊，传统的基础设施团队可能开始看起来像软件工程团队，反之亦然。

创新工作的核心是尝试以前没有做过的新方法。作为人类，我们会犯错，因此当我们创造系统时，我们的系统也会犯错。尽管传统的基础设施团队与 bug 的关系可能更多的是工作请求，而传统的开发团队中 bug 是应该被纠正的错误。

资源是有限的，在某些时候，我们必须将我们的软件交付给我们的内部或外部客户使用。在软件行业，只要有足够的资源和时间，任何问题都是可以解决的，尽管我们总是受到这两点的限制。在我们创作的作品中，bug 是不可避免的。

### 什么是 Bug？

软件 bug 的字面定义是计算机程序中的错误或缺陷，导致它以意想不到的方式运行。从开发人员的角度来看，另一个笑话可能是“错误在键盘和椅子之间”，意思是用户。虫子可以有很多名字；[问题](https://support.atlassian.com/jira-software-cloud/docs/what-is-an-issue/)或缺陷是 bug 的常见名称。

bug 随时可能出现。我们在 SDLC 的[测试和验证阶段](https://harness.io/blog/testing-methodologies-for-cd-pipelines/)所做的大量工作就是找出并验证问题的存在。我们测试的是特性是否符合预期。期望应该与一个需求，一个 [SLA](https://en.wikipedia.org/wiki/Service-level_agreement) ，或者一个合理的功能期望相关联。错误总是可以预料到的，但是当你开始遇到太多的错误，或者它们开始以更快的速度出现时，质量就成了问题。

### 软件质量的通用度量

看一个软件项目的生命力，无论是商业的还是开源的，质量的第一个度量是缺陷的数量。对于一个没有上下文的普通人来说，缺陷的数量越多，项目的质量和活力就越成问题。尽管关闭这些缺陷的能力同等重要或更重要，例如缺陷关闭率。

在开源项目中，大多数项目允许几乎任何人制造问题。例如，在写这篇博文的时候， [Spinnaker 项目](https://github.com/spinnaker/spinnaker/issues)在整个平台上有超过 350 个公开的问题。为了确保提交问题不是有人在寻求帮助，大多数开源项目和商业供应商会询问资格问题，并帮助重现该问题。虽然取决于影响，但并不是所有的错误都是一样的。

### 并非所有的缺陷都是一样的

十多年前，当我第二次在一家大型软件公司实习时，我震惊地发现，我们并没有在发布软件之前修复 100%的错误。我们正在将正在构建的应用服务器从版本 5.x 升级到版本 6.x，我们已经知道第一个服务包中会有什么，我们甚至还没有发布主要版本。我的玫瑰色眼镜裂了。

这个概念与 bug 优先级有关；所有的缺陷都是不一样的。如果发现任何 bug，我们应该停止构建或发布；关键问题必须解决，或者可以通过不同的方法或教育来推迟/解决错误。优先级是至关重要的，你几乎肯定没有带宽来验证和修复每一个 bug。

bug 来自何时何地的优先级因素。是影响更多软件的较大模块上的错误，还是尚未被捕获的特定边缘情况。严重性也影响优先级；该错误是否严重到足以导致您的客户停工，或者是否可以轻松提供解决方法。由于资源是有限的，[排名问题](https://support.atlassian.com/jira-software-cloud/docs/rank-an-issue/)成为开发团队日常工作的一部分。幸运的是，主动质量和缺陷跟踪工具对跟踪缺陷有很大的帮助。

### Bug 跟踪工具

您在开发团队中要做的第一件事就是查看 bug backlog。很可能你会打开[吉拉](https://www.atlassian.com/software/jira)或 [GitHub 问题](https://help.github.com/en/github/managing-your-work-on-github/about-issues)来全面了解你将要处理的 bug 的等级/紧急程度和类型。通常，你对团队的第一个贡献就是帮助减少 bug 积压。

bug 跟踪工具的其他荣誉奖有 [Bugzilla](https://www.bugzilla.org/) 和 [Rational ClearQuest](https://www.ibm.com/us-en/marketplace/rational-clearquest) 。如果不构建一个特性，现代开发团队会将票据/问题编号作为他们构建和部署的一部分，以便能够追溯。根据团队所拥有的机制，您的团队可能会使用一个标签/bug 跟踪系统来获得增强或工作请求。 [ServiceNow](https://www.servicenow.com/) 是一个用于编排工作请求的平台的好例子。随着我们开始接受更多的自动化测试和部署后的持续验证，bug 将成为您的持续交付管道中的决策因素的一部分。

### bug 和你的持续输送管道

现代 DevOps 工具需要与 bug 跟踪工具集成。由于我们工作的速度和灵活性，以人为中心的任务，比如写一份冗长的 bug 报告，可能会显得乏味，特别是在我们期望的广泛的自动化覆盖范围内。

运行您的连续交付管道可能是自动化覆盖被编排的第一次，并且结果将需要被报告或者被用作质量关口。例如，如果需要在 QA 中修复项目，线束平台现在有能力[从您停止的地方恢复流水线](https://developer.harness.io/docs/first-gen/continuous-delivery/concepts-cd/deployments-overview/resume-a-pipeline-deployment/)。这允许继续执行不同的测试用例，例如，当你改变环境时，从特性测试转移到环境测试。

随着错误、回归、问题等开始以更加自动化的格式被捕获，像 [Bugsnag](https://www.bugsnag.com/) 这样的公司正在获得牵引力，自动记录票证的能力开始成为规范。Harness 能够协调几个问题和错误跟踪提供者，作为您持续交付旅程的一部分。

### 驾驭你的伙伴

Harness 平台与几个 bug/标签提供商有很好的集成。随着在管道的执行过程中发现新的发现，Harness 可以帮助记录并根据这些提供者的结果做出决策。如果您使用[吉拉作为记录系统](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/workflows/jira-integration/)，Harness 拥有一流的支持，可以集成为您持续交付渠道的一部分，Harness 可以代表您保持更新。

查看 bug 捕获和报告的新范例，如 [Bugsnag](https://developer.harness.io/docs/first-gen/continuous-delivery/continuous-verification/continuous-verification-overview/concepts-cv/bugsnag-verification-overview/) 可以在 Harness 平台中利用。

Harness 致力于与您的工程组织合作，继续推动您的连续交付管道的质量和可见性。如果您正开始您的旅程，另一个学习软件交付基础知识的极好途径是来自 Harness 的最新资源之一， [Deliver Better](https://www.deliver-better.com/) ，这是我们努力在所有级别教授的概念。如果您正在寻找软件交付自动化之旅的起点或进一步的起点，[今天就注册利用](https://harness.io/try-continuous-delivery-as-a-service-for-free/)！

干杯，

拉维