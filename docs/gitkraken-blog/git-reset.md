# Git 重置|硬、软和混合|学习 Git

> 原文：<https://www.gitkraken.com/learn/git/git-reset>

有时候，当使用一个 [Git 库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)时，你意识到你不想共享，甚至不想保存你的更改，你需要一种方法来撤销它们，比如说[撤销你的最后一次提交](https://www.gitkraken.com/learn/git/problems/undo-git-commit)。Git 提供了几种返回到前一次提交并从该点开始工作的方法。Git 提供的改变到先前状态的最强大的工具之一是 Git reset 命令。

Git reset 类似于 [Git checkout](https://www.gitkraken.com/learn/git/git-checkout) ，因为它允许你将头移动到你的历史中任何先前的提交。然而，与 checkout 不同，Git reset 将有效地取消提交起始状态和指定提交之间的所有更改。Git 可以完全丢弃所有这些更改，就像您将看到的 Git reset hard 命令一样，或者它可以在各种状态下保留这些更改，就像 Git reset soft 和 Git reset mixed 命令一样。

在本文中，我们将介绍 Git reset 命令的各种可用选项，以及如何在命令行和 [GitKraken 客户端](https://www.gitkraken.com/git-client)中执行它们:

[https://www.youtube.com/embed/s1idhUiCk38?feature=oembed](https://www.youtube.com/embed/s1idhUiCk38?feature=oembed)

视频

GitKraken 客户端提交图使得可视化和理解您的 Git 历史更加容易。GitKraken Client today ⬇️

Git 重置头

## 为了更好地理解 Git 重置是如何工作的，我们需要涵盖一些不同的 Git 核心概念。第一，当领导。

HEAD 的一个最好的定义来自于 Pro Git 的书:

通常头文件是对当前所在分支的符号引用。通过符号引用，我们的意思是与普通引用不同，它包含一个指向另一个引用的指针。

通常头文件是对当前所在分支的符号引用。通过符号引用，我们的意思是与普通引用不同，它包含一个指向另一个引用的指针。

通过运行 Git status 命令很容易看出 HEAD 指向的是什么，但是您也可以在。项目的 git 文件夹。`.git`文件夹是 Git 用来管理版本控制的，里面有一个特殊的文件叫做 HEAD。这个文件非常小，通常只包含一行，看起来像这样:

`ref: refs/heads/main`

Git checkout 和 Git reset 都改变了 HEAD 指向的位置，但是在 checkout 的情况下，它保留了所有其他的指针。另一方面，Git reset 告诉 Git 我们也想移动 HEAD 引用的指针。

Git checkout 和 Git reset 都改变了 HEAD 指向的位置，但是在 checkout 的情况下，它保留了所有其他的指针。另一方面，Git reset 告诉 Git 我们也想移动 HEAD 引用的指针。

Git 重置、工作目录和索引

### 为了理解 Git 重置，你必须首先理解另一个核心 Git 概念，它是 Git 用来管理你的工作的内部状态。你有时可能会看到这三种状态被称为“三棵树”。

这三种状态是:

工作目录

1.  索引
2.  提交历史记录
3.  工作目录

工作目录有时被称为您的工作进展，或 WIP。工作目录指的是已经*保存*但尚未暂存或提交的文件。您可以将您的工作目录视为您的沙箱，在您存放、提交或共享它们之前测试任何更改。

#### 索引

索引是 Git 工作流中的“暂存区”。在每个`.git`文件夹中，都有一个名为`index`的特殊文件。这是在执行 Git 添加时跟踪暂存文件的文件。

您可以打开并查看这个文件，但是它不是可读的。Git 将文件压缩成二进制大对象(blob ),并将这些 blob 添加到索引文件中。该指数有时被称为“分期指数”，因为这是它的功能。当您提交时，您提交的是在索引中找到的所有引用。

#### 提交历史记录

这可能是你已经最清楚了解的领域。您的提交历史是您随着时间的推移所做的提交链或项目快照。

虽然最常见的情况是，我们只是通过向历史中添加新的提交来及时向前移动，但是拥有一个清晰的提交链可以毫无问题地更改 HEAD 指向的位置。重要的是要理解，重置到一个特定的提交意味着您正在改变您当前的工作目录，并可能改变先前提交的索引。

去重置软

#### Git 复位可以使用的三种模式中的第一种是 Git 复位软命令的`--soft`。该选项将 HEAD 移回指定的提交，撤消 HEAD 指向的位置和指定的提交之间所做的所有更改，并将所有更改保存在索引中。换句话说，Git 将更改重新添加为 staged，准备再次提交。

我们来看一个例子。假设我们正在处理一个名为 *main* 的分支，并且有三个带有短 sha`98cs9`、`34ac2`和`f30ab`的提交，按照从最早到最新的顺序。

如果我们开始时 HEAD 指向`f30ab`，然后执行一个`git reset –soft 98ca9`，HEAD 将移动到那个提交，同时指针指向链的最后一个提交(在这个例子中，称为 *main* )。

在`34ac2`和`f30ab`中提交的所有更改都被保留，并作为分阶段更改重新添加到索引中。

去重置软

## Git reset soft 是一种非常安全的方法，可以返回到 Git 历史中的前一点，并保留所有更改。由于保留了更改，这是重写历史的一种方式，将多个提交中的所有更改应用到一个提交中，同时提供了同时进行其他更改的路径。

Git 重置混合

与 Git reset soft 类似，使用`--mixed`选项执行 Git reset 将撤销 HEAD 和指定 commit 之间的所有更改，但会将您的更改作为未暂存的更改保留在工作目录中。如果执行 Git 重置并且没有提供选项`--soft`、`--mixed`或`--hard`，Git 将默认使用`--mixed`。

让我们看看前面的同一个例子。同样，假设我们正在处理一个名为 *main* 的分支，并且有三个带有短 sha`98cs9`、`34ac2`和`f30ab`的提交，按照从最早到最新的顺序。

如果当我们开始执行一个`git reset –mixed 98ca9`时，HEAD 指向`f30ab`，HEAD 将移动到那个提交，同时指针指向链的最后一个提交(在这个例子中，称为 *main* )。

在`34ac2`和`f30ab`中提交的所有变更都被保留，并作为未分级变更重新添加到工作目录中。

Git 重置混合

与 Git reset soft 一样，Git reset mixed 是一种非常安全的方式，可以在不丢失更改的情况下返回到 Git 历史中的前一点。以这种方式重写您的 Git 历史允许您有选择地只保留您想要的，并在您认为合适的时候修改项目。一旦您的更改准备就绪，您就可以简单地执行 Git add，然后 [Git commit](https://www.gitkraken.com/learn/git/commit) 将这些更改添加到您项目的 Git 历史中。

## Git 硬重置

与 Git 软重置和混合重置不同，Git 硬重置有一些危险，因为它会自动丢弃 HEAD 和指定提交之间所做的所有更改。

Git reset hard 应该非常谨慎地使用，并且只用于您确定想要消除的本地更改。执行 Git 重置——当在一个共享分支上工作时，很难进行其他贡献者可以访问的提交，这可能会导致您的 Git 历史出现问题。综上所述，Git reset hard 实际上是一个非常方便的工具，可以快速返回到项目的先前状态。

假设您正在本地工作，并且已经提交了几次，然后才意识到您一直在一个糟糕的前提下工作，或者在您的工作中注入了一个反模式。这些提交中的工作都是不可用的，所以您认为没有理由保存这些更改。这是一个很好的例子，说明 Git reset hard 可以成为一个实时节省程序，让您丢弃那些更改并重新开始。

Git reset hard 应该非常谨慎地使用，并且只用于您确定想要消除的本地更改。执行 Git 重置——当在一个共享分支上工作时，很难进行其他贡献者可以访问的提交，这可能会导致您的 Git 历史出现问题。综上所述，Git reset hard 实际上是一个非常方便的工具，可以快速返回到项目的先前状态。

Git 重置很难放弃工作目录和索引更改

利用 Git reset hard 的另一种方法是使用它来丢弃工作目录和索引中的所有更改。您可以单独运行`git reset –hard`,而不是声明一个特定的提交。这仍然会将 Git HEAD 指向它已经签出的位置，并丢弃对索引和工作目录的任何尚未提交的更改。

虽然这仍然有一些危险，但是如果你意识到你已经添加了你肯定不想添加到你的 Git 历史中的代码，并且只是想要一个快速重置选项来让你回到初始状态，这是非常有用的。

Git 硬重置

GitKraken Client 使更高级的 Git 概念，如 reset，变得更容易使用，所以当你需要重写你的项目历史时，你会更有信心。

## 如何在命令行界面中重置 Git

让我们看一下在终端中使用 Git reset 的所有三个选项。对于这些例子，我们将使用 VS 代码的集成终端和开源的 [GitLens](https://www.gitkraken.com/gitlens) 库，以及带有终端和图形视图的 [GitKraken CLI](https://www.gitkraken.com/cli) 。

在执行任何 Git 重置操作之前，您需要首先查看您最近的历史记录。您可以在这里运行`git log` 来查看整个历史和完整的提交 sha。然而，对于这个例子，我们只关心最后几个提交，并且只需要短 sha 来引用这些提交。

要查看最近十次提交和缩短的提交 sha，可以运行:

`git log -10 –oneline`

利用 Git reset hard 的另一种方法是使用它来丢弃工作目录和索引中的所有更改。您可以单独运行`git reset –hard`,而不是声明一个特定的提交。这仍然会将 Git HEAD 指向它已经签出的位置，并丢弃对索引和工作目录的任何尚未提交的更改。

虽然这仍然有一些危险，但是如果你意识到你已经添加了你肯定不想添加到你的 Git 历史中的代码，并且只是想要一个快速重置选项来让你回到初始状态，这是非常有用的。

### `git log -10 –oneline`

GitKraken Client 使更高级的 Git 概念，如 reset，变得更容易使用，所以当你需要重写你的项目历史时，你会更有信心。

现在您可以看到哪些提交可以引用，您可以继续 Git 重置操作。

命令行界面中的 Git 软复位

要使用软选项执行 Git 重置，请使用以下命令:

## `git reset –soft <commit>`

这里，`<commit>`应该替换为提交 SHA，在您的 Git 历史中指定一个您想要重置的提交。

在运行 Git reset 之后，运行 Git status 是一个好主意，正如我们在下面的例子中所做的那样。这是一个非常安全的操作，它将帮助你确认事情按预期进行。

Git 重置混合在 CLI 中

要使用 mixed 选项执行 Git 重置，请使用以下命令:

`git reset –mixed <commit>`

或者，由于`--mixed`是 Git 重置的缺省值，您可以通过使用:

`git reset <commit>`

术语`<commit>`应该替换为提交 SHA，在您的 Git 历史中指定您想要重置到的更早的提交。同样在这个例子中，您可以看到运行 Git status 后的结果。

## 命令行界面中的 Git 硬重置

我们将提醒您仅在本地分支上使用此命令，并且仅当您确定要放弃所有更改时。没有办法硬撤销 Git 重置，所有的更改都将被永久销毁。

要在 CLI 中使用 hard 选项执行 Git 重置，您可以运行:

`git reset –hard <commit>`

用提交 SHA 替换`<commit>`,以指定您想要重置到 Git 历史中的哪个提交。您可以在运行 Git status 后看到结果，如下所示。

Git 重置混合在 CLI 中

要使用 mixed 选项执行 Git 重置，请使用以下命令:

如何使用 GitKraken 客户端重置 Git

## [GitKraken 客户端](https://www.gitkraken.com/git-client)通过上下文菜单轻松实现 Git 重置。当您右键单击任何提交时，您将看到一个选项:`Reset <branch-name> to this commit  >`，其中`<branch-name>`是当前检出的分支。这里的`>`让您知道有一个子菜单，您需要使用它来选择 Git 重置的选项。

GitKraken 客户端中的 Git 重置选项有:

git Rest Soft–保留所有更改

Git 重置混合–保持工作副本，但重置索引

Git 硬重置–放弃所有更改

git Rest Soft–保留所有更改

命令行界面中的 Git 硬重置

GitKraken 客户端中的 Git 重置软件

在 GitKraken 客户端中选择`Soft - keep all changes`选项将保存在 HEAD 指向的位置和所选提交之间所做的所有更改，作为暂存文件。

## `git reset –hard <commit>`

用提交 SHA 替换`<commit>`,以指定您想要重置到 Git 历史中的哪个提交。您可以在运行 Git status 后看到结果，如下所示。

在 GitKraken 客户端中选择`Soft - keep all changes`选项将保存在 HEAD 指向的位置和所选提交之间所做的所有更改，作为暂存文件。

“这是 Git 客户端大放异彩的情况之一。在之前的提交中使用@GitKraken 的软重置功能确实很好地向新开发人员直观地展示了 Git 的软重置功能有多酷。”–[@ igvolow](https://twitter.com/igvolow/status/1418323098504400899)

Replace `<commit>`  with the commit SHA to specify which commit earlier in your Git history you want to reset to. You can see the result after running Git status, as shown below.

Git 重置在 GitKraken 客户端中混合

在 GitKraken 客户端中选择`Mixed - keep working copy but reset index`选项会将 HEAD 指向的位置和所选提交之间所做的所有更改保存为未暂存文件。

GitKraken 客户端中的 Git 重置选项有:

## git Rest Soft–保留所有更改

Git 重置混合–保持工作副本，但重置索引

在 gitkraken client 中执行“重置硬”

*   在 GitKraken 客户端中选择选项`Hard - discard all changes`将会放弃在 HEAD 指向的位置和所选提交之间所做的所有更改。这里你应该小心，因为这是永久性的，不能用[撤销按钮](https://help.gitkraken.com/gitkraken-client/undo-and-redo/)撤销。
*   Git Reset Mixed – keep working copy but reset index
*   GitKraken 客户端中的 Git 重置软件

在 GitKraken 客户端中选择`Soft - keep all changes`选项将保存在 HEAD 指向的位置和所选提交之间所做的所有更改，作为暂存文件。

每个人有时都需要备份和重置到以前的状态。确保使用 GitKraken 客户端以最有效的方式进行。GitKraken 客户端中的传奇提交图可以帮助可视化您的回购历史，以确保您正在重置为正确的提交。而且，使用 GitKraken CLI，您仍然可以使用命令行，但是只需单击一下就可以复制提交的 sha！[下载并开始使用 GitKraken 客户端](https://www.gitkraken.com/git-client/try-free)，传说中的跨平台桌面 Git 客户端今天免费。

## Git Reset Soft in GitKraken Client

Selecting the option of `Soft - keep all changes` in GitKraken Client will preserve all the changes that had been made between where HEAD is pointing and the selected commit as staged files. 

“这是 Git 客户端大放异彩的情况之一。在之前的提交中使用@GitKraken 的软重置功能确实很好地向新开发人员直观地展示了 Git 的软重置功能有多酷。”–[@ igvolow](https://twitter.com/igvolow/status/1418323098504400899)

Git 重置在 GitKraken 客户端中混合

在 GitKraken 客户端中选择`Mixed - keep working copy but reset index`选项会将 HEAD 指向的位置和所选提交之间所做的所有更改保存为未暂存文件。

## Git Reset Mixed in GitKraken Client

Selecting the option of `Mixed - keep working copy but reset index` in GitKraken Client will preserve all the changes that had been made between where HEAD is pointing and the selected commit as unstaged files. 

在 gitkraken client 中执行“重置硬”

在 GitKraken 客户端中选择选项`Hard - discard all changes`将会放弃在 HEAD 指向的位置和所选提交之间所做的所有更改。这里你应该小心，因为这是永久性的，不能用[撤销按钮](https://help.gitkraken.com/gitkraken-client/undo-and-redo/)撤销。

## Git Reset Hard in GitKraken Client

Selecting the option of `Hard - discard all changes` in GitKraken Client will discard all the changes that had been made between where HEAD is pointing and the selected commit.  You should use caution here as this is permanent and can not be undone with the [Undo button](https://help.gitkraken.com/gitkraken-client/undo-and-redo/).  

每个人有时都需要备份和重置到以前的状态。确保使用 GitKraken 客户端以最有效的方式进行。GitKraken 客户端中的传奇提交图可以帮助可视化您的回购历史，以确保您正在重置为正确的提交。而且，使用 GitKraken CLI，您仍然可以使用命令行，但是只需单击一下就可以复制提交的 sha！[下载并开始使用 GitKraken 客户端](https://www.gitkraken.com/git-client/try-free)，传说中的跨平台桌面 Git 客户端今天免费。

Everyone needs to back up and reset to a previous state sometimes. Make sure you’re doing it the most efficient way by using GitKraken Client. The legendary commit graph in GitKraken Client can help visualize your repo’s history to make sure you’re resetting to the right commit.  And, with the GitKraken CLI you can still use the command line, but copy the commit SHAs with just one click!  [Download and start using GitKraken Client](https://www.gitkraken.com/git-client/try-free), the legendary cross platform desktop Git client free today.