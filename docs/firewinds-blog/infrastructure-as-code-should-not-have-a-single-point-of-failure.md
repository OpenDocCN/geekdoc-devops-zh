# 作为代码的基础设施不应该有单点故障

> 原文：<https://www.fairwinds.com/blog/infrastructure-as-code-should-not-have-a-single-point-of-failure>

 几个月前， ， 我在丹佛的一次会议上半开玩笑地谈论了瀑布的荣耀。我谈到授权开发人员部署他们自己的服务是一个多么可怕的 、 可怕的决定。这个谈话完全是故意挖苦人的。让开发人员拥有服务所有权无疑是对组织的授权 — 这是分布式服务开发的未来。

作为那次谈话的一部分，我开玩笑地提到了一位 DevOps 工程师的全部背景。这个神话般的人物 ( 谁 我取名史蒂夫)对底层 **基础设施如代码** 和部署工作负载了如指掌。没有其他人知道所有这些部件是如何工作的，史蒂夫故意严格保守这个秘密。但是当史蒂夫退出时你会怎么做？

## **事情发生的比你想象的还要多**

我发现这种情况比我讽刺的自己愿意承认的要普遍得多。这些年来， ， 我们有很多客户在他们的开发工程师 (或 团队)离开公司时找到我们，他们陷入了恐慌。 “我们 不知道我们的系统如何协同工作，我们已经几个月没有进行部署了。我们需要帮助，我们不希望这再成为单点故障。”

## **神话世界的伟大基建为代号**

Let’s consider for a moment a mythical world where your **infrastructure as code** isn't proprietary—where you have tools, documentation, and code in place so that multiple people can understand all the pieces individually and working together.  [Imagine an infrastructure with a well-oiled team](/thoughts/iaam-infrastructure-as-a-motorcycle) of people working together to maintain it, keep it up to date, and push things forward to enable innovation. In this new reality, developers have the tools they need to deploy without operations getting in the way.

如果你部署到一个有据可查的 PaaS (想想 Heroku) ，这个神话世界是可能的。但是，当一家公司对于简单的 PaaS 来说变得太大时，也可以部署到为满足您的需求而定制的记录良好的 PaaS。有了 Kubernetes(定制的 PaaS 构建器)，这是可能的，并且有一个工程师即服务团队在幕后与您的团队一起工作，这变得更加顺畅。

## **伟大基建为代号不一定要神话**

在 Fairwinds，我们可以自动化地快速建造代号为 的 **基础设施。我们也让团队能够** 用优秀的工具实现 DevOps 文化。没有正确的工具，最好的团队永远不会成功。美妙的是，我们的服务从不休假或退出。幕后工程师来来去去 (休假 等等)，但是我们能够通过可重复的过程和自动化以稳定的方式维护事物。我们还为我们的客户提供一整个团队的工程师，与 — 冗余、冗余、冗余一起工作。我们构建的东西都不是专有的，尽管有时需要根据特定公司的需求进行定制。

总会有一些东西使您的特定基础架构独一无二，但在我们的帮助下，您可以获得冗余 (我提到过冗余？)，并降低单点故障的风险。我们无法为一个了解 s 您的应用程序堆栈的所有信息的开发人员解决问题，但是我们可以为一个掌握部署关键信息的运营人员解决问题。

## **徐达**

ClusterOps by Fairwinds is [managed Kubernetes](/thoughts/the-benefits-of-managed-kubernetes-vs-amazon-ecs). And more than that, it's managed managed Kubernetes. Because while anyone can login to a cloud console and deploy a cluster with a managed service, not everyone will understand all the decisions made above and beyond that. How many clusters do you want? How will you separate environments? What is your plan for upgrades? Where will you run your persistent data? What are best practices for security?Our team of experts is here to help—and unlike Steve, we're not going anywhere. To find out more, visit our [ClusterOps Page](https://www.fairwinds.com/clusterops).