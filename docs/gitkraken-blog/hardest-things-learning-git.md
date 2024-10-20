# 学习 Git | Axosoft 最难的事情

> 原文：<https://www.gitkraken.com/blog/hardest-things-learning-git>

面对现实吧，理解 Git 很难。这很不公平，真的。到目前为止，您已经学习了各种不同的编码语言，您已经跟上了前沿的发展，然后您发现 Git 有自己的一堆术语和单词！

> Git 最难学的是什么？

所以，我们问你们，我们的 Twitter 社区，“关于 Git，最难学的是什么？”以下是我们对收到的一些回复的回答。

### 拥有一个远程存储库和一个本地存储库是怎么回事？

> 不知道最难的是什么，但最重要的是探索本地存储库的概念。 [#LearnGit](https://twitter.com/hashtag/LearnGit?src=hash)
> 
> —尤里·戈尔茨坦(@ urig)[2016 年 8 月 18 日](https://twitter.com/urig/status/766359007955124224)

 <picture decoding="async" class="wp-image-3724" title="<script>">![<script>](img/5d572f873fdc879429d985e66fce7a79.png)</picture> window.addEventListener('DOMContentLoaded', function() {

Repos 是项目中的文件，以及这些文件的历史记录。您的回购历史由提交组成，即您的回购文件在离散时间点的状态快照。

你可以在你的机器上有一个本地的回购，基本上在特定的时间点保存你的变更的历史，所以如果你需要的话你可以恢复。但当你同时拥有本地和远程版本的回购协议时，事情就变得更加复杂了。

在 GitHub 这样的远程服务上托管回购协议意味着所有开发人员都可以参与到同一个回购协议中，处理本地回购协议，然后在远程服务上反映这些变化。从中提取*远程回购将更新您的本地回购，因此它与远程回购保持同步。将*推至*远程回购将整合您的本地变更，以便其他人可以将它们拉至本地回购。*

这意味着不是签出文件，处理它们，然后再签入它们(还记得我们用 Dreamweaver 做的吗，非千禧一代？)团队成员可以同时处理相同的文件，并通过遥控器将更改合并在一起。

一旦你掌握了这个想法，它就会成为你的第二天性。只要记住:“先拉后推。”

### **#2 为什么有的复位软，有的复位硬？**

是啊，关于那个… *叹*。让我们深入研究一下:用`soft`、`mixed`和`hard `重置 git 的区别基本如下:`soft`让你的工作目录 ***和*** 的索引保持不变。`hard `重置你所有的工作。

> [@GitKraken](https://twitter.com/GitKraken) 合并冲突，重置概念。 [#LearnGit](https://twitter.com/hashtag/LearnGit?src=hash)
> 
> —Alex maday(@ Alex maday)[2016 年 8 月 18 日](https://twitter.com/alexmaday/status/766388219407577089)

 <picture decoding="async" class="wp-image-3725" title="<script>">![<script>](img/b95dff2ea4bea7bd2be6e7c1c2bda824.png)</picture> 

这意味着您已暂存/未暂存的任何内容都将保留在原来的位置，并且您之前签出的提交和重置为提交之间的所有更改都将放入暂存。保持您的工作目录不变，但是将您的索引重置为该提交。

基本上，它会将您重置为该提交，然后取消暂存所有内容，而不管您暂存了什么。

`hard`重置**一切**。您在工作目录中的任何工作都会被吹走。明确地说，这意味着当前分支上所有未被另一个提交保存在历史中的提交，无论是远程的还是本地的，都将丢失(除非使用一些 Git reflog 巫术)。

对于初学者来说，`soft`和`hard`的区别是最重要的区别。

### 我如何理解这种重新建立基础和合并的想法？HALP！

> [@GitKraken](https://twitter.com/GitKraken) 改变基础的概念可能很难
> 
> —约什·梅德斯基(@ joshmedeski)[2016 年 8 月 18 日](https://twitter.com/joshmedeski/status/766361520552882176)

 <picture decoding="async" class="wp-image-3726" title="<script>">![<script>](img/43653d2c5eeec502005211062bbd615a.png)</picture> 

这最好用视觉来解释。看看这个解释这一切的超级短视频。GitKraken Dev 特邀嘉宾 Chris Bargren 将带您了解这一过程。

[https://www.youtube.com/embed/a_msiOrYLgM?feature=oembed](https://www.youtube.com/embed/a_msiOrYLgM?feature=oembed)

视频

### 处理合并冲突让我觉得自己处在一段艰难的关系中。有没有不包括分手的建议？

让我们在个案的基础上处理这个问题。是的，大多数时候你可以通过谈话解决问题。有时候，可悲的是，你不得不说再见，从头开始。是的，我说的是你和 Git 的关系。

> 处理合并冲突[# learn git](https://twitter.com/hashtag/LearnGit?src=hash)[@ git kraken](https://twitter.com/GitKraken)
> 
> —布兰登(@ Brandon script)[2016 年 8 月 18 日](https://twitter.com/brandonscript/status/766361777109991424)

 <picture decoding="async" class="wp-image-3728" title="<script>">![<script>](img/e85028374d05c423ab11971656334cc2.png)</picture> 

当您试图合并同一文件同一行上的两个分支时，会发生合并冲突。例如，文件“A”在一个区域发生了更改，在另一个区域也发生了更改。如果变化存在于两条不同的线上，那就没什么大不了的。然而，如果更改在同一行上，我们就有问题了，现在由用户来决定做什么。

幸运的是，有一些工具可以在不同的方面帮助你(夫妻治疗的简称)。当有冲突时，大多数 Git 客户端和 CLI 会告诉您，专用的合并工具会帮助您编辑这些更改。

但是，GitKraken 的界面会以非常直观的方式向您显示冲突在哪里，并询问您希望如何合并。在 GitKraken Pro 中，您不仅可以优雅地合并，还可以选择受影响的行并进行编辑，而无需退出工具。今天没有痛苦的分手，我的朋友！

### 我是一个命令行纯粹主义者。我永远不会离开 **it，但是我对我必须学习的新东西的数量感到沮丧。有什么诀窍吗？**

我明白了。你喜欢命令行界面。你可以看到所有的命令在引擎盖下工作，或封面，或任何你想用的隐喻。你说你只是天生如此。好吧，那很酷。除了时间和训练，没有什么可以加速学习的过程。在 GitKraken，我们这样做是为了让你不必看到正在发生的命令。

你没听错。为了完成工作，你不需要知道每个命令，我在这里告诉你，这不是逃避。你还是个开发员。现在，去照照镜子告诉你自己。

> [@ Git kraken](https://twitter.com/GitKraken)[# Git](https://twitter.com/hashtag/Git?src=hash)[# learn Git](https://twitter.com/hashtag/LearnGit?src=hash)没有一个像样的 Linux git Gui 来支持命令行学习。 [#GoGoGitKraken](https://twitter.com/hashtag/GoGoGitKraken?src=hash)
> 
> —乔纳森·斯托里(@ jonistorey)[2016 年 8 月 18 日](https://twitter.com/jonistorey/status/766367501936652288)

 <picture decoding="async" class="wp-image-3727" title="<script>">![<script>](img/e85028374d05c423ab11971656334cc2.png)</picture> 

GitKraken 的目标是让你不需要知道“引擎盖下”发生的一切。你不需要在大脑中记住你正在运行 Git rebase branch A 和 branch B，你不需要记住收回你不小心做出的最后一次提交的命令是`git reset --soft HEAD~1`。您只需右键单击提交和`Reset -> Soft`。你不必挖掘`git reflog`的输出来撤销那个意外的`git reset --hard`。这是一个简单的点击撤消按钮。

所以，如果你开始认为可能有更好的方法；CLI 可能不是最重要的；也许有一个特殊的图形用户界面会把你从命令行界面拉出来。我们想告诉你:*没事的*！想要快速高效地完成工作，把一些记忆技能留给聚会游戏，这没什么。你会发现在 GitKraken 工作实际上更容易(拜托，谁不想这样呢？)

其他 Git 图形用户界面可能在过去让你焦头烂额，因为是的，虽然它们确实让工作更简单，但不一定更快。我们谅你也不敢给 GitKraken 一个机会，看看它是不是那个！