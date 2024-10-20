# 在节点中执行 git flow:Node git-flow

> 原文：<https://www.gitkraken.com/blog/nodegit-flow>

在 GitKraken V0.5 中，我们增加了对 GitFlow 的支持。在这个过程中，我们创建了一个包装 NodeGit 的库，它允许我们通过一个函数调用来执行 GitFlow 命令。该库现在是开源的，可以节省其他开发人员在 Node.js 中进行 GitFlow 操作的时间。

## **什么是 GitFlow？**

GitFlow 是一种分支和合并策略，可以在任何 Git repo 中使用。它是一个规则列表，用于组织回购的历史，也用于简化发布过程、错误修复和功能创建。

在使用 GitFlow 的 repo 中，将始终存在两个分支:`master`和`develop`:

*   `master`:生产中的版本
*   当前正在为下一个版本开发的版本

其他支持分支是特定的类型(即功能、修补程序或发布分支)，其中每个分支都有自己的分支和合并规则。

正如你所想象的，这些规则执行起来可能会很乏味，违反规则的人迫在眉睫。幸运的是， [GitFlow 方法](http://nvie.com/posts/a-successful-git-branching-model/)的创建者意识到了这些问题，并创建了一个 Git 扩展来自动化启动和完成 GitFlow 分支的过程。Axosoft 的开发人员只是实现了同样的想法；区别在于我们扩展了 NodeGit 而不是 Git core。

**如何使用**

## **[Nodegit-flow](https://github.com/smith-kyle/nodegit-flow) 从初始化开始。**

`nodegit.Flow.init`会将以下密钥添加到回购的 Git 配置中:

```
const nodegit = require('nodegit-flow');
const config = nodegit.Flow.getConfigDefault();
nodegit.Flow.init(repo, config);
```

`gitflow.branch.master`

*   `gitflow.branch.develop`
*   `gitflow.prefix.feature`
*   `gitflow.prefix.hotfix`
*   `gitflow.prefix.release`
*   `gitflow.prefix.versionTag`
*   `gitflow.prefix.versionTag`

这些键定义了 GitFlow 分支在启动某个功能、修补程序或版本时将拥有的前缀。他们还将确定 nodegit-flow 将与`master`和`develop`关联的本地分支名称。

您可以选择坚持使用`.getConfigDefault()`提供的默认配置值，也可以修改这些值来满足您的偏好。

下面是您的回购在初始化后的样子。

现在让我们开始一个专题。

`nodegit.Flow.startFeature(repo, 'myFeature');`

我们刚刚创建并检查了一个名为`feature/myFeature`的分支，它指向与`develop`相同的提交。现在，在用 vanilla NodeGit 创建了一个 commit 之后，我们的图表将看起来像这样:

一旦该功能完成，只需一行代码即可完成:

`nodegit.Flow.finishFeature(repo, 'myFeature');`

这已经将`feature/myFeature`合并到开发中，检出了`develop`，并删除了`feature/myFeature`。

Nodegit-flow 不仅包含特性的方法，还包含补丁、版本和所有其他 git-flow 的东西。

通过扩展 NodeGit 以在单独的库中包含 GitFlow 函数，我们让 GitKraken 很好地与 GitFlow 问题分离，并为对扩展 NodeGit 感兴趣的开发人员提供一个小示例。

虽然 GitKraken 不是开源的，但我们定期贡献和发布我们内部使用的工具，如 nodegit-flow。其他示例包括 NodeGit、libgit2 和 Electron。

While GitKraken is not open source, we regularly contribute to and publish tools that we use internally such as nodegit-flow. Other examples include NodeGit, libgit2, and Electron.