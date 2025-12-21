# 保存和 reflog

> 原文：[`jwiegley.github.io/git-from-the-bottom-up/4-Stashing-and-the-reflog.html`](http://jwiegley.github.io/git-from-the-bottom-up/4-Stashing-and-the-reflog.html)

到目前为止，我们已经描述了 blob 进入 Git 的两种方式：首先，它们在索引中创建，既没有父树也没有拥有提交；然后它们被提交到仓库中，在那里它们作为树叶悬挂在由那个提交持有的树上。但还有两种方式 blob 可以存在于你的仓库中。

这些中的第一个是 Git 的 `reflog`，它是一种元仓库，以提交的形式记录了你对你仓库所做的每一个更改。这意味着当你从索引创建一个树并将其存储在一个提交下（所有这些操作都是由 `commit` 完成的）时，你无意中也将这个提交添加到了 reflog 中，你可以使用以下命令来查看：

```sh
$ git reflog
5f1bc85...  HEAD@{0}: commit (initial): Initial commit

```

reflog 的美妙之处在于它独立于你仓库中的其他更改而持续存在。这意味着我可以使用 `reset` 将上述提交从我的仓库中解除链接，但 reflog 仍然会在接下来的 30 天内引用它，从而保护它免受垃圾回收。这给了我一个月的时间来恢复这个提交，以防我发现我真的需要它。

其他地方，虽然间接，blob 也可以存在，那就是你的工作树本身。我的意思是，比如说你修改了一个文件 `foo.c`，但你还没有将这些更改添加到索引中。Git 可能没有为你创建 blob，但这些更改确实存在，这意味着内容存在——它只是存在于你的文件系统中而不是 Git 仓库中。这个文件甚至有自己的 SHA1 哈希 id，尽管实际上没有真正的 blob 存在。你可以使用以下命令来查看：

```sh
$ git hash-object foo.c
<some hash id>

```

这对你有什么帮助？嗯，如果你发现自己正在对工作树进行修改，并且到了一天结束的时候，养成一个好习惯就是将你的更改保存起来：

```sh
$ git stash

```

这将获取你目录的所有内容——包括你的工作树和索引的状态——并在 Git 仓库中为它们创建 blob，一个树来存储这些 blob，以及一对暂存提交来存储工作树和索引并记录你进行暂存的时间。

这是一个好习惯，因为尽管第二天你只需使用 `stash apply` 将你的更改从暂存中拉出来，但你将在每天结束时有一个所有暂存更改的 reflog。以下是你第二天早上回到工作后要做的（WIP 代表“工作进行中”）：

```sh
$ git stash list
stash@{0}: WIP on master: 5f1bc85...  Initial commit

$ git reflog show stash # same output, plus the stash commit's hash id 2add13e... stash@{0}: WIP on master: 5f1bc85... Initial commit

$ git stash apply

```

因为你的保存工作树存储在一个提交下，所以你可以像使用其他分支一样使用它——随时都可以！这意味着你可以查看日志，看到你保存了它的时间，并从你保存它们的那一刻起检出你过去的任何工作树：

```sh
$ git stash list
stash@{0}: WIP on master: 73ab4c1...  Initial commit
...
stash@{32}: WIP on master: 5f1bc85...  Initial commit
$ git log stash@{32}  # when did I do it?
$ git show stash@{32}  # show me what I was working on
$ git checkout -b temp stash@{32}  # let’s see that old working tree!

```

这个最后的命令特别强大：看吧，我现在正在玩弄一个超过一个月未提交的工作树。我甚至都没有将这些文件添加到索引中；我只是简单地使用在每天登出前调用 `stash` 的简单方法（前提是你实际上在工作树中有要暂存的更改），并在再次登录时使用 `stash apply`。

如果你想要清理你的暂存列表——比如说只保留过去 30 天的活动记录——不要使用 `stash clear`；而是使用 `reflog expire` 命令：

```sh
$ git stash clear  # DON'T! You'll lose all that history
$ git reflog expire --expire=30.days refs/stash
<outputs the stash bundles that've been kept> 
```

`stash` 的美妙之处在于它让你能够将无侵入式的版本控制应用到你的工作流程本身：即，你工作树从一天到另一天的各个阶段。如果你喜欢，甚至可以定期使用 `stash`，就像以下这样的 `snapshot` 脚本：

```sh
$ cat <<EOF > /usr/local/bin/git-snapshot
#!/bin/sh
git stash && git stash apply
EOF $ chmod +x $_
$ git snapshot

```

没有任何理由你不能每小时从这个 `cron` 任务中运行它，同时每周或每月运行一次 `reflog expire` 命令。
