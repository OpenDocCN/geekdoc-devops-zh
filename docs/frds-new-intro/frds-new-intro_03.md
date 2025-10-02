# 第二章: 安装

有两种方法安装 FreeRADIUS 到一个 Linux 服务器. 你已经简单低安装预编译的包或者从源码编译安装. 这一章会教你这两种方法.

在这一章我们将会:

*   从预编译包安装 FreeRADIUS.
*   从源码编译和安装 FreeRADIUS.
*   调查 FreeRADIUS 安装了哪些程序.
*   确保 FreeRADIUS 正确地运行.

# 开始之前

有许多 linux 发行版可以选择. 我们将会选择三种流行的发行版来尽可能的包括所有听众来避免发行版战争.

基于他们使用系统的包管理系统, 许多 linux 发行版分成 2 组. 一组使用 RPM(Red Hat Package Manager), 而另一组使用 dpkg 包管理器. 我们选择两种基于 RPM 的发行版, CentOS 和 SUSE, 在企业中流行. 取代使用 Debian 作为一个基于 dpkg 的发行版, 我们选择 Ubuntu 因为他在初学者中广泛流行. 由于 Ubuntu 派生于 Debian, 讨论 Ubuntu 的章节也适用于 Debian 不需要大的修改.

这个章节的后面步骤需要安装

| Distribution | Version |
| --- | --- |
| CentOS | 5.5 |
| SUSE | SLES 11 |
| Ubuntu | 10.4 |

一个典型的可以 root 访问的服务器是一个基础. 使用这个章节作为一个向导, 如果你有一个带有不同于那个指定版本的发行版.

# 预编译程序

如今的 Linux 发行版有许多预编译软件, 可以容易地安装. 一个单一的命令可以使用来从一个软件仓库来安装 FreeRADIUS. 这个会解决依赖和安装所有需要的包为了展示一个工作的系统.

