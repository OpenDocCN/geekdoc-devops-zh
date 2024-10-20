# 利用集成的连续交付扩展混沌工程弹性功能| Harness

> 原文：<https://www.harness.io/blog/harness-expands-chaos-engineering-resiliency-features-integrated-continuous-delivery>

Harness Chaos Engineering 扩展了其混沌故障库，并提供了与 Harness 连续交付管道的本机集成，以构建和验证弹性。

Harness Chaos Engineering (CE)扩展了其 [chaos fault](https://developer.harness.io/docs/chaos-engineering/chaos-faults/) 库，并提供了与 Harness Continuous Delivery (CD)模块的本机集成，使开发人员和 sre 能够测试软件交付管道中应用的可靠性和弹性，以提高速度并最大限度地降低计划外停机的风险。

随着企业越来越多地转向云原生应用，以跟上创新的速度并满足客户对可靠性的期望，开发人员可以每天多次快速发布代码。随着这些快速的变化，复杂性增加，团队对系统如何运行缺乏理解，导致技术债务、漂移的可靠性度量和业务风险。

Chaos engineering 使团队能够防止因云原生环境的复杂性而导致的不可预见的停机和风险。有了混沌工程，开发人员可以在不影响开发速度的情况下测试可能导致应用程序崩溃的场景。

“混沌工程是可靠性和弹性测试的未来。这是确保应用程序在各种可能的情况下接受压力测试的最佳方式，从灾难恢复到处理流量高峰，”Harness 混沌工程负责人 Uma Mukkara 说。“将这些功能引入 Harness 软件交付平台，使客户能够更轻松地实施混沌工程，而不会降低开发速度，通过改善系统架构理解来提高开发人员的工作效率，并减少响应生产事故所需的时间。这将极大地帮助企业降低计划外停机的风险，并继续提供卓越的客户体验。”

## 将弹性最佳实践与治理混沌工程相结合

有了 Harness [Chaos Engineering](https://www.harness.io/products/chaos-engineering) ，组织可以采用、扩展和自动化软件弹性最佳实践，而不会对速度产生负面影响，也不需要定制工具和复杂的集成。Harness CE 识别受控系统级实验中的弱点，并为开发人员和 SRE 团队提供所需的信息，以防止未来发生故障。

Harness CE 是一个基于开源 [LitmusChaos](https://litmuschaos.io/) 项目的云原生解决方案，它使企业能够在具有护栏和[弹性评分](https://developer.harness.io/docs/chaos-engineering/technical-reference/experiments/)的 CI/CD 管道中自动进行混沌实验，这是一个量化指标，用于衡量在目标环境上执行相应的混沌实验时目标环境的弹性。因此，如果检测到可靠性问题，部署可以自动回滚。利用 CE 行业领先的[功能](https://www.harness.io/blog/harness-chaos-engineering-ce-key-capabilities)，企业可以在几天内(而不是几年内)建立一个可靠且有弹性的实践。

为了开始使用 CE，开发人员可以[创建一个试用账户](https://www.harness.io/trial/chaos-saas-trial)并遵循[简单教程](https://developer.harness.io/tutorials/run-chaos-experiments/#all-tutorials)来安装、测试可靠性，并将弹性构建到软件交付管道中。

### 新的混沌故障将测试能力扩展到无服务器应用

混沌工程企业 ChaosHub 已经扩展到 92 个混沌故障，测试 VMware、AWS、GCP、Azure、Kubernetes 和无服务器系统的弹性。

随着企业采用无服务器架构，六个 AWS Lambda chaos 故障的添加测试了无服务器流程在失败时的恢复能力。这有助于开发人员了解是否有适当的错误处理和/或是否实现了失败事务的自动恢复。

这些故障包括:

随着 IT 中断揭示故障模式和新技术的采用，Harness 继续扩展其混沌故障库，以确保其客户能够将弹性构建到他们的软件交付管道中。

## 用弹性工程构建可靠的软件

Harness CE 帮助企业实现持续弹性 ^(TM) ，通过混沌工程与 CD 的集成，确保软件交付生命周期的所有利益相关者之间的协作。作为唯一一个与本地 CD 集成的混沌工程解决方案，Harness CE 使团队能够在减少停机时间的同时保持速度。

sre、QA 工程师和开发人员可以快速将混沌实验整合到他们的 CD 管道中，以持续测试和验证他们的系统在中断期间将保持性能。

组织可以利用驾驭混沌工程来:

*   **提高开发人员的工作效率**让开发人员能够创新和创造，而不是被事件分散注意力。
*   **通过在不影响业务增长的情况下推动创新，加速数字化转型，保持竞争优势**。随着对系统行为理解的加深，新技术和流程可以更有效地实施。
*   **改善客户体验**通过确保系统高度可用和高性能，对停机做出更快的响应。
*   **通过了解系统如何处理故障来避免灾难**。

## 现在就开始用 Harness 构建软件弹性

这种原生 CD 平台集成是 [Harness 收购 ChaosNative Inc.](https://www.prnewswire.com/news-releases/harness-acquires-chaosnative-bringing-leading-chaos-engineering-solution-to-award-winning-software-delivery-platform-301507820.html) 的高潮，也是 [Harness 软件交付平台](https://www.harness.io/products/platform)的重大扩展。客户可以使用他们现有的工具独立部署每个模块，或者一起使用所有的线束模块来构建一个强大的、统一的现代软件交付管道，该管道跨越[持续集成](https://www.harness.io/products/continuous-integration)(CI)[持续交付](https://www.harness.io/products/continuous-delivery)(CD)[功能标志](https://www.harness.io/products/feature-flags)、[云成本管理](https://www.harness.io/products/cloud-cost)、[站点可靠性管理](https://www.harness.io/products/service-reliability-management)(SRM)[安全测试编排](https://www.harness.io/products/security-testing-orchestration) (STO)和[混乱工程](https://www.harness.io/products/chaos-engineering)。

如果您已经准备好了解您的组织如何采用这种实践并提高可靠性，[请求一个演示](https://harness.io/demo/chaos-engineering/)并注册今天的 [SaaS 试用](https://www.harness.io/trial/chaos-saas-trial)！