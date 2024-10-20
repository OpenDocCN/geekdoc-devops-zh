# 11 个 GitLens 技巧|了解如何在 VS 代码中使用 GitLens

> 原文：<https://www.gitkraken.com/blog/gitlens-tips>

GitLens 是 VS 代码的头号 Git 扩展，帮助超过 1400 万开发人员在他们最喜欢的编辑器中释放 Git 的力量！以下是 11 条专家建议，告诉你如何充分利用 GitLens！

[https://www.youtube.com/embed/9Gr-tp9ucOk?feature=oembed](https://www.youtube.com/embed/9Gr-tp9ucOk?feature=oembed)

视频

立即使用 VS 代码优化您的 Git 体验——安装 GitLens，享受更好的项目历史可视化和洞察力！

[Install GitLens Free!](https://www.gitkraken.com/gitlens/try-free)

## 1.定制您的 GitLens 体验

GitLens 可定制性极强。虽然您可以定制许多 VS 代码扩展，但 GitLens 为您提供了非常细粒度的控制，以微调用户体验。

打开 GitLens 侧边栏菜单的首页部分，在`Get Started`下寻找`Interactive settings editor`，就可以打开交互设置编辑器。您也可以通过搜索`GitLens: Open Settings`从命令面板打开交互式设置编辑器。

这将打开一个新视图，让您通过单击复选框来打开或关闭所有功能。您还可以通过同一视图上的设置子菜单来更改几乎任何功能的行为。

## 2.GitLens 责备多处注释

GitLens 最出名的可能是通过突出显示逐行责备注释来暴露您的 Git 历史。但是您知道吗，将鼠标悬停在注释上可以提供一个更丰富的视图，其中包含可点击的链接，允许您与 Git 引用、 [pull request](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git) 进行交互，或者发布并发现有关该提交的更多信息。我们鼓励您亲自尝试。

你还可以在窗口下方的 VS 代码状态栏看到 GitLens 的怪注释。将鼠标悬停在此部分还会显示更多信息和链接。

## 3.3 GitLens 文件注释

GitLens 为您提供了三种不同的方式来帮助您了解项目中任何文件的历史。在 VS 代码中打开任何文件时，您可以在右上角找到一个 GitLens 图标，它将允许您切换文件注释。点击该图标将显示三个不同的选项:

*   切换文件责备
*   切换文件热图
*   切换文件更改

文件错误显示最后修改文件每一行的提交者和作者。这使您可以快速了解文件是如何达到其当前状态的。

文件热图显示了相对于文件中的所有其他更改，行最近的更改情况。蓝色表示有一段时间没有变化，红色表示最近有变化。

文件更改突出显示任何本地的、未发布的更改或由最近提交更改的行。

文件更改突出显示任何本地的、未发布的更改或由最近提交更改的行。

4.按住 Alt 键并单击打开对任何先前修订的更改

## GitLens 允许用户通过右上角的`Open Changes with Revisions`菜单选项快速比较版本。单击指向左侧的小箭头图标←将一次向后导航一个版本，向您显示该提交与当前版本之间的所有更改。

如果你想查看许多次提交前的修订，你可以一次后退一个修订，或者你可以通过“alt-click”跳回到任何修订。当你在 Mac 上按住`alt`键/ `option`键，然后点击向左的箭头，你会看到一个修改该文件的所有提交的列表。现在你可以简单地选择哪一个来比较你的作品。

5.重新排列侧边栏视图

VS 代码是一个非常可定制的应用程序，GitLens 建立在这种可定制性之上。如果你想改变 GitLens 侧边栏的顺序，你可以通过点击并拖动它到你想要的位置来移动它。

你甚至可以在其他侧边栏菜单之间拖动侧边栏项目。例如，如果你想获取 *Stashes* 侧边栏项目，并将其放入*源代码控制*侧边栏，你可以轻松地做到这一点。您甚至可以将项目拖放到主边栏菜单中，以便更快地访问您最常用的工具。

## 5.重新排列侧边栏视图

6.有许多键盘快捷键

GitLens 自带了很多开箱即用的键盘快捷键。一种发现可用快捷方式完整列表的简单方法是打开`Interactive settings editor`并找到`Keyboard Shortcuts`。如果你想节省一些滚动，在设置页面的右侧`Jump To`菜单中有一个快捷方式。

这一段 GitLens 设置会给你几个选项，并给你提供了在 VS 代码下搜索 GitLens 的选项“快捷方式”视图。搜索“GitLens”将会弹出许多现有的键盘快捷键，以及所有可以分配给快捷键的命令。没错，你可以为 GitLens 特性创建自己的键盘快捷键！

## 6.有许多键盘快捷键

7.持续搜索并在重新启动之间比较结果

您可以快速搜索和比较您的作品的任何先前版本与任何其他版本，包括您当前的标题。这和搜索提交散列一样简单，

[Git 提交消息](https://www.gitkraken.com/learn/git/best-practices/git-commit-message)，作者，文件，具体变更，或者标签。这将显示修订前后的所有提交，让您更好地了解项目是如何随时间变化的。

如果你要经常搜索或比较某些修订，你可以点击搜索结果上的图钉图标，将结果保存在 GitLens 侧边栏菜单中。结果将保持不变，即使你重启 VS 代码(或者你的整个机器！).

您可以快速搜索和比较您的作品的任何先前版本与任何其他版本，包括您当前的标题。这和搜索提交散列一样简单，

8.GitLens 和 Git 命令的命令面板

GitLens 用户可能已经知道可以使用 VS 代码命令面板来快速访问任何 GitLens 特性。GitLens 还为常见的 [Git 命令](https://www.gitkraken.com/learn/git/commands)提供了命令调色板帮助！

用一个`ctl` / `cmd+shift+P`打开命令面板，输入`GitLens: Git`，VS 代码会提示 Git 命令，比如 [Git branch](https://www.gitkraken.com/learn/git/branch) ，reset，merge 和 [Git rebase](https://www.gitkraken.com/learn/git/git-rebase) ，等等。选择其中一个自动完成选项将引导您完成完成 Git 操作所需的步骤。

GitLens 用户可能已经知道可以使用 VS 代码命令面板来快速访问任何 GitLens 特性。GitLens 还为常见的 [Git 命令](https://www.gitkraken.com/learn/git/commands)提供了命令调色板帮助！

9.GitLens 使集成的 VS 代码终端具有交互性

问任何一个开发人员他们为什么喜欢 VS 代码，许多人会告诉你他们喜欢布局和集成的终端。在您处理代码时，让终端在您的指尖是一个不可或缺的工具。

GitLens 使您对 VS 代码终端的体验更好，它将终端中显示的所有 Git refs 变成了可点击的链接，这些链接提供了一系列选项，用于如何与那个 [Git commit](https://www.gitkraken.com/learn/git/commit) 、branch 或 tag 进行交互！

10.自动链接的问题

## 使用 GitLens 中的搜索和比较功能时，通常可以在侧边栏菜单的底部找到，您会发现一个名为“自动链接的问题和拉式请求”的部分。这一部分揭示了所有的 [GitHub pull 请求](https://www.gitkraken.com/learn/git/problems/github-pull-requests)和与修订相关的问题，让您更好地了解不同版本之间解决了哪些功能和缺陷。

问任何一个开发人员他们为什么喜欢 VS 代码，许多人会告诉你他们喜欢布局和集成的终端。在您处理代码时，让终端在您的指尖是一个不可或缺的工具。

11.可视化文件历史显示了更改的范围

[GitLens+](https://www.gitkraken.com/gitlens/plus-features) 用户可以访问可视化的文件历史，让你看到一个文件的演变。这显示了何时由谁进行了更改，还显示了这些更改的大小，较大的提交在网格视图中显示为较大的条目。

要访问该视图，要么打开集成终端面板，点击该视图顶部的`GitLens`，要么从命令面板中搜索:`GitLens: Visual History Timeline`。

## 使用 gittens 做更多事情

这些只是几个例子，展示了如何将 GitLens 用于 VS 代码，以及如何使用 Git 提高效率。随着您继续使用这个流行的 Git 扩展，您会发现有许多方法可以在 VS 代码中释放 Git 的真正力量。我们希望听到您如何在工作中使用 GitLens，并看到您最喜欢的提示和技巧。请在社交媒体上为我们呐喊，并提及 [@GitLens](https://twitter.com/gitlens) 以及您的有用提示！

当然，如果你还没有使用 GitLens，那么在你的工作流程中利用这些令人敬畏的技巧的第一步就是为 VS 代码安装 GitLens。它是免费使用的[你现在就可以开始使用](https://www.gitkraken.com/gitlens/try-free)！

## 11.可视化文件历史显示了更改的范围

[GitLens+](https://www.gitkraken.com/gitlens/plus-features) 用户可以访问可视化的文件历史，让你看到一个文件的演变。这显示了何时由谁进行了更改，还显示了这些更改的大小，较大的提交在网格视图中显示为较大的条目。

要访问该视图，要么打开集成终端面板，点击该视图顶部的`GitLens`，要么从命令面板中搜索:`GitLens: Visual History Timeline`。

使用 gittens 做更多事情

这些只是几个例子，展示了如何将 GitLens 用于 VS 代码，以及如何使用 Git 提高效率。随着您继续使用这个流行的 Git 扩展，您会发现有许多方法可以在 VS 代码中释放 Git 的真正力量。我们希望听到您如何在工作中使用 GitLens，并看到您最喜欢的提示和技巧。请在社交媒体上为我们呐喊，并提及 [@GitLens](https://twitter.com/gitlens) 以及您的有用提示！

当然，如果你还没有使用 GitLens，那么在你的工作流程中利用这些令人敬畏的技巧的第一步就是为 VS 代码安装 GitLens。它是免费使用的[你现在就可以开始使用](https://www.gitkraken.com/gitlens/try-free)！

## Do More with GitLens

These are just a few examples of how to use GitLens for VS Code and become more productive using Git. As you continue to use this popular Git extension, you will find there are numerous ways to unleash the true power of Git in VS Code. We would love to hear how you use GitLens in your work and see your favorite tips and tricks. Please give us a shout on social media and mention [@GitLens](https://twitter.com/gitlens) with your helpful hints!

Of course, if you’re not already using GitLens, the first step to leveraging these awesome tips in your workflow is to install GitLens for VS Code.  It’s free to use and [you can get started right now](https://www.gitkraken.com/gitlens/try-free)!