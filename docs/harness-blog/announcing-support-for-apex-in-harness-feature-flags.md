# 在线束功能标志|线束中宣布支持 Apex

> 原文：<https://www.harness.io/blog/announcing-support-for-apex-in-harness-feature-flags>

今天，我们宣布 Harness Feature Flags 现在拥有开源的 Apex SDK。我们很高兴能够推动新一代 Salesforce 应用程序开发，让用户能够更快、更安全地迭代和交付业务关键型应用程序。

[Apex](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_intro_what_is_apex.htm) 是一种日益流行的语言，用于构建基于 Salesforce 平台的软件。与所有其他应用程序一样，Salesforce 应用程序可以受益于功能标志。

今天，我们宣布 Harness Feature Flags 现在拥有开源的 Apex SDK。我们很高兴能够推动新一代 Salesforce 应用程序开发，让用户能够更快、更安全地迭代和交付业务关键型应用程序。

Salesforce 应用程序是许多组织的生命线。重要的是，他们拥有相同的功能管理权限，这样他们就可以[获得相同的收益](https://harness.io/blog/impact-of-feature-flags)并为客户创造价值。现在，这些团队可以更快、更安全地交付软件，并通过利用 Apex SDK 实现更多创新。

## Apex 功能标志入门

要开始将 Apex 用于您自己的功能标志，请查看 [Apex SDK 参考](https://docs.harness.io/article/aoe0y33mut-apex-sdk-reference)。该指南将带您了解如何获得 SDK、初始化和创建您的第一个特性标志。

Harness Feature Flag 用户可以期待与他们习惯的其他开源 Harness Feature Flag SDKs 相同的功能支持和性能。如果您已经熟悉任何其他受支持的 SDK，同样的模式和用例也适用。如果没有，那么[文档](https://docs.harness.io/article/aoe0y33mut-apex-sdk-reference)会指导你完成。

如果您习惯使用 Apex，使用我们的 Apex SDK 将非常简单。首先，只需安装 SDK:

###### $ > sfdx force:source:deploy-TARGET username = '您的目标组织'- sourcepath='force-app '

从那里，您将初始化 SDK 并配置您的目标模型(如果您不熟悉目标，[在这里阅读更多信息](https://docs.harness.io/article/xf3hmxbaji-targeting-users-with-flags)):

###### //将 flagKey 设置为要计算的功能标志键。

###### string flag = ' harmasppdemodarkmode '；

###### //设置缓存命名空间和分区

###### FFOrgCache cache = new FFOrgCache(' local '，' basic ')；

###### ff config config = new ff config . builder()。缓存(cache)。build()；

###### //设置目标属性。

###### ff target target = ff target . builder()。标识符(“线束”)。名称('线束')。build()；

###### FFClient client = new FFClient('你的 SDK Key '，target，config)；

从那里，只需实现您的标志。

###### // Bool 评估

###### 布尔值= client.evaluate(flag，false)；

###### system . debug(' Feature flag '+flag+'对于此用户是'+value+')；

## 将您的第一个功能标志投入生产

在您的代码中使用 Apex SDK 只是等式的一半。虽然确保 SDK 按预期工作至关重要，但将您的功能或更改包装在代码库内的标志中是您需要采取的两个步骤中的第一步，以获得功能完整的功能标志。

第二步，如果你还没有这样做，是前往你的 Harness 项目，并在 Harness 应用程序中创建标志。一旦这两部分都完成了，你就可以开始展示你的特色旗帜了。如果你需要额外的帮助，你可以遵循应用内向导，或者你可以查看[入门文档](https://docs.harness.io/article/0a2u2ppp8s-getting-started-with-feature-flags)。

准备好开始了吗？[立即开始免费试用](https://app.harness.io/auth/#/signup/?module=cf)！

快乐下垂！