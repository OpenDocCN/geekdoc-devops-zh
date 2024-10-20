# VS 代码下载|入门和设置

> 原文：<https://www.gitkraken.com/blog/vs-code-download>

Sarah 是苏格兰的一名 IT 专业人士。她对 IT 运营充满热情，并乐于帮助他人学习新技能。你可以在[www.techielass.com](http://www.techielass.com/)找到她写的更多内容

* * *

Visual Studio Code (VS Code)是微软开发的一款流行的开源代码编辑器。它为开发人员或操作工程师提供了一个轻量级但功能强大的编码和调试环境。

VS 代码包括语法突出显示、代码完成、调试、Git 集成以及更多特性。它还支持各种扩展和插件，使用户能够定制自己的工作流程并提高工作效率。

VS Code 是我最喜欢的代码编辑器，我每天都在用。这是一个多功能的工具，可以帮助我编辑 markdown 文档，编写 Azure Bicep 代码，甚至帮助我[连接到 SQL 数据库](https://www.techielass.com/connect-to-a-sql-database-with-visual-studio-code/)。

本文将介绍如何在您的机器上安装 VS 代码，解释接口，并分享如何定制接口以适应您的工作流的技巧。我还将介绍扩展的概念以及为什么它们是必不可少的。最后，我将分享一些初学者技巧来帮助你开始使用 VS 代码。

下载用于 Windows 的 VS 代码

## 在 Windows 机器上安装 VS 代码有几种方法。您可以使用以下过程下载安装文件:

下载用于 Windows 的 [Visual Studio 代码安装程序](https://go.microsoft.com/fwlink/?LinkID=534107)。

1.  下载完成后，双击安装程序开始安装。这应该只需要一分钟。
2.  下载完成后，双击安装程序开始安装。这应该只需要一分钟。

或者，如果您使用 [Windows 包管理器](https://www.techielass.com/how-to-install-winget-windows-package-manager/)，您可以打开一个命令提示符窗口并键入命令:

`winget install Microsoft.VisualStudioCode -y`

或者，如果选择了 [Chocolatey](https://chocolatey.org/) 作为您的软件包管理器，您可以打开一个命令提示符窗口，键入以下命令:

`choco install vscode -y`

下载针对 Linux 的 VS 代码

根据您使用的 Linux 的风格，有不同的下载 VS 代码的方法。对于 Ubuntu，您可以遵循以下步骤:

## 打开`Software Catalogue`

搜索`Visual Studio Code`

1.  点击`code`
2.  然后点击`Install`；可能会提示您输入密码以继续安装
3.  点击`code`
4.  然后点击`Install`；可能会提示您输入密码以继续安装

查看官方文档,了解如何在其他版本的 Linux 上下载 VS 代码。

为 Mac 下载 VS 代码

要在 Mac 上安装 VS 代码，请遵循以下说明:

[下载 macOS 的 Visual Studio 代码](https://go.microsoft.com/fwlink/?LinkID=534106)。

## 打开浏览器的下载列表，找到下载的应用程序。

在某些浏览器中使用双击，或者在 Safari 中选择“放大镜”图标。

1.  将 Visual Studio Code.app 拖到 Applications 文件夹，使其在 macOS Launchpad 中可用。
2.  双击图标，从应用程序文件夹打开 VS 代码。
3.  将 VS 代码添加到你的 Dock 中，点击 Dock 中的图标上的`right-clicking`，调出上下文菜单，然后选择`Options`和`Keep in Dock`。
4.  在 VS 代码中进行设置
5.  一旦你下载了 VS 代码并安装在你的机器上，你就可以开始使用它并定制它了！让我们带你浏览一下 VS 代码的界面。
6.  将 VS 代码添加到你的 Dock 中，点击 Dock 中的图标上的`right-clicking`，调出上下文菜单，然后选择`Options`和`Keep in Dock`。

## 菜单系统在你屏幕的顶部，就像微软的其他产品一样。菜单系统将为您提供设置，如屏幕上显示的字体大小以及 VS Code 使用的配色方案。

编辑器部分占据了屏幕的最大部分。这是您处理文件的地方；您打开了许多文件，并在比较模式下并排编辑它们，或者将它们作为许多不同的选项卡。

在屏幕的左侧，有一个活动栏。这允许您在视图之间切换，并为您提供额外的上下文指示器，例如有多少对 Git 的更改正在等待登台。

边栏将在您处理项目时为您提供不同的视图；我最喜欢的是浏览器，它可以让你看到你的项目中的文件和文件夹。

屏幕底部是状态栏。它给你关于项目的信息，你正在编辑的文件，一些插件在这里也有一个状态指示器。

您可以在 VS 代码中显示不同的面板。这可用于显示调试面板或集成终端。默认情况下，面板位于屏幕的下半部分，但是如果愿意，您可以将其移动到垂直位置。

自定义 VS 代码设置

您可以在 VS 代码中定制许多设置，以使编辑器适合您的首选工作流。

改变主题

我喜欢配置的第一个设置是配色方案。当使用 VS 代码时，我更喜欢 light 主题，但是有许多内置的选择。如果点击`File> Preferences > Settings`，导航至`Workbench > Appearance`。你会看到配置`Color Theme`的选项。

在我们最近的博客文章中查看一些[最佳 VS 代码主题](https://www.gitkraken.com/blog/best-vs-code-themes)。

## 打开自动保存

我的自定义列表中的下一个设置是打开自动保存，这将在一定的延迟后保存您的更改。要配置自动保存，请转至`File> Preferences > Settings`，然后导航至`Text Editor > Files`。您将看到自动保存选项；把那个转到`afterDelay`。您还可以配置自动保存延迟时间；默认的 1000 毫秒适合我。

### Changing the Theme

设置浏览器窗口的缩进

我经常设置的另一个设置是树或浏览器窗口的缩进。更改缩进会使文件和文件夹的缩进更明显，并且更容易一目了然。你可以去`File > Preferences > Settings`和`Workbench > Appearance`更改。向下滚动，你会看到一个名为`Tree: Indent`的设置。我喜欢设置为`24`。

安装 VS 代码扩展

### VS 代码提供了各种扩展来增强它的功能。这些扩展对于希望改进工作流和流程的用户来说至关重要。扩展可以添加默认设置中不可用的功能和工具。

例如，扩展可以突出各种编程语言的语法、代码完成、调试支持以及与其他开发工具的集成。

在 VS 代码中使用扩展可以帮助用户节省时间，并通过自动化重复任务和提供对常用功能的快速访问来提高他们的生产力。

扩展可以在 [VS 代码扩展市场](https://marketplace.visualstudio.com/vscode)中找到。市场中的扩展由供应商或社区成员创建。有大量的扩展可供选择，很容易找到满足您特定需求的扩展。

### 您可以按照这些简单的步骤在 VS 代码中安装一个扩展:

打开 VS 代码，点击屏幕左侧的`Extensions` 图标

搜索要安装的扩展

找到想要安装的扩展后，点击`Install`

## 您可能需要关闭并重新启动 VS 代码来激活一些扩展

VS 代码提供了各种扩展来增强它的功能。这些扩展对于希望改进工作流和流程的用户来说至关重要。扩展可以添加默认设置中不可用的功能和工具。

在 VS 代码中使用扩展可以帮助用户节省时间，并通过自动化重复任务和提供对常用功能的快速访问来提高他们的生产力。

查看我们的[最佳 VS 代码扩展列表](https://www.gitkraken.com/blog/best-vs-code-extensions)！

您可以按照这些简单的步骤在 VS 代码中安装一个扩展:

1.  使用 VS 代码的初学者技巧
2.  搜索要安装的扩展
3.  如何在 VS 代码中创建新文件
4.  要在 VS 代码中创建新文件，可以转到`File> New File`。然后你可以给文件一个名字，设置文件扩展名时间，并通过`File> Save`保存。

使用键盘快捷键

默认情况下，VS 代码内置了键盘快捷键，可以节省您的时间并提高您的生产率。

您可以前往`File> Preferences > Keyboard Shortcuts`访问可用键盘快捷键列表。或者你可以使用快捷键`Ctrl+K Ctrl+S` (Windows/Linux)或者`Cmd+K Cmd+S` (Mac)来拉列表。

另一位 GitKraken 大使 Njong Emy 写了一篇精彩的文章，涵盖了 VS Code 中值得学习的[二十个捷径。](https://www.gitkraken.com/blog/vs-code-shortcuts)

在 VS 代码中调试代码

## 您还可以使用 VS 代码调查并修复代码中的错误。

要开始调试，首先通过单击行号左侧要停止执行的位置，确保代码在所需位置设置了断点。

### 然后，点击`Run`菜单并选择`Start Debugging`或使用键盘快捷键`F5`。

调试控制台将出现在屏幕底部，执行将在断点处暂停。从那里，您可以使用调试工具栏遍历代码，查看变量和调用堆栈，以及添加或移除断点。

您还可以设置条件断点，仅在满足特定条件时暂停执行。一旦发现并修复了错误，点击`Run`菜单并选择`Stop Debugging`退出调试模式。通过使用 VS 代码中的调试工具，您可以更快更有效地找到并修复代码中的错误。

### 使用键盘快捷键

在 VS 代码中使用多个文件

在 VS 代码中处理多个文件既简单又直观。

如果您在 VS 代码中打开了多个文件，您可以通过点击编辑器顶部的标签在它们之间切换。您也可以使用键盘快捷键`Ctrl+Tab` (Windows/Linux)或`Cmd+Tab` (Mac)来循环浏览打开的文件。

此外，您可以通过点击编辑器右上角的`Split Editor`图标或使用键盘快捷键`Ctrl+` (Windows/Linux)或`Cmd+` (Mac)，将编辑器分成多个窗格。这对于并排比较两个文件非常有用。

### 立即开始使用 VS 代码

我们已经介绍了 VS 代码的安装，分享了如何根据您的喜好设置界面的技巧，介绍了扩展的概念，并分享了一些初学者技巧。

为您的工作流定制和使用 VS 代码会极大地影响您的生产力和效率。您可以将它用于各种编程语言和框架，VS 代码社区也在不断更新和改进它。那么，为什么不尝试一下，看看它如何改善您的编码体验呢？通过一些定制，您可以将 VS 代码转换成工作流的最终工具。

然后，点击`Run`菜单并选择`Start Debugging`或使用键盘快捷键`F5`。

调试控制台将出现在屏幕底部，执行将在断点处暂停。从那里，您可以使用调试工具栏遍历代码，查看变量和调用堆栈，以及添加或移除断点。

您还可以设置条件断点，仅在满足特定条件时暂停执行。一旦发现并修复了错误，点击`Run`菜单并选择`Stop Debugging`退出调试模式。通过使用 VS 代码中的调试工具，您可以更快更有效地找到并修复代码中的错误。

### 在 VS 代码中使用多个文件

在 VS 代码中处理多个文件既简单又直观。

如果您在 VS 代码中打开了多个文件，您可以通过点击编辑器顶部的标签在它们之间切换。您也可以使用键盘快捷键`Ctrl+Tab` (Windows/Linux)或`Cmd+Tab` (Mac)来循环浏览打开的文件。

此外，您可以通过点击编辑器右上角的`Split Editor`图标或使用键盘快捷键`Ctrl+` (Windows/Linux)或`Cmd+` (Mac)，将编辑器分成多个窗格。这对于并排比较两个文件非常有用。

立即开始使用 VS 代码

## 我们已经介绍了 VS 代码的安装，分享了如何根据您的喜好设置界面的技巧，介绍了扩展的概念，并分享了一些初学者技巧。

为您的工作流定制和使用 VS 代码会极大地影响您的生产力和效率。您可以将它用于各种编程语言和框架，VS 代码社区也在不断更新和改进它。那么，为什么不尝试一下，看看它如何改善您的编码体验呢？通过一些定制，您可以将 VS 代码转换成工作流的最终工具。

Customizing and using VS Code for your workflows can significantly impact your productivity and efficiency. You can use it for various programming languages and frameworks, and it’s constantly being updated and improved by the VS Code community. So why not try it and see how it can improve your coding experience? With some customization, you can transform VS Code into the ultimate tool for your workflow.