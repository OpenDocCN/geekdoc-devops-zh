# 线束产品更新| 2021 年 9 月|线束

> 原文：<https://www.harness.io/blog/harness-product-update-september-2021>

你听到了吗？！那是集体松了一口气的声音:9 月份没有重大产品发布！但这并不意味着我们放松了-没有先生。CD、CI 和 FF 的一些功能已经上线，我们正在努力使上个月的“展望未来”成为现实。

此外，如果你喜欢 DevSecOps，你应该看看我们上一期的 DevOps 女性播客——我们采访了我们的第一位 DevSecOps 女士，Stefania Chaplin！希望你们这个月过得愉快，并确保关注我们即将举办的活动、网络研讨会和 Harness U 课程，满足你们所有的 Harness-y 需求！

## 新功能

### **CD**

### 部署冻结窗口

根据我们早期采用者的反馈，我们增强了部署冻结功能，以支持:

*   临时窗口(用户可以轻松设置冻结窗口，在接下来的 2 小时内停止部署，同时进行关键演示)。
*   重复窗口(轻松设置冻结窗口，以避免在团队不在时进行部署——不再有周末部署)。
*   当您需要修复时，覆盖冻结窗口(超级管理员(英雄)将能够确保即使在冻结时也能进行关键任务部署)。
*   API 支持(现在你也可以通过 API 来管理你的冻结窗口)

用户现在可以从他们选择的任何日志工具获得关于他们的部署管道的事件通知。他们可以[设置事件](https://community.harness.io/t/observability-publish-pipeline-events-to-splunk-hec/822)，当管道开始、完成或者在执行过程中暂停时，将这些事件发送到任何 webhook 端点。这将有助于用户更好地了解整个开发运维流程。这些事件也可以用 GraphQL APIs 来管理。

### API 对批准的支持

Harness 用户现在可以[轻松批准或拒绝](https://docs.harness.io/article/n43s95h2aj-use-approvals-api#step_approve_or_reject_approval)他们的 API 部署管道。这将有助于自动化审批流程，并缩短总体交付时间。用户不必访问 UI 来手动批准/拒绝管道。

### **CI**

### 测试智能中的调用图可视化

调用图帮助您直观地了解哪些代码更改导致了测试智能算法所选择的测试。这向您保证所有相关的测试都被选中了。

### 测试智能中的梯度支持

Test Intelligence 现在支持用所有常见的构建工具——Maven、Bazel 和 Gradle——测试您的 Java 应用程序。

### 通用 Git 提供者的自动 Webhook 注册

当将管道配置为由从 GitHub、Bitbucket 或 GitLab 发送的事件触发时，Harness CI 将自动在您的 git 存储库中配置 webhook。

### **特征标志**

### 功能标志支持 Node.js SDK

我们最新的开源 SDK 在这里。现在，您可以开始在所有 Node.js 应用程序中对 Harness 使用特性标志了。这是回购协议。有关我们支持的 SDK 的完整列表，请参见下文。

## 哈斯林大学

**新增模块和课程:**

我们有**新的**课程，现在在[线束大学](https://university.harness.io)提供。

云成本管理学习路径:

**驾驭大学直播:**

*   **2021 年 10 月 18 日和 19 日**:线束基础，太平洋标准时间上午 8-11 点。
*   **2021 年 10 月 20 日和 21 日**:线束管理，太平洋标准时间上午 8-11 点。

要注册 10 月及以后的活动，请查看我们的[现场活动日历](https://university.harness.io/calendar)。

## 报刊上值得注意的提及

## 即将举行的活动

## 即将举办的网络研讨会和新的点播内容

*   [什么是 CI/CD？](https://harness.io/learn/webinars/continuous-delivery-and-continuous-integration/) -英国夏令时 10 月 13 日下午 3 点，DevOps 高级主管 Martin Reynolds 和工程管理副总裁 Nick Smyth 出席。
*   [在 Discover Dollar 解决云支出问题](https://harness.io/learn/webinars/ccm-discover-dollar/)-10 月 14 日上午 9 点，Discover Dollar 首席技术官迪曼思·r。
*   [利用 BI 来衡量、预测、&优化云支出](https://harness.io/learn/videos/business-intelligence/)-Harness 首席财务官 John Bonney 和战略财务主管 Jeff Merlin - On-Demand
*   [导致停工的因素](https://harness.io/learn/videos/downtime/) -线束高级产品经理 Rohan Gupta - On-Demand

## 展望未来

在接下来的几周/几个月里，你会遇到以下情况:

**CI**

*   **许可证视图** -用户将能够通过 Harness UI 中的许可证视图跟踪他们的许可证条款和使用情况。
*   **将报告路径添加到插件步骤-** 用户将能够从任何自定义插件以 JUnit 格式报告测试结果。Harness 将在 tests 选项卡下的 build execution 视图中显示测试结果。
*   **open shift 上的 Docker 层缓存-** 使用 openshift K8s 集群作为构建基础设施的用户将能够使用 Buildah 插件，利用 Docker 层缓存来构建映像，从而优化构建时间。

**CD**

*   **enableExecuteCommand** -支持 ECS 部署的 enableExecuteCommand 即将推出。
*   **缩放策略** -旧自动缩放组的计划缩放策略不会复制到新 ASG 中。
*   **金丝雀进入初级阶段**——支持确保在金丝雀阶段部署的工件/清单在初级阶段部署的能力。
*   **EKS·艾姆** -我们将在我们更新版的脊甲 CD 中增加对 EKS·艾姆角色的支持。

**FF**

*   **可视化管道构建器-** 从 2021 年 10 月开始，功能标志的用户将能够创建功能推出的可视化管道。
*   **GitOps -** 第四季度初，用户将能够使用带有功能标志的 GitOps。
*   **代理中继** -我们计划发布一个代理中继服务器来帮助使特征标志更加安全。[如果您有兴趣就您的需求提供反馈，请联系我们](mailto:ethan.jones@harness.io)！

**CCM**

*   **RDS 自动停止-** 客户将能够利用智能云自动停止 RDS 实例的能力。
*   **针对 K8s 的自动停止-** 客户将很快能够在其 Kubernetes 集群上使用智能云自动停止，并且适用于 EKS、阿拉斯加、GKE 和 ECS。
*   **带自动停止的固定时间表-** 我们将引入设置正常运行时间和停机时间的固定窗口的功能，自动停止是一个高级选项。
*   **预算增强-** 客户将很快能够设定每日、每周、每月、每季度和每年的预算。设定预算增长率，使您的预算阈值随着您的业务自动增长。

**PL**

*   **对 JAVA_OPTS 的支持** **-** 根据客户的反馈，我们正在为客户提供设置 JAVA_OPTS 的能力。这将支持多种使用情形，包括:
*   覆盖委托使用的信任库。
*   覆盖委托使用的密钥库。
*   指定处理证书的附加安全设置。
*   **使用 SAML 进行身份验证时，支持使用 LDAP/AD 进行授权的附加属性-** 多个客户要求能够使用在使用其 LDAP/AD 系统进行授权检查期间从 SAML 断言接收的附加属性。这将很快启用。