# 你如何签出一个提交？Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/git-checkout-commit>

每当您想要检查特定行为何时被引入到代码中时，检查提交可能是完美的解决方案。

在向您展示如何在命令行中执行操作之前，我们将介绍如何使用跨平台 [GitKraken 客户端](https://www.gitkraken.com/git-client)来执行 Git checkout 提交。

使用 GitKraken 客户端，只需点击两次即可完成结帐。

## 如何用 GitKraken 检查提交？

在 GitKraken 中检查提交既快又简单。只需右键单击中间图形中的提交，并从上下文菜单中选择`Checkout this commit`。

是的，就是这么简单。

用 GitKraken 检查提交时不需要提交散列。💥

如何在命令行中检查提交？

要检验 Git 提交，您将需要提交散列。如果您使用的是终端，那么您无法直接看到您的提交信息。要调出提交及其相关散列的列表，可以运行`git log`命令。

用 GitKraken 检查提交时不需要提交散列。💥

## 如何在命令行中检查提交？

要检验以前的提交，您将使用 Git checkout 命令，后跟从 Git 日志中检索到的提交散列。

Psst:我们知道你正在做一些重要的事情…你可能只是想完成你的行动，然后继续前进…但是[把这个页面加入书签，稍后](https://www.gitkraken.com/download)看看 GitKraken 客户端终端中令人难以置信的自动建议和自动完成功能。

```
git checkout <commit hash>
```

使用适用于 Windows、Mac 和 Linux 的 [GitKraken Client](https://www.gitkraken.com/git-client) 使检查提交的过程变得更加容易。可视化上下文允许您只需点击两次就可以签出提交，而不用担心散列。

如何用 GitLens 检查提交？

如果你在 VS Code 上安装了 GitLens，打开一个 repo，然后点击活动栏上的源代码控制图标。

展开提交视图，右键单击任何提交以访问 **`Switch to Commit`** 操作。

## 如何用 GitLens 检查提交？

这将有效地在分离的 HEAD 状态下检验提交，类似于上面的 GitKraken 客户端和 CLI 选项。

或者，有一个有用的 GitLens 特性，可以帮助您在任何时候浏览作为虚拟文件夹的存储库。右键单击提交视图中的任何提交，但是这次单击`**Browse**`子菜单。

这将允许您从指定的时间点打开一个新的 VS 代码窗口，而无需正式签出分支或提交。

如果你想亲自体验一下这个功能，请进入 [VS 网页代码](https://vscode.dev/)，安装 GitLens 试试看！

GitLens 可以帮助您保持组织性，并跟踪您的任务和项目。免费解锁 VS 代码中 Git 的全部功能！

这将允许您从指定的时间点打开一个新的 VS 代码窗口，而无需正式签出分支或提交。

如果你想自己*检验*这个特性，跳到 [VS 代码网站](https://vscode.dev/)并安装 GitLens 来试试吧！

GitLens 可以帮助您保持组织性，并跟踪您的任务和项目。免费解锁 VS 代码中 Git 的全部功能！

[Install GitLens for VS Code](vscode:extension/eamodio.gitlens)