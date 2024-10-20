# NodeGit 和 libgit2 如何增强 GitKraken |免费 Git GUI 下载

> 原文：<https://www.gitkraken.com/blog/nodegit-libgit2>

## **为 GitKraken 提供动力的喷气发动机**

我们从第一次试用 GitKraken Git GUI 的人那里听到的一个常见问题是“撤销和重做按钮是如何工作的？”如果你习惯于只使用 CLI，或者只在后台运行 [Git CLI 命令](https://www.gitkraken.com/learn/git/commands)的 GUI，这可能看起来像某种奇怪的巫术。

这背后真正的技术，以及 GitKraken 所有的了不起之处，不是魔术，而是开源技术。支持许多其他 Git 项目的相同开源技术。它被称为 NodeGit，我们非常兴奋地告诉你关于它的一切。

使用由 NodeGit 支持的 GitKraken 的撤销和重做按钮，使您的 Git 工作流程更快更安全！

## **不带命令行的 Git**

在 git-scm.com 官方手册的后面，你会发现一个叫做[“管道和瓷器”](https://git-scm.com/book/en/v2/Git-Internals-Plumbing-and-Porcelain)的章节。本章分解了 Git 软件如何在内部运行(“管道”)以及人和代码如何与它交互(“瓷器”)。通常，在为 Git 开发功能时，软件会执行运行 Git“瓷器”API 的子进程，并解析标准输出流的结果。

这可能是非常低效的。当执行 CLI 命令时，Git 需要与底层存储库数据库进行交互，它上面的软件必须通过文本解析重新解释检索到的结果。

幸运的是，我们可以完成基于 Git 的版本控制，而不直接依赖 Git 的软件来实现。Git 提供了一组如何操作文件系统和跟踪随时间变化的协议和标准，以及一组使用这些协议的预打包的 CLI 命令。

对于那些可能是计算机科学新手的人来说，协议指的是类似于简单邮件传输协议(SMTP)的东西。协议本身不是电子邮件，而是电子邮件如何在计算机之间发送。因此，无论你更喜欢 Outlook、iCloud、Gmail 还是 AOL，你都可以发送和接收电子邮件，因为所有工具都使用相同的标准。

那么，GitKraken 如何在没有 CLI 的情况下利用 Git 呢？

## **libgit 2——Git 的开源库**

这就是 libgit2 的用武之地。libgit2 是一个流行的开源库，用于与 git 交互，但作为一个新的实现，它不需要 Git 管道或瓷器命令。该项目将自己描述为*“Git 核心方法的一个可移植的纯 C 实现，作为一个具有可靠 API 的可重入可链接库提供，允许你用任何支持 C 绑定的语言编写本地速度的定制 Git 应用程序。”*

这是一个很棒的项目，但是我们知道的大多数开发人员并不特别喜欢用 c 语言工作。

如果你从未直接使用过 C 语言，你并不孤单。这是一种非常有效和强大的语言。但是，就像后来的 C++一样，它对专家非常友好，但对用户不太友好。这就是为什么这么多开发人员喜欢使用 Python、PHP 和 JavaScript 等语言，包括 Node。

## 【Node 呢？

在 GitKraken，我们热爱 Node！我们传奇的跨平台 Git GUI，可免费下载，作为 T2 电子应用程序跨平台交付。为了在 Node 中有效地使用 libgit2，我们需要一种方法来处理相同的 API，而不需要重写和维护 Node 中的整个库。这就是我们故事中的主人公出现的地方。

## **为胜利而努力**

NodeGit 是一个开源项目，任务是为 libgit2 项目提供节点绑定。简而言之，NodeGit 管理一个小得多的模板文件集，这些模板文件解析 libgit2 的文档以生成 Node 可以访问的代码。这是一个相当深奥的解释，所以让我们后退一步。

如果你看看 libgit2 的代码库，有超过 100，000 行代码。让我们来看看为什么这是如此巨大。

每个 Git 命令实际上都需要几组小步骤。例如，`git add`需要 3 个基本步骤:

1.  根据文件内容计算 SHA-1。
2.  将文件内容存储在位于的存储库数据库中。git/对象
3.  将文件内容注册到。git/索引

可以想象，执行非常复杂的任务，如 [Git rebase](https://www.gitkraken.com/learn/git/git-rebase) 或 [Git cherry pick](https://www.gitkraken.com/learn/git/cherry-pick) 会有更多的步骤，尤其是当您考虑到每个命令有几十个选项时。

尝试手动重写从 C 到 Node 的所有代码，然后尝试在每次更新时手动更新它，这将是一项艰巨的任务。因此，NodeGit 维护了一个小得多的代码库，大约 6000 行，可以查找和解析 libgit2 的文档，以生成我们可以在 Node 而不是 c 中运行的代码。当您在 GitKraken 接口中拖放它们时，新代码会以正确的顺序进行完全相同的 API 调用，以完成类似于 [Git pull](https://www.gitkraken.com/learn/git/tutorials/what-is-git-pull) 或 [Git commit](https://www.gitkraken.com/learn/git/commit) 的任务。

为什么这对 GitKraken 如此重要是因为它允许我们在这些调用中插入自己的函数。这就是让我们给用户一个撤销按钮或者其他任何 GitKraken Git GUI 的独特功能！

## **未来是北海巨妖**

我们非常自豪能够成为这个项目的维护者，这个项目正被世界各地如此多的软件产品所利用。NodeGit 让我们在 GitKraken 中做一些非常棒的事情，并让我们有能力做更酷的事情。我们也很自豪地支持开源社区。最终，我们希望帮助 Git 改进，并帮助地球上的每一个 Git 用户以一种更加透明、可靠和高效的方式管理他们的工作。

如果您还没有了解 GitKraken Git GUI 所提供的功能，那么现在就是您的机会。

“谢谢@GitKraken 的撤销功能。不小心丢弃了我所有的更改，但现在它们又回来了😁"–[@ offset 337](https://twitter.com/offset1337/status/1107615811181658118)

怪怪会保护你不受自己的伤害。