# K8s 教程:使用 Policy Engine，Polaris 来自动修复

> 原文：<https://www.fairwinds.com/blog/k8s-tutorial-policy-engine-polaris-to-automate-fixes>

 在之前的一篇博文中，我们向您展示了如何安装 policy engine，Polaris，以及如何使用仪表板、准入控制器和 CLI 工具审计您的 Kubernetes 工作负载。在本教程中，我们不仅仅看到 Kubernetes 的效率、可靠性和安全性问题，还将向您展示如何使用 Polaris 自动修复它发现的任何问题。

## 使用 Polaris CLI 工具更新您的基础设施代码

Polaris 不仅仅可以从命令行审计文件。使用 polaris fix 命令，它可以自动修改它发现的任何问题的 YAML 清单。例如，要修复部署目录中的任何问题，请运行:

```
polaris fix --files-path ./deploy/ --checks=all
```

Polaris 可能会在一些更改(例如，活性和就绪性探针)旁边留下注释，提示用户根据其应用的上下文将它们设置为更合适的内容。

并非所有问题都可以自动修复，目前只有原始 YAML 清单可以变异。舵图仍然需要手动更改(这方面的功能更新即将推出！).

## 变异 Webhook

默认情况下，Polaris 验证 webhook 将阻止或允许部署，但是您可以将 Polaris 配置为作为变异 webhook 运行，它会在发现问题时自动更改部署，而不是终止操作。

有关如何使用 Helm 安装验证 webhook 的说明，请参见 [Polaris 文档](https://polaris.docs.fairwinds.com/admission-controller/#installation)。

要启用变异 webhook，您需要将 webhook.mutate 标志设置为 true。完整的命令如下:

```
helm upgrade --install polaris fairwinds-stable/polaris --namespace demo --create-namespace --set webhook.enable=true --set webhook.mutate=true --set dashboard.enable=false
```

默认情况下，北极星变异 webhook 将改变的唯一问题是 pullPolicyNotAlways 。如果您想激活其他突变，您可以通过 [webhook.mutatingRules](https://artifacthub.io/packages/helm/fairwinds-stable/polaris#values) 标志来定义它们，或者您可以编辑您的 Polaris 配置的 mutatingRules 部分:

```
webhook:
  enableMutation: true
  mutatingRules:
  - cpuLimitsMissing
  - cpuRequestsMissing
  - dangerousCapabilities
  - deploymentMissingReplicas
  - hostIPCSet
  - hostNetworkSet
  - hostPIDSet
  - insecureCapabilities
  - livenessProbeMissing
  - memoryLimitsMissing
  - memoryRequestsMissing
  - notReadOnlyRootFilesystem
  - priorityClassNotSet
  - pullPolicyNotAlways 
```

为了更深入地了解这一特性，请查看我们的博客文章 [Kubernetes 与北极星的突变:它是如何工作的](https://www.fairwinds.com/blog/how-polaris-kubernetes-mutations-work)。

对于手动将工作负载部署到 Kubernetes 集群的人来说, polaris fix 命令和变异的 webhook 是一个很好的选择，但是如果你通过持续集成系统来验证你的代码和基础设施变化，你也可以使用 polaris。

## 将北极星添加到您的持续集成管道中

Polaris 可以在 GitLab CI、Jenkins、CircleCI 或 CodeShip 等持续集成系统中安装和运行。Polaris 将强制您的部署过程在您设置的任何条件下退出。例如，如果 Polaris 检测到您的基础架构代码 YAML 文件或掌舵图存在某些问题，任何危险级别的问题，或者如果总得分低于 75%，您可以设置退出代码。你可以配置 Polaris 只显示你失败的测试，并漂亮地打印出结果，以便人们更容易阅读。对于这组条件，CI 管道中的 Polaris 配置如下所示:

```
polaris audit --audit-path ./deploy/ \
  	--set-exit-code-on-danger \
  	--set-exit-code-below-score 75 \
	--only-show-failed-tests true \
	--format=pretty 
```

这种方法不会自动修复 Polaris 发现的问题，但会显示 CI 系统日志中的错误。

也可以使用[北极星文档](https://polaris.docs.fairwinds.com/infrastructure-as-code/#as-github-action)中的说明在 [GitHub 操作](https://github.com/features/actions)中设置北极星。

## 一次在多个集群中使用北极星

如果你有多个集群，并想用北极星一次性扫描它们，Fairwinds 提供了一个名为 [Insights](https://www.fairwinds.com/insights) 的平台。用户可以跨集群一致地集中管理 Polaris，以确保您的 Kubernetes 工作负载尽可能高效、可靠和安全。

## 资源

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)