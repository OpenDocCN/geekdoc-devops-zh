# Git Pull 与 Fetch 的区别| Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/git-pull-vs-fetch>

您可能处于以下情况:最近对您的远程存储库进行了更改，并且您想要将它们合并到您的本地副本中。这里有几个选项:从远程获取更改的两个最常见的操作是 Git pull 和 Git fetch。

那么，Git pull 和 fetch 之间有什么区别，什么时候应该使用哪个命令？我们很高兴你问了。

什么是 Git fetch？

### Git fetch 是一个允许您从另一个存储库下载对象的命令。

什么是 Git pull？

Git pull 是一个命令，允许您从另一个存储库或本地分支获取数据并与之集成。

### 从这个定义中，您可以看到 Git pull 实际上是一个 Git fetch，后面跟着一个附加的操作—通常是 Git merge。

什么时候应该使用 Git pull？

当您对将从远程存储库获得并添加到本地副本的更改有完整的上下文时，Git pull 是一个理想的操作。

什么时候应该使用 Git fetch？

### 如果您只想查看远程存储库中的所有当前分支和变更，Git fetch 可以获取您需要的所有信息，而无需对您的工作进行任何本地更改。

这使您有时间决定合并更改的最佳行动方案，例如将它们合并到您的本地分支机构中或快进。

比较 Git 拉取和提取

### 当比较 Git pull 和 fetch 时，Git fetch 是一个更安全的选择，因为它从远程获取所有提交，但不对本地文件做任何更改。

另一方面，Git pull 的速度更快，因为你可以一次执行多个操作——物有所值。使用 Git pull 命令可以被看作是一种方便的特性；您可能不太担心在本地回购中引入冲突，您只想从您所在的远程分支机构获得最新的变更。

Git pull 是一个更高级的操作，重要的是要理解您将引入变更并立即将它们应用到您当前检出的分支。

Git fetch 有点不同；您可以使用 Git fetch 命令查看遥控器的所有更改，而无需应用它们。如果您是 Git 新手，这个动作可能会很棒，因为它提供了关于引入的更改的更多可见性。另一方面，fetch 也可能是 Git 老手的首选，他们只是希望对他们的回购中发生的事情有更多的控制。

### 现在我们已经了解了各个命令的作用，并比较了 Git pull 和 fetch，让我们学习如何使用跨平台 GitKraken Git GUI 来可视化您的存储库，以及如何在 CLI 中执行这些操作。

GitKraken 让您可以看到从远程存储库中提取或取出的所有变更的细节。

另一方面，Git pull 的速度更快，因为你可以一次执行多个操作，这样更划算。使用 Git pull 命令可以被看作是一种方便的特性；您可能不太担心在本地回购中引入冲突，您只想从您所在的远程分支机构获得最新的变更。

你如何使用 GitKraken 来获取数据？

在 GitKraken 中，你可以很容易地从顶部工具栏中获取或拉出。

***GitTip:了解 GitKraken 中默认的 [Git pull/Git fetch 操作。](https://support.gitkraken.com/working-with-repositories/pushing-and-pulling/#fetching)***

或者，您可以在 GitKraken 的中心图中右键单击一个远程分支来获取或提取。

GitTip:需要复习一下如何[拉一个远程 Git 分支](https://www.gitkraken.com/learn/git/problems/pull-remote-git-branch)？我们会掩护你的。

### 但是如果你像许多开发者一样喜欢使用键盘，GitKraken 为你提供了超级方便的模糊查找器，你可以用快捷键`Cmd/Ctrl` + `P`打开它。

打开模糊查找器后，您可以简单地键入`fetch`开始获取 Git，或者键入`pull`开始获取 Git。

***GitTip:了解 GitKraken 中默认的 [Git pull/Git fetch 操作。](https://support.gitkraken.com/working-with-repositories/pushing-and-pulling/#fetching)***

用 GitKraken 自动获取更改

GitKraken 包含一个方便的功能，允许您根据定义的时间间隔自动从远程存储库中获取更改。您可以将间隔时间设置为每分钟、每小时或介于 0 和 60 之间的指定分钟。

这对于让您的本地副本与您的远程副本保持同步非常有帮助，并且当您有许多正在进行的多个存储库和项目时尤其有益。

通过 GitKraken 中的自动获取功能，让您的本地副本与遥控器中的更改保持同步，节省时间。

### 如何在命令行中获取数据？

如果您使用终端，您将使用 Git fetch 命令从远程分支获取更改。

这对于让您的本地副本与您的远程副本保持同步非常有帮助，并且当您有许多正在进行的多个存储库和项目时尤其有益。

你如何获得命令行？

类似地，您将使用 Git pull 命令从您的远程设备中提取更改。

### 这将使用来自远程的更改更新当前签出的分支。

在 GitKraken 中，只需点击一下鼠标，就可以获取你的更改，GitKraken 连续四年被选为最受欢迎的 Git 客户端。今天就免费旋转一圈；你不会后悔的。

```
git fetch
```

### 你如何获得命令行？

类似地，您将使用 Git pull 命令从您的远程设备中提取更改。

```
git pull
```

这将使用来自远程的更改更新当前签出的分支。

在 GitKraken 中，只需点击一下鼠标，就可以获取你的更改，GitKraken 连续四年被选为最受欢迎的 Git 客户端。今天就免费旋转一圈；你不会后悔的。