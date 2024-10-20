# 从去过那里的人那里得到的 GitLab 和马具|马具

> 原文：<https://www.harness.io/blog/gitlab-and-harness>

去过那里，做过那件事:深入探究 GitLab 的好、坏、丑，以及为什么马具为王。

在我的最后一份工作中，我是一个现场工程组织的技术架构师之一。作为一个团队，我们负责处理各种各样的问题，包括定制服务、构建集成、参与开源项目和“创造性”解决方案。随着我们规模的扩大，我负责解决的一个关键问题是 CI/CD，包括实施、维护、标准以及与现有吉拉流程的结合。

面临的挑战:

*   在不到两年的时间里，团队从 3 人发展到 15 人以上。
*   50 多个不同复杂程度和范围的项目，其中许多都附带了支持 SLA。
*   分布在越南、加拿大和美国的团队。
*   使用了许多不同的语言(Python、Golang、React/JS 等)。
*   许多分发和部署方法。
*   测试和部署中混合了传统和现代部署基础架构。

GitLab 是更广泛的工程组织最近的选择，从一个普通的 git 实现，所以它实际上是我们选择的系统。在 GitLab 之前，我们的管道由一些原始的 Jenkins 实现组成，这激发了许多愤怒的 Slack 长篇大论。

## 好人

项目的基本入门非常简单。假设您已经预先配置了基础设施(runners ),那么您只需要添加一个. gitlab-ci 文件，并为新项目提供一些简单的指导，然后您就可以开始运行一些简单的管道，只需花费五分钟的时间。Auto DevOps 模板作为构建更复杂管道的起点工作得很好。

CI 和 git 之间的集成做得很好，这对于实施良好的 git 流程和卫生非常有用。管道相邻的合并请求和直接链接的吉拉问题提供了可靠的协作、审查和治理。

## 坏事

