# 宣布 GitKraken 组曲

> 原文：<https://www.gitkraken.com/blog/announcing-gitkraken-suite>

我们一直专注于创造工具，使开发团队更有生产力。在 2014 年，我们推出了 GitKraken，这是一个传奇的跨平台 Git GUI:一个直观的工具，以某种方式可视化 Git 引擎下正在发生的事情，使版本控制对个人开发人员来说不那么可怕，对组织来说更具可扩展性。

接下来是 Glo Boards，一个面向项目经理和开发人员的看板工具。这个工具已经发展到包括一些重要的特性，比如双向 GitHub 问题同步、工作流自动化、Slack 集成，以及其他许多改善团队间沟通和协作的特性。

组织在开发团队和部门之间交流高层项目目标和里程碑所需的工具集仍然缺少一样东西。进入 GitKraken 时间轴，这是同类工具中的第一个，旨在以连续的线条表示时间来显示主要项目里程碑。该工具允许产品团队将多个时间表重叠在一起，以避免规划期间的能力冲突，并便于在公司范围内共享发布时间表，以便从营销到客户支持的每个人都知道即将发生的事情。

当我们开始在我们自己的组织中一前一后地使用这些工具时，我们亲身体验了开发团队和软件公司如何通过一起使用这一套工具来更有效地交流、更好地协作以及总体上更有生产力。

### **介绍 GitKraken 套件**

