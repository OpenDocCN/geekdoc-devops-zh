# GitKraken Git GUI & GitKraken 板支持 GitHub 动作

> 原文：<https://www.gitkraken.com/blog/gitkraken-glo-boards-support-github-actions>

随着最近 GitHub 动作更新的发布，我们很高兴地宣布在用于任务跟踪的 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 和 [GitKraken 板](https://www.gitkraken.com/boards)中支持 GitHub 动作。

如果你还没有看到，这里是关于 GitHub 动作更新的 GitHub 公告:

[https://www.youtube.com/embed/E1OunoCyuhY?start=2151&feature=oembed](https://www.youtube.com/embed/E1OunoCyuhY?start=2151&feature=oembed)

视频

## **什么是 GitHub 动作？**

GitHub Actions 是一个云服务，您可以使用它来自动构建和测试您的代码项目，并将其提供给其他用户。它几乎适用于任何语言或项目类型。

GitHub Actions 结合了持续集成和持续交付(CI/CD ),以持续一致地测试和构建您的代码，并将其交付给任何目标。

***了解持续集成和交付(CI/CD)如何融入 DevOps 生命周期。***

[**2020 DevOps 工具报告**](https://www.gitkraken.com/resources/devops-report-2020#ci-cd)

您还可以使用 GitHub Actions Marketplace 中的动作来创建自动化和工作流，以执行基于各种触发器的几乎任何动作。

在这篇博文的剩余部分，我们将解释 GitKraken Git GUI 和 GitKraken 板是如何支持 GitHub 动作的。

## **git krak 支持**

### **创建和管理 GitHub 动作工作流程**

GitHub 操作服务中的工作流运行构建、测试和部署代码的操作。它们作为`.yml`文件存在于存储库中的`.github/workflows`目录中。GitHub Actions 集成帮助您在 GitKraken Git GUI 中轻松创建和管理这些工作流文件。

一个新的 GitHub Actions 部分被添加到 GitKraken Git GUI 的左侧面板中，该部分将为在 GitHub 上具有上游遥控器的存储库显示，或者当存储库包含`.github/workflows`目录时显示。此部分将显示您的存储库的当前检出分支上的任何现有工作流文件，并提供使用 GitKraken 的内置编辑器查看和编辑这些文件的快速访问。

您可以通过点击绿色的`Create Workflow`按钮在 GitKraken Git GUI 中创建新的工作流程。

从这里，您可以选择一个可选的模板，或者从头开始您自己的工作流程。

新创建的工作流程文件将立即在 GitKraken 的文件编辑器中打开，您可以在其中查看、修改和保存更改。只需在“提交”面板中暂存工作流文件，然后提交即可看到它与其他工作流一起出现在左侧面板中。

将您的更改推送到 GitHub，GitHub Actions 服务就可以使用您的新工作流了！

## **GitKraken 板支架**

### **自动卡更新**

我们很高兴地宣布 GitHub Actions 将立即在 GitKraken 电路板上发布。您现在可以使用 GitHub 动作自动操作 GitKraken 板上的卡片。我们最初发布了六个 GitHub 动作供您在存储库工作流中使用。有关行动的完整列表和更多信息，请参考[https://github.com/Axosoft/glo-actions/](https://github.com/Axosoft/glo-actions/)。现在，下面的例子可以让你开始。

示例:当在 GitHub 上合并 PR 时，将链接的卡片移动到`Deployed`列。

在您的 GitKraken 帐户中创建一个[个人访问令牌](https://support.gitkraken.com/developers/pats/) (PAT)。动作只需要读/写权限。

**注意:该用户的所有操作都会自动更新 Glo 中的卡片。*

2.在 GitHub 的存储库设置中添加 PAT 的访问令牌作为[秘密的值。](https://help.github.com/en/articles/creating-a-github-action#storing-secrets-in-your-repository)

3.在 GitHub 上为您的存储库建立一个[工作流，它将触发必要的动作(您将添加秘密名称作为`authToken`)。](https://github.com/features/actions)

4.在你的公关描述中添加一个你名片的链接。

就是这样！这些操作现在将根据您的拉取请求自动更新链接的卡。