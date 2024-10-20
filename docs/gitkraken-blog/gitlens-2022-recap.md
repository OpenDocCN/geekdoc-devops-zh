# 2022 年的 GitLens:更多功能、更快速度、更强大

> 原文：<https://www.gitkraken.com/blog/gitlens-2022-recap>

新年快乐 GitLens 在 2022 年度过了忙碌而富有成效的一年，发布了两个主要版本，增加了几个全新的功能。让我们回顾一下过去 12 个月的亮点。但是首先要做的是…

## 感谢所有 GitLens+采用者

在我们回顾 2022 年之前，我们想花点时间感谢所有 GitLens+的早期采用者，感谢你们的承诺和热情。我们非常感谢您的支持，让 GitLens 继续成为 VS 代码市场上最好、发展最快的 Git 扩展。

随着 GitLens+和 GitLens for VS Code for the Web 的发布，2022 年对 GitLens 来说是辉煌的一年。我们刚刚跨越了另一个重要的里程碑—2000 万次安装！这证明了 GitLens 社区的力量，我们感谢你们所有人成为这一旅程的一部分。我们期待继续一起创造奇迹。接下来是 2022 年的集锦。

✨GitLens+特色发布会

随着 GitLens 12 的[发布，我们添加了一组全新的强大功能，增强您当前的 GitLens 体验，称为 GitLens+功能。我们引入的第一个 GitLens+特性是可视化文件历史视图和工作树。在发布的时候，这些功能需要一个免费的帐户来访问本地和公共托管的存储库，而私人托管的存储库则需要一个付费帐户。](https://www.gitkraken.com/blog/gitlens-12)

## 然而，当我们[在 10 月份发布 GitLens 13](https://www.gitkraken.com/blog/gitlens-13) 时——推出了漂亮的新提交图——我们希望每个人都能从本地和公共存储库中强大的 GitLens+功能中受益，而不会有任何摩擦。因此，我们消除了自由账户的障碍。现在，访问 GitLens+功能的唯一要求是拥有一个用于私人存储库的 Pro 帐户。

我们使用 GitLens+功能的目的是创建一个可持续的模型，使我们能够继续大力投资 GitLens 的免费和开源核心，这些功能可以提高工作效率、安全性、易用性等，同时创建一组专门为专业开发人员和团队设计的附加但完全可选的功能。

为了帮助您识别 GitLens+功能，它们标有✨symbol.

✨Commit 图

在 GitLens 13 中，我们从 GitKraken 客户端重新构思了强大而美丽的提交图，并将其引入 GitLens，甚至在此过程中对其进行了改进！易于使用的提交图帮助您可视化并跟踪所有正在进行的工作。您还可以快速查看提交之间的关系，查看提交细节，并通过全功能上下文菜单与分支、提交、标记等进行交互。这可以帮助您和您的团队更好地理解存储库的历史，并使识别变更、恢复错误以及与他人协作变得更加容易。

我们还将 GitLens 强大的提交搜索功能引入到提交图中，使其更易于访问和可视化。使用丰富的提交搜索来准确找到您想要的内容。其强大的过滤器允许您通过特定的提交、消息、作者、一个或多个更改的文件，甚至特定的代码更改进行搜索。

## 作为 GitLens+的一个特性，提交图对所有在本地和公共托管的存储库上工作的 GitLens 用户都是免费的，而私有托管的存储库需要一个 Pro 帐户。

网络上的 GitLens

在 GitLens 12 中，我们为 Web 添加了对 VS 代码的支持，这是一种完整的浏览器内代码编辑体验。这是一个巨大的努力，因为 GitLens 是围绕访问 Git 而构建的，这在 web 上是不可能的。但是能够通过 vscode.dev 和 github.dev 将 GitLens 的强大功能带给每个人是非常值得的。

GitLens for VS Code for the Web 旨在将 GitLens 的强大功能带给每一个人，扩展人们喜爱的功能，如内嵌责备、责备注释、热图和历史导航。GitLens 13 也把新的提交图带到了网上！

要获得 Web 体验的 VS 代码，只需导航到 github.com 上的一个存储库，然后按下`.`，你就会直接跳转到它。一旦你进入 VS 代码，从扩展视图安装 GitLens，就像你在桌面上一样，GitLens 将从此可用。

现在在网络上，当你在 github.dev 和 vscode.dev 上查看或编辑 GitHub repos 时，GitLens 已经覆盖了你。

## 特色亮点

提交详细信息

GitLens 12.2 中发布的提交细节视图提供了关于提交和隐藏的丰富细节。快速查看有关提交的作者、提交 ID、相关问题和拉请求的链接、更改的文件等信息。

当取消锁定时，提交详细信息视图上下文跟随您在编辑器中导航文件，以及在✨提交图、提交视图、存储视图、交互式 Rebase 编辑器和✨可视文件历史中选择提交和存储。您还可以从悬停、上下文菜单和快速选择菜单的提交详细信息视图中打开提交或存储。

✨Visual 档案历史

GitLens 12 中添加的另一个新的丰富的可视化特性是可视化文件历史，它提供了对文件如何随时间变化的洞察。可视化的文件历史视图允许您快速查看文件的演变，包括更改的时间、更改的大小以及更改者。使用它可以快速查找对文件进行最有影响的更改的时间，或者关于文件更改的最佳讨论对象等。

[](https://www.gitkraken.com/wp-content/uploads/2023/01/GitLens-Web.png)

✨Worktrees

## GitLens 12 还增加了对 Git 工作树的支持，允许您同时检查多个分支。工作树通过最小化分支之间的上下文切换来帮助您进行多任务处理，允许您轻松地同时处理存储库的不同分支。当需要查看拉取请求时，避免中断正在进行的工作。只需创建一个新的工作树，并在新的 VS 代码窗口中打开它，这一切都不会影响您的其他工作。

集成

### 我们在 GitLens 12 中添加了一些有价值的新集成。

GitLens 12.2 中发布的提交细节视图提供了关于提交和隐藏的丰富细节。快速查看有关提交的作者、提交 ID、相关问题和拉请求的链接、更改的文件等信息。

✨GitHub 企业和 GitLab 自我管理集成

使用 Pro 帐户，GitHub Enterprise 和 GitLab 自管理用户可以看到提交作者的头像、自动链接问题和拉请求的丰富细节，以及与提交和分支相关的拉请求。

### Gerrit 和 Google Source 集成

GitLens 社区成员(@felipecrs)和(@andrewsavage1)的贡献增加了对 Gerrit 和 Google Source 远程提供者的支持。

感谢 gittens 社区

### 特别感谢令人敬畏的 GitLens 社区和以下 GitLens 代码贡献者！

([@费利佩克斯](https://github.com/felipecrs))费利佩桑托斯

(“t0”@ mcy 村”t1)mcy 村

( [@Brcrwilliams](https://github.com/Brcrwilliams) )布莱恩·威廉斯

## ([@巴尼卡罗尔](https://github.com/barneycarroll))巴尼卡罗尔

( [@rasa](https://github.com/rasa) )罗斯·史密斯二世

( [@smartinio](https://github.com/smartinio) ) Sam Martin

### ( [@ amodudas](https://github.com/amouxaden)

泰勒·詹姆斯·莱恩哈特

( [@rzhao271](https://github.com/rzhao271) )雷蒙德·赵

### ( [@andrewsavage1](https://github.com/andrewsavage1) )

( [@ dimaulpov](https://github.com/dimaulupov) 德米特里·乌尔波夫

( [@markjm](https://github.com/markjm) )马克·莫里纳罗

## ([@ yr 胆小](https://github.com/yrtimiD))德米特里·古罗维奇

( [@alwinw](https://github.com/alwinw) ) Alwin 王

( [@adaex](https://github.com/adaex) ) Aex

([@ slavik-lvovsky](https://github.com/slavik-lvovsky)slavik lvovsky

( [@Git-Lior](https://github.com/Git-Lior) ) Lior Kletter

( [@ tamuratak](https://github.com/tamuratak) taki Tamura

展望未来

这是对我们在 2022 年发布的所有产品的快速回顾，但我们最兴奋的是我们在 2023 年及以后为您准备的所有产品。

我们正在开发并很快发布的首批产品之一是将 [Workspaces](https://help.gitkraken.com/gitkraken-client/workspaces/#focus-view) 的强大功能引入 GitLens。有了工作区，您将能够轻松地关注对您来说重要的问题和拉动式请求。

我们喜欢听 GitLens 社区；考虑您与我们分享的建议和想法。我们还计划更深入地参与 GitLens 社区，因为我们更多地“开放设计”——发布想法和模拟以获得早期反馈。提交图是我们的第一个实验，我们发现您的反馈非常有价值。我们的团队期待继续讨论和改进 GitLens 的想法。

编码快乐！

要了解更多 2022 GitKraken 产品亮点，请阅读我们在 [GitKraken 客户端](https://www.gitkraken.com/blog/gitkraken-client-2022-recap)和[吉拉 Git 集成](https://www.gitkraken.com/blog/git-integration-for-jira-cloud-2022)上的年度综述博文。

([@ dimaulpov](https://github.com/dimaulupov)Dmitry ulpov

( [@markjm](https://github.com/markjm) )马克·莫里纳罗

([@ yr 胆小](https://github.com/yrtimiD))德米特里·古罗维奇

( [@alwinw](https://github.com/alwinw) ) Alwin 王

( [@adaex](https://github.com/adaex) ) Aex

([@ slavik-lvovsky](https://github.com/slavik-lvovsky)slavik lvovsky

( [@Git-Lior](https://github.com/Git-Lior) ) Lior Kletter

([@ tamuratak](https://github.com/tamuratak)taki Tamura

展望未来

## 这是对我们在 2022 年发布的所有产品的快速回顾，但我们最兴奋的是我们在 2023 年及以后为您准备的所有产品。

我们正在开发并很快发布的首批产品之一是将 [Workspaces](https://help.gitkraken.com/gitkraken-client/workspaces/#focus-view) 的强大功能引入 GitLens。有了工作区，您将能够轻松地关注对您来说重要的问题和拉动式请求。

我们喜欢听 GitLens 社区；考虑您与我们分享的建议和想法。我们还计划更深入地参与 GitLens 社区，因为我们更多地“开放设计”——发布想法和模拟以获得早期反馈。提交图是我们的第一个实验，我们发现您的反馈非常有价值。我们的团队期待继续讨论和改进 GitLens 的想法。

编码快乐！

要了解更多 2022 GitKraken 产品亮点，请阅读我们在 [GitKraken 客户端](https://www.gitkraken.com/blog/gitkraken-client-2022-recap)和[吉拉 Git 集成](https://www.gitkraken.com/blog/git-integration-for-jira-cloud-2022)上的年度综述博文。

For more 2022 GitKraken product highlights, read our annual round-up blog posts on [GitKraken Client](https://www.gitkraken.com/blog/gitkraken-client-2022-recap) and [Git Integration for Jira](https://www.gitkraken.com/blog/git-integration-for-jira-cloud-2022).