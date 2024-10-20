# GitKraken 客户端 v8.6 -更快的 Git LFS，大回购，等等！

> 原文：<https://www.gitkraken.com/blog/gitkraken-client-v8-6>

我们知道每个人的代码故事可能有一点不同，但更快的回复是每个人都可以得到的。

无论开发人员将您带到哪里，将所有代码、配置和媒体资产放在一起，并且不要留下任何文件，这一点很重要。这就是为什么我们为 Git LFS 用户做了很多性能改进，并为 Bitbucket 服务器用户增加了 Bitbucket Workspace 支持！

准备好迎接 GitKraken 客户端 8.6 版的发布吧！

[https://www.youtube.com/embed/TsRKcb7hP0I?feature=oembed](https://www.youtube.com/embed/TsRKcb7hP0I?feature=oembed)

视频

今天就用更快的 Git LFS 回购访问开始你的冒险吧！下载 GitKraken 客户端 v8.6\.

## 更快的 Git LFS 存储库性能

我们非常自豪地宣布，随着 GitKraken 客户端 8.6 版的发布，在使用 [Git LFS](https://www.gitkraken.com/learn/git/git-lfs) 时，克隆和检出速度提高了 2 到 10 倍。

如果您曾经需要将大型媒体文件或二进制文件作为存储库的一部分进行管理，那么您可能已经体验过管理一个非常大的存储库所带来的性能下降。谢天谢地 [Git LFS](https://www.gitkraken.com/learn/git/git-lfs) (大文件存储)可以将大文件的存储卸载到另一个服务器上，在你的 repo 中用非常轻量级的指针替换它们。

然而，正如许多 Git LFS 用户发现的那样，在克隆或检查分支时，管理 Git LFS 支持的回购仍然会导致某些性能问题。也就是说，直到现在…

GitKraken 团队一直在努力改进 Git kraken 客户端管理 Git LFS 交互的方式，优化大型文件的调用和过滤方式。我们一直在测试一些最大的媒体资产密集型回购，包括一些视频游戏和超大型开源项目，在克隆或切换分支时，性能提升了 2-10 倍。你会爱上速度的提升——像太空游侠一样快！

### 大型 Git 回购的性能优势

虽然我们在这个版本中性能改进的重点是 Git LFS，但是我们也做了许多其他的性能改进，特别有助于处理大型 Git 存储库。

不管你是在使用 Git LFS 还是仅仅处理大型回购，你都会喜欢 GitKraken 客户端 8.6 版的速度提升！

## Git Stash 性能改进

Git stashing 是 Git 赋予您的超能力之一，允许您保存未提交的更改，同时将它们从索引中删除，同时还允许您在准备好的时候访问这些更改。保存未提交的更改后，您可以签出另一个分支，做您的工作，然后弹出保存的内容，从您离开的地方继续。

如果你经常使用 [Git stashes](https://www.gitkraken.com/learn/git/git-stash) ，你会喜欢 GitKraken Client 的性能改进。您将看到更快的分支检出和更快的图形性能，即使您有许多 stashes。

## Bitbucket 服务器的工作空间支持

我们很高兴宣布在 [Bitbucket 服务器](https://www.atlassian.com/software/bitbucket/enterprise)上为存储库提供 Bitbucket Workspace 支持。现在 Bitbucket 用户可以利用 [GitKraken Workspaces](https://support.gitkraken.com/working-with-repositories/workspaces/) 提供的强大优势来管理多存储库项目。

GitKraken Workspaces 允许您从 GitKraken 客户端的一个选项卡中访问组成项目的所有相关回购。从一个视图中查看关于单个存储库和所有 [Git pull 请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)的信息。您甚至可以触发工作区中所有回购的操作，从而节省您在多个屏幕和文件夹之间切换来管理多个回购项目的大量时间。

我们很自豪地为 Bitbucket 工作区添加了这种支持，以帮助更多用户利用 GitKraken 客户端更高效地进行协作。

## 为您的回购和终端标签设置别名

有时，您的组织选择的命名约定可能会产生一些很长的 [Git 存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)名称。虽然在管理大量存储库时，冗长的存储库命名会有所帮助，但在 GitKraken 客户端中工作时，它也会使找到正确的选项卡变得困难。用户一直在寻找为整个回购协议或单个终端标签设置别名的方法，现在你可以了！

您现在可以将任何存储库设置为别名存储库；这将在选项卡中重命名回购，通过较短的自定义名称引用它。要为回购设置别名，右键单击任意回购选项卡顶部的回购名称，然后选择`Alias repository`。

您也可以为单个终端标签设置别名。要重命名任何终端选项卡，右击顶部的选项卡名称并点击`Rename tab`。与通过“回购”标签设置别名不同，为“终端”标签设置别名只会导致重命名该特定标签。

## 拉式请求的自动问题链接

当提出一个拉请求时，包含一个到请求所解决的问题的链接是非常常见的做法。GitKraken 客户端现在通过自动将问题链接到拉请求，使这变得更快更容易！

自动链接要求您[将您的问题队列](https://support.gitkraken.com/start-here/integrations/)与 GitKraken 客户端整合，并以问题 ID 开始您的分支机构名称。例如，如果您的发行号是`GK-1234`，您的分行名称应该类似于:`GK-1234-<rest-of-branch-name>`。

新子模块更新选项

Git 提供的一个超能力是使得利用 [Git 子模块](https://www.gitkraken.com/learn/git/tutorials/git-submodule-how-to)独立管理每个子库来构建模块化代码架构变得更加容易。但是在某些情况下，当您更新 repo 的其余部分时，例如，您可能还没有准备好更新子模块。这可能是由于依赖性问题，或者是生产分支中尚未解决的新错误。GitKraken 客户端用户现在可以选择在执行 Git 动作时跳过子模块更新。您可以通过 GitKraken Client 将此设置为在您管理的所有回购中全局发生，或者决定在“每个回购”的基础上跳过更新。

要全局切换子模块更新策略，导航至`Preferences` → `General`并寻找`Keep submodules up to date`选项。

## 要更改单个存储库的子模块更新策略，请导航至`Preferences` → `Submodules`，并从下拉菜单中选择您的首选策略。

Git 提供的一个超能力是使得利用 [Git 子模块](https://www.gitkraken.com/learn/git/tutorials/git-submodule-how-to)独立管理每个子库来构建模块化代码架构变得更加容易。但是在某些情况下，当您更新 repo 的其余部分时，例如，您可能还没有准备好更新子模块。这可能是由于依赖性问题，或者是生产分支中尚未解决的新错误。GitKraken 客户端用户现在可以选择在执行 Git 动作时跳过子模块更新。您可以通过 GitKraken Client 将此设置为在您管理的所有回购中全局发生，或者决定在“每个回购”的基础上跳过更新。

要全局切换子模块更新策略，导航至`Preferences` → `General`并寻找`Keep submodules up to date`选项。

要更改单个存储库的子模块更新策略，请导航至`Preferences` → `Submodules`，并从下拉菜单中选择您的首选策略。

Windows 上 GitKraken CLI 的 Git Bash

Windows 用户将很高兴听到他们现在可以将 Powershell 或 [Git Bash](https://www.gitkraken.com/blog/what-is-git-bash) 设置为 GitKraken CLI 的默认 shell。

## GitKraken 客户端用户可以通过 repo 选项卡中的终端面板或终端选项卡使用内置的 [GitKraken CLI](https://www.gitkraken.com/cli) 。直到现在，Windows 用户不得不使用 Powershell 作为他们唯一的 shell 选项。Windows 用户现在可以选择 Git Bash 作为他们的 shell。

Windows 用户可以通过导航到`Preferences` → `Terminal` → `Default Terminal`并从下拉菜单中选择“Git Bash”来设置你的默认终端。

GitKraken 客户端用户可以通过 repo 选项卡中的终端面板或终端选项卡使用内置的 [GitKraken CLI](https://www.gitkraken.com/cli) 。直到现在，Windows 用户不得不使用 Powershell 作为他们唯一的 shell 选项。Windows 用户现在可以选择 Git Bash 作为他们的 shell。

Windows 用户可以通过导航到`Preferences` → `Terminal` → `Default Terminal`并从下拉菜单中选择“Git Bash”来设置你的默认终端。

在提交图中显示较少的提交

随着项目中提交数量的增加，您可能不希望总是在提交图中看到每一次提交。事实上，如果您决定一次显示超过两千个，可能会对性能产生一些负面影响。大多数时候，你希望看到尽可能少的人，同时还能让你完成工作。

您现在可以将提交图中显示的提交数量设置为 500 个。要进行设置，导航至`Preferences` → `General` → `Max Commits in Graph`。

## 在提交图中显示较少的提交

随着项目中提交数量的增加，您可能不希望总是在提交图中看到每一次提交。事实上，如果您决定一次显示超过两千个，可能会对性能产生一些负面影响。大多数时候，你希望看到尽可能少的人，同时还能让你完成工作。

您现在可以将提交图中显示的提交数量设置为 500 个。要进行设置，导航至`Preferences` → `General` → `Max Commits in Graph`。

你在吉克拉肯有个朋友

无论你认为自己是一个编码牛仔，管理一个巨大的回购，还是一个星系间的空间游侠协调整个舰队的服务，你都会喜欢 GitKraken Client v8.6 的改进。我们使 LFS 回购操作以接近光速的速度移动，并增加了对 Bitbucket Server 工作区的支持，以帮助团队一起完成更多工作！我们也在倾听用户希望我们下一步做什么。我们邀请您将您的功能请求添加到我们的反馈网站[,借出您的声音。](https://feedback.gitkraken.com)

## 我们知道您会喜欢我们随 GitKraken Client v8.6 发布的内容，这是适用于 Windows、Mac 和 Linux 的最佳 Git 客户端。

无论你认为自己是一个编码牛仔，管理一个巨大的回购，还是一个星系间的空间游侠协调整个舰队的服务，你都会喜欢 GitKraken Client v8.6 的改进。我们使 LFS 回购操作以接近光速的速度移动，并增加了对 Bitbucket Server 工作区的支持，以帮助团队一起完成更多工作！我们也在倾听用户希望我们下一步做什么。我们邀请您将您的功能请求添加到我们的反馈网站[,借出您的声音。](https://feedback.gitkraken.com)

这不仅仅是版本控制，这是有风格地管理 Git repos！

这不仅仅是版本控制，这是有风格地管理 Git repos！