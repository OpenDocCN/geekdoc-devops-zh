# 使用 CircleCI - CircleCI 构建自动化网站发布管道

> 原文：<https://circleci.com/blog/building-an-automated-website-publishing-pipeline-using-circleci/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

*这是客户成功工程师 Luc Perkins 撰写的 [Reflect 博客](https://reflect.io/blog/)的转贴。 [Reflect](https://reflect.io/) 是一个现代数据平台，结合了强大的 API、漂亮的可视化和易于使用的创作工具*

我有一个坦白:我实际上有点喜欢手动部署和管理网站。我喝一杯咖啡，运行`make publish`或一些类似的命令，凝视 CLI 输出，等待静态资产生成并`rsync`发送到千里之外的服务器，刷新浏览器，直到我看到新的变化开始生效——这是生活中微妙的乐趣之一。

但是让我们面对现实吧，既然[连续部署](https://puppet.com/blog/continuous-delivery-vs-continuous-deployment-what-s-diff)和类似的自动化模式已经成为我们行业的标准实践，通常就没有什么好的理由来这样做了。不久前，我们在 Reflect 决定使用 CI 工具(CircleCI)来自动化我们的网站部署流程，其好处显而易见。

### 我们的网站生成工具包

在我们进入网站部署之前，先简单介绍一下我们如何为上下文构建网站。

我们使用[杰基尔](https://jekyllrb.com/)作为我们的静态站点生成器和我们的[静态资产管道](https://jekyllrb.com/docs/assets/)的管理者。基本上，我们将一堆 [Markdown](https://daringfireball.net/projects/markdown/) 、 [Sass](http://sass-lang.com/) 和 JavaScript 放入 Jekyll，最终在`_site`目录中得到一个完整的站点。我们将在以后的文章中对此进行更多的讨论。

除此之外，我们还使用: [Gulp.js](http://gulpjs.com/) 来管理本地开发的[浏览器 sync](https://browsersync.io/)； [Rake](https://github.com/ruby/rake) 用于运行我们的部署脚本，这些脚本是高度参数化的，并从 Ruby 语法(而不是 Bash)中受益匪浅；当然还有老式的 Make T7，这是项目的主要入口点(我们的 CircleCI 作业只运行 Make 命令)。

### 我们网站背后的基础设施

那么我们的站点在哪里运行，我们如何到达那里？下面是我们设置的快速概述:

*   我们有两个专用的[数字海洋液滴](https://www.digitalocean.com/products/compute/)，一个在纽约运行，另一个在旧金山运行。
*   我们有网站的生产和暂存环境。目前，站点的试运行版本和生产版本在相同的机器上并行运行(考虑到我们的试运行站点的负载很轻，这很好)。
*   所有资产都通过 [nginx](https://www.nginx.com/resources/wiki/) 提供服务。我们为暂存和生产站点提供了单独的 nginx 配置。

> ### 专业提示:将你的 nginx 配置存储在与网站相同的存储库中
> 
> 最初，我们的 nginx 配置只是依赖于我们的液滴。如果我们需要更新那些配置，我们就把它们放进盒子里，进行特别的修改。但是这样做的缺点是非常明显的:很容易更新一个框而忘记更新另一个框，令人讨厌的是必须跳到另一个框才知道配置是什么，等等。因此，我们决定将 nginx 配置存储在 repo 中，与网站本身放在一起，这意味着它们现在受版本控制，易于查看。那么，我们如何确保 nginx 真正使用这些配置呢？我们将在下面详细讨论。

### 圆形构型

我们所有的代码都存在于 [GitHub](https://github.com/) 上，我们可能[对](http://githubreportcard.reflect.io/)略知一二。在 Reflect，我们使用 CircleCI 来处理与 CI 相关的所有事情。我们有大量的回复链接到它，我们在一个专用的空闲频道上从它那里得到稳定的信息流[，到目前为止我们对它非常满意。](https://circleci.com/blog/slack-integration/)

我们网站的 [CircleCI 配置](https://circleci.com/docs/configuration/)位于`circle.yml file`(按照惯例)中，目前非常少:

```
machine:
  timezone:
    America/Los_Angeles
  node:
    version: 7.4.0
  ruby:
    version: 2.3.1

dependencies:
  override:
    - make setup

deployment:
  production:
    branch: master
    commands:
      - make release
  staging:
    branch: development
    commands:
      - make release_staging 
```

该配置指定:

*   CI 输出的时间戳应该是针对`America/Los_Angeles`时区的，因为那是我们都生活在其中的时区(我们正在幕后游说一个`America/Portland`时区…我们会让你知道进展如何)
*   Node.js 版本 7.4.0 将被使用(比如我们的 [Gulp.js](http://gulpjs.com/) 设置)，而 Ruby 版本 2.3.1 将被用于 Jekyll
*   在构建之前，将运行一个额外的`make setup`命令。这个命令在我们的`Makefile`中是这样的:

```
gem install bundler
bundle install
npm install 
```

### 我们的基本发展模式

尽管总会有例外和边缘情况，但在大多数情况下，Reflect 的所有项目都遵循一个工作流程，其中:

*   `master`分支被认为是最新的
*   所有拉取请求都指向一个`development`分支，而不是直接指向`master`
*   `development`仅作为拉取请求的一部分合并到`master`中，这使我们能够在更新`master`之前查看许多拉取请求的收集结果。

网站也遵循这个流程。我们发现这个流程对我们的持续部署模式非常友好，因为我们可以自信地向`development`推送大胆的更改，并保证它们在被推送到`master`之前会被再次审查，这可能意味着从更新我们在 [npm](https://www.npmjs.com/) 中的库到更新 Debian 包的任何事情，在这种情况下，将网站推送到现场。

一旦 CI 环境设置完毕，接下来会发生什么取决于我们要合并到的分支:

1.  每当变更被合并到`master`时，生产站点被更新。这意味着以下所有事件都发生在 CircleCI:
    *   Jekyll 构建站点时将`JEKYLL_ENV`设置为`production`。当我们的 [Jekyll 环境](http://www.csinaction.com/2015/02/07/environments-in-jekyll-aka-jekyll_env/)被设置为生产时，我们的模板为 [Disqus](https://disqus.com/) 和 [Google Analytics](https://www.google.com/analytics/#?modal_active=none) 添加 JavaScript。更多关于 Jekyll 环境的信息将在下一篇文章中发布！
    *   运行`make release`命令，`rsync`将`_site`文件夹的内容保存到两个数字 Ocean droplets，`scp`保存到我们的 nginx 配置文件，并使用 [SSH](https://en.wikipedia.org/wiki/Secure_Shell) 重启 nginx 并执行一些基本的清理工作(比如`chown`某些目录)。
2.  每当变更被合并到`development`时，站点的临时版本被更新(这包括#1 的所有步骤)。

3.  默认情况下，CircleCI 会在您推送到没有任何关联行为的分支时运行`make test`。在我们的例子中，这意味着每当我们推送到`master`和`development`之外的分支时，`make test`就会运行。如果`make test`返回 0，则构建通过；否则，构建会失败。

目前，我们的`make test`命令做一件事:`make buil`，它只是运行`jekyll build`。该过程以 0(成功)或其他值(失败)退出。如果我们将任何东西推到任何一个分支，而 Jekyll 不能构建它，那么 CircleCI 会立即通过 Slack 通知我们。在未来，我们希望通过在我们的网站构建中添加链接检查器和拼写检查器来更全面地“测试”我们的网站。

### 保持您的 CircleCI 配置最小化

一般来说，在您的`circle.yml`配置文件中只包含绝对必要的内容是一个好主意。您不希望您的各种`commands`部分看起来像整个 shell 脚本。每当您需要调用一个命令时，可以将一切委托给一个 shell 脚本或一个 Make 命令。YAML 是一种简单的键/值标记格式，而不是脚本引擎！

### CircleCI 和 SSH

让我们的设置工作的唯一棘手的部分是使 CircleCI 能够通过 SSH 与我们的数字海洋水滴直接通信。我们是这样做的:

1.  我们将网站的 GitHub repo 添加到我们组织的 CircleCI 项目中

2.  我们在本地机器上创建了一个 SSH 密钥，并将密钥上传到我们当前的两个数字海洋水滴上的`~/.ssh/authorized_keys`。我们现在通过[1 密码](https://1password.com/)与任何需要的人分享这个密钥。

3.  我们通过 CircleCI web UI 中的设置页面将生成的密钥上传到 circle ci。

如果你使用过 AWS 或 DigitalOcean 或类似的服务，你可能会遇到这种情况。对我们来说，棘手的部分是确保 CircleCI *和*我们的工程师能够执行相同的任务。毕竟，有时手动发布和部署任务仍然是必要的，例如，如果 CircleCI 在我们的站点完全崩溃时关闭。

我们学到的最重要的事情是，你应该在你的网站 repo 中本地保存你的 SSH 配置*，这样你就可以确保每个用户(包括 CircleCI！)对所有的`ssh`、`scp`和`rsync`命令使用相同的配置。我们网站的 SSH 配置存储在一个`config/ssh-config`文件中，而`-F`标志使我们能够在调用命令时直接使用该配置。以下是一些例子:*

```
# Establishes a basic SSH tunnel using the config within the repo
$ ssh -F config/ssh-config reflect.io-east

# Copies our local production nginx config onto a remote box
$ scp -F config/ssh-config \
  config/nginx/nginx-production.conf \
  reflect.io-west:/etc/nginx/conf.d/reflect.io.conf 
```

网站 repo 中的 SSH 配置如下所示:

```
Host reflect.io-west
  HostName 192.0.2.1
  IdentityFile ~/.ssh/id_reflect_website
  User root

Host reflect.io-east
  HostName 192.0.2.2
  IdentityFile ~/.ssh/id_reflect_website
  User root 
```

为了确保任何人都可以手动管理该站点，我们负责该站点的每个工程师都将密钥保存在本地`~/.ssh/id_reflect_website`。在我们的数字海洋水滴眼中，CircleCI 和我们的工程师是平等的。

### 结论:开始吧，但是要记住几件事

事后看来，建立一个自动化的网站发布流程的好处是显而易见的。设置好一切花费了几个小时，但毫无疑问，这为我们节省了很多时间，让我们安心。如果你为你的网站使用静态站点生成器，我们强烈建议你做同样的投资。但是我们发现了一些你可以从中学习的东西:

*   确保在必要时仍然可以手动部署该站点。即使在高度自动化的设置中，也有很多地方可能出错，您需要为自动化框架出现问题的情况做好准备。在任何时候，您的团队中的几个成员都应该准备好安全地手动部署站点，并且只使用少量的(有良好文档记录的！)命令。我们有八名员工，目前其中一半可以通过运行`git checkout master && make release`在任何时间部署站点*。*
*   在多个环境中运行站点，这两个环境都连接到您的自动化管道中。对我们来说，这意味着`master`直接推向生产，而`development`直接推向产品化，但是您的实践可能会有所不同。您的网站是其他人进入您的组织的门户，应该像解决其他问题一样彻底地解决问题。
*   实际上，在本地发布站点所需的一切都应该位于 repo 中，并接受版本控制(因此对协作开放)。

*[阅读更多由 Luc 和 Reflect 团队发布的关于此类主题的帖子](https://reflect.io/blog)。*