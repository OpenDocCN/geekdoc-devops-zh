# 在 WSL 2 中使用 GitKraken 客户端|更好的开发体验

> 原文：<https://www.gitkraken.com/blog/wsl2-and-gitkraken-client>

对于喜欢 Linux 开发环境但需要使用 Windows 作为主要操作系统的开发人员来说，WSL 2 非常有用。开发人员和团队使用 GitKraken 客户端来可视化和有效地使用他们的 Git 存储库。

有了 GitKraken 客户端的 [9.1 版本，就可以结合两种资源的优点。在本文中，我们将探索在 WSL 2 中使用 GitKraken 客户端的优点，并提供如何设置的分步说明。](https://www.gitkraken.com/blog/gitkraken-client-9-1)

如果你正在读这篇文章，你很可能属于两个阵营之一:

高级 WSL/GitKraken 用户

## 如果您已经掌握了 WSL 2 的丰富经验，下面让我们快速了解一下 GitKraken Client 9.1 是如何改进这种体验的。

GitKraken Client 9.1 允许您立即使用 WSL 2(WSLg)*处理您的回购。这是通过安装两个 GitKraken 客户端来实现的，一个安装在您的 Windows 机器上，另一个安装在您的 WSLg Linux 环境中。这使您可以根据您的存储库所在的位置，在 GitKraken 的两个安装之间快速切换。*

 *我们知道，许多用户希望只在 Windows 端安装 GitKraken 客户端，然后在 WSL Linux 中无缝读取和使用 git repos。然而，现在有显著的架构障碍阻止我们实现这一点。为了避免花很长时间来获得有用的东西，我们决定现在就开始帮助每个人使用 WSL。

此更新的另一个好处是改进了 GitKraken Client 的 Linux 版本，以修复在 WSL 2 环境中操作时的常见问题。这将允许用户以最小的摩擦跨 Windows 和 WSL 实例操作 GitKraken 客户端。

请继续阅读关于如何在 WSL 2 上下载和安装 GitKraken 客户端的说明

WSL/GitKraken 初学者

对于那些不确定 WSL/WSL 2 是什么或者刚刚开始使用 GitKraken 客户端的人。WSL，即 Linux 的 Windows 子系统，是由微软开发的兼容层，允许在 Windows 10/11 上本地运行 Linux 应用程序。WSL 2 是 WSL 的最新版本，它使用由微软构建和维护的真正的 Linux 内核，从而提高了兼容性和性能。WSL 2 在 Windows 中提供了一个完整的 Linux 环境，允许用户运行 Linux 工具和应用程序，包括系统服务，以及 Windows 应用程序和工具。

## 总结一下:

WSL 的总括术语，有时人们说这意味着第一次迭代(WSL 1)通常，人们使用 WSL 2 并且对它感兴趣。

**WSL 2:** 当前用于 Linux 的 Windows 子系统。它相当稳定，使用完整的 Linux 内核。

**WSLg:**“g”代表图形。它为您的 Linux 实例启用了一个 GUI，并允许 GitKraken 客户端在 WSL 2 环境中工作。这实际上并不是一件独立的事情，它是随着 WSL 2 的最新版本自动出现的。

**WSL 2:** 当前用于 Linux 的 Windows 子系统。它相当稳定，使用完整的 Linux 内核。

继续阅读关于如何在 WSL 2 上下载和安装 GitKraken 客户端的说明，并寻找我们在整篇文章中为初学者确定的特定标注。

如何在 WSL 2 中下载和安装 GitKraken 客户端

GitKraken 客户端与 Windows 11 或 Windows 10 build 19044 或更高版本上的 WSL 2 兼容，其中包括用于 GUI 支持的 WSLg。下面几节详细介绍了如何更新或安装 WSL 2，以及一旦有了 WSL 2 的最新版本，如何下载 GitKraken Client。请务必遵循与您的经验水平相符的说明。

## 如何在 WSL 2 中下载和安装 GitKraken 客户端

高级 WSL/GitKraken 用户:如果已经安装了 WSL 2，请更新它

如果您已经在 Linux 发行版中安装了 WSL，请从 PowerShell 或以管理员身份打开的 Windows 命令提示符下运行以下命令，以更新到 WSL 的最新版本。

### `wsl --update `

WSL/GitKraken 初学者:安装 WSL 2

如果您需要使用 Linux 的默认 Ubuntu 发行版安装 WSL，只需从 PowerShell 或以管理员身份打开的 Windows 命令提示符下运行以下命令，并根据提示重新启动。

`wsl --install -d ubuntu`

### WSL/GitKraken 初学者:安装 WSL 2

重新启动后，安装将继续。您将被要求在 Linux 终端中输入用户名和密码。这些将是您的 Linux 凭证。

WSLg 会自动包含在更新和初始 WSL 设置中，因此您可以准备安装 GitKraken 客户端了。

在 WSL 2 上下载并安装 GitKraken 客户端

在 WSL 2 实例上安装哪个版本的 GitKraken 客户端取决于您使用的 Linux 发行版。最常用的是 Ubuntu，但你也可能在用别的。请参见下面的安装选项:

对于 Ubuntu(和其他基于 Debian 的发行版)，您可以在 Linux 终端中运行以下命令来下载和安装 GitKraken 客户端:

### `wget https://release.gitkraken.com/linux/gitkraken-amd64.deb`

`sudo apt install ./gitkraken-amd64.deb`

对于 Ubuntu(和其他基于 Debian 的发行版)，您可以在 Linux 终端中运行以下命令来下载和安装 GitKraken 客户端:

根据您的喜好，您也可以使用 **.tar.gz** 来代替(尽管在大多数情况下最简单的就是**)。deb【上面的 )。要安装 GitKraken 客户端的. tar.gz 版本，请在 Linux 终端中运行以下命令**

