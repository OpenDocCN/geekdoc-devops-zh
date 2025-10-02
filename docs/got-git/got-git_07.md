# 7\. Git 的其它应用

Git 的伟大之处，还在于它不仅仅是作为版本库控制系统。Linus Torvalds 对自己最初设计的 Git 原型是这么评价的：Git 是一系列的底层工具用于内容的追踪，基于 Git 可以实现一个版本控制系统。现在 Git 已经是一个最成功的版本控制系统了，而基于 Git 的其他应用才刚刚开始。

维基是使用格式文本编辑网页，协同网页编辑的工具，又称为“Web 的版本控制”。在[`www.mzlinux.org/node/116`](http://www.mzlinux.org/node/116)可以看到一份用 Git 作为后端实现的维基列表（大部分是技术上的试验）。

SpaghettiFS 项目尝试用 Git 作为数据存储后端提供了一个用户空间的文件系统（FUSE、Filesystem in Userspace）。而另外的一些项目 gitfs 可以直接把 Git 版本库挂载为文件系统。

下面的章节，通过几个典型应用介绍 Git 在版本控制领域之外的应用，可以领略到 Git 的神奇。

目录:

*   7.1\. etckeeper
    *   7.1.1\. 安装 etckeeper
    *   7.1.2\. 配置 etckeeper
    *   7.1.3\. 使用 etckeeper
*   7.2\. Gistore
    *   7.2.1\. Gistore 的安装
        *   7.2.1.1\. 从源码安装 Gistore
        *   7.2.1.2\. 用`easy_install`安装
    *   7.2.2\. Gistore 的使用
        *   7.2.2.1\. 创建并初始化备份库
        *   7.2.2.2\. Gistore 的配置文件
        *   7.2.2.3\. Gistore 的备份项管理
        *   7.2.2.4\. 执行备份任务
        *   7.2.2.5\. 查看备份日志及数据
        *   7.2.2.6\. 查看及恢复备份数据
        *   7.2.2.7\. 备份回滚及设置
        *   7.2.2.8\. 注册备份任务别名
        *   7.2.2.9\. 自动备份：crontab
    *   7.2.3\. Gistore 双机备份
*   7.3\. 补丁中的二进制文件
    *   7.3.1\. Git 版本库中二进制文件变更的支持
    *   7.3.2\. 对非 Git 版本库中二进制文件变更的支持
    *   7.3.3\. 其他工具对 Git 扩展补丁文件的支持
*   7.4\. 云存储
    *   7.4.1\. 现有云存储的问题
    *   7.4.2\. Git 式云存储畅想

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 7.1\. etckeeper

Linux/Unix 的用户对`/etc`目录都是再熟悉不过了，在这个最重要的目录中保存了大部分软件的配置信息，借以实现软件的配置以及整个系统的启动过程控制。对于 Windows 用户来说，可以把`/etc`目录视为 Windows 中的注册表，只不过文件化了，可管理了。

这么重要的`/etc`目录，如果其中的文件被错误编辑或者删除，将会损失惨重。`etckeeper`这个软件可以帮助实现`/etc`目录的持续备份，借用分布式版本控制工具，如：git、mercurial、bazaar、darcs。

那么 etckeeper 是如何实现的呢？以 git 作为 etckeeper 的后端为例进行说明，其他的分布式版本控制系统大同小异。

*   将`/etc`目录 Git 化。将会创建 Git 库于目录`/etc/.git`中，`/etc`目录作为工作区。
*   与系统的包管理器，如 Debian/Ubuntu 的 apt，Redhat 上的 yum 等整合。一旦有软件包安装或删除，对`/etc`目录下的改动执行提交操作。
*   除了能够记录`/etc`目录中的文件内容，还可以记录文件属性等元信息。因为`/etc`目录下的文件的权限设置往往是非常重要和致命的。
*   因为`/etc`目录已经是一个版本库了，可以用 git 命令对`/etc`下的文件进行操作：查看历史，回退到历史版本...
*   也可以将`/etc`克隆到另外的主机中，实现双机备份。

## 7.1.1\. 安装 etckeeper

安装 etckeeper 是一个最简单的活，因为 etckeeper 在主流的 Linux 发行版都有对应的安装包。使用相应 Linux 平台的包管理器（apt、yum）即可安装。

在 Debian/Ubuntu 上安装 etckeeper 如下：

```
$ sudo aptitude install etckeeper

```

安装 etckeeper 软件包，会自动安装上一个分布式版本控制系统工具，除非已经安装了。这是因为 etckeeper 需要使用一个分布式版本控制系统作为存储管理后端。在 Debian/Ubuntu 上会依据下面的优先级进行安装：git > mercurial > bzr > darcs。

在 Debian/Ubuntu 上，使用**dpkg -s**命令查看 etckeeper 的软件包依赖，就会看到这个优先级。

```
$ dpkg -s etckeeper | grep "^Depends"
Depends: git-core (>= 1:1.5.4) | git (>= 1:1.7) | mercurial | bzr (>= 1.4~) | darcs, debconf (>= 0.5) | debconf-2.0

```

## 7.1.2\. 配置 etckeeper

配置 etckeeper 首先要选择好某一分布式版本库控制工具，如 Git，然后用相应的版本控制工具初始化`/etc`目录，并做一次提交。

*   编辑配置文件`/etc/etckeeper/etckeeper.conf`。

    只要有下面一条配置就够了。告诉 etckeeper 使用 git 作为数据管理后端。

    ```
    VCS="git"

    ```

*   初始化`/etc`目录。即将其 Git 化。执行下面的命令（需要以 root 用户身份），会将`/etc`目录 Git 化。

    整个过程可能会比较慢，因为要对`/etc`下的文件执行**git add**，因为文件太多，会慢一些。

    ```
    $ sudo etckeeper init

    ```

*   执行第一次提交。注意使用 etckeeper 命令而非 git 命令进行提交。

    ```
    $ sudo etckeeper commit "this is the first etckeeper commit..."

    ```

    整个过程可能会比较慢，主要是因为 etckeeper 要扫描`/etc`下不属于 root 用户的文件以及特殊权限的文件并进行记录。这是为了弥补 Git 本身不能记录文件属主、权限信息等。

## 7.1.3\. 使用 etckeeper

实际上由于 etckeeper 已经和系统的包管理工具进行了整合（如 Debian/Ubuntu 的 apt，Redhat 上的 yum 等），etckeeper 可以免维护运行。即一旦有软件包安装或删除，对`/etc`目录下的改动会自动执行提交操作。

当然也可以随时以`root`用户身份调用**etckeeper commit**命令对`/etc`目录的改动手动进行提交。

剩下的工作就交给 Git 了。可以在`/etc`目录执行**git log**、**git show**等操作。但要注意以 root 用户身份运行，因为`/etc/.git`目录的权限不允许普通用户操作。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 7.2\. Gistore

当了解了 etckeeper 之后，读者可能会如我一样的提问到：“有没有像 etckeeper 一样的工具，但是能备份任意的文件和目录呢？”

我在 Google 上搜索类似的工具无果，终于决定动手开发一个，因为无论是我还是我的客户，都需要一个更好用的备份工具。这就是 Gistore。

```
Gistore = Git + Store

```

2010 年 1 月，我在公司的博客上发表了 Gistore 0.1 版本的消息，参见：[`blog.ossxp.com/2010/01/406/`](http://blog.ossxp.com/2010/01/406/)。并将 Gistore 的源代码托管在了 Github 上，参见：[`github.com/ossxp-com/gistore`](http://github.com/ossxp-com/gistore)。

Gistore 出现受到了 etckeeper 的启发，通过 Gistore 用户可以对全盘任何目录的数据纳入到备份中，定制非常简单和方便。特点有：

*   使用 Git 作为数据后端。数据回复和历史查看等均使用熟悉的 Git 命令。
*   每次备份即为一次 Git 提交，支持文件的添加/删除/修改/重命名等。
*   每次备份的日志自动生成，内容为此次修改的摘要信息。
*   支持备份回滚，可以设定保存备份历史的天数，让备份的空间占用维持在一个相对稳定的水平上。
*   支持跨卷备份。备份的数据源可以来自任何卷/目录或者文件。
*   备份源如果已经 Git 化，也能够备份。例如`/etc`目录因为 etckeeper 被 Git 化，仍然可以对其用 gistore 进行备份。
*   多机异地备份非常简单，使用 git 克隆即可解决。可以采用 git 协议、http、或者更为安全的 ssh 协议。

说明：Gistore 只能运行在 Linux/Unix 上，而且最好以 root 用户身份运行，以避免因为授权问题导致有的文件不能备份。

## 7.2.1\. Gistore 的安装

### 7.2.1.1\. 从源码安装 Gistore

从源代码安装 Gistore，可以确保安装的是最新的版本。

*   先用 git 从 Github 上克隆代码库。

    ```
    $ git clone git://github.com/ossxp-com/gistore.git

    ```

*   执行`setup.py`脚本完成安装

    ```
    $ cd gistore
    $ sudo python setup.py install

    $ which gistore
    /usr/local/bin/gistore

    ```

### 7.2.1.2\. 用`easy_install`安装

Gistore 是用 Python 语言开发，已经在 PYPI 上注册：[`pypi.python.org/pypi/gistore`](http://pypi.python.org/pypi/gistore)。就像其他 Python 软件包一样，可以使用`easy_install`进行安装。

*   确保机器上已经安装了 setuptools。

    Setuptools 的官方网站在[`peak.telecommunity.com/DevCenter/setuptools`](http://peak.telecommunity.com/DevCenter/setuptools)。几乎每个 Linux 发行版都有 setuptools 的软件包，因此可以直接用包管理器进行安装。

    在 Debian/Ubuntu 上可以使用下面的命令安装 setuptools：

    ```
    $ sudo aptitude install python-setuptools

    $ which easy_install
    /usr/bin/easy_install

    ```

*   使用**easy_install**命令安装 Gistore

    ```
    $ sudo easy_install -U gistore

    ```

## 7.2.2\. Gistore 的使用

先熟悉一下 Gistore 的术语：

*   备份库：通过**gistore init**命令创建的，用于数据备份的数据仓库。备份库包含的数据有：

    *   Git 版本库相关目录和文件。如`repo.git`目录（相当于`.git`目录），`.gitignore`文件等。
    *   Gistore 相关配置。如`.gistore/config`文件。
*   备份项：可以为一个备份库指定任意多的备份项目。

    *   例如备份`/etc`目录，`/var/log`目录，`/boot/grub/menulist`文件等。

    *   备份项在备份库的`.gistore/config`文件中指定，如上述备份项在配置文件中写法为：

        ```
  [store /etc]
        [store /var/log]
        [store /boot/grub/menu.lst]

        ```

*   备份任务：在执行**gistore**命令时，可以指定一个任务或者多个任务。

    *   备份任务可以是对应的备份库的路径。可以使用绝对路径，也可以使用相对路径。
    *   如果不提供备份任务，缺省将当前目录作为备份库的所在。
    *   也可以使用一个任务别名来标识备份任务。
*   任务别名。

    *   在`/etc/gistore/tasks`目录中创建的备份库的符号链接的名称，作为这些备份库的任务别名。
    *   通过任务别名的机制，将可能分散在磁盘各处的备份库汇总一起，便于管理员定位备份库。
    *   将所有的别名显示出来，就是任务列表。

### 7.2.2.1\. 创建并初始化备份库

在使用 Gistore 开始备份之前，必须先初始化一个备份库。命令行格式如下：

```
gistore init [备份任务]

```

初始化备份库的示例如下：

*   将当前目录作为备份库进行初始化：

    ```
    $ mkdir backup
    $ cd backup
    $ gistore init

    ```

*   将指定的目录作为备份库进行初始化:

    ```
    $ sudo gistore init /backup/database

    ```

当一个备份库初始化完毕后，包含下列文件和目录：

*   目录`repo.git`：存储备份的 Git 版本库。
*   文件`.gistore/config`：Gistore 配置文件。
*   目录`logs`：Gistore 运行的日志记录。
*   目录`locks`：Gistore 运行的文件锁目录。

### 7.2.2.2\. Gistore 的配置文件

在每一个备份库的`.gistore`目录下的`config`文件是该备份库的配置文件，用于记录 Gistore 的备份项内容以及备份回滚设置等。

例如下面的配置内容：

```
1   # Global config for all sections
2   [main]
3   backend = git
4   backup_history = 200
5   backup_copies = 5
6   root_only = no
7   version = 2
8
9   [default]
10  keep_empty_dir = no
11  keep_perm = no
12
13  # Define your backup list below. Section name begin with 'store ' will be backup.
14  # eg: [store /etc]
15  [store /opt/mailman/archives]
16  [store /opt/mailman/conf]
17  [store /opt/mailman/lists]
18  [store /opt/moin/conf]
19  [store /opt/moin/sites]

```

如何理解这个配置文件呢？

*   第 2 行到第 7 行的`[main]`小节用于 Gistore 的全局设置。

*   第 3 行设置了 Gistore 使用的 SCM 后端为 Git，这是目前唯一可用的设置。

*   第 4 行设置了 Gistore 的每一个历史分支保存的最多的提交数目，缺省 200 个提交。当超过这个提交数目，进行备份回滚。

*   第 5 行设置了 Gistore 保存的历史分支数量，缺省 5 个历史分支。每当备份回滚时，会将备份主线保存到名为`gistore/1`的历史分支。

*   第 6 行设置非`root_only`模式。如果开启`root_only`模式，则只有 root 用户能够执行此备份库的备份。

*   第 7 行设置了 Gistore 备份库的版本格式。

*   第 9 行开始的`[default]`小节设置后面的备份项小节的缺省设置。在后面的`[store ...]`小节可以覆盖此缺省设置。

*   第 10 行设置是否保留空目录。暂未实现。

*   第 11 行设置是否保持文件属主和权限。暂未实现。

*   第 15 行到第 19 行是备份项小节，小节名称以`store`开始，后面的部分即为备份项的路径。

    如`[store /etc]`的含义是：要对`/etc`目录进行备份。

### 7.2.2.3\. Gistore 的备份项管理

当然可以直接编辑`.gistore/config`文件，通过添加或者删除`[store...]`小节的方式管理备份项。Gistore 还提供了两个命令进行备份项的管理。

**添加备份项**

进入备份库目录，执行下面的命令，添加备份项`/some/dir`。注意备份项要使用全路径，即要以“/”开始。

```
$ gistore add /some/dir

```

**删除备份项**

进入备份库目录，执行下面的命令，策删除备份项`/some/dir`。

```
$ gistore rm /some/dir

```

**查看备份项**

进入备份库目录，执行**gistore status**命令，显示备份库的设置以及备份项列表。

```
$ gistore status
         Task name : system
         Directory : /data/backup/gistore/system
           Backend : git
 Backup capability : 200 commits * 5 copies
       Backup list :
                     /backup/databases (--)
                     /backup/ldap (--)
                     /data/backup/gistore/system/.gistore (--)
                     /etc (AD)
                     /opt/cosign/conf (--)
                     /opt/cosign/factor (--)
                     /opt/cosign/lib (--)
                     /opt/gosa/conf (--)
                     /opt/ossxp/conf (--)
                     /opt/ossxp/ssl (--)

```

从备份库的状态输出，可以看到：

*   备份库的路径是`/data/backup/gistore/system`。

*   备份库有一个任务别名为`system`。

*   备份的容量是 200*5，如果按每天一次备份计算的话，总共保存 1000 天，差不多 3 年的数据备份。

*   在备份项列表，可以看到多达 10 项备份列表。

    每个备份项后面的括号代表其备份选项，其中`/etc`的备份选项为`AD`。`A`代表记录并保持授权，`D`的含义是保持空目录。

### 7.2.2.4\. 执行备份任务

执行备份任务非常简单：

*   进入到备份库根目录下，执行：

    ```
    $ sudo gistore commit

    ```

*   或者在命令行上指定备份库的路径。

    ```
    $ sudo gistore ci /backup/database

    ```

    说明：`ci`为`commit`命令的简称。

### 7.2.2.5\. 查看备份日志及数据

备份库中的`repo.git`就是备份数据所在的 Git 库，这个 Git 库是一个不带工作区的裸库。可以对其执行**git log**命令来查看备份日志。

因为并非采用通常`.git`作为版本库名称，而且不带工作区，需要通过`--git-dir`参数制定版本库位置，如下：

```
$ git --git-dir=repo.git log

```

当然，也可以进入到`repo.git`目录，执行**git log**命令。

下面是我公司内的服务器每日备份的日志片断：

```
commit 9d16b5668c1a09f6fa0b0142c6d34f3cbb33072f
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Thu Aug 5 04:00:23 2010 +0800

    Changes summary: total= 423, A: 407, D: 1, M: 15
    ------------------------------------------------
        A => etc/gistore/tasks/Makefile, opt/cosign/lib/share/locale/cosign.pot, opt/cosign/lib/templates-local.old/expired_error.html, opt/cosign/lib/templates-local.old3/error.html, opt/cosign/lib/templates/inc/en/0020_scm.html, ...402 more...
        D => etc/gistore/tasks/default
        M => .gistore/config, etc/gistore/tasks/gosa, etc/gistore/tasks/testlink, etc/group, etc/gshadow-, ...10 more...

commit 01b6bce2e4ee2f8cda57ceb3c4db0db9eb90bbed
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Wed Aug 4 04:01:09 2010 +0800

    Changes summary: total= 8, A: 7, M: 1
    -------------------------------------
        A => backup/databases/blog_bj/blog_bj.sql, backup/databases/ossxp/mysql.sql, backup/databases/redmine/redmine.sql, backup/databases/testlink/testlink-1.8.sql, backup/databases/testlink/testlink.sql, ...2 more...
        M => .gistore/config

commit 15ef2e88f33dfa7dfb04ecbcdb9e6b2a7c4e6b00
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Tue Aug 3 16:59:12 2010 +0800

    Changes summary: total= 2665, A: 2665
    -------------------------------------
        A => .gistore/config, etc/apache2/sites-available/gems, etc/group-, etc/pam.d/dovecot, etc/ssl/certs/0481cb65.0, ...2660 more...

commit 6883d5c2ca77caab9f9b2cfd68dcbc27526731c8
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Tue Aug 3 16:55:49 2010 +0800

    gistore root commit initialized.

```

从上面的日志可以看出：

*   备份发生在晚上 4 点钟左右。这是因为备份是晚上自动执行的。
*   最老的备份，即 ID 为`6883d5c`的提交，实际上是一个不包含任何数据的空备份，在数据发生回滚的时候，设置为回滚的起点。这个后面会提到。
*   ID 为`15ef2e8`的提交是一次手动提交。提交说明中可以看到添加了 2665 个文件。
*   最新的备份 ID 为`9d16b56`，其中既又文件添加（A），又有文件删除（D），还有文件变更（M），会随机选择各 5 个文件出现在提交日志中。

**如果想查看详细的文件变更列表？**

使用下面的命令：

```
$ git --git-dir=repo.git show --stat 9d16b56

commit 9d16b5668c1a09f6fa0b0142c6d34f3cbb33072f
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Thu Aug 5 04:00:23 2010 +0800

    Changes summary: total= 423, A: 407, D: 1, M: 15
    ------------------------------------------------
        A => etc/gistore/tasks/Makefile, opt/cosign/lib/share/locale/cosign.pot, opt/cosign/lib/templates-local.old/expired_error.html, opt/cosign/lib/templ
        D => etc/gistore/tasks/default
        M => .gistore/config, etc/gistore/tasks/gosa, etc/gistore/tasks/testlink, etc/group, etc/gshadow-, ...10 more...

 .gistore/config                                    |    4 +
 backup/databases/redmine/redmine.sql               |   44 +-
 etc/apache2/include/redmine/redmine.conf           |   40 +-
 etc/gistore/tasks/Makefile                         |    1 +
 etc/gistore/tasks/default                          |    1 -
 etc/gistore/tasks/gosa                             |    2 +-

 ...

 opt/gosa/conf/sieve-spam.txt                       |    6 +
 opt/gosa/conf/sieve-vacation.txt                   |    4 +
 opt/ossxp/conf/cron.d/ossxp-backup                 |    8 +-
 423 files changed, 30045 insertions(+), 51 deletions(-)

```

在备份库的`logs`目录下，还有一个备份过程的日志文件`logs/gitstore.log`。记录了每次备份的诊断信息，主要用于调试 Gistore。

### 7.2.2.6\. 查看及恢复备份数据

所有的备份数据，实际上都在`repo.git`目录指向的 Git 库中维护。如何获取呢？

**克隆方式检出**

执行下面的命令，克隆裸版本库`repo.git`：

```
$ git clone repo.git data

```

进入`data`目录，就可以以 Git 的方式查看历史数据，以及恢复历史数据。当然恢复的历史数据还要拷贝到原始位置才能实现数据的恢复。

**分离的版本库和工作区方式检出**

还有一个稍微复杂的方法，就是既然版本库已经在`repo.git`了，可以直接利用它，避免克隆导致空间上的浪费，尤其是当备份库异常庞大的情况。

*   创建一个工作目录，如`export`。

    ```
    $ mkdir export

    ```

*   设置环境变量，制定版本库和工作区的位置。注意使用绝对路径。

    下面的命令中，用**pwd**命令获得当前工作路径，借以得到绝对路径。

    ```
    $ export GIT_DIR=`pwd:file:`/repo.git
    $ export GIT_WORK_TREE=`pwd:file:`/export

    ```

*   然后就可以进入:file:` export`目录，执行 Git 操作了。

    ```
    $ git status
    $ git checkout .

    ```

**为什么没有历史备份？**

当针对`repo.git`执行**git log**的时候，满心期望能够看到备份的历史，但是看到的却只有孤零零的几个备份记录。不要着急，可能是备份回滚了。

参见下节的备份回滚，会找到如何获取更多历史备份的方法。

### 7.2.2.7\. 备份回滚及设置

我在开发 Gistore 时，最麻烦的就是备份历史的管理。如果不对备份历史进行回滚，必然会导致提交越来越多，备份空间占用越来越大，直至磁盘空间占慢。

最早的想法是使用**git rebase**。即将准备丢弃的早期备份历史合并成为一个提交，后面的提交变基到合并提交之上，这样就实现了对历史提交的丢弃。但是这样的操作即费时，又比较复杂。忽然又一天灵机一动，为什么不用分支来实现对回滚数据的保留？至于备份主线（master 分支）从一个新提交开始重建。

回滚后 master 分支如何从一个新提交开始呢？较早的实现是直接重置到一个空提交（gistore/0）上，但是这样会导致接下来的备份非常耗时。一个更好的办法是使用**git commit-tree**命令，直接从回滚前的 master 分支创建新提交。在读者看到这本书的时候，我应该已经才用了新的实现。

具体的实现过程是：

*   首先在备份库初始化的时候，就会建立一个空的提交，并打上里程碑 Tag：`gistore/0`（新的实现这个步骤变得没有必要）。
*   每次备份，都提交在 Git 库的主线 master 上。
*   当 Git 库的 master 主线的提交数达到规定的阈值（缺省 200），对 gistore 分支进行回滚，并基于当前 master 打上分支：`gistore/1`。
    *   如果设置了 5 个回滚分支，并且存在其他回滚分支，则分支依次向后回滚。
    *   删除`gistore/5`，`gistore/4`分支改名为`gistore/5`，等等，最后将`gistore/1`重命名为`gistore/2`。
    *   基于当前 master 建立分支`gistore/1`。
    *   将当前 master 以最新提交的树创建一个不含历史的提交，并重置到该提交。即 master 分支抛弃所有的备份历史。
    *   在新的 master 分支进行一次新的备份。
*   当回滚发生后，对备份库的远程数据同步不会有什么影响，传输的数据量也仅是新增备份和上一次备份的差异。

**如何找回历史备份？**

通过上面介绍的 Gistore 回滚的实现方法，会知道当回滚发生后，主线 master 只包含两个提交。一个是上一次备份的数据，另外一个是最新的数据备份。似乎大部分备份历史被完全丢弃了。其实，可以从分支`gistore/1`中看到最近备份的历史，还可以从其他分支（如果有的话）会看到更老的历史。

查看回滚分支的提交历史：

```
$ git --git-dir=repo.git log gistore/1

```

通过日志找出要恢复的时间点和提交号，使用**git checkout**即可检出历史版本。

### 7.2.2.8\. 注册备份任务别名

因为 Gistore 可以在任何目录下创建备份任务，管理员很难定位当前到底存在多少个备份库，因此需要提供一个机制，让管理员能够看到系统中有哪些备份库。还有，就是在使用 Gistore 时若使用长长的备份库路径作为参数会显得非常笨拙。任务别名就是用来解决这些问题的。

任务别名实际上就是在备份库在目录`/etc/gistore/tasks`下创建的符号连接。

为备份任务创建任务别名非常简单，只需要在`/etc/gistore/tasks`目录中创建的备份库的符号链接，该符号链接的名称，作为这些备份库的任务别名。

```
$ sudo ln -s /home/jiangxin/Desktop/mybackup /etc/gistore/tasks/jx
$ sudo ln -s /backup/database /etc/gistore/tasks/db

```

于是，就创建了两个任务别名，在以后执行备份时，可以简化备份命令：

```
$ sudo gistore commit jx
$ sudo gistore commit db

```

查看一份完整备份列表也非常简单，执行**gistore list**命令即可。

```
$ gistore list
db        : /backup/database
jx        : /home/jiangxin/Desktop/mybackup

```

当`gistore list`命令后面指定某个任务列表时，相当于执行`gistore status`命令，查看备份状态信息：

```
$ gistore list db

```

可以用一条命令对所有的任务别名执行备份：

```
$ gistore commit-all

```

### 7.2.2.9\. 自动备份：crontab

在`/etc/cron.d/`目录下创建一个文件，如`/etc/cron.d/gistore`，包含如下内容：

```
## gistore backup
0   4  *   *   *    root  /usr/bin/gistore commit-all

```

这样每天凌晨 4 点，就会以 root 用户身份执行**gistore commit-all**命令。

为了执行相应的备份计划，需要将备份库在`/etc/gistore/tasks`目录下创建符号链接。

## 7.2.3\. Gistore 双机备份

Gistore 备份库的主体就是`repo.git`，即一个 Git 库。可以通过架设一个 Git 服务器，远程主机通过克隆该备份库实现双机备份甚至是异地备份。而且最酷的是，整个数据同步的过程是可视的、快速的和无痛的，感谢伟大而又神奇的 Git。

最好使用公钥认证的基于 SSH 的 Git 服务器架设，因为一是可以实现无口令的数据同步，二是增加安全性，因为备份数据中可能包含敏感数据。

还有可以直接利用现成的`/etc/gistore/tasks`目录作为版本库的根。当然还需要在架设的 Git 服务器上，使用一个地址变换的小巧门。Gitosis 服务器软件的地址变换魔法正好可以帮助实现。参见第三十一章第 31.5 节“轻量级管理的 Git 服务”。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 7.3\. 补丁中的二进制文件

有的时候，需要将对代码的改动以补丁文件的方式进行传递，最终合并入版本库。例如直接在软件部署目录内进行改动，再将改动传送到开发平台。或者是因为在某个开源软件的官方版本库中没有提交权限，需要将自己的改动以补丁文件的方式提供给官方。

关于补丁文件的格式，补丁的生成和应用在第 3 篇第二十章“补丁文件交互”当中已经进行了介绍，使用的是**git format-patch**和**git am**命令，但这两个命令仅对 Git 库有效。如果没有使用 Git 对改动进行版本控制，而仅仅是两个目录：一个改动前的目录和一个改动后的目录，大部分人会选择使用 GNU 的**diff**命令及**patch**命令实现补丁文件的生成和补丁的应用。

但是 GNU 的**diff**命令（包括很多版本控制系统，如 SVN 的**svn diff**命令）生成的差异输出有一个非常大的不足或者说漏洞。就是差异输出不支持二进制文件。如果生成了新的二进制文件（如图片），或者二进制文件发生了变化，在差异输出中无法体现，当这样的差异文件被导出，应用到代码树中，会发现二进制文件或二进制文件的改动丢失了！

Git 突破了传统差异格式的限制，通过引入新的差异格式，实现了对二进制文件的支持。并且更为神奇的是，不必使用 Git 版本库对数据进行维护，可以直接对两个普通目录进行 Git 方式的差异比较和输出。

## 7.3.1\. Git 版本库中二进制文件变更的支持

对 Git 工作区的修改进行差异比较（**git diff --binary**），可以输出二进制的补丁文件。包含二进制文件差异的补丁文件可以通过**git apply**命令应用到版本库中。可以通过下面的示例，看看 Git 的补丁文件是如何对二进制文件提供支持的。

*   首先建立一个空的 Git 版本库。

    ```
    $ mkdir /tmp/test
    $ cd /tmp/test
    $ git init
    initialized empty Git repository in /tmp/test/.git/

    $ git ci --allow-empty -m initialized
    [master (root-commit) 2ca650c] initialized

    ```

*   然后在工作区创建一个文本文件`readme.txt`，以及一个二进制文件`binary.data`。

    二进制的数据读取自系统中的二进制文件`/bin/ls`，当然可以用任何其他二进制文件代替。

    ```
    $ echo hello > readme.txt
    $ dd if=/bin/ls of=binary.data count=1 bs=32
    记录了 1+0 的读入
    记录了 1+0 的写出
    32 字节(32 B)已复制，0.0001062 秒，301 kB/秒

    ```

    注：拷贝`/bin/ls`可执行文件（二进制）的前 32 个字节作为`binary.data`文件。

*   如果执行**git diff --cached**看到的是未扩展的差异格式。

    ```
    $ git add .
    $ git diff --cached
    diff --git a/binary.data b/binary.data
    new file mode 100644
    index 0000000..dc2e37f
    Binary files /dev/null and b/binary.data differ
    diff --git a/readme.txt b/readme.txt
    new file mode 100644
    index 0000000..ce01362
    --- /dev/null
    +++ b/readme.txt
    @@ -0,0 +1 @@
    +hello

    ```

    可以看到对于`binary.data`，此差异文件没有给出差异内容，而只是一行“`Binary files ... and ... differ`”。

*   再用**git diff --cached --binary**即增加了`--binary`参数试试。

    ```
    $ git diff --cached --binary
    diff --git a/binary.data b/binary.data
    new file mode 100644
    index 0000000000000000000000000000000000000000..dc2e37f81e0fa88308bec48cd5195b6542e61a20
    GIT binary patch
    literal 32
    bcmb<-^>JfjWMqH=CI&kO5HCR00W1UnGBE;C

    literal 0
    HcmV?d00001

    diff --git a/readme.txt b/readme.txt
    new file mode 100644
    index 0000000..ce01362
    --- /dev/null
    +++ b/readme.txt
    @@ -0,0 +1 @@
    +hello

    ```

    看到了么，此差异文件给出了二进制文件`binary.data`差异的内容，并且差异内容经过`base85`文本化了。

*   提交后，并用新的内容覆盖`binary.data`文件。

    ```
    $ git commit -m "new text file and binary file"
    [master 7ab2d01] new text file and binary file
     2 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 binary.data
     create mode 100644 readme.txt

    $ dd if=/bin/ls of=binary.data count=1 bs=64
    记录了 1+0 的读入
    记录了 1+0 的写出
    64 字节(64 B)已复制，0.00011264 秒，568 kB/秒

    $ git commit -a -m "change binary.data."
    [master a79bcbe] change binary.data.
     1 files changed, 0 insertions(+), 0 deletions(-)

    ```

*   看看更改二进制文件的新差异格式。

    ```
    $ git show HEAD --binary
    commit a79bcbe50c1d278db9c9db8e42d9bc5bc72bf031
    Author: Jiang Xin <jiangxin@ossxp.com>
    Date:   Sun Oct 10 19:22:30 2010 +0800

        change binary.data.

    diff --git a/binary.data b/binary.data
    index dc2e37f81e0fa88308bec48cd5195b6542e61a20..bf948689934caf2d874ff8168cb716fbc2a127c3 100644
    GIT binary patch
    delta 37
    hcmY#zn4qBGzyJX+<}pH93=9qo77QFfQiegA0RUZd1MdI;

    delta 4
    LcmZ=zn4kav0;B;E

    ```

*   更简单的，使用**git format-patch**命令，直接将最近的两次提交导出为补丁文件。

    ```
    $ git format-patch HEAD^^
    0001-new-text-file-and-binary-file.patch
    0002-change-binary.data.patch

    ```

    毫无疑问，这两个补丁文件都包含了对二进制文件的支持。

    ```
    $ cat 0002-change-binary.data.patch
    From a79bcbe50c1d278db9c9db8e42d9bc5bc72bf031 Mon Sep 17 00:00:00 2001
    From: Jiang Xin <jiangxin@ossxp.com>
    Date: Sun, 10 Oct 2010 19:22:30 +0800
    Subject: [PATCH 2/2] change binary.data.

    ---
     binary.data |  Bin 32 -> 64 bytes
     1 files changed, 0 insertions(+), 0 deletions(-)

    diff --git a/binary.data b/binary.data
    index dc2e37f81e0fa88308bec48cd5195b6542e61a20..bf948689934caf2d874ff8168cb716fbc2a127c3 100644
    GIT binary patch
    delta 37
    hcmY#zn4qBGzyJX+<}pH93=9qo77QFfQiegA0RUZd1MdI;

    delta 4
    LcmZ=zn4kav0;B;E

    --
    1.7.1

    ```

**那么如何将补丁合并入代码树呢？**

不能使用 GNU**patch**命令，因为前面曾经说过 GNU 的**diff**和**patch**不支持二进制文件的补丁。当然也不支持 Git 的新的补丁格式。将 Git 格式的补丁应用到代码树，只能使用 git 命令，即**git apply**命令。

接着前面的例子。首先将版本库重置到最近两次提交之前的状态，即丢弃最近的两次提交，然后将两个补丁都合并到代码树中。

*   重置版本库到两次提交之前。

    ```
    $ git reset --hard HEAD^^
    HEAD is now at 2ca650c initialized

    $ ls
    0001-new-text-file-and-binary-file.patch  0002-change-binary.data.patch

    ```

*   使用**git apply**应用补丁。

    ```
    $ git apply 0001-new-text-file-and-binary-file.patch 0002-change-binary.data.patch

    ```

*   可以看到 64 字节长度的`binary.data`又回来了。

    ```
    $ ls -l
    总用量 16
    -rw-r--r-- 1 jiangxin jiangxin 754 10 月 10 19:28 0001-new-text-file-and-binary-file.patch
    -rw-r--r-- 1 jiangxin jiangxin 524 10 月 10 19:28 0002-change-binary.data.patch
    -rw-r--r-- 1 jiangxin jiangxin  64 10 月 10 19:34 binary.data
    -rw-r--r-- 1 jiangxin jiangxin   6 10 月 10 19:34 readme.txt

    ```

*   最后不要忘了提交。

    ```
    $ git add readme.txt binary.data
    $ git commit -m "new text file and binary file from patch files."
    [master 7c1389f] new text file and binary file from patch files.
     2 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 binary.data
     create mode 100644 readme.txt

    ```

Git 对补丁文件的扩展，实际上不只是增加了二进制文件的支持，还提供了对文件重命名（`rename from`和`rename to`指令），文件拷贝（`copy from`和`copy to`指令），文件删除（`deleted file`指令）以及文件权限（`new file mode`和`new mode`指令）的支持。

## 7.3.2\. 对非 Git 版本库中二进制文件变更的支持

不在 Git 版本库中的文件和目录可以比较生成 Git 格式的补丁文件么，以及可以执行应用补丁（apply patch）的操作么？

是的，Git 的 diff 命令和 apply 命令支持对非 Git 版本库/工作区进行操作。但是当前 Git 最新版本(1.7.3)的**git apply**命令有一个 bug，这个 bug 导致目前的**git apply**命令只能应用 patch level（补丁文件前缀级别）为 1 的补丁。我已经将改正这个 Bug 的补丁文件提交到 Git 开发列表中，但有其他人先于我修正了这个 Bug。不管最终是谁修正的，在新版本的 Git 中，这个问题应该已经解决。参见我发给 Git 邮件列表的相关讨论。

*   [`marc.info/?l=git&m=129058163119515&w=2`](http://marc.info/?l=git&m=129058163119515&w=2)

下面的示例演示一下如何对非 Git 版本库使用**git diff**和**git patch**命令。首先准备两个目录，一个为`hello-1.0`目录，在其中创建一个文本文件以及一个二进制文件。

```
$ mkdir hello-1.0
$ echo hello > hello-1.0/readme.txt
$ dd if=/bin/ls of=hello-1.0/binary.dat count=1 bs=32
记录了 1+0 的读入
记录了 1+0 的写出
32 字节(32 B)已复制，0.0001026 秒，312 kB/秒

```

另外一个`hello-2.0`目录，其中的文本文件和二进制文件都有所更改。

```
$ mkdir hello-2.0
$ printf "hello\nworld\n" > hello-2.0/readme.txt
$ dd if=/bin/ls of=hello-2.0/binary.dat count=1 bs=64
记录了 1+0 的读入
记录了 1+0 的写出
64 字节(64 B)已复制，0.0001022 秒，626 kB/秒

```

然后执行**git diff**命令。命令中的`--no-index`参数对于不在版本库中的目录/文件进行比较时可以省略。其中还用了`--no-prefix`参数，这样就可以生成前缀级别（patch level）为 1 的补丁文件。

```
$ git diff --no-index --binary --no-prefix \
      hello-1.0 hello-2.0 > patch.txt
$ cat patch.txt
diff --git hello-1.0/binary.dat hello-2.0/binary.dat
index dc2e37f81e0fa88308bec48cd5195b6542e61a20..bf948689934caf2d874ff8168cb716fbc2a127c3 100644
GIT binary patch
delta 37
hcmY#zn4qBGzyJX+<}pH93=9qo77QFfQiegA0RUZd1MdI;

delta 4
LcmZ=zn4kav0;B;E

diff --git hello-1.0/readme.txt hello-2.0/readme.txt
index ce01362..94954ab 100644
--- hello-1.0/readme.txt
+++ hello-2.0/readme.txt
@@ -1 +1,2 @@
 hello
+world

```

进入到`hello-1.0`目录，执行**git apply**应用补丁，即使`hello-1.0`不是一个 Git 库。

```
$ cd hello-1.0
$ git apply ../patch.txt

```

会惊喜的发现`hello-1.0`应用补丁后，已经变得和`hello-2.0`一样了。

```
$ git diff --stat . ../hello-2.0

```

命令**git apply**也支持反向应用补丁。反向应用补丁后，`hello-1.0`中文件被还原，和`hello-2.0`比较又可以看到差异了。

```
$ git apply -R ../patch.txt
$ git diff --stat . ../hello-2.0
 {. => ../hello-2.0}/binary.dat |  Bin 32 -> 64 bytes
 {. => ../hello-2.0}/readme.txt |    1 +
 2 files changed, 1 insertions(+), 0 deletions(-)

```

## 7.3.3\. 其他工具对 Git 扩展补丁文件的支持

Git 对二进制提供支持的扩展的补丁文件格式，已经成为补丁文件格式的新标准被其他一些应用软件所接受。例如 Mercual/Hg 就提供了对 Git 扩展补丁格式的支持。

为**hg diff**命令增加`--git`参数，实现 Git 扩展 diff 格式输出。

```
$ hg diff --git

```

Hg 的 MQ 插件提供对 Git 补丁的支持。

```
$ cat .hg/patches/1.diff
# HG changeset patch
# User Jiang Xin <worldhello.net AT gmail DOT com>
# Date 1286711219 -28800
# Node ID ba66b7bca4baec41a7d29c5cae6bea6d868e2c4b
# Parent  0b44094c755e181446c65c16a8b602034e65efd7
new data

diff --git a/binary.data b/binary.data
new file mode 100644
index 0000000000000000000000000000000000000000..dc2e37f81e0fa88308bec48cd5195b6542e61a20
GIT binary patch
literal 32
bc$}+u^>JfjWMqH=CI&kO5HCR00n7&gGBE;C

```

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 7.4\. 云存储

通过云存储，将个人数据备份在网络上是非常吸引人的服务，比较著名的公司或产品有 dropbox、surgarsync、Live Mesh、Syncplicity 等。这些产品的特点是能够和操作系统的 shell 整合，例如和 Windows 的资源管理器或者 Linux 上的 nautilus，当本地有数据改动会自动的同步到远程的“云存储”上。用户可以在多个计算机或者手持设备上配置和同一个“云端”的帐号同步，从而实现在多个计算机或者多个手持设备上的数据同步。

## 7.4.1\. 现有云存储的问题

遗憾的是我并未使用过上述云存储服务，主要是支持 Linux 操作系统的云存储客户端比较少，或者即使有也因为网络的局限而无法访问。但是通过相关文档，还是可以了解到其实现的机理。

*   仅支持对部分历史数据的备份。

    dropbox 支持 30 天数据备份，surgarsync 每个文件仅保留 5 个备份（付费用户），对于免费用户仅 2 个备份。

*   数据同步对网络带宽依赖比较高。

*   “云端”被多个设备共享，冲突解决比较困难。

    Surgarsync 会将冲突的文件自动保存为多份，造成磁盘空间超出配额。其他有的产品在遇到冲突时停止同步，让用户决定选择哪个版本。

在 Git 书里介绍云存储，是因为上述云存储实现和 Git 有关么？不是。实际上通过上面各个云存储软件特性的介绍，有经验的 Linux 用户会感觉这些产品在数据同步时和 Linux 下的 rsync、unison 等数据同步工具非常类似，也许只是在服务器端增加了历史备份而已。

已经有用户尝试将云存储和 Git 结合使用，就是将 Git 库本身放在本机的云存储同步区（例如 dropbox 的 Dropbox 目录下），Git 库被同步至云端。即用云存储作为二传手，实际上还是基于本地协议操作 Git。这样实际会有问题的。

*   如果两台机器各自进行了提交，云存储同步一定会引发冲突，这种冲突是难以解决的。
*   云端对 Git 的的每个文件都进行备份，包括执行**git gc**命令打包后丢弃掉的松散对象。这实际对于 Git 是不需要的，会浪费本来就有限的空间配额。
*   因为版本库周期性的执行**git gc --auto**会导致即使 Git 版本库的一个小提交也可能会触发大量的云存储数据传输。

## 7.4.2\. Git 式云存储畅想

GitHub 是 Git 风格的云存储，但缺乏像前提到的云存储提供的傻瓜式服务，只有 Git 用户才能真正利用好，这大大限制了 Git 在云存储领域的推广。下面是我的一个预言：一个结合了 Git 和傻瓜式云存储的网络存储服务终将诞生。新的傻瓜式云存储有下列特征：

**差异同步传输**

对用户体验最为关键的是网络传输，如果用 Git 可以在同步时实现仅对文件差异进行数据传输，会大大提高同步效率。之所以现有的在线备份系统实现不了“差异同步传输”，是因为没有在本地对上一次同步时的数据做备份，只能通过时间戳或者文件的哈希值判断文件是否改变，而无法得出文件修改前后的差异。

可以很容易的测试云存储软件的网络传输性能。准备一个大的压缩包（使同步时压缩传输可以忽略），测试一下同步时间。再在该文件后面追加几个字节，然后检查同步时间。比较前后两个时间，就可以看出同步是否实现了仅对差异的同步传输。

**可预测的本地及云端存储空间的占用**

要想实现前面提到的差异同步传输，就必须在本地保存上一次同步时文件的备份。Subversion 是用一份冗余的本地拷贝实现的，这样本地存储是实际文件的两倍。Git 在本地是完全版本库，占用空间逐渐增加变得不可预测。

使用 Git 实现云存储，就要解决在本地以及在服务器端空间占用不可预测的问题。对于服务器端，可以采用前面介绍的 Gistore 软件采用的重整版本库的方法，或者通过基于历史版本重建提交然后变基来实现提交数量的删减。对于客户端来说，只保留一个提交就够了，类似 Subversion 的文件原始拷贝，这就需要在客户端基于 Git 原理的重新实现。

**更高效的云端存储效率**

现有的云存储效率不高，很有可能因为冗余备份而导致存储超过配额，即使服务提供商的配额计算是以最后一个版本计算的，实际的磁盘占用还是很可观。

Git 底层实现了一个对内容跟踪的文件系统，相同内容的文件即使文件名和目录不同，在 Git 看来都是一个对象并用一个文件存储（文件名是内容相关的 SHA1 哈希值）。因此 Git 方式实现的云存储在空间的节省上有先天的优势。

**自动的冲突解决**

冲突解决是和文件同步相关的，只有通过“差异同步传输”解决了同步的性能瓶颈，才能为冲突解决打下基础。先将冲突的各个版本都同步到本地，然后进行自动冲突解决，如果冲突无法自动解决，再提示用户手工解决冲突。还有，如果在手工冲突解决是引入类似 kdiff3 一样的工具，对用户更有吸引力。

**Git 提交中引入特殊标识**

如果使用变基或其他技术实现备份提交数量的删减，就会在云端的提交和本地数据合并上产生问题。可以通过为提交引入特殊的唯一性标识，不随着 Git 变基而改变，就像在 Gerrit 中的 Change-Id 标签一样。

我相信，基于 Git 的文件系统以及传输机理可以实现一个更好用的云存储服务。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.