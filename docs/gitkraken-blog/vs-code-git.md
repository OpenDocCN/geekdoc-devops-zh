# 在 VS 代码中使用 Git

> 原文：<https://www.gitkraken.com/blog/vs-code-git>

在本文中，我们将介绍如何结合开发人员可用的一些最强大的工具——VS Code 和 Git——开始为开源和私有项目做出有意义的贡献。在我们开始用 VS 代码驱动 Git 之前，让我们先了解一些背景知识。

本文涵盖的主题

#### [在 VS 代码中使用 Git 的先决条件](https://www.gitkraken.com/blog/vs-code-git#prerequisites-to-using-git-in-vs-code)

[在 VS 代码中初始化 Git Repo](https://www.gitkraken.com/blog/vs-code-git#initializing-a-git-repo-in-vs-code)

[让 Git 提交 VS 代码](https://www.gitkraken.com/blog/vs-code-git#making-git-commits-in-vs-code)

[使用 VS 代码](https://www.gitkraken.com/blog/vs-code-git#connecting-a-remote-repository-with-vs-code)连接远程存储库

[在 VS 代码](https://www.gitkraken.com/blog/vs-code-git#pushing-changes-to-a-remote-repo-in-git-with-vs-code)中将更改推送到远程

[在 VS 代码](https://www.gitkraken.com/blog/vs-code-git#pulling-changes-from-a-remote-repo-in-git-with-vs-code)中从远程获取变更

什么是 VS 代码？

visual Studio Code(VS Code)是一个简化的代码编辑器，可以在 Mac、Windows 和 Linux 上使用，它具有交互式调试器、与流行的开发人员工具的大量集成，以及使编写代码更容易的大量扩展。

## 根据 [StackOverflow 2021 年开发者调查](https://insights.stackoverflow.com/survey/2021#section-most-popular-technologies-integrated-development-environment)，VS Code 是目前最受欢迎的集成开发环境，超过 80，000 名受访者中有 71%表示 VS Code 是他们的首选 IDE。有关 VS Code 的功能和历史的更多信息，请访问 VS Code 的官方[“为什么 VS Code”](https://code.visualstudio.com/docs/editor/whyvscode)网站。

Git 是什么？

Git 是一种版本控制的分布式方法。Git 是由 Linus Torvalds 在 2005 年创建的，用于管理 Linux 内核的开发。简而言之，Git 跟踪对文件所做的更改，允许您在必要时恢复到以前的文件版本。

## Git lens——VS 代码的免费 Git 扩展，越来越好！

GitLens 拥有超过 2000 万的安装量，是 Visual Studio 代码最受欢迎的 Git 扩展。它为您提供了关于代码作者身份的有价值的见解，并释放了 Git 在 VS 代码中的全部力量。

2022 年，我们增加了一套全新的强大功能，增强了 GitLens 的体验，称为 GitLens+功能。我们引入的第一个 GitLens+特性是可视化文件历史视图和工作树。在发布的时候，这些功能需要一个免费的帐户来访问本地和公共托管的存储库，而私人托管的存储库则需要一个付费帐户。

## 然而，随着 2022 年 10 月 GitLens 13 的发布——推出了漂亮的新提交图——我们希望每个人都能从本地和公共存储库中强大的 GitLens+功能中受益，而不会有任何摩擦。因此，我们消除了自由账户的障碍。现在，访问 GitLens+功能的唯一要求是拥有一个用于私人存储库的 Pro 帐户。

我们使用 GitLens+功能的目的是创建一个可持续的模型，使我们能够继续大力投资 GitLens 的免费和开源核心，这些功能可以提高工作效率、安全性、易用性等，同时创建一组专门为专业开发人员和团队设计的附加但完全可选的功能。

为了帮助您识别 GitLens+功能，它们标有✨symbol.

然而，随着 2022 年 10 月 GitLens 13 的发布——推出了漂亮的新提交图——我们希望每个人都能从本地和公共存储库中强大的 GitLens+功能中受益，而不会有任何摩擦。因此，我们消除了自由账户的障碍。现在，访问 GitLens+功能的唯一要求是拥有一个用于私人存储库的 Pro 帐户。

想在使用 Git 和 VS 代码方面领先一步吗？下载 GitLens 扩展以访问有价值的文件历史和代码作者信息，从而满怀信心地开始编码。

为了帮助您识别 GitLens+功能，它们标有✨symbol.

GitLens 增强了 VS 代码中内置的 Git 功能。无论您是经验丰富的 Git 开发人员还是刚刚入门，GitLens 都能让您更轻松、更安全、更快速地利用 Git 的全部功能。本 GitLens 教程将向您展示如何在 VS 代码中使用 GitLens，并充分利用该工具。[注册 GitLens+,充分利用您的体验——这是免费的](https://bit.ly/3PHA9L8)！

[Install GitLens Free!](https://www.gitkraken.com/gitlens/try-free)

[https://www.youtube.com/embed/UQPb73Zz9qk?feature=oembed](https://www.youtube.com/embed/UQPb73Zz9qk?feature=oembed)

视频

GitLens 增强了 VS 代码中内置的 Git 功能。无论您是经验丰富的 Git 开发人员还是刚刚入门，GitLens 都能让您更轻松、更安全、更快速地利用 Git 的全部功能。本 GitLens 教程将向您展示如何在 VS 代码中使用 GitLens，并充分利用该工具。[注册 GitLens+,充分利用您的体验——这是免费的](https://bit.ly/3PHA9L8)！

现在让我们开始看看如何使用 VS 代码完成 Git 工作流。您将学习如何创建一个 [Git 存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)，提交更改，连接一个遥控器，推送更改，并在整个过程中利用 GitLens 扩展，使在 VS 代码中使用 Git 更加强大。

[https://www.youtube.com/embed/UQPb73Zz9qk?feature=oembed](https://www.youtube.com/embed/UQPb73Zz9qk?feature=oembed)

视频

在 VS 代码中使用 Git 的先决条件

在开始利用 VS 代码中的 Git 集成之前，首先需要确保已经下载并配置了 Git。要检查是否安装了 Git，使用键盘快捷键`CTRL +` 在 VS 代码中打开集成终端，然后键入`git --version`。如果终端返回`git version`,后跟任何版本号，这表明您实际上已经安装了 Git。如果没有返回任何信息，您将需要导航到您的操作系统的[https://git-scm.com/downloads](https://git-scm.com/downloads)。

接下来，您应该通过设置您的姓名和电子邮件来验证 Git 是否配置正确。要检查您是否已经配置了 Git，请导航到终端并运行

## 在 VS 代码中使用 Git 的先决条件

`git config --global --list`.

接下来，您应该通过设置您的姓名和电子邮件来验证 Git 是否配置正确。要检查您是否已经配置了 Git，请导航到终端并运行

如果已经配置了 Git，终端将返回您设置的用户名和电子邮件。如果不是这样，您需要通过完成以下步骤来配置 Git:

要设置您的跑步名称:

`git config  --global user.name <your name>`

要设置您的电子邮件运行:

`git config  --global user.email <your email>`

这些信息是使用 Git 所必需的，并且必须在提交任何更改之前输入。有关 Git 设置和初始要求的更多信息，请查看官方 Git 文档。

在 VS 代码中打开 Git Repo

一旦您确认 Git 已经在您的机器上正确安装和配置，您将需要初始化一个本地存储库或者克隆并打开一个现有的项目。

初始化回购可以通过多种方式完成，您选择使用的流程取决于您期望的工作流程和当前环境。一般来说，开发人员 [Git 在他们想要为现有项目做贡献时会克隆一个远程存储库](https://www.gitkraken.com/learn/git/git-clone)，而在开始一个全新的项目时会创建一个新的存储库。

`git config  --global user.email <your email>`

用 VS 代码克隆一个带有 URL 的 Git 存储库

要使用带有 VS 代码的 URL 将现有存储库克隆到您的本地机器，请按照下列步骤操作:

1.选择位于活动栏上的源代码管理图标

## 在 VS 代码中打开 Git Repo

一旦您确认 Git 已经在您的机器上正确安装和配置，您将需要初始化一个本地存储库或者克隆并打开一个现有的项目。

2.然后选择`Clone Repository`

3.导航到您首选的存储库托管服务，如 [GitHub](https://github.com/)

### 4.导航到左上角的搜索栏，搜索所需的远程存储库，然后选择 repo

要使用带有 VS 代码的 URL 将现有存储库克隆到您的本地机器，请按照下列步骤操作:

*搜索您想要的远程存储库*

5.选择绿色的`Code`按钮

6.复制远程存储库 URL

*复制远程 URL*

7.将 URL 粘贴到 VS 代码命令面板中

从那里，您将被提示在您的本地机器上为项目选择一个保存位置。一旦完成了这些，您就可以开始对存储库进行操作了。

使用 VS 代码命令面板克隆现有的 Git Repo

您可以克隆一个现有的存储库，以便在您的本地环境中开始工作。要在 VS 代码中克隆现有的 Git 存储库，请遵循以下步骤:

1.选择位于活动栏上的源代码管理图标

*复制远程 URL*

2.选择`Clone Repository`

3.从命令面板下拉菜单中，选择您想要从中克隆存储库的托管服务，比如 GitHub

4.选择要克隆的远程存储库

4.选择要克隆的远程存储库

该项目将被复制到您的本地计算机上，您可以开始工作了。

### 您可以克隆一个现有的存储库，以便在您的本地环境中开始工作。要在 VS 代码中克隆现有的 Git 存储库，请遵循以下步骤:

使用 VS 代码终端初始化一个新的 Git Repo

要使用 VS 代码中的终端初始化一个新的 Git repo，首先在本地机器上创建并命名一个新文件夹。

接下来，只需点击进入 VS 代码终端并运行`git init <your-folder’s-name>`。这将在 VS 代码中打开该文件夹，并在项目的. git 文件夹中自动创建 Git 元数据文件。

2.选择`Clone Repository`

4.选择要克隆的远程存储库

使用侧栏按钮在 VS 代码中初始化一个新的 Git Repo

您还可以选择使用 VS 代码侧栏按钮来初始化一个存储库。要以这种方式初始化存储库:

1.在您的计算机上创建并命名一个新文件夹

2.转到 VS 代码，选择主屏幕上开始菜单下的`Open`

### 要使用 VS 代码中的终端初始化一个新的 Git repo，首先在本地机器上创建并命名一个新文件夹。

接下来，只需点击进入 VS 代码终端并运行`git init <your-folder’s-name>`。这将在 VS 代码中打开该文件夹，并在项目的. git 文件夹中自动创建 Git 元数据文件。

3.打开新创建的文件

4.从活动栏中选择源代码管理图标

5.点击`Initialize Repository`

### VS 代码中的 Git 工作流

一旦您对项目进行了更改并希望确保这些更改得到保存，提交这些更改是一个好主意。将提交视为工作中的保存点可能会有所帮助。

随着项目的增长，您可以查看、修改和构建旧的提交。提交变更的 Git 工作流如下所示:

编辑您的项目

上演你的改变

编写提交消息

犯罪

使用小程序来跟踪 vs 代码中的变化

✨Commit 图

GitLens 提交图使您能够毫不费力地可视化和监控所有正在进行的工作。提供关于每次提交的大量信息。您可以看到每个节点的作者、日期和提交消息，甚至可以看到在每次提交中更改了哪些文件。这使得理解您的代码库的演变和识别可能需要注意的区域变得容易。

## 提交图也是交互式的，具有全功能的上下文菜单，使您可以轻松地与 Git 直接交互。执行各种操作，如合并、精选和恢复更改，或者使用丰富的提交图搜索。

使用 GitLens 的提交图，跟踪和管理工作进度从未如此简单或高效。

## 通过右键单击上下文菜单与分支、提交、标签等进行交互

双击分支以签出分支

提交的丰富搜索和过滤器

1.  获取有关拉取请求的信息
2.  用于快速查看回购活动的活动小地图
3.  更改列以可视化每次提交时对文件所做的更改
4.  滚动标记快速找到并跳转到感兴趣的点

提交图可供所有在公共存储库上工作的用户使用，并且不需要帐户。此外，付费订阅 GitLens+的用户可以通过私有回购使用提交图。

### ✨Commit Graph

#### 内联责备注释和 CodeLens

GitLens 内联责备注释显示文件中每行代码的作者和提交信息。这些信息以工具提示的形式显示，当用户将鼠标悬停在一行代码上时，工具提示就会出现。工具提示显示详细信息，如作者的姓名、用户名和进行更改的提交消息。

这个特性有助于您跟踪变更并了解每一行代码的历史。它还使您能够追溯对特定行所做的更改，并了解是谁做了更改。责备注释可以用来识别不同团队成员所做的代码贡献，这有助于分配代码的信用和责任。它还使用户能够识别和跟踪代码中的 bug 和错误，从而更容易解决问题。

提交图也是交互式的，具有全功能的上下文菜单，使您可以轻松地与 Git 直接交互。执行各种操作，如合并、精选和恢复更改，或者使用丰富的提交图搜索。

使用 GitLens 的提交图，跟踪和管理工作进度从未如此简单或高效。

CodeLens 是 VS 代码中最强大的工具之一，它提供了可点击的链接来展示提交细节，并允许您从快速选择菜单中进行选择，以比较、导航和进一步探索每个提交。

*   文件注释
*   使用文件注释，您可以在一个方便的地方查看关于 Git 存储库和文件的详细信息。当您切换文件注释时，GitLens 会提供一个侧边栏，显示关于当前打开文件的信息。您可以查看文件的提交历史，包括每次提交的作者和日期，甚至可以通过将鼠标悬停在特定提交上来查看完整的提交消息。
*   GitLens 文件注释还显示了每次提交时对文件所做的更改。您可以看到添加、修改或删除了哪些行，从而便于跟踪更改并了解文件随时间的演变。文件注释包括一个交互式代码热图，它直观地显示哪些代码行更改最多。这使得发现需要注意或优化的代码区域变得容易。
*   如果您在团队环境中工作，文件注释可以帮助您识别谁在何时对文件进行了更改。您可以看到每个团队成员所做的贡献，并且可以快速导航到每个变更的提交历史。
*   状态栏责备
*   快速方便地查看当前选中代码行的作者和提交信息，带有状态栏责备。这些信息显示在编辑器窗口底部的 Visual Studio 代码状态栏中。状态栏也会显示代码的年龄信息，所以你可以看到它是多久前最后一次修改的。这对于识别陈旧或过时的代码，或者理解特定代码更改的上下文非常有用。
*   只需点击任意一行代码，GitLens 就会在状态栏显示作者、提交和年龄信息。

提交详细信息

Commit Details 视图提供了关于提交和隐藏的丰富细节。快速查看有关提交的作者、提交 ID、相关问题和拉请求的链接、更改的文件等信息。

#### 当取消锁定时，提交详细信息视图上下文跟随您在编辑器中导航文件，以及在✨提交图、提交视图、存储视图、交互式 Rebase 编辑器和✨可视文件历史中选择提交和存储。您还可以从悬停、上下文菜单和快速选择菜单的提交详细信息视图中打开提交或存储。

在 VS 代码中进行变更

git tens–提交图中的 wip

在您对文件进行更改并保存文件后，您在 GitLens 中正在进行的工作或 WIPs 将在 GitLens 提交图上显示为“正在进行的工作”行。只需单击鼠标右键，即可访问 WIP 上下文菜单。让您访问一系列有用的选项，包括隐藏所有更改、打开细节和开放源代码控制。这些功能允许您更有效地管理 WIP。

VS 代码用户界面

VS 代码通过引用活动栏上的源代码控制图标，可以很容易地识别在任何给定的时间您的项目有多少未分级的更改，其中蓝色的圆圈表示您的项目有多少未分级的更改。

当您准备好暂存您的更改时，您可以通过将鼠标悬停在文件上并选择`+`图标来暂存单个文件。

#### 使用文件注释，您可以在一个方便的地方查看关于 Git 存储库和文件的详细信息。当您切换文件注释时，GitLens 会提供一个侧边栏，显示关于当前打开文件的信息。您可以查看文件的提交历史，包括每次提交的作者和日期，甚至可以通过将鼠标悬停在特定提交上来查看完整的提交消息。

GitLens 文件注释还显示了每次提交时对文件所做的更改。您可以看到添加、修改或删除了哪些行，从而便于跟踪更改并了解文件随时间的演变。文件注释包括一个交互式代码热图，它直观地显示哪些代码行更改最多。这使得发现需要注意或优化的代码区域变得容易。

要从终端暂存单个文件，只需运行`git add <file name>`。

要从终端暂存单个文件，只需运行`git add <file name>`。

#### 快速方便地查看当前选中代码行的作者和提交信息，带有状态栏责备。这些信息显示在编辑器窗口底部的 Visual Studio 代码状态栏中。状态栏也会显示代码的年龄信息，所以你可以看到它是多久前最后一次修改的。这对于识别陈旧或过时的代码，或者理解特定代码更改的上下文非常有用。

或者，如果您有多个文件要暂存，并且您希望一次暂存所有文件，您可以导航到`Changes`并选择`+`图标。

或者，如果您有多个文件要暂存，并且您希望一次暂存所有文件，您可以导航到`Changes`并选择`+`图标。

*一次存放所有更改的文件*

#### Commit Details 视图提供了关于提交和隐藏的丰富细节。快速查看有关提交的作者、提交 ID、相关问题和拉请求的链接、更改的文件等信息。

要从终端暂存您的所有更改，只需运行`git add --all`。

在 VS 代码中进行变更

*警告:将所有的变更一起提交是一个坏习惯。一起暂存和提交的文件共享相同的提交消息，这会使您和其他贡献者对您的 Git 日志感到困惑。提交您的更改通常会限制提交不相关的工作块的需要，并且从长远来看会节省您的时间。

在 VS 代码中提交更改

### 一旦你想要的文件被上传，你就可以 [Git 提交](https://www.gitkraken.com/learn/git/commit)你的修改了。撰写一条清晰的提交消息，在标记为`Message`的部分(就在您存放文件的位置上方)传达您的更改目的，然后选择`✓`图标进行提交。

VS 代码用户界面

#### 要使用 VS 代码集成终端提交，运行`git commit -m`。`-m`标志用于包含提交消息，解释您的更改如何影响项目。包含一个 [Git 提交消息](https://www.gitkraken.com/learn/git/best-practices/git-commit-message)是一个最佳实践；这使您和其他项目参与者能够更容易地跟踪对项目所做的更改。

当您准备好暂存您的更改时，您可以通过将鼠标悬停在文件上并选择`+`图标来暂存单个文件。

#### 要使用终端验证您的提交是否成功，请运行`git log`。终端应该返回最近的提交及其相关消息。

如果您喜欢更直观的方法，您可以通过在 VS 代码中导航到 Timeline 视图来确认您的提交是否成功。要访问该视图，从活动面板中选择浏览器图标，然后选择`Timeline`。

或者，如果您有多个文件要暂存，并且您希望一次暂存所有文件，您可以导航到`Changes`并选择`+`图标。

使用 GitLens 访问有价值的提交历史信息

一旦您的更改被提交，GitLens 责备注释变得更加有价值。

使用 GitLens 责备注释，您可以识别谁进行了特定的更改、何时进行了更改、提交 SHA 等等。要访问 GitLens 中的注释，只需选择并悬停在所需的已提交变更上，信息就会出现在代码行的下方。

要从终端暂存您的所有更改，只需运行`git add --all`。

*GitLens 怪注释*

您还可以使用 GitLens 浏览项目的修订历史。如果你想看看一个文件是如何随着时间的推移而改变的，或者将你的项目的当前状态与一周前进行比较，GitLens 完全可以做到这一点。

要在 GitLens 中访问所需文件的修订历史，请执行以下操作:

选择您想要的文件

点按屏幕右上角看起来像一个向左箭头的圆形图标，以返回到项目的以前迭代。

一旦你想要的文件被上传，你就可以 [Git 提交](https://www.gitkraken.com/learn/git/commit)你的修改了。撰写一条清晰的提交消息，在标记为`Message`的部分(就在您存放文件的位置上方)传达您的更改目的，然后选择`✓`图标进行提交。

用 VS 代码连接 Git 中的远程存储库

### 在 Git 的[远程存储库](https://www.gitkraken.com/learn/git/git-remote)上托管项目是大多数开发人员与其他贡献者共享工作的方式。无论是作为企业团队的一员还是开源贡献者，远程存储库都是项目的最新版本，并被视为事实的最终来源。如果您希望其他人能够看到您的代码并添加到您的项目中，您需要为他们设置一个远程存储库来访问项目的代码库。

要创建远程存储库，请导航到 [GitHub](https://www.gitkraken.com/integrations/github) ，并选择`New`。

*创建新的 GitHub 远程 repo*

要使用终端验证您的提交是否成功，请运行`git log`。终端应该返回最近的提交及其相关消息。

选择`New`后，你的屏幕应该像这样。

要创建远程存储库，请导航到 [GitHub](https://www.gitkraken.com/integrations/github) ，并选择`New`。

*正在创建一个新的 GitHub 远程存储库*

*创建新的 GitHub 远程 repo*

输入远程存储库名称并选择`Create repository`。创建远程存储库后，您可以选择使用 HTTPS 或 SSH 密钥复制远程存储库的 URL。一旦复制了 URL，就可以使用 VS 代码终端或命令面板将本地项目连接到远程存储库。

一旦您的更改被提交，GitLens 责备注释变得更加有价值。

*正在创建一个新的 GitHub 远程存储库*

使用 VS 代码集成终端连接远程 Repo

### 如果您是以键盘为先的开发人员，您可能希望使用终端将本地连接到远程。您可以按照以下步骤在 VS 代码中做到这一点:

复制远程回购 URL

导航到 VS 代码终端

运行`git remote add origin <paste the URL>`并点击`Enter`

点按屏幕右上角看起来像一个向左箭头的圆形图标，以返回到项目的以前迭代。

使用 VS 代码状态栏连接远程 Repo

如果您的 GitHub 帐户连接到 VS 代码，您可以直接从 VS 代码快速创建远程存储库。要在 VS 代码中做到这一点，请遵循以下步骤:

导航到窗口底部的状态栏

1.  选择看起来像带有向上箭头的云的图标
2.  从命令面板中，选择发布到公共或私有存储库

要创建远程存储库，请导航到 [GitHub](https://www.gitkraken.com/integrations/github) ，并选择`New`。

无论您是如何连接到遥控器的，您都可以通过查看窗口底部的状态栏图标来确认您的项目实际上已连接。如果您的本地项目没有连接到远程存储库，状态栏会显示一个图标，看起来像一个带向上箭头的云。如果连接了遥控器，您将会看到一个类似刷新圆圈的图标。

此外，您可以导航到活动栏上的源代码控制图标，然后选择`Remotes`来验证您的 repo 是否已连接。如果连接了遥控器，遥控器栏将如下所示:`REMOTES (1)`。括号中的数字表示连接的远程资料库的数量。

## 选择看起来像带有向上箭头的云的图标

To create a remote repository, navigate to [GitHub](https://www.gitkraken.com/integrations/github), and select `New`. 

用 VS 代码将更改推送到 Git 中的远程 Repo

一旦连接了远程 repo，您就可以开始通过一个称为 pushing 的过程将您对本地项目所做的更改发送到远程。推送通常有助于让您的远程 repo 与您最近的更改保持同步，并使其他项目参与者能够处理这些更改。

要从边栏中 [Git push](https://www.gitkraken.com/learn/git/git-push) 您的阶段性变更，只需选择源代码控制图标，然后选择`Sync Changes`。

*用侧杆*将更改推至遥控器

用 VS 代码将更改推送到 Git 中的远程 Repo

要使用集成终端将您提交的更改推送到遥控器，只需运行`git push origin main`。

如果您是以键盘为先的开发人员，您可能希望使用终端将本地连接到远程。您可以按照以下步骤在 VS 代码中做到这一点:

导航到 VS 代码终端

利用 GitLens 确定项目参与者及其代码

### GitLens 可以通过添加 contributors 视图来帮助您识别项目的所有参与者。这在处理大型项目时会很有帮助，因为它允许您通过简单地点击他们的个人资料来查看所有的合作者以及他们对项目的贡献。

要添加这个视图，只需导航到 VS 代码命令面板，搜索`GitLens: Show Contributors View`，并从下拉菜单中选择它。在下面描述的 VS 代码中，源代码管理图标下会出现一个新选项:

1.  如果您的 GitHub 帐户连接到 VS 代码，您可以直接从 VS 代码快速创建远程存储库。要在 VS 代码中做到这一点，请遵循以下步骤:
2.  导航到窗口底部的状态栏
3.  选择下拉箭头查看所有项目参与者。

*GitLens 撰稿人*

### 成千上万的开发人员正在利用 GitLens 过多的视图选项，包括贡献者视图、责备注释等，来改善他们的团队协作。

无论您是如何连接到遥控器的，您都可以通过查看窗口底部的状态栏图标来确认您的项目实际上已连接。如果您的本地项目没有连接到远程存储库，状态栏会显示一个图标，看起来像一个带向上箭头的云。如果连接了遥控器，您将会看到一个类似刷新圆圈的图标。

1.  用 VS 代码从 Git 中的远程 Repo 提取更改
2.  提取更改是开发人员用来将本地项目与远程项目中包含的任何更新同步的过程。如果你在一个项目中有多个参与者，定期提取变更是一个好主意，这样你可以看到其他人在做什么，并了解项目进展的最新情况。
3.  您可以很容易地从 VS 代码侧栏上的 3 点省略号中提取更改。首先，在 source control 视图中单击省略号，然后从下拉菜单中选择`Pull`。

成千上万的开发人员正在利用 GitLens 过多的视图选项，包括贡献者视图、责备注释等，来改善他们的团队协作。

一旦连接了远程 repo，您就可以开始通过一个称为 pushing 的过程将您对本地项目所做的更改发送到远程。推送通常有助于让您的远程 repo 与您最近的更改保持同步，并使其他项目参与者能够处理这些更改。

要从集成终端获取更改，运行`git pull`。

您可以很容易地从 VS 代码侧栏上的 3 点省略号中提取更改。首先，在 source control 视图中单击省略号，然后从下拉菜单中选择`Pull`。

准备…设置…贡献 Git 和 VS 代码！

现在，您拥有了在 VS 代码中遵循基本 Git 工作流的工具！了解如何使用 VS 代码来驱动 Git 可以帮助您开始更有效地与团队成员协作，并使您更容易开始为开源项目做出有意义的贡献。Git 可能令人望而生畏，但是 VS 代码，尤其是当与 [GitLens](https://www.gitkraken.com/gitlens) 结合使用时，为您提供了必要的工具、强大的功能和直观的 UX 来引导您走向成功。

利用 GitLens 确定项目参与者及其代码

GitLens 可以通过添加 contributors 视图来帮助您识别项目的所有参与者。这在处理大型项目时会很有帮助，因为它允许您通过简单地点击他们的个人资料来查看所有的合作者以及他们对项目的贡献。

要添加这个视图，只需导航到 VS 代码命令面板，搜索`GitLens: Show Contributors View`，并从下拉菜单中选择它。在下面描述的 VS 代码中，源代码管理图标下会出现一个新选项:

### 选择下拉箭头查看所有项目参与者。

*GitLens 撰稿人*

成千上万的开发人员正在利用 GitLens 过多的视图选项，包括贡献者视图、责备注释等，来改善他们的团队协作。

用 VS 代码从 Git 中的远程 Repo 提取更改

提取更改是开发人员用来将本地项目与远程项目中包含的任何更新同步的过程。如果你在一个项目中有多个参与者，定期提取变更是一个好主意，这样你可以看到其他人在做什么，并了解项目进展的最新情况。

您可以很容易地从 VS 代码侧栏上的 3 点省略号中提取更改。首先，在 source control 视图中单击省略号，然后从下拉菜单中选择`Pull`。

*GitLens Contributors*

Thousands of developers are leveraging GitLens’ plethora of view options, including the contributors view, blame annotations, and more to improve their team collaboration.

[Install GitLens Free!](https://www.gitkraken.com/gitlens/try-free)

要从集成终端获取更改，运行`git pull`。

## Pulling Changes from a Remote Repo in Git with VS Code

You can easily pull changes from the 3 dot ellipsis on the VS Code Side Bar. First, click the ellipses from the source control view, and then select `Pull` from the dropdown menu. 

准备…设置…贡献 Git 和 VS 代码！

现在，您拥有了在 VS 代码中遵循基本 Git 工作流的工具！了解如何使用 VS 代码来驱动 Git 可以帮助您开始更有效地与团队成员协作，并使您更容易开始为开源项目做出有意义的贡献。Git 可能令人望而生畏，但是 VS 代码，尤其是当与 [GitLens](https://www.gitkraken.com/gitlens) 结合使用时，为您提供了必要的工具、强大的功能和直观的 UX 来引导您走向成功。

To pull changes from the integrated terminal, run `git pull`.

## Ready…Set…Contribute with Git and VS Code! 

You now have the tools to follow a basic Git workflow in VS Code! Knowing how to use VS Code to drive Git can help you start collaborating with your team members more effectively, and makes it easy to start making meaningful contributions to open source projects. Git can be daunting, but VS Code, especially when combined with [GitLens](https://www.gitkraken.com/gitlens), provides you with the necessary tools, powerful functionality, and intuitive UX to guide you towards success.