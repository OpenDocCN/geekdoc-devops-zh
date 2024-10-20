# Git 下载|下载 Git for Windows & Mac |安装 Git for Linux

> 原文：<https://www.gitkraken.com/learn/git/git-download>

要开始使用 Git，首先需要为您的操作系统下载 Git。在本文中，我们将介绍如何为 Windows、Mac 和 Linux 下载 Git。您还将学习如何用您的身份配置 Git，这样您就可以开始您的第一个 Git 项目了。

### **为您的操作系统下载 Git**

[下载 Windows 版 Git](https://www.gitkraken.com/learn/git/git-download#download-git-for-windows)

[下载 Mac 版 Git](https://www.gitkraken.com/learn/git/git-download#download-git-for-mac)

[为 Linux 安装 Git](https://www.gitkraken.com/learn/git/git-download#install-git-for-linux)

[用你的身份配置 Git](https://www.gitkraken.com/learn/git/git-download#configure-git)

[Git 下载常见问题解答](https://www.gitkraken.com/learn/git/git-download#git-download-faqs)

## **关键词&资源**

Git 是由 Linus Torvalds 在 2005 年创建的分布式版本控制系统(VCS) [。Git 用于跟踪项目中文件的变更，并允许用户恢复到他们项目的先前版本。Git 因为其分布式模型、分支、速度等等，迅速成为最流行的版本控制系统。](https://git-scm.com/book/en/v2/Getting-Started-A-Short-History-of-Git)

[Git SCM](https://git-scm.com/)——这是 Git 的官网。该资源提供了关于什么是 Git 的内容，关于如何使用 Git 的文档，以及与 Git 社区连接和联网的方法。

Pro Git Book–通常简称为“Git Book”，它包含对 Git 命令和功能的解释。Git 这本书非常详细，并且是用技术语言编写的，所以初学者可能会觉得它的材料有些混乱。随着您对 Git 的亲身体验，Git 手册中的内容会变得更容易理解，但是知道 Git 手册的存在是很重要的，因为在您下载 Git 之后，我们将使用它在您的机器上配置 Git。

Edward Thomson 在 GitKon Git 会议上的讲话提供了关于版本控制进展和 Git 历史的更多信息。

想在下载之前尝试使用 Git 吗？你可以用 GitKraken 客户端做到这一点！运行 Git 命令，试验 CLI，体验 Git 工作流，而无需下载 Git。

## **如何为 Windows 下载 Git**

[https://www.youtube.com/embed/j-g8AXr4nR4](https://www.youtube.com/embed/j-g8AXr4nR4)

VIDEO

要下载 Git for Windows，您首先需要导航到 Git SCM 上的 [Git download for Windows 页面。接下来，您需要确定您的机器是 32 位还是 64 位。要确定您的机器是 32 位还是 64 位，只需在开始菜单(通常位于屏幕左下方)中键入`32 or 64 bit`，然后点击`enter`。](https://git-scm.com/download/win)

这将在您的计算机上打开一个`About`菜单，并显示您的计算机是 32 位还是 64 位。接下来，选择与您的系统相匹配的选项—`32-bit Git for Windows Setup`或`64-bit Git for Windows Setup`——开始 Git download for Windows 进程。

### **Git Windows 安装下载**

当 Git 开始下载时，您会看到几个自动出现的设置选项。对于大多数设置选项，您可以简单地接受默认设置，只有一个例外。

通过初始设置选项选择`Next`,直到到达标题为`Adjusting the name of the initial branch in new repositories`的选项。

从这里，选择显示`Override the default branch name for new repositories`的选项，并在下面的文本框中输入`main`。

这使得每当您创建一个新的存储库时，您的初始 Git 分支将被称为`main`而不是`master`。将您的默认分支机构名称更改为 main 是公认的技术行业标准。看看这篇文章，了解更多关于从[主人到主](https://www.theserverside.com/feature/Why-GitHub-renamed-its-master-branch-to-main)的变化。

从这里开始，您可以通过选择剩余选项中的`Next`继续接受默认设置。当您看到一个`Install`选项时，您将知道您已经结束了设置选项。

选择`Install`，会出现一个状态栏，显示 Git 下载的进度。根据您的互联网连接，完成 Windows Git 下载可能需要 1-5 分钟。一旦 Git 下载状态栏被填充，您应该已经在您的计算机上为 Windows 下载了 Git。

### **验证您的 Windows Git 下载**

要再次检查 Git 是否已经正确下载到您的机器上，导航到一个终端，比如 GitKraken CLI，并运行`git –version`。您的终端应该返回这条消息:`git version #.##.#`。

恭喜你！现在您的 Windows 版 Git 下载已经完成，[跳到本文的底部](https://www.gitkraken.com/learn/git/git-download#configure-git)以获得配置 Git 的指南和加速您的 Git 知识的技巧。

## **如何下载 Mac 版 Git**

[https://www.youtube.com/embed/r3SgprWigBc](https://www.youtube.com/embed/r3SgprWigBc)

VIDEO

要在你的苹果电脑上下载 Git for Mac，导航到 Git SCM 上的 [MacOS Git 下载页面。在下载 Git for Mac 之前，您首先需要安装 Homebrew。家酿是一个](https://git-scm.com/download/mac)[软件包管理器](https://en.wikipedia.org/wiki/Package_manager)，它使安装和更新软件包变得容易。要安装自制软件，选择 Git SCM 页面顶部的[自制软件链接](https://brew.sh/)。现在，从 Homebrew 页面中，复制标题下面的代码字符串`Install Homebrew`。

`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

接下来，导航到您选择的终端，粘贴您从家酿页面复制的代码。这将自动开始在你的机器上安装家酿。

根据您的计算机设置，您可能需要在安装 Homebrew 之前输入您的计算机密码。当您的终端返回以下消息时，您就知道已经安装了 home brew:`Installations successful`。

### **用自制软件下载 Mac 版 Git**

一旦家酿安装完毕，导航回到 Git SCM 上的 [MacOS Git 下载页面。从这里，复制在标题`$ brew install git`下找到的代码。将代码粘贴到你的终端，然后点击`return`；这将开始为 Mac 下载 Git。](https://git-scm.com/download/mac)

一旦您的 Git 下载完成，终端将返回如下所示的消息:``Summary 🍺  /usr/local/Cellar/git/#.##.#.``。这只是一个 Homebrew 提供的下载摘要，告诉你你的 Git 下载成功了，并显示你的 Git 下载在你的机器上的位置。

### **验证您为 Mac 下载的 Git**

为了仔细检查 Git 是否已经正确下载到您的机器上，导航到一个终端并运行`git –version`。您的终端应该返回这条消息:`git version #.##.#`。

恭喜你！现在您的 Mac 版 Git 下载已经完成，[跳到本文的底部](http://gitkraken.com/learn/git/git-download#configure-git)获取配置 Git 的指南和提高 Git 知识的技巧。

## **如何安装 Linux 版 Git**

在 Linux 上安装 Git 非常简单。事实上，很多 Linux 系统已经默认安装了 Git。

要确认您的计算机是否已经下载了 Git，请打开终端并键入:`git –version`。如果已经安装了 Git，您的终端将返回类似于:`git version 2.36.1`的内容。如果您的机器没有安装 Git，终端将返回一个错误消息。

在您的 Linux 系统上下载 Git 最简单的方法是使用您的特定发行版的首选包管理器。虽然在 Linux 系统上下载 Git 的具体命令不同，但是下载 Git 的过程非常相似。

首先，更新您的包列表，然后下载您的系统支持的最新版本的 Git。让我们看看最流行的 Linux 发行版是什么样子的。

### **如何在 Ubuntu / Debian 上安装 Git**

要安装 Git for Ubuntu，请从终端运行以下命令:

更新系统包列表:

`sudo apt-get update`

在 Ubuntu / Debian 中安装 Git:

`apt-get install git`

### **在 Fedora 上安装 Git**

要在 Fedora 上安装 Git，从终端运行以下命令:

更新系统包列表:

`sudo dnf -y update`

在 Fedora 上安装 Git:

`sudo dnf -y install git`

### **Gentoo 安装 Git**

要在 Gentoo 上安装 Git，从您的终端运行这些命令:
更新系统包列表:

`emerge -avDuN world`

在 Gentoo 上安装 Git:

`emerge --ask --verbose dev-vcs/git`

### **在 Arch Linux 上安装 Git**

要在 Arch Linux 上安装 Git，从终端运行以下命令:

更新系统包列表:

`sudo pacman --sync --refresh --sysupgrade`

在 Arch Linux 上安装 Git:

`pacman -S git`

**确认您在 Linux 上的 Git 安装**

### 一旦您运行了特定于您的包管理器的代码来在 Linux 上下载 Git，您将希望确认您的 Git 在 Linux 上的安装是成功的。要确保 Git 已经正确下载到您的机器上，请导航到一个终端并键入:`git –version`。您的终端应该返回此消息:`git version #.##.#`

一旦您运行了特定于您的包管理器的代码来在 Linux 上下载 Git，您将希望确认您的 Git 在 Linux 上的安装是成功的。要确保 Git 已经正确下载到您的机器上，请导航到一个终端并键入:`git –version`。您的终端应该返回此消息:`git version #.##.#`

**为您的操作系统配置 Git**

## 在开始使用 Git 跟踪您的项目之前，您需要配置您的身份。您必须配置您的身份，因为正如 [Pro Git 一书所述](https://git-scm.com/book/en/v2/Getting-Started-First-Time-Git-Setup#:~:text=list%20%2D%2Dshow%2Dorigin-,Your%20Identity,-The%20first%20thing)，“每个 Git 提交都使用这个信息，并且它不可改变地融入到您开始创建的提交中”。要配置您的身份，您需要打开一个终端并运行以下命令:

配置您的用户名:

`git config --global user.name "Your Name"`

配置您的电子邮件:

`git config --global user.email [youremail@example.com](mailto:youremail@example.com)`

将默认分支名称配置为 Main(仅限 Mac 和 Linux):

`git config `--`全局初始化.默认分支主`

您可以通过运行`git config –list`来验证您已经正确配置了 Git。如果配置正确，终端将返回您在上面输入的用户名、电子邮件和默认分行名称。

`git config `--`全局初始化.默认分支主`

**Git 下载常见问题解答**

**问:如何从 Git 下载**

答:Git 是一种软件，一旦你下载了 Git，它就会在你的机器上本地运行。选择本文顶部的操作系统，下载 Git for Windows、Mac 或 Linux。

## **Git 下载常见问题解答**

### **问:如何用 Git 下载**

答:Git 可以从 Git 官方网站 [Git SCM](https://git-scm.com/) 下载。

### Git 可以有一个陡峭的学习曲线。GitKraken Client 提供了自动建议和自动完成 Git 命令、可视化提交图、集成终端等等，使 Git 变得更加简单和直观。

答:Git 可以从 Git 官方网站 [Git SCM](https://git-scm.com/) 下载。

**利用 GitKraken 的学习 Git 资源加快您的进度**

现在您已经下载并配置了 Git，您可以开始您的第一个 Git 项目了！当您使用 Git 并参与到项目中时，您会发现一个由开发人员、学者、游戏玩家、天才以及介于两者之间的所有人组成的充满活力的社区。

如果你想加快你的进度，这样你就可以开始对你的工作或开源项目做出有意义的贡献，看看 GitKraken 的 [Learn Git resources](https://www.gitkraken.com/learn/git) 。GitKraken 的 Learn Git 库包括 Git 教程、定义、最佳实践等等，完全免费，一定会帮助您将 Git 技能提升到一个新的水平。

## **利用 GitKraken 的学习 Git 资源加快您的进度**

现在您已经下载并配置了 Git，您可以开始您的第一个 Git 项目了！当您使用 Git 并参与到项目中时，您会发现一个由开发人员、学者、游戏玩家、天才以及介于两者之间的所有人组成的充满活力的社区。

如果你想加快你的进度，这样你就可以开始对你的工作或开源项目做出有意义的贡献，看看 GitKraken 的 [Learn Git resources](https://www.gitkraken.com/learn/git) 。GitKraken 的 Learn Git 库包括 Git 教程、定义、最佳实践等等，完全免费，一定会帮助您将 Git 技能提升到一个新的水平。