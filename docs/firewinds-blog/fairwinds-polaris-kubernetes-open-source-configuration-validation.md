# Fairwinds 的北极星是什么？Kubernetes 开源配置验证

> 原文：<https://www.fairwinds.com/blog/fairwinds-polaris-kubernetes-open-source-configuration-validation>

 Kubernetes 是一个非常强大的软件部署平台。它提供的灵活性水平几乎可以适应任何用例，无论多么独特。这就是 Kubernetes 被超过半数的财富 500 强企业采用的原因。根据 Dimensional Research 和 VMware 的一项研究，“Kubernetes 2020 年状态报告”K8s 的采用率从 2018 年的 27%大幅飙升至 2020 年的 48%。

但是和所有工具一样，在动力和安全之间有一个自然的权衡。有数百万种方法可以配置 Kubernetes 及其运行的工作负载，但其中 99%都是危险的。很容易引入安全性、效率或可靠性方面的问题——通常只是因为忘记在 YAML 配置中指定特定字段。

为了解决这个问题，社区提出了一套配置 Kubernetes 工作负载的 Kubernetes 最佳实践。除非你有一个非常好的理由不这样做，否则这些都是应该一直遵循的指导方针。Fairwinds 的 Polaris 项目就是为了帮助定义和实施这些最佳实践而诞生的。

## 一个例子

这里有一个 Kubernetes 部署的例子，直接取自[北极星文档](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/):

```
YAML
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

你能看出它有什么毛病吗？可能不会，除非你非常熟悉 Kubernetes 的配置。但是还有几个字段没有指定，可能会导致严重的问题。

## CPU 和内存设置

首先，告诉 Kubernetes 您的应用程序预计会使用多少内存和 CPU 是很重要的。这使得 Kubernetes 可以有效地将您的工作负载打包到运行它们的底层节点上，并在应用程序出现问题时(例如，由于内存泄漏)为 it 部门提供指导。

一个更好的容器规范应该是这样的:

```
YAML
      containers:
      - name: nginx
        image: nginx:1.14.2
        resources:
          requests:
            memory: 512MB
            cpu: 500m
          limits:
            memory: 1GB
            cpu: 1000m
```

## 健康探针

上面的例子还缺少活性和就绪性探测。这些设置告诉 Kubernetes 如何检查您的应用程序是否正常，是否准备好为流量服务。如果没有活跃度探测器，Kubernetes 将无法在应用程序死机时进行自我修复；如果没有准备就绪探测器，它可能会将流量导向尚未完全准备就绪的 pod。

活性和就绪性探测需要一些特定于应用程序的知识，但通常会轮询特定的 HTTP 端点或运行 Unix 命令来测试应用程序是否正确响应。例如:

```
YAML
      containers:
        livenessProbe:
          exec:
            command:
            - cat
            - /tmp/healthy
        readinessProbe:
          httpGet:
            path: /healthz
            port: 8080
```

## 安全措施加强

许多 Kubernetes 的工作负载设置是“默认不安全的”——它们错误地授予应用程序权限来做它可能需要或可能不需要的事情。例如，默认情况下，每个容器都会安装一个可写的根文件系统，这使得攻击者能够替换系统二进制文件或修改配置。

更安全的容器配置如下所示:

```
YAML
      containers:
      - name: nginx
        image: nginx:1.14.2
        securityContext:
          allowPrivilegeEscalation: false
          privileged: false
          readOnlyRootFilesystem: true
          runAsNonRoot: true
          capabilities:
            drop:
              - ALL
```

## 北极星如何提供帮助

北极星检查所有上述问题和更多。它带有 24 个内置检查(截至 2021 年 5 月)。随着用户提交反馈以及社区学习新的和更好的配置工作负载的方法，检查不断被添加到我们的库中。

我们的每个检查都是在 JSON Schema 中定义的——每次运行`kubectl`来验证添加到集群中的资源时，Kubernetes 都使用相同的模式语言。

最简单的检查只需要几行配置:

```
YAML
successMessage: Host network is not configured
failureMessage: Host network should not be configured
category: Security
target: Pod
schema:
  '$schema': http://json-schema.org/draft-07/schema
  type: object
  properties:
    hostNetwork:
      not:
        const: true
```

但是我们也可以利用 JSON Schema 和 Go 模板的全部功能来创建一些非常复杂的检查。您可以查看一下[Polaris 文档](https://polaris.docs.fairwinds.com/customization/custom-checks/)以了解更多关于如何编写您自己的自定义 Polaris 检查的信息，如果您的组织有自己的内部政策和想要实施的最佳实践，这将非常有帮助。

一旦设置好 Polaris 配置(或者您对我们提供的默认配置感到满意)，Polaris 可以在三种不同的模式下运行:作为仪表板，向您显示集群中哪些资源需要关注；作为准入控制器，阻止有问题的资源进入集群；或者在 CI/CD 中，在签入之前检查基础结构代码。

## 自信地部署

Kubernetes 是一个非常强大的平台，但是伴随着强大的能力而来的是巨大的责任。在部署到 Kubernetes 时，确保遵循最佳实践是非常重要的。如果您忽视验证您的配置，可能会导致安全漏洞、生产中断或云成本超支。

将 Polaris 添加到您的工作流程中——无论是在 CI/CD、准入控制，还是只是一个被动的仪表板——都可以帮助您充满信心地在这些危险的水域中航行。如果你想在一系列集群中使用 Polaris，或者将它与其他一些优秀的 Kubernetes 审计工具结合使用，例如用于扫描集装箱图像的 [Trivy](https://github.com/aquasecurity/trivy) 和用于调整内存大小和 CPU 设置的 [Goldilocks](https://github.com/FairwindsOps/goldilocks/) ，请查看用于在 Kubernetes 环境中审计和执行策略的平台 [Fairwinds Insights](/insights) 。

> Fairwinds Insights 可供免费使用。你可以[在这里](/coming-soon)报名。

因此，无论您是经验丰富的 Kubernetes 专家，还是刚刚开始构建您的第一个集群，请确保您已经准备好了一些防护栏！Fairwinds 的北极星和 T2 的洞察力是一个很好的起点。

[![Fairwinds Insights is Available for Free Sign Up Now](img/90e93a941f22f2087c3a229a91ea6c10.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/d329e036-9905-4715-85b8-31a98b50623c)