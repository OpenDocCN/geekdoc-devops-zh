# 如何从 SVN 迁移到 Git

> 原文：<https://www.gitkraken.com/blog/migrating-git-svn>

*文章更新于 2022 年 1 月*

您当前的 Subversion (SVN)版本控制系统不能满足您的开发团队的需求吗？也许你听说过 Git，但是你在 SVN 根深蒂固，转换到一个新的版本控制系统似乎是一项艰巨的任务。不要害怕！当你拥有传奇的 [GitKraken 客户端](https://www.gitkraken.com/git-client)的力量时，没有什么任务是无法完成的。

让我们来看看为什么应该考虑从 SVN 迁移到 Git，以及如何最好地完成这项任务。

超过 90%的开发者都在使用 Git。GitKraken Client 将有助于减轻迁移到 Git 的压力，并使您和您的团队的入职过程更加容易。

Git 的优势

## **流行度**–Git 是目前软件开发领域最广泛使用的版本控制系统。根据 Stack Overflow 的 [2021 年开发者调查，全球近 95%的专业开发者都在使用 Git 进行版本控制。这意味着在您的公司中采用 Git 不仅会让您进入业界首选的版本控制系统，而且还会减少新开发人员学习您的系统所需的时间。](https://insights.stackoverflow.com/survey/2021#technology-most-popular-technologies)

1.  **分布式版本控制**–Git 使用分布式方法进行版本控制，这与 SVN 的集中式方法形成了鲜明的对比。这意味着每个用户将存储库的完整版本克隆到他们的本地机器上，这在几个方面都是有利的。它消除了集中式存储库的单点故障，减少了日常操作的网络流量，并允许您的团队离线工作。
2.  **规模和速度**——可以说，迁移到 Git 的最大原因是分支和合并。创建一个分支是毫不费力的，并且是非常轻量级的，允许您的开发人员更快地工作和更容易地合并。****
3.  从 SVN 迁移到吉特

赞美北海巨妖！您已经决定继续进行从 SVN 到 Git 的迁移。那么，下一步是什么？规划总是一件好事，微软已经整理了一份全面的清单，列出了将您的团队迁移到 Git 时需要考虑的事项。

一旦您完成了规划阶段，就到了真正开始迁移代码的时候了。迁移的复杂性取决于几个因素:您的 SVN 存储库的复杂性，您已经完成了多少次合并，以及您是否关心重新格式化您的 SVN 存储库的历史。

## 重新格式化历史包括以下附加步骤:

将提交用户名转换为名字和姓氏，以及电子邮件地址

移除一些额外的特定于 SVN 的元数据

将`svn:ignore`文件迁移到`.gitignore`文件

*   将您的 SVN 标签转换为 git 标签
*   将所有 SVN 分支迁移到新的 Git remote 上
*   将`svn:ignore`文件迁移到`.gitignore`文件
*   如果你不关心保存上面的对象，继续进行下面的说明。如果您想迁移所有的历史数据，请跳到[导入和保存历史指令](#import-and-preserve-history)。
*   Git SVN Clone

如果您不担心重新格式化您的 SVN 存储库中的历史，那么转换过程就变得容易多了！Git 有一个内置的`git svn ` [命令](https://git-scm.com/docs/git-svn)，用于将一个 SVN 仓库克隆到一个新的 Git 仓库中。您只需运行:

`git svn clone <SVN_URL> -T trunk -b branches -t tags `

喝点咖啡…

## 这个过程可能需要一些时间，因为 g it 从您的 SVN 存储库中获取每个提交，并使用 Git 再次处理它。

如果您不担心重新格式化您的 SVN 存储库中的历史，那么转换过程就变得容易多了！Git 有一个内置的`git svn ` [命令](https://git-scm.com/docs/git-svn)，用于将一个 SVN 仓库克隆到一个新的 Git 仓库中。您只需运行:

`git svn clone <SVN_URL> -T trunk -b branches -t tags `

一旦命令完成，继续在 [GitKraken 客户端](https://www.gitkraken.com/git-client)中打开这个 repo，您应该会看到一个新转换的 Git repo 的漂亮图形。

此时，您可以跳到[设置您的新 Git 遥控器](#set-up-your-new-git-remote)，并且您就要完成了！

GitKraken Client 使 Git 更加可视化和直观，使初学者和专家开发者的入门过程更加容易，并为您提供了在 GUI 或终端之间切换的灵活性。

一旦命令完成，继续在 [GitKraken 客户端](https://www.gitkraken.com/git-client)中打开这个 repo，您应该会看到一个新转换的 Git repo 的漂亮图形。

带着历史和分支从 SVN 迁徙到 Git

进口由`git svn `做一个勇敢的工作；但是，还可以采取一些额外的步骤来执行更准确的导入，清理历史信息并对其进行重新格式化，使其看起来更像 Git 历史。

首先，我们来看作者信息。SVN 使用用户名跟踪提交，而 Git 使用全名和电子邮件地址。您可以在 SVN 存储库的工作目录中运行以下 bash 命令，以输出您的 SVN 作者列表:

您现在需要编辑`author-transformed.txt`文件中的每个作者，以匹配 Git 作者信息所需的语法。

## 例如:

`ryanp = ryanp <ryanp>`

变成了:

```
svn log -q | awk -F '|' '/^r/ {sub("^ ", "", $2); sub(" $", "", $2); print $2" = "$2" <"$2">"}' | sort -u > authors-transform.txt
```

`ryanp = Ryan Pinkus <ryanp@example.com>`

现在您已经准备好了作者列表，您可以使用`git svn`运行导入并指定`authors-transform.txt`。在 CLI 中将`authors-transform.txt`复制到一个新目录`C:/repo/temp`中，并将 cd 复制到该目录。

**注意**:如果在这个命令完成后，您看到一个空白的 Git 存储库，那么您的 SVN 存储库中可能没有标准布局。尝试移除
`--stdlayout`标志。

该命令会将 SVN 存储库克隆到 repo 目录的“temp”文件夹中的新 Git 存储库中。如果您在 [GitKraken 客户端](https://www.gitkraken.com/git-client)中打开 repo，您会看到提交现在是 Git 格式的。

`ryanp = Ryan Pinkus <ryanp@example.com>`

现在您已经准备好了作者列表，您可以使用`git svn`运行导入并指定`authors-transform.txt`。在 CLI 中将`authors-transform.txt`复制到一个新目录`C:/repo/temp`中，并将 cd 复制到该目录。

```
cd c:/repo/temp

git svn clone [SVN repo URL] --no-metadata --authors-file=authors-transform.txt --stdlayout your_project
```

接下来，如果您正在使用一个文件的话，您将想要寻址到`svn:ignore`文件。您可以运行下面的命令将`svn:ignore`转换成一个`.gitignore`文件，并提交到您的新 Git 存储库中。

您现在应该在 GitKraken 客户端的 WIP 节点中看到`.gitignore`。继续将新的`.gitignore`提交到您的存储库中。

接下来，您需要将所有的 SVN 标签转换成合适的 Git 标签。为此，您可以运行以下命令:

```
cd c:/repo/temp/your-project

git svn show-ignore > .gitignore
```

你可以使用 GitKraken 客户端来检查你的图表，并确保所有的标签都正确显示。

接下来，您需要为每个远程 ref 创建本地分支。您可以使用以下命令来完成此操作:

同样，您可以使用 GitKraken 客户端查看您的所有分支，并清理您不再需要的任何分支。如果您仍然看到一个“主干”分支，请验证它是否指向与“主”相同的提交。在这篇 [GitKraken 客户端提示文章](/blog/gitkraken-git-tips-7)中，了解如何设置自定义默认分支名称——如“main”而不是“master”。

如果一切正常，您可以继续删除那个分支，因为您现在将使用“主”分支。右键单击左侧面板中的分支名称，然后选择“删除主干”。

```
for t in $(git for-each-ref --format='%(refname:short)' refs/remotes/tags); do git tag ${t/tags\//} $t && git branch -D -r $t; done
```

你可以使用 GitKraken 客户端来检查你的图表，并确保所有的标签都正确显示。

接下来，您需要为每个远程 ref 创建本地分支。您可以使用以下命令来完成此操作:

```
for b in $(git for-each-ref --format='%(refname:short)' refs/remotes); do git branch $b refs/remotes/$b && git branch -D -r $b; done
```

此时，您的本地回购应该已经准备就绪；因此，您可以创建您的遥控器，并将回购推送到您的本地回购。

设置您的新 Git 遥控器

在本例中，我们将使用 [GitHub](https://www.gitkraken.com/integrations/github) 向您展示如何创建远程，但 GitKraken 还集成了托管和自托管版本的 [GitLab](https://www.gitkraken.com/integrations/gitlab) 、 [Bitbucket](https://www.gitkraken.com/integrations/bitbucket) 和 [Azure DevOps](https://www.gitkraken.com/integrations/azure-devops) ，使从任何这些服务添加远程回购变得快速而简单。好的，现在回到例子…

取消选中`Clone after init`选项，仅创建远程存储库。

此时，您的本地回购应该已经准备就绪；因此，您可以创建您的遥控器，并将回购推送到您的本地回购。

## 您将需要新的远程回购的 URL，因此单击`View on GitHub.com`按钮。

在本例中，我们将使用 [GitHub](https://www.gitkraken.com/integrations/github) 向您展示如何创建远程，但 GitKraken 还集成了托管和自托管版本的 [GitLab](https://www.gitkraken.com/integrations/gitlab) 、 [Bitbucket](https://www.gitkraken.com/integrations/bitbucket) 和 [Azure DevOps](https://www.gitkraken.com/integrations/azure-devops) ，使从任何这些服务添加远程回购变得快速而简单。好的，现在回到例子…

将 URL 复制到 repo，并在 GitKraken 客户端中返回到您的本地 repo。

点击左侧面板远程部分的`+`图标。

您将需要新的远程回购的 URL，因此单击`View on GitHub.com`按钮。

在 URL 选项卡上，命名您的新遥控器，将 repo 的 URL 粘贴到 Pull 和 Push URL 字段中，然后单击`Add Remote`。

将 URL 复制到 repo，并在 GitKraken 客户端中返回到您的本地 repo。

点击左侧面板远程部分的`+`图标。

现在，您可以右键单击每个分支和标签，将它们推送到您的遥控器上。

如果您有大量的分支或标签，那么您可以使用下面的命令将它们推送到您的遥控器。

恭喜🎉您到 Git 的迁移已经完成！

您现在应该有一个正常工作的 Git repo 了。如果您准备好帮助您的组织扩展 Git，请查看我们的 [GitKraken 客户端资源](https://www.gitkraken.com/git-client/resources)，包括迁移技巧和入门材料。

数百万开发者已经依靠 GitKraken Client 成功地过渡和扩展了 Git，那么为什么不加入北海巨妖的大家庭，免费试用今天最流行的 Git 工具呢？！

```
git push origin --all
git push origin --tags
```

## 恭喜🎉您到 Git 的迁移已经完成！

您现在应该有一个正常工作的 Git repo 了。如果您准备好帮助您的组织扩展 Git，请查看我们的 [GitKraken 客户端资源](https://www.gitkraken.com/git-client/resources)，包括迁移技巧和入门材料。

数百万开发者已经依靠 GitKraken Client 成功地过渡和扩展了 Git，那么为什么不加入北海巨妖的大家庭，免费试用今天最流行的 Git 工具呢？！