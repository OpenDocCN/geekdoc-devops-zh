# 介绍 blob

> 原文：[`jwiegley.github.io/git-from-the-bottom-up/1-Repository/2-introducing-the-blob.html`](http://jwiegley.github.io/git-from-the-bottom-up/1-Repository/2-introducing-the-blob.html)

现在基本的情况已经描绘出来了，让我们来探讨一些实际例子。我将从一个示例 Git 仓库开始创建，并展示 Git 在该仓库中是如何从底层工作的。在阅读时，请随意跟随：

```sh
$ mkdir sample; cd sample
$ echo 'Hello, world!' > greeting

```

在这里，我创建了一个名为“sample”的新文件系统目录，其中包含一个内容平淡无奇的文件。我甚至还没有创建仓库，但已经可以开始使用一些 Git 命令来理解它将要做什么。首先，我想知道 Git 将会把我问候文本存储在哪个哈希 ID 下：

```sh
$ git hash-object greeting
af5626b4a114abcb82d63db7c8082c3c4756e51b

```

如果你在这个系统上运行这个命令，你会得到相同的哈希 ID。尽管我们创建了两个不同的仓库（可能相隔甚远，甚至）但这两个仓库中的问候 blob 将会有相同的哈希 ID。我甚至可以从你的仓库中提取提交到我的仓库中，Git 会意识到我们正在跟踪相同的内容——而且我也会这样做！所以它只会存储一个副本！非常酷。下一步是初始化一个新的仓库并将文件提交进去。我现在将一次性完成这个步骤，然后回来分阶段做，这样你可以看到下面发生了什么：

```sh
$ git init
$ git add greeting
$ git commit -m "Added my greeting"

```

在这个阶段，我们的 blob 应该已经按照我们预期的样子存在于系统中，使用了上面确定的哈希 ID。为了方便起见，Git 只需要哈希 ID 的必要位数来在仓库中唯一标识它。通常只需要六位或七位数字就足够了：

```sh
$ git cat-file -t af5626b
blob
$ git cat-file blob af5626b
Hello, world!

```

就在这里！我甚至还没有查看哪个提交包含它，或者它在哪个树中，但仅凭内容，我就能假设它在那里，而且它就在那里。无论仓库存在多久，或者文件存储在哪里，它都会始终有这个相同的标识符。这些特定的内容现在已经被验证性地永久保存了。

以这种方式，Git blob 代表了 Git 中的基本数据单元。实际上，整个系统都是关于 blob 管理的。
