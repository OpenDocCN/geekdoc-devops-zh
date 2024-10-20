# 创建自定义 Docker 映像以运行您的 CI 构建| CircleCI

> 原文：<https://circleci.com/blog/creating-a-custom-docker-image-to-run-your-ci-builds/>

本指南提供了关于如何创建一个有效的 Docker 映像作为 [Docker 执行器](https://circleci.com/docs/executor-types/#using-docker)中的主映像的提示。作业中的所有步骤，包括 orb 命令，都将在该映像中执行。然而，本指南并不包括如何构建和发布 Docker 映像，然后在您的生产环境中使用或发布您的开源应用程序。

*欲了解更多 Docker 内容，请访问[CI/CD 管道 Docker 使用指南](https://circleci.com/blog/creating-a-custom-docker-image-to-run-your-ci-builds/)。*

我希望你问问自己，你确定要这么做吗？有许多人不想在运行时安装几个包，因为这会增加他们 20-45 秒的构建时间。然后，他们花几个小时想办法建立自己的 Docker 形象，然后花几个月甚至几年的时间来维护这个形象。有时候不值得努力。

在需要安装大量软件包、需要编译源代码或通过慢速连接下载的情况下，这是您自己的自定义 Docker 映像大放异彩的最佳时机。

还感兴趣吗？太好了，我们开始吧。

## 编写 Dockerfile 文件

一切都以 Dockerfile 开始和结束。这是创建 Docker 图像的蓝图。有关 Docker 文件的更多信息，请访问 Docker 的[入门-第 2 部分](https://docs.docker.com/get-started/part2/)指南。

基本图像

Dockerfile 通常以声明新图像将使用的基础图像的`FROM`语句开始。虽然不是必须的，但我们强烈建议使用 CircleCI base 便利图像作为您的基本图像。

```
FROM cimg/base:stable 
```

CircleCI 基础映像是所有下一代便利映像的基础。你可以在这里了解更多关于所有新图片的信息[。它完全是为了在 CircleCI 上工作而设计的。它拥有我们大多数客户需要的所有必需的工具以及最常用的工具。例如，](https://circleci.com/blog/announcing-our-next-generation-convenience-images-smaller-faster-more-deterministic/) [checkout](https://circleci.com/docs/configuration-reference/#checkout) 特殊步骤要求`git`安装在 Docker 映像中。 [save_cache](https://circleci.com/docs/configuration-reference/#save_cache) 和 [persist_workspace](https://circleci.com/docs/configuration-reference/#persist_to_workspace) 特殊步骤，以及它们的加载等价物，需要安装`tar`和`gzip`。我们的基本映像包括所有这些工具，并且我们确保随着额外需求的增加或删除，软件列表得到维护。

这个图像在我们的平台上也相当受欢迎，这意味着通过将您的图像基于我们的基础图像可以获得缓存优势。您可以在 GitHub 页面上了解更多关于 CircleCI base 便利图片以及如何使用它的信息。如果你选择使用不同的基本映像，我建议你至少访问一下 GitHub 页面看看我们安装了什么包，这样你就可以自己复制这个列表了。

### 安装和下载软件

```
RUN sudo apt-get update && sudo apt-get install -y \
        bison \
        llvm \
        zlib1g-dev \
        xz-utils && \
    rm -rf /var/lib/apt/lists/* 
```

在本例中，我们可以学习几个最佳实践:

*   当使用不以 root 用户身份运行的映像时，需要在一些命令前加上前缀`sudo`。
*   在脚本场景中，总是使用`apt-get`而不是`apt`。前者更适合没有人类的环境，而后者更适合在你自己的本地计算机上摆弄。
*   当用软件包管理器安装时，当软件包管理器试图询问一个问题时，像`-y`这样的标志被用来假定“是”。
*   每行按字母顺序列出一个包。使用 git 和 GitHub 创造奇迹。这使得阅读源代码更加愉快，当查看 PR 差异时，可以非常清楚地看到哪些包被添加或删除。
*   最后一行删除图像中的 Apt 缓存。这有助于缓存，但更重要的是减小了图像的大小。

```
ENV GO_VERSION=1.14.1
RUN curl -sSL "https://golang.org/dl/go${GO_VERSION}.linux-amd64.tar.gz" | \
    sudo tar -xz -C /usr/local/ 
```

这个例子给了我们另外两个提示:

*   将频繁变化的字符串设置为环境变量，可以更直观地管理变化。尤其是因为 ENV 指令在 Docker 中不算一个层。当该值在即将到来的`RUN`指令中被多次使用时，这种技术大大提高了效率。
*   在这里，cURL 下载了一个 tarball，并将其直接传送到下一个命令 tar 中。这允许我们避免将文件保存到文件系统中，这样速度更快，也避免了事后清理 tarball。在你不能使用这个技巧的情况下，不要忘记用`rm`删除 tarball、zip 包等来清理自己。

### 缓存和效率

对于较大的图像，指令的顺序很重要。您希望`RUN`变化频繁的步骤靠近 docker 文件的底部，而变化不频繁的步骤应该靠近顶部。这是因为 Docker 按层缓存图像。每当一个层发生变化时，该层及其下的层都需要重新缓存。这种行为在 Docker 的 [Dockerfile 最佳实践指南](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)中有更详细的解释。

另一个效率项目是图像层应该尽可能的精简。不需要的文件应该在创建它的`RUN`步骤中删除，这样可以使该层的整体尺寸更小。

## 维护形象

创建自定义图像只是工作的一半。一旦该映像存在，就需要进行维护。随着您的 CI 需求的变化和增长，您的形象也需要随之调整。

### 你的 Dockerfile 的家

让你的 docker 文件处于版本控制之下。有些人喜欢将这个 docker 文件保存在与他们需要它的项目相同的存储库中。其他人，比如我自己，更喜欢把它放在自己的仓库里。

我更喜欢单独的回购途径，因为:

*   当它与项目在同一个 repo 中时，该项目的 CircleCI 配置会变得更加复杂。您不希望在每次提交主项目时都构建 Docker 映像。那是浪费时间，代价昂贵。为了避免这种情况，您需要在构建项目和图像时进行逻辑分离。
*   作为一个独立的回购，它使得在您的公司或团队的多个项目中使用该图像变得更加容易。

### 保持 Dockerfile 文件最新

当在 docker 文件中添加或删除内容时，用您的更改创建一个新的分支，检查并合并。标准的东西。Docker 映像的某些部分会在 Docker 文件之外进行更新。我们如何保持这些部分的更新？

您的基础映像是它自己的项目，并按照自己的时间表进行更新。例如，CircleCI base 便利映像每个月会进行一次稳定的更新。如果您下载并安装文件名为`example.com/download/some-thing-4.3.x.tar.gz`的软件，您将需要确保使用该软件可用的最新补丁版本更新您的映像。我们通过 CircleCI [计划工作流](https://circleci.com/docs/workflows/)来实现这一点。

使用预定的工作流程发布您的自定 Docker 图像可让您定期更新图像的背景部分，而无需您进行任何手动操作。这样做的频率取决于你的个人需求。CircleCI 基础映像每月 2 日更新。如果你以此为基础，你自己在 3 号或 5 号安排的每月工作流程会很好。

### 托管图像

您可以选择在哪里托管您发布的 Docker 图像。存放 Docker 映像的地方称为 Docker 注册表。托管 Docker 映像的实际位置是 Docker Hub。如果你对其他任何注册表都不熟悉，坚持使用 DockerHub 就没问题了。它可以免费使用，即使是私人图像。

如果您使用领先的云提供商进行托管，他们可能会有一个 Docker 注册表供您使用，我们可能会为该提供商提供一个 [CircleCI orb](https://circleci.com/orbs/) ,使所有这些设置更加容易。以下是主要的 Docker 注册表和相应的 orb:

## 讨论和反馈

要进一步讨论这个话题或提出问题，请访问我们的 [CircleCI 讨论](https://discuss.circleci.com/)论坛。你可以找到我和 CircleCI 的用户，他们就像你一样，随时准备讨论和提供帮助。