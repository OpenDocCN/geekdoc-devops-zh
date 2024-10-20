# 通过策略即代码和自动化变更管理满足联邦合规性要求|利用

> 原文：<https://www.harness.io/blog/federal-compliance-requirements-policy-as-code-automated-change-management>

有了大量的法规遵从性政策，联邦 IT 部门最终如何实施、管理和执行这些政策呢？

在美国联邦政府、国防机构和情报部门的各种政策中导航是一个复杂而漫长的过程。正如[国防情报局](https://www.dia.mil/News-Features/Articles/Article-View/Article/2926343/gamechanger-where-policy-meets-ai/) (DIA)所说:

> 你知道吗，如果你阅读国防部的所有政策，就相当于通读《战争与和平》100 多遍。

为了让您更好地了解 IT 运营所面临的复杂性，请查看国防部(DoD)负责网络安全的副首席信息官发布的关于如何[构建和运营可信 DoDIN](https://dodiac.dtic.mil/wp-content/uploads/2022/07/2022-06-24-csiac-dod-cybersecurity-policy-chart.pdf) (DoD 信息网络)的文档，分享如下。正如国防部所言，“国防部网络安全政策图表的目标是在一个有益的组织方案中捕捉适用政策的巨大广度，其中一些政策许多网络安全专业人员可能甚至没有意识到。”

正如您在上面的框架中看到的，有许多步骤，具有多层复杂性和大量信息。虽然旨在组织信息，但它说明了联邦一级的治理和政策执行是多么复杂。不仅如此，随着每年新政策的实施，这个列表还会继续增长。有时，这些策略甚至会相互冲突，使得法规遵从性成为一项极具挑战性的任务。

有了这样一套广泛的政策，联邦 IT 部门最终如何实施、管理和执行它们呢？

## 手动执行政策的艰辛

IT 策略的大部分实现、[治理、](https://www.harness.io/blog/pipeline-security-and-governance-a-catalyst-for-devsecops)和执行都是由昂贵的 [IT 专家团队手工完成的。这些角色的入职也是一项巨大的投资。公共部门信息技术专家不仅需要具备组织所实施系统的专业知识，还需要保留所有相关的政策信息。](https://www.usajobs.gov/job/668659300)

因为这些 IT 专家最终负责法规遵从性，所以这些操作通常集中在两种执行方法中的一种:

1.  一切都被封锁了。 DevOps 工作人员必须精心计划、申请和获得所有必要资源的人工批准，确定和访问管理(IAM)权限，以及实施管道所需的依赖关系。如果引入了一个新的需求，这个过程将从头开始。
2.  所有的事情都是公开的，员工要对自己负责。细心的 DevOps 员工需要不断地重新创建他们的管道，并手动参考和应用冗长的合规清单文档。当错误确实发生时，必须进行冗长的发现和事后分析，以确定哪些行为和哪些人员应对此负责。

随着 IT 政策数量的增加，专家承担的精神负担也在增加，如果他们离开，会导致精疲力尽和潜在的知识损失。离职意味着入职和培训要从头开始，这会带来巨大的财务成本。

虽然联邦机构可以雇用更多的人员，但由于对加速“任务速度”的需求，运营需求很快超过了这些角色的招聘所有这些导致政策审查和系统评估的大量积压。最终，这些延迟会阻碍技术的采用和创新。

## 迈向进步的步伐

一些努力正在进行中，以减少政策管理的辛劳。像 [Gamechanger](https://www.dia.mil/News-Features/Articles/Article-View/Article/2926343/gamechanger-where-policy-meets-ai/) 这样的工具正在开发中，以利用人工智能来组织和筛选堆积如山的政策信息。虽然这些努力很重要，但从本质上来说，它们仍然依赖于大量的手动任务，并辅以优化的信息检索系统。

其他供应商与政府合作伙伴和顾问合作，通过联邦风险和授权管理计划( [FedRAMP](https://www.fedramp.gov/) )等合规性框架来评估和认证他们的产品，该计划为许多 IT 政策捆绑包提供授权批准。也就是说，这些合规框架仍然拥有[臭名昭著的长期视野](https://itif.org/publications/2020/06/15/reforming-fedramp-guide-improving-federal-procurement-and-risk-management/)，并且它们需要大规模的[前期投资](https://itif.org/publications/2020/06/15/reforming-fedramp-guide-improving-federal-procurement-and-risk-management/)，这对于小企业和初创公司来说是望而却步的。这些框架也没有被所有联邦机构广泛接受。

机构通过自动化策略合规性(如脚本检查和测试)展示其 IT 运营的成熟度。虽然这是一个正确方向的趋势，但这种方法经常被放弃，因为这些实践仍然需要手工劳动，因为必须手工为每个管道重新创建符合性测试。此外，每次添加额外的阶段和步骤时，都必须对其进行人工重新评估和重新批准。

## 政策即代码

虽然我们无法减少策略的数量或消除它们之间的冲突，但许多联邦组织正在将策略即代码实施到他们的 IT 策略中。[策略即代码](https://www.harness.io/blog/policy-as-code)允许 [DevSecOps](https://www.harness.io/blog/devsecops-strategies-secure-applications) 员工保留开发和部署的灵活性，而无需在安全性和合规性方面做出折衷。

策略即代码通过允许团队编写策略来实现这一点，这些策略定义了组织不能拥有什么操作，以及他们必须拥有什么以及以什么顺序拥有。然后，机构可以通过告知用户他们不遵守哪些政策的建议模式或主动禁止运营的强制模式来实施这些政策。

例子包括:

*   CD 管道必须包含 Snyk 容器扫描，然后才能推入生产
*   集装箱图像必须来源于注册管理机构的批准列表
*   生产管道不能包含“关键”级别的安全漏洞

这并不是一个全新的想法，因为它是由 CNCF 的开放政策代理(OPA)项目推广开来的。机构可以使用自动化流程的工具来简化流程。Harness 平台整合了基于 OPA 的[策略代码](https://www.harness.io/blog/harness-policy-as-code)，作为集中式策略管理和规则服务，帮助组织创建和实施有关部署、基础架构等的策略，在不牺牲合规性和标准的情况下提高开发速度。

政策即代码直接解决了辛劳的痛苦:

*   IT 专家不再需要手动评估每个新软件的采用或部署。相反，他们可以一次编写一个策略，并在开发的前端强制执行它。
*   当 IT 专家离开项目或组织时，他们的专业知识会被捕获并在策略代理中重用，从而有效地整理部落知识并允许无缝地继续策略遵从操作。
*   开发人员拥有护栏，通过确保政策合规性，使他们能够安全地实施和利用可用的软件和基础设施资产。

## 自动化变更管理

尽管策略即代码可以减少管理和实施 IT 策略的工作量，但仍有一些情况需要来自可信的审计官员或变更顾问委员会的记录验证和授权。此外，“人在回路”方法—将手动检查点纳入自动化流程—可以轻松替代更复杂的策略合规性评估。

虽然我们不能实现人类的自动化，但是我们可以消除在您的组织中可靠地执行变更管理所涉及的大量工作。当今使用的许多变更管理流程需要持久的、手动的流程，这些流程不会增加价值。一个例子是，当操作的状态必须在各种记录系统中手工更新时，以及当批准之后通常是沿着它们的开发生命周期手工继续操作时。

许多解决方案提供了与流行的变更管理工具的管道集成，如[吉拉](https://docs.harness.io/article/077hwokrpr-jira-integration)和 [ServiceNow](https://docs.harness.io/article/7vsqnt0gch-service-now-integration) ，以及为目前不依赖供应商系统的公司提供的内置[人工批准](https://docs.harness.io/article/43pzzhrcbv-using-harness-approval-steps-in-cd-stages)功能。例如，Harness 以编程方式向吉拉和 ServicNow 票证添加完整的上下文更新，例如到来自[安全测试编排](https://docs.harness.io/article/2ap1uol6ti-sto-overview)和[持续验证](https://docs.harness.io/article/3xhqq9xllp-verify-deployments-with-the-verify-step)的结果的链接，并在审批关口需要他们注意时通知变更管理人员。如果变更得到授权方的批准，Harness 会记录这一批准，并自动进入下一阶段的流程。如果更改未被批准，Harness 可以暂停或停止管道，并在需要时启动任何自动回滚。

# 使用安全带开始您的 DevSecOps 之旅

Harness 是将您的人员和 IT 运营纳入现代 DevSecOps 方法的最快速、最简单的方式，这些方法超越了公共部门治理和法规遵从性要求，而不会影响交付。

Harness 为联邦 IT 团队提供了主要优势，包括:

*   可靠地管理策略代理的实现，因此您的团队不需要部署和维护 OPA 服务器。
*   在从 CI/CD 到云资源的所有 IT 运营中广泛地模板化、收集和应用策略。
*   基于角色的访问控制(RBAC)和您的 SSO 和 LDAP 提供商，允许您跨组织和个人用户精确地应用和控制策略实施。

在我们的[治理页面](https://www.harness.io/products/platform/governance)上了解更多有关我们如何实施策略即代码、自动化变更管理的信息，以及更多帮助联邦机构大规模采用 DevSecOps 的信息，并且[请求个性化演示](https://www.harness.io/demo/next-gen)以查看这些功能的运行情况。