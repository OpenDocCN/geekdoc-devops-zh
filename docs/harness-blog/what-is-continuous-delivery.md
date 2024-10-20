# 什么是持续交付？装具

> 原文：<https://www.harness.io/blog/what-is-continuous-delivery>

理解连续交付(CD)的基本术语和概念。

准备饭菜或把事先准备好的饭菜送到家里的现象越来越多。作为软件工程师，我们以类似的方式准备。我们用我们最喜欢的容器格式构建图像，比如说 [Docker](https://www.docker.com/) 、 [Mesos](https://mesos.apache.org/) 或 [RKT](https://coreos.com/rkt/) 等等。虽然当我们建立这些的时候，我们如何把它们放到运行的容器中呢？当然，我们可以利用我们的笔记本电脑或隔离的生产前环境。我们最不想做的事情就是扔掉我们准备好的饭菜，因为它们已经变质了，没有人吃。

## 我的图像在 Nexus 中，但我完成了吗？

恭喜你，你的 [Docker 推送](https://docs.docker.com/engine/reference/commandline/push/)成功进入你选择的神器库，例如[Nexus](https://www.sonatype.com/nexus-repository-sonatype)/[Artifactory](https://jfrog.com/artifactory/)/[Harbour](https://goharbor.io/)。当标签增加到下一个版本时，感觉很棒。是时候吃点小吃了，也许是时候吃一顿你本周早些时候准备的午餐了。当你在加热你的食物时，点击你手机上的应用程序的刷新按钮，改变就不会出现了。您跳回到 [Slack](https://slack.com/) 上，检查部署经历的任何警报，什么也没有。这是怎么回事？

## 好吧，部署警报到底在哪里？

我确实认为 Slack 是你的聊天工具的一部分。不过，如果你的公司没有使用某种聊天操作机制来发出警报，也不用担心。突出的一点是，你做了艰苦的工作，使一个图像准备成为一个运行的容器，而不是你的笔记本电脑，这是不会发生的。有一个伟大的目标，容器将协调我们所有的环境，并允许按钮或简单的按钮部署。如果使用像 [Kubernetes](https://kubernetes.io/) 或 [Marathon](https://mesosphere.github.io/marathon/) 这样的管弦乐队，这甚至不应该成为 easy button deploy 的问题；发送给编制者，让编制者直接规划部署。要是事情有这么简单就好了。

## 没那么快，我们还得部署

让我们不要忘记部署软件的严格性和纪律性。我们当然是有才华的工程师，一路上不断改进我们的工艺，但我们不想成为生产中断的过错方。FaceBook 创造的格言“快速工作，打破常规”很可能不适用于我们大多数人。回到我们的午餐时间部署。我们吃了我们的餐前准备，因为我们没有食物了，面临着一个选择:找到另一种小吃或回到我们的笔记本电脑，找出哪里出了问题。我们登录到我们的工件库来验证我们的最新版本在那里，甚至为了理智起见，对我们刚刚创建的东西做一个 [Docker Pull](https://docs.docker.com/engine/reference/commandline/pull/) 。当您执行 [Docker Run](https://docs.docker.com/engine/reference/run/) 时，您的更改就在那里了。现在把它们放到野外是另一回事了(也就是你的管道)。

## 欢迎来到你的管道

当我们在环境中从一个阶段进入另一个阶段时，我们努力建立信心，相信我们引入的更改将按设计运行，不会降低或破坏我们的应用程序。有大量的测试、代码覆盖、部署和回滚策略来确保这一点，更不用说底层基础设施和潜在的监管/变更控制要求，这些都是为您的辛勤工作营造一个良好的环境所必需的。随着 [GitOps](https://harness.io/blog/what-is-gitops/) 的出现，提交/合并可以成为将代码更改为图像并最终部署到您选择的编排工具中的容器/pod 所需的触发器。有了将您的图像制作成产品所需的一切，当您想要开发下一组特性时，是否可以放慢速度或为明天准备午餐？这里的目标是，当您的更改准备就绪时，这些更改将被发布/部署。这是连续交货。

## 即将到来的涅槃

在 Harness，我们已经解决了持续交付的复杂性，并以常规和简单的方式来解决它们。任何人都可以构建一个管道，声明性地完成最复杂的交付任务。向 Kubernetes 部署[金丝雀版本？没问题。我们发布了 Harness 平台的社区版，让任何人都能够拥有强大的持续交付渠道。](https://docs.harness.io/article/wkvsglxmzy-kubernetes-canary-workflows)

## 立即获取您的社区版！

我们会很高兴让你注册[你的免费账户](https://harness.io/try-continuous-delivery-as-a-service-for-free/)来实现比你吃午饭还快地取出你的文物的梦想。我们还推出了[马具社区](https://community.harness.io/)来培养那些寻找下一代持续交付的人的社区意识。把船修好！拉维