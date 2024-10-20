# 找到你的 Kubernetes 成本盲点

> 原文：<https://www.fairwinds.com/blog/find-kubernetes-cost-blind-spots>

 在过去的十年里，云基础设施发生了翻天覆地的变化，这几乎令人难以置信。我在技术领域的第一份工作是在一个壁橱里有一个服务器室，我们自己处理一切，从硬件和操作系统到网络，甚至是我们用来更新软件的光驱。今天，我们将越来越多的 it 交给云本身，我们不担心底层的服务器，也不担心网络。没有我们曾经与之互动的物理驱动，因此我们的生活变得更好。

有趣的是，当我们远离所有系统中如此多的有形部分时，我们就更加远离了运行这种基础设施的实际成本。云对于有形硬件就像信用卡对于现金一样，它不会花费更多，只是当我们不动手时，更难说出所有的钱去了哪里。

## **Kubernetes 仍然是一个新的范例**

Kubernetes 把事情带到了一个新的水平。在 Kubernetes 之前，我们可能已经为工作负载提供了自动化，但随着 container orchestrator 成为我们与云基础架构交互的方式，一切都变成了一个更大的黑匣子。

Kubernetes 的美妙之处在于，我们可以交给它一个工作负载，让它担心如何根据需求进行伸缩。负面影响是，如果我们在将工作交给 Kubernetes 之前错误地配置了工作负载，那么很容易出现成本失控的情况，因为 Kubernetes 完全按照我们(错误地)告诉它的那样去做。

## **Kubernetes 的成本**

Kubernetes 本身的运行成本非常低，因为它主要只是一个控制平面，负责协调其他工作负载，但是每个新模式都有相关的成本。在 Kubernetes 中，当资源请求和限制设置不正确时，将导致支出超过您的需要(因为工作负载过度供应，Kubernetes 扩展的内容超过需要)，或者导致性能不佳(因为工作负载耗尽内存或 CPU 受限)。它还会导致工作负载优先级过高或过低，因为 Kubernetes 会尽力理解交给它的工作。

如果没有良好的工具来了解集群的成本或工作负载的成本，开发人员(特别是在高级 [服务所有权](https://www.fairwinds.com/cloud-native-service-ownership) 环境中)很容易过度调配资源，平台团队也很难洞察如何解决失控的成本，直到为时已晚，账单已经上涨。

我经常听到这样的故事:一个团队在很久以后才发现——当他们最终收到云账单时——他们的工作负载配置错误，成本飙升。

## **Fairwinds Insights 提供了控制成本所需的可见性**

Kubernetes guardrails 平台 Fairwinds Insights 为您的所有集群提供了一个单一平台，以查看哪些集群和哪些工作负载的成本最高。您还可以跟踪一段时间内的趋势，这样您就可以看到事情失控的地方以及如何解决它们。

> Fairwinds Insights 可供免费使用。你可以在 这里 [报名。](https://www.fairwinds.com/coming-soon)

伟大的工具造就伟大的团队。云的承诺是你的花费实际上可以满足你的需求。在你控制你的 Kubernetes 基础设施之前，不要让事情失去控制。

观看 Clover 如何利用洞察力控制成本。

[![Your Cost to Fix Kubernetes Misconfiguration - Find out how much your organization can save. Book Call](img/616e48733b5a477f8f4e79be9d1a42dc.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/877fc29a-a30e-4a0c-95a7-ad1597996764)