# Fairwinds Insights 如何帮助您节省云成本

> 原文：<https://www.fairwinds.com/blog/how-fairwinds-insights-helps-you-save>

 容器技术超越虚拟机的一个原因是，可以在主机上放置更多的容器，从而减少计算实例的总数，并节省基础设施成本。但是，如果您使用带有 Kubernetes 的容器，则可以通过设置合理的 CPU 和内存请求以及对 pods 的限制来进一步提高效率，这将允许您将更多的工作负载打包到工作节点上。

### 如何启用资源建议

到目前为止，收集 Kubernetes 工作负载效率数据的工具很难安装和使用，但 [Fairwinds Insights](http://insights.fairwinds.com/) 让您可以访问我们的工具 Goldilocks，帮助您“恰到好处地”获得 pod 资源请求如果您想查看 Kubernetes 集群的资源建议，请遵循以下说明:

1.  一旦您注册了 Fairwinds Insights 公共测试版计划，请登录[insights.fairwinds.com](http://insights.fairwinds.com)，创建一个新组织，并按照安装说明在您的集群中安装 Fairwinds Insights。
2.  回到您的组织，选择您希望看到哪个集群的金发女孩推荐。
3.  转到“报告”选项卡，在 Goldilocks 旁边，单击“最新报告”。您将看到一个类似于这里的仪表板:[https://raw . githubusercontent . com/FairwindsOps/Goldilocks/master/img/screen shot . png](https://raw.githubusercontent.com/FairwindsOps/goldilocks/master/img/screenshot.png)

### 仪表板显示了 Goldilocks 正在评估的名称空间和部署的列表。该工具在推荐模式下使用 Kubernetes[vertical-pod-auto scaler](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler)，如果您当前设置了 CPU 和内存请求和限制，您将在“当前”列中看到这些值。最有价值的数据在“建议更改”框中，它显示了成功运行 pods 所需设置的最小 CPU 和内存量。我们已经看到，我们的许多客户将其 CPU 和内存请求和限制设置得太高，当他们应用这些建议时，他们能够在更少的 Kubernetes 工作节点上放置更多的 pod。启用 cluster-autoscaler 后，任何多余的节点在未使用时都会被删除，从而为您节省资金。

了解你的工作量平衡

Goldilocks 的另一个好处是，它可以让您知道您的工作负载是 CPU 密集型、内存密集型还是两者之间的平衡。这些数据可以帮助您评估是否为 Kubernetes 工作节点选择了最有效的机器类型。

几个月前，我们为一家客户生成了一份 Golidlocks 报告，我们发现他们的一个部署生成了几乎不使用 CPU 但使用大量内存的 pod。我们最初将他们的 Kubernetes 集群设置为仅使用一种 AWS 实例类型:m5.xlarge。这些机器旨在实现计算、内存和网络资源之间的平衡，但由于它们最大的 pod 部署是内存密集型应用程序，我们寻找一种具有更多内存资源但 vCPUs 较少的 AWS EC2 实例类型。我们最终将他们的一些 worker 节点改为 r5.xlarge 实例。r5.xlarge worker 节点上安装了更多这样的内存密集型 pod，这些实例类型比原来的便宜 36%,为该客户节省了资金，并减小了他们的 Kubernetes 集群的规模。

如果你对机器类型感兴趣，以下是三大云提供商的规格:

总之，我们的 Fairwinds Insights 套件中的工具 Goldilocks 可帮助您和您的团队设置基于数据的 CPU 和内存请求和限制，这最终可以提高您使用云基础架构的效率，减少 Kubernetes 集群的蔓延，并为您节省资金。

Fairwinds Insights 可供免费使用。可以在这里报名[。](/coming-soon)

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)