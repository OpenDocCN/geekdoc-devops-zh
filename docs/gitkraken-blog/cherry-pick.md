# 如何在 Git 中挑选一个提交

> 原文：<https://www.gitkraken.com/learn/git/cherry-pick>

在我们深入 Git 中的挑选之前，让我们快速回顾一下提交。一个 [Git 提交](https://www.gitkraken.com/learn/git/commit)是您的存储库在某个时间点的快照，每次提交累积起来形成您的回购历史。

GitKraken 的直观用户界面使得执行挑选任务变得很容易，因为您可以在可视化图形中清楚地看到您的所有提交。

## 如何在 Git 中挑选提交？

在 Git 中，cherry pick 命令从目标提交中获取更改，并将它们放在当前签出分支的头部。

从这里，您可以继续在您的工作目录中处理这些更改，或者您可以立即将更改提交到新的分支。

## 什么时候应该使用 Git cherry pick 命令？

如果您不小心提交到错误的分支，cherry pick 命令会很有帮助。Cherry picking 允许您将这些更改放到正确的分支上，而无需重做任何工作。

提交被摘到新的分支后会发生什么？从这里开始，您可以在提交之前继续处理变更，或者您可以立即将变更提交到目标分支。

## Git 中的樱桃采摘和合并有什么区别？

挑选是在 Git 中将提交从一个分支转移到另一个分支的一种方法。但是要注意！谨慎使用 cherry pick 命令，因为过度使用它会导致重复提交和混乱的回购历史。

将提交转移到 Git 中另一个分支的另一种方法是合并，这种方法可能比 cherry pick 更好，以便保留提交历史。

了解更多关于[如何进行 Git 合并](https://www.gitkraken.com/learn/git/git-merge)以及当您遇到[合并冲突](https://www.gitkraken.com/learn/git/tutorials/how-to-resolve-merge-conflict-in-git)时会发生什么。

Git Cherry Pick 示例

## 现在我们已经讨论了什么是摘樱桃，让我们看看如何使用你的新能力！首先，我们将在介绍如何在命令行中使用 GitKraken Git GUI 进行 cherry pick 之前，先介绍一下如何使用 Git kraken Git GUI 进行 cherry pick。

现在我们已经讨论了什么是摘樱桃，让我们看看如何使用你的新能力！首先，我们将在介绍如何在命令行中使用 GitKraken Git GUI 进行 cherry pick 之前，先介绍一下如何使用 Git kraken Git GUI 进行 cherry pick。

git Cherry Pick git kraken 中的示例

## 首先，让我们看看 cherry pick 动作是如何使用跨平台 GitKraken Git GUI 工作的。

降低出错的风险！在你挑选之前，在 GitKraken Git GUI 中清楚地看到你的提交。

降低出错的风险！在你挑选之前，在 GitKraken Git GUI 中清楚地看到你的提交。

使用 GitKraken 的一个巨大好处是您的图形的视觉上下文是恒定的。您可以很容易地看到按时间顺序列出的所有提交，最近的提交在最上面。

不需要运行命令来显示图表，甚至不需要为目标提交获取 SHA。GitKraken 会帮你搞定一切的。

在这个精选的例子中，假设您有一个分支——*feature——一个*——被检出，并且您使用提交面板将您的更改提交到所述分支。

该死。您刚刚意识到应该提交到不同的分支: *feature-B* 。与其回去重做您的工作，您不如挑选提交。

要在 GitKraken 中[摘樱桃，双击你的目标分支——在本例中是*feature-B*——查看它。接下来，右键单击来自*特性的目标提交——一个*分支；这将打开一个上下文菜单。从这里，您可以选择`Cherry pick commit`。](https://support.gitkraken.com/working-with-commits/cherrypick/)

现在，你有两个选择。您可以立即将变更提交到 *feature-B* 分支，或者您可以继续处理它们。如果您选择后者，GitKraken 会将更改作为 WIP 节点的一部分添加到您的工作目录中。

你还没说完呢！为了确保您保持一个干净的回购历史，让我们回过头来检查一下*特性——一个*分支，并对父提交进行硬重置。这将从*特性——一个*分支中删除重复的提交。

命令行中的 Git Cherry Pick 示例

## 要在 CLI 中开始挑选过程，您首先需要获得您希望挑选的提交的 SHA。

因为终端缺少图形的持续可视上下文，所以您可能需要运行

显示您的图表并获得您的目标提交的 SHA。

```
git log --all --decorate --oneline --graph
```

从日志中复制 SHA 后，您可以运行

然后由 SHA 挑选目标提交。

```
git cherry-pick
```

如果该命令正确地执行了挑选，那么应该可以看到一个具有唯一 SHA 的新提交。

您还可以通过运行来验证一切看起来都很好

再次查看您的图表，现在应该在图表的顶部显示新提交及其 SHA 标识符(上面突出显示)。

```
git log --all --decorate --oneline --graph
```

再次查看您的图表，现在应该在图表的顶部显示新提交及其 SHA 标识符(上面突出显示)。

准备好更自信地采摘樱桃了吗？

## GitKraken Git 客户端让您可以更好地控制自己的选择，在中央图表中清楚地列出所有提交，这样您就可以在开始摆弄您的存储库之前确切地知道发生了什么。

GitKraken Git 客户端让您可以更好地控制自己的选择，在中央图表中清楚地列出所有提交，这样您就可以在开始摆弄您的存储库之前确切地知道发生了什么。