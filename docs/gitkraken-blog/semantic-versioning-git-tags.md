# 使用语义版本和 Git 标签管理发布

> 原文：<https://www.gitkraken.com/gitkon/semantic-versioning-git-tags>

[https://www.youtube.com/embed/4wPjo5C-v8Y?feature=oembed](https://www.youtube.com/embed/4wPjo5C-v8Y?feature=oembed)

视频

麻省理工学院斯隆管理学院网站，像许多网站和应用一样，使用 Git 来管理其代码库。像任何系统一样，当需要部署到生产环境时，代码被打包成工件发布。那个工件是在特定时间点的一套标准化的代码版本。

您维护项目的时间越长，发布管理就会变得更加混乱和复杂。如果您和您的团队缺乏一个标准的、简洁的方法来确定一个发布意味着什么，那么发布管理就会变得混乱。如果您没有可靠的方法来确定不同版本之间您的代码库发生了什么变化，那么当您查看过去的版本时，您可能不明白哪个版本中推出了什么。

一种已经被证明有助于随时间管理版本的方法是语义版本化。与 Git 标签一起使用，语义版本控制允许您轻松地指出生产代码中的变化程度，并在将来查看它们时理解这些变化。让我们看看语义版本化、Git 标记的基础，以及如何将两者结合起来创建语义 Git 版本。

GitKraken 漂亮的提交图将帮助您以一种快速理解的方式可视化您的项目回购，使版本发布和管理过去的版本变得安全和容易。

## **定义语义版本**

本质上，语义版本化只是一个编号模式。这是一种行业标准的软件开发实践，用于指示自上一个产品发布以来的变化程度。

如果你看看你今天使用的任何一个软件，产品的开发团队很可能使用语义版本化来跟踪它们的变化。例如，您的操作系统和 web 浏览器都将有一个语义版本号。几乎每个人都在使用它，因为语义版本化是一种清晰、简洁的方式来表示变更的级别。

语义版本号有三个部分:

主要号码

1.  次要编号
2.  路径号
3.  当这些增量中的每一个时，它意味着不同程度的变化已经进入你的代码库。

    我们先来看补丁号。当你有一个补丁发布时，比如从版本 1.0.1 升级到版本 1.0.2，这表明已经有了错误修复或者微小的更新。这些并不是对应用程序功能的重大改变。

    一个小版本，比如从版本 1.0.2 到 1.1.0，表示功能上有变化。这可以是功能更改、添加全新功能或删除现有功能。

主要版本，如从版本 1.1.0 到版本 2.0.0，表示可能破坏向后兼容性或显著改变功能的变化。这可能是从移除不推荐使用的代码到引入应用程序的完整重新架构的所有事情。主要版本号告诉人们已经发生了重大变化，有些东西可能不再像在旧版本上那样工作，或者需要遵循新的步骤才能获得相同的功能。

When each one of these increments, it means a different degree of changes has gone into your codebase.

Let’s first look at the patch number. When you have a patch release, like going from version 1.0.1 to version 1.0.2, this indicates that there have been bug fixes or trivial updates. These are not major alterations to your application’s functionality.

A minor release, like going from version 1.0.2 to 1.1.0, indicates that there’s been a change to functionality. This can be a functionality change, adding completely new functionality, or removing existing functionality. 

A major release, like going from version 1.1.0 to version 2.0.0, indicates changes that might break backward compatibility or significantly alter functionality. This could be everything from removing deprecated code to introducing a complete re-architecture of your application. Major version numbers tell people that there have been major changes and some things may no longer work as they did on the old version, or that there are new steps that need to be followed to get the same functionality. 

语义版本一旦确定，就应该被认为是一成不变的。一旦发布，该版本的代码就不能更改。一旦你用一个语义版本标记了你的代码库的一个版本，你就不应该改变那个版本指向的代码状态。如果你已经发布了 1.3.0 版本，一个次要版本，并且你意识到你错过了一些东西，你不能随意地把它添加到你的 [Git 库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)中，然后重新标记这个提交。如果您已经发布了一个需要更新的版本，那么您将不得不创建一个新的发布，用一个新的语义版本号来完成。

在最坏的情况下，如果您发布了应用程序的 2.0.0 版本，然后意识到您忘记向 repo 中添加一堆文件，您将不得不创建一个新的主要版本，立即从 2.0.0 版本升级到 3.0.0 版本。这对任何人来说都不好玩。在你的项目中使用语义版本化意味着在你发布你的代码之前要负责仔细检查你的版本号。一旦您的代码被发布到生产环境中，您必须假设有人正在使用该代码，并且您不应该更改版本号。对于公开发布的代码来说尤其如此，比如 npm 包。

A semantic version, once set, should be considered set in stone. Once released, that version of the code cannot be changed. Once you’ve marked a version of your codebase with a semantic version, you are not supposed to change which state of your code that version points to. If you’ve released version 1.3.0, a minor release, and you realize you missed something, you can’t just arbitrarily add it to your [Git repository](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository) and then re-tag that commit. If you’ve released a version that needs to be updated, you will have to create a new release, complete with a new semantic version number.

**添加 Git 标签**

因为语义版本是一成不变的，这使得它们非常适合与 Git 标签结合使用。Git 标签是一种给 [Git 提交](https://www.gitkraken.com/learn/git/tutorials/what-is-git-commit)添加标记的方式，以表明这是一个有意义的提交。

## 有两种不同类型的 Git 标签。

首先是轻量级标签。轻量级标签基本上只是指向提交的命名指针。这是一个人类可读的名称，可以分配给 Git 提交散列。要在 [Git CLI](https://www.gitkraken.com/cli) 中生成一个轻量级标记，您可以使用`git tag`命令，后跟标记名，然后可能是您希望标记应用到的提交的散列。如果您只想让标记引用当前提交，可以省略提交散列。

Because semantic versions are set in stone, this makes them perfect for combining with Git tags. A Git tag is a way to add a marker to a [Git commit](https://www.gitkraken.com/learn/git/tutorials/what-is-git-commit) to signify that it’s a meaningful commit in some way. 

There are two different types of Git tags.

First are lightweight tags. Lightweight tags are basically just named pointers to a commit. It’s a human-readable name that you can assign to a Git commit hash. To generate a lightweight tag in the [Git CLI](https://www.gitkraken.com/cli), you use the `git tag` command followed by the tag name, and then potentially the hash of the commit you want that tag applied to. If you only want the tag to reference the current commit, you can omit the commit hash.

在 [GitKraken](https://www.gitkraken.com/git-client) 中，您可以简单地右键单击中心图中的任何提交，然后单击`Create tag here`到[分配一个 Git 标签](https://support.gitkraken.com/working-with-repositories/tags/)。其他类型的 Git 标签被称为注释标签。这些是推荐在您的项目中使用的标记类型，因为它们除了是与 Git 提交散列相关联的人类可读名称之外，还是 Git 数据库中的完整对象。在不深入了解数据库 Git 结构的情况下,“完整对象”意味着对象可以被[校验和](https://en.wikipedia.org/wiki/Checksum#:~:text=A%20checksum%20is%20a%20small,upon%20to%20verify%20data%20authenticity.)。

带注释的 Git 标签也是推荐使用的标签类型，因为它们包含关于提交的附加信息，比如谁创建了标签，何时创建的，以及除了提交消息之外还提供标签消息。对于公共项目，可以使用像 [PGP](https://en.wikipedia.org/wiki/Pretty_Good_Privacy) 或 [GPG](https://gnupg.org/) 这样的系统对它们进行签名和验证。

使用`git tag`命令可以创建一个轻量级的标签，但是添加`-a`标志会将它表示为一个带注释的标签。添加`-m`标志将提供某种注释消息，以及标签所应用到的提交的散列。

Annotated Git tags are also the recommended type of tags to use as they contain additional information about the commit, like who created the tag, when they created it, as well as providing tag messages in addition to the commit message. For public projects, they can be signed and verified using systems like [PGP](https://en.wikipedia.org/wiki/Pretty_Good_Privacy) or [GPG](https://gnupg.org/). 

Using the `git tag` command can be used to make a lightweight tag, but appending the `-a` flag will denote it as an annotated tag. Adding the `-m` flag will provide some sort of annotation message, and the hash for the commit you’re applying the tag to. 

在 GitKraken 中，同样像轻量级标签一样，您所需要做的就是右键单击中央图中的特定提交，然后选择`Create annotated tag here`选项。GitKraken 会指导你完成剩下的过程。

In GitKraken, again like lightweight tags, all you need to do is right-click on a particular commit from the central graph and select the `Create annotated tag here` option.  GitKraken will guide you through the rest of the process.

无论您喜欢 GUI 提供的视觉效果，还是 CLI 的控制，GitKraken 都能满足您的需求。

**带注释的 Git 标签+语义版本化**

使用语义版本号命名带注释的标签有很大的价值。这种实践生成语义发布，有时称为语义版本构建号，它允许您在 Git 存储库中用特定的版本号标记提交。如果您更改了主版本号、次版本号或补丁版本号，那么您就表明了自上一个 Git 标记以来的更改级别。这是一个非常有用的方法来展示你的代码库的变化水平。

## 在 Git 存储库中使用语义发布可以让您更容易地回顾您的历史，并跟踪您的代码库是如何以及何时被修改的。这对你很有帮助，对你的团队和潜在的用户也很有帮助，尤其是在查看你的发布说明的时候。

许多产品界面都支持对 Git 标签使用语义版本控制。例如，GitKraken 支持本地工作的语义版本控制，GitHub 支持在线存储库的语义版本控制。许多系统都很容易暴露版本号或标签接口。这些界面可以快速帮助您找到哪些版本是补丁更新，哪些版本包含主要功能更改。

Using semantic releases in a Git repository allows you to more easily review your history and track how and when changes were made in your codebase. This is really helpful to you, but also to your team and potentially your users, especially when viewing your release notes.

The use of semantic versioning for your Git tags is supported by many product interfaces. For example, semantic versioning is supported by GitKraken for working locally, and GitHub for online repositories. Many systems make it easy to expose a release number or tag interface. These interfaces can quickly help you find which releases were patch updates and which ones included major functionality changes.

**自动化的语义发布**

使用语义版本号来命名您的注释标签也有助于自动化您的过程。当使用自动化工具进行生产部署时，例如 [CircleCI、](https://circleci.com/)，您可以在`config.yaml`中指明只应在某些标签上执行作业。如果您使用语义版本控制，您可以放入一个非常简单的[正则表达式](https://en.wikipedia.org/wiki/Regular_expression)来跟踪这种变化，并且只在匹配语义版本结构的标签上触发动作。您也可以在其他系统中这样做，如 [Bitbucket 管道](https://bitbucket.org/product/features/pipelines)、 [Jenkins](https://www.jenkins.io/) 或 [Travis CI](https://travis-ci.org/) 。

## 观看 GitKon speaker 的一个剪辑，Michael Miles 通过终端和使用 GitKraken 添加带注释的标签并将标签推送到 GitHub repo 来完成创建语义发布的[过程。](https://youtu.be/4wPjo5C-v8Y?t=794)

Using ​​semantic version build numbers for naming your annotated tags also helps with automating your process. When using automation tools for your production deployments, such as [CircleCI,](https://circleci.com/) you can indicate in your `config.yaml` that a job should only be executed on certain tags. If you use semantic versioning, you can put in a very simple [regular expression](https://en.wikipedia.org/wiki/Regular_expression) to track that change and trigger actions only on tags matching the structure of a semantic version.  You can do this in other systems like [Bitbucket Pipelines](https://bitbucket.org/product/features/pipelines), [Jenkins](https://www.jenkins.io/), or [Travis CI](https://travis-ci.org/) as well. 

有了 GitKraken 强大的搜索和过滤功能，您再也不会丢失标签。此外，您只需点击两下就可以创建新标签。💯

**语义版本和发布说明**

虽然语义发布对于从事项目的内部团队非常有用，但是如果您打算在发布说明中对外公开这些发布号，仍然有一些额外的考虑。有多种方法可以创建发行说明——大多数软件发行版都会推出附带的文档——但在每种情况下，目标都是提供不仅仅是“代码被更新了”的信息如果您只是在这些发行说明中公开语义版本构建号，这可以提供一定程度的更改，但是它不提供任何有关更改的信息。虽然对于补丁发布来说，您可能能够做到这一点，但对于次要或主要的更新来说，这是不够的。

## 微小的变化，或者引入新功能的任何变化，至少需要公开注释消息并提供使用细节。理想情况下，这些细节可以包含在注释消息中。

主要版本，或者任何需要人们进行某种升级的版本，应该让人们了解他们需要采取的任何行动。

Minor changes, or any change that introduces new functionality, needs, at a very minimum, to expose the annotation messages and provide usage details. Ideally, these details could be included in the annotation message. 

Major releases, or any release that requires people to do some sort of upgrade, should give people an understanding of any actions that they will need to take as a result.  

对于完全私有的存储库和项目，详细的发行说明可能没有保证。然而，随着时间的推移，让内部团队了解变化仍然很重要。至少，生成两次发布之间发生的提交消息的列表可以作为注释标签的消息。这可以节省大量时间来跟踪何时引入了某些更改，即使您正在用 GitKraken 可视化您的 Git 历史。

**用语义版本和 Git 标签让生活更美好**

观看一段视频，其中有一些关于如何使用语义版本和 Git 标签做更多事情的建议。

将语义版本化和 Git 标签结合到你的项目中有很多好处，无论是对你的开发团队还是阅读发布说明的最终用户。基于语义发布方法的标准化允许您更好地自动化您的过程，并以未来开发人员肯定会欣赏的方式公开变更。GitKraken 可以让您在创建和查看标签的过程中更加轻松，从而让您的生活更加美好。立即下载 GitKraken [**Git 客户端**](https://www.gitkraken.com/git-client) ，它包括一个 GUI 和 [**CLI**](https://www.gitkraken.com/cli) ，并开始语义发布之路。

## **Making Your Life Better with Semantic Versioning and Git Tags**

Watch a clip with some ‘extra credit’ advice on how to do even [more with semantic versioning and Git Tags](https://youtu.be/4wPjo5C-v8Y?t=1196).

There are a lot of advantages to combining semantic versioning and Git tagging for your projects, both for your dev team and for your end users reading the release notes. Standardizing on a semantic release approach allows you to better automate your processes and exposes changes in a way that future developers will certainly appreciate. GitKraken can help make your life even better by making tagging a lot less painless, both in creating and viewing your tags.  Download the GitKraken [**Git client**](https://www.gitkraken.com/git-client) today, which includes a GUI and [**CLI**](https://www.gitkraken.com/cli), and get started down the path to semantic releases.