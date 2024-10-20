# DevSecOps -安全 CI/CD | CircleCI

> 原文：<https://circleci.com/blog/devsecops-and-circleci-orbs-security-focused-ci-cd-best-practices/>

DevSecOps 是从构思到部署安全开发应用程序和基础设施的理念。它要求在开发生命周期的所有阶段考虑安全风险。虽然 DevOps 团队过去一直专注于自动化他们的应用程序的构建、测试和部署，但 DevSecOps 包括自动化安全实践，以允许团队在不损失速度的情况下提高安全性。

为了构建、部署和测试你的应用，你的 [CI/CD](https://circleci.com/continuous-integration/) 管道访问你的技术栈中的每一个资源。这些资源包括分析密钥、代码签名凭证、安全机密、专有代码和数据。您必须保护您的 CI/CD 管道，这样您就永远不会将受保护的信息暴露给不需要的人。如果没有经过深思熟虑的努力来保护您的管道，这些资源中的任何一个都是潜在的安全漏洞。

最近，我们为 CI/CD 定义了三个重要的[安全最佳实践类别。它们包括保护您的管道配置、保护代码和 Git 历史分析，以及实施安全策略。](https://circleci.com/blog/security-best-practices-for-ci-cd/) [CircleCI orbs](https://circleci.com/orbs/) 只需几行代码，您就可以轻松地将集成添加到工具和服务中，以解决您管道中的这些类别。orb 是 CircleCI config 的可重用、可共享的开源包，支持这些服务的即时集成。借助 orbs，您可以获得一个保护管道的开箱即用的解决方案。

## 使用这些 orb 保护您的 CI/CD 管道:

当您运行 Kubernetes 集群时，在开发的早期检测漏洞和错误配置漂移。

[Anchore](https://circleci.com/developer/orbs/orb/anchore/anchore-engine)
将 Anchore 图像扫描添加到任何 CircleCI 工作流程中。

[Aqua Security](https://circleci.com/developer/orbs/orb/aquasecurity/microscanner)
通过精细的容器级控制和报告，修复管道中的漏洞并强制执行法规合规性。

[AWS 参数存储](https://circleci.com/developer/orbs/orb/circleci/aws-parameter-store)
管理并从 AWS 参数存储中加载环境机密。

[对比安全性](https://circleci.com/developer/orbs/orb/contrastsecurity/verify)
为您的 CircleCI 执行环境添加自定义代码和开源库的漏洞发现。

[CryptoMove](https://circleci.com/developer/orbs/orb/cryptomove/cryptomove)
通过在 CryptoMove 上存储您的加密密钥和秘密，加强版本管理和共享。

[福塔尼克斯](https://circleci.com/developer/orbs/orb/fortanix/sdkms-cli)
通过安全的分布式密钥管理服务，存储和管理秘密、API 密钥和其他敏感数据。

将 OSS 合规性和漏洞检查集成到您的 CI/CD 工作流程中。

[GCP Bin 授权](https://circleci.com/developer/orbs/orb/circleci/gcp-binary-authorization)
配置 Google 的二进制授权服务，对部署的容器映像进行签名和认证。

[NeuVector](https://circleci.com/developer/orbs/orb/neuvector/neuvector-orb)
在构建期间使用 NeuVector 扫描容器映像的漏洞，并在运行时保护 Kubernetes 容器部署。

[Probely](https://circleci.com/developer/orbs/orb/probely/security-scan)
将 Probely 集成到您的 CI/CD 管道中，持续扫描您的 web 应用程序的安全漏洞。

[Snyk](https://circleci.com/developer/orbs/orb/snyk/snyk)
查找、修复并监控应用程序开源依赖项和容器映像中的漏洞。

通过动态应用程序安全测试，发现、筛选并修复应用程序安全漏洞。

[扭锁](https://circleci.com/developer/orbs/orb/twistlock/twistcli-scan)
将扭锁漏洞和合规问题扫描集成到您的 CircleCI 工作流中。

[WhiteSource](https://circleci.com/developer/orbs/orb/whitesource/vulnerability-checker)
扫描您的产品中已知的开源漏洞，并接收可行的修复建议。

## 我们的合作伙伴在说什么:

“随着容器应用的不断加速，企业需要一种解决方案，让他们能够在整个应用程序生命周期中全面自动化安全性。我们非常高兴与 CircleCI 合作，继续弥合开发和安全团队之间的差距，并帮助他们安全地构建和运行应用程序，而不会牺牲速度或性能。

“完整的生命周期漏洞管理解决方案对 NeuVector 客户至关重要。借助 NeuVector CircleCI orb，开发人员和 DevOps 团队可以通过在 CircleCI 管理的构建过程中触发容器映像扫描，将自动化安全性构建到他们的 CI/CD 管道中。这使得 NeuVector 的客户能够在构建、交付和运行阶段执行安全策略，”NeuVector 的联合创始人兼首席技术官 Gary Duan 表示。

“在 CryptoMove，我们希望让开发人员和团队能够轻松地将他们的数据进出我们的产品移动目标防御。在 CircleCI 及其开发团队的帮助下，我们构建了 orb，使开发人员能够在 CircleCI 构建和部署过程中，无缝地从我们的移动目标防御中读取敏感信息作为环境变量。

## 你能做什么

你还想做些什么来保护你的管道，而这在 orb 上是不可行的吗？orb 是开源的，所以在一个现有的 orb 上增加功能只是获得你的 PR 批准和合并的问题。查看 [orbs 注册表](https://circleci.com/developer/orbs)中所有可用的 orbs。你是否有一个用例，你觉得它不同于当前的一组安全聚焦的 orb？你可以[自己创作一个](https://circleci.com/blog/how-to-make-an-easy-and-valuable-open-source-contribution-with-circleci-orbs/)并贡献给社区。我们甚至发布了为 orb 创建自动化构建、测试和部署管道的最佳实践([第 1 部分](https://circleci.com/blog/creating-automated-build-test-and-deploy-workflows-for-orbs/)和[第 2 部分](https://circleci.com/blog/creating-automated-build-test-and-deploy-workflows-for-orbs-part-2/))来帮助您。

为了保护您的管道，让您的团队利用第三方服务，消除内部开发的需要。有了 orb，您的团队只需要知道如何使用这些服务，而不需要知道如何集成或管理它们。