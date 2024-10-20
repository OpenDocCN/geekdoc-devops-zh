# 北极星对库贝林特

> 原文：<https://www.fairwinds.com/blog/polaris-vs-kubelinter>

 识别 Kubernetes 的错误配置可以帮助您确保您的集群是安全、高效和可靠的。Fairwinds 的[【Polaris】](https://polaris.docs.fairwinds.com/)是我们团队创建的开源工具，用于帮助我们管理 [客户](//www.fairwinds.com/customer-stories) 的数百个生产工作负载。它运行各种检查，以确保 Kubernetes pods 和控制器使用 Kubernetes 最佳实践进行配置，从而帮助避免工作负载配置问题。

您可以在每个集群中安装 Polaris 来识别错误配置。如果您需要多集群可见性，我们有一条 [升级](https://www.fairwinds.com/fairwinds-polaris-upgrade) 到 Fairwinds Insights 的路径，其中不仅包含北极星，还包含其他开源工具，包括:

*   [冥王星](https://github.com/FairwindsOps/pluto) (识别弃用的 K8S 资源)

*   [Nova](https://github.com/FairwindsOps/Nova) (查找过时或弃用的舵图)

*   [Trivy](https://github.com/aquasecurity/trivy) (针对容器的漏洞扫描器)

*   [](https://github.com/FairwindsOps/goldilocks)(建议资源请求和限制)

这不仅允许您跨多个集群、团队和环境管理 Kubernetes 的错误配置，还可以识别问题、提醒团队(通过仪表板或 Slack 集成)并建议补救措施。

KubeLinter 是 2020 年末推出的新开源工具。KubeLinter 分析 Kubernetes YAML 文件和舵图，并对照各种最佳实践进行检查，重点是生产就绪性和安全性。

如果你正在比较北极星和库伯林特，这里有一个快速的检查纲要。(截至 2011 年 2 月 23 日的对比)

| **北极星** | **库贝林特** | **描述** |
| **安全** |   |   |
| 是 | 不 | security context . allowprivilegescalation |
| 是 | 不 | 缺少 TLS 设置 |
| 是 | 不 | 主机端口属性已配置 |
| 是 | 不 | PID 属性已配置 |
| 是 | 不 | 主机网络已配置 |
| 是 | 不 | 主机 IPC 已配置 |
| 是 | 不 | [【危险能力(此处阅读列表)](https://github.com/FairwindsOps/polaris/blob/master/checks/insecureCapabilities.yaml) |
| 是 | 不 | 不安全的功能包括 |
| 是 | 是 | 不删除 NET_RAW 功能的容器 |
| 是 | 是 | 以特权身份运行的容器 |
| 是 | 是 | 未运行 readOnlyRootFilesystem 的容器 |
| 是 | 是 | 容器设置为 runAsNonRoot |
| 不 | 是 | 对象使用环境变量中的机密 |
| 不 | 是 | 没有“电子邮件”注释但有有效电子邮件的对象 |
| 不 | 是 | 缺少“所有者”标签 |
| 不 | 是 | 将主机路径装载为可写的容器 |
| 不 | 是 | 公开端口 22 的部署，通常保留用于 SSH 访问 |
| 不 | 是 | Pods 使用默认服务帐户 |
| **效率** | 是 | 是 |
| 缺少内存请求和限制 | 是 | 是 |
| 缺少 CPU 请求和限制 | **可靠性** | 是 |
| 不 | 图像标签未指定或最新 | 是 |
| 不 | 映像获取策略未设置为“始终” | 是 |
| 不 | 没有为 pod 设置 PriorityClassName | 是 |
| 不 | 只有一个副本集用于部署 | 是 |
| 是 | 缺少活性探测 | 是 |
| 是 | 缺少就绪探测 | 没有；[冥王星](https://github.com/FairwindsOps/pluto)手柄 |
| 是 | 在扩展 v1beta 下使用不推荐使用的 API 版本的对象 | 不 |
| 是 | 未指定单元间反关联性的多个副本，以确保 orchestrator 尝试在不同的 | 不 |
| 是 | 引用未找到的服务帐户的窗格 | 不 |
| 是 | 选择器与 pod 模板标签不匹配 | 不 |
| 是 | 没有匹配部署的服务 | 不 |
| 是 | 部署使用不推荐使用的 serviceAccount 字段 | [![New call-to-action](img/985ac214d57c5bf773f314566431f015.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/0a801c86-38f7-468e-aad9-ce5bc41dc69b)

 |
| No | Yes | Deployments use deprecated serviceAccount field |

[![New call-to-action](img/985ac214d57c5bf773f314566431f015.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/0a801c86-38f7-468e-aad9-ce5bc41dc69b)