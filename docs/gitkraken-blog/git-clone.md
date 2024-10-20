# Git 克隆|创建现有 Git 存储库的副本

> 原文：<https://www.gitkraken.com/learn/git/git-clone>

更新日期:2022 年 6 月 15 日

Git clone 用于将现有的 [Git 存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)复制到一个新的本地目录中。

Git clone 命令将为存储库创建一个新的本地目录，复制指定存储库的所有内容，创建远程跟踪分支，并在本地签出一个初始分支。默认情况下，Git clone 会创建一个对名为`origin`的远程存储库的引用。

如何 Git 克隆一个仓库

## 要 Git 克隆存储库，请导航到您喜欢的存储库托管服务，如 GitHub，选择您想要克隆的存储库，通过 HTTPS 或 SSH 复制存储库 URL，在命令行中键入`git clone`，粘贴 URL，然后点击`enter`。

克隆一个存储库允许您在[提交](https://www.gitkraken.com/learn/git/commit)和[将它们](https://www.gitkraken.com/learn/git/git-push)推送到远程存储库之前，对存储库进行本地更改。

这对于初学者来说尤其有益，因为克隆给了你一个实验的沙箱，而不会影响原始代码库(也不会激怒你的团队成员)😅).在本文中，我们将深入了解如何从 GitKraken 客户端的命令行以及 GUI 中进行 Git clone。

涵盖 Git 克隆主题

