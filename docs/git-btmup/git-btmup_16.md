# 进行混合重置

> 原文：[`jwiegley.github.io/git-from-the-bottom-up/3-Reset/2-doing-a-mixed-reset.html`](http://jwiegley.github.io/git-from-the-bottom-up/3-Reset/2-doing-a-mixed-reset.html)

如果你使用`--mixed`选项（或者根本不使用任何选项，因为这是默认设置），重置将使你的索引部分以及 HEAD 引用回滚以匹配给定的提交。与`--soft`选项的主要区别是`--soft`只改变 HEAD 的含义，而不触及索引。

```sh
$ git add foo.c  # add changes to the index as a new blob
$ git reset HEAD  # delete any changes staged in the index
$ git add foo.c  # made a mistake, add it back

```
