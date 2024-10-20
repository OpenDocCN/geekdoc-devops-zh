# GitKraken CLI 预览| GitKraken 命令行功能

> 原文：<https://www.gitkraken.com/gitkon/gitkraken-cli-preview>

[https://www.youtube.com/embed/WgLSyk-iSgc?feature=oembed](https://www.youtube.com/embed/WgLSyk-iSgc?feature=oembed)

视频

用于 Windows、Mac、& Linux 的传奇式跨平台 [GitKraken 客户端](https://www.gitkraken.com/git-client)提供的高级功能之一是 GitKraken CLI，这是一种开箱即用的终端优先体验，具有 Git 命令的自动完成和自动建议功能，以及您的存储库的可视化。

## **一些 GitKraken 客户的历史**

在深入 GitKraken CLI 本身之前，让我们先来看一下 GitKraken Client 作为 Git 生产力工具的发展历史。在团队开始开发 GitKraken 客户端之前，整个团队在使用 Git 时效率低下。没有多少人是在命令行上使用 Git 的专家，大多数人不得不记住或保留最常见的 Git 命令的手写列表。当时，大多数人只是简单地循环这些命令，直到他们遇到问题，然后不得不去问他们团队或在线的 Git 专家。高级 Git 用户经常被打断，以帮助新开发人员进行设置，解决困难的[合并冲突](https://www.gitkraken.com/learn/git/tutorials/how-to-resolve-merge-conflict-in-git)，或者指导某人完成复杂的 [Git 基础改造](https://www.gitkraken.com/learn/git/git-rebase)。

使用 GitKraken 的 learn Git 资源，包括初学者、中级和高级概念的教程和指南，减轻加入 Git 的痛苦。

[Learn Git with GitKraken](/learn/git)

GitKraken 团队注意到，仅仅让 Git 在某些操作系统上工作就浪费了大量时间。虽然市场上有一些工具在某种程度上有所帮助，但没有任何高质量的、真正跨平台的 Git 客户端能够在 Windows、Mac 和 Linux 上正常、一致地工作。大多数现有的接口都不直观，所以 GitKraken 开发人员做了任何明智的开发团队都会做的事情:我们决定构建自己的 Git 客户端。

构建 GitKraken 客户端早期的指导原则很简单:“构建一个不烂的 Git 客户端。”这是一个相当低的门槛，但我们希望制作一个 Git 工具，它易于设置，对于 Git 新手来说很直观，对于 Git 专家来说足够强大，同时看起来非常好。GitKraken 团队主要依靠吸引人的设计和令人惊叹的用户体验来吸引用户使用 GitKraken 客户端，同时致力于与竞争对手的功能对等。正是这种对清晰可视化 g it 的持续关注，并通过直观的 UI 使其更易访问，帮助 GitKraken Client 成为当今世界上最受欢迎的 Git 客户端。

GitKraken Client 是 Git 工具的完美范例，它已经发展到可以应对新的挑战，具有交互式拉请求管理、合并冲突检测和解决、深度链接等高级功能。

## **进入 GitKraken CLI**

专注于为 Git 制作最好的 GUI 或图形用户界面，导致我们有时采取某种反 CLI 的立场。虽然 GUI 相对于命令行界面的优势对某些人来说似乎是显而易见的，但是有相当多的开发人员已经学会并习惯于每天只使用 CLI。在我们帮助所有开发人员更有效地使用 Git 的持续努力中，我们试图更好地理解为什么开发人员使用 Git CLI，希望解决与在命令行上使用 Git 相关的最常见的痛点。GitKraken CLI 是大量研究和采访开发人员的结果，这些开发人员更喜欢使用命令行而不是 GUI 来使用 Git。

在内部测试和私有 alpha 测试之后，GitKraken CLI 在 GitKraken 客户端 8.0 版本中作为预览版发布。

**端子标签**

### 虽然终端并没有什么新东西，但是 GitKraken 客户端中的终端标签提供了一种全新的方式来在命令行中使用 Git 存储库。这些选项卡旨在与现有的 shells 一起工作，因此您可以继续使用您习惯的插件和别名，同时利用 GitKraken CLI 中内置的一些真正独特而强大的 Git 增强功能。

While there is nothing new about terminals, the terminal tabs in GitKraken Client provides an entirely new way to work in a command line with your Git repositories. The tabs are designed to work with existing shells, so you can continue to use the plug-ins and aliases you’re used to while taking advantage of some truly unique and powerful Git enhancements built into the GitKraken CLI.

GitKraken CLI 是多年来为 Git 构建强大 GUI 的经验的结晶，结合了从采访在日常 Git 交互中严重依赖 CLI 的用户中获得的知识。其结果是终端优先体验，这将从根本上改善 Git 在命令行上的使用方式。

The GitKraken CLI is a culmination of years of experience building a powerful GUI for Git combined with knowledge gained from interviewing users who depend heavily on the CLI in their daily Git interactions. The result is a terminal first experience that will radically improve the way Git is used on the command line.

**自动完成&Git 命令的自动建议**

## GitKraken CLI 提供的增强功能之一是 Git 命令的自动完成和自动建议。当 GitKraken 团队问用户在命令行上使用 Git 最困扰他们的是什么时，最常见的回答是记住 Git 命令和大量参数。

GitKraken CLI 提供了一个内置系统，可以在您键入时自动建议 Git 命令和标志。它甚至在每个命令旁边都有一个简短的描述，以防您需要回忆这些命令是做什么的。

The GitKraken CLI offers a built-in system that automatically suggests Git commands and flags as you type. It even includes a brief description right next to each command, just in case you need to jog your memory on what those commands do.

如果您正在编写需要提交 SHA、分支名称或文件名的命令，GitKraken CLI 将使用[模糊逻辑搜索](https://en.wikipedia.org/wiki/Approximate_string_matching)来帮助您轻松找到它们，即使您不记得确切的路径，也可以将它们插入到您的命令中。长期以来，自动完成建议一直是对 IDEs 的重要增强，它们在 GitKraken CLI 中也同样自然和强大。

If you’re writing a command that requires a commit SHA, branch name, or file name, the GitKraken CLI will use a [fuzzy logic search](https://en.wikipedia.org/wiki/Approximate_string_matching) to help you find them easily, even if you can’t remember the exact path, by inserting them into your command. Auto-complete suggestions have been an essential enhancement to IDEs for a long time, and they feel just as natural and powerful in the GitKraken CLI. 

GitKraken 客户端中提供的易于阅读的提交图将帮助所有技能水平的 Git 用户更好地可视化和理解他们的存储库的分支结构和提交历史。

The easy-to-read commit graph offered in GitKraken Client will help Git users of all skill levels better visualize and understand the branch structure and commit history of their repositories.

**可视化 Git 仓库**

## 关于在命令行中使用 Git，最困扰人们的第二件事是 [Git 存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)的不良可视化。这一点很有意义，因为 CLI 确实擅长显示文本，但不擅长其他；当然不会可视化分支结构和提交历史。

许多 GitKraken 客户端用户也是大量 Git CLI 用户，他们告诉我们，他们使用 GitKraken 客户端只是因为提交图是可视化存储库的最佳方式。

在 GitKraken CLI 中，我们从标准 GitKraken 客户端界面中提取了最有用的可视化功能，并通过命令行轻松访问它们。这结合了 CLI 用户在终端中期望的能力和速度，以及关于存储库的可视化信息，使得通过 GUI 使用 Git 更容易，所有这些都在一个最小的视图中。

In GitKraken CLI, we’ve taken the most helpful visualization features from the standard GitKraken Client interface and made them easily accessible through the command line. This combines the power and speed that CLI users expect in a terminal, with the visual information about repositories that make working with Git easier via a GUI, all in one minimal view.

当您第一次导航到 Git 存储库时，这些可视化效果就会出现。GitKraken 客户端将立即在终端底部的面板中打开提交图，提供一个快速的存储库可视化表示，在上面的终端中执行 Git 命令时可以引用该表示。

提交图是完全交互式的，如果您需要的话，可以继续使用拖放和上下文菜单选项。在图表的上方，将会显示存储库名称、分支、标签以及任何前后的更改，这样您就可以随时知道自己的确切位置。

These visualizations appear the first time you navigate into a Git repository. GitKraken Client will immediately open the commit graph in a panel at the bottom of the terminal, providing a quick visual representation of the repository that can be referenced while performing Git commands in the terminal above.

The commit graph is fully interactive and continues to function with drag-and-drop and context menu options if you need them.  Just above the graph, the repository name, branch, tag, and any changes ahead or behind will be displayed so you always know exactly where you are.

GitKraken CLI 命令`gk`命令

## 要控制这个新的可视化面板，您可以通过键入`gk`和以下参数之一来利用 GitKraken CLI 的一组命令:

`gk graph`打开或关闭图形

*   添加`top`、`bottom`、`left`或`right`改变位置。例如，`gk graph right`将视图设置为分屏
    *   `gk diff`快速查看提交之间发生了什么变化

*   `gk history`显示文件历史视图
*   显示文件或提交中发生了什么变化，以及是谁做出了这些变化
*   `gk blame` shows what has changed in a file or commit and who made those changes

**命令调色板功率**

## 针对 GitKraken CLI 用户的 ProTip:使用 GitKraken 客户端的命令面板，可通过`Ctrl/Cmd+P`访问，或点击工具栏中的新魔棒图标，快速访问所有高级功能。例如，要在编辑器中打开一个文件，只需键入`edit`后跟文件名，该文件将在终端中以编辑模式打开。

ProTip for GitKraken CLI users: use GitKraken Client’s Command Palette, accessible using `Ctrl/Cmd+P`, or click the new magic wand icon in the toolbar, for quick access to all advanced features. For example, to open a file in the editor, just type `edit` followed by the name of the file, and that file will open in edit mode inside of your terminal.

我们才刚刚开始！

## 在 [GitKraken CLI](https://www.gitkraken.com/cli) 的未来版本中，还会有更多的特性出现。GitKraken 团队期待着听到用户对 GitKraken 客户端和 GitKraken CLI 的反馈，这样我们就可以继续完善我们传奇的 Git 工具，并在命令行上提供最佳体验。你可以在 Mac、Windows 和 Linux 上免费下载并开始使用传说中的 [GitKraken 客户端](https://www.gitkraken.com/git-client)，现在有了 GitKraken CLI！

There are even more features to come in the future releases of the [GitKraken CLI](https://www.gitkraken.com/cli).  The GitKraken team is looking forward to hearing feedback about GitKraken Client and the GitKraken CLI from users so we can continue to refine our legendary Git tools and provide the best experience on the command line.  You can download and start using the legendary [GitKraken Client](https://www.gitkraken.com/git-client) today for free on Mac, Windows, and Linux, now with GitKraken CLI!