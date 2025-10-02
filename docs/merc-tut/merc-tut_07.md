# 教程 - 合并改变

## 教程 - 合并改变

在 《教程 - 和别人分享改变》一节, 我们学会了如何与其他人共享变更. 但是因为(0.7 版本开始) Import 不能正确的处理通过邮件发送的合并, 我们要演示如何从其他做了不兼容变更的仓库用拖合并。

首先, 我们必须创建合并的目标. 我们再次 Clone `my-hello`的仓库:

```
$ cd ..
$ hg clone my-hello my-hello-desc 
```

我们给 `hello.c` 的注释段加一段描述.

```
$ cd my-hello-desc
$ vi hello.c 
```

我们将第二行:

```
 * hello.c 
```

改为:

```
 * hello.c - hello, world 
```

我们存档并退出编辑器, 然后 Commit 我们的变更. 这次, 我们在`commit`命令中使用`-m` 选项来节省时间, 以免我们又要进入编辑器:

```
$ hg commit -m 'Add description of hello.c' 
```

这时, 我们已经对 `my-hello-new-output` 仓库中的 `hello.c` 作了一个变更 , 同时对 `my-hello-desc` 仓库 中的 `hello.c` 作了另一个变更. 我们怎样 *merge* 这两个分叉开发主线? 我们从一个仓库拖进另外一个仓库的时候会出现问题吗?

这完全没有问题. 现在仍然在 `my-hello-desc`中, 我们从 `my-hello-new-output`中 Pull 并且看看发生了什么:

```
$ hg pull ../my-hello-new-output
pulling from ../my-hello-new-output
searching for changes
adding changesets
adding manifests
adding file changes
added 1 changesets with 1 changes to 1 files (+1 heads)
(run 'hg update' to get a working copy) 
```

这看起来像 TutorialShareChange! 中 `pull` 的输出，所以我们现在要进行 Update 了, 对吧?

```
$ hg update
this update spans a branch affecting the following files:
 hello.c (resolve)
aborting update spanning branches!
(use 'hg merge' to merge across branches or 'hg update -C' to lose changes) 
```

噢，好像发生了什么事情. Mercurial 告诉我们必须 Merge 每个 仓库 中的变更. 看起来有点麻烦, 对吗? 其实很简单. 现在我们按照输出最后一行的指令执行:

```
$ hg merge
merging hello.c 
```

这就完成了! 通过调用`hgmerge`脚本(或者你自己定义的合并程序),Mercurial 可以自动的替你完成合并. Depending on your environment, the `hgmerge` may call a graphical merge resolver tool. If we look at `hello.c`, we find that it contains *both* the change from `my-hello-new-output` and the change from `my-hello-desc`.

(注意：在 Mercurial 0.9 版之前，因该使用`hg update -m`替代`hg merge`).

当合并他人所做的修改时，大部分时间都是这种还算容易应付的合并方式。

恩，让我们继续去学习如何处理冲突性变更的情况，这部分将在合并有冲突的改变中介绍。