# JFrog Artifactory & Harness -不要陷入持续交付|线束的困境

> 原文：<https://www.harness.io/blog/jfrog-harness-dont-get-bogged-down-with-continuous-delivery>

JFrog 是如何管理工件的先锋和领导者。立即了解有关在 Harness 中集成 Artifactory 的更多信息。

这几天大家都在做 [CI/CD](https://harness.io/blog/what-is-ci-cd/) 。如果你参加任何 DevOps 会议，你会注意到两件事:1 .)许多团队声称每天完成 100 亿次部署，2。)许多供应商声称每天可以帮助实现 100 亿次部署。

事实是，[连续交付](https://harness.io/blog/what-is-continuous-delivery/)被误解了，对于大众来说，这是一个仍然没有解决的问题。

“胡说！t，我们已经建立了自己的 CI/CD 流程/平台，”我听到你这么说，问题就在这里:几乎每个人都试图自己建立它。

正常的配方是 Jenkins 加上几十万行 BASH 脚本(又名 jobs)。这是进行生产部署的好方法...呱呱。

## 持续集成！=连续交货

将代码转化为工件([持续集成](https://harness.io/blog/what-is-continuous-integration/))已经在很大程度上被 Jenkins 解决了，而 [JFrog](https://www.jfrog.com) 已经成为如何管理这些工件的先锋和领导者。

尽管在 Ansible 和 Terraform 等基础设施自动化技术和框架方面有很多创新，但如果你用 Jenkins 或类似的工具来构建，将工件投入生产仍然是一个复杂、手工和痛苦的过程。

部署脚本不是自动化。为什么？因为部署脚本的变化比特斯拉的股价还要频繁。这不是自动化；这叫软件维护。

由 DevOps 工程师团队来照看部署管道也并不罕见。从能力和治理的角度来看，真正使开发人员能够自己部署和测试仍然是一个巨大的挑战。

## 利用 Harness 和 JFrog 实现持续交付即服务

在 Harness，我们试图通过为开发人员和开发人员提供交钥匙服务来解决连续交付问题。

Harness 将在几分钟内与您的云堆栈和工具集成，然后您可以构建动态部署管道来在您的环境中推广工件。

在这个过程中，我们的 JFrog 故事和集成至关重要。事实上，我们最近支持了这个星球上最大的 [Artifactory](https://jfrog.com/artifactory/) 实现之一(> 1 PB)。下面是如何将 JFrog 集成到您的部署管道中的快速入门。

## 使用线束配置 Artifactory

您需要做的第一件事是在 Harness 中注册 Artifactory 实例。
为此，请转到:**设置>连接器>工件服务器>添加工件服务器**

现在输入您的工厂网址、用户名和密码。

**可选:**您还可以对 Artifactory 服务器设置使用限制。例如，Artifactory A 只能用于应用程序 X 和环境 D、E 和 f。

## 基于 Artifactory 工件创建应用程序/服务

现在集成了 Artifactory，您可以开始在 Harness 中定义您的应用程序和服务层次结构。
让我们使用 Artifactory 创建一个简单的微服务。

转到:**设置>您的应用>服务>添加服务**
为您的服务指定一个**名称**，并选择“ **Docker** ”作为工件类型。
现在，点击**添加工件源**

现在将线束指向您的工厂**服务器**、**存储库**和**映像** **名称**:

就是这样。Harness 现在将自动对 Artifactory 实例中的每个新构件版本进行版本控制:

## 构建您的第一个部署管道(使用 Xray)

有了现在在 Harness 中定义的服务构件，您可以在几分钟内构建部署工作流和管道。

转到:**设置>您的应用>工作流>创建工作流**

例如，我们可以构建一个三阶段 canary 部署工作流来部署我们的新微服务，首先对容器工件进行 [JFrog Xray](https://jfrog.com/xray/) 安全扫描:

要集成 JFrog Xray，只需单击“**预部署步骤**”部分下的“**添加步骤**”。选择“**HTTP**”，并创建一个指向 Xray 的 webhook，该 web hook 在您的工作流程中引用＄{ artifact . display name }和＄{ artifact . artifact path }。就是这样！

当这个部署工作流(或管道)启动时，您可以实时观察所有这些部署步骤的执行，并全面了解控制台输出和日志。

使用 JFrog 和 Harness，现在可以使用工件库作为源/触发器，在几分钟内创建端到端的部署管道。

欲了解更多信息和免费试用，请立即访问[线束](https://app.harness.io/auth/#/signup/?module=cd)。

干杯，
史蒂夫。@ BurtonSays 说