# Kubernetes 治理:什么是 OPA，为什么要关注？

> 原文：<https://www.fairwinds.com/blog/kubernetes-governance-what-is-opa>

 政策和治理是 Kubernetes 空间中经常出现的术语，但是我们实际上是如何做的呢？这就是开放策略代理(OPA)的用武之地。OPA 是一种工具，它允许我们通过使用减压阀语言将策略定义为代码来设置防护栏。

使用开放策略代理，我们可以创建的策略类型的一些示例包括:

*   阻止部署到特定的名称空间

*   限制我们可以从哪些容器库中提取图像

*   设置部署的最小副本数量以增强可靠性

*   当主机名太长时，阻止使用重复的主机名进行访问

*   围绕安全性、可靠性等实施最佳实践

通常，组织会试图通过使用某人创建的遵循 Kubernetes 最佳实践的模板来建立一个松散的策略和治理系统。然后，他们将与团队的其他成员分享它，假设没有人冒险离开这个模板，那么一切都将正常工作。不幸的是，现实情况是部署的东西确实偏离了该模板，如果攻击者由于凭据受损而设法进入集群，您可以肯定他们不会遵守您的模板，因为他们会部署额外的映像或安全配置来允许他们访问更多信息。

## Kubernetes 中的准入控制器

OPA 本身实际上不能阻止资源进入 Kubernetes 集群。这就是准入控制器适合的地方。准入控制器是 Kubernetes API 中决定是否允许资源进入集群的最后一项检查。OPA 与准入控制器一起工作，让它知道试图进入集群的资源是否符合我们已经定义的策略。如果是，那么资源将被应用到集群。如果没有，资源就会被阻塞。

### 示例规则

```
notInNamespace[actionItem] {
    # List Kubernetes namespaces which are forbidden.
    blockedNamespaces := ["default"]
```

```
    namespace := blockedNamespaces[_]
    input.kind == "Pod"
    input.metadata.namespace == namespace
```

```
    actionItem := {
        "title": "Creating resources in this namespace is forbidden",
        "description": “This pod is forbidden from the specified namespace”,
        "severity": 0.7,
        "remediation": "Move this resource to a different namespace",
        "category": "Reliability"
    }
}
```

对于上面的 OPA 策略，我们看到了一个名为 notInNamespace 的规则。notInNamespace 规则有许多条件，所有这些条件都列在花括号中。为了阻止资源进入集群，所有这些条件必须评估为真。让我们来看看它们:

1.  第一个和第二个条件 blockedNamespaces := ["default"]，以及 namespace:= blocked namespaces[_]，定义了我们希望阻止的名称空间列表。在这种情况下，它只是默认的名称空间。

2.  第三个条件 input.kind == "Pod ",检查我们试图应用到集群的资源是否是一个 Pod。如果是，我们将继续评估条件，如果不是，它将通过此策略并自由进入集群。

3.  下一个条件 input . metadata . namespace = = namespace 根据禁止的名称空间列表检查 pod 的名称空间。如果 pod 名称空间不在禁止列表中，那么它将通过该策略，并可以进入集群。如果不是，它将继续评估规则。

4.  这条规则的最后一条只有在前面的所有条件都为真时才会被计算。一个单元正试图进入集群，它希望部署到我们禁止的名称空间。此时，它将创建具有适当描述、标题、严重性、补救和类别的操作项，因为此规则捕获了试图部署到禁止的名称空间的 pod。

从这里开始，准入控制器需要以某种方式进行配置，以知道应该阻止什么。这就是 Fairwinds Insights 和 T2 OPA 合作得很好的地方。在上面列出的操作项示例中，我们使用操作项和严重性的概念来告诉准入控制器在 OPA 的具体实现中应该阻止什么。任何高于 0.7 的严重性都会被阻止，任何低于 0.7 的严重性只会在 Insights 平台中创建一个操作项。

## Fairwinds Insights 和 OPA/准入控制器

那么什么是 Insights，它如何连接准入控制器和开放策略代理？

Insights 是一个围绕 Kubernetes 中的安全性、可靠性、效率、成本和节约信息的平台。该平台使用各种插件，大部分是开源的，来收集集群或 CI 管道中的信息。这些操作项可以自动向 Slack 发送信息，在吉拉创建票据，或者向 PagerDuty 发送警报。UI 还提供了一种登录和查看集群和成本信息的方式。

> Fairwinds Insights 可供免费使用。你可以在 这里 [报名。](https://www.fairwinds.com/coming-soon)

在 Insights 中设置准入控制器的方式会导致严重性为 0.7 或更高(高或关键)的操作项被阻止。在上面的规则中，如果 pod 试图部署到禁止的名称空间，我们创建了严重性为 0.7 的操作项，因此在这种情况下，pod 将被拒绝进入集群。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)