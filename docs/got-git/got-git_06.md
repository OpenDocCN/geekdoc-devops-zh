# 6\. 迁移到 Git

随着 Git 版本控制系统的成熟，越来越多的项目把版本控制系统迁移到了 Git 上。迁移大多是无损的，即迁移到 Git 后忠实的保留之前的变更历史、分支和里程碑。如果正打算迁移版本控制系统，本篇介绍的版本库迁移方法和注意事项将会为你提供帮助。

本篇首先会介绍 CVS、Subversion、Mercurial 等几个著名的开源版本控制系统如何迁移到 Git 上。除此之外的其他版本控制系统也许可以找到类似的迁移方案，或者可以针对**git fast-import**通过编程定制转换过程。在本篇的最后，还会介绍一个让 Git 版本库改头换面的更为强大的工具**git filter-branch**。

目录:

*   6.1\. CVS 版本库到 Git 的迁移
    *   6.1.1\. 安装 cvs2svn（含 cvs2git）
    *   6.1.2\. 版本库转换（命令行参数模式）
    *   6.1.3\. 版本库转换（配置文件模式）
    *   6.1.4\. 迁移后版本库检查
        *   6.1.4.1\. 文件名和日志的中文
        *   6.1.4.2\. 图片文件被破坏
        *   6.1.4.3\. `.cvsignore`文件的转换
        *   6.1.4.4\. 迁移后的测试
*   6.2\. 更多版本控制系统的迁移
    *   6.2.1\. SVN 版本库到 Git 的迁移
    *   6.2.2\. Hg 版本库到 Git 的迁移
    *   6.2.3\. 通用版本库迁移
    *   6.2.4\. Git 版本库整理
        *   6.2.4.1\. 环境变量过滤器
        *   6.2.4.2\. 树过滤器
        *   6.2.4.3\. 暂存区过滤器
        *   6.2.4.4\. 父节点过滤器
        *   6.2.4.5\. 提交说明过滤器
        *   6.2.4.6\. 提交过滤器
        *   6.2.4.7\. 里程碑名字过滤器
        *   6.2.4.8\. 子目录过滤器

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 6.1\. CVS 版本库到 Git 的迁移

CVS 是最早广泛使用的版本控制系统，因为其服务器端存储结构的简单直白，至今仍受到不少粉丝的钟爱。但是毕竟是几十年前的产物，因为设计上的原因导致缺乏现代版本控制系统的一些必须功能，如：没有原子提交，分支管理不便（慢），分支合并困难因为合并过程缺乏跟踪，不支持文件名/目录名的修改等等。很多 CVS 的用户都已经转换到 Subversion 这一更好的集中式版本控制系统了。如果还在使用 CVS，那么可以考虑直接迁移到 Git。

CVS 到 Git 迁移可以使用 cvs2svn 软件包中的**cvs2git**命令。为什么该项目叫做 cvs2svn 而非 cvs2git 呢？这是因为该项目最早是为 CVS 版本库迁移到 Subversion 版本库服务的，只是最近才增加了 CVS 版本转换为 Git 版本库的功能。cvs2svn 将 CVS 转换为 Subversion 版本库的过程一直以稳定著称，在 cvs2svn 2.1 版开始，增加了将 CVS 版本库转换为 Git 版本库的功能，无疑让这个工具更具生命力，也减少了之前 CVS 到 Git 库的转换环节。在推出 cvs2git 功能之前，通常的 CVS 到 Git 迁移路径是用 cvs2svn 将 CVS 版本库迁移到 Subversion 版本库，再用 git-svn 将 Subversion 版本库迁移到 Git。

关于 cvs2svn 及 cvs2git 可以参考下面的链接：

