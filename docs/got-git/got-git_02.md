# 2\. Git 独奏

通过前面的章节，我们了解了版本控制系统的历史，也学习了如何安装 Git。从这一章开始，我们就真正地进入到 Git 的学习中。Git 有着陡峭的学习曲线，即使是有着其他版本控制工具经验的老手也不例外。因为有经验的老手会按照在其他版本控制系统中遗留的习惯去想当然地操作 Git，努力地在 Git 中寻找对应物，最终会因为 Git 的“别扭”而放弃使用。这也是作者的亲身经历。

Git 是开源社区送给每一个人的宝贝，用好它可以实现个人的知识积累、保护好自己的数据，以及和他人分享自己的成果。这在其他的很多版本控制系统中是不可想象的。试问你会花费少则几万美金去购买商业版本的控制工具么？你会去使用必须搭建额外的服务器才能使用的版本控制系统么？你会把“鸡蛋”放在具有单点故障、服务器软硬件有可能崩溃的唯一的“篮子”里么？如果你不会，那么选择 Git，一定是最明智的选择。

本篇还不会涉及团队如何使用 Git，而是从个人的视角去研究如何用好 Git，并且还会揭示 Git 的原理和奥秘。本篇是全书最重要的部分，是下一步进行团队协作必需的知识准备，也是理解全书其余各部分内容的基础。本篇各章节将以实践、思考、再实践的方式循序渐进地学习 Git。到本篇的结尾，我们会发现通过“Git 独奏”也可以演绎出美妙的“乐曲”。

那么我们就开始第一个实践吧。

目录:

*   2.1\. Git 初始化
    *   2.1.1\. 创建版本库及第一次提交
    *   2.1.2\. 思考：为什么工作区下有一个`.git`目录？
    *   2.1.3\. 思考：**git config**命令参数的区别？
    *   2.1.4\. 思考：是谁完成的提交？
    *   2.1.5\. 思考：随意设置提交者姓名，是否太不安全？
    *   2.1.6\. 思考：命令别名是干什么的？
    *   2.1.7\. 备份本章的工作成果
*   2.2\. Git 暂存区
    *   2.2.1\. 修改不能直接提交？
    *   2.2.2\. 理解 Git 暂存区（stage）
    *   2.2.3\. Git Diff 魔法
    *   2.2.4\. 不要使用**git commit -a**
    *   2.2.5\. 搁置问题，暂存状态
*   2.3\. Git 对象
    *   2.3.1\. Git 对象库探秘
    *   2.3.2\. 问题：SHA1 哈希值到底是什么，如何生成的？
    *   2.3.3\. 问题：为什么不用顺序的数字来表示提交？
*   2.4\. Git 重置
    *   2.4.1\. 分支游标 master 的探秘
    *   2.4.2\. 用 reflog 挽救错误的重置
    *   2.4.3\. 深入了解**git reset**命令
*   2.5\. Git 检出
    *   2.5.1\. HEAD 的重置即检出
    *   2.5.2\. 挽救分离头指针
    *   2.5.3\. 深入了解**git checkout**命令
*   2.6\. 恢复进度
    *   2.6.1\. 继续暂存区未完成的实践
    *   2.6.2\. 使用**git stash**
    *   2.6.3\. 探秘**git stash**
*   2.7\. Git 基本操作
    *   2.7.1\. 先来合个影
    *   2.7.2\. 删除文件
        *   2.7.2.1\. 本地删除不是真的删除
        *   2.7.2.2\. 执行**git rm**命令删除文件
        *   2.7.2.3\. 命令**git add -u**快速标记删除
    *   2.7.3\. 恢复删除的文件
    *   2.7.4\. 移动文件
    *   2.7.5\. 一个显示版本号的`Hello World`
    *   2.7.6\. 使用**git add -i**选择性添加
    *   2.7.7\. `Hello world`引发的新问题
    *   2.7.8\. 文件忽略
    *   2.7.9\. 文件归档
*   2.8\. 历史穿梭
    *   2.8.1\. 图形工具：gitk
    *   2.8.2\. 图形工具：gitg
    *   2.8.3\. 图形工具：qgit
    *   2.8.4\. 命令行工具
        *   2.8.4.1\. 版本表示法：**git rev-parse**
        *   2.8.4.2\. 版本范围表示法：git rev-list
        *   2.8.4.3\. 浏览日志：**git log**
        *   2.8.4.4\. 差异比较：**git diff**
        *   2.8.4.5\. 文件追溯：**git blame**
        *   2.8.4.6\. 二分查找：**git bisect**
        *   2.8.4.7\. 获取历史版本
*   2.9\. 改变历史
    *   2.9.1\. 悔棋
    *   2.9.2\. 多步悔棋
    *   2.9.3\. 回到未来
        *   2.9.3.1\. 时间旅行一
        *   2.9.3.2\. 时间旅行二
        *   2.9.3.3\. 时间旅行三
    *   2.9.4\. 丢弃历史
    *   2.9.5\. 反转提交
*   2.10\. Git 克隆
    *   2.10.1\. 鸡蛋不装在一个篮子里
    *   2.10.2\. 对等工作区
    *   2.10.3\. 克隆生成裸版本库
    *   2.10.4\. 创建生成裸版本库
*   2.11\. Git 库管理
    *   2.11.1\. 对象和引用哪里去了？
    *   2.11.2\. 暂存区操作引入的临时对象
    *   2.11.3\. 重置操作引入的对象
    *   2.11.4\. Git 管家：`git gc`
    *   2.11.5\. Git 管家的自动执行

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 2.1\. Git 初始化

## 2.1.1\. 创建版本库及第一次提交

您当前使用的是 1.5.6 或更高版本的 Git 么？

```
$ git --version
git version 1.7.11.2

```

Git 是一个活跃的项目，仍在不断的进化之中，不同 Git 版本的功能不尽相同。本书对 Git 的介绍涵盖了 1.5.6 到 1.7.11 版本，这也是目前 Git 的主要版本。如果您使用的 Git 版本低于 1.5.6，那么请升级到 1.5.6 或更高的版本。本书示例使用的是 1.7.11.2 版本的 Git，我们会尽可能地指出那些低版本不兼容的命令及参数。

在开始 Git 之旅之前，我们需要设置一下 Git 的环境变量，这个设置是一次性的工作。即这些设置会在全局文件（用户主目录下的`.gitconfig`）或系统文件（`/etc/gitconfig`）中做永久的记录。

*   告诉 Git 当前用户的姓名和邮件地址，配置的用户名和邮件地址将在版本库提交时作为提交者的用户名和邮件地址。

    注意下面的两条命令不要照抄照搬，而是用您自己的用户名和邮件地址代替这里的用户名和邮件地址，否则您的劳动成果（提交内容）可要算到作者的头上了。

    ```
    $ git config --global user.name "Jiang Xin"
    $ git config --global user.email jiangxin@ossxp.com

    ```

