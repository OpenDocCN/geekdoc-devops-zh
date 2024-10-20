# gitop For Secrets Management | Harness | Harness

> 原文：<https://www.harness.io/blog/gitops-secrets>

学习管理 Git、GitOps 和 GitOps 秘密管理的基础知识。

当 Linus Torvalds 在 2005 年创建 Git 时，软件行业转向了更加分布式和声明性的工作模型。与传统的中央版本控制系统不同，Git 是完全分布式的，可以在开发人员希望的任何地方本地托管。这允许诸如分支、合并和拉请求之类的技术。所有这些术语对开发人员来说都是常识，因为 Git 成为了世界标准，Git 库成为了开发人员项目的唯一来源。

但是新的世界标准提出了一个新的挑战:在一个 Git 存储库作为项目唯一真实来源的世界里，你如何管理 Git 秘密？

像 API 密钥、OAuth 令牌、证书和密码这样的秘密是非常敏感的。传统智慧会说不要把它们放在 Git 存储库中，但是秘密仍然会进入危险暴露的存储库，因为这很方便，它们被硬编码到文本文件中，或者开发人员认为它是安全的，因为存储库是私有的，秘密并不总是安全的。事实上，Git 存储库是可以克隆的，一旦被克隆，所有存储的东西都会随之而去。私人储存库不能提供足够的保护来确保某人不会得到敏感的秘密。开发人员需要密切关注他们的秘密管理流程，以降低违规风险。

GitOps 增加了 GitOps 秘密管理的复杂性。GitOps 是开发人员高效部署软件的直观方式，并且非常依赖 Git repos 作为项目的唯一真实来源。为了成功完成基于 GitOps 的部署，开发人员将需要访问这些秘密。组织不希望通过提出复杂的机密管理过程来阻碍开发速度，但同时，不当的机密管理可能会给业务带来巨大的风险。

## **GitOps 基础知识和最佳实践**

GitOps 使用存储在版本控制系统(如 Git)中的系统所需配置来管理软件部署。开发人员对表示所需状态的配置文件进行更改，而不是通过 UI 或 CLI 进行更改。当进行比较时，存储在 Git 中的期望状态和系统的实际状态之间的差异表明并没有部署所有的更改。这些变更可以通过标准的修订控制流程进行审查和批准，例如拉式请求、代码审查和合并到主文档。当更改被批准并合并到主分支时，操作员软件进程负责根据存储在 Git 中的配置将系统的当前状态更改为所需状态。

GitOps 不需要特定的工具集，但这些工具必须提供以下标准功能:

*   对存储在 Git 中的系统的期望状态进行操作。
*   检测期望状态和实际状态之间的差异。
*   在基础结构上执行所需的操作，以使实际状态与所需状态同步。

在 GitOps 的理想实现中，不允许对系统进行手动更改，对配置的所有更改都必须对存储在 Git 中的文件进行。在极端情况下，改变系统的许可仅授予操作员软件进程。然后，基础设施和运营工程师在 GitOps 模型中的角色从执行基础设施变更和应用程序部署转变为维护 GitOps 自动化并帮助团队审查和批准变更。

## **如何立方吉卜赛秘密工作**

在最基本的层面上，Kubernetes 中的“秘密”是一种安全存储敏感数据和信息的方法，比如私钥、密码或身份验证令牌。信息很容易访问，并且仍然受到保护，不会受到可能危及安全的情况的影响。默认情况下，在 Kubernetes 中运行的容器化应用程序通常需要凭证才能正确运行并与其他基础设施或应用程序交互。

