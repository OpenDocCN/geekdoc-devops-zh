# CI/CD 管道的自动密钥轮换- CircleCI

> 原文：<https://circleci.com/blog/securing-ci-cd-pipelines-with-circleci-contexts-rest-api/>

使用 CircleCI [contexts API](https://circleci.com/docs/contexts/#secure-environment-variable-creation-deletion-and-rotation) ，您可以节省团队的宝贵时间，同时增强安全性实践。我们知道维护贵组织的安全至关重要。有严格的[合规要求](https://circleci.com/blog/automate-software-delivery-compliance/)要满足，比如 FedRAMP 和 GDPR，还有越来越多的安全威胁和供应链漏洞要防范。在 CircleCI 上，客户可以[以多种方式主动管理安全需求](https://circleci.com/blog/security-best-practices-for-ci-cd/)，比如管理您组织的敏感密钥或机密。

机密用于构建、测试和跨多个项目部署。在维护它们的安全性的同时在您的团队中共享它们可能会很困难。在本文中，我们将介绍如何使用 contexts API 作为您团队安全策略的一部分，帮助您根据最佳实践自动执行秘密轮换，从而提供尽可能多的保护。

## 用 CircleCI 上下文管理秘密

为了充分利用 CircleCI 上的 CI/CD 工作流，开发人员连接到私有数据存储和访问受限服务。团队甚至可以直接连接到他们的基础设施来推动生产工件。CircleCI 使您能够以上下文的形式安全地存储凭证和其他秘密，以便在构建期间使用。

尽管 CircleCI 和您的组织投入了无数的时间和智慧来保护这些价值观的安全，但没有一个系统是完美的。良好的机密保健包括定期轮换机密以及在某人的访问权限被撤销时轮换机密。如果一名员工离开您的公司，或者如果您组织中的某人被调到另一个部门，他们有权访问的所有机密都应该轮换。

你不知道的秘密曝光怎么办？日志中的异常可能包含一个秘密值，将它暴露给不应该访问它的人。定期轮换静态机密有助于保护您的组织免受风险。

我们努力保护您的秘密，包括静态和传输中的加密、构建日志中秘密值的屏蔽，以及 CircleCI 员工对秘密数据存储的访问记录。我们是首款符合 FedRAMP 严格的安全和隐私 NIST 标准的 CI/CD 工具，此外还符合 SOC 2 Type II 标准。

## 秘密轮换的建议

CircleCI 建议自动进行秘密轮换，以防止人为错误，并确保定期进行。以下是在 CircleCI 中使用机密的一些准则:

*   使用最小特权原则。只对您传递给 CircleCI 的秘密授予构建和部署所必需的权限。
*   使用 [CircleCI CLI](https://circleci.com/docs/local-cli/#context-management) 或 API 在 CircleCI 上下文中自动添加和更新机密。
*   根据团队的独特需求和风险状况，安排定期秘密轮换。

本文中描述的指导方针尤其适用于大型组织，但即使是小型开源项目的维护者也可能希望提高他们的保密水平，以防止流行库遭到破坏。通过在设计和自动化方面的一些前期工作，无论您的组织规模如何，您都可以加强您的安全状况。

有关如何开始使用 API 或 CLI 自动执行秘密轮换和创建环境变量的更多信息，请查看我们的[上下文文档](https://circleci.com/docs/contexts/#secure-environment-variable-creation-deletion-and-rotation)。