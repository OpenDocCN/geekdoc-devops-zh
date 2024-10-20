# DevSecOps -安全 CI/CD -移动目标防御| CircleCI 和 CryptoMove

> 原文：<https://circleci.com/blog/dynamically-pull-in-your-stored-sensitive-data-with-the-cryptomove-orb/>

DevSecOps 可以定义为 DevOps 的长期实践，将系统的安全性作为中心租户。使用传统的 DevOps，开发人员已经找到了使软件开发生命周期变得更加容易的方法。一个常见的实践是[持续集成](https://circleci.com/continuous-integration/)和持续部署(CI/CD ),它自动化了测试和部署新版本代码的手动步骤，节省了无数的工程时间，这些时间可以用于进一步增强产品。

虽然 DevOps 理所当然地获得了许多软件开发团队的关注，但是安全性经常被忽视，因为它“妨碍了”发布特性。DevOps 在某种程度上与安全性相冲突的这种范式已经导致了薄弱的软件实践，比如将应用程序秘密存储在代码中，或者允许公司中的任何人查看构建。因此，DevOps 引入了新的威胁媒介，使公司及其专有软件和数据变得不必要的脆弱。例如，Jenkins，一个团队常用的开源 CI/CD 服务器，已经成为黑客攻击的目标。

我们建议不要认为开发运维的实践是针对开发速度的，安全应该被认为是团队发布新版本软件的主要租户，因此有了术语 DevSecOps。速度虽然重要，但如果它意味着你所有的努力都落入坏人之手，那就没有意义了。此外，实施安全最佳实践并不像人们通常认为的那样耗费时间或资源。

为了节省时间和提高安全性，您可以使用第三方 CI/CD 提供商(如 CircleCI)和数据安全供应商(如 CryptoMove)来部署您自己的 DevSecOps 管道，其资源比 Jenkins 服务器少得多。CryptoMove 的工作原理是通过移动目标防御来存储您的敏感数据，这是一项使您的数据保持移动的专利技术，使攻击者更难锁定它。CirlceCI 通过管理您的构建和部署服务器来工作，因此您所需要的只是一个在 YAML 编写的[配置文件](https://circleci.com/docs/configuration-reference/)，它声明了用于测试和部署您的软件新版本的命令。CircleCI 和 CryptoMove 都可以“本地”部署，这利用了公司的网络来增加安全性。

你可以将 CryptoMove 和 CircleCI 与 [CryptoMove orb](https://circleci.com/developer/orbs/orb/cryptomove/cryptomove) 一起使用。有了这个 [orb](https://circleci.com/orbs/) ，你可以在 CryptoMove 中存储 API 秘密和数据库密码等敏感数据，并在 CircleCI 构建和部署中动态地提取它们。CircleCI 作业是短暂的，这意味着在构建完成后，您的敏感数据会被垃圾收集。这消除了黑客以前用来访问敏感信息的关键攻击媒介。

与所有安全性一样，无论是物理安全性还是代码安全性，一个良好、可靠的基础设施都需要实现许多迭代步骤来保护您的数据。然而，这并不意味着实现这些控制会妨碍您的软件开发生命周期。相反，您可以用一个健壮的 DevSecOps 范例来完成这两个任务。

* * *

这篇文章是我们制作的关于 DevSecOps 的系列文章的一部分。要阅读本系列的更多文章，请单击下面的链接之一。