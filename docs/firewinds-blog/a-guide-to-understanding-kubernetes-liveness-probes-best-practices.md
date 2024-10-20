# 理解 Kubernetes 活性探针最佳实践的指南

> 原文：<https://www.fairwinds.com/blog/a-guide-to-understanding-kubernetes-liveness-probes-best-practices>

 Kubernetes 最初发布于 2014 年，是一个开源的容器编排系统，在 Apache License 2.0 下发布，用编程语言 Go 编写。谷歌最初创建了它，但今天它由 [云本地计算基金会](https://www.cncf.io/) (CNCF)维护。Kubernetes 提供了令人难以置信的灵活性，允许组织轻松地部署、扩展和管理生产级的容器化工作负载。这种灵活性伴随着巨大的复杂性和许多团队难以理解的不熟悉的术语和技术。活性探测器和[就绪探测器 是在 Kubernetes 上部署成熟应用程序时应该知道的两个术语。在本指南中，我们将讨论在 Kubernetes 集群中运行活性探测，作为一种健康检查来确定容器是否仍在运行和响应。](https://www.fairwinds.com/blog/kubernetes-readiness-probe-best-practices)

## **探针在库伯内特中的重要性**

您希望您的应用程序是可靠的，但是有时...他们不是。它们可能由于配置错误、应用程序错误或临时连接问题而失败。虽然应用程序变得不可靠的原因很重要，但知道问题已经发生或正在发生同样重要。探测器可以通过监控应用程序的问题来帮助开发人员排除故障，还可以通过指示应用程序何时出现资源争用来帮助他们规划和 [管理资源](https://www.fairwinds.com/blog/how-to-correctly-set-resource-requests-and-limits) 。

探测是监视应用程序运行状况的定期检查，通常使用命令行客户端或 YAML 部署模板进行配置。开发人员可以使用任何一种方法来配置探测器。K8s 中有三种类型的探头:

*   **启动探测器:** 这些探测器验证容器内的应用程序是否已经启动。它仅在启动时执行，如果一个容器未能通过此探测，该容器将被终止，并按照 `restartPolicy` 进行处理。您可以在 pod 配置的 `spec.containers.startupProbe` 属性中配置启动探测器。启动探测器的一个主要动机是，一些遗留应用程序在首次初始化时需要额外的启动时间，这使得设置活跃度探测器参数变得棘手。当配置一个 `startupProbe`， 使用与应用相同的协议，并确保 `failureThreshold * periodSeconds` 足以覆盖最坏的启动时间。

*   **准备就绪探测器:** 这些探测器持续验证 Docker 容器是否准备好服务请求。如果探测器返回失败状态，Kubernetes 会从所有服务的端点中删除 pod 的 IP 地址。准备就绪探测器使开发人员能够指示 Kubernetes，在其他任务完成之前，正在运行的容器不应该接收流量，例如加载文件、预热缓存和建立网络连接。您可以在 pod 配置的 `spec.containers.readinessProbe` 属性中配置就绪探测器。这些探测器按照 `periodSeconds` 属性的定义定期运行。

*   **活性探测器:** 这些探测器帮助您评估在容器中运行的应用程序是否处于健康状态。如果没有，Kubernetes 会终止容器并尝试重新部署它。当您想要确保您的应用程序没有死锁或无声无响应时，这些是有用的。您可以在 pod 配置的 `spec.containers.livenessProbe`代码> 属性中配置活性探针。像准备就绪探测器一样，活性探测器也定期运行。我们将在下面查看它们的详细信息和配置选项。

## **Kubernetes 中有哪些活性探针？**

根据设计，Kubernetes 在 pod 的整个生命周期中自动监控它们，当它检测到进程 ID 1 **(** pid 1)，即负责启动和关闭系统的 init 进程出现故障时，就会重新启动它们。当您的应用程序崩溃时，这非常有用，因为 Kubernetes 会终止它的进程并发出一个非零的访问代码。不幸的是，并非所有的应用程序都是一样的，Kubernetes 也不总是能检测到故障。例如，如果您的应用程序失去了其数据库连接，或者如果您的应用程序在与第三方服务连接时遇到超时，它可能无法自行恢复。在这种情况下，pod 在[kube let](https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/#:~:text=The%20kubelet%20is%20the%20primary,object%20that%20describes%20a%20pod.)看来可以按预期运行，但最终用户将无法访问应用程序。

这些类型的问题可能很难调试，因为在容器级别，一切都按预期运行。活性探测器解决了这个问题，因为它们将关于 pod 内部状态的信息传递给 Kubernetes，这意味着您的集群将处理这个问题，而不需要人工监控和干预。活性探测减轻了您的维护负担，并确保您的应用程序不会无声无息地失败。

## **活性探针的类型**

下面是 Kubernetes 中可用的活跃度探测器类型的详细信息。选择与您的应用程序架构最接近并准确公开您的应用程序内部状态的活跃度探测器类型，对于成功地将工作负载部署到 Kubernetes 至关重要。

**1。命令执行活跃度探测器:** 该探测器在容器内部运行命令或脚本。如果命令以 0 作为退出代码终止，这意味着容器正在按预期运行。

**2。HTTP GET 活跃度探测器:** 该探测器向容器中的 URL 发送 HTTP GET 请求。如果容器的响应包含 200-399 范围内的 HTTP 状态代码，则意味着探测成功。  您可以在 httpGet 上为您的 HTTP 探测器设置这些附加字段:

*   *   *主机:* 要连接的主机名称。默认为 pod IP 相反，您可能希望在 httpHeaders 中设置“Host”。

    *   *方案:* 用于连接主机(HTTP 或 HTTPS)的方案；默认为 HTTP。

    *   *路径:*HTTP 服务器上访问的路径；默认为/。

    *   *httpHeaders:* 您可以在请求中设置的自定义标头；HTTP 允许重复的头。

    *   *港口:* 集装箱上要访问的港口的名称或编号；该数字必须在 1 到 65535 的范围内。

**3。TCP 套接字活跃度探测器:** 该探测器试图连接到容器内部的特定 TCP 端口。如果指定的端口打开，则认为探测成功。

**4。gRPC:** 使用 gRPC 的应用程序可以使用 [gRCP 健康检查探测器](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-a-grpc-liveness-probe) 。这种类型的探测器从 Kubernetes v1.23 开始就可用了。如果实现了 gRPC 健康检查协议，您可以配置 kubelet 使用它进行应用程序活性检查。您需要启用`GRPCContainerProbe`[特征门](https://kubernetes.io/docs/reference/command-line-tools-reference/feature-gates/) 来配置依赖 gRPC 的检查。您必须将端口配置为使用 gRPC 探测，如果健康端点配置在非默认服务上，您还需要指定该服务。

## **在 Kubernetes 中配置活跃度探测器**

在生产环境中，将活跃度探测作为应用程序部署模板的一部分被认为是最佳实践。这样，您可以在类似的应用程序中模板化和重用您的活跃度探测器配置。

开始时，最好在与您计划在生产中使用的应用程序类似的应用程序中部署旨在测试和演示您的活跃度探测配置的应用程序。为了说明这一点，下面我们将从 registry.k8s.io/busybox 的[](http://registry.k8s.io/busybox)**中选取一个示例图像，并使用命令执行方法部署一个带有活跃度探测器的静态 pod。**

 **### **活性探针配置的示例**

在您的集群中应用这个 yaml 将部署一个示例 pod，它将在前 40 秒内成功，然后故意进入一个失败状态，在这个状态下，活性探测将失败，kubelet 将重新启动容器以恢复服务。

T2`apiVersion: v1`

T2`kind: Pod`

T2`metadata:`

T2`labels:`

T2`test: liveness`

T2`name: liveness-example`

T2`spec:`

T2`containers:`

T2`- name: liveness`

T2`image: registry.k8s.io/busybox`

T2`args:`

T2`- /bin/sh`

T2`- -c`

T2`- touch /tmp/healthz; sleep 40; rm -f /tmp/healthz; sleep 700`

T2`livenessProbe:`

T2`exec:`

T2`command:`

T2`- cat`

T2`- /tmp/healthz`

`initialDelaySeconds: 6`

T2`periodSeconds: 6`

### **了解活跃度探头设置**

在上例中，Pod 只有一个容器。livenessProbe 属性下的字段和命令指定您希望 kubelet 如何执行健康检查:

*   **initialDelaySeconds:**该字段指示 kubelet 在执行第一次探测之前等待的秒数；默认值为 0 秒，最小值为 0。在不影响应用程序性能的情况下，该值应设置得尽可能低。

*   **period seconds:**该字段指定 kubelet 执行活跃度探测的频率，以秒为单位；默认值为 10 秒，最小值为 1。

*   **超时秒数** :探头超时的秒数；默认值为 1 秒，最小值为 1。

*   **成功阈值:** 在一次探测失败后，这是一次探测被认为成功所需的最小连续成功次数；默认值为 1，最小值为 1，对于活跃度和启动探测，该值必须为 1。

*   **失败阈值:** 如果至少有这个数量的探测失败，Kubernetes 确定应用程序不正常，并触发该容器的重启(启动或活动探测)。kubelet 使用该容器的设置 `terminationGracePeriodSeconds` 作为该阈值的一部分。

*   **terminationgraceperiodes:**在触发关闭失败的容器和强制容器运行时停止该容器之间，kubelet 必须等待的时间。默认情况下，它继承 `terminationGracePeriodSeconds` 的 Pod 级别值。如果未指定，默认值为 30 秒，最小值为 1。

*   **/tmp/healthy:**kube let 执行命令 `cat /tmp/healthz` 在目标容器中执行探测。在容器生命周期的前 40 秒，该命令返回一个成功代码。之后，它返回一个失败代码。

对于 HTTP 和 TCP 探测，可以使用命名端口。一个例子是`port: http.`注意 gRPC 探测器不支持命名端口或定制主机。

## **在 Kubernetes** 中使用活性探测器的最佳实践

成功的活动探测不会影响集群的健康状况。被探测的容器保持运行，并且在`periodSeconds`延迟之后安排新的探测。但是，如果探针运行过于频繁，会浪费您的 [资源](https://www.fairwinds.com/blog/kubernetes-resource-usage-estimate-workload-cost-with-goldilocks-open-source) ，还会对应用程序性能产生负面影响。另一方面，如果您的探测不够频繁，您的容器可能会在被处理之前长时间处于不健康的状态。

使用上面概述的字段和命令，根据您的应用对探头进行微调。一旦您知道您的活性探测器的命令、API 请求或 gRPC 调用需要多长时间才能完成，您就可以在您的`timeoutSeconds`中使用这些值(同时，考虑添加一个小的缓冲期)。对于简单、短期运行的探测，尽可能使用最小值。密集或长时间运行的命令可能要求您在重复之前等待更长时间，因此您将无法获得容器运行状况的最新视图。

还要确保探测命令或 HTTP 请求的目标独立于您的主应用程序。这确保了即使您的主应用程序失败了，它也可以向 kubelet 报告它的状态。如果您的活跃度探测器由标准应用程序入口点提供服务，那么如果它的框架失败或者它请求了一个不可用的外部依赖项，它可能会导致不准确的结果。

### **其他几个活跃度探针最佳实践:**

1.  **探测器受重启策略影响:** 探测器后应用容器重启策略。您的容器必须设置为 `restartPolicy: Always` (这是默认设置)或 `restartPolicy: OnFailure` ，以确保 Kubernetes 可以在活性探测失败后重新启动容器。如果使用 Never 策略，则在活动探测失败后，容器将无限期地保持失败状态。

2.  **每个容器都不需要探测器:** 可以省略那些总是在失败和低优先级服务时终止的简单容器。

1.  不在 pid 1 上运行的作业是一个很好的 pod 受益于活性探测的例子。如果作业失败，我们希望它重新启动，以确保最近的运行是成功的，并且没有正在静默构建的工作队列。

4.  **重新评估你的探头:** 不要设置了就忘了。应用中的新优化、特性和回归会影响探头性能。确保定期检查探头，并根据需要进行调整。最精确的探测来自于从真实世界的度量中得到的设置。部署后重新访问您的探测器允许您根据历史性能调整您的探测器。

## **在 Kubernetes** 中使用活性探针的优势

在 Kube 中使用活跃度探测器可以帮助您提高应用程序的可用性，因为它们可以让您持续了解容器中应用程序的健康状况。在某些方面，Kubernetes 可能会造成断开，因为虽然你的 pod 可能看起来很健康，但你的用户可能实际上无法访问你的应用和服务。活性探测帮助您验证您的应用程序、容器和 pod 是否都按照设计运行，并确保 K8s 在容器变得不健康时重新启动容器。

您可以使用开源工具，如 [【北极星】](https://www.fairwinds.com/blog/k8s-tutorial-policy-engine-polaris-to-automate-fixes) ，应用自动化来审计和修订 YAML 清单中发现的任何问题。在活性探针和就绪探针的情况下，Polaris 可能会留下注释，以提示用户根据其应用程序的上下文做出适当的更改。下面是我在 [上演示一些基本示例的视频，使用](https://training.fairwinds.com/action-item-setting-liveness-probes)[fair winds Insights](https://www.fairwinds.com/insights)跨集群设置活性探测器 以确保可靠性，您可以使用它开始学习。

不同类型的健康检查可以帮助您在 Kubernetes 中执行活性检查和就绪检查。活性探测、就绪探测和启动探测都可以帮助您确保您的 Kubernetes 服务建立在良好的基础上，以便您的 DevOps 团队可以为您的应用和服务提供更好的可靠性和更长的正常运行时间。如果你开始有困难，在 Kubernetes 网站 上查看这个 [教程。](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)

### **Fairwinds Insights 使您的开发人员能够快速识别潜在问题，并主动预防 Kubernetes 中的停机或中断。** [**今天就试试免费等级吧！**](https://insights.fairwinds.com/auth/register/)

**[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)****