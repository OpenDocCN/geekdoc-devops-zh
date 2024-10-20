# Fairwinds 为 Kubernetes 开发的开源项目

> 原文：<https://www.fairwinds.com/blog/here-is-an-overview-of-our-open-source-projects-for-kubernetes>

 开源软件是 Fairwinds 的核心，为我们的客户和更大的社区提供 Kubernetes 支持。我们努力构建开源项目，帮助我们的客户进行创新，并使用户能够设计正确的 Kubernetes 架构和部署。

## 开源的快乐

构建应用程序通常需要一些棘手的技术决策。相对于依赖第三方，你自己构建什么？在做出这些决定之前，你需要问自己开源如何帮助你的组织成功——以及你是否愿意投入时间使用开源代码(查看我们关于开源的[隐藏成本](https://www.fairwinds.com/hidden-cost-of-open-source)的论文)。开源软件带来了一些挑战，包括设置、成本、维护、安全性和可靠性。即便如此，社区中仍有大量支持来帮助从业者在没有第三方的情况下实现他们需要的东西。

## 我们的项目概述

在 Fairwinds，我们努力提供企业需要的开源代码类型，以构建更好的产品和服务。对我们的 Kubernetes 开源项目的快速概述可以让您了解我们的贡献是如何改变现代软件的面貌的。

[北极星](https://www.fairwinds.com/polaris) 运行各种检查，以确保使用 Kubernetes 最佳实践配置 pod 和控制器。作为我们最受欢迎的开源项目之一，Polaris 识别 Kubernetes 部署配置中的错误，以帮助用户找到导致安全漏洞、中断、扩展限制等的错误配置。北极星可以在三种不同的模式下运行:

1.  作为审计集群内部运行情况的仪表板

2.  作为准入控制器，自动拒绝不符合策略的工作负载

3.  作为测试 YAML 文件的命令行工具，例如作为 CI/CD 流程的一部分

Kubernetes 安全性的一个重要目标是确保容器以最小权限运行，避免升级，不使用根用户运行容器，尽可能使用只读文件系统。在 pod 和 container 级别的配置都可用的情况下，Polaris 可以对两者进行验证，让您避免潜在的问题，同时通过成熟的策略获得成功。

[在 GitHub 中查看北极星](https://github.com/FairwindsOps/polaris?__hstc=91154437.430b158c0def384b1ef4aa7afcfab38c.1648670844598.1648670844598.1648754157339.2&__hssc=91154437.1.1648754157339&__hsfp=192797845&hsCtaTracking=53ec0fcc-d520-40dd-892b-11e16913e4df%7Cbf8294c1-ed88-450d-8bc5-763432826a2a) | [查看北极星文档页面](https://polaris.docs.fairwinds.com/)

[Goldilocks](https://www.fairwinds.com/?utm_source=adwords&utm_medium=ppc&utm_term=fairwinds%20goldilocks&utm_campaign=Branded&hsa_cam=9424392662&hsa_mt=b&hsa_ver=3&hsa_src=g&hsa_ad=524172068842&hsa_net=adwords&hsa_tgt=kwd-1494176964644&hsa_acc=8748715703&hsa_grp=122280890629&hsa_kw=fairwinds%20goldilocks&gclid=CjwKCAjwxZqSBhAHEiwASr9n9GvHMnoqkIIgeDcE-3vwVPAjRHjcaPI0LPwtV2H0pfSiVTztbMU_CxoCQIgQAvD_BwE) 是一款用于推荐资源请求的开源工具，允许用户在推荐模式下使用 Kubernetes[vertical-pod-autoscaler](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler)(VPA)查看每个应用程序的建议。Goldilocks 为名称空间中的每个部署创建一个 VPA，并查询它们的信息，同时考虑到您的 pod 的当前资源使用情况，以获得更好的指导。Goldilocks 没有运行水平 pod 自动缩放器(与 VPA 不太匹配)，而是使用 VPA 建议来提供可靠且可行的反馈。

Goldilocks 仪表板提供了 VPA 建议的可视化，因此您可以访问集群中的服务并查看两种类型的建议，具体取决于部署所需的 QoS 等级。QoS 类别指的是可以设置资源请求和限制的不同方式。

[查看 GitHub 中的金发女郎](https://github.com/FairwindsOps/goldilocks) | [查看金发女郎文档页面](https://goldilocks.docs.fairwinds.com/)

[Pluto](https://pluto.docs.fairwinds.com/quickstart/) 帮助用户在他们的代码库和 Helm 版本中轻松找到破旧的 Kubernetes API 版本。随着 Kubernetes 生态系统的不断发展，API 也在不断发展，这意味着弃用和删除是不可避免的。如果你已经迁移到 Kubernetes 1.22 或者 OpenShift 4.9，你就知道这种痛苦是真实存在的。如果您正在计划升级，Pluto 是一个完美的开源实用程序，可以帮助您找到已弃用或已删除的 Kubernetes apiVersions。一旦 Pluto 集成到您的 CI/CD 管道中，它就会根据您的目标 Kubernetes 版本扫描资源。

当几个应用程序部署到 Kubernetes 集群时，集群升级可能会中断部署过程，从而可能影响数百个存储库。Pluto 旨在提前提供这些关键信息，以确保在升级发生之前解决部署过程。作为一个开源工具，Pluto 可以用来扫描各种来源的过时版本，包括平面清单文件和使用 Helm 进行部署的集群。

[在 GitHub 上查看冥王星](https://github.com/FairwindsOps/pluto) | [查看冥王星文档页面](https://pluto.docs.fairwinds.com/)

是一个 Helm 的命令行工具，使用 YAML 语法在一个文件中安装和管理多个 Helm 图表，允许从 git 提交、分支或发布安装图表。你想要使用这个开源工具安装的图表的定义被称为“课程”，它包括告诉 Reckoner 如何使用 Helm 安装你的图表的设置，在哪个名称空间和用什么值。本课程还概述了您可以从哪些远程图表存储库中提取数据，并像管理任何其他基础设施代码一样进行管理。

[在 GitHub 中查看计算者](https://github.com/FairwindsOps/reckoner?__hstc=91154437.430b158c0def384b1ef4aa7afcfab38c.1648670844598.1648754157339.1648759026796.3&__hssc=91154437.18.1648759026796&__hsfp=192797845&hsCtaTracking=da29f636-61c9-4f2b-aa4e-ebe3fd729b77%7Cd2c39f20-6713-4fe3-909a-dd39fb7fd3c0) | [查看计算者文档页面](https://reckoner.docs.fairwinds.com/usage/#course-yaml-definition)

[Nova](https://nova.docs.fairwinds.com/) 是一个命令行界面，用于交叉检查 Kubernetes 集群中运行的最新版本的舵图。让您的云原生环境中的一切保持最新是最关键(也是最困难)的任务之一。在新版本发布后未能升级可能会使您的组织暴露于安全漏洞、功能缺陷和潜在违规。

开源工具 Nova 通过让您知道是否有新版本可用，来帮助您确定何时更新。这个功能可以很容易地看到你是否落后于任何已安装的舵图表，节省用户大量的时间和资源。运营商无需监控每个图表的更新，只需运行一条 Nova 命令即可检测旧版本，确保 YAML 文件是最新的，基础设施代码符合这些标准。

[在 GitHub 中查看 Nova](https://github.com/FairwindsOps/nova)|[查看 Nova 文档页面](https://nova.docs.fairwinds.com/)

[Gemini](https://www.fairwinds.com/blog/gemini-automate-backups-of-persistentvolumes-in-kubernetes) 构建于 Kubernetes-native volume snapshot API 之上，为用户创建一个更加健壮和用户友好的界面。这款开源工具允许按照可定制的细粒度计划进行自动备份，自动删除过时备份，并从特定备份中恢复数据。

Gemini 帮助用户处理 Kubernetes 中的持久存储，从而允许开发人员在其云环境中更好地管理卷和备份。为 Gemini 添加自动化功能意味着卷快照不再需要手动完成，这使得开发人员可以更轻松地确保数据安全。

[在 GitHub 中查看双子座](https://github.com/FairwindsOps/gemini?__hstc=91154437.430b158c0def384b1ef4aa7afcfab38c.1648670844598.1648759026796.1648828126213.4&__hssc=91154437.1.1648828126213&__hsfp=192797845&hsCtaTracking=336f4dfa-0147-41da-b7f5-e87f74704ad2%7Cf865d08d-ff77-4eab-a24f-6d611806b064)

Saffire 通过从各种注册表中提取图像，帮助开发人员和工程团队避免单点故障。这个开源控制器运行在一个 Kubernetes 集群中，监视那些在提取底层映像时遇到问题的 pod。Saffire 使用户能够在主注册中心检测到问题时，例如在停机期间，自动在注册中心之间切换。

在当今高度自动化的 DevOps 环境中，工程师和开发团队不断更新和部署新版本的软件和应用程序。从注册表中提取容器映像对于部署过程至关重要。如果注册中心不可用，部署就会中断，从而导致停机，并给开发人员和业务带来重大问题。Saffire 解决了这个问题。

[在 GitHub 中查看 Saffire](https://github.com/FairwindsOps/saffire?__hstc=91154437.430b158c0def384b1ef4aa7afcfab38c.1648670844598.1648759026796.1648828126213.4&__hssc=91154437.18.1648828126213&__hsfp=192797845&hsCtaTracking=5f066949-fc02-4dec-b78b-e639f79ed69c%7C0558c42c-6ae3-4b5e-9d1c-cf07fe09546c)

[Rok8s-scripts](https://rok8s-scripts.docs.fairwinds.com/) 提供了一个用 Docker 和 Kubernetes 构建 GitOps 工作流的框架。将这个开源工具添加到您的 CI/CD 管道中，允许用户在使用一组经过测试的最佳实践的同时构建、推送和部署应用程序。它可以用来验证部署和数据库迁移，处理机密和组织 YAML 文件。

除了构建 Docker 映像并将其部署到 Kubernetes，rok8s-scripts 还处理安全机密管理、特定于环境的配置、Docker 构建缓存等等。Rok8s-scripts 旨在很好地与各种用例及环境一起工作，提供了许多配置 CI 管道的有效方法。

[查看 GitHub 中的 Rok8s-scripts](https://github.com/FairwindsOps/rok8s-scripts?__hstc=91154437.430b158c0def384b1ef4aa7afcfab38c.1648670844598.1648759026796.1648828126213.4&__hssc=91154437.1.1648828126213&__hsfp=192797845&hsCtaTracking=bfba2d02-898b-4884-b0eb-82804cd895aa%7Cac7ed043-4418-4c0d-9699-cb9f1080e1c0)|[查看 Rok8s-scripts 文档页面](https://rok8s-scripts.docs.fairwinds.com/)

[RBAC 管理器](https://rbac-manager.docs.fairwinds.com/introduction/)通过使用新的定制资源支持 RBAC 的声明式配置，简化了 Kubernetes 中的授权。用户可以指定所需的状态，而不是直接管理角色绑定或服务帐户，RBAC 管理器将确保进行必要的更改来实现这一点。RBAC 管理器通过减少大授权所需的配置工作量和实现 RBAC 配置更新的自动化，提供了一个更易接近和可扩展的解决方案。这个开源工具大大减少了配置所需的时间。

[在 GitHub 中查看 RBAC 经理](https://github.com/FairwindsOps/rbac-manager) | [查看 RBAC 经理文档页面](https://rbac-manager.docs.fairwinds.com/introduction/)

[RBAC 查找](https://rbac-lookup.docs.fairwinds.com/)是一个 CLI，允许从业者容易地找到绑定到服务帐户、组名或单个用户的 Kubernetes 角色和集群角色。"这个用户对这个群集有多少访问权限？"现在更容易回答了。虽然 RBAC 管理器使该过程更容易管理，但 RBAC 查找可以提供对 Kubernetes 授权的可见性，并有助于遵守集群中的最小特权原则..

[查看 GitHub 中的 RBAC 查找](https://github.com/FairwindsOps/rbac-lookup?__hstc=91154437.430b158c0def384b1ef4aa7afcfab38c.1648670844598.1648759026796.1648828126213.4&__hssc=91154437.1.1648828126213&__hsfp=192797845&hsCtaTracking=b39c3dc0-eb80-4cfd-a915-8e114c92bbd9%7C22899e08-1fbe-452d-bf2b-fa48bc5e93a8) | [查看 RBAC 查找文档页面](https://rbac-lookup.docs.fairwinds.com/)

## 加入我们的开源社区！

Fairwinds 提供了一个集成的、可信的开源工具平台，可以主动监控 Kubernetes 的配置，并提出改进建议以避免将来出现问题。Fairwinds 开源社区的目标是交流思想，影响开源路线图，并与 Kubernetes 用户建立联系。要参与进来，[在 Slack 上与我们聊天](https://join.slack.com/t/fairwindscommunity/shared_invite/zt-e3c6vj4l-3lIH6dvKqzWII5fSSFDi1g)或者[加入我们的开源用户组](https://www.fairwinds.com/open-source-software-user-group)！

> 下一次开源会议将于 2022 年 6 月 22 日东部时间上午 11 点|太平洋时间上午 8 点举行。[立即注册！](/open-source-software-user-group)

[![Join Our Open Source User Group](img/c49a4992f50d857242d0dea90d04774f.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/c6cdf058-d3f9-440a-a3bb-c4077ed54fcf)

[![Use Fairwinds Insights for Free Security, Cost and Developer Enablement In One](img/7c86296320eb01b215d8e2755e9c5b9d.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/34aa4987-a1f9-438a-a145-d7d82d5c479a)