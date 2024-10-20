# 关于 Pluto 的更新，以及这个开源项目如何识别 Kubernetes 1.22 中被否决的 API

> 原文：<https://www.fairwinds.com/blog/update-on-pluto-and-how-this-open-source-project-can-identify-deprecated-apis-in-kubernetes-1.22>

 在 Fairwinds，我们依靠 [我们的开源项目](https://www.fairwinds.com/open-source-software) 来帮助客户和我们的社区为他们的组织构建合适的 Kubernetes 架构。我们最受欢迎的开源项目之一， [Pluto](https://pluto.docs.fairwinds.com/) ，允许用户在他们的基础设施即代码存储库和 Helm 版本中轻松找到已弃用的 Kubernetes API 版本。我们的开源项目 Pluto 以最初被弃用的矮行星命名，它识别 Kubernetes 对象中过时的 API，因为它们被更稳定的 API 所取代。

## 折旧政策

虽然废弃的 API 版本很容易修复，但诀窍是找到所有您可能使用过在下一次升级中将会过时的版本的地方。让 Kubernetes API 服务器为您识别它们可能会有问题。如果您要求 API 服务器为您提供 deployments.v1.apps，并且部署最初创建为 deployments.v1beta1.extensions，服务器将转换 API 版本并返回一个包含 apps/v1 的清单。这意味着，至少可以说，定位被否决的 API 被使用的位置是一个挑战。关于冥王星最初为什么被创造的更多信息可以在冥王星文档 中找到。

Pluto 通过检查一些可能运行废弃版本的不同地方来解决这个问题。在基础设施即代码存储库中，Pluto 可以检查旧 API 版本的静态清单和图表。在实时 Helm 版本中，Pluto 可以确认在集群中运行的 Helm 版本不包含废弃版本。Pluto 区分简单地被否决的 API 版本和从 Kubernetes API 中完全删除的版本。

## 冥王星升级

CircleCI 以“orbs”的形式引入了可重用配置的概念，这是一个可重用的 YAML 配置包，它将重复的配置压缩到一行代码中。这些 [球体](https://pluto.docs.fairwinds.com/orb/) 是可重复使用的代码片段，可以帮助组织:

*   加速项目设置
*   自动化重复的流程
*   简化第三方集成
*   节省项目配置时间
*   提高组织效率
*   使与第三方工具的集成更加容易

从 Pluto v5.2.0 开始，Fairwinds 发布了一个名为 [fairwinds/pluto](https://circleci.com/orbs/registry/orb/fairwinds/pluto) 的球体，以便在 CircleCI 内部提供更简单的配置。我们要感谢我们的开源社区对这个项目的贡献。

我们最近也开始用 cosign 为我们的容器签名，将它们推到一个新的位置:us-docker.pkg.dev/fairwinds-ops/oss/pluto.用户现在可以运行 cosign verify—key[https://artifacts.fairwinds.com/cosign.pub](https://artifacts.fairwinds.com/cosign.pub)来检查签名。这是一个供应链安全措施，帮助从业者验证他们正在运行 Fairwinds 发布的实际 Pluto 容器。

最后，鉴于 Kubernetes v1.22 中的许多弃用和删除，我们已经确保用最新的可用信息更新我们的版本列表。如果您确实在 Pluto 报告的版本中发现错误或更新，请通过提交 [Github 问题](https://github.com/FairwindsOps/pluto/issues/new?assignees=&labels=bug%2Ctriage&template=bug.yaml) 让我们知道。

## 我们的开源社区

在 Fairwinds，我们是 Kubernetes 和围绕它的开源技术社区的忠实拥护者。多年来，我们的团队已经为客户管理了数百个集群，我们已经了解了很多关于运行 Kubernetes 所涉及的问题和陷阱。事实上，我们坚信 [开源软件为更具竞争力的商业模式铺平了道路。](https://www.forbes.com/sites/forbestechcouncil/2022/03/11/how-open-source-software-is-paving-the-way-for-more-competitive-business-models/?sh=64c8a00e19eb)

即使我们的开源社区中有成千上万的用户，我们也一直在寻找方法将人们聚集在这些项目周围，讨论开发并给实践者一个影响正在进行的路线图的机会。这就是为什么我们在去年建立了我们的 [Fairwinds 开源用户组](https://www.fairwinds.com/open-source-software-user-group)——给成员一个提问、解决问题、与其他用户交流和分享他们环境中工作的机会。

我们的下一次开源社区会议将于 2022 年 6 月 22 日*美国东部时间上午 11 点|太平洋时间上午 8 点举行。*如果你有兴趣加入我们，请 [注册](https://www.fairwinds.com/open-source-software-user-group) 群，我们将向你发出正式邀请。如果您参加我们的下一次开源会议，您将获得一些优秀的 Fairwinds 奖品！

## 费尔温德见解

如果您有兴趣在多个集群中运行 Pluto，随着时间的推移跟踪结果，与 Slack、Datadog 和吉拉集成，或者解锁其他功能，请查看我们在 Kubernetes 集群中用于审计和执行策略的平台[fair winds Insights](https://www.fairwinds.com/pluto-user-insights-demo?utm_source=pluto&utm_medium=pluto&utm_campaign=pluto)。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)