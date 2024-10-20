# Gitflow:轻松发布管理工作流

> 原文：<https://www.gitkraken.com/blog/gitflow>

*文章更新于 2020 年 9 月*

在 [Syncfusion](https://www.syncfusion.com/) ，我们从 2001 年开始为软件开发者开发控件和框架。随着时间的推移，随着我们产品线的扩展——从我们的第一个网格控件发展到跨各种平台的 800 多个不同的控件——我们的发布管理过程变得越来越复杂。幸运的是，Git 的出现让一切都变得简单了。

Git 是一个开源的分布式版本控制系统，它灵活易用，适合各种团队，无论团队规模有多大。为了在日常开发中采用 Git，Vincent Driessen 引入了一个名为 [Gitflow](http://nvie.com/posts/a-successful-git-branching-model/) 的模型来帮助简化开发和发布管理。本文假设您对 Git 及其基本术语有一定的了解。它旨在进一步描述 Vincent Driessen 的分支模型，以及他的 [Gitflow 扩展](https://github.com/nvie/gitflow)如何在企业的发布管理工作流中发挥作用。

你也可以看看这个视频，它解释了 Gitflow 如何在跨平台 [GitKraken 客户端](https://www.gitkraken.com/git-client)中工作。

[https://www.youtube.com/embed/eTOgjQ9o4vQ?feature=oembed](https://www.youtube.com/embed/eTOgjQ9o4vQ?feature=oembed)

视频

无论您的团队依赖于哪种分支策略，GitKraken Client 都提供了卓越的 repo 可视化，因此您可以完全了解您的分支结构和提交历史。

工作流模型

## Gitflow 利用了 Git 的核心特性，即[分支的力量](https://www.gitkraken.com/learn/git/branch)。在这个模型中，存储库有两个核心分支:

**Master/Main**—这是一个高度稳定的分支，它总是生产就绪，并且包含生产中源代码的最后发布版本。(出于本文的目的，我们将这个分支称为“main”)。

1.  **开发**—从主分支中派生出来，开发分支作为一个分支，用于集成为即将到来的发布计划的不同特性。这个分支可能像主枝一样稳定，也可能不稳定。这是开发人员协作和合并功能分支的地方。
2.  **开发**—从主分支中派生出来，开发分支作为一个分支，用于集成为即将到来的发布计划的不同特性。这个分支可能像主枝一样稳定，也可能不稳定。这是开发人员协作和合并功能分支的地方。

***注*** *:前两个分支是任何项目的起点。它们非常重要，应该防止意外删除，直到项目被更好地定义。只有被授权的领导或者项目所有者应该被赋予将来自其他分支的变更合并到开发或者主分支的责任——比如我们将在后面讨论的特性分支。*

除了这两个主要分支，工作流中还有其他分支:

**特征**—这来自开发分支，用于开发特征。

1.  **Release**—这也来自开发分支，但是在发布期间使用。
2.  **hot fix**—它来自于主分支，用于修复产品分支中在发布后发现的 bug。
3.  **hot fix**—它来自于主分支，用于修复产品分支中在发布后发现的 bug。

*图表作者:Vincent Driessen | [原创博文](https://nvie.com/posts/a-successful-git-branching-model/) |许可:Creative Commons BY-SA*

*图表作者:Vincent Driessen | [原创博文](https://nvie.com/posts/a-successful-git-branching-model/) |许可:Creative Commons BY-SA*

我们将详细讨论这些分支以及用于简化这些分支管理的 Gitflow 扩展。我们用于 Gitflow 扩展的命令是基于 Windows 环境的，但是其他平台也有类似的命令。您可以查看 [Gitflow wiki](https://github.com/nvie/gitflow/wiki/Command-Line-Arguments) 以获得关于支持命令的完整细节。

安装 Gitflow 扩展

尽管 GitHub 也为不同的平台提供了[安装说明，但以下简化的步骤将帮助您在 Windows 上运行。](https://github.com/nvie/gitflow/wiki/Installation)

## 下载并安装 [Git for Windows](https://git-scm.com/download/win) 。默认情况下，它将安装在这个目录:`C:\Program Files\Git`。

接下来，您需要检索三个文件:来自 [util-linux 包](http://gnuwin32.sourceforge.net/packages/util-linux-ng.htm)、`libintl3.dll`的`exe`，以及来自依赖包 [libintl](http://gnuwin32.sourceforge.net/packages/libintl.htm) 和 [libiconv](http://gnuwin32.sourceforge.net/packages/libiconv.htm) 的`libiconv2.dll`。(为了便于安装，我们在这里收集并上传了所有这些文件[。)](http://www.syncfusion.com/downloads/support/directtrac/general/ze/dependencies142480694.zip)

1.  一旦有了这三个文件，将它们复制并粘贴到系统中 Git 的安装位置(即在`C:\Program Files\Git\bin`的 bin 文件夹中)。
2.  接下来，克隆或下载这个库:[https://github.com/nvie/gitflow](https://github.com/nvie/gitflow)。
3.  完成后，导航到名为“contrib”(`gitflow-develop\contrib`)**的文件夹。**

4.  在管理模式下打开该目录中的命令提示符，并键入以下命令:`msysgit-install.cmd "C:\Program Files\Git"`。
5.  完成后，导航到名为“contrib”(`gitflow-develop\contrib`)**的文件夹。**
6.  Gitflow 将被安装和配置到您的系统中，并随时可以使用。您可以通过在命令提示符下键入`git flow help`来测试它。

***注意*** *:在接下来的讨论中，我们将使用一个* [*示例 GitHub 库*](https://github.com/bharatdwarkani/gitflow-example) *和一个 Gitflow 扩展来演示工作流中的分支。*

7.  Gitflow 将被安装和配置到您的系统中，并随时可以使用。您可以通过在命令提示符下键入`git flow help`来测试它。

在存储库中设置 Gitflow

当开始一个项目时，你不会有任何代码文件。没问题，只需创建一个带有空目录的 Git 存储库。完成后，您可以在系统中克隆您的存储库。在这个例子中，我们使用了一个示例 GitHub 存储库，但是这个过程适用于任何 Git 存储库。

## 使用 Windows 命令提示符克隆系统中的分支:

切换到以下目录:

1.  初始化此存储库中的 Gitflow:

```
git clone https://github.com/bharatdwarkani/gitflow-example.git
```

2.  您将收到一条消息，说明不存在分支，并提示您使用通用分支名称。如果不想更改默认分支名称，请按 enter 键继续。完成后，您将在存储库中初始化一个 Gitflow 扩展。

    ***注意*** *:每个开发人员都必须为他们在系统中克隆的任何存储库完成这个过程。它不限于新的存储库；它也可以用于现有的存储库。*

```
cd gitflow-example
```

3.  主分支和发展分支

```
git flow init
```

主分支和开发分支构成了 Git 中任何存储库的基础。主分支包含生产中的代码版本，而开发分支包含即将发布的版本。在命令提示符下执行以下命令，检查系统中的主分支。

将提示您输入用户名和密码；输入它们以继续。接下来，您需要将 develop 分支推送到远程存储库(即从您的系统与 GitHub 同步)。因为 develop 分支只在您的本地机器上，所以必须通过执行以下命令将它移动到远程存储库中。

## 现在您已经有了一个包含从本地机器上复制的 main 和 develop 分支的存储库。只有当您第一次从头开始一个项目时，才需要这样做；否则，您可以使用以下分支之一。

特征分支

```
git checkout main
git push -u origin main
```

特征分支从开发分支分离，并在特征完成后合并回开发分支。这个分支的常规命名以`feature/*`开始。这个分支主要由与团队合作的开发人员创建和使用。特性分支的目的是在一个项目中开发一个特性的小模块。

```
git checkout develop
git push origin develop
```

您可能想知道为什么开发人员不能直接从开发分支工作。为什么他们需要分支到一个功能分支？为了解释这一点，考虑这样一个场景:您正在开发一个特性，但是管理层决定放弃这个特性，因为它不再需要或者实现它的可行性降低了。

那时候，如果你直接在开发分支工作，会产生很多冲突，可能会破坏现有的代码。此外，为此，您需要手动删除或注释掉代码。

## 相反，如果您从一个单独的特性分支中分支出来，您可以悄悄地丢弃和删除那个分支，而不会影响开发分支。这不仅有助于开发需要反复试验技术的特性，而且通过使用单独的分支，您还可以在开发分支中获得额外的稳定性，因为来自特性分支的代码在合并到开发分支之前要经过几个级别的代码审查和质量评估。

一旦特性分支与开发分支合并，它的生命周期就结束了。如果多个开发人员或团队在同一个特性上工作，他们通过在一个共同的特性分支上工作更容易协作。

以下步骤显示了如何在 Windows 命令提示符下使用 Gitflow 扩展创建和发布功能分支。

要[克隆一个存储库](https://www.gitkraken.com/learn/git/git-clone):

启动功能分支(功能分支的名称将是功能的名称；我们在这个例子中使用的是`feature1` )。

执行之后，`feature1`分支被创建，但是它只存在于您的系统中，在远程 GitHub 存储库中不可用。现在您可以继续您的开发，添加文件和修改代码。当您完成这个特性时，您可以将它提交到您的本地系统，稍后再将它推送到远程存储库。

完成后，可以检查新添加或修改的文件的更改状态。

1.  以下命令将特征发布到远程存储库。

```
git clone https://github.com/bharatdwarkani/gitflow-example.git
cd gitflow-example
git checkout develop
git flow init
```

2.  如果您签入远程存储库，将会创建一个名为`feature/feature1`的分支。

```
git flow feature start feature1
```

***注意*** *:在命令提示符下，你使用的分支名称是`feature1`，但是 Gitflow 会自动添加一个命名前缀(`feature/branch` **)** 作为约定。在 Git 命令中指定分支名称时，需要使用完整的分支名称(`feature/feature1`)，但是在 Gitflow 命令中不需要指定通用前缀(`feature/`)。*

3.  一旦一个特性完成并且代码已经被检查，您可以通过发出下面的命令来完成您在一个分支中的工作。在执行时，代码将自动合并到开发分支，而特性分支将从远程存储库中删除。

```
git status
git add .
git commit -am "Your message"
```

4.  如果需要[删除一个分支](https://www.gitkraken.com/learn/git/problems/delete-local-git-branch)，可以执行:`git branch -d feature/feature1`

```
git flow publish feature1
git push 
```

如果您签入远程存储库，将会创建一个名为`feature/feature1`的分支。

***注意*** *:如果多个开发人员正在协作开发一个特性，他们需要按照前面的步骤进行克隆，但有一点需要注意:其中一个开发人员必须创建并发布一个特性分支，这个分支可能是空的，这样其他人就可以协同工作。如果一个新的开发人员需要工作，他或她可以通过修改下面的命令来遵循相同的过程。*

5.  **`git flow feature track feature1`** 代替 **`git flow feature start feature1`**

```
git flow finish feature1
```

6.  除此之外，还有一个分支叫做`bugfix` **。**它有一个类似于`feature`分支的工作流程，但是它是用来修复一个 bug 的。

发布分支

发布分支源自开发分支，并在发布完成后合并回开发和主分支。按照惯例，这个分支的命名以`release/*`开头。当版本化版本的功能完成并最终确定时，将创建并使用此分支。

为什么不能直接从 develop 分支发布？因为发布分支的唯一目的是从即将到来的版本中分离出一个最终的但是需要一些质量支持和稳定性的版本。如果我们从发布分支中分支出来，那么被指派为即将到来的发布开发特性并且不参与发布稳定性过程的其他开发人员可以继续开发，并且将他们的特性合并到开发分支中，而不需要等待或者影响当前的发布过程。发布分支有助于隔离即将到来的版本和当前版本的开发。

当项目的特定版本发布时，发布分支的生命周期就结束了。一旦这个分支合并到 develop 和 main 分支中，它就可以被删除。一旦你完成了这些，你就可以用一个特定的发布版本来标记一个主分支——比如说`v1.0`—来创建一个历史里程碑。

以下示例解释了如何在命令提示符下使用 Gitflow 扩展创建和发布发布分支。

## 启动一个发布分支。

提交新添加或修改的更改，并推送到远程存储库。

将更改合并到开发分支。

发布后，将变更合并到主分支。

-或者-

1.  Start a release branch.

```
git checkout develop
git pull
git flow release start release1
```

2.  修补程序分支

```
git add .
git commit -am "Your message"
git flow publish release1
git push
```

3.  修补程序分支是从主分支派生出来的，在完成后合并回开发和主分支。按照惯例，这个分支的名称以`hotfix/*`开头。这个分支是在产品的特定版本发布后创建和使用的，为产品版本提供关键的错误修复。

```
git checkout develop
git merge release/release1
```

4.  我们这样做的原因是，当从开发分支分支出来时，您可能面临的一个问题是，当您处于当前版本的中间时，您的一些开发人员可能已经开始为即将到来的版本工作了。您的版本将包含下一版本的特性，这些特性还没有最终确定，但是您只需要提供当前版本的错误修复。您可以从主分支中分支出来，而不是从开发分支中分支出来，因为该分支只包含生产中代码的当前版本。这样，从主分支分出的分支不会影响产品的生产或开发版本。

```
git checkout release/release1
git pull
git flow release finish release1
```

一旦当前生产版本的关键修补程序发布并与主分支和开发分支合并，就可以删除修补程序分支。一旦你完成了这些，你就可以再次用发布的迭代版本来标记主分支；先说`v1.1`。

```
git flow release finish -m "Your message" "release1"
git checkout main
git push --all origin
```

这个例子展示了如何在命令提示符下使用 Gitflow 扩展创建和发布`hotfix1`分支。

## 启动新的热修复分支，并在修改后提交更改。

将分支发布到远程存储库。

`git flow publish hotfix1` 将更改合并到远程存储库。

-或者-

***注意*** *:所有以 **git flow** 开头的命令都是基于 git flow 扩展的。实际的 Git 命令中没有流关键字。它们只以“ **git** 开头。*

1.  启动新的热修复分支，并在修改后提交更改。

```
git checkout develop
git flow hotfix start hotfix1
git status
git add .
git commit -am "Your message"
```

2.  使用 GitKraken 客户端简化 Gitflow
3.  通过使用 Gitflow 扩展，Gitflow 模型有助于更好地管理和组织发布。感谢 Vincent Driessen 提出 Gitflow 并提供一个扩展来帮助简化企业级版本的管理工作流。

```
git flow hotfix finish hotfix1 
```

如果您选择完全接受 Gitflow order 的租户，您可以将它们与传说中的跨平台 [GitKraken 客户端、](https://www.gitkraken.com/git-client)一起使用，该客户端旨在帮助将 Git 转化为人类术语，以便开发人员可以更有信心地进行编码。

```
git flow hotfix finish -m "Your message" "hotfix1"
git status
git checkout main
git push --all origin
```

掌握 Git 的全部功能需要时间，所以你可能会从类似于《Git 简洁地》 和《GitHub 简洁地》 的书籍中受益。还可以考虑查看 GitKraken 的全面的 Git 教程视频库[，包括初级、中级和高级 Git 概念。](https://www.gitkraken.com/learn/git/tutorials)

## GitKraken 客户端已经发展到可以应对新的挑战，它具有 GitKraken CLI 等高级功能，这是一种 Git 增强的终端体验，具有 Git 命令的自动完成和自动建议功能。

通过使用 Gitflow 扩展，Gitflow 模型有助于更好地管理和组织发布。感谢 Vincent Driessen 提出 Gitflow 并提供一个扩展来帮助简化企业级版本的管理工作流。

如果您选择完全接受 Gitflow order 的租户，您可以将它们与传说中的跨平台 [GitKraken 客户端、](https://www.gitkraken.com/git-client)一起使用，该客户端旨在帮助将 Git 转化为人类术语，以便开发人员可以更有信心地进行编码。

掌握 Git 的全部功能需要时间，所以你可能会从类似于《Git 简洁地》 和《GitHub 简洁地》 的书籍中受益。还可以考虑查看 GitKraken 的全面的 Git 教程视频库[，包括初级、中级和高级 Git 概念。](https://www.gitkraken.com/learn/git/tutorials)

GitKraken 客户端已经发展到可以应对新的挑战，它具有 GitKraken CLI 等高级功能，这是一种 Git 增强的终端体验，具有 Git 命令的自动完成和自动建议功能。