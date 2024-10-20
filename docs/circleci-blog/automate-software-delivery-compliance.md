# 软件交付自动化合规| CircleCI

> 原文：<https://circleci.com/blog/automate-software-delivery-compliance/>

软件开发团队面临着越来越多的障碍:不断变化的设计需求、组织障碍、紧迫的期限、复杂的技术栈和软件供应链。开发人员和 IT 领导者面临的一个新挑战是需要遵守规定全面数据安全、事件响应以及监控和报告要求的法规和控制框架。

法规遵从性要求会给组织增加大量开销。幸运的是，可以使用持续集成和第三方工具来自动化与法规遵从性相关的活动。在本文中，您将回顾常见的法规遵从性框架的例子，作为软件交付组织实现法规遵从性的最佳实践，以及您如何自动化 CI/CD 的法规遵从性。

使用新的触发器和权限控制来管理您的生态系统中的变化

[Learn More](/blog/announcing-gitlab-support/)

## 软件合规性要求的示例

不同行业中的许多监管标准都要求遵守软件开发指南。这些标准的快速增长源于数字技术的爆炸式增长及其带来的新兴网络威胁。虽然法规遵从性不同于安全性，但它确实建立了组织的安全性必须满足的最低综合基准，如果没有超过的话。

例如，[健康保险便携性和责任法案(HIPAA)](https://www.hhs.gov/guidance/document/hipaa-professionals) 规定了美国医疗保健提供商和从业者使用的个人可识别健康信息的隐私和安全保护。另一个标准是[支付卡行业数据安全标准(PCI-DSS)](https://www.pcisecuritystandards.org/documents/PCI_DSS-QRG-v3_2_1.pdf) 。它提供了保护消费者隐私的卡交易护栏。具体来说，它禁止存储和未加密传输和处理个人卡详细信息，如卡验证值(CVV2s)、整个磁条和 pin。

对于服务提供商而言，[服务组织控制 2 (SOC2)](https://us.aicpa.org/interestareas/frc/assuranceadvisoryservices/socforserviceorganizations) 报告是一份有价值的合规性认可，它确认了组织的服务交付流程和控制的[信任服务标准](https://us.aicpa.org/content/dam/aicpa/interestareas/frc/assuranceadvisoryservices/downloadabledocuments/trust-services-criteria.pdf)。

甚至政府在与承包商开展业务之前也有合规要求。例如，[联邦风险和授权管理计划(FedRAMP)](https://www.fedramp.gov/) 是一项认证，政府承包商必须证明他们的云产品足够安全，可以存储国家数据。

在实践中，大多数企业的目标是获得多个合规标准的认证，以瞄准不同地区的市场，并让客户放心。因此，许多总部位于美国的云服务提供商希望获得欧盟(EU) [通用数据保护法规](https://gdpr.eu/compliance/) (GDPR)合规性批准，以便为总部位于 EU-的客户提供服务。

## 软件合规性最佳实践

实施众所周知的遵从性最佳实践是衡量您在内部公司治理、风险管理流程、组织监督政策、供应商管理和一般安全意识方面的能力的一个很好的方式。常见软件合规性实践的示例包括:

*   严格的访问控制和权限
*   全面的测试和变更管理
*   供应链漏洞扫描
*   定期合规审计

下一节将探讨其中的一些实践，并描述您的组织如何通过自动化和简化您的法规遵从性实践来减轻其法规遵从性负担。

### 访问控制、角色和权限

访问控制是一套针对用户和基础设施访问的指导原则和程序。它确保您已经封锁了私人和专有信息，防止未经授权的访问，并且在发生违规事件时爆炸半径很小。

访问控制始于身份验证，包括在授权访问敏感数据之前确认用户的身份。至少，用户身份验证包括验证唯一的用户名和密码。它还可以包括面部、指纹或眼睛的扫描，或者加密证书。例如，双因素认证通常需要用户名和密码组合*和*生物特征扫描。

身份验证后，您可以实施基于角色的访问控制(RBAC)策略。这些策略基于用户在组织中的分配角色来限制对信息的访问。RBAC 策略应该基于最低特权原则，也就是说用户应该只能访问他们工作所需的资源。除了 RBAC，您还可以使用基于属性的访问控制(ABAC)来提供基于用户或对象特征、操作类型等的细粒度访问。例如，您可以使用 ABAC 来实施公司在转换时间或周末停止工作的策略。

访问、角色和权限应该在固定时间内有效，并在到期或不再使用时撤销。这意味着持续监控，您可以通过[计划清理扫描](https://circleci.com/blog/schedule-recurring-database-cleanup/)或策略即代码软件(如[开放策略代理](https://www.openpolicyagent.org/))来评估和执行声明性代码编写的访问策略，从而实现自动化。

### 综合测试

软件测试包括[功能测试和](https://circleci.com/blog/functional-vs-non-functional-testing/)非功能测试。功能测试证明，与一组需求或规范相比，应用程序运行良好。非功能性测试评估应用程序的大规模行为、它对最终用户体验的贡献以及它遵守内部和外部安全要求的程度。

即使您的软件和基础设施通过了功能、性能、可靠性和安全性测试，它们仍然可能不符合标准。不合规的代码更容易受到攻击，可能导致不合规的状态，对声誉产生负面影响，并招致巨额罚款。

组织可以进行全面的合规性测试，以检查用户访问权限、个人身份信息的传输和存储以及程序更改控制程序中的应用程序和基础架构漏洞。合规性检查还会验证组织的文档和程序、活动日志以及软件许可证。

虽然许多合规性测试可以自动化，但有些组织需要自动化和手动检查的结合。例如:

*   自动化功能测试确认应用程序的登录功能正常。
*   手动非功能性测试确认密码是不可见的，并且按照开发代码中的规定在数据库上进行了加密。

类似地，自动化的安全测试可以确保应用程序免受[不安全的直接对象引用(IDOR)](https://owasp.org/www-project-web-security-testing-guide/latest/4-Web_Application_Security_Testing/05-Authorization_Testing/04-Testing_for_Insecure_Direct_Object_References) 的影响。通过手工检查，符合性测试可能集中在编码风格上，并在定义用户和对象模型时确认通用唯一标识符(UUIDs)的使用。它还可能证实:

*   在执行任务之前，应用程序会检查有效的对象和登录的用户
*   URL 参数模糊不清
*   错误消息是描述性的，但不够模糊，无法正确猜测软件架构和漏洞

### 供应链漏洞扫描

为了避免重新发明轮子，软件开发人员依赖于预先构建的外部组件。一个组织继承了其软件组件的软件供应链。这种继承带来了可能产生深远后果的风险。

减轻这种风险的一个很好的方法是在您的代码库中加入自动化的安全漏洞扫描和咨询。至少，大多数安全漏洞扫描器可以扫描[十大开放 Web 应用安全项目(OWASP)漏洞](https://owasp.org/www-project-top-ten/)。漏洞也是从安全公告、聊天和独立研究中整理出来的。

### 符合性审计

合规性审计审查您在记录中对成文政策的实施与官方政策的符合程度。审计可能从您的代码库开始，检查错误配置和暴露的秘密。然后，审计可以继续审查您的构建、部署的工件和基础设施。

确保为资源提供最低权限，并通过身份验证和适当的角色进行访问。当不再使用时，应该对它们进行逻辑分割和移交。其他检查应验证数据存储生命周期，并确保个人身份信息不以明文形式存储或根本不存储。此外，必须有写得很好的回切和故障转移过程文档，并且定期严格地演示这些技术。

您可以通过使用能够提供端到端跟踪、管理和分析的工具来审核您所执行的测试，从而进一步保证产品的合规性。确保捕获所有系统活动的日志可用，以提供可观察性并简化系统审计练习。

## 如何自动遵守 CI/CD

在您的软件开发组织中自动化遵循的最好方法是实现一个全面的[持续集成和持续交付(CI/CD)](https://circleci.com/continuous-integration/) 实践。使用自动化的 CI/CD 管道可以加速团队的发展。您还可以将 [DevSecOps](https://circleci.com/blog/security-best-practices-for-ci-cd/) 和合规最佳实践整合到您的工作流程中，以实现持续合规。

许多 CI/CD 提供商鼓励使用[配置-代码](https://circleci.com/blog/configuration-as-code/)方法来定义管道。这种方法有助于团队

您的团队还可以为 CI/CD 管道实施基于角色的[访问控制策略，以及分支和标记过滤器，以限制谁可以更改管道配置或触发发布以及在什么条件下发布。大多数服务还提供全面的](https://circleci.com/blog/access-control-cicd/)[审计日志](https://circleci.com/docs/security/#audit-logs)，允许组织检索系统事件的详细记录以用于报告目的。

除了可追溯性和访问控制的好处，持续集成使团队能够将一系列第三方安全和漏洞扫描工具整合到他们管道的构建和测试阶段。您可以使用 [CircleCI 的 orbs](https://circleci.com/blog/devsecops-and-circleci-orbs-security-focused-ci-cd-best-practices/) 将流行的安全和漏洞扫描工具整合到您管道的所有阶段，只需几行代码。

例如，orbs 使得集成 [SAST 和 DAST 扫描](https://circleci.com/blog/sast-vs-dast-when-to-use-them/)进行漏洞和合规性管理变得非常简单。

在管道的构建阶段，您可以使用 [Snyk orb](https://circleci.com/developer/orbs/orb/snyk/snyk) 运行静态应用程序安全测试(SAST)作业，以[检测依赖漏洞、合规性和许可问题。](https://circleci.com/blog/security-with-snyk-in-the-circleci-workflow/)

在部署阶段，您可以运行动态应用程序安全测试(DAST)作业来捕获生产中的运行时漏洞。像 [Deepfactor](https://circleci.com/developer/orbs/orb/deepfactor/deepfactor) 和 [StackHawk](https://circleci.com/developer/orbs/orb/stackhawk/stackhawk) 这样的工具可以根据实际的应用程序行为，提供对应用程序代码、包依赖关系、web APIs 以及对常见漏洞和暴露( [CVEs](https://owasp.org/www-project-top-ten/) )的遵从性的优先洞察。

对于数据保护要求严格的高度管制行业的团队，您可以选择[在防火墙后的场所](https://circleci.com/pricing/server/)安装持续集成工具，以增加安全性。或者，如果你更喜欢混合方法的灵活性，你可以建立自托管的运行器来[在私有基础设施上运行特定的工作](https://circleci.com/blog/ci-on-self-hosted-infrastructure/)。借助此选项，您可以自动执行冗余步骤，例如在高峰需求期间自动扩展，或者在发生区域性故障时切换到其他数据中心。

## 结论

软件交付的法规遵从性要求定义了组织的安全性和业务实践必须满足或超过的最低要求。不同的合规性法规适用于不同的行业，但通常提倡最低特权原则、基于角色的访问控制、个人身份数据的隐私和保护、敏捷和全面的测试，以及对可观察性的定期审计。

通过持续的集成和交付，您可以减少软件开发和交付中的遵从性开销。CircleCI 促进了合规性最佳实践，如分配基于角色的精细权限、自动化安全和漏洞扫描、在私有基础架构上运行作业，以及为系统中的事件和 CI/CD 管道中的作业生成审计日志。作为一家 [SOC-2 认证的](https://circleci.com/blog/continuous-integration-that-you-can-trust/)和 [FedRAMP 授权的](https://circleci.com/blog/modernizing-federal-devops-circleci-becomes-first-continuous-integration-tool-with-fedramp-authorization/)云提供商，CircleCI 为高度监管行业中的组织提供了快速移动并保持安全性和合规性所需的信心。

为了开始自动化您的合规实践并消除开发流程中的瓶颈，[立即注册免费的 CircleCI 计划](https://circleci.com/signup/)。