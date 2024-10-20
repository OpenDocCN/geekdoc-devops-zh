# 是忽必烈过度杀戮吗？

> 原文：<https://www.fairwinds.com/blog/is-kubernetes-overkill>

 是不是忽必烈过度杀戮？

这是一个很常见的问题，尤其是因为大多数中等规模的 SaaS 网络和电子商务公司总有一天会决定离开 Heroku。( [最终大家都离开了龙。](http://blog.reactiveops.com/the-tenth-rule-of-cloud-based-infrastructure) )决定是这样的——选择 AWS、Docker Swarm 或其他一些“简单”的解决方案，或者大胆地选择 Kubernetes。

从 Heroku 迁移到 AWS、Docker Swarm 或本土解决方案通常会带来可避免的迁移创伤。这是因为这些解决方案看起来比 Kubernetes 简单，但从长远来看，总是更具限制性、更具挑战性且成本更高。Heroku 退出的恐怖故事层出不穷，但许多公司对接受 Kubernetes 犹豫不决。让我们来看看为什么。

## **Kubernetes:可扩展性故事**

中小型公司不选择 Kubernetes 的一个最常见的原因与规模有关。“我只有一个 Rails 应用程序，”CTO 可能会说，“而普通的老 EC2 虚拟机会给我我需要的东西。Kubernetes 是关于规模的。太多了，我不需要无限扩展。”

问题来了。没有基础设施需要从十个节点增加到几千个节点。(但是，您确实需要至少两个节点，因为现实是应用程序可能会崩溃。)这个可伸缩性故事是在转移视线。Kubernetes 不仅仅是关于可伸缩性。

首先，在常规 EC2 实例上运行的应用程序不会自动重启，如果你的 Rails 应用程序耗尽内存并崩溃的话；相反，有人在半夜收到传呼，不得不重启它。另一方面，Kubernetes 具有自动健康检查功能，如果您的 Rails 应用程序由于任何原因(包括内存不足或锁定)无法响应，Kubernetes 会自动重启它。使用 Kubernetes，基于分支的开发环境也很容易，但是使用 自主开发的 EC2 自动化，这几乎是不可能的。

考虑一下——通过更高的可用性、自动扩展和更丰富的特性集，您可以实现什么？

## **偶然与本质复杂性**

中小型公司不想使用 Kubernetes 的另一个常见原因？复杂。

事实是 Kubernetes 很复杂。不可否认，启动并运行可能是一个挑战。但是这种复杂性意味着有一个对它有利的论点。

20 世纪 60 年代，弗雷德·布鲁克斯写了一部计算机科学领域的开创性著作，名为 [*【神话中的人月*](https://www.amazon.com/Mythical-Man-Month-Software-Engineering-Anniversary/dp/0201835959/ref=sr_1_1?ie=UTF8&qid=1502989277&sr=8-1&keywords=mythical+man+month) 。在这篇文章中，他讨论了他的团队为构建 IBM 大型机 OS/360 所做的努力，并描述了两种类型的系统复杂性:偶然的和本质的。偶然是随机引入的那种(坏的那种)，本质是完成一项工作所必需的那种(好的那种)。

ECS 和 Docker Swarm 表面上看起来更简单，但它们都有更多的意外复杂性——而且它们把这种复杂性强加给了你。这种复杂性一开始并不明显。那么偶然复杂性到底是什么样子的呢？首先也是最重要的，你需要补偿系统做不到的。例如，ECS 需要您编写大量代码才能使其可用(有时需要数万行代码)。这些工具的工作方式也有结构上的限制，并且有一个陡峭的学习曲线。

Kubernetes 反过来又具有较低的偶然复杂度和较高的本质复杂度(实现你实际想要实现的事情所需的复杂度)。Kubernetes 的强大之处在于它是 Google 的第三代容器管理，而 Swarm 和 ECS 才刚刚起步。

## **不变税**

一些公司不太担心 Kubernetes 的复杂性，而是担心这种复杂性可能得不偿失。他们担心他们的团队将不得不支付 Kubernetes“税”,并且不会得到足够的价值来证明开销的合理性。

为了避免这种“税收”，他们雇佣了一个聪明的开发团队，看看他们能想出什么。你猜怎么着？如果你允许，他们会建立一个基于 Kubernetes 的平台。如果没有，他们会试图从零开始建立类似 Kube(我们称之为“faux netes”)的东西，这会给公司留下技术债务。(当你自己构建的时候，你总是得到一个满是 bug 的、更贵的 Heroku 版本。这就是云计算基础设施的第十条规则[](http://blog.reactiveops.com/the-tenth-rule-of-cloud-based-infrastructure)。)

当您的目标是构建产品时，为什么要将有限的开发资源部署到运营任务中？如果你不需要或者没有必要建造自己的 Heroku，为什么要雇佣 DevOps 工程师呢？ ***在我们的下期帖子中了解一下:*** [***如何让********成为你的开发者的硬汉*****](http://blog.reactiveops.com/how-to-make-your-developers-badass-with-kubernetes-and-devops-as-a-service)