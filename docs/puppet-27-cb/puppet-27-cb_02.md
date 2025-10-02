# 译者序

## 译者序

您正阅读的是 《Puppet 2.7 Cookbook》的简体中文翻译版。

# 项目缘起

### 项目缘起

[Puppet](http://puppetlabs.com/) 是一款优秀的**自动化系统管理部署工具**。

去年底曾经在网络上看到过网友 **Sky** 基于《Puppet 2.7 Cookbook》征求意见版（**RAW**）的发布在 BLOG 上的翻译。

*   网友 Sky 的微博： [`weibo.com/u/1938768691`](http://weibo.com/u/1938768691)

*   BLOG 发布： [`www.mysqlops.com/category/puppet`](http://www.mysqlops.com/category/puppet)

基于 BLOG 的翻译**不利于**协同翻译、纠错以及追踪原书的版本更新。

今年年初我看到了 [蒋鑫](http://www.worldhello.net/) 所著的开源书 [GotGitHub](http://www.worldhello.net/gotgithub/) ， 觉得基于 [github](http://www.github.com) 写作开放的技术图书或协同翻译图书是个不错的选择。

今年二月无意中从电驴下载了 《Puppet 2.7 Cookbook》 的正式版，**遂决意以更利于协同工作的方式重新翻译此书**。 并**为 Puppet 中文社区或其他社区翻译技术图书提供一个案例**。

# 翻译方法

### 翻译方法

1.  为了协同翻译或协同纠错，应将图书原稿纳入版本控制系统，我选择使用 [github](http://www.github.com) 。 有关 Github 的使用，请参考 [蒋鑫](http://www.worldhello.net/) 所著的开源书 [GotGitHub](http://www.worldhello.net/gotgithub/) 。

2.  显然将 PDF/Word 文档纳入 VCS 的意义不大，为了便于协同工作，应选用一种标记语言书写图书或译作的源码，之后生成 HTML/PDF 文档。 我选择使用 [AsciiDoc](http://www.methods.co.nz/asciidoc/) ，它是一款优秀的基于标记语言的技术文档写作和翻译工具。 为此，我首先将原书的 PDF 文档手工转化成了 AsciiDoc 格式。

3.  AsciiDoc 支持 [GNU source-highlight](http://www.gnu.org/software/src-highlite/) 和 [Pygments](http://pygments.org/) 两种代码加亮系统。 由于当前只有 Pygments 支持 *.pp 代码加亮， 所以我选择使用基于 Pygments 的 [puppet-pygments-lexer](http://github.com/rodjek/puppet-pygments-lexer) 对翻译版进行代码加亮处理。

4.  作为一个翻译图书案例，本书所有的翻译工作都是我一个人做的。若要多人协同工作可以采用如下方式：

    *   若要协同翻译，可以开启 Github 中图书翻译项目仓库的 Wiki，进行翻译章节的认领和进度规划等工作。

        *   参考： [使用 Github Wiki](http://www.worldhello.net/gotgithub/04-work-with-others/060-wiki.html)

        *   参考： [Dive into Python 翻译项目](http://wiki.woodpecker.org.cn/moin/DiveIntoPythonZh) 的 Wiki 组织形式

    *   翻译成员可以在 Github 上克隆核心成员的仓库，在自己的仓库中翻译，之后提交到核心成员的仓库。

        *   参考： [Fork + Pull 模式](http://www.worldhello.net/gotgithub/04-work-with-others/010-fork-and-pull.html)
    *   审校者也可用上述方式提交修改，还可以用 [缺陷跟踪](http://www.worldhello.net/gotgithub/04-work-with-others/050-issue.html) 方式提交修改意见

5.  使用 asciidoc 或 a2x 生成 HTML/PDF/epub 等版本。

> ![注记](img/note.png) Git and Github Useful Links
> 
> *   Git
>     
>     
>     *   [git - 简易指南](http://rogerdudler.github.com/git-guide/index.zh.html)
>         
>         
>     *   [Why Git is Better than X](http://zh-cn.whygitisbetterthanx.com/)
>         
>         
>     *   [Git Magic](http://www-cs-students.stanford.edu/~blynn/gitmagic/intl/zh_cn/)
>         
>         
>     *   [git ready](http://gitready.com/)
>         
>         
>     *   [Git: Everyday Stuff](http://www.javahotchocolate.com/tutorials/git-everyday.html)
>         
>         
>     *   [Everyday GIT With 20 Commands Or So](http://schacon.github.com/git/everyday.html)
>         
>         
>     *   [Pro Git](http://progit.org/)
>         
>         
>     *   [参与 Git 的中文本地化](http://www.worldhello.net/2012/02/28/git-l10n.html)
>         
>         
> *   Github
>     
>     
>     *   [GotGitHub](http://www.worldhello.net/gotgithub/)
>         
>         
>     *   [如何高效利用 GitHub](http://www.yangzhiping.com/tech/github.html)
>         
>         
>     *   [The GitHub Help](http://help.github.com/)

# 社区链接

### 社区链接

*   [Puppet 中文 Wiki](http://puppet.chinaec2.com/)

*   [Puppet 中文技术社区](http://www.puppetfans.com/)

# 社区建议

### 社区建议

1.  关于 Puppet 文档的协同翻译建议

    1.  直接使用 Github Wiki 协同翻译（Puppet 文档是 Markdown 格式的，Github Wiki 默认支持 Markdown 格式）

    2.  使用 [Jekyll](http://jekyllrb.com/) 创建基于 github 的静态站点生成系统协同翻译 Puppet 文档 （参见： [使用 Jekyll 维护网站](http://www.worldhello.net/gotgithub/03-project-hosting/050-homepage.html#jekyll)）

2.  关于 Puppet 图书的协同翻译建议

    1.  建议以本书的翻译方式组织 《Pro Puppet》 一书的中文翻译

> ![提示](img/tip.png)
> 
> 建议 [Puppet 中文 Wiki](http://puppet.chinaec2.com/) 从 DokuWiki 转向 **Github+Jekyll**。

# 贡献者

### 贡献者

*   me

*   you

*   him

*   her