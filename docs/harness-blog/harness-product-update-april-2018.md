# 线束产品更新| 2018 年 4 月|线束

> 原文：<https://www.harness.io/blog/harness-product-update-april-2018>

2018 年 4 月的 Harness 产品更新-查看我们在这 3 个类别中的最新进展:智能自动化、CV 和持续安全性。

我将 2018 年 4 月的产品更新分为 3 个部分，反映了 Harness 的核心功能:

## 智能自动化

智能自动化允许客户在几分钟内构建部署管道。我们通过为客户用来运行其应用和服务的技术堆栈和工具提供本机集成来实现这一点。

早在二月份，一个主要的可交付成果就是我们的**按代码配置**特性。用户现在可以在 YAML 编写部署管道，并使用 Git 进行版本控制。

我们还增强了我们的 **Kubernetes 自动化**,支持:

*   水平 Pod 自动缩放
*   入口控制器
*   Istio 规则(蓝/绿部署和流量分流需要)
*   蓝色库伯内特服务
*   舵

了解更多:[视频](https://harness-1.wistia.com/medias/bheen1ceze)

关于**亚马逊 Web 服务**，我们早在去年 11 月就发布了 **CodeDeploy** 、 **CodePipelines** 和 **Lambda** 支持(了解更多:[视频](https://harness-1.wistia.com/medias/04xixetxvz))，我们刚刚发布了对 **AMI 工件**和 **AWS Fargate** 的支持。AKS 很快也会跟进。

为了帮助提供基础设施，我们最近增加了对**hashi corp Terraform**的支持，还增加了对**Helm**和**Microsoft Team Foundation Server(TFS)**的 CI 支持。

最后，我们添加了一个新的**目录**特性，允许用户“编目”他们自己现有的 shell 脚本，并跨线束部署管道重用它们。

## 持续验证

持续验证允许客户使用机器学习及其现有的 APM 和日志分析工具来自动化部署运行状况检查。

除了支持 AppDynamics、New Relic、Splunk、ELK、Sumo Logic 和 Logz.io，我们现在还支持 **Dynatrace** 、 **Datadog** 、**普罗米修斯**和 **AWS CloudWatch。**阅读更多:[博客](https://harness.io/blog/harness-extends-continuous-verification-dynatrace/)

我们还在主导航中添加了一个新的**实时连续验证仪表板**:

这个新的仪表板总结了所有的部署验证(测试/运行状况检查),让用户只需双击鼠标即可深入了解每个故障的根本原因。更多阅读:[博客](https://harness.io/blog/real-time-analytics/)

我们的数据科学团队也一直在尝试使用**神经网络**作为一种更准确的方法来检测应用程序日志文件中的异常。目前，我们使用 KMeans 聚类和一些 Jacard 和余弦距离算法。随着神经网络的加入，我们现在有更多的异常检测选项。

看看下面的对比:左边是传统算法(A)如何检测异常(红色)，左边是神经网络(B)如何解释相同的数据并检测异常(红色)。

## 持续安全性

持续的安全性允许客户审计、治理和控制他们的部署管道。在 Harness 诞生之初，我们就从客户那里听到了关于安全性的响亮而清晰的声音，正如您所料，您将在未来看到大量与安全性相关的功能。

上个季度，我们增加了**审计跟踪**和**秘密管理**，这样客户就可以在管理范围内管理秘密。他们也可以使用自己的商店，如 **HashiCorp Vault。**

第一个重大更新是我们新的**互联内部部署**，它允许企业在其自己的数据中心和防火墙内部署和管理线束。阅读更多:[博客](https://harness.io/blog/continuously-delivering-for-harness-on-premises-customers/)

其次，我们引入了新的**基于角色的访问控制(RBAC)** 功能，集成了 LDAP/SAML/OKTA 进行身份验证，还提供了灵活的粒度权限进行授权。阅读更多:[博客](https://harness.io/blog/harness-delivers-role-based-access-control/)
下面是这个控件的外观图:

此外，我们添加了 **IP 白名单**以进一步限制线束使用，还添加了**使用限制**以便用户可以将云提供商帐户和工具限制在特定的应用和环境中。

2018 年 4 月的产品更新到此结束，非常感谢我们的工程团队完成任务！

别忘了你可以在这里申请试驾。
干杯，
史蒂夫。
@BurtonSays