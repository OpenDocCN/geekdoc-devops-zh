# Git 补丁|了解如何 Git 应用补丁和 Git 创建补丁

> 原文：<https://www.gitkraken.com/learn/git/git-patch>

在 [Git pull requests](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git) 出现之前，开发人员创建了 Git 补丁来与团队成员和项目合作者共享他们的代码。Git 补丁是包含代码和 [Git 提交](https://www.gitkraken.com/learn/git/commit)元数据的文本文件。创建一个 Git 补丁，本质上就是复制并打包你的工作，然后发送给其他人。应用一个 Git 补丁需要将某人的工作添加到您的本地 [Git 库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)。

在本文中，我们将介绍如何使用 CLI 和 [GitKraken 客户端](https://www.gitkraken.com/git-client)创建和应用 Git 补丁。

使用内置的 CLI、命令面板或在提交图中点击几下，在 GitKraken 客户端中快速创建和应用 Git 补丁。

## **Git 补丁简史**

在 Git 的早期阶段，开发人员通过创建 Git 补丁并通过电子邮件发送给他们的同事来共享他们的代码。那时，Git 拉请求并不像今天这样存在，Git 补丁是共享代码的最佳选择。

虽然现在开发人员使用拉请求来共享代码肯定更普遍了，但是仍然有一些著名的项目使用 Git 补丁。Linux 内核、 [Drupal](https://www.drupal.org/) 和 [Git](https://git-scm.com/) 项目经常使用 Git 补丁。如今，使用 Git 补丁的项目通常会让项目维护人员能够在应用补丁之前全面检查每个 Git 补丁中的更改，这增加了另一层安全性。

## **Git 创建补丁**

要创建一个 Git 补丁，您可以运行一个不同的`git format-patch`命令，这取决于您是为单次提交创建补丁还是从 [Git 分支](https://www.gitkraken.com/learn/git/branch)创建补丁。一旦你的 Git 补丁被创建，你就可以通过电子邮件把它发送给你的团队成员，让他们应用到他们的代码库中

## **Git 应用补丁**

要应用 Git 补丁， [Git 检查您希望应用更改的提交](https://www.gitkraken.com/learn/git/problems/git-checkout-commit)或分支，然后在终端中运行以下命令:

`git apply <.patch file>`

虽然应用补丁可能比其他 Git 操作花费更多的时间，但是它的价值在于能够将其他人的工作添加到您的 repo 中，而不必自己编写代码。

## **Git 补丁格式**

`git format-patch`命令用于从终端创建 Git 补丁。该命令的基本语法如下:

`git format-patch <commit SHA or branch name>`

访问 Git 的官方网站，了解更多信息和 [Git 补丁格式命令](https://git-scm.com/docs/git-format-patch)的变体。

## **Git 在 CLI 中创建补丁**

在从 [CLI](https://www.gitkraken.com/cli) 创建 Git 补丁之前，您需要确定想要在补丁中包含什么文件。这将决定您将使用哪个`git format-patch`命令变体来创建 Git 补丁。

### **Git 在 CLI 中通过一次提交创建补丁**

要创建包含来自单个 Git 提交的信息的 Git 补丁，请执行以下步骤:

1.  在终端中键入`git log`并点击`Enter`以获得您想要为其创建补丁的提交的提交 SHA。
2.  一旦有了提交 SHA，运行:

`git format-patch -1 <commit SHA> -o <name of the directory you want the patch saved to>`

您选择的提交中包含的所有更改将被打包到一个 Git 补丁中，然后您可以将其发送给其他协作者。

### **Git 在 CLI 中从 Git 分支创建补丁**

通过执行下面概述的单个步骤，可以从 Git 分支创建 Git 补丁:

1.  在终端键入`git format-patch <branch name> -o <name of the directory you want the patch saved to>`并点击`Enter`

这将创建一个 Git 补丁，其中包含 Git 分支上保存的所有信息，包括提交、差异、新文件和任何其他更改。

## **如何在 CLI 中 Git 应用补丁**

要在终端中正确应用 Git 补丁，您需要执行以下步骤:

1.  [Git checkout](https://www.gitkraken.com/learn/git/git-checkout) 要应用补丁的相关提交或分支
2.  运行命令:`git apply <.patch file>`

补丁文件中包含的更改将会反映在您的本地存储库中。现在，您可以继续您的工作流，因为您知道您的本地存储库与其他项目协作者保持同步。

## **Git 用 GitKraken 客户端创建补丁**

使用 GitKraken 客户端创建 Git 补丁有几种简单直观的方法。与在 CLI 中创建 Git 修补程序类似，创建 Git 修补程序的方式取决于您想要创建修补程序的文件类型和数量。无论你如何创建你的 Git 补丁，GitKraken Client 都更容易，[现在就免费试用](https://www.gitkraken.com/git-client/try-free)。

### **Git 使用 GitKraken 客户端通过一次提交创建补丁**

如果您想使用 GitKraken 客户端创建并发送包含特定提交信息的 Git 补丁，请执行以下步骤:

1.  在中央提交图中，右键单击要从中创建补丁的提交
2.  从下拉菜单中选择`Create patch from commit`
3.  GitKraken 客户端将提示您将补丁保存到计算机上的指定位置

### **Git 使用 GitKraken 客户端通过多次提交创建补丁**

要创建包含来自多次提交的信息的 Git 补丁，请执行以下步骤:

1.  按住`Shift`或`Cmd`并选择您想要创建补丁的提交
2.  右键单击其中一个选定的提交，并从下拉菜单中选择`Create patch from commits`
3.  GitKraken 客户端将提示您将补丁保存到计算机上的指定位置

### **Git 用 GitKraken 客户端从一个未提交的文件创建补丁**

要创建包含来自尚未提交的单个文件的信息的 Git 补丁，请执行以下步骤:

1.  在 GitKraken 客户端右侧的提交面板中，右键单击要从中创建补丁的未提交文件
2.  从下拉菜单中选择`Create patch from file changes`
3.  GitKraken 客户端将提示您将补丁保存到计算机上的指定位置

### **Git 用 GitKraken 客户端从多个未提交的文件创建补丁**

为了从尚未提交的多个文件中创建 Git 补丁，请执行以下步骤:

1.  从 GitKraken 客户端右侧的提交面板中，按住`Shift`或`Cmd`选择您想要创建补丁的未提交的更改
2.  右键单击其中一个未提交的更改，并从下拉菜单中选择`Create patch from changes in # files`
3.  GitKraken 客户端将提示您将补丁保存到计算机上的指定位置

### **Git 使用命令面板从所有工作目录变更中创建补丁**

按照下面列出的步骤，使用 GitKraken 客户端的命令面板从所有工作目录更改中创建 Git 补丁！

1.  通过选择魔棒图标🪄或使用键盘快捷键`Cmd +  Shift + P`访问命令调板
2.  在命令面板中键入`patch`
3.  从下拉菜单中选择`Create patch from all working directory changes`

创建和应用 Git 补丁并不是一个常见的工作流程，第一次尝试时可能会感到害怕。GitKraken Client 使得使用 Git 补丁变得简单、直观且没有痛苦。

## **如何使用 GitKraken 客户端应用补丁**

GitKraken 客户端要求通过命令面板应用补丁。要应用补丁程序，请执行以下操作:

1.  [Git checkout](https://www.gitkraken.com/learn/git/git-checkout) 要应用补丁的分支或提交
2.  通过选择魔棒图标🪄或使用键盘快捷键`Cmd +  Shift + P`进入命令调板
3.  在命令面板中键入`patch`
4.  选择`Apply patch`；这将打开您的文件浏览器
5.  在文件浏览器中，点击您想要应用的`.patch`文件，然后选择`Open`

Git 补丁将被应用到您已经签出的分支或提交，并且您可以继续您的工作流。

## **Git 去打补丁！**

恭喜你！现在您知道了如何使用 CLI 和 GitKraken 客户端创建和应用 Git 补丁。这些新获得的知识为您打开了一扇门，让您开始为使用这种独特的 Git 工作流的开源项目和私有存储库做出贡献。

知道如何自信地使用 Git 补丁的开发人员非常少，这对任何团队都非常有帮助。将您的 Git 补丁知识与 GitKraken Client 结合起来，使创建和应用 Git 补丁变得更快更容易！[今天免费下载 GitKraken 客户端](https://www.gitkraken.com/git-client/try-free)。