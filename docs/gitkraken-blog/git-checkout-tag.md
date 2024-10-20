# 你如何检查一个 Git 标签？Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/git-checkout-tag>

在 Git 中，标签是指向特定时间点的引用，通常用于标识代码的发布版本。

在向您介绍如何在 CLI 中签出一个标记之前，我们将讨论如何使用跨平台 GitKraken Git 客户端来签出一个 Git 标记。

### 如何用 GitKraken 结账？

在 GitKraken 中，您可以立即看到存储库中的所有标签，它们清楚地列在主 UI 的左侧面板中。你也可以从这个位置过滤和搜索标签，这对于大的转发来说特别方便。

要在 GitKraken 中签出一个标签，只需在中间的图中右键单击一个标签，这里的标签用🏷标签图标。从这里，您可以选择`Checkout this commit`以分离的头部状态检出标签。

使用 GitKraken，只需点击 2 次，即可将标签签出为提交或分支。

### 如何使用 GitKraken 将标签签出为分支？

要将 Git 标签作为 GitKraken 中的一个分支签出，右键单击左侧面板或中央图中的标签，并从上下文菜单中选择`Create branch here`。

使用 GitKraken 更自信地快速识别和检查标签。

### 如何在终端中检查 Git 标签？

如果你使用命令行，你将看不到你的标签列表整齐地排列在你的用户界面的左边，就像你在 GitKraken 看到的那样。要查看 Git 存储库中存在哪些标签，可以运行`git tag`命令来查看 Git 标签的列表。

从这里，您可以在分离的 head 状态或作为分支签出标签。让我们从如何在分离的 head 状态下签出 Git 标签开始。这可以通过以下方式实现:

```
git checkout <tag name>
```

### 如何在终端中将 Git 标签签出为分支？

或者，您可以使用以下代码将 Git 标记作为分支签出:

```
git checkout -b <branch name> <tag name>
```

使用传说中的 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 检查标签是无缝和直观的，并让您对您的工作流程有更多的控制。[今天免费下载](https://www.gitkraken.com/download)。