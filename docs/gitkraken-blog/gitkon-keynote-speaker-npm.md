# 宣布 GitKon 主题演讲人:npm 和 GitHub 的 Ed Thomson

> 原文：<https://www.gitkraken.com/blog/gitkon-keynote-speaker-npm>

我们很自豪地宣布我们的第二位 GitKon 主题演讲人: [Edward Thomson](https://twitter.com/ethomson) ，GitHub NPM 的产品经理，libgit2 的维护者。

GitKon 是一个免费的虚拟 Git 会议，旨在将来自不同背景和关注领域的人们聚集在一起，相互学习。不管他们使用什么样的服务和框架，GitKon 的参与者都因为他们对 Git 的热爱以及 Git 如何帮助他们提高工作效率而团结在一起。

**libgit2**

Edward 维护着一个项目，该项目运行在世界上许多服务和工具下，包括名为 [libgit2](https://libgit2.org/) 的 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 。这是一个流行的用于与 Git 交互的开源库，但是它是完全独立的，这意味着不需要在 Git CLI 软件上安装它。

## 该项目将自己描述为“Git 核心方法的一个可移植的纯 C 实现，作为一个具有可靠 API 的可重入可链接库提供，允许您用任何支持 C 绑定的语言编写本地速度的定制 Git 应用程序。”

如果你曾经停下来想一想，像 [GitHub](https://www.gitkraken.com/integrations/github) 、 [GitLab](https://www.gitkraken.com/integrations/gitlab) 、 [Azure DevOps](https://www.gitkraken.com/integrations/azure-devops) 这样的 Git 托管提供商，除了很多本地工具之外，并不进行 CLI 调用。将请求解析成 CLI 命令、执行命令，然后将文本输出重新解析回产品或平台的用户界面，这将非常慢。相反，libgit2 用于支持这些请求，从而加快整个过程。每当你点击“合并拉取请求”时，这个软件就会执行 [Git 合并](https://www.gitkraken.com/learn/git/git-merge)。

libgit2 还允许开发者插入自己的函数，GitKraken 就是这样使用 [NodeGit 和 libgit2](https://www.gitkraken.com/blog/nodegit-libgit2) 实现“撤销”和“重做”的。

如果你曾经停下来想一想，像 [GitHub](https://www.gitkraken.com/integrations/github) 、 [GitLab](https://www.gitkraken.com/integrations/gitlab) 、 [Azure DevOps](https://www.gitkraken.com/integrations/azure-devops) 这样的 Git 托管提供商，除了很多本地工具之外，并不进行 CLI 调用。将请求解析成 CLI 命令、执行命令，然后将文本输出重新解析回产品或平台的用户界面，这将非常慢。相反，libgit2 用于支持这些请求，从而加快整个过程。每当你点击“合并拉取请求”时，这个软件就会执行 [Git 合并](https://www.gitkraken.com/learn/git/git-merge)。

“谢谢@GitKraken 的撤销功能，不小心丢弃了我所有的更改，但现在它们又回来了:D”–

可以想象，将 Git 工作的协议翻译成 libgit2 并不是一件容易的事情。Edward 对全球团队如何制定代码以及他们在维护代码时面临的挑战提出了独特的见解。这是一项特别困难的任务，因为 Git CLI 团队从未停止更新该软件的功能。

**GitHub 动作**

除了他在这个项目上的工作涉及到互联网的方方面面，Edward 还是 GitHub 的一名产品经理。他是 2018 年将 GitHub Actions 带入生活的团队的一员，现在，爱德华负责监督 npm。

## GitHub Actions 允许 GitHub 做的不仅仅是处理源代码管理托管。动作最初旨在让开发人员自动执行常见的代码任务，比如在启动拉请求时自动添加标签。虽然 GitHub 长期以来一直支持 [webhooks](https://github.blog/2014-02-11-webhooks-level-up/) 的概念——它们是等待特定事件触发动作的监听器——但开发人员需要自己设置和管理任何由 webhooks 触发的所需应用程序。

随着 GitHub Actions 的引入，这些应用程序可以在 GitHub 虚拟环境中运行，使 GitHub 成为开发人员真正完整的平台，无论您选择什么语言或框架。该项目一直在发展，现在是开发人员进行持续集成的最流行的方法之一。

**npm**

最近，Edward 开始监督 JavaScript 包之家 npm，自从 npm 于 2020 年被 GitHub 收购后，他就一直在从事这个项目。

对于可能不熟悉这项技术的人来说，npm 代表“节点包管理器”,它是节点生态系统中所有可用软件包集合的注册表。该团队还负责维护 npm 命令行界面本身。对于开发 JavaScript 应用程序的开发人员来说，这意味着他们可以通过调用包名来安装数千个包。

## 例如，如果您想安装 ESLint，这是一个非常流行的静态代码分析工具，用于识别 JavaScript 代码中的问题模式，您只需要在终端 [install npm](https://eslint.org/docs/user-guide/getting-started) 中键入`npm install eslint`。npm 帮助加速了许多项目和产品的开发，比如 GitKraken 本身。

Edward 正带头通过投资注册管理机构基础设施和平台来改善核心体验并更好地与机构群体互动，从而使 npm 变得更好。你可以在 GitHub 博客上阅读更多关于他目前升级 npm 安全性的工作。

对于可能不熟悉这项技术的人来说，npm 代表“节点包管理器”,它是节点生态系统中所有可用软件包集合的注册表。该团队还负责维护 npm 命令行界面本身。对于开发 JavaScript 应用程序的开发人员来说，这意味着他们可以通过调用包名来安装数千个包。

**团结在 Git 的旗帜下**

GitKon 就是让人们聚在一起互相学习。我们肯定会学到一些很酷的技巧和诀窍，但最终希望每个人都能带着更大的感激离开，不管我们的专业领域如何，我们都有很多共同点。任何人对 Git 的热爱背后都有一种团队合作的热情，这种热情赋予了我们力量。

除了任何单一的平台或代码方法，我们相信每个开发人员都有帮助塑造他们周围世界的愿望。通过共同探索新的方法和理念，我们可以更聪明地工作，更有成效。通过分享我们的经验，我们希望拓宽对合作意义的理解。我们知道像 Edward 这样的演讲者会激励你，但我们也希望你能来参加 GitKon，分享你的想法和经历。我们知道，作为团体和团队，我们可以一起做很多精彩的事情。通过利用更好的工具、流程、工作流程和想法，我们可以更快地完成工作，获得更好的结果，并尽可能保持透明。

## **团结在 Git 的旗帜下**

**注册免费虚拟 Git 大会 Git kon**

GitKon 2021 团队非常高兴有 Edward 作为主题演讲人，他不仅代表 libgit2，还代表 GitHub，GitHub 是目前全球超过 90%的开发人员使用的平台！

你不想错过这个独特的虚拟事件。与会者注册已开放，请保留您的免费门票，以确保您不会错过 2021 年 9 月 22 日至 23 日举行的这一令人难以置信的活动。

## **注册免费虚拟 Git 大会 Git kon**

GitKon 2021 团队非常高兴有 Edward 作为主题演讲人，他不仅代表 libgit2，还代表 GitHub，GitHub 是目前全球超过 90%的开发人员使用的平台！

你不想错过这个独特的虚拟事件。与会者注册已开放，请保留您的免费门票，以确保您不会错过 2021 年 9 月 22 日至 23 日举行的这一令人难以置信的活动。