# 通过一个开发人员和工具|工具迁移到 Kubernetes

> 原文：<https://www.harness.io/blog/kubernetes-migration>

了解 Tilting Point 如何利用 Harness 加速他们的云迁移。

## 关于

Tilting Point 是一家领先的、屡获殊荣的免费游戏发行商，专注于开发独立开发者的游戏。他们最成功的游戏包括《星际迷航时间线》、《战锤:混乱与征服》和《海绵宝宝:克鲁斯蒂烹饪大赛》。上一款游戏在 97 个不同的国家进入了前 10 名，并且仍然受到数百万玩家的喜爱，还有他们的游戏扩展目录。Tilting Point 采用数据驱动的方法进行游戏开发和用户获取，为独立开发者的现有直播游戏提供动力，随着他们之间的关系加强，他们可能会成为新联合项目的更紧密合作伙伴。

## **成长……豆茎到库伯内特**

Tilting Point 的首席软件工程师埃文·托马斯(Evan Thomas)使用亚马逊 Elastic Beanstalk 和詹金斯部署了该公司的旗舰应用程序。Evan 在 Jenkins 建立了 CI/CD 管道，允许 Tilting Point 每周部署两次。但是随着公司的发展，Elastic Beanstalk 和 Jenkins 开始显示出他们的局限性。

部署管道只能进行有风险的滚动部署。如果生产中出现问题，就不容易回滚。集成到管道中的监控警报返回了 10%的误报，导致冗长的故障排除。Evan 是唯一有能力建设新管道和维护现有管道的倾斜点工程师，这意味着 Evan 要花 20%的时间照看 Jenkins。

当部署停止工作时，我成了单点故障。这真令人沮丧。

> Evan Thomas |首席软件工程师|倾斜点

倾斜点决定是时候从弹性豆茎转移到 Kubernetes。他们知道 Kubernetes 将帮助他们扩展应用程序，并最终减少维护软件交付所需的人工时间。但是，Tilting Point 没有专门的 DevOps 团队或带宽来雇佣资源来支持迁移。他们需要一个解决方案来帮助启动迁移。

我们试图找出在没有完整 DevOps 团队的情况下如何迁移到 Kubernetes。

> Evan Thomas |首席软件工程师|倾斜点

## **Harness 赋予一个开发人员整个 DevOps 团队的力量**

Evan 选择了 Harness 来启动 Tilting Point 的 Kubernetes 迁移。埃文在第一天完成了他的第一次部署。Harness 一直在推动 Tilting Point 实现其应用的现代化和成熟化。

有了 Harness，我能够给工程师们带来与拥有庞大 DevOps 团队的大型科技公司相同的部署体验。

> Evan Thomas |首席软件工程师|倾斜点

Harness 提供了开箱即用的金丝雀部署，减少了 Evan 维护 CI/CD 所花费的时间。由于线束，Tilting Point 能够放弃雇用 DevOps 工程师，节省了超过 100，000 美元的劳动力成本。Harness 和 Kubernetes 的结合使 Tilting Point 可以每天部署，部署频率提高了 3 倍。

Harness 帮助创建了 Tilting Point 的新部署流程。线束支持团队继续与 Tilting Point 合作，以提升他们的软件部署水平。

线束支持团队是首屈一指的。

> Evan Thomas |首席软件工程师|倾斜点