# 教程 - 合并有冲突的改变

## 教程 - 合并有冲突的改变

在合并中我们已经学会了如何处理简单的 Merge。

Mercurial 当然也处理更加复杂的 Merge。很平常的情况是两个人同时更改同一个文件的同一段代码，然后必须给出处理的方法。这称之为冲突；处理这类冲突称之为合并。

首先让我们人为的创建一个冲突的实例。 正如我们前面所做的， 通过做一个`my-hello`的 Clone 开始：

```
$ cd ..
$ hg clone my-hello my-hello-not-cvs 
```

现在，加入一些新的输出语句到 hello.c:

```
$ cd my-hello-not-cvs
$ vi hello.c 
```

改变 `main` 如下所示：

```
int main(int argc, char **argv)
{
        printf("hello, world!\n");
        printf("sure am glad I'm not using CVS!\n");
        return 0;
} 
```

然后 Commit 这些改变：

```
$ hg commit -m 'Give thanks for dodging bullet' 
```

正如第一次改变那样，我们在`my－hello-new－output`里建立了一个变更集，`my－hello-new－output`包含了第二输出行。如果这时我们使用 Pull 指令时，会发生什么事情呢？

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

到目前为止，非常顺利。 让我们试试 Update。

```
$ hg update
this update spans a branch affecting the following files:
 hello.c (resolve)
aborting update spanning branches!
(use 'hg merge' to merge across branches or 'hg update -C' to lose changes) 
```

正如合并那样，我们不得不运行 `hg merge`. 像往前一样， 合并程序将被启动。经常是不能自动合并，因为同样源文件的相同的代码在每个 ChangeSet 中被不同的方式更改(一个是我们提交的更改方式，一个是我们［Pull]来的)。

```
$ hg merge 
```

这时，会发生什么决定于电脑中安装了什么样的程序。 如果我们有先见之明或者幸运的话， 并且安装了图形的合并程序，我们就能够看到在两个更改之间发生了什么冲突，并决定如何去做。

Mercurial 使用了三路合并。 这就意味着有三个文件来做合并，分别是：

*   本地文件（当前仓库）
*   其它文件（正在被合并的仓库）
*   基文件 (在分支分开前的最后一个版本）

要了解更多的三路合并, 请参考[ThreeWayMerge](http://revctrl.org/ThreeWayMerge) on the [Revctrl wiki](http://revctrl.org/).

另外，如果我们没有安装图形合并程序， 我们就会开启文本编辑器来访问需要合并的文件。用手工来做这些事情是**非常**容易出错并且繁琐的。 最好是退出编辑器并用`hg rollback`指令来清除［"Pull"]带来的改变，然后安装合并程序，再做一次。

(注意： Mercurial 0.9 之前的版本，"hg merge" 必须使用 "hg update -m"代替，"hg rollback" 必须用 "hg undo" 代替。)

现在让我们继续并在总结中完成我们的教程。