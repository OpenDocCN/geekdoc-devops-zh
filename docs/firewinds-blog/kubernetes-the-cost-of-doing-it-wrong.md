# Kubernetes:做错事的代价

> 原文：<https://www.fairwinds.com/blog/kubernetes-the-cost-of-doing-it-wrong>

 我经常骑自行车。我骑自行车长距离锻炼，我骑自行车翻山越岭寻求冥想的平静，我和朋友一起骑自行车分享活动和欢笑(特别是当那个朋友以一种无伤且滑稽的方式摔倒时)。我也骑自行车去商店、市场、面包店和其他需要我把工作量带回家的地方。我在背上绑了一个床垫，骑着它像船帆一样顶风回家(不推荐)。我把所有的食物、水、帐篷和生活用品都装在自行车上，准备好了整个探险过程。

由于我携带的货物和携带货物的规律性，一个好的自行车包是必不可少的。我以前有过行李架、篮子，甚至还有一辆三轮自行车，用来装载大量货物。当我几个月前搬到欧洲时，我把我的自行车收藏减少到只有一辆。我随身携带的这辆自行车的几何形状(我是一个大块头，因此自行车架也很大)使得自行车架几乎就在我的正下方，而不是在我座位的后面。所以当我把包挂在后面的行李架上时(称为[](https://www.outdoorgearlab.com/topics/biking/best-bike-panniers))，我每蹬一下就踢一下包。

正如我提到的，我经常骑自行车。所以，你可以想象这让我发疯。因为我需要带很多货物，所以我需要实用的包。我总是做错。我一直在更换行李架(框架安装式或座柱安装式)和袋子(绑带式、夹式、包裹式、velcro 式)。我想也许这个周末我终于找到了一个有效的组合。但这是经过了六个月的反复试验之后。我觉得自己像个白痴，因为本该很容易解决的问题却让我花了一大笔钱(商店不会收回我的旧包)。

我要说的是……你可能对你的 [Kubernetes 基础设施](https://www.fairwinds.com/blog/is-your-kubernetes-infrastructure-production-grade) 摆弄得太多了。就像我的自行车包一样，你花了太多的时间和太多的钱试图把它做好。很有可能，有一个专家可以请教，或者有一个指导者，或者其他什么东西，可以让你不用去艰难地学习每一课。

没有理由让你的自行车锁在崎岖的道路上弹出来，因为你已经求助于 [蹦极绳](http://www.quickmeme.com/meme/3r0k3s) 来将东西固定在一起。当你在那个交通圈里全速前进时，袋子被吸进你的辐条里，你几乎要飞过车把时，那辆装满年轻可爱的人的汽车没有必要嘲笑你。没有理由让 [停机](https://www.fairwinds.com/blog/kubernetes-best-practices-reliability) ，没有必要让 [超支](https://www.fairwinds.com/blog/6-kubernetes-cost-control-strategies-you-need-for-2023) ， [过度许可](https://www.fairwinds.com/blog/over-provisioned-and-over-permissioned-containers-kubernetes) ，崩溃，供应不足，未能 [扩展](https://www.fairwinds.com/blog/the-secret-to-running-scalable-kubernetes-clusters-is-here) ，整个开发部门都不需要嘲笑你，因为“在你重新部署之前，老方法工作得很好。”

最近，一名前员工告诉我，Fairwinds Insights 在 Kubernetes 中实现了标准化，标准化是基础设施中最性感的东西。我不确定 sexy 和云基础设施曾经一起使用过。我不确定它是否有效。但是另一个朋友说了一些类似于“Kubernetes 你的团队实际上可以运行”的话，这听起来很不错。

他的意思是，一辆带支架和袋子的自行车可以节省我大量的时间和精力(我不想谈论钱——钱让我感到羞耻)。有效的 Kubernetes 基础设施可以为您节省大量资金。洞察力可以帮助你做正确的事情。Sane 开箱即用的默认设置让您可以继续工作，编写 [自定义策略](https://www.fairwinds.com/blog/kubernetes-policy-enforcement-for-developers) 的能力可以帮助您扩展到您需要的地方。一个舰队安装程序意味着你可以把它放在任何你需要的地方。而且是只读的。并且涵盖了 [安全性](https://www.fairwinds.com/kubernetes-security) 、可靠性 [成本](https://www.fairwinds.com/cloud-native-service-ownership) 。不知何故第一次就选对了。和右边的篮子。和右框袋。还有右边的鞍囊。你可以带着它露营，带着它购物，必要时，带着床垫回家。

Kubernetes 和我的自行车之间的隐喻现在已经深深地交织在一起，我担心它可能会卡在一些辐条中，所以我会停下来。

我的观点是。是有成本的(不小的成本！)到运行 Kubernetes 错误。别做错了。有太多的好人从 [到](https://www.fairwinds.com/kubernetes-maturity-model) 的惨痛教训中吸取了教训。你没必要自己去做同样的事。查看见解。有了 [我们的自由层](https://www.fairwinds.com/insights-pricing) 你可以在集群上试一试，看看你的设置有多远，你可以看看你是否在运行已知的 CVE，检查是否有容器在使用它们不需要的权限运行，看看哪些工作负载被过度配置。犯错的代价越大，补救起来就越容易——现在很容易就能纠正错误，实现标准化，让你的团队能够真正运行 Kubernetes。

做错是有代价的，做对太容易了。

查看 Fairwinds Insights 和 [在这里报名](https://www.fairwinds.com/insights-pricing) 。

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)