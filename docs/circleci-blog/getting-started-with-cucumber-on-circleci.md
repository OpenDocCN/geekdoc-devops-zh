# CircleCI - CircleCI 上的黄瓜入门

> 原文：<https://circleci.com/blog/getting-started-with-cucumber-on-circleci/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

![cucumber-logo.png](img/0e00bac053a2f552019f301362a7e675.png)

Cucumber 是一个行为驱动开发(BDD)工具，用于开发 web 应用程序的验收测试。出于本文的目的，我将假设您的 CircleCI 设置运行顺利，并将重点放在【Cucumber 是什么，如何很好地使用它，以及如何将其与您的 CircleCI 设置集成。

## 黄瓜到底是什么？

Cucumber 是一个 BDD 工具，你可以用它来验证你的应用程序的功能。cumber 测试是用一种叫做“ [Gherkin](https://github.com/cucumber/cucumber/wiki/Gherkin) 的语言编写的，它用一种自然的、类似语言的语法来表达期望的行为和软件的预期结果。BDD 不同于传统的自动化测试，因为测试既不是代码级的单元测试，也不是与 UI 交互的应用程序级脚本。BDD 测试表达了测试的一组先决条件，然后表达了测试本身，然后是预期的结果。结合一些文档(可以很容易地以一种小黄瓜友好的方式格式化)，一组 Cucumber BDD 测试也可以作为一组全面的文档，供产品所有者或开发人员/QA 人员在将来审查。对于主要语言不是英语的团队，小黄瓜支持[多种语言](https://docs.behat.org/en/v2.5/guides/1.gherkin.html#gherkin-in-many-languages/)。

## 当时给定的

黄瓜测试的基本结构是“给定-何时-然后”格式。

“给定”是测试的设置。您可以使用 GIVEN 来设置测试所需的数据类型。您使用 GIVEN 来设置状态。

“何时”是测试的动作。WHEN 描述了您希望针对在测试的给定部分中设置的状态执行的操作。

“然后”是预期的结果。你用 THEN 来陈述你希望测试的结果是什么。

请注意，GIVEN、WHEN 和 THEN 都支持使用“and”关键字的多个步骤。这里有一个例子:

*   鉴于简有活跃的档案
*   她的资料显示萨姆是她的朋友
*   她的档案里没有朋友比尔
*   当她输入新的公共状态更新时
*   然后，Sam 应该会看到她新的公共状态更新
*   比尔应该看不到她新的公开状态更新

## 功能和场景

一大堆“给定时间”的步骤很快就会变得笨拙。Gherkin 提供了一种将测试组织成特性和场景的方法。一个特性是一个高层次的类别，它很好地映射到一个用户故事或者你的团队用于基本开发单元的任何东西。一个特性由一个或多个场景组成，一个场景由给定的 When Then 步骤组成。(上面的给定时间示例是一个场景。)你可以把一个场景想象成一个测试用例，把一个特性想象成一组逻辑测试用例。

## 步骤定义和挂钩

该工具需要一些后端编码，以便正确运行其测试。Given-When-Then 中的每一步都需要一个步骤定义才能执行。您可以使用与您的步骤相匹配的正则表达式，以及当正则表达式匹配时要执行的 Ruby 代码块来设置步骤定义。步骤定义可以跨场景重用。

钩子是您在测试运行之前和之后设置和拆除测试状态的方式，特别是为了保持测试数据库的良好状态。

## 与 CircleCI 集成

将您的黄瓜测试与 CircleCI 集成非常简单。Cucumber 有一些用于测试结果输出的格式化程序插件。使用 JUnit 格式化程序并配置 Cucumber 输出到`$CIRCLE_TEST_REPORTS/cucumber`目录。 [CircleCI 文档页面](https://circleci.com/docs/test-metadata/#cucumber)建议将以下 YAML 添加到您的 circle.yml:

```
test:
  override:
    - mkdir -p $CIRCLE_TEST_REPORTS/cucumber
    - bundle exec cucumber --format junit --out $CIRCLE_TEST_REPORTS/cucumber/junit.xml 
```

很简单。如果您喜欢 JSON 格式化程序，文档页面还显示了如何配置 circle.yml 文件。

这个配置允许 CircleCI 查看您的黄瓜测试是通过还是失败，并且您可以配置 CircleCI 如何响应失败的黄瓜测试。

Cucumber 是一个强大的工具，用来表达软件系统的期望行为，并将其集成到 CircleCI 构建中，可以为您的开发过程带来很多价值。