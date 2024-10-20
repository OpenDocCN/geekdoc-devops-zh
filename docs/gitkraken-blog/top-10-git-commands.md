# 掌握 Git 需要知道的 10 大 Git 命令

> 原文：<https://www.gitkraken.com/blog/top-10-git-commands>

今天，如果不使用 [Git](https://git-scm.com/) ，开发人员的典型工作日将是不完整的。是的，Git 已经成长为我们正常开发过程的一个重要组成部分，它极大地促进了我们与他人的合作。

一个人必须熟悉它是如何运作的，才能有效地使用它并充分利用它。如果你是 Git 新手，你能做的最糟糕的事情就是复制和粘贴你在网上找到的答案，而不知道它们是做什么的或者什么时候会用到它们。

Git 有很多命令，学习如何使用它们需要时间。然而，某些命令比其他命令使用得更频繁。在这篇博客中，我们将帮助你学习 10 个信息丰富的 [Git 命令](https://www.gitkraken.com/learn/git/commands)，它们将把你的开发提升到一个全新的水平。

## 10 个最有用的 Git 命令

让我们列出 10 个关键的 Git 命令，您需要熟悉这些命令才能像专家一样管理您的 GitHub 库。

Git config

### Git 需要在使用之前进行配置。Git config 命令用于输入您希望与您的提交相关联的登录和电子邮件地址。

***#用你的名字*设置 Git**

`git config --global user.name "<Your-Full-Name>`******

 *`git config --global user.name "<Your-Full-Name>`

 ****#用你的邮箱*设置 Git**

`git config --global user.email "<your-email-address>"`

去吧，林特克

Git init 命令用于启动一个新的存储库。结果是在当前工作目录中创建一个. git 文件夹。

### `$ git init`

一个空的。上面的命令创建了 git 存储库。考虑在您的桌面上创建一个 Git 存储库。为此，您必须在桌面上打开 Git Bash 来运行上面的命令。

名为“”的新子目录。git”将由上面的命令创建，它包含所有需要的存储库文件。Git 存储库的概要可以在。git 子目录。

Git clone

命令“git clone”用于从远程存储库下载当前源代码(例如像 [GitHub](https://github.com/) )。Git clone 本质上是在存储库中创建一个项目最新版本的精确副本，并将其存储在您的计算机上。

有几种方法可以下载源代码，但我倾向于使用 *https 克隆*方法:

`$ git clone [https://github.com/](https://github.com/)<repo-url> `

### Git clone

例如，要从 GitHub 获得一个项目，我们只需点击下载或克隆按钮，从框中复制 URL，然后粘贴到 Git clone 命令之后。通过这样做，将在您的本地工作空间中创建一个项目的副本，允许您开始使用它。

Git status

Git status 命令是理解 Git 的关键。它将让我们知道 Git 正在处理什么，以及 Git 如何感知我们的存储库的状况。

`$ git status`

当您第一次启动时，您应该总是使用 Git status 命令！在任何其他命令之后启动它是一个聪明的想法。它将帮助您学习 Git，并防止您假设文件或存储库的当前状态。

去把它给我

### 使用 Git add 命令，可以将文件从工作目录转移到临时索引。要将本地版本与远程存储库中的版本进行比较，可以使用 Git add 命令将文件中的修改保存到临时区域。在提交之前，使用 Git add 命令将新的或修改过的文件包含在临时区域中。

要添加特定文件，请使用命令:

`$ git add <file1> <file2> … <fileN>`

要添加所有文件:

`$ git add -A`

去吧，Committee

### 经常使用的可能是 Git 命令。使用此命令–保存对 Git 存储库所做更改的日志消息和提交 id。Git 提交可以将修改存储在本地存储库中。

每次修改代码时，都必须提供一条关于所做更改的提交消息。这个提交消息使其他人更容易理解所做的更改。

`$ git commit –m "<Type your commit message here>"`

Git 分支

在 Git 领域，分支是基础。通过使用分支，多个开发人员可以同时协作处理同一个项目。git branch 命令可以创建、列出甚至删除分支。

`$ git branch <branch-name>`

`$ git add -A`

您可以使用此命令在本地创建一个分支。如果您想要将新的分支推入远程存储库，请使用以下命令。

### `$ git push-u <hybrid> <section-list>`

Git checkout

我们可以使用 Git checkout 命令创建并切换到一个新的分支或者一个现有的分支。要实现这一点，您必须能够在本地系统上访问您希望切换到的分支，并且您必须在切换之前提交或隐藏对当前分支的任何修改。

以下命令也可用于签出文件。

`$ git checkout <branch-name>`

### Git branch

如果要创建并切换到新分支，请使用命令:

`$ git checkout -b <new-branch-name>`

Git merge

一旦你完成了你的分支的开发，并且一切正常，最后一步就是将分支和父分支合并。Git merge 命令用于实现这一点。

要将不同的分支合并成一个分支，可以使用 Git merge 命令。请注意，您必须首先位于要与特征分支合并的特定分支上。要将您的特性分支合并到开发分支中，您需要使用以下命令切换到开发分支:

`$ git checkout dev`

### Git checkout

然后使用以下命令更新本地 dev 分支:

`$ git fetch`

最后，使用以下命令合并它:

`$ git merge <branch-name>`

Git 推拉

提交更改后，接下来的步骤是将它们发送到远程服务器。Git push 会将您的提交更新到远程存储库中。

`$ git push`

### 要创建新品牌并上传，请使用以下命令:

`$ git push --set-upstream`

要将不同的分支合并成一个分支，可以使用 Git merge 命令。请注意，您必须首先位于要与特征分支合并的特定分支上。要将您的特性分支合并到开发分支中，您需要使用以下命令切换到开发分支:

另一方面，您可以使用 Git pull 命令从远程存储库中获取更新。当您使用这个命令时，Git fetch 和 Git merge 操作都被执行，更新本地更改并将更新上传到远程存储库。

`$ git pull`

将你的新 Git 技能付诸行动

就是这样！我们已经学习了最常用的 Git 命令来帮助您简化开发，并将您的生产力提升到一个新的水平。

Git 是一个分布式版本控制系统，是一个开源软件。它使得处理各种源代码版本更加容易。Git 现在是所有开发人员的必备工具&要完全使用它们，开发人员必须熟悉它的命令。随着您将在开发过程中继续使用这些命令，您会发现它们有更多的用途。

如果你在执行 Git 命令时遇到问题，你可以随时[雇佣印度程序员](https://www.esparkinfo.com/hire-dedicated-developers-india.html)来简化你的编码之旅。

`$ git merge <branch-name>`

Git 推拉

### 提交更改后，接下来的步骤是将它们发送到远程服务器。Git push 会将您的提交更新到远程存储库中。

`$ git push`

要创建新品牌并上传，请使用以下命令:

`$ git push --set-upstream`

To create a new brand and upload it, use the command:

另一方面，您可以使用 Git pull 命令从远程存储库中获取更新。当您使用这个命令时，Git fetch 和 Git merge 操作都被执行，更新本地更改并将更新上传到远程存储库。

`$ git pull`

将你的新 Git 技能付诸行动

就是这样！我们已经学习了最常用的 Git 命令来帮助您简化开发，并将您的生产力提升到一个新的水平。

Git 是一个分布式版本控制系统，是一个开源软件。它使得处理各种源代码版本更加容易。Git 现在是所有开发人员的必备工具&要完全使用它们，开发人员必须熟悉它的命令。随着您将在开发过程中继续使用这些命令，您会发现它们有更多的用途。

## 如果你在执行 Git 命令时遇到问题，你可以随时[雇佣印度程序员](https://www.esparkinfo.com/hire-dedicated-developers-india.html)来简化你的编码之旅。

That’s it! We have learned the most frequently used Git commands to help you ease your development and take your productivity to the next level. 

Git is a distributed version control system that is open-source software. It makes handling various source code versions easier. Git is now a necessary tool for all developers & to use them in complete capacity, developers must be acquainted with its commands. As you will continue using these commands in your development process, you’ll find more use for such. 

And if you face concerns about executing the Git commands, you can always[hire Indian Coders](https://www.esparkinfo.com/hire-dedicated-developers-india.html) to simplify your coding journey.**