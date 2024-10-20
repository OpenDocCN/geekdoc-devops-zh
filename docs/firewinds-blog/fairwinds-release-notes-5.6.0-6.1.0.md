# Fairwinds 发行说明:5.6.0 - 6.1.0

> 原文：<https://www.fairwinds.com/blog/fairwinds-release-notes-5.6.0-6.1.0>

 我们在费尔温斯一直很忙。今年 10 月，我们升级了软件的许多元素，包括对服务质量工作负载、页面任务集成和导航栏的改进，此外，我们还修复了几个错误。这个博客将涵盖你需要知道的关于新版本 5.6.0-6.1.0 的一切，以及它们的进步将如何影响你的 Kubernetes 所有权的安全性和可靠性。

[Fairwinds Insights](https://www.fairwinds.com/fairwinds-insights-trial?utm_campaign=Fairwinds%20Insights%3A%20Release%20notes&utm_medium=email&_hsmi=2&_hsenc=p2ANqtz-8NSjSStDb8S0MyXLWD0AWZ_9QhupEwmfzpisIOjKSUtRweiyJ0xoetUWZszVE-4ZTQu1wlAVOjzrRxixa3bOXgqhCC0FCmQ3RGBCWhtOWe3WQqy_c&utm_content=2&utm_source=hs_email) 旨在确保您的组织继续发现正在进行的 [Kubernetes 安全](https://www.fairwinds.com/kubernetes-security)监控、[政策执行](https://www.fairwinds.com/enforce-kubernetes-policy)、[合规](https://www.fairwinds.com/kubernetes-compliance)和[成本优化](https://www.fairwinds.com/kubernetes-compliance)。

## 5.6.0 工作负载的服务质量

新的 QoS 参数允许用户指出其工作负载的重要性，同时还允许 Insights 根据该参数推荐资源。从重要性最高到最低，这些值是关键的、有保证的、突发的和有限的。此版本中的错误修复包括:

*   集群的操作项目计数不再包括在组织仪表板中传递它们。
*   行动项目表现在可以根据解决方案正确过滤。
*   组织仪表板的用户界面修复已完成。

## 5.7.0 页面负载集成

组织现在可以设置 PagerDuty 集成，以便从 Insights 自动发送突发事件。团队管理现在在邀请已经是组织成员的用户时显示更好的错误。你可以在这里了解更多关于我们的 PagerDuty 集成[。](https://insights.docs.fairwinds.com/configure/policy/rules/#integrations)

## 重命名导航栏项目

一些导航项目已被重命名如下:报告中心以安装中心，Repos 到所有存储库，集群到所有集群。错误修复包括:

*   主存储库页面不再使用旧的操作项徽章。
*   准入控制器项目现在在折叠后加载。
*   “集群概述”页面上历史条形图下的链接将被修复。

## 6.1.0 新普罗米修斯行动项目

Prometheus Collector 报告现在将根据工作负载指标生成行动项目。组织仪表板中集群表的加载时间现在更快了，并且跨 Insights 进行了各种 UI 改进。

您可以随时在此查看发行说明[的完整列表。](https://insights.docs.fairwinds.com/release-notes/?utm_campaign=Fairwinds%20Insights%3A%20Release%20notes&utm_medium=email&_hsmi=2&_hsenc=p2ANqtz-_lgp8r2KcXX2TXvtC2Jtizl9Xga1slQH8C_PsPwwVbdmJZd6O-gRleTd9W4zyR52z5er6_6xDp0-VVH2CIpMGykqz6ELYGDe1YijMG8SgcuMWzeQg&utm_content=2&utm_source=hs_email)

你们中的一些人可能已经在使用我们的开源项目了。我们最近推出了一个开源用户组，我们一直希望有更多的人加入我们。[在这里](https://cfvbp04.na1.hubspotlinks.com/Btc/I2+113/cfVbp04/VVVHms5Y6dgsV18gyd5KW-V0W6tdK5Q4zpNHgN7Mjfyw3q90JV1-WJV7CgH3QW54cSXs2JVHJQMPFXrq3pCY5W3J1qK96Y78_sW10CXtY4d6FN6W3km4v98dr92wW1rfrrK5R0F6WW2gyqZt34cfz-W5gFPQK5R7tHXW6qHTBZ4z-_5sW88NfJG46DV85W3F254p7Vz4VqW2mGbCT8P9x-DW3wTFNh74JZ41VRQ2Tm4cSyFbW3n8sxz2Jr16cW3f49hq1qwcdYW3XmB2-5dXqkqW6XNfJF3_q-q-W4WL-Kg6mF48sW8Gjz7z5PTTHPVZH0BS23RkfqW8TDc2m2GpW40W7smLZd5TYCHJN9dMbpy5vTTrVNZy_78q1rMXW6BX0mC4JYgMjW3rvDv78Nx3mCW7TrGhr3HF7G132YG1)注册 Fairwinds 开源用户组，并加入我们 12 月的下一次聚会！

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)