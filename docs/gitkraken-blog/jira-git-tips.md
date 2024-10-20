# 吉拉 Git 提示|吉拉 Git 集成

> 原文：<https://www.gitkraken.com/blog/jira-git-tips>

您可以使用针对吉拉的 Git 集成来集成 Git 和吉拉。吉拉 Git Integration 是一款允许用户使用 Git 驾驶吉拉的应用程序。这个吉拉 Git 集成工具允许你花更少的时间在吉拉和 Git 之间切换上下文，而花更多的时间编写和交付优秀的代码。

许多开发团队在他们的常规开发过程中使用 Git 和吉拉来帮助和跟踪项目的进展。Git 主要由团队的技术成员用来为项目贡献代码。另一方面，吉拉通常被技术和非技术用户用来查看和交流项目的状态。

在本文中，我们将介绍使用 Git 和吉拉的各种技巧，这些技巧可以帮助您增强团队对项目的可见性，节省时间，并提高生产率。

## 本文涵盖的吉拉 Git 技巧

**虽然本文关注的是如何将 Git 用于吉拉云，但本文中提到的许多特性也适用于吉拉服务器和吉拉数据中心。访问 Git Integration for 吉拉[官方文档](https://help.gitkraken.com/)，找到关于吉拉服务器和吉拉数据中心的资源。*

[https://www.youtube.com/embed/9gizWzapCeI?feature=oembed](https://www.youtube.com/embed/9gizWzapCeI?feature=oembed)

视频

## 如何设置吉拉 Git 集成

创建吉拉 Git 集成的第一步是[为吉拉](https://www.gitkraken.com/git-integration-for-jira/try-free)安装 Git 集成。如果您想在购买订阅之前预览该工具，可以选择开始 30 天的免费试用。

一旦安装了针对吉拉的 Git Integration，您就可以通过从您首选的 Git 托管服务连接您的远程存储库来将 Git 连接到吉拉。吉拉的 Git 集成[支持所有的 Git 服务器](https://www.gitkraken.com/git-integration-for-jira/features)，这意味着你可以集成 GitHub、GitLab、Bitbucket 等等！下面的例子详细说明了如何使用[吉拉 GitHub 集成](https://www.gitkraken.com/blog/jira-github-integration)从 GitHub 连接远程存储库。

1.  从吉拉的顶部工具栏中，选择`Apps`
2.  从出现的下拉菜单中选择`Git Integration: Manage integrations`
3.  点击屏幕中央附近的`Add integration`按钮
4.  选择`Git service integration (Recommended)`
5.  选择`GitHub.com - OAth`或`GitHub.com - Personal Access Token`
6.  点击`Connect GitHub.com`按钮
7.  输入您的 GitHub 帐户信息，然后选择`Sign in`

以上操作会将您带到一个页面，在该页面中，您可以通过简单地选中所需存储库名称旁边的复选框，将一个或多个存储库连接到 Git Integration for 吉拉。

就这么简单！您现在已经学习了如何将 Git 与吉拉集成，以及如何使用吉拉添加 Git 存储库。

## 从吉拉票证创建 Git 分支

针对吉拉的 Git 集成允许您直接从吉拉发行创建 [Git 分支](https://www.gitkraken.com/learn/git/branch)。你也可能听到人们把吉拉问题称为吉拉问题。虽然吉拉问题是官方术语，但票和问题可以互换使用，意思相同。从吉拉创建 Git 分支是一个过程，可以通过两种不同的方式启动，这取决于您的偏好。

您可以通过使用问题中心的`Git Development`面板正下方的`Create branch`按钮开始创建 Git 分支的过程。或者，您可以通过选择`Details`面板底部的`Open Git Integration`按钮，然后选择`Create branch`，开始从吉拉创建 Git 分支。无论您使用哪个过程来开始分支创建过程，下面的选项都是相同的，用于从吉拉完成 Git 分支的创建。

1.  从`Repository`下拉菜单中，选择您想要在其中创建分支的 [Git 库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)
2.  从`Source branch`下拉菜单中选择一个分支
3.  吉拉的 Git Integration 会在`Branch name`字段中自动填充一个建议的分支名称。对分支名称进行任何所需的调整
4.  点击 C `reate branch`按钮

您可以通过导航到`Details`面板，将鼠标悬停在`# branch`上，并选择随后出现的`Open branch`图标来确认 Git 分支已经成功创建。这将把您带到 Git 托管服务上最近创建的 Git 分支。为了在本地与新创建的分支进行交互，您需要执行一个 [Git fetch 或 Git pull](https://www.gitkraken.com/learn/git/problems/git-pull-vs-fetch) 。

## 禁用从吉拉创建 Git 分支的功能

一些吉拉管理员更喜欢禁用从吉拉创建 Git 分支的功能，以避免非技术用户可能遇到的麻烦。例如，想象一下，如果您团队中的非技术用户无意中创建了 10 个随机的 Git 分支，那么存储库会变得多么混乱。

虽然类似上面场景的错误肯定不是标准，但是如果您不希望用户能够从吉拉创建 Git 分支，您可以很容易地禁用这个特性。要禁用`Create branch`选项，遵循以下步骤:

1.  从顶部工具栏选择`Apps`
2.  从下拉菜单中选择`Git Integration: Manage integrations`
3.  从左侧面板中，选择`General settings`
4.  向下滚动到`Git Integration Options`标题，取消选择`Enable create/delete branch`
5.  滚动到`General settings`选项的底部，选择蓝色的`Update`按钮

从吉拉创建拉式请求

准备好提交您的作品供项目经理审阅了吗？您可以直接从吉拉机票发起[拉取请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)！

要创建吉拉拉动请求，您可以点击问题中心`Git Development panel`的`Create pull request`按钮，或者点击`Details`面板底部的`Open Git Integration`按钮。从那里，选择`Create pull request`。就像从吉拉创建 Git 分支的过程一样，您选择哪条路径来发起 pull 请求并不重要，因为后续步骤是相同的。

从`Repository`下拉菜单中选择您想要开始提取请求的存储库

从`Source branch`下拉菜单中，选择包含您想要合并的变更的分支

1.  从`Target branch`下拉菜单中，选择您想要将更改合并到的分支
2.  吉拉的 Git Integration 自动使用以下格式填充`Title`字段:将密钥合并<源分支>发布到<目标分支>。如有必要，对提取请求标题进行调整
3.  点击`Create pull request`按钮
4.  吉拉的 Git Integration 自动使用以下格式填充`Title`字段:将密钥合并<源分支>发布到<目标分支>。如有必要，对提取请求标题进行调整
5.  为了验证您已经从吉拉创建了一个拉取请求，导航到`Details`面板，将鼠标悬停在`# pull request`上，并选择随后出现的`Open pull request`图标。这将允许您在 Git 托管服务上查看 pull 请求。

针对吉拉的 Git 集成允许团队查看与吉拉问题相关的所有 pull 请求的列表，帮助他们保持有组织和按部就班。

禁用从吉拉创建拉式请求的选项

[Install Git Integration for Jira](https://www.gitkraken.com/git-integration-for-jira/try-free)

正如一些吉拉管理员可能禁用从吉拉创建 Git 分支的选项一样，他们也可能选择禁用从吉拉创建 pull 请求的选项，以保护存储库免受非技术团队成员的意外更改。通过几个简单的步骤，您可以禁用从吉拉创建拉式请求:

从顶部工具栏中，选择`Apps`

从下拉菜单中选择`Git Integration: Manage integrations`

1.  从左侧面板中选择`General settings`
2.  向下滚动到`Git Integration Options`标题，取消选择`Enable create pull/merge request`
3.  导航到`General settings`选项的底部，点击`Update`按钮
4.  如何建立一个吉拉网钩
5.  吉拉每隔 8 分钟就会自动对连接的 Git 库进行重新索引或检查更新。但是，您可以配置吉拉 webhooks，也称为索引触发器，以更频繁地更新吉拉。吉拉 webhooks 可以被设置为当某些 Git 动作发生时自动重新索引存储库，例如 [Git 提交](https://www.gitkraken.com/learn/git/commit)或 [Git 推送](https://www.gitkraken.com/learn/git/git-push)。要在吉拉云中配置吉拉 webhooks，请遵循以下步骤:

从吉拉的顶部工具栏中，选择`Apps`

## 从下拉菜单中选择`Git Integration: Manage integrations`

从左侧栏中选择`Indexing triggers`

确保通过切换`Indexing triggers settings`下方的按钮启用步进触发器

导航到您的 Git 托管服务并选择您想要添加索引触发器的存储库

从顶部工具栏中选择 repo `Settings`

1.  从左侧栏中选择`Webhooks`
2.  点击右上角的`Add webhook`按钮
3.  导航回吉拉
4.  复制吉拉网页挂钩的网址
5.  在你的 Git 托管服务上，将吉拉的 webhook URL 粘贴到`Payload URL field`
6.  点击`Content type`，从下拉菜单中选择`application/json`
7.  回到吉拉，复制标题下的密钥
8.  导航到您的 Git 托管服务，并将密钥粘贴到`Secret`字段中
9.  选择您想要触发吉拉 webhook 的事件，如提交、推送、拉取请求等。
10.  点击`Add webhook`按钮
11.  在你的 Git 托管服务上，将吉拉的 webhook URL 粘贴到`Payload URL field`
12.  现在，当您选择的事件(如 Git 提交)发生时，您的存储库将自动重新索引，这要感谢您的吉拉 webhooks。
13.  回到吉拉，复制标题下的密钥
14.  通过 Git 集成为吉拉节省时间
15.  在本文中，我们展示了如何使用吉拉 Git 集成创建吉拉 Git 集成。每次你从吉拉创建一个 Git 分支，从吉拉创建一个 pull 请求，或者创建一个吉拉 webhook，你都在节省宝贵的时间。虽然这可能不会对一天产生巨大的影响，但是这些节省下来的时间会变成几个小时，在你意识到之前，你已经大大提高了你的整体效率。通过[为吉拉](https://www.gitkraken.com/git-integration-for-jira)安装 Git 集成，从今天开始节省时间。
16.  点击`Add webhook`按钮

现在，当您选择的事件(如 Git 提交)发生时，您的存储库将自动重新索引，这要感谢您的吉拉 webhooks。

## 通过 Git 集成为吉拉节省时间

在本文中，我们展示了如何使用吉拉 Git 集成创建吉拉 Git 集成。每次你从吉拉创建一个 Git 分支，从吉拉创建一个 pull 请求，或者创建一个吉拉 webhook，你都在节省宝贵的时间。虽然这可能不会对一天产生巨大的影响，但是这些节省下来的时间会变成几个小时，在你意识到之前，你已经大大提高了你的整体效率。通过[为吉拉](https://www.gitkraken.com/git-integration-for-jira)安装 Git 集成，从今天开始节省时间。