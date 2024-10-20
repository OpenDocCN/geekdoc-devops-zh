# 使用 Git 回到过去

> 原文：<https://www.gitkraken.com/blog/review-file-history-git>

## **时间**行程

你有没有发现自己在想这样的事情:*‘几个月前我的项目看起来怎么样？’去年春天我的网站看起来怎么样？*

有时，审查代码的过去版本可能是推进项目的最佳方式。谢天谢地，Git 使这成为可能，GitKraken 使这变得容易。

跳上我们的 Git 时间机器，我们一起回到过去！

确认你的目的地

## 首先，你要问自己:*‘我需要回溯到多远？’。*然后，记住你的日期范围，在 GitKraken 中打开你的回购协议，浏览你的图表，寻找最接近目标日期的承诺。

浏览你的图表，寻找最接近目标日期的承诺。

浏览你的图表，寻找最接近目标日期的承诺。

如果您不确定在哪里找到日期，请选择一个提交并查看右侧的日期戳。

*选择提交以查看日期戳。*

如果你正在处理一个有超过 2000 个提交的糟糕的回购协议，你可能需要[单独或者隐藏分支](https://support.gitkraken.com/working-with-repositories/hiding-and-soloing)来回到更远的时间。

*单飞或隐藏树枝。*

信任，但要核实

一旦你选择了一个提交，GitKraken 将在右边的面板中显示修改过的文件列表。然而，如果您点击`View all files`选项，您将获得整个项目的树形视图。

## *查看所有文件。*

单击一个文件，打开左侧的 diff。这应该能帮你找到你要找的东西。🎉

*打开文件 diff。*

开始工作

如果您只需要复制和粘贴代码，从树视图中查看文件的差异可能就足够了。然而，假设你需要运行一个本地版本的网站，或者你希望从这个提交开始构建。这就是创建新分支的关键所在。

右键单击提交，并选择`Create branch here`。

## *在此创建分支。*

现在您可以签出该分支，您的回购将会改变，以匹配文件在您返回的时间点的状态。

其他时间旅行选项

啊，是的，模糊发现者。如果你在 Mac 上使用`Command + P`或者在 Windows 和 Linux 上使用`Ctrl + P`来打开模糊查找器，你可以输入“History”，然后在 GitKraken 中搜索文件名。

使用模糊查找器搜索您的文件历史记录。

选择该文件，您将跳转到该文件的整个历史。`File View`选项可用于快速复制/粘贴您需要的内容。

## *选择文件视图。*

这怎么可能呢？！

Git 跟踪您的项目变更，当您在历史中分支一个提交时，您是在告诉 Git 应用变更的特定子集。这使得浏览项目历史变得容易，尤其是在 GitKraken 中。

选择该文件，您将跳转到该文件的整个历史。`File View`选项可用于快速复制/粘贴您需要的内容。

*选择文件视图。*

这怎么可能呢？！

## Git 跟踪您的项目变更，当您在历史中分支一个提交时，您是在告诉 Git 应用变更的特定子集。这使得浏览项目历史变得容易，尤其是在 GitKraken 中。

Git tracks your project changes, and when you branch off a commit in your history, you are telling Git to apply a specific subset of changes. This makes it easy to travel through your project’s history, especially with GitKraken.