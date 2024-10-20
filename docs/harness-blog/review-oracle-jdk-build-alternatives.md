# 对 Oracle JDK 构建替代方案| Harness 的回顾

> 原文：<https://www.harness.io/blog/review-oracle-jdk-build-alternatives>

对当前可用的 OpenJDK 构建的回顾。

自从甲骨文在 2018 年将其 JDK 构建许可分成两层——商业，可以免费用于开发和测试，但你必须付费才能在生产中使用它，以及开源，在任何环境下都是免费的——已经出现了不少基于 [OpenJDK](https://openjdk.java.net/) 源代码的开源构建。

现在，在 2021 年，我们正处于最初的 Oracle JDK 版本有许多替代方案的时刻。其中一些(例如，微软、Red Hat 和 Amazon Corretto)似乎有一个自私的目的——即帮助 Java 在他们自己的技术上运行得更好(或者根本不运行)。其他公司(如 Zing 和 OpenJ9)正在以某种方式增加技术。而其他公司(例如 Zulu 和 AdoptOpenJDK)似乎正试图通过自己的(表面上更便宜的)支持合同成为非 Oracle 版本供应商。

有了这些选择，我们觉得是时候回顾一下当前可用的 OpenJDK 版本了。下面是最常用的工具及其主要产品。

## 1.微软

微软刚刚宣布了自己的 OpenJDK 版本。他们本质上提供了特定于 Windows 的优化，这些优化还没有回到 OpenJDK 源代码中。

这是一个开源、免费的 OpenJDK 长期支持(LTS)发行版，任何人都可以在任何地方免费部署。它与 Java 11 的 TCK 兼容，包括 Java 11 的二进制文件，基于 OpenJDK 11.0.10+9，在 x64 服务器和 macOS、Linux 和 Windows 的桌面环境上运行。他们还发布了基于最新 OpenJDK 16+36 版本的 Java 16 for Windows on ARM 的早期访问二进制文件，并为 Surface Pro X 提供了 Windows ARM ( JEP-388)的端口

微软表示:“在过去的 18 个月里，我们贡献了 50 多个补丁，涵盖了 MacOS 打包、构建和基础设施、GC 修复和 Windows 增强等领域。…我们现在很高兴能够继续在这项工作的基础上，为 x64 平台生产基于 OpenJDK 11 的二进制文件，涵盖三大操作系统，并与 Java 社区和我们的 Microsoft Azure 客户分享这项工作。”关于 TCK 合规性，微软报告称“我们的 Java 11 二进制文件已经通过了 Java 11 的 Java 技术兼容性工具包(TCK)。”

## 2.Azul 平台 Prime

[Azul Platform Prime](https://www.azul.com/products/prime/) ，前身为 Zing，完全符合 Java SE 11、8 或 7 规范。Azul 表示，多亏了 Zing，“…Java 应用程序可以扩展到高性能水平，具有前所未有的响应时间一致性，并满足您最苛刻的服务级别要求，而无需更改应用程序，甚至无需重新编译。”

Azul 还声称 Zing 是“唯一一个为大堆大小消除 Java 垃圾收集暂停的 JVM”，并解决了自 Java 诞生以来一直困扰开发人员的 GC 问题。

它针对 Linux 服务器部署进行了优化，专为需要大内存、高事务率、低延迟和一致响应时间的企业应用程序和工作负载而设计。

## 3.Azul 平台核心

Azul Platform Core ，原名 Zulu，自称是 Oracle 的一个经济高效的 Java 支持替代方案，是“获得 Java 而没有锁定风险”的一种方式主要特性包括:

*   由严格的 SLA 支持的安全更新
*   比 Oracle Java SE 订阅价格更低
*   百分百开源
*   没有专有许可证或“使用领域”限制
*   季度和带外安全更新和错误修复
*   已验证 TCK 与 Java SE 兼容

## 4.红衣主教

Red Hat 声称其 OpenJDK 版本与 Oracle 的“功能非常相似”,并且“几乎不需要修改”,尽管它可能需要一些代码修改。

它以 OpenJDK 和 TCK 兼容为基础，在并入 OpenJDK 之前，可能会包含针对 RHEL/CentOS 的错误修复。它有一些加密方面的差异，时区数据稍微更新(取自 RHEL/CentOS)，并且包括 JavaFX 和 Java Web Start 实现。

## 5.Eclipse OpenJ9

Eclipse OpenJ9 是 IBM 对 [Eclipse 项目](https://www.eclipse.org/eclipse/)的贡献。按照 Eclipse 的说法，它是一个“高性能、可伸缩的 Java 虚拟机(JVM)实现，代表了数百人一年的努力。”

它最初是为在内存有限的移动设备上运行而构建的，并针对快速启动、更低的内存占用和快速升级进行了优化。然而，较低的内存占用是以牺牲计算为代价的，因此 TPS 通常明显低于 OpenJDK。

Eclipse OpenJ9 与 Java SE 的 TCK 兼容。

## 6.AdoptOpenJDK(即将被称为“Eclipse Adoptium”)

作为 AdoptOpenJDK 向 Eclipse Foundation 迁移的一部分，AdoptOpenJDK 将很快成为“Eclipse Adoptium”，免费提供预构建的 OpenJDK 二进制文件。

领养开放 JDK 的使命是:

1.  通过 OpenJDK 开发人员、学者和研究人员实现 R&D。
2.  提供一个开放的、通用的、经过审核的构建基础设施供供应商使用(如果他们愿意)。
3.  提供一个地方来试验构建和测试基础设施的想法，这些想法可以在 OpenJDK 中被吸收/重新实现。

他们提供社区支持，并推荐 IBM 提供商业支持。AdoptOpenJDK 目前与 TCK 不兼容。

## 7.亚马逊科雷托

最后但同样重要的是，我们有[亚马逊 Corretto](https://aws.amazon.com/corretto/) 。OpenJDK 的免费、多平台、生产就绪发行版。Corretto 的作案手法很大程度上是支持亚马逊 Linux 和针对亚马逊服务的优化。它附带了长期的商业支持，包括性能增强和安全修复。

Corretto 被认证为与 Java SE 标准兼容，亚马逊在成千上万的生产服务上运行它。使用 Corretto，您可以在流行的操作系统上开发和运行 Java 应用程序，包括 Linux、Windows 和 macOS。

以下是 Corretto 8 在 OpenJDK 8 中不可用的[补丁列表，以下是 Corretto 其他版本的](https://docs.aws.amazon.com/corretto/latest/corretto-8-ug/patches.html)[补丁](https://docs.aws.amazon.com/corretto/latest/corretto-11-ug/patches.html)列表。Corretto 与 Java SE 的 TCK 兼容。

所以你有它。当然，根据你的需要，使用 OpenJDK 的各种发行版也没什么不好。

你试过哪个 JDK，哪个适合你？