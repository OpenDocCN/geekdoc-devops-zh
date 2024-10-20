# 针对吉拉 4.0 版数据中心和服务器的 Git 集成|新的 Git 索引

> 原文：<https://www.gitkraken.com/blog/git-integration-for-jira-v4-data-center-server>

## **吉拉数据中心和吉拉服务器**

压缩到吉拉 4.0 版的 Git 集成！我们的团队已经开发了一些快速的新功能和改进，以增强您对吉拉数据中心和 T2 吉拉服务器的 Git 集成体验。

通过针对吉拉的 Git 集成，在吉拉直接利用强大的新 Git 索引功能。今天就开始免费使用吧！

[Start Free Trial](https://marketplace.atlassian.com/apps/4984/git-integration-for-jira?tab=overview&hosting=cloud)

## **新的 Git 索引特性**

系好安全带。索引队列现在存储在吉拉数据库中，实现了许多新的改进。预计 Git 索引状态的重新索引时间和一致性/准确性会有显著改善。

### **吉拉数据中心索引**

作为新 Git 索引功能的一部分，数据中心客户可以获得以下优势:

*   索引队列现在在所有吉拉数据中心节点之间共享。
*   每个节点将运行单独的重新索引作业；节点越多，索引越快！
*   集成/存储库的 Git 索引状态在所有节点之间共享，并且对管理员可见，不管您连接到哪个节点。

**跳过**

### 跳过将所有索引队列入口点组合在一起，并对它们进行去重复。如果存储库已排队，那么重新索引请求如何到达队列并不重要——手动重新索引、预定重新索引、webhook 重新索引、REST API 请求重新索引——队列中只会存储一个存储库条目。

跳过重复的 webhook 索引请求现在已经内置到索引引擎中，不需要额外的操作！如果索引队列已经包含要重新索引的存储库，后续的 webhook 索引请求将被跳过。

管理大量带有 webhooks 的存储库的吉拉管理员将会发现重建索引的时间有很大的改善，因为队列不会被重复的重建索引请求填满。

管理大量带有 webhooks 的存储库的吉拉管理员将会发现重建索引的时间有很大的改善，因为队列不会被重复的重建索引请求填满。

**为 Git Webhooks 休眠**

### 我们为 Git webhooks 提供了一个新的睡眠设置，以防止过于频繁地添加存储库。

要设置睡眠值，首先点击 Jira UI 右上角的 gear ⚙️图标(下面截图中的#1)，然后点击`Applications` (#2)。访问`Webhooks`设置选项卡(#3)并展开`Advanced`部分(#4)。`Min repository reindex interval`值(#5)是索引引擎在 webhook 索引请求触发重复的特定存储库的重新索引之前必须等待的时间。别忘了打`Save` (#6)！

要设置睡眠值，首先点击 Jira UI 右上角的 gear ⚙️图标(下面截图中的#1)，然后点击`Applications` (#2)。访问`Webhooks`设置选项卡(#3)并展开`Advanced`部分(#4)。`Min repository reindex interval`值(#5)是索引引擎在 webhook 索引请求触发重复的特定存储库的重新索引之前必须等待的时间。别忘了打`Save` (#6)！

这给了您的吉拉管理员更多的控制，特别是在有大量繁忙的存储库的吉拉实例上，他们可能会管理速率限制问题。

有关更多信息，请参见[吉拉 Git 集成文档](https://bigbrassband.com/documentation.html)。

有关更多信息，请参见[吉拉 Git 集成文档](https://bigbrassband.com/documentation.html)。

**Git 索引状态**

### 没有必要怀疑一个 [Git 库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)是否已经在吉拉被索引了！在 4.0 版中，集成和存储库索引状态比以前的版本更加准确。

要查看集成和存储库的当前索引状态，请导航至`Administration`(下面截图中的# 1)→`Applications`(# 2)→`Git repositories`(# 3)。可用的 Git 索引状态包括:

`Not indexed`:全新知识库的状态。

*   `Updated`:储存库是最新的(#4)。
*   `Fetching` : App 正在从 Git 服务器获取更改。
*   `Queued`:存储库在索引队列中，等待按优先级顺序编制索引/重新编制索引。
*   `Indexing`:存储库当前正在被(重新)索引。
*   `Error`:应用程序在试图(重新)索引此储存库(#5)时遇到错误。
*   `Error`:应用程序在试图(重新)索引此储存库(#5)时遇到错误。

**优先队列**

### 在 v4.0 中，我们向存储库队列引入了顺序。以下是完成请求的优先级:

吉拉管理员手动重新索引和 REST API 重新索引请求

1.  Webhook 索引触发器
2.  计划的作业
3.  具有相同优先级的排队存储库按先进先出(FIFO)顺序处理。

**针对吉拉改进的 Git 集成**

当你热爱某样东西时，你会改进它。❤️这是为 4.0 版调整的内容

## **针对吉拉改进的 Git 集成**

**更快地在吉拉导航提交**

对于那些有无数提交的吉拉问题，`Git Commits` issue 选项卡现在被分页，以便更容易、更快速地导航。

### **更快地在吉拉导航提交**

对于那些有无数提交的吉拉问题，`Git Commits` issue 选项卡现在被分页，以便更容易、更快速地导航。

从`General`设置，管理员现在可以包括`${basebranch}`分支名称模板，用于从吉拉发行创建 Git 分支。

从`General`设置，管理员现在可以包括`${basebranch}`分支名称模板，用于从吉拉发行创建 Git 分支。

**按状态分类的拉取请求**

从吉拉问题开始，[拉动式请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)现在将按状态排序，以便更好地识别未完成的拉动式请求。

### **按状态分类的拉取请求**

从吉拉问题开始，[拉动式请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)现在将按状态排序，以便更好地识别未完成的拉动式请求。

还有几件事。首先，所有凭证现在都在吉拉数据库中加密，我们已经将`Git`下拉菜单中的“查看所有存储库”重命名为“存储库浏览器”。

还有几件事。首先，所有凭证现在都在吉拉数据库中加密，我们已经将`Git`下拉菜单中的“查看所有存储库”重命名为“存储库浏览器”。

这涵盖了吉拉 4.0 版的 Git 集成的新特性！要查看改进和错误修复的完整列表，请查看完整的[吉拉 Git 集成发布说明](https://marketplace.atlassian.com/apps/4984/git-integration-for-jira/version-history)。

如果您还没有开始使用吉拉的 Git 集成，现在就开始免费试用吧！

如果您还没有开始使用吉拉的 Git 集成，现在就开始免费试用吧！

[Start Free Trial](https://marketplace.atlassian.com/apps/4984/git-integration-for-jira?tab=overview&hosting=cloud)