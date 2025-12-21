# Blob 存储在树中

> [`jwiegley.github.io/git-from-the-bottom-up/1-Repository/3-blobs-are-stored-in-trees.html`](http://jwiegley.github.io/git-from-the-bottom-up/1-Repository/3-blobs-are-stored-in-trees.html)

你的文件内容存储在 blob 中，但这些 blob 功能相当有限。它们没有名字，没有结构——毕竟它们只是“blob”。

为了让 Git 表示你的文件的结构和命名，它将 blob 作为树中的叶节点附加。现在，仅通过查看它，我无法发现 blob 所在的树，因为它可能有成千上万的拥有者。但我知道它必须存在于我刚刚做出的提交持有的树中：

```sh
$ git ls-tree HEAD
100644 blob af5626b4a114abcb82d63db7c8082c3c4756e51b greeting

```

就在这里！这个第一个提交将我的问候文件添加到了仓库中。这个提交包含一个 Git 树，它只有一个叶节点：问候内容的 blob。

尽管我可以通过将 HEAD 传递给`ls-tree`来查看包含我的 blob 的树，但我还没有看到该提交引用的底层树对象。这里有一些其他命令来强调这种差异，并发现我的树：

```sh
$ git rev-parse HEAD
588483b99a46342501d99e3f10630cfc1219ea32 # different on your system

$ git cat-file -t HEAD
commit

$ git cat-file commit HEAD
tree 0563f77d884e4f79ce95117e2d686d7d6e282887
author John Wiegley <johnw@newartisans.com> 1209512110 -0400
committer John Wiegley <johnw@newartisans.com> 1209512110 -0400
Added my greeting

```

第一个命令将 HEAD 别名解码为它引用的提交，第二个命令验证其类型，而第三个命令显示了该提交持有的树的哈希 ID，以及存储在提交对象中的其他信息。提交的哈希 ID 对于我的仓库是唯一的——因为它包括我提交时的名字和日期——但树的哈希 ID 应该在你的示例和我的示例之间是通用的，因为它包含相同名称下的相同 blob。

让我们验证这确实是一个相同的树对象：

```sh
$ git ls-tree 0563f77
100644 blob af5626b4a114abcb82d63db7c8082c3c4756e51b greeting

```

就这样：我的仓库中只有一个提交，它引用了一个包含 blob 的树——包含我要记录的内容的 blob。我还可以运行一个额外的命令来验证这一点：

```sh
$ find .git/objects -type f | sort
.git/objects/05/63f77d884e4f79ce95117e2d686d7d6e282887
.git/objects/58/8483b99a46342501d99e3f10630cfc1219ea32
.git/objects/af/5626b4a114abcb82d63db7c8082c3c4756e51b

```

从这个输出中，我看到我的整个仓库包含三个对象，每个对象的哈希 ID 都出现在前面的例子中。让我们再最后看看这些对象的类型，以满足好奇心：

```sh
   $ git cat-file -t 588483b99a46342501d99e3f10630cfc1219ea32
   commit
   $ git cat-file -t 0563f77d884e4f79ce95117e2d686d7d6e282887
   tree
   $ git cat-file -t af5626b4a114abcb82d63db7c8082c3c4756e51b
   blob

```

在这个时刻，我本可以使用 show 命令来查看这些对象的简洁内容，但我将把这个作为读者的练习。
