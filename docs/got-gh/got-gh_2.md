# 2\. 加入 GitHub

本章介绍如何在 GitHub 上注册账号，并以现有项目为例介绍 GitHub 的主要功能。

# 2.1\. 创建 GitHub 账号

注册 GitHub 账号，只要点击导航条中的“Signup and Pricing”，或者点击首页中那个大大的“Plans, Pricing and Signup”按钮，即进入收费方案介绍及注册页面。

收费？不必担心，开源软件托管是 GitHub 的基石，对于开源项目的版本库（即非私有版本库）的托管，GitHub 是免费的。在收费方案及注册页面中，最上面的就是针对于开源的免费托管方案，如图 2-1 所示。

![../images/free-plan.png](http://box.kancloud.cn/2015-07-09_559de00cc66db.png)

图 2-1：针对开源项目（公开版本库）的免费方案

至于本页其他付费方案，将在后面的章节介绍。点击免费方案右侧的“Create a free account”按钮，就进入到注册页面，如图 2-2 所示。

![../images/signup.png](http://box.kancloud.cn/2015-07-09_559de016815f9.png)

图 2-2：账号注册

GitHub 的注册页面非常简洁，只有登录 ID，邮件地址和口令需要输入。要注意的是每个邮件地址只能注册一次。

注册完毕即以新注册的账号自动登录。登录后即进入用户的仪表板（Dashborad）页面。首次进入的仪表板页面还会在其中显示 GitHub BootCamp（GitHub 新手训练营）的链接，以帮助新用户快速入门。如果图 2-3 所示。

![../images/loggedin.png](http://box.kancloud.cn/2015-07-09_559de01862bcc.png)

图 2-3：登录后的 GitHub 首页

仪表板页面是用户最重要的页面，因为创建新项目（新版本库）的链接就位于该页面。重新设计的 GitHub 用户界面[[1]](https://github.com/blog/1007-skinny-header)中跳转到仪表板页面的链接不像之前那么直观，鼠标移动到页面左上角的“github”文字图标会发现此图标可以点击，该文字图标即是进入仪表板页面的快捷。

在页面右上方显示当前登录用户的名称和头像。图 2-3 中显示登录用户为 gotgithub，而用户头像因为尚未设置所以显示为缺省图片——GitHub 吉祥物 Octocat 的剪影。在页面右上方还有三个图标，从左至右分别是：通知、账号设置和退出。点击账号设置图标对账号进行进一步设置，如图 2-4 所示。

![../images/setting-profile.png](http://box.kancloud.cn/2015-07-09_559de0217b54c.png)

图 2-4：账户设置页

账号设置的第一个页面是对用户公开身份信息进行设置，所有内容均为可选项，如果填写将显示在个人页面中，并能被所有人访问。注意修改用户头像需要访问第三方头像设置网站：[gravatar.com](http://gravatar.com/)，Gravatar 网站提供的头像服务是一个通用服务，可为大部分 Web 应用所使用。

图 2-4 中还显示了当前用户使用的 GitHub 托管方案（Free）和使用统计。因为当前注册用户选择的是免费方案，所以可用的私有版本库数量和私有空间的协同者数目都是零。免费方案拥有 300MB 托管空间，因当前尚未创建版本库托管，所以空间占用为零。GitHub 对开源软件的 300MB 托管空间限制并非硬性限制，可以申请扩增托管空间，如果不存在滥用情况的话。

点击菜单中的“Account Admin”，可以更改口令、查看 API Token、修改用户名，以及删除自身账号，如图 2-5 所示。

![../images/setting-admin.png](http://box.kancloud.cn/2015-07-09_559de0235d158.png)

图 2-5：账户管理

其中 API Token 是和用户口令相关的密钥，当用户口令更改时 API Token 也随之更改。GitHub 的某些应用会使用 API Token 进行身份认证，从而避免直接使用用户口令造成泄露的风险。API Token 若泄露的危害要远远小于口令泄露，这因为 API Token 不能用于登录 GitHub 网站等，而且一旦 API Token 泄露可以很容易通过更改口令的方式更换 API Token。

点击菜单中的“Email Addresses”，可以添加和删除邮件地址，如图 2-6 所示。GitHub 允许为一个账号绑定多个邮件地址，以便能够将 Git 版本库中的提交（提交者以 “用户名 ” 的格式给出）正确对应到 GitHub 账户。

![../images/setting-email.png](http://box.kancloud.cn/2015-07-09_559de0269cd35.png)

图 2-6：邮件地址管理

GitHub 为托管的 Git 版本库提供 SSH 协议支持，即用户可以用公钥认证的方式连接到 GitHub 的 SSH 服务器。下面的示例用 ssh 命令连接 github.com 的 SSH 服务，登录用户名为 git（所有 GitHub 用户共享此 SSH 用户名，不要写成其他）。

```
$ ssh -T git@github.com
Permission denied (publickey). 
```

上面的示例显示登录失败，这是因为我们尚未在 GitHub 账户中正确设置公钥认证。图 2-7 显示的是 GitHub 的 SSH 公钥设置界面。

![../images/setting-ssh.png](http://box.kancloud.cn/2015-07-09_559de0278ceb6.png)

图 2-7：SSH 公钥管理

要想向 GitHub 添加 SSH 公钥，首先要确保正确生成了对应的公钥/私钥对。关于 SSH 公钥认证，在我的《Git 权威指南》一书的“第二十九章使用 SSH 协议”中有详细介绍，这里仅做简要的介绍。

GitHub 的 SSH 服务支持 OpenSSH 格式的公钥认证，可以通过 Linux、Mac OS X、或 Cygwin 下的 ssh-keygen 命令创建公钥/私钥对。命令如下：

```
$ ssh-keygen 
```

然后根据提示在用户主目录下的.ssh 目录中创建默认的公钥/私钥对文件，其中`~/.ssh/id_rsa`是私钥文件，`~/.ssh/id_rsa.pub`是公钥文件。注意私钥文件要严加保护，不能泄露给任何人。如果在执行 ssh-keygen 命令时选择了使用口令保护私钥，私钥文件是经过加密的。至于公钥文件`~/.ssh/id_rsa.pub`则可以放心地公开给他人。

也可以用 ssh-keygen 命令以不同的名称创建多个公钥，当拥有多个 GitHub 账号时，非常重要。这是因为虽然一个 GitHub 账号允许使用多个不同的 SSH 公钥，但反过来，一个 SSH 公钥只能对应于一个 GitHub 账号。下面的命令在`~/.ssh`目录下创建名为 gotgithub 的私钥和名为 gotgithub.pub 的公钥文件。

```
$ ssh-keygen -C "gotgithub@gmail.com" -f ~/.ssh/gotgithub 
```

当生成的公钥/私钥对不在缺省位置（~/.ssh/id_rsa 等）时，使用 ssh 命令连接远程主机时需要使用参数-i 指定公钥/私钥对。或者在配置文件~/.ssh/config 中针对相应主机进行设定。例如对于上例创建了非缺省公钥/私钥对~/.ssh/gotgithub，可以在~/.ssh/config 配置文件中写入如下配置。

```
Host github.com
  User git
  Hostname github.com
  PreferredAuthentications publickey
  IdentityFile ~/.ssh/gotgithub 
```

好了，有了上面的准备，就将~/.ssh/gotgithub.pub 文件内容拷贝到剪切板。公钥是一行长长的字符串，若用编辑器打开公钥文件会折行显示，注意拷贝时切莫在其中插入多余的换行符、空格等，否则在公钥认证过程因为服务器端和客户端公钥不匹配而导致认证失败。命令行下可直接用 pbcopy 命令[[2]](http://www.worldhello.net/gotgithub/02-join-github/010-account-setup.html#id4)将文件内容拷贝到剪切板以避免拷贝错误：

```
$ cat ~/.ssh/gotgithub.pub | pbcopy 
```

> Mac 下的命令行工具 pbcopy 和 pbpaste 可以在命令行下操作剪贴板，Linux 下的命令行工具 xsel 亦可实现类似功能。在 Linux 下可以创建别名用 xsel 命令来模拟 pbcopy 和 pbpaste 。 alias pbcopy='xsel --input' alias pbpaste='xsel --output'

然后将公钥文件中的内容粘贴到 GitHub 的 SSH 公钥管理的对话框中，如图 2-8 所示。

![../images/setting-ssh-gotgithub.png](http://box.kancloud.cn/2015-07-09_559de02c85ecf.png)

图 2-8：添加 SSH 公钥认证

设置成功后，再用 ssh 命令访问 GitHub，会显示一条认证成功信息并退出。在认证成功的信息中还会显示该公钥对应的用户名。

```
$ ssh -T git@github.com
Hi gotgithub! You've successfully authenticated, but GitHub does not provide shell access. 
```

如果您未能看到类似的成功信息，可以通过在 ssh 命令后面添加-v 参数加以诊断，会在冗长的会话中看到认证所使用的公钥文件等信息。然后比对所使用的公钥内容是否和 GitHub 账号中设置的相一致。

```
$ ssh -Tv git@github.com
...
debug1: Authentications that can continue: publickey
debug1: Next authentication method: publickey
debug1: Offering RSA public key: /Users/jiangxin/.ssh/gotgithub
...
debug1: Entering interactive session.
Hi gotgithub! You've successfully authenticated, but GitHub does not provide shell access.
... 
```

账号设置的最后一项是向 GitHub 提供你的求职信息。GitHub 作为一个优秀程序员的聚集地，已成为一个 IT 人才招聘的途径，如果你需要找工作的话，提供简历并打开“Available for hire”选项，GitHub 会向你推荐合适的工作机会。如图 2-9 所示。

![../images/setting-job.png](http://box.kancloud.cn/2015-07-09_559de02ee2a5e.png)

图 2-9：求职信息管理

# 2.2\. 浏览托管项目

在上一节学习了如何建立 GitHub 的账户，接下来在建立自己的项目托管之前，先来看看别人是怎么做的。

在 GitHub 中搜索字符串“GotGit”，可以搜索到我建立的一个项目，项目地址是：[`github.com/gotgit/gotgit/`](https://github.com/gotgit/gotgit/)。由上至下，GotGit 项目首页可以分为如下几个区域。

*   区域一：项目概要介绍及版本库 URL 等。

    项目 GotGit 托管在组织账号 gotgit 之下(项目 gotgit 最早由用户 ossxp-com 创建，现已转移到组织 gotgit 账号之下。)，并且已经有若干关注用户和派生项目。最下面一行显示版本库的访问地址，只显示了 HTTP 和 Git-daemon 两个协议的 URL 地址，这是因为当前用户对该版本库只具有只读权限，因此没有显示 SSH 协议的 URL 地址。

    ![../images/gotgit-repo-tree-1.png](http://box.kancloud.cn/2015-07-09_559de203841e2.png)

    图 2-10：版本库概要信息

    使用任意一种协议均可克隆该 Git 版本库，但要注意只有 Git 1.6.6 及以上版本才支持智能 HTTP 协议，低版本 Git 则无法用 HTTP 协议克隆 GitHub 上的版本库[[2]](https://github.com/blog/809-git-dumb-http-transport-to-be-turned-off-in-90-days)。

```
$ git clone https://github.com/gotgit/gotgit.git 
```

```
或者使用 Git-daemon 协议。 
```

```
$ git clone git://github.com/gotgit/gotgit.git 
```

*   区域二：代码浏览子菜单及分支切换对话框。

    默认项目代码页（即项目首页）显示项目文件列表（即 Files 子菜单），如图 2-11 所示。右侧还显示项目 gotgit/gotgit 默认的分支为 gh-pages 而非常见的 master 分支。关于 gh-pages 分支，在“第 3.5.2 节 [*创建项目主页*](http://www.worldhello.net/gotgithub/03-project-hosting/050-homepage.html#project-homepage)”会介绍该分支的神奇用途。

    ![../images/gotgit-repo-tree-1-submenu.png](http://box.kancloud.cn/2015-07-09_559de204bd809.png)

    图 2-11：代码浏览子菜单及分支

*   区域三：显示最新提交的提交说明、提交用户头像、提交时间等提交信息。右侧还显示此次提交对应的提交 ID。

    ![../images/gotgit-repo-tree-2.png](http://box.kancloud.cn/2015-07-09_559de20670506.png)

    图 2-12：提交信息

*   区域四：目录树。每个目录和文件后面还显示最后一次变更的提交说明。

    ![../images/gotgit-repo-tree-3.png](http://box.kancloud.cn/2015-07-09_559de209382b0.png)

    图 2-13：目录树

*   区域五：根目录下的文件 README.md 格式化为 HTML 输出。

    GitHub 内置了多种文本标记语言的支持，如 Markdown、Textile、reStructuredText、asciidoc、Wiki 等。当发现根目录下的 README 文件后，会根据其扩展名判断所用的标记语言类型，自动转换为 HTML 格式显示。

    ![../images/gotgit-repo-tree-4.png](http://box.kancloud.cn/2015-07-09_559de20ab086e.png)

    图 2-14：README 文件

在 GitHub 的页面中可以使用键盘快捷键，按下问号（?）会在弹出窗口显示当前页面可用的快捷键。

在项目的代码浏览页按下字母“w”，弹出分支切换菜单，如图 2-15 所示。

![../images/gotgit-repo-switch-branch.png](http://box.kancloud.cn/2015-07-09_559de210b285c.png)

图 2-15：快捷键“w”切换分支

按下字母“t”，开启目录树中文件查找和过滤。图 2-16 就是在按下字母“t”后，当逐一输入单词“download”时的过滤效果。

![../images/gotgit-repo-tree-filter.png](http://box.kancloud.cn/2015-07-09_559de2115636b.png)

图 2-16：快捷键“t”开启过滤器在目录树中搜索

点击代码浏览子菜单中的“Commits”（提交）显示版本库 GotGit 的提交历史，如图 2-17 所示。

![../images/gotgit-repo-commit-history.png](http://box.kancloud.cn/2015-07-09_559de213e2992.png)

图 2-17：提交历史

提交历史页面也支持快捷键，按下问号（?）或者点击页面中的键盘标志会显示快捷键帮助。其中快捷键”j”和”k”用于在提交列表中向上和向下选择提交，在选中的提交按下回车键，会显示该提交包含的文件改动差异，如图 2-18 所示。

![../images/gotgit-repo-commit-diff.png](http://box.kancloud.cn/2015-07-09_559de2166c3a6.png)

图 2-18：文本文件改动差异

在文本文件的差异比较中，不但将有差异的行标识出来，还将行内具体改动的字词用特殊颜色进行了标识，不由得感叹 GitHub 的细致入微。

GitHub 还支持对图形文件的差异比较，并提供四种比较方式。在如下地址：[`git.io/image-diff`](http://git.io/image-diff) (短格式 URL，实际对应于: [`github.com/cameronmcefee/Image-Diff-View-Modes/commit/8e95f7)提供了一个示例提交。您可以去尝试一下不同的图形文件比较方式，以便更直观地观察图形文件前后的改动`](https://github.com/cameronmcefee/Image-Diff-View-Modes/commit/8e95f7)%E6%8F%90%E4%BE%9B%E4%BA%86%E4%B8%80%E4%B8%AA%E7%A4%BA%E4%BE%8B%E6%8F%90%E4%BA%A4%E3%80%82%E6%82%A8%E5%8F%AF%E4%BB%A5%E5%8E%BB%E5%B0%9D%E8%AF%95%E4%B8%80%E4%B8%8B%E4%B8%8D%E5%90%8C%E7%9A%84%E5%9B%BE%E5%BD%A2%E6%96%87%E4%BB%B6%E6%AF%94%E8%BE%83%E6%96%B9%E5%BC%8F%EF%BC%8C%E4%BB%A5%E4%BE%BF%E6%9B%B4%E7%9B%B4%E8%A7%82%E5%9C%B0%E8%A7%82%E5%AF%9F%E5%9B%BE%E5%BD%A2%E6%96%87%E4%BB%B6%E5%89%8D%E5%90%8E%E7%9A%84%E6%94%B9%E5%8A%A8)。

*   默认修改前后的两幅图片左右并排显示，如图 2-19 所示。

    ![../images/image-diff-2-up.png](http://box.kancloud.cn/2015-07-09_559de21a201a0.png)

    图 2-19：左右并排比较图形文件差异

*   选择交换显示比较修改前后的图片，用鼠标左右拖动进度条，可以非常直观地看到图片的差异。如图 2-20 所示。

    ![../images/image-diff-swipe.png](http://box.kancloud.cn/2015-07-09_559de21c8d6a9.png)

    图 2-20：交换显示图形文件比较差异

*   还提供洋葱皮和色差比较，自己动手试试吧。

网络图是 GitHub 的一大特色，显示一个项目的版本库被不同用户派生（Fork）后，各个版本库的派生关系。这个网络图最早使用 Flash 实现的，目前已经改为 HTML5 实现[[4]](https://github.com/blog/621-bye-bye-flash-network-graph-is-now-canvas)。图 2-21 的示例网络图来自于 Gitosis 项目[[5]](https://github.com/res0nat0r/gitosis)。

![../images/network-graph.png](http://box.kancloud.cn/2015-07-09_559de21dc8991.png)

图 2-21：Gitosis 项目网络图

Pull Requests（拉拽请求）是派生（Fork）版本库的开发者向项目贡献提交的方法。如图 2-22 所示，GotGit 项目目前没有未被处理的 Pull Request，但是可以看到有一个已经关闭的 Pull Request 请求。

![../images/gotgit-repo-pull-request.png](http://box.kancloud.cn/2015-07-09_559de21f409ae.png)

图 2-22：Pull Requests 界面

这个 Pull Request 是 GitHub 用户 agate 发现了 GotGit 脚本中一个和 ruby1.9 不兼容的 Bug，当我把 agate 派生版本库中的提交合并到 GotGit 版本库后，该 Pull Request 自动关闭。整个 Pull Request 的变更记录如图 2-23 所示。

![../images/gotgit-repo-pull-request-detail.png](http://box.kancloud.cn/2015-07-09_559de2208f576.png)

图 2-23：Pull Request 的变更历史

缺陷追踪（Issue）也是 GitHub 工作流中一个重要的组件。GotGit 项目用缺陷跟踪系统帮助维护《Git 权威指南》一书的勘误，图 2-24 可以看到当前有 2 个打开的问题和 9 个已关闭的问题。

![../images/gotgit-repo-issue.png](http://box.kancloud.cn/2015-07-09_559de225578af.png)

图 2-24：缺陷追踪

GitHub 还为项目提供报表分析。图 2-25 是 GotGit 项目中用到的开发语言分布图。

![../images/gotgit-repo-graph-lang.png](http://box.kancloud.cn/2015-07-09_559de22743c5b.png)

图 2-25：GotGit 项目开发语言分布图

图 2-26 是开发者对 GotGit 项目贡献分布图。

![../images/gotgit-repo-graph-impact.png](http://box.kancloud.cn/2015-07-09_559de2285906e.png)

图 2-26：GotGit 项目贡献分布图

# 2.3\. 社交网络

社交网络的一大特征就是用户间的相互关注，从而形成朋友圈或媒体圈，实现便捷的信息分享和传播。GitHub 支持项目级别及用户级别的关注。

关注一个项目很简单，只需点击项目名称右侧的“Watch”按钮。

![../images/watch-project.png](http://box.kancloud.cn/2015-07-09_559de24236963.png)

图 2-27：项目的关注按钮

添加对项目的关注后，点击页面左上角的“github”文字图标进入仪表板（Dashboard）页面，如图 2-28 所示。

![../images/dashboard-with-watched-prj.png](http://box.kancloud.cn/2015-07-09_559de247188a2.png)

图 2-28：关注项目在仪表板页的显示

仪表板页面的左侧显示所关注项目的最新动态，右侧会列表显示关注的项目列表。

GitHub 还可以关注用户。访问 [`github.com/mojombo`](https://github.com/mojombo) 可以看到 mojombo（GitHub 创始者之一）的用户页，关注他只需点击图 2-29 中的“Follow”按钮。从 mojombo 的用户页还可以看到 majombo 关注的开发者，可以以此扩大 GitHub 朋友圈。

![../images/github-mojombo.png](http://box.kancloud.cn/2015-07-09_559de24a740b0.png)

图 2-29：mojombo 的用户页

GitHub 仪表板页面，有一个“RSS Feed”链接，如图 2-30 所示。点击该链接可以使用 RSS 客户端（如 Google Reader）订阅，实现无需登录 GitHub 即可访问所关注的项目和人的动态。

![../images/rss-feed.png](http://box.kancloud.cn/2015-07-09_559de24e8dc97.png)

图 2-30：RSS Feed

RSS 中可能包括隐私信息，如私有版本库的更新信息，那么 RSS 订阅是如何保护个人隐私呢？难道需要口令认证么？查看一下 RSS 订阅的 URL，你会看到类似如下格式的 URL 地址：

```
https://github.com/gotgithub.private.atom?token=681a8a5a38419ecfb80f8633d4cb4e16 
```

原来 RSS 订阅用到了 API Token 进行身份认证，即保障了个人 RSS 的私密性，又避免直接使用明文口令导致的密码泄露。关于 API Token，参见本章[*第 2.1 节中相关介绍*](http://www.worldhello.net/gotgithub/02-join-github/010-account-setup.html#api-token)。