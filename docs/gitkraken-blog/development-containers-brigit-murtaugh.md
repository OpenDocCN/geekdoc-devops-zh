# 开发容器| GitKon 2022 | Brigit Murtaugh，微软

> 原文：<https://www.gitkraken.com/gitkon/development-containers-brigit-murtaugh>

[https://www.youtube.com/embed/6IesIsmzzRc?feature=oembed](https://www.youtube.com/embed/6IesIsmzzRc?feature=oembed)

视频

容器在部署时一直被用来标准化应用程序，但现在有很好的机会来支持其他场景，包括持续集成(CI)、测试自动化和全功能编码环境。

开发容器为这些场景提供了环境，并确保您的项目拥有所需的工具和软件。虽然部署和开发容器可能彼此相似，但您可能不希望在开发过程中使用的部署映像中包含工具。

自 2019 年以来，Visual Studio 代码中已经[支持开发容器，最近](https://code.visualstudio.com/docs/remote/containers) [GitHub 代码空间](https://github.com/features/codespaces)也支持开发容器。这种支持得到了 **devcontainer.json** 的支持，这是一种带有注释的结构化 JSON(jsonc)元数据格式，用于配置容器化的环境。

## **开发容器规格**

随着容器化生产工作负载变得越来越普遍，开发容器在 VS 代码之外的场景中变得越来越有用。微软和 GitHub 的一个团队已经启动了开发容器规范，该规范允许任何人使用任何工具来配置一致的开发环境。

您可以在相关的 [containers.dev spec 网站](https://containers.dev/implementors/spec/)上浏览该规范，并在 [devcontainers/spec](https://containers.dev/) repo 中查看该规范及其活动提案。与开发容器和规范工作相关的其他 repos 也在 [devcontainers GitHub org](https://github.com/devcontainers) 中托管。

让我们更深入地了解开发容器规范，以及如何立即开始使用开发容器:

## **开发容器 CLI**

开发容器 CLI 是该规范的开源参考实现。它可以在 [devcontainers/cli](https://github.com/devcontainers/cli) 存储库中获得。

当像 VS Code 和 Codespaces 这样的工具在用户的项目中检测到`devcontainer.json`时，它们使用 CLI 来配置开发容器。这就打开了这个 CLI，使得单个用户和其他工具可以读取 devcontainer.json 元数据并从中创建 dev 容器。

开发容器 CLI 既可以直接使用，也可以集成到产品体验中，类似于今天它与远程容器和代码空间的集成。dev container CLI 目前既支持简单的单个容器选项，也支持与用于多容器场景的 [Docker Compose](https://docs.docker.com/compose/) 集成。

dev 容器 CLI 也可用于预构建映像，以加快启动时间。

## **用于构建和测试的开发容器**

除了可重复的设置之外，相同的开发容器还提供了一致性，以避免开发人员之间的环境特定问题，以及集中的构建和测试自动化服务。

GitHub 动作和 Azure DevOps 任务在 [devcontainers/ci](https://github.com/devcontainers/ci/blob/main/README.md) 中可用，用于在持续集成(ci)构建中运行存储库的开发容器。这允许您重用用于本地开发的相同设置，以便在 CI 中构建和测试您的代码。

## **开发容器特性**

开发容器中的特性是安装代码的自包含单元，旨在安装到各种基础容器映像上。

微软和 VS Code 的团队最近开源了一个新的 [devcontainers/features](https://code.visualstudio.com/blogs/2022/09/15/dev-container-features) 库。从 devcontainers/features 中引用特性就像在您的`devcontainer.json`中添加一个`features`属性一样简单。

以下示例安装了 go 和 docker-in-docker 功能:

`"name": "my-project-devcontainer",`

`"image": "mcr.microsoft.com/devcontainers/base:ubuntu", // Any generic, debian-based  image. `

`"features": { `

`"ghcr.io/devcontainers/features/go:1": { `

`"version": "1.18" `

`}, `

`"ghcr.io/devcontainers/features/docker-in-docker:1": {`

`"version": "latest",`

`"moby": true `

`} `

`}`

以下是官方和社区支持的开发容器特性:

### **开发容器特性创作**

如果您想创建自己的开发容器特性，新的[开发容器特性模板库](https://github.com/microsoft/dev-container-features-template)是一个很好的起点。除了包含给定特性内容的良好模板，该模板还包含 GitHub Actions 工作流，使用 GitHub 容器注册表为您的帐户快速发布它们，让您尽快启动并运行。

开发容器功能有两个必需的组件:

1.  `install.sh`:安装入口点脚本。这在概念上作为映像 Dockerfile 的一层添加，并在构建时执行。这个入口点脚本可以安装语言(即 ruby)和工具(即 GitHub CLI)。
2.  `devcontainer-feature.json`:它包含了关于特性的元数据，一组可以在安装过程中传递给特性的安装脚本的选项，以及将被合并到最终开发容器中的`devcontainer.json`的“片段”。

Dev 容器特性可以用多种语言编写，最简单的是 [shell 脚本](https://www.gitkraken.com/blog/shell-scripting)。如果一个特性是用不同的语言创作的，那么关于该特性的信息应该包含在元数据中，以便用户可以做出明智的决定。

### **开发容器特性分布**

开发容器特性以 tarballs 的形式分发。tarball 包含特性子目录的全部内容，包括`devcontainer-feature.json`、`install.sh`和目录中的任何其他文件。

开放容器倡议(Open Container Initiative)又称 OCI，定义了容器和容器资源的行业标准。微软将开发容器特性视为 OCI 工件，并使用 OCI 注册中心的概念来分发它们。

上面提到的 features template repository 包含了一个 GitHub Actions 工作流来自动化发布过程。它将每个特性打包成一个 tarball，并将资产作为 OCI 工件发布给 GHCR。

通过在 GitHub 上的库的`Actions`选项卡左侧选择模板库来触发`release.yaml`工作流。这将在`<owner>/<repo>`名称空间下向 GHCR 发布每个特性。仅当`devcontainer-feature.json`中的版本属性被更新时，特征才被重新发布。

### **共享开发容器特性**

如果您希望您的贡献出现在 VS 代码远程开发容器或 GitHub 代码空间 UI 中，用于开发容器的创建，您可以执行以下操作:

转到 [devcontainers.github.io](https://containers.dev/)

打开一个 PR 来修改`collection-index.yml`文件

## 开发容器还有什么新特性？

除了新的开发容器功能 repo，微软最近还开源了一个[dev containers/images repository](https://github.com/devcontainers/images)，在那里他们托管了一组特定的图像，这些图像之前在[vs code-dev-containers repository](https://github.com/microsoft/vscode-dev-containers)中。

微软团队正在为开发容器模板开发一个社区分发计划——在 vscode-dev-containers 中也称为“定义”——这将类似于特性。更新将在 vscode-dev-containers repo 中共享。

## **了解更多关于开发容器的信息**

如果你有兴趣了解更多关于开发容器的知识，你可以看看 https://containers.dev/和 T2 的开发容器/规范回购。

在 repo 中，您将能够审查和评论活动的提议，如开发容器特性和特性发布的提议，并创建您自己的提议。您还可以在`devcontainers` org 中的任何回购上打开问题和[拉请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git),以帮助塑造开发容器的未来。

如果您在工作流程中使用 GitHub 和 VS 代码，请考虑使用 GitLens+ for VS 代码升级您的游戏，提供强大的可视化和协作功能。

[Download GitLens Free!](https://www.gitkraken.com/gitlens/try-free)

本文中的信息由微软 Visual Studio 代码团队的高级项目经理 Brigit Murtaugh 提供。

*联系人:brigit.murtaugh@microsoft.com；* [@BrigitMurtaugh *在推特上*](https://twitter.com/brigitmurtaugh)