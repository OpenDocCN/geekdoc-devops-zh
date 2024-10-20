# 自主跑步者为 CircleCI 增加手臂支撑

> 原文：<https://circleci.com/blog/self-hosted-runners-add-arm-support-to-circleci/>

今天早些时候，我们宣布将[自主跑步者](https://circleci.com/blog/our-cloud-platform-your-compute-introducing-the-circleci-runner/)添加到我们的云产品中。这让我们新的[规模计划](https://circleci.com/pricing/)的客户能够在他们自己的基础设施上运行 CircleCI 作业。我们也很高兴地宣布，这些跑步者有史以来第一次在 CircleCI 上支持 Arm 计算。

## 手臂的动量

最近， [Arm](https://www.arm.com/) 架构在边缘计算、台式机和笔记本电脑、数据中心服务器等领域获得了快速发展。Arm 处理器可以提供显著的性能和效率优势，因此在移动和物联网设备中几乎无处不在。随着 Arm 架构的发展，对基于 Arm 的 CI/CD 解决方案的需求也在增长。随着我们的自托管 runners 上增加了 Arm 支持，现在那些在 Arm 上开发的人可以在 Arm 资源上运行他们的 CircleCI 管道。

## 手臂环扣

有了[自托管运行程序](/execution-environments/runner/)，CircleCI 客户现在就可以开始为基于 Arm 的设备构建、测试和部署应用程序。在发布时，CircleCI runner 正式支持 Linux 上的 arm64 架构。我们计划在不久的将来扩大支持平台的列表，以涵盖最流行的操作系统。

runner 只是 CircleCI 上的一种 Arm 支持，但它是我们致力于在基于 Arm 的设备上实现进一步软件开发的开始。除了自托管跑步者，CircleCI 还提供世界上最全面的 CI/CD 云托管计算车队。这些机器称为资源类，为 CircleCI 用户提供即时、按需访问干净、安全的执行环境，以构建和测试他们的代码。它们还让开发人员能够为管道中的每个作业定制操作系统、CPU、内存等，从而在云中提供无与伦比的灵活性。此外，在 2021 年初，CircleCI 将向我们已经庞大的设备群添加基于 Arm 的资源类，以便用户可以在 Arm 上构建，而无需管理自己的 CI/CD 基础设施。

如果你今天想在你自己的 Arm 计算上运行 CircleCI 作业，你可以在这里了解更多关于使用自托管运行器的信息。