# Kubernetes 1.18.0 出来了:现在怎么办？

> 原文：<https://www.fairwinds.com/blog/kubernetes-1.18.0-is-out-now-what>

 最新的 [Kubernetes 版本](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.18.md)现已上市。你可以在[变更日志](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG/CHANGELOG-1.18.md#changes-by-kind)中找到更新和错误修复的完整列表。发布的主要主题包括`deprecations`、`metrics`、`node`和`kubectl`。如果你是 Kubernetes 的新手或者正在考虑升级，你可能会想，现在该怎么办？

我们 Fairwinds 团队的存在是为了帮助工程师、开发人员和基础设施团队在 Kubernetes 上取得成功。在这种情况下，大多数时候我们建议客户在最新版本之后运行 N-1 或 N-2。为什么？有几个原因。

你必须在长期稳定性和尖端技术之间取得平衡。采用 Kubernetes 可以让您大规模受益于容器技术。随着您部署越来越多的容器，对 Kubernetes 的需求也会增加。但是对于 Kubernetes 的每个新版本，您都需要确保对服务或操作者的任何升级在您的生产环境中是兼容的和安全的。这仅仅需要发布、测试和修补最新版本。

接下来，如果使用托管的 Kubernetes 服务(GKE、EKS、阿拉斯加)，他们有时会落后，在某些情况下，几周到几个月都不会推出最新版本。他们需要相同的时间来发布、测试和修补。

最后，构建在 Kubernetes 上的云原生应用可能需要更改以支持最新版本。你需要考虑需要做什么改变，以及需要多长时间。您还需要保护您所依赖的关键功能。跳到最新版本可能意味着您会失去一些功能。

### **先审核，后实施**

我们的 [Kubernetes 升级到最新版本的最佳实践](https://www.fairwinds.com/kubernetes-best-practices-comprehensive-white-paper)是给发布时间进行测试(当然，除非你有一个超级酷的创新实验室，在这种情况下，测试它，修复并与开源社区共享！).

我们在客户的整个 Kubernetes 之旅中支持他们的一种方式是，我们的 R&D 团队在向客户的生产环境推出新版本之前审查新版本。这使我们能够了解每个版本的优缺点，以便根据客户的应用、服务和基础设施，在正确的时间进行升级。这也意味着依赖特定功能的公司不会升级，如果该功能不再正常工作，他们会感到恐慌。

我们不害怕升级，相反，我们准备好并热爱升级。当升级的时候，确保你已经检查了版本。如果您没有时间，我们的 [SRE 团队](https://www.fairwinds.com/contact-us)随时可以提供帮助。