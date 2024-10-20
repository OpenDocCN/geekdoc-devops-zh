# 吉拉 GitLab 集成| git lab+吉拉使用技巧

> 原文：<https://www.gitkraken.com/blog/jira-gitlab-integration>

集成吉拉和 GitLab 是软件团队简化工作流程和提高团队协作的最佳方式之一。GitLab 和吉拉是追踪项目和开发工作最流行的工具。GitLab 主要是一个托管服务，允许开发团队存储他们的代码。GitLab 于 2011 年首次提交，2021 年注册用户已超过 3000 万。吉拉于 2002 年推出，是使用最多的项目管理工具之一，拥有超过 190，000 名客户。

虽然工程师和开发人员通常在 GitLab 上花费更多的时间，但较少的技术利益相关者经常依赖吉拉来更新状态。在本文中，我们将向您展示如何设置吉拉 GitLab 集成，以便吉拉用户可以看到提交、拉请求、分支等等。通过将 GitLab 和吉拉集成在一起，您将能够更有效地管理您的项目，并为关键利益相关方提供更大的项目可见性。

其中的一个关键部分是理解如何将问题/任务/bug 链接到您的 Git 存储库中的源代码。这种联系对于确保代码变更相对于分配给开发人员的任务的可追溯性和透明性是至关重要的。

