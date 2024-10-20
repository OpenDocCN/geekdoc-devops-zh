# 介绍线束混沌工程|线束

> 原文：<https://www.harness.io/blog/introducing-harness-chaos-engineering>

它在这里！Harness Chaos Engineering 包括 48 个实验&允许 Harness CE 和开源项目 Litmus 的用户对项目进行反馈。

今天，我们在 Harness 软件交付平台中发布了我们的最新模块—[Harness Chaos Engineering(CE)](https://harness.io/products/chaos-engineering)—进行公开预览。利用混沌工程使团队能够交付更具弹性的系统，帮助组织避免代价高昂的停机时间，包括经济损失和名誉损失。

Harness Chaos Engineering 使 DevOps 和 SRE 团队能够有目的地创建故障场景(“混沌”)，以确定其部署中潜在的弹性和可靠性问题。这些场景超越了传统的单元、集成和系统测试，更接近地代表了生产操作环境中的随机故障。这种对您的系统在定义的故障场景下如何表现的洞察，使团队能够了解应用程序和基础架构中存在的薄弱环节，并主动创建弹性以帮助防止代价高昂的停机。

## **混沌工程——让您的系统更具弹性**

Harness Chaos Engineering 提供的一些系统级故障场景[混沌实验](https://litmuschaos.github.io/litmus/experiments/categories/contents/)包括:

*   删除 Kubernetes pods 后，验证系统的能力仍然可用。
*   单元、节点、应用程序和基础架构级别的 CPU 和内存消耗。
*   网络挑战的注入，例如增加的延迟和网络损坏，以及 DNS 故障。
*   VMware 环境中的虚拟机可以停止一段定义的时间，然后重新启动。

## **混沌工程的下一次进化**

混沌工程是一个在软件交付中迅速流行的概念。Harness CE 不仅仅是满足混沌工程需求的另一种解决方案。以下是 Harness 在帮助公司交付具有混沌工程的可靠系统方面的一些独特之处:

*   尽可能多地覆盖真实世界的故障实验会导致更可靠的部署，从而减少停机时间。到目前为止，Harness CE 包括了通过任何提供商可获得的最多的实验(在发布时有 48 个)，而且 Harness CE 和[开源项目 Litmus](https://www.cncf.io/projects/litmus/) 的用户能够将实验反馈给项目。这意味着你将有机会访问一个不断增长的不同混沌实验库，以防止停机的影响。

*   创建可靠的系统是一项团队运动。许多团队需要来自混沌实验的信息来帮助建立可靠性。为了确保所有相关人员能够获得他们需要的访问和信息，Harness CE 提供了与 CI/CD、GitOps 的集成，以及与您现有的可观察性工具的集成。今天市场上的其他混沌解决方案不像 Harness Chaos Engineering 那样具有管道感知能力或管道集成能力，不包括 SRE 以外的团队，这些团队有必要帮助加快努力，使系统更加可靠。

*   关于系统中哪里存在漏洞的信息是高度敏感的信息。Harness 通过支持我们的自托管、内部和无间隙 CE 部署，实现了对所有实验运行以及这些实验结果的企业级安全和隐私控制。此外，Harness CE 还支持完全私有的混沌实验仓库(称为“私有混沌中心”)，其中包含您想要运行的实验和您已经创建的实验——甚至您正在运行以增强系统的实验也可以是完全私有的。

## **开始利用混沌工程**

Harness CE 提供当今市场上最完整、最全面、最包容的混沌工程解决方案，帮助您避免系统级停机。

要开始或了解更多信息，[点击这里](https://harness.io/products/chaos-engineering)。