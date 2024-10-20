# 去克隆区

> 原文：<https://www.gitkraken.com/learn/git/problems/git-clone-branch>

在开始使用 Git 中现有的项目存储库之前，首先需要在您的机器上创建项目的本地副本。这就是 [Git 克隆](https://www.gitkraken.com/learn/git/git-clone)命令的用武之地。

Git 克隆动作将一个现有的 [远程 Git 库](https://www.gitkraken.com/learn/git/tutorials/what-is-git-remote)的副本下载到您的机器上，包括所有的分支。默认情况下，Git 将在新的本地存储库中检出原始 repo 的主分支。

***GitTip:获得关于 [如何结帐远程 Git 分支](https://www.gitkraken.com/learn/git/problems/git-checkout-remote-branch)的分步教程。***

为什么要 Git 克隆一个分支？假设您想要立即开始某个特定分支的工作，那么您想要克隆远程并同时签出该分支。或者也许您不需要克隆整个存储库，因为您只负责一个分支，所以您只想要 Git 克隆分支。

输入 Git 克隆分支。

Git 克隆分支命令提供了两个主要功能:

1.  克隆整个远程存储库，然后签出并设置特定分支的上游。
2.  仅克隆指定的分支，然后检出并设置该分支的上游。

在这个 Git 克隆分支的例子中，我们将回顾如何使用跨平台 [GitKraken Git 客户端](https://www.gitkraken.com/git-client)来克隆 Git 分支，然后在 CLI 中完成整个过程。

## 如何使用 GitKraken 克隆一个分支？

与命令行不同，GitKraken Git GUI 提供了令人难以置信的可视性，因此您可以轻松地管理您的遥控器，并确切地看到克隆过程中发生了什么。

## 如何使用 GitKraken 克隆一个存储库并检出一个特定的分支？

首先打开一个新标签(`+`)并选择 UI 左侧的`Clone a repo`选项。

也可以使用键盘快捷键:`Ctrl/Cmd` + `O`，选择`Clone`。然后，您将选择您的集成远程托管服务:GitHub、GitLab、Bitbucket 或 Azure DevOps。

在这个 Git 克隆分支示例中，我们将对远程 GitHub 存储库使用`Clone with URL`选项。

在开始之前，您需要仔细检查以下字段:

1.  您希望克隆到的目录
2.  新本地目录的名称

如果一切正常，您将点击`Clone the repo!`按钮。

成功克隆存储库之后，您只需在左侧面板中双击您想要签出的分支。

因为 CLI 没有您需要的可见性，您浪费了多少时间来克隆错误的分支？GitKraken 会照亮道路。

## 如何在命令行中克隆一个分支？

在使用 can Git 克隆分支之前，您需要两条信息:

1.  托管远程存储库的 URL
2.  要克隆和签出的分支的名称

在这个 Git 克隆分支示例中，我们将使用一个公共的 GitHub 存储库，并将克隆 Git 分支:`Development`。

## 如何在命令行中克隆一个库并检出一个特定的分支？

首先，您将导航到您希望克隆遥控器的本地父目录，在本例中为:`/projects`。

如果需要创建一个新的本地目录，可以使用:

```
mkdir <directory-name>
```

接下来，您将使用 Git clone branch 命令:

```
git clone --branch <branch-name> <repository-url>
```

(`-b`也代替这里的`--branch`工作)

如果执行正确，Git 将获取所有分支，并立即检出指定的分支。

通过我们在 GitHub 上的交互式实践库，直接掌握 Git 中克隆的诀窍。

[Practice Cloning](https://github.com/Axosoft/moby-git)

## 如何在命令行中克隆一个特定的分支？

如果您只希望 Git 克隆一个分支，而不是远程存储库的其他内容，那么您将再次导航到您希望克隆远程存储库的本地父目录。

接下来，您将键入:

```
git clone --branch <branch-name> --single-branch <repository-url>
```

这将只从远程获取特定的分支。

因为 CLI 缺乏 GitKraken 所提供的可见性，所以您不能轻易地确认您是否克隆了预期的 Git 分支。

如果您希望验证您的 Git 克隆分支操作是否成功获取了目标分支，您可以运行:

```
git remote show origin
```

如果您想节省时间，在克隆一个 repo 后就开始处理分支，那么 Git clone branch 命令是一个非常有用的工具。另一方面，只克隆一个 Git 分支可以通过只从远程获取您需要的数据来帮助增强您的注意力。

GitKraken 主界面左侧的远程面板可以帮助您准确地看到您可以访问哪些远程分支，这样您就不必记住分支名称列表，从而节省您的时间和精力。现在使用 GitKraken Git 客户端简化常见操作，如克隆遥控器和分支。