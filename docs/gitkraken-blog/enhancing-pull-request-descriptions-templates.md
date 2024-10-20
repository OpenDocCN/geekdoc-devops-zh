# 如何使用模板增强拉取请求

> 原文：<https://www.gitkraken.com/blog/enhancing-pull-request-descriptions-templates>

Syncfusion 更喜欢使用 Git 工作流来管理我们跨各种平台的所有复杂产品。自从在我们的开发阶段采用 [Gitflow](/blog/gitflow) 模型以来，我们的日常工作已经被简化了。我假设这篇文章的读者已经熟悉了 [Gitflow](/blog/gitflow) 的基础知识，并且知道[拉请求](/blog/pull-requests-gitflow)在每个开发阶段的重要性。

作为开发人员，我们的责任不仅仅是修复问题或实现新的特性，而是将开发工作清楚地传达给评审人员。开发人员可以通过详细的文档或其他交流方式传达建议的代码更改及其目的。

这里是拉或合并请求的不可避免的描述发挥作用的地方，代码贡献者分享他们代码的详细注释。

GitKraken 团队在开发其工具时仔细考虑了团队协作:GitKraken [Git 客户端](http://gitkraken.com/git-client)和 [Glo 板](https://www.gitkraken.com/glo)。在 GitKraken 中有很多机会为您的文件更改和拉取请求添加重要的上下文，git kraken 支持提交到 GitHub、GitLab 或 VSTS(包括具有传统 VSTS URL 的 Azure DevOps)上的远程回购的拉取请求模板。稍后会有更多相关信息…

## 描述在拉取请求中扮演什么角色？

为了更好的代码审查过程，强烈建议提供对拉请求的描述。描述清楚地回答:

*   评审者在评审提交的代码时可以期待什么？
*   在提交他们的代码供评审之前，开发人员考虑了哪些标准？

稍后，评审者可以在他们自己和开发人员之间发起一个公开的讨论，以便在那些变更被推进到存储库之前跟踪每一个代码变更。

描述清单

## 为了使“拉”请求尽可能清晰，它应该包括一个关于建议的代码更改的相关信息的适当清单(而不是一行摘要)，例如:

如何修复 bug 以及解决方案的描述。

*   对新功能的描述或总结。
*   涵盖的单元测试用例。
*   这段代码是否破坏了现有的功能。
*   任何测试细节。
*   是否在所有设备和浏览器中进行了测试。
*   是否在所有设备和浏览器中进行了测试。

如果每次您发出新的拉取请求时，这些清单都自动出现在描述字段中，这不是很好吗？好消息！Gitflow 模型通过**模板**选项提供了这种能力。

更好的是，GitKraken 让您在整个工作流程中轻松访问模板。在本文的后面，我们将回顾如何在 GitKraken 中使用 pull 请求模板。

更好的是，GitKraken 让您在整个工作流程中轻松访问模板。在本文的后面，我们将回顾如何在 GitKraken 中使用 pull 请求模板。

免费下载我们的 Git GUI 客户端，轻松访问拉请求模板。

免费下载我们的 Git GUI 客户端，轻松访问拉请求模板。

在下图中，您可以看到一个如何在拉取请求中查看这些描述的示例。

在下图中，您可以看到一个如何在拉取请求中查看这些描述的示例。

什么是拉取请求模板？

## 当您为项目存储库中已实现的功能或错误修复启动新的 pull 请求时，您可以允许 description 字段预先填充与您的团队相关的项目的清单，就像上一节中提供的清单一样。在拉请求表单中显示预先填充的描述字段的过程通常被称为拉请求或合并请求模板。

对于 description 字段中详细信息的自动预填充，您必须首先定义自己的一组模板，然后将它们添加到项目的根目录中。您还可以为 bug、特性、文档等分别创建多个模板..

描述模板通常被起草为 Markdown 文件，并且应该被添加到项目存储库的适当目录中。目录的命名，以及项目存储库中单个和多个模板的处理，根据您更喜欢使用的服务(GitHub 或 GitLab)而有所不同。出于本文的目的，我们将重点关注 GitHub。

***注**:参考[降价文档](https://guides.github.com/features/mastering-markdown/)了解如何根据提供的语法指南编写降价文件中的内容。*

例如，您可以使用下面屏幕截图中显示的任务列表语法，引用减价文件中定义的清单。

相同的任务列表将显示在提交的合并请求页面中，如下所示。

相同的任务列表将显示在提交的合并请求页面中，如下所示。

如何在 GitHub 中创建单个拉请求模板

## 如果您的项目存储库在 GitHub 上，模板的名称和它在存储库中的位置非常重要。默认情况下，创建为 Markdown 文件的模板应该命名为`PULL_REQUEST_TEMPLATE.md`，并放在项目的根文件夹或`.github`目录中。

创建一个 Markdown 文件，将其命名为`PULL_REQUEST_TEMPLATE.md`，并将其放在项目的根文件夹中。

或者，创建一个目录，命名为`.github`，并将 Markdown 文件放在这个文件夹中。

***注意**:确保将 Markdown 文件放在前面提到的位置之一，并用相同的名称保存并合并。*

现在，当您创建一个新的 pull 请求时，您将看到模板内容自动加载到 description 字段中，如下图所示。

现在，当您创建一个新的 pull 请求时，您将看到模板内容自动加载到 description 字段中，如下图所示。

在 GitKraken 中使用拉请求模板

## 正如我们之前简单提到的， [GitKraken 支持 pull 请求模板](https://support.gitkraken.com/working-with-repositories/pull-requests/#pull-request-templates)提交到 GitHub、GitLab 或 VSTS 上的远程回购(包括带有传统 VSTS URL 的 Azure DevOps)。

当您在 GitKraken 中创建新的拉式请求时，将出现拉式请求模板下拉菜单，允许您选择您在其中一个存储库中创建的拉式请求。这使您可以使用预先起草的模板上指定的所有信息，轻松快速地填写拉动式请求。

[https://www.youtube.com/embed/YUiz3uZ_2Gc?feature=oembed](https://www.youtube.com/embed/YUiz3uZ_2Gc?feature=oembed)

视频

[https://www.youtube.com/embed/YUiz3uZ_2Gc?feature=oembed](https://www.youtube.com/embed/YUiz3uZ_2Gc?feature=oembed)

视频

如何在 GitHub 中创建多个拉请求模板

## 对不同类别的拉请求使用相同的描述模板是不明智的。例如，您可能需要发起一个 pull 请求来提交一个新实现的特性的代码变更、一个简单的 bug 修复、文档或者任何类型的配置工作。对于每个拉请求链接来说，最好根据提交的拉请求类别，在描述字段中加载和预填充不同的模板内容。

要使用不同的模板，您必须首先创建多个模板文件，并将它们作为单独的降价文件保存在存储库中，如以下步骤所述:

创建一个名为`PULL_REQUEST_TEMPLATE`的文件夹，并将其放在根目录或名为`.github`的目录中。

1.  创建多个模板降价文件，并将它们全部放在文件夹`PULL_REQUEST_TEMPLATE`中。
2.  创建多个模板降价文件，并将它们全部放在文件夹`PULL_REQUEST_TEMPLATE`中。

**注意**:在这个过程中创建的 Markdown 文件可以随意命名，但是第一步中提到的文件夹必须有名称`PULL_REQUEST_TEMPLATE`。

如果您在 GitHub 中有多个模板文件，您总是需要手动导航到您想要在 pull request 表单中预填充的特定 Markdown 文件的 URL。

例如，我正在创建一个 Markdown 文件，并将其命名为`bug-template.md`。我将它放在主存储库的`PULL_REQUEST_TEMPLATE`文件夹中。当我想将一些更改推入这个主存储库时，我将启动一个新的 pull 请求。为了用`bug-template.md`文件的内容预先填充我的拉取请求表单，我需要通过传递一个额外的模板参数导航到 URL:

`https://github.com/Scheduler_Project/compare/master…Test_branch?expand=1&template=bug-template.md`

这里，`Scheduler-Project`指的是我的主存储库的名称，我试图将来自`Test_branch`的代码变更合并到我的`master`分支中。在 URL 的末尾，有`template=bug-template.md`将`bug-template.md`文件的内容预填充到我的拉取请求表单中。

***注**:也可以参考 [GitHub 帮助文档](https://help.github.com/en/articles/about-automation-for-issues-and-pull-requests-with-query-parameters)，里面解释了如何在 URL 中使用模板查询参数。*

现在，在进行这些 URL 更改之后，您可以看到描述内容被加载到 pull request 表单中，如下所示。

您可以使用相关详细信息编辑之前的描述，并单击`Create pull request`按钮，这将打开您提交的拉取请求页面，如下所示。

您可以使用相关详细信息编辑之前的描述，并单击`Create pull request`按钮，这将打开您提交的拉取请求页面，如下所示。

在 GitLab 中创建合并请求模板

## 与 GitHub 存储库不同，GitLab 允许您在合并请求表单的用户界面中选择多个模板。

你可以在 [GitLab 支持文档](https://docs.gitlab.com/ee/user/project/description_templates.html)中看到更多相关信息。

结论

GitHub 和 GitLab 提供了一种更好的方法来为 pull 请求创建预定义的模板。两者都提供了模板选项，允许开发人员在评审过程开始时共享他们提出的代码变更的准确细节。提供更清晰的代码描述使得代码审查过程更容易，这反过来有助于实现更好的代码质量，并限制未被注意到的错误的风险。

## 一旦您花时间在 repo 中创建拉式请求模板，您和您的团队就可以在使用 [GitKraken](https://www.gitkraken.com/) 时持续使用它们——节省您的时间，减少上下文切换，鼓励一致性，并提高生产率。

GitHub 和 GitLab 提供了一种更好的方法来为 pull 请求创建预定义的模板。两者都提供了模板选项，允许开发人员在评审过程开始时共享他们提出的代码变更的准确细节。提供更清晰的代码描述使得代码审查过程更容易，这反过来有助于实现更好的代码质量，并限制未被注意到的错误的风险。

一旦您花时间在 repo 中创建拉式请求模板，您和您的团队就可以在使用 [GitKraken](https://www.gitkraken.com/) 时持续使用它们——节省您的时间，减少上下文切换，鼓励一致性，并提高生产率。