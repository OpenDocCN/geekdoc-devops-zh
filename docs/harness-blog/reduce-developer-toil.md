# 使用无人机和马具|马具将开发人员的工作量减少 94%

> 原文：<https://www.harness.io/blog/reduce-developer-toil>

Lavasoft 使用 Drone 和 Harness 来创建理想的软件交付环境。

## 关于 Lavasoft

Lavasoft 是 Avanquest Software 的子公司，是世界领先的个人和专业软件开发商，包括创意和学习软件、实用程序和多媒体等众多类别。

## **识别交付问题**

Mathieu Fecteau 是 Lavasoft 的软件交付经理，他帮助发现了该公司的软件交付问题。

Lavasoft 的应用程序是用多种语言编写的，其中 80%是在本地托管的。Lavasoft 使用同样复杂的 Jenkins 管道和定制脚本来管理这种复杂性。对 Jenkins CI 管道的任何更改都可能需要数小时，而 Lavasoft 没有有效管理该系统的工程资源。复杂性和备用资源的缺乏使得 Lavasoft 很难实施 DevOps 原则。

Lavasoft 的产品团队对部署时间表和指标缺乏了解。产品负责人没有意识到部署失败会导致对实际产品的误解。部署失败需要密集的手动干预，并导致开发人员每周花费 8 小时进行验证。失败部署的回滚需要手动重新部署本地文件。

我们通常会排除部署出错的原因。

> 马修·费克图|软件交付经理| Lavasoft

Mathieu 需要降低 Lavasoft 的交付复杂性，并增强对部署流程的信心。

## **寻找容器原生 CI 工具**

在评估了其他 CI 厂商后，Mathieu 决定评估 Drone。

其他配置项选项并不顺利，设置过程也很困难。

> 马修·费克图|软件交付经理| Lavasoft

马修在 15 分钟内完成了他的第一份申请。随着 Mathieu 搭载的应用越来越多，他发现了无人机成功背后的秘密。简单。Drone 的容器化插件减少了相互依赖性，并且不需要定制 groovy 脚本。升级无人机花费了 2 分钟，比 Lavasoft 升级 Jenkins 管道花费的 1 小时减少了 97%。无人机非常直观，新员工在 Lavasoft 的第一天就可以使用无人机。

## **停止临时保姆部署**

在解决了持续集成之后，Mathieu 将注意力转向了持续交付。Harness 引起了他的兴趣，因为它关注 Kubernetes，并且与本地应用程序兼容。

在使用 Harness 的前 15 分钟，Mathieu 将他的管道连接到无人机上。在使用 Harness 的第一周，Mathieu 装载了 40 个应用程序，并将 Kubernetes 每个应用程序的迁移时间减少到 1 小时。

Mathieu 还让 Lavasoft 的产品经理了解了部署过程，消除了项目经理和开发人员之间的摩擦。插入 Datadog 和 ELK 的持续验证将验证时间减少 94%,从每周 8 小时减少到 30 分钟。

Harness 帮助我从已经使用的昂贵工具中获得更多价值。

> 马修·费克图|软件交付经理| Lavasoft

Mathieu 使用无人机 CI 和 Harness CD 的组合找到了他的完美 CI/CD 解决方案。