# Git 是如何工作的？了解 Git 内部是如何工作的

> 原文：<https://www.gitkraken.com/gitkon/how-does-git-work-under-the-hood>

[https://www.youtube.com/embed/gzJk1ruqSok?feature=oembed](https://www.youtube.com/embed/gzJk1ruqSok?feature=oembed)

视频

**Git 是什么？**

## Git 由 Linus Torvalds 于 2005 年创建，旨在维护 Linux 内核的开发。Git 建立在完全分布式、速度、简单设计、处理大型项目的能力和对非线性开发的强大支持的基础之上。围绕 Git 的[历史和它的创建存在着大量的传说，包括它为什么和如何得到这个奇特的名字，它最初创建的原因，等等。](https://www.gitkraken.com/gitkon/history-of-git)

让我们看看 Git 是如何工作的，以便更好地理解这个强大的版本控制系统(VCS)。我们将介绍什么是 Git 文件夹，它是如何在内部使用的，使用了什么数据结构，在执行常见操作时 Git 中实际发生了什么，以及如何从存储库中可能发生的特定数据丢失中恢复。

Let’s take a look at how Git works under the hood in order to better understand this powerful version control system (VCS). We’ll cover what the Git folder is, how it’s utilized internally, what data structures are used, what really happens in Git when performing common operations, and how to recover from specific data loss that may occur within your repositories.

“@GitKraken 使 Git 中难的或多余的部分变得简单。它没有隐藏 Git 概念。这真是太好了。”–[@德州](https://twitter.com/texastoland/status/1108080357247725569)

“@GitKraken makes the hard or redundant parts of Git easy. It doesn’t hide Git concepts. It’s as good as it Gits.” – [@texastoland](https://twitter.com/texastoland/status/1108080357247725569)

**了解 Git 文件夹**

## `.git`文件夹存在于你机器上的每个本地 [Git 库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)中。它包含多个用来跟踪你的代码库的文件。

**Git 挂钩**

当您初始化一个新的存储库时，会自动创建 Git hooks 文件夹。Git 挂钩是在事件之后执行的 shell 脚本，比如一个 [Git commit](https://www.gitkraken.com/learn/git/commit) 或 push。开发人员使用 Git 挂钩将用户功能注入常见的 Git 操作。有 17 种不同类型的 Git 挂钩可以实现这一功能。

例如，如果您想要使用一个 [Git 钩子来帮助您格式化一个提交消息](https://git-scm.com/book/en/v2/Customizing-Git-An-Example-Git-Enforced-Policy)以匹配您的团队所实施的一个标准模式，您将执行以下步骤:

## **Git Hooks**

导航到`.git/hooks`目录

移除。commit-msg Git 挂钩示例

通过运行``chmod +x prepare-commit-msg``确保准备-提交-消息正常工作

1.  **索引文件**
2.  在 Git 中，索引文件定义了临时区域。当您运行`git add`或`git rm`时，这一点清晰可见。如果您尝试使用`cat`或`HEAD`来“抓取”该文件，您将会看到大部分输出是乱码和不清楚的。这是因为存储在索引文件中的[信息是用二进制](https://git-scm.com/docs/index-format)表示的，这使得 Git 和计算机程序更容易阅读，但是人们理解起来有点困难。
3.  Ensure that the prepare-commit-msg works by running ``chmod +x prepare-commit-msg``

## Git 包含索引中每个文件的元数据，比如文件类型和其他相关信息。然而，Git 的索引不知道如何自己处理空文件夹。这就是为什么你通常要创建一个名为“Git keep”的文件。

**头文件**

头文件是 Git 文件夹中的另一个文件，是索引文件的附录。头文件跟踪当前签出的内容。

什么是裁判？

一个 [Ref 可以是三种不同事物](https://git-scm.com/book/en/v2/Git-Internals-Git-References)中的一种。

## **HEAD File**

HEAD——对您当前所在分支的象征性引用。包含指向另一个引用的指针。

remote——类似于分支，但是存储在 GitLab、GitHub 等任何云 Git 提供者中。

## 标签——类似于提交对象，但通常指向提交而不是树。

A [Ref can be one of three different things](https://git-scm.com/book/en/v2/Git-Internals-Git-References). 

**提交对象**

1.  提交对象是在门提交期间创建的。如果您运行`cat-file-p`命令，您可以看到它包含与提交相关的文件树的信息，包括存在什么文件、它跟踪哪个父提交、关于作者提交者的信息以及提交消息。
2.  所有提交对象共享一些公共属性；它们各自存在于一个对象的文件夹中，该文件夹存储在`.git`文件夹中，它们的所有路径都是它们的内容之一的阿沙，所有对象都用`zlib`压缩。
3.  提交对象有四种类型:

## tree–存储关于目录树的信息

提交-您的更改的快照

Blob-文件的内容

标记-指向 Git 历史中特定点引用

1.  GitKraken 客户端提供的易于阅读的提交图有助于您可视化所有类型的提交对象，以及分支结构和完整的提交历史，让您对项目有更多的控制和了解。

2.  **打包文件**
3.  packfile 是一个文件中许多对象的集合。虽然 packfile 本身没有用 zlib 压缩，但是里面存储的对象是压缩的。packfile 有一个扩展名为`.pack` 的文件，以及一个扩展名为`.idx`的补充索引文件。这个索引文件只是使从 packfile 中解包单个对象的速度更快。
4.  如果您访问 Git objects 目录，您可能会发现 packfiles 与对象放在一起。那些不属于该包文件的对象称为松散对象。松散对象可以在以后添加到 packfile 中，这通常是在运行`git gc`时自动完成的。只有在检测到相当大的回购规模并运行`git gc`后，才会触发该流程。

当您使用网络操作时，Packfiles 也会发挥作用。当您运行`git push`或`git pull`时，所有文件都被压缩到一个 packfile 中，以便通过管道传输。抓取克隆也是如此，因为在幕后， [Git 克隆](https://www.gitkraken.com/learn/git/git-clone)利用抓取。在这里你可以看到克隆和获取之间的[关系被演示](https://youtu.be/gzJk1ruqSok?t=929)。

**配置文件**

与提交对象不同，配置文件不是 zlib 压缩的，这意味着您可以以纯文本文件的形式查看它们。配置文件包含存储库设置、关于远程的信息以及关于被跟踪分支的信息。

## **Packfile**

**使用日志从错误中恢复**

我们都经历过不小心犯了不该犯的错误。假设你刚刚运行了一个`git log`,发现你做了一些你不打算做的事情。然后您运行一个`git reset hard`来删除该提交，但是意识到该提交还包括一个您仍然需要的重要文件。这些内容会永远消失吗？你又回到起点了吗？😅

幸运的是它不是。这就是我们的日志文件夹发挥作用的地方。每次更新 ref 时，都会更新日志文件夹。也就是说，每次您提交或以某种方式更改引用时，日志文件夹都会存储这些信息。使用日志文件，您仍然能够看到被删除的提交的对象 id。即使重置更新了磁头，它也没有删除本地对象。您现在可以 [Git checkout](https://www.gitkraken.com/learn/git/git-checkout) 提交并获取您需要的重要文件。

您还可以使用 Git 的`ref log`命令来访问这个日志功能。默认情况下，该命令将显示已检出内容的标题，但是您可以使用该命令选择其他引用，只需提供所需引用的名称。因此，如果您想查看对于另一个分支的引用，哪些提交已经被更改，只需运行`git ref log`并指向该引用。

## **Config File**

**恢复无法到达的物体**

虽然 Git reset 对于更新 ref 来说是一个有益的工具，但是需要注意的是它不会修改本地对象。这意味着即使你正在做一个 [Git rebase](https://www.gitkraken.com/learn/git/git-rebase) 或者一个 force push，你仍然能够在对象文件夹中跟踪你的对象。

## 使用 Git reset 可以使对象不再被 ref 引用。这些对象被称为不可及对象。`git gc`默认情况下，两周后删除这些对象，因此有一个短暂的窗口，在此期间数据可能是可恢复的。

您可以使用代表 Git 文件系统检查的`git fsck`来找到不可到达的对象。`git fsck`不显示在 logs 文件夹中跟踪的不可到达的提交对象，所以您需要在运行该命令之前临时删除该文件夹。这将显示任何未完成的不可到达的对象，并允许您访问它们。

你知道的越多

了解 Git 的来龙去脉会让你成为团队和雇主的宝贵财富。如果您想继续增长您的 Git 知识并添加更多有助于您成功的工具，只需看看现在带有 GUI 和 T2 CLI 的 GitKraken 客户端就可以了。GitKraken 客户端以其漂亮的提交图使编码变得简单和可视化，以其撤销错误的能力变得安全，并以其集成兼容性变得强大。

## **Recovering Unreachable Objects**

While Git reset can be a beneficial tool for updating a ref, it’s important to note that it doesn’t modify local objects. This means that even if you’re doing a [Git rebase](https://www.gitkraken.com/learn/git/git-rebase) or a force push, you’re still able to track your objects in the object folder. 

Using Git reset makes it so objects are no longer referenced by a ref. Those objects are called unreachable objects. `git gc` by default removes these objects after two weeks, so there is a brief window where the data may be recoverable.

You can find unreachable objects using `git fsck` which stands for Git file system check. `git fsck` does not show unreachable commit objects that are tracked within the logs folder, so you’ll need to temporarily get rid of that folder before running the command. This should reveal any outstanding unreachable objects and allow you to access them. 

## The More You Know

Knowing the ins and outs of Git makes you a valuable asset to your team and employers. If you want to continue to grow your Git knowledge and add more tools that will help you succeed, look no further than the [GitKraken Client](https://www.gitkraken.com/git-client), now with a GUI and [CLI](https://www.gitkraken.com/cli). The GitKraken Client makes coding easy and visual with its beautiful commit graph, safe with its ability to undo mistakes, and powerful with its integration compatibility.