集成 GitLab 和吉拉最简单的方法之一是用 [Git 集成吉拉](https://marketplace.atlassian.com/plugins/com.xiplink.jira.git.jira_git_plugin/cloud/overview?utm_source=gitkraken.com&utm_medium=blog&utm_campaign=learn%20jira&utm_content=jira%20github%20integration&utm_term=)。这个顶级的 Atlassian marketplace 应用程序由 GitKraken 构建，可以部署在吉拉云、服务器和数据中心上。它还支持在云中(GitLab.com)或在您自己的基础设施上(GitLab 自我管理)与 GitLab 集成。

## **将 GitLab 与吉拉整合**

## **连接 GitLab 和吉拉**

我们建议专门为集成创建一个 GitLab 用户。通过这种方式，GitLab 用户可以定义执行给定任务的权限。确保该用户有权访问您希望与吉拉集成的存储库。集成的第一步是创建一个个人访问令牌，用于在创建 GitLab 吉拉集成时进行身份验证。

**在 GitLab 中创建个人访问令牌的步骤**

1.  登录 GitLab(例如[GitLab.com](https://gitlab.com/users/sign_in)
2.  在右上角选择您的用户资料，然后选择`Preferences`
3.  在左侧，选择`Access Tokens`
4.  输入令牌名称以及您希望令牌过期的时间(将此时间设置为未来足够长的时间，以确保集成不会中断)
5.  在选择范围下，选择`api`、`read_api`、`read_repository`和`write_repository`
6.  选择`Create personal access token`
7.  该页面将更新–将值复制到您的新个人访问令牌下

要继续您的 GitLab 吉拉集成，您需要从 Atlassian marketplace 安装用于吉拉的 Git 集成。像所有的 Atlassian marketplace 应用程序一样，你需要一个吉拉管理员来安装应用程序。一旦安装了 Git Integration for 吉拉，就可以开始将 GitLab 存储库与相关的吉拉项目连接起来。

1.  在吉拉的顶部导航栏中，选择`Apps`，然后选择`Git Integration: Manage integrations`
2.  选择右上方的`Add Integration`按钮
3.  从选择 Git 托管服务页面，选择`GitLab.com`用于云，或者选择`GitLab Server`用于自托管
4.  输入您之前创建的个人访问令牌，然后选择`Connect and select repositories`按钮
5.  选择您希望与吉拉集成的 GitLab 库，然后单击`Connect repositories button`

在这一点上，该应用程序将索引您的存储库，并使它们可以在吉拉查看。

## **将 GitLab 与吉拉问题联系起来**

要在 Git 提交和吉拉问题之间建立联系，开发人员必须在其提交消息中包含吉拉问题密钥。

Git 提交消息示例:`GIT-4322 – Updated the plugin …`

在这种情况下，`GIT-4322`是将提交消息链接到吉拉问题的吉拉问题密钥。只有当主分支没有提交时，才包含属于非主分支的提交。

作为处理子任务时的最佳实践，将父任务和子任务吉拉发布键放在提交消息中，以便提交在两个地方都显示。这样，子任务的提交就不会在父问题的多次提交中丢失。

## **在吉拉创建 GitLab 分公司**

在您的吉拉云实例上，打开一个吉拉问题。在吉拉 Git 集成开发面板上，点击`Open Git integration`，然后点击`Create branch`。这将打开一个对话框，可以在其中创建分支。

1.  从列表中选择一个存储库。
    *   下拉列表中显示了所有存储库的 git 主机服务徽标，以便于识别它们属于哪个 git 服务。
    *   如果有几个存储库具有相同的名称，列出的 GitLab 存储库的名称将带有 GitLab 所有者名称。比如 John Smith/second-web hook-test-repo。
    *   使用下拉列表中的搜索框过滤显示的存储库。
    *   *可选-将存储库指定为当前吉拉项目的默认选定存储库。
2.  选择一个`Source branch`。
    *   *可选-将分支指定为当前选定存储库的默认选定分支。
3.  输入一个`Branch name`或保持原样(推荐)。
4.  点击`Create branch`完成该过程。

新创建的分支现在与开发面板中`Branches`下列出的吉拉问题相关联。对新创建的分支执行 commit，为合并做好准备。

## **在吉拉创建 GitLab 拉取请求**

要从吉拉发行创建拉请求，打开所需的吉拉发行，选择吉拉 Git 集成`development panel`，点击`Open Git integration`，然后点击`Create pull request`。这将打开一个对话框，可以在其中提出合并请求。

1.  从列表中选择一个存储库。
    *   下拉列表中显示了所有存储库的 git 主机服务徽标，以便于识别它们属于哪个 Git 服务。
    *   如果有几个存储库具有相同的名称，列出的 GitLab 存储库的名称会附加一个 GitLab 组名。比如 BigBrassBand/second-web hook-test-repo。
    *   使用下拉列表中的搜索框过滤显示的存储库。
    *   *可选-将存储库指定为当前吉拉项目的默认选定存储库。
2.  选择新创建的分支作为源分支。
    *   *可选-将分支指定为当前选定存储库的默认选定分支。
3.  将 main 设置为目标分支。
4.  输入描述性标题或保持不变(推荐)。
5.  点击`Create pull request`完成该过程。按照 PR 的链接进行设置，以供审查和批准。

即使 MR 标题没有吉拉发行密钥，只要分行名称包含吉拉发行密钥，合并请求仍然基于分行名称进行索引。预览允许您查看所选源分支与目标分支(通常是主分支)中当前变更的比较视图。

## **吉拉 GitLab Webhooks**

默认情况下，存储库会按固定的时间间隔重新编制索引。如果你想让吉拉实时反映 GitLab 的更新，你必须设置 Webhooks。要将 webhooks 添加到您连接的 GitLab 存储库中，请完成以下步骤。

1.  在吉拉，从顶部导航栏中选择`Apps`，然后选择`Git Integration: Manage integrations`
2.  选择要添加 webhooks 的 GitLab 集成
3.  在左侧，选择`Feature Settings`
4.  复制`Indexing triggers`下的网址

接下来，我们需要去 GitLab 完成集成。

1.  在 GitLab 中，从您的项目中选择`Settings`，然后选择`Webhooks`
2.  粘贴您之前复制的 URL
3.  在`Trigger`下，选择`Push events`、`merge request events`和`Enable SSL verification`
4.  选择`Add webhook`

您的 webhook 现已创建。与吉拉问题相关联的提交、分支和合并请求现在将在吉拉问题中实时可见。

您的 webhook 现已创建。与吉拉问题相关联的提交、分支和合并请求现在将在吉拉问题中实时可见。

**吉拉 GitLab 集成优势**

## 将 GitLab 与吉拉集成对个人开发者和关键利益相关者都是有益的。通过弥合开发运维与项目管理工具之间的差距，团队成员可以获得更多的信息和更高的效率。

将 GitLab 与吉拉集成对个人开发者和关键利益相关者都是有益的。通过弥合开发运维与项目管理工具之间的差距，团队成员可以获得更多的信息和更高的效率。

**开发人员的主要优势**

### **更少的管理工作:**使用智能提交和吉拉自动化来更新吉拉，这样您就不需要改变您的工作流程，并且在吉拉花费的时间也更少。

*   **更少的特别状态更新:**开发涉众知道你何时创建分支，何时提交，然后是拉请求。给他们想要的可见性，而不增加你的工作量。
*   **少上下文切换:**留在你的流程里，专注编码。如果你必须去吉拉，你的回购协议托管服务提供商的链接可以让你轻松过渡。
*   **少上下文切换:**留在你的流程里，专注编码。如果你必须去吉拉，你的回购协议托管服务提供商的链接可以让你轻松过渡。

**项目和产品经理的最大收益**

### **更高的可见度:**开发活动不一定要成为吉拉的黑洞。通过与吉拉问题和项目一起呈现的上下文数据，轻松了解 Git repos 中发生的事情。

*   更干净的吉拉:减少开发人员手动更新吉拉问题的需要。利用吉拉自动化和智能提交来转换问题状态、受托人等，而无需手动干预。
*   **更好的计划和警告系统:**问题在没有提交和合并的情况下关闭？轻松标记应该有活动但没有的问题。
*   **更好的计划和警告系统:**问题在没有提交和合并的情况下关闭？轻松标记应该有活动但没有的问题。