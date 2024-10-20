# 使用开放策略代理|工具创建策略强制管道

> 原文：<https://www.harness.io/blog/policy-enforced-pipeline-opa>

在这篇博文中，我们将展示如何使用 OPA 创建一个在 Harness 中构建的策略强制 CI/CD 管道。

在之前的博客文章中，我们分享了一些关于作为治理和风险降低手段的[策略代码](https://harness.io/blog/policy-as-code/)的高级概念。我们还讨论了作为策略引擎的开放策略代理(OPA ),它强制执行作为代码创建的策略。在这篇博文中，我们将展示如何使用 OPA 创建一个在 Harness 中构建的策略强制的 CI/CD 管道。

## 开放式策略代理入门

要开始创建策略强制的管道，您需要将 OPA 安装到您的环境中。在本指南中，我们将 OPA 直接安装到 Kubernetes 集群上。开放策略代理的本教程描述了如何在 Kubernetes 中将 OPA 部署为准入控制器。

注意:您还可以创建一个利用工作流或管道来部署 OPA 和应用预定义的策略。

## 在 Kubernetes 上创建 OPA 策略

在减压阀制定政策相当简单。在验证和创建策略时，您还可以利用开发人员工具来获得更好的开发体验。下面是一个示例策略，它拒绝 Kubernetes 部署任何以名称 Nginx 开头的容器映像。我们想要控制我们的部署，所以特定的图像被拒绝进入 Kubernetes 环境。

包 kubernetes . admission
deny[msg]{
input . request . kind . kind = = " Deployment "
image:= input . request . object . spec . template . spec . containers[I]。image
startswith(image，" nginx")
msg := sprintf(" "无法部署映像%v "，[image])
}

将代码片段复制到名为“deploy-restrict.rego”的文件中。将 Kubernetes 中的这个策略作为 ConfigMap 存储到运行 OPA 的同一名称空间中。

ku bectl 创建配置映射 deploy-restrict-from-file = deploy-restrict。rego-n opa

默认情况下，kube-mgmt 从 OPA 名称空间中的配置映射或标记为 openpolicyagent.org/policy=rego.的其他名称空间中的配置映射加载策略

接下来，我们可以通过尝试部署一个简单的 Nginx 映像来实践这个策略。这里我有一个包含 nginx-deployment 的 deployment.yaml 文件:

apiVersion: apps/v1
种类:部署
元数据:
名称:Nginx-deployment
标签:
app: nginx
规格:
副本:3 个
选择器:
matchLabels:
app:Nginx
模板:
元数据:
标签:
app: nginx
规格:
容器:
-名称:nginx
图片:Nginx

如下所示，当我们尝试在默认名称空间中创建部署时，策略拒绝该操作并返回格式化的策略消息。

为 police Kubernetes 资源创建附加策略可以帮助您保护特定的环境，如 QA 或生产环境。

## 在线创建策略强制的管道

现在我们有了一个在 Kubernetes 集群中有效的策略，我们可以确保我们的部署管道遵守这些策略。

我有一个简单的 Harness 管道，其中包含一些要部署到开发名称空间的工作流。我的管道还将就绪容器映像提升到 QA 和生产。请注意，在我们的策略中，我们不希望集群中有任何以名称 Nginx 开头的映像。

如下所示，我们的整个部署管道都失败了，因为它没有根据我们定义的策略继续进行的授权。

我们可以创建更复杂的策略，同时利用我们的管道来应用、测试、验证和触发这些策略。如果您想了解更多关于策略即代码的常见用例，请查看这个[策略作为代码介绍](https://harness.io/blog/policy-as-code/)。否则，我希望这篇博文有助于展示 Harness 和 OPA 的能力。为不仅快速而且安全地交货干杯。

## 结论

随着组织继续更好地治理他们的过程和软件，作为代码的策略将继续是重要的。创建策略并将它们与 CI/CD 管道集成在一起有许多用例。

有兴趣深入了解治理吗？我们有一本很棒的电子书，讲述了在 CI/CD 中[治理的重要性。](https://harness.io/learn/ebooks/ebook-applying-governance/)