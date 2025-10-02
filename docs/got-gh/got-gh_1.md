# 1\. 探索 GitHub

熟悉[Git](http://git-scm.com)的人几乎都知道并喜欢[GitHub](https://github.com)，反过来 GitHub 也吸引更多的人来使用 Git。GitHub 正在成为开源项目托管的主要平台，是什么成就了 GitHub？

> 也可以参考：
> 
> 本书并非一本介绍 Git 的书，并且假设读者已经掌握了 Git 的相关操作。如果读者对 Git 尚不了解，可以参考我写的 [《Git 权威指南》] [注 1]一书。此外还可以从网上找到很多免费的、很好的 Git 资料，如：[Git 社区书](http://book.git-scm.com)、[Pro Git](http://progit.org/book/)等。

* * *

[注 1]: ISBN：9787111349679, 由机械工业出版社华章公司于 2011 年 7 月出版。

# 1.1\. 什么是 GitHub

GitHub（网址 [`github.com/`](https://github.com/)）是一个面向开源及私有软件项目的托管平台，因为只支持 Git 作为唯一的版本库格式进行托管，故名 GitHub。

GitHub 的注册用户已经超过百万[[1]](https://github.com/blog/936-one-million)，托管的版本库数量已超三百万，其中不乏知名的开源项目，如：Ruby on Rails[[2]](https://github.com/rails/rails)、Hibernate[[3]](https://github.com/hibernate)、phpBB[[4]](https://github.com/phpbb/phpbb3)、jQuery[[5]](https://github.com/jquery/jquery)、Prototype[[6]](https://github.com/sstephenson/prototype)、Homebrew[[7]](https://github.com/mxcl/homebrew)等。

GitHub 于 2008 年 4 月 10 日正式发布[[8]](https://github.com/blog/40-we-launched)，相比始于 1999 年的 SourceForge[[9]](http://sourceforge.net/)和 2005 年的 GoogleCode[[10]](http://code.google.com/)，GitHub 后来者居上。以 2011 年的数据从代码提交数量上看，GitHub 已经超越其前辈[[11]](http://www.slideshare.net/sogrady/survival-of-the-forges)，如图 1-1 所示。

![../images/survival-of-the-forges.png](http://box.kancloud.cn/2015-07-09_559dce78adf37.png)

图 1-1：开源项目托管平台提交数量对照

对于一个开源项目，从开发角度讲大体上分为两类人群，一类称为核心开发团队，他们可以向保存源代码的版本库提交，即对源代码的修改具有最终的决定权。另外一类称为贡献者，他们不属于核心开发团队，虽然也能看到源代码，但无权向版本库提交修改。

采用传统的集中式版本控制系统（如 SVN）的开源项目，这两个群体的用户体验都不是太好。如图 1-2 所示，项目的贡献者（非核心成员）很不“高兴”，因为他们即便有修改源代码的能力和渴望，也不能直接向版本库提交，要想成为提交者需要一个很长的建立信任的过程。然而即便是核心开发团队的成员，体验也不是太好，因为凡是涉及到版本库的操作（检入、检出、查看日志等）都需要在联网的状态下进行，网络带宽对用户体验影响相当大。

![../images/cvcs-model.png](http://box.kancloud.cn/2015-07-09_559dce7bef014.png)

图 1-2：使用集中式版本控制系统

Git 等分布式版本控制系统的出现，彻底颠覆了原有代码管理的组织模式。使用 Git，不再依赖唯一的、集中式的版本库，而是每个开发者本地都拥有一份完整的版本库。Git 并不排斥集中式的使用模式，但更倾向于将集中式版本库称为共享版本库。核心开发团队的成员和贡献者（非核心成员）都可以从共享版本库克隆一份本地版本库，但只有核心团队成员才可以将自己本地版本库的提交推送到共享版本库上。如图 1-3 所示。

![../images/dvcs-model.png](http://box.kancloud.cn/2015-07-09_559dce87daaf1.png)

图 1-3：使用分布式版本控制系统

使用 Git 做版本控制（如图 1-3 所示），核心开发团队非常“高兴”，因为他们和共享版本库之间不必一直保持连接状态，诸如查看日志、提交、创建分支等几乎全部操作都（脱离网络）在本地的版本库中完成。项目贡献者（非核心成员）也不再那么沮丧，因为版本库人人皆可更改（当然是对本地版本库而言）。稍微让贡献者感到困难的就是如何将自己对项目的改进被核心开发团队所了解并接纳。Git 提供了多种途径，一个方法是先用 git format-patch 命令将本地提交转换为补丁文件或补丁文件序列，再通过邮件发送给核心开发团队。另外一个办法就是搭建一个自己专有的共享版本库，通过邮件创建一个拉拽请求（Pull Request），让核心团队的开发者到自己的版本库来抓取（Pull）。

GitHub 的出现进一步推动了 Git 的普及，简化了版本控制的管理和操作流程，为开发者提供了更好的交流平台。如图 1-4 所示。

![../images/github-model.png](http://box.kancloud.cn/2015-07-09_559dce8da565d.png)

图 1-4：GitHub 的协同模式

使用 GitHub，无论是项目的核心开发团队，还是普通的项目贡献者都工作得非常“愉快”。创建项目变得非常轻松，创建者只需在 GitHub 上点击一下鼠标即可创建一个新版本库，通过简单的 Web 操作即可完成项目授权进而组建项目核心团队。在 GitHub 中，非核心团队成员参与项目也很容易。先找到自己希望参与的项目，然后只需在 Web 上点击一下鼠标即可在自己的托管空间下创建一个派生（fork）的项目，并对派生项目的版本库具有读写的完全权限，就好像这个项目原本就是由自己创立的那样。当贡献者完成开发并向自己派生的版本库推送后，可以通过 GitHub 的 Web 界面向项目的核心开发团队发送一个 Pull Request，请求审核。项目的核心团队收到 Pull Request 后审核代码，审核通过后可以直接通过 Web 界面执行合并操作接纳贡献者的提交。

# 1.2\. GitHub 亮点

是什么让 GitHub 如此成功？GitHub 有什么魔力？

1.  只用 Git。

    GitHub 只支持 Git 格式的版本库托管，而不像其他开源项目托管平台还对 CVS、SVN、Hg 等格式的版本库进行托管。GitHub 的哲学很简单，既然 Git 是最好的版本控制系统之一（对于很多喜欢 Git 和 GitHub 的人没有之一），没有必要为兼顾其他版本控制系统而牺牲 Git 某些独有特性。因此没有支持其他版本控制系统的历史负担，是 GitHub 成功的要素之一。

    只用 Git 并不是说 GitHub 完全无视其他版本控制系统的使用者，相反，GitHub 面向 SVN（Subversion）用户和 Hg（Mercurial）用户开发了接口，让这些用户可以使用 SVN 或 Hg 的客户端工具访问 Git 版本库。

2.  对 Git 的完整支持。

    相比其他开源项目托管平台，GitHub 对 Git 版本库提供了完整的协议支持，支持 HTTP 智能协议、Git-daemon、SSH 协议。相比只支持 HTTP 协议的 GoogleCode，GitHub 通过 SSH 协议可以实现版本库访问的无口令认证（实际上使用 HTTP 协议也可以免口令输入。即通过文件`~/.netrc`写入 HTTP 认证的明文口令。具体文件格式参见 ftp 命令 MAN 手册中相关介绍）。

3.  无处不在的 Git。

    除了在版本库托管上使用 Git，Git 还被 GitHub 应用到更多领域。维基使用 Git，可以通过克隆维基所在的版本库，离线修改维基；在线粘贴数据的 Gist 网站[[2]](https://gist.github.com/)使用 Git，记录变更历史；以及在 Jekyll 应用的帮助下，用 Git 版本库维护个人网站和博客等。

4.  在线编辑文件。

    GitHub 提供了在线编辑文件的功能，不熟悉 Git 的用户也可以直接通过浏览器修改版本库里的文件。

5.  社交编程。

    将社交网络引入项目托管平台是 GitHub 的创举。用户可以关注项目、关注其他用户进而了解项目和开发者动态。项目的派生（Fork）和拉拽请求（Pull Request）构成 GitHub 最独具一格的工作模式。对提交代码的逐行评注及 Pull Request 构成了 GitHub 特色的代码审核。

6.  商业上的成功。

    GitHub 通过私有版本库托管、面向企业的版本库托管和项目管理平台、人员招聘等付费服务获得了商业上的成功，这种成功使得 GitHub 不必以页面中嵌入广告的方式维持运营，最大的受益者还是用户。

7.  关注细节。

    GitHub 网站采用了 Ruby on Rails 架构，在 Web 设计中运用了大量的 JavaScript、AJAX、HTML5 等技术，支持对使用 Markdown 等标记语言的内容进行渲染和显示等。关注细节使得 GitHub 成为了项目托管领域的后起之秀。

# 1.3\. 探索 GitHub

打开浏览器，访问网址 [`github.com/`](https://github.com/)，来探索 GitHub 吧。GitHub 的首页（图 1-5 所示）特意给出了 Git 和 GitHub 的音标，可能不少国人需要据此校准一下 Git 的读音(《Git 权威指南》第 1 页就提到了两种常见的对 Git 的读音错误。)。

![../images/github-homepage.png](http://box.kancloud.cn/2015-07-09_559dd219773e8.png)

图 1-5：GitHub 的首页

在首页的右上角是导航条，从左至右分别是：注册和收费方案、探索 GitHub、功能、博客和登录。还醒目地显示出不断增长着的注册用户数和托管的版本库数目。

如果想要了解 GitHub 上哪些项目最热门，进而寻找到好的开源产品，那么可以从导航条中的“Explore GitHub”开始。图 1-6 显示通过对社交数据的分析得到的托管版本库动态趋势。

![../images/explore-trends.png](http://box.kancloud.cn/2015-07-09_559dd22dea9f2.png)

图 1-6：版本库动态趋势

还可以根据感兴趣的人数、建立分支的数量、关注程度等寻找热门项目。图 1-7 显示分支最多的项目是 Homebrew —— 一款用 ruby 开发的苹果 Mac OS X 通用的非官方包管理软件。考虑到不断攀升的苹果用户数量以及易于上手的 ruby 语言，这并不奇怪。

![../images/explore-repositories.png](http://box.kancloud.cn/2015-07-09_559dd242206b8.png)

图 1-7：热门版本库排行

图 1-8 显示了托管版本库所用编程语言的动态分布，程序员多掌握几个热门编程语言一定会对找工作有帮助。;-)

![../images/explore-languages.png](http://box.kancloud.cn/2015-07-09_559dd248d9ac4.png)

图 1-8：托管项目的编程语言统计

GitHub 通过屏幕截图等方式介绍了 GitHub 的常见功能，可以通过点击导航条中的“Features”访问到。如图 1-9 可以看到在项目管理中，如何利用 GitHub 提供的团队管理功能、维基、缺陷追踪以及代码审核。

![../images/features-pm.png](http://box.kancloud.cn/2015-07-09_559dd26e41af8.png)

图 1-9：GitHub 功能介绍

博客也是了解 GitHub 的一个重要的途径，可以获知 GitHub 的最新动态，如最新改进等。图 1-10 显示的是 GitHub 在感恩节推出的促销活动：收费服务免费试用一个月！[[1]](http://git.io/N9WJ5w)如果及时关注博客就不会错过噢。

![../images/blog.png](http://box.kancloud.cn/2015-07-09_559dd2704cbcf.png)

图 1-10：GitHub 博客

图 1-10 的博客中一个由小章鱼和小猫组合而成的吉祥物，名字叫做 Octocat。这个可爱的 GitHub 吉祥物时不时会出来带给你惊喜。

马上到 GitHub 上注册，开始 GitHub 之旅。