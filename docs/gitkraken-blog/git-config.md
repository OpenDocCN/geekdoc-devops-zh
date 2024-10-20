# Git 配置|配置您的用户名和电子邮件|了解 Git

> 原文：<https://www.gitkraken.com/learn/git/git-config>

Git config 是一个强大的 [Git 命令](https://www.gitkraken.com/learn/git/commands)，它允许你定制 Git 的工作方式，并优化它以适应你的工作流程。如下所示，Git 从缩小的文件列表中提取配置设置。下图中的每个配置级别都可以覆盖其上一级的配置设置。例如，Git config 全局设置会覆盖 Git config 系统设置，但不会覆盖 Git config 本地设置。

[](https://www.gitkraken.com/wp-content/uploads/2023/01/Screen-Shot-2023-01-04-at-1.55.47-PM.png)

在[下载 Git](https://www.gitkraken.com/learn/git/git-download) 之后，大多数开发者会立即运行 Git config 命令来设置他们的用户名和电子邮件。之后，有成千上万种方法可以配置 Git 来适应特定的用例和工作流。

为了保持简单和信息丰富，我们将解释最常见的 Git 配置级别，包括 Git 配置系统、全局和本地，以及如何在[命令行](https://www.gitkraken.com/cli)和 GitKraken 客户端中配置您的用户名和电子邮件。

目录

### 有了 GitKraken Client，您可以直接开始利用 Git 的强大功能，而不必下载或配置它。

Git 配置系统

Git 配置系统级别是最广泛的配置级别，包含整个计算机的配置信息。因为 Git config 系统配置是特定于计算机的操作系统的，所以大多数开发人员倾向于保留这个级别的默认配置设置。

要在终端中查看 Git 配置系统设置的完整列表，请运行:

`git config –system –list`

Git 配置全局

## Git config 全局配置作为用户应用于您，存储在您的主目录中，并且可以覆盖 Git config 系统设置。与系统配置不同，开发人员定期定制 Git config 全局设置以适应他们的工作流。一些常见的 Git config 全局配置可用于设置您的用户名、电子邮件地址、默认编辑器、提交消息模板、终端颜色、自动更正等等。

要在终端中查看 Git 配置全局设置的完整列表，请运行:

`git config –global –list`

Git 本地配置

Git 配置本地级别包括特定于 [Git 存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)的设置，并覆盖全局和系统级别的 Git 配置。开发人员经常使用 Git config local 命令来更新与他们的工作相关的用户名和电子邮件，这取决于他们所工作的存储库的类型。

## 我们将用 GitKraken 可爱的北海巨妖吉祥物 Keif 来演示这个过程。假设 Keif 的全球配置使用他们的工作邮箱: *keif@kraken.sea* 。这意味着 Keif 的所有 [Git 提交](https://www.gitkraken.com/learn/git/commit)都与 *keif@kraken.sea* 电子邮件相关联，除非它们运行 Git config 本地命令来更改特定 Git 存储库的相关电子邮件。

现在让我们假设 Keif 已经完成了当天的工作，现在他们想要为他们最喜欢的开源项目 [GitLens](https://github.com/gitkraken/vscode-gitlens) 做贡献。Keif 不希望他们的工作电子邮件与他们在这个项目中的提交相关联，而是更喜欢使用他们的个人电子邮件:【steampunkkraken@cephalopod.com*。为了确保他们对 GitLens 回购的贡献与 steampunkkraken@cephalopod.com*的*电子邮件相关联，Keif 可以简单地运行以下命令:*

`git config --local user.email steampunkkraken@cephalopod.com`

现在，Keif 对 GitLens 库的贡献将与他们更喜欢的个人邮件而不是工作邮件联系在一起。请记住，有了这个过程，Keif 在返回工作时将不必运行任何 Git config 命令，因为他们只更新了 GitLens 存储库的本地配置。

要查看 Git 配置本地设置的完整列表，请运行:

## `git config –local –list`

Git 配置本地级别包括特定于 [Git 存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)的设置，并覆盖全局和系统级别的 Git 配置。开发人员经常使用 Git config local 命令来更新与他们的工作相关的用户名和电子邮件，这取决于他们所工作的存储库的类型。

命令行中的 Git 配置用户名

现在您已经对不同的 Git 配置级别有了大致的了解，让我们来看看如何使用 Git config 命令来设置您的用户名和电子邮件。Git 要求在您可以利用它的所有功能之前建立用户名和电子邮件，所以在您下载 Git 之后立即解决这个问题是一个好主意。

要使用 Git config username 设置您的用户名，请导航到终端并运行:

`git config --global user.name "Your Name"`

要查看 Git 配置本地设置的完整列表，请运行:

## 命令行中的 Git 配置电子邮件

要使用 Git config email 在终端中设置您的电子邮件，请运行:

`git config --global user.email youremail@example.com`

`git config --global user.name "Your Name"`

值得注意的是，因为使用了全局级配置，Git 将使用您为您创建的每个后续 Git 存储库设置的用户名和电子邮件，除非您执行本地配置以逐个存储库地覆盖它。

## 命令行中的 Git 配置电子邮件

Git 将您的默认分支名称配置为 Main

2020 年，GitHub、其他托管服务和整个开发社区合作，将默认或初始分支名称从 master 改为 main。这反映了社会意识向更具包容性的术语的转变。阅读更多关于从主[到主](https://www.techrepublic.com/article/github-to-replace-master-with-main-starting-in-october-what-developers-need-to-know/)的变化。

要将默认分支名称从 master 更改为 main，请在终端中运行以下命令:

`git config --global init.defaultBranch main`

## 现在，每当您初始化一个新的 Git 存储库时，创建的第一个分支将被称为 main。

2020 年，GitHub、其他托管服务和整个开发社区合作，将默认或初始分支名称从 master 改为 main。这反映了社会意识向更具包容性的术语的转变。阅读更多关于从主[到主](https://www.techrepublic.com/article/github-to-replace-master-with-main-starting-in-october-what-developers-need-to-know/)的变化。

在命令行中显示 Git 配置

您可以通过在终端中运行 show Git config 命令来查看所有 Git 配置设置:

`Git config –list –show-origin`.

终端将按照我们之前讨论过的层次顺序返回您的 Git 配置设置，从系统级开始，缩小到全局和本地。

现在，每当您初始化一个新的 Git 存储库时，创建的第一个分支将被称为 main。

Git 客户端的 git 配置

## 使用 [GitKraken Client](https://www.gitkraken.com/git-client) ，您可以开始使用 Git 及其所有功能，而无需运行任何 Git config 命令。事实上，如果你使用 GitKraken 客户端，你根本不需要在你的机器上下载 Git。为了在不运行 Git config 命令的情况下在 GitKraken 客户端自定义您的用户名和电子邮件，请遵循以下步骤:

选择右上角的齿轮图标⚙️进入您的`Preferences`

在左侧面板中，点击`Profiles`

在`Profiles`部分选择您姓名旁边的三个省略号，并从下拉菜单中点击`Edit Profile`

从这里您可以更新您的`Author Name`和`Author Email`

## 您可以通过更新 GitKraken 客户端`Preferences`左侧面板中的选项来访问进一步的定制和配置设置。

用它来配置 Git

1.  我们已经介绍了 Git config 命令的基本层次，并展示了一些开发人员如何使用这个命令定制 Git 的例子。如果你想深入了解更高级的 Git 配置选项，请访问官方 Git 配置文档。随着您对 Git 越来越熟悉，您将会遇到更多独特的方式来配置它以满足您的开发需求。GitKraken 客户端支持您独特的 Git 配置，但也允许您在根本不需要配置 Git 的情况下利用 Git，从而节省您的时间和精力。[免费试用 GitKraken 客户端](https://www.gitkraken.com/git-client/try-free)，让你的 Git 技能更上一层楼。
2.  在左侧面板中，点击`Profiles`
3.  在`Profiles`部分选择您姓名旁边的三个省略号，并从下拉菜单中点击`Edit Profile`
4.  从这里您可以更新您的`Author Name`和`Author Email`

您可以通过更新 GitKraken 客户端`Preferences`左侧面板中的选项来访问进一步的定制和配置设置。

用它来配置 Git

我们已经介绍了 Git config 命令的基本层次，并展示了一些开发人员如何使用这个命令定制 Git 的例子。如果你想深入了解更高级的 Git 配置选项，请访问官方 Git 配置文档。随着您对 Git 越来越熟悉，您将会遇到更多独特的方式来配置它以满足您的开发需求。GitKraken 客户端支持您独特的 Git 配置，但也允许您在根本不需要配置 Git 的情况下利用 Git，从而节省您的时间和精力。[免费试用 GitKraken 客户端](https://www.gitkraken.com/git-client/try-free)，让你的 Git 技能更上一层楼。

## Git Config-gy with it

We’ve covered the basic hierarchy of the Git config command and shown a few examples of how developers use this command to customize Git. If you want to delve deeper into the more advanced Git config options, visit the [official Git config documentation](https://git-scm.com/docs/git-config). As you become more familiar with Git, you’ll encounter more unique ways you can configure it to fit your development needs. GitKraken Client supports your unique Git configurations, but also allows you to leverage Git without having to configure it at all, saving you time and mental energy. [Try GitKraken Client for free](https://www.gitkraken.com/git-client/try-free) and take your Git skills to the next level.