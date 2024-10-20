# Kubernetes 安全性最佳实践

> 原文：<https://www.fairwinds.com/blog/kubernetes-best-practices-for-security>

 Kubernetes 是主流的容器编排解决方案。Kubernetes 的天才之处在于它能够为您提供一个框架来灵活地运行分布式系统。然而，它引入了令人难以承受的复杂程度。通过遵循 Kubernetes 在安全性、可靠性、效率和监控方面的最佳实践，团队可以为成功过渡做好准备。在一系列的博客文章中，我们将从安全性开始讨论这些主题。

## **Kubernetes 最佳实践:安全性**

Kubernetes 抽象出足够的基础设施层，以便开发人员可以自由部署，同时运营团队保留对重要治理和风险控制的访问权。挑战在于 Kubernetes 的新开发团队可能会忽略一些关键的安全特性。通常，最简单的工作方式就是降低安全性。

让我们看看三个常见的安全挑战，以及如何克服它们。

## **好的突发与坏的突发**

Kubernetes 能很好地应对交通突发——不管是好是坏。如果你看到一个合法的流量爆发，Kubernetes 将扩大规模，以满足需求的增长。您的应用程序将消耗集群中的更多资源，而不会降低性能。这是一个很大的好处。然而，在拒绝服务(DoS)攻击的情况下，Kubernetes 将做完全相同的事情，而您将为这种流量过载付出代价。

### K8S 最佳实践# 1–针对以下方面设定限制:

*   每个 IP 地址的并发连接数
*   每个用户每秒、每分钟或每小时可以发出的请求数
*   请求主体的大小
*   并为各个主机名和路径调整这些限制

## **授予安全访问级别**

部署新应用程序或供应新用户的最简单方法是放弃管理权限。但这也是最危险的方式——如果攻击者获得了该帐户的访问权限，他们就可以访问所有内容。

### K8S 最佳实践# 2–采用基于角色的访问控制(RBAC)来遵守最小特权原则。

RBAC 允许您授予用户对 Kubernetes API 资源的细粒度访问权限。您应该使用`Roles`或`ClusterRoles.`定义访问配置文件，使用`Roles,`您将授予对单个名称空间的访问权限。使用`ClusterRoles,`，你可以授权访问没有名称空间的资源，比如`Nodes`和`PersistentVolumes`以及所有有名称空间的资源。 

虽然 RBAC 配置可能会令人困惑和冗长，但像 [rbac-manager](https://github.com/FairwindsOps/rbac-manager) 这样的工具可以帮助简化语法。这有助于防止错误，并为谁有权访问什么提供更清晰的感觉。

最终结果？通过只授予工作负载完成工作所需的权限，可以限制攻击者对 Kubernetes 环境造成的损害。

## **保守 Kubernetes 的秘密，秘密**

如果您使用的是 Kubernetes 基础设施即代码(IaC)模式，那么拥有一个完全可复制的环境会让您受益匪浅。但是有一个问题——您的基础设施可能包括 Kubernetes `Secrets,`,它存储和管理敏感信息，比如密码、OAuth 令牌和 ssh 密钥。您不应该将`Secrets`添加到您的 IaC 存储库中。

将您的 Kubernetes `Secrets`签入到您的基础设施即代码存储库中，以便您的构建是 100%可重复的，这很有诱惑力。但如果你在乎安全，就不要。一旦签入，您的`Secrets`将永久地暴露给任何能够访问您的 Git 存储库的人。

### K8S 最佳实践# 3——在将秘密签入基础设施存储库之前，对它们进行加密。

解决方案是分割差异:加密你所有的秘密，这样你就可以安全地将它们签入你的存储库，而不用担心暴露它们。然后，您将只需要访问一个加密密钥来“解锁”您的 IaC 存储库，并拥有完全可复制的基础设施。像 Mozilla 的 SOPS 这样的开源工具可以在这方面有所帮助。

您可以通过查看我们如何为客户的托管 Kubernetes 部署实施安全性来阅读更多的 [Kubernetes 安全性最佳实践](/kubernetes-security-best-practices)。

您还可以查看 [Kubernetes 关于效率的最佳实践](/blog/kubernetes-best-practice-efficient-resource-utilization)。寻找一个完整的 Kubernetes 治理平台？Fairwinds Insights 是免费的。[今天开始](/coming-soon)。

[![Download the Kubernetes Best Practices Whitepaper](img/ff6b63b515c18edd13b80bc25f17c2de.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/e68d92d3-c876-4525-b775-6123e46c7212)