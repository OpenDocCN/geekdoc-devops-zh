# 你如何执行一个交互式的 rebase？Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/git-interactive-rebase>

使用 Git 库时，即使是最细心的开发人员也会犯错误。假设您有一个包含错误的提交；对于如何[撤销 Git 提交](https://www.gitkraken.com/learn/git/problems/revert-git-commit#undo-git-commit)，您有几个选择。

在这种情况下，让我们假设错误的提交不是您的最后一次提交，并且您不想丢失中间的任何工作。例如，您的错误可能会影响随后的提交，因此通过更改一个提交，您可能想要删除一个提交，更改提交消息，或者将其中的一些提交挤压在一起。

这就是交互式 rebase 的用武之地。在包含错误的提交之后，您不必放弃所有的提交，而是可以再次应用它们，并逐个提交地分析更改的历史记录的影响。

Git 中的交互式 rebase 是一个工具，它为您的历史修订过程提供了更多的手动控制。当使用交互式 rebase 时，您将在您的分支的历史上指定一个点，然后您将看到一个到该点为止的提交列表。从这里开始，对于每个提交，您有多个选项:

1.  Pick:保持提交当前在历史中的状态
2.  删除:从历史记录中完全删除提交
3.  挤压:将提交与其之前的提交合并
4.  改写:更改 Git 提交消息

一旦您为 rebase 中的每个提交做出了选择，Git 将逐个提交从最旧的提交到最新的提交，尝试应用所请求的更改。

### **利用交互式重置基础解决冲突**

值得注意的是，在 Git 中使用交互式 rebase 工具时会出现冲突，每个冲突都必须在 rebase 继续之前解决。

类似于 Git 重置，交互式 rebase 改变提交的历史；它提供了选择我们想要保留和想要删除的提交的能力。当交互式 rebase“重放”新的提交历史时，它使用冲突来解决删除提交所引起的副作用。

合并冲突会阻止你拥抱交互式 rebase 的力量吗？坐下来，让 GitKraken 为你解决冲突。

### **在共享分支机构上使用交互式再基准**

和 Git reset 一样，interactive rebase 是一个应该谨慎使用的工具，尤其是当你发布到一个共享分支的时候。更改提交历史可能会对您的团队造成令人惊讶的影响，所以当您想要撤销共享分支上的提交时，通常最好使用 [Git revert](https://www.gitkraken.com/learn/git/problems/revert-git-commit) 。

现在，如果您决定对一个远程或者共享分支执行一个交互式的基础变更，那么在您开始基础变更之前，最好创建一个分支的备份。

*如果你喜欢在 Git 中观看交互式 rebase 的教程视频，[点击这里](https://www.gitkraken.com/learn/git/problems/git-interactive-rebase#interactive-rebase-tutorial-video)。*

### **Git Rebase 南瓜**

压缩提交有助于最小化与长提交链相关的噪音。尤其是在与他人协作时，在将您的工作合并到主分支之前，通常最好将一个长的提交链压缩成一个或几个提交。

使用交互式 rebase 来压缩提交可以让整个团队更容易阅读和维护提交日志。

### **如何在 GitKraken 中使用交互式 Rebase**

当对比在命令行中使用交互式 rebase 的体验时， [GitKraken](https://www.gitkraken.com/git-client) 让这个动作变得轻而易举。使用 Git 的可视化工具，您将对您的 rebase 有更多的信心和控制。

现在就开始大胆优化您的 GitKraken 项目历史。

要在 GitKraken 中启动交互式 rebase，首先将一个分支拖放到目标分支上，并从上下文菜单中选择`Interactive Rebase <branch name> onto <target branch name>`选项。或者，您可以右键单击任何父提交来访问相同的菜单选项。

在打开 GitKraken interactive rebase 工具时，默认情况下每个提交都将被设置为`Pick`，但是您可以在开始 rebase 之前手动定制您希望在每个提交上执行的操作。
T3，您可以选择`Reword`来更改提交消息。

GitKraken 也给了你快速`Drop`删除它们的能力。另外，您甚至可以在交互式 rebase 工具中拖放提交来重新排序它们。那是一些严重的控制。

### 你如何用 GitKraken 得到 rebase 壁球？

在 GitKraken 中的交互式 rebase 过程中，还可以选择`Squash`提交，它不仅结合了来自挤压提交的更改，还结合了提交消息，并将其添加到之前的提交中。

在此过程中的任何时候，您都可以单击`Reset`按钮重新开始，或者单击`Cancel Rebase`退出交互式重设基础工具。另一方面，如果一切看起来都很好，你可以点击`Start Rebase`，观察你的提交图神奇地相应更新。

当比较在命令行中执行交互式 rebase 时，GitKraken 提供了无缝的体验，允许您清楚地看到实时提交发生了什么。GitKraken 消除了交互式 rebase 的风险，并保护您和您的存储库免受冲突的影响。

使用 GitKraken Git GUI
像专业人员一样执行交互式重置

### **Git 命令行中的交互式 Rebase**

如果您希望在命令行中执行交互式 rebase，您可能会从运行`git branch`命令开始，以查看您的本地分支列表以及您当前已经签出的分支。

一旦您确定了您的目标分支，您将使用下面的命令来执行一个交互式的 rebase:

```
git rebase <target branch name> -i
```

这是在告诉 Git，您想要将当前签出的分支交互式地重新基础到目标分支上；这也将打开 Git 交互式 rebase 工具。

您将在顶部看到您已签出的分支上的提交列表，从这里，您可以为每个提交手动定制操作——拾取、挤压、丢弃或重绕。

在您为每个提交做出选择之后，您可以键入`wq`表示“写入并退出”,以启动交互式基础。

### **在命令行中解决交互式 Rebase 冲突**

如前所述，Git 会提醒您由于重定基数而产生的任何冲突，并且不允许您继续操作，直到冲突得到解决。

不像在 GitKraken 中，您可以立即识别冲突并使用有用的上下文来解决它，在命令行中可能会更麻烦。在这个例子中，您可以看到 Git 警告我们与我们的一个提交发生了冲突:`6780f75`。

从这里开始，您有三个选择:

`git rebase --continue`在解决冲突后，继续调整基数。

`git rebase --skip`跳过引起冲突的动作，

`git rebase --abort`一起取消改基。

要解决终端中的冲突，您将从运行`git status`命令开始，以获得关于冲突文件的更多信息。然后，您必须在文本编辑器中分别打开这些文件来解决冲突。

这就是所谓的上下文切换。这不仅会给你的工作流程增加时间，还会耗费更多的精神能量。这些压力累积起来。还记得在 GitKraken 解决合并冲突吗？其中以清晰的格式并排显示了冲突的文件，让您可以轻松选择保留什么和丢弃什么。无需离开应用程序。

好了，继续…

如果您试图使用`git rebase --continue`而没有解决冲突，Git 将再次告诉您有一个冲突需要解决。但是不错的尝试。😉

如果您现在不想花时间解决合并冲突，您可以跳过任何会导致冲突的操作，继续进行`git rebase --skip`操作。

GitKraken 不仅通过更好的视觉上下文使 Git 交互式 rebase 的整体体验更快、更直观，而且在解决合并冲突(执行此操作的常见风险)时发现了真正的特殊之处。

[https://www.youtube.com/embed/JkpYvXdbnfQ?feature=oembed](https://www.youtube.com/embed/JkpYvXdbnfQ?feature=oembed)

视频

谁的生活中需要更多的冲突？立即下载适用于 Windows、Mac、& Linux 的跨平台 [GitKraken Git 客户端](https://www.gitkraken.com/git-client)。