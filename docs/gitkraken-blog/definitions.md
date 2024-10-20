# Git 定义-术语|了解 Git

> 原文：<https://www.gitkraken.com/learn/git/definitions>

Git add 可用于将修改后的文件存放到 Git 索引中，为提交做准备。Git add 可用于一次性存放单个、多个或所有修改过的文件。[了解更多信息](https://www.gitkraken.com/learn/git/git-add)

* * *

分支是指向特定提交的指针。分支指针随着您所做的每个新提交而移动，并且仅当在公共祖先提交上进行提交时才在图中分叉。[了解更多信息](https://www.gitkraken.com/learn/git/branch)

* * *

在 Git 中，checkout 命令告诉 Git 您希望您的更改应用于哪个分支或提交。

检出一个分支将会更新您的回购文件，以匹配该分支指向的任何提交的快照。[了解更多信息](https://www.gitkraken.com/learn/git/git-checkout)

* * *

cherry pick 命令从目标提交中获取更改，并将它们放在当前签出的分支的开头。[了解更多信息](https://www.gitkraken.com/learn/git/cherry-pick)

* * *

Git clone 命令用于将现有的 Git 存储库复制或克隆到本地目录中。Git 中的克隆将为克隆的存储库创建一个新的本地目录，复制指定的远程存储库的所有内容，并在新的本地存储库中签出一个初始分支。

默认情况下，Git clone 命令将创建一个对名为“origin”的远程存储库的引用，并创建远程跟踪分支，这些都可以通过运行“git branch -a”看到。[了解更多信息](https://www.gitkraken.com/learn/git/git-clone)

* * *

提交代表了您的存储库在特定时间点的快照。在 Git 中执行 commit 是一种将本地文件更改在工作目录中暂存后应用到 Git 存储库的方法。[了解更多信息](https://www.gitkraken.com/learn/git/commit)

* * *

Git config 允许用户定制 Git 的工作方式，并优化 Git 以适应他们的工作流程。Git 从缩小的文件列表中提取配置设置，从系统→全局→本地。每个配置级别都可以覆盖其上一级的配置设置。[了解更多信息](https://www.gitkraken.com/learn/git/git-config)

* * *

Git fetch 是一种下载提交的机制，这些提交存在于远程存储库中，但不存在于本地远程跟踪分支中。Git fetch 还更新 FETCH_HEAD 文件，Git 使用该文件在执行 Git fetch 后协调合并。[了解更多信息](https://www.gitkraken.com/learn/git/git-fetch)

* * *

Git merge 命令允许您将一个分支中的更改合并到任何其他分支中。[了解更多信息](https://www.gitkraken.com/learn/git/git-merge)

* * *

Git Pull 将从远程存储库中获取变更，并将它们合并到您当前签出的本地分支中。使用这个命令时，Git 将首先运行 Git fetch，然后运行 Git merge。[了解更多信息](https://www.gitkraken.com/learn/git/tutorials/what-is-git-pull)

* * *

Git push 用于将本地存储库的内容上传到远程存储库。

此外，Git push 还可以用来删除远程存储库。

* * *

Git rebase 命令从源分支获取一个或一组提交，并将它们应用到目标分支之上。[了解更多信息](https://www.gitkraken.com/learn/git/git-rebase)

* * *

Git Reset 移动 HEAD 和指针，该指针命名并跟踪分支的最后一次提交。使用–mixed 或–soft 选项，Git reset 将保留 HEAD 和指定提交之间所做的所有更改，或者使用–hard 选项，Git reset 将放弃所有更改。[了解更多信息](https://www.gitkraken.com/learn/git/git-reset)

* * *

Git stash 命令将获取当前工作目录的状态，并将其保存在一个未完成的更改堆栈中，可以随时重新应用。这将通过一个干净的工作目录恢复到头提交。只能隐藏被跟踪的文件。[了解更多信息](https://www.gitkraken.com/learn/git/git-stash)