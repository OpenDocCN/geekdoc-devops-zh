# 持续集成。网络应用| CircleCI

> 原文：<https://circleci.com/blog/cicd-for-dotnet/>

。NET 是一个流行的开源、跨平台开发框架，用于为 web、桌面、移动和云构建快速、可伸缩的全栈应用程序。这种灵活性使得。NET 是开发企业 web 应用程序的领先平台。网络开发是市场上最受欢迎的技能之一。

高性能。NET 团队可以通过采用 [DevOps 原则](/blog/essential-devops-principles/)和一个成熟的、专用的持续集成平台来加速他们的开发周期，并且更加一致和可靠地发布代码。借助 CircleCI 的[原生 Windows 支持](/blog/windows-general-availability-announcement/)，您可以自动化您的构建、测试和部署阶段。NET 开发工作流从概念到产品的快速和安全。

Windows 支持包含在 CircleCI 的免费计划中，因此团队和个人开发者可以利用持续集成带来的效率提升。

## 建筑。CircleCI 上的 NET 项目

CircleCI 的支持。NET 开发从 [Windows 执行环境](/execution-environments/windows/)开始。

Windows executor 使用专用虚拟机为每个作业提供强大的、可定制的构建流程编排，并包括[构建和测试您的所需的所有工具。NET 应用程序，包括:](https://circleci.com/docs/hello-world-windows/#software-pre-installed-in-the-windows-image)

*   Windows Server 2019
*   Visual Studio 2019
*   。网络 5
*   。NET Core SDK

您还可以访问在您的 Windows 环境中运行作业的三个 shell——PowerShell、Bash 和 CMD——您可以使用它们来 [SSH 到您的 Windows VM](https://circleci.com/docs/ssh-access-jobs/) 并对失败的管道进行故障排除。

想要构建私有基础设施的团队可以使用带有机器执行器和 Windows 映像的 [runners](https://circleci.com/docs/runner-overview/) 。为了让执行环境更加灵活，您可以在 Windows executor 上运行 [Docker 容器](/blog/guide-to-using-docker-for-your-ci-cd-pipelines/)。您还可以通过运行您的。NET 核心应用程序、SQL Server 和 Docker executor 上 Linux 容器中的其他依赖项。

的简单演示，演示如何使用 Windows executor 来构建您的？NET 项目，查看 Windows 上的 Hello World。

你可以在 CircleCI 学院的[构建环境课程中或者通过观看约翰·哈蒙德的](https://academy.circleci.com/build-environments-1?access_code=public-2021)[在 CircleCI](https://www.youtube.com/watch?v=stsLkbiskiM) 上使用 Windows 入门视频教程来了解更多关于使用 Windows executor 进行构建的信息。

## 为配置持续集成管道。带球体的网

[orb](https://circleci.com/docs/orb-intro/)是 [CircleCI YAML](/blog/what-is-yaml-a-beginner-s-guide/) 的预打包位，您可以在您的管道配置文件中使用它来降低复杂性，并跨项目共享命令、执行器和作业规范。

使用 [Windows orb](https://circleci.com/developer/orbs/orb/circleci/windows) ，您可以调用 Windows executor，设置想要使用的 Windows 资源的大小，并开始在您的。NET 开发工作流和作业，只需几行代码。

将 Windows orb 添加到您的管道中就像在您的`config.yml`文件中包含以下行一样简单:

```
orbs:
  win: circleci/windows@2.4 
```

您可以通过在配置文件中包含额外的 orb 来扩展管道的功能。例如，你可以使用 [MSIX 球体](https://circleci.com/developer/orbs/orb/circleci/microsoft-msix)来[建造你的。NET 桌面应用程序作为签名的 MSIX 包](/blog/building-msix-installers-on-circleci/)。

游戏开发者可以使用第三方 [Unity orb](https://circleci.com/developer/orbs/orb/w3d/unity-build) 来构建 Unity 项目，并在构建完成后存储构建日志和工件。关于使用 CircleCI 构建 Unity 项目的更多信息，请参见[使用 CircleCI 持续集成 Unity](https://medium.com/xrpractices/continuous-integration-for-unity-using-circleci-8d555f9fa35d)。

## 构建、测试和部署。CI/CD 管道中的. NET 应用程序

随着持续集成管道的建立，您可以自动构建和测试您的。NET 应用程序，每次你的代码发生变化。

例如，您可以使用 Windows orb 来[构建和测试 ASP.NET 核心 web 应用程序](/blog/building-and-testing-an-asp-net-core-application/)。在 Windows 执行环境中，您可以使用`dotnet.exe test`命令对您的代码自动运行单元测试，允许您在问题到达您的用户之前快速识别和修复问题。

您还可以将您的管道配置为[自动部署](https://circleci.com/integrations/deployment/)。NET web 应用程序应用于各种宿主环境。一旦您的应用程序通过了您设置的所有测试，您就可以将您的应用程序部署到任意数量的托管平台上。

您可以使用 [Heroku orb](https://circleci.com/developer/orbs/orb/circleci/heroku) 来[将 ASP.NET 核心应用部署到 Heroku](/blog/deploy-dotnetcore-heroku/) 或 [Azure CLI orb](https://circleci.com/developer/orbs/orb/circleci/azure-cli) 来[将 ASP.NET 核心应用部署到 Azure Web 应用服务](/blog/deploy-dotnetcore-azure/)。自动化部署将消除手动批准步骤，并为您的开发团队节省大量时间和精力。

## 开始建设。CircleCI 上的免费网络

自动化。具有全功能 CI/CD 平台的. NET 开发工作流[使团队更快更有弹性](https://circleci.com/resources/2020-state-of-software-delivery/)。合适的工具让开发者有更多的机会创新，并快速持续地向用户交付价值。CircleCI 一流的支持。NET 开发，包括新的。NET 6 和 Visual Studio 2022 版本，您可以放心地选择 CircleCI 作为您的团队的长期家园，进行持续集成和交付。

了解 CircleCI 如何帮助您构建、测试和部署您的。NET 应用的速度和信心，[今天就注册一个免费的 CircleCI 账户](https://circleci.com/signup/)。