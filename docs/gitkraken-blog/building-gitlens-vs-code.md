# 为 VS 代码构建 git lens | Eric Amodio

> 原文：<https://www.gitkraken.com/gitkon/building-gitlens-vs-code>

[https://www.youtube.com/embed/GAmZgzqJKgY?feature=oembed](https://www.youtube.com/embed/GAmZgzqJKgY?feature=oembed)

视频

您准备好增强 Visual Studio 代码的 Git 功能了吗？无缝导航 Git repos，让代码作者一目了然。

[Install GitLens Free](/gitlens)

在 2021 [GitKon Git conference](https://gitkon.com/) 上，Eric Amodio 分享了他构建 GitLens 的旅程，Git lens 是一款 VS 代码应用，全球有超过 1000 万开发者使用。

很久以前，Eric 在一家公司工作，为 web 开发构建所见即所得的应用程序。虽然他有一些版本控制的经验，使用过像[微软 Visual SourceSafe](https://en.wikipedia.org/wiki/Microsoft_Visual_SourceSafe) 、 [Apache Subversion、【SVN】](https://subversion.apache.org/)和 [Borland 的 StarTeam](https://www.microfocus.com/en-us/products/starteam/overview) 这样的工具，但是他加入的团队没有在他们的项目中使用过源代码控制。相反，他们只是手动共享文件。

随着埃里克的加入，他推动他们采用 SVN，这是他们的斗争。与此同时，Git 刚刚出道，开始成为一个成熟的玩家。随着时间的推移，他和他的团队研究了很多分布式配置管理工具，但是当他们比较了 Git 和 SVN 的时候，他们发现 Git 成为了事实上的领导者。Git 是开源的，并且有一个很好的社区围绕着它，这一事实促使他们从 SVN 迁移到 T2 的 Git T3。虽然有很高的学习曲线，但是 Git 的强大和多功能性让他们很高兴采用了它。

## **遇到 VS 代码**

Eric 最初是一名开发人员，在学校学习汇编语言，这让他对编程失去了兴趣。然后，他发现了[微软 Visual Basic](https://docs.microsoft.com/en-us/dotnet/visual-basic/) 并坠入爱河。他觉得自己有能力迅速将事情变成现实，将伟大的想法变成现实。

从那里，他继续前进。NET，看着[ide](https://en.wikipedia.org/wiki/Integrated_development_environment)，像 [Visual Studio](https://visualstudio.microsoft.com/) 越来越重，越来越大。在涉猎了 Sublime Text 和其他 ide 之后，VS Code 首映；埃里克认为它很美。VS 代码部分的开放性意味着他可以打开[开发工具](https://www.gitkraken.com/reports/best-developer-tools-2021)并调整内部。

在早期，Eric 发现了 VS 代码的一些文件共享方面的问题。他能够自己调试问题，并给 VS 代码团队发了一封电子邮件，附上他的补丁。就在第二天，他的补丁出现在了 VS 代码发布中，这是他与 VS 代码社区合作的第一次正面体验。

不久之后，VS 代码完全开源并引入了扩展模型。这是 Eric 最感兴趣的地方，因为不必改变整个应用程序来做你想做的事情，比如使用 TypeScript，你可以逐渐地添加东西。基本上，代码会在你所在的地方遇到你。

## 【GitLens 的创意

Visual Studio 有一个名为 CodeLens 的系统，它在 IDE 中的代码上方给出很少的注释。这些注释描述了代码和其他各种元数据的作者。VS Code 公开了 CodeLens API，给了 Eric 尝试各种可能用例的机会。他的第一个想法是看看是否可以从 Git 获得信息，并在 VS 代码中显示。这就是 [GitLens](http://gitkraken.com/gitlens) 如何开始的，只是玩玩 Git 和 VS 代码，看看什么是可能的。

GitLens 的口号是:“GitLens 增强了 Visual Studio 代码中内置的 Git 功能。”VS 代码有自己内置的 Git 提供程序，支持查看和提交您的更改。您还可以创建、查看和切换分支，但是总的来说，在执行 Git 操作时，VS 代码相当有限。

Eric 想在 VS 代码中直接公开一个文件的 Git 历史。对于许多开发人员来说，行内代码注释基本上是不存在的。开发人员很少在代码库中添加对他们工作的解释来解释他们的选择和工作。它让合作者猜测为什么做出某些选择，以及某些代码行做了什么。然而，这些开发人员经常会留下像样的 Git 提交消息。回溯历史，看看事情是如何演变成现在这个样子的能力是非常强大的。如果您不知道事情是如何或为什么发展到某一点的，那么错误和问题的风险就会增加。

更好地理解您的代码。GitLens 提供了 VS 代码中遗漏的 Git 回复的可视化。

[Install GitLens Free](/gitlens)

这种历史资料传统上是很难挖掘出来的。Eric 希望释放 Git 存储库内部的能量，并获得这些有用的信息，将其带到最前沿。这是 GitLens 的主要目标。为了使可视化您的 [Git 库变得非常容易，](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)无缝地导航您的项目历史，并且能够更容易地定位诸如特定提交之类的事情。Eric 真的想帮助开发人员理解为什么事情会变成现在这样，以及它们是如何随着时间的推移而改变的。

这一切都始于一个简单的问题:有人可以通过 CodeLens 对任何文档进行洞察吗？例如，是否有人可以打开一个文件，查看谁修改了文件，并准确显示出发生了什么变化，以及为什么发生了变化？Eric 认为，如果 API 允许他在一行代码周围公开 Git 提交消息，那么 GitLens 就可以真正赋予开发人员权力。例如，如果您正在调试您的应用程序，并且看到一个两年都没有改变的函数，这就给了您一个线索，也许这不是寻找新出现的问题的原因的地方。但是如果一个函数昨天发生了变化，也许那是开始调试新问题的正确地方。

## **GitLens 和解决问题**

因为您通过 VS 代码与 GitLens 交互，所以您不必从编辑代码切换到与 Git 存储库交互。所呈现的信息与您正在处理的代码行是上下文相关的。无论你的光标在屏幕上的哪个位置，都会显示谁最后修改了选中的行，同时显示该行的 [Git Diff](https://www.gitkraken.com/learn/git/git-diff) ,并显示它是多久前被修改的。

所有相关的历史信息都在 VS 代码中，这意味着您不必去寻找它们。它很大程度上是不引人注目的，留在后台，直到你真的想要它。当你真的需要它的时候，它很容易浮出水面，在你所在的地方与你相遇，把你的历史的上下文视图带到你正在工作的文件中的代码。Eric 还想构建一个更丰富的客户端来洞察整个存储库，让你能够看到自己的分支，并深入到任何一个 [Git 分支](https://www.gitkraken.com/learn/git/branch)中，查看变化。他希望能够实时查看更改并比较提交，将众多更改浓缩起来，只查看分支中发生更改的文件集。这让您可以快速查看和检查随着时间的推移发生了什么变化。

GitLens 为 VS 代码添加了强大的功能，允许您比较存储库中的任意两点，而无需切换上下文。

## **GitLens 和 Git 合并冲突**

Eric 的动机是让 [Git 合并冲突更容易解决](https://www.gitkraken.com/learn/git/tutorials/how-to-resolve-merge-conflict-in-git)。虽然 VS 代码确实为合并冲突提供了一些帮助，但是还可以做得更多。目前，在冲突的情况下，无论是来自[合并还是](https://www.gitkraken.com/learn/git/problems/git-rebase-vs-merge)重置，都没有足够的默认上下文。GitLens 为每个文件提供了信息，让你在发生 [Git 合并](https://www.gitkraken.com/learn/git/problems/merge-git-branch#:~:text=How%20do%20you%20merge%20a%20Git%20branch%20with%20master%20in,master%20from%20the%20context%20menu.)之前很容易看到不同之处。它可以很容易地看到提议的更改，只需拉进解决给定问题的个别行。

如果您现在需要帮助解决合并冲突，GitKraken 可以通过有用的可视化上下文帮助您完成解决过程。更不用说:如果你和一个团队成员同时处理同一个文件，GitKraken 会提醒你潜在的合并冲突。🤯

## **git lens 的未来**

GitLens 的未来是让 Git 的难用但[强大的特性更容易使用，比如合并冲突解决和查看分支之间的差异。GitLens 能够可视化这些特性，但是只能在树形视图中，与你已经在看的地方分开。试图将两种不同的观点联系起来可能会导致一点认知上的不协调，所以 Eric 的目标是将这些观点拉近，以进一步减少上下文切换。](https://www.gitkraken.com/git-client/powerful-git-features)

GitLens 有一些按钮，可以让你浏览一个回购的历史，并查看以前的提交。但是，如果您不确定到底要搜索哪个提交或特定的代码行，这可能会令人困惑。在未来，Eric 希望 GitLens 展示一些比之前的提交更有意义的东西。这可能包括像时间线这样的东西，在那里你可以看到对文件进行某些更改的时间点的“大气泡”。理想情况下，这将让你快速识别并在最感兴趣的点之间跳转。

GitLens 也在努力整合工作树。这目前是 Git 的一个迟钝的特性，但是在适当的情况下，可能非常有价值。如果 GitLens 可以提供多种视图，并在它们之间无缝切换，那么当你想同时处理多件事情时，你可以隐藏你的更改并切换分支。

这对开发人员来说是一个难题，他们需要暂停，然后隐藏或重置他们当前的工作，去修复一个 bug 或其他出现的问题。

## **GitLens 功能更多人应该利用**

在 GitLens 中，比较非常重要，但在 Eric 看来，这一点没有得到充分利用。能够看到不同分支或不同时间点之间的比较，并快速查看任何一组变更是非常强大的。

当查看 [Git pull 请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)时，您可以查看 pull 请求中的每个单独的提交，或者您可以查看作为一个整体被更改的文件集。在 GitLens 中，您可以通过比较任意两点来设置更改后的集合视图的特定版本。从单个视图中，您可以看到提交和包含提交的更改的文件集。

GitLens 还有一个 Git 命令调色板。VS 代码有一个命令面板，您可以在其中打开和执行 VS 代码命令。GitLens 有一个 Git 命令面板，可以让你调出并执行 [Git 命令](https://www.gitkraken.com/learn/git/commands)，提供引导式体验。例如，如果你想[创建一个 Git 分支](https://www.gitkraken.com/learn/git/problems/create-git-branch):

打开命令选项板

1.  键入并选择`git branch`，然后选择`create`选项。
2.  选择您要从哪个回购和哪个分支创建新分支
3.  添加新分支的名称，VS 代码会为您创建它。
4.  Add the name of the new branch and VS Code creates it for you.

GitLens 命令面板提供了一个很好的 Git 命令向导，展示了不同的可用选项。

The GitLens command palette gives a nice guided tour through Git commands exposing the different options available.

**为 VS 代码构建 git lens**

## VS 代码 API 被有意锁定，以确保它是健壮的，经得起时间的考验，不会对性能造成负面影响。这些年来，VS 代码团队已经花了很多时间来扩展 API 的功能，并且接口变得越来越容易使用。现在，您可以托管 web 视图并自己控制 UI 和 UX，超越最初提供的锁定视图和代码。

在开始使用 VS 代码时，一定要意识到可能会有一定的挑战。API 的功能仍然有限制。在开始时，您应该检查您感兴趣的领域是否已经存在 API。这可以帮助您避免对 API 如何完成某些事情的误解感到沮丧。由于 API 的锁定特性，它的表面积较小，因此您必须在组合构件的方式上有所创新。一旦你完全理解了事物真正是如何工作的以及极限在哪里，你就可以构建一些真正有趣和强大的东西。

API 拥有强大的工具来构建像 VS 代码文件系统抽象这样的东西。这些抽象让您可以构建真正丰富的元素，这正是 GitLens 在 Git 之上构建文件抽象所依赖的。通过访问特定的结构化 URI 来查找历史中的特定文件，GitLens 可以利用整个文件系统。这些 API 内置了很多功能，让您可以想象任何事情，并且约束非常有助于帮助您理解您可以做什么。它们还确保您实现的东西是好的和高性能的，让您一路思考 UX。

The API has powerful tools for building things like the VS Code file system abstractions. These abstractions let you build really rich elements, which is what GitLens relies on to build the file abstraction on top of Git. By going to a specific, structured URI for a particular file in history, GitLens can then leverage the whole file system. There’s a lot of power built into these APIs that let you envision just about anything, and the constraints are very helpful to help you understand what you can do. They also ensure that what you implement is nice and performant, making you think about the UX along the way.

**参与 GitLens 和 VS 代码**

## 对于那些希望加入 GitLens 和 VS 代码社区的人，Eric 为扩展作者运营了一个 [Slack 社区](https://aka.ms/vscode-dev-community)。欢迎所有人加入，提出问题，并会见其他作者，这是开始 VS 代码贡献并找到您想要做的事情的可能示例的好方法。问问题和使用 VS Code API 是找到方法的好方法，因为它非常容易访问、调试和使用。

甚至有一个用于 VS 代码的[自耕农生成器](https://yeoman.io/)，它将为你搭建一个项目。这让您可以快速完成设置，并且可以很容易地帮助调试和测试。

For those looking to get involved in the GitLens and VS Code community, Eric runs a [Slack community](https://aka.ms/vscode-dev-community) for extension authors. All are welcome to join, ask questions, and meet other authors, which is a good way to get started with VS Code contributions and find possible examples of what you want to do. Asking questions and playing around with the VS Code API is a great way to find your way, as it’s pretty accessible, easy to debug, and use.

There is even a [Yeomen generator](https://yeoman.io/) for VS Code that will scaffold out a project for you. This lets you quickly get through setup, and can help debug and test things out quite easily. 

**Git 的未来是可见的**

## Eric 认为，Git 的未来是更好的可视化。你可以 **[跳到他演讲的最后](https://youtu.be/GAmZgzqJKgY?t=1871)** 去听更多他对 Git 和整个科技行业未来的想法。

在 GitKon 之后不久， [GitKraken 收购了 GitLens](https://www.gitkraken.com/blog/gitkraken-acquires-gitlens-for-visual-studio-code) ，Eric 加入 GitKraken 担任新的首席技术官，领导继续开发 **[GitLens](http://gitkraken.com/gitlens)** 和 GitKraken 广受欢迎的 **[Git 客户端](http://gitkraken.com/git-client)** 。通过与 GitKraken 合作，Eric 现在全职专注于 GitLens，GitLens 获得了一家领先软件公司的全力支持和资源，该公司专门为开发团队提供 Git 协作和生产力解决方案。

下载免费的 **[GitKraken Git 客户端](https://www.gitkraken.com/git-client)** 以利用桌面上的可视化 Git GUI 和 **[CLI](https://www.gitkraken.com/cli)** 的功能，并且 [**安装** **GitLens**](https://gitlens.amod.io/) 以免费访问 VS 代码中 Git 的功能。

Download the free **[GitKraken Git client](https://www.gitkraken.com/git-client)** to leverage the power of a visual Git GUI and **[CLI](https://www.gitkraken.com/cli)** on your desktop, and [**install** **GitLens**](https://gitlens.amod.io/) for free to access the power of Git in VS Code.