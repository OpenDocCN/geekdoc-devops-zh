# 使用 Helm | Harness 将 CD Jenkins 管线迁移到线束

> 原文：<https://www.harness.io/blog/cd-jenkins-pipelines-harness>

从詹金斯管道到驾驭 CD 的转变轻而易举！这是科里的经历。

作为一个加入 Harness 和现有产品的新团队，我们已经在 Jenkins 上建立了持续集成/持续部署(CI/CD)管道。我想分享我们迁移到 Harness CD 的经验。剧透:这是一个快照！

回顾我们在使用 Harness 之前的部署解决方案，我们一直在使用 CI 工具来执行基本的 CD 功能。在摆脱了这些束缚之后，很明显我们有了斯德哥尔摩综合症的所有症状。为了突出区别，CI/CD 工具中的特性比较已经在中介绍过[。](https://harness.io/blog/comparing-harness-vs-jenkins/)

让我们看一下 Jenkins 管道的基本概况，它将每夜构建部署到非生产使用的开发或 QA [环境](https://harness.io/blog/deployment-environments/)。

我们的 Jenkins CD 管道利用了从 CI 发布的容器，以及来自 git 的舵图/值。在我们的示例中，管道部署了多个服务和一个客户端应用程序，该应用程序将对 K8s 名称空间中的所有内容进行冒烟测试。到目前为止，这些都是非常标准的东西，但是让我们从 Jenkins 管道中截取一段:

stage(' Deploy to Nightly K8S '){
when {
表达式{ params。DEPLOY _ TO _ NIGHTLY = = true }
}
步骤{
脚本{

dir('运行时/服务 A') {
脚本{
sh(脚本:"/usr/local/bin/helm upgrade-install-atomic-time out 10m-n $ { NIGHTLY _ namespace }-f ./helm/service a/NIGHTLY/NIGHTLY-values . YAML-set image . tag = $ { tagName }-set service . deployment name = $ { tagName } $ { back end _ release } helm-repo

Jenkins 管道中的一些阶段成为 Helm 的包装器，Helm 使用动态变量。在 Jenkins 中，我们没有丰富的 CD 功能，而是依靠 Helm 来管理部署的版本和回滚。用 Jenkins 做 CD 感觉就像在旧硬件上玩现代 PC 游戏——这可以做到，但不会很好看！

## 转向利用，不再需要脚本！

幸运的是，基于我们的一些最佳实践，我们能够快速利用:

*   CI 已经发布了可以在各种环境中运行的容器映像。
*   我们有成熟的舵图，减少了 CD 管道中的逻辑。
*   环境细节存储在私有 git 存储库中的 Helm 值中。

现在，让我们运行一个名为**收集器**的示例服务的线束管道:

装具就是为这种部署而设计的。因此，重新创建管道只是一个基本的配置问题，不需要任何代码！通过使用连接器，线束定义和访问相同的组件(例如，容器、舵图、舵值)。上面的突出显示显示了包含舵图和舵值以及位置的清单部分。工件部分包含所有必需的容器映像配置。此外，基础设施以同样的方式处理——设置一个 Kubernetes 连接器并选择一个名称空间！

最后，管道本身:

就是这样！此服务的单一阶段和单一步骤！每当发布新的容器映像时，这个特定的管道就会被触发。首次展示部署只是一个例子。其他复杂的部署，如 [Canary 或 Blue-Green](https://harness.io/blog/blue-green-canary-deployment-strategies/) ，无需在 Jenkins 管道或其他工具中重新创建逻辑就可以使用。

Harness 还具有持续验证等功能，这可以确保部署是健康的，甚至可以建议回滚。与 Helm 相比，Helm 在部署时通常依赖于基本的 K8s 探测器，Harness 能够感知部署，帮助用户做出回滚决定。

## 额外提示:利用头盔值控制诱惑

Harness 还有一个模板引擎。如果您熟悉 Helm，那么在 Harness 中使用模板的方式与此类似。一个常见的用例是将动态管道值传递给你的舵图，这通常是通过命令行参数*集来完成的。*然而，这并不总是需要的。以下来自 values.yaml 文件的示例显示了在管道执行期间被替换的线束变量的使用。

配置:
安装密钥:“<+服务配置。服务定义。规格。变量。install _ KEY>"
backendURL:<+服务配置。服务定义。规格。变量。back end _ URL >

图片:
仓库:线束/服务 b
标签:<+神器。标签>

一个基本的情况是使用触发的工件标签。然而，其他示例可以是用户输入或特定于环境的值。另一层模板永远不会伤害！查看[内置线束变量参考](https://ngdocs.harness.io/article/lml71vhsim-harness-variable)了解更多示例。

## 结论

如果您的部署与我们的有任何相似之处，那么您可能正在努力为您的云原生部署使用一个原始的 CI 工具。转向利用肯定会给你同样的“啊哈！”我们的经历。

当部署一个完全为 CD 设计的产品时，像连续验证和金丝雀部署这样的事情不仅是可能的，而且集成到 Harness 提供的解决方案中。现在，您的管道已经获得了足够的经验来达到下一个级别，是时候升级旧 PC，并花一些时间来玩下一代游戏！

在接下来的几周里，我们将分享我们将 CI Jenkins 管道转移到 Harness 的经验。当这篇文章出来的时候，我们会更新它的链接！