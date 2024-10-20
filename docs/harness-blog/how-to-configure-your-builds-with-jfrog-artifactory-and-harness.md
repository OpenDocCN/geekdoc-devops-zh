# 如何使用 JFrog Artifactory 和 Harness | Harness 配置您的构件

> 原文：<https://www.harness.io/blog/how-to-configure-your-builds-with-jfrog-artifactory-and-harness>

本文将解释开发人员如何使用同类最佳的 DevOps 工具，如 Harness 和 JFrog Artifactory，来测试和构建 CI 并存储构建工件。

‍ [JFrog Artifactory](https://jfrog.com/artifactory/) 是一个用于管理二进制文件、包和存储库的解决方案，在持续集成(CI)和交付(CD)的世界中，它们被称为“软件资产”。Artifactory 帮助软件开发人员编码、存储和跟踪文件及其元数据，而无需了解它们的内部工作原理。它还提供了二进制资产的集中存储，因此其他团队成员可以重用它们。由于所有构建工件和二进制文件都存储在一个地方，Artifactory 提供了整个开发过程的单一视图，使开发人员能够无缝地共享他们的工作。

本文将解释开发人员如何使用同类最佳的 DevOps 工具，如 Harness 和 JFrog Artifactory，来测试和构建 CI 并存储构建工件。这使得团队中的其他开发人员很容易协作并使用存储的工件，这不仅提高了开发人员的生产力，还加快了软件交付过程。

[线束](https://harness.io/)与 Artifactory 无缝集成。用户只需要[设置一个线束持续集成的免费试用](https://app.harness.io/auth/#/signup/?utm_source=internal&utm_medium=social&utm_campaign=community-devrel-post&utm_content=jfrog-harness-article&utm_term=get-started)，添加测试和构建步骤，然后将构建工件推送到 Artifactory。我们将通过一个简单的例子来了解这一点，并了解 Artifactory 和 Harness 如何协同工作来简化您的软件开发生命周期。

## 先决条件

## 设置

首先，用 Maven 构建一个简单的 Java 项目。出于本教程的目的，我已经创建了一个简单的应用程序，您可以克隆它。你可以在 Github 上找到这个例子。

然后，登录到您的 Harness 帐户并创建一个项目。

‍

从列表中选择持续集成模块。

首先向项目添加一个委托，开始项目设置。

***注意:*** 你可能会有点不知所措，不知道什么是委托，为什么需要委托。Harness Delegate 是一个服务/软件，您需要在目标集群(在我们的例子中是 Kubernetes 集群)上安装/运行，以将您的工件、基础设施、协作、验证和其他提供者与 Harness Manager 连接起来。第一次设置线束时，安装线束委托。如果你想了解更多关于委托的信息，请阅读[线束委托文档](https://docs.harness.io/article/h9tkwmkrm7-delegate-installation)。

‍

然后添加 Artifactory 作为新连接器。

‍

添加您的 Artifactory 详细信息，如 URL、用户名和密码。密码通过高度加密的方法得到安全保护。

确保您的连接成功。

‍

接下来，您需要将 repo 添加为连接器。Harness 支持各种代码库管理平台。由于我们的代码在 GitHub 上，我们将选择 Git。

选择 Git 之后，添加存储库细节。

‍

‍

‍

确保连接成功。

现在，转到创建管道步骤，并添加所有相关的细节。

命名管道并继续。

‍

‍

然后选择“构建”作为阶段。

‍

这一步获取我们已经连接的 GitHub repo 和连接器。点击设置阶段。

接下来，定义基础设施。我们使用的是 Kubernetes 集群，我们将指定相同的名称。

‍

‍

‍

在执行阶段，添加步骤并选择**和*运行。***

‍

‍

‍

另一个要添加的连接器是我们的 DockerHub 注册表。让我们添加一个连接符。

确保连接成功。

‍

使用所需的详细信息配置运行设置，如名称、docker 注册连接器、映像和命令。

添加运行步骤后，再添加一个步骤将构建工件推送到 Artifactory。

选择 JFrog Artifactory。

‍

像我们在上一步中所做的那样提及所需的细节。只要确保您的目标和源路径是正确的。

运行 mvn install 后，您可以在项目的目标文件夹中看到您的源路径

现在，我们都准备好了。保存设置并运行管道。

‍

‍

在成功执行所有步骤之后，您应该看到您的构建工件被推送到 Artifactory。让我们转到我们的 Artifactory 帐户/控制面板来验证这一点。

‍

‍

‍

恭喜，你刚刚在 Harness 的帮助下将我们的构建工件推送到 Artifactory。

## 线束工厂集成

Artifactory 是市场上保持二进制文件完整的最佳解决方案之一。此外，它通过重用二进制文件并在整个开发过程中推广它们，帮助开发人员加速软件交付过程。Harness 是一个现代软件交付平台，与 Artifactory 紧密集成，可帮助组织简化开发操作。

准备好使用 Harness CI 进行测试了吗？[下载免费试用版](https://app.harness.io/auth/#/signin)。