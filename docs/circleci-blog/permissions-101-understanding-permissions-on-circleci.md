# 权限 101:了解 CircleCI - CircleCI 上的权限

> 原文：<https://circleci.com/blog/permissions-101-understanding-permissions-on-circleci/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

我们最近为所有 CircleCI 用户推出了许多新功能。请求最多的功能之一集中在权限设置上。这篇博客将深入探讨这个新特性，它是如何工作的，以及它对使用 CircleCI 的你和你的团队意味着什么。

**什么是权限？**

权限是 CircleCI 上的一个新特性，它可以让您更好地控制项目的设置。使用权限，组织可以限制谁可以在 CircleCI 上更改“项目设置”。此设置在组织级别或组织内的项目级别可用。为此，CircleCI 利用 GitHub 的权限来确定用户的访问权限。

**用户或组织为什么要启用权限？？**

为您的组织启用权限后，只有 GitHub repo 管理员或 GitHub 所有者才能在 CircleCI 上更改“项目设置”。这对于大型团队来说特别有用，可以确保您的项目设置只能由具有管理员权限的团队成员更改。这对于大型团队来说尤其有用，可以确保您的项目设置不会被团队中的非 GitHub 管理员/所有者意外更新。

这将如何影响我的团队？

为您的组织启用权限后，GitHub 成员和贡献者将无法对 CircleCI 的“项目设置”页面进行任何更改。这包括为您的组织设置容器、项目级并行化设置、构建设置等。GitHub 成员或合作者仍然可以使用 CircleCI 的功能，如重建、使用缓存重建或通过 SSH 重建。

如果我改变主意了怎么办？我可以关闭这个功能吗？

如果您发现权限不适合您，您可以随时创建支持请求(在应用程序中，点按“帮助？”按钮并选择“支持”)，我们将为您的组织禁用权限。

**入门:如何在 GitHub 上设置用户权限？**

要设置您的团队在 GitHub 上的访问权限，请导航到您所在组织的以下链接:https://github.com/orgs/org-name/teams.

您可以使用以下链接设置 repo 级别的用户访问:https://github . com/org-name/repo-name/settings/collaboration。

阅读更多关于[在 GitHub](https://help.github.com/articles/setting-up-teams/) 上设置团队权限的信息。

如何在 CircleCI 上启用权限？

如果您希望为您的组织或组织内的特定项目启用此功能，请创建支持请求(在应用程序中，单击“？”当你滚动鼠标时，按钮上写着“救命？”并选择“支持”)

你希望 CircleCI 增加什么功能？您可以在我们社区网站的功能请求类别上提交一个新主题。你也可以在我们的[开发者选择奖](https://discuss.circleci.com/t/developers-choice-nominees/3215)中投票，在这里我们收集了九个最常被请求的功能，我们很想构建这些功能，但由于各种原因，它们从未出现在我们的待办事项列表中。这是您直接对这些功能进行投票的机会，我们将构建获得最多投票的功能。投票将于太平洋时间 4 月 22 日星期一下午 5:00 结束。请访问[开发者的选择](https://discuss.circleci.com/t/developers-choice-nominees/3215)，为您希望我们下一步开发的功能投票。