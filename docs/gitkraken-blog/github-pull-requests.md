# GitHub 拉取请求|如何创建、审核和批准

> 原文：<https://www.gitkraken.com/learn/git/problems/github-pull-requests>

在 Git 中，拉请求是一个涉及项目贡献者的事件，他请求存储库维护者审查他们希望合并到项目存储库中的代码。这个特性并不是 Git 本身内置的，而是由远程存储库托管服务控制的一个功能，比如 [GitHub](https://github.com/) 。

GitTip:更像是视觉学习者？查看我们关于[什么是 Git](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git) 中的拉请求的简短中级 Git 教程视频。

## **如何创建 GitHub 拉取请求**

关于 Git pull 请求，需要记住的基本知识是它们依赖于分支。这意味着在您开始创建一个 pull 请求之前，您需要有一个包含您想要合并的变更的分支。

一旦您创建了一个包含您想要合并到项目主存储库中的更改的本地 Git 分支，您将想要 Git 将该分支推送到您的远程存储库中。

我们将使用著名的跨平台 GitKraken Git GUI 来查看创建和提交 GitHub pull 请求的过程，然后在 CLI 中查看该过程。

“GitKraken 有一个很好的用户界面来处理拉请求，并告诉你你在哪个分支，PR 指的是哪个分支(这是我经常出错的事情)。”–[@ spaghettidba](https://twitter.com/spaghettidba/status/1101111228888465408)

你有什么 GitKraken 可以帮你克服的反复出现的错误吗？

## **如何使用 GitKraken 创建 GitHub pull 请求？**

GitKraken 中的 [GitHub 集成](https://www.gitkraken.com/integrations/github)使得创建 GitHub 拉请求变得容易。

从左侧面板的`PULL REQUESTS`窗格中点击绿色的`+`按钮开始。

接下来，您将把想要合并的分支直接从中心图拖放到想要合并到的分支上。

了解更多关于使用[拖放来推入 GitKraken](https://support.gitkraken.com/working-with-repositories/pushing-and-pulling/#drag-and-drop-to-push) 的信息。

GitKraken 中的拖放功能使得推和拉以及创建拉请求变得直观，同时节省了您宝贵的编码时间。

## **GitHub 拉取请求模板**

如果这是一个新的本地分支，GitKraken 会询问您希望将更改推送到哪个远程分支。此时，您还将有机会在单击`Create Pull Request`按钮之前完成 GitHub pull 请求模板的字段，包括标题、描述、审阅者、受托人和标签。

## **GitHub 拉取请求草稿**

您还可以看到，通过单击`Submit as draft`旁边的框，可以选择创建 GitHub pull 请求草稿。

此外，如果您喜欢直接在托管服务上编辑 GitHub pull 请求，您可以单击`Continue editing on GitHub`链接。

## **查看 GitHub 拉取请求**

在 GitKraken 中创建 GitHub pull 请求后，您将能够从`PULL REQUESTS`窗格中查看 PR。点击单个 PR 将打开 GitKraken 针对 GitHub 的交互式拉式请求管理视图。

从这里，您可以查看您的 GitHub pull 请求，对您的 GitHub pull 请求进行额外的编辑，包括添加注释和关联构建状态和里程碑。您甚至可以在 GitKraken 的这个视图中直接合并和批准 GitHub pull 请求，并检出和测试 pull 请求分支。🤯

“新的 GitHub Pull 请求视图太棒了！👏加载速度比 GitHub 快！🔥"–杰克，GitKraken 用户

## **如何在命令行中创建 GitHub pull 请求？**

现在，如果您正在命令行中工作，并且有一个本地分支，其中包含您希望推送到远程的更改，那么您将从使用以下命令执行 Git push 开始:

`git push branch`

或者，如果您的分支在远程存储库中还不存在，您可以在 Git 中设置分支的上游。

`git push --set-upstream <branch-name> <branch-name>`

现在您的本地分支已经被推送，您可以开始您的 GitHub pull 请求了。要开始这个过程，您将导航到 GitHub 存储库的主页，并选择包含您的更改的分支。

接下来你会打`Contribute` → `Open pull request`。

您也可以导航到 GitHub pull requests 部分并点击`New pull request`。

现在，在这一点上，您需要检查两件事情:

*   “基础”分支应该是您希望合并到的分支
*   “比较”分支是您想要合并的分支

现在，您可以在点击`Create pull request`按钮之前添加 GitHub pull 请求标题和描述了。

专业提示:如果您还没有准备好提交您的 PR，您可以通过从下拉菜单中选择`Create draft pull request`选项来创建一个 GitHub pull 请求草稿。

关于使用命令行创建 GitHub pull 请求的更多信息，您可以阅读关于创建 pull 请求的 [GitHub 文档](https://docs.github.com/en/github/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request#creating-the-pull-request)和[从 fork 创建 GitHub pull 请求](https://docs.github.com/en/github/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request-from-a-fork)。

现在，使用 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 这个过程不是简单多了吗？