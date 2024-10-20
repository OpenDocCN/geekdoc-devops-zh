# 线束 CI/CD 管道。NET 和 Azure | Harness

> 原文：<https://www.harness.io/blog/ci-cd-for-microsoft-net-and-azure>

完成 CI/CD。NET 应用程序。Harness CD 遵循 4 个概念:服务、环境、工作流和管道。

距离 Harness 走出隐身状态才一年多。在此期间，我们已经为我们的[连续交付平台](https://harness.io/products/continuous-delivery/)创建了跨 AWS、GCP、PCF、Kubernetes 和 OpenShift 的多云支持。

今天是对微软全面支持的最后一个重要阶段。NET 技术和微软 Azure 云。我们前面已经看到了。像 iHerb 这样的. NET 客户使用 Harness 和 Kubernetes 迁移到云中。

## 支持传统和云原生。网

我们注意到一件事。微服务和公共云的采用仍处于初级阶段。我们交谈过的大多数客户只有少数。运行在 AWS 或 Azure 中的. NET 核心应用程序。

因此，大多数客户都要求为传统的内部 IIS 应用程序及其云原生微服务应用程序提供支持。就像 Harness 支持传统的 Java 应用程序如 Tomcat & JBoss 一样，我们现在也支持您的“vintage”。网络应用程序:

这种级别的支持对于拥有大量混合的大型企业来说非常有价值。NET/Java 应用程序，以及包含 Docker、Kubernetes 和无服务器技术(Harness 也支持)的新的云原生应用程序。

## Linux、Windows、内部部署、AWS、Azure、GCP 等等

用你的。NET 服务/工件的支持，我们进一步扩展了我们的。NET 基础设施、计算和容器编排支持:

*   亚马逊网络服务(EC2，EKS)
*   微软 Azure
*   谷歌云平台(GKE)
*   RedHat OpenShift
*   内部数据中心(直接 Kubernetes、用于 Linux 的 SSH、用于 Windows 服务器/主机的 WinRM)

例如，只需几分钟，您就可以在 Harness 中创建一个新环境。只需选择您的工件、部署类型和云提供商帐户(例如下面的 Azure ), Harness 将动态获取并引用您的所有云提供商计算配置:

Harness 是真正的多云端持续交付平台，所以一个服务是无所谓的。运行在 Azure AKS 上的. NET core 或者运行在 AWS EC2 上的 IIS 服务或者你自己的数据中心。利用 Harness，您可以在几分钟内建模(并部署到)混合云。

## 利用线束完成 CI/CD

如果您熟悉 Harness 平台，那么部署就像平常一样。NET 应用程序。Harness 有 5 个核心概念，它们构成了典型的部署渠道:

1.  **服务** -您希望部署的工件(例如容器、IIS 网站)
2.  **环境** -给定服务的基础设施映射(例如开发、QA、生产)
3.  **工作流** -将一个或多个服务部署到给定的环境中(例如 IIS 网站-Canary-Prod)
4.  **管道**——将工作流整合成跨越各种环境的阶段
5.  **触发** -根据条件执行管道(例如工件的新版本)

例如，下面我们创建了一个新的 IIS 网站**服务**，名为 **todolist-app** ，其工件源链接到 AWS S3 桶。Harness 还支持 Azure Container Registry (ACR)、Google Container Registry (GCR)、Docker Hub 和 JFrog Artifactory。

Harness 已经自动生成了部署这个工件所需的所有标准部署脚本(下载工件、展开工件、创建 AppPool、创建网站)。

如果需要，您可以编辑这些默认的 Powershell 脚本，或者通过[线束模板库](https://harness.io/blog/introducing-the-harness-template-library-for-continuous-delivery/)导入您自己的脚本。

定义服务后，您需要创建工作流，将这些服务部署到给定的环境中。例如，您可以[构建一个金丝雀部署](https://harness.io/blog/build-canary-deployment/)来部署您的。NET 服务分阶段提供给目标环境或用户。

当你的。NET 部署工作流和管道执行，您可以使用我们的实时分析来逐步观察/调试它们:

## GitOps &按代码配置

Harness 还通过 YAML API 和 Git 集成提供了全面的 GitOps 功能，用于构建和版本控制部署管道。您可以通过 Harness UI 构建/管理一切，或者通过您的应用程序源代码库将一切作为代码驱动。

下面是. NET 应用程序的代码配置的快速截图:

## 验证 Azure 部署

除了自动化部署。NET 服务，Harness 还可以自动执行。网络服务一旦被部署。我们将这一过程称为持续验证，它与您现有的所有应用性能监控(APM)和日志分析工具相集成，使用无监督的机器学习来自动识别性能或质量退化。

**底线:** Harness 会告诉你所有的业务影响。NET 部署，如果需要的话，如果检测到性能(例如响应时间)异常或质量(错误/异常)倒退，将自动将应用程序回滚到上一个工作版本。

下面是使用 [AppDynamics](https://harness.io/blog/introducing-harness-service-impact-verification-for-appdynamics/) 对性能进行持续验证的快速预览:

我们还支持 New Relic、Dynatrace、Datadog、Prometheus 和自定义时序数据进行性能验证。

从质量角度来看，我们的验证还可以通过标记引入的新错误/异常(下面的红点)来检测质量退化。网络服务发布部署。

以下是对 Splunk 的这种支持的预览:

用户可以点击任何红点，立即看到导致回归的堆栈痕迹。

我们还支持 Sumo Logic、Elastic、Logz.io 和 Scaylr 进行质量验证。

## 注册免费试用

在这里注册[，免费试用](https://app.harness.io/auth/#/signup)安全带。

“干杯，”史蒂夫说