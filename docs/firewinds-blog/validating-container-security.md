# 利用 Fairwinds Insights 验证集装箱安全性

> 原文：<https://www.fairwinds.com/blog/validating-container-security>

 ## 概述:什么是容器安全性？

容器安全性保护容器的完整性，包括容器中的应用程序和它们所依赖的基础设施。容器使得为不同的环境和部署目标构建、打包和部署应用程序和服务变得容易。 [Docker](https://www.docker.com/) 是一种流行的容器化技术，因此了解 Docker 映像是漏洞进入集群的最简单方式之一非常重要。

## 为什么集装箱安全很重要

容器由文件层组成，这些文件层称为容器映像。基本映像对于安全性至关重要，因为它是 Linux 容器的起点。如果您的基础映像中存在漏洞，那么包含该基础映像的每个容器中都会存在该漏洞。这就是为什么找到可靠的基础映像来源非常重要。请记住，当您添加应用程序和进行配置更改时，这些更改会引入新的变量。

“根据 451 Research 的 2020 年[调查](https://www.stackrox.com/press-releases/2020/02/stackrox-report-reveals-that-container-and-kubernetes-security-concerns-are-inhibiting-business-innovation/)，在 12 个月的时间里，94%的组织报告了 K8s 和容器环境中的严重安全事件。”

## 集装箱安全风险

Docker 容器使开发更加高效和可预测，消除了一些重复的配置任务。此外，与直接在主机上运行应用程序相比，使用 Docker 可以提高安全级别。开放 Web 应用安全项目(OWASP)提供了 [12 条规则来帮助您防止常见的安全错误，并应用最佳实践](https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html)来帮助您保护 Docker 容器，包括保持最新的补丁，确保您不会暴露 Docker 守护进程套接字，限制功能等等。遵循这些规则将有助于您的组织降低与集装箱安全相关的风险，但您将需要额外的工具来[扫描图像](https://insights.docs.fairwinds.com/first-steps/container-security/)和[检测错误配置](/insights)。

## 集装箱安全挑战

要看到一个容器的内部并理解你的容器内部使用的是什么开源并不容易。Docker 容器是一个虚拟化的运行时环境，Docker 映像是 Docker 容器在特定时间点的记录，其中包含所有应用程序代码、工具、库、依赖项和运行应用程序所必需的附加文件——这太多了！Docker 图像也有多个层，每个层都基于前一个层，但也有所不同。Docker 图像可以重复使用，但也不能更改。因此，如果出现问题，您需要修复漏洞并构建新的容器映像。

请记住，即使您定期扫描容器映像作为 CI/CD 或注册表的一部分，[常见漏洞和暴露](https://cve.mitre.org/) (CVEs)也可能随时出现。这意味着即使您的实际环境中的映像也可能包含新发现的 CVE，因此您必须主动扫描您的环境中运行的映像。

## Kubernetes 的集装箱安全

云从根本上改变了基础设施安全发生的方式，这意味着对大多数组织来说，重新思考安全性，以规划容器安全性和 Kubernetes 安全性考虑。Kubernetes 是容器编排的事实上的标准，因此将容器安全性视为您的 [Kubernetes 安全性最佳实践](/kubernetes-best-practices-comprehensive-white-paper)的组成部分是至关重要的。Kubernetes 虽然在默认情况下不安全，但确实提供了几个内置的安全特性，包括 Kubernetes 基于角色的访问控制( [RBAC](/blog/introducing-rbac-manager-simplifying-kubernetes-rbac-management) )、网络策略和准入控制器。

Kubernetes 中容器安全性的额外考虑是额外的复杂性。这几乎是一个等式，你必须考虑工程师的数量，乘以云帐户、微服务、API 等的数量。本质上，这意味着你处在一个不断变化的环境中。有很多东西需要跟上，尤其是[一项成熟的技术](https://www.fairwinds.com/kubernetes-maturity-model)，有限的安全人才资源，以及不断增加的合规性要求。在 Kubernetes 中找到获得容器安全性可见性的方法是必不可少的，自动化策略执行是维护持续安全性和治理的要求。 [Datadog](https://www.fairwinds.com/blog/prevent-risk-monitor-kubernetes-fairwinds-datadog) 报道称[一半运行容器的组织使用 Kubernetes](https://www.datadoghq.com/container-report/) ，并且几乎 90%的容器被编排为自动化容器部署和维护，使得容器安全成为 Kubernetes 安全的重要组成部分。

## 什么是集装箱扫描？

容器扫描，也称为容器图像扫描，是扫描容器及其所有组件以识别安全漏洞的过程或方法。对于任何致力于保护容器化开发工作流的团队来说，容器扫描是容器安全的一个重要组成部分。

容器图像来源广泛，包括公共存储库，这可能会增加潜在的危害风险。来自不可信来源的映像可能包含漏洞或恶意组件，并且可能没有正确配置以满足合规性标准。由于图像来源的多样性，保持对容器图像的信任尤为重要，容器扫描可以帮助您更好地理解图像或容器中的组件。为了提高整个应用程序生命周期的安全性，您的团队应该在三个方面利用容器安全性。

1.  **将图像扫描集成到 CI/CD 管道中:**最好在构建容器时扫描组件。如果您构建时存在漏洞，它们将成为您的容器和映像的一部分—您需要修复漏洞并重建容器和映像。集成图像扫描以在代码进入管道之前检测和阻止漏洞，有助于您将安全性向左转移，从而改进您的 DevSecOps 实践。
2.  **扫描您的容器注册表:**容器注册表是您存储所有应用程序映像的地方，它可能包含成百上千个从不同来源构建的映像，包括第三方位置。一个漏洞或不安全的配置可能导致对您的注册表和应用程序的威胁。自动持续扫描注册表中漏洞状态的任何变化，并确保扫描每张图像，以识别和防止潜在的外来威胁。
3.  **运行时的开源漏洞扫描:**要实现最优的容器安全性，不能只扫描注册表——需要自动进行持续扫描，以便在发现新的 CVE 时立即识别它们。持续的自动扫描有助于您检测新的漏洞，向您的安全团队报告发现的问题，并快速修复它们。因为您的环境可能包含多个应用程序和集群，所以最好能够了解跨环境的容器漏洞，以便您可以确定补救工作的优先级。

## 持续的容器扫描至关重要

根据最近的 451 Research 调查，69%的受访者经历过错误配置事件，27%的受访者在运行时经历过安全事件，24%的受访者报告他们有重大漏洞需要修复。容器安全问题影响了许多组织，由于安全问题而延迟了应用程序的部署。延迟部署会影响您的组织向市场交付应用程序和服务的能力，并可能影响您的底线。

## 集装箱安全验证

[Fairwinds Insights](/insights) 支持 Kubernetes-native 安全、策略和治理，通过为您的 Kubernetes 环境提供持续扫描和运行时监控，帮助您的组织降低跨多个团队和集群的风险。Insights 使用 Trivy 定位集装箱风险，Trivy 是一种开源的集装箱扫描解决方案。然后，它会为任何具有已知漏洞的映像创建操作项，以便您了解漏洞所在，并计划补救步骤。

> 对使用 Fairwinds Insights 感兴趣吗？免费提供！点击此处了解更多信息。

当您在 Kubernetes 环境中部署应用程序和服务时，您将拥有多个团队、多个集群和许多正在运行的容器。重要的是要有一个单一的控制面板来显示跨环境的信息，以最大限度地了解复杂的环境，大规模自动化您的安全，并集成左移安全以更快地降低风险。Fairwinds Insights 验证容器安全性和 Kubernetes 安全性最佳实践，帮助您专注于您的应用程序，而不仅仅是维护您的容器和 Kubernetes 环境的安全性。

阅读这份白皮书，了解如何验证容器安全性并[采用 Kubernetes 最佳实践](/kubernetes-best-practices-comprehensive-white-paper)。

[![Download Kubernetes Best Practices Whitepaper](img/c38df324d5163c7ccc9c6b998a78ad26.png)](https://cta-redirect.hubspot.com/cta/redirect/2184645/cb39a009-a458-4282-9211-41e010cb3376)