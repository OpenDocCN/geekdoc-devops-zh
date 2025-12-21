# 重置与否

> 原文：[`jwiegley.github.io/git-from-the-bottom-up/3-Reset/1-to-reset-or-not-to-reset.html`](http://jwiegley.github.io/git-from-the-bottom-up/3-Reset/1-to-reset-or-not-to-reset.html)

在 Git 中，`reset` 是一个较难掌握的命令，它似乎比其他命令更容易让人陷入困境。这是可以理解的，因为它有可能改变你的工作树和当前的 HEAD 引用。因此，我认为快速回顾这个命令会很有用。

基本上，`reset` 是一个参考编辑器、索引编辑器和工作树编辑器。这部分的复杂性在于它能够执行许多任务。让我们来分析一下这三种模式之间的区别，以及它们如何融入 Git 提交模型。
