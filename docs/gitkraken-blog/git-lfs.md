# 如何使用 Git LFS 大文件存储|学习 Git

> 原文：<https://www.gitkraken.com/learn/git/git-lfs>

Git LFS 代表大文件存储，是许多开发人员在处理二进制文件时用来节省空间的工具。

简而言之，Git LFS 是一个 Git 扩展，它允许用户通过将二进制文件存储在不同的位置来节省空间。

[https://www.youtube.com/embed/jXsvFfksvd0](https://www.youtube.com/embed/jXsvFfksvd0)

VIDEO

GitKraken 使得在现有的存储库上初始化 LFS 和指定想要跟踪的文件类型变得很容易。

## **Git LFS 的好处**

当处理包含音频、视频或图像文件(也称为二进制文件)的存储库时，对这些文件所做的任何更改都不会像跟踪非二进制文件那样通过 Git 进行跟踪。

虽然 Git 在跟踪文本文件中的更改集方面做得很好，但是对二进制文件的更改是作为文件的附加副本来跟踪的。

假设您正在处理一个占用 100MB 存储空间的二进制映像，并且您对该文件进行了更改。Git 将跟踪一个占用 100MB 空间的文件的变化，并将继续跟踪每一个变化。

如果您想做一些快速计算…对二进制 100MB 文件进行四次文件更改将导致总共使用 500 MB(100 MB 原始文件+ 100MB 更改文件+ 100MB 更改文件+ 100MB 更改文件+ 100MB 更改文件= 500MB)。

因为这些更改也会被推送到您的遥控器，所以您的遥控器也会变大，从而降低您克隆、推、拉或执行其他回购操作的速度。

## **使用 Git LFS 节省时间**

当使用 Git LFS 时，您的提交将指向一个轻量级引用对象，而不是指向二进制文件(您实际上是将原始二进制文件推送到一个 LFS repo)。

现在，当您克隆 LFS 回购协议或签出 LFS 回购协议中的一个分支时，您将只从 Git LFS 服务器获取您需要的二进制文件版本，从而节省空间和时间。

GitKraken 揭开了高级 Git 概念的神秘面纱，如 LFS，因此您的团队可以更有信心地使用 Git，即使是讨厌的二进制文件。

## **Git LFS 安装**

在 GitKraken 中使用 Git LFS 之前，你必须首先在你的机器上安装 [Git LFS](https://git-lfs.github.com/) 。您还需要设置您的 Git LFS 服务器。

如果你刚刚开始使用 Git LFS， [GitHub 有一个内置的免费选项](https://docs.github.com/en/github/managing-large-files/versioning-large-files/about-storage-and-bandwidth-usage)。

## **去 LFS 在 gitkraken】中的应用**

一旦你安装了 Git LFS，导航到 GitKraken 中的`Preferences`来访问 LFS 选项。

然后，您可以为现有的 repo 初始化 Git LFS，并指定需要应用程序跟踪的文件类型。

完成初始化过程后，工具栏中会出现一个`LFS`图标，允许您与 Git LFS 服务器上的文件进行交互。

## **查看 Git LFS 文件**

Git LFS 追踪的所有文件都会在 GitKraken 中心图的右面板用`LFS`图标标记。

当你点击查看这样一个文件的 [Git diff](https://www.gitkraken.com/learn/git/git-diff) 时，你会看到 SHA 引用而不是你的大文件。

最后，当您从 GitKraken 进行任何回购时，您可以选择初始化 Git LFS 应用程序。

我们希望这有助于解释使用 Git 大型文件服务器的好处，以及使用 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 有多容易。