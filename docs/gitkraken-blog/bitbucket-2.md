# 吉拉比特桶集成|比特桶+吉拉使用技巧

> 原文：<https://www.gitkraken.com/jira-integrations/bitbucket-2>

吉拉被开发人员用作项目管理工具来跟踪和区分工作项目的优先级，而 Bitbucket 被用作 Git 存储库管理解决方案来存储和管理源代码。通过集成这两个工具，开发人员可以从在一个地方跟踪代码更改和任务的能力中受益，从而改进他们的工作流和团队内部的协作。

将 Bitbucket 和吉拉无缝集成的最佳方式之一是与 GitKraken 开发的顶级 Atlassian marketplace 应用程序吉拉的 [Git 集成。通过这种集成，开发人员可以专注于他们的核心职责，而不必经常在工具之间切换，并且项目经理可以获得对关键项目状态的额外洞察力。](https://marketplace.atlassian.com/apps/4984/git-integration-for-jira?tab=overview&hosting=cloud)

在本文中，我们将讨论以下与将 Bitbucket 与吉拉集成相关的主题:

[吉拉 Bitbucket 整合的好处](#benefits)

[用吉拉连接比特桶](#connect)

[将 Bitbucket 与吉拉问题链接](#link)

[在吉拉创建 Bitbucket 分支机构](#branch)

[在吉拉创建位桶拉取请求](#pull-request)

[吉拉 Bitbucket Webhooks](#webhook)

## 吉拉 Bitbucket 集成优势

通过针对吉拉的 Git 集成来集成 Bitbucket 和吉拉，为开发人员和项目经理提供了各自的优势。

开发人员的主要优势

### 消除上下文切换:集成 Bitbucket 和吉拉允许开发人员在一个中心位置跟踪代码变更和工作项目，消除了在不同工具之间切换的需要，并改善了他们的工作流程。

1.  改进的协作:通过将这两种工具结合在一起，开发人员可以更容易地共享代码并与其团队合作，促进更好的交流和协作。

2.  改进的协作:通过将这两种工具结合在一起，开发人员可以更容易地共享代码并与其团队合作，促进更好的交流和协作。

项目和产品经理的主要优势

### 提高关键项目状态的可见性:集成 Bitbucket 和吉拉为项目经理提供了每个项目状态的集中视图，使他们能够做出更明智的决策，更有效地跟踪进度。

1.  更准确地项目交付时间和设置准确的截止日期:通过集成 Bitbucket 和吉拉，项目经理可以访问关于代码更改和工作项目状态的实时信息，这有助于他们更准确地估计交付时间和设置准确的截止日期。

2.  将 Bitbucket 与吉拉连接

我们建议专门为集成创建一个 Bitbucket 用户。这样，Bitbucket 用户可以定义权限来执行给定的任务。确保该用户有权访问您希望与吉拉集成的存储库。

## 要继续您的 Bitbucket 吉拉集成，您需要从 Atlassian marketplace 安装用于吉拉的 Git 集成。像所有的 Atlassian marketplace 应用程序一样，你需要一个吉拉管理员来安装应用程序。一旦安装了 Git Integration for 吉拉，就可以开始将您的 Bitbucket 存储库与相关的吉拉项目连接起来。

在吉拉的顶部导航栏中，选择`Apps`，然后选择`Git Integration: Manage integrations`

选择右上方的`Add Integration`按钮

1.  从选择 Git 托管服务页面，选择`Bitbucket.org`用于云
2.  选择`Bitbucket OAuth`集成类型，点击`Connect Bitbucket`
3.  您可以通过展开工作区来连接所有存储库或选择单个存储库
4.  选择`Bitbucket OAuth`集成类型，点击`Connect Bitbucket`
5.  在这一点上，该应用程序将索引您的存储库，并使它们可以在吉拉查看。

将 Bitbucket 与吉拉问题联系起来

要在 Git 提交和吉拉问题之间建立联系，开发人员必须在其提交消息中包含吉拉问题密钥。

## Git 提交消息示例:`GIT-4322 – Updated the plugin` …

要在 Git 提交和吉拉问题之间建立联系，开发人员必须在其提交消息中包含吉拉问题密钥。

在上面的例子中，`GIT-4322`是将提交消息链接到吉拉问题的吉拉问题密钥。只有当主分支没有提交时，才包含属于非主分支的提交。

专业提示:在处理子任务时，将父任务和子任务吉拉发布键放在提交消息中，以便提交在两个地方都显示。这样，子任务的提交就不会在父问题的多次提交中丢失。

在上面的例子中，`GIT-4322`是将提交消息链接到吉拉问题的吉拉问题密钥。只有当主分支没有提交时，才包含属于非主分支的提交。

在吉拉创建 Bitbucket 分支

在您的吉拉云实例上，打开一个吉拉问题。在吉拉 Git 集成开发面板上，点击`Open Git integration`，然后点击`Create branch`。这将打开一个对话框，可以在其中创建分支。

## 从列表中选择一个存储库。

下拉列表中显示了所有存储库的 git 主机服务徽标，以便于识别它们属于哪个 Git 服务

1.  如果有几个存储库具有相同的名称，则列出的位存储库的名称将附加所有者名称。例如，`johnsmith/second-webhook-test-repo`
    *   使用下拉列表中的搜索框过滤显示的存储库
    *   *可选-将存储库指定为当前吉拉项目的默认选定存储库
    *   选择源分支。
    *   *可选-将分支指定为当前选定存储库的默认选定分支。
2.  输入分支名称或保持不变(推荐)。
    *   点击`Create branch`完成该过程。
3.  输入分支名称或保持不变(推荐)。
4.  新创建的分支现在与“分支”下的“发展”面板中列出的吉拉问题相关联。对新创建的分支执行 commit，为合并做好准备。

在吉拉创建位存储桶提取请求

要从吉拉问题创建拉式请求，请打开所需的吉拉问题，点击`Open Git integration`，然后点击`Create pull request`。这将打开一个对话框，可以在其中提出合并请求。

## 从列表中选择一个存储库。

下拉列表中显示了所有存储库的 Git 主机服务徽标，以便于识别它们属于哪个 Git 服务

1.  如果有几个存储库具有相同的名称，则列出的位桶存储库的名称将附加一个位桶组名称。例如，`BigBrassBand/second-webhook-test-repo`
    *   使用下拉列表中的搜索框过滤显示的存储库
    *   *可选-将存储库指定为当前吉拉项目的默认选定存储库。
    *   选择新创建的分支作为源分支。
    *   *可选-将分支指定为当前选定存储库的默认选定分支
2.  将 main 设置为`Target branch`
    *   输入描述性标题或保持不变(推荐)
3.  点击``Create pull request``完成该过程。请点击请购单链接进行审批
4.  输入描述性标题或保持不变(推荐)
5.  即使 PR 标题没有吉拉问题密钥，只要分支机构名称包含吉拉问题密钥，拉式请求仍然基于分支机构名称进行索引。预览允许您查看所选源分支与目标分支(通常是主分支)中当前变更的比较视图。

吉拉比特水桶网钩

默认情况下，存储库会按固定的时间间隔重新编制索引。如果您希望吉拉实时反映位桶更新，您必须设置 Webhooks。要将 webhooks 添加到您连接的 Bitbucket 存储库中，请完成以下步骤。

在吉拉，从顶部导航栏中选择`Apps`，然后选择`Indexing triggers`。

选择`Enable Indexing triggers`

转到左侧面板上的`Manage integrations`

## 选择要添加 webhooks 的位存储桶集成

在左侧，选择`Feature Settings`

1.  复制索引触发器下的 URL
2.  选择`Enable Indexing triggers`
3.  接下来，我们需要转到 Bitbucket 来完成集成。
4.  在 Bitbucket 中，从您的项目中选择`Settings`，然后选择`Webhooks`
5.  粘贴您之前复制的 URL
6.  在触发下，选择`Push events`、`Pull request events`和`Enable SSL verification`

选择`Add webhook`

您的 webhook 现已创建。与吉拉问题相关联的提交、分支和合并请求现在将在吉拉问题中实时可见。

1.  在 Bitbucket 中，从您的项目中选择`Settings`，然后选择`Webhooks`

3.  在触发下，选择`Push events`、`Pull request events`和`Enable SSL verification`
4.  今天整合吉拉和比特桶

集成吉拉和 Bitbucket 可以让开发者在更短的时间内完成更多的工作。通过将所有开发信息放在一个系统中，开发人员可以快速访问高效完成任务所需的数据。

当你节省了这么多时间，问题就变成了，你打算用这些额外的时间做什么？以下是我们的一些建议:

献身于最终击败你的朋友在超级粉碎兄弟

学习另一个 Javascript 框架

做个人项目

## 检查[r/程序幽默](https://www.reddit.com/r/ProgrammerHumor/)

集成吉拉和 Bitbucket 可以让开发者在更短的时间内完成更多的工作。通过将所有开发信息放在一个系统中，开发人员可以快速访问高效完成任务所需的数据。

不管你选择如何来打发这段新的时间，我们都祝你好运，并祝你编码愉快。想要将吉拉与您的其他回购托管服务整合吗？看看这些相关的分步指南:

*   [整合 JIRA 和 GITHUB](https://www.gitkraken.com/blog/jira-github-integration)
*   [整合 JIRA 和 GITLAB](https://www.gitkraken.com/blog/jira-gitlab-integration)
*   [整合 JIRA 和 AZURE DEVOPS](https://www.gitkraken.com/jira-integrations/jira-azure-devops-integration)
*   检查[r/程序幽默](https://www.reddit.com/r/ProgrammerHumor/)

不管你选择如何来打发这段新的时间，我们都祝你好运，并祝你编码愉快。想要将吉拉与您的其他回购托管服务整合吗？看看这些相关的分步指南:

[整合 JIRA 和 GITHUB](https://www.gitkraken.com/blog/jira-github-integration)

[整合 JIRA 和 GITLAB](https://www.gitkraken.com/blog/jira-gitlab-integration)

[整合 JIRA 和 AZURE DEVOPS](https://www.gitkraken.com/jira-integrations/jira-azure-devops-integration)