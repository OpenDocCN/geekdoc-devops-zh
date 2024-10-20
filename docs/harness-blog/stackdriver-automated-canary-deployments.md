# 引入 StackDriver 支持自动金丝雀部署和运行状况检查| Harness

> 原文：<https://www.harness.io/blog/stackdriver-automated-canary-deployments>

Stackdriver 是一个流行的监视工具，用于观察性能和诊断数据。了解 Harness 如何支持 Stackdriver。

[Stackdriver](https://cloud.google.com/products/operations) 是一款流行的监控工具，于 2014 年[被谷歌](https://techcrunch.com/2014/05/07/google-acquires-cloud-monitoring-service-stackdriver/)收购，用于观察在谷歌云计算(GCP)、亚马逊网络服务(AWS)和 Kubernetes 环境中运行的服务的性能和诊断数据。

Stackdriver 聚合了来自云基础设施的指标、日志和事件，为开发者和运营商提供了一组丰富的可观察信号。在 Harness，我们使用无监督的机器学习自动分析这些信号，以确定新的部署或金丝雀阶段是否成功或包含异常，这可能需要自动回滚。

## 为什么要自动化 Canary 部署和部署运行状况检查？

如果您对[连续交付](https://harness.io/continuous-delivery/)很认真，您会希望您的部署管道在 30 分钟内执行并完成。

为什么？实际上有几个原因:

*   部署工作/时间严重限制了部署频率。
*   如果您的部署需要几个小时才能完成，则不能按小时部署。
*   快速失败总比缓慢失败好，记住部署是实验。
*   您花在部署上的时间越多，花在开发上的时间就越少。

举个例子，我们的一个早期客户[Build.com](https://www.build.com)曾经每周部署 3 次生产。每次部署后，5-6 名团队领导将登录到 New Relic 和 Sumo Logic，并手动验证 60 分钟，确保一切正常。这相当于每个部署仅花费 5-6 个工程小时进行验证。借助 Harness，Build.com 自动化验证，每次部署只需一名工程师和 15 分钟。

## 机器学习在堆叠驾驶监控数据中的应用

Harness 帮助客户在几分钟内构建部署管道。它还通过将机器学习与客户现有的监控工具相结合，帮助客户自动验证部署。在这种情况下，Stackdriver。

一旦 Harness 将新的应用程序或服务部署到目标环境中，它将立即连接到 Stackdriver，并为它正在观察的内容构建一个模型。然后，它会将该模型与以前的部署模型进行比较，以便识别新的异常或倒退，如果需要，会自动回滚到以前的工作版本。在 Harness，我们称之为持续验证。

## 将 Stackdriver 与 Harness 集成在一起以实现持续验证

好消息！没有 Stackdriver 的设置。

Harness 会为您注册的每个云提供商帐户自动发现 Stackdriver API。因此，如果您在 Harness 中注册了 10 个 GCP 帐户，它将自动发现这些帐户的 Stackdriver APIs。

## 将堆栈驱动程序验证添加到您的部署工作流中

在线束中创建或打开现有的部署工作流:

点击**添加验证**，选择**堆栈驱动**:

验证可以应用于任何类型的部署工作流，例如金丝雀、蓝绿色、滚动等等。

现在，让我们挑选与我们正在部署的**服务**相关的**云客户、区域、**和**指标**:

定义所有指标后，您可以选择是否需要以前的分析或金丝雀分析来建立基线。您还可以指定验证的持续时间(默认为 15 分钟)。

## 让 Harness 和 StackDriver 自动验证您的部署

通过集成和配置 StackDriver，您现在可以自动验证您的所有部署。
以下面的示例为例，该示例展示了一个已使用 StackDriver 成功验证的典型部署。

您可以在右侧看到，所有的 StackDriver 指标都已经过 Harness 机器学习算法的验证。绿色表示未发现异常或倒退，部署在其正常范围内运行。

您可以通过注册[免费试用](https://harness.io/try-continuous-delivery-as-a-service-for-free/)来开始使用 Harness 和 StackDriver。

干杯，
史蒂夫。
@伯顿说