# 如何在 Fairwinds Insights 中创建您的第一个 OPA 自定义策略

> 原文：<https://www.fairwinds.com/blog/how-to-create-your-first-opa-custom-policy-in-fairwinds-insights>

 Fairwinds Polaris 已经到了 4.0 版本，有一些令人敬畏的新功能！(对于那些保持分数的人来说，由于一些[突破性的变化](https://github.com/FairwindsOps/polaris/blob/master/docs/changelog.md)，我们很快就跳过了 2.0)。

我们最初编写 Polaris 是为了帮助 Kubernetes 用户在部署工作负载时避免常见的陷阱。在为几十个组织管理数百个集群的过程中，Fairwinds SRE 团队不断看到相同的错误:资源请求和限制未设置，活跃度和就绪性探测被忽略，容器请求完全不必要的安全权限。这些肯定会造成令人头疼的问题——从停机到成本超支，甚至是安全漏洞。我们将 Polaris 视为将我们所有的战斗伤痕编码到一个单一的配置验证器中的一种方式，这将使整个 Kubernetes 社区受益。

随着 Polaris 从一个仪表板发展到一个准入控制器(以防止这些资源进入集群)，再到现在的 CI 工具(以防止它们甚至进入代码回购基础设施)，我们收到了越来越多的实施新检查的请求，比如入口是否使用 TLS，或者一个部署是否附带一个 PodDisruptionBudget。

为了更好地满足这些需求，我们在[自定义检查功能](https://polaris.docs.fairwinds.com/customization/custom-checks/)中实现了三个主要的新特性:

*   检查非工作负载类型的能力，如入口、服务和集群角色
*   引用模式中其他字段的能力
*   交叉检查资源的能力，例如，确保部署有相应的 PodDisruptionBudget

## 支持非工作负载类型

Polaris 最初设计用于检查集群中运行的工作负载，例如，Pods 和任何创建 Pods 的东西，如部署、CronJobs 和 StatefulSets。这是我们看到最痛苦的错误发生的地方，也是一个自然的起点。

然而，当团队开始部署 Polaris 并看到控制工作负载配置的价值时，他们看到了检查其他 Kubernetes 资源的自然潜力。例如，一些公司有关于入口使用 TLS 的内部或监管要求，并希望检查每个入口对象是否启用了 TLS。

添加对新资源类型的支持需要一点重构。最初我们只需要检索一组固定的资源类型，所以我们能够使用类型良好的 [client-go](https://github.com/kubernetes/client-go) 函数，比如`Deployments(``""``).List()`。但是支持任意类型需要我们利用[动态客户端](https://godoc.org/k8s.io/client-go/dynamic)，由于缺乏类型安全，这需要更多的关注。

为了让您开始，我们已经实现了一个[检查来确保入口对象正在使用 TLS](https://github.com/FairwindsOps/polaris/blob/master/checks/tlsSettingsMissing.yaml) 。如果您有其他想法，您可以将它们添加到您自己的 Polaris 配置中，或者甚至更好，打开一个 PR 将它们贡献回核心回购！

## 支持自我参考

JSON Schema 是一种非常直观和强大的验证资源的方法，但是与更具编程性的框架(比如 OPA)相比，它有一个缺点:JSON Schema 更简单，但是它不能做 OPA 能做的一些更复杂的事情。

特别是，北极星 2.0 没有自我参照的方法。例如，您可能想要检查的一件事是`app.kubernetes.io/name`标签是否与`metadata.name`字段匹配。OPA 可以很容易地做到这一点:

```
package fairwinds

labelMustMatchName[result] { 

  input.metadata.labels["app.kubernetes.io/name"] != metadata.name 

  result := { 

    "description": "label app.kubernetes.io/name must match metadata.name", 

  } 

} 
```

为了在 Polaris 中支持这一点，我们在 JSON 模式支持中添加了一些基本的模板:

```
successMessage: Label app.kubernetes.io/name matches metadata.name
failureMessage: Label app.kubernetes.io/name must match metadata.name
inds:
- Deployment
schema:
  '$schema': http://json-schema.org/draft-07/schema
  type: object
  properties:
    metadata:
      type: object
      properties:
        labels:
          type: object
          required: ["app.kubernetes.io/name"]
          properties:
            app.kubernetes.io/name: ""
```

虽然这仍然没有给*OPA 所提供的所有*灵活性，但它允许我们处理 Polaris 2.0 无法解决的大多数用例。

## 支持跨资源引用

我们收到的第一个也是最常见的请求之一是能够检查部署是否有关联的 PodDisruptionBudget 或 HorizontalPodAutoscaler。这些资源对于确保部署适当扩展至关重要，并且是大多数组织的部署策略的重要组成部分，因此想要检查这些资源是很自然的事情。

这里的挑战是 Polaris 检查是使用 JSON 模式定义的。这对于单个资源来说非常好——我们只是根据检查的模式验证 Kubernetes API 返回的 JSON。但是为了支持交叉引用，我们必须做一些事情:

*   提供一种访问非控制器资源的方法(上面的✅)
*   将某些字段模板化，例如，可以在 poddisruptionbudget(上面的✅)中断言部署的名称
*   提供在同一检查中定义多个模式的语法

事不宜迟，下面是我们创建的检查，以确保 PDB 附加到所有部署，它使用所有三个新功能:

```
successMessage: A PodDisruptionBudget is attached
failureMessage: Should have a PodDisruptionBudget
kinds:
- Deployment
schema:
  '$schema': http://json-schema.org/draft-07/schema
  type: object
  properties:
    metadata:
      type: object
      properties:
        labels:
          type: object
          required: ["app.kubernetes.io/name"]
          properties:
            app.kubernetes.io/name: 
additionalSchemas:
  PodDisruptionBudget:
    '$schema': http://json-schema.org/draft-07/schema
    type: object
    properties:
      metadata:
        type: object
        properties:
          name:
            type: string
            const: -pdb
      spec:
        type: object
        properties:
          selector:
            type: object
            properties:
              matchLabels:
                type: object
                properties:
                  app.kubernetes.io/name:
                    type: string
                    pattern: 
```

]

这里需要注意一些事情:

首先，`kinds`字段告诉 Polaris 将这个检查与哪些资源相关联。也就是说，如果上述检查失败，您将在相关部署旁边看到一个❌(而不是在 PDB 旁边)。

接下来，`schema`域像往常一样工作，检查主资源。

最后，`additionalSchemas`字段是从 Kind 到 JSON 模式的映射。在上面的检查中，Polaris 将检查同一名称空间中的所有 pdb，并试图找到一个与模式匹配的 pdb。如果它没有发现任何东西，检查就会失败。

## 结论

我们对最新发布的北极星和我们增加的新功能感到兴奋。如今，北极星拥有超过 10，000 名用户，涵盖所有行业。如果您有兴趣管理整个集群舰队的北极星，跨团队合作，或随着时间的推移跟踪调查结果，请查看我们的持续 Kubernetes 安全监控和治理平台 [Fairwinds Insights](https://www.fairwinds.com/insights) 。

我们也希望北极星用户加入我们新的 [Fairwinds 开源软件用户组](https://www.fairwinds.com/open-source-software-user-group)。我们对您对 Polaris 的贡献感到非常兴奋，并共同努力验证和实施 Kubernetes 部署。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)