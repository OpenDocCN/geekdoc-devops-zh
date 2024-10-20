# 来自 2021 GitKon Git 会议的 Git 提示面板

> 原文：<https://www.gitkraken.com/gitkon/git-tips-panel>

[https://www.youtube.com/embed/bu9c_5OzvIQ?feature=oembed](https://www.youtube.com/embed/bu9c_5OzvIQ?feature=oembed)

视频

**Git 是什么？**

## Git 由 Linus Torvalds 于 2005 年创建，旨在维护 Linux 内核的开发。Git 建立在完全分布式、速度、简单设计、处理大型项目的能力和对非线性开发的强大支持的基础之上。如果我们看看 Git 的[历史，我们可以看到它已经成为全世界许多开发者的首选版本控制系统。](https://www.gitkraken.com/gitkon/history-of-git)

下面的每一个 Git 技巧都来自参加 2021 [GitKon Git 会议](https://gitkon.com/)小组的一位业内公认的 Git 专家:“Git 技巧，我希望我能早点知道。”小组成员包括 Kanopi 工作室的 Sean Dietrich、PHP 顾问 Carl Alexander、Cyral 的 Rob Richardson、LinearB 的 Nick Hodges 和 Enova 的 Meriem Zaid。

Each of the below Git tips comes from one of the industry proven Git experts that participated on the 2021 [GitKon Git conference](https://gitkon.com/) panel: “ Git Tips I Wish I Had Known Sooner.” Panelists included Sean Dietrich of Kanopi Studios, PHP consultant Carl Alexander, Rob Richardson of Cyral, Nick Hodges of LinearB, and Meriem Zaid from Enova.

这里有一个 Git 技巧:在 GitKraken 的帮助下启用 Git 最强大的功能，Git kraken 包括一个 GUI 和 CLI。

Here’s a Git tip: enable the most powerful capabilities of Git with the help of GitKraken, which includes a GUI and CLI.

**Git 技巧 1:使用 Git 钩子发布代码**

## 在提交新的一行代码之前，尝试使用 [Git 钩子](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) s。Git 挂钩允许你在本地机器上运行测试，并与项目中的其他开发人员协调，以确保当你把新代码放到你的 [Git 遥控器](https://www.gitkraken.com/learn/git/git-remote)上时，不会“破坏任何东西”。使用 Git 挂钩可以帮助您避免破坏一些东西，并花费宝贵的时间与其他开发人员协调，试图识别和修复问题。

Try using [Git Hook](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)s before you commit a new line of code. Git Hooks allow you to run tests on your local machine and coordinate with other developers on the project to make sure your new code doesn’t “break anything” when you pull it up to your [Git remote](https://www.gitkraken.com/learn/git/git-remote). Using Git Hooks helps you avoid breaking something and spending valuable time coordinating with other developers trying to identify and fix problems.

Git 提示 2: GrumPHP

## 如果你正在使用 PHP，试着设置一下 [GrumPHP 工具](https://github.com/phpro/grumphp)。这个 composer 插件将在你的包库中注册一些 Git 钩子。当有人提交变更时，GrumPHP 会对提交的代码进行一些测试。如果测试失败，您将无法提交您的更改。当该工具识别出一个问题时，你会在你的控制台上看到一个脾气暴躁的老人图标，详细描述手头的问题。

If you’re using PHP, try setting up the [GrumPHP tool](https://github.com/phpro/grumphp). This composer plugin will register some Git Hooks in your package repository. When somebody commits changes, GrumPHP will run some tests on the committed code. If the tests fail, you won’t be able to commit your changes. When the tool identifies a problem, you’ll be met with a grumpy old man icon on your console, detailing the issue at hand.

**Git 技巧 3:使用 Git GUI**

## 不管你有多少使用 Git 的经验，都不要回避使用 Git GUI。请记住，并不是所有的图形用户界面都是一样的，您需要货比三家，选择一个符合您需求的。使用 Git GUI 允许您跟踪您的 [Git 提交](https://www.gitkraken.com/learn/git/commit)，查看 [Git 分支](https://www.gitkraken.com/learn/git/branch)，并以一种非常有形和可见的方式与您的团队协作。

Regardless of how much experience you have with Git, don’t shy away from using a Git GUI. Keep in mind that not all GUIs are created equal and you’ll need to shop around for one that meets your needs. Using a Git GUI allows you to track your [Git commits](https://www.gitkraken.com/learn/git/commit), view [Git branches](https://www.gitkraken.com/learn/git/branch), and coordinate with your team in a very tangible and visible way.

Git 非常强大，但是学习起来很复杂。GitKraken 提供的可视化将帮助您更快地理解 Git，以便您可以安全地为项目做出贡献。

Git is incredibly powerful, but can be complicated to learn. The visualization offered by GitKraken will help you understand Git faster so you can safely contribute to projects.

**Git 提示 4:不要将资产直接存储在存储库中**

## 当编译您的资产时，确保避免将它们存储在您的 [Git 存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)中。相反，尝试将您的资产，如缩小的 javascript 工具和 CSS，存储在 CI 管道中。从长远来看，这将有助于保持您的存储库更小、更易管理，同时也防止了 [Git 合并冲突](https://www.gitkraken.com/learn/git/tutorials/how-to-resolve-merge-conflict-in-git)。

When compiling your assets be sure to avoid storing them in your [Git repository](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository). Instead, try storing your assets, like minified javascript tools and CSS, in a CI pipeline. This will help keep your repository smaller and more manageable while also preventing [Git merge conflicts](https://www.gitkraken.com/learn/git/tutorials/how-to-resolve-merge-conflict-in-git) in the long run.

**Git 提示 5:微提交**

## 当处理一个大项目时，考虑花一些时间来推动较小的提交。这个过程被称为“[微提交策略](https://www.quora.com/What-is-the-meaning-of-Micro-Commit-in-Git-Is-it-for-the-small-changes-If-it-is-then-how-can-we-perform-Micro-Commit-for-small-changes)”。这种策略不仅有助于保持一个干净的存储库，而且如果必要的话，它还使得恢复 Git 提交变得容易[。](https://www.gitkraken.com/learn/git/problems/revert-git-commit)

When working with a large project, consider taking some time to push smaller commits. This process is known as the “[micro-commit strategy](https://www.quora.com/What-is-the-meaning-of-Micro-Commit-in-Git-Is-it-for-the-small-changes-If-it-is-then-how-can-we-perform-Micro-Commit-for-small-changes)”. Not only does this strategy help keep a clean repository, but it also makes it easy to [revert a Git commit](https://www.gitkraken.com/learn/git/problems/revert-git-commit) if necessary.

Git 提示 6: Git 自动更正

## 使用 Git 自动更正功能很容易配置，并且在您赶时间时会有所帮助。打字错误时有发生，自动更正功能允许您或者让系统为您更正命令，或者简单地提示您它假定您正试图使用的命令。

Using the Git autocorrect feature is easy to configure and can help when you’re in a hurry. Typos happen, and the autocorrect feature allows you to either have the system correct the command for you or simply give you hints about the command it assumes you were trying to use.

GitKraken CLI 不仅为 Git 命令提供了自动完成功能，它还更进了一步，包括了自动建议功能，直观地加快了您的工作流程，同时提供了有关 Git 操作的有用上下文。

Not only does the GitKraken CLI offer auto-complete for Git commands, it goes one step further to include auto-suggest, intuitively speeding up your workflow while providing helpful context about your Git actions.

**转到类型 7:转到别名**

## 使用 [Git 别名](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases)允许开发者在 Git 中创建其他[命令，以获得更快的编码体验。例如，如果您发现自己一天要输入多次命令，那么您可以创建并命名自己的 Git 别名来缩写该命令。提醒一句:虽然这可以节省大量时间，但是使用 Git 别名可能是危险的，因为有可能重命名或创建已经存在的命令。](https://www.gitkraken.com/learn/git/commands)

Using [Git aliases](https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases) allow developers to create other [commands in Git](https://www.gitkraken.com/learn/git/commands) for a faster coding experience. For example, if you have a command that you find yourself typing out multiple times a day, you can create and name your own Git alias to abbreviate the command. A word of caution: while this can be a major time-saver, using Git aliases can be dangerous because it’s possible to rename or create commands that already exist.

**Git 技巧 8:保持一个干净的 Git Repo**

## 为了保持一个干净的存储库，你和你的团队应该确定一个可重复的命名和 [Git 分支策略。添加一个随机的或有趣的分支标题可能很容易，但随着项目的增长，这可能会造成混乱。创建一致的分支命名模式将确保所有团队成员的无缝发布和导航过程。](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy)

花时间编写经过深思熟虑的提交消息看起来像是一件苦差事，但是在保持一个干净的存储库的时候也是一个救命稻草。花时间为你的提交做一个简短的总结将会让你收获回报，因为你在稍后的评审和发布过程中节省了时间。

使用完要素分支后，将其删除。GitHub 确实很擅长保存它们，如果你以后需要它们的话，但是从你的项目中删除它们也将有助于你的整体库健康。

Delete your feature branches once you are done with them. GitHub is really good about saving those if you need them later on, but deleting them from your project will also help with your overall repository health.

**Git Tip 9: Git Rebase**

## 你有没有发现自己远离主分支，在一个特性分支上有一堆分组提交？这是一个很好的例子，说明何时重定基数是有帮助的。Git rebase 的目标是改变你的项目历史。Git 中的重新基础化允许您将提交合并在一起，删除它们，分配一个新的父提交，重新排序它们，或者完全更改基础提交。需要注意的是，您的提交将会以不同的 Git 散列结束，因为 Git 散列是内容和父散列的组合，使用 rebase 将会改变父散列。

Have you ever found yourself with a bunch of grouped commits on one feature branch, away from your main branch? This is a perfect example of when rebase would be helpful. The goal of [Git rebase](https://www.gitkraken.com/learn/git/git-rebase) is to change your project history. Rebasing in Git allows you to take your commits and squish them together, remove them, assign a new parent commit, reorder them, or change the base commit entirely. It’s important to note that your commits will end up with different Git hashes because the Git hash is a combination of the content and the parent hashes, and using rebase will change the parent hash.

Rebase 可能是一个令人生畏的命令，因为它涉及到许多移动的部分，如果你不小心，有可能把事情搞砸，但是如果你花时间学习如何有效地使用这个工具，你将能够保持一个更健康、更线性的库。

**在它之后的 Git**

关于 Git，要认识到的最重要的一点是，它是不断发展的。许多 Git 技巧和诀窍来自多年的实践经验以及大量的反复试验。适用于一个开发人员的技巧不一定适用于下一个开发人员，所以我们强烈建议您测试一下上面的列表，看看哪些适合您。

## 你有自己的 Git 技巧分享吗？将它发布到社交媒体，标记 [@GitKraken](https://twitter.com/gitkraken) ，并使用标签 `#GitTip`——你可能会在 GitKraken 的某个社交频道上看到你的提示！

The most important thing to recognize about Git is that it’s constantly evolving. Many of these Git tips and tricks came from years of hands-on experience as well as a good amount of trial and error. The tricks that work for one developer may not work for the next, so we highly encourage you to put the above list to the test and find out which ones work for you.

Do you have your own Git tip to share? Post it to social media, tag [@GitKraken](https://twitter.com/gitkraken), and use the hashtag `#GitTip` – you might just see your tip featured on one of the GitKraken social channels!