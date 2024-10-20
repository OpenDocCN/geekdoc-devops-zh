# 如何编写一个好的 Git 提交消息| Git 最佳实践

> 原文：<https://www.gitkraken.com/learn/git/best-practices/git-commit-message>

在我们进入编写顶层 Git 提交消息的最佳实践之前，让我们先快速回顾一下 Git 中的提交是什么。

Git 提交是一种“记录对存储库的变更”的方式一个 [Git 库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)是在。项目的 git 文件夹。简单地说，提交是本地存储库的快照。

将提交视为项目的检查点或保存点可能会有所帮助。在许多视频游戏中，在完成特定的动作或挑战后，会到达检查点并保存您的进度。类似地，Git 提交通常是在对项目做出重大贡献之后执行的，并且您希望保存您的进度。

从这个角度来看，很容易理解为什么“git commit”是最常用的 [Git 命令](https://www.gitkraken.com/learn/git/commands)之一。每次开发人员执行提交时，他们都可以选择编写所谓的提交消息。Git 提交消息用于解释提交的功能。

什么是好的承诺

### 如果通过快速查看以前的提交消息，您可以辨别每个提交做了什么以及为什么做出更改，那么您就在正确的轨道上。但是，如果您的提交消息令人困惑或杂乱无章，那么您可以通过这篇文章的帮助来改进您的提交消息实践，从而帮助您未来的自己和您的团队。 ****

例如，如果您在对项目的自述文件进行简单更新后提交，您可能会包含类似于以下内容的消息:“更新自述文件的标点符号”。现在想象一个 README 文件更新，包含以下任何提交消息:“更新标点符号”、“进行了修复”或“Chipotle 规则”。很容易看出这种类型的提交消息是如何失去控制的。虽然您可能会得到一两个跑题或不清楚的提交信息，但当它们开始累积后，很快就会回来困扰您和您的团队。在本文中，我们将介绍如何使用 GitKraken 客户端的 CLI 和 GUI 编写良好的 Git 提交消息，以及您可以应用于提交消息的提示和技巧，以改善团队沟通和存储库健康。尽管我们将引用的例子是在 GitKraken 客户端中，但是这些提示同样适用于 VS 代码用户。

例如，如果您在对项目的自述文件进行简单更新后提交，您可能会包含类似于以下内容的消息:“更新自述文件的标点符号”。现在想象一个 README 文件更新，包含以下任何提交消息:“更新标点符号”、“进行了修复”或“Chipotle 规则”。很容易看出这种类型的提交消息是如何失去控制的。虽然您可能会得到一两个跑题或不清楚的提交信息，但当它们开始累积后，很快就会回来困扰您和您的团队。在本文中，我们将介绍如何使用 GitKraken 客户端的 CLI 和 GUI 编写良好的 Git 提交消息，以及您可以应用于提交消息的提示和技巧，以改善团队沟通和存储库健康。尽管我们将引用的例子是在 GitKraken 客户端中，但是这些提示同样适用于 VS 代码用户。

GitKraken 客户端的提交图，在 CLI 和 GUI 中都可用，可以很容易地可视化引用您的提交和相关消息。

Git 提交工作流

在开始编写一流的提交消息之前，首先需要理解与 Git 提交相关的工作流。为了进行提交，开发人员必须:

## 更改

阶段变化

1.  提交更改
2.  进行更改包括添加、删除或更改本地分支上的任何内容。该分支上的任何更改都包含在所谓的工作目录中。
3.  暂存更改意味着获取您已经更改的文件，并将它们移动到“准备提交”状态，称为暂存目录。您可能还会听到开发人员将临时目录称为临时区域或索引。您可以存放任意数量的文件，但请注意，多个文件存放在一起将共享相同的提交消息，这一点很重要。

这意味着您将希望确保所有的提交都以对您和您的团队有意义的方式相互关联。例如，可行的做法是，将一组文件集中在您正在处理的特定修补程序上。相反，如果每个文件关注于项目的完全不同的范围，那么存放多个文件是没有意义的。

正如我们所介绍的，提交是创建本地存储库快照的行为。这是开发人员保存工作的方式，也是一个常见的 Git 命令。

提交历史记录

一个项目中包含的所有提交的记录被称为[提交历史](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History#:~:text=The%20most%20basic%20and%20powerful,is%20the%20git%20log%20command.&text=By%20default%2C%20with%20no%20arguments,recent%20commits%20show%20up%20first.)，可以根据您使用的 Git 工具以多种方式访问。使用 GitKraken 客户端访问提交历史可以通过在内置 CLI 中运行`git log`来完成，或者通过简单地查看 GUI 的中央提交图来完成。

如果您使用 VS 代码，您可以利用 GitLens 在每一行代码上公开项目的提交历史，如下所示。快速识别与一行代码相关的提交消息以及作者信息的能力非常有价值。编写好的提交消息，清楚地解释代码做了什么，使得这个功能更加有用。

提交历史记录

## 使用 GitLens for VS Code 访问每一行代码中有价值的信息，包括作者信息、提交消息、差异视图等！

如果您使用 VS 代码，您可以利用 GitLens 在每一行代码上公开项目的提交历史，如下所示。快速识别与一行代码相关的提交消息以及作者信息的能力非常有价值。编写好的提交消息，清楚地解释代码做了什么，使得这个功能更加有用。

一个项目的提交历史对于你将来是有价值的，对于任何其他贡献代码的开发人员也是有价值的，因为它提供了关于项目中已经完成了什么的上下文。随着开发人员变得忙碌，已建立的项目文档往往会落后并变得陈旧。当这种情况发生时，许多开发人员转向提交历史来了解项目并识别最近的更新。有鉴于此，编写有意义且易于理解的提交消息变得更加重要。

Git 提交消息结构

Git 提交消息有两个主要组成部分:标题或摘要，以及描述。提交消息标题限于 72 个字符，描述没有字符限制。虽然这是既定的字符限制，但大多数开发人员建议提交消息摘要不要超过 50 个字符，描述不要超过 72 个字符。

最终，构造异常提交消息的技巧是在简洁和细节之间找到适当的平衡。简洁到易于阅读，但详细到易于理解。

[Download GitLens Free!](https://www.gitkraken.com/gitlens/try-free)

一个项目的提交历史对于你将来是有价值的，对于任何其他贡献代码的开发人员也是有价值的，因为它提供了关于项目中已经完成了什么的上下文。随着开发人员变得忙碌，已建立的项目文档往往会落后并变得陈旧。当这种情况发生时，许多开发人员转向提交历史来了解项目并识别最近的更新。有鉴于此，编写有意义且易于理解的提交消息变得更加重要。

标准化 Git 提交消息结构

编写提交消息时，一致性是关键。无论您是独自工作还是与数百名贡献者合作开发一个开源项目，保持一致的风格并遵守既定的提交消息约定都是至关重要的。

## 建立标准化的提交消息结构有助于保持您的存储库干净、清晰，并使项目参与者更容易理解每个提交背后的目的。

如果您在团队中工作，项目负责人应该传达预期的提交消息指南。大多数开源项目在其文档中都有关于提交消息最佳实践的说明。如果您找不到清晰的文档，您可以查看提交历史，并根据过去的提交对您的消息进行建模。

诚然，遵循一个标准化的提交消息有时会令人感到乏味，但是现在花时间构建一个深思熟虑的消息不仅会在[代码审查](https://www.gitkraken.com/blog/code-review)或项目审计期间节省你的时间，这是一个将你与你的同行区分开来的简单方法，并且受到项目经理的赞赏。

ProTip:一些团队在他们的提交消息摘要中包含一个类似于`*`的特殊字符，以表示在提交消息描述中写入了更多信息。

### 编写提交消息时，一致性是关键。无论您是独自工作还是与数百名贡献者合作开发一个开源项目，保持一致的风格并遵守既定的提交消息约定都是至关重要的。

想要撰写具有个人风格的精彩提交消息吗？GitKraken 客户端允许您在提交消息中使用表情符号！😍💥🦑

快速 Git 提交消息提示

您可以做一些事情来立即改进 Git 提交消息:

避免不必要的大写

仔细检查你的拼写

不要用标点符号结束提交消息摘要

不要用标点符号结束提交消息摘要

遵循这些简单的指导方针可以更容易地搜索和过滤提交，并使您的存储库看起来更具视觉吸引力。这些步骤不仅快速简单，而且你的团队成员也会感谢你。

## 您可以做一些事情来立即改进 Git 提交消息:

使用祈使句动词形式

1.  帮助您编写好的 Git 提交消息的另一个最佳实践是使用命令动词形式。使用命令式动词形式可以确保提交消息中使用的潜在动词，如“fixed”或“updated ”,以正确的时态写成“fix”和“updated”。
2.  这已经成为许多 Git 项目事实上的标准。确保您的消息符合标准的一种方法是应用公式:“如果应用，我的提交将…”您可以将此应用于示例:
3.  如果应用，我的提交将(插入提交消息文本)

总结:

`Add new Kief-the-kraken Keanu-Kief to keif gallery`

描述:

### `Modify menu.yml and keif-gallery.md related to ticket #12345`

这已经成为许多 Git 项目事实上的标准。确保您的消息符合标准的一种方法是应用公式:“如果应用，我的提交将…”您可以将此应用于示例:

吉拉集成问题 ID 或票证编号

在提交消息中包含附加上下文的一种方法是引用相关的吉拉问题 ID。虽然这不应该作为高质量提交消息的替代，但是添加吉拉 ID 可以帮助您团队中的其他开发人员引用关于您的提交地址的附加信息。

GitKraken Client Pro 用户可以利用[吉拉集成](https://www.gitkraken.com/integrations/jira)开始在他们的工作流程中使用这种做法。

要使用 GitKraken Client Pro 编写与吉拉问题相关的提交消息，请按照以下步骤操作:

从左侧面板中选择`JIRA ISSUES`

找到您要参考的问题

标识问题 ID

### 选择问题旁边的省略号并点击`Copy issue link`

GitKraken Client Pro 用户可以利用[吉拉集成](https://www.gitkraken.com/integrations/jira)开始在他们的工作流程中使用这种做法。

要使用 GitKraken Client Pro 编写与吉拉问题相关的提交消息，请按照以下步骤操作:

提交与问题相关的更改，并编写一条 Git 提交消息，如下例所示:

1.  总结:
2.  `Update electron version to enable faster speeds <closes Jira Issue #123>`
3.  描述:
4.  `Changes the abc in order to comply with xyz. See Jira ticket #123 for further details. `paste issue link``

常规提交

传统的提交消息样式是另一种提升提交消息级别的方式。传统的提交结构包括用指定的提交类型开始提交消息。提交类型包括:

`Feat`–特征

`Fix`–错误修复

`Docs`–对自述文件等文档的更改

`Style`–样式或格式改变

`Perf`–提高代码性能

`Test`–测试一项功能

`Test`–测试一项功能

使用传统的提交方法，项目参与者可以轻松地筛选和搜索特定的提交，如下例所示:

### 总结:

`Docs: Fixes typo on in-from-the-depths.md`

描述:

`Closes ticket #54321`

Git 经常提交

另一个显著提高提交消息质量和效用的简单方法是更频繁地提交。首先，提交通常是一个好主意，因为它可以确保你的工作被保存，它还可以使你的提交信息简洁明了。

比方说，你已经完成了一整天的工作，但是还没有完成。精心制作一个清晰简洁的提交消息几乎是不可能的。

另一方面，如果您经常提交，很容易写出一个简单的提交消息，如下所示，它清楚地表达了这一点，而不会占用太多时间或过于冗长。

使用传统的提交方法，项目参与者可以轻松地筛选和搜索特定的提交，如下例所示:

Using the conventional commit method makes it easy for project contributors to filter and search for specific commits, as shown in the example below: 

如何用命令行创建优秀的提交消息

要在 GitKraken 中使用 [Git CLI](https://www.gitkraken.com/cli) 来使用您新发现的 Git commit 消息最佳实践，您首先需要从顶部工具栏中选择`Terminal`。

描述:

`Closes ticket #54321`

现在，您需要从您的工作目录转移变更。运行`git status`是查看需要暂存的文件的好方法。从这里，您有两个选择:您可以使用`git add <file name>`暂存单个文件，这是通常推荐的，或者，您可以使用`git add -all`暂存工作目录中的所有文件。

### 另一个显著提高提交消息质量和效用的简单方法是更频繁地提交。首先，提交通常是一个好主意，因为它可以确保你的工作被保存，它还可以使你的提交信息简洁明了。

在 GitKraken 客户端中使用 CLI 的一个惊人优势是，您不必再次运行`git status`来确认您的文件是否被正确暂存。

GitKraken 客户端通过其著名的提交图和有用的提交面板，可以很容易地从视觉上确认预期的 Git 操作是否真正发生。提交图显示了项目的提交历史，而右侧的提交面板显示了文件从工作目录移动到暂存目录，最后到达提交消息的过程。

从这里开始，您可以通过使用`git commit -m <Summary>`或更详细的`git commit -m <Summary> -m <Description>`来应用您所学到的 Git 提交消息最佳实践。决定使用哪一个将取决于您的团队同意的标准化提交消息机制，或者项目的默认消息结构。

请记住，现在编写一个全面而详细的提交消息将在将来为您节省时间。

如何用命令行创建优秀的提交消息

如何使用 GitKraken 客户端编写一个好的提交消息

使用 GitKraken 客户端中的 GUI 编写提交消息很简单。首先从您的工作目录中暂存您想要提交的文件。您会注意到 GitKraken 客户端在 UI 右侧的“提交面板”中保存了一个工作目录中所有未暂存文件的列表。

如果您已经对工作目录中的多个文件进行了更改，但想要暂存一个特定的文件，请将鼠标悬停在文件名上，然后单击出现在文件名右侧的`Stage File`按钮。要准备您的所有更改，只需选择面板右上角的`Stage all changes`按钮。

当文件从提交面板的`Unstaged Files`部分向下移动到正下方的`Staged Files`部分时，您可以使用 GitKraken Client 轻松验证文件是否已暂存。

从这里开始，您可以通过使用`git commit -m <Summary>`或更详细的`git commit -m <Summary> -m <Description>`来应用您所学到的 Git 提交消息最佳实践。决定使用哪一个将取决于您的团队同意的标准化提交消息机制，或者项目的默认消息结构。

一旦您的文件被暂存，您就可以编写顶层提交消息了。导航到提交面板底部标记为`Commit Message`的部分，并选择`Summary`。

这里，您有 72 个字符来总结您提交的目的，并应用本文中介绍的最佳实践。当您对提交摘要感到满意时，您可以选择下面的`Description`来提供额外的上下文。遵循您的团队或项目负责人制定的准则来填写详细描述。

当您对提交消息感到满意时，选择绿色的`Commit changes to (#) file`按钮。您可以通过参考中央提交图来验证您的提交是否成功。

## 使用 GitKraken 客户端中的 GUI 编写提交消息很简单。首先从您的工作目录中暂存您想要提交的文件。您会注意到 GitKraken 客户端在 UI 右侧的“提交面板”中保存了一个工作目录中所有未暂存文件的列表。

Composing commit messages is simple using the GUI in GitKraken Client. Start by staging the files from your working directory that you want to commit. You’ll notice that GitKraken Client keeps a list of all the unstaged files in your working directory in the “Commit Panel” located on the right side of the UI. 

如何编辑 Git 提交消息

因为提交在 Git 工作流中非常常见，所以几乎可以肯定的是，在某些时候，您需要知道如何编辑 Git 提交消息。要求您更改 Git 提交消息的最常见原因包括打字错误和添加澄清内容。

要在 GitKraken 客户端中编辑提交消息，只需选择提交，浏览提交面板，单击显示当前提交消息的框，进行必要的编辑，然后选择绿色的`Update Message`按钮。就这么简单！

要在命令行中更改提交消息，可以使用:

`git commit --amend -m “new commit message”`

这里，您有 72 个字符来总结您提交的目的，并应用本文中介绍的最佳实践。当您对提交摘要感到满意时，您可以选择下面的`Description`来提供额外的上下文。遵循您的团队或项目负责人制定的准则来填写详细描述。

您应该知道，您只能编辑最近的提交。这就是为什么确保仔细检查每个提交消息的拼写和格式如此重要。

更改与旧提交相关联的 Git 提交消息需要您[恢复 Git 提交，](https://www.gitkraken.com/learn/git/problems/revert-git-commit)，这是一个更加核心的选项。

## 因为提交在 Git 工作流中非常常见，所以几乎可以肯定的是，在某些时候，您需要知道如何编辑 Git 提交消息。要求您更改 Git 提交消息的最常见原因包括打字错误和添加澄清内容。

想要帮助您的团队实现更好的沟通和可见性吗？查看 GitKraken 客户端的团队功能，包括查看团队成员正在处理的文件、深度链接、预防性合并冲突工具和灵活的许可管理。

高质量 Git 提交消息=以后节省时间

应用这些 Git 提交消息最佳实践将有助于提高您浏览项目历史、理解变更的能力，并为项目的未来提供有用的指导。编写全面的提交消息不一定是乏味的或者不合理的耗时。另外，像 [GitLens](https://www.gitkraken.com/gitlens) 和 [GitKraken Client](https://www.gitkraken.com/git-client) 这样的工具可以帮助你以直观和不引人注目的方式获得编写高质量提交消息的好处！

`git commit --amend -m “new commit message”`

 `git commit --amend -m “new commit message”` 

您应该知道，您只能编辑最近的提交。这就是为什么确保仔细检查每个提交消息的拼写和格式如此重要。

更改与旧提交相关联的 Git 提交消息需要您[恢复 Git 提交，](https://www.gitkraken.com/learn/git/problems/revert-git-commit)，这是一个更加核心的选项。

Changing the Git commit message associated with an older commit requires you to [revert a Git commit,](https://www.gitkraken.com/learn/git/problems/revert-git-commit) and is a much more nuclear option. 

想要帮助您的团队实现更好的沟通和可见性吗？查看 GitKraken 客户端的团队功能，包括查看团队成员正在处理的文件、深度链接、预防性合并冲突工具和灵活的许可管理。

Want to help facilitate even better communication and visibility for your team? Check out GitKraken Client’s team features including the ability to see which files your team members are working on, deep linking, preventative merge conflict tool, and flexible license management.

高质量 Git 提交消息=以后节省时间

## 应用这些 Git 提交消息最佳实践将有助于提高您浏览项目历史、理解变更的能力，并为项目的未来提供有用的指导。编写全面的提交消息不一定是乏味的或者不合理的耗时。另外，像 [GitLens](https://www.gitkraken.com/gitlens) 和 [GitKraken Client](https://www.gitkraken.com/git-client) 这样的工具可以帮助你以直观和不引人注目的方式获得编写高质量提交消息的好处！

Applying these Git commit message best practices will help improve your ability to navigate your project’s history, understand changes, and provide useful direction for the future of your project. Writing comprehensive commit messages doesn’t have to be tedious or unreasonably time consuming. Plus, tools like [GitLens](https://www.gitkraken.com/gitlens) and [GitKraken Client](https://www.gitkraken.com/git-client) are here to help you capture the benefits of writing high-quality commit messages in intuitive and unobtrusive ways!