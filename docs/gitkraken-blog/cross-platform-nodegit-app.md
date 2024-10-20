# 新的跨平台 NodeGit 应用

> 原文：<https://www.gitkraken.com/blog/cross-platform-nodegit-app>

在 Axosoft，我们总是在寻找很酷的新事物，这在我们每年的 30 天项目中达到高潮。在去年的 30 天项目中，我们一群人开始研究一种与 git 交互的新方法。

我们想要的东西之一是一个跨平台的工具。我们所有人都有不同的工作方式，没有人想牺牲我们工具的质量。所以我们决定尝试一些新的不同的东西；我们决定使用一些很酷的新技术来制作一个跨平台的 git 应用。我们研究了使用 [nw.js(以前的 node-webkit)](https://github.com/nwjs/nw.js/) 和 [Angular JS 构建 git 客户端。](https://angularjs.org/)我们需要的一个关键东西是一个跨平台的节点模块，它允许我们访问 git。

### **没有好的跨平台 git 选项…**

在我们为期 30 天的项目中，我们决定我们的应用不应该要求用户在他们的机器上安装 git。它应该完全独立于大多数 git 客户端所依赖的其他第三方应用程序(例如 windows 上的 msysgit/putty 和 OSX/Linux 上的 git/OpenSSH)。

我们认为最好的选择是使用 [libgit2](http://libgit2.github.com/) 。这是一个 100%跨平台的低级 C 库。不幸的是，这个库没有很好的移植到 Node.js。所有这些都需要一些额外的工作才能真正起步。我们决定继续进行我们应用程序的 UI 演示，如果有任何兴趣，我们会回来尝试解决更多的技术问题。

### 所有系统启动！

在为期 30 天的项目结束时展示了我们的 UI 演示后，我们得到了非常好的总体反应。所以 Axosoft 决定开始支持一些技术，我们需要这些技术来真正实现这样一个产品。

在浏览了 libgit2 的所有端口后，我们最终选定了 [NodeGit](http://www.nodegit.org/) 。我们联系了该项目的维护者[蒂姆·布兰尼恩](http://tbranyen.com/)，看看需要做些什么，我们能在哪些方面提供帮助。他指导我们帮助编写一些代码，这些代码使用来自 [libgit2 文档页面](https://libgit2.github.com/libgit2/#HEAD)的 [libgit2 JSON 文件](https://github.com/nodegit/nodegit/blob/7d2f3572194b8156e1bd61a6499e05ecc1a63aa8/generate/input/v0.22.1.json)自动生成包装 libgit2 库的 C++原生节点模块。

等待什么？！？

### 是的，NodeGit 自己生成。我们有一些代码，我们将把一些 JSON 放入其中，这些代码将产生 C++。很酷吧？我可以没完没了地谈论我们需要做的疯狂的事情，但我想我会留到另一个帖子。

Yep, NodeGit generates itself. We have some code that we’ll feed some JSON into that will spit out C++. Pretty cool huh? I could talk endlessly about the crazy stuff we needed to do to pull that off, but I think I’ll save that for another post.

**那么*到底是什么* NodeGit？**

### NodeGit 是一个异步的本地节点模块，允许您调用 libgit2。这意味着使用 NodeGit 的节点项目可以执行低级 Git 命令，而不需要对运行它的机器做任何假设！它甚至处理 SSH 凭证 auth！在 Windows 上！！！

您不需要安装 msysgit 或特定版本的 git。现在，所有这些都将在这个小巧的 libgit2 包装器中为您处理。不仅如此，它还超级快[(并将变得更快)](https://github.com/nodegit/nodegit/pull/478)更不用说，它不会阻塞 I/O。这意味着即使在克隆大型回购或做任何其他事情时，你的应用程序也能保持超级响应。

You don’t need msysgit or a specific version of git installed. Now that would all be handled for you in this neat little libgit2 wrapper. Not only that, but it’s super fast [(and going to get faster)](https://github.com/nodegit/nodegit/pull/478) not to mention, it doesn’t block for I/O. That means that your app stays super responsive even while cloning large repos or doing anything else.

**大牌已经在用了！**

### 任何一天，我们都有超过 80 个人在我们的 [gitter](https://gitter.im/nodegit/nodegit) 频道里闲荡。NodeGit 现在正在使用，或者计划在越来越多的项目中使用，包括微软 Azure、GitHub 的 Atom 编辑器、网飞、PayPal 等等。2015 年 3 月，我们从 NPM 下载了 7700 次，我们有望在 4 月份超过这个数字。我们是 libgit2 的第二大包装项目，仅次于 Rugged，这是 GitHub 的基础。

自从 Axosoft 几个月前决定支持 NodeGit 以来，node git 已经取得了很大的进步，我们出色的团队已经取得了很大的成就，对此我感到非常自豪。我认为这个项目真的可以推动更好的 git 集成，更完整的 git 客户端，并帮助社区创建利用 git 的惊人工具。我也很期待你能用它做些什么。

祝您黑客愉快，让我们知道您对我们新的跨平台 Git 客户端的看法！

Happy hacking and let us know what you think of our new cross-platform Git client [Axosoft GitKraken!](http://gitkraken.com/)