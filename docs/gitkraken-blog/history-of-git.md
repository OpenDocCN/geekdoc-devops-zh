# Git |更差更好和 Git 生态系统的历史

> 原文：<https://www.gitkraken.com/gitkon/history-of-git>

[https://www.youtube.com/embed/AhJmv1paL_w?feature=oembed](https://www.youtube.com/embed/AhJmv1paL_w?feature=oembed)

视频

2005 年 4 月，Linus Torvalds 给了我们一千行 C 代码，其中点缀着 TODOs 和关于设计决策的评论。这些代码是“信息经理”的基础，帮助他开发 Linux 内核。

Git 是主要的版本控制系统。如果有版本控制之战，Git 赢了。但是为什么 Git 会变得如此流行，Git 的历史是怎样的？根据 Lisp 的创造者 [Richard Gabriel](https://en.wikipedia.org/wiki/Richard_P._Gabriel) 的说法，这是因为“越差越好”当被问及为什么 C 语言和 Unix 操作系统会“赢”Lisp 时，他半开玩笑地回答了这个问题。他后来继续将“越差越好”定义为一种被广泛接受的整体哲学。

## **软件哲学**

为了符合“越差越好”的理念，一项技术需要满足四个条件:

1.简单

2.正确性

3.一致性

4.完全

“越差越好”是一种哲学，与另一种被称为“正确的事情”或有时被称为麻省理工学院哲学的思维方式形成对比。“正确的事情”哲学同样具有简单、正确、一致和完整的首要原则。

这两种哲学有什么不同？让我们先来看看“正确的事情”哲学。

‘Worse is better’ is a philosophy that sits in contrast to another way of thinking called ‘the right thing’ or sometimes referred to as the MIT philosophy.  ‘The right thing’ philosophy has the same overarching tenets of simplicity, correctness, consistency, and completeness. 

How do these two philosophies differ? Let’s look first at ‘the right thing’ philosophy. 

在“正确的事情”哲学中，当你看一个系统的设计时，你首先看它的简单性。设计和代码必须简单，无论是在实现方面还是在接口方面。一定要用起来简单。事实上，最后一个，简单易用的 UI，是最重要的租户，取代了实现的简单性。

下一个租户说软件必须正确。不正确是不允许的，所以你最好有一个没有错误的程序。

软件也必须始终如一地适合‘正确的事情’。事实上，一致性甚至比界面简单性更重要。只要软件保持一致，你可以在简单性方面走一些捷径。

最后，软件必须完整。它必须是可用的，并且做用户期望它做的所有事情。

这些听起来都是大房客。

In ‘the right thing’ philosophy, when you look at the design of a system, you look first at its simplicity. The design and code must be simple, both in terms of its implementation and its interface. It must be simple to use. In fact, that last one, the simple to use UI, is the most important tenant, superseding the simplicity of implementation.

The next tenant says software must be correct. Incorrectness is not allowed, so you should ideally have a bug-free program.

Software must also consistently fit into ‘the right thing’. In fact, consistency is even more important than interface simplicity. You can cut a few corners in the simplicity area as long as the software remains consistent. 

**“越差越好”与“正确的事情”**

“正确的事情”与“越差越好”的哲学相比如何？

## 在“越差越好”的方法中，简单也是第一点，但是有一点扭曲。与其关注用户界面的简单，不如关注实现的简单。代码必须易于理解和扩展。这比简单易用更重要。“越差越好”方法中的正确性要求软件在所有可观察到的方面都应该是正确的，但实际上简单比正确要好得多。简单的实现胜过完全正确，尤其是在早期版本中。

“越差越好”下的设计需要一致，但实际上最好是去掉设计中处理不太常见情况的部分，而不是在应用程序中引入复杂性或不一致性，因此，实现的简单性再次胜出。

最后,“越差越好”中的完整性是重要的，但是当实现的简单性受到危害时，它可能会被牺牲掉。“越差越好”的核心是构建简单的可扩展代码，而不一定是好用、一致或完整的代码。诚然，这有点像稻草人的观点，但正如 Richard Gabriel 逐渐意识到的，坚持“越差越好”哲学的软件比信奉“正确的事情”世界观的软件有更好的存活率。这也是 C 和 Unix 最终战胜 Lisp 的原因。

How does ‘the right thing’ compare with the ‘worse is better’ philosophy? 

**Linux 和 Git 以及“越差越好”**

有一个人比其他任何软件工程师都更坚持“最坏就是最好”的哲学，他就是 Linus Torvalds。Linus 构建了 Linux 内核和 Git，这是世界上最流行的版本控制系统。这是实践“最坏就是最好”哲学的两个极好的例子。

## **Linux and Git and ‘Worse is Better’**

There’s one person who stands out as adhering to the ‘worst is better’ philosophy better than any other software engineer: Linus Torvalds. Linus built the Linux kernel and also Git, the world’s most popular version control system. Those are two excellent examples of the ‘worst is better’ philosophy in practice.

第一次有人听说 Linux 操作系统要追溯到 1991 年，当时一个名叫 Linus 的年轻学生在 Minix 邮件列表上发帖。Minix 是 80 年代后期的一个类似 Unix 的操作系统。Linus 有一台 386 计算机，他试图学习如何在很深的底层对它编程，以便在 386 芯片组上构建一个操作系统。在早期的声明中，Linus 指出 Linux“永远不会成为一个专业的操作系统”。然而，Linux 内核非常受欢迎。它发展得非常快，满足了许多计算机程序员的需求。人们在他的 Linux 内核和大量“用户空间”代码(如 shells)的基础上构建发行版，以与系统(如 [Bash](https://www.gitkraken.com/blog/what-is-git-bash) 、Zsh 或 Tcsh)进行交互。

像麻省理工学院和自由软件基金会的 Gnu/Hurd 项目这样的竞争架构从来没有像 Linux 发行版那样真正成为完整的系统。这是因为他们太专注于架构和获得“恰到好处”的东西，以至于他们从来没有真正把代码拿出来。Linux 以其“越差越好”的方式胜出，因为它发布得早，用户可以立即在它的基础上进行构建。Linus 刚刚宣布了他正在构建的东西，尽管当时它几乎无法运行，然后全世界都在看着它发展成为一个健壮而强大的操作系统。经过近 30 年的时间，经过深思熟虑和精心设计的 Gnu/Hurd 内核仍然没有发布 1.0 版本。

The first time anybody heard of the Linux operating system was way back in 1991 when a young student named Linus posted on a Minix mailing list.  Minix was a Unix-like OS from the late 80s. Linus had a 386 computer and was trying to learn how to program it at a very deep, low level to build an operating system on the 386 chip set.

In his early announcement, Linus noted that Linux will “never be a professional operating system”. However, the Linux kernel was incredibly popular. It took off really fast and filled a need that a lot of computer programmers had. People built distributions on top of his Linux kernel and a lot of the “user space” code, such as shells, to interact with the system like [Bash](https://www.gitkraken.com/blog/what-is-git-bash), Zsh, or Tcsh.

**Git 前的协作**

Linux 内核是作为一个开源项目合作开发的；可以说是世界上有史以来最成功的开源项目。合作者的工作方式是通过邮件列表，很像 Linus 用来宣布他最初想法的那个。人们交换补丁，在本地机器上编写代码，运行 diff，然后将输出发送到邮件列表。如果特定内核子系统的维护者对这一变化感兴趣，他们会在邮件列表中讨论这一变化，并最终将这一变化应用到下一个发行版中。从根本上说，这就是 2002 年 Linux 内核的变化。没有真正的版本控制系统。有项目维护者版本的源代码，仅此而已。不过，这个协作系统很快就达到了一个扩展点，在这个点上，手动操作太多了。他们需要一个系统来帮助，这就是 [BitKeeper](http://www.bitkeeper.org/) 的用武之地。

## **Collaboration before Git**

**从 BitKeeper 到 Git**

BitKeeper 是第一个分布式版本控制系统。它在邮件列表系统之上增加了严密性，使得协作更加容易。BitKeeper 的问题在于它的许可证。这在当时是一种相当新颖的许可模式，BitKeeper 对其专有软件的商业使用收费，但给 Linux 内核开发者一个免费的“开源”许可。他们可以免费使用它，但他们没有源代码，而且许可证禁止逆向工程 BitKeeper。它还指出，用户不能试图建立一个竞争的版本控制系统。这导致了一些 Linux 内核开发者和 BitKeeper 背后的团队之间的紧张关系。最终，Linux 社区决定他们应该寻找其他的选择。

## **From BitKeeper to Git**

然后是水银

在采用 BitKeeper 和寻找替代方案之间的时间里，有许多其他人在研究分布式版本控制问题。值得注意的是，2005 年 Matt MacKall 在 Linux 内核邮件列表上发布了 Mercurial v 0.1——一个最小的可扩展分布式 SCM。

## **Then Came Mercurial**

In the time between adopting BitKeeper and looking for alternatives, there were a number of other folks working on the distributed version control problem. Of note, in 2005 Matt MacKall posted on the Linux kernel mailing list announcing Mercurial v0.1 – a minimal scalable distributed SCM.

虽然 Mercurial 还处于早期的概念验证阶段，但它已经表现得令人惊讶地好，并赢得了粉丝。在 Matt 的第一封邮件中，他提到了 Mercurial 仍需做的一些事情，包括各种清理、打包、文档和测试。这是一个“越差越好”方法的完美例子。有趣的是，在 Linux 内核邮件列表上，他得到的唯一回应是有人告诉他，这是**太**了，“越差越好。”

有一种普遍的观点认为，如果一个工具很难使用，格式不好，或者文档不好，那么不管它有多棒，人们都不会使用它。这与“越差越好”的哲学正好相反。事实上，只要代码简单，我们就可以把用户界面的简单性放在一边。Mercurial 的代码非常出色，这也是它如此受欢迎的原因。

同样在 2005 年 4 月，Linus 在 Linux 内核邮件列表上发布了一个公告，用的是他最初发布 Linux 时同样低调的方式。他宣布他正在编写一些脚本来帮助他管理 Linux 内核邮件列表上的补丁。他说，他的脚本不会是一个源代码控制管理系统或版本控制系统，只是一些特定于他的用例。Git 最初只是 Linus 为帮助他的用例而编写的一些脚本，但是他在开发的早期发布了这些脚本，以防其他贡献者会发现它们很有帮助。

**管理 Git(和 Linux)开发**

Linus 在 2005 年 4 月宣布的是 Git 的第一个版本。如果您在 [Git 源代码镜像库](https://github.com/git/git)上运行一个 Git 日志，您可以看到这个项目看起来如何，以及它是如何随着时间的推移而增长的。这些第一批 Git 日志消息是 Git 被“自托管”的第一次引用，这意味着 Git 项目正在使用 Git 来管理 Git。

## 自 Linus 在 2005 年发表第一篇文章以来，我们已经走过了漫长的道路。令人惊讶的是，Git 的统治地位是如此有影响力和持久，尤其是当你想到它最初只是少数几个文件，只有 1250 多行代码。它是不完整的，你几乎不能用它做任何事情。在其最初的形式中，Git 充其量只是一种分布式文件系统的机制。

正如 Linus 所说，它不是版本控制系统，也不是 SCM。它既不一致，也不真正正确，尤其是在缓存方面。这当然不简单，因为你必须是 Linus Torvalds 才能知道如何使用它。这绝对不是你今天看到的那个饭桶。

尽管存在这些问题，人们还是开始使用它。具体来说，Linux 内核黑客开始使用它，因为它简化了他们的生活，就像它简化了 Linus 的生活一样。

黑客的目标是创建补丁让 Linus 接受，而 Linus 的目标是管理和接受这些补丁。这正是这个系统所做的。

最初，没有服务器端组件。这非常符合“越差越好”的哲学:当 Gmail 这样的服务可以为你做的时候，为什么要建立自己的服务器呢？其核心是通过电子邮件打补丁，它把实际的硬件部分——运行保存补丁的服务器——卸载给已经编写了工作版本的其他人。

莱纳斯的目标实现了吗？是的。它出现了并被使用。人们开始使用它而不仅仅是 Linux 内核开发，并且开始使用 g it 来管理 Git 开发！有人可以修复 Git 中的一个 bug，并向邮件列表发送一个补丁。

Originally, there was no server-side component. This very much fits into the ‘worst is better’ philosophy: why build your own server when a service like Gmail can do it for you? At its core, it was about patches over email and it offloaded the actual hard bits—running a server that held the patches—to somebody else who already wrote a working version. 

Was Linus’ goal satisfied? Yes. It got out there and got used. People started using it beyond just Linux kernel development and started using Git to manage Git development!  Somebody could fix a bug in Git and send in a patch to the mailing list.

Git 的接口并不简单，但它的代码很简单，这也是它能够被扩展的原因。正如 GitHub 的联合创始人汤姆·普雷斯顿-沃纳所说，“Git 让协作成为可能，但并没有让协作变得容易。”它使一种非常特殊的合作形式成为可能，一种非常分布式的、拥抱开源哲学的合作形式。

**开放开源**

如果很难合作，只有某些人掌握着源代码的钥匙，那么开源就不是非常开放的。汤姆和他的朋友克里斯·万斯特拉斯和 PJ·凯悦决定解决这个问题，于是想出了 GitHub.com。

## GitHub 具有托管公共存储库的能力，使得来回发送更改和讨论这些更改变得非常容易。不需要邮寄补丁和等待，你可以通过推你的代码突然打开一个 [Git pull 请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)。您可以派生一个存储库，并与分支机构合作开发任何类型的开源项目。它真的降低了合作的障碍。这是革命性的。

Git 的数据结构很简单，代码也非常简单。即使您不了解图论，在阅读 Git 的格式文档后，您也会对数据结构的简单性印象深刻。简单的数据结构允许人们在 Git 之上构建其他东西，比如 Heroku 中的 [Git 端点。Git 还允许简单的部署；运行 Git 推送即可。](https://devcenter.heroku.com/articles/git)

GitHub came with the ability to host public repositories, making it really easy to send changes back and forth and discuss those changes. Instead of mailing a patch and waiting, you could suddenly open a [Git pull request](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git) by just pushing your code up. You could fork a repository and work with branches to collaborate on any type of open source project.  It really lowered the barriers to collaboration. It was revolutionary.

**进入 Git 客户端**

Git 很快从“另一个”版本控制系统变成了全球范围内占主导地位的版本控制系统。随着 Git 越来越受欢迎，人们开始构建命令行客户端和 Git GUI 客户端，以使其更易于使用。

## 有一个图表有助于可视化的变化和分支。能够看到 [Git 差异](https://www.gitkraken.com/learn/git/git-diff)也非常有帮助，这使得审查变更或决定 [Git rebase](https://www.gitkraken.com/learn/git/git-rebase) 或 [Git merge](https://www.gitkraken.com/learn/git/git-merge) 是否对某些提交更有意义变得更容易。自从上市以来，开发这些工具的开发人员已经推动用户界面变得越来越直观，满足了“越差越好”和“正确的事情”这两个“简单”的理念。多年来，Git 客户端已经从像 [gitk](https://git-scm.com/docs/gitk/) 这样的初级工具发展到 [TortoiseGit](https://www.gitkraken.com/compare/gitkraken-vs-tortoisegit) 的点击和指向，并最终发展到 [GitKraken Git 客户端](https://www.gitkraken.com/git-client)提供的拖放和漂亮的提交图形，它包括一个 GUI 和 [CLI](https://www.gitkraken.com/cli) 。

Git 给了我们一个版本控制更容易使用和理解的世界。在这个世界里，我们拥有使协作和代码部署更容易的平台。由于这种“越差越好”的方法，Git 工具形成了一个非常活跃的生态系统。

Git quickly went from just ‘another’ version control system, to *the* dominant version control system worldwide. As Git’s popularity grew, people started building both command line clients and Git GUI clients to make it easier to use.  

GitKraken 是 Git 工具的一个完美例子，它已经发展到可以应对新的挑战，具有交互式拉请求管理、合并冲突检测和解决、深度链接等高级功能。

**从‘Git’到 Git**

我们也已经从 Git 仅仅是 Linus 最初编写并由 Junio Himano 维护了多年的命令行应用程序，发展到 Git 作为一套 SCM 协议，允许您管理 [Git 库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)。像 [libgit2](https://libgit2.org/) 、 [Jit](https://github.com/cristibaluta/Jit) 、 [NodeGit](https://www.nodegit.org/) 和 [pygit2](https://www.pygit2.org/) 这样的库不需要调用 shell 命令来运行`git`并解析输出。他们可以直接针对文件系统执行任何 [Git 命令](https://www.gitkraken.com/learn/git/commands)，比如 [Git Commit](https://www.gitkraken.com/learn/git/commit) 、 [Git Branch](https://www.gitkraken.com/learn/git/branch) 或 Diff，以各自的编程语言生成输出。这就是 Git 服务和工具真正可以扩展的方式。

## **From `git` to Git**

**赢家是 Git(还有你)**

Git 赢了，因为它信奉“越差越好”的哲学。它不担心是否完整或是否一定正确，至少一开始是这样。Linus 构建了一个最小可行的产品，就像他们在开源世界中说的那样“挠自己的痒”。他构建了一个简单且易于实现的小型工具集。他向全世界发布了它们，现在很难低估 Git 对我们软件开发所有领域的影响。

尽管 Git 取得了胜利，但 Git 仍有一些可以改进的地方。[点击这里查看 Edward 演讲的最后部分](https://youtu.be/AhJmv1paL_w?t=2216)，了解这些领域以及 Git 如何继续发展。另外，你可以看到 2021 年 [GitKon Git 大会](https://gitkon.com/)的现场 Q & A。

充分利用 Linus、Edward 和所有 Git 贡献者在最流行的版本控制系统中所做的工作，以及 GitKraken 团队所做的工作，通过可视化和更好的 UI 使 Git 更加可用[今天下载 GitKraken Git 客户端](https://www.gitkraken.com/download)，包括 GUI 和 [CLI](https://www.gitkraken.com/cli) ！

## **The Winner is Git (and You)**

Git won because it embraced the ‘worse is better’ philosophy. It didn’t worry about being complete or being necessarily correct, at least at first. Linus built a minimum viable product, ‘scratching his own itch’ as they say in the open source world. He built a small set of tools that were simple and easily implemented. He released them to the world and it’s now hard to underestimate the impact that Git has had on all areas of our software development. 

Despite the fact that Git has won, there are still some areas where Git can be improved. [Click here to check out the final section of Edward’s talk](https://youtu.be/AhJmv1paL_w?t=2216) to hear about these areas and how Git can continue to evolve. Plus, you can see a live Q&A from the 2021 [GitKon Git conference](https://gitkon.com/). 

Take advantage now of all the work that Linus, Edward, and all the Git contributors have put into the most popular version control system and all the work the GitKraken team has done to make Git more usable through visualizations and a better UI by [downloading the GitKraken Git client](https://www.gitkraken.com/download) today, including a GUI and [CLI](https://www.gitkraken.com/cli)!