# 构建和测试 ASP.NET 核心应用程序

> 原文：<https://circleci.com/blog/building-and-testing-an-asp-net-core-application/>

本文探讨了如何使用 CircleCI 的 Windows 执行环境在 Windows 虚拟机上构建和运行一个简单的计算器应用程序，该应用程序是使用 ASP.NET 核心构建的。它包括应用程序的完整配置文件，并对相关部分进行了分解解释。

web 和应用程序开发中有许多框架，每一个都有不同的特性和优势。ASP.NET 是一个开源的服务器端 web 应用程序框架，由微软创建，运行在 Windows 上，允许开发者创建 web 应用程序、web 服务和动态内容驱动的网站。ASP.NET 核心是 ASP.NET 的一个改进的跨平台版本，可以在每一个主要的计算平台上运行，包括 Windows、macOS 和 Linux。

CircleCI 的 Windows 支持构建和测试您的应用程序的好处是:

*   支持基于 Docker 的 Windows 工作流的 Docker 引擎企业版
*   Windows 环境作业基于虚拟机，并提供完全的构建隔离
*   PowerShell 是默认的 Shell，但是 Bash 和 cmd 可以通过声明获得
*   Windows 作业可以访问同一套强大的 CircleCI 功能，如缓存、工作区、审批作业和受限上下文，并且支持级别相同

## 先决条件

要跟进，您需要:

## 申请详情

本教程使用了一个简单的计算器应用程序，您可以在这里找到并派生出。以下是应用程序的目录结构:

```
.
├── Dockerfile
├── LICENSE
├── README.md
├── SimpleCalc
│   ├── Controllers
│   ├── Models
│   ├── Program.cs
│   ├── Services
│   ├── SimpleCalc.csproj
│   ├── Startup.cs
│   ├── Views
│   ├── appsettings.Development.json
│   ├── appsettings.json
│   └── wwwroot
├── SimpleCalc.sln
└── test
    └── SimpleCalc.Tests

8 directories, 9 files 
```

