# 吉拉 Azure DevOps 集成

> 原文：<https://www.gitkraken.com/jira-integrations/jira-azure-devops-integration>

将 Azure DevOps 与吉拉集成，对于希望简化开发流程并改善团队间协作的组织来说，可能是一个游戏规则改变者。通过将这两个强大的平台结合在一起，开发人员可以消除在工具之间进行上下文切换的需要，同时项目经理可以更好地了解每个项目的状态。在本文中，我们将进一步了解将 Azure DevOps 与吉拉集成的好处，演示如何使用针对吉拉的 Git Integration 来设置集成，并探索开发团队可以立即开始利用它来显著改善协作和可见性的几种方式。

[连接 Azure DevOps 和吉拉](https://www.gitkraken.com/jira-integrations/jira-azure-devops-integration#connect)

[将 Azure DevOps 与吉拉问题联系起来](https://www.gitkraken.com/jira-integrations/jira-azure-devops-integration#link)

[在吉拉创建 Azure DevOps 分公司](https://www.gitkraken.com/jira-integrations/jira-azure-devops-integration#branch)

[在吉拉创建 Azure DevOps 拉请求](https://www.gitkraken.com/jira-integrations/jira-azure-devops-integration#pull-request)

[吉拉 Azure DevOps Webhooks](https://www.gitkraken.com/jira-integrations/jira-azure-devops-integration#webhooks)

想把吉拉和你的 GitHub 或者 GitLab repos 整合起来吗？看看这些相关的分步指南:

什么是吉拉？

## 吉拉是一个在开发者中广泛使用的问题跟踪和项目管理平台。它是敏捷友好的，对于管理任务、错误和分配非常有帮助。吉拉有两个不同的版本，一个是你自己托管的，另一个是云托管的。云托管版本将是本文的重点，但值得一提的是，如果您愿意，您可以选择自托管。吉拉的另一个好处是，如果你的项目碰巧是开源的，那么利用吉拉不会花你一分钱！

什么是 Azure DevOps？

Azure DevOps 允许开发人员管理代码版本、测试、自动化构建和发布项目。与吉拉类似，它因与 Visual Studio 和 Eclipse 等流行的开发环境兼容而被开发人员广泛使用。

## 什么是 Azure DevOps？

为什么要整合 Azure DevOps 和吉拉？

开发人员和项目经理(更不用说其他非技术利益相关者)都从 Azure DevOps 和吉拉的集成中获得了巨大的价值。实时访问相同的 Git 信息，如提交、分支、拉请求和标记，可以使团队更加协作和高效地工作。另外，你可以避免过多的状态会议和即兴更新。

## 开发人员的主要优势

改善协作和沟通:通过将 Azure DevOps 与吉拉集成，团队成员可以在一个集中的位置查看和更新工作项信息，这有助于改善团队之间的沟通和协作。

提高效率:通过集成这两个平台，团队成员可以在吉拉和 Azure DevOps 之间无缝移动以完成他们的工作，这有助于减少上下文切换以及管理开发项目所需的时间和精力。

### 开发人员的主要优势

1.  项目和产品经理的主要优势

2.  涉众对问题的进展和状态有了更好的理解，因为他们可以通过链接的提交、构建和其他工件看到已经完成的工作，并确定仍然需要做什么。

改进的项目规划:集成还可以帮助团队成员更有效地规划和跟踪工作，因为他们可以使用吉拉来创建工作项并确定其优先级，然后使用 Azure DevOps 来填写工作完成时的进度细节。

### 将 Azure DevOps 与吉拉联系起来

1.  要开始将 Azure DevOps 与吉拉集成，你首先需要从 Atlassian marketplace 安装一个 Git 集成应用。对于这个例子，我们将使用 [Git Integration for 吉拉](https://marketplace.atlassian.com/apps/4984/git-integration-for-jira?hosting=cloud&tab=overview)——一个拥有超过 10，000 次安装的高评级应用程序，支持云和 Azure DevOps 和吉拉的自托管版本。
2.  可以使用 OAuth 或个人访问令牌来执行集成身份验证。对于本例，我们将使用 OAuth 进行身份验证。

从 Atlassian Marketplace 安装 Git Integration for 吉拉应用程序。注意:像所有市场应用程序一样，你需要成为吉拉管理员才能安装该应用程序。

## 从吉拉顶部的导航栏中，选择应用程序，然后选择 Git 集成:管理集成

选择右上角的添加集成按钮

在 Git 托管服务页面上，为云选择 Azure DevOps Repos 或为自托管选择 Azure DevOps Server。在本例中，我们将选择云。

1.  选择 OAuth，然后从右下角连接 Azure Repos。
2.  遵循 Azure DevOps 流程并选择 Accept 以授权对您的存储库的访问。注意:我们建议创建一个新的 Microsoft 用户，并设置特定权限以使用吉拉云。
3.  选择您希望与吉拉连接的 Azure DevOps 存储库，然后选择“连接存储库”按钮
4.  在 Git 托管服务页面上，为云选择 Azure DevOps Repos 或为自托管选择 Azure DevOps Server。在本例中，我们将选择云。

6.  遵循 Azure DevOps 流程并选择 Accept 以授权对您的存储库的访问。注意:我们建议创建一个新的 Microsoft 用户，并设置特定权限以使用吉拉云。
7.  将 Azure DevOps 与吉拉问题联系起来

既然您已经将 Azure DevOps 存储库与吉拉连接起来，那么您就可以开始将吉拉问题与其相关的开发工作联系起来了。这很容易通过在提交消息中包含吉拉发布密钥来实现。

Git 提交消息示例:`GLK-312 - moved the images down the page`

在这种情况下，带有关键字`GLK-312`的吉拉问题将显示相关的提交信息。

## 将 Azure DevOps 与吉拉问题联系起来

Git 提交消息示例:`GLK-312 - moved the images down the page`

在吉拉创建 Azure DevOps 分公司

在吉拉，打开一期，你会看到一个标题为 Git 开发的版块。或者，您也可以从详细信息面板中单击 Open Git Integration。这将打开一个对话框，可以在其中创建分支。

从存储库下拉菜单中，确保选择了所需的 Azure DevOps 存储库。

从源分支下拉菜单中选择所需的源分支。

## 在分支机构名称下输入分支机构的名称。该值的默认格式在常规设置中配置。确保在标题中保留吉拉问题的关键。

选择“创建分支”按钮以结束流程。

1.  从存储库下拉菜单中，确保选择了所需的 Azure DevOps 存储库。
2.  *注意:默认存储库和分支可以在用户设置页面上更改，也可以通过单击设为默认按钮来更改。*
3.  在分支机构名称下输入分支机构的名称。该值的默认格式在常规设置中配置。确保在标题中保留吉拉问题的关键。
4.  此时，将在您的 Azure DevOps repo 中创建分支，并与选定的吉拉发行相关联。您将能够在 Development 和 Git integration 下的 Details 面板中看到这个分支。

在吉拉创建 Azure DevOps 拉请求

要在吉拉提出拉取请求，您将遵循与创建分支机构相似的流程。打开一个问题，然后选择 Git Development 下的 Create pull request。这将打开一个对话框，您可以在其中发出拉取请求。

从存储库下拉菜单中选择所需的存储库。

从源分支下拉列表中为您的拉取请求选择源分支。

## 从目标分支下拉列表中选择您希望将分支合并到的位置。

确认或更改您请求的标题。确保标题中有吉拉问题的答案。

1.  选择“创建拉式请求”按钮。
2.  从源分支下拉列表中为您的拉取请求选择源分支。
3.  拉请求将在 Azure DevOps 上创建，并与所选吉拉问题相关联。在 Details 面板中，您将能够看到开发或 Git 集成下的 pull 请求。
4.  吉拉蔚蓝 DevOps 网页挂钩
5.  Webhooks 是保持你的 Azure DevOps 和吉拉数据实时同步的好方法。如果没有启用 webhooks，默认情况下，存储库会以固定的时间间隔重新编制索引。要为 Azure DevOps 存储库设置 webhooks，请完成以下步骤。

在吉拉

从顶部导航栏中选择应用程序，然后选择 Git 集成:管理集成

选择要添加 webhooks 的 Azure DevOps 集成

## 在左侧选择特性设置

复制索引触发器下的 URL

在蔚蓝的 DevOps

在您的 Azure DevOps 项目中，选择左下方的项目设置

1.  在左侧导航窗格中，选择服务挂钩
2.  选择创建订阅
3.  向下滚动并选择 Web 挂钩，然后单击下一步
4.  选择所需的触发器(例如，创建拉式请求)

根据需要对存储库、目标分支或其他选项进行筛选，然后单击下一步。

粘贴你从吉拉复制的网址，然后点击完成。

1.  在您的 Azure DevOps 项目中，选择左下方的项目设置
2.  这将创建并启用您的 webhook。对其他所需的触发事件重复该过程。
3.  选择创建订阅
4.  立即集成吉拉和 Azure DevOps
5.  整合吉拉和 AzureDevOps 是每个使用这两个工具的组织都应该做的事情。由于 Git 数据(如提交和拉取请求)对所有开发涉众来说都是可见的，因此团队协作得到了极大的改进。此外，开发人员将能够通过在吉拉和 Azure DevOps 之间无缝执行操作来减少上下文切换并提高效率。
6.  立即开始吉拉 Azure DevOps 集成，免费试用 [Git Integration for 吉拉](https://www.gitkraken.com/git-integration-for-jira/try-free)！
7.  粘贴你从吉拉复制的网址，然后点击完成。

这将创建并启用您的 webhook。对其他所需的触发事件重复该过程。

## 立即集成吉拉和 Azure DevOps

整合吉拉和 AzureDevOps 是每个使用这两个工具的组织都应该做的事情。由于 Git 数据(如提交和拉取请求)对所有开发涉众来说都是可见的，因此团队协作得到了极大的改进。此外，开发人员将能够通过在吉拉和 Azure DevOps 之间无缝执行操作来减少上下文切换并提高效率。

立即开始吉拉 Azure DevOps 集成，免费试用 [Git Integration for 吉拉](https://www.gitkraken.com/git-integration-for-jira/try-free)！