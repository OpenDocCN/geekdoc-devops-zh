# 利用持续交付迁移云|利用

> 原文：<https://www.harness.io/blog/migrating-clouds-with-harness-continuous-delivery>

Harness CD 允许您自动将软件工件部署到任何云或数据中心基础设施，并且它使用抽象来实现这一点。

大约一年前，Harness 迁移了公共云提供商，令我们的团队惊讶的是，这个过程只花了 76 分钟，没有一个嗝或喷嚏。

下面是我们的#工程松弛渠道的样子:

2018 年 6 月 5 日晚上 8 点 12 分，我们的副总裁客户 Success Ohad(他喜欢冰淇淋)向我们的工程师询问状态更新，几分钟后，我们的工程师 Brett 通过线束部署管道开始了迁移([线束使用线束进行连续交付](https://harness.io/blog/harness-uses-harness-cd/))。76 分钟后，部署管道完成，DNS 被切换。没有爆炸、电击、雷电。这是你可能见过的最平静的云迁移。

那么，我们是怎么做到的呢？

**简单。我们吃了自己的狗粮。**

简而言之:Harness 允许您将软件工件自动部署到任何云或数据中心基础设施，并且它使用抽象来实现这一点。

在这篇文章中，我将介绍 Harness CD 抽象模型(CDAM ),然后总结该模型可以帮助您的不同云迁移策略。

## 线束光盘抽象模型(CDAM)

Harness 背后的关键架构决策之一是为客户提供服务、基础设施和工具链之间的抽象。

依赖是邪恶的；您的部署管道越是特定于服务/基础设施/工具，那么如果您希望重新搭建平台或重新装备，迁移它们所花费的时间就越长。

Jenkins 管道就是一个例子，因为所有底层脚本/作业都倾向于特定于服务/基础架构/工具，这意味着部署管道是硬连线的(例如，仅 AWS AMI 部署)。随着服务随着时间的推移而发展，并结合新的技术和工具，它们变得“脆弱”和“难以扩展”。

为了最小化依赖性并提高速度，我们创建了 Harness CD 抽象模型(CDAM):

简而言之，我们的[连续交付平台](https://harness.io/platform/continuous-delivery/)不依赖于服务、基础设施或工具。

这里有几个例子:

**服务** - Harness 支持多种工件(jar、AMIs、容器、lambda 函数、...)和存储库(Artifactory、Nexus、Jenkins、S3、DockerHub、...).
**Provisioners**——Harness 支持 AWS CloudFormation、HashiCorp Terraform 和自定义脚本，用于供应基础设施。
**云提供商** - Harness 支持每个主要的云提供商，并支持传统的内部云/DC。
**验证**——Harness 支持所有主要的监控、APM 和日志管理工具，此外还有一个定制的 API。
**机密** - Harness 支持 HashiCorp Vault、AWS 机密管理器，并通过 AWS KMS 提供本机机密管理。
**部署策略** -线束支持滚动、多服务、蓝/绿和淡黄色部署策略。
**通知策略**——Harness 支持 Email、Slack、HipChat、PagerDuty、吉拉、ServiceNow。

底线是您可以接入和断开服务、基础设施、工具和策略，而不必重写所有的部署管道。Harness 使用抽象和 API 为您完成集成工作，因此您不需要自己编写脚本。

自从 Harness 在 2017 年 10 月退出隐身以来，像 [OpenBank(桑坦德)](https://harness.io/customers/case-studies/openbank-on-demand-deployments/)、 [SoulCycle](https://harness.io/customers/case-studies/soulcycle-ci-cd-harness/) 、 [Nutanix](https://harness.io/customers/case-studies/master-kubernetes-deployments-on-gcp/) 和 [Advanced Software](https://harness.io/customers/case-studies/empower-engineers-with-cd/) 这样的公司都使用 Harness CDAM 大大加快了他们的云迁移。

## 提升和移动

提升和转移可能是最容易理解和执行的迁移策略。

将你的应用程序/服务放在一个包装虚拟机、AMI 或容器中，并将其放在任何云计算上。任务完成。这适用于运行在 Docker、Kubernetes、Microsoft IIS、Apache Tomcat 或 JBoss 等平台上的独立服务。唯一改变的是底层计算资源。

要在 Harness 中做到这一点:
定义服务(工件类型)及其位置(存储库):

定义目标环境(服务基础设施/集群/区域/负载均衡器&云提供商):

定义工作流程(部署和发布策略):

现在运行线束工作流来提升和移动服务。就这么简单。

## 重新搭建平台

我们的 Harness 云迁移实际上是一种重新平台迁移策略。Harness by design 是作为一个软件即服务(SaaS)平台构建的，但随着我们与几家企业买家的合作，对内部解决方案的需求增加了。因此，我们使用云迁移作为一种手段，重新平台化到一个更加云原生的容器化 Kubernetes 应用程序，这样我们就可以从增强的可移植性、管理和可伸缩性中受益。它还将允许那些要求内部利用的少数企业客户在他们想要的任何 Kubernetes 集群或基础设施上运行。

重平台类似于升降移；唯一的区别是，您可能正在更改服务的工件类型。在我们的例子中，Harness 从在我们的部署工作流中部署几个本地 jar 变成了一个容器。我们还将我们的云提供商从 X 改为 Y 以及环境集群。

好消息是，Harness 为 Kubernetes 提供了一流的公民支持，还会查询您的云提供商帐户，以便动态填充环境的集群、配置和元数据。切换云提供商和集群只需 30 秒。您不必编写任何脚本、代码或配置来重新平台化和重新部署您的服务，Harness 提供了所有这些。

## 加入新的云服务

加入新服务(例如 Kubernetes 微服务)可能是我们在客户那里看到的最常见的迁移策略。

通常，当客户从整体架构过渡到微服务架构时，他们需要构建新的部署管道来“搭载”这些服务。客户无需花费数周时间使用 Jenkins Pipelines 编写脚本和进行板载，只需使用 Harness 几个小时就可以实现这一点。

例如，OpenBank(桑坦德银行的数字银行)说“我们的詹金斯管道工作太难管理和部署了。”

以下是如何在 5 分钟内建立一个 Kubernetes 管道:

## 混合云架构

有了 Harness，定义一个构建在多个服务(和混合云)上的单一环境变得非常容易。

例如，让我们假设我们创建了一个新的生产环境(如下)，它有 3 个微服务，每个微服务运行在不同的云中(AWS、Azure、GCP)。我们可以使用线束环境向导在不到两分钟的时间内定义该环境:

混合云的利用也不仅限于公共云提供商，您可以为内部数据中心指定私有云或裸机基础架构。

显然，Harness 无法重构或重新构建现有的整体或遗留应用程序，使其成为云原生的。我们的工程师还没有得到吉拉的那张名为“自动重构魔法”的入场券。我们只能希望。

在此期间，快乐的云朵跳跃着。

你可以[在这里注册我们的免费社区版](https://harness.io/try-continuous-delivery-as-a-service-for-free/)Harness。
干杯，
史蒂夫。