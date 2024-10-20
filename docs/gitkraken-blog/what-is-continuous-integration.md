# 持续集成入门

> 原文：<https://www.gitkraken.com/blog/what-is-continuous-integration>

*本文由特邀作者 MONCCO PR 首席执行官卡格拉·埃图格鲁尔撰写。*

当扩展您的团队的能力时，不可能过高估计一个强大的开发环境的重要性。

如今，大多数组织都使用 [Git](https://git-scm.com/) ，许多组织使用的流行且成功的开发模型之一是 [Gitflow](/blog/gitflow) 。当利用这种模型时，持续集成(CI)是关键，因为它能够更快地交付项目，并由于自我管理和易于控制的[专门团队](https://www.scnsoft.com/services/dedicated-development-teams)而降低风险和费用。在本文中，我将解释持续集成的好处以及如何实现它们。

## **什么是持续集成？**

[持续集成](https://martinfowler.com/articles/continuousIntegration.html)始于开发人员通过版本控制系统，如 Git，通过共享存储库与代码进行交互和协作。开发人员定期推进他们的工作，一旦变更完成，就会运行一系列自动化操作。

自动化操作可能包括静态代码分析、拼写检查、安全检查、单元测试、集成测试等等。这些操作是为了在工作流程的各个步骤中出现任何失败时快速通知开发人员。

## **持续集成工具**

有各种各样的持续集成工具可用。一些最受欢迎的工具包括:

*   Jenkins :这个 CI 工具管理和控制软件交付过程。让 Jenkins 能够监视您托管的回购上的代码更改，并启动自动化构建。
*   [Azure DevOps:](https://www.gitkraken.com/resources/devops-report-2020#azure-devops-deploy) 微软创建了这套 DevOps 解决方案，通过持续集成自动化流程的每一步，允许开发人员从代码到云。
*   GitLab CI: 该工具提供了从规划到部署的无缝体验，可以在多台机器上拆分构建以快速执行。
*   GitHub Actions:GitHub Actions 非常适合在 GitHub 上托管项目回购的团队，它提供了构建、测试和部署的自动化操作。

## **用于代码审查的静态分析器**

代码质量在任何软件项目中都是至关重要的，但是开发人员通常会走捷径来满足最后期限。包含静态分析器有助于通过避免错误来提高代码质量。

静态分析器可以很容易地集成到开发环境和持续集成过程中。Analysis-Tools.dev 按照编程语言整理了一份流行的静态分析工具列表。

通过启用分析器，开发人员可以更容易地识别和修复错误。这反过来又减轻了最终代码审查者的任务，他们可以专注于审查代码的功能，而不是专注于常见的反模式或代码风格。

**单元测试**

## 单元测试是测试一个功能单元的过程。这是一个自动化的测试过程，在这个过程中，每当代码变更被推送到存储库时，测试用例就会被编写和执行。

单元测试有利于测试逻辑功能或算法，并且可以避免在重构代码时中断更改。这里有一些在编写单元测试时避免致命错误的[技巧。](https://lukaszcoding.com/2020/07/7-fatal-unit-test-mistakes-to-avoid/)

在单元测试中，一个外部依赖被模仿，这有助于在几分钟内运行几千个测试。当您的团队重构现有功能时，正确编写的覆盖更多代码的单元测试有助于降低风险。

In unit tests, an external dependency is mocked which helps run several thousands of tests within minutes.  Properly written unit tests covering more code helps reduce risks when your team is refactoring existing functionalities. 

**集成测试**

## 在[集成测试](https://martinfowler.com/bliki/IntegrationTest.html)中，测试整个功能，而不仅仅是功能单元。在这里，数据库或外部依赖没有被嘲笑。因为没有模仿依赖项和数据库，所以与单元测试相比，集成测试将花费更多的时间来运行。集成测试应该被巧妙的编写以覆盖所有的功能，并且应该有足够的测试来确保系统按预期运行。

添加太多的集成测试可能会产生一个维护噩梦，尤其是如果没有正确编写的话。如果使用单元测试可以确保功能性，那么这应该是集成测试的首选策略。更多的集成测试将需要更多的执行时间，这反过来会减慢部署过程。

例如，集成测试可以在预定的基础上运行，也可以按需运行，而不是对每个拉请求都运行集成测试。集成测试还有助于避免在代码库或团队扩展时破坏现有功能的变化，并可以节省数小时的手动测试。

了解更多关于集成测试的信息，包括一种叫做[基于快照的测试](https://blog.sharetechlinks.com/integration-testing-asp-net-core-web-api-snapshot-based-technique-system-design/)的方法。

Learn more about integration testing, including a method called [snapshot-based testing](https://blog.sharetechlinks.com/integration-testing-asp-net-core-web-api-snapshot-based-technique-system-design/). 

**拉请求&持续集成**

## [拉请求](https://docs.github.com/en/github/collaborating-with-issues-and-pull-requests/about-pull-requests)创造了一个健康讨论代码的机会。不管你的团队是大是小，都应该鼓励拉请求。

当一个开发人员提交一个拉请求时，他们是在请求一个同事，或者可能是经理，在他们被接受为项目的最终版本之前审查和批准代码变更。代码审查有助于提高质量和避免常见错误。

***在这个中级 Git 教程视频中，增加你关于拉请求的知识。[学习 Git:什么是拉请求？](https://www.gitkraken.com/resources/video-pull-request)***

虽然对于持续集成来说不是强制性的，但是拉请求启用了一个人工检查点，这是一个无法通过设计实现自动化的好处。

While not mandatory for continuous integration, pull requests enable a human checkpoint, a benefit that can’t be automated by design. 

**功能切换为更快释放**

## [特性切换](https://martinfowler.com/articles/feature-toggles.html)，也称为特性标志，有助于以更快的速度交付特性。通过这种技术，可以使用配置来启用或禁用某个功能，这有时就像真/假条件检查一样简单。当一个特性完成时，只需将该特性的配置值设为 true，就可以在产品中启用该特性。这在处理扩展到多个版本的大型功能时尤其有用。

在 Git 中，一个常见的工作流程是在一个特性完全完成后，将一个特性分支从开发合并到生产中。虽然这个过程对于小特性来说很棒，但是对于可能需要一个月或更长时间来实现的大特性来说，它可能会造成复杂性。在这种情况下，与开发分支相比，本地分支将变得过时，很可能导致大量的代码冲突。特性切换是解决这个问题的一个很好的方法。

这种技术也可以用于部分地推送到远程存储库，但是也可以合并和发布。在发布期间，尚未完成的特性可以被“关闭”，而其他特性则被发布。当一个特性被完全测试并准备就绪时，它就可以被“打开”了

最后，开发人员可以使用这种技术为特定的用户群打开一个特性来进行 A/B 测试。

Lastly, developers can use this technique to turn on a feature for a certain group of users for A/B testing. 

**利用 CI 优化您的工作流程**

## 如果配备了正确的工具，持续集成可能比您想象的更容易实现。当谈到软件开发时，自动化是未来的发展方向，现在未能采用这些策略的团队将被落在后面。

If equipped with the right tools, continuous integration can be easier to implement than you might expect. Automation is the way of the future when it comes to software development, and teams who fail to adopt these strategies now will be left in the dust.