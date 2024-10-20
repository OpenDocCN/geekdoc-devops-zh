# CI/CD 魔力象限—持续交付状态|管理

> 原文：<https://www.harness.io/blog/will-the-real-continuous-delivery-vendors-please-stand-up>

在这篇文章中了解所有关于 CD 的状态。

今天，CI/CD 市场大约有 35 家以上的供应商都声称在做同样的事情。事实上，如果你坚信每个开发人员都需要午餐来更快地交付软件，这些供应商中的一些会卖给你午餐。在过去的两年中，我们与数千名客户交谈过，公平地说，大多数客户对 [CI/CD](https://harness.io/blog/what-is-ci-cd/) 、其流程、核心用例以及工具感到困惑。我们认为是时候把一些事实摆在桌面上澄清混乱了。我是说，为什么要让事实妨碍一个好故事呢？

## 事实一:持续集成！=连续交货

剧透提醒:CI 和 CD 不是一回事。[持续集成](https://en.wikipedia.org/wiki/Continuous_integration) (CI)是关于将代码转化为工件。请将此视为代码的构建和测试。[连续交付](https://en.wikipedia.org/wiki/Continuous_delivery) (CD)是关于以一种安全、快速、可重复的方式将那些工件交付到产品中。请将此视为向您的客户部署代码。现在，以下是我们在客户会议上听到最多的 5 大 CI 工具:

1.  詹金斯
2.  切尔莱西
3.  亚特兰蒂斯竹子
4.  特拉维斯奇
5.  孔科西

我们实际上使用 Jenkins at Harness 进行 CI，我敢说我们 90%的客户也在使用它。对，就这么定了。现在，让我们把重点放在把这些美丽的文物投入生产。

## 事实 2:应用程序发布流程编排！=连续交货

17 年前的 2002 年，一家名为 Electric Cloud 的供应商诞生了，他们多年来一直致力于将应用软件自动化交付到生产中。今天，同一家厂商以大约 1500 万到 2000 万美元的收入出现在 [Gartner 应用发布协调魔力象限(ARO)](https://www.gartner.com/document/3889022?ref=solrAll&refval=222508359&qid=e1d82c697bf9284d9c5c5ffbad84) 中。电气云并不是唯一的领导者。XebiaLabs(成立于 2008 年)的收入在 3000 万至 3500 万美元之间，IBM UrbanCode Deploy(成立于 1996 年)的收入在 3500 万至 4000 万美元之间，最后，Broadcom/CA/Automic Software(成立于 1985 年)的收入在 5000 万至 5500 万美元之间。所有这些内部解决方案都是围绕“发布”的概念建立和构建的，也就是说，这是一个改变生活的重大事件，需要对需求、时间表、变更控制、静态基础设施建模以及软件工件依赖关系的管理进行项目管理。为什么？因为从 1985 年到 2008 年，这正是我们作为软件专业人员所做的。我们有项目经理，他们一丝不苟地与工程和 IT 运营部门合作，一年协调一次、两次或四次发布(如果你幸运的话)。自 80 年代和 90 年代以来，一些事情发生了变化。我同意流行音乐，总的来说，已经下降到新的绝望水平。但是，里面很多东西都变好了。例如，像 Jez Humble 和 Dave Farley 这样的先驱在 2010 年开始宣扬[连续交付](https://continuousdelivery.com/)的概念。大约在同一时间，亚马逊网络服务(AWS)在[云计算](https://aws.amazon.com/)上做了同样的事情，帕特里克·德博伊斯/约翰·威利斯[用 DevOps 挑战开发/IT 运营文化的现状](https://www.linux.com/blog/what-devops-patrick-debois-explains)。上面提到的 ARO 解决方案都不是围绕连续交付、云或开发运维的诞生而建立、设计或构建的。它们是为前几代应用程序、基础架构和 IT 组织而构建的。这些供应商自己甚至没有实践连续交付，尽管他们将自己推销为 DevOps 或 CI/CD 平台/解决方案。几周前， [CloudBees 收购了 Electric Cloud](https://venturebeat.com/2019/04/18/cloudbees-acquires-software-automation-startup-electric-cloud/) 以补充其投资组合。因此，现在您有了内部 CI + ARO，就好像它是游戏规则的改变者一样。

## 事实三:DevOps！=连续交货

DevOps 是关于团队心态和实现一系列商业想法、目标或结果的一致性——这种心态是由文化、自动化、测量和共享定义的(CAMS)。我们在 [Harness](https://harness.io) 参与的绝大多数 DevOps 团队负责拥有自动化和工具，因此他们可以为开发人员提供自助服务能力，以提高开发速度。在一个组织中，你最不想要的就是开发人员拥有自动化和每一个部署；否则，您只是创建了另一个 IT 运营孤岛和瓶颈。连续交付是软件工程师(不是开发人员)遵循的过程或模式，这样他们可以更频繁、更快速地为业务交付软件工件。CD 可以通过开发运维文化来完善，但是开发运维并不是 CD 在组织内发生的强制性条件。CD 是团队练习的东西；DevOps 是他们实践它的文化/环境。当然，DevOps 和持续交付是一个成功的组合——但是不要混淆这两者。

## 事实 4:很少有持续交付供应商真正存在

直到几年前，唯一可行的解决方案是通过用 bash 脚本扩展 CI 工具来自己构建 CD 功能。网飞就是这么做的，[在 2016 年](https://wikibon.com/netflix-highlights-the-evolution-of-cloud-platforms-with-spinnaker/)开源了他们 Spinnaker 平台的部分内容。大约在同一时间，我们创建了 Harness 来提供[持续交付即服务](https://harness.io/harness-continuous-delivery/)。在我们的第一年(出于秘密行动)，Harness 与超过 50 个[客户](https://harness.io/customers/)合作，其中包括 OpenBank(桑坦德银行的数字银行)和 SoulCycle。我们最近也很幸运地得到了 Google Ventures 和 ServiceNow 对我们的 B 轮投资。秉承我们的事业和愿景，Harness 还[通过日常生产部署](https://harness.io/2017/11/harness-uses-harness-deploy-harness-io-not-jyoti-said/)向我们的 SaaS 和内部客户持续交付产品。我的意思是，如果我们不吃自己的狗粮，我们怎么能说服客户做连续交付呢？Harness 最近也成为了 [CNCF](https://www.cncf.io/) 和 [CD 基金会](https://cd.foundation/)的一部分，以帮助推动我们行业的标准和 API。我们下周在巴塞罗那的 KubeCon 见！干杯，史蒂夫。