*   [`cvs2svn.tigris.org/cvs2svn.html`](http://cvs2svn.tigris.org/cvs2svn.html)
*   [`cvs2svn.tigris.org/cvs2git.html`](http://cvs2svn.tigris.org/cvs2git.html)

## 6.1.1\. 安装 cvs2svn（含 cvs2git）

**Linux 下 cvs2svn 的安装**

大部分 Linux 发行版都提供 cvs2svn 的发布包，可以直接用平台自带的 cvs2svn 软件包。cvs2svn 在 2.1 版本之后开始引入了到 Git 库的转换，2.3.0 版本有了独立的 cvs2git 转换脚本，cvs2git 正在逐渐完善当中，因此尽量选择最新版本的 cvs2svn。

例如在 Debian 或 Ubuntu 下，可以通过下面命令查看源里面的 cvs2svn 版本。

```
$ aptitude versions cvs2svn
p   2.1.1-1               stable                     990
pi  2.3.0-2               testing,unstable           1001

```

可以看出 Debian 的 Testing 和 Sid 的仓库中才有 2.3.0 版本的 cvs2svn。于是执行下面的命令安装在 Testing 版本才有的 2.3.0-2 版本的 cvs2svn：

```
$ sudo aptitude cvs2svn/testing

```

如果对应的 Linux 发行版没有对应的版本也可以从源码开始安装。cvs2svn 的官方版本库在[`cvs2svn.tigris.org/svn/cvs2svn/trunk`](http://cvs2svn.tigris.org/svn/cvs2svn/trunk)，已经有人将 cvs2svn 项目转换为 Git 库。可以从 Git 库下载源码，安装 cvs2svn。

*   下载 cvs2svn 源代码

    ```
    $ git clone git://repo.or.cz/cvs2svn.git

    ```

*   进入 cvs2svn 源码目录，安装 cvs2svn。

    ```
    $ cd cvs2svn
    $ sudo make install

    ```

*   安装用户手册。

    ```
    $ sudo make man

    ```

cvs2svn 对其他软件包的依赖：

*   Python 2.4 或以上版本（Python 3.x 暂不支持）。
*   RCS：如果在转换中使用了`--use-rcs`，就需要安装 RCS 软件包。参见：[`www.cs.purdue.edu/homes/trinkle/RCS/`](http://www.cs.purdue.edu/homes/trinkle/RCS/)。
*   CVS：如果在转换中使用了`--use-cvs`，就需要安装 CVS 软件包。参见：[`ccvs.cvshome.org/`](http://ccvs.cvshome.org/)。
*   Git：1.5.4.4 或以上的版本。之前版本的 Git 的`git fast-import` 命令有 Bug，加载 cvs2git 导出文件有问题。

**Mac OS X 下 cvs2svn 的安装**

Mac OS X 下可以使用 Homebrew 安装 cvs2svn。

*   Mac OS X 缺省安装的 Python 缺少 cvs2svn 依赖的 gdbm 模组，先用 Homebrew 来重新安装 python。

    ```
    $ brew install python

    ```

*   安装 cvs2svn

    ```
    $ export PATH=/usr/local/bin:$PATH
    $ brew install cvs2svn

    ```

## 6.1.2\. 版本库转换（命令行参数模式）

转换 CVS 版本库的注意事项：

*   使用 cvs2git 对 CVS 版本库转换，必须在 CVS 的服务器端执行，即 cvs2git 必须能够通过文件系统直接访问 CVS 版本库中的`,v`文件。

*   在转换前，确保所有人的修改都已经提交到 CVS 版本库中。

*   在转换前，停止 CVS 版本库的访问，以免在转换过程中有新提交写入。

*   在转换前，对原始版本库进行备份，以免误操作对版本库造成永久的破坏。

*   在转换完成后，永久停止 CVS 版本库的写入服务，可以仅开放只读服务。

    这是由于 cvs2git 是一次性操作，不能对 CVS 后续提交执行增量式的到 Git 库转换，因此当 CVS 版本库转换完毕后，须停止 CVS 服务。

*   先做小规模的试验性转换。

    转换 CVS 版本库切忌一上来就对整个版本库进行转换，等到发现日志乱码、文件名乱码、提交者 ID 不完全后重新转换会浪费大量时间。

    应该先选择 CVS 版本库中的部分文件和目录作为样本，进行小规模的转换测试。

*   不要对包含`CVSROOT`目录的版本库的根进行操作，可以先对服务器目录布局进行调整。

    如果转换直接针对包含`CVSROOT`目录的版本库根目录进行操作，会导致`CVSROOT`目录下的文件及更改历史也被纳入到 Git 版本库，这是不需要的。

**检查 CVS 版本库中的文件名乱码**

CVS 中保存的数据在服务器端直接和同名文件（文件多了一个“`,v`”后缀）相对应，当转换的 CVS 版本库是从其他平台（如 Windows）拷贝过来的，就可能因为平台本身字符集不一致导致中文文件名包含乱码，在 CVS 版本库转换过程造成乱码。可以先对有问题的目录名和文件名进行重命名，转换为当前平台正确的编码。

**小规模的转换试验**

前面提到过，最好先进行小规模的转换试验，然后再对整个版本库进行转换。例如版本库是如下方式部署：`CVSROOT`为`/cvshome/user`，需要将之下的`jiangxin/homepage/worldhello`转换为一个 Git 版本库。先检查一下版本库中的数据，找出典型的目录用于转换。

典型的数据是这样的：包含中文文件名，并且日志中包含中文。例如在版本库中，执行 CVS 查看日志命令，看到类似下面的输出。

```
RCS file: /cvshome/user/jiangxin/homepage/worldhello/archive/2003/.mhonarc.db,v
Working file: archive/2003/.mhonarc.db
head: 1.16
branch:
locks: strict
access list:
symbolic names:
keyword substitution: kv
total revisions: 16;    selected revisions: 16
description:
----------------------------
revision 1.16
date: 2004-09-21 15:56:30 +0800;  author: jiangxin;  state: Exp;  lines: +3 -3;  commitid: c2c414fdea20000;
<D0><U+07B8><C4><D3>ʼ<FE><B5><D8><A3><BB>
<D0><U+07B8><C4><CB><D1><CB><F7><D2><FD><C7>棻
----------------------------

```

日志乱码是因为 CVS 并没有对日志的字符转换为统一的 UTF-8 字符集。此版本库之前用 CVSNT 维护，缺省字符集为 GBK。那么就先对有乱码的这一个目录进行一下试验性的转换。

*   调用 cvs2git 执行转换，产生两个导出文件。这两个导出文件将作为 Git 版本库创建时的导入文件。

    命令行用了两个`--encoding`参数设置编码，会依次进行尝试将日志中的非 Ascii 字符转换为 UTF-8。

    ```
    $ cvs2git --blobfile git-blob.dat --dumpfile git-dump.dat \
      --encoding utf8 --encoding gbk --username cvs2git \
      /cvshome/user/jiangxin/homepage/worldhello/archive/2003/

    ```

*   成功导出后，产生两个导出文件，一个保存各个文件的各个不同版本的数据内容，即在命令行指定的输出文件`git-blob.dat`。另外一个文件是上面命令行指定的`git-dump.dat`用于保存各个提交相关信息（提交者、提交时间、提交日志等）。

    ```
    $ du -sh git*dat
    9.8M    git-blob.dat
    24K     git-dump.dat

    ```

    可以看出保存文件内容的导出文件（`git-blob.dat`）相对更大一些。

*   创建空的 Git 库，使用 Git 通用的数据迁移命令**git fast-import**将 cvs2git 的导出文件导入版本库中。

    ```
    $ mkdir test
    $ cd test
    $ git init
    $ cat ../git-blob.dat ../git-dump.dat | git fast-import

    ```

*   检查导出结果。

    ```
    $ git reset HEAD
    $ git checkout .
    $ git log -1
    commit 8334587cb241076bcd2e710b321e8e16b5e46bba
    Author: jiangxin <>
    Date:   Tue Sep 21 07:56:31 2004 +0000

        修改邮件地址；
        修改搜索引擎；

    ```

很好，导出的 Git 库的日志，中文乱码问题已经解决。但是会发现提交日志中的作者（Author）字段信息不完整：缺乏邮件地址。这是因为 CVS 的提交者仅为用户登录 ID，而 Git 的提交者信息还要包含邮件地址。cvs2git 提供参数实现两种提交者 ID 的转换，不过需要通过配置文件予以指定，这就需要采用下面介绍的转换方法。

## 6.1.3\. 版本库转换（配置文件模式）

使用命令行参数调用 cvs2git 麻烦、可重用性差，而且可配置项有限。采用 cvs2git 配置文件模式运行不但能够简化 cvs2git 的命令行参数，而且能够提供更多的命令行无法提供的配置项，可以更精确的对 CVS 到 Git 版本库转换进行定制。

cvs2svn 软件包提供了一个 cvs2git 的配置示例文件，见源码中的`cvs2git-example.options`[[1]](#id9)。将该示例文件在本地复制一份，对其进行更改。该文件是 Python 代码格式，以“#”（井号）开始的行是注释，文件缩进不要随意更改，因为缩进也是 Python 语法的一部分。可以考虑针对下列选项进行定制。

*   设置 CVS 版本库位置。

    使用配置文件方式运行 cvs2git，只能在配置文件中设置要转换的 CVS 版本库位置，而不能在命令行进行设置。具体说是在配置文件的最后面`run_options`的`set_project`方法中指定。

    ```
    run_options.set_project(
        # CVS 版本库的位置（不是工作区，而是包含,v 文件的版本库）
        # 可以是版本库下的子目录。
        r'/cvshome/user/jiangxin/homepage/worldhello/archive/2003/',

    ```

*   导出文件的位置也在配置文件中预先设置好了，也不能再在命令行中设置。

    *   导出 CVS 版本文件的内容至文件`cvs2svn-tmp/git-blob.dat`。

        缺省使用**cvs**命令做导出，最稳定。

        ```
  ctx.revision_collector = GitRevisionCollector(
            'cvs2svn-tmp/git-blob.dat',

            #RCSRevisionReader(co_executable=r'co'),
            CVSRevisionReader(cvs_executable=r'cvs'),
            )

        ```

    *   另外一个导出文件的缺省位置：`cvs2svn-tmp/git-dump.dat`。

        ```
  ctx.output_option = GitOutputOption(
            os.path.join(ctx.tmpdir, 'git-dump.dat'),

            # The blobs will be written via the revision recorder, so in
            # OutputPass we only have to emit references to the blob marks:
            GitRevisionMarkWriter(),

            # Optional map from CVS author names to git author names:
            author_transforms=author_transforms,
            )

        ```

*   设置无提交用户信息时使用的用户名。这个用户名可以用接下来的用户映射转换为 Git 用户名。

    ```
    ctx.username = 'cvs2svn'

    ```

*   建立 CVS 用户和 Git 用户之间的映射。Git 用户名可以用 Python 的 tuple 语法`(name, email)`或者用字符串`name <email>`来表示。

    ```
    author_transforms={
        'jiangxin'  : ('Jiang Xin', 'jiangxin@ossxp.com'),
        'dev1'      : u'开发者 1 <dev1@ossxp.com>',

        'cvs2svn'   : 'cvs2svn <admin@example.com>',
        }

    ```

*   字符集编码。即如何转换日志中的用户名、提交说明以及文件名的编码。

    对于可能在日志中出现中，必须做出下面类似设置。编码的顺序对输出也会有影响，一般将 utf8 放在 gbk 之前能保证当日志中同时出现两种编码时都能正常转换[[2]](#id10)。

    ```
    ctx.cvs_author_decoder = CVSTextDecoder(
        [
            'utf8',
            'gbk',
            ],
        fallback_encoding='gbk'
        )

    ctx.cvs_log_decoder = CVSTextDecoder(
        [
            'utf8',
            'gbk',
            ],
        fallback_encoding='gbk'
        )

    ctx.cvs_filename_decoder = CVSTextDecoder(
        [
            'utf8',
            'gbk',
            ],
        #fallback_encoding='ascii'
        )

    ```

*   是否忽略`.cvsignore`文件？缺省保留`.cvsignore`文件。

    无论选择保留或是不保留，最好在转换后手工进行`.cvsignore`到`.gitignore`的转换。因为 cvs2git 不能自动将`.cvsignore`文件转换为`.gitignore`文件。

    ```
    ctx.keep_cvsignore = True

    ```

*   对文件换行符等的处理。下面的配置原本是针对 CVS 到 Subversion 的属性转换，但是也会影响到 Git 转换时的换行符设置。

    维持默认值比较安全。

    ```
    ctx.file_property_setters.extend([
        # 基于配置文件设置文件的 mime 类型
        #MimeMapper(r'/etc/mime.types', ignore_case=False),

        # 对于二进制文件（-kb 模式）不设置 svn:eol-style 属性（对于 Subverson 来说）
        CVSBinaryFileEOLStyleSetter(),

        # 如果文件是二进制，并且 svn:mime-type 没有设置，将其设置为 'application/octet-stream'。
        CVSBinaryFileDefaultMimeTypeSetter(),

        # 如果希望根据文件的 mime 类型来判断文件的换行符，打开下面注释
        #EOLStyleFromMimeTypeSetter(),

        # 如果上面的规则没有为文件设置换行符类型，则为 svn:eol-style 设置缺省类型。
        # （二进制文件除外）
        # 缺省把文件视为二进制，不为其设置换行符类型，这样最安全。
        # 如果确认 CVS 的二进制文件都已经设置了 -kb 参数，或者使用上面的规则能够对
        # 文件类型做出正确判断，也可以使用下面参数为非二进制文件设置缺省换行符号。
        ## 'native': 服务器端文件的换行符保存为 LF，客户端根据需要自动转换。
        ## 'CRLF':   服务器端文件的换行符保存为 CRLF，客户端亦为 CRLF。
        ## 'CR':     服务器端文件的换行符保存为 CR，客户端亦为 CR。
        ## 'LF':     服务器端文件的换行符保存为 LF，客户端亦为 LF。
        DefaultEOLStyleSetter(None),

        # 如果文件没有设置 svn:eol-style ，也不为其设置 svn:keywords 属性
        SVNBinaryFileKeywordsPropertySetter(),

        # 如果 svn:keywords 未色环只，基于文件的 CVS 模式进行设置。
        KeywordsPropertySetter(config.SVN_KEYWORDS_VALUE),

        # 设置文件的 svn:executable 属性，如果文件在 CVS 中标记为可执行文件。
        ExecutablePropertySetter(),
        ])

    ```

*   是否只迁移主线，忽略分支和里程碑？

    缺省对所有分支和里程碑都进行转换。如果选择忽略分支和里程碑，将`False`修改为`True`。

    ```
    ctx.trunk_only = False

    ```

*   分支和里程碑迁移及转换。

    ```
    global_symbol_strategy_rules = [

        # 和正则表达式匹配的 CVS 标识，转换为 Git 的分支。
        #ForceBranchRegexpStrategyRule(r'branch.*'),

        # 和正则表达式匹配的 CVS 标识，转换为 Git 的里程碑。
        #ForceTagRegexpStrategyRule(r'tag.*'),

        # 忽略和正则表达式匹配的 CVS 标识，不进行（到 Git 分支/里程碑）转换。
        #ExcludeRegexpStrategyRule(r'unknown-.*'),

        # 岐义的 CVS 标识的处理选项。
        # 缺省根据使用频率自动确定转换为分支或里程碑。
        HeuristicStrategyRule(),
        # 或者全部转换为分支。
        #AllBranchRule(),
        # 或者全部转换为里程碑。
        #AllTagRule(),

        ...

    run_options.set_project(

        ...

        # A list of symbol transformations that can be used to rename
        # symbols in this project.
        symbol_transforms=[
            # 是否需要重新命名里程碑？第一个参数用于匹配，第二个参数用于替换。
            #RegexpSymbolTransform(r'release-(\d+)_(\d+)',
            #                      r'release-\1.\2'),
            #RegexpSymbolTransform(r'release-(\d+)_(\d+)_(\d+)',
            #                      r'release-\1.\2.\3'),

    ```

**使用配置文件的 cvs2git 转换过程**

参照上面的方法，从缺省的 cvs2git 配置文件定制，在本地创建一个文件，例如名为`cvs2git.options`文件。

*   使用 cvs2git 配置文件，命令行大大简化了。

    ```
    $ cvs2git --options cvs2git.options

    ```

*   成功导出后，产生两个导出文件，都保存在`cvs2git-tmp`目录中。

    一个保存各个文件的各个不同版本的数据内容，即在命令行指定的输出文件`git-blob.dat`。另外一个文件是上面命令行指定的`git-dump.dat`用于保存各个提交相关信息（提交者、提交时间、提交日志等）。

    可以看出保存文件内容的导出文件相对更大一些。

    ```
    $ du -sh cvs2svn-tmp/*
    9.8M    cvs2svn-tmp/git-blob.dat
    24K     cvs2svn-tmp/git-dump.dat

    ```

*   创建空的 Git 库，使用 Git 通用的数据迁移命令**git fast-import**将 cvs2git 的导出文件导入版本库中。

    ```
    $ mkdir test
    $ cd test
    $ git init
    $ cat ../cvs2svn-tmp/git-blob.dat \
          ../cvs2svn-tmp/git-dump.dat | git fast-import

    ```

*   检查导出结果。

    ```
    $ git reset HEAD
    $ git checkout .
    $ git log -1
    commit e3f12f57a77cbffcf62e19012507d041f1c9b03d
    Author: Jiang Xin <jiangxin@ossxp.com>
    Date:   Tue Sep 21 07:56:31 2004 +0000

        修改邮件地址；
        修改搜索引擎；

    ```

可以看到，这一次的转换结果不但日志中的中文可以显示，而且提交者 ID 也转换成了 Git 的风格。

修改`cvs2git.optoins`中的 CVS 版本库地址，开始正式的转换过程。

## 6.1.4\. 迁移后版本库检查

完成迁移还不能算是大功告成，还需要进行细致的检验。

### 6.1.4.1\. 文件名和日志的中文

如果转换过程参考了前面的步骤和注意事项，文件名和版本库提交日志中的中文不应该出现乱码。

### 6.1.4.2\. 图片文件被破坏

最典型的错误就是转换后部分图片被破坏导致无法显示。这是怎么造成的呢？

CVS 缺省将提交的文件以文本方式添加，除非用户在添加文件时使用了`-kb`参数。用命令行提交的用户经常会忘记，这就导致一些二进制文件（如图片文件）被以文本文件的方式添加到其中。文本文件在 CVS 检入和检出时会进行换行符转换 ，在服务器端换行符保存为 LF，在 Windows 上检出时为 CRLF。如果误做文本文件方式添加的图片中恰好出现`CRLF`，则在 Windows 上似乎没有问题（仍然是`CRLF`），但是 CVS 库转换成 Git 库后，图片文件在 Windows 上再检出时文件数据中原来 CRLF 被换成了 LF，导致文件被破坏。

出现这种情况是 CVS 版本库使用和管理上出现了问题，应该在 CVS 版本库中对有问题的文件重新设置属性，标记为二进制文件。然后再进行 CVS 版本库到 Git 库的转换。

### 6.1.4.3\. `.cvsignore`文件的转换

CVS 版本库中可能存在`.cvsignore`文件用于设置文件忽略，相当于 Git 版本库中的`.gitignore`。因为当前版本的 cvs2git 不能自动将`.cvsignore`转换为`.gitignore`，需要在版本库迁移后手工完成。CVS 的`.cvsignore`文件只对目录内文件有效，不会向下作用到子目录上，这一点和 Git 的`.gitignore`相区别。还有不同就是`.cvsignore`文件每一行用空格分割多个忽略，而 Git 每个忽略为单独的一行。

### 6.1.4.4\. 迁移后的测试

一个简单的检查方法是，在同一台机器上分别用 CVS 和 Git 检出（或克隆），然后比较本地的差异。要在不同的系统上（Windows，Linux）分别进行测试。

* * *

| [[1]](#id3) | [`repo.or.cz/w/cvs2svn.git/blob/HEAD:/cvs2git-example.options`](http://repo.or.cz/w/cvs2svn.git/blob/HEAD:/cvs2git-example.options) |

| [[2]](#id4) | 部分中文的 UTF8 编码在 GBK 中存在古怪的对应 |

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 6.2\. 更多版本控制系统的迁移

*   6.2.1\. SVN 版本库到 Git 的迁移
*   6.2.2\. Hg 版本库到 Git 的迁移
*   6.2.3\. 通用版本库迁移
*   6.2.4\. Git 版本库整理
    *   6.2.4.1\. 环境变量过滤器
    *   6.2.4.2\. 树过滤器
    *   6.2.4.3\. 暂存区过滤器
    *   6.2.4.4\. 父节点过滤器
    *   6.2.4.5\. 提交说明过滤器
    *   6.2.4.6\. 提交过滤器
    *   6.2.4.7\. 里程碑名字过滤器
    *   6.2.4.8\. 子目录过滤器

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 6.2.1\. SVN 版本库到 Git 的迁移

Subversion 版本库到 Git 版本库的转换，最好的方法就是 git-svn。而 git-svn 的使用方法在前面“Git 和 SVN 协同模型”一章已经详细介绍过。本章的内容将不再对 git-svn 的用法做过多的重复，只在这里强调一下版本库迁移时的注意事项，相关 git-svn 内容还请参照前面的内容。

在迁移之前要确认一个问题，Subversion 转换到 Git 库之后，Subversion 还继续使用么？意思是说还允许向 Subversion 提交么？

如果回答是，那么直接查看“Git 和 SVN 协同模型”一章，用 Git 作为前端工具来操作 Subversion 版本库，而不要理会下面的内容。因为下面描述的迁移步骤针对的是一次性的 Subversion 到 Git 版本库的迁移。

**需要提交中出现 git-svn-id 标识么？**

如果一次性、永久性的将 Subverison 迁移到 Git 库，可以选择`git-svn-id:`标识不在转换后 Git 的提交日志中出现，这样转换后的 Git 库根本看不出来曾经用 Subversion 版本库维护过。

在 git-svn 的`clone`或者`init`子命令行中使用参数：`--no-metadata`。Git 库的配置会自动配置`svn-remote.noMetadata`为 1。之后执行**git svn fetch**时就不会在日志中产生`git-svn-id:`的标识。

**Subversion 用户名到 Git 用户名的映射**

缺省转换后 Git 库的提交者 ID 为如下格式：`userid <userid@SVN-REPOS-UUID>`。即在邮件地址域名处以 SVN 版本库的 UUID 代替。可以在执行**git svn fetch**时通过下面的参数提供一个映射文件完成 SVN 用户名到 Git 用户名的转换。

```
-A<filename>, --authors-file=<filename>

```

即用`-A`或者`--authors-file`参数给出一个映射文件，这个文件帮助 git-svn 将 Subversion 用户名映射为 Git 用户名。此文件的每一行定义一个用户名映射，每一行的格式为：

```
loginname = User Name <user@example.com>

```

也可以通过下面的命令在 Git 库的`config`文件中设置，这样就不必在每次执行**git svn fetch**都带上这个参数。

```
$ git config svn.authorsfile /path/to/authersfile

```

当设定了用户映射文件后，如果在执行**git svn fetch**是发现 SVN 的用户在该映射文件中没有定义，转换过程被中断。需要重新编辑用户映射文件，补充新的用户映射后，再重新执行 git-svn 命令。

**将 Subversion 分支和里程碑直接转换为 Git 分支和里程碑，不要放在``refs/remotes``下**

使用缺省参数执行 SVN 到 Git 的转换时，SVN 的里程碑和分支转换到 Git 库的`refs/remotes`引用下。这会导致其他人从转换后的 Git 库克隆时，看不到 Subversion 原有的分支和里程碑。

当以缺省参数执行**git svn init**时，Git 的配置文件中会生成下面的配置：

```
[svn-remote "svn"]
        fetch = trunk:refs/remotes/trunk
        branches = branches/*:refs/remotes/*
        tags = tags/*:refs/remotes/tags/*

```

可以直接编辑 Git 配置文件，将其内容调整如下：

```
[svn-remote "svn"]
        fetch = trunk:refs/heads/master
        branches = branches/*:refs/heads/*
        tags = tags/*:refs/tags/*

```

之后再执行**git svn fetch**后，就可以实现 SVN 的分支和里程碑正确的转换为 Git 库的里程碑。否则就需要将`.git/refs/remots/`下的引用移动到`.git/refs/heads`以及`.git/refs/tags`下。

**清除 git-svn 的中间文件**

git-svn 的中间文件位于目录`.git/svn`下，删除此目录完成对 git-svn 转换数据库文件的清理。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 6.2.2\. Hg 版本库到 Git 的迁移

Mercurial（水银）是和 Git 同时代的、与之齐名的一款著名的分布式版本控制系统，也有相当多的使用者。就像水银又名汞，作为版本控制系统的 Mercurial 又称作 Hg（水银元素符号）。Hg 具有简单易用的优点，至少 Hg 提交的顺序递增的数字编号让 Subversion 用户感到更为亲切。Hg 的开发语言除少部分因性能原因使用 C 语言外，大部分用 Python 语言开发完成，因而更易扩展，最终形成了 Hg 最具特色的插件系统。例如 MQ 就是 Hg 一个很有用的插件，通过 Quilt 式的补丁集实现对定制开发的特性分支的版本控制，当然 StGit 和 Topgit 也可以实现类似的功能。

但是 Hg 存在一些不足。例如服务器的存储效率不能和 Git 相比，服务器存储空间占用更大。Hg 还不支持真正的分支，只能通过版本库克隆来进行分支开发。因为 Hg 不支持真正的分支，所以不能向 git-svn 那样完整的将 Subversion 版本库转换和互操作。Hg 的速度相比 Git 要慢，尤其是网络操作没有像 Git 一样精确的进度显示。Hg 提交只能回退一次，要想多次回退和整理版本库需要用到 MQ 插件。作为定制开发的利器“Hg+MQ”不适合多人协作开发而“Git+Topgit”更为适合。

不论是何原因想从 Hg 迁移到 Git，用一个名为 fast-export 的转换工具可以很方便的实现。fast-export 是一个用 Python 开发的命令行工具，可以将本地的 Hg 版本库迁移为 Git 版本库。其原理和 CVS 版本库迁移至 Git 时使用的 cvs2git 相仿，都是先从源版本库生成导出文件，再用 Git 的通用版本库转换工具**git fast-import**导入到新建的 Git 版本库中。

安装 fast-export 非常简单，只要用 Git 克隆 fast-export 的版本库即可。

```
$ cd /path/to
$ git clone git://repo.or.cz/fast-export.git

```

完成克隆后，会看到`/path/to/fast-export`目录中有一个名为`hg-fast-import.sh`的脚本文件，该文件封装了对相应 Python 脚本的调用。使用该脚本可以实现 Hg 版本库到 Git 版本库的迁移。

下面就演示一下 Hg 版本库到 Git 版本库的转换。

*   要转换的 Hg 版本库位于路径`/path/to/hg/hello/.hg`下。

    Hg 不支持真正的分支，而且版本库中可能存在尚未合并的多个头指针。检查一下不要存在具有相同分支名但尚未合并的多个头指针，否则转换会失败。下面显示的该 Hg 版本库中具有两个具名分支`r1.x`和`next`，还有一个缺省未设置名称的头指针，因为分支名各不相同所以不会为转换过程造成麻烦。

    ```
    $ hg heads
    修改集:      7:afdd475caeee
    分支:        r1.x
    标签:        tip
    父亲:        0:798a9568e10e
    用户:        Jiang Xin <jiangxin@ossxp.com>
    日期:        Fri Jan 14 17:01:47 2011 +0800
    描述:
    start new branch: r1.x

    修改集:      6:7f5a46201dda
    分支:        next
    用户:        Jiang Xin <jiangxin@ossxp.com>
    日期:        Fri Jan 14 17:01:04 2011 +0800
    文件:        src/locale/zh_CN/LC_MESSAGES/helloworld.po
    描述:
    imported patch 100_locale_zh_cn.patch

    修改集:      1:97f0a21021c6
    用户:        Jiang Xin <worldhello.net AT gmail DOT com>
    日期:        Sun Aug 23 23:53:05 2009 +0800
    文件:        src/COPYRIGHT src/main.bak src/main.c
    描述:
    Fixed #6: import new upstream version hello-2.0.0

    ```

*   初始化一个 Git 版本库，该版本库就是迁移的目标版本库。

    ```
    $ mkdir -p /path/to/my/workspace/hello
    $ cd /path/to/my/workspace/hello
    $ git init
    Initialized empty Git repository in /path/to/my/workspace/hello/.git/

    ```

*   在刚刚完成初始化的 Git 工作区中调用`hg-fast-export.sh`脚本完成版本库转换。

    ```
    $ /path/to/fast-export/hg-fast-export.sh -r /path/to/hg/hello

    ```

*   转换完毕，执行**git branch**会看到 Hg 版本库中的具名分支都转换为相应的分支，没有命名的缺省头指针转换为 master 分支。

    ```
    $ git branch
    * master
      next
      r1.x

    ```

在转换后的 Git 版本库目录中，保存了几个用于记录版本库转换进度的状态文件（`.git/hg2git-*`），当在 Git 工作区不带任何参数执行`hg-fast-export.sh`命令时，会继续增量式的进行转换，将 Hg 版本库中的新提交迁移到 Git 版本库中。

如果使用了多个不同的 Hg 克隆版本库进行分支管理，就需要一一对 Hg 版本库进行转换，然后在对转换后的 Git 版本库进行合并。在合并 Git 版本库的时候可以参考下面的命令。

```
$ git remote add <name1> <path/to/repos/1>
$ git remote add <name2> <path/to/repos/2>
$ git remote update
$ git checkout -b <branch1> origin/<name1>/master
$ git checkout -b <branch2> origin/<name2>/master

```

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 6.2.3\. 通用版本库迁移

如果读者的版本控制工具在前面的迁移方案没有涉及到，也不要紧，因为很可能通过搜索引擎就能找到一款合适的迁移工具。如果找不到相应的工具，可能是您使用的版本控制工具太冷门，或者是一款不提供迁移接口的商业版本控制工具。这时您可以通过手工检入的方式或者针对 Git 提供的版本库导入接口**git fast-import**实现版本库导入。

手工检入的方式适合于只有少数几个提交或者对大部分提交历史不关心而只需要对少数里程碑版本执行导入。这种版本库迁移方式非常简单，相当于在完成 Git 版本库初始化后，在工作区重复执行：工作区文件清理，文件复制，执行**git add -A**添加到暂存区，执行**git commit**提交。

但是如果需要将版本库完整的历史全部迁移到新的 Git 版本库中，手工检入方法就不可取了，采用针对**git fast-import**编程是一个可以考虑的方法。Git 提供了一个通用的版本库导入解决方案，即通过向命令**git fast-import**传递特定格式的字节流，就可以实现 Git 版本库的创建。工具**git fast-import**的导入文件格式设计的相对比较简单，当理解了其格式约定后，可以相对容易的开发出针对特定版本库的迁移工具。

下面就是一个简单的导入文件，为说明方便前面标注了行号。将这个文件保存为`/path/to/file/dump1.dat`。

```
 1 commit refs/heads/master
 2 mark :1
 3 committer User1 <user1@ossxp.com> 1295312699 +0800
 4 data <<EOF
 5 My initial commit.
 6 EOF
 7 M 644 inline README
 8 data <<EOF
 9 Hello, world.
10 EOF
11 M 644 inline team/user1.txt
12 data <<EOF
13 I'm user1.
14 EOF

```

上面这段文字应该这样理解：

*   第 1 行以`commit`开头，标记一个提交的开始。该提交会创建（或更新）引用`refs/heads/master`。
*   第 2 行以`mark`开头，是一个标记指令，将这个提交用“`:1`”标示以方便后面的提交参照。
*   第 3 行记录了这个提交的提交者是`User1`，邮件地址为`<user1@ossxp.com>`，提交时间则采用 Unix 时间格式。
*   第 4-6 行是该提交的提交说明，提交说明用`data`数据块的方式进行定义。
*   第 4 行在`data`语句后紧接着的`<<EOF`含义为`data`的内容到以`EOF`标记的行截止。这样的表示法称为“Here Documents”表示法。
*   第 7 行以字母`M`开头，含义是修改（或新建）了一个文件，文件名为`README`，而文件的内容以`inline`的方式提供。
*   第 8-10 行则是以内联（inline）数据块的方式提供`README`文件的内容。
*   第 11 行定义了该提交修改的第二个文件`team/user1.txt`。该文件的内容也是以内联（inline）的方式给出。
*   第 12-14 行给出文件`team/user1.txt`的内容。

下面初始化一个新的版本库，并通过导入文件`/path/to/file/dump1.dat`的方式为版本库注入数据。

*   初始化版本库。

    ```
    $ mkdir -p /path/to/my/workspace/import
    $ cd /path/to/my/workspace/import
    $ git init

    ```

*   调用**git fast-import**命令。

    ```
    $ git fast-import < /path/to/file/dump1.dat
    git-fast-import statistics:
    ---------------------------------------------------------------------
    Alloc'd objects:       5000
    Total objects:            5 (         0 duplicates                  )
          blobs  :            2 (         0 duplicates          0 deltas)
          trees  :            2 (         0 duplicates          0 deltas)
          commits:            1 (         0 duplicates          0 deltas)
          tags   :            0 (         0 duplicates          0 deltas)
    Total branches:           1 (         1 loads     )
          marks:           1024 (         1 unique    )
          atoms:              3
    Memory total:          2344 KiB
           pools:          2110 KiB
         objects:           234 KiB
    ---------------------------------------------------------------------
    pack_report: getpagesize()            =       4096
    pack_report: core.packedGitWindowSize = 1073741824
    pack_report: core.packedGitLimit      = 8589934592
    pack_report: pack_used_ctr            =          1
    pack_report: pack_mmap_calls          =          1
    pack_report: pack_open_windows        =          1 /          1
    pack_report: pack_mapped              =        323 /        323
    ---------------------------------------------------------------------

    ```

*   看看提交日志。

    ```
    $ git log --pretty=fuller --stat
    commit 18f4310580ca915d7384b116fcb2e2ca0b833714
    Author:     User1 <user1@ossxp.com>
    AuthorDate: Tue Jan 18 09:04:59 2011 +0800
    Commit:     User1 <user1@ossxp.com>
    CommitDate: Tue Jan 18 09:04:59 2011 +0800

        My initial commit.

     README         |    1 +
     team/user1.txt |    1 +
     2 files changed, 2 insertions(+), 0 deletions(-)

    ```

再来看一个导入文件。将下面的内容保存到文件`/path/to/file/dump2.dat`中。

```
 1 blob
 2 mark :2
 3 data 25
 4 Hello, world.
 5 Hi, user2.
 6 blob
 7 mark :3
 8 data <<EOF
 9 I'm user2.
10 EOF
11 commit refs/heads/master
12 mark :4
13 committer User2 <user2@ossxp.com> 1295312799 +0800
14 data <<EOF
15 User2's test commit.
16 EOF
17 from :1
18 M 644 :2 README
19 M 644 :3 team/user2.txt

```

上面的内容标注了行号，注意不要把行号也代入文件中。其中：

*   第 1-5 行定义了编号为“`:2`”的文件内容。该文件的内容共有 25 字节，第 3 行开始的`data`文字块就通过在后面跟上一个表示文件长度的十进制数字界定了内容的起止。
*   第 6-10 行定义了编号为“`:3`”的文件内容。第 8 行界定该文件内容使用了“Here Documents”的语法，该语法对于文本内容比较适合，使用内容长度标示内容起止对于二进制文件更为适合。
*   第 11 行开始定义了一个新的提交。
*   第 12 行设定该提交的编号为“`:4`”。
*   第 17 行以`from`开头，定义了该提交的父提交为编号为“`:1`”的提交，即在`/path/to/file/dump1.dat`中定义的提交。
*   第 18 行和第 19 行设定了该提交更改的两个文件，这两个文件的内容不像之前的导出文件`dump1.dat`那样使用内联方式定义内容，而是采用引用方式引用前面定义的二进制数据流（blob）作为文件的内容。

如果以增量方式导入`dump2.dat`会报错，因为在第 17 行引用的“`:1`”没有定义。

```
$ git fast-import < /path/to/file/dump2.dat
fatal: mark :1 not declared
fast-import: dumping crash report to .git/fast_import_crash_21772

```

如果将文件`/path/to/file/dump2.dat`的第 17 行的引用修改为提交 ID，是可以增量导入的。不过为了说明的方便，还是通过将两个导入文件一次性传递给**git fast-import**创建一个新版本库。

*   初始化版本库`import2`。

    ```
    $ mkdir -p /path/to/my/workspace/import2
    $ cd /path/to/my/workspace/import2
    $ git init

    ```

*   调用**git fast-import**命令。

    ```
    $ cat /path/to/file/dump1.dat \
          /path/to/file/dump2.dat | git fast-import

    ```

*   导入之后的日志显示：

    ```
    $ git log --graph --stat
    * commit 73a6f2742f9da7c1b4bb8748e018a2becad39dd6
    | Author: User2 <user2@ossxp.com>
    | Date:   Tue Jan 18 09:06:39 2011 +0800
    |
    |     User2's test commit.
    |
    |  README         |    1 +
    |  team/user2.txt |    1 +
    |  2 files changed, 2 insertions(+), 0 deletions(-)
    |
    * commit 18f4310580ca915d7384b116fcb2e2ca0b833714
      Author: User1 <user1@ossxp.com>
      Date:   Tue Jan 18 09:04:59 2011 +0800

          My initial commit.

       README         |    1 +
       team/user1.txt |    1 +
       2 files changed, 2 insertions(+), 0 deletions(-)

    ```

下面再来看一个导入文件，在这个导入文件中，包含了合并提交以及创建里程碑。

```
 1 blob
 2 mark :5
 3 data 25
 4 Hello, world.
 5 Hi, user1.
 6 blob
 7 mark :6
 8 data 35
 9 Hello, world.
10 Hi, user1 and user2.
11 commit refs/heads/master
12 mark :7
13 committer User1 <user1@ossxp.com> 1295312899 +0800
14 data <<EOF
15 Say helo to user1.
16 EOF
17 from :1
18 M 644 :5 README
19 commit refs/heads/master
20 mark :8
21 committer User2 <user2@ossxp.com> 1295312900 +0800
22 data <<EOF
23 Say helo to both users.
24 EOF
25 from :4
26 merge :7
27 M 644 :6 README
28 tag refs/tags/v1.0
29 from :8
30 tagger Jiang Xin <jiangxin@ossxp.com> 1295312901 +0800
31 data <<EOF
32 Version v1.0
33 EOF

```

将这个文件保存到`/path/to/file/dump3.dat`。下面针对该文件内容进行简要的说明：

*   第 1-5 行和第 6-10 行定义了两个 blob 对象，代表了两个对`README`文件的不同修改。

*   第 11 行开始定义了编号为“`:7`”的提交。从第 17 行可以看出该提交的父提交也是由`dump1.dat`导入的第一个提交。

*   第 19 行开始定义了编号为“:8``”的提交。该提交为一个合并提交，除了在第 25 行设定了第一个父提交外，还由第 26 行给出了第二个父提交。

*   第 28 行开始定义了一个里程碑。里程碑的名字为`refs/tags/v1.0`。第 29 行指定了该里程碑对应的提交。里程碑说明由第 31-33 行指令给出。

*   初始化版本库`import3`。

    ```
    $ mkdir -p /path/to/my/workspace/import3
    $ cd /path/to/my/workspace/import3
    $ git init

    ```

*   调用**git fast-import**命令。

    ```
    $ cat /path/to/file/dump1.dat /path/to/file/dump2.dat \
          /path/to/file/dump3.dat | git fast-import

    ```

*   查看创建的版本库的日志。

    从日志中可以看出里程碑`v1.0`已经建立在最新的提交上了。

    ```
    $ git log --oneline --graph --decorate
    *   a47790e (HEAD, tag: refs/tags/v1.0, master) Say helo to both users.
    |\
    | * f486a44 Say helo to user1.
    * | 73a6f27 User2's test commit.
    |/
    * 18f4310 My initial commit.

    ```

理解了`git fast-import`的导入文件格式，针对特定的版本控制系统开发一个新的迁移工具不是难事。Hg 的迁移工具**fast-export**是一个很好的参照。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 6.2.4\. Git 版本库整理

Git 提供了太多武器进行版本库的整理，可以将一个 Git 版本库改动换面成为另外的一个 Git 版本库。

*   使用交互式变基操作，将多个提交合并为一个。
*   使用 StGit，合并提交以及更改提交。
*   借助变基操作，抛弃部分历史提交。
*   使用子树合并，将多个版本库整合在一起。
*   使用`git-subtree`插件，将版本库的一个目录拆分出来成为独立版本库的根目录。

但是有些版本库重整工作如果使用上面的工具会非常困难，而采用另外一个还没有用到的 Git 命令**git filter-branch**却可以做到事半功倍。看看使用这个新工具来实现下面的这几个任务是多么的简单和优美。

*   将版本库中某个文件彻底删除。即凡是有该文件的提交都一一作出修改，撤出该文件。

    下面的命令并非最优实现，后面会介绍一个运行更快的命令。

    ```
    $ git filter-branch --tree-filter 'rm -f filename' -- --all

    ```

*   更改历史提交中某一提交者的姓名及邮件地址。

    ```
    $ git filter-branch --commit-filter '
          if [ "$GIT_AUTHOR_NAME" = "Xin Jiang" ]; then
              GIT_AUTHOR_NAME="Jiang Xin"
              GIT_AUTHOR_EMAIL="jiangxin@ossxp.com"
              GIT_COMMITTER_NAME="$GIT_AUTHOR_NAME"
              GIT_COMMITTER_EMAIL="$GIT_AUTHOR_EMAIL"
          fi
          git commit-tree "$@";
          ' HEAD

    ```

*   为没有包含签名的历史提交添加签名。

    ```
    $ git filter-branch -f --msg-filter '
          signed=false
          while read line; do
              if echo $line | grep -q Signed-off-by; then
                  signed=true
              fi
              echo $line
          done
          if ! $signed; then
              echo ""
              echo "Signed-off-by: YourName <your@email.address>"
          fi
          ' HEAD

    ```

通过上面的例子，可以看出命令**git filter-branch**通过针对不同的过滤器提供可执行脚本，从不同的角度对 Git 版本库进行重构。该命令的用法：

```
git filter-branch [--env-filter <command>] [--tree-filter <command>]
        [--index-filter <command>] [--parent-filter <command>]
        [--msg-filter <command>] [--commit-filter <command>]
        [--tag-name-filter <command>] [--subdirectory-filter <directory>]
        [--prune-empty]
        [--original <namespace>] [-d <directory>] [-f | --force]
        [--] [<rev-list options>...]

```

这条命令异常复杂，但是大部分参数是用于提供不同的接口，因此还是比较好理解的。

*   该命令最后的`<rev-list>`参数提供要修改的版本范围，如果省略则相当于`HEAD`指向的当前分支。也可以使用`--all`来指代所有引用，但是要在`--all`和前面的参数间使用分隔符`--`。
*   运行**git filter-branch**命令改写分支之后，被改写的分支会在`refs/original`中对原始引用做备份。对于在`refs/original`中已有备份的情况下，该命令拒绝执行，除非使用`-f`或`--force`参数。
*   其他需要接以`<command>`的参数都为**git filter-branch**提供相应的接口进行过虑，在下面会针对各个过滤器进行介绍。

## 6.2.4.1\. 环境变量过滤器

参数`--env-filter`用于设置一个环境过滤器。该过滤器用于修改环境变量，对特定的环境变量的修改会改变提交。下面的示例[[1]](#id10)可以用于修改作者/提交者的邮件地址。

```
$ git filter-branch --env-filter '
    an="$GIT_AUTHOR_NAME"
    am="$GIT_AUTHOR_EMAIL"
    cn="$GIT_COMMITTER_NAME"
    cm="$GIT_COMMITTER_EMAIL"

    if [ "$cn" = "Kanwei Li" ]; then
      cm="kanwei@gmail.com"
    fi
    if [ "$an" = "Kanwei Li" ]; then
      am="kanwei@gmail.com"
    fi

    export GIT_AUTHOR_EMAIL=$am
    export GIT_COMMITTER_EMAIL=$cm
    '

```

这个示例和本节一开始介绍的更改作者/提交者信息的示例功能相同，但是使用了不同的过滤器，读者可以根据喜好选择。

## 6.2.4.2\. 树过滤器

参数`--tree-filter`用于设置树过滤器。树过滤器会将每个提交检出到特定目录中（`.git-rewrite/`目录或者用`-d`参数指定的目录），针对检出目录中文件的修改、添加、删除会改变提交。注意此过滤器忽略`.gitignore`，因此任何对检出目录的修改都会记录在新的提交中。之前介绍的文件删除就是一例。再比如对文件名的修改：

```
$ git filter-branch --tree-filter '
      [ -f oldfile ] && mv oldfile newfile || true
      ' -- --all

```

## 6.2.4.3\. 暂存区过滤器

树过滤器因为要将每个提交检出，因此非常费时，而参数`--index-filter`给出的暂存区过滤器则没有这个缺点。之前使用树过滤器删除文件的的操作如果换做用暂存区过滤器实现运行的会更快。

```
$ git filter-branch --index-filter '
      git rm --cached --ignore-unmatch filename
      ' -- --all

```

其中参数`--ignore-unmatch`让**git rm**命令不至于因为暂存区中不存在`filename`文件而失败。

## 6.2.4.4\. 父节点过滤器

参数`--parent-filter`用于设置父节点过滤器。该过滤器用于修改提交的父节点。提交原始的父节点通过标准输入传入脚本，而脚本的输出将作为提交新的父节点。父节点参数的格式为：如果没有父节点（初始提交）则为空。如果有一个父节点，参数为`-p parent`。如果是合并提交则有多个父节点，参数为`-p parent1 -p parent2 -p parent3 ...`。

下面的命令将当前分支的初始提交嫁接到`<graft-id>`所指向的提交上。

```
$ git filter-branch --parent-filter 'sed "s/^\$/-p <graft-id>/"' HEAD

```

如果不是将初始提交（没有父提交）而是任意的一个提交嫁接到另外的提交上，可以通过`GIT_COMMIT`环境变量对提交进行判断，更改其父节点以实现嫁接。

```
$ git filter-branch --parent-filter \
          'test $GIT_COMMIT = <commit-id> && \
           echo "-p <graft-id>" || cat
          ' HEAD

```

关于嫁接，Git 可以通过配置文件`.git/info/grafts`实现，而**git filter-branch**命令可以基于该配置文件对版本库实现永久性的更改。

```
$ echo "$commit-id $graft-id" >> .git/info/grafts
$ git filter-branch $graft-id..HEAD

```

## 6.2.4.5\. 提交说明过滤器

参数`--msg-filter`用于设置提交说明过滤器。该过滤器用于改写提交说明。原始的提交说明作为标准输入传入脚本，而脚本的输出作为新的提交说明。

例如将使用 git-svn 从 Subversion 迁移过来的 Git 版本库，缺省情况下在提交说明中饱含`git-svn-id:`字样的说明，如果需要将其清除可以不必重新迁移，而是使用下面的命令重写提交说明。

```
$ git filter-branch --msg-filter 'sed -e "/^git-svn-id:/d"' -- --all

```

再如将最新的 10 个提交添加“`Acked-by:`”格式的签名。

```
$ git filter-branch --msg-filter '
      cat &&
      echo "Acked-by: Bugs Bunny <bunny@bugzilla.org>"
      ' HEAD~10..HEAD

```

## 6.2.4.6\. 提交过滤器

参数`--commit-filter`用于设置树过滤器。提交过滤器所给出的脚本，在版本库重整过程的每次提交时运行，取代缺省要执行的**git commit-tree**命令。不过一般情况会在脚本中调用**git commit-tree**命令。传递给脚本的参数格式为`<TREE_ID> [(-p <PARENT_COMMIT_ID>)...]`，提交日志以标准输入的方式传递给脚本。脚本的输出是新提交的提交 ID。作为扩展，如果脚本输出了多个提交 ID，则这些提交 ID 作为子提交的多个父节点。

使用下面的命令，可以过滤掉空提交（合并提交除外）。

```
$ git filter-branch --commit-filter 'git_commit_non_empty_tree "$@"'

```

函数`git_commit_non_empty_tree`函数是在脚本**git-filter-branch**中已经定义过的函数。可以打开文件`$(git --exec-path)/git-filter-branch`查看。

```
# if you run 'git_commit_non_empty_tree "$@"' in a commit filter,
# it will skip commits that leave the tree untouched, commit the other.
git_commit_non_empty_tree()
{
  if test $# = 3 && test "$1" = $(git rev-parse "$3^{tree}"); then
    map "$3"
  else
    git commit-tree "$@"
  fi
}

```

如果想某个用户的提价非空但是也想跳过，可以使用下面的命令：

```
$ git filter-branch --commit-filter '
      if [ "$GIT_AUTHOR_NAME" = "badboy" ];
      then
              skip_commit "$@";
      else
              git commit-tree "$@";
      fi' HEAD

```

其中函数`skip_commit`也是在`git-filter-branch`脚本中已经定义好的。该函数的作用就是将传递给提交过滤器脚本的参数`<tree_id> -p parent1 -p parent2 ...`进行处理，形成`parent1 parent2`的输出。参见 Git 命令脚本`$(git --exec-path)/git-filter-branch`中相关函数。

```
# if you run 'skip_commit "$@"' in a commit filter, it will print
# the (mapped) parents, effectively skipping the commit.
skip_commit()
{
  shift;
  while [ -n "$1" ];
  do
    shift;
    map "$1";
    shift;
  done;
}

```

## 6.2.4.7\. 里程碑名字过滤器

参数`--tag-name-filter`用于设置里程碑名字过滤器。该过滤器也是经常要用到的过滤器。上面介绍的各个过滤器都有可能改变提交 ID，如果在原有的提交 ID 上建有里程碑，可能会随之更新但是会产生大量的警告日志，提示使用里程碑过滤器。里程碑过滤器脚本以原始里程碑名称作为标准输入，并把新里程碑名称作为标准输出。如果不打算变更里程碑名称，而只是希望里程碑随提交而更新，可以在脚本中使用**cat**命令。例如下面的命令中里程碑名字过滤器和目录树过滤器同时使用。

```
$ git filter-branch --tree-filter '
      [ -f oldfile ] && mv oldfile newfile || true
      ' -- tag-name-filter 'cat' -- --all

```

在前面的里程碑一章曾经提到过**git branch**命令没有提供里程碑重名名的功能，而使用里程碑名字过滤器可以实现里程碑的重命名。下面的的示例会修改里程碑的名字，将前缀为`old-prefix`的里程碑改名为前缀为`new-prefix`的里程碑。

```
$ git filter-branch --tag-name-filter '
      oldtag=`cat`
      newtag=${oldtag#old-prefix}
      if [ "$oldtag" != "$newtag" ]; then
          newtag="new-prefix$newtag"
      fi
      echo $newtag
      '

```

注意因为签名里程碑重建后，因为签名不可能保持所以新里程碑会丢弃签名，成为一个普通的包含说明的里程碑。

## 6.2.4.8\. 子目录过滤器

参数`--subdirectory-filter`用于设置子目录过滤器。子目录过滤器可以将版本库的一个子目录提取为一个新版本库，并将该子目录作为版本库的根目录。例如从 Subversion 转换到 Git 版本库因为参数使用不当，将原 Subversion 的主线转换为 Git 版本库的一个目录`trunk`。可以使用**git filter-branch**命令的子目录过滤器将`trunk`提取为版本库的根。

```
$ git filter-branch --subdirectory-filter trunk HEAD

```

* * *

| [[1]](#id2) | 摘自[`kanwei.com/code/2009/03/29/fixing-git-email.html`](http://kanwei.com/code/2009/03/29/fixing-git-email.html)。 |

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.