# fair winds Polaris 1.0-Kubernetes 工作负载的最佳实践

> 原文：<https://www.fairwinds.com/blog/fairwinds-polaris-1.0-best-practices-for-kubernetes-workloads>

 ![party popper](img/ce3e820a2c7fd287c8d291b4b3532c9e.png "party popper")  我很兴奋地宣布，我们已经发布了北极星 1.0！

We launched [Polaris](https://www.fairwinds.com/polaris) almost a year ago to help Kubernetes users avoid common mistakes when configuring their workloads. Over the course of managing hundreds of clusters for dozens of organizations, the Fairwinds SRE team kept seeing the same mistakes over and over: resource requests and limits going unset, liveness and readiness probes being ignored, and containers requesting completely unnecessary security permissions. These are sure-fire ways to create headaches down the line – from outages, to cost overruns, to security breaches. We saw Polaris as a way to encode all our battle scars into a single configuration validator that could benefit the entire Kubernetes community.

Polaris 最初是一个简单的仪表板，显示集群中所有未达到最佳实践的工作负载。但是我们很快为真正的偏执狂添加了一个验证 Webhook 这将拒绝ku bectl apply任何未通过危险级别检查的工作负载。但是我们的用户仍然不满意——他们想在 CI/CD 中运行 Polaris，这样他们就可以在错误被合并到 master 之前捕获它们。很快北极星也可以运行在 YAML 文件和舵图表。

现在，在 GitHub 上获得了超过 1200 颗星以及来自社区的大量反馈之后，我们已经了解了很多，我们很高兴地宣布 1.0 中的一些令人惊叹的新功能:

*   使用 JSON 模式的自定义检查
*   支持所有控制器类型，包括 OpenShift 和自定义资源
*   为确实需要特殊权限的工作负载检查豁免
*   简化配置和输出格式

## 使用 JSON 模式进行自定义检查

这是迄今为止对北极星最大和最现有的改变:你现在可以使用 JSON 模式定义你自己的定制检查。

最初，我们在 Golang 中对支票进行了硬编码。我们将手动检查如下内容:

```
if cv.Container.LivenessProbe == nil {
    cv.addFailure(
        messages.LivenessProbeFailure,
        conf.HealthChecks.LivenessProbeMissing,
        category,
        id)
} else {
    cv.addSuccess(messages.LivenessProbeSuccess, category, id)
} 
```

从长远来看，这被证明有点麻烦且容易出错。例如，我们最终意识到 Jobs 和 CronJobs 可能应该免于活性探测检查，这涉及到在一些额外的条件中包装上述语句。随着我们发现更多这样的异常，我们的 Go 代码变得相当笨拙。

更重要的是，我们的用户没有简单的方法来添加他们自己的检查——他们需要将它们添加到我们的代码库中，并且不是每个检查都适合每个组织。

So we decided to move towards using a configuration language for checks. After heavily investigating OPA  (more on that below), we decided to go with [JSON Schema](https://json-schema.org/). Now, the check above, plus all of its exceptions and configuration, looks something like this:

```
successMessage: Liveness probe is configured
failureMessage: Liveness probe should be configured
category: Health Checks
controllers:
  exclude:
  - Job
  - CronJob
containers:
  exclude:
  - initContainer
target: Container
schema:
  '$schema': http://json-schema.org/draft-07/schema
  type: object
  required:
  - livenessProbe
  properties:
    livenessProbe:
      type: object
      not:
        const: null 
```

现在，与特定检查相关的所有配置都位于同一个位置。我们可以调整它应用的控制器、它生成的消息，甚至检查本身的逻辑，只需编辑一些 YAML。

JSON Schema is incredibly powerful - you can force numbers to fall between maxima and minima, force strings and object keys to match particular patterns, set bounds on array lengths, and even combine schemas using allOf, oneOf, or anyOf. We’ve even extended JSON Schema to let you set minima and maxima for CPU and Memory using human readable strings, like 1GB. To help you get started, we’ve [provided some sample schemas](https://github.com/FairwindsOps/polaris/blob/master/examples/config-full.yaml#L36) for things like restricting memory usage and disallowing particular docker registries.

## 但是等等，OPA 怎么办？

Kubernetes veterans will probably be wondering why we didn’t go with [OPA](https://www.openpolicyagent.org/docs/latest/kubernetes-tutorial/), a heavyweight validation configuration mechanism that has been developed by the community. We looked at the project carefully, but decided ultimately that it was much more complex and powerful than we  (or our users) needed. It takes some time and effort to get used to [Rego](https://www.openpolicyagent.org/docs/latest/policy-language/), the DSL that drives OPA policy. JSON Schema is also already an integral part of Kubernetes - it’s used by CRDs and core resources for validation, and is a part of Open API, which drives the Kubernetes API.

## 新的控制器类型

When we originally built Polaris, it only checked [Deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/), arguably the most common controller type in Kubernetes. But soon after launching we had requests for more controller types, like StatefulSets, CronJobs, and DaemonSets. After adding support for half a dozen different controllers  (as well as writing a large amount of boilerplate code, thanks to Go’s rigid type system), we realized we’d never be able to keep up with all the controllers out there - in addition to the core Kubernetes controllers, there are non-standard controllers like OpenShift’s [DeploymentConfig](https://docs.openshift.com/container-platform/4.1/applications/deployments/what-deployments-are.html).So we decided to try a different tack - instead of fetching controllers, we’d fetch the underlying pods that the controllers create. We could then use [Owner References](https://kubernetes.io/docs/concepts/workloads/controllers/garbage-collection/#owners-and-dependents) to walk up the hierarchy until we hit something without an owner - i.e. the parent controller.

(作为一个有趣的题外话，我们了解到一些工作负载并不属于一个控制器，而是属于ode 本身！在 1.0 仪表板中，您会注意到 `kube-system` 中的一些几乎重复的条目。)

With this change in place, we’re able to support any controllers out there in the wild, whether or not we’d even heard of them before. So go ahead and build your own controllers! We’ll still bug you about your liveness probes. ![grinning face with smiling eyes](img/b598d2653db391fbf9d75c3810523e48.png "grinning face with smiling eyes") 

然而，请注意，我们还不能在验证 Webhook 中捕获这些新的控制器类型——web hook 仍然监视一组固定的控制器类型。我们可能会对此提供支持，但需要注意的是，我们仍然允许工作负载适当地扩展。

## 检查豁免

有些工作负载确实需要以 root 用户身份运行，或者访问主机网络。对于很多存在于kube-system中的东西来说都是如此，还有一些实用程序比如cert-manager和nginx-ingress。

To help cut down on the noise generated by these workloads, we’ve created a [library of exemptions](https://github.com/FairwindsOps/polaris/blob/master/examples/config.yaml#L28), which will allow particular workloads to ignore particular checks. You can add your own workloads to this configuration, or you can [add annotations](https://github.com/FairwindsOps/polaris/blob/master/docs/usage.md#exemptions) like `polaris.fairwinds.com/exempt=true` and `polaris.fairwinds.com/cpuRequestsMissing-exempt=true.`

我们希望继续构建这个库，所以如果你运行的是 Istio 或 Linkerd 等常用工具，

## 新的配置和输出格式

我们的配置语法最初看起来是这样的:

```
 networking:
  hostNetworkSet: warning
  hostPortSet: warning
security:
  runAsPrivileged: error
  capabilities:
    error:
      ifAnyAdded:
        - SYS_ADMIN
        - NET_ADMIN
        - ALL
    warning:
      ifAnyAddedBeyond:
        - CHOWN
        - DAC_OVERRIDE
        - FSETID 
```

困难在于有些检查只是布尔型的，比如 `hostNetworkSet,` ，而其他的，比如`capabilities,`需要一些额外的配置。因此，经过大量的内部讨论后，我们决定采用一种有些不一致但很简单的语法。

在 1.0 中，我们已经去掉了所有额外的配置，转而支持 JSON 模式 (见上面的 )。所以现在语法简单多了:

```
checks:
  # networking
  hostNetworkSet: warning
  hostPortSet: warning
  # security
  dangerousCapabilities: danger
  insecureCapabilities: warning
```

请注意，我们也改变了

`error`

到

`danger`

区分意外错误和检查失败。虽然输出格式也简化了很多，但它并不完全面向用户，所以我们在这里不做深入讨论。但是如果你有兴趣看的话，

[here’s an example](https://github.com/FairwindsOps/polaris/blob/master/examples/output.json).

## 继续到 2.0

So what’s next for Polaris?

首先，我们想增加更多的支票。现在我们已经有了一个可扩展的方法来构建它们，并且社区也有了一个简单的方法来贡献新的，我们想要构建一个大的检查库，用户可以打开和关闭它。我们希望开始检查不仅仅是工作负载——像入口、服务和 RBAC 这样的东西也需要验证。

我们可能还会添加对 OPA 的支持。虽然 JSON Schema 仍将是 Polaris 的首选格式，但 OPA 将吸引那些已经投入时间创建减压阀策略的组织，或者那些想要编写复杂支票的组织。添加 OPA 支持将使 Polaris 成为一个高度灵活的验证框架。

Finally, we will look to build a deeper integration with [Fairwinds Insights](https://www.fairwinds.com/insights), our commercial configuration validation platform. Fairwinds Insights allows you to aggregate Polaris results across multiple clusters, track the lifecycle of findings over time, and push the data out to third-parties like Slack and Datadog. Fairwinds Insights also has plugins for other open-source auditing tools, like [Trivy](https://github.com/aquasecurity/trivy), [kube-bench](https://github.com/aquasecurity/kube-bench/), and [Goldilocks](https://github.com/FairwindsOps/goldilocks/), so you can be confident you’re covered all the bases from security to efficiency to reliability.If you have some time to [try out Polaris](https://github.com/FairwindsOps/polaris), or if you’re already using it, we’d love to hear from you! Reach out on [GitHub](https://github.com/FairwindsOps/polaris) and [Slack](https://join.slack.com/t/fairwindscommunity/shared_invite/zt-e3c6vj4l-3lIH6dvKqzWII5fSSFDi1g), or send an email to opensource@fairwinds.com.