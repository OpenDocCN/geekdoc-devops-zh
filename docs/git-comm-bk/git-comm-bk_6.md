# 6\. Git 生态体系

# Git 与之 Windows

(mSysGit)[[`code.google.com/p/msysgit/`](http://code.google.com/p/msysgit/)]

# 使用 Git 进行系统部署

### Capistrano 与 Git

[GitHub Guide on Deploying with Cap](http://github.com/guides/deploying-with-capistrano)

[Git and Capistrano Screencast](http://www.vimeo.com/369095)

# 与 Subversion 集成

# 从其他代码管理工具迁移到 Git

你决定要把你的整个项目从原来的代码管理工具迁移到 Git, 要怎么做才比较简单呢?

## 从 Subversion 导入

Git 包含了一个名为 git-svn 的脚本, 它有一个克隆(clone)命令, 可以把一个 Subversion 仓库导入到一个新的 Git 仓库. GitHub 上也有完成同样工作的免费工具.

```
$ git-svn clone http://my-project.googlecode.com/svn/trunk new-project

```

上面的命令会创建一个包含原来 Subversion 仓库全部历史记录的 Git 仓库. 通常这个操作会花相当长的时间, 因为它从第 1 个版本开始, 一个一个版本地签出, 然后再把这些版本进行本地提交.

## 从 Perforce 导入

在 contrib/fast-import 目录下, 你会找到 git-p4 脚本, 它会帮你导入 Perforce 仓库.

```
$ ~/git.git/contrib/fast-import/git-p4 clone //depot/project/main@all myproject

```

## 从其他管理工具导入

These are other SCMs that listed high on the Git Survey, should find import docs for them. !!TODO!!

*   CVS
*   Mercurial (hg)

*   Bazaar-NG

*   Darcs
*   ClearCase

# 图形化的 Git

Git 有不少图形化界面工具用于读取和维护仓库.

## 捆绑的 GUI

Git 自带了两个使用 Tcl/Tk 写成的 GUI 程序. Gitk 是一个仓库浏览器, 也是一个历史信息可视化工具.

[gitk](http://www.kernel.org/pub/software/scm/git/docs/gitk.html)

[git gui](http://www.kernel.org/pub/software/scm/git/docs/git-gui.html)是一个帮助你可视化索引操作的工具, 它支持 add, remove 和 commit. 它不能取代命令行, 但是对于基本使用是足够的.

[git gui](http://www.kernel.org/pub/software/scm/git/docs/git-gui.html)

## 第三方项目

Mac 用户可以参考 [GitX](http://gitx.frim.nl/) and [GitNub](http://github.com/Caged/gitnub/wikis)

Linux 和其他一些 Qt 用户可以参考 [QGit](http://digilander.libero.it/mcostalba/)

# Git 仓库托管

[github](http://wwww.github.com/)

[repo.or.cz](http://repo.or.cz/)

# Git 的其它用法

ContentDistribution

[TicGit](http://github.com/schacon/ticgit)

# Git 的脚本支持

## Ruby 与 Git

grit

jgit + jruby

## PHP 与 Git

## Python 与 Git

pygit

## Perl 与 Git

perlgit

> 译者注:　此章的英文版原文也只是列出了大纲, 因此中文版看出起就很单薄, 以后我们尽量完善:)

# Git 与编辑器

textmate

eclipse

netbeans