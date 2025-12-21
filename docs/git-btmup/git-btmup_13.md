# 深入索引

> 原文：[`jwiegley.github.io/git-from-the-bottom-up/2-The-Index/2-taking-the-index-further.html`](http://jwiegley.github.io/git-from-the-bottom-up/2-The-Index/2-taking-the-index-further.html)

让我们看看索引……有了它，你可以在提交到仓库之前预先分阶段一组更改，从而迭代地构建一个补丁。现在，我在哪里听说过这个概念……

如果你想到“Quilt!”，那你就完全正确了。实际上，索引与 Quilt 几乎没有区别，它只是增加了每次只能构建一个补丁的限制。

但如果，不是在 `foo.c` 中的两组更改，而是有四组呢？使用纯 Git，我必须逐一提取它们，提交，然后提取下一个。使用索引可以使这变得容易得多，但如果我们想在检查它们之前以各种组合测试这些更改，那会怎样？也就是说，如果我为补丁标记了 A、B、C 和 D，那么在决定任何更改是否真正完整之前，我想测试 A + B，然后是 A + C，然后是 A + D 等，会怎样？

Git 本身没有机制允许你即时混合和匹配并行集的更改。当然，多个分支可以让你进行并行开发，索引可以让你将多个更改分阶段放入一系列提交中，但你不能同时进行：在分阶段一系列补丁的同时，选择性地启用和禁用其中一些，以在最终提交之前验证补丁的一致性。你需要一个可以一次处理多个提交的索引，这正是 Stacked Git 提供的。

这是如何使用纯 Git 将两个不同的补丁提交到我的工作树中的示例：

```sh
$ git add -i # select first set of changes
$ git commit -m "First commit message"
$ git add -i # select second set of changes
$ git commit -m "Second commit message"

```

这效果很好，但我不能选择性地禁用第一个提交以单独测试第二个。为了做到这一点，我必须做以下操作：

```sh
$ git log # find the hash id of the first commit
$ git checkout -b work <first commit’s hash id>
$ git cherry-pick <second commit’s hash id>
<... run tests ...>
$ git checkout master # go back to the master "branch"
$ git branch -D work # remove my temporary branch

```

当然，肯定有更好的方法！使用 `stg`，我可以排队等待多个补丁，然后按我喜欢的顺序重新应用它们，用于独立或组合测试等。以下是我如何使用 `stg` 排队之前示例中的相同两个补丁的示例：

```sh
$ stg new patch1
$ git add -i  # select first set of changes
$ stg refresh --index
$ stg new patch2
$ git add -i  # select second set of changes
$ stg refresh --index

```

现在我如果想选择性地禁用第一个补丁以仅测试第二个，那就非常直接：

```sh
$ stg applied
patch1
patch2
<...  do tests using both patches ...>
$ stg pop patch1
<...  do tests using only patch2 ...>
$ stg pop patch2
$ stg push patch1
<...  do tests using only patch1 ...>
$ stg push -a
$ stg commit -a  # commit all the patches

```

这肯定比创建临时分支并使用 `cherry-pick` 应用特定的提交 ID，然后删除临时分支要简单得多。
