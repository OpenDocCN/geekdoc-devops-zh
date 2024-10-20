# 审计 Kubernetes 基础设施的更简单方法:自托管的 Fairwinds Insights

> 原文：<https://www.fairwinds.com/blog/an-easier-way-to-audit-your-kubernetes-infrastructure-self-hosted-fairwinds-insights>

 我们很高兴地宣布，Fairwinds Insights 的自主发布现已进入测试阶段！

大约两年前，我们启动了 [Polaris](https://github.com/FairwindsOps/polaris) ，这是一个帮助团队将政策和最佳实践应用于 Kubernetes 资源的项目。该项目已经被 Kubernetes 社区迅速采用，我们似乎找到了一个很多组织都在纠结的痛点。来自成千上万用户的社区反馈导致了 Polaris 的大量改进，但也有要求更多企业友好的功能，这将违反 Polaris 的“做好一件事”的哲学；这些特性包括随着时间的推移跟踪调查结果、将行动项目分配给合适的工程师，以及将数据映射到 Slack、GitHub、Datadog 或他们的工程团队可能居住的任何地方。

为了满足这些需求，我们建立了一个 SaaS 产品， [Fairwinds Insights](https://www.fairwinds.com/insights) 。Fairwinds Insights 可以从 Polaris 以及近十几个其他 Kubernetes 审计机构(如 Trivy、Goldilocks 和 kube-bench)获取数据，并将所有结果放在单一窗口中。到目前为止，Fairwinds Insights 已经帮助 200 多个组织更好地了解和强化了他们的 Kubernetes 环境，使他们更加安全、高效和可靠。

然而，我们发现一些 Polaris 用户的业务需求使其难以升级。他们喜欢 Polaris 完全在他们自己的环境中运行——不需要担心将数据发送给第三方。这种担忧在医疗保健和金融等数据敏感行业的企业中尤为普遍。

Fairwinds Insights 只能在 Polaris 之上增加价值，因此我们在过去几个月中努力构建了一个可以完全在客户环境中运行的 Insights 版本。更棒的是，我们已经免费提供了前 30 天！

## 它是如何工作的

要在您自己的环境中尝试 Fairwinds 的见解，您只需`helm install:`

(注意-目前，您需要与我们的团队联系以获得安装代码)

```
helm repo add fairwinds-stable https://charts.fairwinds.com/stable
helm install fairwinds-insights fairwinds-stable/fairwinds-insights \
  --set installationCode=xyz
```

如果您已经在使用 Polaris，您也可以使用`polaris.config`参数传递一个现有的 Polaris 配置。如果您已经构建了一些自定义检查或配置了自定义严重性和豁免，这将很有帮助。

部署运行后，您可以通过运行以下命令来访问控制面板:

```
kubectl port-forward -n fairwinds-insights svc/fairwinds-insights-dashboard 8080:80
```

参见[文档](https://insights.docs.fairwinds.com/self-hosted/installation/)了解如何建立一个更永久的入口的细节，以及其他好的加固实践。

请注意，您仍然需要注册一个帐户-该应用程序将与我们的 SaaS 共享以下详细信息，以便跟踪您的试用持续时间和您的环境规模:

但就是这样！所有与安全性相关的数据，以及潜在的敏感信息，如命名空间名称、RBAC 角色和映像 sha，都是您环境的私有信息。

## 利弊

当然，托管自己的软件也会带来一些麻烦。虽然在您的环境中运行 Fairwinds Insights 很容易，但是如果您打算长期使用它，您可能需要花一些时间来强化您的安装。

### 赞成的意见

自托管部署的最大优点之一是安全风险。将漏洞数据发送给第三方可能会让安全团队不寒而栗，并且可能会阻止您以 SaaS 的身份尝试 Fairwinds Insights。它还有助于满足合规性要求，比如将数据集中存放在特定区域。

这带来了另一个好处——自托管安装更容易在内部销售。一些人告诉我们，在试用 SaaS 之前，他们必须通过安全审查，但他们可以立即开始使用自托管安装。我们希望这个选项可以减少对 Fairwinds Insights 提供的内容感兴趣的人之间的摩擦。

最后，通过自托管安装，您可以更好地控制升级到 Insights 新版本的方式和时间。我们只通过 SaaS 提供最新版本，所以如果你想确保用户界面永远不变，或者你不需要升级代理，自托管安装可能是有吸引力的。

### 骗局

自托管任何软件的最大缺点是，您现在要对运行它的基础设施负责。

首先，你需要确保一切都是安全的。幸运的是，Insights 会自我扫描，并提醒您由于配置错误或部署过时而可能出现的任何漏洞。

其次，可靠性可能会成为一个问题。Kubernetes 可以很好地自我修复，但您可能无法获得与使用 SaaS 时相同的正常运行时间。

最后，你需要有一个好的数据存储和备份计划。虽然 Insights 附带了 Postgres 和 Minio 的短暂实例，但您可能希望设置一个 RDS 实例和一个 S3 存储桶来进行更持久的存储和定期备份。

## 试试吧！

Fairwinds Insights 可供免费使用。你可以[在这里](/coming-soon)报名。我们也很乐意听到您的体验反馈！如果你想取得联系，请随时联系我们的 [Slack 社区](https://join.slack.com/t/fairwindscommunity/shared_invite/zt-e3c6vj4l-3lIH6dvKqzWII5fSSFDi1g)或发电子邮件给 insights@fairwinds.com。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)