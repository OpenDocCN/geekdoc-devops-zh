# 树是如何制作的

> 原文：[`jwiegley.github.io/git-from-the-bottom-up/1-Repository/4-how-trees-are-made.html`](http://jwiegley.github.io/git-from-the-bottom-up/1-Repository/4-how-trees-are-made.html)

每个提交都包含一个单独的树，但树是如何制作的呢？我们知道，blob 是通过将你的文件内容填充到 blob 中创建的——而树拥有 blob——但我们还没有看到包含 blob 的树是如何制作的，或者这棵树是如何与其父提交链接的。

让我们再次从一个新的示例仓库开始，但这次是通过手动操作，这样你可以感受到底层到底发生了什么：

```sh
$ rm -fr greeting .git
$ echo 'Hello, world!' > greeting
$ git init
$ git add greeting

```

所有这一切都是从你第一次将文件添加到索引开始的。现在，让我们只是说索引是你用来从文件中最初创建 blob 的东西。当我添加了 `greeting` 文件时，我的仓库发生了一个变化。我目前还看不到这个变化作为一个提交，但这是我可以知道发生了什么的一种方法：

```sh
$ git log # this will fail, there are no commits!
fatal: bad default revision 'HEAD'
$ git ls-files --stage # list blob referenced by the index
100644 af5626b4a114abcb82d63db7c8082c3c4756e51b 0 greeting

```

这是怎么回事？我还没有向仓库提交任何内容，但已经有一个对象产生了。它具有与我开始整个业务相同的哈希 ID，所以我知道它代表了我的 `greeting` 文件的内容。我现在可以使用 `cat-file -t` 在这个哈希 ID 上，我会看到它是一个 blob。实际上，这正是我在第一次创建这个示例仓库时得到的同一个 blob。相同的文件总是会得到相同的 blob（以防我没有强调这一点）。

这个 blob 还没有被树引用，也没有任何提交。目前它只被一个名为 `.git/index` 的文件引用，该文件引用了组成当前索引的 blob 和树。所以现在让我们在仓库中为我们的 blob 创建一个树：

```sh
$ git write-tree # record the contents of the index in a tree
0563f77d884e4f79ce95117e2d686d7d6e282887

```

这个数字也应该很熟悉：包含相同 blob（和子树）的树将始终具有相同的哈希 ID。我还没有提交对象，但现在在那个仓库中有一个树对象，它包含了 blob。低级 `write-tree` 命令的目的是将索引的内容取出来，放入一个新树中，以便创建提交。

我可以直接使用这个树手动创建一个新的提交对象，这正是 `commit-tree` 命令所做的：

```sh
$ echo "Initial commit" | git commit-tree 0563f77
5f1bc85745dcccce6121494fdd37658cb4ad441f

```

原始的 `commit-tree` 命令接受一个树的哈希 ID 并创建一个提交对象来持有它。如果我想让这个提交有一个父提交，我必须使用 `-p` 选项显式指定父提交的哈希 ID。此外，请注意这里的哈希 ID 与你系统上显示的哈希 ID 可能不同：这是因为我的提交对象既引用了我的名字，也引用了我创建提交的日期，这两个细节将始终与你不同。

尽管如此，我们的工作还没有完成，因为我还没有将这个提交注册为分支的新头：

```sh
$ echo 5f1bc85745dcccce6121494fdd37658cb4ad441f > .git/refs/heads/master

```

这个命令告诉 Git，分支名“master”现在应该指向我们的最新提交。另一种更安全的方法是使用 `update-ref` 命令：

```sh
$ git update-ref refs/heads/master 5f1bc857

```

在创建 `master` 分支后，我们必须将我们的工作树与之关联。通常情况下，每次你检出分支时，这都会自动发生：

```sh
$ git symbolic-ref HEAD refs/heads/master

```

这个命令将 HEAD 符号性地关联到 `master` 分支。这很重要，因为从现在起，任何来自工作树的新提交都将自动更新 `refs/heads/master` 的值。

相信这如此简单似乎难以置信，但确实如此，我现在可以使用日志来查看我刚刚创建的提交：

```sh
$ git log
commit 5f1bc85745dcccce6121494fdd37658cb4ad441f
Author: John Wiegley <johnw@newartisans.com>
Date:   Mon Apr 14 11:14:58 2008 -0400
        Initial commit

```

顺便提一下：如果我没有将 `refs/heads/master` 设置为指向新的提交，它将被视为“不可达”，因为目前没有任何东西指向它，它也不是任何可达提交的父提交。在这种情况下，提交对象将在某个时刻从仓库中移除，连同其树和所有其 blob 文件一起。（这会自动通过一个名为 `gc` 的命令发生，你很少需要手动使用它）。通过将提交链接到 `refs/heads` 中的名称，就像我们上面做的那样，它成为一个可达提交，这确保了它从现在起将被保留。
