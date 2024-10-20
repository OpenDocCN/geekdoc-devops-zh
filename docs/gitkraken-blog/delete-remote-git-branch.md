# 如何删除远程 Git 分支？Git 问题的解决方案

> 原文：<https://www.gitkraken.com/learn/git/problems/delete-remote-git-branch>

在您开始在 Git 中删除远程分支之前，我们建议您熟悉如何删除本地分支。

在 Git 中，删除远程分支的工作方式与删除本地分支略有不同。

在 Git 中删除分支是一个破坏性的操作——Git kraken 使您能够安全地执行这个操作，而不会牺牲速度，无论您更喜欢可视化 Git 客户端还是 CLI，都可以让您对工作流更有信心。

## **如何使用 GitKraken CLI 删除远程 Git 分支**

如果使用 GitKraken CLI 删除远程分支，实际上不会使用 Git branch 命令来完成这个操作。如果您运行与远程分支相关的``git branch -d``命令，Git 会告诉您没有找到该分支。

您将使用 Git push 命令来删除远程分支，而不是使用 [Git branch](https://www.gitkraken.com/learn/git/branch) 命令。

您需要告诉 Git 您想要使用哪个远程存储库，后面是``--delete``标志，再后面是分支名称。

它应该是这样的:

```
`$ git push <name-of-remote-repository> --delete <branch-name>`
```

## **如何找到你的远程 Git 分支列表**

如果您使用终端来查看 Git 中远程分支的列表，您将需要运行``git branch -r``。

在对您的回购没有足够了解的情况下删除远程分支机构可能会很危险。降低丢失工作或使您的回购处于不良状态的风险，并通过 GitKraken 获得对您的远程分支机构的更多控制。

## **如何用 GitKraken 客户端删除远程分支**

因为删除一个远程分支是一个破坏性的操作，所以依赖 GitKraken 提供的视觉优势对初学者和专家都有好处。

在主 UI 中，您可以在左侧面板中的`REMOTE`下快速看到远程分支的列表，这些分支方便地组织在相关的远程存储库名称下。

ProTip:如果你的一个遥控器上有大量的分支，你可以在 Mac 上使用`Cmd + Option + f`，或者在 Windows/Linux 上使用`Ctrl + Alt + f`从左侧面板中过滤特定的分支。

要删除远程分支，只需在中央提交图或左侧面板中右键单击目标分支，然后从上下文菜单中选择`Delete <remote-branch-name>`。

还记得我们说过这是一个破坏性的 Git 操作吗？为了格外小心，GitKraken 会在继续之前要求你确认，让你和你的团队安心。

想让在 Git 中使用远程分支更容易吗？下载 [GitKraken Git 客户端](https://www.gitkraken.com/git-client)，现在有 GUI 和 [CLI](https://www.gitkraken.com/cli) ，今天免费。