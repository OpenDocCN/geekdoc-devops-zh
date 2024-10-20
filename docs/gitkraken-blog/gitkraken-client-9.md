# 告别回购轮盘赌:发现 GitKraken v9.0 并简化您的开发工作流程

> 原文：<https://www.gitkraken.com/blog/gitkraken-client-9>

我们知道在多个回购中处理进行中的工作、拉式请求和问题分支是多么乏味。这就是为什么我们如此兴奋地推出 [GitKraken 客户端 9.0](https://help.gitkraken.com/gitkraken-client/current/#version-9-0-0) 。这个主要版本的发布将真正改变您的开发工作流程。

以下是头条新闻:

*   新的本地工作区使处理多个回购变得轻而易举，无论它们托管在哪里。
*   工作区定制选项的增强菜单，如添加或删除表列。
*   整个应用程序的显著界面和可用性升级旨在提高易读性，促进更好的定制，并使查找和关注提交图中的分支和提交变得更加容易。
*   最后，但绝对不是最不重要的，GitKraken Insights 的预览，这是一个全新的工具，用于监控拉请求的速度，以缩短个人和团队的开发周期。

工作空间改进

## 对于每天使用多个存储库的开发人员来说，工作区已经帮助他们快速回答了如下问题:

我昨天在做什么？上周呢？

*   哪些回购正在进行中？
*   接下来我需要做什么？
*   正如一位用户所说:

*“我真的很期待让 GitKraken Client 成为我的‘早晨咖啡登陆页’目标是能够在 40 多个存储库中找出什么是可操作的，以及以什么顺序。"*

正如一位用户所说:

以前版本的工作区要求存储库将远程主机托管在一个主机服务上，这使得那些在本地工作或使用非集成服务的人无法使用它们。有了 9.0，我们很高兴地宣布，您现在可以创建本地工作区，包括您机器上的任何存储库，而没有任何远程限制。

> *“我真的很期待让 GitKraken Client 成为我的‘早晨咖啡登陆页’目标是能够在 40 多个存储库中找出什么是可操作的，以及以什么顺序。"*

要创建一个本地工作区，导航到 GitKraken 客户端左上角的工作区选项卡并点击`New Workspace`。

通过选择一个存储库目录，甚至通过选择一个现有的 VS 代码工作区，浏览并添加存储库。创建本地工作空间后，您将获得以下好处:

点击任意回购名称，在 GitKraken 客户端中以标签形式快速打开。

查看每个回购的当前签出分支及其远程状态。

查看所有回购中正在进行的工作，以及有多少修改、添加或删除的文件。

*   查看回购详细信息，包括自述文件的渲染视图。
*   多选回购以:
*   对所选的仓库执行提取
*   在 GitKraken 客户端或 VS 代码工作区中以标签形式打开 repos
*   使用您的选择创建云工作区(以前称为个人或团队工作区)
    *   对所选的仓库执行提取
    *   本地工作区也可以用于创建云工作区。云工作区增强了所有回购的拉式请求和问题信息，并可与您的团队共享，以保持每个人的一致性。
    *   使用您的选择创建云工作区(以前称为个人或团队工作区)

云工作空间也将支持 GitKraken Insights。说到这个…

git kraken Insights–预览

接下来，我们很高兴向您介绍 GitKraken Insights，这是一项全新的功能，可以帮助您和您的团队测量工作空间中所有 repos 的启动和合并速度。

云工作空间也将支持 GitKraken Insights。说到这个…

## 要启用 GitKraken Insights，首先需要打开一个云工作区，然后导航到`Pull Request`部分。从这里，单击将 Insights 连接到您的远程托管服务。

接下来，我们很高兴向您介绍 GitKraken Insights，这是一项全新的功能，可以帮助您和您的团队测量工作空间中所有 repos 的启动和合并速度。

连接完成后，返回到云工作区的`Pull Request`部分，查看工作区拉取请求的以下指标:

平均周期时间:衡量在所选时间范围内合并拉取请求所需的平均时间。

平均吞吐量:衡量所选时间范围内合并的拉请求的平均数量。

合并率:选定时间范围内合并的拉式请求与未完成的拉式请求的百分比。

打开:在选定时间范围内打开的拉请求总数。

*   合并:在所选时间范围内合并的拉请求总数。
*   平均吞吐量:衡量所选时间范围内合并的拉请求的平均数量。
*   但是，如果您跟踪像拉式请求周期时间和吞吐量这样的指标，这又有什么关系呢？
*   打开:在选定时间范围内打开的拉请求总数。
*   基本原则是，你的代码被合并的时间越长，你的工作流就会变得越复杂。因此，随着这些变更落地，您的 PR 和您的变更越来越落后于主干分支，您就越有可能需要做更多的工作来让代码再次工作。”

GitKraken Insights 目前正在预览。随着时间的推移，我们将向所有用户推出这一功能，我们希望听到您对应用内表单的反馈。

用户界面/UX 刷新

> GitKraken 最近在 VS 代码中发布了 GitLens 的提交图，我们很高兴能在 GitKraken 客户端 9.0 版本中引入该流程的一些改进。
> 
> *– Jeff Schinella, Director of Product, GitKraken*

幽灵树枝

在 GitKraken 客户端 9.0 中，当您将鼠标悬停在提交上时，您将会看到一个“幽灵”分支。这将显示包含该提交的最近的分支，如果指向分支头的指针在视图之外，这将很有帮助。“ghost”分支也将在选择提交时显示，双击该 ghost 分支将检查引用分支的头。

用户界面/UX 刷新

## 用户可以从`Preferences > UI Customization`开始打开或关闭该设置。

提交突出显示

我们要分享的另一个改进最初是在 GitLens 中首次亮相的。

### 将鼠标悬停在任何分支上，短暂延迟后，应用程序将突出显示该分支引用的所有提交，帮助您快速关注与该分支相关的更改。

将鼠标悬停在任何分支上，短暂延迟后，应用程序将突出显示该分支引用的所有提交，帮助您快速关注与该分支相关的更改。

与 ghost 分支类似，用户可以从`Preferences > UI Customization`开始打开或关闭该设置。

用户界面刷新

我们也很高兴地分享这个版本对 GitKraken 客户端界面的许多改进。听取客户反馈，我们努力减少应用程序中的视觉噪音，最大限度地减少之前定义内容区域的不同颜色的使用。9.0 现在更依赖于间距和排版来增加易读性。如果你使用自定义主题，你会注意到一个变化，但不会破坏现有的主题系统。

### 我们要分享的另一个改进最初是在 GitLens 中首次亮相的。

将鼠标悬停在任何分支上，短暂延迟后，应用程序将突出显示该分支引用的所有提交，帮助您快速关注与该分支相关的更改。

我们还更新了 Mac 应用程序图标，以符合当前的苹果指南，Windows 和 Linux 应用程序图标也已刷新。我们希望你喜欢新的外观！

GitKraken Client 9.0 是一个改变游戏规则的版本，它提高了产品的声誉，使开发工作流程更简单、更快速。包括工作区和洞察力在内的新功能既实用又具有前瞻性，它们反映了 GitKraken 致力于构建满足开发人员和团队需求的工具。首席执行官 Matt Johnston 将此次发布描述为“真正改变了开发工作流程”，并将开发从个人运动发展为团队运动。总的来说，GitKraken Client 9.0 是任何希望改善其开发工作流程并领先于未来挑战的人的必备工具。

与 ghost 分支类似，用户可以从`Preferences > UI Customization`开始打开或关闭该设置。

体验工作空间、洞察力和 GitKraken Client 9.0 的所有改进。

用户界面刷新

### 我们也很高兴地分享这个版本对 GitKraken 客户端界面的许多改进。听取客户反馈，我们努力减少应用程序中的视觉噪音，最大限度地减少之前定义内容区域的不同颜色的使用。9.0 现在更依赖于间距和排版来增加易读性。如果你使用自定义主题，你会注意到一个变化，但不会破坏现有的主题系统。

We are also excited to share that this release features a number of improvements to the GitKraken Client interface. Listening to customer feedback, we worked hard to reduce visual noise across the app by minimizing the use of different colors that previously defined content areas. 9.0 now relies more on spacing and typography to increase legibility. If you use custom themes, you’ll notice a change, but nothing that breaks the existing theming system.

我们还更新了 Mac 应用程序图标，以符合当前的苹果指南，Windows 和 Linux 应用程序图标也已刷新。我们希望你喜欢新的外观！

GitKraken Client 9.0 是一个改变游戏规则的版本，它提高了产品的声誉，使开发工作流程更简单、更快速。包括工作区和洞察力在内的新功能既实用又具有前瞻性，它们反映了 GitKraken 致力于构建满足开发人员和团队需求的工具。首席执行官马特·约翰斯顿(Matt Johnston)将此次发布描述为“真正改变了开发工作流程”，并将开发从个人运动发展为团队运动。总的来说，GitKraken Client 9.0 是任何希望改善其开发工作流程并领先于未来挑战的人的必备工具。

[GitKraken Client 9.0](https://help.gitkraken.com/gitkraken-client/current/#version-9-0-0) is a game-changing release that improves upon the product’s reputation for making development workflows easier and faster. The new features, including Workspaces and Insights, are both practical and forward-looking, and they reflect GitKraken’s commitment to building tools that meet the needs of developers and teams. CEO Matt Johnston describes the release as “truly transforming the development workflow” and evolving development from an individual sport to a team sport. Overall, GitKraken Client 9.0 is a must-have for anyone looking to improve their development workflow and stay ahead of the challenges of the future.

体验工作空间、洞察力和 GitKraken Client 9.0 的所有改进。

Experience Workspaces, Insights, and all the improvements in GitKraken Client 9.0.