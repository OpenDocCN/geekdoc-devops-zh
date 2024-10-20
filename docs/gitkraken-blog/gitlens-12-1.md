# GitLens 12.1 -丰富的 GitLab 集成和改进的自动链接

> 原文：<https://www.gitkraken.com/blog/gitlens-12-1>

我们很自豪地向全世界介绍 GitLens 的最新版本。我们增加了更丰富的 GitLab 集成、Gerrit 支持、在创建交互式 rebase 时重新排序提交的能力等等。我们迫不及待地想让您了解 GitLens 12.1 的所有新功能！

更丰富的 GitLab 集成

## 利用[g itLab.com](https://docs.gitlab.com/ee/subscriptions/gitlab_com/)管理回购协议的用户现在将体验到与 GitLens 更强大的集成，比以往任何时候都更容易在 VS 代码中释放 Git 的全部力量。

GitLens 现在可以自动将合并请求与 GitLens 所熟知的行注释关联起来。合并请求也会出现在 hovers 中，它提供了关于提交、合并请求以及任何相关问题的更多信息。合并请求也会显示在状态栏中。GitLab 用户会很高兴看到他们的 GitLab 头像与注释和悬停相关联。GitLens 视图现在将显示分支和提交的相关合并请求，以及 GitLab 问题的丰富自动链接，包括标题、状态和作者信息。

