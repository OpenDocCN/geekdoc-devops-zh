# WordPress、GitHub 和 GitKraken |如何为 WordPress 使用 Git

> 原文：<https://www.gitkraken.com/blog/wordpress-github>

这篇文章是一位客座作者写的。

如果你已经建立客户网站有一段时间了，你可能还记得 WordPress 出现之前的一段时间。当[建立网站](https://zyro.com/)意味着手工创建每一个 HTML 页面的时候。在某个时候，你可能认为每个客户在他们的网站上都需要*的共同特征，所以你开始使用一个客户的网站作为下一个客户的模板。当然，如今，WordPress 是许多现代网站的底层软件，没有必要重新发明核心功能。也许你总是从你建立的每个网站的相同主题开始，或者也许有你喜欢的一堆插件。*

如果你发现自己总是在建立一个新的 WordPress 站点时安装相同的主题和插件，那么可能是时候开始在 WordPress 中使用 Git 了，更具体地说是在 GitHub 中使用 WordPress。

毕竟，使用 Git 的可靠工作流对于灾难恢复非常有用，并且是快速部署新客户站点的高度可伸缩的模板。在本文中，您将学习在 WordPress 中使用 Git 和 GitHub，以及一些特定于 WordPress 的提示和技巧。

如果您在 GitHub 中使用 Git，请使用 GitKraken Git 客户端和 CLI 增强您的体验并提高工作效率。

## **为 WordPress 模板站点创建 GitHub 知识库**

为了将代码从你的 WordPress 站点复制到 GitHub，你需要[创建一个 GitHub 库](https://github.com/new)。考虑只为核心、插件和主题文件创建一个存储库(没有媒体)，因为存储库的[推荐大小](https://docs.github.com/en/repositories/working-with-files/managing-large-files/about-large-files-on-github)是 1GB 或更小。如果你使用 GitHub 来备份你的代码库，这是一个专业的举动。如果你把它作为一个完整的备份解决方案，有[更好的选项](https://help.nexcess.net/74095-wordpress/creating-backups-in-managed-wordpress-portal)来考虑媒体文件和数据库归档。

WordPress .gitignore

## ProTip:创建存储库时，选择。gitignore 模板:WordPress 选项。它将为某些插件和插件文件夹添加一些默认的允许列表和跟踪首选项。或者，如果您愿意，您可以从这个[示例](https://gist.github.com/lukecav/bc2a0c368ca55185cbcf5d5687d717b4)中创建自己的 gitignore 文件。

ProTip:创建存储库时，选择。gitignore 模板:WordPress 选项。它将为某些插件和插件文件夹添加一些默认的允许列表和跟踪首选项。或者，如果您愿意，您可以从这个[示例](https://gist.github.com/lukecav/bc2a0c368ca55185cbcf5d5687d717b4)中创建自己的 gitignore 文件。

在命名您的 [Git 存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)时，尽可能地描述清楚。

ProTip:当 Git 和来自管理 WordPress 公司的 WordPress 一起使用时，从仓库中排除 WordPress 核心和特定于主机的 MU 插件是一个好主意，因为它们是代表你管理的。如有疑问，请咨询您的主机，以了解哪些文件是托管的。

ProTip:当 Git 和来自管理 WordPress 公司的 WordPress 一起使用时，从仓库中排除 WordPress 核心和特定于主机的 MU 插件是一个好主意，因为它们是代表你管理的。如有疑问，请咨询您的主机，以了解哪些文件是托管的。

**如何同步 WordPress 和 GitHub**

## 为了获得 Git for WordPress 的强大功能，你需要确保你的主机支持 Git *和*，这样你就可以访问命令行工具，比如 [GitKraken CLI](https://www.gitkraken.com/cli) 。

在本教程中，我们将连接到 Nexcess 上强大的[托管 WordPress 主机，并用现有站点创建我们的存储库。](https://www.nexcess.net/wordpress/)

在本教程中，我们将连接到 Nexcess 上强大的[托管 WordPress 主机，并用现有站点创建我们的存储库。](https://www.nexcess.net/wordpress/)

**找到您的 SSH 凭证**

### 首先，在站点的托管门户中获取 SSH 凭证。

首先，在站点的托管门户中获取 SSH 凭证。

记下主机名、端口、用户名和密码。这些都是连接到您的 web 服务器所必需的。如果你刚刚开始使用 Git for WordPress，学习一下 Git 中 SSH 的工作原理。

记下主机名、端口、用户名和密码。这些都是连接到您的 web 服务器所必需的。如果你刚刚开始使用 Git for WordPress，学习一下 Git 中 SSH 的工作原理。

GitKraken 提供的强大的 GitHub 集成使您能够轻松地生成 SSH 密钥并将其添加到 GitHub，推送 GitHub repos 和从 GitHub repos 中提取，管理 GitHub pull 请求，等等。

GitKraken 提供的强大的 GitHub 集成使您能够轻松地生成 SSH 密钥并将其添加到 GitHub，推送 GitHub repos 和从 GitHub repos 中提取，管理 GitHub pull 请求，等等。

[Learn More about GitKraken for GitHub](/integrations/github)

初始化你的远程代码库

## 在你通过 SSH 连接到 WordPress 站点 Git 之后，是时候寻找你想要组织到一个存储库中的文件了。

我们的文件在`public_html`目录中，因此我们将导航到该目录:

`cd public_html`

接下来，我们想要初始化 WordPress 服务器上的 Git 存储库。

`git init`

最后，是时候将活动目录指定为存储库的主要分支了。

接下来，我们想要初始化 WordPress 服务器上的 Git 存储库。

`git checkout -b main`

`git init`

最后，是时候将活动目录指定为存储库的主要分支了。

**将你的远程 WordPress 代码库推送到 GitHub**

接下来，您需要将您的用户电子邮件和姓名(来自 GitHub)添加到 git 配置中。为此，您可以使用下面的 [Git 命令](https://www.gitkraken.com/learn/git/commands)(记住交换您的实际信息)。如果使用 GitKraken，这些都可以从[你的用户档案](https://support.gitkraken.com/start-here/profiles/)中配置。

**将你的远程 WordPress 代码库推送到 GitHub**

`git config --global user.email "[you@example.com]"                                       `

`git config --global user.name "[Your Name]"`

接下来，您需要将您的用户电子邮件和姓名(来自 GitHub)添加到 git 配置中。为此，您可以使用下面的 [Git 命令](https://www.gitkraken.com/learn/git/commands)(记住交换您的实际信息)。如果使用 GitKraken，这些都可以从[你的用户档案](https://support.gitkraken.com/start-here/profiles/)中配置。

## 现在大概也是[抢你 GitHub 令牌](https://github.com/settings/tokens/new)的大好时机。GitKraken 用户可以[通过 OAuth](https://support.gitkraken.com/integrations/github/) 向 GitHub 认证，而不需要手动检索。

接下来，您需要定义 GitHub 存储库的位置。

现在大概也是[抢你 GitHub 令牌](https://github.com/settings/tokens/new)的大好时机。GitKraken 用户可以[通过 OAuth](https://support.gitkraken.com/integrations/github/) 向 GitHub 认证，而不需要手动检索。

`git remote add origin https://github.com/[USERNAME]/[REPOSITORY NAME].git`

拉下。gitignore 文件从远程存储库使用；

接下来，您需要定义 GitHub 存储库的位置。

`git pull --set-upstream origin main  `

`git remote add origin https://github.com/[USERNAME]/[REPOSITORY NAME].git`

既然您已经设置好了一切，那么是时候将您的代码库提交到您刚刚在远程服务器上创建的存储库中了。要创建您的第一个 [Git commit](https://www.gitkraken.com/learn/git/commit) ，您需要在暂存文件之前添加文件。您可以使用以下命令:

`git add`

然后，您可以使用以下命令将您的更改提交给 Git:

`git pull --set-upstream origin main  `

`git commit -m "initial commit"`

既然您已经设置好了一切，那么是时候将您的代码库提交到您刚刚在远程服务器上创建的存储库中了。要创建您的第一个 [Git commit](https://www.gitkraken.com/learn/git/commit) ，您需要在暂存文件之前添加文件。您可以使用以下命令:

现在是关键时刻了！是时候将远程文件推送到您的 GitHub 存储库了。

`git add`

`git push origin main `

然后，您可以使用以下命令将您的更改提交给 Git:

就在那里！就这样，你的文件在你的远程服务器上被 Git 管理，并被推送到 GitHub。现在，下一次你需要建立一个客户的 WordPress 网站时，你可以放下你的代码库开始工作了！

`git commit -m "initial commit"`

**从 GitHub 恢复文件到 WordPress**

WordPress 的硬件设置几乎和天上的星星一样多。也许你只是想对你的内容文件夹进行版本控制，或者你想保持核心更新并部署到你所有的站点上。您从 GitHub 向 live 站点拉取或克隆的工作流程将在很大程度上取决于您的特定设置，无论您是否在托管主机上，以及其他服务器细节。

为了让你从正确的方向开始，[看看 stack exchange 上的这个帖子](https://stackoverflow.com/questions/5377960/git-whats-the-best-practice-to-git-clone-into-an-existing-folder)。

`git push origin main `

**使用 GitHub 部署 WordPress 插件**

因为许多 WordPress 主题和插件都是在 GitHub 中管理的，所以你可以直接从 GitHub 快速部署对特定站点插件或主题的修改。使用 WP-CLI 的强大功能和任何存储库的归档端点，只需使用以下命令就可以强制更新插件:

**使用 GitHub 部署 WordPress 插件**

`wp plugin install https://github.com/liquidweb/woo-better-reviews/archive/develop.zip --force`

WordPress 的硬件设置几乎和天上的星星一样多。也许你只是想对你的内容文件夹进行版本控制，或者你想保持核心更新并部署到你所有的站点上。您从 GitHub 向 live 站点拉取或克隆的工作流程将在很大程度上取决于您的特定设置，无论您是否在托管主机上，以及其他服务器细节。

## 为了让你从正确的方向开始，[看看 stack exchange 上的这个帖子](https://stackoverflow.com/questions/5377960/git-whats-the-best-practice-to-git-clone-into-an-existing-folder)。

就像魔术一样，WordPress 插件是从 GitHub 库安装的。

**使用 GitHub 部署 WordPress 插件**

因为许多 WordPress 主题和插件都是在 GitHub 中管理的，所以你可以直接从 GitHub 快速部署对特定站点插件或主题的修改。使用 WP-CLI 的强大功能和任何存储库的归档端点，只需使用以下命令就可以强制更新插件:

## **Git+GitHub+WordPress = Heaven**

记住，如果你发现你自己在你创建的每一个新的 WordPress 站点上都安装了相同的主题和插件，那么是时候寻找一个节省你时间的工作流程了(我们都知道时间就是金钱)。

最后一个提示: [GitKraken Git 客户端](https://www.gitkraken.com/git-client)和 [GitKraken CLI](https://www.gitkraken.com/cli) 使得使用 Git、GitHub 和你的 WordPress 代码库更加容易、安全和强大。您可以在 GitKraken 中快速集成、克隆和可视化您的 GitHub 库，节省您在 WordPress 中构建客户端网站的更多时间。

记住，如果你发现你自己在你创建的每一个新的 WordPress 站点上都安装了相同的主题和插件，那么是时候寻找一个节省你时间的工作流程了(我们都知道时间就是金钱)。

GitKraken 使得使用 WordPress 和 Git 更加容易、安全和强大。无论您更喜欢在 GUI 中工作还是直接在终端中工作，您都可以得到两个世界的好处。

GitKraken 使得使用 WordPress 和 Git 更加容易、安全和强大。无论您更喜欢在 GUI 中工作还是直接在终端中工作，您都可以得到两个世界的好处。

**Git+GitHub+WordPress = Heaven**

记住，如果你发现你自己在你创建的每一个新的 WordPress 站点上都安装了相同的主题和插件，那么是时候寻找一个节省你时间的工作流程了(我们都知道时间就是金钱)。

## 最后一个提示: [GitKraken Git 客户端](https://www.gitkraken.com/git-client)和 [GitKraken CLI](https://www.gitkraken.com/cli) 使得使用 Git、GitHub 和你的 WordPress 代码库更加容易、安全和强大。您可以在 GitKraken 中快速集成、克隆和可视化您的 GitHub 库，节省您在 WordPress 中构建客户端网站的更多时间。

Remember, if you find yourself installing the same themes and plugins on every new WordPress site you create, it’s time to look for a workflow that saves you time (and we all know that time is money). 

GitKraken 使得使用 WordPress 和 Git 更加容易、安全和强大。无论您更喜欢在 GUI 中工作还是直接在终端中工作，您都可以得到两个世界的好处。

GitKraken makes working with WordPress and Git easier, safer, and more powerful. Whether you prefer to work in a GUI or directly in the terminal, you get the best of both worlds.