一旦你完成了这个项目，按照[的说明在 CircleCI](https://circleci.com/docs/getting-started/#setting-up-your-build-on-circleci) 上建立你的构建，把它连接到 CircleCI。

## 配置 CircleCI

为了[配置 CircleCI](https://circleci.com/docs/configuration-reference/) ，我在项目根目录下的`.circleci`文件夹中添加了一个`config.yml`文件。以下是该项目的完整配置文件:

```
version: 2.1

orbs:
  windows: circleci/windows@2.2.0

jobs:
  test:
    description: Setup and run application tests
    executor:
      name: windows/default
    steps:
      - checkout
      - restore_cache:
          keys:
            - dotnet-packages-v1-{{ checksum "SimpleCalc/SimpleCalc.csproj" }}
      - run:
          name: "Install project dependencies"
          command: dotnet.exe restore
      - save_cache:
          paths:
            - C:\Users\circleci\.nuget\packages
          key: dotnet-packages-v1-{{ checksum "SimpleCalc/SimpleCalc.csproj" }}
      - run:
          name: "Run Application Tests"
          command: dotnet.exe test -v n --results-directory:test_coverage --collect:"Code Coverage"
      - run:
          name: "Print Working Directory"
          command: pwd
      - store_artifacts:
          path: C:\Users\circleci\project\test_coverage
  build:
    description: Build application with Release configuration
    executor:
      name: windows/default
    steps:
      - checkout
      - run:
          name: "Build Application according to some given configuration"
          command: dotnet.exe build --configuration Release
workflows:
  test_and_build:
    jobs:
      - test
      - build:
          requires:
            - test 
```

现在，我们来分解一下配置。

### 定义操作系统/环境和运行时

我们正在使用 CircleCI 版，它支持使用 [orbs](https://circleci.com/orbs/) 。CircleCI orbs 是可共享的配置元素包，包括作业、命令和执行器。我们正在使用的是 [Windows orb](https://circleci.com/developer/orbs/orb/circleci/windows) 。它为用户提供了构建 Windows 项目的工具，如通用 Windows 平台(UWP)应用程序、. NET 可执行文件或特定于 Windows 的项目(如。NET 框架)。

```
version: 2.1

orbs:
  windows: circleci/windows@2.2.0 
```

该项目有`test`和`build`个作业。构建作业需要通过所有测试才能运行。为了配置这种排序，我们将使用 [CircleCI 的工作流](https://circleci.com/docs/workflows/)，这是一组用于定义作业集合及其运行顺序的规则。工作流声明位于示例配置文件的末尾。

```
workflows:
  test_and_build:
    jobs:
      - test
      - build:
          requires:
            - test 
```

### 设置和运行测试

Windows executor 预装了 Visual Studio 2019 以及许多其他开发工具。我们将使用默认的 PowerShell shell，所以它不需要声明。

```
 test:
    description: Setup and run application tests
    executor:
      name: windows/default 
```

[步骤](https://circleci.com/docs/jobs-steps/#steps-overview)是在作业期间运行的可执行命令的集合。在接下来的步骤中，我们将检查我们的代码并恢复我们项目的依赖项。

```
 steps:
      - checkout
      - restore_cache:
          keys:
            - dotnet-packages-v1-{{ checksum "SimpleCalc/SimpleCalc.csproj" }}
      - run:
          name: "Install project dependencies"
          command: dotnet.exe restore
      - save_cache:
          paths:
            - C:\Users\circleci\.nuget\packages
          key: dotnet-packages-v1-{{ checksum "SimpleCalc/SimpleCalc.csproj" }} 
```

CircleCI 的[缓存文档](https://circleci.com/docs/caching/)详细描述了可用的缓存选项以及如何使用它们。`dotnet.exe restore`命令使用 NuGet 来恢复依赖项以及项目文件中指定的项目特定工具。默认情况下，依赖项和工具的恢复是并行执行的。

单元测试对于现实世界的应用程序是必不可少的。您还可以从单元测试中收集代码覆盖率。单元测试是一种软件测试方法，通过这种方法对源代码的各个单元进行测试，以确定它们是否适合使用。单元是最小的可测试软件组件。“dotnet.exe 测试”是。用于执行单元测试的. NET 测试驱动程序。我们的项目使用了 xUnit 测试框架。如果所有测试都成功，测试运行程序返回 0 作为退出代码；否则，如果任何测试失败，它将返回 1。测试运行器和单元测试库被打包为 NuGet 包，并被恢复为项目的普通依赖项。

```
 - run:
         name: "Run Application Tests"
         command: dotnet.exe test -v n --results-directory:test_coverage --collect:"Code Coverage" 
```

我们将从运行测试中收集的覆盖率作为工件存储在`test_coverage`文件夹中。ASP.NET 核心项目的代码覆盖率报告不是现成的。

```
 - store_artifacts:
         path: C:\Users\circleci\project\test_coverage 
```

### 构建项目

在成功运行测试之后，我们想要构建我们的项目。为此，我们定义要使用的构建配置。此项目的可用选项有:

*   释放；排放；发布
*   调试下面的配置显示了我们选择的版本。

```
 build:
    description: Build application with Release configuration
    executor:
      name: windows/default
    steps:
      - checkout
      - run:
          name: "Build Application according to some given configuration"
          command: dotnet.exe build --configuration Release 
```

从……开始。NET Core 2.0 SDK，您不必运行“dotnet restore”，因为它是由所有需要进行恢复的命令隐式运行的，如“dotnet new”、“dotnet build”和“dotnet run”。

## 结论

在 CircleCI 上使用 Windows executor 使得构建和测试 ASP.NET 核心应用程序变得轻而易举。CircleCI certified Windows orb 是一个值得信赖的解决方案，因为它经过了 CircleCI 的测试和社区的验证。要了解更多关于您的配置中可用的 orb 的信息，请访问 [orb 注册表](https://circleci.com/developer/orbs)。

orb 节省了原本要花在以下方面的时间:

*   研究、测试和设置 Windows 虚拟机
*   安装开发工具，例如 vs2019、Nuget
*   学习使用 Windows 工具，如 PowerShell

orb 的使用还减少了必须在配置文件中编写的代码行数。这个项目的配置文件有 42 行代码，更容易理解和维护。

* * *

Dominic Motuka 是 Andela 的 DevOps 工程师，在 AWS 和 GCP 支持、自动化和优化生产就绪部署方面拥有 4 年多的实践经验，利用配置管理、CI/CD 和 DevOps 流程。

[阅读多米尼克·莫图卡的更多帖子](/blog/author/dominic-motuka/)