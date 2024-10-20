# Git for Windows &跨平台协作的挑战

> 原文：<https://www.gitkraken.com/gitkon/git-for-windows>

[https://www.youtube.com/embed/tamENApCvs0?feature=oembed](https://www.youtube.com/embed/tamENApCvs0?feature=oembed)

视频

平台之间的协作很难。多个社区之间的合作更加困难。在 2021 [GitKon Git conference](https://gitkon.com/) 的演讲中， [Git for Windows](https://gitforwindows.org/) 的维护者 Johannes Schindelin 分享了他制作他所谓的“友好形式的 Git，修改后可以在所有版本的 Windows 上运行”的故事，以及他在这个过程中学到的一些经验。

**一个 Git 历史**

## 如果我们看一看 Git 的[历史，我们可以看到，版本控制系统是出于 Linux 项目的需要而诞生的，因此，Git 最初只是为 Linux 用户在 Linux 上运行而设计的。这种心态影响了最初 Git 团队做出的许多决策。](https://www.gitkraken.com/gitkon/history-of-git)

对其他平台用户来说不幸的是，这使得 Git 团队产生的代码假设某些底层工具会出现在他们试图运行 Git 的任何机器上。Git 的早期用户发现他们需要在 Linux 机器上安装 Perl、Bash 和 Python 等工具的最新版本，这样 Git 才能正确运行。虽然 Git 的大部分是用 C 语言实现的，C 语言是几乎所有系统上的通用语言，但是 Git 的很大一部分是作为 shell 脚本和 Perl 脚本运行的。例如， [Git merge](https://www.gitkraken.com/learn/git/git-merge) 动作作为 Python 脚本运行。

所有这些意味着 Git 还不能在 Windows 上运行。

And all this meant that Git could not yet run on Windows.

如果您正在寻找 Windows 上更好的 Git 终端体验，GitKraken Client 是不二之选。Git 增强的终端为 Git 命令、CLI diff 视图等提供了自动建议和自动完成功能。

If you’re looking for a better Git terminal experience on Windows, look no farther than GitKraken Client. The Git-enhanced terminal offers auto-suggest and auto-complete for Git commands, CLI diff view, and more.

Johannes 在 2006 年接受一份需要使用 Windows 的自由职业时发现 Git 不能在 Windows 上运行。作为一个忠实的 Linux 和 Git 社区成员，他不太乐意放弃他喜欢的工具链。使用 Windows 意味着他不得不使用 Apache Subversion 或微软的 Visual SourceSafe，他认为这两者都不如 Git。

而 Johannes 看到了一条帮助他弥合差距和改进工作流的途径，即使用 [git-svn](https://git-scm.com/docs/git-svn) ，这是 Subversion 库和 git 之间的双向操作。但是为了让他的计划成功，他首先需要让 Git 在 Windows 上运行。

While Johannes saw a path to help him bridge the gap and improve his workflows though using [git-svn](https://git-scm.com/docs/git-svn), a bidirectional operation between a Subversion repository and Git. But to make his plan work, he first needed to make Git work on Windows. 

**让 Git 在 Windows 上工作**

## Johannes 让 Git 在 Windows 上运行的第一条途径是利用 Cygwin，这是一个允许你在 Windows 上运行 Linux 软件的系统。Johannes 很快发现了 Cygwin 的一个问题:它编译的每个程序都必须使用 Cygwin 运行时，这是一个仿真层，运行这种仿真需要付出很高的速度代价。虽然它非常慢，但变通方法仍然可用，并允许 Johannes 获得 Git 的所有好处，如打补丁、 [Git rebasing](https://www.gitkraken.com/learn/git/git-rebase) ，以及预先检查每个单独的提交。

即使存在速度问题，Git 也能让 Johannes 更高效地工作。他的工作速度超过了经理的预期。他远远领先于利用他的工作的另一个团队，以至于他被要求暂停该项目的开发一段时间。这给了 Johannes 一个研究他的想法的机会:“移植 Git 在 Windows 上本地运行真的有多难？”这是他今天仍在进行的旅程。

The first path Johannes followed to make Git run on Windows was leveraging [Cygwin](https://www.cygwin.com/), a system that allows you to run Linux software on Windows. Johannes quickly discovered a problem with Cygwin: every program it compiles must use the Cygwin runtime, which is an emulation layer, and running this emulation comes at a steep price of speed. While it was very slow, the workaround was still usable and allowed Johannes to get all the benefits of Git, like patching, [Git rebasing](https://www.gitkraken.com/learn/git/git-rebase), and pre-reviewing each individual commit.

Even with the speed issues, Git allowed Johannes to work much more efficiently. He ended up working faster than his manager thought he should be able to work. He got so far ahead of another team that leveraged his work that he was asked to hold off on development for that project for a little while. This gave Johannes a chance to work on his idea: “how hard can it really be to port Git to run natively on Windows?”

This is a journey he is still on today.  

**将 Git 移植到 Windows**

## Johannes 被迫立即克服的一个障碍是直接从 Linux 移植 Git 根本不可能。Git 的很大一部分是作为 shell 脚本实现的，Windows 上没有本地 shell 脚本解释器。虽然他可以接近利用 [Git Bash](https://www.gitkraken.com/blog/what-is-git-bash) 的 [MSys/MINGW](https://www.msys2.org/) 端口，并让类似 [Git commit](https://www.gitkraken.com/learn/git/commit) 的命令工作，但像`git log`这样的许多其他命令实际上并不可用。

大约就在这个时候，Johannes 停止了为一家基于 Windows 的公司工作，本着真正的社区精神，他把他在 Git for Windows 中创建的东西交给了 Git 邮件列表。大约有半年时间，他没有听到任何回音；没有人接这个项目。但是有一天，他终于听说另一个 Windows 用户已经让他的代码工作了，而且那个用户也分享了他的补丁。其他 Windows 用户也开始尝试让他的代码工作，但几乎没有人能让所有的系统组件都按预期工作。

It was about this time that Johannes stopped working for a Windows-based company, and in true community spirit, he handed what he had created in Git for Windows off to the Git mailing list. And he heard nothing back for about half a year; no one picked up the project. But one day, he finally heard that another Windows user had gotten his code to work and that user too had shared his patches. Other Windows users started popping up who also attempted to make his code work, but almost no one could get all the system components lined up to make the code work as expected.  

**不同的社区，不同的心态**

## 几年后，有人联系 Johannes，让他加入微软，成为 Windows 开发人员的专业 Git。

After a couple of years, someone contacted Johannes about coming aboard as a professional Git for Windows developer inside Microsoft. 

他准备迎接新的挑战。他在微软遇到的人让他惊讶，不仅是他们的技术技能，还有他们对开源的思考方式。与微软的部分交易是，Git for Windows 将不是微软的产品，它将保持开源，但微软将向 Johannes 支付维护费用。微软对此非常高兴，因为他们想在开源社区尝试一种新的方法。

Johannes 于 2015 年 8 月开始在微软工作，距离开始 Git for Windows 项目整整 8 年。微软付钱让他专注于改进他的项目，他开始做一些已经搁置了很久的事情。例如，替换多年未维护的底层 Msys 系统，并在 [Git 库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)中支持不仅仅是 ASCII。Johannes 很快采用了新的 Msys2 运行时，该运行时得到了社区的积极支持。这一改变意味着，Git for Windows 第一次拥有了一个真正优秀的终端，你可以在那里调整和复制文本。Git for Windows 用户对这些更新非常满意！

事情就是这样开始的。但是，还有许多工作要做，前面还有许多挑战。

That was how it started. But there was a lot more work to be done and a lot more challenges ahead.  

GitKraken 是 Git 工具的一个完美例子，它已经发展到可以应对新的挑战，具有交互式拉请求管理、合并冲突检测和解决、深度链接等高级功能。

GitKraken is the perfect example of a Git tool that has evolved to meet new challenges, with advanced features like interactive pull request management, merge conflict detection and resolution, deep linking, and more.

**为 Windows 开发 Git 的挑战**

## **Challenges of Developing Git for Windows**

**POSIX 和 Windows**

### 任何操作系统的基础都是可移植的操作系统接口，或称 [POSIX](https://en.wikipedia.org/wiki/POSIX) ，它提供内部 API、外壳和实用程序接口。因为 Git 仅仅是为 Linux 构建的，所以开发人员从来不会费心为诸如文件访问之类的事情引入一个抽象层，而是假设所有用户都可以访问同一个 POSIX。

在 Windows 中，POSIX 子系统在某个阶段已经停止，取而代之的是，操作系统试图模拟与 Linux 相同的 POSIX，不是通过引入新的抽象层，而是通过在 Windows 之上模拟 POSIX APIs。这确实有效，提供了与 POSIX 相同的函数签名和功能，但代价是效率低下。有时，为了使调用工作，您必须采取原始请求甚至没有使用的额外步骤。对于单个调用来说，这是不好的，但是对于许多请求来说，系统消耗的资源比应该需要的要多。

In Windows, the POSIX subsystem had been discontinued at some stage, and instead, the OS tries to emulate the same POSIX as Linux, not by introducing a new abstraction layer, but by emulating the POSIX APIs on top of Windows. This does work, providing the same function signatures and functionality as POSIX, but comes at a price of inefficiency. Sometimes, to make a call work, you have to take additional steps that aren’t even used by the original request. For a single call, this is bad, but multiplied over many requests, the system consumes more resources than should be required. 

**进程与线程**

### 在 Linux 或 Unix 思维中，为每个任务生成一个全新的进程是很平常的事情。在 Linux 中，就机器资源消耗而言，新进程被视为“超级便宜”的操作。虽然在现实中，它们并不是超级便宜，只是在新过程开始时超级快。这是因为 Linux“作弊”，在进程的后面复制它需要的东西，使它看起来很有效率。然而，在 Windows 上，这样做既不便宜，也没有效率。

产生新进程的另一种方法是使用线程。线程具有线程间适当锁定的优势，因为它们可以访问完全相同的内存池。如果您需要并行化，或者跨多组资源执行代码，工作线程在资源消耗和内存使用方面实际上是更好的模型。线程在 Windows 上得到很好的支持，但在 Linux 上却不行。Linux 维护者最初认为线程是不必要的，并且花了几年时间来添加对它们的支持。即使在今天，在 Linux 上使用线程仍然不是一种非常流畅的体验，因此，并行化在 Git 中是一个真正的挑战。

例如，如果你对数百万个文件执行一个 [Git checkout](https://www.gitkraken.com/learn/git/git-checkout) ,你不可能真正拥有并行进程，因为那是超级昂贵的。这些不同的进程可以完全相互绊倒，霸占总线，而 CPU 只是等待，因为这些进程基本上阻碍了彼此的良好执行。有了线程，在可用资源之间划分任务来执行诸如 Git checkout 之类的事情就容易多了，对操作系统的开销也少了。

For example, if you perform a [Git checkout](https://www.gitkraken.com/learn/git/git-checkout) against millions of files, you can’t really have parallel processes because that’s super expensive. These different processes can completely stumble over each other, hogging the bus while the CPU just waits because the processes basically hinder each other from performing well.  With threads, dividing tasks among the available resources to perform things like Git checkout is all much easier to do, with much less overhead for the operating system.  

**明蒂不是巴什**

### Git for Windows 最初使用 [cmd](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/windows-commands) 作为其外壳，但现在使用 [Mintty](https://mintty.github.io/) 。虽然这一改变使得有效运行 Git 成为可能，但是也有一些折衷。Mintty 并不能完全替代 Bash。例如，任何颜色输出都需要额外的步骤，这增加了模拟与在 Linux 上使用 Git 和 Bash 相同的体验的操作开销。此外，不允许转义序列(允许光标位置控制和样式选项)。

**行尾**

最初，Windows 操作系统的前身 [DOS](https://en.wikipedia.org/wiki/DOS) 操作系统设置了许多 Windows 采用的标准，不允许除回车之外的行尾。如果你想到一个物理打印机，回车告诉机器回到行的开始，然后对齐饲料向前移动过程。随着开发人员转向终端和光标对齐，返回字符不再严格地意味着需要将光标返回到行首；你可以把光标放在那里。

Linux 和 Windows 在行尾的工作方式上有所不同。使用 Git for Windows 意味着您总是必须转换到 Unix 行尾。当二进制数据被误认为文本时，这就成了一个非常大的问题。数据被破坏，你的 jpegs 文件将无法加载。

### **符号链接**

符号链接是文件引用其他文件或目录的方式。当然，考虑到操作系统之间的其他潜在差异，Linux 和 Windows 处理这个问题的方式不同。在 Windows 上，符号链接需要指定链接指向什么:文件或目录。如果目标不存在，Windows 会很不高兴。另一方面，Linux 不需要指定这些信息。此外，符号链接或符号链接指向的东西可以改变，而不会使系统在 Linux 上无法操作。

### **路径问题**

Johannes 认为跨平台开发的最大挑战是路径问题。因为 Windows 上没有原生的 Unix shell，所以它使用 Msys2 Bash 工具，该工具只假装路径看起来像在 Linux 或 Unix 上一样。在 Linux 绝对路径中，文件和目录的完整路径以正斜杠开头。分隔符只是正斜杠，绝不是反斜杠。在 Windows 上，绝对路径以驱动器号开头，然后是冒号，最后是反斜杠。

在 Windows 上，你没有单一的根目录；每个驱动器都有一个根目录。Git 存在于 Linux 系统的根目录中，但是它存在于 Windows 中完全不同的地方。当您转到 Msys2 Bash 中的根目录时，它实际上会扩展到 Git 安装的 Windows 路径。为了支持 shell 脚本，Msys2 试图在 Windows 和类似 Unix 的路径之间动态转换，这在某些情况下可能会出错。例如，使用以正斜杠开头并被转换为 Windows 路径的正则表达式会导致一些重大问题。

### **跨平台协作就是跨文化协作**

虽然 Johannes 不知道确切的数字，但他认为 Windows 用户有 300 万到 1000 万 Git。约翰内斯只是为数百万人维护工具的一个人。🤯他总是在寻找那些想要克服技术挑战和跨文化障碍的贡献者来继续改进和发展 Windows Git。

协作很有挑战性，尤其是跨平台的协作，但最终，我们的软件会因此变得更好。

### **Cross-Platform Collaboration is Cross-Cultural Collaboration **

While Johannes doesn’t know exact numbers, he thinks there are between 3-10 million Git for Windows users out there. And Johannes is just one person maintaining a tool for millions. 🤯 He is [always looking for contributors](https://gitforwindows.org/#contribute) who want to overcome both the technical challenges and the cross-cultural hurdles to continue to improve and evolve Git for Windows. 

It’s challenging to collaborate, especially cross-platform but in the end, our software will be better for it.