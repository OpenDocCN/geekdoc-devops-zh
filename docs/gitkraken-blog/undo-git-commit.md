# 撤销 Git 提交|如何撤销最后一次 Git 提交？

> 原文：<https://www.gitkraken.com/learn/git/problems/undo-git-commit>

即使对于更有经验、更勤奋的开发人员来说，在使用 Git 库时也会出错。那么当你在 Git 中犯了一个错误之后，最好的步骤是什么呢？嗯，那要看情况。

根据手头的情况，修复错误的最佳方法可能是全部撤销。但是请注意:如果您不小心，试图用错误的方式修复错误可能会导致您丢失重要的工作，引入代码冲突等等。

幸运的是，Git 确实提供了一些工具来帮助您撤销新提交引入的错误。让我们先看看在 [GitKraken Git 客户端](https://www.gitkraken.com/git-client)中撤销 Git 提交的所有选项，在查看 [CLI](https://www.gitkraken.com/cli) 中的过程之前，先看看 GUI。

“谢谢@GitKraken 的撤销功能，不小心丢弃了我所有的更改，但现在它们又回来了😃"–[@ offset 1337](https://twitter.com/offset1337/status/1107615811181658118)

## 你需要撤销 Git 提交吗？

在您开始撤销 Git 提交之前，您应该确保您真的想要*撤销*某个东西，而不仅仅是修复或编辑某个东西。如果您确实需要编辑您的最后一次提交，您可以[修改 Git 提交](https://www.gitkraken.com/learn/git/problems/git-commit-amend)。修改 Git 提交允许您纠正以前的提交消息，并向它添加更多的更改。

## **撤销上一次 Git 提交**

假设您提交了错误的文件，但是您还没有将代码更改推送到您的 [Git remote](https://www.gitkraken.com/learn/git/git-remote) 中，因此您需要 Git 撤销本地提交。如何从本地 Git 存储库中撤销最后一次提交或一组提交？

## **撤销 GitKraken 中的最后一次 Git 提交**

当你在 GitKraken 犯了一个错误时，解决方案只需点击一下。如果您在上次提交时犯了错误，并且希望在推送之前撤销上次 Git 提交，您可以简单地点击 UI 顶部工具栏上神奇的`Undo`按钮。

更好的是，如果您意识到您实际上不想撤销上一次 Git 提交，您可以单击`Redo`按钮来撤销您的撤销。

记住这一点很重要，如果你没有执行任何其他动作，使用`Undo`按钮可以撤销提交，但是假设你已经[创建了一个 Git 分支](https://www.gitkraken.com/learn/git/problems/create-git-branch)。在这种情况下，使用`Undo`按钮将撤销分支，而不是提交。

要在 GitKraken 中执行了另一个动作后撤销 Git 提交，必须使用 Git 重置特性。只需右击中间图形中的 commit，选择`Reset` - > `Soft`保留所有更改，或者选择->-`Hard`放弃更改，如果您确定将来不再需要它们的话。

无论您喜欢使用 GUI 还是 CLI，GitKraken 都可以使撤销操作(如撤销上次提交)变得简单而安全，因此您可以对自己的工作流程更有信心。

## **在 GitKraken CLI 中撤销上一次 Git 提交**

现在，让我们回顾一下如何使用 [GitKraken CLI](https://www.gitkraken.com/cli) 撤销您的最后一次 Git 提交。

**去重置**

## 与[恢复 Git 提交](https://www.gitkraken.com/learn/git/problems/revert-git-commit)相比，Git 重置允许您及时返回到特定的提交，并将您的活动位置重置为选定的提交。使用 Git 重置，您有两种选择:可以“软”重置 Git，也可以“硬”重置 Git。

与[恢复 Git 提交](https://www.gitkraken.com/learn/git/problems/revert-git-commit)相比，Git 重置允许您及时返回到特定的提交，并将您的活动位置重置为选定的提交。使用 Git 重置，您有两种选择:可以“软”重置 Git，也可以“硬”重置 Git。

如何撤销上一次提交并保留更改？

## 如果您想撤销上一次 Git 提交，但保留更改，软 Git 重置将达到目的。使用`--soft`标志将确保保留未完成修订中的更改。在执行软 Git 重置后，您可以在工作副本中找到这些未提交的本地修改。如果您使用的是 GitKraken CLI，只需输入:

如果您想撤销上一次 Git 提交，但保留更改，软 Git 重置将达到目的。使用`--soft`标志将确保保留未完成修订中的更改。在执行软 Git 重置后，您可以在工作副本中找到这些未提交的本地修改。如果您使用的是 GitKraken CLI，只需输入:

`$  git reset --soft HEAD~1`

`$  git reset --soft HEAD~1`

Git 重置应该小心使用。比方说，你回到了很久以前的某次犯罪。您一路传递的所有提交现在可能都以悬空状态结束，虽然它们存在，但是没有任何东西引用它们。如果您执行“硬”重置，您可能会丢失没有正确备份的工作。如果您确信不再需要这些更改，您应该只执行硬 Git 重置。

Git 重置应该小心使用。比方说，你回到了很久以前的某次犯罪。您一路传递的所有提交现在可能都以悬空状态结束，虽然它们存在，但是没有任何东西引用它们。如果您执行“硬”重置，您可能会丢失没有正确备份的工作。如果您确信不再需要这些更改，您应该只执行硬 Git 重置。

`$  git reset --hard HEAD~1`

`$  git reset --hard HEAD~1`

作为 Git 的最佳实践，您应该总是避免做任何需要强制推送的事情，这将重写您的主要分支的历史。

作为 Git 的最佳实践，您应该总是避免做任何需要强制推送的事情，这将重写您的主要分支的历史。

准备好使用 Git 革新您的工作流程吧，让 GUI 和 CLI 的强大功能触手可及，让您能够根据自己的喜好为每个操作选择最佳工具。

准备好使用 Git 革新您的工作流程吧，让 GUI 和 CLI 的强大功能触手可及，让您能够根据自己的喜好为每个操作选择最佳工具。