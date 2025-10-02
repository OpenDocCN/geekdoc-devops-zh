# 6\. GitHub 副产品

GitHub 最核心的产品是 Git 版本库（即项目）托管，此外 GitHub 还提供一些副产品（Side Project），通过附加的服务或技术提供了更多有趣的功能。例如提供数据粘贴的 Gist 网站，对其他版本控制工具如 SVN 和 Hg 的支持，各具特色的客户端工具，求职网站，销售纪念品的 GitHub 商店等等。

# 6.1\. GitHub:Gist

在 GitHub 网站的导航条上就有 Gist 子网站的链接： [`gist.github.com/`](https://gist.github.com/) ，在本节我们就揭开其面纱。

![../images/gist-in-github-navbar.png](http://box.kancloud.cn/2015-07-09_559de645bc10a.png)

图 6-1：GitHub 上的 Gist 链接

Gist 作为一个粘贴数据的工具，就像 Pastie 网站[[1]](http://pastie.org/)一样，可以很容易地将数据粘贴在 Gist 网站中，并在其他网页中引用 Gist 中粘贴的数据。作为 GitHub 的一个子网站，很自然地，Gist 使用 Git 版本库对粘贴数据进行维护，这非常酷。

## 6.1.1\. 数据的粘贴和引用

进入 Gist 网站的首页，就会看到一个大大的数据粘贴对话框，如图 6-2 所示。

![../images/gist-homepage.png](http://box.kancloud.cn/2015-07-09_559de6474a0d2.png)

图 6-2：Gist 网站首页

只要提供一行简单的描述、文件名，并粘贴文件内容，即可创建一个新的粘贴。创建新的粘贴时，如果不指定文件名，将由系统自动指派。如图 6-3 所示。

![../images/gist-create-new.png](http://box.kancloud.cn/2015-07-09_559de6487345c.png)

图 6-3：创建新的 Gist

每一个新的粘贴称为一个 Gist，并拥有唯一的 URL。如果选择创建公开的 Gist，URL 中将使用顺序递增的 ID 号，如本例创建的 Gist 的 URL 地址为：[`gist.github.com/1202870`](https://gist.github.com/1202870)。

若选择创建私有 Gist，URL 中则采用 20 位十六进制数字的 ID，例如私密 Gist：[`gist.github.com/78d67164131ec9e08dfe`](https://gist.github.com/78d67164131ec9e08dfe)。需要指出的是，私有 Gist 的私密性并不像 GitHub 私有版本库的私密性那么强，只是其 URL 没有在用户 Gist 列表中列出，也不能通过 Gist 网站搜索到而已。如果用户知道某私密 Gist 的 URL 地址，同样可以访问、克隆该私密 Gist，甚至创建基于该 Gist 创建分支（fork）。

当一个粘贴创建完毕后，会显示新建立的 Gist 页面，如图 6-4 所示。

![../images/gist-created.png](http://box.kancloud.cn/2015-07-09_559de64c9de03.png)

图 6-4：新创建的 Gist

点击其中的“embed”（嵌入）按钮，就会显示一段用于嵌入其他网页的 JavaScript 代码，如图 6-5 所示。

![../images/gist-embed.png](http://box.kancloud.cn/2015-07-09_559de64ee275a.png)

图 6-5：显示嵌入 Java

对应的嵌入 JavaScript 代码如下：

```
<script src="https://gist.github.com/1202870.js?file=countdown.rb"></script> 
```

将上面的 JavaScript 代码嵌入到网页（如博客[[2]](http://www.worldhello.net/2011/09/14/2521.html)）中，即可在相应的网页中嵌入来自 Gist 的数据，并保持语法加亮等功能，如图 6-6 所示。

![../images/gist-embed-in-blog.png](http://box.kancloud.cn/2015-07-09_559de6548b006.png)

图 6-6：博客中引用 Gist 数据

## 6.1.2\. Gist 背后的 Git 库

创建的每一个 Gist 的背后都对应着一个 Git 版本库。例如之前创建的 ID 为 1202870 的 Gist 对应的 Git 版本库，可以使用两种协议进行访问：

*   Git 协议：git://gist.github.com/1202870.git
*   SSH 协议：git@gist.github.com:1202870.git

可以通过 Git 命令克隆和操作该版本库。

*   克隆该 Gist 对应的版本库。

```
$ git clone git@gist.github.com:1202870.git
$ cd 1202870 
```

*   查看修改日志。每一次对 Gist 中文件的修改对应于一次提交。

```
$ git log
commit 993d28a1319eca314ab2e3f4c46882cf328e5ff9
Author: GotGitHub <gotgithub@gmail.com>
Date:   Thu Sep 15 15:41:10 2011 +0800

commit 4dd9cfd54e1522d0b62d92dd5f705a61e3fe8778
Author: GotGitHub <gotgithub@gmail.com>
Date:   Thu Sep 8 00:46:50 2011 -0700 
```

*   查看最近一次更改。

```
$ git show HEAD
commit 993d28a1319eca314ab2e3f4c46882cf328e5ff9
Author: GotGitHub <gotgithub@gmail.com>
Date:   Thu Sep 15 15:41:10 2011 +0800

diff --git a/countdown.rb b/countdown.rb
index a9d747b..9045738 100644
--- a/countdown.rb
+++ b/countdown.rb
@@ -4,4 +4,8 @@
 require 'Date'

 days=(DateTime.new(2012,10,15)-DateTime.now).ceil
-puts "Maybe #{days} days left."
\ No newline at end of file
+if days >= 0
+  puts "Maybe #{days} days left."
+else
+  puts "Passed for #{days.abs} days."
+end
\ No newline at end of file 
```

Gist 网站并没有像 GitHub 网站那样对于 Git 版本库提供完整的、近乎复杂的操作界面和工作流支持，而只提供了最基本的操作界面。如图 6-7 所示。

![../images/gist-git-repo.png](http://box.kancloud.cn/2015-07-09_559de6558578a.png)

图 6-7：Gist 版本库简易操作界面

在这个简易的 Git 版本库操作界面中，左侧是版本库的简介、文件预览以及在线编辑、下载、加注星标(对感兴趣的 Gist 进行收藏，参见博客 [`github.com/blog/673-starring-gists`](https://github.com/blog/673-starring-gists))、版本库分支(访问他人创建的 Gist 时，提供分支功能按钮。)等相关操作按钮。若以 Gist 创建者登录，会在右侧看到他人基于该 Gist 创建分支的情况，但是并不提供 GitHub 才有的 Pull Request 等功能。在界面的右侧还显示了 Gist 修订历史，和之前通过 git log 命令从 Git 版本库看到的一样。

## 6.1.3\. Greasemonkey

Gist 除了被用于粘贴数据（如代码块）并在网页中引用之外，还被用户挖掘出了新的应用模式，例如用作 Greasemonkey 脚本的维护[[5]](https://github.com/blog/302-gist-for-greasemonkey)。

Greasemonkey[[6]](https://addons.mozilla.org/en-US/firefox/addon/greasemonkey/)或类似插件为浏览器提供用户端 JavaScript 扩展功能，最早出现于 FireFox 浏览器中。其他浏览器也陆续增加了对用户端 JavaScript 的支持，如 Safari 的 NinjaKit[[7]](http://ss-o.net/safari/extension/NinjaKit.safariextz)插件，IE 的 Trixie[[8]](http://www.bhelpuri.net/Trixie/)插件，以及 Chrome 的 Greasemetal 插件(版本 4 之后的 Chrome 内置了 Greasemonkey 类似功能，无需额外插件。)。关于如何在浏览器中安装并启用相应的插件，参照相关插件网站的介绍，在此不做过多叙述。

当浏览器安装了 Greasemonkey 或类似插件之后，当访问扩展名为.user.js 的 URL 时，会将该 URL 指向的 JavaScript 脚本安装在浏览器中，当访问指定的网址时会自动调用相应的 JavaScript 脚本，修改相关网页内容或添加特效等等。

我针对《Git 权威指南》官网的测试网页写了一个 Greasemonkey 示例脚本，可以展示用户端 JavaScript 的魔法，这个用户端 JavaScript 脚本保存在 Gist 中：[`gist.github.com/1084591`](https://gist.github.com/1084591)，如图 6-8 所示。

![../images/gist-greasemonkey.png](http://box.kancloud.cn/2015-07-09_559de6566030c.png)

图 6-8：保存 Greasemonkey 用户端脚本的 Gist

该 Greasemonkey 脚本的文件名为 click_more.user.js，该文件的文件头使用特殊的注释语句为 Greasemonkey 提供相关的安装和注册信息，内容如下（为方便描述添加了行号）：

```
1  // ==UserScript==
2  // @name           Click more for toggle
3  // @namespace      gotgit
4  // @description    Add a toogle effect at the location where anchor with a click-more css.
5  // @include        http://www.worldhello.net/gotgit/demo*
6  // @include        http://gotgit.github.com/gotgit/demo*
7  // @include        http://www.ossxp.com/doc/gotgit/demo*
8  // @require        http://code.jquery.com/jquery-1.6.2.min.js
9  // ==/UserScript== 
```

其中第 5、6、7 行三条 include 语句限定了此用户端 JavaScript 脚本的应用范围，即只针对指定的 URL（使用通配符）执行该脚本。第 8 行设定脚本依赖，即该脚本依赖 jQuery，会在运行前到指定的 URL 地址加载 jQuery 脚本。

在安装该脚本前，先用浏览器访问网址[`www.worldhello.net/gotgit/demo.html`](http://www.worldhello.net/gotgit/demo.html)，看看不加载用户端 JavaScript 脚本时网页的模样。该网页中包含一个长长的网上书店列表，如图 6-9 所示。

![../images/gist-user-js-apply-before.png](http://box.kancloud.cn/2015-07-09_559de658518b9.png)

图 6-9：应用用户端 JavaScript 脚本前的网页内容

接下来开始安装该用户端 JavaScript 脚本。安装非常简单，只要点击图 6-8 的 Gist 当中的脚本文件对应的“raw”链接，即点击脚本文件原始内容链接[[10]](https://gist.github.com/raw/1084591/73c3e4dfc827732241ca753fe7bb985c14c9d7ab/click_more.user.js)即可开启安装。这是因为该 URL 以.user.js 结尾，会被 Greasemonkey（或类似插件）识别并安装，如图 6-10 是 Greasemonkey 弹出的用户端脚本安装界面。

![../images/gist-user-js-install.png](http://box.kancloud.cn/2015-07-09_559de65ae95d1.png)

图 6-10：安装用户端 JavaScript 脚本

用户端脚本安装完毕后，再访问同样的测试网页[`www.worldhello.net/gotgit/demo.html`](http://www.worldhello.net/gotgit/demo.html)，会发现网页中出现了一个名为“更多”的可点击链接，长长的网上书店列表不见了。如图 6-11 所示。

![../images/gist-user-js-apply-after.png](http://box.kancloud.cn/2015-07-09_559de65cd5e01.png)

图 6-11：应用用户端 JavaScript 脚本后的网页内容

如果查看网页源码，会发现该网页中根本没有包含和调用任何 JavaScript 脚本，只是在页面源码中包含着一个没有任何实质输出的标签：

```
<p><a class="click-more"></a></p> 
```

实际上正是这个特殊的标签被 Greasemonkey 所加载的用户端脚本识别，为 HTML 网页添加了特效。

## 6.1.4\. 命令行操作 Gist

GitHub 开发者还写了一个名为 gist 的命令行工具对 Gist 进行操作，地址见[`github.com/defunkt/gist`](https://github.com/defunkt/gist)。

该工具使用 Ruby 开发，对两个特定的 Git 风格的配置变量进行如下设置后，即可实现在命令行中自动以特定用户身份登录操作 Gist。

```
$ git config --global github.user "your-github-username"
$ git config --global github.token "your-github-token" 
```

其中 github.token 中保存的是用户的 API TOKEN，这在“2.1 创建 GitHub 账号”一节有过介绍。

使用 gist 命令行工具创建新的 Gist 非常简单。

*   创建包含一个文件（如 script.py）的 Gist，使用如下命令。

```
$ gist script.py 
```

*   创建包含多个文件的 Gist，使用类似如下的命令。

```
$ gist script.js notes.txt 
```

如果对命令行操作方式感兴趣，参考 gist 工具网站的 README 文件。

# 6.2\. 其他版本控制工具支持

如果曾经有人对 GitHub 只提供唯一的版本库格式（Git）托管表示过怀疑的话，那么今天看到 GitHub 对其他版本控制工具提供的愈发完善的支持，争议应该烟消云散了吧。

# 6.2.1\. 用 SVN 操作 GitHub

2008 年 4 月 1 日，GitHub 宣布推出基于 SVN 的 SVNHub 网站，后证实这是一个愚人节玩笑[[1]](https://github.com/blog/31-back-to-subversion)。2010 年愚人节，类似消息再起，可这一次不再是玩笑[[2]](https://github.com/blog/626-announcing-svn-support)。即对于 GitHub 上的每一个 Git 版本库，现在都可以用 SVN 命令进行操作。更酷的是 SVN 版本库使用的是和 Git 版本库同样的地址[[3]](https://github.com/blog/966-improved-subversion-client-support)。

例如用下面的 Git 命令访问本书的 Git 版本库，显示版本库包含的引用。其中分支 master 用于维护书稿，分支 gh-pages 保存书稿编译后的 HTML 网页用于在 GitHub 上显示。

```
$ git ls-remote --heads https://github.com/gotgit/gotgithub
ce5d3dda9b9ce8ec90def1da10181a094bea152f        refs/heads/gh-pages
c4d370b1b0bafb103de14e104ca18b8c31d80add        refs/heads/master 
```

如果使用 SVN 命令访问相同的版本库地址，Git 服务器变身为一个 SVN 服务器，将 Git 的引用对应为 SVN 风格的分支。如下：

```
$ svn ls https://github.com/gotgit/gotgithub
branches/
trunk/
$ svn ls https://github.com/gotgit/gotgithub/branches
gh-pages/ 
```

SVN 支持部分检出，下面命令将整个主线 trunk（相当于 Git 版本库的 master 分支）检出。

```
$ svn checkout https://github.com/gotgit/gotgithub/trunk gotgithub
A    gotgithub/Makefile
A    gotgithub/README.rst
...
Checked out revision 30. 
```

还可以使用 SVN 命令创建分支，即相当于在 Git 版本库中创建新的引用。测试发现 GitHub 尚不支持 SVN 远程拷贝创建分支，需要通过本地拷贝再提交的方式创建新分支。操作如下：

1.  为避免检出版本库所有分支过于耗时，在检出时使用--depth=empty 参数。

```
$ svn checkout --depth=empty \
  https://github.com/gotgit/gotgithub gotgithub-branches
Checked out revision 30. 
```

1.  进入到检出目录中，更新出 trunk 和 branches 两个顶级目录。

```
$ cd gotgithub-branches
$ svn up --depth=empty trunk branches
A    trunk
Updated to revision 30.
A    branches
Updated to revision 30. 
```

1.  通过拷贝从主线 trunk 创建分支 branches/svn-github。

```
$ svn cp trunk branches/svn-github
A         branches/svn-github
$ svn st
A  +    branches/svn-github 
```

1.  提交完成分支创建。

```
$ svn ci -m "create branch svn-github from trunk"
Authentication realm: <https://github.com:443> GitHub
Username: gotgithub
Password for 'gotgithub':
Adding         branches/svn-github

Committed revision 31. 
```

1.  用 Git 命令可以看到服务器上创建了一个新的同名引用，并且指向和 master 一致。

```
$ git ls-remote --heads https://github.com/gotgit/gotgithub
ce5d3dda9b9ce8ec90def1da10181a094bea152f        refs/heads/gh-pages
c4d370b1b0bafb103de14e104ca18b8c31d80add        refs/heads/master
c4d370b1b0bafb103de14e104ca18b8c31d80add        refs/heads/svn-github 
```

下面尝试一下用 SVN 命令在新创建的分支 svn-github 中提交。

1.  进入到之前检出完整主线 trunk 的 gotgithub 目录，并将工作区切换为分支 branches/svn-github。

```
$ cd ../gotgithub
$ svn switch https://github.com/gotgit/gotgithub/branches/svn-github
At revision 31. 
```

1.  修改文件，查看工作区状态。

```
$ svn st
M       06-side-projects/040-svn.rst 
```

1.  用 SVN 提交。

```
$ svn ci -m "GitHub svn client support improved. Refs: http://git.io/svn"
Sending        06-side-projects/040-svn.rst
Transmitting file data .
Committed revision 32. 
```

1.  同样查看 Git 版本库的更新，会发现 svn-github 分支的指向已和 master 不同。

```
$ git ls-remote --heads https://github.com/gotgit/gotgithub
ce5d3dda9b9ce8ec90def1da10181a094bea152f        refs/heads/gh-pages
c4d370b1b0bafb103de14e104ca18b8c31d80add        refs/heads/master
64b80cb5331e28fdfb896e2ab3085779bf6ca019        refs/heads/svn-github 
```

# 6.2.2\. 用 Hg 操作 GitHub

Hg（又称 Mercurial）和 Git 一样也是一个被广泛使用的分布式版本库控制工具。如果一个熟悉 Hg 的开发者参与托管在 GitHub 上的项目，大可不必为更换版本控制工具而苦恼，GitHub 上的一个名为 hg-git[[1]](https://github.com/schacon/hg-git)的开源项目可以帮上忙。

得益于 Hg 的强大的插件扩展机制，安装 hg-git 并将其注册为 Hg 插件后可提供 Hg 操作 Git 版本库的能力。安装 hg-git 可以直接使用 **easy_install** 命令：

```
$ sudo easy_install hg-git 
```

还可以直接从 GitHub 上下载 hg-git 最新代码进行安装：

```
$ curl -L -k -o hg-git.zip https://github.com/schacon/hg-git/zipball/master
$ unzip hg-git.zip
$ cd schacon-hg-git-*
$ sudo easy_install . 
```

插件 hg-git 依赖于 Dulwich 项目，如果在安装过程遇到 Dulwich 无法编译，可能是因为缺乏 C 编译器，或者尚未安装 python-dev 软件包。Dulwich 是一个 Python 语言的 Git 实现，因此 hg-git 在运行过程中无需 Git 命令行。

和其他 Hg 插件类似，安装完毕后需要修改 Hg 配置文件（如文件 ~/.hgrc ）如下，以启用 hg-git 插件以及另外一个必须的 Hg 内置插件 —— bookmarks 插件。

```
[extensions]
bookmarks =
hggit = 
```

对 hg-git 安装配置完毕，就可以使用 Hg 操作 Git 版本库了。

*   克隆 Git 版本库。

```
$ hg clone git://github.com/ossxp-com/hello-world.git
$ cd hello-world 
```

*   Git 版本库的分支转换为 Hg 版本库中的 bookmarks。

    新克隆的 Hg 版本库默认会更新到最新提交（即 tip 版本），未必处于所需的分支上。用命令**hg bookmarks**显示分支列表，命令**hg parents**显示工作区对应的版本。

```
$ hg bookmarks
   helper/master             10:2767ad9d7008
   helper/v1.x               8:994c2f0adc0b
   master                    1:dcd365e3175c

$ hg parents
修改集:      12:928384ca1e87
标签:        jx/v1.0-i18n
标签:        tip
用户:        Jiang Xin <jiangxin@ossxp.com>
日期:        Fri Dec 31 12:12:42 2010 +0800
摘要:        Translate for Chinese. 
```

*   切换到所需的工作分支（如 master 分支）。

    用 hg update -r 命令切换分支。之后执行**hg bookmarks**命令会看到当前工作分支用星号标识出来。

```
 $ hg update -r master
    $ hg book
       helper/master             10:2767ad9d7008
       helper/v1.x               8:994c2f0adc0b
     * master                    1:dcd365e3175c 
```

*   Git 的里程碑也被记录，并可被 **hg tags** 命令显示。

```
 $ hg tags
    tip                               12:928384ca1e87
    jx/v1.0-i18n                      12:928384ca1e87
    jx/v2.3                           10:2767ad9d7008
    ... 
```

*   使用**hg pull**命令和**hg push**命令可以实现和 Git 版本库的同步。

*   有的命令如**hg outgoing**可在 1.7 版本的 Hg 中运行正常，但对于高版本库的 Hg 存在兼容性问题。

实际上 hg-git 插件并非只针对 GitHub 的版本库，而是可以支持任意 Git 版本库包括本地 Git 版本库。为了提供对 Git 版本库的透明支持，对 Git 版本库的 URL 的写法有特殊要求，即要能够从协议名称区分开 Git 版本库和默认的 Hg 版本库。

*   Git 协议：

    git://example.com[:port]/path/to/repo.git

*   SSH 协议：

    git+ssh://[user@]example.com[:port]/path/to/repo.git

*   HTTP 协议：

    git+[http://[user@]example.com[:port]/path/to/repo.git](path/to/repo.git)

*   HTTPS 协议：

    git+

*   本地协议：

    /path/to/repo.git

# 6.3\. 客户端工具

GitHub 提供的 Web 服务，在客户端通常只需要浏览器及 Git 命令行工具就可以满足需要了。而 GitHub 还开发了一些客户端工具，以便用户有更好的客户端体验。

## 6.3.1\. github:mac

GitHub 专为 Mac 用户开发了一款图形化客户端应用 github:mac，在 Mac 下操作 GitHub 更简单。软件下载地址： [`mac.github.com/`](http://mac.github.com/) 。

github:mac 可以实现版本库克隆、查看历史、提交、分支管理、与 GitHub 同步等功能。图 6-12 展示的是提交界面，在提交界面中同时显示了变更的差异比较，用户可以挑选文件中的部分变更进行提交，显然这个操作要比在命令行中执行**git add –patch**或**git commit –patch**要更加直观。

![../images/mac-partial-commit.png](http://box.kancloud.cn/2015-07-09_559de6e25fdb3.png)

图 6-12：筛选文件中的部分更改进行提交

github:mac 和 GitHub 深度集成，当配置好关联的 GitHub 账号后，会自动在本地创建专用的 SSH 公钥/私钥对文件 ~/.ssh/github_rsa （如果该文件不存在的话），然后将公钥文件传递到 GitHub 网站并自动完成配置。新增的 SSH 公钥文件显示在 GitHub 网站的账号设置中，如图 6-13 所示：

![../images/mac-new-ssh-pubkey.png](http://box.kancloud.cn/2015-07-09_559de6f520c12.png)

图 6-13：在 GitHub 上自动添加的 SSH 公钥

同时 github:mac 还在本地将新生成的私钥文件添加到 ssh-agent 认证代理中，这样一旦通过 SSH 协议连接 GitHub，首先采用该公钥/私钥对进行身份认证。用下面的命令可以查看添加到 ssh-agent 中的私钥文件。

```
$ ssh-add -l
2048 aa:01:4f:d2:14:ba:5f:9f:8c:dc:b5:9d:44:cd:8e:18 /Users/jiangxin/.ssh/github_rsa (RSA) 
```

这种透明的公钥认证管理非常酷，对于大多数只使用唯一一个 GitHub 账号的用户来说是非常方便的。但如果用户拥有多个 GitHub 账号并需要不时切换账号，这种实现却很糟糕，会导致认证错误。因为当 ssh-agent 认证代理缓存了私钥后，连接由文件~/.ssh/config 设置的 SSH 别名主机无法使用指定的公钥/私钥对进行认证，导致认证失败。

遇到 GitHub 账户 SSH 认证问题，可以运行下面命令清空 ssh-agent 缓存的私钥。

```
$ ssh-add -d ~/.ssh/github_rsa
Identity removed: /Users/jiangxin/.ssh/github_rsa (/Users/jiangxin/.ssh/github_rsa.pub) 
```

## 6.3.2\. hub

对于命令行用户，GitHub 提供了名为 hub 的命令行工具，对 Git 进行了简单的封装。该项目在 GitHub 上的地址为： [`github.com/defunkt/hub`](https://github.com/defunkt/hub) 。

使用 hub 可以在命令行中简化对 GitHub 的操作。例如克隆本电子书的版本库，若用 hub 命令，地址可大大简化：

```
$ hub clone gotgit/gotgithub 
```

若要在自己账号下创建派生项目，无需登录 GitHub 网站，直接通过命令行即可实现：

```
$ cd gotgithub
$ hub fork 
```

安装 hub 很简单，可使用如下方法任意一种方法。

*   克隆 hub 的版本库，从源码安装。安装步骤如下：

```
$ git clone git://github.com/defunkt/hub.git
$ cd hub
$ rake install prefix=/usr/local 
```

*   用 RubyGems 包方式安装。

    hub 用 Ruby 开发，也可用 RubyGems 包方式安装。需要注意，在安装完毕后最好将 hub 打包为一独立运行脚本，以便运行时不再靠 RubyGems 加载，提高加载速度。安装步骤如下：

```
$ gem install hub
$ hub hub standalone > ~/bin/hub && chmod 755 ~/bin/hub 
```

安装完毕后，还需要对 hub 进行设置。定义两个 Git 风格的配置变量，以便 hub 命令能确定当前 GitHub 用户账号，并能够完成所需的 GitHub API 认证。

```
$ git config --global github.user "your-github-username"
$ git config --global github.token "your-github-token" 
```

其中 github.token 中保存的是用户的 API TOKEN，这在“2.1 创建 GitHub 账号”一节有过介绍。

在使用 hub 过程中，如果要为区分哪些命令是 git 的，哪些是 hub 的，而不断在两个命令间切换显然太不方便了。hub 命令支持以系统别名 git 的方式运行，即设置 hub 的系统别名为 git，然后只需执行 git 命令，这样无论是 git 本身的命令还是 hub 扩展的命令都可正常运行。但要注意要用系统提供的别名方式，而不能把 hub 脚本改名为 git，因为 hub 只是简单地对 Git 进行封装，运行时仍依赖 git 命令。在 bash 环境下建立别名可运行如下命令：

```
$ alias git=hub 
```

其他 shell 环境下如何建立系统别名呢？运行 hub alias 命令查看相关 shell 环境下建立别名的方法。例如对于 csh：

```
$ hub alias csh
Run this in your shell to start using `hub` as `git`:
  alias git hub 
```

下面介绍 hub 的常用命令，节选自 hub 的项目页[[1]](https://github.com/defunkt/hub#readme)。示例使用了别名命令 git 调用，并把对应的原始的 git 命令写在命令的下面（用提示符>表示，方括号中是说明）。

*   git create

    在 GitHub 上创建项目。

```
$ git create -d '项目表述'
> [ 在 GitHub 上创建版本库 ]
> git remote add origin git@github.com:YOUR_USER/CURRENT_REPO.git

$ git create recipes
> [ 在 GitHub 上创建版本库 ]
> git remote add origin git@github.com:YOUR_USER/recipes.git

$ git create sinatra/recipes
> [ 在组织账号 sinatra 下创建版本库 ]
> git remote add origin git@github.com:sinatra/recipes.git 
```

*   git clone

    克隆版本库可使用 URL 简写，即“用户名/版本库”格式地址会自动扩展为 Git 协议（只读）地址或 SSH 协议（可写）地址。

```
$ git clone schacon/ticgit
> git clone git://github.com/schacon/ticgit.git

$ git clone -p schacon/ticgit
> git clone git@github.com:schacon/ticgit.git

$ git clone resque
> git clone git@github.com/YOUR_USER/resque.git 
```

*   git fork

    在 GitHub 自己账号下建立派生项目。

```
$ git fork
> [ 先在 GitHub 上建立派生项目 ]
> git remote add -f YOUR_USER git@github.com:YOUR_USER/CURRENT_REPO.git 
```

*   git pull-request

    打开编辑器输入标题和内容，然后在 GitHub 上创建 Pull Request。

*   git remote add

    设置远程版本库。和 git clone 命令一样支持 URL 简写。

```
$ git remote add rtomayko
> git remote add rtomayko git://github.com/rtomayko/CURRENT_REPO.git

$ git remote add -p rtomayko
> git remote add rtomayko git@github.com:rtomayko/CURRENT_REPO.git

$ git remote add origin
> git remote add origin git://github.com/YOUR_USER/CURRENT_REPO.git 
```

*   git fetch

    获取他人同名版本库。自动建立远程版本库并获取提交。

```
$ git fetch mislav
> git remote add mislav git://github.com/mislav/REPO.git
> git fetch mislav

$ git fetch mislav,xoebus
> git remote add mislav ...
> git remote add xoebus ...
> git fetch --multiple mislav xoebus 
```

*   git cherry-pick

    获取远程提交，并拣选至本地版本库。

```
$ git cherry-pick http://github.com/mislav/REPO/commit/SHA
> git remote add -f mislav git://github.com/mislav/REPO.git
> git cherry-pick SHA 
```

*   git am, git apply

    获取 Pull Request，并应用于本地版本库。

```
$ git am https://github.com/defunkt/hub/pull/55
> curl https://github.com/defunkt/hub/pull/55.patch -o /tmp/55.patch
> git am /tmp/55.patch 
```

*   git browse

    打开浏览器访问相应的 URL 地址。

```
$ git browse
> open https://github.com/YOUR_USER/CURRENT_REPO

$ git browse -- commit/SHA
> open https://github.com/YOUR_USER/CURRENT_REPO/commit/SHA

$ git browse -- issues
> open https://github.com/YOUR_USER/CURRENT_REPO/issues

$ git browse resque
> open https://github.com/YOUR_USER/resque

$ git browse schacon/ticgit
> open https://github.com/schacon/ticgit

$ git browse schacon/ticgit commit/SHA
> open https://github.com/schacon/ticgit/commit/SHA 
```

*   git help hub

    查看 hub 命令的帮助。

## 6.3.3\. iOS 应用

GitHub 还为 iOS 平台开发了应用，这样就可以在 iPhone、iPad 等苹果设备上实时跟踪 GitHub 上的项目了。在苹果 AppStore 上搜索 GitHub 公司的应用，可以找到 GitHub Issues 和 GitHub Jobs 等应用，如图 6-14 所示。

![../images/ios-apps.png](http://box.kancloud.cn/2015-07-09_559de72abe08f.png)

图 6-14：iPhone 上的 issues 应用

在 iPhone 中安装 GitHub Issues 应用，就可以随时查看所关注的 GitHub 项目的问题报告和 Pull Request 等，如图 6-15 所示。

![../images/ios-issues-iphone.png](img/c09a3614.png)

图 6-15：iPhone 上的 GitHub Issues 应用

而 GitHub Jobs 应用则和即将要介绍的 GitHub 招聘网站有关，用于浏览招聘信息。

# 6.4\. 其他

## 6.4.1\. GitHub:Jobs

GitHub 求职网站[[1]](http://jobs.github.com/)，于 2010 年 8 月开通，提供求职招聘服务[[2]](https://github.com/blog/694-github-jobs-launches-tomorrow)。还记得在“第 2.1 节 创建 GitHub 账号”介绍的相关内容么？当用户在个人设置中对简历和求职状态进行设置和启用后，GitHub 就会帮助用户寻找合适的工作机会，而工作机会就来自于 GitHub 的求职网站（如图 6-16 所示）。

![../images/jobs-homepage.png](http://box.kancloud.cn/2015-07-09_559de749bd74d.png)

图 6-16：GitHub 求职网站

个人用户除了开启求职状态坐等通知外，还可以主动出击，直接到 GitHub 求职网站上寻找合适的工作机会，整个求职过程是免费的。而作为企业主发布招聘启示则是收费服务，发布招聘启示的流程如图 6-17 所示。

![../images/jobs-post-process.png](http://box.kancloud.cn/2015-07-09_559de74d256ae.png)

图 6-17：企业主发布招聘启示流程

企业发布招聘启示，首先要按照模版填写职位说明及留下供求职者投递简历的邮件地址，然后用信用卡付费，每一个招聘启示的付费标准为 350 美元/月。一旦付费完成招聘马上生效。GitHub 作为程序员的聚集地，无疑是招聘和应聘的理想之地。

## 6.4.2\. GitHub:Shop

GitHub 商店[[1]](http://shop.github.com/)销售着一些你在其他地方买不到的小东西——如印着 GitHub 吉祥物 Octocat 的纪念品，图 6-18 展示的就是一款印着 Octocat 的杯子。

![../images/shop-octocat-mug.jpg](http://box.kancloud.cn/2015-07-09_559de7627f55d.jpg)

图 6-18：印着 Octocat 吉祥物的杯子

图 6-19 则是 GitHub 热卖的一款体恤的前后两面的图案设计[[2]](https://github.com/blog/676-fork-you)。体恤前面印着 GitHub 社区编程最核心的理念（fork you），体恤后面则可以用记号笔写下你在 GitHub 上的主页地址。

![../images/shop-fork-you-shirt.png](http://box.kancloud.cn/2015-07-09_559de7642752a.png)

图 6-19：超酷的 GitHub 体恤

GitHub 商店实际上是架设于 Shopify[[3]](http://www.shopify.com/)电子商务网站上的网店。GitHub 商店并非 GitHub 主业，销售纪念品可以增强 GitHub 用户的认同感，而 GitHub 粉丝可以购买一项“装备”让自己看起来更酷。

## 6.4.3\. GitHub 短网址服务

在“第 2.2 节 浏览托管项目”一节介绍图形文件差异比较时，需要给出一个网址，但这个网址很长。如下：

[`github.com/cameronmcefee/Image-Diff-View-Modes/commit/8e95f70c9c47168305970e91021072673d7cdad8`](https://github.com/cameronmcefee/Image-Diff-View-Modes/commit/8e95f70c9c47168305970e91021072673d7cdad8)

很自然地想到了 Google 短网址服务，于是由上面的长网址生成出一个短小精干的网址：[`goo.gl/Gy85b`](http://goo.gl/Gy85b)，访问该短网址会自动重定向到对应的长网址。

2011 年 11 月，GitHub 也推出了自己的短网址服务[[1]](http://git.io/help)，为 GitHub 自身网址提供短网址转换服务。GitHub 短网址服务没有像 Google 短网址服务那样提供基于 Web 的图形化转换界面，而是需要用命令行进行网址转换。

例如对于网址 [`github.com/blog/985-git-io-github-url-shortener`](https://github.com/blog/985-git-io-github-url-shortener) 的转换，使用 curl 命令如下操作。

*   将长网址转换为短网址。

    命令 curl 输出中的 Location:语句即是转换后的短网址。

```
$ curl -i http://git.io -F 'url=https://github.com/blog/985-git-io-github-url-shortener'
...
HTTP/1.1 201 Created
...
Location: http://git.io/help 
```

*   查看短网址对应的原网址，同样使用 curl 命令。

    命令 curl 输出 302 重定向地址即为原始网址。

```
$ curl -i http://git.io/help
HTTP/1.1 302 Found
...
Location: https://github.com/blog/985-git-io-github-url-shortener 
```

为使转换的短网址更易于记忆和识别，可在 curl 命令中用 code 参数设定期望的短网址。例如下面命令将本节一开始提到的长网址转换为短网址：[`git.io/image-diff`](http://git.io/image-diff)。

```
$ curl -i http://git.io -F \
  'url=https://github.com/cameronmcefee/Image-Diff-View-Modes/commit/8e95f70c9c47168305970e91021072673d7cdad8' \
  -F 'code=image-diff'
...
HTTP/1.1 201 Created
...
Location: http://git.io/image-diff 
```

## 6.4.4\. GitHub Open Source

GitHub 已成为新的开源项目大本营，而且 GitHub 也将其 API 开放，并将部分模块开源，借助社区的力量让 GitHub 变得更好。

GitHub 大部分的开源项目托管在其官方账号下： [`github.com/github`](https://github.com/github) 。

### API 接口

GitHub 通过域名 api.github.com 提供 API 接口，数据以 JSON 格式传递。

详细的 API 参考手册参见网址： [`developer.github.com/`](http://developer.github.com/) 。API 手册的版本库地址： [`github.com/github/developer.github.com`](https://github.com/github/developer.github.com) 。

### 官方手册

GitHub 官方手册参见 [`help.github.com/`](http://help.github.com/) ，使用 Jekyll 维护。

项目地址： [`github.com/github/help.github.com`](https://github.com/github/help.github.com) 。

### Grit

Grit 是 Git 的 Ruby 封装和实现，是 GitHub 调用 Git 的接口。部分是通过封装对 git 命令的调用实现的，部分则是纯 Ruby 实现。

项目地址： [`github.com/mojombo/grit`](https://github.com/mojombo/grit) 。

### GitHub Services

Git 版本库推送会触发服务器端 post-receive 钩子脚本。此项目将 GitHub 的服务器端钩子脚本开源，用户可以开发针对特定应用的钩子。GitHub 还为其他 GitHub 应用提供了事件接口，如问题变更、Pull Request、维基页面修改等[[1]](https://github.com/blog/964-all-of-the-hooks)。

项目地址： [`github.com/github/github-services`](https://github.com/github/github-services) 。

### Hubot 和 Hubot Scripts

可以把 hubot[[2]](http://hubot.github.com/)看做是 GitHub 的 Siri（最早出现于 iPhone 4S 的智能语音助理）或是新浪微博上的饮水姬[[3]](http://weibo.com/u/2625288792)。GitHub 将 hobot 和 Campfire 聊天室整合，hobot 被聊天室会话触发可以实现诸如：打开办公室的门、根据 wifi 使用情况列出公司中的人、通过公司喇叭读一段信息等等许多好玩的事情[[4]](http://zachholman.com/posts/why-github-hacks-on-side-projects/)，而实现 GitHub 自动化部署则证明 hubot 可以完成更严肃的事情，在公司工作流中扮演举足轻重的地位[[5]](http://scottchacon.com/2011/08/31/github-flow.html#6__deploy_immediately_after_review)。

Hobot 已经开源，项目库地址：[`github.com/github/hubot`](https://github.com/github/hubot)和[`github.com/github/hubot-scripts`](https://github.com/github/hubot-scripts)（脚本）。

### Gollum

GitHub 以 Git 为后端的维基系统就是由 Gollum 实现的。每一个维基网页对应于一个文件，文件格式可以是 Markdown、textile、rdoc、org、creole、mediawiki、reStructuredText、asciidoc、pod 等。Gollum 调用名为 github-markup 的 Ruby gem 包（来自于下面要介绍的 Markup 项目）完成文件到网页的格式转换。

项目地址： [`github.com/github/gollum`](https://github.com/github/gollum) 。

关于 GitHub 维基参见本书“第 4.6 节维基”。

### Jekyll

Jekyll 是一个简单的、支持博客的静态网站编译器。可以使用 Markdown 和 Textile 两种标记语言或者 HTML 撰写网页，并支持 Liquid 模版。实际上 GitHub 为托管项目生成静态网页使用的就是 Jekyll。

项目地址： [`github.com/mojombo/jekyll`](https://github.com/mojombo/jekyll) 。

### Linguist

Linguist 是一个 Ruby 模块，GitHub 使用该模块对数据文件进行语义分析，检测文件的语言种类，代码加亮，对二进制文件进行忽略，限制非必须的差异显示，以及生成语言分类图等。

项目地址： [`github.com/github/linguist`](https://github.com/github/linguist) 。

### Markup

GitHub 通过这个 ruby 包对项目版本库根目录下的 README 文件，以及维基页面等文件进行解析、转换为网页显示。支持 Markdown、textile、rdoc、org、creole、mediawiki、reStructuredText、asciidoc、pod 等标记语言。实际上在对上述标记语言的解析和转换中，还依赖其他软件包，例如对于 Markdown 格式首选 Redcarpet(Redcarpet 是对一个高效的 Markdown 解析器，通过对 C 语言的 Sundown 库封装实现。项目地址：[`github.com/tanoku/redcarpet`](https://github.com/tanoku/redcarpet)。)，对 textile 格式使用 RedCloth，对 reStructuredText 格式调用外部命令 rst2html，对 asciidoc 格式调用外部命令 asciidoc 等。

项目地址： [`github.com/github/markup`](https://github.com/github/markup) 。

关于 Markup 软件包以及其他 GitHub 扩展的 Markdown 语法，参见：[`github.github.com/github-flavored-markdown`](http://github.github.com/github-flavored-markdown)。

### Resque

Resque（发音类似 “rescue”）是一个以 Redis 为后端的 Ruby 包，用于创建和管理后台任务。可创建任务，将任务分配到多个队列，并在后台执行任务。

项目地址： [`github.com/defunkt/resque`](https://github.com/defunkt/resque) 。

### GitPad

这是一个运行于 Windows 下类似 Notepad.exe 的应用程序，安装此应用后在 Windows 下做 Git 提交操作会调用类似记事本（Notepad）的应用撰写提交说明。

项目地址： [`github.com/github/GitPad`](https://github.com/github/GitPad) 。

### Maven Plugins

GitHub 的 Maven 插件。

项目地址： [`github.com/github/maven-plugins`](https://github.com/github/maven-plugins) 。

### Gitignore

集合了针对各种语言环境的.gitignore（忽略文件）模版。例如其中针对 VisualStudio 的忽略文件模版 Global/VisualStudio.gitignore 部分内容如下：

```
# User-specific files
*.suo
*.user
*.sln.docstates

# Build results
[Dd]ebug/
[Rr]elease/ 
```

项目地址： [`github.com/github/gitignore`](https://github.com/github/gitignore) 。

### Media

提供 GitHub 网站 Logo 和吉祥物 Octocat 的图片，只能在授权范围内使用。

项目地址： [`github.com/github/media`](https://github.com/github/media) 。