# Kubernetes CI/CD 最佳实践|线束

> 原文：<https://www.harness.io/blog/kubernetes-ci-cd-best-practices>

拥有 Kubernetes 的所有优势，拥有良好的 CI/CD 实践是关键。Kubernetes 并没有神奇地抹去你在 Kubernetes 之前的 CI/CD 之旅中所接受的训练。利用 Kubernetes 的优势来推进您的 CI/CD 之旅。

容器化和 Kubernetes 为计算世界带来了一个新的一致性范例，允许工程团队提高速度和敏捷性。通用声明性语言提供的描述应用程序和操作任务的融合使 Kubernetes 成为运行分布式工作负载的流行平台。

在声明性的 YAML 中创作期望的状态，一旦应用，Kubernetes 就开始解决和实现已声明的状态；例如应用程序的副本数量。如果有任何偏差，Kubernetes 将努力解决实际状态和声明状态之间的差异；例如 pod/集装箱停止运转并被重新旋转。

对于那些第一次部署到 Kubernetes 的人来说，这种体验可能会非常迅速，而且是反气候的。创作最小部署。YAML，一旦给库贝克下了[申请]的命令，你就可以开始比赛了。当需要做出改变的时候，Kubernetes 将利用它的优势之一，滚动更新，来进行渐进式的改变。如果你已经习惯了手工编写滚动更新规则的平台，那么看着滚动更新的发生会让 Kubernetes 变得轻而易举。

尽管 Kubernetes 有这么多好处，但拥有良好的 [CI/CD 实践](https://harness.io/blog/continuous-delivery/ci-cd-best-practices/)是关键。Kubernetes 并没有神奇地抹去你的 CI/CD 之旅在进入画面之前就已经采取的纪律。利用 Kubernetes 的优势推动您的 CI/CD 之旅。

### CI 和 Kubernetes 的最佳实践

[持续集成(CI)](https://harness.io/blog/continuous-integration/what-is-continuous-integration/) 是构建自动化的过程。例如，一个 JAVA 应用程序需要构建到一个 JAR 中，那么如果要进入 Kubernetes，就需要对其进行文档化，并且可能以一种格式(比如 Helm Chart)进行打包/描述。在容器化的世界里，由于容器是不可变的，任何需要的改变都会产生一个新的映像，因此你的 CI 过程会被调用很多来构建和打包新的映像。

先有鸡还是先有蛋的争论，在 Kubernetes 上运行您的持续集成过程是一个谨慎的举动。构建和打包软件会占用大量计算资源。在现代方法中，每次提交都会启动一个构建，这对于基础设施来说是非常繁重的——尤其是对于容器化的构建。利用 Kubernetes 构建和打包软件是一个很好的用例，因为现代 CI 工具专注于在 Kubernetes 中创建短暂的构建运行器/节点。随着构建请求的到来，启动一个新的实例来创建构建工件，然后在任务完成时关闭该实例。

可以在临时容器中轻松运行的持续集成信任建立步骤是单元测试、集成测试和安全扫描步骤。尤其是图像/容器扫描步骤可能是相当计算密集型的分解和验证 Docker 层，类似于运行计算密集型的构建任务。因为每次构建都可能引入新的依赖项或依赖项的新版本，所以每次构建新映像时运行容器扫描都很重要。

但是，有些物品需要比短暂的容器更持久，并且需要更持久的存储。持续集成的退出步骤是将创建的工件/包发布到工件存储库和/或将清单发布到各自的源代码管理/包管理器解决方案。在 Kubernetes 世界中，这也可以是创建 Kubernetes 需要部署的清单。例如，Helm Charts 或 Kustomize/JSONNET 资源。使用 Kubernetes 的 CI 的一个目标是产生一个易于部署的工件，包/配置/模板管理器允许这样做。

除非 Kubernetes 集群上的工作负载可以使用高可用性/持久性存储，否则将工件存储库作为 SaaS 或者脱离 K8s 集群运行是有意义的。致命的弱点是工件库在设计上是存储密集型的。拥有一个可部署的工件/清单只是将您的想法传递到最终用户手中的等式的一部分；接下来就是部署了。

### CD 和 Kubernetes 的最佳实践

[连续交付(CD)](https://harness.io/blog/continuous-delivery/what-is-continuous-delivery/) 的目标是以安全的方式将您的变更投入生产。Kubernetes 能够非常快速地部署，特别是如果使用[再造战略](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)，所有的吊舱都被杀死并替换，而不是以滚动战略递增；但是这会导致停机。虽然我们大多数人都在处理一直在运行的工作负载，但停机将是一种损失。由于 Kubernetes 的即时性，尽可能快地抵制部署看起来是违反直觉的，但却是信心所必需的。

应用程序在 Kubernetes 之前经历的建立信任的练习并没有随着 Kubernetes 神奇地消失。例如，测试和覆盖需求并没有消失。有了 Kubernetes，更多担忧的可能性就出现了。对于 Kubernetes 来说，运行一致性测试来验证由于可移植性原因而部署到的 Kubernetes 基础设施并不罕见。首先，可移植性是利用 Kubernetes 的一大吸引力。

类似于在 Kubernetes 上运行持续集成步骤，在 Kubernetes 上运行某些持续交付步骤本身是谨慎的。在 Kubernetes 集群上，很容易建立测试基础设施，然后关闭测试基础设施。根据建立信任步骤的长度，编排可能需要一个长期存在的工作流方面。在 Kubernetes 上或不在 Kubernetes 上运行长期/有状态工作负载的设计原则和决策同样适用于编排。

Kubernetes 非常有可能利用蓝绿色或金丝雀色等发布策略。虽然可以手工处理几个精心制作的 Kubernetes 清单并及时应用这些清单，但是涵盖这些发布策略的工具正在增加。构建适当的健康检查，如[活性和就绪性](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/)探测，以使增量部署能够继续，这是为 Kubernetes 设计架构时的关键。Kubernetes 之前所需要的安全性并没有随着 Kubernetes 而消失。随着生态系统和工具的不断成熟，新的范例将会出现。

### 继续旅程

随着 Kubernetes 模糊了基础设施和应用程序之间的界限，系统中一个常见的系统设计悖论“*作者能成为执行者吗*”可以在 Kubernetes 中轻松上演。在 Kubernetes 之前，开发工程师直接部署到生产中并不常见。它通常由某种 CI/CD 平台作为前台，具有不同级别的自动化和批准，以便投入生产。

有了 Kubernetes，您可以通过[名称空间](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)分离，轻松地在同一个集群上运行构建、信任建立步骤和部署，这取决于您对单个集群的隔离程度。随着现代工具和 GitOps 运动的兴起，现在作者可以实施一些标准，如漂移检测和部署声明状态的自我修复。

Kubernetes 具有一般意义上的反应能力。随着监控和可观察性工具的不断发展，如果部署成功，采取行动进一步部署或回滚 Kubernetes 现在肯定是可能的，例如在 Harness [软件交付平台](https://harness.io/platform/)上。随着越来越多的组织为了 Kubernetes 提供的所有好处而继续他们的 Kubernetes 之旅，明智的做法是不要忘记规程(在 Kubernetes 之前就已经存在)并拥抱新的范例。

干杯，

拉维河