[https://www.youtube.com/embed/59Gwvj8somU?feature=oembed](https://www.youtube.com/embed/59Gwvj8somU?feature=oembed)

视频

[https://www.youtube.com/embed/59Gwvj8somU?feature=oembed](https://www.youtube.com/embed/59Gwvj8somU?feature=oembed)

视频

GitLens+支持 GitLab 自管理

## 如果您的组织正在使用 [GitLab 自我管理](https://docs.gitlab.com/ee/subscriptions/self_managed/)回购，您现在将能够通过[付费 GitLens+ plan](https://www.gitkraken.com/gitlens/pricing) 在 VS 代码中释放 Git 的全部潜力。GitLab 自管理存储库托管支持许多具有特定安全和 IT 管理需求的企业。我们很高兴能够帮助管理这些回购更加容易。

如果您的组织正在使用 [GitLab 自我管理](https://docs.gitlab.com/ee/subscriptions/self_managed/)回购，您现在将能够通过[付费 GitLens+ plan](https://www.gitkraken.com/gitlens/pricing) 在 VS 代码中释放 Git 的全部潜力。GitLab 自管理存储库托管支持许多具有特定安全和 IT 管理需求的企业。我们很高兴能够帮助管理这些回购更加容易。

解锁付费 GitLens+计划的所有优势，包括丰富的 GitLab 自我管理集成。

解锁付费 GitLens+计划的所有优势，包括丰富的 GitLab 自我管理集成。

[See Pricing](https://www.gitkraken.com/gitlens/pricing)

添加和更新内脏设置中的自动链接

## 将 GitLens 与 GitHub 和 GitLab 等服务集成的用户喜欢 GitLens 自动链接到问题，并在提交比较中引用请求。现在，吉拉问题或 Zendesk 门票的粉丝可以通过在 GitLens 设置中定义自己的外部参考来获得自动链接的功能。

现在你可以在 GitLens 设置里面找到`Autolinks`部分。达到这个设置的最快方法之一是打开命令面板， **⇧⌘P** 并输入`GitLens: Configure Autolinks`。这将打开 GitLens 设置的`Autolinks`部分，在这里你可以确定你将在提交消息中使用的外部引用，GitLens 会将这些引用转换成可点击的链接。

将 GitLens 与 GitHub 和 GitLab 等服务集成的用户喜欢 GitLens 自动链接到问题，并在提交比较中引用请求。现在，吉拉问题或 Zendesk 门票的粉丝可以通过在 GitLens 设置中定义自己的外部参考来获得自动链接的功能。

现在你可以在 GitLens 设置里面找到`Autolinks`部分。达到这个设置的最快方法之一是打开命令面板， **⇧⌘P** 并输入`GitLens: Configure Autolinks`。这将打开 GitLens 设置的`Autolinks`部分，在这里你可以确定你将在提交消息中使用的外部引用，GitLens 会将这些引用转换成可点击的链接。

[https://www.youtube.com/embed/9sjQDYIBxYk?feature=oembed](https://www.youtube.com/embed/9sjQDYIBxYk?feature=oembed)

视频

重新排序交互式 rebase

GitLens 使得处理 [Git rebase](https://www.gitkraken.com/learn/git/git-rebase) 变得非常简单，这是更复杂但极其有用的 Git 特性之一。GitLens [交互式 rebase 编辑器](https://support.gitkraken.com/gitlens/features/#interactive-rebase-editor)可以帮助你轻松清理你的代码历史。

一些用户更喜欢认为他们的提交图的顺序是升序，而一些人认为提交越多，提交图的顺序就越低。我们认为两者都是正确的。GitLens 现在允许用户在交互式 rebase 编辑器中切换显示提交的顺序。

## 重新排序交互式 rebase

[https://www.youtube.com/embed/s6plz4UX_BM?feature=oembed](https://www.youtube.com/embed/s6plz4UX_BM?feature=oembed)

视频

改进的注释

文件热图

GitLens 现在比以往任何时候都更容易在使用文件热图注释时快速区分旧代码和最近添加的代码行。GitLens 将在编辑器中突出显示较新的代码行，以便更容易识别最近发生了什么变化。此外，较老的代码行将被淡出，但仍然可见，从而清楚地表明哪些代码已经存在了一段时间！

## 文件更改

能够快速识别文件中的更改非常重要。无论是为了确定您尚未修改的文件在最后一次提交时发生了什么变化，还是为了查看已经发生了变化但尚未提交的文件，GitLens 文件更改注释已经涵盖了所有内容。现在，比以往任何时候都更容易快速找到添加或更改的确切行。从“文件注释”菜单中选择“文件更改”,现在将在编辑器中突出显示相关的代码更改，帮助您更快地完成工作！

### 普通 Gerrit 支持

Gerrit 是目前最流行的基于 Git 的开源代码评审工具之一。为世界上最流行的 VS 代码 Git 扩展添加 Gerrit 支持是非常有意义的！新的集成一定会让所有 Gerrit 和 GitLens 用户疯狂地进行代码审查！

每天都有更好的协作

GitLens 致力于让你在 VS 代码中的工作更容易释放 Git 的力量。随着 rush GitLab 支持和 vanilla Gerrit 集成的加入，我们比以往任何时候都更容易连接到您的首选工具链。没有出色的 GitLens 社区的反馈，我们无法做出这些改进。如果您对我们如何使 GitLens 变得更好有任何建议或想法，我们邀请您在 [GitLens GitHub repo](https://github.com/gitkraken/vscode-gitlens) 上发表您的意见。

### 我们期待着帮助更多的 VS 代码用户利用世界上最受欢迎的版本控制系统！

能够快速识别文件中的更改非常重要。无论是为了确定您尚未修改的文件在最后一次提交时发生了什么变化，还是为了查看已经发生了变化但尚未提交的文件，GitLens 文件更改注释已经涵盖了所有内容。现在，比以往任何时候都更容易快速找到添加或更改的确切行。从“文件注释”菜单中选择“文件更改”,现在将在编辑器中突出显示相关的代码更改，帮助您更快地完成工作！

使用免费的 GitLens+帐户从 Git 中获得更多。利用工作树和可视化文件历史记录等附加功能，更多功能即将推出。

## 普通 Gerrit 支持

Gerrit 是目前最流行的基于 Git 的开源代码评审工具之一。为世界上最流行的 VS 代码 Git 扩展添加 Gerrit 支持是非常有意义的！新的集成一定会让所有 Gerrit 和 GitLens 用户疯狂地进行代码审查！

每天都有更好的协作

## GitLens 致力于让你在 VS 代码中的工作更容易释放 Git 的力量。随着 rush GitLab 支持和 vanilla Gerrit 集成的加入，我们比以往任何时候都更容易连接到您的首选工具链。没有出色的 GitLens 社区的反馈，我们无法做出这些改进。如果您对我们如何使 GitLens 变得更好有任何建议或想法，我们邀请您在 [GitLens GitHub repo](https://github.com/gitkraken/vscode-gitlens) 上发表您的意见。

我们期待着帮助更多的 VS 代码用户利用世界上最受欢迎的版本控制系统！

We are looking forward to helping even more VS Code users leverage the world’s favorite version control system!

使用免费的 GitLens+帐户从 Git 中获得更多。利用工作树和可视化文件历史记录等附加功能，更多功能即将推出。

Get more out of Git with a free GitLens+ account. Leverage additional features like Worktrees and Visual File History, with more coming soon.

[Sign up for GitLens+ for free!](https://app.gitkraken.com/register?product=gitlens&referrer=gitlens)