关于软件包管理系统: [`en.wikipedia.org/wiki/Package_management_system`](http://en.wikipedia.org/wiki/Package_management_system)

3 种发行版的默认安装将会包含软件仓库包含 FreeRADIUS 包.

# 安装 FreeRADIUS

预编译的 FreeRADIUS 包可以通过下面的命令在相应发行版进行安装.

*   CentOS: `yum install freeradius2 freeradius2-utils`
*   SUSE: `zypper in freeradius-server freeradius-server-utils freeradius-server-doc`
*   Ubuntu: `sudo apt-get install freeradius`

## 优点

*   自动解决依赖. 这个包括考虑到未来的安全更新, 跟踪所有可选的包, 确保正确的版本依赖.
*   linux distributor 的 QA 测试确保软件正常工作.
*   更新会被 linux distributor 照顾好.
*   发行版相关的 tweak 已经执行.

使用预编译包的一个妥协是你不会用到最新的 FreeRADIUS 包到你的机器上.

## 额外的包

FreeRADIUS 是一个功能丰富的软件. 不同的发行版展示他的 FreeRADIUS 不同地, 通过分散到多个不同的包里.

CentOS 和 Ubuntu 包含某些 FreeRADIUS 服务器的包作为可选包. 这使得基本的服务器安装包保持瘦的. 安装可选的服务器模块包将会也安装需要的依赖. 这表示, 例如, 当你安装 freeradius-mysql 包的时候, 所有需要的 MySQL 库将作为依赖安装.

SUSE 通过功能划分他们的包. 你可以发现客户端和服务器端都有他各自的包集合. SUSE 也有 FreeRADIUS 的 utilities 和文档的包.

## 可得到的包

这一节列出每种发行版的可得到的预编译的 FreeRADIUS 包. 加粗的名字是推荐作为基本的 FreeRADIUS 安装的.

### CentOS

| 包名 | 简短描述 |
| --- | --- |
| **freeradius2** | 高度可配置的 RADIUS 服务器 |
| freeradius2-krb5 | FreeRADIUS 的 Kerberos 5 支持 |
| freeradius2-ldap | FreeRADIUS 的 LDAP 支持 |
| freeradius2-mysql | FreeRADIUS 的 MySQL 支持 |
| freeradius2-perl | FreeRADIUS 的 Perl 支持 |
| freeradius2-postgresql | FreeRADIUS 的 PostgreSQL 支持 |
| freeradius2-python | FreeRADIUS 的 python 支持 |
| freeradius2-unixODBC | FreeRADIUS 的 Unix ODBC 支持 |
| freeradius2-utils | FreeRADIUS 的 utilities |

### SUSE

| 包名 | 简短描述 |
| --- | --- |
| freeradius-client | FreeRADIUS 的客户端软件 |
| freeradius-client-libs | FreeRADIUS 客户端的共享库 |
| **freeradius-server** | Highly configurable RADIUS server |
| freeradius-server-dialupadmin | Web management for FreeRADIUS |
| **freeradius-server-doc** | FreeRADIUS 的文档 |
| **freeradius-server-libs** | FreeRADIUS 的共享库 |
| **freeradius-server-utils** | FreeRADIUS 的客户端 |

注意, SUSE 提供的`freeradius-client`包是被软件开发者使用来利用 RADIUS 来 AAA. 客户端程序像`radtest`是包含在`freeradius-server-utils`包中.

### Ubuntu

包名 | 简短描述 **freeradius** | FreeRADIUS 的服务器端包 freeradius-dbg | 包含 FreeRADIUS 包的分离的调试符号 **libfreeradius2** | FreeRADIUS 的共享库 freeradius-ldap | FreeRADIUS 服务器的 LDAP 模块 **freeradius-common** | FreeRADIUS 的普通文件, 包括字典和 man pages freeradius-iodbc | FreeRADIUS 服务器的 iODBC 模块 freeradius-krb5 | FreeRADIUS 服务器的 Kerberos 模块 **freeradius-utils** | FreeRADIUS 的客户端程序, 包括程序像 radclient, radtest, smbencrypt, radsniff, 和 radzap freeradius-postgresql | FreeRADIUS 服务器的 PostgreSQL 模块 freeradius-mysql | FreeRADIUS 服务器的 MySQL 模块 freeradius-dialupadin | web 管理插件 libfreeradius-dev | FreeRADIUS 的共享开发库

## 特殊的考虑

旧版本的 Ubuntu 的预编译包没有编译进去 SSL 库支持. 当安装 FreeRADIUS 到这些版本的时候, 如果你需要在一些 EAP 扩展上支持 SSL, 你需要自己编译.

SUSE 也提供`yast -i`命令来安装软件. 但使用`zypper`替代, 因为当安装软件和依赖时, 他有更好的决策能力.

在 CentOS 中 FreeRADIUS 包的名称是`freeradius2`而不是`freeradius`. 这是因为最开始在 CentOS 5 上支持的 FreeRADIUS 版本是 1.1.3\. FreeRADIUS 的 1.x 和 2.x 之间的配置文件有改变, 所以需要在名称上有所改变.

不是所有的 FreeRADIUS 模块有他们在 Ubuntu 或 CentOS 上匹配的包. 没有匹配的包的模块就简单低包含在 FreeRADIUS 的主包上.

## 留意防火墙

CentOS 和 SUSE 默认安装一个活跃的防火墙. 请确保 UDP 端口 1812 和 1813 是对外开放的.

### CentOS

有一个程序来配置 CentOS 上的防火墙, 叫做`system-config-securitylevel-tui`, 需要 root 来运行. 这个会启动一个基于鼠标的程序. 选择`Customize`选项然后按下`Enter`. `Allow incoming | Other Ports`列表应该包括`1812:udp 1813:udp`. 选择`OK`来返回到主界面, 然后再选择`OK`, 来提交修改.

确认端口是开放的通过下面命令的输出:

```
# iptables -L -n | grep 181*
ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0           state NEW udp dpt:1812
ACCEPT     udp  --  0.0.0.0/0            0.0.0.0/0           state NEW udp dpt:1813 
```

不推荐你用下面的命令来关闭防火墙.

```
# /etc/init.d/iptables save
# /etc/init.d/iptables stop
# /sbin/chkconfig iptables off 
```

为了确认是否防火墙是关闭的, 使用下面的命令.

```
# iptables -L -n
Chain INPUT (policy ACCEPT)
target     prot opt source               destination

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination 
```

## SUSE

在 SUSE 上配置防火墙可能有点 a catch-22 situation 由于默认的防火墙是如此安全, 你甚至不能用 SSH 进入系统! 进入 SLES 服务器然后开始`YaST`. 选择`Security and Users | Firewall`. 选择左边的`Allowed Services`. 我建议你添加`Secure Shell Server`到`External Zone`. 点击`Advanced`按钮, 然后添加 1812, 1813 到`UDP Ports`. 点击`OK`. 点击`Next`和`Finish`来提交这些修改.

通过下面的命令来确认端口是开放的.

```
# iptables -L -n | grep 181
ACCEPT     udp  --  0.0.0.0/0        0.0.0.0/0         udp dpt:1812
ACCEPT     udp  --  0.0.0.0/0        0.0.0.0/0         udp dpt:1813 
```

不推荐用下面的方法关闭防火墙.

1.  使用`YaST`然后选择`Security and Users | Firewall`.
2.  选择左边的`Start-Up`, 选择右边的`Disable Firewall Automatic Starting`. 也选择`Stop Firewall Now`, 然后按`Enter`键来停止当前运行的防火墙.
3.  点击`Next`和`Finish`来提交修改.

通过下面的命令来确认防火墙现在是关闭的.

```
# iptables -L -n
Chain INPUT (policy ACCEPT)
target     prot opt source               destination

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination 
```

# 从源码安装

从源码安装软件是`configure, make, make install`的同义词. 我们将使用发型吧的包管理器来从源码编译新的软件.

下一节是可选的内容如果你已经安装了 FreeRADIUS 的预编译的包.

## 从源码编译

有时有需要来安装最新版本的软件或者包含一些预编译软件没有支持的模块. 这需要从源码编译软件. 大多数开源软件包是作为一个 TAR 压缩包发布的. TAR 实际上可以指用于创建 TAR 文件的程序(名称是 tape archive 的简写)或者合成文件的文件格式. 一个 TAR 文件也通常是压缩的来减少体积. 文件名将会表示压缩的格式. 一个文件名以`.tgz`或者`.tar.gz`结尾使用`gzip`. 一个文件名以`.tbz`, `.tb2`, 或者`.tar.bz2`结尾使用`bzip2`压缩. 为了编译软件我们解压这个文件, 然后执行命令`configure`, `make`和`make install`在解压的文件夹内.

然而有更好的方式利用发行版的包管理系统来编译软件. 不是所有的 TAR 文件帮助这个容易实现, 但是 FreeRADIUS 是这样一个成熟的项目, 允许在任意这 3 个发行版上从源码编译软件.

## 编译包的优势

下面是一些从源码编译包的优势

*   容易安装, 升级, 卸载或发布软件.
*   容易检查那个版本的软件是安装的.
*   能够看到哪些文件被安装, 哪些发行版相关的修改.

既然你已经确定用这种方式编译包, 我们将会为每个发行版来做他.

## 创建编译环境

不用生产机器来开发是一个好的实践. 这包括从源码编译软件. 创建一个你可以编译包的虚拟机. 测试之后简单低拷贝和安装新的包到一个生产机器.

## CentOS

CentOS 是一个 RedHat 风格的发行版. 他是 RHEL 的社区版. CentOS 不是有 RedHat 直接赞助像 Fedora 项目那样. 尽管 RedHat 的品牌和 logo 从 CentOS 移除, 但是它跟 RHEL 是一样的, 由于相同的源代码用来生成发行版.

CentOS 使用 RPM 包管理器来管理软件. 安装的软件包叫做 RPM.

## 编译 CentOS RPMs

这一节使用从 FreeRADIUS 的 wiki 页面的推荐和指令来创建最新的 RPM 到 CentOS 上. [`wiki.freeradius.org/Red_Hat_FAQ#How_to_build_an_SRPM`](http://wiki.freeradius.org/Red_Hat_FAQ#How_to_build_an_SRPM)

你必须用 root 用户来执行大多数命令. 一旦完成, 遵循这些步骤.

1.  安装`rpm-build`包, 这个创建的编译包需要的目录结构. `yun install rpm-build`
2.  允许普通用户对这个目录有写和执行权限. `find /usr/src/redhat -type d | xargs chmod a+wx`
3.  Fedora 上可以得到 FreeRADIUS 的最新版本. 我们可以使用这个版本来在 CentOS 上编译 RPM. 从下面的 URL 上下载 RPM 源码. [`kojipkgs.fedoraproject.org//packages/freeradius/2.2.5/2.fc19/src/freeradius-2.2.5-2.fc19.src.rpm`](https://kojipkgs.fedoraproject.org//packages/freeradius/2.2.5/2.fc19/src/freeradius-2.2.5-2.fc19.src.rpm) RPM 的源码包以`.src.rpm`结尾.
4.  安装这个源码包. 例如使用下面的命令. 你的源码包的命名可能不同. `rpm -ihv --nomd5 freeradius-2.2.5-2.fc19.src.rpm`.
5.  使用下面的命令来决定需要的依赖. 这些依赖必须在包边以前被安装. `rpmbuild -ba /usr/src/redhat/SPECS/freeradius.spec`
6.  使用`yum`来安装这些依赖. 在我的系统上时下面这些. `yum install autoconf gdbm-devel libtool libtool-ltdl-devel openssl-devel pam-devel zlib-devel net-snmp-devel net-snmp-utils readline-devel libpcap-devel openldap-devel krb5-devel python-devel mysql-devel postgresql-devel unixODBC-devel`
7.  安装一个编译器. `yum install gcc`
8.  再次运行`rpmbuild`命令. 使用普通用户来做, 使用`tee`来输出到一个文件来为了将来的研究. 这将会启动编译过程, 需要花费一些时间. `rpmbuild -ba /usr/src/redhat/SPECS/freeradius.spec | tee /tmp/freeradius_build_out.txt`.
9.  当它成功完成, 将会有一个指示 RPM 已经写入. 新安装的 RPM 将会得到下面这行. `Wrote: /usr/src/redhat/RPMS/i386/freeradius-debuginfo-2.1.10-1.i386.rpm`
10.  安装新编译的 RPM. 下面的命令将会导致一个基本的 FreeRADIUS 安装. `yum --nogpgcheck install /usr/src/redhat/RPMS/i386/freeradius-2.1.10-1.i386.rpm /usr/src/redhat/RPMS/i386/freeradius-utils-2.1.10-1.i386.rpm` 如果你得到消息通知你`No package <path and package name> available`, 确认你指定了正确的版本号.

## 发生了什么?

我们刚刚创建和安装了 CentOS 上最新的 RPM. 下面是一些重点的分解.

## 安装`rpm-build`

`rpm-build`包包含脚本和可执行程序用来编译 RPM 包. 他包括`rpmbuild`程序, 我们将用来创建 RPM.

## RPM 包的源码.

我们使用 FreeRADIUS wiki 上推荐的 Fedora 的 RPM 包源码. 这导致一个更加稳定和更好的包工作集合, 已经包含了发行版相关的 tweak 和修改.

## 包名

注释包名是`freeradius`而不是`freeradius2`. 这是因为所有的在 Fedora 里面的包已经是 2.x.

从 FreeRADIUS 1.x 版本升级在 Fedora 系统不像 CentOS 那样没有问题.

## 更新一个已经存在的安装

如果你已经有 FreeRADIUS 安装了, 他可能包含一些模块你需要包含在更新的时候.

*   决定 FreeRADIUS 的版本和当前安装的包通过使用下面的命令. `yun list freeradius\*`
*   当你已经作为预编译包安装了`freeradius2`, `yum`将会抱怨, 当尝试安装新的`freeradius`包, 因为名字冲突. 你可以卸载`freeradius2`包, 通过下面的命令. `yum erase freeradius\*`
*   上面假定你没有做任何配置修改到安装, 你想要保留.

# 编译 RPM 包

## CentOS

CentOS 是一个 RedHat 风格的发行版. 他是 RHEL 的社区版. CentOS 不是有 RedHat 直接赞助像 Fedora 项目那样. 尽管 RedHat 的品牌和 logo 从 CentOS 移除, 但是它跟 RHEL 是一样的, 由于相同的源代码用来生成发行版.

CentOS 使用 RPM 包管理器来管理软件. 安装的软件包叫做 RPM.

## 编译 CentOS RPMs

这一节使用从 FreeRADIUS 的 wiki 页面的推荐和指令来创建最新的 RPM 到 CentOS 上. [`wiki.freeradius.org/Red_Hat_FAQ#How_to_build_an_SRPM`](http://wiki.freeradius.org/Red_Hat_FAQ#How_to_build_an_SRPM)

你必须用 root 用户来执行大多数命令. 一旦完成, 遵循这些步骤.

1.  安装`rpm-build`包, 这个创建的编译包需要的目录结构. `yun install rpm-build`
2.  允许普通用户对这个目录有写和执行权限. `find /usr/src/redhat -type d | xargs chmod a+wx`
3.  Fedora 上可以得到 FreeRADIUS 的最新版本. 我们可以使用这个版本来在 CentOS 上编译 RPM. 从下面的 URL 上下载 RPM 源码. [`kojipkgs.fedoraproject.org//packages/freeradius/2.2.5/2.fc19/src/freeradius-2.2.5-2.fc19.src.rpm`](https://kojipkgs.fedoraproject.org//packages/freeradius/2.2.5/2.fc19/src/freeradius-2.2.5-2.fc19.src.rpm) RPM 的源码包以`.src.rpm`结尾.
4.  安装这个源码包. 例如使用下面的命令. 你的源码包的命名可能不同. `rpm -ihv --nomd5 freeradius-2.2.5-2.fc19.src.rpm`.
5.  使用下面的命令来决定需要的依赖. 这些依赖必须在包边以前被安装. `rpmbuild -ba /usr/src/redhat/SPECS/freeradius.spec`
6.  使用`yum`来安装这些依赖. 在我的系统上时下面这些. `yum install autoconf gdbm-devel libtool libtool-ltdl-devel openssl-devel pam-devel zlib-devel net-snmp-devel net-snmp-utils readline-devel libpcap-devel openldap-devel krb5-devel python-devel mysql-devel postgresql-devel unixODBC-devel`
7.  安装一个编译器. `yum install gcc`
8.  再次运行`rpmbuild`命令. 使用普通用户来做, 使用`tee`来输出到一个文件来为了将来的研究. 这将会启动编译过程, 需要花费一些时间. `rpmbuild -ba /usr/src/redhat/SPECS/freeradius.spec | tee /tmp/freeradius_build_out.txt`.
9.  当它成功完成, 将会有一个指示 RPM 已经写入. 新安装的 RPM 将会得到下面这行. `Wrote: /usr/src/redhat/RPMS/i386/freeradius-debuginfo-2.1.10-1.i386.rpm`
10.  安装新编译的 RPM. 下面的命令将会导致一个基本的 FreeRADIUS 安装. `yum --nogpgcheck install /usr/src/redhat/RPMS/i386/freeradius-2.1.10-1.i386.rpm /usr/src/redhat/RPMS/i386/freeradius-utils-2.1.10-1.i386.rpm` 如果你得到消息通知你`No package <path and package name> available`, 确认你指定了正确的版本号.

## 发生了什么?

我们刚刚创建和安装了 CentOS 上最新的 RPM. 下面是一些重点的分解.

## 安装`rpm-build`

`rpm-build`包包含脚本和可执行程序用来编译 RPM 包. 他包括`rpmbuild`程序, 我们将用来创建 RPM.

## RPM 包的源码.

我们使用 FreeRADIUS wiki 上推荐的 Fedora 的 RPM 包源码. 这导致一个更加稳定和更好的包工作集合, 已经包含了发行版相关的 tweak 和修改.

## 包名

注释包名是`freeradius`而不是`freeradius2`. 这是因为所有的在 Fedora 里面的包已经是 2.x.

从 FreeRADIUS 1.x 版本升级在 Fedora 系统不像 CentOS 那样没有问题.

## 更新一个已经存在的安装

如果你已经有 FreeRADIUS 安装了, 他可能包含一些模块你需要包含在更新的时候.

*   决定 FreeRADIUS 的版本和当前安装的包通过使用下面的命令. `yun list freeradius\*`
*   当你已经作为预编译包安装了`freeradius2`, `yum`将会抱怨, 当尝试安装新的`freeradius`包, 因为名字冲突. 你可以卸载`freeradius2`包, 通过下面的命令. `yum erase freeradius\*`
*   上面假定你没有做任何配置修改到安装, 你想要保留.

# 编译 SUSE 包

SUSE 也利用 RPM 包管理器来软件管理就像 RedHat 发行版一样. 尽管都使用 RPM 作为基础的软件管理, SLES 使用`zypper`来管理软件包仓库, CentOS 使用`yum`.

SLES 是一个硬核的企业发行版. 尽可能使用预编译包, 但是如果你感到勇敢, 想要你的手变脏, 这一节为你准备.

在 SLES 上编译 FreeRADIUS 的 RPM 包, 要比 CentOS 或者 Ubuntu 徐璈更多的努力.

## 添加一个 OpenSUSE 仓库

从源码编译 FreeRADIUS 需要许多开发库. 标准的 SLES 仓库不需要全部包含他们他们, 甚至当使用的 SDK DVD 是 SLES 的一部分. 我们因此需要添加 OpenSUSE 仓库. 这将会满足所有的开发库需求.

1.  通过下面的 URL 定位离你最近的 OpenSUSE 镜像: [`mirrors.opensuse.org`](http://mirrors.opensuse.org)
2.  使用 root 用户开始`YAST`.
3.  选择`Software | Software Repositories`并且然后选择`Add`来添加到一个仓库.
4.  添加在开始时你选择的 OpenSUSE 镜像. 对于我的 OpenSUSE11.3 最近的 URL 是: ftp://opensuse.mirror.ac.za/opensuse/distribution/11.3/repo/oss/suse
5.  你可能要被接受 GnuPG 密钥的导入对于新的仓库. 选择`Import`来继续.

这个设置编译 FreeRADIUS RPM 的阶段. 现在遵循下面的步骤:

1.  从官网下载最新的`.tar.bz2`格式的 FreeRADIUS 源码.
2.  SLES 包括一个目录结构专门用于编译 RPM. 位于`/usr/src/packages`. 拷贝原始的`bz2`源文件到 SOURCES 目录. 替换 x 和 y 为对应的版本. `cp freeradius-server-2.x.y.tar.bz2 /usr/src/packages/SOURCES/`.
3.  从 TAR 文件解压 SUSE RPM .spec 文件, 替换 x,y 为对应版本.

    ```
    # cd /usr/src/packages/SOURCES/
    # tar -xvjf freeradius-server-2.x.y.tar.bz2 freeradius-server-2.x.y/suse/freeradius.spec 
    ```

4.  拷贝下面的文件到`/usr/src/packages/SPECS`目录:

    ```
    # cp freeradius-server-2.x.y/suse/freeradius.spec ../SPECS/ 
    ```

5.  编辑`spec`文件的下面的行, 修改`%{fillup_and_insserv -s freeradius START_RADIUSD}`为`%{fillup_and_insserv freeradius}`.
6.  运行下面的命令来决定依赖. `rpmbuild -ba /usr/src/packages/SPECS/freeradius.spec`
7.  这个将会列出必要的开发包来安装, 为了 FreeRADIUS RPM 被编译. 在你的系统上的列表可能不同. 下面是我的系统上的结果(用`zypper`安装他们)

    ```
    zypper in db-devel e2fsprogs-devel gcc-c++ gdbm-devel gettext-devel glibc-devel ncurses-devel openldap2-devel openssl-devel pam-devel postgresql-devel python-devel unixODBC-devel zlib-devel apache2-devel cyrus-sasl-devel krb5-devel libapr1-devel libmysqlclient-devel 
    ```

8.  再次运行`rpmbuild`命令. 如果所有的依赖都满足, RPM 的编译将会开始. 把结果 tee 到一个文件用于将来的调查.

    ```
    rpmbuild -ba /usr/src/packages/SPECS/freeradius.spec | tee /tmp/build_out.txt 
    ```

9.  当编译完成, RPM 位于`/usr/src/packages/RPMS/<architecture>/`目录.
10.  使用下面命令安装新的 FreeRADIUS 包.

    ```
    # cd /usr/src/packages/RPMS/<architecture>/
    # zypper in freeradius-server-2.x.y-0.i586.rpm freeradius-server-libs-2.x.y-0.i586.rpm freeradius-server-utils-2.x.y-0.i586.rpm 
    ```

11.  注意默认 FreeRADIUS 将会已用户`radiusd`运行. 这个用户是在安装 FreeRADIUS 过程中创建的. 给这个用户`certs`目录的权限. 否则会安装失败.

    ```
    chown -R radiusd. /etc/raddb/certs 
    ```

12.  通过`radiusd -X`来确认`radiused`可以正确启动.
13.  `Ctrl+C`将会停止正在运行的 FreeRADIUS. 你现在可以使用下面的启动脚本来启动 FreeRADIUS

    ```
    /etc/init.d/freeradius start 
    ```

## 发生了什么?

我们完成了在 SLES 上编译和安装 FreeRADIUS. 尽管不像 CentOS 那样优雅, 仍然没有许多问题.

## zypper 还是 yast -i

使用`zypper`来安装需要的依赖. 使用`yast -i`来安装需要的`gettext-devel`包将会返回一个错误. `zypper`知道`gettext-tools`包将会满足依赖.

使用`yast -i`来安装依赖也导致从 OpenSUSE 仓库不需要的更新. 你会被警告.

## 手动 tweak

有一些 tweak 我们必须手动执行, 为了获取在 SLES 上最新的 FreeRADIUS.

*   添加 OpenSUSE 仓库作为一个开发包源.
*   从 OpenSUSE 安装各种开发包.
*   编辑`freeradius.spec`来卸载旧的宏.
*   修改包含证书的目录的权限.

# 编译 deb 包

Ubuntu 是基于 Debian Linux, 使用 dpkg 包挂利器来管理软件像 deb.

`apt`程序是用来管理 dpkg 仓库, 跟`zypper`和`yum`使用 RPM 是一样的.

下面的步骤将会展示如何安装 deb.

1.  安装`dpkg-dev`包. 这个包提供开发工具(包括`dpkg-source`), 用来 unpack, 编译, 和上传 Debian 源码包. `sudo apt-get install dpkg-dev` 如果这个包没有安装, 检查被`apt`程序使用的源列表. 这个仓库的源定义在`/etc/apt/sources.list`文件, 并且通过`apt-get update`命令更新.
2.  安装所有`freeradius`包需要的开发库

    ```
    sudo apt-get build-dep freeradius 
    ```

    如果你得到一个信息: `Unable to find a source package for freeradius`, 可能因为仓库源(deb-src)没有包含在`/etc/apt/sources.list`文件.
3.  确保`fakeroot`程序被安装.

    ```
    sudo apt-get install fakeroot 
    ```

4.  确保`ssl-cert`包被安装. 不能安装这个包将会导致编译 EAP 模块出现问题.

    ```
    sudo apt-get install ssl-cert 
    ```

5.  下载最新版本的 FreeRADIUS 源码.

    ```
    wget ftp://ftp.freeradius.org/pub/freeradius/freeradius-server-2.x.y.tar.gz 
    ```

6.  解压 FreeRADIUS 的源码 TAR 文件.

    ```
    tar -xzvf freeradius-server-2.x.y.tar.gz 
    ```

7.  切换到解压目录.

    ```
    cd freeradius-server-2.x.y 
    ```

8.  在当前目录创建一个`fakeroot`环境, 并且使用已经包含在`debian`子目录的控制文件来编译包. 编译过程将会花费一些时间并且依赖计算机能性能. 你可以监控这个命令的结果,是否有 fatal 错误, 尽管他应该能够工作.

    ```
    fakeroot dpkg-buildpackage -b -uc 
    ```

9.  这将会创建所有需要的 deb 并且把他们放到`freeradius-server-2-x.y`目录外.
10.  通过下面的命令安装新的 deb.

    ```
    cd ../
    sudo dpkg -i freeradius_2.x.y+git_i386.deb freeradius-common_2.x.y+git_all.deb freeradius-utils_2.x.y+git_i386.deb libfreeradius2_2.x.y+git_i386.deb 
    ```

## 发生了什么?

我们刚刚创建和安装了最新的 FreeRADIUS 包到 Ubuntu. 下面是一些分解的重点.

### 安装 dpkg-dev

`dpkg-dev`包安装编译 deb 的需要的所有工具. 这包括`autoconfig`和`make`工具, 还有一个编译器. 也包括`dpkg-buildpackage`陈天旭, 我们将会使用来创建 deb.

### 使用 build-dep

`apt-get`是接下来使用的命令来指导他做什么. 当我们安装包时, 使用`install`命令, `build-dep`命令用来安装所有一个指定源码包的依赖.

我们没有使用随着 ubuntu 一起的源码包, 而是使用来自 FreeRADIUS 最新的源码包. 这表示他不是一个十分安全的方法, 尤其当最新的源码包含开发库跟 ubuntu 默认装的版本不一致的时候. 然而你将会被通知在编译过程中, 可以最后安装这个依赖的库.

### fakeroot

这个命令允许我们作为一个普通用户编译 deb, 并且是推荐的方式. `Fakeroot`跟着一个命令. 这个`fakeroot`调用的命令将会认为他是通过 root 用户有 root 权限对于文件操作. 在我们的例子中, 我们使用`dpkg-buildpackage`命令.

### dpkg-buildpackage

这个命令在 FreeRADIUS 源码目录运行. 他将会利用`debian`目录的指定信息来编译源码和术后创建各种 FreeRADIUS 包. `-b`选项是对于只有二进制的编译, 而`-uc`选项是跳过一个`gpg`密钥的签名.

### 安装 debs

我们安装基础的包. 当我们需要额外的二模考时, 他们可以作为一个包来安装. 需要的依赖可以通过使用`apt`命令来安装.

## 对于那些守旧派的人

尽管我们鼓励这样编译包, 我们没有阻止你使用`configure, make, make install`模式. 如果你运行`./configure --help`你会看到一个选项列表. 你可以用来 tweak FreeRADIUS 的编译过程. 你可以例如指定某个目录(--bindir=/usr/bin), 使能或禁用某个特性(--enable-developer), 或者包括某个包(--with-openssl)

# 安装的二进制程序

FreeRADIUS 有许多可执行文件被安装. 当在不同发行版之间移动时, 有小的不同点需要考虑.

一个这样的不同是配置文件的位置. 另一个不同是 FreeRADIUS 服务器程序的名称的不同. 在 Ubuntu(和 Debian)上, 他叫做`freeradius`. 在 CentOS 和 SLES, 他叫做`radiusd`. 下表列出了重要的可执行程序, 带有一个简短描述.

| 名称 | 描述 |
| --- | --- |
| /usr/sbin/raddebug | radmin 的 shell 脚本 wrapper 用于调试输出不必在 debug 模式下运行`radiusd`. |
| /usr/sbin/radiusd 或 /usr/sbin/freeradius | RADIUS 服务器程序 |
| /usr/sbin/radmin | 连接到一个运行的`radiusd` daemon 的管理程序 |
| /usr/bin/radclient | 用来发送各种 RADIUS 包到 RADIUS 服务器和显示回复 |
| /usr/bin/radconf2xml | 以 XML 格式显示当然服务器的配置 |
| /usr/bin/radcrypt | 加密或者检查密码以 DES 或 MD5 格式 |
| /usr/bin/radeapclient | 发送 EAP 包给一个 RADIUS 服务器 |
| /usr/bin/radlast | 系统的上一个命令的前端用于显示从 accounting 日志文件的输出 |
| /usr/bin/radsqlrelay | 用于管理记录在一个 SQL 日志文件的账户细节. 这个文件是`rlm_sql_log`模块创建的 |
| /usr/bin/radtest | 发送`Access-Request`(code 1)包给一个 RADIUS 服务器, 并且显示回复. `radclient`的前端 |
| /usr/bin/radwho | 从`radutmp`文件显示活跃的连接 |
| /usr/bin/radzap | shell 脚本 wrapper 用于移除在 session 数据库(file 或者 SQL)里的欺骗记录(rogue entries) |
| /usr/bin/smbencrypt | 给定一个明文密码的`nt`密码哈希, 被 MS-CHAP 需要. |

不是所有的列出的命令默认都可以执行. 命令像`radwho`和`radlast`依赖日志文件的存在, 因为他们作为这些命令的输入. 命令像`radmin`和`raddebug`需要一个在服务器上的特殊的设置在他们能够运行之前. 如果不是所有的这些命令都能在新的安装上运行, 不要担心.

# 是否用 root 运行

FreeRADIUS 应该被尽可能低的权限运行在生产环境中. 一个正常的安装创建一个专用的用户和组为了这个目的.

在 CentOS 和 SLES 上, 用户和组叫做`radiusd`. 在 Ubuntu 上, 用户和组叫做`freerad`. 只有特殊的配置才需要 root 权限.

# 客户端程序的字典访问

在一些安装上(SLES), FreeRADIUS 客户端程序不能读取字典文件. 这只有当你用一个普通用户运行时才会发生. 访问`dictionary`文件是需要的对于客户端来解析许多 RADIUS AVPs 到数字.

当这个问题存在时, 你将会的到下面的信息:

```
radclient: dict_init: Couldn't open dictionary "/etc/raddb/dictionary": Permission denied 
```

通过使用下面命令来修复该问题.

```
# chmod o+xr /etc/raddb
# chomod o+r /etc/raddb/dictionary 
```

在这之后一个普通用户应该可以运行 FreeRADIUS 客户端程序没有任何权限问题.

# 确保合适的启动

执行`radiusd -X`(或者`freeradius -X`在 Ubuntu), 用 root 用户, 将会启动 FreeRADIUS 以 debug 模式, 并且会给出提示, 如果有任何问题.

使用`Ctrl + C`来停止服务器程序.

如果你得到一个错误信息关于 1812 端口已经被使用, FreeRADIUS 已经运行. 你可以通过启动脚本关闭然后再运行来解决.

我们也需要确保 FreeRADIUS 在重启后能够启动带有其余的服务. 不幸的是每个发行版在这里工作不同. 下表可以被用做一个向导来管理 FreeRADIUS 的启动.

这些脚本的每一个可以被列出的参数之一调用.

| 发行版 | 启动脚本 |
| --- | --- |
| CentOS | `/etc/init.d/radiusd start/stop/restart` |
| SLES | `/etc/init.d/freeradius start/stop/restart` |
| Ubuntu | `/etc/init.d/freeradius start/stop/restart` |

| 发行版 | 使能和禁用 |
| --- | --- |
| CentOS | `/sbin/chkconfig radiusd on/off` |
| SLES | `/sbin/chkconfig -a freeradius` 和 `/sbin/chkconfig -d freeradius` |
| Ubuntu | `update-rc.d freeradius defaults` 和 `update-rc.d -f freeradius remove` |

确保 FreeRADIUS 在运行: `pidof freeradius` 或者 `ps aux | grep radius`

检查 FreeRADIUS 在使用哪个网络接口和 UDP 端口: `netstat -unap | grep radius`

# 总结

这一章是关于安装 FreeRADIUS 到你的机器.

*   安装预编译包. 最快和确定的方式来拥有一个工作的 FreeRADIUS 服务器.
*   从源码编译和安装 FreeRADIUS. 这个会有点复杂, 但是使你可以运行最新的版本.
*   FreeRADIUS 安装各种可执行程序并且包含各种模块. 有些模块是单独打包的, 基于不同的 linux 发行版.
*   你也用过安装客户端程序. 让你高效低测试和排查问题.
*   用普通用户运行客户端程序导致文件权限问题, 当尝试访问字典问题. 这个可以通过修改权限来修复.

我们也讨论了如何确保重启后自动启动, 以 debug 模式运行 FreeRADISU 来排查问题.

既然 FreeRADIUS 安装了并且运行在我们的机器上, 我们可以开始使用它. 开始使用 FreeRADIUS 是下一章的主题.

## 课堂作业 - 安装

1.  Isaac 是困惑的. 他安装 FreeRADIUS 到 Ubuntu 使用跟 CentOS 相同的方式, 但是 radiusd 二进制程序不见了, 这是为什么?
2.  在你安装 FreeRADIUS 的预编译程序之后, 你重启服务器. 你想要运行`radiusd`以 debug 模式, 但是`radiusd`抱怨, 地址以及被占用, 这是什么问题?
3.  你想要 FreeRADIUS 来连接到一个 CentOS 上的 MySQL 数据库. 预编译的 FreeRADIUS 包需要什么?
4.  你的一个朋友请求帮助. 他想要在 SLES11 上编译 FreeRADIUS, 到那时他尝试安装需要的编译库, 他不能找到所有的需要的库. 这是什么问题?