[Git 远程存储库 URL 协议](https://www.gitkraken.com/learn/git/git-clone#url-protocols)

[Git SSH 远程协议](https://www.gitkraken.com/learn/git/git-clone#ssh-protocol)

## [Git 远程协议](https://www.gitkraken.com/learn/git/git-clone#remote-protocol)

[HTTP(S)远程协议](https://www.gitkraken.com/learn/git/git-clone#https-protocol)

[如何使用命令行在 HTTPS 上进行 Git 克隆](https://www.gitkraken.com/learn/git/git-clone#git-clone-over-https-command-line)

[如何使用 GitKraken 客户端通过 SSH 进行 Git 克隆](https://www.gitkraken.com/learn/git/git-clone#git-clone-over-ssh)

[如何使用 GitKraken 客户端在 HTTPS 上进行 Git 克隆](https://www.gitkraken.com/learn/git/git-clone#git-clone-over-https)

[Git 克隆与 GitKraken 客户端集成](https://www.gitkraken.com/learn/git/git-clone#git-clone-with-integrations)

[Git 克隆常见问题解答](https://www.gitkraken.com/learn/git/git-clone#git-clone-faqs)

[GitKraken 客户端使 Git 克隆变得更加容易](https://www.gitkraken.com/learn/git/git-clone#git-clone-easily-with-gitkraken-client)

[Git 克隆与 GitKraken 客户端集成](https://www.gitkraken.com/learn/git/git-clone#git-clone-with-integrations)

不要冒险损害代码的安全性。GitKraken 客户端允许安全连接到远程存储库——这是您不用担心的一件事。

[GitKraken 客户端使 Git 克隆变得更加容易](https://www.gitkraken.com/learn/git/git-clone#git-clone-easily-with-gitkraken-client)

Git 远程存储库 URL 协议

Git 中支持的远程 URL 协议有 SSH、Git、HTTP 和 HTTPS。

Git SSH 远程协议

## SSH 代表安全外壳。使用 Git SSH 允许您使用密钥向远程存储库进行身份验证。

SSH URL 的格式:`ssh://{user}@{host}:{port}/path/to/repo.git`或`{user}@host:path/to/repo.git`

要在命令行中使用 [Git SSH](https://www.gitkraken.com/learn/git/tutorials/how-git-ssh-works) ，您需要生成一个公共和私有的 SSH 密钥，将密钥添加到您的 SSH-agent 中，并将公共密钥添加到远程托管服务中。

### Git 远程协议

Git 协议是一种未经身份验证的方法，应该谨慎使用。

Git URL 的格式:`git://{host}:{port}/path/to/repo.git`

HTTP(S)远程协议

HTTP(S)代表超文本传输协议(安全)。通过使用 HTTPS，您可以使用用户名和密码凭证向存储库进行身份验证。

### 与 HTTP 相比，HTTPS 是一个更安全的版本。

HTTPS 网址的格式:`https://{host}:{port}/path/to/repo.git`

***GitTip:通过我们的 Git 初学者教程视频系列，了解更多关于[本地 Git 仓库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)和[远程仓库](https://www.gitkraken.com/learn/git/tutorials/what-is-git-remote)的信息。***

### 使用 GitHub 练习报告，跟随 GitKraken 客户端和命令行中即将出现的克隆示例。

HTTP(S)代表超文本传输协议(安全)。通过使用 HTTPS，您可以使用用户名和密码凭证向存储库进行身份验证。

如何使用命令行在 HTTPS 上进行 Git 克隆

在这个例子中，我们将回顾在 HTTPS 克隆 GitHub 库的过程。要克隆 Git 存储库，首先要从存储库托管服务复制远程 URL 在本例中是 GitHub。

***GitTip:通过我们的 Git 初学者教程视频系列，了解更多关于[本地 Git 仓库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)和[远程仓库](https://www.gitkraken.com/learn/git/tutorials/what-is-git-remote)的信息。***

使用 GitHub 练习报告，跟随 GitKraken 客户端和命令行中即将出现的克隆示例。

[Practice Cloning](https://github.com/Axosoft/moby-git)

然后，您将使用 Git clone 命令，后跟远程 repo 的 URL。

## 如何使用命令行在 HTTPS 上进行 Git 克隆

如果您使用的是私有存储库，系统会提示您输入远程托管服务凭据。

克隆之后，您可以将目录(`cd <path>`)更改到存储库中，开始在其中工作。有许多额外的标志可以在克隆时使用，比如命名本地目录、命名远程目录或检查分支。

然后，您将使用 Git clone 命令，后跟远程 repo 的 URL。

```
git clone <repository-url>
```

如何使用 GitKraken 客户端通过 SSH 进行 Git 克隆

如果在 GitKraken 客户机上使用 URL 通过 SSH 克隆，首先需要在 Git 客户机上配置 Git SSH。GitKraken 客户端有一个内置的 SSH 代理，使得使用 SSH 密钥(包括 GitHub SSH 密钥)变得非常容易。

您的 GitKraken 客户端 SSH 设置可以在`Preferences` > `SSH`下找到。在这里，您可以生成一个新的 SSH 密钥，指向一个现有的密钥，或者使用本地 SSH 代理来管理 SSH。

如果这是您第一次通过 GitKraken 客户端使用 SSH，您需要将 SSH 密钥添加到远程托管服务——在本例中是 GitHub。您只需从 GitKraken 客户端复制公钥，并将其添加到托管服务的设置部分。

克隆之后，您可以将目录(`cd <path>`)更改到存储库中，开始在其中工作。有许多额外的标志可以在克隆时使用，比如命名本地目录、命名远程目录或检查分支。

## 如何使用 GitKraken 客户端通过 SSH 进行 Git 克隆

完成这些步骤后，您将能够简单地提供 SSH URL 来用 GitKraken 客户端克隆远程存储库。

您的 GitKraken 客户端 SSH 设置可以在`Preferences` > `SSH`下找到。在这里，您可以生成一个新的 SSH 密钥，指向一个现有的密钥，或者使用本地 SSH 代理来管理 SSH。

有关在 GitKraken 客户端使用 SSH 的更多信息，请参见我们的 [Git SSH 文档](https://support.gitkraken.com/integrations/authentication/#ssh)。GitHub 也为[如何使用 SSH](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh) 提供了很好的指南。

如何使用 GitKraken 客户端在 HTTPS 上进行 Git 克隆

如果你用 GitKraken 客户端使用 URL 来克隆 HTTPS，你只需在提示时提供远程 URL。

有关在 GitKraken 客户端使用 SSH 的更多信息，请参见我们的 [Git SSH 文档](https://support.gitkraken.com/integrations/authentication/#ssh)。GitHub 也为[如何使用 SSH](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh) 提供了很好的指南。

在这里，您还将设置克隆的本地路径和本地存储库目录名。

## 如何使用 GitKraken 客户端在 HTTPS 上进行 Git 克隆

Git 克隆与 GitKraken 客户端集成

GitKraken Client 为以下远程存储库托管服务提供集成，使克隆和许多其他操作变得更加简单:

如下所示，您连接到 GitKraken 客户端帐户的每个主机服务集成将列出所有可用于克隆的远程存储库。

HTTPS 是 GitKraken 客户端的默认远程协议。在连接到您的远程存储库托管服务之后，在 HTTPS 上工作时，您将不需要填写凭据，因为集成使用 OAuth 或 PAT(个人访问令牌)进行身份验证。

GitKraken Client 为以下远程存储库托管服务提供集成，使克隆和许多其他操作变得更加简单:

对于这些集成中的每一个，您都可以快速创建一个 SSH 密钥或指向一个现有的密钥。生成一个 SSH 密钥会自动将它添加到托管服务中，不再需要从一个平台手动复制它并将其添加到另一个平台。

您可以通过导航至`Preferences` > `Integrations`随时管理您的 GitKraken 客户端集成。在这个例子中，您可以看到 GitKraken 客户端正在创建一个 GitHub SSH 密钥。

HTTPS 是 GitKraken 客户端的默认远程协议。在连接到您的远程存储库托管服务之后，在 HTTPS 上工作时，您将不需要填写凭据，因为集成使用 OAuth 或 PAT(个人访问令牌)进行身份验证。

或者，如果您想指向一个现有的 SSH 密钥，您可以通过单击将公共 SSH 密钥添加到托管服务中。

对于这些集成中的每一个，您都可以快速创建一个 SSH 密钥或指向一个现有的密钥。生成一个 SSH 密钥会自动将它添加到托管服务中，不再需要从一个平台手动复制它并将其添加到另一个平台。

测试你的新技能，并使用这个指导性的 GitHub repo 练习用 GitKraken 克隆一个库。

常见问题

问:如何克隆 Git 存储库

答:如果您想要克隆一个现有的 Git 存储库，请在您喜欢的代码托管服务(如 GitHub 或 GitLab)上导航到您想要克隆的 repo，并选择 clone with SSH 或 HTTPS。

问:如何克隆 Git

答:Git 是一个分布式版本控制软件，允许你跟踪项目。您不太可能会遇到想要克隆 Git 本身的情况。相反，使用 Git，您可以通过 SSH 或 HTTPS 从 GitHub、GitLab 等托管服务中克隆一个存储库。如需更多高级克隆选项，如使用 Git clone-shared、Git clone-no check out 等，请访问 [Git SCM](https://git-scm.com/docs/git-clone) 。

测试你的新技能，并使用这个指导性的 GitHub repo 练习用 GitKraken 克隆一个库。

[Practice Cloning](https://github.com/Axosoft/moby-git)

GitKraken 客户端使 Git 克隆更容易

## 你的 Git 投资组合越多，管理你的远程回购库就越难。GitKraken Git client 可以帮助您组织来自多个托管服务的远程信息，让您能够快速找到并克隆您需要的回购。

### 问:如何克隆 Git 存储库

答:如果您想要克隆一个现有的 Git 存储库，请在您喜欢的代码托管服务(如 GitHub 或 GitLab)上导航到您想要克隆的 repo，并选择 clone with SSH 或 HTTPS。

问:如何克隆 Git

### 答:Git 是一个分布式版本控制软件，允许你跟踪项目。您不太可能会遇到想要克隆 Git 本身的情况。相反，使用 Git，您可以通过 SSH 或 HTTPS 从 GitHub、GitLab 等托管服务中克隆一个存储库。如需更多高级克隆选项，如使用 Git clone-shared、Git clone-no check out 等，请访问 [Git SCM](https://git-scm.com/docs/git-clone) 。

A: Git is a distributed version control software that allows you to track projects. It is unlikely that you will run into a situation where you would want to clone Git itself. Instead, using Git, you can Git clone a repository from a hosting service like GitHub, GitLab, and others via SSH or HTTPS. For more advanced cloning options like using git clone –shared, git clone –no checkout, and more, visit [Git SCM](https://git-scm.com/docs/git-clone). 

GitKraken 客户端使 Git 克隆更容易

## 你的 Git 投资组合越多，管理你的远程回购库就越难。GitKraken Git client 可以帮助您组织来自多个托管服务的远程信息，让您能够快速找到并克隆您需要的回购。

The more your Git portfolio grows, the harder it will be to manage your library of remote repos. The [GitKraken Git client](https://www.gitkraken.com/git-client) can help organize your remotes from multiple hosting services, giving you the ability to quickly find and clone the repo you need.