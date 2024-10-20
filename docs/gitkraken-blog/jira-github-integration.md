# 吉拉 GitHub 集成| GitHub+吉拉使用技巧

> 原文：<https://www.gitkraken.com/blog/jira-github-integration>

您可以通过针对吉拉的的 [Git 集成来创建吉拉 GitHub 集成。这个工具将允许你使用智能提交，创建分支，直接从吉拉的 GitHub 库上拉请求，用 webhooks 索引库，等等！](https://marketplace.atlassian.com/plugins/com.xiplink.jira.git.jira_git_plugin/cloud/overview?utm_source=gitkraken.com&utm_medium=blog&utm_campaign=learn%20jira&utm_content=jira%20github%20integration&utm_term=)

由 Atlassian 于 2002 年开发的[吉拉](https://www.atlassian.com/software/jira/guides/use-cases/who-uses-jira#companies-and-teams-that-use-jira)是一个项目管理工具，旨在帮助软件开发团队有效地跟踪他们的项目。凭借专为[敏捷开发](https://www.atlassian.com/agile)设计的特性，如 scrum、看板、问题跟踪等等，吉拉已经成为许多开发团队技术堆栈中的流行工具。

GitHub 是一个被广泛使用的存储库托管服务，允许开发者在项目上合作。通过在 GitHub 上托管一个[远程存储库](https://www.gitkraken.com/learn/git/git-remote)，团队成员可以在本地对存储库进行更改，将他们的更改推送到 GitHub 上的远程存储库，以及拉取他们的队友添加的更改。

因为同时使用吉拉和 GitHub 可以增加项目的可见性，使团队更容易协同工作，并极大地提高生产力，许多人选择在他们的工作流程中利用这两种工具。在本文中，我们将介绍如何使用 Git Integration for 吉拉来结合吉拉和 GitHub 的力量，并将您的工作流程提升到一个新的水平。

### 创建吉拉 GitHub 集成

[连接吉拉+ GitHub](https://www.gitkraken.com/blog/jira-github-integration#connect-jira-gihub)

[吉拉 GitHub 智能提交](https://www.gitkraken.com/blog/jira-github-integration#jira-github-smart-commits)

[吉拉 GitHub 创建分支](https://www.gitkraken.com/blog/jira-github-integration#jira-github-create-branch)

[吉拉 GitHub 集成:拉取请求](https://www.gitkraken.com/blog/jira-github-integration#jira-github-pull-request)

[吉拉 GitHub Webhooks](https://www.gitkraken.com/blog/jira-github-integration#jira-github-webhooks)

[GitHub 吉拉集成:今天就开始](https://www.gitkraken.com/blog/jira-github-integration#get-started-github-jira-integration)

连接吉拉+ GitHub

## 要开始您的吉拉 GitHub 集成，您需要从 Atlassian market place[安装吉拉 Git 集成](https://marketplace.atlassian.com/apps/4984/git-integration-for-jira?hosting=cloud&tab=overview)。如果你以前没有使用过吉拉的 Git 集成，你可以选择开始 30 天的免费试用。一旦安装了 Git Integration for 吉拉，就可以开始将 GitHub 库连接到吉拉实例。要开始连接您的 GitHub 库，您需要使用以下步骤设置 GitHub 集成:

从您的吉拉实例中，选择顶部导航栏中的`Apps`

1.  从随后的下拉菜单中，选择`Git Integration: Manage integrations`
2.  从页面中央选择`Add integration`按钮
3.  选择`Git service integration (Recommended)`
4.  根据您喜欢的连接方式，选择`GitHub.com - OAth`或`GitHub.com - Personal Access Token`
5.  在`Connect Git service`标题下方，选择蓝色的`Connect GitHub.com`按钮
6.  输入您的 GitHub 帐户信息，然后选择`Sign in`
7.  输入您的 GitHub 帐户信息，然后选择`Sign in`

您的吉拉 GitHub 集成现在应该完成了。现在，您可以通过选择每个 repo 名称旁边的复选框来连接您的 GitHub 存储库。如果你想把你所有的 GitHub 库连接到吉拉，只需选择你的 GitHub 用户名左边的复选框。

您的吉拉 GitHub 集成现在应该完成了。现在，您可以通过选择每个 repo 名称旁边的复选框来连接您的 GitHub 存储库。如果你想把你所有的 GitHub 库连接到吉拉，只需选择你的 GitHub 用户名左边的复选框。

将 GitHub 库连接到吉拉 Git 集成是安全、快速和直观的！您可以通过一个集成连接一个或多个存储库。

吉拉 GitHub 智能提交

[Start Free Trial of Git Integration for Jira](https://marketplace.atlassian.com/plugins/com.xiplink.jira.git.jira_git_plugin/cloud/overview?utm_source=gitkraken.com&utm_medium=blog&utm_campaign=learn%20jira&utm_content=jira%20github%20integration&utm_term=)

通过利用智能提交，您可以进一步集成吉拉和 GitHub repos。智能提交允许您使用提交消息中的特定语法对吉拉问题执行各种操作。这种针对吉拉的 Git 集成特性有助于开发人员避免在他们的开发工具和吉拉之间进行上下文切换，并为他们提供更多的时间来编写代码。使用智能提交可以对吉拉问题采取的一些操作包括:

## 添加评论

记录时间跟踪

*   转换到特定的工作流状态
*   将问题分配给团队成员
*   还有更多！
*   只要 Git 配置中的电子邮件与用于吉拉配置文件的电子邮件相匹配，就可以开始对任何连接的存储库使用智能提交。
*   要创建智能提交，请以关联的吉拉问题密钥开始您的提交消息，后跟一个`#`，然后是您要对该问题采取的操作的命令。这里有几个智能提交的例子，您可以立即开始利用:

**添加评论**

`ISSUE_KEY #comment`

要创建智能提交，请以关联的吉拉问题密钥开始您的提交消息，后跟一个`#`，然后是您要对该问题采取的操作的命令。这里有几个智能提交的例子，您可以立即开始利用:

**记录时间跟踪信息**

`ISSUE_KEY #time [Some amount in Jira time syntax]`

*吉拉管理员必须启用此功能，此操作才能生效

**记录时间跟踪信息**

**过渡问题**

`ISSUE_KEY #`

*要使此操作生效，您必须拥有必要的吉拉权限来转换问题

**过渡问题**

访问 [Git Integration for 吉拉的官方文档](https://bigbrassband.com/git-integration-for-jira/documentation/smart-commits.html#bbb-nav-smart-commits),了解智能提交可用的更多高级操作。

*要使此操作生效，您必须拥有必要的吉拉权限来转换问题

访问 [Git Integration for 吉拉的官方文档](https://bigbrassband.com/git-integration-for-jira/documentation/smart-commits.html#bbb-nav-smart-commits),了解智能提交可用的更多高级操作。

您可以从问题的`Details`面板查看与吉拉问题相关的所有 Git 提交的列表。您可以在 GitHub 中直接从问题中打开任何相关的提交，这使得快速评估项目状态和审查代码变得容易。

吉拉 GitHub Create 分公司

吉拉 GitHub 集成还允许您创建与吉拉问题相关的分支。要从吉拉发行创建分支，请执行以下操作:

选择一个吉拉问题

点击问题右上角的`Show Git development panel`按钮

从`Git Development`面板中选择`Create branch`按钮

## 从`Repository`下拉菜单中，选择想要创建分支的 GitHub 存储库

从`Source branch`下拉菜单中选择一个分支

*   对`Branch name`字段进行任何所需的更改。您会注意到，吉拉 GitHub 集成会根据问题密钥和摘要自动填充一个分支名称。
*   选择右下角的蓝色`Create branch`按钮
*   从`Git Development`面板中选择`Create branch`按钮

*   从`Source branch`下拉菜单中选择一个分支
*   这将在您的 GitHub 存储库中创建一个新的分支，与您从中创建分支的吉拉问题相关联。要从本地项目中处理新创建的分支，只需执行一个`Git pull`。与智能提交类似，您可以在该问题的`Details`面板上看到与吉拉问题相关的分支列表。
*   选择右下角的蓝色`Create branch`按钮

吉拉 GitHub 集成:拉请求

使用吉拉 GitHub 集成，您还可以直接从吉拉创建拉请求。按照以下步骤从吉拉问题创建拉式请求:

选择一个吉拉问题

点击问题右上角的`Show Git development panel`按钮

从`Git Development`面板中选择`Create pull request`按钮

## 从`Repository`下拉菜单中，选择想要启动拉请求的 GitHub 存储库

从`Source branch`下拉菜单中选择您想要合并的分支

1.  从`Target branch`下拉菜单中选择您想要合并的分支
2.  对拉动请求`Title`字段进行所需的调整。吉拉的 Git Integration 自动使用以下格式填充该字段`issue key merge to`
3.  选择右下角的蓝色`Create pull request`按钮
4.  从`Repository`下拉菜单中，选择想要启动拉请求的 GitHub 存储库

6.  从`Target branch`下拉菜单中选择您想要合并的分支
7.  **恭喜恭喜！**您现在已经直接从吉拉创建了一个拉式请求。您可以通过转到相关的吉拉问题并导航到右侧的`Details`面板来查看您的拉动式请求。在这里，您可以看到有多少个拉取请求与问题相关联，以及每个拉取请求的状态。
8.  吉拉 GitHub Webhooks

如果你使用的是吉拉云或者吉拉服务器，你也可以创建 [webhooks](https://developer.atlassian.com/server/jira/platform/webhooks/) 或者索引触发器，从远程快速重新索引你的存储库。您可以配置您的 webhooks，以便在执行某些操作(比如 Git commit)时重新索引您连接的存储库。要将 webhooks 添加到已连接的 GitHub 存储库中，请完成以下步骤:

吉拉云的吉拉 GitHub Webhooks

在您的吉拉仪表板上，从顶部工具栏中选择`Apps`

从下拉菜单中选择`Git Integration: Manage integrations`

从左侧栏选择`Indexing triggers`

## 将`indexing triggers enabled`标题下的选项切换到`enabled`

复制`Webhook URL`

导航到 GitHub 并选择您想要配置 webhooks 的存储库

### 从顶部工具栏选择`settings`

1.  从左侧栏中，选择`Webhooks`
2.  选择右上角的`Add webhook`按钮
3.  将你从吉拉复制的 Webhook URL 粘贴到`Payload URL`字段
4.  从`Content type`下拉菜单中，选择`application/json`
5.  导航回吉拉并复制`Secret key`
6.  返回 GitHub，将密钥粘贴到`Secret`字段中
7.  选择您想要触发 webhook 的事件，如提交、新分支等。
8.  选择绿色的`Add webhook`按钮
9.  选择右上角的`Add webhook`按钮
10.  jira github 服务器 web 手册
11.  在您的吉拉仪表板上，从顶部工具栏中选择`Git`
12.  从下拉菜单中选择`Manage repositories`
13.  从左侧栏选择`Webhooks`
14.  为 webhooks 选择`enabled`选项
15.  复制`Webhook URL`

导航到 GitHub 并选择您想要配置 webhooks 的存储库

### 从顶部工具栏选择`settings`

1.  从左侧栏中，选择`Webhooks`
2.  选择右上角的`Add webhook`按钮
3.  将你从吉拉复制的 Webhook URL 粘贴到`Payload URL`字段
4.  从`Content type`下拉菜单中，选择`application/json`
5.  导航回吉拉并复制`Secret key`
6.  返回 GitHub，将密钥粘贴到`Secret`字段中
7.  选择您想要触发 webhook 的事件，如提交、新分支等。
8.  选择绿色的`Add webhook`按钮
9.  选择右上角的`Add webhook`按钮
10.  通过将吉拉和 GitHub 与针对吉拉的 Git 集成进行集成，释放新的生产力水平，节省自己的时间，并增加团队的可见性。
11.  从`Content type`下拉菜单中，选择`application/json`
12.  GitHub 吉拉集成–立即开始
13.  连接吉拉和 GitHub 可以将生产力和效率提升到一个新的水平。吉拉 GitHub 集成，通过吉拉 Git 集成成为可能，允许你用你的 GitHub 库轻松驱动吉拉。您可以查看与吉拉问题相关的提交和分支的完整列表，直接从相关问题打开提交和分支，消除重复任务，如复制提交消息信息等。
14.  选择您想要触发 webhook 的事件，如提交、新分支等。
15.  选择绿色的`Add webhook`按钮

通过将吉拉和 GitHub 与针对吉拉的 Git 集成进行集成，释放新的生产力水平，节省自己的时间，并增加团队的可见性。

[Start Free Trial of Git Integration for Jira](https://marketplace.atlassian.com/plugins/com.xiplink.jira.git.jira_git_plugin/cloud/overview?utm_source=gitkraken.com&utm_medium=blog&utm_campaign=learn%20jira&utm_content=jira%20github%20integration&utm_term=)

## GitHub 吉拉集成–立即开始

连接吉拉和 GitHub 可以将生产力和效率提升到一个新的水平。吉拉 GitHub 集成，通过吉拉 Git 集成成为可能，允许你用你的 GitHub 库轻松驱动吉拉。您可以查看与吉拉问题相关的提交和分支的完整列表，直接从相关问题打开提交和分支，消除重复任务，如复制提交消息信息等。