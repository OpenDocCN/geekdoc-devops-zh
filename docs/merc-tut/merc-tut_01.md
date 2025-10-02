# 教程 - 开始：安装

## 教程 - 开始：安装

安装 Mercurial 是简单的。

*   Linux， MacOS X， 和其它 Unix 的变体，参照 UnixInstall 目录。

*   在 Windows 中，参照 WindowsInstall 的说明.

你完成后回到这里。

**注：** 本教程假定你正在运行 Mercurial 0.7 以上版本。换句话说，如果你有 0.6 版的，本教程不适用。本教程是为 Mercurial 0.7 进行了升级。

Mercurial 程序命名为 `hg`。每一个 Mercurial 命令以 `hg` 开头，后面跟命令名，然后是选项和参数。

目前 Mercurial 已经安装，我们应该可以在命令行上简单键入 `hg`，程序应该显示一些有用的命令汇总：

```
$ hg
Mercurial Distributed SCM

basic commands (use "hg help" for the full list or option "-v" for details):

 add        add the specified files on the next commit
 annotate   show changeset information per file line
 clone      make a copy of an existing repository
(...) 
```

如果不是这样的，你安装的程序有问题，你应该看看 InstallTroubleshooting。

为了知道 Mercurial 是什么版本，请键入：

```
$ hg version
Mercurial Distributed SCM (version 0.7)

Copyright (C) 2005 Matt Mackall <mpm@selenic.com>
This is free software; see the source for copying conditions. There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. 
```

如果所有都运行地很好，让我们继续进入 教程 - 克隆仓库。