*   设置一些 Git 别名，以便可以使用更为简洁的子命令。

    例如：输入**git ci**即相当于**git commit -s**[[1]](#id11)，输入**git st**即相当于**git -p status**[[2]](#id12)。

    *   如果拥有系统管理员权限（可以执行**sudo**命令），希望注册的命令别名能够被所有用户使用，可以执行如下命令：

        ```
  $ sudo git config --system alias.br branch
        $ sudo git config --system alias.ci "commit -s"
        $ sudo git config --system alias.co checkout
        $ sudo git config --system alias.st "-p status"

        ```

    *   也可以运行下面的命令，只在本用户的全局配置中添加 Git 命令别名：

        ```
  $ git config --global alias.st status
        $ git config --global alias.ci "commit -s"
        $ git config --global alias.co checkout
        $ git config --global alias.br branch

        ```

Git 的所有操作，包括创建版本库等管理操作都用**git**一个命令即可完成，不像其他有的版本控制系统（如 Subversion），一些涉及管理的操作要使用另外的命令（如**svnadmin**）。创建 Git 版本库，可以直接进入到包含数据（文件和子目录）的目录下，通过执行**git init**完成版本库的初始化。

下面就从一个空目录开始初始化版本库，这个版本库命名为“demo”，这个 DEMO 版本库将贯穿本篇始终。为了方便说明，使用了名为`/path/to/my/workspace`的目录作为个人的工作区根目录，您可以在磁盘中创建该目录并设置正确的权限。

首先建立一个新的工作目录，进入该目录后，执行**git init**创建版本库。

```
$ cd /path/to/my/workspace
$ mkdir demo
$ cd demo
$ git init
初始化空的 Git 版本库于 /path/to/my/workspace/demo/.git/

```

实际上，如果 Git 的版本是 1.6.5 或更新的版本，可以在**git init**命令的后面直接输入目录名称，自动完成目录的创建。

```
$ cd /path/to/my/workspace
$ git init demo
初始化空的 Git 版本库于 /path/to/my/workspace/demo/.git/
$ cd demo

```

从上面版本库初始化后的输出中，可以看到执行**git init**命令在工作区创建了隐藏目录`.git`。

```
$ ls -aF
./  ../  .git/

```

这个隐藏的`.git`目录就是 Git 版本库（又叫仓库，repository）。

`.git`版本库目录所在的目录，即`/path/to/my/workspace/demo`目录称为**工作区**，目前工作区除了包含一个隐藏的 file:.git 版本库目录外空无一物。

下面为工作区中加点料：在工作区中创建一个文件`welcome.txt`，内容就是一行“`Hello.`”。

```
$ echo "Hello." > welcome.txt

```

为了将这个新建立的文件添加到版本库，需要执行下面的命令：

```
$ git add welcome.txt

```

切记，到这里还没有完。Git 和大部分其他版本控制系统都需要再执行一次提交操作，对于 Git 来说就是执行**git commit**命令完成提交。在提交过程中需要输入提交说明，这个要求对于 Git 来说是强制性的，不像其他很多版本控制系统（如 CVS、Subversion）允许空白的提交说明。在 Git 提交时，如果在命令行不提供提交说明（没有使用`-m`参数），Git 会自动打开一个编辑器，要求您在其中输入提交说明，输入完毕保存退出。需要说明的是，读者要在一定程度上掌握 vim 或 emacs 这两种 Linux 下常用编辑器的编辑技巧，否则保存退出也会成为问题。

下面进行提交。为了说明方便，使用`-m`参数直接给出了提交说明。

```
$ git ci -m "initialized"
[master（根提交） 7e749cc] initialized
 1 个文件被修改，插入 1 行(+)
 create mode 100644 welcome.txt

```

从上面的命令及输出可以看出：

*   使用了 Git 命令别名，即**git ci**相当于执行**git commit**。在本节的一开始就进行了 Git 命令别名的设置。
*   通过`-m`参数设置提交说明为：”initialized”。该提交说明也显示在命令输出的第一行中。
*   命令输出的第一行还显示了当前处于名为`master`的分支上，提交 ID 为 7e749cc[[3]](#id13)，且该提交是该分支的第一个提交，即根提交（root-commit）。根提交和其他提交的区别在于没有关联的父提交，这会在后面的章节中加以讨论。
*   命令输出的第二行开始显示本次提交所做修改的统计：修改了一个文件，包含一行的插入。

## 2.1.2\. 思考：为什么工作区下有一个`.git`目录？

Git 及其他分布式版本控制系统（如 Mercurial/Hg、Bazaar）的一个显著特点是，版本库位于工作区的根目录下。对于 Git 来说，版本库位于工作区根目录下的`.git`目录中，且仅此一处，在工作区的子目录下则没有任何其他跟踪文件或目录。Git 的这个设计要比 CVS、Subversion 这些传统的集中式版本控制工具来说方便多了。

看看版本控制系统前辈们是如何对工作区的跟踪进行设计的。通过其各自设计的优缺点，我们会更加深刻地体会到 Git 实现的必要和巧妙。

对于 CVS，工作区的根目录及每一个子目录下都有一个`CVS`目录，`CVS`目录中包含几个配置文件，建立了对版本库的追踪。如`CVS`目录下的`Entries`文件记录了从版本库检出到工作区的文件的名称、版本和时间戳等，这样就可以通过对工作区文件时间戳的改变来判断文件是否更改。这样设计的好处是，可以将工作区移动到任何其他目录中，而工作区和版本控制服务器的映射关系保持不变，这样工作区依然能够正常工作。甚至还将工作区的某个子目录移动到其他位置，形成新的工作区，在新的工作区下仍然可以完成版本控制相关的操作。但是缺点也很多，例如工作区文件修改了，因为没有原始文件做比对，因此向服务器提交修改的时候只能对整个文件进行传输而不能仅传输文件的改动部分，导致从客户端到服务器的网络传输效率降低。还有一个风险是信息泄漏。例如 Web 服务器的目录下如果包含了`CVS`目录，黑客就可以通过扫描`CVS/Entries`文件得到目录下的文件列表，由此造成信息泄漏。

对于 Subversion 来说，工作区的根目录和每一个子目录下都有一个`.svn`目录。目录`.svn`中不但包含了类似 CVS 的跟踪目录下的配置文件，还包含了当前工作区下每一个文件的拷贝。多出文件的原始拷贝让某些 svn 命令可以脱离版本库执行，还可以在由客户端向服务器提交时，仅仅对文件改动的内容进行提交，因为改动的文件可以和原始拷贝进行差异比较。但是这么做的缺点除了像 CVS 因为引入`CVS`跟踪目录而造成的信息泄漏的风险外，还导致了加倍占用工作区的空间。再有一个不方便的地方就是，当在工作区目录下针对文件内容进行搜索的时候，会因为`.svn`目录下文件的原始拷贝，导致搜索的结果加倍，而出现混乱的搜索结果。

有的版本控制系统，在工作区根本就没有任何跟踪文件，例如，某款版本控制的商业软件（就不点名了），工作区就非常干净没有任何的配置文件和配置目录。但是这样的设计更加糟糕，因为它实际上是由服务器端建立的文件跟踪，在服务器端的数据库中保存了一个表格：哪台客户端，在哪个本地目录检出了哪个版本的版本库文件。这样做的后果是，如果客户端将工作区移动或改名会导致文件的跟踪状态丢失，出现文件状态未知的问题。客户端操作系统重装，也会导致文件跟踪状态丢失。

Git 的这种设计，将版本库放在工作区根目录下，所有的版本控制操作（除了和其他远程版本库之间的互操作）都在本地即可完成，不像 Subversion 只有寥寥无几的几个命令才能脱离网络执行。而且 Git 也没有 CVS 和 Subversion 的安全泄漏问题（只要保护好`.git`目录），也没有 Subversion 在本地文件搜索时出现搜索结果混乱的问题，甚至 Git 还提供了一条**git grep**命令来更好地搜索工作区的文件内容。

例如作者在本书的 Git 库中执行下面的命令对版本库中的文件进行内容搜索：

```
$ git grep "工作区文件内容搜索"
02-git-solo/010-git-init.rst::command:`git grep`\ 命令来更好地搜索工作区的文件内容。

```

**当工作区中包含了子目录，在子目录中执行 Git 命令时，如何定位版本库呢？**

实际上，当在 Git 工作区目录下执行操作的时候，会对目录依次向上递归查找`.git` 目录，找到的`.git`目录就是工作区对应的版本库，`.git`所在的目录就是工作区的根目录，文件`.git/index`记录了工作区文件的状态（实际上是暂存区的状态）。

例如在非 Git 工作区执行**git**命令，会因为找不到`.git`目录而报错。

```
$ cd /path/to/my/workspace/
$ git status
fatal: Not a git repository (or any of the parent directories): .git

```

如果跟踪一下执行**git status**命令时的磁盘访问[[4]](#id14)，会看到沿目录依次向上递归的过程。

```
$ strace -e 'trace=file' git status
...
getcwd("/path/to/my/workspace", 4096)           = 14
...
access(".git/objects", X_OK)            = -1 ENOENT (No such file or directory)
access("./objects", X_OK)               = -1 ENOENT (No such file or directory)
...
chdir("..")                             = 0
...
access(".git/objects", X_OK)            = -1 ENOENT (No such file or directory)
access("./objects", X_OK)               = -1 ENOENT (No such file or directory)
...
chdir("..")                             = 0
...
access(".git/objects", X_OK)            = -1 ENOENT (No such file or directory)
access("./objects", X_OK)               = -1 ENOENT (No such file or directory)
fatal: Not a git repository (or any of the parent directories): .git

```

**那么有什么办法知道 Git 版本库的位置，以及工作区的根目录在哪里呢？**

当在工作区执行**git**命令时，上面查找版本库的操作总是默默地执行，就好像什么也没有发生的一样。如果希望显示工作区的根目录，Git 有一个底层命令可以实现。

*   在工作区下建立目录`a/b/c`，进入到该目录中。

    ```
    $ cd /path/to/my/workspace/demo/
    $ mkdir -p a/b/c
    $ cd /path/to/my/workspace/demo/a/b/c

    ```

*   显示版本库`.git`目录所在的位置。

    ```
    $ git rev-parse --git-dir
    /path/to/my/workspace/demo/.git

    ```

*   显示工作区根目录。

    ```
    $ git rev-parse --show-toplevel
    /path/to/my/workspace/demo

    ```

*   相对于工作区根目录的相对目录。

    ```
    $ git rev-parse --show-prefix
    a/b/c/

    ```

*   显示从当前目录（cd）后退（up）到工作区的根的深度。

    ```
    $ git rev-parse --show-cdup
    ../../../

    ```

**把版本库:file:`.git`目录放在工作区，是不是太不安全了？**

从存储安全的角度上来讲，将版本库放在工作区目录下，有点“把鸡蛋装在一个篮子里”的味道。如果忘记了工作区中还有版本库，直接从工作区的根执行目录删除就会连版本库一并删除，这个风险的确是蛮高的。将版本库和工作区拆开似乎更加安全，但是不要忘了之前的讨论，将版本库和工作区拆开，就要引入其他机制以便实现版本库对工作区的追踪。

Git 克隆可以降低因为版本库和工作区混杂在一起导致的版本库被破坏的风险。可以通过克隆版本库，在本机另外的磁盘/目录中建立 Git 克隆，并在工作区有改动提交时，手动或自动地执行向克隆版本库的推送（`git push`）操作。如果使用网络协议，还可以实现在其他机器上建立克隆，这样就更安全了（双机备份）。对于使用 Git 做版本控制的团队，每个人都是一个备份，因此团队开发中的 Git 版本库更安全，管理员甚至根本无须顾虑版本库存储安全问题。

## 2.1.3\. 思考：**git config**命令参数的区别？

在之前出现的**git config**命令，有的使用了`--global`参数，有的使用了`--system`参数，这两个参数有什么区别么？执行下面的命令，您就明白**git config`** 命令实际操作的文件了。

*   执行下面的命令，将打开`/path/to/my/workspace/demo/.git/config`文件进行编辑。

    ```
    $ cd /path/to/my/workspace/demo/
    $ git config -e

    ```

*   执行下面的命令，将打开`/home/jiangxin/.gitconfig`（用户主目录下的`.gitconfig`文件）全局配置文件进行编辑。

    ```
    $ git config -e --global

    ```

*   执行下面的命令，将打开`/etc/gitconfig`系统级配置文件进行编辑。

    如果 Git 安装在`/usr/local/bin`下，这个系统级的配置文件也可能是在`/usr/local/etc/gitconfig`。

    ```
    $ git config -e --system

    ```

Git 的三个配置文件分别是版本库级别的配置文件、全局配置文件（用户主目录下）和系统级配置文件（`/etc`目录下）。其中版本库级别配置文件的优先级最高，全局配置文件其次，系统级配置文件优先级最低。这样的优先级设置就可以让版本库`.git`目录下的`config`文件中的配置可以覆盖用户主目录下的 Git 环境配置。而用户主目录下的配置也可以覆盖系统的 Git 配置文件。

执行前面的三个**git config**命令，会看到这三个级别配置文件的格式和内容，原来 Git 配置文件采用的是 INI 文件格式。示例如下：

```
$ cat /path/to/my/workspace/demo/.git/config
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true

```

命令**git config**可以用于读取和更改 INI 配置文件的内容。使用命令**git config <section>.<key>**，来读取 INI 配置文件中某个配置的键值。例如读取`[core]`小节的`bare`的属性值，可以用如下命令：

```
$ git config core.bare
false

```

如果想更改或设置 INI 文件中某个属性的值也非常简单，命令格式是：**git config <section>.<key> <value>**。可以用如下操作：

```
$ git config a.b something
$ git config x.y.z others

```

如果打开`.git/config`文件，会看到如下内容：

```
[a]
        b = something

[x "y"]
        z = others

```

对于类似`[x "y"]`一样的配置小节，会在本书第三篇介绍远程版本库的章节中经常遇到。

从上面的介绍中，可以看到使用**git config**命令可以非常方便地操作 INI 文件，实际上可以用**git config**命令操作任何其他的 INI 文件。

*   向配置文件`test.ini`中添加配置。

    ```
    $ GIT_CONFIG=test.ini git config a.b.c.d "hello, world"

    ```

*   从配置文件`test.ini`中读取配置。

    ```
    $ GIT_CONFIG=test.ini git config a.b.c.d
    hello, world

    ```

后面介绍的 git-svn 软件，就使用这个技术读写 git-svn 专有的配置文件。

## 2.1.4\. 思考：是谁完成的提交？

在本章的一开始，先为 Git 设置了`user.name`和`user.email`全局环境变量，如果不设置会有什么结果呢？

执行下面的命令，删除 Git 全局配置文件中关于`user.name`和`user.email`的设置：

```
$ git config --unset --global user.name
$ git config --unset --global user.email

```

这下关于用户姓名和邮件的设置都被清空了，执行下面的命令将看不到输出。

```
$ git config user.name
$ git config user.email

```

下面再尝试进行一次提交，看看提交的过程会有什么不同，以及提交之后显示的提交者是谁？

在下面的命令中使用了`--allow-empty`参数，这是因为没有对工作区的文件进行任何修改，Git 默认不会执行提交，使用了`--allow-empty`参数后，允许执行空白提交。

```
$ cd /path/to/my/workspace/demo
$ git commit --allow-empty -m "who does commit?"
[master 252dc53] who does commit?
 Committer: JiangXin <jiangxin@hp.moon.ossxp.com>
Your name and email address were configured automatically based
on your username and hostname. Please check that they are accurate.
You can suppress this message by setting them explicitly:

    git config --global user.name "Your Name"
    git config --global user.email you@example.com

If the identity used for this commit is wrong, you can fix it with:

    git commit --amend --author='Your Name <you@example.com>'

```

喔，因为没有设置`user.name`和`user.email`变量，提交输出乱得一塌糊涂。仔细看看上面执行`git commit`命令的输出，原来 Git 提供了详细的帮助指引来告诉如何设置必需的变量，以及如何修改之前提交中出现的错误的提交者信息。

看看此时版本库的提交日志，会看到有两次提交。

注意：下面的输出和您的输出肯定会有所不同，一个是提交时间会不一样，再有就是由 40 位十六进制数字组成的提交 ID 也不可能一样，甚至本书中凡是您亲自完成的提交，相关的 40 位魔幻般的数字 ID 都会不一样（原因会在后面的章节看到）。因此凡是涉及数字 ID 和作者示例不一致的时候，以读者自己的数字 ID 为准，作者提供的仅是示例和参考，切记切记。

```
$ git log --pretty=fuller
commit 252dc539b5b5f9683edd54849c8e0a246e88979c
Author:     JiangXin <jiangxin@hp.moon.ossxp.com>
AuthorDate: Mon Nov 29 10:39:35 2010 +0800
Commit:     JiangXin <jiangxin@hp.moon.ossxp.com>
CommitDate: Mon Nov 29 10:39:35 2010 +0800

    who does commit?

commit 9e8a761ff9dd343a1380032884f488a2422c495a
Author:     Jiang Xin <jiangxin@ossxp.com>
AuthorDate: Sun Nov 28 12:48:26 2010 +0800
Commit:     Jiang Xin <jiangxin@ossxp.com>
CommitDate: Sun Nov 28 12:48:26 2010 +0800

    initialized.

```

最早的提交（下面的提交），提交者的信息是由之前设置的环境变量`user.name`和`user.email`给出的。而最新的提交（上面第一个提交）因为删除了`user.name`和`user.email`，提交时 Git 对提交者的用户名和邮件地址做了大胆的猜测，这个猜测可能是错的。

为了保证提交时提交者和作者信息的正确性，重新恢复`user.name`和`user.email`的设置。记住不要照抄照搬下面的命令，请使用您自己的用户名和邮件地址。

```
$ git config --global user.name "Jiang Xin"
$ git config --global user.email jiangxin@ossxp.com

```

然后执行下面的命令，重新修改最新的提交，改正作者和提交者的错误信息。

```
$ git commit --amend --allow-empty --reset-author

```

说明：

*   参数`--amend`是对刚刚的提交进行修补，这样就可以改正前面错误的提交（用户信息错误），而不会产生另外的新提交。
*   参数`--allow-empty`是因为要进行修补的提交实际上是一个空白提交，Git 默认不允许空白提交。
*   参数`--reset-author`的含义是将 Author（提交者）的 ID 重置，否则只会影响最新的 Commit（提交者）的 ID。这条命令也会重置`AuthorDate`信息。

通过日志，可以看到最新提交的作者和提交者的信息已经改正了。

```
$ git log --pretty=fuller
commit a0c641e92b10d8bcca1ed1bf84ca80340fdefee6
Author:     Jiang Xin <jiangxin@ossxp.com>
AuthorDate: Mon Nov 29 11:00:06 2010 +0800
Commit:     Jiang Xin <jiangxin@ossxp.com>
CommitDate: Mon Nov 29 11:00:06 2010 +0800

    who does commit?

commit 9e8a761ff9dd343a1380032884f488a2422c495a
Author:     Jiang Xin <jiangxin@ossxp.com>
AuthorDate: Sun Nov 28 12:48:26 2010 +0800
Commit:     Jiang Xin <jiangxin@ossxp.com>
CommitDate: Sun Nov 28 12:48:26 2010 +0800

    initialized.

```

## 2.1.5\. 思考：随意设置提交者姓名，是否太不安全？

使用过 CVS、Subversion 等集中式版本控制系统的用户会知道，每次提交的时候须要认证，认证成功后，登录 ID 就作为提交者 ID 出现在版本库的提交日志中。很显然，对于 CVS 或 Subversion 这样的版本控制系统，很难冒充他人提交。那么像 Git 这样的分布式版本控制系统，可以随心所欲的设定提交者，这似乎太不安全了。

Git 可以随意设置提交的用户名和邮件地址信息，这是分布式版本控制系统的特性使然，每个人都是自己版本库的主人，很难也没有必要进行身份认证从而使用经过认证的用户名作为提交的用户名。

在进行“独奏”的时候，还要为自己强制加上一个“指纹识别”实在是太没有必要了。但是团队合作时授权就成为必需了。不过一般来说，设置的 Git 服务器只会在个人向服务器版本库执行推送操作（推送其本地提交）的时候进行身份认证，并不对所推送的提交本身所包含的用户名作出检查。但 Android 项目是个例外。

Android 项目为了更好的使用 Git 实现对代码的集中管理，开发了一套叫做 Gerrit 的审核服务器来管理 Git 提交，对提交者的邮件地址进行审核。例如下面的示例中在向 Gerrit 服务器推送的时候，提交中的提交者邮件地址为`jiangxin@ossxp.com`，但是在 Gerrit 中注册用户时使用的邮件地址为`jiangxin@moon.ossxp.com`。因为两者不匹配，从而导致推送失败。

```
$ git push origin master
Counting objects: 3, done.
Writing objects: 100% (3/3), 222 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To ssh://localhost:29418/new/project.git
 ! [remote rejected] master -> master (you are not committer jiangxin@ossxp.com)
error: failed to push some refs to 'ssh://localhost:29418/new/project.git'

```

即使没有使用类似 Gerrit 的服务，作为提交者也不应该随意改变`user.name`和`user.email`的环境变量设置，因为当多人协同时这会给他人造成迷惑，也会给一些项目管理软件造成麻烦。

例如 Redmine 是一款实现需求管理和缺陷跟踪的项目管理软件，可以和 Git 版本库实现整合。Git 的提交可以直接关闭 Redmine 上的 Bug，还有 Git 的提交可以反映出项目成员的工作进度。Redmine 中的用户（项目成员）是用一个 ID 做标识，而 Git 的提交者则用一个包含用户名和邮件地址的字符串，如何将 Redmine 的用户和 Git 提交者相关联呢？Redmine 提供了一个配置界面用于设置二者之间的关系，如图 4-1 所示。

> ![../_images/redmine-user-config.png](img/redmine-user-config.png)
> 
> 图 4‑1：Redmine 中用户 ID 和 Git 提交者关联

显然如果在 Git 提交时随意变更提交者的姓名和邮件地址，会破坏 Redmine 软件中设置好的用户对应关系。

## 2.1.6\. 思考：命令别名是干什么的？

在本章的一开始，通过对`alias.ci`等 Git 环境变量的设置，为 Git 设置了命令别名。命令别名可以帮助用户解决从其他版本控制系统迁移到 Git 后的使用习惯问题。像 CVS 和 Subversion 在提交的时候，一般习惯使用`ci`（check in）子命令，在检出的时候则习惯使用`co`（check out）子命令。如果 Git 不能提供对`ci`和`co`这类简洁命令的支持，对于拥有其他版本控制系统使用经验的用户来说，Git 的用户体检就会打折扣。幸好聪明的 Git 提供了别名机制，可以满足用户特殊的使用习惯。

本章前面列出的四条别名设置指令，创建的是最常用的几个 Git 别名。实际上别名还可以包含命令参数。例如下面的别名设置指令：

```
$ git config --global alias.ci "commit -s"

```

如上设置后，当使用`git ci`命令提交的时候，会自动带上`-s`参数，这样会在提交的说明中自动添加上包含提交者姓名和邮件地址的签名标识，类似于`Signed-off-by: User Name <email@address>`。这对于一些项目（Git、Linux kernel、Android 等）来说是必要甚至是必须的。

不过在本书会尽量避免使用别名命令，以免由于读者因为尚未设置别名而造成学习上的困惑。

## 2.1.7\. 备份本章的工作成果

执行下面的命令，算是对本章工作成果的备份。

```
$ cd /path/to/my/workspace
$ git clone demo demo-step-1
Cloning into demo-step-1...
done.

```

* * *

| [[1]](#id2) | 命令**git commit -s**中的参数`-s`含义为在提交说明的最后添加“Signed-off-by:”签名。 |

| [[2]](#id3) | 命令**git -p status**中的参数`-p`含义是为**git status**命令的输出添加分页器。 |

| [[3]](#id4) | 大家实际操作中看到的 ID 肯定和这里写的不一样，具体原因会在后面的“6.1 Git 对象库探秘”一节中予以介绍。如果碰巧您的操作显示出了同样的 ID（78cde45），那么我建议您赶紧去买一张彩票。;) |

| [[4]](#id6) | 示例中使用了 Linux 下的**strace**命令跟踪系统调用，在 Mac OS X 下则可使用**sudo dtruss git status**命令跟踪相关 Git 操作的系统调用。 |

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 2.2\. Git 暂存区

上一章主要学习了三个命令：**git init**、**git add**和**git commit**，这三条命令可以说是版本库创建的三部曲。同时还通过对几个问题的思考，使读者能够了解 Git 版本库在工作区中的布局，Git 三个等级的配置文件以及 Git 的别名命令等内容。

在上一章的实践中，DEMO 版本库经历了两次提交，可以用**git log**查看提交日志（附加的`--stat`参数看到每次提交的文件变更统计）。

```
$ cd /path/to/my/workspace/demo
$ git log --stat
commit a0c641e92b10d8bcca1ed1bf84ca80340fdefee6
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Mon Nov 29 11:00:06 2010 +0800

    who does commit?

commit 9e8a761ff9dd343a1380032884f488a2422c495a
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Sun Nov 28 12:48:26 2010 +0800

    initialized.

 welcome.txt |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)

```

可以看到第一次提交对文件`welcome.txt`有一行的变更，而第二次提交因为是使用了`--allow-empty`参数进行的一次空提交，所以提交说明中看不到任何对实质内容的修改。

下面仍在这个工作区，继续新的实践。通过新的实践学习 Git 一个最重要的概念：“暂存区”。

## 2.2.1\. 修改不能直接提交？

首先更改`welcome.txt`文件，在这个文件后面追加一行。可以使用下面的命令实现内容的追加。

```
$ echo "Nice to meet you." >> welcome.txt

```

这时可以通过执行**git diff**命令看到修改后的文件和版本库中文件的差异。（实际上这句话有问题，和本地比较的不是版本库中的文件，而是一个中间状态的文件）

```
$ git diff
diff --git a/welcome.txt b/welcome.txt
index 18832d3..fd3c069 100644
--- a/welcome.txt
+++ b/welcome.txt
@@ -1 +1,2 @@
 Hello.
+Nice to meet you.

```

差异输出是不是很熟悉？在之前介绍版本库“黑暗的史前时代”的时候，曾经展示了**diff**命令的输出，两者的格式是一样的。

既然文件修改了，那么就提交吧。提交能够成功么？

```
$ git commit -m "Append a nice line."
# On branch master
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#       modified:   welcome.txt
#
no changes added to commit (use "git add" and/or "git commit -a")

```

提交成功了么？好像没有！

提交没有成功的证据：

*   先来看看提交日志，如果提交成功，应该有新的提交记录出现。

    下面使用了精简输出来显示日志，以便更简洁和清晰的看到提交历史。在其中能够看出来版本库中只有两个提交，都是在上一章的实践中完成的。也就是说刚才针对修改文件的提交没有成功！

    ```
    $ git log --pretty=oneline
    a0c641e92b10d8bcca1ed1bf84ca80340fdefee6 who does commit?
    9e8a761ff9dd343a1380032884f488a2422c495a initialized.

    ```

*   执行**git diff**可以看到和之前相同的差异输出，这也说明了之前的提交没有成功。

*   执行**git status**查看文件状态，也可以看到文件处于未提交的状态。

    而且**git status**命令的输出和**git commit**提交失败的输出信息一模一样！

*   对于习惯了像 CVS 和 Subversion 那样简练的状态输出的用户，可以在执行**git status** 时附加上`-s`参数，显示精简格式的状态输出。

    ```
    $ git status -s
     M welcome.txt

    ```

提交为什么会失败呢？再回过头来仔细看看刚才**git commit**命令提交失败后的输出：

> ```
> # On branch master
> # Changes not staged for commit:
> #   (use "git add <file>..." to update what will be committed)
> #   (use "git checkout -- <file>..." to discard changes in working directory)
> #
> #       modified:   welcome.txt
> #
> no changes added to commit (use "git add" and/or "git commit -a")
> 
> ```

把它翻译成中文普通话：

> ```
> # 位于您当前工作的分支 master 上
> # 下列的修改还没有加入到提交任务（提交暂存区，stage）中，不会被提交：
> #   (使用 "git add <file>..." 命令后，改动就会加入到提交任务中，
> #    要在下一次提交操作时才被提交)
> #   (使用 "git checkout -- <file>..." 命令，工作区当前您不打算提交的
> #    修改会被彻底清除！！！)
> #
> #       已修改:   welcome.txt
> #
> 警告：提交任务是空的噻，您不要再搔扰我啦
> (除非使用 "git add" 和/或 "git commit -a" 命令)
> 
> ```

也就是说要对修改的`welcome.txt`文件执行**git add**命令，将修改的文件添加到“提交任务”中，然后才能提交！

这个行为真的很奇怪，因为`add`操作对于其他版本控制系统来说是向版本库添加新文件用的，修改的文件（已被版本控制跟踪的文件）在下次提交时会直接被提交。Git 的这个古怪的行为会在下面的介绍中找到答案，读者会逐渐习惯并喜欢 Git 的这个设计。

好了，现在就将修改的文件“添加”到提交任务中吧：

```
$ git add welcome.txt

```

现在再执行一些 Git 命令，看看当执行文“添加”动作后，Git 库发生了什么变化：

*   执行**git diff**没有输出，难道是被提交了？可是只是执行了“添加” 到提交任务的操作，相当于一个“登记”的命令，并没有执行提交哇？

    ```
    $ git diff

    ```

*   这时如果和 HEAD（当前版本库的头指针）或者 master 分支（当前工作分支）进行比较，会发现有差异。这个差异才是正常的，因为尚未真正提交么。

    ```
    $ git diff HEAD
    diff --git a/welcome.txt b/welcome.txt
    index 18832d3..fd3c069 100644
    --- a/welcome.txt
    +++ b/welcome.txt
    @@ -1 +1,2 @@
     Hello.
    +Nice to meet you.

    ```

*   执行**git status**命令，状态输出和之前的不一样了。

    ```
    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       modified:   welcome.txt
    #

    ```

再对新的 Git 状态输出做一回翻译：

> ```
> $ git status
> # 位于分支 master 上
> # 下列的修改将被提交：
> #   (如果你后悔了，可以使用 "git reset HEAD <file>..." 命令
> #    将下列改动撤出提交任务（提交暂存区, stage），否则
> #    执行提交命令可真的要提交喽)
> #
> #       已修改:   welcome.txt
> #
> 
> ```

不得不说，Git 太人性化了，它把各种情况下可以使用到的命令都告诉给用户了，虽然这显得有点罗嗦。如果不要这么罗嗦，可以用简洁方式显示状态：

```
$ git status -s
M  welcome.txt

```

上面精简的状态输出与执行**git add**之前的精简状态输出相比，有细微的差别，发现了么？

*   虽然都是 M（Modified）标识，但是位置不一样。在执行**git add**命令之前，这个`M`位于第二列（第一列是一个空格），在执行完**git add**之后，字符`M`位于第一列（第二列是空白）。
*   位于第一列的字符`M`的含义是：版本库中的文件和处于中间状态——提交任务（提交暂存区，即 stage）中的文件相比有改动。
*   位于第二列的字符`M`的含义是：工作区当前的文件和处于中间状态——提交任务（提交暂存区，即 stage）中的文件相比也有改动。

是不是还有一些不明白？为什么 Git 的状态输出中提示了那么多让人不解的命令？为什么存在一个提交任务的概念而又总是把它叫做暂存区（stage）？不要紧，马上就会专题讲述“暂存区”的概念。当了解了 Git 版本库的设计原理之后，理解相关 Git 命令就易如反掌了。

这时如果直接提交（**git commit**），加入提交任务的`welcome.txt`文件的更改就被提交入库了。但是先不忙着执行提交，再进行一些操作，看看能否被彻底的搞糊涂。

*   继续修改一下`welcome.txt`文件（在文件后面再追加一行）。

    ```
    $ echo "Bye-Bye." >> welcome.txt

    ```

*   然后执行**git status**，查看一下状态：

    ```
    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       modified:   welcome.txt
    #
    # Changes not staged for commit:
    #   (use "git add <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #       modified:   welcome.txt
    #

    ```

    状态输出中居然是之前出现的两种不同状态输出的灵魂附体。

*   如果显示精简的状态输出，也会看到前面两种精简输出的杂合体。

    ```
    $ git status -s
    MM welcome.txt

    ```

上面的更为复杂的 Git 状态输出可以这么理解：不但版本库中最新提交的文件和处于中间状态 —— 提交任务（提交暂存区, stage）中的文件相比有改动，而且工作区当前的文件和处于中间状态 —— 提交任务（提交暂存区, stage）中的文件相比也有改动。

即现在`welcome.txt`有三个不同的版本，一个在工作区，一个在等待提交的暂存区，还有一个是版本库中最新版本的`welcome.txt`。通过不同的参数调用**git diff**命令可以看到不同版本库`welcome.txt`文件的差异。

*   不带任何选项和参数调用**git diff**显示工作区最新改动，即工作区和提交任务（提交暂存区，stage）中相比的差异。

    ```
    $ git diff
    diff --git a/welcome.txt b/welcome.txt
    index fd3c069..51dbfd2 100644
    --- a/welcome.txt
    +++ b/welcome.txt
    @@ -1,2 +1,3 @@
     Hello.
     Nice to meet you.
    +Bye-Bye.

    ```

*   将工作区和 HEAD（当前工作分支）相比，会看到更多的差异。

    ```
    $ git diff HEAD
    diff --git a/welcome.txt b/welcome.txt
    index 18832d3..51dbfd2 100644
    --- a/welcome.txt
    +++ b/welcome.txt
    @@ -1 +1,3 @@
     Hello.
    +Nice to meet you.
    +Bye-Bye.

    ```

*   通过参数`--cached`或者`--staged`参数调用**git diff**命令，看到的是提交暂存区（提交任务，stage）和版本库中文件的差异。

    ```
    $ git diff --cached
    diff --git a/welcome.txt b/welcome.txt
    index 18832d3..fd3c069 100644
    --- a/welcome.txt
    +++ b/welcome.txt
    @@ -1 +1,2 @@
     Hello.
    +Nice to meet you.

    ```

好了现在是时候**提交**了。现在执行**git commit**命令进行提交。

```
$ git commit -m "which version checked in?"
[master e695606] which version checked in?
 1 files changed, 1 insertions(+), 0 deletions(-)

```

这次提交终于成功了。如何证明提交成功了呢？

*   通过查看提交日志，看到了新的提交。

    ```
    $ git log --pretty=oneline
    e695606fc5e31b2ff9038a48a3d363f4c21a3d86 which version checked in?
    a0c641e92b10d8bcca1ed1bf84ca80340fdefee6 who does commit?
    9e8a761ff9dd343a1380032884f488a2422c495a initialized.

    ```

*   查看精简的状态输出。

    状态输出中文件名的前面出现了一个字母`M`，即只位于第二列的字母`M`。

    ```
    $ git status -s
     M welcome.txt

    ```

那么第一列的`M`哪里去了？被提交了呗。即提交任务（提交暂存区，stage）中的内容被提交到版本库中，所以第一列因为提交暂存区（提交任务，stage）和版本库中的状态一致，所以显示一个空白。

提交的`welcome.txt`是哪个版本呢？可以通过执行**git diff**或者**git diff HEAD**命令查看差异。虽然命令**git diff**和**git diff HEAD**的比较过程并不不同（可以通过**strace**命令跟踪命令执行过程中的文件访问），但是会看到下面相同的差异输出结果。

```
$ git diff
diff --git a/welcome.txt b/welcome.txt
index fd3c069..51dbfd2 100644
--- a/welcome.txt
+++ b/welcome.txt
@@ -1,2 +1,3 @@
 Hello.
 Nice to meet you.
+Bye-Bye.

```

## 2.2.2\. 理解 Git 暂存区（stage）

把上面的实践从头至尾走一遍，不知道读者的感想如何？

*   ——“被眼花缭乱的 Git 魔法彻底搞糊涂了？”
*   ——“Git 为什么这么折磨人，修改的文件直接提交不就完了么？”
*   ——“看不出 Git 这么做有什么好处？”

在上面的实践过程中，有意无意的透漏了“暂存区”的概念。为了避免用户被新概念吓坏，在暂存区出现的地方用同时使用了“提交任务”这一更易理解的概念，但是暂存区（stage，或称为 index）才是其真正的名称。我认为 Git 暂存区（stage，或称为 index）的设计是 Git 最成功的设计之一，也是最难理解的一个设计。

在版本库`.git`目录下，有一个`index`文件，下面针对这个文件做一个有趣的试验。要说明的是：这个试验是用 1.7.3 版本的 Git 进行的，低版本的 Git 因为没有针对**git status**命令进行优化设计，需要使用**git diff**命令，才能看到`index`文件的日期戳变化。

首先执行**git checkout**命令（后面会介绍此命令），撤销工作区中 welcome.txt 文件尚未提交的修改。

```
$ git checkout -- welcome.txt
$ git status -s     # 执行 git diff ，如果 git 版本号小于 1.7.3

```

通过状态输出，看以看到工作区已经没有改动了。查看一下**.git/index**文件，注意该文件的时间戳为：19:37:44。

```
$ ls --full-time .git/index
-rw-r--r-- 1 jiangxin jiangxin 112 2010-11-29 19:37:44.625246224 +0800.git/index

```

再次执行**git status**命令，然后显示`.git/index`文件的时间戳为：19:37:44，和上面的一样。

```
$ git status -s     # 执行 git diff ，如果 git 版本号小于 1.7.3
$ ls --full-time .git/index
-rw-r--r-- 1 jiangxin jiangxin 112 2010-11-29 19:37:44.625246224 +0800 .git/index

```

现在更改一下 welcome.txt 的时间戳，但是不改变它的内容。然后再执行**git status**命令，然后查看`.git/index`文件时间戳为：19:42:06。

```
$ touch welcome.txt
$ git status -s     # 执行 git diff ，如果 git 版本号小于 1.7.3
$ ls --full-time .git/index
-rw-r--r-- 1 jiangxin jiangxin 112 2010-11-29 19:42:06.980243216 +0800 .git/index

```

看到了么，时间戳改变了！

这个试验说明当执行**git status**命令（或者**git diff**命令）扫描工作区改动的时候，先依据`.git/index`文件中记录的（工作区跟踪文件的）时间戳、长度等信息判断工作区文件是否改变。如果工作区的文件时间戳改变，说明文件的内容**可能**被改变了，需要要打开文件，读取文件内容，和更改前的原始文件相比较，判断文件内容是否被更改。如果文件内容没有改变，则将该文件新的时间戳记录到`.git/index`文件中。因为判断文件是否更改，使用时间戳、文件长度等信息进行比较要比通过文件内容比较要快的多，所以 Git 这样的实现方式可以让工作区状态扫描更快速的执行，这也是 Git 高效的因素之一。

文件`.git/index`实际上就是一个包含文件索引的目录树，像是一个虚拟的工作区。在这个虚拟工作区的目录树中，记录了文件名、文件的状态信息（时间戳、文件长度等）。文件的内容并不存储其中，而是保存在 Git 对象库`.git/objects`目录中，文件索引建立了文件和对象库中对象实体之间的对应。下面这个图展示了工作区、版本库中的暂存区和版本库之间的关系。

> ![../_images/git-stage.png](img/git-stage.png)
> 
> 工作区、版本库、暂存区原理图

在这个图中，可以看到部分 Git 命令是如何影响工作区和暂存区（stage，亦称 index）的。下面就对这些命令进行简要的说明，而要彻底揭开这些命令的面纱要在接下来的几个章节。

*   图中左侧为工作区，右侧为版本库。在版本库中标记为`index`的区域是暂存区（stage，亦称 index），标记为`master`的是 master 分支所代表的目录树。
*   图中可以看出此时 HEAD 实际是指向 master 分支的一个“游标”。所以图示的命令中出现 HEAD 的地方可以用 master 来替换。
*   图中的 objects 标识的区域为 Git 的对象库，实际位于`.git/objects`目录下，会在后面的章节重点介绍。
*   当对工作区修改（或新增）的文件执行**git add**命令时，暂存区的目录树被更新，同时工作区修改（或新增）的文件内容被写入到对象库中的一个新的对象中，而该对象的 ID 被记录在暂存区的文件索引中。
*   当执行提交操作（**git commit**）时，暂存区的目录树写到版本库（对象库）中，master 分支会做相应的更新。即 master 最新指向的目录树就是提交时原暂存区的目录树。
*   当执行**git reset HEAD**命令时，暂存区的目录树会被重写，被 master 分支指向的目录树所替换，但是工作区不受影响。
*   当执行**git rm --cached <file>**命令时，会直接从暂存区删除文件，工作区则不做出改变。
*   当执行**git checkout .**或者**git checkout -- <file>**命令时，会用暂存区全部或指定的文件替换工作区的文件。这个操作很危险，会清除工作区中未添加到暂存区的改动。
*   当执行**git checkout HEAD .**或者**git checkout HEAD <file>**命令时，会用 HEAD 指向的 master 分支中的全部或者部分文件替换暂存区和以及工作区中的文件。这个命令也是极具危险性的，因为不但会清除工作区中未提交的改动，也会清除暂存区中未提交的改动。

## 2.2.3\. Git Diff 魔法

在本章的实践中展示了具有魔法效果的命令：**git diff**。在不同参数的作用下，**git diff**的输出并不相同。在理解了 Git 中的工作区、暂存区、和版本库最新版本（当前分支）分别是三个不同的目录树后，就非常好理解**git diff**魔法般的行为了。

**暂存区目录树的浏览**

有什么办法能够像查看工作区一样的，直观的查看暂存区以及 HEAD 当中的目录树么？

对于 HEAD（版本库中当前提交）指向的目录树，可以使用 Git 底层命令**git ls-tree**来查看。

```
$ git ls-tree -l HEAD
100644 blob fd3c069c1de4f4bc9b15940f490aeb48852f3c42      25    welcome.txt

```

其中:

*   使用`-l`参数，可以显示文件的大小。上面`welcome.txt`大小为 25 字节。
*   输出的`welcome.txt`文件条目从左至右，第一个字段是文件的属性(rw-r–r–)，第二个字段说明是 Git 对象库中的一个 blob 对象（文件），第三个字段则是该文件在对象库中对应的 ID——一个 40 位的 SHA1 哈希值格式的 ID（这个会在后面介绍），第四个字段是文件大小，第五个字段是文件名。

在浏览暂存区中的目录树之前，首先清除工作区当中的改动。通过**git clean -fd**命令清除当前工作区中没有加入版本库的文件和目录（非跟踪文件和目录），然后执行**git checkout .**命令，用暂存区内容刷新工作区。

```
$ cd /path/to/my/workspace/demo
$ git clean -fd
$ git checkout .

```

然后开始在工作区中做出一些修改（修改`welcome.txt`，增加一个子目录和文件），然后添加到暂存区。最后再对工作区做出修改。

```
$ echo "Bye-Bye." >> welcome.txt
$ mkdir -p a/b/c
$ echo "Hello." > a/b/c/hello.txt
$ git add .
$ echo "Bye-Bye." >> a/b/c/hello.txt
$ git status -s
AM a/b/c/hello.txt
M  welcome.txt

```

上面的命令运行完毕后，通过精简的状态输出，可以看出工作区、暂存区、和版本库当前分支的最新版本（HEAD）各不相同。先来看看工作区中文件的大小：

```
$ find . -path ./.git -prune -o -type f -printf "%-20p\t%s\n"
./welcome.txt           34
./a/b/c/hello.txt       16

```

要显示暂存区的目录树，可以使用**git ls-files**命令。

```
$ git ls-files -s
100644 18832d35117ef2f013c4009f5b2128dfaeff354f 0       a/b/c/hello.txt
100644 51dbfd25a804c30e9d8dc441740452534de8264b 0       welcome.txt

```

注意这个输出和之前使用**git ls-tree**命令输出不一样，如果想要使用**git ls-tree**命令，需要先将暂存区的目录树写入 Git 对象库（用**git write-tree**命令），然后在针对**git write-tree**命令写入的 tree 执行**git ls-tree**命令。

```
$ git write-tree
9431f4a3f3e1504e03659406faa9529f83cd56f8
$ git ls-tree -l 9431f4a
040000 tree 53583ee687fbb2e913d18d508aefd512465b2092       -    a
100644 blob 51dbfd25a804c30e9d8dc441740452534de8264b      34    welcome.txt

```

从上面的命令可以看出：

*   到处都是 40 位的 SHA1 哈希值格式的 ID，可以用于指代文件内容（blob），用于指代目录树（tree），还可以用于指代提交。但什么是 SHA1 哈希值 ID，作用是什么，这些疑问暂时搁置，下一章再揭晓。
*   命令**git write-tree**的输出就是写入 Git 对象库中的 Tree ID，这个 ID 将作为下一条命令的输入。
*   在**git ls-tree**命令中，没有把 40 位的 ID 写全，而是使用了前几位，实际上只要不和其他的对象 ID 冲突，可以随心所欲的使用缩写 ID。
*   可以看到**git ls-tree**的输出显示的第一条是一个 tree 对象，即刚才创建的一级目录`a`。

如果想要递归显示目录内容，则使用`-r`参数调用。使用参数`-t`可以把递归过程遇到的每棵树都显示出来，而不只是显示最终的文件。下面执行递归操作显示目录树的内容。

```
$ git write-tree | xargs git ls-tree -l -r -t
040000 tree 53583ee687fbb2e913d18d508aefd512465b2092       -    a
040000 tree 514d729095b7bc203cf336723af710d41b84867b       -    a/b
040000 tree deaec688e84302d4a0b98a1b78a434be1b22ca02       -    a/b/c
100644 blob 18832d35117ef2f013c4009f5b2128dfaeff354f       7    a/b/c/hello.txt
100644 blob 51dbfd25a804c30e9d8dc441740452534de8264b      34    welcome.txt

```

好了现在工作区，暂存区和 HEAD 三个目录树的内容各不相同。下面的表格总结了不同文件在三个目录树中的文件大小。

> | 文件名 | 工作区 | 暂存区 | HEAD |
> | --- | --- | --- | --- |
> | welcome.txt | 34 字节 | 34 字节 | 25 字节 |
> | a/b/c/hello.txt | 16 字节 | 7 字节 | 0 字节 |

**Git diff 魔法**

通过使用不同的参数调用**git diff**命令，可以对工作区、暂存区、HEAD 中的内容两两比较。下面的这个图，展示了不同的**git diff**命令的作用范围。

> ![../_images/git-diff.png](img/git-diff.png)

通过上面的图，就不难理解下面**git diff**命令不同的输出结果了。

*   工作区和暂存区比较。

    ```
    $ git diff
    diff --git a/a/b/c/hello.txt b/a/b/c/hello.txt
    index 18832d3..e8577ea 100644
    --- a/a/b/c/hello.txt
    +++ b/a/b/c/hello.txt
    @@ -1 +1,2 @@
     Hello.
    +Bye-Bye.

    ```

*   暂存区和 HEAD 比较。

    ```
    $ git diff --cached
    diff --git a/a/b/c/hello.txt b/a/b/c/hello.txt
    new file mode 100644
    index 0000000..18832d3
    --- /dev/null
    +++ b/a/b/c/hello.txt
    @@ -0,0 +1 @@
    +Hello.
    diff --git a/welcome.txt b/welcome.txt
    index fd3c069..51dbfd2 100644
    --- a/welcome.txt
    +++ b/welcome.txt
    @@ -1,2 +1,3 @@
     Hello.
     Nice to meet you.
    +Bye-Bye.

    ```

*   工作区和 HEAD 比较。

    ```
    $ git diff HEAD
    diff --git a/a/b/c/hello.txt b/a/b/c/hello.txt
    new file mode 100644
    index 0000000..e8577ea
    --- /dev/null
    +++ b/a/b/c/hello.txt
    @@ -0,0 +1,2 @@
    +Hello.
    +Bye-Bye.
    diff --git a/welcome.txt b/welcome.txt
    index fd3c069..51dbfd2 100644
    --- a/welcome.txt
    +++ b/welcome.txt
    @@ -1,2 +1,3 @@
     Hello.
     Nice to meet you.
    +Bye-Bye.

    ```

## 2.2.4\. 不要使用**git commit -a**

实际上 Git 的提交命令（**git commit**）可以带上`-a`参数，对本地所有变更的文件执行提交操作，包括本地修改的文件，删除的文件，但不包括未被版本库跟踪的文件。

这个命令的确可以简化一些操作，减少用**git add**命令标识变更文件的步骤，但是如果习惯了使用这个“偷懒”的提交命令，就会丢掉 Git 暂存区带给用户最大的好处：对提交内容进行控制的能力。

有的用户甚至通过别名设置功能，创建指向命令**git commit -a**的别名`ci`，这更是不可取的行为，应严格禁止。在本书会很少看到使用**git commit -a**命令。

## 2.2.5\. 搁置问题，暂存状态

查看一下当前工作区的状态。

```
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       new file:   a/b/c/hello.txt
#       modified:   welcome.txt
#
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#       modified:   a/b/c/hello.txt
#

```

在状态输出中 Git 体贴的告诉了用户如何将加入暂存区的文件从暂存区撤出以便让暂存区和 HEAD 一致（这样提交就不会发生），还告诉用户对于暂存区更新后在工作区所做的再一次的修改有两个选择：或者再次添加到暂存区，或者取消工作区新做出的改动。但是涉及到的命令现在理解还有些难度，一个是**git reset**，一个是**git checkout**。需要先解决什么是 HEAD，什么是 master 分支以及 Git 对象存储的实现机制等问题，才可以更好的操作暂存区。

为此，我作出一个非常艰难的决定[[1]](#id4)：就是保存当前的工作进度，在研究了 HEAD 和 master 分支的机制之后，继续对暂存区的探索。命令**git stash**就是用于保存当前工作进度的。

```
$ git stash
Saved working directory and index state WIP on master: e695606 which version checked in?
HEAD is now at e695606 which version checked in?

```

运行完**git stash**之后，再查看工作区状态，会看见工作区尚未提交的改动（包括暂存区的改动）全都不见了。

```
$ git status
# On branch master
nothing to commit (working directory clean)

```

“I’ll be back” —— 施瓦辛格, 《终结者》, 1984.

| [[1]](#id3) | 此句式模仿 2010 年 11 月份发生的“3Q 大战”。参见：[`zh.wikipedia.org/wiki`](http://zh.wikipedia.org/wiki)/奇虎 360 与腾讯 QQ 争斗事件。 |

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 2.3\. Git 对象

在上一章学习了 Git 的一个最重要的概念：暂存区（stage，亦称 index）。暂存区是一个介于工作区和版本库的中间状态，当执行提交时实际上是将暂存区的内容提交到版本库中，而且 Git 很多命令都会涉及到暂存区的概念，例如：**git diff**命令。

但是上一章还是留下了很多疑惑，例如什么是 HEAD？什么是 master？为什么它们二者（在上一章）可以相互替换使用？为什么 Git 中的很多对象像提交、树、文件内容等都用 40 位的 SHA1 哈希值来表示？这一章的内容将会揭开这些奥秘，并且还会画出一个更为精确的版本库结构图。

## 2.3.1\. Git 对象库探秘

在前面刻意回避了对提交 ID 的说明，现在是时候来揭开由 40 位十六进制数字组成的“魔幻数字”的奥秘了。

通过查看日志的详尽输出，会惊讶的看到非常多的“魔幻数字”，这些“魔幻数字”实际上是 SHA1 哈希值。

```
$ git log -1 --pretty=raw
commit e695606fc5e31b2ff9038a48a3d363f4c21a3d86
tree f58da9a820e3fd9d84ab2ca2f1b467ac265038f9
parent a0c641e92b10d8bcca1ed1bf84ca80340fdefee6
author Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800
committer Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800

    which version checked in?

```

一个提交中居然包含了三个 SHA1 哈希值表示的对象 ID。

*   `commit e695606fc5e31b2ff9038a48a3d363f4c21a3d86`

    这是本次提交的唯一标识。

*   `tree f58da9a820e3fd9d84ab2ca2f1b467ac265038f9`

    这是本次提交所对应的目录树。

*   `parent a0c641e92b10d8bcca1ed1bf84ca80340fdefee6`

    这是本地提交的父提交（上一次提交）。

研究 Git 对象 ID 的一个重量级武器就是**git cat-file**命令。用下面的命令可以查看一下这三个 ID 的类型。

```
$ git cat-file -t e695606
commit
$ git cat-file -t f58d
tree
$ git cat-file -t a0c6
commit

```

在引用对象 ID 的时候，没有必要把整个 40 位 ID 写全，只需要从头开始的几位不冲突即可。

下面再用**git cat-file**命令查看一下这几个对象的内容。

*   commit 对象`e695606fc5e31b2ff9038a48a3d363f4c21a3d86`

    ```
    $ git cat-file -p e695606
    tree f58da9a820e3fd9d84ab2ca2f1b467ac265038f9
    parent a0c641e92b10d8bcca1ed1bf84ca80340fdefee6
    author Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800
    committer Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800

    which version checked in?

    ```

*   tree 对象`f58da9a820e3fd9d84ab2ca2f1b467ac265038f9`

    ```
    $ git cat-file -p f58da9a
    100644 blob fd3c069c1de4f4bc9b15940f490aeb48852f3c42    welcome.txt

    ```

*   commit 对象`a0c641e92b10d8bcca1ed1bf84ca80340fdefee6`

    ```
    $ git cat-file -p a0c641e
    tree 190d840dd3d8fa319bdec6b8112b0957be7ee769
    parent 9e8a761ff9dd343a1380032884f488a2422c495a
    author Jiang Xin <jiangxin@ossxp.com> 1290999606 +0800
    committer Jiang Xin <jiangxin@ossxp.com> 1290999606 +0800

    who does commit?

    ```

在上面目录树（tree）对象中看到了一个新的类型的对象：blob 对象。这个对象保存着文件`welcome.txt`的内容。用**git cat-file**研究一下。

*   该对象的类型为 blob。

    ```
    $ git cat-file -t fd3c069c1de4f4bc9b15940f490aeb48852f3c42
    blob

    ```

*   该对象的内容就是`welcome.txt`文件的内容。

    ```
    $ git cat-file -p fd3c069c1de4f4bc9b15940f490aeb48852f3c42
    Hello.
    Nice to meet you.

    ```

这些对象保存在哪里？当然是 Git 库中的`objects`目录下了（ID 的前两位作为目录名，后 38 位作为文件名）。用下面的命令可以看到这些对象在对象库中的实际位置。

```
$ for id in e695606 f58da9a a0c641e fd3c069; do \
  ls .git/objects/${id:0:2}/${id:2}*; done
.git/objects/e6/95606fc5e31b2ff9038a48a3d363f4c21a3d86
.git/objects/f5/8da9a820e3fd9d84ab2ca2f1b467ac265038f9
.git/objects/a0/c641e92b10d8bcca1ed1bf84ca80340fdefee6
.git/objects/fd/3c069c1de4f4bc9b15940f490aeb48852f3c42

```

下面的图示更加清楚的显示了 Git 对象库中各个对象之间的关系。

> ![../_images/git-objects.png](img/git-objects.png)

从上面的图示中很明显的看出提交（Commit）对象之间相互关联，通过相互之间的关联则很容易的识别出一条跟踪链。这条跟踪链可以在运行**git log**命令时，通过使用`--graph`参数看到。下面的命令还使用了`--pretty=raw`参数以便显示每个提交对象的 parent 属性。

```
$ git log --pretty=raw --graph e695606
* commit e695606fc5e31b2ff9038a48a3d363f4c21a3d86
| tree f58da9a820e3fd9d84ab2ca2f1b467ac265038f9
| parent a0c641e92b10d8bcca1ed1bf84ca80340fdefee6
| author Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800
| committer Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800
|
|     which version checked in?
|
* commit a0c641e92b10d8bcca1ed1bf84ca80340fdefee6
| tree 190d840dd3d8fa319bdec6b8112b0957be7ee769
| parent 9e8a761ff9dd343a1380032884f488a2422c495a
| author Jiang Xin <jiangxin@ossxp.com> 1290999606 +0800
| committer Jiang Xin <jiangxin@ossxp.com> 1290999606 +0800
|
|     who does commit?
|
* commit 9e8a761ff9dd343a1380032884f488a2422c495a
  tree 190d840dd3d8fa319bdec6b8112b0957be7ee769
  author Jiang Xin <jiangxin@ossxp.com> 1290919706 +0800
  committer Jiang Xin <jiangxin@ossxp.com> 1290919706 +0800

      initialized.

```

最后一个提交没有 parent 属性，所以跟踪链到此终结，这实际上就是提交的起点。

**现在来看看 HEAD 和 master 的奥秘吧**

因为在上一章的最后执行了**git stash**将工作区和暂存区的改动全部封存起来，所以执行下面的命令会看到工作区和暂存区中没有改动。

```
$ git status -s -b
## master

```

说明：上面在显示工作区状态时，除了使用了`-s`参数以显示精简输出外，还使用了`-b`参数以便能够同时显示出当前工作分支的名称。这个`-b`参数是在 Git 1.7.2 以后加入的新的参数。

下面的**git branch**是分支管理的主要命令，也可以显示当前的工作分支。

```
$ git branch
* master

```

在 master 分支名称前面出现一个星号表明这个分支是当前工作分支。至于为什么没有其他分支以及什么叫做分支，会在本书后面的章节揭晓。

现在连续执行下面的三个命令会看到相同的输出：

```
$ git log -1 HEAD
commit e695606fc5e31b2ff9038a48a3d363f4c21a3d86
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Mon Nov 29 17:23:01 2010 +0800

    which version checked in?
$ git log -1 master
commit e695606fc5e31b2ff9038a48a3d363f4c21a3d86
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Mon Nov 29 17:23:01 2010 +0800

    which version checked in?
$ git log -1 refs/heads/master
commit e695606fc5e31b2ff9038a48a3d363f4c21a3d86
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Mon Nov 29 17:23:01 2010 +0800

    which version checked in?

```

也就是说在当前版本库中，HEAD、master 和 refs/heads/master 具有相同的指向。现在到版本库（`.git`目录）中一探它们的究竟。

```
$ find .git -name HEAD -o -name master
.git/HEAD
.git/logs/HEAD
.git/logs/refs/heads/master
.git/refs/heads/master

```

找到了四个文件，其中在`.git/logs`目录下的文件稍后再予以关注，现在把目光锁定在`.git/HEAD`和`.git/refs/heads/master`上。

显示一下`.git/HEAD`的内容：

```
$ cat .git/HEAD
ref: refs/heads/master

```

把 HEAD 的内容翻译过来就是：“指向一个引用：refs/heads/master”。这个引用在哪里？当然是文件`.git/refs/heads/master`了。

看看文件`.git/refs/heads/master`的内容。

```
$ cat .git/refs/heads/master
e695606fc5e31b2ff9038a48a3d363f4c21a3d86

```

显示的`e695606...`所指为何物？用**git cat-file**命令进行查看。

*   显示 SHA1 哈希值指代的数据类型。

    ```
    $ git cat-file -t e695606
    commit

    ```

*   显示该提交的内容。

    ```
    $ git cat-file -p e695606fc5e31b2ff9038a48a3d363f4c21a3d86
    tree f58da9a820e3fd9d84ab2ca2f1b467ac265038f9
    parent a0c641e92b10d8bcca1ed1bf84ca80340fdefee6
    author Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800
    committer Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800

    which version checked in?

    ```

原来分支 master 指向的是一个提交 ID（最新提交）。这样的分支实现是多么的巧妙啊：既然可以从任何提交开始建立一条历史跟踪链，那么用一个文件指向这个链条的最新提交，那么这个文件就可以用于追踪整个提交历史了。这个文件就是`.git/refs/heads/master`文件。

下面看一个更接近于真实的版本库结构图：

> ![../_images/git-repos-detail.png](img/git-repos-detail.png)

目录`.git/refs`是保存引用的命名空间，其中`.git/refs/heads`目录下的引用又称为分支。对于分支既可以使用正规的长格式的表示法，如`refs/heads/master`，也可以去掉前面的两级目录用`master`来表示。Git 有一个底层命令**git rev-parse**可以用于显示引用对应的提交 ID。

```
$ git rev-parse master
e695606fc5e31b2ff9038a48a3d363f4c21a3d86
$ git rev-parse refs/heads/master
e695606fc5e31b2ff9038a48a3d363f4c21a3d86
$ git rev-parse HEAD
e695606fc5e31b2ff9038a48a3d363f4c21a3d86

```

可以看出它们都指向同一个对象。为什么这个对象是 40 位，而不是更少或者更多？这些 ID 是如何生成的呢？

## 2.3.2\. 问题：SHA1 哈希值到底是什么，如何生成的？

哈希(hash)是一种数据摘要算法（或称散列算法），是信息安全领域当中重要的理论基石。该算法将任意长度的输入经过散列运算转换为固定长度的输出。固定长度的输出可以称为对应的输入的数字摘要或哈希值。例如 SHA1 摘要算法可以处理从零到一千多万个 TB 的输入数据，输出为固定的 160 比特的数字摘要。两个不同内容的输入即使数据量非常大、差异非常小，两者的哈希值也会显著不同。比较著名的摘要算法有：MD5 和 SHA1。Linux 下**sha1sum**命令可以用于生成摘要。

```
$ echo -n Git |sha1sum
5819778898df55e3a762f0c5728b457970d72cae  -

```

可以看出字符串`Git`的 SHA1 哈希值为 40 个十六进制的数字组成。那么能不能找出另外一个字符串使其 SHA1 哈希值和上面的哈希值一样呢？下面看看难度有多大。

每个十六进制的数字用于表示一个 4 位的二进制数字，因此 40 位的 SHA1 哈希值的输出为实为 160bit。拿双色球博彩打一个比喻，要想制造相同的 SHA1 哈希值就相当于要选出 32 个“红色球”，每个红球有 1 到 32 个（5 位的二进制数字）选择，而且红球之间可以重复。相比“双色球博彩”总共只需选出 7 颗球，SHA1“中奖”的难度就相当于要连续购买五期“双色球”并且必须每一期都要中一等奖。当然由于算法上的问题，制造冲突（相同数字摘要）的几率没有那么小，但是已经足够小，能够满足 Git 对不同对象的进行区分和标识了。即使有一天像发现了类似 MD5 摘要算法漏洞那样，发现了 SHA1 算法存在人为制造冲突的可能，那么 Git 可以使用更为安全的 SHA-256 或者 SHA-512 的摘要算法。

可是 Git 中的各种对象：提交（commit）、文件内容（blob）、目录树（tree）等（还有 Tag）对象对应的 SHA1 哈希值是如何生成的呢？下面就来展示一下。

提交的 SHA1 哈希值生成方法。

*   看看 HEAD 对应的提交的内容。使用**git cat-file**命令。

    ```
    $ git cat-file commit HEAD
    tree f58da9a820e3fd9d84ab2ca2f1b467ac265038f9
    parent a0c641e92b10d8bcca1ed1bf84ca80340fdefee6
    author Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800
    committer Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800

    which version checked in?

    ```

*   提交信息中总共包含 234 个字符。

    ```
    $ git cat-file commit HEAD | wc -c
    234

    ```

*   在提交信息的前面加上内容`commit 234<null>`（<null>为空字符），然后执行 SHA1 哈希算法。

    ```
    $ ( printf "commit 234\000"; git cat-file commit HEAD ) | sha1sum
    e695606fc5e31b2ff9038a48a3d363f4c21a3d86  -

    ```

*   上面命令得到的哈希值和用**git rev-parse**看到的是一样的。

    ```
    $ git rev-parse HEAD
    e695606fc5e31b2ff9038a48a3d363f4c21a3d86

    ```

下面看一看文件内容的 SHA1 哈希值生成方法。

*   看看版本库中`welcome.txt`的内容。使用**git cat-file**命令。

    ```
    $ git cat-file blob HEAD:welcome.txt
    Hello.
    Nice to meet you.

    ```

*   文件总共包含 25 字节的内容。

    ```
    $ git cat-file blob HEAD:welcome.txt | wc -c
    25

    ```

*   在文件内容的前面加上`blob 25<null>`的内容，然后执行 SHA1 哈希算法。

    ```
    $ ( printf "blob 25\000"; git cat-file blob HEAD:welcome.txt ) | sha1sum
    fd3c069c1de4f4bc9b15940f490aeb48852f3c42  -

    ```

*   上面命令得到的哈希值和用**git rev-parse**看到的是一样的。

    ```
    $ git rev-parse HEAD:welcome.txt
    fd3c069c1de4f4bc9b15940f490aeb48852f3c42

    ```

最后再来看看树的 SHA1 哈希值的形成方法。

*   HEAD 对应的树的内容共包含 39 个字节。

    ```
    $ git cat-file tree HEAD^{tree} | wc -c
    39

    ```

*   在树的内容的前面加上`tree 39<null>`的内容，然后执行 SHA1 哈希算法。

    ```
    $ ( printf "tree 39\000"; git cat-file tree HEAD^{tree} ) | sha1sum
    f58da9a820e3fd9d84ab2ca2f1b467ac265038f9  -

    ```

*   上面命令得到的哈希值和用**git rev-parse**看到的是一样的。

    ```
    $ git rev-parse HEAD^{tree}
    f58da9a820e3fd9d84ab2ca2f1b467ac265038f9

    ```

在后面学习里程碑（Tag）的时候，会看到 Tag 对象（轻量级 Tag 除外）也是采用类似方法在对象库中存储的。

## 2.3.3\. 问题：为什么不用顺序的数字来表示提交？

到目前为止所进行的提交都是顺序提交，这可能让读者产生这么一个想法，为什么 Git 的提交不依据提交顺序对提交进行编号呢？可以把第一次提交定义为提交 1，依次递增。尤其是对于拥有像 Subversion 等集中式版本控制系统使用经验的用户更会有这样的体会和想法。

集中式版本控制系统因为只有一个集中式的版本库，可以很容易的实现依次递增的全局唯一的提交号，像 Subversion 就是如此。Git 作为分布式版本控制系统，开发可以是非线性的，每个人可以通过克隆版本库的方式工作在不同的本地版本库当中，在本地做的提交可以通过版本库之间的交互（推送/push 和拉回/pull 操作）而互相分发，如果提交采用本地唯一的数字编号，在提交分发的时候不可避免的造成冲突。这就要求提交的编号不能仅仅是本地局部有效，而是要“全球唯一”。Git 的提交通过 SHA1 哈希值作为提交 ID，的确做到了“全球唯一”。

Mercurial(Hg)是另外一个著名的分布式版本控制系统，它的提交 ID 非常有趣：同时使用了顺序的数字编号和“全球唯一”的 SHA1 哈希值。但实际上顺序的数字编号只是本地有效，对于克隆版本库来说没有意义，只有 SHA1 哈希值才是通用的编号。

```
$ hg log --limit 2
修改集:      3009:2f1a3a7e8eb0
标签:        tip
用户:        Daniel Neuhäuser <dasdasich@gmail.com>
日期:        Wed Dec 01 23:13:31 2010 +0100
摘要:        "Fixed" the CombinedHTMLDiff test

修改集:      3008:2fd3302ca7e5
用户:        Daniel Neuhäuser <dasdasich@gmail.com>
日期:        Wed Dec 01 22:54:54 2010 +0100
摘要:        #559 Add `html_permalink_text` confval

```

Hg 的设计使得本地使用版本库更为方便，但是要在 Git 中做类似实现却很难，这是因为 Git 相比 Hg 拥有真正的分支管理功能。在 Git 中会存在当前分支中看不到的其他分支的提交，如何进行提交编号的管理十分的复杂。

幸好 Git 提供很多方法可以方便的访问 Git 库中的对象。

*   采用部分的 SHA1 哈希值。不必写全 40 位的哈希值，只采用开头的部分，不和现有其他的冲突即可。

*   使用`master`代表分支`master`中最新的提交，使用全称`refs/heads/master`亦可。

*   使用`HEAD`代表版本库中最近的一次提交。

*   符号` `^`可以用于指代父提交。例如：

    *   `HEAD^`代表版本库中上一次提交，即最近一次提交的父提交。
    *   `HEAD^^`则代表`HEAD^`的父提交。
*   对于一个提交有多个父提交，可以在符号`^`后面用数字表示是第几个父提交。例如：

    *   `a573106²`含义是提交`a573106`的多个父提交中的第二个父提交。
    *   `HEAD¹`相当于`HEAD^`含义是 HEAD 多个父提交中的第一个。
    *   `HEAD^²`含义是`HEAD^`（HEAD 父提交）的多个父提交中的第二个。
*   符号`~<n>`也可以用于指代祖先提交。下面两个表达式效果等同：

    ```
    a573106~5
    a573106^^^^^

    ```

*   提交所对应的树对象，可以用类似如下的语法访问。

    ```
    a573106^{tree}

    ```

*   某一此提交对应的文件对象，可以用如下的语法访问。

    ```
    a573106:path/to/file

    ```

*   暂存区中的文件对象，可以用如下的语法访问。

    ```
    :path/to/file

    ```

读者可以使用**git rev-parse**命令在本地版本库中练习一下：

```
$ git rev-parse HEAD
e695606fc5e31b2ff9038a48a3d363f4c21a3d86
$ git cat-file -p e695
tree f58da9a820e3fd9d84ab2ca2f1b467ac265038f9
parent a0c641e92b10d8bcca1ed1bf84ca80340fdefee6
author Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800
committer Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800

which version checked in?
$ git cat-file -p e695^
tree 190d840dd3d8fa319bdec6b8112b0957be7ee769
parent 9e8a761ff9dd343a1380032884f488a2422c495a
author Jiang Xin <jiangxin@ossxp.com> 1290999606 +0800
committer Jiang Xin <jiangxin@ossxp.com> 1290999606 +0800

who does commit?
$ git rev-parse e695^{tree}
f58da9a820e3fd9d84ab2ca2f1b467ac265038f9
$ git rev-parse e695^^{tree}
190d840dd3d8fa319bdec6b8112b0957be7ee769

```

在后面的介绍中，还会了解更多访问 Git 对象的技巧。例如使用 tag 和日期访问版本库对象。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 2.4\. Git 重置

在上一章了解了版本库中对象的存储方式以及分支 master 的实现。即 master 分支在版本库的引用目录（.git/refs）中体现为一个引用文件`.git/refs/heads/master`，其内容就是分支中最新提交的提交 ID。

```
$ cat .git/refs/heads/master
e695606fc5e31b2ff9038a48a3d363f4c21a3d86

```

上一章还通过对提交本身数据结构的分析，看到提交可以通过到父提交的关联实现对提交历史的追溯。注意：下面的**git log**命令中使用了`--oneline`参数，类似于`--pretty=oneline`，但是可以显示更短小的提交 ID。参数`--oneline`在 Git 1.6.3 及以后版本提供，老版本的 Git 可以使用参数`--pretty=oneline --abbrev-commit`替代。

```
$ git log --graph --oneline
* e695606 which version checked in?
* a0c641e who does commit?
* 9e8a761 initialized.

```

那么是不是有新的提交发生的时候，代表 master 分支的引用文件的内容会改变呢？代表 master 分支的引用文件的内容可以人为的改变么？本章就来探讨用**git reset**命令改变分支引用文件内容，即实现分支的重置。

## 2.4.1\. 分支游标 master 的探秘

先来看看当有新的提交发生的时候，文件`.git/refs/heads/master`的内容如何改变。首先在工作区创建一个新文件，姑且叫做`new-commit.txt`，然后提交到版本库中。

```
$ touch new-commit.txt
$ git add new-commit.txt
$ git commit -m "does master follow this new commit?"
[master 4902dc3] does master follow this new commit?
 0 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 new-commit.txt

```

此时工作目录下会有两个文件，其中文件`new-commit.txt`是新增的。

```
$ ls
new-commit.txt  welcome.txt

```

来看看 master 分支指向的提交 ID 是否改变了。

*   先看看在版本库引用空间（.git/refs/目录）下的`master`文件内容的确更改了，指向了新的提交。

    ```
    $ cat .git/refs/heads/master
    4902dc375672fbf52a226e0354100b75d4fe31e3

    ```

*   再用**git log**查看一下提交日志，可以看到刚刚完成的提交。

    ```
    $ git log --graph --oneline
    * 4902dc3 does master follow this new commit?
    * e695606 which version checked in?
    * a0c641e who does commit?
    * 9e8a761 initialized.

    ```

引用`refs/heads/master`就好像是一个游标，在有新的提交发生的时候指向了新的提交。可是如果只可上、不可下，就不能称为“游标”。Git 提供了**git reset**命令，可以将“游标”指向任意一个存在的提交 ID。下面的示例就尝试人为的更改游标。（注意下面的命令中使用了`--hard`参数，会破坏工作区未提交的改动，慎用。）

```
$ git reset --hard HEAD^
HEAD is now at e695606 which version checked in?

```

还记得上一章介绍的`HEAD^`代表了`HEAD`的父提交么？所以这条命令就相当于将 master 重置到上一个老的提交上。来看一下 master 文件的内容是否更改了。

```
$ cat .git/refs/heads/master
e695606fc5e31b2ff9038a48a3d363f4c21a3d86

```

果然 master 分支的引用文件的指向更改为前一次提交的 ID 了。而且通过下面的命令可以看出新添加的文件`new-commit.txt`也丢失了。

```
$ ls
welcome.txt

```

重置命令不仅仅可以重置到前一次提交，重置命令可以直接使用提交 ID 重置到任何一次提交。

*   通过**git log**查询到最早的提交 ID。

```
$ git log --graph --oneline
* e695606 which version checked in?
* a0c641e who does commit?
* 9e8a761 initialized.

```

*   然后重置到最早的一次提交。

```
$ git reset --hard 9e8a761
HEAD is now at 9e8a761 initialized.

```

*   重置后会发现`welcome.txt`也回退到原始版本库，曾经的修改都丢失了。

```
$ cat welcome.txt
Hello.

```

使用重置命令很危险，会彻底的丢弃历史。那么还能够通过浏览提交历史的办法找到丢弃的提交 ID，再使用重置命令恢复历史么？不可能！因为重置让提交历史也改变了。

```
$ git log
commit 9e8a761ff9dd343a1380032884f488a2422c495a
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Sun Nov 28 12:48:26 2010 +0800

    initialized.

```

## 2.4.2\. 用 reflog 挽救错误的重置

如果没有记下重置前 master 分支指向的提交 ID，想要重置回原来的提交真的是一件麻烦的事情（去对象库中一个一个地找）。幸好 Git 提供了一个挽救机制，通过`.git/logs`目录下日志文件记录了分支的变更。默认非裸版本库（带有工作区）都提供分支日志功能，这是因为带有工作区的版本库都有如下设置：

```
$ git config core.logallrefupdates
true

```

查看一下 master 分支的日志文件`.git/logs/refs/heads/master`中的内容。下面命令显示了该文件的最后几行。为了排版的需要，还将输出中的 40 位的 SHA1 提交 ID 缩短。

```
$ tail -5 .git/logs/refs/heads/master
dca47ab a0c641e Jiang Xin <jiangxin@ossxp.com> 1290999606 +0800    commit (amend): who does commit?
a0c641e e695606 Jiang Xin <jiangxin@ossxp.com> 1291022581 +0800    commit: which version checked in?
e695606 4902dc3 Jiang Xin <jiangxin@ossxp.com> 1291435985 +0800    commit: does master follow this new commit?
4902dc3 e695606 Jiang Xin <jiangxin@ossxp.com> 1291436302 +0800    HEAD^: updating HEAD
e695606 9e8a761 Jiang Xin <jiangxin@ossxp.com> 1291436382 +0800    9e8a761: updating HEAD

```

可以看出这个文件记录了 master 分支指向的变迁，最新的改变追加到文件的末尾因此最后出现。最后一行可以看出因为执行了**git reset --hard**命令，指向的提交 ID 由`e695606`改变为`9e8a761`。

Git 提供了一个**git reflog**命令，对这个文件进行操作。使用`show`子命令可以显示此文件的内容。

```
$ git reflog show master | head -5
9e8a761 master@{0}: 9e8a761: updating HEAD
e695606 master@{1}: HEAD^: updating HEAD
4902dc3 master@{2}: commit: does master follow this new commit?
e695606 master@{3}: commit: which version checked in?
a0c641e master@{4}: commit (amend): who does commit?

```

使用**git reflog**的输出和直接查看日志文件最大的不同在于显示顺序的不同，即最新改变放在了最前面显示，而且只显示每次改变的最终的 SHA1 哈希值。还有个重要的区别在于使用**git reflog**的输出中还提供一个方便易记的表达式：`<refname>@{<n>}`。这个表达式的含义是引用`<refname>`之前第<n>次改变时的 SHA1 哈希值。

那么将引用 master 切换到两次变更之前的值，可以使用下面的命令。

*   重置 master 为两次改变之前的值。

    ```
    $ git reset --hard master@{2}
    HEAD is now at 4902dc3 does master follow this new commit?

    ```

*   重置后工作区中文件`new-commit.txt`又回来了。

    ```
    $ ls
    new-commit.txt  welcome.txt

    ```

*   提交历史也回来了。

    ```
    $ git log --oneline
    4902dc3 does master follow this new commit?
    e695606 which version checked in?
    a0c641e who does commit?
    9e8a761 initialized.

    ```

此时如果再用**git reflog**查看，会看到恢复 master 的操作也记录在日志中了。

```
$ git reflog show master | head -5
4902dc3 master@{0}: master@{2}: updating HEAD
9e8a761 master@{1}: 9e8a761: updating HEAD
e695606 master@{2}: HEAD^: updating HEAD
4902dc3 master@{3}: commit: does master follow this new commit?
e695606 master@{4}: commit: which version checked in?

```

## 2.4.3\. 深入了解**git reset**命令

重置命令（**git reset**）是 Git 最常用的命令之一，也是最危险，最容易误用的命令。来看看**git reset**命令的用法。

```
用法一： git reset [-q] [<commit>] [--] <paths>...
用法二： git reset [--soft | --mixed | --hard | --merge | --keep] [-q] [<commit>]

```

上面列出了两个用法，其中 <commit> 都是可选项，可以使用引用或者提交 ID，如果省略 <commit> 则相当于使用了 HEAD 的指向作为提交 ID。

上面列出的两种用法的区别在于，第一种用法在命令中包含路径`<paths>`。为了避免路径和引用（或者提交 ID）同名而冲突，可以在`<paths>`前用两个连续的短线（减号）作为分隔。

第一种用法（包含了路径`<paths>`的用法）**不会**重置引用，更不会改变工作区，而是用指定提交状态（<commit>）下的文件（<paths>）替换掉暂存区中的文件。例如命令**git reset HEAD <paths>**相当于取消之前执行的**git add <paths>**命令时改变的暂存区。

第二种用法（不使用路径`<paths>`的用法）则会**重置引用**。根据不同的选项，可以对暂存区或者工作区进行重置。参照下面的版本库模型图，来看一看不同的参数对第二种重置语法的影响。

> ![../_images/git-reset.png](img/git-reset.png)

命令格式: git reset [–soft | –mixed | –hard ] [<commit>]

*   使用参数`--hard`，如：**git reset --hard <commit>**。

    会执行上图中的 1、2、3 全部的三个动作。即：

    1.  替换引用的指向。引用指向新的提交 ID。
    2.  替换暂存区。替换后，暂存区的内容和引用指向的目录树一致。
    3.  替换工作区。替换后，工作区的内容变得和暂存区一致，也和 HEAD 所指向的目录树内容相同。
*   使用参数`--soft`，如:**git reset --soft <commit>**。

    会执行上图中的操作 1。即只更改引用的指向，不改变暂存区和工作区。

*   使用参数`--mixed`或者不使用参数（缺省即为`--mixed`），如:**git reset <commit>**。

    会执行上图中的操作 1 和操作 2。即更改引用的指向以及重置暂存区，但是不改变工作区。

下面通过一些示例，看一下重置命令的不同用法。

*   命令：**git reset**

    仅用 HEAD 指向的目录树重置暂存区，工作区不会受到影响，相当于将之前用**git add**命令更新到暂存区的内容撤出暂存区。引用也未改变，因为引用重置到 HEAD 相当于没有重置。

*   命令：**git reset HEAD**

    同上。

*   命令：**git reset -- filename**

    仅将文件`filename`撤出暂存区，暂存区中其他文件不改变。相当于对命令**git add filename**的反向操作。

*   命令：**git reset HEAD filename**

    同上。

*   命令：**git reset --soft HEAD^**

    工作区和暂存区不改变，但是引用向前回退一次。当对最新提交的提交说明或者提交的更改不满意时，撤销最新的提交以便重新提交。

    在之前曾经介绍过一个修补提交命令**git commit --amend**，用于对最新的提交进行重新提交以修补错误的提交说明或者错误的提交文件。修补提交命令实际上相当于执行了下面两条命令。（注：文件`.git/COMMIT_EDITMSG`保存了上次的提交日志）

    ```
    $ git reset --soft HEAD^
    $ git commit -e -F .git/COMMIT_EDITMSG

    ```

*   命令：**git reset HEAD^**

    工作区不改变，但是暂存区会回退到上一次提交之前，引用也会回退一次。

*   命令：**git reset --mixed HEAD^**

    同上。

*   命令：**git reset --hard HEAD^**

    彻底撤销最近的提交。引用回退到前一次，而且工作区和暂存区都会回退到上一次提交的状态。自上一次以来的提交全部丢失。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 2.5\. Git 检出

在上一章学习了重置命令（**git reset**）。重置命令的一个用途就是修改引用（如 master）的游标。实际上在执行重置命令的时候没有使用任何参数对所要重置的分支名进行设置，这是因为重置命名实际上所针对的是头指针 HEAD。之所以没有改变 HEAD 的内容是因为 HEAD 指向了一个引用`refs/heads/master`，所以重置命令体现为分支“游标”的变更，HEAD 本身一直指向的是 refs/heads/master，并没有在重置时改变。

如果 HEAD 的内容不能改变而一直都指向 master 分支，那么 Git 如此精妙的分支设计岂不浪费？如果 HEAD 要改变该如何改变呢？本章将学习检出命令（**git checkout**），该命令的实质就是修改 HEAD 本身的指向，该命令不会影响分支“游标”（如 master）。

## 2.5.1\. HEAD 的重置即检出

HEAD 可以理解为“头指针”，是当前工作区的“基础版本”，当执行提交时，HEAD 指向的提交将作为新提交的父提交。看看当前 HEAD 的指向。

```
$ cat .git/HEAD
ref: refs/heads/master

```

可以看出 HEAD 指向了分支 master。此时执行**git branch**会看到当前处于 master 分支。

```
$ git branch -v
* master 4902dc3 does master follow this new commit?

```

现在使用**git checkout**命令检出该 ID 的父提交，看看会怎样。

```
$ git checkout 4902dc3^
Note: checking out '4902dc3^'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b new_branch_name

HEAD is now at e695606... which version checked in?

```

出现了大段的输出！翻译一下，Git 肯定又是在提醒我们了。

```
$ git checkout 4902dc3^
注意: 正检出 '4902dc3^'.

您现在处于 '分离头指针' 状态。您可以检查、测试和提交，而不影响任何分支。
通过执行另外的一个 checkout 检出指令会丢弃在此状态下的修改和提交。

如果想保留在此状态下的修改和提交，使用 -b 参数调用 checkout 检出指令以
创建新的跟踪分支。如：

  git checkout -b new_branch_name

头指针现在指向 e695606... 提交说明为： which version checked in?

```

什么叫做“分离头指针”状态？查看一下此时 HEAD 的内容就明白了。

```
$ cat .git/HEAD
e695606fc5e31b2ff9038a48a3d363f4c21a3d86

```

原来“分离头指针”状态指的就是 HEAD 头指针指向了一个具体的提交 ID，而不是一个引用（分支）。

查看最新提交的 reflog 也可以看到当针对提交执行**git checkout**命令时，HEAD 头指针被更改了：由指向 master 分支变成了指向一个提交 ID。

```
$ git reflog -1
e695606 HEAD@{0}: checkout: moving from master to 4902dc3^

```

注意上面的 reflog 是 HEAD 头指针的变迁记录，而非 master 分支。

查看一下 HEAD 和 master 对应的提交 ID，会发现现在它们指向的不一样。

```
$ git rev-parse HEAD master
e695606fc5e31b2ff9038a48a3d363f4c21a3d86
4902dc375672fbf52a226e0354100b75d4fe31e3

```

前一个是 HEAD 头指针的指向，后一个是 master 分支的指向。而且还可以看到执行**git checkout**命令并不像**git reset**命令，分支（master）的指向并没有改变，仍旧指向原有的提交 ID。

现在版本库的 HEAD 是基于`e695606`提交的。再做一次提交，HEAD 会如何变化呢？

*   先做一次修改：创建一个新文件`detached-commit.txt`，添加到暂存区中。

    ```
    $ touch detached-commit.txt
    $ git add detached-commit.txt

    ```

*   看一下状态，会发现其中有：“当前不处于任何分支”的字样，显然这是因为 HEAD 处于“分离头指针”模式。

    ```
    $ git status
    # Not currently on any branch.
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       new file:   detached-commit.txt
    #

    ```

*   执行提交。在提交输出中也会出现`[detached HEAD ...]`的标识，也是对用户的警示。

    ```
    $ git commit -m "commit in detached HEAD mode."
    [detached HEAD acc2f69] commit in detached HEAD mode.
     0 files changed, 0 insertions(+), 0 deletions(-)
     create mode 100644 detached-commit.txt

    ```

*   此时头指针指向了新的提交。

    ```
    $ cat .git/HEAD
    acc2f69cf6f0ae346732382c819080df75bb2191

    ```

*   再查看一下日志会发现新的提交是建立在之前的提交基础上的。

    ```
    $ git log --graph --pretty=oneline
    * acc2f69cf6f0ae346732382c819080df75bb2191 commit in detached HEAD mode.
    * e695606fc5e31b2ff9038a48a3d363f4c21a3d86 which version checked in?
    * a0c641e92b10d8bcca1ed1bf84ca80340fdefee6 who does commit?
    * 9e8a761ff9dd343a1380032884f488a2422c495a initialized.

    ```

记下新的提交 ID（acc2f69），然后以 master 分支名作为参数执行**git checkout**命令，会切换到 master 分支上。

*   切换到 master 分支。没有之前大段的文字警告。

    ```
    $ git checkout master
    Previous HEAD position was acc2f69... commit in detached HEAD mode.
    Switched to branch 'master'

    ```

*   因为 HEAD 头指针重新指向了分支，而不是处于“断头模式”（分离头指针模式）。

    ```
    $ cat .git/HEAD
    ref: refs/heads/master

    ```

*   切换之后，之前本地建立的新文件`detached-commit.txt`不见了。

    ```
    $ ls
    new-commit.txt  welcome.txt

    ```

*   切换之后，刚才的提交日志也不见了。

    ```
    $ git log --graph --pretty=oneline
    * 4902dc375672fbf52a226e0354100b75d4fe31e3 does master follow this new commit?
    * e695606fc5e31b2ff9038a48a3d363f4c21a3d86 which version checked in?
    * a0c641e92b10d8bcca1ed1bf84ca80340fdefee6 who does commit?
    * 9e8a761ff9dd343a1380032884f488a2422c495a initialized.

    ```

刚才的提交在版本库的对象库中还存在么？看看刚才记下的提交 ID。

```
$ git show acc2f69
commit acc2f69cf6f0ae346732382c819080df75bb2191
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Sun Dec 5 15:43:24 2010 +0800

    commit in detached HEAD mode.

diff --git a/detached-commit.txt b/detached-commit.txt
new file mode 100644
index 0000000..e69de29

```

可以看出这个提交现在仍在版本库中。由于这个提交没有被任何分支跟踪到，因此并不能保证这个提交会永久存在。实际上当 reflog 中含有该提交的日志过期后，这个提交随时都会从版本库中彻底清除。

## 2.5.2\. 挽救分离头指针

在“分离头指针”模式下进行的测试提交除了使用提交 ID（acc2f69）访问之外，不能通过 master 分支或其他引用访问到。如果这个提交是 master 分支所需要的，那么该如何处理呢？如果使用上一章介绍的**git reset**命令，的确可以将 master 分支重置到该测试提交`acc2f69`，但是如果那样就会丢掉 master 分支原先的提交`4902dc3`。使用合并操作（**git merge**）可以实现两者的兼顾。

下面的操作会将提交`acc2f69`合并到 master 分支中来。

*   确认当前处于 master 分支。

    ```
    $ git branch -v
    * master 4902dc3 does master follow this new commit?

    ```

*   执行合并操作，将`acc2f69`提交合并到当前分支。

    ```
    $ git merge acc2f69
    Merge made by recursive.
     0 files changed, 0 insertions(+), 0 deletions(-)
     create mode 100644 detached-commit.txt

    ```

*   工作区中多了一个`detached-commit.txt`文件。

    ```
    $ ls
    detached-commit.txt  new-commit.txt  welcome.txt

    ```

*   查看日志，会看到不一样的分支图。即在`e695606`提交开始出现了开发分支，而分支在最新的`2b31c19`提交发生了合并。

    ```
    $ git log --graph --pretty=oneline
    *   2b31c199d5b81099d2ecd91619027ab63e8974ef Merge commit 'acc2f69'
    |\
    | * acc2f69cf6f0ae346732382c819080df75bb2191 commit in detached HEAD mode.
    * | 4902dc375672fbf52a226e0354100b75d4fe31e3 does master follow this new commit?
    |/
    * e695606fc5e31b2ff9038a48a3d363f4c21a3d86 which version checked in?
    * a0c641e92b10d8bcca1ed1bf84ca80340fdefee6 who does commit?
    * 9e8a761ff9dd343a1380032884f488a2422c495a initialized.

    ```

*   仔细看看最新提交，会看到这个提交有两个父提交。这就是合并的奥秘。

    ```
    $ git cat-file -p HEAD
    tree ab676f92936000457b01507e04f4058e855d4df0
    parent 4902dc375672fbf52a226e0354100b75d4fe31e3
    parent acc2f69cf6f0ae346732382c819080df75bb2191
    author Jiang Xin <jiangxin@ossxp.com> 1291535485 +0800
    committer Jiang Xin <jiangxin@ossxp.com> 1291535485 +0800

    Merge commit 'acc2f69'

    ```

## 2.5.3\. 深入了解**git checkout**命令

检出命令（**git checkout**）是 Git 最常用的命令之一，同样也很危险，因为这条命令会重写工作区。

```
用法一： git checkout [-q] [<commit>] [--] <paths>...
用法二： git checkout [<branch>]
用法三： git checkout [-m] [[-b|--orphan] <new_branch>] [<start_point>]

```

上面列出的第一种用法和第二种用法的区别在于，第一种用法在命令中包含路径`<paths>`。为了避免路径和引用（或者提交 ID）同名而冲突，可以在`<paths>`前用两个连续的短线（减号）作为分隔。

第一种用法的`<commit>`是可选项，如果省略则相当于从暂存区（index）进行检出。这和上一章的重置命令大不相同：重置的默认值是 HEAD，而检出的默认值是暂存区。因此重置一般用于重置暂存区（除非使用`--hard`参数，否则不重置工作区），而检出命令主要是覆盖工作区（如果`<commit>`不省略，也会替换暂存区中相应的文件）。

第一种用法（包含了路径`<paths>`的用法）**不会**改变 HEAD 头指针，主要是用于指定版本的文件覆盖工作区中对应的文件。如果省略`<commit>`，会拿暂存区的文件覆盖工作区的文件，否则用指定提交中的文件覆盖暂存区和工作区中对应的文件。

第二种用法（不使用路径`<paths>`的用法）则会**改变**HEAD 头指针。之所以后面的参数写作`<branch>`，是因为只有 HEAD 切换到一个分支才可以对提交进行跟踪，否则仍然会进入“分离头指针”的状态。在“分离头指针”状态下的提交不能被引用关联到而可能会丢失。所以用法二最主要的作用就是切换到分支。如果省略`<branch>`则相当于对工作区进行状态检查。

第三种用法主要是创建和切换到新的分支（`<new_branch>`），新的分支从`<start_point>`指定的提交开始创建。新分支和我们熟悉的 master 分支没有什么实质的不同，都是在`refs/heads`命名空间下的引用。关于分支和**git checkout**命令的这个用法会在后面的章节做具体的介绍。

下面的版本库模型图描述了**git checkout**实际完成的操作。

> ![../_images/git-checkout.png](img/git-checkout.png)

下面通过一些示例，具体的看一下检出命令的不同用法。

*   命令：**git checkout branch**

    检出 branch 分支。要完成如图的三个步骤，更新 HEAD 以指向 branch 分支，以 branch 指向的树更新暂存区和工作区。

*   命令：**git checkout**

    汇总显示工作区、暂存区与 HEAD 的差异。

*   命令：**git checkout HEAD**

    同上。

*   命令：**git checkout -- filename**

    用暂存区中`filename`文件来覆盖工作区中的`filename`文件。相当于取消自上次执行**git add filename**以来（如果执行过）本地的修改。

    这个命令很危险，因为对于本地的修改会悄无声息的覆盖，毫不留情。

*   命令：**git checkout branch -- filename**

    维持 HEAD 的指向不变。将 branch 所指向的提交中的`filename`替换暂存区和工作区中相应的文件。注意会将暂存区和工作区中的`filename`文件直接覆盖。

*   命令：**git checkout -- . 或写做 git checkout .**

    注意：**git checkout**命令后的参数为一个点（“.”）。这条命令最危险！会取消所有本地的修改（相对于暂存区）。相当于将暂存区的所有文件直接覆盖本地文件，不给用户任何确认的机会！

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 2.6\. 恢复进度

在之前“Git 暂存区”一章的结尾，曾经以终结者（The Terminator）的口吻说：“我会再回来”，会继续对暂存区的探索。经过了前面三章对 Git 对象、重置命令、检出命令的探索，现在已经拥有了足够多的武器，是时候“回归”了。

本章“回归”之后，再看 Git 状态输出中关于**git reset**或者**git checkout**的指示，有了前面几章的基础已经会觉得很亲切和易如反掌了。本章还会重点介绍“回归”使用的**git stash**命令。

## 2.6.1\. 继续暂存区未完成的实践

经过了前面的实践，现在 DEMO 版本库应该处于 master 分支上，看看是不是这样。

```
$ cd /path/to/my/workspace/demo
$ git status -sb       # Git 1.7.2 及以上版本才支持 -b 参数哦
## master
$ git log --graph --pretty=oneline --stat
*   2b31c199d5b81099d2ecd91619027ab63e8974ef Merge commit 'acc2f69'
|\
| * acc2f69cf6f0ae346732382c819080df75bb2191 commit in detached HEAD mode.
| |  0 files changed, 0 insertions(+), 0 deletions(-)
* | 4902dc375672fbf52a226e0354100b75d4fe31e3 does master follow this new commit?
|/
|    0 files changed, 0 insertions(+), 0 deletions(-)
* e695606fc5e31b2ff9038a48a3d363f4c21a3d86 which version checked in?
|  welcome.txt |    1 +
|  1 files changed, 1 insertions(+), 0 deletions(-)
* a0c641e92b10d8bcca1ed1bf84ca80340fdefee6 who does commit?
* 9e8a761ff9dd343a1380032884f488a2422c495a initialized.
   welcome.txt |    1 +
   1 files changed, 1 insertions(+), 0 deletions(-)

```

还记得在之前“Git 暂存区”一章的结尾，是如何保存进度的么？翻回去看一下，用的是**git stash**命令。这个命令用于保存当前进度，也是恢复进度要用的命令。

查看保存的进度用命令**git stash list**。

```
$ git stash list
stash@{0}: WIP on master: e695606 which version checked in?

```

现在就来恢复进度。使用**git stash pop**从最近保存的进度进行恢复。

```
$ git stash pop
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       new file:   a/b/c/hello.txt
#
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#       modified:   welcome.txt
#
Dropped refs/stash@{0} (c1bd56e2565abd64a0d63450fe42aba23b673cf3)

```

先不要管**git stash pop**命令的输出，后面会专题介绍**git stash**命令。通过查看工作区的状态，可以发现进度已经找回了（状态和进度保存前稍有不同）。

```
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       new file:   a/b/c/hello.txt
#
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#       modified:   welcome.txt
#

```

此时再看 Git 状态输出，是否别有一番感觉呢？有了前面三章的基础，现在可以游刃有余的应对各种情况了。

*   以当前暂存区状态进行提交，即只提交`a/b/c/hello.txt`，不提交`welcome.txt`。

    *   执行提交：

        ```
  $ git commit -m "add new file: a/b/c/hello.txt, but leave welcome.txt alone."
        [master 6610d05] add new file: a/b/c/hello.txt, but leave welcome.txt alone.
         1 files changed, 2 insertions(+), 0 deletions(-)
         create mode 100644 a/b/c/hello.txt

        ```

    *   查看提交后的状态：

        ```
  $ git status -s
         M welcome.txt

        ```

*   反悔了，回到之前的状态。

    *   用重置命令放弃最新的提交：

        ```
  $ git reset --soft HEAD^

        ```

    *   查看最新的提交日志，可以看到前面的提交被抛弃了。

        ```
  $ git log -1 --pretty=oneline
        2b31c199d5b81099d2ecd91619027ab63e8974ef Merge commit 'acc2f69'

        ```

    *   工作区和暂存区的状态也都维持原来的状态。

        ```
  $ git status -s
        A  a/b/c/hello.txt
         M welcome.txt

        ```

*   想将`welcome.txt`提交。

    再简单不过了。

    ```
    $ git add welcome.txt
    $ git status -s
    A  a/b/c/hello.txt
    M  welcome.txt

    ```

*   想将`a/b/c/hello.txt`撤出暂存区。

    也是用重置命令。

    ```
    $ git reset HEAD a/b/c
    $ git status -s
    M  welcome.txt
    ?? a/

    ```

*   想将剩下的文件（`welcome.txt`）从暂存区撤出，就是说不想提交任何东西了。

    还是使用重置命令，甚至可以不使用任何参数。

    ```
    $ git reset
    Unstaged changes after reset:
    M       welcome.txt

    ```

*   想将本地工作区所有的修改清除。即清除`welcome.txt`的改动，删除添加的目录`a`即下面的子目录和文件。

    *   清除`welcome.txt`的改动用检出命令。

        实际对于此例执行**git checkout .**也可以。

        ```
  $ git checkout -- welcome.txt

        ```

    *   工作区显示还有一个多余的目录`a`。

        ```
  $ git status
        # On branch master
        # Untracked files:
        #   (use "git add <file>..." to include in what will be committed)
        #
        #       a/

        ```

    *   删除本地多余的目录和文件，可以使用**git clean**命令。先来测试运行以便看看哪些文件和目录会被删除，以免造成误删。

        ```
  $ git clean -nd
        Would remove a/

        ```

    *   真正开始强制删除多余的目录和文件。

        ```
  $ git clean -fd
        Removing a/

        ```

    *   整个世界清净了。

        ```
  $ git status -s

        ```

## 2.6.2\. 使用**git stash**

命令**git stash**可以用于保存和恢复工作进度，掌握这个命令对于日常的工作会有很大的帮助。关于这个命令的最主要的用法实际上通过前面的演示已经了解了。

*   命令：**git stash**

    保存当前工作进度。会分别对暂存区和工作区的状态进行保存。

*   命令：**git stash list**

    显示进度列表。此命令显然暗示了**git stash**可以多次保存工作进度，并且在恢复的时候进行选择。

*   命令：**git stash pop [--index] [<stash>]**

    如果不使用任何参数，会恢复最新保存的工作进度，并将恢复的工作进度从存储的工作进度列表中清除。

    如果提供`<stash>`参数（来自于**git stash list**显示的列表），则从该`<stash>`中恢复。恢复完毕也将从进度列表中删除`<stash>`。

    选项`--index`除了恢复工作区的文件外，还尝试恢复暂存区。这也就是为什么在本章一开始恢复进度的时候显示的状态和保存进度前略有不同。

实际上还有几个用法也很有用。

*   命令：**git stash [save [--patch] [-k|--[no-]keep-index] [-q|--quiet] [<message>]]**

    *   这条命令实际上是第一条**git stash**命令的完整版。即如果需要在保存工作进度的时候使用指定的说明，必须使用如下格式：

        ```
  git stash save "message..."

        ```

    *   使用参数`--patch`会显示工作区和 HEAD 的差异，通过对差异文件的编辑决定在进度中最终要保存的工作区的内容，通过编辑差异文件可以在进度中排除无关内容。

    *   使用`-k`或者`--keep-index`参数，在保存进度后不会将暂存区重置。缺省会将暂存区和工作区强制重置。

*   命令：**git stash apply [--index] [<stash>]**

    除了不删除恢复的进度之外，其余和**git stash pop**命令一样。

*   命令：**git stash drop [<stash>]**

    删除一个存储的进度。缺省删除最新的进度。

*   命令：**git stash clear**

    删除所有存储的进度。

*   命令：**git stash branch <branchname> <stash>**

    基于进度创建分支。对了，还没有讲到分支呢。;)

## 2.6.3\. 探秘**git stash**

了解一下**git stash**的机理会有几个好处：当保存了多个进度的时候知道从哪个进度恢复；综合运用前面介绍的 Git 知识点；了解 Git 的源码，Git 将不再神秘。

在执行**git stash**命令时，Git 实际调用了一个脚本文件实现相关的功能，这个脚本的文件名就是`git-stash`。看看`git-stash`安装在哪里了。

```
$ git --exec-path
/usr/lib/git-core

```

如果检查一下这个目录，会震惊的。

```
$ ls /usr/lib/git-core/
git                    git-help                 git-reflog
git-add                git-http-backend         git-relink
git-add--interactive   git-http-fetch           git-remote
git-am                 git-http-push            git-remote-ftp
git-annotate           git-imap-send            git-remote-ftps
git-apply              git-index-pack           git-remote-http
..................
... 省略 40 余行 ...
..................

```

实际上在 1.5.4 之前的版本，Git 会安装这些一百多个以**git-<cmd>**格式命名的程序到可执行路径中。这样做的唯一好处就是不用借助任何扩展机制就可以实现命令行补齐：即键入`git-`后，连续两次键入`<Tab>`键，就可以把这一百多个命令显示出来。这种方式随着 Git 子命令的增加越来越显得混乱，因此在 1.5.4 版本开始，不再提供**git-<cmd>**格式的命令，而是用唯一的**git**命令。而之前的名为**git-<cmd>**的子命令则保存在非可执行目录下，由 Git 负责加载。

在后面的章节中偶尔会看到形如**git-<cmd>**字样的名称，以及同时存在的**git <cmd>**命令。可以这样理解：**git-<cmd>**作为软件本身的名称，而其命令行为**git <cmd>**。

最早很多 Git 命令都是用 Shell 或者 Perl 脚本语言开发的，在 Git 的发展中一些对运行效率要求高的命令用 C 语言改写。而`git-stash`（至少在 Git 1.7.3.2 版本）还是使用 Shell 脚本开发的，研究它会比研究用 C 写的命令要简单的多。

```
$ file /usr/lib/git-core/git-stash
/usr/lib/git-core/git-stash: POSIX shell script text executable

```

解析`git-stash`脚本会比较枯燥，还是通过运行一些示例更好一些。

当前的进度保存列表是空的。

```
$ git stash list

```

下面在工作区中做一些改动。

```
$ echo Bye-Bye. >> welcome.txt
$ echo hello. > hack-1.txt
$ git add hack-1.txt
$ git status -s
A  hack-1.txt
 M welcome.txt

```

可见暂存区中已经添加了新增的`hack-1.txt`，修改过的`welcome.txt`并未添加到暂存区。执行**git stash**保存一下工作进度。

```
$ git stash save "hack-1: hacked welcome.txt, newfile hack-1.txt"
Saved working directory and index state On master: hack-1: hacked welcome.txt, newfile hack-1.txt
HEAD is now at 2b31c19 Merge commit 'acc2f69'

```

再来看工作区恢复了修改前的原貌（实际上用了 git reset –hard HEAD 命令），文件`welcome.txt`的修改不见了，文件`hack-1.txt`整个都不见了。

```
$ git status -s
$ ls
detached-commit.txt  new-commit.txt  welcome.txt

```

再做一个修改，并尝试保存进度。

```
$ echo fix. > hack-2.txt
$ git stash
No local changes to save

```

进度保存失败！可见本地没有被版本控制系统跟踪的文件并不能保存进度。因此本地新文件需要执行添加再执行**git stash**命令。

```
$ git add hack-2.txt
$ git stash
Saved working directory and index state WIP on master: 2b31c19 Merge commit 'acc2f69'
HEAD is now at 2b31c19 Merge commit 'acc2f69'

```

不用看就知道工作区再次恢复原状。如果这时执行**git stash list**会看到有两次进度保存。

```
$ git stash list
stash@{0}: WIP on master: 2b31c19 Merge commit 'acc2f69'
stash@{1}: On master: hack-1: hacked welcome.txt, newfile hack-1.txt

```

从上面的输出可以得出两个结论：

*   在用**git stash**命令保存进度时，提供说明更容易找到对应的进度文件。
*   每个进度的标识都是`stash@{<n>}`格式，像极了前面介绍的 reflog 的格式。

实际上，**git stash**的就是用到了前面介绍的引用和引用变更日志（reflog）来实现的。

```
$ ls -l .git/refs/stash .git/logs/refs/stash
-rw-r--r-- 1 jiangxin jiangxin 364 Dec  6 16:11 .git/logs/refs/stash
-rw-r--r-- 1 jiangxin jiangxin  41 Dec  6 16:11 .git/refs/stash

```

那么在“Git 重置”一章中学习的 reflog 可以派上用场了。

```
$ git reflog show refs/stash
e5c0cdc refs/stash@{0}: WIP on master: 2b31c19 Merge commit 'acc2f69'
6cec9db refs/stash@{1}: On master: hack-1: hacked welcome.txt, newfile hack-1.txt

```

对照**git reflog**的结果和前面**git stash list**的结果，可以肯定用**git stash**保存进度，实际上会将进度保存在引用`refs/stash`所指向的提交中。多次的进度保存，实际上相当于引用`refs/stash`一次又一次的变化，而`refs/stash`引用的变化由 reflog（即**.git/logs/refs/stash**）所记录下来。这个实现是多么的简单而巧妙啊。

新的一个疑问又出现了，如何在引用`refs/stash`中同时保存暂存区的进度和工作区中的进度呢？查看一下引用`refs/stash`的提交历史能够看出端倪。

```
$ git log --graph --pretty=raw  refs/stash -2
*   commit e5c0cdc2dedc3e50e6b72a683d928e19a1d9de48
|\  tree 780c22449b7ff67e2820e09a6332c360ddc80578
| | parent 2b31c199d5b81099d2ecd91619027ab63e8974ef
| | parent c5edbdcc90addb06577ff60f644acd1542369194
| | author Jiang Xin <jiangxin@ossxp.com> 1291623066 +0800
| | committer Jiang Xin <jiangxin@ossxp.com> 1291623066 +0800
| |
| |     WIP on master: 2b31c19 Merge commit 'acc2f69'
| |
| * commit c5edbdcc90addb06577ff60f644acd1542369194
|/  tree 780c22449b7ff67e2820e09a6332c360ddc80578
|   parent 2b31c199d5b81099d2ecd91619027ab63e8974ef
|   author Jiang Xin <jiangxin@ossxp.com> 1291623066 +0800
|   committer Jiang Xin <jiangxin@ossxp.com> 1291623066 +0800
|
|       index on master: 2b31c19 Merge commit 'acc2f69'

```

可以看到在提交关系图可以看到进度保存的最新提交是一个合并提交。最新的提交说明中有`WIP`字样（是 Work In Progess 的简称），说明代表了工作区进度。而最新提交的第二个父提交（上图中显示为第二个提交）有`index on master`字样，说明这个提交代表着暂存区的进度。

但是上图中的两个提交都指向了同一个树——tree ``780c224``...，这是因为最后一次做进度保存时工作区相对暂存区没有改变，这让关于工作区和暂存区在引用`refs/stash`中的存储变得有些扑朔迷离。别忘了第一次进度保存工作区、暂存区和版本库都是不同的，可以用于验证关于`refs/stash`实现机制的判断。

第一次进度保存可以用 reflog 中的语法，即用`refs/stash@{1}`来访问，也可以用简称`stash@{1}`。下面就用第一次的进度保存来研究一下。

```
$ git log --graph --pretty=raw  stash@{1} -3
*   commit 6cec9db44af38d01abe7b5025a5190c56fd0cf49
|\  tree 7250f186c6aa3e2d1456d7fa915e529601f21d71
| | parent 2b31c199d5b81099d2ecd91619027ab63e8974ef
| | parent 4560d76c19112868a6a5692bf9379de09c0452b7
| | author Jiang Xin <jiangxin@ossxp.com> 1291622767 +0800
| | committer Jiang Xin <jiangxin@ossxp.com> 1291622767 +0800
| |
| |     On master: hack-1: hacked welcome.txt, newfile hack-1.txt
| |
| * commit 4560d76c19112868a6a5692bf9379de09c0452b7
|/  tree 5d4dd328187e119448c9171f99cf2e507e91a6c6
|   parent 2b31c199d5b81099d2ecd91619027ab63e8974ef
|   author Jiang Xin <jiangxin@ossxp.com> 1291622767 +0800
|   committer Jiang Xin <jiangxin@ossxp.com> 1291622767 +0800
|
|       index on master: 2b31c19 Merge commit 'acc2f69'
|
*   commit 2b31c199d5b81099d2ecd91619027ab63e8974ef
|\  tree ab676f92936000457b01507e04f4058e855d4df0
| | parent 4902dc375672fbf52a226e0354100b75d4fe31e3
| | parent acc2f69cf6f0ae346732382c819080df75bb2191
| | author Jiang Xin <jiangxin@ossxp.com> 1291535485 +0800
| | committer Jiang Xin <jiangxin@ossxp.com> 1291535485 +0800
| |
| |     Merge commit 'acc2f69'

```

果然上面显示的三个提交对应的三棵树各不相同。查看一下差异。用“原基线”代表进度保存时版本库的状态，即提交`2b31c199`；用“原暂存区”代表进度保存时暂存区的状态，即提交`4560d76`；用“原工作区”代表进度保存时工作区的状态，即提交`6cec9db`。

*   原基线和原暂存区的差异比较。

    ```
    $ git diff stash@{1}²^ stash@{1}²
    diff --git a/hack-1.txt b/hack-1.txt
    new file mode 100644
    index 0000000..25735f5
    --- /dev/null
    +++ b/hack-1.txt
    @@ -0,0 +1 @@
    +hello.

    ```

*   原暂存区和原工作区的差异比较。

    ```
    $ git diff stash@{1}² stash@{1}
    diff --git a/welcome.txt b/welcome.txt
    index fd3c069..51dbfd2 100644
    --- a/welcome.txt
    +++ b/welcome.txt
    @@ -1,2 +1,3 @@
     Hello.
     Nice to meet you.
    +Bye-Bye.

    ```

*   原基线和原工作区的差异比较。

    ```
    $ git diff stash@{1}¹ stash@{1}
    diff --git a/hack-1.txt b/hack-1.txt
    new file mode 100644
    index 0000000..25735f5
    --- /dev/null
    +++ b/hack-1.txt
    @@ -0,0 +1 @@
    +hello.
    diff --git a/welcome.txt b/welcome.txt
    index fd3c069..51dbfd2 100644
    --- a/welcome.txt
    +++ b/welcome.txt
    @@ -1,2 +1,3 @@
     Hello.
     Nice to meet you.
    +Bye-Bye.

    ```

从`stash@{1}`来恢复进度。

```
$ git stash apply stash@{1}
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       new file:   hack-1.txt
#
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#       modified:   welcome.txt
#

```

显示进度列表，然后删除进度列表。

```
$ git stash list
stash@{0}: WIP on master: 2b31c19 Merge commit 'acc2f69'
stash@{1}: On master: hack-1: hacked welcome.txt, newfile hack-1.txt
$ git stash clear

```

删除进度列表之后，会发现 stash 相关的引用和 reflog 也都不见了。

```
$ ls -l .git/refs/stash .git/logs/refs/stash
ls: cannot access .git/refs/stash: No such file or directory
ls: cannot access .git/logs/refs/stash: No such file or directory

```

通过上面的这些分析，有一定 Shell 编程基础的读者就可以尝试研究`git-stash`的代码了，可能会有新的发现。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 2.7\. Git 基本操作

之前的实践选取的示例都非常简单，基本上都是增加和修改文本文件，而现实情况要复杂的多，需要应对各种情况：文件删除，文件复制，文件移动，目录的组织，二进制文件，误删文件的恢复等等。

本章要用一个更为真实的例子：通过对`Hello World`程序源代码的版本控制，来介绍工作区中其他的一些常用操作。首先会删除之前历次实践在版本库中留下的“垃圾”数据，然后再在其中创建一些真实的代码，并对其进行版本控制。

## 2.7.1\. 先来合个影

马上就要和之前实践遗留的数据告别了，告别之前是不是要留个影呢？在 Git 里，“留影”用的命令叫做**tag**，更加专业的术语叫做“里程碑”（打 tag，或打标签）。

```
$ cd /path/to/my/workspace/demo
$ git tag -m "Say bye-bye to all previous practice." old_practice

```

在本章还不打算详细介绍里程碑的奥秘，只要知道里程碑无非也是一个引用，通过记录提交 ID（或者创建 Tag 对象）来为当前版本库状态进行“留影”。

```
$ ls .git/refs/tags/old_practice
.git/refs/tags/old_practice

$ git rev-parse refs/tags/old_practice
41bd4e2cce0f8baa9bb4cdda62927b408c846cd6

```

留过影之后，可以执行**git describe**命令显示当前版本库的最新提交的版本号。显示的时候会选取离该提交最近的里程碑作为“基础版本号”，后面附加标识距离“基础版本”的数字以及该提交的 SHA1 哈希值缩写。因为最新的提交上恰好被打了一个“里程碑”，所以用“里程碑”的名字显示为版本号。这个技术在后面的示例代码中被使用。

```
$ git describe
old_practice

```

## 2.7.2\. 删除文件

看看版本库当前的状态，暂存区和工作区都包含修改。

```
$ git status -s
A  hack-1.txt
 M welcome.txt

```

在这个暂存区和工作区都包含文件修改的情况下，使用删除命令更具有挑战性。删除命令有多种使用方法，有的方法很巧妙，而有的方法需要更多的输入。为了分别介绍不同的删除方法，还要使用上一章介绍的进度保存（**git-stash**）命令。

*   保存进度。

    ```
    $ git stash
    Saved working directory and index state WIP on master: 2b31c19 Merge commit 'acc2f69'
    HEAD is now at 2b31c19 Merge commit 'acc2f69'

    ```

*   再恢复进度。注意不要使用**git stash pop**，而是使用**git stash apply**，因为这个保存的进度要被多次用到。

    ```
    $ git stash apply
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       new file:   hack-1.txt
    #
    # Changed but not updated:
    #   (use "git add <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #       modified:   welcome.txt
    #

    ```

### 2.7.2.1\. 本地删除不是真的删除

当前工作区的文件有：

```
$ ls
detached-commit.txt
hack-1.txt
new-commit.txt
welcome.txt

```

直接在工作区删除这些文件，会如何呢？

```
$ rm *.txt

```

通过下面的命令，可以看到在暂存区（版本库）中文件仍在，并未删除。

```
$ git ls-files
detached-commit.txt
hack-1.txt
new-commit.txt
welcome.txt

```

通过文件的状态来看，文件只是在本地进行了删除，尚未加到暂存区（提交任务）中。也就是说：**直接在工作区删除，对暂存区和版本库没有任何影响**。

```
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       new file:   hack-1.txt
#
# Changed but not updated:
#   (use "git add/rm <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#       deleted:    detached-commit.txt
#       deleted:    hack-1.txt
#       deleted:    new-commit.txt
#       deleted:    welcome.txt
#

```

从 Git 状态输出可以看出，本地删除如果要反映在暂存区中应该用**git rm**命令，对于不想删除的文件执行**git checkout -- <file>**可以让文件在工作区重现。

### 2.7.2.2\. 执行**git rm**命令删除文件

好吧，按照上面状态输出的内容，将所有的文本文件删除。执行下面的命令。

```
$ git rm detached-commit.txt hack-1.txt new-commit.txt welcome.txt
rm 'detached-commit.txt'
rm 'hack-1.txt'
rm 'new-commit.txt'
rm 'welcome.txt'

```

再看一看状态：

```
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       deleted:    detached-commit.txt
#       deleted:    new-commit.txt
#       deleted:    welcome.txt
#

```

删除动作加入了暂存区。这时执行提交动作，就真正意义上执行了文件删除。

```
$ git commit -m "delete trash files. (using: git rm)"
[master 483493a] delete trash files. (using: git rm)
 1 files changed, 0 insertions(+), 2 deletions(-)
 delete mode 100644 detached-commit.txt
 delete mode 100644 new-commit.txt
 delete mode 100644 welcome.txt

```

不过不要担心，文件只是在版本库最新提交中删除了，在历史提交中尚在。可以通过下面命令查看历史版本的文件列表。

```
$ git ls-files --with-tree=HEAD^
detached-commit.txt
new-commit.txt
welcome.txt

```

也可以查看在历史版本中尚在的删除文件的内容。

```
$ git cat-file -p HEAD^:welcome.txt
Hello.
Nice to meet you.

```

### 2.7.2.3\. 命令**git add -u**快速标记删除

在前面执行**git rm**命令时，一一写下了所有要删除的文件名，好长的命令啊！能不能简化些？实际上**git add**可以，即使用`-u`参数调用**git add**命令，含义是将本地有改动（包括添加和删除）的文件标记为删除。为了重现刚才的场景，先使用重置命令抛弃最新的提交，再使用进度恢复到之前的状态。

*   丢弃之前测试删除的试验性提交。

    ```
    $ git reset --hard HEAD^
    HEAD is now at 2b31c19 Merge commit 'acc2f69'

    ```

*   恢复保存的进度。（参数`-q`使得命令进入安静模式）

    ```
    $ git stash apply -q

    ```

然后删除本地文件，状态依然显示只在本地删除了文件，暂存区文件仍在。

```
$ rm *.txt
$ git status -s
 D detached-commit.txt
AD hack-1.txt
 D new-commit.txt
 D welcome.txt

```

执行**git add -u**命令可以将（被版本库追踪的）本地文件的变更（修改、删除）全部记录到暂存区中。

```
$ git add -u

```

查看状态，可以看到工作区删除的文件全部被标记为下次提交时删除。

```
$ git status -s
D  detached-commit.txt
D  new-commit.txt
D  welcome.txt

```

执行提交，删除文件。

```
$ git commit -m "delete trash files. (using: git add -u)"
[master 7161977] delete trash files. (using: git add -u)
 1 files changed, 0 insertions(+), 2 deletions(-)
 delete mode 100644 detached-commit.txt
 delete mode 100644 new-commit.txt
 delete mode 100644 welcome.txt

```

## 2.7.3\. 恢复删除的文件

经过了上面的文件删除，工作区已经没有文件了。为了说明文件移动，现在恢复一个删除的文件。前面已经说过执行了文件删除并提交，只是在最新的提交中删除了文件，历史提交中文件仍然保留，可以从历史提交中提取文件。执行下面的命令可以从历史（前一次提交）中恢复`welcome.txt`文件。

```
$ git cat-file -p HEAD~1:welcome.txt > welcome.txt

```

上面命令中出现的`HEAD~1`即相当于`HEAD^`都指的是 HEAD 的上一次提交。执行**git add -A**命令会对工作区中所有改动以及新增文件添加到暂存区，也是一个常用的技巧。执行下面的命令后，将恢复过来的`welcome.txt`文件添加回暂存区。

```
$ git add -A
$ git status -s
A  welcome.txt

```

执行提交操作，文件`welcome.txt`又回来了。

```
$ git commit -m "restore file: welcome.txt"
[master 63992f0] restore file: welcome.txt
 1 files changed, 2 insertions(+), 0 deletions(-)
 create mode 100644 welcome.txt

```

通过再次添加的方式恢复被删除的文件是最自然的恢复的方法。其他版本控制系统如 CVS 也采用同样的方法恢复删除的文件，但是有的版本控制系统如 Subversion 如果这样操作会有严重的副作用——文件变更历史被人为的割裂而且还会造成服务器存储空间的浪费。Git 通过添加方式反删除文件没有副作用，这是因为在 Git 的版本库中相同内容的文件保存在一个 blob 对象中，而且即便是内容不同的 blob 对象在对象库打包整理过程中也会通过差异比较优化存储。

## 2.7.4\. 移动文件

通过将`welcome.txt`改名为`README`文件来测试一下在 Git 中如何移动文件。Git 提供了**git mv**命令完成改名操作。

```
$ git mv welcome.txt README

```

可以从当前的状态中看到改名的操作。

```
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       renamed:    welcome.txt -> README
#

```

提交改名操作，在提交输出可以看到改名前后两个文件的相似度（百分比）。

```
$ git commit -m "改名测试"
[master 7aa5ac1] 改名测试
 1 files changed, 0 insertions(+), 0 deletions(-)
 rename welcome.txt => README (100%)

```

**可以不用:command:`git mv`命令实现改名**

从提交日志中出现的文件相似度可以看出 Git 的改名实际上源自于 Git 对文件追踪的强大支持（文件内容作为 blob 对象保存在对象库中）。改名操作实际上相当于对旧文件执行删除，对新文件执行添加，即完全可以不使用**git mv**操作，而是代之以**git rm**和一个**git add**操作。为了试验不使用**git mv**命令是否可行，先撤销之前进行的提交。

*   撤销之前测试文件移动的提交。

    ```
    $ git reset --hard HEAD^
    HEAD is now at 63992f0 restore file: welcome.txt

    ```

*   撤销之后`welcome.txt`文件又回来了。

    ```
    $ git status -s
    $ git ls-files
    welcome.txt

    ```

新的改名操作不使用**git mv**命令，而是直接在本地改名（文件移动），将`welcome.txt` 改名为`README`。

```
$ mv welcome.txt README
$ git status -s
 D welcome.txt
?? README

```

为了考验一下 Git 的内容追踪能力，再修改一下改名后的 README 文件，即在文件末尾追加一行。

```
$ echo "Bye-Bye." >> README

```

可以使用前面介绍的**git add -A**命令。相当于对修改文件执行**git add**，对删除文件执行**git rm**，而且对本地新增文件也执行**git add**。

```
$ git add -A

```

查看状态，也可以看到文件重命名。

```
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       renamed:    welcome.txt -> README
#

```

执行提交。

```
$ git commit -m "README is from welcome.txt."
[master c024f34] README is from welcome.txt.
 1 files changed, 1 insertions(+), 0 deletions(-)
 rename welcome.txt => README (73%)

```

这次提交中也看到了重命名操作，但是重命名相似度不是 100%，而是 73%。

## 2.7.5\. 一个显示版本号的`Hello World`

在本章的一开始为纪念前面的实践留了一个影，叫做`old_practice`。现在再次执行**git describe**看一下现在的版本号。

```
$ git describe
old_practice-3-gc024f34

```

就是说：当前工作区的版本是“留影”后的第三个版本，提交 ID 是`c024f34`。

下面的命令可以在提交日志中显示提交对应的里程碑（Tag）。其中参数`--decorate`可以在提交 ID 的旁边显示该提交关联的引用（里程碑或分支）。

```
$ git log --oneline --decorate -4
c024f34 (HEAD, master) README is from welcome.txt.
63992f0 restore file: welcome.txt
7161977 delete trash files. (using: git add -u)
2b31c19 (tag: old_practice) Merge commit 'acc2f69'

```

命令**git describe**的输出可以作为软件版本号，这个功能非常有用。因为这样可以很容易的实现将发布的软件包版本和版本库中的代码对应在一起，当发现软件包包含 Bug 时，可以最快、最准确的对应到代码上。

下面的`Hello World`程序就实现了这个功能。创建目录`src`，并在`src`目录下创建下面的三个文件：

*   文件：`src/main.c`

    没错，下面的几行就是这个程序的主代码，和输出相关代码的就两行，一行显示“Hello, world.”，另外一行显示软件版本。在显示软件版本时用到了宏`_VERSION`，这个宏的来源参考下一个文件。

    源代码：

    > ```
    > #include "version.h"
    > #include <stdio.h>
    > 
    > int
    > main()
    > {
    >     printf( "Hello, world.\n" );
    >     printf( "version: %s.\n", _VERSION );
    >     return 0;
    > }
    > 
    > ```

*   文件：`src/version.h.in`

    没错，这个文件名的后缀是`.h.in`。这个文件其实是用于生成文件`version.h`的模板文件。在由此模板文件生成的`version.h`的过程中，宏`_VERSION`的值 “<version>” 会动态替换。

    源代码：

    > ```
    > #ifndef HELLO_WORLD_VERSION_H
    > #define HELLO_WORLD_VERSION_H
    > 
    > #define _VERSION "<version>"
    > 
    > #endif
    > 
    > ```

*   文件：`src/Makefile`

    这个文件看起来很复杂，而且要注意所有缩进都是使用一个`<Tab>`键完成的缩进，千万不要错误的写成空格，因为这是`Makefile`。这个文件除了定义如何由代码生成可执行文件`hello`之外，还定义了如何将模板文件`version.h.in`转换为`version.h`。在转换过程中用**git describe**命令的输出替换模板文件中的`<version>`字符串。

    源代码：

    > ```
    > OBJECTS = main.o
    > TARGET = hello
    > 
    > all: $(TARGET)
    > 
    > $(TARGET): $(OBJECTS)
    >         $(CC) -o $@ $^
    > 
    > main.o: | new_header
    > main.o: version.h
    > 
    > new_header:
    >         @sed -e "s/<version>/$$(git describe)/g" \
    >                 < version.h.in > version.h.tmp
    >         @if diff -q version.h.tmp version.h >/dev/null 2>&1; \
    >         then \
    >                 rm version.h.tmp; \
    >         else \
    >                 echo "version.h.in => version.h" ; \
    >                 mv version.h.tmp version.h; \
    >         fi
    > 
    > clean:
    >         rm -f $(TARGET) $(OBJECTS) version.h
    > 
    > .PHONY: all clean
    > 
    > ```

上述三个文件创建完毕之后，进入到`src`目录，试着运行一下。先执行**make**编译，再运行编译后的程序**hello**。

```
$ cd src
$ make
version.h.in => version.h
cc    -c -o main.o main.c
cc -o hello main.o
$ ./hello
Hello, world.
version: old_practice-3-gc024f34.

```

## 2.7.6\. 使用**git add -i**选择性添加

刚刚创建的`Hello World`程序还没有添加到版本库中，在`src`目录下有下列文件：

```
$ cd /path/to/my/workspace/demo
$ ls src
hello  main.c  main.o  Makefile  version.h  version.h.in

```

这些文件中`hello`,`main.o`和`version.h`都是在编译时生成的程序，不应该加入到版本库中。那么选择性添加文件除了针对文件逐一使用**git add**命令外，还有什么办法么？通过使用`-i`参数调用**git add**就是一个办法，提供了一个交互式的界面。

执行**git add -i**命令，进入一个交互式界面，首先显示的是工作区状态。显然因为版本库进行了清理，所以显得很“干净”。

```
$ git add -i
           staged     unstaged path

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now>

```

在交互式界面显示了命令列表，可以使用数字或者加亮显示的命令首字母，选择相应的功能。对于此例需要将新文件加入到版本库，所以选择“4”。

```
What now> 4
  1: src/Makefile
  2: src/hello
  3: src/main.c
  4: src/main.o
  5: src/version.h
  6: src/version.h.in
Add untracked>>

```

当选择了“4”之后，就进入了“Add untracked”界面，显示了本地新增（尚不再版本库中）的文件列表，而且提示符也变了，由“What now>”变为“Add untracked>>”。依次输入 1、3、6 将源代码添加到版本库中。

*   输入“1”：

    ```
    Add untracked>> 1
    * 1: src/Makefile
      2: src/hello
      3: src/main.c
      4: src/main.o
      5: src/version.h
      6: src/version.h.in

    ```

*   输入“3”：

    ```
    Add untracked>> 3
    * 1: src/Makefile
      2: src/hello
    * 3: src/main.c
      4: src/main.o
      5: src/version.h
      6: src/version.h.in

    ```

*   输入“6”：

    ```
    Add untracked>> 6
    * 1: src/Makefile
      2: src/hello
    * 3: src/main.c
      4: src/main.o
      5: src/version.h
    * 6: src/version.h.in
    Add untracked>>

    ```

每次输入文件序号，对应的文件前面都添加一个星号，代表将此文件添加到暂存区。在提示符“Add untracked>>”处按回车键，完成文件添加，返回主界面。

```
Add untracked>>
added 3 paths

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now>

```

此时输入“1”查看状态，可以看到三个文件添加到暂存区中。

```
What now> 1
           staged     unstaged path
  1:       +20/-0      nothing src/Makefile
  2:       +10/-0      nothing src/main.c
  3:        +6/-0      nothing src/version.h.in

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help

```

输入“7”退出交互界面。

查看文件状态，可以发现三个文件被添加到暂存区中。

```
$ git status -s
A  src/Makefile
A  src/main.c
A  src/version.h.in
?? src/hello
?? src/main.o
?? src/version.h

```

完成提交。

```
$ git commit -m "Hello world initialized."
[master d71ce92] Hello world initialized.
 3 files changed, 36 insertions(+), 0 deletions(-)
 create mode 100644 src/Makefile
 create mode 100644 src/main.c
 create mode 100644 src/version.h.in

```

## 2.7.7\. `Hello world`引发的新问题

进入`src`目录中，对`Hello world`执行编译。

```
$ cd /path/to/my/workspace/demo/src
$ make clean && make
rm -f hello main.o version.h
version.h.in => version.h
cc    -c -o main.o main.c
cc -o hello main.o

```

运行编译后的程序，是不是对版本输出不满意呢？

```
$ ./hello
Hello, world.
version: old_practice-4-gd71ce92.

```

之所以显示长长的版本号，是因为使用了在本章最开始留的“影”。现在为`Hello world`留下一个新的“影”（一个新的里程碑）吧。

```
$ git tag -m "Set tag hello_1.0." hello_1.0

```

然后清除上次编译结果后，重新编译和运行，可以看到新的输出。

```
$ make clean && make
rm -f hello main.o version.h
version.h.in => version.h
cc    -c -o main.o main.c
cc -o hello main.o
$ ./hello
Hello, world.
version: hello_1.0.

```

还不错，显示了新的版本号。此时在工作区查看状态，会发现工作区“不干净”。

```
$ git status
# On branch master
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#       hello
#       main.o
#       version.h

```

编译的目标文件和以及从模板生成的头文件出现在了 Git 的状态输出中，这些文件会对以后的工作造成干扰。当写了新的源代码文件需要添加到版本库中时，因为这些干扰文件的存在，不得不一一将这些干扰文件排除在外。更为严重的是，如果不小心执行**git add .**或者**git add -A**命令会将编译的目标文件及其他临时文件加入版本库中，浪费存储空间不说甚至还会造成冲突。

Git 提供了文件忽略功能，可以解决这个问题。

## 2.7.8\. 文件忽略

Git 提供了文件忽略功能。当对工作区某个目录或者某些文件设置了忽略后，再执行**git status**查看状态时，被忽略的文件即使存在也不会显示为未跟踪状态，甚至根本感觉不到这些文件的存在。现在就针对`Hello world`程序目录试验一下。

```
$ cd /path/to/my/workspace/demo/src
$ git status -s
?? hello
?? main.o
?? version.h

```

可以看到`src`目录下编译的目标文件等显示为未跟踪，每一行开头的两个问号好像在向我们请求：“快把我们添加到版本库里吧”。

执行下面的命令可以在这个目下创建一个名为`.gitignore`的文件（注意文件的前面有个点），把这些要忽略的文件写在其中，文件名可以使用通配符。注意：第 2 行到第 5 行开头的右尖括号是**cat**命令的提示符，不是输入。

```
$ cat > .gitignore << EOF
> hello
> *.o
> *.h
> EOF

```

看看写好的`.gitignore`文件。每个要忽略的文件显示在一行。

```
$ cat .gitignore
hello
*.o
*.h

```

再来看看当前工作区的状态。

```
$ git status -s
?? .gitignore

```

把`.gitignore`文件添加到版本库中吧。（如果不希望添加到库里，也不希望`.gitignore`文件带来干扰，可以在忽略文件中忽略自己。）

```
$ git add .gitignore
$ git commit -m "ignore object files."
[master b3af728] ignore object files.
 1 files changed, 3 insertions(+), 0 deletions(-)
 create mode 100644 src/.gitignore

```

**:file:`.gitignore`文件可以放在任何目录**

文件`.gitignore`的作用范围是其所处的目录及其子目录，因此如果把刚刚创建的`.gitignore`移动到上一层目录（仍位于工作区内）也应该有效。

```
$ git mv .gitignore ..
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       renamed:    .gitignore -> ../.gitignore
#

```

果然移动`.gitignore`文件到上层目录，`Hello world`程序目录下的目标文件依然被忽略着。

提交。

```
$ git commit -m "move .gitignore outside also works."
[master 3488f2c] move .gitignore outside also works.
 1 files changed, 0 insertions(+), 0 deletions(-)
 rename src/.gitignore => .gitignore (100%)

```

**忽略文件有错误，后果很严重**

实际上面写的忽略文件不是非常好，为了忽略`version.h`，结果使用了通配符`*.h`会把源码目录下的有用的头文件也给忽略掉，导致应该添加到版本库的文件忘记添加。

在当前目录下创建一个新的头文件`hello.h`。

```
$ echo "/* test */" > hello.h

```

在工作区状态显示中看不到`hello.h`文件。

```
$ git status
# On branch master
nothing to commit (working directory clean)

```

只有使用了`--ignored`参数，才会在状态显示中看到被忽略的文件。

```
$ git status --ignored -s
!! hello
!! hello.h
!! main.o
!! version.h

```

要添加`hello.h`文件，使用**git add -A**和**git add .**都失效。无法用这两个命令将`hello.h`添加到暂存区中。

```
$ git add -A
$ git add .
$ git st -s

```

只有在添加操作的命令行中明确的写入文件名，并且提供`-f`参数才能真正添加。

```
$ git add -f hello.h
$ git commit -m "add hello.h"
[master 48456ab] add hello.h
 1 files changed, 1 insertions(+), 0 deletions(-)
 create mode 100644 src/hello.h

```

**忽略只对未跟踪文件有效，对于已加入版本库的文件无效**

文件`hello.h`添加到版本库后，就不再受到`.gitignore`设置的文件忽略影响了，对`hello.h`的修改都会立刻被跟踪到。这是因为 Git 的文件忽略只是对未入库的文件起作用。

```
$ echo "/* end */" >> hello.h
$ git status
# On branch master
# Changed but not updated:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#       modified:   hello.h
#
no changes added to commit (use "git add" and/or "git commit -a")

```

偷懒式提交。（使用了`-a`参数提交，不用预先执行**git add**命令。）

```
$ git commit -a -m "偷懒了，直接用 -a 参数直接提交。"
[master 613486c] 偷懒了，直接用 -a 参数直接提交。
 1 files changed, 1 insertions(+), 0 deletions(-)

```

**本地独享式忽略文件**

文件`.gitignore`设置的文件忽略是共享式的。之所以称其为“共享式”，是因为`.gitignore`被添加到版本库后成为了版本库的一部分，当版本库共享给他人（克隆）或者把版本库推送（PUSH）到集中式的服务器（或他人的版本库），这个忽略文件就会出现在他人的工作区中，文件忽略在他人的工作区中同样生效。

与“共享式”忽略对应的是“独享式”忽略。独享式忽略就是不会因为版本库共享或者版本库之间的推送传递给他人的文件忽略。独享式忽略有两种方式：

*   一种是针对具体版本库的“独享式”忽略。即在版本库`.git`目录下的一个文件`.git/info/exclude`来设置文件忽略。
*   另外一种是全局的“独享式”忽略。即通过 Git 的配置变量`core.excludesfile`指定的一个忽略文件，其设置的忽略对所有文件均有效。

至于哪些情况需要通过向版本库中提交`.gitignore`文件设置共享式的文件忽略，哪些情况通过`.git/info/exclude`设置只对本地有效的独享式文件忽略，这取决于要设置的文件忽略是否具有普遍意义。如果文件忽略对于所有使用此版本库工作的人都有益，就通过在版本库相应的目录下创建一个`.gitignore`文件建立忽略，否则如果是需要忽略工作区中创建的一个试验目录或者试验性的文件，则使用本地忽略。

例如我的本地就设置着一个全局的独享的文件忽略列表（这个文件名可以随意设置）：

```
$ git config --global core.excludesfile /home/jiangxin/_gitignore
$ git config core.excludesfile
/home/jiangxin/_gitignore

$ cat /home/jiangxin/_gitignore
*~        # vim 临时文件
*.pyc     # python 的编译文件
.*.mmx    # 不是正则表达式哦，因为 FreeMind-MMX 的辅助文件以点开头

```

**Git 忽略语法**

Git 的忽略文件的语法规则再多说几句。

*   忽略文件中的空行或者以井号（#）开始的行被忽略。
*   可以使用通配符，参见 Linux 手册：glob(7)。例如：星号（*）代表任意多字符，问号（?）代表一个字符，方括号（[abc]）代表可选字符范围等。
*   如果名称的最前面是一个路径分隔符（/），表明要忽略的文件在此目录下，而非子目录的文件。
*   如果名称的最后面是一个路径分隔符（/），表明要忽略的是整个目录，同名文件不忽略，否则同名的文件和目录都忽略。
*   通过在名称的最前面添加一个感叹号（!），代表不忽略。

下面的文件忽略示例，包含了上述要点：

```
# 这是注释行 —— 被忽略
*.a       # 忽略所有以 .a 为扩展名的文件。
!lib.a    # 但是 lib.a 文件或者目录不要忽略，即使前面设置了对 *.a 的忽略。
/TODO     # 只忽略根目录下的 TODO 文件，子目录的 TODO 文件不忽略。
build/    # 忽略所有 build/ 目录下的文件。
doc/*.txt # 忽略文件如 doc/notes.txt，但是文件如 doc/server/arch.txt 不被忽略。

```

## 2.7.9\. 文件归档

如果使用压缩工具（tar、7zip、winzip、rar 等）将工作区文件归档，一不小心会把版本库（`.git`目录）包含其中，甚至将工作区中的忽略文件、临时文件也包含其中。Git 提供了一个归档命令：**git archive**，可以对任意提交对应的目录树建立归档。示例如下：

*   基于最新提交建立归档文件`latest.zip`。

    ```
    $ git archive -o latest.zip HEAD

    ```

*   只将目录`src`和`doc`建立到归档`partial.tar`中。

    ```
    $ git archive -o partial.tar  HEAD src doc

    ```

*   基于里程碑 v1.0 建立归档，并且为归档中文件添加目录前缀 1.0。

    ```
    $ git archive --format=tar --prefix=1.0/ v1.0 | gzip > foo-1.0.tar.gz

    ```

在建立归档时，如果使用树对象 ID 进行归档，则使用当前时间作为归档中文件的修改时间，而如果使用提交 ID 或里程碑等，则使用提交建立的时间作为归档中文件的修改时间。

如果使用 tar 格式建立归档，并且使用提交 ID 或里程碑 ID，还会把提交 ID 记录在归档文件的文件头中。记录在文件头中的提交 ID 可以通过**git tar-commit-id**命令获取。

如果希望在建立归档时忽略某些文件或目录，可以通过为相应文件或目录建立`export-ignore`属性加以实现。具体参见本书第 8 篇第四十一章“41.1 属性”一节。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 2.8\. 历史穿梭

经过了之前众多的实践，版本库中已经积累了很多次提交了，从下面的命令可以看出来有 14 次提交。

```
$ git rev-list HEAD | wc -l
14

```

有很多工具可以研究和分析 Git 的历史提交，在前面的实践中已经用过很多相关的 Git 命令进行查看历史提交、查看文件的历史版本、进行差异比较等。本章除了对之前用到的相关 Git 命令作以总结外，还要再介绍几款图形化的客户端。

## 2.8.1\. 图形工具：gitk

gitk 是最早实现的一个图形化的 Git 版本库浏览器软件，基于 tcl/tk 实现，因此 gitk 非常简洁，本身就是一个 1 万多行的 tcl 脚本写成的。gitk 的代码已经和 Git 的代码放在同一个版本库中，gitk 随 Git 一同发布，不用特别的安装即可运行。gitk 可以显示提交的分支图，可以显示提交，文件，版本间差异等。

在版本库中调用 gitk，就会浏览该版本库，显示其提交分支图。gitk 可以像命令行工具一样使用不同的参数进行调用。

*   显示所有的分支。

    ```
    $ gitk --all

    ```

*   显示 2 周以来的提交。

    ```
    $ gitk --since="2 weeks ago"

    ```

*   显示某个里程碑（v2.6.12）以来，针对某些目录和文件（`include/scsi`目录和`drivers/scsi`目录）的提交。

    ```
    $ gitk v2.6.12.. include/scsi drivers/scsi

    ```

下面的图示就是在 DEMO 版本库中运行**gitk --all**的显示。

![../_images/gitk.png](img/gitk.png)

在上图中可见不同颜色和形状区分的引用：

*   绿色的`master`分支。
*   黄色的`hello_1.0`和`old_practice`里程碑。
*   灰色的`stash`。

gitk 使用 tcl/tk 开发，在显示上没有系统中原生图形应用那么漂亮的界面，甚至可以用丑陋来形容，下面介绍的 gitg 和 qgit 在易用性上比 gitk 进步了不少。

## 2.8.2\. 图形工具：gitg

`gitg`是使用 GTK+图形库实现的一个 Git 版本库浏览器软件。Linux 下最著名的 Gnome 桌面环境使用的就是 GTK+，因此在 Linux 下`gitg`有着非常漂亮的原生的图形界面。`gitg`不但能够实现 gitk 的全部功能，即浏览提交历史和文件，还能帮助执行提交。

在 Linux 上安装 gitg 很简单，例如在 Debian 或 Ubuntu 上，直接运行下面的命令就可以进行安装。

```
$ sudo aptitude install gitg

```

安装完毕就可以在可执行路径中找到`gitg`。

```
$ which gitg
/usr/bin/gitg

```

为了演示`gitg`具备提交功能，先在工作区作出一些修改。

*   删除没有用到的`hello.h`文件。

    ```
    $ cd /path/to/my/workspace/demo
    $ rm src/hello.h

    ```

*   在`README`文件后面追加一行。

    ```
    $ echo "Wait..." >> README

    ```

*   当前工作区的状态。

    ```
    $ git status -s
     M README
     D src/hello.h

    ```

现在可以在工作区下执行`gitg`命令。

```
$ gitg &

```

下图就是`gitg`的缺省界面，显示了提交分支图，以及选中提交的提交信息和变更文件列表等。

> ![../_images/gitg-history.png](img/gitg-history.png)

在上图中可以看见用不同颜色的标签显示的状态标识（包括引用）：

*   橙色的`master`分支。
*   黄色的`hello_1.0`和`old_practice`里程碑。
*   粉色的`stash`标签。
*   以及白色的显示工作区非暂存状态的标签。

点击`gitg`下方窗口的标签“tree”，会显示此提交的目录树。

> ![../_images/gitg-tree.png](img/gitg-tree.png)

提交功能是`gitg`的一大特色。点击`gitg`顶部窗口的`commit`标签，显示下面的界面。

> ![../_images/gitg-commit-1-all-unstaged.png](img/gitg-commit-1-all-unstaged.png)

左下方窗口显示的是未更新到暂存区的本地改动。鼠标右击，在弹出菜单中选择“Stage”。

> ![../_images/gitg-commit-2-add-stage.png](img/gitg-commit-2-add-stage.png)

当把文件`README`添加到暂存区后，可以看到`README`文件出现在右下方的窗口中。

> ![../_images/gitg-commit-3-mixed-stage-unstage.png](img/gitg-commit-3-mixed-stage-unstage.png)

此时如果回到提交历史查看界面，可以看到在“stash”标签的下方，同时出现了“staged”和“unstaged”两个标签分别表示暂存区和工作区的状态。

> ![../_images/gitg-commit-4-history-stage-unstage.png](img/gitg-commit-4-history-stage-unstage.png)

当通过`gitg`的界面选择好要提交的文件（加入暂存区）之后，执行提交。

> ![../_images/gitg-commit-5-commit.png](img/gitg-commit-5-commit.png)

上图的提交说明对话框的下方有两个选项，当选择了“Add signed-off-by”选项后，在提交日志中会自动增加相应的说明文字。下图可以看到刚刚的提交已经显示在提交历史的最顶端，在提交说明中出现了`Signed-off-by`文字说明。

> ![../_images/gitg-commit-6-new-history.png](img/gitg-commit-6-new-history.png)

`gitg`还是一个比较新的项目，在本文撰写的时候，`gitg`才是 0.0.6 版本，相比下面要介绍的`qgit`还缺乏很多功能。例如`gitg`没有文件的 blame（追溯）界面，也不能直接将文件检出，但是`gitg`整体的界面风格，以及易用的提交界面给人的印象非常深刻。

## 2.8.3\. 图形工具：qgit

前面介绍的`gitg`是基于 GTK+这一 Linux 标准的图形库，那么也许有读者已经猜到`qgit`是使用 Linux 另外一个著名的图形库 QT 实现的 Git 版本库浏览器软件。QT 的知名度不亚于 GTK+，是著名的 KDE 桌面环境用到的图形库，也是蓄势待发准备和 Android 一较高低的 MeeGo 的 UI 核心。`qgit`目前的版本是 2.3，相比前面介绍的`gitg`其经历的开发周期要长了不少，因此也提供了更多的功能。

在 Linux 上安装`qgit`很简单，例如在 Debian 或 Ubuntu 上，直接运行下面的命令就可以进行安装。

```
$ sudo aptitude install qgit

```

安装完毕就可以在可执行路径中找到`qgit`。

```
$ which qgit
/usr/bin/qgit

```

`qgit`和`gitg`一样不但能够浏览提交历史和文件，还能帮助执行提交。为了测试提交，将在上一节所做的提交回滚。

*   使用重置命令回滚最后一次提交。

    ```
    $ git reset HEAD^
    Unstaged changes after reset:
    M       README
    M       src/hello.h

    ```

*   当前工作区的状态。

    ```
    $ git status
    # On branch master
    # Changed but not updated:
    #   (use "git add/rm <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #       modified:   README
    #       deleted:    src/hello.h
    #
    no changes added to commit (use "git add" and/or "git commit -a")

    ```

现在可以在工作区下执行`qgit`命令。

```
$ qgit &

```

启动`qgit`，首先弹出一个对话框，提示对显示的提交范围和分支范围进行选择。

> ![../_images/qgit-splash-select.png](img/qgit-splash-select.png)

对所有的选择打钩，显示下面的`qgit`的缺省界面。其中包括了提交分支图，以及选中提交的提交信息和变更文件列表等。

> ![../_images/qgit-history.png](img/qgit-history.png)

在上图中可以看见用不同颜色的标签显示的状态标识（包括引用）：

*   绿色的`master`分支。
*   黄色的`hello_1.0`和`old_practice`里程碑。
*   灰色的`stash`标签，显示在了创建时候的位置，并其包含的针对暂存区状态的提交也显示出来。
*   最顶端显示一行绿色背景的文件：工作区有改动。

`qgit`的右键菜单非常丰富，上图显示了鼠标右击提交时显示的弹出菜单，可以创建、切换标签或分支，可以将提交导出为补丁文件。

点击`qgit`右下方变更文件列表窗口，可以选择将文件检出或者直接查看。

> ![../_images/qgit-changefiles.png](img/qgit-changefiles.png)

要想显示目录树，键入大写字母`T`，或者鼠标单击工具条上的图标![QGIT-TREE-TOGGLE](img/icon-11-12-qgit-tree-toggle.png)，就会在左侧显示目录树窗口，如下。

> ![../_images/qgit-tree-view.png](img/qgit-tree-view.png)

从上图也可以看到目录树的文件包含的右键菜单。当选择查看一个文件时，会显示此文件的追溯，即显示每一行是在哪个版本由谁修改的。追溯窗口见下图右下方窗口。

> ![../_images/qgit-blame.png](img/qgit-blame.png)

`qgit`也可以执行提交。选中`qgit`顶部窗口最上一行“Working dir changes”，鼠标右击，显示的弹出菜单包含了“Commit...”选项。

> ![../_images/qgit-commit-1-revlist.png](img/qgit-commit-1-revlist.png)

点击弹出菜单中的“Commit...”，显示下面的对话框。

> ![../_images/qgit-commit-2-dialog-unstaged.png](img/qgit-commit-2-dialog-unstaged.png)

自动选中了所有的文件。上方窗口的选中文件目前状态是“Not updated in index”，就是说尚未添加到暂存区。

使用`qgit`做提交，只要选择好要提交的文件列表，即使未添加到暂存区，也可以直接提交。在下方的提交窗口写入提交日志，点击“Commit”按钮开始提交。

> ![../_images/qgit-commit-3-commit-unstaged.png](img/qgit-commit-3-commit-unstaged.png)

提交完毕返回`qgit`主界面，在显示的提交列表的最上方，原来显示的“Working dir changes”已经更新为“Nothing to commit”，并且可以看到刚刚的提交已经显示在提交历史的最顶端。

> ![../_images/qgit-commit-4-revlist.png](img/qgit-commit-4-revlist.png)

## 2.8.4\. 命令行工具

上面介绍的几款图形界面的 Git 版本库浏览器最大的特色就是更好看的提交关系图，还能非常方便的浏览历史提交的目录树，并从历史提交的目录树中提取文件等。这些操作对于 Git 命令行同样可以完成。使用 Git 命令行探索版本库历史对于读者来说并不新鲜，因为在前几章的实践中已经用到了相关命令，展示了对历史记录的操作。本节对这些命令的部分要点进行强调和补充。

前面历次实践的提交基本上是线性的提交，研究起来没有挑战性。为了能够更加接近于实际又不失简洁，我构造了一个版本库，放在了 Github 上。可以通过如下操作在本地克隆这个示例版本库。

```
$ cd /path/to/my/workspace/
$ git clone git://github.com/ossxp-com/gitdemo-commit-tree.git
Cloning into gitdemo-commit-tree...
remote: Counting objects: 63, done.
remote: Compressing objects: 100% (51/51), done.
remote: Total 63 (delta 8), reused 0 (delta 0)
Receiving objects: 100% (63/63), 65.95 KiB, done.
Resolving deltas: 100% (8/8), done.
$ cd gitdemo-commit-tree

```

运行`gitg`命令，显示其提交关系图。

![../_images/gitg-demo-commit-tree.png](img/gitg-demo-commit-tree.png)

是不是有点“乱花渐欲迷人眼”的感觉。如果把提交用里程碑标识的圆圈来代表，稍加排列就会看到下面的更为直白的提交关系图。

![../_images/commit-tree.png](img/commit-tree.png)

Git 的大部分命令可以使用提交版本作为参数（如：**git diff <commit-id>**），有的命令则使用一个版本范围作为参数（如：**git log <rev1>..<rev2>**）。Git 的提交有着各式各样的表示法，提交范围也是一样，下面就通过两个命令**git rev-parse**和**git rev-list**分别研究一下 Git 的版本表示法和版本范围表示法。

### 2.8.4.1\. 版本表示法：**git rev-parse**

命令**git rev-parse**是 Git 的一个底层命令，其功能非常丰富（或者说杂乱），很多 Git 脚本或工具都会用到这条命令。

此命令的部分应用在“Git 初始化”章节中就已经看到。例如可以显示 Git 版本库的位置（`--git-dir`），当前工作区目录的深度（`--show-cdup`），甚至可以用于被 Git 无关应用用于解析命令行参数（`--parseopt`）。

此命令可以显示当前版本库中的引用。

*   显示分支。

    ```
    $ git rev-parse --symbolic --branches

    ```

*   显示里程碑。

    ```
    $ git rev-parse --symbolic --tags
    A
    B
    C
    D
    E
    F
    G
    H
    I
    J

    ```

*   显示定义的所有引用。

    其中`refs/remotes/`目录下的引用成为远程分支（或远程引用），在后面的章节会予以介绍。

    ```
    $ git rev-parse --symbolic --glob=refs/*
    refs/heads/master
    refs/remotes/origin/HEAD
    refs/remotes/origin/master
    refs/tags/A
    refs/tags/B
    refs/tags/C
    refs/tags/D
    refs/tags/E
    refs/tags/F
    refs/tags/G
    refs/tags/H
    refs/tags/I
    refs/tags/J

    ```

命令**git rev-parse**另外一个重要的功能就是将一个 Git 对象表达式表示为对应的 SHA1 哈希值。针对本节开始克隆的版本库`gitdemo-commit-tree`，做如下操作。

*   显示 HEAD 对应的 SHA1 哈希值。

    ```
    $ git rev-parse  HEAD
    6652a0dce6a5067732c00ef0a220810a7230655e

    ```

*   命令**git describe**的输出也可以显示为 SHA1 哈希值。

    ```
    $ git describe
    A-1-g6652a0d
    $ git rev-parse A-1-g6652a0d
    6652a0dce6a5067732c00ef0a220810a7230655e

    ```

*   可以同时显示多个表达式的 SHA1 哈希值。

    下面的操作可以看出 master 和 refs/heads/master 都可以用于指代 master 分支。

    ```
    $ git rev-parse  master  refs/heads/master
    6652a0dce6a5067732c00ef0a220810a7230655e
    6652a0dce6a5067732c00ef0a220810a7230655e

    ```

*   可以用哈希值的前几位指代整个哈希值。

    ```
    $ git rev-parse  6652  6652a0d
    6652a0dce6a5067732c00ef0a220810a7230655e
    6652a0dce6a5067732c00ef0a220810a7230655e

    ```

*   里程碑的两种表示法均指向相同的对象。

    里程碑对象不一定是提交，有可能是一个 Tag 对象。Tag 对象包含说明或者签名，还包括到对应提交的指向。

    ```
    $ git rev-parse  A  refs/tags/A
    c9b03a208288aebdbfe8d84aeb984952a16da3f2
    c9b03a208288aebdbfe8d84aeb984952a16da3f2

    ```

*   里程碑 A 指向了一个 Tag 对象而非提交的时候，用下面的三个表示法都可以指向里程碑对应的提交。

    实际上下面的语法也可以直接作用于轻量级里程碑（直接指向提交的里程碑）或者作用于提交本身。

    ```
    $ git rev-parse  A^{}  A⁰  A^{commit}
    81993234fc12a325d303eccea20f6fd629412712
    81993234fc12a325d303eccea20f6fd629412712
    81993234fc12a325d303eccea20f6fd629412712

    ```

*   `A`的第一个父提交就是`B`所指向的提交。

    回忆之前的介绍，`^`操作符代表着父提交。当一个提交有多个父提交时，可以通过在符号`^`后面跟上一个数字表示第几个父提交。`A^` 就相当于 `A¹`。而`B⁰`代表了`B`所指向的一个 Commit 对象（因为`B`是 Tag 对象）。

    ```
    $ git rev-parse  A^  A¹  B⁰
    776c5c9da9dcbb7e463c061d965ea47e73853b6e
    776c5c9da9dcbb7e463c061d965ea47e73853b6e
    776c5c9da9dcbb7e463c061d965ea47e73853b6e

    ```

*   更为复杂的表示法。

    连续的`^`符号依次沿着父提交进行定位至某一祖先提交。`^`后面的数字代表该提交的第几个父提交。

    ```
    $ git rev-parse  A^³²  F²  J^{}
    3252fcce40949a4a622a1ac012cb120d6b340ac8
    3252fcce40949a4a622a1ac012cb120d6b340ac8
    3252fcce40949a4a622a1ac012cb120d6b340ac8

    ```

*   记号`~<n>`就相当于连续<n>个符号`^`。

    ```
    $ git rev-parse  A~3  A^^^  G⁰
    e80aa7481beda65ae00e35afc4bc4b171f9b0ebf
    e80aa7481beda65ae00e35afc4bc4b171f9b0ebf
    e80aa7481beda65ae00e35afc4bc4b171f9b0ebf

    ```

*   显示里程碑 A 对应的目录树。下面两种写法都可以。

    ```
    $ git rev-parse  A^{tree}  A:
    95ab9e7db14ca113d5548dc20a4872950e8e08c0
    95ab9e7db14ca113d5548dc20a4872950e8e08c0

    ```

*   显示树里面的文件，下面两种表示法均可。

    ```
    $ git rev-parse  A^{tree}:src/Makefile  A:src/Makefile
    96554c5d4590dbde28183e9a6a3199d526eeb925
    96554c5d4590dbde28183e9a6a3199d526eeb925

    ```

*   暂存区里的文件和 HEAD 中的文件相同。

    ```
    $ git rev-parse  :gitg.png  HEAD:gitg.png
    fc58966ccc1e5af24c2c9746196550241bc01c50
    fc58966ccc1e5af24c2c9746196550241bc01c50

    ```

*   还可以通过在提交日志中查找字串的方式显示提交。

    ```
    $ git rev-parse :/"Commit A"
    81993234fc12a325d303eccea20f6fd629412712

    ```

*   再有就是 reflog 相关的语法，参见“Git 重置”章节中关于 reflog 的介绍。

    ```
    $ git rev-parse HEAD@{0} master@{0}
    6652a0dce6a5067732c00ef0a220810a7230655e
    6652a0dce6a5067732c00ef0a220810a7230655e

    ```

### 2.8.4.2\. 版本范围表示法：git rev-list

有的 Git 命令可以使用一个版本范围作为参数，命令**git rev-list**可以帮助研究 Git 的各种版本范围语法。

![../_images/commit-tree-with-id.png](img/commit-tree-with-id.png)

*   一个提交 ID 实际上就可以代表一个版本列表。含义是：该版本开始的所有历史提交。

    ```
    $ git rev-list --oneline  A
    8199323 Commit A: merge B with C.
    0cd7f2e commit C.
    776c5c9 Commit B: merge D with E and F
    beb30ca Commit F: merge I with J
    212efce Commit D: merge G with H
    634836c commit I.
    3252fcc commit J.
    83be369 commit E.
    2ab52ad commit H.
    e80aa74 commit G.

    ```

*   两个或多个版本，相当于每个版本单独使用时指代的列表的并集。

    ```
    $ git rev-list --oneline  D  F
    beb30ca Commit F: merge I with J
    212efce Commit D: merge G with H
    634836c commit I.
    3252fcc commit J.
    2ab52ad commit H.
    e80aa74 commit G.

    ```

*   在一个版本前面加上符号（`^`）含义是取反，即排除这个版本及其历史版本。

    ```
    $ git rev-list --oneline  ^G D
    212efce Commit D: merge G with H
    2ab52ad commit H.

    ```

*   和上面等价的“点点”表示法。使用两个点连接两个版本，如`G..D`，就相当于`^G D`。

    ```
    $ git rev-list --oneline  G..D
    212efce Commit D: merge G with H
    2ab52ad commit H.

    ```

*   版本取反，参数的顺序不重要，但是“点点”表示法前后的版本顺序很重要。

    *   语法：`^B C`

        ```
  $ git rev-list --oneline  ^B C
        0cd7f2e commit C.

        ```

    *   语法：`C ^B`

        ```
  $ git rev-list --oneline  C ^B
        0cd7f2e commit C.

        ```

    *   语法：`B..C`相当于`^B C`

        ```
  $ git rev-list --oneline  B..C
        0cd7f2e commit C.

        ```

    *   语法：`C..B`相当于`^C B`

        ```
  $ git rev-list --oneline  C..B
        776c5c9 Commit B: merge D with E and F
        212efce Commit D: merge G with H
        83be369 commit E.
        2ab52ad commit H.
        e80aa74 commit G.

        ```

*   三点表示法的含义是两个版本共同能够访问到的除外。

    B 和 C 共同能够访问到的 F、I、J 排除在外。

    ```
    $ git rev-list --oneline  B...C
    0cd7f2e commit C.
    776c5c9 Commit B: merge D with E and F
    212efce Commit D: merge G with H
    83be369 commit E.
    2ab52ad commit H.
    e80aa74 commit G.

    ```

*   三点表示法，两个版本的前后顺序没有关系。

    实际上`r1...r2`相当于`r1 r2 --not $(git merge-base --all r1 r2)`，和顺序无关。

    ```
    $ git rev-list --oneline  C...B
    0cd7f2e commit C.
    776c5c9 Commit B: merge D with E and F
    212efce Commit D: merge G with H
    83be369 commit E.
    2ab52ad commit H.
    e80aa74 commit G.

    ```

*   某提交的历史提交，自身除外，用语法`r1^@`表示。

    ```
    $ git rev-list --oneline  B^@
    beb30ca Commit F: merge I with J
    212efce Commit D: merge G with H
    634836c commit I.
    3252fcc commit J.
    83be369 commit E.
    2ab52ad commit H.
    e80aa74 commit G.

    ```

*   提交本身不包括其历史提交，用语法`r1^!`表示。

    ```
    $ git rev-list --oneline  B^!
    776c5c9 Commit B: merge D with E and F

    $ git rev-list --oneline  F^! D
    beb30ca Commit F: merge I with J
    212efce Commit D: merge G with H
    2ab52ad commit H.

    ```

### 2.8.4.3\. 浏览日志：**git log**

命令**git log**是老朋友了，在前面的章节中曾经大量的出现，用于显示提交历史。

**参数代表版本范围**

当不使用任何参数调用，相当于使用了缺省的参数 HEAD，即显示当前 HEAD 能够访问到的所有历史提交。还可以使用上面介绍的版本范围表示法，例如：

```
$ git log --oneline F^! D
beb30ca Commit F: merge I with J
212efce Commit D: merge G with H
2ab52ad commit H.
e80aa74 commit G.

```

**分支图显示**

通过`--graph`参数调用**git log**可以显示字符界面的提交关系图，而且不同的分支还可以用不同的颜色来表示。如果希望每次查看日志的时候都看到提交关系图，可以设置一个别名，用别名来调用。

```
$ git config --global alias.glog "log --graph"

```

定义别名之后，每次希望自动显示提交关系图，就可以使用别名命令：

```
$ git glog --oneline
* 6652a0d Add Images for git treeview.
*   8199323 Commit A: merge B with C.
|\
| * 0cd7f2e commit C.
| |
|  \
*-. \   776c5c9 Commit B: merge D with E and F
|\ \ \
| | |/
| | *   beb30ca Commit F: merge I with J
| | |\
| | | * 3252fcc commit J.
| | * 634836c commit I.
| * 83be369 commit E.
*   212efce Commit D: merge G with H
|\
| * 2ab52ad commit H.
* e80aa74 commit G.

```

**显示最近的几条日志**

可以使用参数`-<n>`（<n>为数字），显示最近的<n>条日志。例如下面的命令显示最近的 3 条日志。

```
$ git log -3 --pretty=oneline
6652a0dce6a5067732c00ef0a220810a7230655e Add Images for git treeview.
81993234fc12a325d303eccea20f6fd629412712 Commit A: merge B with C.
0cd7f2ea245d90d414e502467ac749f36aa32cc4 commit C.

```

**显示每次提交的具体改动**

使用参数`-p`可以在显示日志的时候同时显示改动。

```
$ git log -p -1
commit 6652a0dce6a5067732c00ef0a220810a7230655e
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Thu Dec 9 16:07:11 2010 +0800

    Add Images for git treeview.

    Signed-off-by: Jiang Xin <jiangxin@ossxp.com>

diff --git a/gitg.png b/gitg.png
new file mode 100644
index 0000000..fc58966
Binary files /dev/null and b/gitg.png differ
diff --git a/treeview.png b/treeview.png
new file mode 100644
index 0000000..a756d12
Binary files /dev/null and b/treeview.png differ

```

因为是二进制文件改动，缺省不显示改动的内容。实际上 Git 的差异文件提供对二进制文件的支持，在后面“Git 应用”章节予以专题介绍。

**显示每次提交的变更概要**

使用`-p`参数会让日志输出显得非常冗余，当不需要知道具体的改动而只想知道改动在哪些文件上，可以使用`--stat`参数。输出的变更概要像极了 Linux 的**diffstat**命令的输出。

```
$ git log --stat --oneline  I..C
0cd7f2e commit C.
 README    |    1 +
 doc/C.txt |    1 +
 2 files changed, 2 insertions(+), 0 deletions(-)
beb30ca Commit F: merge I with J
3252fcc commit J.
 README           |    7 +++++++
 doc/J.txt        |    1 +
 src/.gitignore   |    3 +++
 src/Makefile     |   27 +++++++++++++++++++++++++++
 src/main.c       |   10 ++++++++++
 src/version.h.in |    6 ++++++
 6 files changed, 54 insertions(+), 0 deletions(-)

```

**定制输出**

Git 的差异输出命令提供了很多输出模板提供选择，可以根据需要选择冗余显示或者精简显示。

*   参数`--pretty=raw`显示提交的原始数据。可以显示提交对应的树 ID。

    ```
    $ git log --pretty=raw -1
    commit 6652a0dce6a5067732c00ef0a220810a7230655e
    tree e33be9e8e7ca5f887c7d5601054f2f510e6744b8
    parent 81993234fc12a325d303eccea20f6fd629412712
    author Jiang Xin <jiangxin@ossxp.com> 1291882031 +0800
    committer Jiang Xin <jiangxin@ossxp.com> 1291882892 +0800

        Add Images for git treeview.

        Signed-off-by: Jiang Xin <jiangxin@ossxp.com>

    ```

*   参数`--pretty=fuller`会同时显示作者和提交者，两者可以不同。

    ```
    $ git log --pretty=fuller -1
    commit 6652a0dce6a5067732c00ef0a220810a7230655e
    Author:     Jiang Xin <jiangxin@ossxp.com>
    AuthorDate: Thu Dec 9 16:07:11 2010 +0800
    Commit:     Jiang Xin <jiangxin@ossxp.com>
    CommitDate: Thu Dec 9 16:21:32 2010 +0800

        Add Images for git treeview.

        Signed-off-by: Jiang Xin <jiangxin@ossxp.com>

    ```

*   参数`--pretty=oneline`显然会提供最精简的日志输出。也可以使用`--oneline`参数，效果近似。

    ```
    $ git log --pretty=oneline -1
    6652a0dce6a5067732c00ef0a220810a7230655e Add Images for git treeview.

    ```

如果只想查看、分析某一个提交，也可以使用**git show**或者**git cat-file**命令。

*   使用**git show**显示里程碑 D 及其提交：

    ```
    $ git show D --stat
    tag D
    Tagger: Jiang Xin <jiangxin@ossxp.com>
    Date:   Thu Dec 9 14:24:52 2010 +0800

    create node D

    commit 212efce1548795a1edb08e3708a50989fcd73cce
    Merge: e80aa74 2ab52ad
    Author: Jiang Xin <jiangxin@ossxp.com>
    Date:   Thu Dec 9 14:06:34 2010 +0800

        Commit D: merge G with H

        Signed-off-by: Jiang Xin <jiangxin@ossxp.com>

     README    |    2 ++
     doc/D.txt |    1 +
     doc/H.txt |    1 +
     3 files changed, 4 insertions(+), 0 deletions(-)

    ```

*   使用**git cat-file**显示里程碑 D 及其提交。

    参数`-p`的含义是美观的输出（pretty）。

    ```
    $ git cat-file -p D⁰
    tree 1c22e90c6bf150ee1cde6cefb476abbb921f491f
    parent e80aa7481beda65ae00e35afc4bc4b171f9b0ebf
    parent 2ab52ad2a30570109e71b56fa1780f0442059b3c
    author Jiang Xin <jiangxin@ossxp.com> 1291874794 +0800
    committer Jiang Xin <jiangxin@ossxp.com> 1291875877 +0800

    Commit D: merge G with H

    Signed-off-by: Jiang Xin <jiangxin@ossxp.com>

    ```

### 2.8.4.4\. 差异比较：**git diff**

Git 差异比较功能在前面的实践中也反复的接触过了，尤其是在介绍暂存区的相关章节重点介绍了**git diff**命令如何对工作区、暂存区、版本库进行比较。

*   比较里程碑 B 和里程碑 A，用命令：**git diff B A**
*   比较工作区和里程碑 A，用命令：**git diff A**
*   比较暂存区和里程碑 A，用命令：**git diff --cached A**
*   比较工作区和暂存区，用命令：**git diff**
*   比较暂存区和 HEAD，用命令：**git diff --cached**
*   比较工作区和 HEAD，用命令：**git diff HEAD**

**Git 中文件在版本间的差异比较**

差异比较还可以使用路径参数，只显示不同版本间该路径下文件的差异。语法格式：

```
$ git diff <commit1> <commit2> -- <paths>

```

**非 Git 目录/文件的差异比较**

命令**git diff**还可以在 Git 版本库之外执行，对非 Git 目录进行比较，就像 GNU 的**diff**命令一样。之所以提供这个功能是因为 Git 差异比较命令更为强大，提供了对 GNU 差异比较的扩展支持。

```
$ git diff <path1> <path2>

```

**扩展的差异语法**

Git 扩展了 GNU 的差异比较语法，提供了对重命名、二进制文件、文件权限变更的支持。在后面的“Git 应用”辟专题介绍二进制文件的差异比较和补丁的应用。

**逐词比较，而非缺省的逐行比较**

Git 的差异比较缺省是逐行比较，分别显示改动前的行和改动后的行，到底改动哪里还需要仔细辨别。Git 还提供一种逐词比较的输出，有的人会更喜欢。使用`--word-diff`参数可以显示逐词比较。

```
$ git diff --word-diff
diff --git a/src/book/02-use-git/080-git-history-travel.rst b/src/book/02-use-git/080-git-history-travel.rst
index f740203..2dd3e6f 100644
--- a/src/book/02-use-git/080-git-history-travel.rst
+++ b/src/book/02-use-git/080-git-history-travel.rst
@@ -681,7 +681,7 @@ Git 的大部分命令可以使用提交版本作为参数（如：git diff），

::

  [-18:23:48 jiangxin@hp:~/gitwork/gitbook/src/book$-]{+$+} git log --stat --oneline  I..C
  0cd7f2e commit C.
   README    |    1 +
   doc/C.txt |    1 +

```

上面的逐词差异显示是有颜色显示的：删除内容`[-...-]`用红色表示，添加的内容`{+...+}`用绿色表示。

### 2.8.4.5\. 文件追溯：**git blame**

在软件开发过程中当发现 Bug 并定位到具体的代码时，Git 的文件追溯命令可以指出是谁在什么时候，什么版本引入的此 Bug。

当针对文件执行**git blame**命令，就会逐行显示文件，在每一行的行首显示此行最早是在什么版本引入的，由谁引入。

```
$ cd /path/to/my/workspace/gitdemo-commit-tree
$ git blame README
^e80aa74 (Jiang Xin 2010-12-09 14:00:33 +0800  1) DEMO program for git-scm-book.
^e80aa74 (Jiang Xin 2010-12-09 14:00:33 +0800  2)
^e80aa74 (Jiang Xin 2010-12-09 14:00:33 +0800  3) Changes
^e80aa74 (Jiang Xin 2010-12-09 14:00:33 +0800  4) =======
^e80aa74 (Jiang Xin 2010-12-09 14:00:33 +0800  5)
81993234 (Jiang Xin 2010-12-09 14:30:15 +0800  6) * create node A.
0cd7f2ea (Jiang Xin 2010-12-09 14:29:09 +0800  7) * create node C.
776c5c9d (Jiang Xin 2010-12-09 14:27:31 +0800  8) * create node B.
beb30ca7 (Jiang Xin 2010-12-09 14:11:01 +0800  9) * create node F.
³²⁵²fcc (Jiang Xin 2010-12-09 14:00:33 +0800 10) * create node J.
⁶³⁴⁸³⁶c (Jiang Xin 2010-12-09 14:00:33 +0800 11) * create node I.
⁸³be369 (Jiang Xin 2010-12-09 14:00:33 +0800 12) * create node E.
212efce1 (Jiang Xin 2010-12-09 14:06:34 +0800 13) * create node D.
²ab52ad (Jiang Xin 2010-12-09 14:00:33 +0800 14) * create node H.
^e80aa74 (Jiang Xin 2010-12-09 14:00:33 +0800 15) * create node G.
^e80aa74 (Jiang Xin 2010-12-09 14:00:33 +0800 16) * initialized.

```

只想查看某几行，使用`-L n,m`参数，如下：

```
$ git blame -L 6,+5 README
81993234 (Jiang Xin 2010-12-09 14:30:15 +0800  6) * create node A.
0cd7f2ea (Jiang Xin 2010-12-09 14:29:09 +0800  7) * create node C.
776c5c9d (Jiang Xin 2010-12-09 14:27:31 +0800  8) * create node B.
beb30ca7 (Jiang Xin 2010-12-09 14:11:01 +0800  9) * create node F.
³²⁵²fcc (Jiang Xin 2010-12-09 14:00:33 +0800 10) * create node J.

```

### 2.8.4.6\. 二分查找：**git bisect**

前面的文件追溯是建立在问题（Bug）已经定位（到代码上）的基础之上，然后才能通过错误的行（代码）找到人（提交者），打板子（教育或惩罚）。那么如何定位问题呢？Git 的二分查找命令可以提供帮助。

二分查找并不神秘，也不是万灵药，是建立在测试的基础之上的。实际上每个进行过软件测试的人都曾经使用过：“最新的版本出现 Bug 了，但是在给某某客户的版本却没有这个问题，所以问题肯定出在两者之间的某次代码提交上”。

Git 提供的**git bisect**命令是基于版本库的，自动化的问题查找和定位工作流程。取代传统软件测试中粗放式的、针对软件发布版本的、无法定位到代码的测试。

执行二分查找，在发现问题后，首先要找到一个正确的版本，如果所发现的问题从软件最早的版本就是错的，那么就没有必要执行二分查找了，还是老老实实的 Debug 吧。但是如果能够找到一个正确的版本，即在这个正确的版本上问题没有发生，那么就可以开始使用**git bisect**命令在版本库中进行二分查找了：

1.  工作区切换到已知的“好版本”和“坏版本”的中间的一个版本。
2.  执行测试，问题重现，将版本库当前版本库为“坏版本”，如果问题没有重现，将当前版本标记为“好版本”。
3.  重复 1-2，直至最终找到第一个导致问题出现的版本。

下面是示例版本库标记了提交 ID 后的示意图，在这个示例版本库中试验二分查找流程：首先标记最新提交（HEAD）是“坏的”，G 提交是好的，然后通过查找最终定位到坏提交（B）。

![../_images/commit-tree-bisect.png](img/commit-tree-bisect.png)

在下面的试验中定义坏提交的依据很简单，如果在`doc/`目录中包含文件`B.txt`，则此版本是“坏”的。（这个示例太简陋，不要见笑，聪明的读者可以直接通过`doc/B.txt`文件就可追溯到 B 提交。）

下面开始通过手动测试（查找`doc/B.txt`存在与否），借助 Git 二分查找定位“问题”版本。

*   首先确认工作在 master 分支。

    ```
    $ cd /path/to/my/workspace/gitdemo-commit-tree/
    $ git checkout master
    Already on 'master'

    ```

*   开始二分查找。

    ```
    $ git bisect start

    ```

*   已经当前版本是“坏提交”，因为存在文件`doc/B.txt`。而 G 版本是“好提交”，因为不存在文件`doc/B.txt`。

    ```
    $ git cat-file -t master:doc/B.txt
    blob
    $ git cat-file -t G:doc/B.txt
    fatal: Not a valid object name G:doc/B.txt

    ```

*   将当前版本（HEAD）标记为“坏提交”，将 G 版本标记为“好提交”。

    ```
    $ git bisect bad
    $ git bisect good G
    Bisecting: 5 revisions left to test after this (roughly 2 steps)
    [0cd7f2ea245d90d414e502467ac749f36aa32cc4] commit C.

    ```

*   自动定位到 C 提交。没有文件`doc/B.txt`，也是一个好提交。

    ```
    $ git describe
    C
    $ ls doc/B.txt
    ls: 无法访问 doc/B.txt: 没有那个文件或目录

    ```

*   标记当前版本（C 提交）为“好提交”。

    ```
    $ git bisect good
    Bisecting: 3 revisions left to test after this (roughly 2 steps)
    [212efce1548795a1edb08e3708a50989fcd73cce] Commit D: merge G with H

    ```

*   现在定位到 D 版本，这也是一个“好提交”。

    ```
    $ git describe
    D
    $ ls doc/B.txt
    ls: 无法访问 doc/B.txt: 没有那个文件或目录

    ```

*   标记当前版本（D 提交）为“好提交”。

    ```
    $ git bisect good
    Bisecting: 1 revision left to test after this (roughly 1 step)
    [776c5c9da9dcbb7e463c061d965ea47e73853b6e] Commit B: merge D with E and F

    ```

*   现在定位到 B 版本，这是一个“坏提交”。

    ```
    $ git bisect bad
    Bisecting: 0 revisions left to test after this (roughly 0 steps)
    [83be36956c007d7bfffe13805dd2081839fd3603] commit E.

    ```

*   现在定位到 E 版本，这是一个“好提交”。当标记 E 为好提交之后，输出显示已经成功定位到引入坏提交的最接近的版本。

    ```
    $ git bisect good
    776c5c9da9dcbb7e463c061d965ea47e73853b6e is the first bad commit

    ```

*   最终定位的坏提交用引用`refs/bisect/bad`标识。可以如下方法切换到该版本。

    ```
    $ git checkout bisect/bad
    Previous HEAD position was 83be369... commit E.
    HEAD is now at 776c5c9... Commit B: merge D with E and F

    ```

*   当对“Bug”定位和修复后，撤销二分查找在版本库中遗留的临时文件和引用。

    撤销二分查找后，版本库切换回执行二分查找之前所在的分支。

    ```
    $ git bisect reset
    Previous HEAD position was 776c5c9... Commit B: merge D with E and F
    Switched to branch 'master'

    ```

**把“好提交”标记成了“坏提交”该怎么办？**

在执行二分查找的过程中，一不小心就有可能犯错，将“好提交”标记为“坏提交”，或者相反。这将导致前面的查找过程也前功尽弃。Git 的二分查找提供一个恢复查找进度的办法。

*   例如对 E 提交，本来是一个“好版本”却被错误的标记为“坏版本”。

    ```
    $ git bisect bad
    83be36956c007d7bfffe13805dd2081839fd3603 is the first bad commit

    ```

*   用**git bisect log**命令查看二分查找的日志记录。

    把二分查找的日志保存在一个文件中。

    ```
    $ git bisect log > logfile

    ```

*   编辑这个文件，删除记录了错误动作的行。

    以井号（#）开始的行是注释。

    ```
    $ cat logfile
    # bad: [6652a0dce6a5067732c00ef0a220810a7230655e] Add Images for git treeview.
    # good: [e80aa7481beda65ae00e35afc4bc4b171f9b0ebf] commit G.
    git bisect start 'master' 'G'
    # good: [0cd7f2ea245d90d414e502467ac749f36aa32cc4] commit C.
    git bisect good 0cd7f2ea245d90d414e502467ac749f36aa32cc4
    # good: [212efce1548795a1edb08e3708a50989fcd73cce] Commit D: merge G with H
    git bisect good 212efce1548795a1edb08e3708a50989fcd73cce
    # bad: [776c5c9da9dcbb7e463c061d965ea47e73853b6e] Commit B: merge D with E and F
    git bisect bad 776c5c9da9dcbb7e463c061d965ea47e73853b6e

    ```

*   结束上一次出错的二分查找。

    ```
    $ git bisect reset
    Previous HEAD position was 83be369... commit E.
    Switched to branch 'master'

    ```

*   通过日志文件恢复进度。

    ```
    $ git bisect replay logfile
    We are not bisecting.
    Bisecting: 5 revisions left to test after this (roughly 2 steps)
    [0cd7f2ea245d90d414e502467ac749f36aa32cc4] commit C.
    Bisecting: 0 revisions left to test after this (roughly 0 steps)
    [83be36956c007d7bfffe13805dd2081839fd3603] commit E.

    ```

*   再一次回到了提交 E，这一次不要标记错了。

    ```
    $ git describe
    E
    $ git bisect good
    776c5c9da9dcbb7e463c061d965ea47e73853b6e is the first bad commit

    ```

**二分查找使用自动化测试**

Git 的二分查找命令支持`run`子命令，可以运行一个自动化测试脚本。

*   如果脚本的退出码是 0，正在测试的版本是一个“好版本”。
*   如果脚本的退出码是 125，正在测试的版本被跳过。
*   如果脚本的退出码是 1 到 127（125 除外），正在测试的版本是一个“坏版本”。

对于本例写一个自动化测试太简单了，无非就是判断文件是否存在，存在返回错误码 1，不存在返回错误码 0。

测试脚本`good-or-bad.sh`如下：

> ```
> #!/bin/sh
> 
> [ -f doc/B.txt ] && exit 1
> exit 0
> 
> ```

用此自动化脚本执行二分查找就非常简单了。

*   从已知的坏版本 master 和好版本 G，开始新一轮的二分查找。

    ```
    $ git bisect start master G
    Bisecting: 5 revisions left to test after this (roughly 2 steps)
    [0cd7f2ea245d90d414e502467ac749f36aa32cc4] commit C.

    ```

*   自动化测试，使用脚本`good-or-bad.sh`。

    ```
    $ git bisect run sh good-or-bad.sh
    running sh good-or-bad.sh
    Bisecting: 3 revisions left to test after this (roughly 2 steps)
    [212efce1548795a1edb08e3708a50989fcd73cce] Commit D: merge G with H
    running sh good-or-bad.sh
    Bisecting: 1 revision left to test after this (roughly 1 step)
    [776c5c9da9dcbb7e463c061d965ea47e73853b6e] Commit B: merge D with E and F
    running sh good-or-bad.sh
    Bisecting: 0 revisions left to test after this (roughly 0 steps)
    [83be36956c007d7bfffe13805dd2081839fd3603] commit E.
    running sh good-or-bad.sh
    776c5c9da9dcbb7e463c061d965ea47e73853b6e is the first bad commit
    bisect run success

    ```

*   定位到的“坏版本”是 B。

    ```
    $ git describe refs/bisect/bad
    B

    ```

### 2.8.4.7\. 获取历史版本

提取历史提交中的文件无非就是下面表格中的操作，在之前的实践中多次用到，不再赘述。

> | 动作 | 命令格式 | 示例 |
> | --- | --- | --- |
> | 查看历史提交的目录树 | git ls-tree <tree-ish> <paths> |  
> *   git ls-tree 776c5c9 README
> *   git ls-tree -r refs/tags/D doc
> 
>  |
> | 整个工作区切换到历史版本 | git checkout <commit> |  
> *   git checkout HEAD^^
> 
>  |
> | 检出某文件的历史版本 | git checkout <commit> – <paths> |  
> *   git checkout refs/tags/D – README
> *   git checkout 776c5c9 – doc
> 
>  |
> | 检出某文件的历史版本到其他文件名 | git show <commit>:<file> > new_name |  
> *   git show 887113d:README > README.OLD
> 
>  |

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 2.9\. 改变历史

我是《回到未来》的粉丝，偶尔会做梦，梦见穿梭到未来拿回一本 2000-2050 体育年鉴。操作 Git 可以体验到穿梭时空的感觉，因为 Git 像极了一个时光机器，不但允许你在历史中穿梭，而且能够改变历史。

在本章的最开始，介绍两种最简单和最常用的历史变更操作——“悔棋”操作，就是对刚刚进行的一次或几次提交进行修补。对于跳跃式的历史记录的变更，即仅对过去某一个或某几个提交作出改变，会在“回到未来”小节详细介绍。在“丢弃历史”小节会介绍一种版本库瘦身的方法，这可能会在某些特定的场合用到。

作为分布式版本控制系统，一旦版本库被多人共享，改变历史就可能是无法完成的任务。在本章的最后，介绍还原操作实现在不改变历史提交的情况下还原错误的改动。

## 2.9.1\. 悔棋

在日常的 Git 操作中，会经常出现这样的状况，输入**git commit**命令刚刚敲下回车键就后悔了：可能是提交说明中出现了错别字，或者有文件忘记提交，或者有的修改不应该提交，诸如此类。

像 Subversion 那样的集中式版本控制系统是“落子无悔”的系统，只能叹一口气责怪自己太不小心了。然后根据实际情况弥补：马上做一次新提交改正前面的错误；或者只能将错就错：错误的提交说明就让它一直错下去吧，因为大部分 Subversion 管理员不敢或者不会放开修改提交说明的功能导致无法对提交说明进行修改。

Git 提供了“悔棋”的操作，甚至因为“单步悔棋”是如此经常的发生，乃至于 Git 提供了一个简洁的操作——修补式提交，命令是：**git commit --amend**。

看看当前版本库最新的两次提交：

```
$ cd /path/to/my/workspace/demo
$ git log --stat -2
commit 822b4aeed5de74f949c9faa5b281001eb5439444
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Wed Dec 8 16:27:41 2010 +0800

    测试使用 qgit 提交。

 README      |    1 +
 src/hello.h |    2 --
 2 files changed, 1 insertions(+), 2 deletions(-)

commit 613486c17842d139871e0f1b0e9191d2b6177c9f
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Tue Dec 7 19:43:39 2010 +0800

    偷懒了，直接用 -a 参数直接提交。

 src/hello.h |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)

```

最新一次的提交是的确是在上一章使用`qgit`进行的提交，但这和提交内容无关，因此需要改掉这个提交的提交说明。使用下面的命令即可做到。

```
$ git commit --amend -m "Remove hello.h, which is useless."
[master 7857772] Remove hello.h, which is useless.
 2 files changed, 1 insertions(+), 2 deletions(-)
 delete mode 100644 src/hello.h

```

上面的命令使用了`-m`参数是为了演示的方便，实际上完全可以直接输入**git commit --amend**，在弹出的提交说明编辑界面修改提交说明，然后保存退出完成修补提交。

下面再看看最近两次的提交说明，可以看到最新的提交说明更改了（包括提交的 SHA1 哈希值），而它的父提交（即前一次提交）没有改变。

```
$ git log --stat -2
commit 78577724305e3e20aa9f2757ac5531d037d612a6
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Wed Dec 8 16:27:41 2010 +0800

    Remove hello.h, which is useless.

 README      |    1 +
 src/hello.h |    2 --
 2 files changed, 1 insertions(+), 2 deletions(-)

commit 613486c17842d139871e0f1b0e9191d2b6177c9f
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Tue Dec 7 19:43:39 2010 +0800

    偷懒了，直接用 -a 参数直接提交。

 src/hello.h |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)

```

如果最后一步操作不想删除文件`src/hello.h`，而只是想修改`README`，则可以按照下面的方法进行修补操作。

*   还原删除的`src/hello.h`文件。

    ```
    $ git checkout HEAD^ -- src/hello.h

    ```

*   此时查看状态，会看到`src/hello.h`被重新添加回暂存区。

    ```
    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       new file:   src/hello.h
    #

    ```

*   执行修补提交，不过提交说明是不是也要更改呢，因为毕竟这次提交不会删除文件了。

    ```
    $ git commit --amend -m "commit with --amend test."
    [master 2b45206] commit with --amend test.
     1 files changed, 1 insertions(+), 0 deletions(-)

    ```

*   再次查看最近两次提交，会发现最新的提交不再删除文件`src/hello.h`了。

    ```
    $ git log --stat -2
    commit 2b452066ef6e92bceb999cf94fcce24afb652259
    Author: Jiang Xin <jiangxin@ossxp.com>
    Date:   Wed Dec 8 16:27:41 2010 +0800

        commit with --amend test.

     README |    1 +
     1 files changed, 1 insertions(+), 0 deletions(-)

    commit 613486c17842d139871e0f1b0e9191d2b6177c9f
    Author: Jiang Xin <jiangxin@ossxp.com>
    Date:   Tue Dec 7 19:43:39 2010 +0800

        偷懒了，直接用 -a 参数直接提交。

     src/hello.h |    1 +
     1 files changed, 1 insertions(+), 0 deletions(-)

    ```

## 2.9.2\. 多步悔棋

Git 能够提供悔棋的奥秘在于 Git 的重置命令。实际上上面介绍的单步悔棋也可以用重置命令来实现，只不过 Git 提供了一个更好用的更简洁的修补提交命令而已。多步悔棋顾名思义就是可以取消最新连续的多次提交，多次悔棋并非是所有分布式版本控制系统都具有的功能，像 Mercurial/Hg 只能对最新提交悔棋一次（除非使用 MQ 插件）。Git 因为有了强大的重置命令，可以悔棋任意多次。

多步悔棋会在什么场合用到呢？软件开发中针对某个特性功能的开发就是一例。某个开发工程师领受某个特性开发的任务，于是在本地版本库进行了一系列开发、测试、修补、再测试的流程，最终特性功能开发完毕后可能在版本库中留下了多次提交。在将本地版本库改动推送（PUSH）到团队协同工作的核心版本库时，这个开发人员就想用多步悔棋的操作，将多个试验性的提及合为一个完整的提交。

以 DEMO 版本库为例，看看版本库最近的三次提交。

```
$ git log --stat --pretty=oneline -3
2b452066ef6e92bceb999cf94fcce24afb652259 commit with --amend test.
 README |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
613486c17842d139871e0f1b0e9191d2b6177c9f 偷懒了，直接用 -a 参数直接提交。
 src/hello.h |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
48456abfaeab706a44880eabcd63ea14317c0be9 add hello.h
 src/hello.h |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)

```

想要将最近的两个提交压缩为一个，并把提交说明改为“modify hello.h”，可以使用如下方法进行操作。

*   使用`--soft`参数调用重置命令，回到最近两次提交之前。

    ```
    $ git reset --soft HEAD^^

    ```

*   版本状态和最新日志。

    ```
    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       modified:   README
    #       modified:   src/hello.h
    #
    $ git log -1
    commit 48456abfaeab706a44880eabcd63ea14317c0be9
    Author: Jiang Xin <jiangxin@ossxp.com>
    Date:   Tue Dec 7 19:39:10 2010 +0800

        add hello.h

    ```

*   执行提交操作，即完成最新两个提交压缩为一个提交的操作。

    ```
    $ git commit -m "modify hello.h"
    [master b6f0b0a] modify hello.h
     2 files changed, 2 insertions(+), 0 deletions(-)

    ```

*   看看提交日志，“多步悔棋”操作成功。

    ```
    $ git log --stat --pretty=oneline -2
    b6f0b0a5237bc85de1863dbd1c05820f8736c76f modify hello.h
     README      |    1 +
     src/hello.h |    1 +
     2 files changed, 2 insertions(+), 0 deletions(-)
    48456abfaeab706a44880eabcd63ea14317c0be9 add hello.h
     src/hello.h |    1 +
     1 files changed, 1 insertions(+), 0 deletions(-)

    ```

## 2.9.3\. 回到未来

电影《回到未来》（Back to future）第二集，老毕福偷走时光车，到过去（1955 年）给了小毕福一本书，导致未来大变。

![../_images/back-to-future.png](img/back-to-future.png)

布朗博士正在解释为何产生两个平行的未来

Git 这一台“时光机”也有这样的能力，或者说也会具有这样的行为。当更改历史提交（SHA1 哈希值变更），即使后续提交的内容和属性都一致，但是因为后续提交中有一个属性是父提交的 SHA1 哈希值，所以一个历史提交的改变会引起连锁变化，导致所有后续提交必然的发生变化，就会形成两条平行的时间线：一个是变更前的提交时间线，另外一条是更改历史后新的提交时间线。

把此次实践比喻做一次电影（回到未来）拍摄的话，舞台依然是之前的 DEMO 版本库，而剧本是这样的。

*   角色：最近的六次提交。分别依据提交顺序，编号为 A、B、C、D、E、F。

    ```
    $ git log --oneline -6
    b6f0b0a modify hello.h                        # F
    48456ab add hello.h                           # E
    3488f2c move .gitignore outside also works.   # D
    b3af728 ignore object files.                  # C
    d71ce92 Hello world initialized.              # B
    c024f34 README is from welcome.txt.           # A

    ```

*   坏蛋：提交 D。

    即对`.gitignore`文件移动的提交不再需要，或者这个提交将和前一次提交（C）压缩为一个。

*   前奏：故事人物依次出场，坏蛋 D 在图中被特殊标记。

    ![../_images/git-rebase-orig.png](img/git-rebase-orig.png)
*   第一幕：抛弃提交 D，将正确的提交 E 和 F 重新“嫁接”到提交 C 上，最终坏蛋被消灭。

    ![../_images/git-rebase-c.png](img/git-rebase-c.png)
*   第二幕：坏蛋 D 被 C 感化，融合为”CD”复合体，E 和 F 重新“嫁接”到”CD”复合体上，最终大团圆结局。

    ![../_images/git-rebase-cd.png](img/git-rebase-cd.png)
*   道具：分别使用三辆不同的时光车来完成“回到未来”。

    分别是：核能跑车，清洁能源飞车，蒸汽为动力的飞行火车。

### 2.9.3.1\. 时间旅行一

《回到未来-第一集》布朗博士设计的第一款时间旅行车是一辆跑车，使用核燃料：钚。与之对应，此次实践使用的工具也没有太出乎想象，用一条新的指令——拣选指令（**git cherry-pick**）实现提交在新的分支上“重放”。

拣选指令——**git cherry-pick**，其含义是从众多的提交中挑选出一个提交应用在当前的工作分支中。该命令需要提供一个提交 ID 作为参数，操作过程相当于将该提交导出为补丁文件，然后在当前 HEAD 上重放形成无论内容还是提交说明都一致的提交。

首先对版本库要“参演”的角色进行标记，使用尚未正式介绍的命令**gi t tag**（无非就是在特定命名空间建立的引用，用于对提交的标识）。

```
$ git tag F
$ git tag E HEAD^
$ git tag D HEAD^^
$ git tag C HEAD^^^
$ git tag B HEAD~4
$ git tag A HEAD~5

```

通过日志，可以看到被标记的 6 个提交。

```
$ git log --oneline --decorate -6
b6f0b0a (HEAD, tag: F, master) modify hello.h
48456ab (tag: E) add hello.h
3488f2c (tag: D) move .gitignore outside also works.
b3af728 (tag: C) ignore object files.
d71ce92 (tag: hello_1.0, tag: B) Hello world initialized.
c024f34 (tag: A) README is from welcome.txt.

```

**现在演出第一幕：干掉坏蛋 D**

*   执行**git checkout**命令，暂时将 HEAD 头指针切换到 C。

    切换过程显示处于非跟踪状态的警告，没有关系，因为剧情需要。

    ```
    $ git checkout C
    Note: checking out 'C'.

    You are in 'detached HEAD' state. You can look around, make experimental
    changes and commit them, and you can discard any commits you make in this
    state without impacting any branches by performing another checkout.

    If you want to create a new branch to retain commits you create, you may
    do so (now or later) by using -b with the checkout command again. Example:

      git checkout -b new_branch_name

    HEAD is now at b3af728... ignore object files.

    ```

*   执行拣选操作将 E 提交在当前 HEAD 上重放。

    因为`E`和`master^`显然指向同一角色，因此可以用下面的语法。

    ```
    $ git cherry-pick master^
    [detached HEAD fa0b076] add hello.h
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 src/hello.h

    ```

*   执行拣选操作将`F`提交在当前 HEAD 上重放。

    F 和 master 也具有相同指向。

    ```
    $ git cherry-pick master
    [detached HEAD f677821] modify hello.h
     2 files changed, 2 insertions(+), 0 deletions(-)

    ```

*   通过日志可以看到坏蛋 D 已经不在了。

    ```
    $ git log --oneline --decorate -6
    f677821 (HEAD) modify hello.h
    fa0b076 add hello.h
    b3af728 (tag: C) ignore object files.
    d71ce92 (tag: hello_1.0, tag: B) Hello world initialized.
    c024f34 (tag: A) README is from welcome.txt.
    63992f0 restore file: welcome.txt

    ```

*   通过日志还可以看出来，最新两次提交的原始创作日期（AuthorDate）和提交日期（CommitDate）不同。AuthorDate 是拣选提交的原始更改时间，而 CommitDate 是拣选操作时的时间，因此拣选后的新提交的 SHA1 哈希值也不同于所拣选的原提交的 SHA1 哈希值。

    ```
    $ git log --pretty=fuller --decorate -2
    commit f677821dfc15acc22ca41b48b8ebaab5ac2d2fea (HEAD)
    Author:     Jiang Xin <jiangxin@ossxp.com>
    AuthorDate: Sun Dec 12 12:11:00 2010 +0800
    Commit:     Jiang Xin <jiangxin@ossxp.com>
    CommitDate: Sun Dec 12 16:20:14 2010 +0800

        modify hello.h

    commit fa0b076de600a53e8703545c299090153c6328a8
    Author:     Jiang Xin <jiangxin@ossxp.com>
    AuthorDate: Tue Dec 7 19:39:10 2010 +0800
    Commit:     Jiang Xin <jiangxin@ossxp.com>
    CommitDate: Sun Dec 12 16:18:34 2010 +0800

        add hello.h

    ```

*   最重要的一步操作，就是要将 master 分支指向新的提交 ID（f677821）上。

    下面的切换操作使用了 reflog 的语法，即`HEAD@{1}`相当于切换回 master 分支前的 HEAD 指向，即`f677821`。

    ```
    $ git checkout master
    Previous HEAD position was f677821... modify hello.h
    Switched to branch 'master'
    $ git reset --hard HEAD@{1}
    HEAD is now at f677821 modify hello.h

    ```

*   使用`qgit`查看版本库提交历史。

    ![../_images/git-rebase-graph.png](img/git-rebase-graph.png)

**幕布拉上，后台重新布景**

为了第二幕能够顺利演出，需要将 master 分支重新置回到提交 F 上。执行下面的操作完成“重新布景”。

```
$ git checkout master
Already on 'master'
$ git reset --hard F
HEAD is now at b6f0b0a modify hello.h
$ git log --oneline --decorate -6
b6f0b0a (HEAD, tag: F, master) modify hello.h
48456ab (tag: E) add hello.h
3488f2c (tag: D) move .gitignore outside also works.
b3af728 (tag: C) ignore object files.
d71ce92 (tag: hello_1.0, tag: B) Hello world initialized.
c024f34 (tag: A) README is from welcome.txt.

```

布景完毕，大幕即将再次拉开。

**现在演出第二幕：坏蛋 D 被感化，融入社会**

*   执行**git checkout**命令，暂时将 HEAD 头指针切换到坏蛋 D。

    切换过程显示处于非跟踪状态的警告，没有关系，因为剧情需要。

    ```
    $ git checkout D
    Note: checking out 'D'.

    You are in 'detached HEAD' state. You can look around, make experimental
    changes and commit them, and you can discard any commits you make in this
    state without impacting any branches by performing another checkout.

    If you want to create a new branch to retain commits you create, you may
    do so (now or later) by using -b with the checkout command again. Example:

      git checkout -b new_branch_name

    HEAD is now at 3488f2c... move .gitignore outside also works.

    ```

*   悔棋两次，以便将 C 和 D 融合。

    ```
    $ git reset --soft HEAD^^

    ```

*   执行提交，提交说明重用 C 提交的提交说明。

    ```
    $ git commit -C C
    [detached HEAD 53e621c] ignore object files.
     1 files changed, 3 insertions(+), 0 deletions(-)
     create mode 100644 .gitignore

    ```

*   执行拣选操作将 E 提交在当前 HEAD 上重放。

    ```
    $ git cherry-pick E
    [detached HEAD 1f99f82] add hello.h
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 src/hello.h

    ```

*   执行拣选操作将 F 提交在当前 HEAD 上重放。

    ```
    $ git cherry-pick F
    [detached HEAD 2f13d3a] modify hello.h
     2 files changed, 2 insertions(+), 0 deletions(-)

    ```

*   通过日志可以看到提交 C 和 D 被融合，所以在日志中看不到 C 的标签。

    ```
    $ git log --oneline --decorate -6
    2f13d3a (HEAD) modify hello.h
    1f99f82 add hello.h
    53e621c ignore object files.
    d71ce92 (tag: hello_1.0, tag: B) Hello world initialized.
    c024f34 (tag: A) README is from welcome.txt.
    63992f0 restore file: welcome.txt

    ```

*   最重要的一步操作，就是要将 master 分支指向新的提交 ID（2f13d3a）上。

    下面的切换操作使用了 reflog 的语法，即`HEAD@{1}`相当于切换回 master 分支前的 HEAD 指向，即`2f13d3a`。

    ```
    $ git checkout master
    Previous HEAD position was 2f13d3a... modify hello.h
    Switched to branch 'master'
    $ git reset --hard HEAD@{1}
    HEAD is now at 2f13d3a modify hello.h

    ```

*   使用`gitk`查看版本库提交历史。

    ![../_images/git-rebase-graph-gitk.png](img/git-rebase-graph-gitk.png)

**别忘了后台的重新布景**

为了接下来的时间旅行二能够顺利开始，需要重新布景，将 master 分支重新置回到提交 F 上。

```
$ git checkout master
Already on 'master'
$ git reset --hard F
HEAD is now at b6f0b0a modify hello.h

```

### 2.9.3.2\. 时间旅行二

《回到未来-第二集》布朗博士改进的时间旅行车使用了未来科技，是陆天两用的飞车，而且燃料不再依赖核物质，而是使用无所不在的生活垃圾。而此次实践使用的工具也进行了升级，采用强大的**git rebase**命令。

命令**git rebase**是对提交执行变基操作，即可以实现将指定范围的提交“嫁接”到另外一个提交之上。其常用的命令行格式有：

```
用法 1: git rebase --onto  <newbase>  <since>      <till>
用法 2: git rebase --onto  <newbase>  <since>
用法 3: git rebase         <newbase>               <till>
用法 4: git rebase         <newbase>
用法 5: git rebase -i ...
用法 6: git rebase --continue
用法 7: git rebase --skip
用法 8: git rebase --abort

```

不要被上面的语法吓到，用法 5 会在下节（时间旅行三）中予以介绍，后三种用法则是变基运行过程被中断时可采用的命令——继续变基或终止等。

*   用法 6 是在变基遇到冲突而暂停后，当完成冲突解决后（添加到暂存区，不提交），恢复变基操作的时候使用。
*   用法 7 是在变基遇到冲突而暂停后，跳过当前提交的时候使用。
*   用法 8 是在变基遇到冲突后，终止变基操作，回到之前的分支时候使用。

而前四个用法如果把省略的参数补上（方括号内是省略掉的参数），看起来就都和用法 1 就一致了。

```
用法 1: git rebase  --onto  <newbase>  <since>      <till>
用法 2: git rebase  --onto  <newbase>  <since>      [HEAD]
用法 3: git rebase [--onto] <newbase>  [<newbase>]  <till>
用法 4: git rebase [--onto] <newbase>  [<newbase>]  [HEAD]

```

下面就以归一化的**git rebase**命令格式来介绍其用法。

```
命令格式: git rebase  --onto  <newbase>  <since>  <till>

```

变基操作的过程：

*   首先会执行**git checkout**切换到`<till>`。

    因为会切换到`<till>`，因此如果`<till>`指向的不是一个分支（如 master），则变基操作是在`detached HEAD`（分离头指针）状态进行的，当变基结束后，还要像在“时间旅行一”中那样，对 master 分支执行重置以实现把变基结果记录在分支中。

*   将`<since>..<till>`所标识的提交范围写到一个临时文件中。

    还记得前面介绍的版本范围语法，`<since>..<till>`是指包括`<till>`的所有历史提交排除`<since>`以及`<since>`的历史提交后形成的版本范围。

*   当前分支强制重置（git reset –hard）到`<newbase>`。

    相当于执行：**git reset --hard <newbase>**。

*   从保存在临时文件中的提交列表中，一个一个将提交按照顺序重新提交到重置之后的分支上。

*   如果遇到提交已经在分支中包含，跳过该提交。

*   如果在提交过程遇到冲突，变基过程暂停。用户解决冲突后，执行**git rebase --continue**继续变基操作。或者执行**git rebase --skip**跳过此提交。或者执行**git rebase --abort**就此终止变基操作切换到变基前的分支上。

很显然为了执行将 E 和 F 提交跳过提价 D，“嫁接”到 C 提交上。可以如此执行变基命令：

```
$ git rebase --onto C E^ F

```

因为`E^`等价于 D，并且 F 和当前 HEAD 指向相同，因此可以这样操作：

```
$ git rebase --onto C D

```

有了对变基命令的理解，就可以开始新的“回到未来”之旅了。

确认舞台已经布置完毕。

```
$ git status -s -b
## master
$ git log --oneline --decorate -6
b6f0b0a (HEAD, tag: F, master) modify hello.h
48456ab (tag: E) add hello.h
3488f2c (tag: D) move .gitignore outside also works.
b3af728 (tag: C) ignore object files.
d71ce92 (tag: hello_1.0, tag: B) Hello world initialized.
c024f34 (tag: A) README is from welcome.txt.

```

**现在演出第一幕：干掉坏蛋 D**

*   执行变基操作。

    因为下面的变基操命令行使用了参数 F。F 是一个里程碑指向一个提交，而非 master，会导致后面变基完成还需要对 master 分支执行重置。在第二幕中会使用 master，会发现省事不少。

    ```
    $ git rebase --onto C E^ F
    First, rewinding head to replay your work on top of it...
    Applying: add hello.h
    Applying: modify hello.h

    ```

*   最后一步必需的操作，就是要将 master 分支指向变基后的提交上。

    下面的切换操作使用了 reflog 的语法，即`HEAD@{1}`相当于切换回 master 分支前的 HEAD 指向，即`3360440`。

    ```
    $ git checkout master
    Previous HEAD position was 3360440... modify hello.h
    Switched to branch 'master'
    $ git reset --hard HEAD@{1}
    HEAD is now at 3360440 modify hello.h

    ```

*   经过检查，操作完毕，收工。

    ```
    $ git log --oneline --decorate -6
    3360440 (HEAD, master) modify hello.h
    1ef3803 add hello.h
    b3af728 (tag: C) ignore object files.
    d71ce92 (tag: hello_1.0, tag: B) Hello world initialized.
    c024f34 (tag: A) README is from welcome.txt.
    63992f0 restore file: welcome.txt

    ```

**幕布拉上，后台重新布景**

为了第二幕能够顺利演出，需要将 master 分支重新置回到提交 F 上。执行下面的操作完成“重新布景”。

```
$ git checkout master
Already on 'master'
$ git reset --hard F
HEAD is now at b6f0b0a modify hello.h

```

布景完毕，大幕即将再次拉开。

**现在演出第二幕：坏蛋 D 被感化，融入社会**

*   执行**git checkout**命令，暂时将 HEAD 头指针切换到坏蛋 D。

    切换过程显示处于非跟踪状态的警告，没有关系，因为剧情需要。

    ```
    $ git checkout D
    Note: checking out 'D'.

    You are in 'detached HEAD' state. You can look around, make experimental
    changes and commit them, and you can discard any commits you make in this
    state without impacting any branches by performing another checkout.

    If you want to create a new branch to retain commits you create, you may
    do so (now or later) by using -b with the checkout command again. Example:

      git checkout -b new_branch_name

    HEAD is now at 3488f2c... move .gitignore outside also works.

    ```

*   悔棋两次，以便将 C 和 D 融合。

    ```
    $ git reset --soft HEAD^^

    ```

*   执行提交，提交说明重用 C 提交的提交说明。

    ```
    $ git commit -C C
    [detached HEAD 2d020b6] ignore object files.
     1 files changed, 3 insertions(+), 0 deletions(-)
     create mode 100644 .gitignore

    ```

*   记住这个提交 ID：`2d020b6`。

    用里程碑是最好的记忆提交 ID 的方法：

    ```
    $ git tag newbase
    $ git rev-parse newbase
    2d020b62034b7a433f80396118bc3f66a60f296f

    ```

*   执行变基操作，将 E 和 F 提交“嫁接”到`newbase`上。

    下面的变基操命令行没有像之前的操作使用使用了参数 F，而是使用分支 master。所以接下来的变基操作会直接修改 master 分支，而无须再进行对 master 的重置操作。

    ```
    $ git rebase --onto newbase E^ master
    First, rewinding head to replay your work on top of it...
    Applying: add hello.h
    Applying: modify hello.h

    ```

*   看看提交日志，看到提交 C 和提交 D 都不见了，代之以融合后的提交`newbase`。

    还可以看到最新的提交除了和 HEAD 的指向一致，也和 master 分支的指向一致。

    ```
    $ git log --oneline --decorate -6
    2495dc1 (HEAD, master) modify hello.h
    6349328 add hello.h
    2d020b6 (tag: newbase) ignore object files.
    d71ce92 (tag: hello_1.0, tag: B) Hello world initialized.
    c024f34 (tag: A) README is from welcome.txt.
    63992f0 restore file: welcome.txt

    ```

*   当前的确已经在 master 分支上了，操作全部完成。

    ```
    $ git branch
    * master

    ```

*   清理一下，然后收工。

    前面的操作中为了方便创建了标识提交的新里程碑`newbase`，将这个里程碑现在没有什么用处了删除吧。

    ```
    $ git tag -d newbase
    Deleted tag 'newbase' (was 2d020b6)

    ```

**别忘了后台的重新布景**

为了接下来的时间旅行三能够顺利开始，需要重新布景，将 master 分支重新置回到提交 F 上。

```
$ git checkout master
Already on 'master'
$ git reset --hard F
HEAD is now at b6f0b0a modify hello.h

```

### 2.9.3.3\. 时间旅行三

《回到未来-第三集》铁匠布朗博士手工打造了可以时光旅行的飞行火车，使用蒸汽作为动力。这款时间旅行火车更大，更安全，更舒适，适合一家四口外加宠物的时空旅行。与之对应本次实践也将采用“手工打造”：交互式变基。

交互式变基就是在上一节介绍的变基命令的基础上，添加了`-i`参数，在变基的时候进入一个交互界面。使用了交互界面的变基操作，不仅仅是自动化变基转换为手动确认那么没有技术含量，而是充满了魔法。

执行交互式变基操作，会将`<since>..<till>`的提交悉数罗列在一个文件中，然后自动打开一个编辑器来编辑这个文件。可以通过修改文件的内容（删除提交，修改提交的动作关键字）实现删除提交，压缩多个提交为一个提交，更改提交的顺序，更改历史提交的提交说明。

例如下面的界面就是针对当前 DEMO 版本库执行的交互式变基时编辑器打开的文件：

```
pick b3af728 ignore object files.
pick 3488f2c move .gitignore outside also works.
pick 48456ab add hello.h
pick b6f0b0a modify hello.h

# Rebase d71ce92..b6f0b0a onto d71ce92
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x <cmd>, exec <cmd> = Run a shell command <cmd>, and stop if it fails
#
# If you remove a line here THAT COMMIT WILL BE LOST.
# However, if you remove everything, the rebase will be aborted.

```

从该文件可以看出：

*   开头的四行由上到下依次对应于提交 C、D、E、F。

*   前四行缺省的动作都是`pick`，即应用此提交。

*   参考配置文件中的注释，可以通过修改动作名称，在变基的时候执行特定操作。

*   动作`reword`或者简写为`r`，含义是变基时应用此提交，但是在提交的时候允许用户修改提交说明。

    这个功能在 Git 1.6.6 之后开始提供，对于修改历史提交的提交说明异常方便。老版本的 Git 还是使用`edit`动作吧。

*   动作`edit`或者简写为`e`，也会应用此提交，但是会在应用时停止，提示用户使用**git commit --amend**执行提交，以便对提交进行修补。

    当用户执行**git commit --amend**完成提交后，还需要执行**git rebase --continue**继续变基操作。Git 会对用户进行相应地提示。

    实际上用户在变基暂停状态执行修补提交可以执行多次，相当于把一个提交分解为多个提交。而且`edit`动作也可以实现`reword`的动作，因此对于老版本的 Git 没有`reword`可用，则可以使用此动作。

*   动作`squash`或者简写为`s`，该提交会与前面的提交压缩为一个。

*   动作`fixup`或者简写为`f`，类似`squash`动作，但是此提交的提交说明被丢弃。

    这个功能在 Git 1.7.0 之后开始提供，老版本的 Git 还是使用`squash`动作吧。

*   可以通过修改配置文件中这四个提交的先后顺序，进而改变最终变基后提交的先后顺序。

*   可以对相应提交对应的行执行删除操作，这样该提交就不会被应用，进而在变基后的提交中被删除。

有了对交互式变基命令的理解，就可以开始新的“回到未来”之旅了。

确认舞台已经布置完毕。

```
$ git status -s -b
## master
$ git log --oneline --decorate -6
b6f0b0a (HEAD, tag: F, master) modify hello.h
48456ab (tag: E) add hello.h
3488f2c (tag: D) move .gitignore outside also works.
b3af728 (tag: C) ignore object files.
d71ce92 (tag: hello_1.0, tag: B) Hello world initialized.
c024f34 (tag: A) README is from welcome.txt.

```

**现在演出第一幕：干掉坏蛋 D**

*   执行交互式变基操作。

    ```
    $ git rebase -i D^

    ```

*   自动用编辑器修改文件。文件内容如下：

    ```
    pick 3488f2c move .gitignore outside also works.
    pick 48456ab add hello.h
    pick b6f0b0a modify hello.h

    # Rebase b3af728..b6f0b0a onto b3af728
    #
    # Commands:
    #  p, pick = use commit
    #  r, reword = use commit, but edit the commit message
    #  e, edit = use commit, but stop for amending
    #  s, squash = use commit, but meld into previous commit
    #  f, fixup = like "squash", but discard this commit's log message
    #  x <cmd>, exec <cmd> = Run a shell command <cmd>, and stop if it fails
    #
    # If you remove a line here THAT COMMIT WILL BE LOST.
    # However, if you remove everything, the rebase will be aborted.
    #

    ```

*   将第一行删除，使得上面的配置文件看起来像是这样（省略井号开始的注释）：

    ```
    pick 48456ab add hello.h
    pick b6f0b0a modify hello.h

    ```

*   保存退出。

*   变基自动开始，即刻完成。

    显示下面的内容。

    ```
    Successfully rebased and updated refs/heads/master.

    ```

*   看看日志。当前分支 master 已经完成变基，消灭了“坏蛋 D”。

    ```
    $ git log --oneline --decorate -6
    78e5133 (HEAD, master) modify hello.h
    11eea7e add hello.h
    b3af728 (tag: C) ignore object files.
    d71ce92 (tag: hello_1.0, tag: B) Hello world initialized.
    c024f34 (tag: A) README is from welcome.txt.
    63992f0 restore file: welcome.txt

    ```

**幕布拉上，后台重新布景**

为了第二幕能够顺利演出，需要将 master 分支重新置回到提交 F 上。执行下面的操作完成“重新布景”。

```
$ git checkout master
Already on 'master'
$ git reset --hard F
HEAD is now at b6f0b0a modify hello.h

```

布景完毕，大幕即将再次拉开。

**现在演出第二幕：坏蛋 D 被感化，融入社会**

*   同样执行交互式变基操作，不过因为要将 C 和 D 压缩为一个，因此变基从 C 的父提交开始。

    ```
    $ git rebase -i C^

    ```

*   自动用编辑器修改文件。文件内容如下（忽略井号开始的注释）：

    ```
    pick b3af728 ignore object files.
    pick 3488f2c move .gitignore outside also works.
    pick 48456ab add hello.h
    pick b6f0b0a modify hello.h

    ```

*   修改第二行（提交 D），将动作由`pick`修改为`squash`。

    修改后的内容如下：

    ```
    pick b3af728 ignore object files.
    squash 3488f2c move .gitignore outside also works.
    pick 48456ab add hello.h
    pick b6f0b0a modify hello.h

    ```

*   保存退出。

*   自动开始变基操作，在执行到`squash`命令设定的提交时，进入提交前的日志编辑状态。

    显示的待编辑日志如下。很明显 C 和 D 的提交说明显示在了一起。

    ```
    # This is a combination of 2 commits.
    # The first commit's message is:

    ignore object files.

    # This is the 2nd commit message:

    move .gitignore outside also works.

    ```

*   保存退出，即完成`squash`动作标识的提交以及后续变基操作。

*   看看提交日志，看到提交 C 和提交 D 都不见了，代之以一个融合后的提交。

    ```
    $ git log --oneline --decorate -6
    c0c2a1a (HEAD, master) modify hello.h
    c1e8b66 add hello.h
    db512c0 ignore object files.
    d71ce92 (tag: hello_1.0, tag: B) Hello world initialized.
    c024f34 (tag: A) README is from welcome.txt.
    63992f0 restore file: welcome.txt

    ```

*   可以看到融合 C 和 D 的提交日志实际上是两者日志的融合。在前面单行显示的日志中看不出来。

    ```
    $ git cat-file -p HEAD^^
    tree 00239a5d0daf9824a23cbf104d30af66af984e27
    parent d71ce9255b3b08c718810e4e31760198dd6da243
    author Jiang Xin <jiangxin@ossxp.com> 1291720899 +0800
    committer Jiang Xin <jiangxin@ossxp.com> 1292153393 +0800

    ignore object files.

    move .gitignore outside also works.

    ```

时光旅行结束了，多么神奇的 Git 啊。

## 2.9.4\. 丢弃历史

历史有的时候会成为负担。例如一个人使用的版本库有一天需要作为公共版本库多人共享，最早的历史可能不希望或者没有必要继续保持存在，需要一个抛弃部分早期历史提交的精简的版本库用于和他人共享。再比如用 Git 做文件备份，不希望备份的版本过多导致不必要的磁盘空间占用，同样会有精简版本的需要：只保留最近的 100 次提交，抛弃之前的历史提交。那么应该如何操作呢？

使用交互式变基当然可以完成这样的任务，但是如果历史版本库有成百上千个，把成百上千个版本的变基动作有`pick`修改为`fixup`可真的很费事，实际上 Git 有更简便的方法。

现在 DEMO 版本库有如下的提交记录：

```
$ git log --oneline --decorate
c0c2a1a (HEAD, master) modify hello.h
c1e8b66 add hello.h
db512c0 ignore object files.
d71ce92 (tag: hello_1.0, tag: B) Hello world initialized.
c024f34 (tag: A) README is from welcome.txt.
63992f0 restore file: welcome.txt
7161977 delete trash files. (using: git add -u)
2b31c19 (tag: old_practice) Merge commit 'acc2f69'
acc2f69 commit in detached HEAD mode.
4902dc3 does master follow this new commit?
e695606 which version checked in?
a0c641e who does commit?
9e8a761 initialized.

```

如果希望把里程碑 A（c024f34）之前的历史提交历史全部清除可以如下进行操作。

*   查看里程碑 A 指向的目录树。

    用`A^{tree}`语法访问里程碑 A 对应的目录树。

    ```
    $ git cat-file -p A^{tree}
    100644 blob 51dbfd25a804c30e9d8dc441740452534de8264b    README

    ```

*   使用`git commit-tree`命令直接从该目录树创建提交。

    ```
    $ echo "Commit from tree of tag A." | git commit-tree A^{tree}
    8f7f94ba6a9d94ecc1c223aa4b311670599e1f86

    ```

*   命令`git commit-tree`的输出是一个提交的 SHA1 哈希值。查看这个提交。

    会发现这个提交没有历史提交，可以称之为孤儿提交。

    ```
    $ git log 8f7f94ba6a9d94ecc1c223aa4b311670599e1f86
    commit 8f7f94ba6a9d94ecc1c223aa4b311670599e1f86
    Author: Jiang Xin <jiangxin@ossxp.com>
    Date:   Mon Dec 13 14:17:17 2010 +0800

        Commit from tree of tag A.

    ```

*   执行变基，将 master 分支从里程碑到最新的提交全部迁移到刚刚生成的孤儿提交上。

    ```
    $ git rebase --onto 8f7f94ba6a9d94ecc1c223aa4b311670599e1f86 A master
    First, rewinding head to replay your work on top of it...
    Applying: Hello world initialized.
    Applying: ignore object files.
    Applying: add hello.h
    Applying: modify hello.h

    ```

*   查看日志看到当前 master 分支的历史已经精简了。

    ```
    $ git log --oneline --decorate
    2584639 (HEAD, master) modify hello.h
    30fe8b3 add hello.h
    4dd8a65 ignore object files.
    5f2cae1 Hello world initialized.
    8f7f94b Commit from tree of tag A.

    ```

使用图形工具查看提交历史，会看到两棵树：最上面的一棵树是刚刚通过变基抛弃了大部分历史提交的新的 master 分支，下面的一棵树则是变基前的提交形成的。下面的一棵树之所以还能够看到，或者说还没有从版本库中彻底清除，是因为有部分提交仍带有里程碑标签。

![../_images/git-rebase-purge-history-graph.png](img/git-rebase-purge-history-graph.png)

## 2.9.5\. 反转提交

前面介绍的操作都涉及到对历史的修改，这对于一个人使用 Git 没有问题，但是如果多人协同就会有问题了。多人协同使用 Git，在本地版本库做的提交会通过多人之间的交互成为他人版本库的一部分，更改历史操作只能是针对自己的版本库，而无法去修改他人的版本库，正所谓“覆水难收”。在这种情况下要想修正一个错误历史提交的正确做法是反转提交，即重新做一次新的提交，相当于错误的历史提交的反向提交，修正错误的历史提交。

Git 反向提交命令是:**git revert**，下面在 DEMO 版本库中实践一下。注意：Subversion 的用户不要想当然的和**svn revert**命令对应，这两个版本控制系统中的`revert`命令的功能完全不相干。

当前 DEMO 版本库最新的提交包含如下改动：

```
$ git show HEAD
commit 25846394defe16eab103b92efdaab5e46cc3dc22
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Sun Dec 12 12:11:00 2010 +0800

    modify hello.h

diff --git a/README b/README
index 51dbfd2..ceaf01b 100644
--- a/README
+++ b/README
@@ -1,3 +1,4 @@
 Hello.
 Nice to meet you.
 Bye-Bye.
+Wait...
diff --git a/src/hello.h b/src/hello.h
index 0043c3b..6e482c6 100644
--- a/src/hello.h
+++ b/src/hello.h
@@ -1 +1,2 @@
 /* test */
+/* end */

```

在不改变这个提交的前提下对其修改进行撤销，就需要用到`git revert`反转提交。

```
$ git revert HEAD

```

运行该命令相当于将 HEAD 提交反向再提交一次，在提交说明编辑状态下暂停，显示如下（注释行被忽略）：

```
Revert "modify hello.h"

This reverts commit 25846394defe16eab103b92efdaab5e46cc3dc22.

```

可以在编辑器中修改提交说明，提交说明编辑完毕保存退出则完成反转提交。查看提交日志可以看到新的提交相当于所撤销提交的反向提交。

```
$ git log --stat -2
commit 6e6753add1601c4efa7857ab4c5b245e0e161314
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Mon Dec 13 15:19:12 2010 +0800

    Revert "modify hello.h"

    This reverts commit 25846394defe16eab103b92efdaab5e46cc3dc22.

 README      |    1 -
 src/hello.h |    1 -
 2 files changed, 0 insertions(+), 2 deletions(-)

commit 25846394defe16eab103b92efdaab5e46cc3dc22
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Sun Dec 12 12:11:00 2010 +0800

    modify hello.h

 README      |    1 +
 src/hello.h |    1 +
 2 files changed, 2 insertions(+), 0 deletions(-)

```

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 2.10\. Git 克隆

到现在为止，读者已经零略到 Git 的灵活性以及健壮性。Git 可以通过重置随意撤销提交，可以通过变基操作更改历史，可以随意重组提交，还可以通过 reflog 的记录纠正错误的操作。但是再健壮的版本库设计，也抵挡不了存储介质的崩溃。还有一点就是不要忘了 Git 版本库是躲在工作区根目录下的`.git`目录中，如果忘了这一点直接删除工作区，就会把版本库也同时删掉，悲剧就此发生。

“不要把鸡蛋装在一个篮子里”，是颠扑不破的安全法则。

在本章会学习到如何使用**git clone**命令建立版本库克隆，以及如何使用**git push**和**git pull**命令实现克隆之间的同步。

## 2.10.1\. 鸡蛋不装在一个篮子里

Git 的版本库目录和工作区在一起，因此存在一损俱损的问题，即如果删除一个项目的工作区，同时也会把这个项目的版本库删除掉。一个项目仅在一个工作区中维护太危险了，如果有两个工作区就会好很多。

![../_images/git-clone-pull-push.png](img/git-clone-pull-push.png)

上图中一个项目使用了两个版本库进行维护，两个版本库之间通过拉回（PULL）和/或推送（PUSH）操作实现同步。

*   版本库 A 通过克隆操作创建克隆版本库 B。
*   版本库 A 可以通过推送（PUSH）操作，将新提交传递给版本库 B；
*   版本库 A 可以通过拉回（PULL）操作，将版本库 B 中的新提交拉回到自身（A）。
*   版本库 B 可以通过拉回（PULL）操作，将版本库 A 中的新提交拉回到自身（B）。
*   版本库 B 可以通过推送（PUSH）操作，将新提交传递给版本库 A；

Git 使用**git clone**命令实现版本库克隆，主要有如下三种用法：

```
用法 1: git clone <repository> <directory>
用法 2: git clone --bare   <repository> <directory.git>
用法 3: git clone --mirror <repository> <directory.git>

```

这三种用法的区别如下：

*   用法 1 将`<repository>`指向的版本库创建一个克隆到`<directory>`目录。目录`<directory>`相当于克隆版本库的工作区，文件都会检出，版本库位于工作区下的`.git`目录中。
*   用法 2 和用法 3 创建的克隆版本库都不含工作区，直接就是版本库的内容，这样的版本库称为裸版本库。一般约定俗成裸版本库的目录名以`.git`为后缀，所以上面示例中将克隆出来的裸版本库目录名写做`<directory.git>`。
*   用法 3 区别于用法 2 之处在于用法 3 克隆出来的裸版本对上游版本库进行了注册，这样可以在裸版本库中使用**git fetch**命令和上游版本库进行持续同步。
*   用法 3 只在 1.6.0 或更新版本的 Git 才提供。

Git 的 PUSH 和 PULL 命令的用法相似，使用下面的语法：

```
git push [<remote-repos> [<refspec>]]
git pull [<remote-repos> [<refspec>]]

```

其中方括号的含义是参数可以省略，`<remote-repos>`是远程版本库的地址或名称，`<refspec>`是引用表达式，暂时理解为引用即可。在后面的章节再具体介绍 PUSH 和 PULL 命令的细节。

下面就通过不同的 Git 命令组合，掌握版本库克隆和镜像的技巧。

## 2.10.2\. 对等工作区

不使用`--bare`或者`--mirror`创建出来的克隆包含工作区，这样就会产生两个包含工作区的版本库。这两个版本库是对等的，如下图。

![../_images/git-clone-1.png](img/git-clone-1.png)

这两个工作区本质上没有区别，但是往往提交是在一个版本（A）中进行的，另外一个（B）作为备份。对于这种对等工作区模式，版本库的同步只有一种可行的操作模式，就是在备份库（B）执行 git pull 命令从源版本库（A）拉回新的提交实现版本库同步。为什么不能从版本库 A 向版本库 B 执行 git push 的推送操作呢？看看下面的操作。

执行克隆命令，将版本库`/path/to/my/workspace/demo`克隆到`/path/to/my/workspace/demo-backup`。

```
$ git clone /path/to/my/workspace/demo /path/to/my/workspace/demo-backup
Cloning into /path/to/my/workspace/demo-backup...
done.

```

进入 demo 版本库，生成一些测试提交（使用`--allow-empty`参数可以生成空提交）。

```
$ cd /path/to/my/workspace/demo/
$ git commit --allow-empty -m "sync test 1"
[master 790e72a] sync test 1
$ git commit --allow-empty -m "sync test 2"
[master f86b7bf] sync test 2

```

能够在 demo 版本库向 demo-backup 版本库执行 PUSH 操作么？执行一下**git push**看一看。

```
$ git push /path/to/my/workspace/demo-backup
Counting objects: 2, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 274 bytes, done.
Total 2 (delta 1), reused 0 (delta 0)
Unpacking objects: 100% (2/2), done.
remote: error: refusing to update checked out branch: refs/heads/master
remote: error: By default, updating the current branch in a non-bare repository
remote: error: is denied, because it will make the index and work tree inconsistent
remote: error: with what you pushed, and will require 'git reset --hard' to match
remote: error: the work tree to HEAD.
remote: error:
remote: error: You can set 'receive.denyCurrentBranch' configuration variable to
remote: error: 'ignore' or 'warn' in the remote repository to allow pushing into
remote: error: its current branch; however, this is not recommended unless you
remote: error: arranged to update its work tree to match what you pushed in some
remote: error: other way.
remote: error:
remote: error: To squelch this message and still keep the default behaviour, set
remote: error: 'receive.denyCurrentBranch' configuration variable to 'refuse'.
To /path/to/my/workspace/demo-backup
 ! [remote rejected] master -> master (branch is currently checked out)
error: failed to push some refs to '/path/to/my/workspace/demo-backup'

```

翻译成中文：

```
$ git push /path/to/my/workspace/demo-backup
...
对方说: 错了:
              拒绝更新已检出的分支 refs/heads/master 。
              缺省更新非裸版本库的当前分支是不被允许的，因为这将会导致
              暂存区和工作区与您推送至版本库的新提交不一致。这太古怪了。

              如果您一意孤行，也不是不允许，但是您需要为我设置如下参数：

                  receive.denyCurrentBranch = ignore|warn

              到 /path/to/my/workspace/demo-backup

 ! [对方拒绝] master -> master (分支当前已检出)
错误: 部分引用的推送失败了, 至 '/path/to/my/workspace/demo-backup'

```

从错误输出可以看出，虽然可以改变 Git 的缺省行为，允许向工作区推送已经检出的分支，但是这么做实在不高明。

为了实现同步，需要进入到备份版本库中，执行**git pull**命令。

```
$ git pull
From /path/to/my/workspace/demo
   6e6753a..f86b7bf  master     -> origin/master
Updating 6e6753a..f86b7bf
Fast-forward

```

在 demo-backup 版本库中查看提交日志，可以看到在 demo 版本库中的新提交已经被拉回到 demo-backup 版本库中。

```
$ git log --oneline -2
f86b7bf sync test 2
790e72a sync test 1

```

**为什么执行 git pull 拉回命令没有像执行 git push 命令那样提供那么多的参数呢？**

这是因为在执行**git clone**操作后，克隆出来的 demo-backup 版本库中对源版本库（上游版本库）进行了注册，所以当在 demo-backup 版本库执行拉回操作，无须设置上游版本库的地址。

在 demo-backup 版本库中可以使用下面的命令查看对上游版本库的注册信息：

```
$ cd /path/to/my/workspace/demo-backup
$ git remote -v
origin  /path/to/my/workspace/demo (fetch)
origin  /path/to/my/workspace/demo (push)

```

实际注册上游远程版本库的奥秘都在 Git 的配置文件中（略去无关的行）：

```
$ cat /path/to/my/workspace/demo-backup/.git/config
...
[remote "origin"]
        fetch = +refs/heads/*:refs/remotes/origin/*
        url = /path/to/my/workspace/demo
[branch "master"]
        remote = origin
        merge = refs/heads/master

```

关于配置文件`[remote]`小节和`[branch]`小节的奥秘在后面的章节予以介绍。

## 2.10.3\. 克隆生成裸版本库

上一节在对等工作区模式下，工作区之间执行推送，可能会引发大段的错误输出，如果采用裸版本库则没有相应的问题。这是因为裸版本库没有工作区。没有工作区还有一个好处就是空间占用会更小。

![../_images/git-clone-2.png](img/git-clone-2.png)

使用`--bare`参数克隆 demo 版本库到`/path/to/repos/demo.git`，然后就可以从 demo 版本库向克隆的裸版本库执行推送操作了。（为了说明方便，使用了`/path/to/repos/`作为 Git 裸版本的根路径，在后面的章节中这个目录也作为 Git 服务器端版本库的根路径。可以在磁盘中以 root 账户创建该路径并设置正确的权限。）

```
$ git clone --bare /path/to/my/workspace/demo /path/to/repos/demo.git
Cloning into bare repository /path/to/repos/demo.git...
done.

```

克隆出来的`/path/to/repos/demo.git`目录就是版本库目录，不含工作区。

*   看看`/path/to/repos/demo.git`目录的内容。

    ```
    $ ls -F /path/to/repos/demo.git
    branches/  config  description  HEAD  hooks/  info/  objects/  packed-refs  refs/

    ```

*   还可以看到`demo.git`版本库`core.bare`的配置为`true`。

    ```
    $ git --git-dir=/path/to/repos/demo.git config core.bare
    true

    ```

进入 demo 版本库，生成一些测试提交。

```
$ cd /path/to/my/workspace/demo/
$ git commit --allow-empty -m "sync test 3"
[master d4b42b7] sync test 3
$ git commit --allow-empty -m "sync test 4"
[master 0285742] sync test 4

```

在 demo 版本库向 demo-backup 版本库执行 PUSH 操作，还会有错误么？

*   不带参数执行**git push**，因为未设定上游远程版本库，因此会报错：

    ```
    $ git push
    fatal: No destination configured to push to.

    ```

*   在执行**git push**时使用`/path/to/repos/demo.git`作为参数。

    推送成功。

    ```
    $ git push /path/to/repos/demo.git
    Counting objects: 2, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (2/2), done.
    Writing objects: 100% (2/2), 275 bytes, done.
    Total 2 (delta 1), reused 0 (delta 0)
    Unpacking objects: 100% (2/2), done.
    To /path/to/repos/demo.git
       f86b7bf..0285742  master -> master

    ```

看看`demo.git`版本库，是否已经完成了同步？

```
$ git log --oneline -2
0285742 sync test 4
d4b42b7 sync test 3

```

这个方式实现版本库本地镜像显然是更好的方法，因为可以直接在工作区修改、提交，然后执行**git push**命令实现推送。稍有一点遗憾的是推送命令还需要加上裸版本库的路径。这个遗憾在后面介绍远程版本库的章节会给出解决方案。

## 2.10.4\. 创建生成裸版本库

裸版本库不但可以通过克隆的方式创建，还可以通过**git init**命令以初始化的方式创建。之后的同步方式和上一节大同小异。

![../_images/git-clone-3.png](img/git-clone-3.png)

命令**git init**在“Git 初始化”一章就已经用到了，是用于初始化一个版本库的。之前执行**git init**命令初始化的版本库是带工作区的，如何以裸版本库的方式初始化一个版本库呢？奥秘就在于`--bare`参数。

下面的命令会创建一个空的裸版本库于目录`/path/to/repos/demo-init.git` 中。

```
$ git init --bare /path/to/repos/demo-init.git
Initialized empty Git repository in /path/to/repos/demo-init.git/

```

创建的果真是裸版本库么？

*   看看 `/path/to/repos/demo-init.git` 下的内容：

    ```
    $ ls -F /path/to/repos/demo-init.git
    branches/  config  description  HEAD  hooks/  info/  objects/  refs/

    ```

*   看看这个版本库的配置`core.bare`的值：

    ```
    $ git --git-dir=/path/to/repos/demo-init.git config core.bare
    true

    ```

可是空版本库没有内容啊，那就执行 PUSH 操作为其创建内容呗。

```
$ cd /path/to/my/workspace/demo
$ git push /path/to/repos/demo-init.git
No refs in common and none specified; doing nothing.
Perhaps you should specify a branch such as 'master'.
fatal: The remote end hung up unexpectedly
error: failed to push some refs to '/path/to/repos/demo-init.git'

```

为什么出错了？翻译一下错误输出。

```
$ cd /path/to/my/workspace/demo
$ git push /path/to/repos/demo-init.git
没有指定要推送的引用，而且两个版本库也没有共同的引用。
所以什么也没有做。
可能您需要提供要推送的分支名，如 'master'。
严重错误：远程操作意外终止
错误：部分引用推送失败，至 '/path/to/repos/demo-init.git'

```

关于这个问题详细说明要在后面的章节介绍，这里先说一个省略版：因为`/path/to/repos/demo-init.git` 版本库刚刚初始化完成，还没有任何提交更不要说分支了。当执行**git push**命令时，如果没有设定推送的分支，而且当前分支也没有注册到远程某个分支，将检查远程分支是否有和本地相同的分支名（如 master），如果有，则推送，否则报错。

所以需要把**git push**命令写的再完整一些。像下面这样操作，就可以完成向空的裸版本库的推送。

```
$ git push /path/to/repos/demo-init.git master:master
Counting objects: 26, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (20/20), done.
Writing objects: 100% (26/26), 2.49 KiB, done.
Total 26 (delta 8), reused 0 (delta 0)
Unpacking objects: 100% (26/26), done.
To /path/to/repos/demo-init.git
 * [new branch]      master -> master

```

上面的**git push**命令也可以简写为：**git push /pat h/to/repos/demo-init.git master**。

推送成功了么？看看`demo-init.git`版本库中的提交。

```
$ git --git-dir=/path/to/repos/demo-init.git log --oneline -2
0285742 sync test 4
d4b42b7 sync test 3

```

好了继续在 demo 中执行几次提交。

```
$ cd /path/to/my/workspace/demo/
$ git commit --allow-empty -m "sync test 5"
[master 424aa67] sync test 5
$ git commit --allow-empty -m "sync test 6"
[master 70a5aa7] sync test 6

```

然后再向`demo-init.git`推送。注意这次使用的命令。

```
$ git push /path/to/repos/demo-init.git
Counting objects: 2, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 273 bytes, done.
Total 2 (delta 1), reused 0 (delta 0)
Unpacking objects: 100% (2/2), done.
To /path/to/repos/demo-init.git
   0285742..70a5aa7  master -> master

```

为什么这次使用**git push**命令后面没有跟上分支名呢？这是因为远程版本库（demo-init.git）中已经不再是空版本库了，而且有名为 master 的分支。

通过下面的命令可以查看远程版本库的分支。

```
$ git ls-remote /path/to/repos/demo-init.git
70a5aa7a7469076fd435a9e4f89c4657ba603ced        HEAD
70a5aa7a7469076fd435a9e4f89c4657ba603ced        refs/heads/master

```

至此相信读者已经能够把鸡蛋放在不同的篮子中了，也对 Git 更加的喜爱了吧。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 2.11\. Git 库管理

版本库管理？那不是管理员要干的事情么，怎么放在“Git 独奏”这一部分了？

没有错，这是因为对于 Git，每个用户都是自己版本库的管理员，所以在“Git 独奏”的最后一章，来谈一谈 Git 版本库管理的问题。如果下面的问题您没有遇到或者不感兴趣，读者大可以放心的跳过这一章。

*   从网上克隆来的版本库，为什么对象库中找不到对象文件？而且引用目录里也看不到所有的引用文件？
*   不小心添加了一个大文件到 Git 库中，用重置命令丢弃了包含大文件的提交，可是版本库不见小，大文件仍在对象库中。
*   本地版本库的对象库里文件越来越多，这可能导致 Git 性能的降低。

## 2.11.1\. 对象和引用哪里去了？

从 GitHub 上克隆一个示例版本库，这个版本库在“历史穿梭”一章就已经克隆过一次了，现在要重新克隆一份。为了和原来的克隆相区别，克隆到另外的目录。执行下面的命令。

```
$ cd /path/to/my/workspace/
$ git clone git://github.com/ossxp-com/gitdemo-commit-tree.git i-am-admin
Cloning into i-am-admin...
remote: Counting objects: 65, done.
remote: Compressing objects: 100% (53/53), done.
remote: Total 65 (delta 8), reused 0 (delta 0)
Receiving objects: 100% (65/65), 78.14 KiB | 42 KiB/s, done.
Resolving deltas: 100% (8/8), done.

```

进入克隆的版本库，使用**git show-ref**命令看看所含的引用。

```
$ cd /path/to/my/workspace/i-am-admin
$ git show-ref
6652a0dce6a5067732c00ef0a220810a7230655e refs/heads/master
6652a0dce6a5067732c00ef0a220810a7230655e refs/remotes/origin/HEAD
6652a0dce6a5067732c00ef0a220810a7230655e refs/remotes/origin/master
c9b03a208288aebdbfe8d84aeb984952a16da3f2 refs/tags/A
1a87782f8853c6e11aacba463af04b4fa8565713 refs/tags/B
9f8b51bc7dd98f7501ade526dd78c55ee4abb75f refs/tags/C
887113dc095238a0f4661400d33ea570e5edc37c refs/tags/D
6decd0ad3201ddb3f5b37c201387511059ac120c refs/tags/E
70cab20f099e0af3f870956a3fbbbda50a17864f refs/tags/F
96793e37c7f1c7b2ddf69b4c1e252763c11a711f refs/tags/G
476e74549047e2c5fbd616287a499cc6f07ebde0 refs/tags/H
76945a15543c49735634d58169b349301d65524d refs/tags/I
f199c10c3f1a54fa3f9542902b25b49d58efb35b refs/tags/J

```

其中以`refs/heads/`开头的是分支；以`refs/remotes/`开头的是远程版本库分支在本地的映射，会在后面章节介绍；以`refs/tags/`开头的是里程碑。按照之前的经验，在`.git/refs`目录下应该有这些引用所对应的文件才是。看看都在么？

```
$ find .git/refs/ -type f
.git/refs/remotes/origin/HEAD
.git/refs/heads/master

```

为什么才有两个文件？实际上当运行下面的命令后，引用目录下的文件会更少：

```
$ git pack-refs --all
$ find .git/refs/ -type f
.git/refs/remotes/origin/HEAD

```

那么本应该出现在`.git/refs/`目录下的引用文件都到哪里去了呢？答案是这些文件被打包了，放到一个文本文件`.git/packed-refs`中了。查看一下这个文件中的内容。

```
$ head -5 .git/packed-refs
# pack-refs with: peeled
6652a0dce6a5067732c00ef0a220810a7230655e refs/heads/master
6652a0dce6a5067732c00ef0a220810a7230655e refs/remotes/origin/master
c9b03a208288aebdbfe8d84aeb984952a16da3f2 refs/tags/A
⁸¹⁹⁹³²³⁴fc12a325d303eccea20f6fd629412712

```

再来看看 Git 的对象（commit、blob、tree、tag）在对象库中的存储。通过下面的命令，会发现对象库也不是原来熟悉的模样了。

```
$ find .git/objects/ -type f
.git/objects/pack/pack-969329578b95057b7ea1208379a22c250c3b992a.idx
.git/objects/pack/pack-969329578b95057b7ea1208379a22c250c3b992a.pack

```

对象库中只有两个文件，本应该一个一个独立保存的对象都不见了。读者应该能够猜到，所有的对象文件都被打包到这两个文件中了，其中以`.pack`结尾的文件是打包文件，以`.idx`结尾的是索引文件。打包文件和对应的索引文件只是扩展名不同，都保存于`.git/objects/pack/`目录下。Git 对于以 SHA1 哈希值作为目录名和文件名保存的对象有一个术语，称为松散对象。松散对象打包后会提高访问效率，而且不同的对象可以通过增量存储节省磁盘空间。

可以通过 Git 一个底层命令可以查看索引中包含的对象：

```
$ git show-index < .git/objects/pack/pack-*.idx | head -5
661 0cd7f2ea245d90d414e502467ac749f36aa32cc4 (0793420b)
63020 1026d9416d6fc8d34e1edfb2bc58adb8aa5a6763 (ed77ff72)
3936 15328fc6961390b4b10895f39bb042021edd07ea (13fb79ef)
3768 1a588ca36e25f58fbeae421c36d2c39e38e991ef (86e3b0bd)
2022 1a87782f8853c6e11aacba463af04b4fa8565713 (e269ed74)

```

为什么克隆远程版本库就可以产生对象库打包以及引用打包的效果呢？这是因为克隆远程版本库时，使用了“智能”的通讯协议，远程 Git 服务器将对象打包后传输给本地，形成本地版本库的对象库中的一个包含所有对象的包以及索引文件。无疑这样的传输方式——按需传输、打包传输，效率最高。

克隆之后的版本库在日常的提交中，产生的新的对象仍旧以松散对象存在，而不是以打包的形式，日积月累会在本地版本库的对象库中形成大量的松散文件。松散对象只是进行了压缩，而没有（打包文件才有的）增量存储的功能，会浪费磁盘空间，也会降低访问效率。更为严重的是一些非正式的临时对象（暂存区操作中产生的临时对象）也以松散对象的形式保存在对象库中，造成磁盘空间的浪费。下一节就着手处理临时对象的问题。

## 2.11.2\. 暂存区操作引入的临时对象

暂存区操作有可能在对象库中产生临时对象，例如文件反复的修改和反复的向暂存区添加，或者添加到暂存区后不提交甚至直接撤销，就会产生垃圾数据占用磁盘空间。为了说明临时对象的问题，需要准备一个大的压缩文件，10MB 即可。

在 Linux 上与内核匹配的`initrd`文件（内核启动加载的内存盘）就是一个大的压缩文件，可以用于此节的示例。将大的压缩文件放在版本库外的一个位置上，因为这个文件会多次用到。

```
$ cp /boot/initrd.img-2.6.32-5-amd64 /tmp/bigfile
$ du -sh bigfile
11M     bigfile

```

将这个大的压缩文件复制到工作区中，拷贝两份。

```
$ cd /path/to/my/workspace/i-am-admin
$ cp /tmp/bigfile bigfile
$ cp /tmp/bigfile bigfile.dup

```

然后将工作区中两个内容完全一样的大文件加入暂存区。

```
$ git add bigfile bigfile.dup

```

查看一下磁盘空间占用：

*   工作区连同版本库共占用 33MB。

    ```
    $ du -sh .
    33M     .

    ```

*   其中版本库只占用了 11MB。版本库空间占用是工作区的一半。

    如果再有谁说版本库空间占用一定比工作区大，可以用这个例子回击他。

    ```
    $ du -sh .git/
    11M     .git/

    ```

看看版本库中对象库内的文件，会发现多出了一个松散对象。之所以添加两个文件而只有一个松散对象，是因为 Git 对于文件的保存是将内容保存为 blob 对象中，和文件名无关，相同内容的不同文件会共享同一个 blob 对象。

```
$ find .git/objects/ -type f
.git/objects/2e/bcd92d0dda2bad50c775dc662c6cb700477aff
.git/objects/pack/pack-969329578b95057b7ea1208379a22c250c3b992a.idx
.git/objects/pack/pack-969329578b95057b7ea1208379a22c250c3b992a.pack

```

如果不想提交，想将文件撤出暂存区，则进行如下操作。

*   当前暂存区的状态。

    ```
    $ git status -s
    A  bigfile
    A  bigfile.dup

    ```

*   将添加的文件撤出暂存区。

    ```
    $ git reset HEAD

    ```

*   通过查看状态，看到文件被撤出暂存区了。

    ```
    $ git status -s
    ?? bigfile
    ?? bigfile.dup

    ```

文件撤出暂存区后，在对象库中产生的 blob 松散对象仍然存在，通过查看版本库的磁盘占用就可以看出来。

```
$ du -sh .git/
11M     .git/

```

Git 提供了**git fsck**命令，可以查看到版本库中包含的没有被任何引用关联松散对象。

```
$ git fsck
dangling blob 2ebcd92d0dda2bad50c775dc662c6cb700477aff

```

标识为 dangling 的对象就是没有被任何引用直接或者间接关联到的对象。这个对象就是前面通过暂存区操作引入的大文件的内容。如何将这个文件从版本库中彻底删除呢？Git 提供了一个清理的命令：

```
$ git prune

```

用**git prune**清理之后，会发现：

*   用**git fsck**查看，没有未被关联到的松散对象。

    ```
    $ git fsck

    ```

*   版本库的空间占用也小了 10MB，证明大的临时对象文件已经从版本库中删除了。

    ```
    $ du -sh .git/
    236K    .git/

    ```

## 2.11.3\. 重置操作引入的对象

上一节用**git prune**命令清除暂存区操作时引入的临时对象，但是如果是用重置命令抛弃的提交和文件就不会轻易的被清除。下面用同样的大文件提交到版本库中试验一下。

```
$ cd /path/to/my/workspace/i-am-admin
$ cp /tmp/bigfile bigfile
$ cp /tmp/bigfile bigfile.dup

```

将这两个大文件提交到版本库中。

*   添加到暂存区。

    ```
    $ git add bigfile bigfile.dup

    ```

*   提交到版本库。

    ```
    $ git commit -m "add bigfiles."
    [master 51519c7] add bigfiles.
     2 files changed, 0 insertions(+), 0 deletions(-)
     create mode 100644 bigfile
     create mode 100644 bigfile.dup

    ```

*   查看版本库的空间占用。

    ```
    $ du -sh .git/
    11M     .git/

    ```

做一个重置操作，抛弃刚刚针对两个大文件做的提交。

```
$ git reset --hard HEAD^

```

重置之后，看看版本库的变化。

*   版本库的空间占用没有变化，还是那么“庞大”。

    ```
    $ du -sh .git/
    11M     .git/

    ```

*   查看对象库，看到三个松散对象。

    ```
    $ find .git/objects/ -type f
    .git/objects/info/packs
    .git/objects/2e/bcd92d0dda2bad50c775dc662c6cb700477aff
    .git/objects/d9/38dee8fde4e5053b12406c66a19183a24238e1
    .git/objects/51/519c7d8d60e0f958e135e8b989a78e84122591
    .git/objects/pack/pack-969329578b95057b7ea1208379a22c250c3b992a.idx
    .git/objects/pack/pack-969329578b95057b7ea1208379a22c250c3b992a.pack

    ```

*   这三个松散对象分别对应于撤销的提交，目录树，以及大文件对应的 blob 对象。

    ```
    $ git cat-file -t 51519c7
    commit
    $ git cat-file -t d938dee
    tree
    $ git cat-file -t 2ebcd92
    blob

    ```

向上一节一样，执行**git prune**命令，期待版本库空间占用会变小。可是：

*   版本库空间占用没有变化！

    ```
    $ git prune
    $ du -sh .git/
    11M     .git/

    ```

*   执行**git fsck**也看不到未被关联到的对象。

    ```
    $ git fsck

    ```

*   除非像下面这样执行。

    ```
    $ git fsck --no-reflogs
    dangling commit 51519c7d8d60e0f958e135e8b989a78e84122591

    ```

还记得前面章节中介绍的 reflog 么？reflog 是防止误操作的最后一道闸门。

```
$ git reflog
6652a0d HEAD@{0}: HEAD^: updating HEAD
51519c7 HEAD@{1}: commit: add bigfiles.

```

可以看到撤销的操作仍然记录在 reflog 中，正因如此 Git 认为撤销的提交和大文件都还被可以被追踪到，还在使用着，所以无法用**git prune**命令删除。

如果确认真的要丢弃不想要的对象，需要对版本库的 reflog 做过期操作，相当于将`.git/logs/`下的文件清空。

*   使用下面的 reflog 过期命令做不到让刚刚撤销的提交过期，因为 reflog 的过期操作缺省只会让 90 天前的数据过期。

    ```
    $ git reflog expire --all
    $ git reflog
    6652a0d HEAD@{0}: HEAD^: updating HEAD
    51519c7 HEAD@{1}: commit: add bigfiles.

    ```

*   需要要为**git reflog**命令提供`--expire=<date>`参数，强制`<date>`之前的记录全部过期。

    ```
    $ git reflog expire --expire=now --all
    $ git reflog

    ```

使用`now`作为时间参数，让 reflog 的全部记录都过期。没有了 reflog，即回滚的添加大文件的提交从 reflog 中看不到后，该提交对应的 commit 对象、tree 对象和 blob 对象就会成为未被关联的 dangling 对象，可以用**git prune**命令清理。下面可以看到清理后，版本库变小了。

```
$ git prune
$ du -sh .git/
244K    .git/

```

## 2.11.4\. Git 管家：`git gc`

前面两节介绍的是比较极端的情况，实际操作中会很少用到**git prune**命令来清理版本库，而是会使用一个更为常用的命令**git gc**。命令**git gc**就好比 Git 版本库的管家，会对版本库进行一系列的优化动作。

*   对分散在`.git/refs`下的文件进行打包，打包到文件`.git/packed-refs`中。

    如果没有将配置`gc.packrefs`关闭，就会执行命令：**git pack-refs --all --prune**实现对引用的打包。

*   丢弃 90 天前的 reflog 记录。

    会运行使 reflog 过期命令：**git reflog expire --all**。因为采用了缺省参数调用，因此只会清空 reflog 中 90 天前的记录。

*   对松散对象进行打包。

    运行**git repack**命令，凡是有引用关联的对象都被打在包里，未被关联的对象仍旧以松散对象形式保存。

*   清除未被关联的对象。缺省只清除 2 周以前的未被关联的对象。

    可以向**git gc**提供`--prune=<date>`参数，其中的时间参数传递给**git prune --expire <date>**，实现对指定日期之前的未被关联的松散对象进行清理。

*   其他清理。

    如运行**git rerere gc**对合并冲突的历史记录进行过期操作。

从上面的描述中可见命令**git gc**完成了相当多的优化和清理工作，并且最大限度照顾了安全性的需要。例如像暂存区操作引入的没有关联的临时对象会最少保留 2 个星期，而因为重置而丢弃的提交和文件则会保留最少 3 个月。

下面就把前面的例子用**git gc**再执行一遍，不过这一次添加的两个大文件要稍有不同，以便看到**git gc**打包所实现的对象增量存储的效果。

复制两个大文件到工作区。

```
$ cp /tmp/bigfile bigfile
$ cp /tmp/bigfile bigfile.dup

```

在文件`bigfile.dup`后面追加些内容，造成`bigfile`和`bigfile.dup`内容不同。

```
$ echo "hello world" >> bigfile.dup

```

将这两个稍有不同的文件提交到版本库。

```
$ git add bigfile bigfile.dup
$ git commit -m "add bigfiles."
[master c62fa4d] add bigfiles.
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 bigfile
 create mode 100644 bigfile.dup

```

可以看到版本库中提交进来的两个不同的大文件是不同的对象。

```
$ git ls-tree HEAD | grep bigfile
100644 blob 2ebcd92d0dda2bad50c775dc662c6cb700477aff    bigfile
100644 blob 9e35f946a30c11c47ba1df351ca22866bc351e7b    bigfile.dup

```

做版本库重置，抛弃最新的提交，即抛弃添加两个大文件的提交。

```
$ git reset --hard HEAD^
HEAD is now at 6652a0d Add Images for git treeview.

```

此时的版本库有多大呢，还是像之前添加两个相同的大文件时占用 11MB 空间么？

```
$ du -sh .git/
22M     .git/

```

版本库空间占用居然扩大了一倍！这显然是因为两个大文件分开存储造成的。可以用下面的命令在对象库中查看对象的大小。

```
$ find .git/objects -type f -printf "%-20p\t%s\n"
.git/objects/0c/844d2a072fd69e71638558216b69ebc57ddb64  233
.git/objects/2e/bcd92d0dda2bad50c775dc662c6cb700477aff  11184682
.git/objects/9e/35f946a30c11c47ba1df351ca22866bc351e7b  11184694
.git/objects/c6/2fa4d6cb4c082fadfa45920b5149a23fd7272e  162
.git/objects/info/packs 54
.git/objects/pack/pack-969329578b95057b7ea1208379a22c250c3b992a.idx   2892
.git/objects/pack/pack-969329578b95057b7ea1208379a22c250c3b992a.pack  80015

```

输出的每一行用空白分隔，前面是文件名，后面是以字节为单位的文件大小。从上面的输出可以看出来，打包文件很小，但是有两个大的文件各自占用了 11MB 左右的空间。

执行**git gc**并不会删除任何对象，因为这些对象都还没有过期。但是会发现版本库的占用变小了。

*   执行**git gc**对版本库进行整理。

    ```
    $ git gc
    Counting objects: 69, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (49/49), done.
    Writing objects: 100% (69/69), done.
    Total 69 (delta 11), reused 63 (delta 8)

    ```

*   版本库空间占用小了一半！

    ```
    $ du -sh .git/
    11M     .git/

    ```

*   原来是因为对象库重新打包，两个大文件采用了增量存储使得版本库变小。

    ```
    $ find .git/objects -type f -printf "%-20p\t%s\n" | sort
    .git/objects/info/packs 54
    .git/objects/pack/pack-7cae010c1b064406cd6c16d5a6ab2f446de4076c.idx 3004
    .git/objects/pack/pack-7cae010c1b064406cd6c16d5a6ab2f446de4076c.pack 11263033

    ```

如果想将抛弃的历史数据彻底丢弃，如下操作。

*   不再保留 90 天的 reflog，而是将所有 reflog 全部即时过期。

    ```
    $ git reflog expire --expire=now --all

    ```

*   通过**git fsck**可以看到有提交成为了未被关联的提交。

    ```
    $ git fsck
    dangling commit c62fa4d6cb4c082fadfa45920b5149a23fd7272e

    ```

*   这个未被关联的提交就是删除大文件的提交。

    ```
    $ git show c62fa4d6cb4c082fadfa45920b5149a23fd7272e
    commit c62fa4d6cb4c082fadfa45920b5149a23fd7272e
    Author: Jiang Xin <jiangxin@ossxp.com>
    Date:   Thu Dec 16 20:18:38 2010 +0800

        add bigfiles.

    diff --git a/bigfile b/bigfile
    new file mode 100644
    index 0000000..2ebcd92
    Binary files /dev/null and b/bigfile differ
    diff --git a/bigfile.dup b/bigfile.dup
    new file mode 100644
    index 0000000..9e35f94
    Binary files /dev/null and b/bigfile.dup differ

    ```

*   不带参数调用**git gc**虽然不会清除尚未过期（未到 2 周）的大文件，但是会将未被关联的对象从打包文件中移出，成为松散文件。

    ```
    $ git gc
    Counting objects: 65, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (45/45), done.
    Writing objects: 100% (65/65), done.
    Total 65 (delta 8), reused 63 (delta 8)

    ```

*   未被关联的对象重新成为松散文件，所以`.git`版本库的空间占用又反弹了。

    ```
    $ du -sh .git/
    22M     .git/
    $ find .git/objects -type f -printf "%-20p\t%s\n" | sort
    .git/objects/0c/844d2a072fd69e71638558216b69ebc57ddb64  233
    .git/objects/2e/bcd92d0dda2bad50c775dc662c6cb700477aff  11184682
    .git/objects/9e/35f946a30c11c47ba1df351ca22866bc351e7b  11184694
    .git/objects/c6/2fa4d6cb4c082fadfa45920b5149a23fd7272e  162
    .git/objects/info/packs 54
    .git/objects/pack/pack-969329578b95057b7ea1208379a22c250c3b992a.idx 2892
    .git/objects/pack/pack-969329578b95057b7ea1208379a22c250c3b992a.pack 80015

    ```

*   实际上如果使用立即过期参数`--prune=now`调用**git gc**，就不用再等 2 周了，直接就可以完成对未关联的对象的清理。

    ```
    $ git gc --prune=now
    Counting objects: 65, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (45/45), done.
    Writing objects: 100% (65/65), done.
    Total 65 (delta 8), reused 65 (delta 8)

    ```

*   清理过后，版本库的空间占用降了下来。

    ```
    $ du -sh .git/
    240K    .git/

    ```

## 2.11.5\. Git 管家的自动执行

对于老版本库的 Git，会看到帮助手册中建议用户对版本库进行周期性的整理，以便获得更好的性能，尤其是对于规模比较大的项目，但是对于整理的周期都语焉不详。

实际上对于 1.6.6 及以后版本的 Git 已经基本上不需要手动执行**git gc**命令了，因为部分 Git 命令会自动调用**git gc --auto**命令，在版本库确实需要整理的情况下自动开始整理操作。

目前有如下 Git 命令会自动执行**git gc --auto**命令，实现对版本库的按需整理。

*   执行命令**git merge**进行合并操作后，对版本库进行按需整理。

*   执行命令**git receive-pack**，即版本库接收其他版本库推送（push）的提交后，版本库会做按需整理操作。

    当版本库接收到其他版本库的推送（push）请求时，会调用**git receive-pack**命令以接收请求。在接收到推送的提交后，对版本库进行按需整理。

*   执行命令**git rebase -i**进行交互式变基操作后，会对版本库进行按需整理。

*   执行命令**git am**对 mbox 邮箱中通过邮件提交的补丁在版本库中进行应用的操作后，会对版本库做按需整理操作。

对于提供共享式“写操作”的 Git 版本库，可以免维护。所谓的共享式写操作，就是版本库作为一个裸版本库放在服务器上，团队成员可以通过推送（push）操作将提交推送到共享的裸版本中。每一次推送操作都会触发**git gc --auto**命令，对版本库进行按需整理。

对于非独立工作的本地工作区，也可以免维护。因为和他人协同工作的本地工作区会经常执行**git pull**操作从他人版本库或者从共享的版本库拉回新提交，执行**git pull**操作会，会触发**git merge**操作，因此也会对本地版本库进行按需整理。

Git 管家命令使用`--auto`参数调用，会进行按需整理。因为版本库整理操作对于大的项目可能会非常费时，因此实际的整理并不会经常被触发，即有着非常苛刻的触发条件。想要观察到触发版本库整理操作是非常不容易的事情。

主要的触发条件是：松散对象只有超过一定的数量时才会执行。而且在统计松散对象数量时，为了降低在`.git/objects/`目录下搜索松散对象对系统造成的负担，实际采取了取样搜索，即只会对对象库下一个子目录`.git/objects/17`进行文件搜索。在缺省的配置下，只有该目录中对象数目超过 27 个，才会触发版本库的整理。至于为什么只在对象库下选择了一个子目录进行松散对象的搜索，这是因为 SHA1 哈希值是完全随机的，文件在由前两位哈希值组成的目录中差不多是平均分布的。至于为什么选择`17`，不知道对于作者 Junio C Hamano 有什么特殊意义，也许是向 Linus Torvalds 被评选为二十世纪最有影响力的 100 人中排名第 17 位而进行致敬。

可以通过配置**gc.auto**的值，调整 Git 管家自动运行时触发版本库整理操作的频率，但是注意不要将**gc.auto**设置为 0，否则**git gc --auto**命令永远不会触发版本库的整理。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.