`wget https://release.gitkraken.com/linux/gitkraken-amd64.tar.gz`

`sudo tar -xvzf gitkraken-amd64.tar.gz`

根据您的喜好，您也可以使用 **.tar.gz** 来代替(尽管在大多数情况下最简单的就是**)。deb【上面的 )。要安装 GitKraken 客户端的. tar.gz 版本，请在 Linux 终端中运行以下命令**

如果您正在使用其他 Linux 发行版，如 RHEL 7+，CentOS 7+或 Fedora 34+，您可能希望使用**。转速**。

`wget https://release.gitkraken.com/linux/gitkraken-amd64.rpm`

`sudo yum install ./gitkraken-amd64.rpm`

如果您正在使用其他 Linux 发行版，如 RHEL 7+，CentOS 7+或 Fedora 34+，您可能希望使用**。转速**。

如果安装因缺少软件包而未完成，您可能需要在尝试再次安装之前运行以下命令。

`sudo apt --fix-broken install`

你都准备好了！您现在应该能够打开 GitKraken 客户端并使用 WSL 中存储的 repos。

`sudo apt --fix-broken install`

在 WSL 2 中使用 GitKraken 客户端的好处

GitKraken 客户端帮助开发人员可视化他们的 Git 库，并使使用 Git 变得容易。在 WSL 2 中使用 GitKraken Client 的好处很简单——您可以一起使用 Linux 和 GitKraken Client，就像您正在本地运行它们一样！将该应用程序安装在 WSL 2 / WSLg 上的 Linux 实例中，您将获得与在任何非虚拟 Linux 机器上运行 GitKraken 客户端几乎相同的性能速度。这为您带来了一个 Linux 环境，带有著名的 Git 客户机，同时运行 Windows 作为主要操作系统。

## 在 WSL 2 中使用 GitKraken 客户端的好处

GitKraken 客户端帮助开发人员可视化他们的 Git 库，并使使用 Git 变得容易。在 WSL 2 中使用 GitKraken Client 的好处很简单——您可以一起使用 Linux 和 GitKraken Client，就像您正在本地运行它们一样！将该应用程序安装在 WSL 2 / WSLg 上的 Linux 实例中，您将获得与在任何非虚拟 Linux 机器上运行 GitKraken 客户端几乎相同的性能速度。这为您带来了一个 Linux 环境，带有著名的 Git 客户机，同时运行 Windows 作为主要操作系统。*