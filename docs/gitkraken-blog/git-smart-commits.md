# 将 Git 提交与智能提交的吉拉问题联系起来

> 原文：<https://www.gitkraken.com/jira/git-smart-commits>

智能提交允许您将 [Git 提交](https://www.gitkraken.com/learn/git/commit)链接到吉拉问题。通过在提交消息中使用特定的语法，开发人员可以利用智能提交来执行额外的操作，如向吉拉问题添加注释、转换问题等。最终，智能提交帮助开发人员避免在他们的开发工具和吉拉之间切换上下文，这为他们提供了更多的时间来编写代码。

### 了解如何使用吉拉智能提交

[智能提交先决条件](https://www.gitkraken.com/jira/git-smart-commits#prerequisites)

[为吉拉问题添加评论](https://www.gitkraken.com/jira/git-smart-commits#comment)

[记录时间跟踪](https://www.gitkraken.com/jira/git-smart-commits#time)

[过渡一期吉拉](https://www.gitkraken.com/jira/git-smart-commits#transition)

[智能提交的好处](https://www.gitkraken.com/jira/git-smart-commits#benefits)

[https://www.youtube.com/embed/IkDxc9UZjag?feature=oembed](https://www.youtube.com/embed/IkDxc9UZjag?feature=oembed)

视频

## 吉拉智能提交先决条件

在开始利用智能提交的力量之前，您需要[为吉拉](https://www.gitkraken.com/git-integration-for-jira/try-free)安装 Git 集成。如果您从未在吉拉使用过 Git 集成，可以选择开始 30 天的免费试用。为了集成正常工作，请确保您的 [Git 配置](https://www.gitkraken.com/learn/git/git-config)中的电子邮件与您的吉拉个人资料中使用的电子邮件相匹配。

编写智能提交的能力只是 Git 集成为吉拉提供的众多好处之一。该工具使开发人员可以轻松地使用 Git 驱动吉拉，消除不必要的上下文切换，并让关键利益相关者了解公司目标的状态。[了解有关吉拉 Git 集成的更多信息](https://www.gitkraken.com/git-integration-for-jira/features)。

一旦安装了吉拉 Git Integration，使用下面的步骤连接您想要的存储库。

1.  从您的吉拉实例中，选择顶部导航栏中的`Apps`
2.  从随后的下拉菜单中，选择`Git Integration: Manage integrations`
3.  从页面中央选择`Add integration`按钮
4.  选择`Git service integration (Recommended)`
5.  选择一个存储库托管服务，如 GitHub、GitLab、Bitbucket 等。取决于您的首选服务

一旦您在吉拉和您选择的存储库托管服务之间建立了连接，您就可以选择想要连接的单个存储库。

要编写智能提交，请以关联的吉拉问题密钥开始提交消息，后跟`#`，然后是针对该问题要采取的操作的命令。

**基本智能提交语法示例**

`<ISSUE_KEY> <ignored text> #<command> <optional command_params>`

智能提交可用于直接从 Git 提交消息向吉拉问题添加注释。这可以提供关于正在进行的代码更改的附加上下文或信息。要做到这一点，开发人员只需要在他们的提交消息中包含一个特定的语法，其中包括吉拉问题编号和他们想要添加的注释。

**添加注释示例**

`ISSUE_KEY #comment text`

例如，开发人员可以编写这样的提交消息:“JIRA-123 #注释这是一个测试注释。”当提交被推送到 Git 存储库时，注释将被自动添加到指定的吉拉问题中。这有助于简化跟踪和管理问题的过程，使团队成员更容易了解和掌握项目的最新进展。

## 吉拉智能提交在吉拉问题上创下时间记录

智能提交还可以用于记录吉拉问题的时间跟踪信息。这对于跟踪在不同任务上花费了多少时间很有用，并且可以帮助团队更好地管理他们的时间和资源。要做到这一点，开发人员只需在 Git 提交消息中包含特定的语法，其中包括吉拉问题编号和他们想要记录的时间量。

**记录时间跟踪信息示例**

`ISSUE_KEY #time [in Jira time syntax]`

**吉拉管理员必须启用此功能，此操作才能生效*

例如，开发人员可以编写这样的提交消息:“JIRA-123 #时间 1 小时 30 分钟这是一个测试时间条目。”当提交被推送到 Git 存储库时，时间跟踪信息将被自动添加到指定的吉拉问题中。这确保了时间跟踪数据是准确的和最新的，使团队更容易对未来做出准确的预测。

## 通过吉拉智能提交解决吉拉问题

智能提交还可以用于将吉拉问题从一种状态转换到另一种状态。这对于在进行代码更改时自动在工作流或流程中移动问题非常有用。要做到这一点，开发人员只需在 Git 提交消息中包含一个特定的语法，其中包括吉拉问题编号和所需的转换。

**过渡问题示例**

`ISSUE_KEY #jira workflow status`

**您必须拥有必要的吉拉权限来转换问题，此操作才能生效*

例如，开发人员可以编写这样的提交消息:“JIRA-123 #进行中。”当提交被推送到 Git 存储库时，指定的吉拉问题将自动转换到工作流的“进行中”阶段。这使得团队更容易保持组织，并了解项目中各种问题的状态。

## 吉拉智能提交的优势

智能提交有助于开发人员消除重复的任务，如复制提交消息信息，从而加快他们的工作流程。

利用智能提交的开发团队能够保持提交消息的组织性和一致性，从而更容易审查和理解已经做出的代码更改。

最终，通过 [Git Integration for 吉拉](https://marketplace.atlassian.com/plugins/com.xiplink.jira.git.jira_git_plugin/cloud/overview?utm_source=gitkraken.com&utm_medium=blog&utm_campaign=learn%20jira&utm_content=jira%20github%20integration&utm_term=)的智能提交有助于改善开发团队的协作和组织，使每个人更容易保持一致并有效地合作。