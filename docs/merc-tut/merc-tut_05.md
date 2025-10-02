# 教程 - 与别的仓库分享改变

## 教程 - 与别的仓库分享改变

在 第一次改变的教程中，我们在`my-hello-new-output` 仓库中创建了一个变更集。现在我们希望在其它地方扩展那个变化。

遵循 Mercurial 好的风格，我们首先克隆我们原始的仓库.

```
$ cd ..
$ hg clone my-hello my-hello-share 
```

我们可以使用 `tip` 命令来找出每一个仓库的 Tip。(记住， Tip 是最后一个变更集。) 我们在这用了一个 `-q` ("保持安静") 参数来让 Mercurial 不要输出 Tip 的完整描述。

```
$ cd my-hello-share
$ hg -q tip
1:82e55d328c8c
$ cd ../my-hello-new-output
$ hg -q tip
2:a58809af174d 
```

我们可以看到， Tip 在各个仓库中是不同的。让我们回到 `my-hello-share` 并在那里扩展我们的新变更集。要达到这个目的，我们用 `pull` 命令，这个命令所有在别的仓库中有而在本仓库中没有的 变更集 从别的仓库 拉 到本仓库。

```
$ cd ../my-hello-share
$ hg pull ../my-hello-new-output
pulling from ../my-hello-new-output
searching for changes
adding changesets
adding manifests
adding file changes
added 1 changesets with 1 changes to 1 files
(run 'hg update' to get a working copy) 
```

不像其它普通的 Mercurial 命令，`pull` 有点罗嗦。在这点上 Pull 是成功的。

最近一行输出是重要的。在 Pull 后，缺省情况下 Mercurial 不更新工作目录。这意味着虽然 仓库 现在有变更集， 但在工作目录中的 `hello.c` 文件仍然是 Pull 之前老的内容。

我们可以用以下 Mercurial 的提醒来 Update 这个文件 (也包括所有其它 Pull 时改变的文件）。

```
$ hg update 
```

现在，我们可以检查并看到 `my-hello-share` 和 `my-hello-new-output` 有同样的内容和版本历史记录。

为确保与仓库是相同的，我们可以进入`my-hello-share`路径，然后做一个

```
$ hg pull ../my-hello-new-output
$ hg push ../my-hello-new-output 
```

如果这两个命令都返回'no changes found'，那意味着两个仓库是一致的，反之亦然，用'my-hello-new-output'代替'my-hello-share'。

为了和别人分享改变，我们继续导出。