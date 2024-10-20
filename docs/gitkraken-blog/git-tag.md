# Git Tag | Learn Git

> 原文：<https://www.gitkraken.com/learn/git/git-tag>

Git 允许您使用 [Git 提交](https://www.gitkraken.com/learn/git/commit)来拍摄您的工作随时间变化的快照。虽然您可以随时使用 [Git checkout](https://www.gitkraken.com/learn/git/git-checkout) 返回查看您的工作，但是如果您有许多提交，那么试图记住哪些提交包含重要的里程碑或特性就绪代码会很快变得令人困惑。这就是 Git 标记的用武之地。

Git 标记是一个命令，它将一个指针附加到一个人类可读的特定提交。与 Git 中的其他指针不同，标签一旦设置就不会移动；它们将在您的项目历史中始终指向同一个提交。Git 标签也是类似提交的引用，这意味着它们导致特定的提交，并可用于类似于 [Git checkout](https://www.gitkraken.com/learn/git/git-checkout) 、 [Git push](https://www.gitkraken.com/learn/git/git-push) 或 [Git diff](https://www.gitkraken.com/learn/git/git-diff) 的操作。

Git 标签的一个常见用例是在发布可以使用的代码的过程中。例如，查看 [GitLens 项目存储库](https://github.com/gitkraken/vscode-gitlens/tags)，您可以看到每个发布都标有一个发布号。但是，您可以出于任何原因使用标签来标记您的位置。虽然标记是不可移动的，但是它们很容易创建和删除，所以无论出于什么目的，当您想要标记提交时，都应该毫不犹豫地尝试使用 Git 标记。

在本文中，我们将探索以下利用 Git 标签的方法:

如果您想扩展您对 Git 标记的了解，可以看看这些相关的文章:

轻量级 Git 标签

## 轻量级 Git 标签只是一个可以引用的命名指针。它们之所以被称为轻量级，是因为它们只包含标签的名称。因为它永久连接到提交，所以在检查时，它将显示提交消息，而不是任何附加信息。如果您想要向 Git 标签添加自定义消息，您将需要使用带注释的标签。

注释 Git 标签

带注释的 Git 标签是除了标签名称之外还包含自定义消息的标签。带注释的 Git 标签通常携带标签所指向的提交消息之外的信息。这可能是一些具体的功能或产品信息，或者只是提醒你为什么要做这个标签。当在有许多参与者的大型项目中工作时，带注释的标签是受欢迎的，因为标签的名称通常不足以解释为什么要创建标签。

## Git Tag List

要查看与您的存储库相关联的 Git 标签，您有两个选项。

首先，Git 标签确实出现在你的 Git 历史中。当运行`git log`或`git log –oneline`时，Git 会在提交 ID 旁边显示标签。

要查看与您的存储库相关联的 Git 标签，您有两个选项。

首先，Git 标签确实出现在你的 Git 历史中。当运行`git log`或`git log –oneline`时，Git 会在提交 ID 旁边显示标签。

查看所有标签的另一种方法是用命令`git tag –list`列出它们

查看所有标签的另一种方法是用命令`git tag –list`列出它们

这将只显示按字母升序排列的标签名称。如果您想要查看与标签相关联的注释，您将需要使用选项`-n`。命令是`git tag –list -n`

这将只显示按字母升序排列的标签名称。如果您想要查看与标签相关联的注释，您将需要使用选项`-n`。命令是`git tag –list -n`

请注意所有标签是如何显示注释的。如果标记的制作者没有手动设置注释，`-n`选项将显示与标记指向的提交相关的提交消息。

Git Delete Tag

有时您会意识到您创建的 Git 标签有问题。您可能需要更新命名约定，或者您可能将标记添加到了错误的提交中。这里最简单的解决方案是删除现有的标签并创建一个新的。由于有了`-d`选项，删除 Git 标签变得很简单。要删除 Git 标记，请使用以下命令

`git tag -d <tag-name>`

记住用您想要删除的标签的名称替换`<tag-name>`。

## Git Delete Tag

终端中的 Git 标签

要在 Git CLI 中创建轻量级标记，请使用命令

`git tag <tag-name>`

记住用您想要创建的标签的名称替换<tag-name>。</tag-name>

要在 Git CLI 中创建轻量级标记，请使用命令

要在 Git CLI 中创建带注释的标记，请使用命令

`git tag -a <tag-name> -m '<message>'`

记住用您想要创建的标签的名称替换<tag-name>，用您想要用于注释的消息替换<message>。</message></tag-name>

要在 Git CLI 中创建带注释的标记，请使用命令

要列出标签及其注释，使用命令

`git tag --list -n`

记住用您想要创建的标签的名称替换<tag-name>，用您想要用于注释的消息替换<message>。</message></tag-name>

`git tag -d <tag-name>`

记得用您想要删除的标签的名称替换<tag-name>。</tag-name>

转到 gitkraken client 中的标签

GitKraken 客户端使得使用 Git 标签变得异常容易。你可以在左侧面板的[标签部分看到 GitKraken 客户端的所有 Git 标签。GitKraken 客户端方便地按时间倒序显示 Git 标签，这意味着为了方便起见，最新的标签总是在顶部。](https://help.gitkraken.com/gitkraken-client/interface/)

记得用您想要删除的标签的名称替换<tag-name>。</tag-name>



使用 GitKraken 客户端的内置终端或点击几下鼠标，使用 Git 标签非常容易。

## 转到 gitkraken client 中的标签

为了在 GitKraken 客户端中查看 Git 标签注释，将鼠标悬停在提交图中 Git 标签旁边的标签图标上。

单击一个标签将跳转到提交历史中的特定 Git 标签的提交图。

注意:如果你点击一个没有加载到 GitKraken 客户端提交图中的提交，它不会在图中显示。您需要调整从`Preferences`–>–`General`菜单加载的提交范围。

要使用 GitKraken 客户端创建一个新的 Git 标记，右键单击要添加标记的提交并选择`Create tag here`或`Create annotated tag here`选项。

为了在 GitKraken 客户端中查看 Git 标签注释，将鼠标悬停在提交图中 Git 标签旁边的标签图标上。

如果你选择`Create annotated tag here`，GitKraken 客户端将打开一个界面，允许你输入想要的注释。

单击一个标签将跳转到提交历史中的特定 Git 标签的提交图。

注意:如果你点击一个没有加载到 GitKraken 客户端提交图中的提交，它不会在图中显示。您需要调整从`Preferences`–>–`General`菜单加载的提交范围。

要删除 GitKraken 客户端中的标签，只需右击您想要删除的标签并选择`Delete <tag-name> locally`。

vs 代码 gittens 中的 git 标签

当您使用 [GitLens](https://www.gitkraken.com/gitlens) 时，在 VS 代码中使用 Git 标签非常简单。GitLens 将 Git 标签公开为 GitLens 侧栏部分。打开此部分将列出所有标签及其注释。

要在 GitLens 中创建新的 Git 标签，请导航到 GitLens 的标签侧栏部分，然后单击`+`图标。GitLens 会打开一个界面，引导你创建一个新的标签。

## vs 代码 gittens 中的 git 标签

要删除 VS 代码中带有 GitLens 的 Git 标签，右键单击要删除的标签，选择选项`Delete Tag`

开始利用 Git 标签

无论您如何制作和管理 Git 标签，它们都可以让您向任何标志着里程碑的提交添加消息，从而帮助您理解您的 Git 历史。无论是为了发布候选版本、新特性的引入，还是为了方便以后查找代码片段，标签都是保持有序的好方法。GitLens 和 GitKraken Client 都可以让您更好地查看您的标签，并以更有效的方式管理它们。如果你使用的是 VS 代码，今天就安装并开始使用 GitLens。无论你的 chouse 的代码编辑器是什么，GitKraken 客户端，传说中的跨平台 Git 客户端，可以帮助你管理你的 repos，并与他人更好地协作！

要在 GitLens 中创建新的 Git 标签，请导航到 GitLens 的标签侧栏部分，然后单击`+`图标。GitLens 会打开一个界面，引导你创建一个新的标签。

要删除 VS 代码中带有 GitLens 的 Git 标签，右键单击要删除的标签，选择选项`Delete Tag`

开始利用 Git 标签

## 无论您如何制作和管理 Git 标签，它们都可以让您向任何标志着里程碑的提交添加消息，从而帮助您理解您的 Git 历史。无论是为了发布候选版本、新特性的引入，还是为了方便以后查找代码片段，标签都是保持有序的好方法。GitLens 和 GitKraken Client 都可以让您更好地查看您的标签，并以更有效的方式管理它们。如果你使用的是 VS 代码，今天就安装并开始使用 GitLens。无论你的 chouse 的代码编辑器是什么，GitKraken 客户端，传说中的跨平台 Git 客户端，可以帮助你管理你的 repos，并与他人更好地协作！

No matter how you make and manage Git tags, they can help you make sense of your Git history by letting you add messages to any commit that marked a milestone. Wether for a release candidate, a new feature’s introduction, or just to make it easy to find a snippet of code later, tags are a great way to stay organized.  GitLens and GitKraken Client can both give you a better view of you tags and a more efficient way to manage them.  If you are using VS Code, install and start using GitLens today.  No matter what your code editor of chouse is, GitKraken Client, the legendary cross platform Git client can help you manage your repos and collaborate better with others!