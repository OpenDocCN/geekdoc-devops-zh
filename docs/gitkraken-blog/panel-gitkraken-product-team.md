# GitKraken 产品团队小组- 2021 GitKon Git 会议

> 原文：<https://www.gitkraken.com/gitkon/panel-gitkraken-product-team>

[https://www.youtube.com/embed/Ql3QJW8MBvI?feature=oembed](https://www.youtube.com/embed/Ql3QJW8MBvI?feature=oembed)

视频

在 2021 [GitKon Git conference](https://gitkon.com/) 的这个非常特别的小组讨论中，GitKraken Client 背后的产品团队分享了对世界上最流行的 Git 工具的独特见解，并回答了社区问题。

GitKraken 产品团队小组包括:

*   GitKraken 产品经理兼小组主持人 Jonathan Silva
*   然后是 suceava，CTO，gitkraken
*   GitKraken 产品总监 Jeff Schinella
*   GitKraken 产品总监 Justin Roberts

## **git kraken 客户端的外观和感觉**

有人把 GitKraken 客户端比作[吉他英雄](https://en.wikipedia.org/wiki/Guitar_Hero)，拥有充满活力的彩色阵列。Justin Roberts 透露，GitKraken 团队最初决定构建 GitKraken 客户端的原因之一是 Git 客户端的环境在他们开始时相当糟糕。令人惊讶的是，甚至没有 Git 客户端有深色主题，所以从一个带有非常基本的深色调色板的开发工具开始很容易。带有亮色的深色主题让 GitKraken Client 立即从市场上的其他 Git 客户端中脱颖而出。

Jeff Schinella 指出，不同城市的地铁风格图也有一些启发。节点连接的想法，以及能够查看 Git repo 的状态，沿着一条停止的路径，给了团队构建 GitKraken 客户端的方向。事实上，很多设计，包括臭名昭著的 [Keif 北海巨妖标志](https://www.gitkraken.com/store#logos)，都深受干净的 60 年代风格的平面设计和地铁系统图的影响。

## **拖放**

GitKraken 客户端的中央图形允许用户拖放一个分支来开始一系列其他操作。Jonathan Silva 指出，这是帮助他[了解 Git](https://www.gitkraken.com/learn/git) 以及“触发”T2 Git 合并或 T4 Git 重组意味着什么的事情之一。根据 Jeff 的说法，拖放的想法已经存在了一段时间，所以团队在 GitKraken 客户端实现了它，使人们更容易对他们的存储库进行更改，而不必在终端上键入。

GitKraken 客户端中的可视化表示使得抓取提交并将其拖到新位置变得非常直观。当你谈到 Git 可视化 GitKraken 客户端之所以成为现在这个样子的一个重要原因——你是在谈论能够看到和可视化一个[提交](https://www.gitkraken.com/learn/git/commit)或 [Git 分支](https://www.gitkraken.com/learn/git/branch)的样子。它有助于用户理解 Git，并发现您可以在 GUI 中做的事情，这些事情您可能无法仅使用 CLI 来完成。只要把一个分支拖到另一个分支上，你就会得到一个上下文菜单，上面写着“这是你可以在这两个分支之间做的所有事情。”

## **连接到 Git 托管服务**

使用 GitKraken 客户端与 Git 托管服务的集成与仅通过 SSH 和 HTTPS 连接之间的区别在于，您可以使用特定于提供商的操作，例如针对 [GitHub pull 请求](https://www.gitkraken.com/learn/git/problems/github-pull-requests)的交互式 PR 管理，它甚至允许您在不离开工具的情况下合并 pull 请求。连接集成后，您可以通过 GitKraken 客户端中的其他托管服务与拉请求进行交互，也可以直接在 GitKraken 客户端中进行类似[分叉远程](https://www.gitkraken.com/learn/git/problems/github-how-to-fork)的操作。

虽然 Git 托管服务集成非常强大，但是您仍然可以使用 SSH 和 HTTPS 连接到其他尚未与 GitKraken Client 完全集成的服务。您甚至可以拥有不同的概要文件来连接到不同的集成。GitKraken 团队一直在听取社区的意见，看看下一步要集成什么样的 Git 托管服务。 [GitKraken 维护着一个反馈网站](https://feedback.kraken.com)，社区成员可以在这里提出新的请求并投票支持现有的请求。例如，社区真正推动了与 AWS CodeCommit 和微软 Azure Repos 的集成。这些已被添加到 GitKraken 客户端的基本功能，并计划在未来的版本中提供更丰富的集成功能。

## **未充分利用的 GitKraken 客户端功能**

**饭桶怪**

### Dan Suceava 最喜欢的未被充分利用的 GitKraken 客户端特性是 Git 责备。这是一个非常强大的功能，可以让你查看历史和作者信息。现在不要用它去指责别人😉，但是能够看到谁做了哪些更改以及何时做的更改也很好。跟踪某些东西何时损坏以及谁可能损坏它们可以节省大量调查和实施修复的时间。

**活动日志**

活动日志是 GitKraken 客户端的另一个功能，丹希望更多的人知道。要使用此功能，请点击 GitKraken 客户端页脚栏中看起来像项目符号列表的图标。

### 活动日志向您显示在客户端内部执行了哪些操作。许多习惯于 CLI 的人经常会问，当 GitKraken 客户端执行一个操作时，会发生什么情况。虽然 GitKraken Client 实际上并不执行 Git CLI 命令，但它在后台做了很多事情，活动日志会向您显示执行了哪些操作。

Activity logs is another feature in GitKraken Client that Dan wishes more people knew about. To access this feature, click on the icon in the footer bar of GitKraken Client that looks like a bulleted list. 

Activity logs show you what actions were performed inside the client. Many people who are used to the CLI often ask to see what happens under the hood when GitKraken Client does an action.  While GitKraken Client does not actually perform Git CLI commands, it does a lot of stuff in the background, and the activity logs show you what operations were performed. 

**命令面板**

对于贾斯汀来说，GitKraken 客户端中最没有被充分利用的功能是[命令面板](https://support.gitkraken.com/start-here/command-palette/)。你可以通过输入`Ctrl/Cmd+P`或者点击顶部导航菜单中的魔棒图标来快速打开它。从那里，您可以访问应用程序提供的许多命令，而不必实际点击它们。

### Justin 个人很喜欢它，因为当他审查 pull 请求时，他可以通过键入问题编号找到正确的分支。命令面板显示所有接近预定目标的选项，因为它使用“模糊搜索”逻辑，也称为快速全局搜索。

For Justin, the most underutilized feature in GitKraken Client is the [Command Palette](https://support.gitkraken.com/start-here/command-palette/). You can open this quickly by typing `Ctrl/Cmd+P` or by clicking the magic wand icon in the top nav menu. From there, you gain access to a lot of commands that the application offers without having to go and actually click them. 

Justin personally loves it for checking out branches when he’s reviewing pull requests as he can find the right branch by just typing the issue number. The command palette reveals all the options close to the intended target because it uses “fuzzy search” logic, also known as fast global search.  

**内置代码编辑器**

Justin 认为值得更多关注的另一个特性是 GitKraken 客户端中包含的编辑器。客户端嵌入了 [Monaco 编辑器](https://microsoft.github.io/monaco-editor/)，这是与代码使用的基本编辑器相同的编辑器。

### 在 GitKraken 客户端进入一个文件并开始编辑真的很容易；只需使用命令面板搜索要在编辑器中打开的文件，然后开始工作！

Another feature that Justin thinks deserves more spotlight is the editor included inside of GitKraken Client. The client embeds the [Monaco editor](https://microsoft.github.io/monaco-editor/), which is the same basic editor VS Code uses.  

**交互式重置基础**

GitKraken 客户端提供的 Jeff 最喜欢的功能是交互式 rebase。从开始到结束，试图弄清楚并理解 [Git interactive rebase](https://www.gitkraken.com/learn/git/problems/git-interactive-rebase) 如何发生的整个过程使用 CLI 非常困难。利用 GitKraken 客户端的拖放功能，交互式 rebase 成为一种直观的体验，您可以在单个 GUI 菜单中对提交进行重新排序、合并、重命名或压缩。当在 GitKraken 客户端之外执行时，交互式 rebase 是相当乏味的，并且可能是非常具有破坏性的。

### **Interactive Rebase**

GitKraken Client 降低了您的回购处于不良状态的风险，并在执行高级操作时给予您更多的控制，如交互式 rebase。

**吉塔拉克 CLI**

根据 Justin 的说法，GitKraken 客户端 8.0 版本发布的 GitKraken CLI 可以追溯到 GitKraken 团队最初的目标。他们希望为可能是 Git 新手的用户开发超级直观和清晰的工具。该团队希望为用户提供一个真正易于使用的图形用户界面。

## 从一开始，GitKraken 团队就对 CLI 有一点厌恶，这有点像是一种“Git 客户端对 CLI”的心态。这是该团队长期以来的看法，因为他们不明白为什么有人会选择使用 CLI 而不是 GUI。

然而，这些年来，GitKraken 团队开始更多地了解用户和软件开发行业，并且很明显，虽然 Gi 工作流确实已经发展了，但是命令行并没有走向任何地方。有些人是使用 CLI 长大的，他们对此非常熟悉。在 CLI 中还有一些更强大的功能，GitKraken 不想忽视这部分开发人员。

GitKraken 团队真的很想接受 CLI 中可以做的事情，然后在 CLI 工具中应用 GitKraken Client 提供的一些可用性优势。

该团队通过采访用户发现的一件事是，终端设置总是非常紧张、烦人和困难。您可以用来定制终端的包和东西的数量可能看起来非常多，而且非常耗时。

The GitKraken team really wanted to embrace what can be done in the CLI, and then apply some of the same usability benefits offered by GitKraken Client in a CLI tool. 

**Git 命令的自动完成建议**

GitKraken 团队希望提供许多对 CLI 用户有帮助的特性，比如 Git 命令的自动完成建议。当你输入 [Git 命令](https://www.gitkraken.com/learn/git/commands)时，GitKraken Client 会根据你输入命令的位置给出建议，而不是去记忆命令或浏览文章寻找你需要的东西。

### **Auto-Complete Suggestions for Git Commands**

The GitKraken team wanted to offer a lot of features helpful for CLI users, like auto-complete suggestions for Git commands. When you’re typing [Git commands](https://www.gitkraken.com/learn/git/commands), rather than having to memorize commands or dig through articles trying to find what you need, GitKraken Client will start to suggest things based on where you’re at in typing the command.  

**提交终端选项卡中的图形**

GitKraken CLI 给终端带来可视化，真的很强大。GitKraken 团队经常从用户那里听到的是，他们使用 GitKraken 客户端是因为他们可以将事情可视化，但他们实际上是回到 CLI 来运行命令。GitKraken CLI 创建了一个地方，您可以在这里做所有使用 Git 做的事情，无论是使用 CLI 还是使用提交图，都可以在一个地方完成，无需切换上下文。

### **Commit Graph in Terminal Tabs**

GitKraken CLI brings visualization to the terminal, which is really powerful. Something the GitKraken team often hears from users is that they use GitKraken Client because they can visualize things, but they actually go back to the CLI to run commands. GitKraken CLI creates a place where you can do all of the things that you do with Git, whether that’s with CLI or with the commit graph, all in one place without context switching.

[观看此视频剪辑](https://youtu.be/Ql3QJW8MBvI?t=1018)听听 GitKraken 团队对 GitKraken CLI 的更多介绍。

**git kraken 客户端中的团队**

GitKraken 客户端在[7.7](https://www.gitkraken.com/blog/gitkraken-v7-7)版本中引入了团队视图。在左侧面板中，有一个团队窗格，您可以在其中选择您所属的任何团队并执行某些团队操作。根据 Justin 的说法，这是更好地可视化 Git 存储库的原始目标的自然延伸。

## GitKraken 客户端中的团队功能超越了个人对项目的贡献，可视化了仓库中每个人的工作，包括他们所有的远程控制，他们如何连接，以及他们如何改变项目。虽然 GitKraken Client 可能曾经被认为主要是一个单一的开发工具，但团队的加入已经是朝着更好地服务组织迈出的一大步。

GitKraken Client introduced a team view in [version 7.7](https://www.gitkraken.com/blog/gitkraken-v7-7).  In the left panel, there is a Teams pane where you can select any teams that you belong to and perform certain team actions.  According to Justin, this is a natural extension of the original goal to better visualize a Git repository.  

**避免合并冲突**

我们都知道这样的场景，您正在做团队中其他人也在做的事情，并且您最终会遇到 Git 合并冲突。合并冲突会导致很多问题。

### Teams 面板显示了谁同时在处理存储库，GitKraken 客户端实际上会在代码被推送之前提醒您潜在的合并冲突。

We all know the scenario where you’re working on something that someone else on your team is also working on, and you ultimately run into a Git merge conflict. Merge conflicts can cause a lot of problems. 

The Teams panel shows who is working on the repository at the same time, and GitKraken Client will actually alert you of a potential merge conflict before the code is pushed.  

GitKraken Client 不仅会警告您潜在的合并冲突，如果它们确实发生了，该工具还会帮助您解决它们。GitKraken 客户端的内置合并工具使得合并冲突解决这一众所周知的高风险任务变得更加安全。

**深度链接**

想象一个场景，你想和某人共享一个 [Git 分支](https://www.gitkraken.com/learn/git/branch)。你可能会跳到 Slack 上说“嘿，你能去看看我的分店吗？”你必须记住分支机构的名称，输入分支机构的名称，然后你的同事必须读出分支机构的名称，并返回到 GitKraken 客户端搜索分支机构。这正是 GitKraken Client 引入深度链接的原因。

深度链接使得点击一个分支，抓取一个链接，然后将该链接粘贴到 Slack，或者任何你需要引用它的地方变得非常容易。这使得其他人能够点击链接，并在他们的机器上轻松打开 GitKraken 客户端中的分支。

### [跳到视频中的这一部分](https://youtu.be/Ql3QJW8MBvI?t=1508)听关于 GitKraken 客户团队功能和深度链接的完整对话。

Imagine a scenario where you want to share a [Git branch](https://www.gitkraken.com/learn/git/branch) with someone. You might hop on Slack and say “hey can you go check out my branch.” You have to remember the branch name, type the branch name, and then your colleague has to read the branch name and go back into GitKraken Client and search for the branch. That scenario is exactly why GitKraken Client has introduced deep linking. 

**团队最喜欢的厨师**

Keif 是 GitKraken 的吉祥物，他们喜欢穿上戏服(psst:看看 Keif 画廊！)各有各的个性，每个 GitKraken 团队成员都有自己喜欢的。

[观看此视频](https://youtu.be/Ql3QJW8MBvI?t=2049)了解哪些 Keifs 进入了小组成员的最爱名单。

## **GitKraken 客户端继续进化**

小组讨论了 GitKraken 客户端是如何从一种解决内部问题的方式发展成为世界领先的 Git 客户端，帮助[数百万用户](https://www.gitkraken.com/customers)更高效地使用 Git 的。

无论是可视化，执行高级和复杂 Git 任务的便利性，还是令人敬畏的 CLI 和团队功能，我们知道您会喜欢在您的工作流程中使用 [GitKraken Client](https://www.gitkraken.com/git-client) 。[今天就下载 Mac、Windows 和 Linux 版的传奇 GitKraken 免费客户端](https://www.gitkraken.com/git-client/try-free)！

## **GitKraken Client Continued Evolution**

The panel gave a look into how GitKraken Client has evolved from a way to solve an internal issue to the leading Git client in the world, helping [millions of users](https://www.gitkraken.com/customers) become more productive using Git.

 Whether it’s the visualization, the ease of performing advanced and complex Git tasks, or for the awesome CLI and team features, we know you will love using [GitKraken Client](https://www.gitkraken.com/git-client) in your workflow. [Download the legendary GitKraken Client free](https://www.gitkraken.com/git-client/try-free) for Mac, Windows, and Linux today!