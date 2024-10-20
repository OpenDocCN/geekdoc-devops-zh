# Linux 的最佳 GitLab 客户端|免费下载 GitKraken

> 原文：<https://www.gitkraken.com/blog/gitlab-client-linux>

这篇文章是一位客座作者写的。

## **GitKraken Git 客户端和 GitLab ==天造地设的一对**

在 DevOps 领域工作，我经常发现自己需要在相对较短的时间内交付一个特性或一个改进(基本上是一段代码)，甚至并行处理不同的任务。

一个相当通用的软件开发栈包括:

*   Linux 环境
*   一个版本控制系统(通常是 Git)和一个 Git 客户端(比如 GitKraken Git GUI
*   一个提供一些持续集成和部署特性的平台(这里是 GitLab)

Git 中的存储库和分支管理从未如此简单。利用 GitKraken 提供的可视化功能，获得对 Git 工作流的更多控制。

## **Git 中的存储库管理**

开始时，这似乎不是什么难以管理的事情，但是随着项目数量的增加，项目之间的技术堆栈也不同，维护和使用各种类型的设置往往变得相当难以管理，即使对于一个有经验的软件工程师来说也是如此。

对于初学者来说，这种情况可能会令人望而生畏。尽管我是 CLI 的忠实粉丝，但我可以诚实地承认，我一直被困在仓库地狱中，有无尽的 Git 仓库、远程分支和依赖项。

## **管理 GitLab 库的 Git 客户端**

这是我知道我需要一个 Git 客户机的时刻。在尝试了多个变体之后，我感觉最舒服的一个是 GitKraken，最棒的是它不需要任何依赖(甚至不需要 Git)；它只是直接与你的 GitLab 库一起工作。此外，GitKraken 是少数几个支持 Linux 的 Git 客户端之一。

首先引起我注意的是 GitKraken 直观的布局。一个具体的例子是查看所有远程分支的可能性，以及您可以轻松地[签出远程 Git 分支](https://www.gitkraken.com/learn/git/problems/git-checkout-remote-branch)。

在分布式环境中工作的一个普通任务是 [Git SSH](https://www.gitkraken.com/learn/git/tutorials/how-git-ssh-works) 密钥设置。跟踪 SSH 密钥并将它们添加到遥控器通常是团队中新成员的首要任务之一，这对于新手来说有时会很麻烦。

GitKraken 通过提供与 GitLab 的无缝集成，轻松解决了这个问题；不需要使用`ssh-keygen`，将公钥复制到您的远程存储库，或者管理~/。ssh/config。GitLab SSH 密钥配置完全由 GitKraken 管理。

## **GitLab 拉取请求**

另一个真正加速开发过程的很酷的特性是可以在 Linux 上的 GitKraken Git GUI 中快速创建 GitLab pull 请求。

对于一个新特性，通常过程包括[创建一个新的 Git 分支](https://www.gitkraken.com/learn/git/problems/create-git-branch)(通常来自开发分支)，添加所需的修改，将分支推送到您的远程，然后是一个合并请求。

所有这些任务都可以在几秒钟内从 Git 客户端执行，只需将一个分支拖放到另一个分支上，然后选择`Start a pull request`。使用 GitKraken 合并本地机器上的分支有相同的过程，只需拖放即可。

只要您连接到 GitLab 远程存储库，就可以直接在 Linux 上的 Git 客户机中方便地管理您的 [Git pull 请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)。

[GitKraken 的拉取请求功能](https://support.gitkraken.com/working-with-repositories/pull-requests/)与 GitLab 的集成非常好，它甚至允许用户通过在 GitLab 的合并请求视图的浏览器中打开一个新标签来打开和查看 GitLab 中的拉取请求。GitKraken 的 [GitLab 集成](https://www.gitkraken.com/integrations/gitlab)提供了跨所有操作系统的无缝体验，包括 Linux。

**GitLab 问题**

GitKraken 还为 GitLab 问题提供了一个特性，使团队能够使用 Linux 上的 Git 客户端进行协作和计划工作。

## **GitLab Issues**

GitKraken 完全支持与 GitLab 问题的集成，允许用户查看、更新、添加对某个 Gitlab 问题的评论，并创建与该特定问题相关的分支。

GitKraken 是唯一提供问题跟踪集成的 Git 客户端。这允许您减少上下文切换，节省您宝贵的编码时间。

**撤销命令行中的 Git 错误**

最后但同样重要的是，Git 最棘手的问题之一是撤销操作的过程。

从高层次的角度来看，Git 与以下三个主要的名称空间/区域“协同工作”:

## 工作目录(“未跟踪”区域)

索引(临时区域)

本地存储库(下的所有内容。git 目录)

*   Working directory (“untracked” area)
*   Index (staging area)
*   因此，假设您想要撤销最近所做的更改，例如[撤销 Git 提交](https://www.gitkraken.com/learn/git/problems/revert-git-commit#undo-git-commit)。虽然这看起来是一个简单的过程，但是会出现一些挑战，比如更改后的文件处于哪种状态？我们要撤消所有更改吗？对于一个相当简单的用例，比如更新一个文件(比如 Dockerfile)，需要执行一些命令。

那么，如果我们想撤销对 docker 文件所做的更改，会发生什么呢？从技术上来说，我们需要撤销我们的本地工作，更确切地说是撤销最后一次提交，并为了修改文件而对文件进行卸载。

这可以通过在 CLI 中使用以下命令来实现:

这些命令是准备提交文件并随后将其带回工作区所必需的。

更不用说后面的命令还穿插着`git status`命令(显示你所在区域的状态)。前一个场景实际上非常简单，因为在一个文件中只做了一个更改。这仍然需要在 CLI 中执行五个命令。😰

```
git add <FILE>

git commit -m '<COMMIT MESSAGE>’

git reset --soft HEAD^1

git reset HEAD <FILE>  

git checkout -f <FILE>
```

**使用 Git 客户端一键撤销 Git 错误**

如果我们在 Linux 上使用 GitLab 的 GitKraken Git 客户端来做同样的场景，这将是一个毫不费力的动作，不用说，我们将拥有所有 Git 区域的可视化表示。更容易跟踪文件的状态，并且在多个文件发生多次更改的情况下，灵活程度会更大。

## **Undo Git Mistakes in 1 Click with a Git Client**

**Git lab 和 Linux 的 Git 客户端**

随着我越来越多地使用 GitKraken，我发现它不仅仅是一个 GUI 这是一个成熟的工具，通过直观的设计和无缝集成提供了一种提高效率的方法。它的设计方式有助于用户理解 Git，并支持流畅的工作流。

## GitKraken 的撤销/恢复功能让您可以更好地控制高级 Git 操作，使它们更加安全，同时让您高枕无忧。🧘‍♂️

As I used GitKraken more and more, I discovered that it’s more than a GUI; it’s a fully fledged tool that provides a way to increase efficiency through intuitive design and seamless integrations. It’s designed in such a way that helps the user to make sense of Git and enables a fluid workflow.

GitKraken’s Undo/Redo gives you more control over advanced Git actions, making them more secure while giving you peace of mind.🧘‍♂️