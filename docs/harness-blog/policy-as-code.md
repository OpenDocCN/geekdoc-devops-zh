# 什么是作为代码的策略？-马具|马具

> 原文：<https://www.harness.io/blog/policy-as-code>

策略作为代码将决策逻辑从服务中的业务逻辑中分离出来。我们甚至可以根据应用程序或环境的上下文来制定策略决策。这篇博客文章将分享为什么我们关注作为代码的策略，以及它如何应用于软件交付。

自动化治理是我们在软件交付中讨论的一个重要话题。对于许多组织来说，这可能是投资 CI/CD 平台的驱动因素。然而，并不是每个部署都有风险。一种做法是将策略作为代码引入，允许用户在软件中编写策略决策。这是有益的，因为策略作为代码将决策逻辑从服务中的业务逻辑中分离出来。我们甚至可以根据应用程序或环境的上下文来制定策略决策。这篇博客文章将分享为什么我们关注作为代码的策略，以及它如何应用于软件交付。

## 为什么要用策略做代码？

每个企业在向消费者发布产品之前都会对其进行测试。软件服务也不例外，每个组织都有自己的实践、工具和过程来验证应用程序的健康、就绪、性能和准确性。这些允许我们对我们产品的消费者和用户做出保证。这有助于确保所有变更都符合一套标准。

在测试服务时，有两种方法可以做出决定。个人或团队可以手动决定决策是否正确，也可以自动完成决策过程。其中一个比另一个的伸缩性更好。将策略视为代码允许自动决策，使开发人员和工程师能够独立管理特性定义工作，而不会牺牲合规性。

## 什么是作为代码的策略？

作为代码的策略包括用高级语言编写代码来管理和自动化策略。高级语言依赖于策略引擎，它接受查询输入、一些数据和策略来产生查询结果。对于诸如开源的[开放策略代理](https://www.openpolicyagent.org/) (OPA)这样的策略引擎，策略是用一种称为减压阀的声明性语言来表达的。除了 OPA 之外，可选的策略引擎解决方案包括 Hashicorp 的[哨兵](https://www.hashicorp.com/sentinel)。

A Policy Engine returns a response given a query, policy, and some data.

让我们描述一下做出决策所需的三个输入。

1.  策略:策略表示对决策过程建模的编码逻辑。
2.  数据:数据是来自应用程序、环境或服务的信息。这些数据以 JSON 格式提供给 OPA。
3.  查询输入:查询根据提供的数据和上传到策略引擎的策略触发决策过程。策略引擎服务在端口上监听通过 API 调用进行的 JSON 查询。

这里有一些资源链接，可以学习更多关于 OPA 和写保单的知识。

*   开放政策代理介绍@ CloudNativeCon EU 2018: [视频](https://youtu.be/XEHeexPpgrA)、[幻灯片](https://www.slideshare.net/TorinSandall/opa-the-cloud-native-policy-engine)
*   减压阀深潜@ CloudNativeCon EU 2018: [视频](https://youtu.be/4mBJSIhs2xQ)、[幻灯片](https://www.slideshare.net/TorinSandall/rego-deep-dive)
*   网飞如何解决跨云的授权问题@ CloudNativeCon US 2017: [视频](https://www.youtube.com/watch?v=R6tUNpRpdnY)、[幻灯片](https://www.slideshare.net/TorinSandall/how-netflix-is-solving-authorization-across-their-cloud)。

像任何其他代码一样，拥有合适的过程来支持开发是很重要的。OPA 有一个 VSCode 扩展来支持 IDE 中的开发，此外还有一个完整的测试套件，允许您对策略进行单元测试。

## 作为代码用例，有哪些策略？

策略的一些常见使用案例包括:

1.  应用程序服务的授权控制:为应用程序实现细粒度的访问控制是策略代码最常见的用例之一。为了检查授权，服务对策略引擎进行 API 调用，以输出请求是否被授权。这里有一些代码为的[策略，您可以使用它在 OPA 中实现 API 授权。](https://github.com/open-policy-agent/opa#example-api-authorization)
2.  云内的基础设施供应:[对公共云资源实施特定要求](https://www.terraform.io/docs/cloud/sentinel/examples.html)，例如实例上的强制标记、防火墙和网络设置，以及供应的机器或实例类型。
3.  对于 Kubernetes 控制:您可以通过针对不同的 Kubernetes 资源(如 pods、名称空间和节点)编写策略来管理 Kubernetes。您可以确保容器图像来自受信任的注册中心。我建议看看 OPA Gatekeeper(我们在另一篇 Harness 博客文章[中描述了如何部署 Gatekeeper](https://harness.io/blog/open-policy-agent-primer/) 作为您持续交付过程的一部分)。

## 结论

创建检查是软件交付过程的主要部分。我们越早、越快地发现错误或不一致，软件交付过程就越好。这篇博文分享了一些有趣的用例，策略作为代码可以帮助自动化。如果你有兴趣尝试将策略作为代码，我推荐你看看[开放策略代理](https://harness.io/blog/policy-enforced-pipeline-opa/)。