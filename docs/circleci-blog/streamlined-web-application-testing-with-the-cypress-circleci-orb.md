# 用 Cypress | CircleCI 测试 Web 应用程序

> 原文：<https://circleci.com/blog/streamlined-web-application-testing-with-the-cypress-circleci-orb/>

我们公司 Cypress.io 开发了一个开源的、麻省理工学院许可的、免费的端到端测试运行程序，可以测试任何在浏览器中运行的东西。test runner 使有效地测试复杂的现代 web 应用程序变得容易，而且它安装简单，易于学习，并且运行良好。

## 问题是

Cypress 测试运行器可以用于在开发人员的机器上本地运行测试，或者在持续集成服务器上运行测试。我们坚信每一个提交都应该被测试，因此我们记录了[如何设置 CI 测试](https://on.cypress.io/circleci-orb)，甚至在大量 CI 提供者上运行我们自己的示例项目。但是设置 CI 套件从来没有想象中那么容易。当然，开发人员可以从我们的示例中复制代码并粘贴到他们的配置中，但是他们通常必须运行项目并调整设置，直到项目的构建和测试通过。我们的团队意识到，整个过程比想象的要困难得多，而且常常令人沮丧。我们，这个工具的作者，没有办法帮助开发人员配置他们的 CI 套件。

## 球体

也就是说，直到我们看到 CircleCI Orb 提案和示例。哇——现在 Cypress 团队可以制作小的可重用 CI 配置，并与我们的用户共享它们！下面是我们在 CircleCI 上运行端到端测试的用户可以使用的最简单的例子:

```
version: 2.1
orbs:
  cypress: cypress-io/cypress@1.0.0
workflows:
  build:
    jobs:
      - cypress/run 
```

七行，十个单词:它正确地检查代码，安装 npm 依赖项以及 Cypress 应用程序，缓存需要缓存的所有内容，并运行所有端到端测试。美丽而优雅。

对于想要构建应用程序并在 Cypress 仪表板上记录测试结果和工件的用户来说，这里有一个更复杂的例子。不需要编写任何定制代码——因为`cypress/run`作业接受这些常见用例的参数:

```
version: 2.1
orbs:
  cypress: cypress-io/cypress@1.0.0
workflows:
  build:
    jobs:
      - cypress/run:
           build: "npm run build"
           record: true 
```

参数被静态检查，并且至少防止一些配置错误。即使是我们的用户通常需要的复杂工作流，如“在一台机器上安装和构建应用，然后在 10 台机器上运行负载平衡的所有测试”,也可以通过`cypress/run`作业的参数轻松完成:

```
version: 2.1
orbs:
  cypress: cypress-io/cypress@1.0.0
workflows:
  build:
    jobs:
      - cypress/install:
           build: "npm run build"
      - cypress/run:
           record: true
           parallel: true
           parallelism: 10 
```

最少的配置，但最高的效率。

## 自定义配置

Cypress Orb 提供的工作足够全面，可以覆盖大多数典型用例。最终，您的项目可能会达到真正需要自定义设置的程度。当您达到这一点时，您可以“弹出”并使用完全解析的版本替换本地`circle.yml`配置文件，该版本公开了所有 Cypress Orb 命令及其完整的 YAML 文本。为此，您需要安装 CircleCI CLI 工具并运行:

```
$ circleci config process config.yml > config.yml 
```

通过运行这个命令，项目的 config.yml 文件将被完全解析的配置所替换，而不使用 Cypress Orb。现在，您可以根据项目需求调整配置。⚠️警告:没有自动的方法从“被驱逐”的配置回到使用 orb。

## 最后的想法

自从 Docker 出现并使建立可重复构建变得超级简单以来，我从未对持续集成服务如此兴奋过。orb 给了我们，测试工具的作者，创建可重用的、版本化的 CI 配置的能力，这立即使我们用户的生活变得更加简单。

如果您想了解更多信息，请务必注册参加 12 月 5 日的特别 [Cypress + CircleCI 网络研讨会](https://www2.circleci.com/circleci-cypress-webinar.html?utm_campaign=Cypress%20CircleCI%20Webinar&utm_medium=blog%20post&utm_source=CircleCI&utm_content=text%20link)。

请务必在[on.cypress.io/circleci-orb](https://on.cypress.io/circleci-orb)查看我们的 orb 文档，并查看 repo[cypress-io/circle ci-orb](https://github.com/cypress-io/circleci-orb)。

* * *

*Gleb Bahmutov 是 JavaScript 忍者，图像处理专家，软件质量狂热分子。白天，作为 Cypress.io 的工程副总裁，格莱布让网络变得更好。晚上，他在 https://glebbahmutov.com/blog/的[与软件错误作斗争，并写博客。微软开源软件 MVP。你可以跟踪他和他的作品](https://glebbahmutov.com/blog/) [@bahmutov](https://twitter.com/bahmutov?ref_src=twsrc%5Egoogle%7Ctwcamp%5Eserp%7Ctwgr%5Eauthor) 并找到他在[https://slides.com/bahmutov](https://slides.com/bahmutov)会议上的演讲幻灯片。*