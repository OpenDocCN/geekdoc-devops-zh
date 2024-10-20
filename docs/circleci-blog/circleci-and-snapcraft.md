# CircleCI 和 Snapcraft:你需要知道的一切

> 原文：<https://circleci.com/blog/circleci-and-snapcraft/>

软件包管理系统 Snapcraft 在 Linux 平台上为自己的位置而战，它重新想象了你如何交付你的软件。一组新的跨发行版工具可以帮助您构建和发布“快照”。我们将介绍如何使用 CircleCI 2.0 来支持这个过程，以及在这个过程中可能遇到的一些问题。

## 什么是快照包？还有 Snapcraft？

**快照**是 Linux 发行版的软件包。它们的设计吸取了在 Android 以及物联网设备等移动平台上交付软件的经验教训。 **Snapcraft** 是一个包含快照和构建快照的命令行工具的名称，[是一个网站](https://snapcraft.io)，几乎是围绕实现这一点的技术的整个生态系统。

Snap 包旨在隔离和封装整个应用程序。这一概念实现了 Snapcraft 提高软件安全性、稳定性和可移植性的目标，允许单个“snap”不仅可以安装在多个版本的 Ubuntu 上，还可以安装在 Debian、Fedora、Arch 等版本上。根据 Snapcraft 的网站:

> 为每个 Linux 桌面、服务器、云或设备打包任何应用程序，并直接提供更新。

## 资源

### CircleCI 上的建筑快照

Snap 包可以在 CircleCI 上快速无缝地构建。我们有一个关于如何在 CircleCI [这里](https://circleci.com/docs/build-publish-snap-packages/)构建 snap 包的文档。Snapcraft 自己的[文档网站](https://docs.snapcraft.io)也是一个有价值的资源。

### Docker 图像

Snapcraft CLI `snapcraft`在 Ubuntu 中运行得非常好。然而，如果你试图在其他发行版中构建快照，可能会涉及更多的工作。本着与 snap 本身类似的精神，Snapcraft 团队创建了 Docker 映像，允许构建 snap 包，而不管 Linux 发行版(也包括 macOS 和 Windows)。这意味着您可以在 Debian 或 Fedora 上本地构建快照，或者在 Ubuntu 上构建完全干净的环境，与文件系统的其余部分分离。

我已经基于官方的 Snapcraft 映像创建了一组第二方的 Docker 映像，这些映像旨在在 CircleCI 等持续集成(CI)环境中很好地工作。两个 Docker 图片都可以在下面找到。

Snapcraft Docker 图像:

### CircleCI 本地 CLI

[CircleCI Local CLI](https://circleci.com/docs/local-cli/) 是一个省时的工具，允许您在本地运行 CI 构建，就在您自己的机器上。这是最有用的替代方法，可以替代[进入构建](https://circleci.com/docs/ssh-access-jobs/)进行故障排除。另一个成功的特性是在提交和推送到 GitHub 之前验证 CircleCI 配置文件的能力。再也不会因为您不小心在`.circleci/config.yml`中留下了一个“标签”而收到一封关于构建失败的电子邮件。

使用 CircleCI 文档中描述的构建过程，本地 CLI 现在被打包成一个 snap 包，可以通过 [Snap Store](https://snapcraft.io/store) 立即获得。安装说明可以在[这里](https://circleci.com/docs/local-cli/#ubuntu-1604-fedora-27-and-other-distros-supporting-snap-packages)找到，但是它的要点是，对于 Ubuntu 16.04+用户:

```
sudo snap install docker circleci
sudo snap connect circleci:docker docker 
```

## 包管理和开源

你的团队计划使用其他工具打包软件吗？有没有您希望看到的开源流程和工作流？如果你想看到更多关于这些话题的博客帖子和文档，请在 [CircleCI 讨论](https://discuss.circleci.com)中告诉我们，或者甚至在 Twitter 上给我喊一声: [@FelicianoTech](https://twitter.com/FelicianoTech) 。