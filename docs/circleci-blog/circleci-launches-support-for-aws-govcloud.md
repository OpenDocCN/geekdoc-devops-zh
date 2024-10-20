# CircleCI 推出对 AWS GovCloud 的支持

> 原文：<https://circleci.com/blog/circleci-launches-support-for-aws-govcloud/>

我们最近发布了 CircleCI server v2.19，其中包括 AWS GovCloud 支持以及其他新的升级和性能增强。在 AWS 上运行 v2.19 的服务器安装现在配置为在 GovCloud 上工作，帮助客户满足各种监管要求，如 FedRAMP、ITAR、国防部 SRG、CJIS 和 HIPAA。现在，政府的开发者社区将能够在世界上最安全的环境中轻松快速地移动。

[GovCloud](https://aws.amazon.com/govcloud-us/) 是专为美国政府机构和其他监管行业(如医疗保健、金融、司法、公共安全和能源)打造的 AWS 服务。借助 AWS GovCloud，用户可以获得所需的控制力和信心，利用最灵活、最安全的云计算环境安全地运营业务。对 GovCloud 的支持紧随 [CircleCI 的 SOC 2 Type II 合规性](https://circleci.com/blog/continuous-integration-that-you-can-trust/)之后，在获得 [FedRAMP 认证](https://circleci.com/blog/modernizing-federal-devops-circleci-becomes-first-continuous-integration-tool-with-fedramp-authorization/)18 个月后，成为第一个也是唯一一个获得认证的[持续集成](https://circleci.com/continuous-integration/) (CI)解决方案。

CircleCI 的目标是通过为客户提供最强大的 CI 工具来帮助他们推动创新。不幸的是，为政府机构构建产品和服务的开发人员被迫依赖 CI 选项，这限制了他们的生产力，而且维护成本通常很高。

CircleCI 服务器提供了在私有基础设施的防火墙后运行 CircleCI 的访问。server v2.19 中的其他升级和性能增强包括:

*   **升级的 Nomad 实例:**我们已经将我们的 Nomad 集群从 0.5x 升级到 0.9，减少了死作业的累积，提高了服务器客户的性能。在升级到服务器 2.19 版之前，您的 Nomad 启动配置必须按照[本指南](https://circleci.com/docs/update-nomad-clients/index.html/)进行更新。如果您已经将 Nomad 外部化，请在升级前联系您的支持工程师。
*   **使用默认资源类增强控制和灵活性:**用户现在可以设置和编辑默认资源类，为开发人员提供用于其配置作业的 [CPU/RAM 选项](https://circleci.com/docs/optimizations/#resource-class)。有关更多信息，请参见我们的[在服务器 v2.19](https://circleci.com/docs/optimizations/#resource-class) 中定制资源类的指南。