“包含”形式的可重用组件是一个有用的构造，但是当许多项目有许多“包含”时，扩展这种方法是困难的从开发者的角度来看，获得管道的整体视图是困难的。当我第一次动手使用 Harness 时，我立刻意识到[的模板化](https://docs.harness.io/article/60j7391eyy-templatize-pipelines)非常简单，并且易于可视化。

更高级的模式和优化过程通常是一种反复试验的过程。GitLab 的自助服务方面很好，因为您不需要专门的 DevOps 人员来构建每个管道，但缺点是解决更复杂的问题需要大量的时间投资来构建、测试和验证解决方案，然后再将它们推广到更广泛的项目。在 Harness，[在 YAML 和 UI 中开发、可视化和测试组件](https://docs.harness.io/article/m220i1tnia-workflow-configuration)(工作流)的能力有助于简化构建更复杂的逻辑和流程的过程。

A Very Nice™ pipeline in Harness.

吉拉集成(能够将合并请求绑定到问题上，反之亦然)对于一些用例来说工作得很好，但是在 CI/CD 管道的环境中，如果没有额外的脚本和集成，它就不能真正绑定到我们想要的流程中。管道不能直接更新和添加上下文到吉拉。考虑到吉拉是我们发布的最终真实来源，这个过程以一些手动步骤而不是一个闭环结束。[吉拉与 Harness](https://docs.harness.io/article/077hwokrpr-jira-integration) 的集成允许票据被打开、更新、用于变更管理等等——直接从管道中。

## 丑陋的

部署给我们的团队带来了各种挑战。Auto DevOps 适用于一些特定的项目，但大多数项目都需要定制，并且包括传统的基础架构。最后，大多数部署都是手动完成的，或者是脚本和手动工作的结合。这是我们在 Harness 看到的一个常见挑战，在 Harness，许多客户通过扩展 CI 工具来尝试 CD，却发现他们应该从一开始就使用真正的 CD 工具。使用不正确的工具所带来的测试和维护挑战是不值得的。

我们执行了一些常见的任务，例如推送至 Docker 注册表或提供资源，其中一些解决方案是两个内衬，而其他解决方案要复杂得多，需要足够的变化，内联脚本无法扩展。大多数问题的解决方案最初是写出用例，但在许多回购中，我们发现缺乏这一点，并且以可扩展的方式在项目间共享成为一个挑战。[到公共服务的可配置连接器](https://docs.harness.io/article/a7n7lwsjpk-harness-connectors)提供了一种方式，使得这些动作以安全、可重复的方式自上而下可用。

供应和配置本身就是一项挑战。能够一致地调配、验证和配置新的虚拟机和基础架构归结为手动脚本编制和一些手动流程。线束提供了在管道中使用[地形](https://docs.harness.io/article/9pvvgcdbjh-terrform-provisioner)和[云形成](https://docs.harness.io/article/78g32khjcu-cloud-formation-provisioner)的能力。

鉴于项目数量庞大而资源有限，在没有人工审查的情况下，很难确保新项目符合标准。这个问题是通过审计、项目评审和模板化的结合来解决的。在同质项目的情况下，这要容易得多。例如，我们项目的一个子集遵循语言和结构的标准格式，因此实施良好的实践更加简化。对于各种各样的、一次性的、古怪的项目，这导致了大量的辛劳。Harness 提供了开箱即用的[管道治理](https://docs.harness.io/article/zhqccv0pff-pipeline-governance)，提供了自上而下管理管道标准的能力。

最后，GitLab 缺乏构建和发布活动的全局视图。我们最终使用吉拉作为我们的“单一控制台”解决了这个问题，但是从工程的角度来看，我们必须检查单个项目或者编写 API 脚本来收集信息。了解完整的何人、何事、何时、何地以及为什么需要从吉拉和 GitLab 的许多项目中收集信息。管理层需要定期更新所有这些活动的信息，所以最终要花很多时间来汇编信息。Harness 提供了[开箱即用的仪表盘](https://docs.harness.io/article/xldc13iv1y-meet-harness)，以及[创建和共享定制仪表盘](https://docs.harness.io/article/rxlbhvwe6q-custom-dashboards)的能力，即将到来的更新将为[定制仪表盘](https://ngdocs.harness.io/article/ardf4nbvcy-create-dashboards)带来更加强大的体验。

Did someone say custom dashboards? It was us. We said custom dashboards.

## 关键要点

GitLab 作为 CI/CD 工具的主要好处是与源代码控制紧密集成。这是 Harness CI Enterprise 提供[开发人员体验](https://harness.io/blog/developer-experience/)的原因之一，在这里，与超出 git 级别的源代码控制的集成增强了工具的整体工作流和效用。

我的团队面临的许多挑战都归结于我们多样化的用例集，这些用例最终必须通过脚本和手工劳动来解决。当我开始在这里工作时，部署到不同的基础设施是我喜欢 Harness 的第一件真实的事情之一——它不局限于 [Kubernetes 和 EC2](https://docs.harness.io/category/esd8pq6adw-quickstarts-category) 。

另一个关键挑战是常见的重复任务。用于常见交互的连接器，如[吉拉](https://docs.harness.io/article/077hwokrpr-jira-integration)、[地形](https://docs.harness.io/article/97wr4kz3ew-harness-hashi-corp-integrations)等，都提供了开箱即用的线束。

其他特性，比如开发人员在项目级别的可见性，非常有用——但是从整个组织的角度来看，还有很多需要改进的地方。Harness 既提供了包含最重要内容的[现成仪表板](https://docs.harness.io/article/c3s245o7z8-main-and-services-dashboards)，也提供了[创建定制仪表板](https://docs.harness.io/article/rxlbhvwe6q-custom-dashboards)的能力，以全面了解 DevOps 生命周期的各个方面。

More dashboards.

最后一点，如果我不提及 Harness 中最酷的特性之一，那将是我的失职:[持续验证](https://docs.harness.io/article/ina58fap5y-what-is-cv)。GitLab 提供了与监控相关的内容，但总体来说范围非常有限。对于我的团队来说，相对于现有的监控和流程，我们看不出设置它有多大价值。Harness 提供了绑定到已经配置好的指标提供者的能力，并使用该数据来了解稳定状态、检测问题以及自动回滚有问题的部署。我们的客户已经使用 CV 来[减少部署后的观看监控时间](https://harness.io/customers/case-studies/automated-ci-cd-rollback/)和[提高质量](https://harness.io/customers/case-studies/deploy-more-software-with-less-downtime/)。

希望你已经了解了更多关于 GitLab 和 Harness 的区别。如果你和我一样对这个平台感到兴奋——现在仍然如此——今天就来试试吧！