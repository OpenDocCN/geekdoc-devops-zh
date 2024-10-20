# 北极星突变:它是如何工作的

> 原文：<https://www.fairwinds.com/blog/how-polaris-kubernetes-mutations-work>

 在过去的三年里， [Polaris](https://polaris.docs.fairwinds.com/) 已经允许 Kubernetes 用户审计他们的集群和基础设施作为最佳实践的代码。它带有 20 多个内置检查，并支持使用 [JSON 模式](https://json-schema.org/)的定制检查。它可以查看正在运行的集群内部以查找有问题的资源，作为准入控制器运行以阻止具有高严重性问题的资源，或者作为 CI/CD 过程的一部分运行以扫描基础架构代码。

但是解决北极星发现的问题可能会很乏味。您必须查找修改需要更改的特定属性的语法，并编辑每个需要改进的基础设施代码文件。如果您的代码分布在许多存储库或团队中，这尤其困难。新的 YAML 文件一直在创建，这意味着无论是谁负责实施最佳实践，都会有源源不断的工作要做。

为了帮助解决这个问题，我们在北极星中建立了一个突变的概念。突变精确地指定了需要对 YAML 的一部分做什么来使它符合最佳实践。变异可以在基础设施代码文件上运行，因此可以将更改签入到您的存储库中，或者可以作为变异的 Webhook 运行，在资源进入您的 Kubernetes 集群时对其进行修改。了解[实际应用](/blog/how-polaris-kubernetes-mutations-work)。

## 突变语法

Polaris 利用 JSON 模式来决定特定资源是否符合最佳实践。例如，确保未配置 hostIPC 的检查如下所示:

```
successMessage: Host IPC is not configured
failureMessage: Host IPC should not be configured
category: Security
target: PodSpec
schema:
  '$schema': http://json-schema.org/draft-07/schema
  type: object
  properties:
    hostIPC:
      not:
        const: true
```

要解决 hostIPC 的问题，我们只需从 YAML 中移除该字段。因此，我们可以在上面的 YAML 中添加一个突变模块，如下所示:

```
mutations:
  - op: remove
    path: /hostIPC
```

这里的语法是 [JSON 补丁](https://jsonpatch.com/)，一个用于修改 JSON(或 YAML)数据的 IETF 标准。

值得注意的是，并不是每一个突变都是现成的。例如，活性和就绪性探测需要根据您的应用程序进行定制。但是 Polaris 至少可以做出猜测，让您更容易填写它们，而不必翻遍 Kubernetes 文档。为了明确这一点，Polaris 可以添加一个注释，提示用户进行进一步的修改。下面是向活性探测检查添加注释的语法:

```
mutations:
  - op: add
    path: /livenessProbe
    value: {"exec": { "command": [ "cat", "/tmp/healthy" ] }, "initialDelaySeconds": 5, "periodSeconds": 5 }
comments:
  - find: "livenessProbe:"
    comment: "TODO: Change livenessProbe setting to reflect your health endpoints"
```

所有这些语法都可以在 Polaris 自定义检查中找到。因此，如果您已经构建了一个自定义检查库，或者正计划创建一些库，那么您可以在创建时添加一些变化！

## 在基础设施即代码上运行突变

polaris fix 命令将允许您修改基础设施代码文件，以便它们遵循最佳实践。

例如，如果我们从这个最小配置开始，直接从 Kubernetes 文档复制:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

我们可以运行北极星修复文件路径。/deploy . YAML-checks = runasroot allowed获取:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
        securityContext:
          runAsNonRoot: true 
```

请注意，该配置现在有了一个 securityContext ，并被设置为以非 root 身份运行。

或者，如果我们想看看*Polaris 能用这个文件做的一切，我们可以运行 polaris fix - files-path。/deploy.yaml - checks=all 获取:*

```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx:1.14.2
        imagePullPolicy: Always
        livenessProbe: #TODO: Change livenessProbe setting to reflect your health endpoints
          exec:
            command:
            - cat
            - /tmp/healthy
          initialDelaySeconds: 5
          periodSeconds: 5
        name: nginx
        ports:
        - containerPort: 80
        readinessProbe: #TODO: Change livenessProbe setting to reflect your health endpoints
          exec:
            command:
            - cat
            - /tmp/healthy
          initialDelaySeconds: 5
          periodSeconds: 5
        resources:
          limits:
            cpu: 100m #TODO: Set this to the amount of CPU you want to reserve for your workload
            memory: 512Mi #TODO: Set this to the amount of Memory you want to reserve for your workload
          requests:
            cpu: 100m #TODO: Set this to the amount of CPU you want to reserve for your workload
            memory: 512Mi #TODO: Set this to the amount of Memory you want to reserve for your workload
        securityContext:
          capabilities:
            drop:
            - ALL
          readOnlyRootFilesystem: true
          runAsNonRoot: true 
```

请注意，北极星加强了安全性，并设置了一些字段，如健康探针和资源设置。如上所述，您可以在一些设置旁边看到注释，这些设置应该由用户进行微调，而不是照原样接受。

## 启用变异 Webhook

更改所有的基础设施代码文件可能会很乏味，即使有 polaris fix 命令帮助填补空白。当创建新的部署文件时尤其如此——很难跟上！

对于一些突变，当对象进入集群时，简单地修改它们更容易。一个很好的例子是imagePullPolicy——Polaris 建议将此设置为 Always ，如果你这样做，你可以确信不会有任何问题。

要启用变异的 Webhook，你可以通过[舵图](https://github.com/FairwindsOps/charts/tree/master/stable/polaris)安装北极星。确保添加以下配置来启用 webhook:

```
webhook:
  enable: true
  mutate: true 
```

默认情况下，对突变启用的唯一检查是 pullPolicyNotAlways 。如果你想启用其他突变，你可以编辑北极星配置的突变部分:

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
  - readinessProbeMissing
  - runAsPrivileged
  - runAsRootAllowed
```

## 未来的改进

我们对在 Polaris 中实现突变的进展感到非常兴奋，但也有一些限制。在接下来的几个月里，我们将努力从几个方面改进这一功能:

*   修改头盔模板的能力:目前北极星修正命令只适用于原始的 YAML 文件，不适用于头盔模板。关于如何让北极星修复在更广泛的环境中工作，我们有一些想法。

*   保留注释和格式:Polaris fix命令反序列化、修改和重新序列化 YAML，一些修饰性的功能在翻译中丢失了。例如，注释被删除，字段顺序可能会改变。我们正在努力改善这种体验。

*   更多突变:目前并不是每个检查都有突变。当前的 JSON 补丁语法有一些限制，例如在修改数组内容时。我们还希望支持多资源检查的突变，例如，如果不存在 PDB，则创建一个。

如果您尝试一下当前的实现，我们很乐意听到您的反馈！你可以在 [Fairwinds 社区 Slack](https://fairwindscommunity.slack.com/) ，我们的[开源用户组](https://www.fairwinds.com/open-source-software-user-group)，或者在 [GitHub](https://github.com/FairwindsOps/polaris) 上找到我们。

## 资源

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)