Kubernetes 提供了 [**秘密**](https://kubernetes.io/docs/concepts/configuration/secret/) 作为[对象](https://kubernetes.io/docs/concepts/overview/working-with-objects/kubernetes-objects/)来存储在应用程序代码中不可见的机密信息。这种机制使得在管理 Kubernetes 自身的运营时，可以使用意外暴露风险较低的敏感数据。从基本层面来看，创建 Kubernetes secrets 的过程相当简单:

*   受信任的个人或来源通过为其创建特定名称并输入机密数据来定义秘密。Kubernetes 操作员从来不会看到实际的数据本身，只会看到名称。
*   一旦工作负载需要访问和使用机密数据，它将引用之前创建的特定名称来检索它

### **在 GitOps 中处理 Kubernetes 的秘密**

Git 依靠 Git 工作流和自动化来保持存储库中的配置在更新时与生产环境同步。但是在 Git 中以任何容量保存敏感数据都会带来安全风险，即使存储库是私有的，有严格的访问控制。如果一个秘密以纯文本的形式提交给 Git 存储库，它必须被撤销并且不再被使用。然而，在 GitOps 中仍然有处理秘密的方法来减轻安全风险。

对于那些希望采用 GitOps 方法，但又想在不使其敏感信息面临风险的情况下为其应用程序提供机密的人，让我们来看看在 GitOps 中管理机密的两种流行方法:

1.  将加密的秘密存储在 Git 存储库中，并利用自动化将它们解密并呈现为 Kubernetes 秘密
2.  将对秘密的引用存储在 Git 存储库中，而不是将加密的秘密直接存储在 Git 中。然后在将它们呈现为 Kubernetes 秘密之前，利用自动化根据它们的引用获取实际的秘密。

## **方法 1:加密的秘密**

Bitnami 的 Sealed Secrets 和 T2 的 Mozilla 的 SOPS (Secrets OPerationS) 是在 Git 存储库中存储加密秘密的两个最受欢迎的开源选项，并采用了类似的方法。

这两种工具都有各自的优点、缺点和局限性，我们将一一介绍。

### **比特纳米密封的秘密**

Bitnami Sealed Secrets 以易于使用而闻名，它利用公钥加密技术通过两个核心步骤来加密机密:使用源代码控制来存储机密，使用 Kubernetes 集群中的解密密钥来解密机密。加密在您的计算机上进行，而解密在 Kubernetes 集群上由控制器[进行。CLI 工具 Kubeseal 使加密机密变得简单，因为它可以访问集群来检索加密密钥。](https://kubernetes.io/docs/concepts/architecture/controller/)

还有一个自定义资源，或者 Kubernetes API 的扩展，称为 **SealedSecret** ，它保存加密的信息，以便控制器可以解密它并创建一个 Kubernetes 秘密。这个密封的秘密控制器充当“守卫”，在每个名称空间的集群中寻找任何密封的秘密。这意味着每次新的密封秘密被创建，控制器将自动检测它们。

Bitnami Sealed Secrets 的主要优势是易用性，并且不需要使用单独的 Secrets Manager，如 Hashicorp Vault。也就是说，您仍然需要手动加密每个秘密。需要指出的主要限制是，该解决方案仅适用于 Kubernetes，并且在有大量集群的情况下变得难以大规模管理。

### **Mozilla SOPs**

SOPS (Secrets OPerationS)是一个更加灵活的 CLI 加密和解密工具，并不局限于 Kubernetes 的用例。这个工具已经存在了一段时间，并在密钥库是标准的时候流行起来。鉴于它是一个 CLI 工具，在原生 Kubernetes 环境中有效地使用它可能效率较低，并且需要额外的工具。这些工具包括使用 Helm 等工具的插件，使用 sop 构建定制容器映像的额外工作，以及支持使用它的扩展，如 Argo CD。

SOPS 支持多种输入格式，如 YAML、ENV、INI、二进制和 JSON。它支持与 Hashicorp Vault、Azure Key Vault、GCP KMS 和 AWS KMS 等密钥管理系统(KMS)集成，以提供加密密钥来保护机密，而不是将机密直接存储在 KMS 内。当没有 KMS 可用时，可以选择从命令行使用一个非常好的隐私(PGP)密钥对。这是一种独特的方法，提供了跨不同开发环境的灵活性。例如，团队可以在沙盒环境中使用 PGP 密钥对，然后在生产环境中使用 KMS，而无需更改底层工具。

与密封机密的工作方式类似，机密由开发人员手动加密。它使用密封保密工作流程，但使用 sop 进行加密。它从密钥库中读取解密密钥，然后使用 SOPS 二进制文件解密秘密，Argo CD 等工具可以使用 SOPS 插件运行该二进制文件。

### 加密秘密的缺点

在高层次上，开发人员需要手动以纯文本形式获取机密，对其进行加密，并将其存储在 Git 中。然而，这为提交代码的人的身份暴露创造了机会，因为遗留在 Git 日志中的信息可以很容易地追溯到提交者。这种方法的另一个缺点是，如果任何加密的密钥遭到破坏，就很难追溯性地找到并撤销所有的密钥。

值得注意的是，由于手动加密过程和所需的人工干预水平，sop 和 SealedSecrets 都存在扩展问题。如果可能的话，使用 KMS 可以使管理密钥变得更加容易，并为管理机密的其他选项打开了大门。

## **方法二:GitOps 秘密参考**

secrets 引用方法使用外部 secrets 管理系统来管理您的 Kubernetes 机密，并需要 GitOps 操作员将机密部署到集群。清单存储在 Git 中，以表示对存储在上述秘密管理系统中的秘密的引用。然后，GitOps 操作员将清单部署到 Kubernetes 集群。在这里，可以从秘密管理器中获取秘密，并作为 Kubernetes 秘密应用到集群。

这种方法听起来很有效——但是您如何从管理器中检索秘密并将其放入您的集群中呢？您需要在集群上安装一个操作员来与 secrets manager 交互。市场上最流行的两个工具是 [ExternalSecrets](https://github.com/godaddy/kubernetes-external-secrets) 和[Kubernetes Secrets Store CSI Driver](https://secrets-store-csi-driver.sigs.k8s.io/)。

### 外部秘密

GoDaddy 创建了 [ExternalSecrets](https://www.godaddy.com/engineering/2019/04/16/kubernetes-external-secrets/) 项目，用于将存储在外部机密管理解决方案中的机密安全地连接到 Kubernetes 集群。ExternalSecrets 社区继续增加对额外的秘密管理器的支持，使得该工具对于不同的工具集非常通用。

实际上，开发人员会将包含秘密引用的 ExternalSecret 自定义资源提交给他们的 Git 存储库。GitOps 操作员将把 ExternalSecret 部署到集群，在集群中，ExternalSecret 操作员使用引用从外部密钥管理系统中检索秘密。操作员接受秘密并创建一个 Kubernetes 秘密。ExternalSecret 资源存储在 Git repo 中是安全的，因为它实际上不包含机密信息。

### **CSI 驱动程序库秘密商店**

与 ExternalSecrets 类似，Secret Store CSI 驱动程序是一种从外部机密管理工具获取机密并将其放入 Kubernetes 集群的解决方案。该工具本身支持几个秘密管理器，但比外部秘密支持的少。为了弥补这一差距，Secrets Store CSI 驱动程序允许第三方开发自己与 Secrets Store 的连接。这种方法允许更大的灵活性，但是如果您公司使用的秘密管理工具本身不受支持，也需要手动操作。

CSI 驱动程序比 ExternalSecrets 更复杂。该解决方案使用附加到 pod 的单独卷来存储机密，而不是检索外部机密和创建机密资源。开发人员提交了带有对机密的引用的 SecretProviderClass。GitOps 操作员部署变更，然后 CSI 秘密存储操作员插件从秘密管理系统中检索秘密。操作员插件随后创建一个带有秘密的卷，该卷被附加到一个指定的 pod。

## **秘密管理的替代工具**

以下是一些值得注意的使用 GitOps 方法管理机密的附加解决方案:

## **GitOps 机密管理通过利用变得更加容易**

在决定切换到 GitOps 部署模型之前，需要仔细考虑秘密管理策略。一旦决定了策略，建立安全的 GitOps 实践可能是一个挑战。[利用 GitOps-as-a-Service](https://harness.io/blog/generally-available-harness-gitops-as-a-service) 提供企业控制，如高级 RBAC、审计跟踪和治理规则，以保持部署安全和合规。

Harness GitOps GitOps 即服务易于实施和使用，几乎不需要任何工程维护和故障排除工作。如果你有兴趣尝试 Harness GitOps，请申请完整的演示。