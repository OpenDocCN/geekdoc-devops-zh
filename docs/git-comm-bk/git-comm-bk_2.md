# 2\. 第一步

# 安装 Git

## 从源代码开始安装

如果你在一个其基于 Unix 的系统中，你可以从 Git 的官网上[Git Download Page](http://git-scm.com/download)下载它的源代码,并运行像下面的几行命令,你就可以安装:

```
$ make prefix=/usr all ;# as yourself 
$ make prefix=/usr install ;# 以 root 权限运行

```

你需一些库: [expat](http://expat.sourceforge.net/),[curl](http://curl.linux-mirror.org/), [zlib](http://www.zlib.net/), 和 [openssl](http://www.openssl.org/); 除了 expat 外，其它的可能在你的机器上都安装了。

## Linux

如果你用的是 Linux，你可以用你的本地包管理系统(native package management system)来安装.

```
$ yum install git-core  #译者注，在 redhat 等系统下用 yum

$ apt-get install git-core  #译者注，在 debian, ubuntu 等系统下用 apt-get

```

如果你用上面的命令不起作用的话，你可以从下面两个站点下载 .deb 或 .rpm 包:

[RPM Packages](http://kernel.org/pub/software/scm/git/RPMS/)

[Stable Debs](http://www.backports.org/debian/pool/main/g/git-core/)

如果你在 Linux 兴趣从源代码开始安装的话,下面的这篇文章也许对你有帮助: [Article: Installing Git on Ubuntu](http://chrisolsen.org/2008/03/10/installing-git-on-ubuntu/)

## Mac 10.4

在 Mac10.4 和 10.5,如果你安装了 MacPorts,你可以通过 [MacPorts](http://www.macports.org/)来安装 Git。如果你没有安装 MacPort, 你可以从 [这里](http://www.macports.org/install.php)来安装它.

当你安装好 MacPorts 后，你可通过下面的命令来安装:

```
$ sudo port install git-core

```

如果你想从源代码开始安装，下面这些文章可能对你有帮助:

[Article: Installing Git on Tiger](http://rails.wincent.com/wiki/Installing_Git_1.5.2.3_on_Mac_OS_X_Tiger)

[Article: Installing Git and git-svn on Tiger from source](http://larrytheliquid.com/2007/12/29/compiling-git-and-git-svn-on-osx-tiger/)

## Mac 10.5

在 Leopard 系统下，你也可以通过 MacPorts 来安装,但是你有一个新的选项:"一个漂亮的安装包", 你可以从这里来下载:[Git OSX Installer](http://code.google.com/p/git-osx-installer/downloads/list?can=3)

如果你想从源代码开始安装，我希望下面些资料能对你有帮助:

[Article: Installing Git on OSX Leopard](http://solutions.treypiepmeier.com/2008/02/25/installing-git-on-os-x-leopard/)

[Article: Installing Git on OS 10.5](http://dysinger.net/2007/12/30/installing-git-on-mac-os-x-105-leopard/)

## Windows

在 Windows 下安装 Git 是很简单的，你只要下载[msysGit](http://code.google.com/p/msysgit/downloads/list)就可以了。

*Git on Windows* 这一章有一个"screencast"来在演示如何在 windows 下使用 Git。

# 安装与初始化

## Git 配置

使用 Git 的第一件事就是设置你的名字和 email,这些就是你在提交 commit 时的签名。

```
$ git config --global user.name "Scott Chacon"
$ git config --global user.email "schacon@gmail.com"

```

执行了上面的命令后,会在你的主目录(home directory)建立一个叫 *~/.gitconfig* 的文件. 内容一般像下面这样:

```
[user]
        name = Scott Chacon
        email = schacon@gmail.com

```

译者注:这样的设置是全局设置,会影响此用户建立的每个项目.

如果你想使项目里的某个值与前面的全局设置有区别(例如把私人邮箱地址改为工作邮箱);你可以在项目中使用 git config 命令不带 *--global* 选项来设置. 这会在你项目目录下的 *.git/config* 文件增加一节[user]内容(如上所示)。