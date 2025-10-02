# 教程 - 克隆仓库

## 教程 - 克隆仓库

我们已经按照 ChineseTutorialInstall 安装了 Mercurial，对吗？很好！

Mercurial 中，我们在 仓库 里做我们所有的工作。仓库是一个目录，它包含所有我们希望保留历史的源代码和这些源代码的历史记录。

最简单开始 Mercurial 的方法是使用一个已经包含文件和一些历史记录的仓库。

我们使用 `clone` 命令来做这个事情。 这生产一个仓库的克隆，它生成一个完整的仓库复本，这样我们有一个本地私有的仓库来工作。

让我们克隆一个在 selenic.com 上的 "hello, world" 仓库：

```
$ hg clone http://www.selenic.com/repo/hello my-hello 
```

如果所有都没问题，`clone` 命令输出：

```
requesting all changes
adding changesets
adding manifests
adding file changes
added 2 changesets with 2 changes to 2 files 
```

我们应该在当前目录下发现一个目录叫 `my-hello`：

```
$ ls
my-hello 
```

在 `my-hello` 目录中，我们应该发现这些文件：

```
$ ls my-hello
Makefile  hello.c 
```

这些文件是我们刚克隆的仓库的精确复本。

**注：** 在 Mercurial 中， 每一个仓库是自包含的。当你克隆一个仓库后，新仓库变成克隆时它的精确复本， 但是后续的两个仓库当中任一方改变都不会在对方显示，除非你用 Pull 或 Push 明确地传递改变。

现在我们可以检查新仓库的一些历史记录, 继续 教程 - 检查仓库历史。