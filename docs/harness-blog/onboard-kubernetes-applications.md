# 3 小时内上线 Kubernetes 应用程序| Harness

> 原文：<https://www.harness.io/blog/onboard-kubernetes-applications>

ConsumerTrack 通过线束节省了 58 万美元。

## 关于消费者追踪

[ConsumerTrack](https://www.consumertrack.com/) 帮助品牌与聪明、有抱负的受众建立联系。通过我们的合作伙伴和自有运营的网站，我们吸引了市场中的消费者，并将他们与他们寻求的产品和服务联系起来。

## 在 AWS 上迁移到 Kubernetes

ConsumerTrack 正在向运行在 [AWS](https://aws.amazon.com/) 和 [Kubernetes](https://kubernetes.io/) 上的微服务架构迁移。大约 24 项服务(现有 4 项&新增 20 项)将在 12 个月内需要新的部署渠道和入职。ConsumerTrack 的[持续集成(CI)](https://en.wikipedia.org/wiki/Continuous_integration) 过程(代码到工件)已经成熟，使用的工具有 Git、Jenkins、CircleCI、Selenium 和 BrowserStack。然而，[持续交付(CD)流程](https://harness.io/harness-continuous-delivery/)和消费者跟踪的部署管道仍然是 DevOps 团队拥有的手动临时脚本流程。“在部署新的 Kubernetes 服务时，两名 DevOps 工程师使用 [Jenkins pipelines](https://harness.io/2018/09/4-reasons-your-jenkins-pipelines-are-brittle/) 手动编写脚本平均需要两周时间。”ConsumerTrack 的 DevOps 经理杰里米·马拉拉说。这 24 款新 Kubernetes 应用的总投入/资源约为 96 周，成本约为 290，000 美元。

## 手动、脚本化部署，由开发人员托管

ConsumerTrack 的开发团队目前每天部署 6-7 次。Jeremy 说:“过去，部署是手动的，通常由来自每个开发团队的 2 名 DevOps 工程师和 1 名开发人员负责。将代码从每个开发环境提升到 QA 环境可能需要 2 个小时(最好的情况)，而将代码从 QA 提升到生产环境需要 1 个小时(最好的情况)。此外，生产部署的验证/运行状况检查是手动的，这意味着开发人员和开发人员必须手动检查 Datadog/Prometheus 中的指标，并检查 Splunk 中的应用程序事件，才能认为部署成功。

部署是手动的，通常由两名 DevOps 工程师和一名开发人员负责

> Jeremy Malara | DevOps 经理|消费者跟踪

## 面向开发人员的自助式持续交付

为了自动化和简化他们的手动连续交付(CD)流程，ConsumerTrack 评估了[线束](https://harness.io/)。“仅在我们的线束评估中，我们就将从开发到 QA 的部署时间从 2 小时减少到了 5 分钟，”Jeremy 说。最终目标是让开发人员拥有[自助式持续交付](https://harness.io/harness-continuous-delivery/)。DevOps 工程师不再拥有每个部署(和故障排除)，他们现在管理和模板化管道，开发人员可以使用 DevOps 团队的 Harness 管道模板和工作流进行实例化。然而，消费者跟踪和管理最大的胜利是新的 Kubernetes 应用程序的上线。Jeremy 说:“我们不需要 2 名 DevOps 工程师花费 96 周时间手动编写 Kubernetes Jenkins 管道，而只需一周时间就可以完成同样的工作。创建新的 Kubernetes 管道的工作和资源从 2 周和 2 名 DevOps 工程师减少到仅 3 小时和 1 名 DevOps 工程师。有了 Harness，Kubernetes 管道现在是端到端自动化的，因此代码从开发到生产只需几分钟，而不是几小时。Harness 还使用无人监管的机器学习，使用来自 Datadog、Splunk 和 Prometheus 的数据来自动进行部署验证。

我们可以在一周内完成同样的工作，而不是 2 个 DevOps 工程师花费 96 周手动编写 K8 管道脚本。

> Jeremy Malara | DevOps 经理|消费者跟踪

## 第一年节省 580，000 美元

今天，ConsumerTrack 的开发人员现在部署他们自己的代码。
ConsumerTrack 还从线束连续交付中获得了以下业务优势:

*   **将部署时间(投入生产)减少 92%** 从 3 小时减少到 15 分钟。
*   **每天减少 16 个小时的部署工作**相当于 2 个 DevOps 全职员工(290，000 美元)
*   **每项服务的入职时间减少了 96%** 从 80 小时(2 周)减少到 3 小时。
*   **将 24 项 Kubernetes 服务的入职工作从 96 周减少到 72 小时**，相当于 2 个 devo PS fte(290，000 美元)。

ConsumerTrack 的上述优势相当于第一年节省了约 580，000 美元。