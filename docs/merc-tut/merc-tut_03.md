# 教程 - 检查仓库历史

## 教程 - 检查仓库历史

现在我们已经参照 教程 - 克隆仓库 克隆了一个 仓库; 我们仓库的本地拷贝叫 `my-hello`.

让我们看一看这个仓库的历史记录。我们用 `log` 命令来做这个事情。这个命令按时间顺序从近到远输出在仓库中发生的每一个事件。

```
$ cd my-hello
$ hg log
changeset:   1:82e55d328c8c
tag:         tip
user:        mpm@selenic.com
date:        Fri Aug 26 01:21:28 2005 -0700
summary:     Create a makefile

changeset:   0:0a04b987be5a
user:        mpm@selenic.com
date:        Fri Aug 26 01:20:50 2005 -0700
summary:     Create a standard "hello, world" program 
```

这些输出行的含义是这样的。

*   每一段描述一个特定的改变集。一个或几个文件的改变集合在一起形成一个逻辑单元，称为改变集。

    *   在上面的例子中，我们可以看到该仓库 的历史包括了两个改变集。
*   `changeset` 标识了一个 改变集.

    *   冒号前面的数字代表版本号; 它是一种标识改变集的本地缩写.只是在你的本地仓库中这个版本号才有意义.

    *   冒号后面的那个很长的十六进制串是 ChangeSetID; 它是标识改变集的全局唯一标识符, 在所有包含这个改变集的仓库中都相同. 如果你正在和其他人讨论某个改变集,请使用这个 ChangeSetID,而不是上面说的版本号.

*   `tag` 是一个标签,可以理解成为一个改变集指定的名字.

    *   你可以给任何改变集指定一个或者多个标签. 实际上, 许多改变集都是没有标签的, 所以`tag`这一行很多时候都不存在.

    *   名叫`tip` 的特殊标签总是表示，它是仓库中最后一个改变集。 如果你创建另外的改变集（一会我们会看到），那么它将会变成 Tip。

*   `user` 确定了谁创建了本改变集。这是一个无格式的字符串; 它通常包括电子邮件地址，个人姓名等。

*   `date` 描述了改变集是什么时候创建的。这些时间是创建改变集的人所在区域的当地时间。

*   `summary` 给出了改变集描述的首行。它是在创建改变集时进入的，它可以帮助创建者和其它人了解改变集的目的。

*   `parent` 标志了改变集的父辈，当你从几个仓库合并而来的情况下，父辈有多个。

    *   大多数情况只有一个父辈，它比目前的改变集旧。这是在我们例子中使用的。

我们可以通过指定`-v`诊断输出选项来获得更多更详细的历史信息, 或者指定`--debug`选项来获得历史信息中的一切细节:

```
$ hg log -v
changeset:   1:82e55d328c8ca4ee16520036c0aaace03a5beb65
tag:         tip
user:        mpm@selenic.com
date:        Fri Aug 26 01:21:28 2005 -0700
files:       Makefile
description:
Create a makefile

(...)

$ hg log --debug
manifest:    1:0c7c1d435e6703e03ac6634a7c32da3a082d1600
changeset:   1:82e55d328c8ca4ee16520036c0aaace03a5beb65
tag:         tip
parent:      0:0a04b987be5ae354b710cefeba0e2d9de7ad41a9
parent:      -1:0000000000000000000000000000000000000000
user:        mpm@selenic.com
date:        Fri Aug 26 01:21:28 2005 -0700
files+:      Makefile
description:
Create a makefile

(...) 
```

-v 的输出比缺省输出要多。

*   `changeset` 给出不缩略的改变集标号。

*   `files` 列出在本改变集中文件的改变。

*   `description` 包含了改变集的完整多行描述而不仅是头一行。

    *   在我们例子中，描述都只有一行，所以没什么特别不同的地方。

--debug 给谓词输出添加了以下方面：

*   `file+` 列出了在此改变集中增加的文件。

*   `file-` 列出了在此改变集中删除的文件。

*   `manifest` 给这个改变集一个 Manifest 号。

*   两个 `parent` 域给这个改变集的两个父辈号码， `-1:0000000000000000000000000000000000000000` 表示没有父改变集。

`log` 命令与 `-r` 选项一起使用可以查看特定改变集。

```
$ hg log -r1
changeset:   1:82e55d328c8c
tag:         tip
user:        mpm@selenic.com
date:        Fri Aug 26 01:21:28 2005 -0700
summary:     Create a makefile 
```

![<!>](img/attention.png "<!>") The `-r` 选项实际上支持一种非常灵活的语法来选择改变集的范围。但是由于在我们例子的仓库里的改变集很有限，我们不能做很好的示范。你可以看 Mercurial 的 [manpage](http://www.selenic.com/mercurial/hg.1.html) 来得到更多的信息。

`log` 命令与 `-p` 选项一起可以显示和此改变集相关联的补丁。

```
$ hg log -r1 -p
changeset:   1:82e55d328c8c
tag:         tip
user:        mpm@selenic.com
date:        Fri Aug 26 01:21:28 2005 -0700
summary:     Create a makefile

diff -r 0a04b987be5a -r 82e55d328c8c Makefile
--- /dev/null   Fri Aug 26 01:20:50 2005 -0700
+++ b/Makefile  Fri Aug 26 01:21:28 2005 -0700
@@ -0,0 +1,1 @@
+all: hello 
```

我们也可以使用 `tip` 命令来显示 *tip* 的信息，如最后的改变集。 `tip` 命令除了不支持 `-p` 选项外，它可以当做 `log -r tip` 的快捷方式，

```
$ hg tip
changeset:   1:82e55d328c8c
tag:         tip
user:        mpm@selenic.com
date:        Fri Aug 26 01:21:28 2005 -0700
summary:     Create a makefile

$ hg log -r tip
changeset:   1:82e55d328c8c
tag:         tip
user:        mpm@selenic.com
date:        Fri Aug 26 01:21:28 2005 -0700
summary:     Create a makefile 
```

现在我们对发生了什么有了一点概念，现在让我们进入并做一些修改吧！进入 教程 - 生成第一个[变更]!