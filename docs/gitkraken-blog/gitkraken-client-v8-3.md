# GitKraken 客户端 8.3 版-现在苹果芯片用户的速度提高了 2 倍

> 原文：<https://www.gitkraken.com/blog/gitkraken-client-v8-3>

奥运速滑选手和开发者有什么共同点？他们需要…需要速度。😏

没有人喜欢慢节奏，不管你是在争夺金牌，还是只是想部署一个令人敬畏的新功能。GitKraken 团队一直在努力工作，以确保所有 GitKraken 客户端用户，尤其是 macOS 用户，在利用我们传奇的 Git 客户端与团队协作时，拥有尽可能最快的体验。准备好体验 GitKraken 客户端 8.3 版的速度提升吧！

[https://www.youtube.com/embed/Nh9aQ8RUyP4?feature=oembed](https://www.youtube.com/embed/Nh9aQ8RUyP4?feature=oembed)

视频

⚡️ Git 在 GitKraken Client v8.3 版本中引入了超高速、破纪录的速度，进入了快车道！⚡️

## **苹果芯片的原生支持**

GitKraken Client v8.3 引入了一个新的 ARM64 兼容版本，为苹果硅架构提供了原生支持，如在采用 M1 芯片的 MAC 中使用的那样。开发人员喜欢使用苹果硅技术的设备明显更快的性能和改进的资源管理；GitKraken 客户端现在充分利用这一创新的架构！

如果你使用的是以苹果芯片为核心的 Mac，你需要从我们的网站下载新版 GitKraken 客户端，替换掉你现有的客户端，但是不要担心，你所有的设置，包括配置文件和集成，都将保持不变。

### **本地支持 vs 仿真**

虽然 GitKraken 客户端一直很快，但最近苹果公司构建笔记本电脑和 Mac Minis 的方式发生了变化，这使得我们需要做出一些重大改变。到目前为止，为了保持与所有 Mac 产品的兼容性，GitKraken 客户端使用了一个[仿真层](https://en.wikipedia.org/wiki/Rosetta_(software))与苹果硅处理器进行交互。因为它需要在操作系统和应用层之间进行转换，所以仿真天生就很慢，不管你的速度有多快⛸.

借助新的 ARM64 兼容版本，GitKraken Client 可以利用该架构的共享内存池，并改善其高性能和高效率内核的线程管理。我们已经准备好在世界奥林匹克舞台上竞争了…如果他们把“版本控制”作为一项运动加入进来的话。

## **Mac Big Sur 更新**

如果你在基于 Intel 的机器上使用 macOS Big Sur edition 或更高版本，我们有一些好消息要告诉你。🎉您将不再需要运行自定义终端命令来解决 macOS Big Sur 中引入的部分签名[问题。以前，产生一个子进程可能需要几秒钟，但是现在默认情况下，只需要几毫秒。所有 macOS 用户一开始就将提高处理速度，使整个应用程序感觉更快。](https://www.gitkraken.com/blog/workaround-gitkraken-big-sur-issues)

## **为所有用户加速电子化**

无论您使用什么操作系统，您都会注意到应用程序速度和资源消耗方面的改进。这是因为 GitKraken 团队已经将 GitKraken 客户端的底层应用程序堆栈 Electron 更新到一个非常新的版本，带来了 Electron 社区最近添加的所有改进。

我们也打了补丁电子来满足我们的具体要求。这将使我们能够最好地支持跨各种平台的用户。例如， [Fedora 35](https://getfedora.org/en/workstation/download/) 用户在启动 GitKraken 客户端时将不再需要使用`--no-sandbox`选项。

虽然这不是一个小壮举，但我们致力于确保所有 GitKraken 客户端用户，无论是现在还是将来，都有可能获得最佳体验。有了这次对 electronic 的补丁更新，我们就能在未来的版本中更快地推出更多的高级功能。

## **GitHub Enterprise 和 GitLab 自主管理的 GitKraken 工作区**

GitKraken 知道开发是一项团队运动，有些团队需要企业级设备。GitKraken Workspaces 允许用户一次对多个存储库进行分组和操作，现在可用于 GitHub Enterprise 和 GitLab 自管理存储库。团队成员之间可以共享工作空间，这有助于让新团队成员快速高效地工作。从来没有比这更好的方式来获得你的回购高层次的看法！

要使用工作区，导航至工作区选项卡，并点击`+ New Workspace`按钮。这将打开一个新菜单，您可以从 [GitHub](https://www.gitkraken.com/integrations/github) 、GitHub Enterprise、 [Bitbucket](https://www.gitkraken.com/integrations/bitbucket) 、 [GitLab](https://www.gitkraken.com/integrations/gitlab) 和 GitLab 自管理主机源中选择存储库。

## **制作 GitKraken CLI G.O.A.T.**

### **新终端设置**

我们增加了两种 GitKraken CLI 用户自定义体验的新方式。

用户现在可以指定默认目录，如果新的终端标签实例不是从现有的回购或终端标签中打开的，例如当从回购管理标签中的`New CLI Tab`按钮或从命令面板启动时，新的终端标签实例将在该目录中打开。

用户还可以设置行高，确定打印到终端屏幕的每行之间的间距。

这两个新选项都可以在`Preferences`下的``Terminal``设置中找到。

### **改进了 Git 命令的自动完成和自动建议功能**

GitKraken 客户端用户不断告诉我们他们喜欢的特性之一是自动完成和自动建议 [Git 命令](https://www.gitkraken.com/learn/git/commands)。这让我们非常兴奋地推出这些改进，旨在让您的 CLI 工作流体验更好。

**去流**

#### 利用“git flow”进行工作的用户现在将会看到自动完成建议，以满足他们的用例，例如`git flow init`或`git flow feature start`，甚至`git flow release publish`。Psst: GitKraken 的 [Learn Git 库](https://www.gitkraken.com/learn/git)拥有关于 [Git 流](https://www.gitkraken.com/learn/git/git-flow)和其他 [Git 分支策略](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy)的大量资源。

利用“git flow”进行工作的用户现在将会看到自动完成建议，以满足他们的用例，例如`git flow init`或`git flow feature start`，甚至`git flow release publish`。Psst: GitKraken 的 [Learn Git 库](https://www.gitkraken.com/learn/git)拥有关于 [Git 流](https://www.gitkraken.com/learn/git/git-flow)和其他 [Git 分支策略](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy)的大量资源。

**Git 别名的自动完成**

#### Git 用户可以在 Git 内部创建便捷的快捷方式，方法是[向他们的 Git 配置](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases)添加别名。对于在终端工作的人来说，这些别名可以节省大量的输入来完成他们的工作。 [GitKraken CLI](https://www.gitkraken.com/cli) 现在会选择这些别名，并以自动完成建议的形式呈现，为用户节省更多时间。

Git 用户可以在 Git 内部创建便捷的快捷方式，方法是[向他们的 Git 配置](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases)添加别名。对于在终端工作的人来说，这些别名可以节省大量的输入来完成他们的工作。 [GitKraken CLI](https://www.gitkraken.com/cli) 现在会选择这些别名，并以自动完成建议的形式呈现，为用户节省更多时间。

**Git 重置的暂存文件建议**

#### `git reset`命令允许用户撤销存储库中的更改。这是一个非常强大的命令，部分原因是它允许用户非常具体地了解他们正在重置的内容，直到文件级别。GitKraken CLI 的 autocomplete 现在会建议暂存文件，使使用 Git 重置更加容易。

`git reset`命令允许用户撤销存储库中的更改。这是一个非常强大的命令，部分原因是它允许用户非常具体地了解他们正在重置的内容，直到文件级别。GitKraken CLI 的 autocomplete 现在会建议暂存文件，使使用 Git 重置更加容易。

**Git 添加和相对路径**

#### 当在一个目录中工作时，完整的路径名通常会比自动完成建议窗口所能完全显示的要长得多。现在，当在终端标签中使用`git add`命令时，用户将在存储库的子文件夹中看到相对路径。

当在一个目录中工作时，完整的路径名通常会比自动完成建议窗口所能完全显示的要长得多。现在，当在终端标签中使用`git add`命令时，用户将在存储库的子文件夹中看到相对路径。

**更新终端界面选项**

### **更新终端界面选项**

**新储存库的自动可视化面板**

#### 当用户在本地启动一个新的 Git 存储库并使用 GitKraken CLI 完成第一次提交时，可视化面板将自动打开。这个面板显示了 [Git 客户端](https://www.gitkraken.com/git-client)众所周知的强大视觉效果，包括丰富多彩的提交图和历史、差异和责任视图。

当用户在本地启动一个新的 Git 存储库并使用 GitKraken CLI 完成第一次提交时，可视化面板将自动打开。这个面板显示了 [Git 客户端](https://www.gitkraken.com/git-client)众所周知的强大视觉效果，包括丰富多彩的提交图和历史、差异和责任视图。

CLI 的上下文菜单

#### 在终端选项卡中工作时，用户现在可以右键单击打开新的上下文菜单。该菜单显示某些选项，如将 Git 命令粘贴到终端、复制文本、打开新的终端标签、清除屏幕和关闭终端标签。

在终端选项卡中工作时，用户现在可以右键单击打开新的上下文菜单。该菜单显示某些选项，如将 Git 命令粘贴到终端、复制文本、打开新的终端标签、清除屏幕和关闭终端标签。

**新的主题选项**

## 当我们在奥运会期间关注金牌、银牌或铜牌时，您可能希望您的 GitKraken 客户端提交图是紫红色、洋红色和靛蓝色！🌈

用户现在可以在他们的自定义主题中自定义提交图形的颜色！包含的主题文件中增加了`// graph colors`的例子，为你指出正确的方向。可以从`Preferences`下的`UI Customization`部分的`Theme`下拉菜单中设置自定义主题。

用户现在可以在他们的自定义主题中自定义提交图形的颜色！包含的主题文件中增加了`// graph colors`的例子，为你指出正确的方向。可以从`Preferences`下的`UI Customization`部分的`Theme`下拉菜单中设置自定义主题。

成功的道路上没有速度限制

## 这一发布标志着 GitKraken 客户端的一个主要基准和 GitKraken 客户端用户性能的一个新时代。对该工具底层引擎的改进将帮助所有用户更快地完成更多工作。

GitKraken 团队投入了大量资金，以确保我们的工具具有创新性，能够应对计算领域接下来将采用的任何新架构。毕竟，这不是苹果第一次通过架构转换来颠覆 CPU 行业，它在 2000 年代中期就从 PowerPC 转向了英特尔处理器。我们怀疑这将是最后一次出现类似的创新。

无论接下来会发生什么，我们都致力于确保 GitKraken 客户端将继续使基于 Git 的团队协作变得简单、安全和强大！

无论接下来会发生什么，我们都致力于确保 GitKraken 客户端将继续使基于 Git 的团队协作变得简单、安全和强大！

您将会爱上 GitKraken Client for Windows、Mac 和 Linux 8.3 版的破纪录性能。立即下载，亲自体验！

您将会爱上 GitKraken Client for Windows、Mac 和 Linux 8.3 版的破纪录性能。立即下载，亲自体验！