[https://www.youtube.com/embed/5E7MO9OiAxU?feature=oembed](https://www.youtube.com/embed/5E7MO9OiAxU?feature=oembed)

视频

## **新产品名称**

随着软件套件的引入，自然的演变是更新我们的产品名称以提供清晰性。

我们的 Git GUI，以前叫做 GitKraken，现在是 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 。

我们的问题和任务跟踪工具，以前叫做 Glo Boards，现在叫做 [GitKraken Boards](https://www.gitkraken.com/boards) 。

我们的在线时间线制作者将继续被称为 [GitKraken 时间线](https://www.gitkraken.com/timelines)。

## **一套规划工具&编码**

可以说 DevOps 生命周期中最重要的两个阶段是计划和代码。GitKraken 套件为您提供了在一个生态系统中连接这两个阶段所需的一切。

### **项目管理&问题跟踪**

不管您的开发团队是否实践 Scrum、看板或混合敏捷方法，项目管理和问题跟踪都是开发运维计划阶段的基础。GitKraken Boards 是一个任务和问题跟踪系统，允许开发团队在看板、日历、仪表板或时间线中可视化任务。

[GitKraken Boards 直接与 GitHub](https://support.gitkraken.com/boards/integrations/github-sync/) 集成，以减少开发团队完成任务并朝着里程碑前进时的上下文切换。GitHub 问题和里程碑的双向同步功能使项目经理能够在开发人员更新卡状态后立即了解当前的项目进度。

高水平的开发人员依靠自动化他们的工作流程来提高生产力，这就是为什么我们让 GitKraken 板的工作流程自动化变得如此容易。使用 [GitHub Actions](https://support.gitkraken.com/boards/integrations/github-actions/) 或内置的列自动化来消除重复的过程，例如在工作流列中移动卡片、更新标签、分配用户、添加相对到期日等。

此外，[将卡链接到拉取请求](https://support.gitkraken.com/boards/integrations/github-pr/)提供了进一步的自动化。当在 GitHub 中更新拉取请求状态时，你板上的卡片将根据你选择的映射自动前进到另一列。

GitKraken Boards 与 Git kraken Git GUI 完全集成，因此开发人员可以直接从他们的编码环境中查看、编辑和创建新问题，或者创建与问题相关的分支，并立即在 GitKraken Boards 中看到它们。

对于全球 60%使用 Slack 作为主要沟通方式的软件团队来说，GitKraken Boards 的 [Slack 集成让您的团队能够预览卡片、根据 Slack 消息创建新卡、更新卡片受托人、标签和列，而无需离开 Slack。此外，当有人在 GitKraken 论坛上提到他们时，可以设置 Slack 通知来提醒团队成员。](https://support.gitkraken.com/boards/integrations/slack/)

这些自动化功能和集成非常符合 DevOps 战略，可减少上下文切换并提高效率。

GitKraken Timelines 增加了一个规划层，这是目前大多数团队的[项目管理工具](https://buyersguide.org/project-management/t/best)所缺少的。它允许团队在更高的层次上计划和交流项目目标和里程碑。它允许项目经理只传达最重要的特性或目标，而不是看到几十个甚至几百个日常项目任务。

在会议中，您可以使用演示模式将注意力集中在每个里程碑标志上。

时间线上的每个里程碑都可以有一个图像或 gif 以及相关的子项。很容易将多个时间线叠加起来，以比较项目或团队的截止日期。随着项目的进展，使用自动转换日期功能来调整一个截止日期，所有其他截止日期都会相应地自动调整。

开发人员可以将 GitKraken 时间表中的里程碑链接到 GitKraken 板上的单个任务卡，或者来自 GitKraken Git GUI 的 pull 请求。GitKraken 时间轴可以直接从 GitKraken Git GUI 或在浏览器中访问。

**编码**

### 版本控制——特别是 Git 是开发团队工作的基础。为了使用 Git 进行项目协作，您需要为您的存储库提供托管服务；尽管您可以选择将它们托管在内部服务器上，以更好地满足您组织的安全或部署需求。不管您的存储库托管在哪里，您都会想要一个 Git 客户机，比如 GitKraken Git GUI。

[连续四年被选为第一开发工具](/reports/top-developer-tools-2020/)，GitKraken Git GUI 是 GitKraken 工具套件中的旗舰产品。它允许开发人员在一个彩色的图形中可视化他们的 Git 库的历史，并且它将复杂的 Git 命令简化为拖放动作。GitKraken Git GUI 具有内置的合并冲突编辑器、交互式 rebase 模式、内置的代码编辑器、集成等等，为有经验的开发人员简化了 Git 工作流程，并减少了 Git 新手的陡峭学习曲线。

它通过紧密连接各种代码工具在 DevOps 工作流中发挥着关键作用:Git 客户端、托管服务和 IDE(它内置了来自 VS 代码的 Monaco 代码编辑器)。

GitKraken Git GUI 集成了所有顶级 Git 托管服务:GitHub、GitLab、Bitbucket 和 Azure DevOps 及其自托管产品(不包括 TFS ),以便在 GitKraken 内部直接实现以下功能:

在您的托管帐户上创建存储库，包括。git 忽略和许可

*   自动生成一个 SSH 密钥对并添加它
*   分叉仓库
*   将验证保存到配置文件中
*   从回购列表中克隆
*   添加用于回购的遥控器
*   创建带有添加的受分配者、审阅者和标签的提取请求
*   查看拉式请求的构建状态
*   查看拉式请求的构建状态

GitKraken Git GUI 通过连接计划和代码步骤支持无缝的 DevOps 工作流。GitKraken Git GUI 内置了 GitKraken 板和 GitKraken 时间表等规划工具。它与吉拉云/服务器和 GitKraken 板集成，因此您可以查看、过滤、创建、编辑和评论问题，甚至创建与问题相关的分支。

为了增强任务管理，GitKraken Git GUI 中的存储库可以与 GitKraken 电路板上的卡片相关联。创建新的 GitHub pull 请求时，只需链接一张卡；这将自动更新 GitHub 中的拉请求描述。

为了增强任务管理，GitKraken Git GUI 中的存储库可以与 GitKraken 电路板上的卡片相关联。创建新的 GitHub pull 请求时，只需链接一张卡；这将自动更新 GitHub 中的拉请求描述。

**立即入住 GitKraken 套房**

## 一个成功的 DevOps 工作流是关于实现工具和过程的，这些工具和过程通过自动化和集成来提高代码质量和最大化生产力。GitKraken 套件是一套免费工具，可以帮助您的组织左移并优化开发运维的规划和编码阶段。

如果您需要付费 GitKraken 计划提供的高级功能，我们现在提供仅 79 美元/用户/年的 [GitKraken Pro 套件](https://www.gitkraken.com/pricing)!投资于一套优先考虑用户体验、效率和互连性的工具，你将看到你的开发团队和组织的生产力提高。

如果您需要付费 GitKraken 计划提供的高级功能，我们现在提供仅 79 美元/用户/年的 [GitKraken Pro 套件](https://www.gitkraken.com/pricing)!投资于一套优先考虑用户体验、效率和互连性的工具，你将看到你的开发团队和组织的生产力提高。