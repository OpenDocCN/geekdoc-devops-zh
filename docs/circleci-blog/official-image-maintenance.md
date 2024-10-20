# 我们对构建映像的更新策略- CircleCI

> 原文：<https://circleci.com/blog/official-image-maintenance/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

以下是我们将在 CircleCI 2.0 和 CircleCI 1.0 上更新构建映像的时间和原因。

## CircleCI 图像

### Docker 便利图片

**[文档页面](https://circleci.com/docs/circleci-images/)-48 小时内更新** - CircleCI 2.0 支持使用 Docker 映像作为 CI 构建的环境。有些用户不想制作自定义映像，或者他们使用的映像缺少在 CI 环境中有用的工具。这就是便利图片的用武之地。这些图片基于官方的 Docker 库图片。由于我们获取这些图像并添加常用工具(如`git`和`tar`)以及浏览器工具(chrome、selenium 等)，更新频率基于上游图像。我们通常会在 Docker 库版本发布 24-48 小时后进行图像更新。

### Ubuntu 14.04

**[文档页面](https://circleci.com/docs/1.0/build-image-trusty/) -每季度更新** - CircleCI 1.0 使用超大 LXC 图片。这是因为我们在一个图像中有许多语言和所有相关的工具。如果我们删除用户正在使用的软件版本，更新此映像有时会导致用户的构建中断。这就是为什么我们建议切换到 CircleCI 2.0，在那里这种情况不会再发生。为了最大限度地减少损坏，CircleCI 1.0 Ubuntu 14.04 映像大约每季度(每 3 个月)更新一次。不会添加映像中不存在的软件。只会添加现有软件的新的主要版本。

### Ubuntu 12.04

**[“文档”页面](https://circleci.com/docs/1.0/build-image-precise/)——从不更新**——这张图片是 CircleCI 1.0 的大型 LXC 图片，类似于 Ubuntu 14.04 的图片。生产 Ubuntu 的 Canonical 公司已经停止生产 Ubuntu 12.04。这意味着将不会有更多的错误修复或安全更新。反过来，我们也停止了对该图像的支持。我们建议改用 CircleCI 2.0。如果你因为任何原因不得不继续使用 CircleCI 1.0，你可以切换到 Ubuntu 14.04 镜像来继续更新。Canonical 的停产公告可以在[这里](https://lists.ubuntu.com/archives/ubuntu-announce/2017-March/000218.html)找到。

## 反馈和贡献

CircleCI 2.0 Docker 便利图片随着 2.0 的[发布而开花结果。他们仍然是相当新的和开源的。如果你想浏览 docker 文件的源代码，报告错误，或者修复问题，你可以在](https://circleci.com/blog/launching-today-circleci-2-0-reaches-general-availability/) [GitHub 项目](https://github.com/circleci/circleci-images)上这样做。

如果您不熟悉 CircleCI 2.0、Docker 或我们的图像，您可以在[circle ci discuse](https://discuss.circleci.com/)上提问、查看示例并讨论工作流程。

最后，你也可以使用 CircleCI 讨论来分享你的公共 Docker 图片或 CircleCI 配置片段，你认为这些可以帮助其他人。我们很想看看你在做什么。