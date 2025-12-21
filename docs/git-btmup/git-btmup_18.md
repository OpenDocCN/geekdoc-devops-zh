# 进行硬重置

> 原文：[`jwiegley.github.io/git-from-the-bottom-up/3-Reset/4-doing-a-hard-reset.html`](http://jwiegley.github.io/git-from-the-bottom-up/3-Reset/4-doing-a-hard-reset.html)

硬重置（`--hard` 选项）可能非常危险，因为它能够同时做两件事：首先，如果你对你的当前 HEAD 进行硬重置，它将擦除工作树中的所有更改，使得你的当前文件与 HEAD 的内容相匹配。

此外，还有一个名为 `checkout` 的命令，如果索引为空，则其操作方式与 `reset --hard` 相同。否则，它会强制你的工作树与索引匹配。

现在，如果你对较早的提交进行硬重置，这相当于先进行软重置，然后使用 `reset --hard` 来重置你的工作树。因此，以下命令是等效的：

```sh
$ git reset --hard HEAD~3  # Go back in time, throwing away changes
$ git reset --soft HEAD~3  # Set HEAD to point to an earlier commit
$ git reset --hard  # Wipe out differences in the working tree

```

如你所见，进行硬重置可能会非常破坏性。幸运的是，有一个更安全的方法可以达到相同的效果，那就是使用 Git stash（见下一节）：

```sh
$ git stash
$ git checkout -b new-branch HEAD~3   # head back in time!

```

如果你不确定你是否真的想现在修改当前分支，这种方法有两个明显的优点：

1.  它将你的工作保存到 stash 中，你可以在任何时候返回。请注意，stash 不是分支特定的，所以你可以在一个分支上保存树的状态，然后稍后将其应用到另一个分支上。

1.  它将你的工作树回滚到过去的状态，但是在一个新的分支上，所以如果你决定在过去的状态下提交你的更改，你不会改变你的原始分支。

如果你修改了 `new-branch` 并决定希望它成为你的新主分支，请运行以下命令：

```sh
$ git branch -D master  # goodbye old master (still in reflog)
$ git branch -m new-branch master  # the new-branch is now my master

```

这个故事的意义是：虽然你可以使用 `reset --soft` 和 `reset --hard`（这也会更改工作树）对你的当前分支进行重大手术，但你为什么要这样做呢？Git 使得处理分支变得如此简单和便宜，几乎总是值得你在分支上进行破坏性修改，然后将该分支移动到取代旧主分支的位置。它有一种几乎像西斯一样的吸引力……

如果你意外地运行了 `reset --hard`，不仅丢失了当前更改，还从主分支中移除了提交？嗯，除非你已经养成了使用 stash 来保存快照的习惯（见下一节），否则你无法恢复丢失的工作树。但你可以通过再次使用带有 reflog 的 `reset --hard` 来恢复你的分支到之前的状态（这将在下一节中解释）：

```sh
$ git reset --hard HEAD@{1}   # restore from reflog before the change

```

为了安全起见，永远不要在没有先运行 `stash` 的情况下使用 `reset --hard`。这将在以后为你节省许多白发。如果你已经运行了 stash，现在你可以使用它来恢复你的工作树更改：

```sh
$ git stash  # because it's always a good thing to do
$ git reset --hard HEAD~3  # go back in time
$ git reset --hard HEAD@{1}  # oops, that was a mistake, undo it!
$ git stash apply  # and bring back my working tree changes

```
