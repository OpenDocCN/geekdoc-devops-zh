# 无服务器系列第 2/3 部分-无服务器|线束的黑暗面

> 原文：<https://www.harness.io/blog/dark-side-of-serverless>

我们的朋友阿纳金·天行者，又名达斯·瓦达，在《星球大战》第三集“西斯的复仇”中向欧比-万·卡诺比讲述他在黑暗面取得的成就，这句令人难忘的话是这部电影令人难忘的一部分。就像两个针锋相对的工程师激烈讨论一个设计范例的对与错，哪个可能是未来，哪个不是。就像任何技术上的转变一样，旧的和新的守卫之间可以划分战线。对于那些还记得 [JAVA/JEE](https://en.wikipedia.org/wiki/Java_Platform,_Enterprise_Edition) vs [NodeJS](https://en.wikipedia.org/wiki/Node.js) 争论的人来说，可能会觉得想拔牙。虽然在技术上不是 JAVA/JEE vs NodeJS，但与 JAVA/JEE 和 NodeJS 一起使用会更好。在这个无服务器博客系列的第一部分[中，我们了解到了无服务器调用的灵活性和低开销。但是像任何技术一样，无服务器也有一些缺点，在设计应用程序时，记住这些缺点是很重要的。像](https://harness.io/2019/09/serverless-series-part-1-3-give-me-code-or-give-me-death-by-infrastructure/) [Kubernetes](https://kubernetes.io/) 和 [Hadoop](https://hadoop.apache.org/) 一样，无服务器将解决你所有的问题，对吗？第一个设计考虑是如何使用这些新的无服务器功能。

## 便携性问题

将您从基础设施的束缚中解放出来，在一个无服务器的平台上执行您的代码。诱惑力听起来的确很惊人。尽管无服务器本身是基础设施，而不是在以太中运行代码的神奇平台。对于你的代码应该如何运行有不同的看法。NodeJS 是 NodeJS，JAVA 是 JAVA，但是记住无服务器生命周期的三个部分；触发、工作和输出。工作部分就是你的代码。例如，在 AWS 上，你被限制在语言的某些版本。想用 JAVA，那么在 AWS Lambda 上支持的版本是 [JDK](https://en.wikipedia.org/wiki/Java_Development_Kit) 1.8，在我写这篇博客的时候，JDK 1.12 是最新的。难以从一个提供商移植到另一个提供商的部分是触发因素，也是无服务器功能如何扩展的原因。这些功能特定于每个云供应商，如果您自己或在[平台即服务](https://harness.io/blog/what-is-paas/)上托管无服务器基础设施，它们都是不同的。使用更普遍的技术，比如 Kubernetes，可能会有一些缓解，例如， [Google KNative](https://cloud.google.com/knative/) ，这是 Google 的 [Istio](https://istio.io/) + Kubernetes +无服务器组合。KNative 很重要，因为 KNative 公开了无服务器实现中的许多活动部分。即使有了 KNative，我们仍然有很多选择和意见。

## 闻起来像基础设施

今天，我们已经习惯于选择我们的基础架构，无服务器实施也不例外。减去一系列公共云服务，如 [AWS Lambda](https://aws.amazon.com/lambda/) 、[谷歌云功能](https://cloud.google.com/functions/)和 [Azure 功能](https://azure.microsoft.com/en-us/services/functions/)，当然有办法让无服务器服务更受您控制。Apache OpenWhisk 是一个 Apache 软件基金会项目，其目标是在无服务器基础设施中更加通用。Apache OpenWhisk 可以部署到 Mesos 和 Kubernetes 中作为编排器。荣誉奖还包括[开放 FaaS](https://www.openfaas.com/) 【功能即服务】和[银河迷雾](https://galacticfog.github.io/)。谷歌确实有一些 [KNative](https://cloud.google.com/knative/) 的组合，比如 [Cloud Run](https://cloud.google.com/run/) 。KNative 生态系统随着最近来自 Lightbend 的 [CloudState 的加入而继续扩展。无论您选择哪种更通用的无服务器基础架构，您或您的组织都必须维护无服务器基础架构。随着功能以快速且有时不可预测的方式扩展，需要足够的集群容量来处理峰值。看起来好像我们把复杂性从一个领域转移到另一个领域。](https://www.lightbend.com/company/news/lightbend-launches-open-source-project-cloudstate-for-knative-kubernetes)

## 变化的复杂性

我有机会就基于容器的工作负载和基于无服务器的工作负载做了几次演讲，这两张幻灯片可以解释使用 tacos，yum！

*每个请求都会导致函数的伸缩不同。在本例中，复杂性也被转移到数据库集群*[*Redis*](https://redis.io/)*。*

与 Kubernetes 共度更简单的时光。如果没有定制，orchestrator 是非常便携的。鉴于无服务器功能是短暂的，并且可以极快地扩展，复杂性转移到堆栈的不同部分。像 2014 年第一套[基于 Kubernetes 的工作负载](https://harness.io/2019/08/kubernetes-series-part-5-6-not-everything-fits-in-kubernetes/)，有维护状态的可能。尽管在我们上面的 taco 示例中，状态将被委托给一个外部系统，比如 Redis 集群。如果开始有多个函数来完成一个请求，状态就变得非常关键。

## 你的函数有一个函数

就像《Docker 》中的 Docker 一样，【DnD】似乎认为抽象是没有界限的。反对过去的无服务器函数的一个当代论点是，你可以很容易地在一个函数中包含太多的逻辑，试图一次执行一次。尽管有了像[亚马逊步骤函数](https://aws.amazon.com/step-functions/)这样的进步，调用多个或一系列函数变得更加容易。由于调用无服务器函数的速度和简易性，臭名昭著的[大泥球](https://www.laputan.org/mud/)反模式如果不加检查的话会很快出现。“无服务器容器”可以很快实例化。有时，甚至无服务器功能中的底层语言也可以发挥作用，例如冷启动。

## 冷启动

在非无服务器的世界中，我没有过多考虑的一个问题是冷启动。在 JAVA AppServer 的世界里，除了部署或重启，你不会太在意冷启动。在与我们的朋友 [Docker](https://www.docker.com/resources/what-container) 一起到来的容器中，是的，冷启动/引导变得更加重要，但是增加足够的容量可以帮助您克服冷启动。换句话说，在需要额外容量之前，等待热容量将会接管。而无服务器则将这种冷启动问题发挥到了极致。每个请求都是一次调用，因为当请求结束时，无服务器容器被销毁。如果使用 JAVA 之类的语言，冷启动问题[是一个需要仔细设计和基础设施考虑的问题。作为前述](https://medium.com/thundra/mastering-java-cold-start-on-aws-lambda-volume-1-21c30ce378b7)[冷启动文章](https://medium.com/thundra/mastering-java-cold-start-on-aws-lambda-volume-1-21c30ce378b7)的一部分，有一些工具可以帮助你的功能保持“温暖”，比如[桑德拉](https://github.com/thundra-io/thundra-lambda-warmup)。暖功能的另一面，在这种情况下，暖的 AWS Lambda 会影响您的计费。尽管无服务器基础设施存在局限性，但无服务器正引领下一代工作负载，Harness 将助您一臂之力。

## 利用线束增强无服务器信心

Harness 的美妙之处在于能够让您的管道变得灵活和健壮。创建新的或找到适当的工作负载来利用无服务器非常重要。您的管道应该足够灵活，能够包含无服务器和非无服务器工作负载。凭借这种灵活性，增量引入无服务器比以往任何时候都更容易。在接下来的系列文章的第三部分中，我们将深入探讨 Harness 平台和无服务器基础设施，敬请关注！干杯-拉维