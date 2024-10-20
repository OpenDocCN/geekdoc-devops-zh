# 吉拉数据中心版本的 Git 集成:吉拉自动化

> 原文：<https://www.gitkraken.com/blog/automation-for-jira-git-integration-for-jira-data-center>

吉拉数据中心的 Git 集成在上一个季度有了一些重大更新，包括新的吉拉支持自动化:让您对存储库触发操作有更多的控制和权力。

让我们深入了解一下针对吉拉数据中心的 [Git 集成的新特性和改进:](https://marketplace.atlassian.com/apps/4984/git-integration-for-jira?tab=overview&hosting=datacenter)

## **吉拉触发器的自动化**

希望在用户创建拉取请求时自动创建吉拉问题？或者当一个拉取请求被合并时触发一封电子邮件呢？安装了吉拉 Git 集成和吉拉自动化应用程序的用户现在可以在吉拉自动完成所有这些操作。

当管理员导航到项目自动化并创建规则时，以下 DevOps automation for 吉拉触发器现在可用:

*   分支已创建
*   提交已创建
*   已创建拉式请求
*   拒绝拉取请求
*   已合并拉取请求

在选择吉拉触发条件的自动化后，用户可以选择以下自动发生的问题操作之一:

分配问题:选择要向其分配问题的用户

克隆问题

*   对问题的评论
*   创建问题
*   创建子任务
*   删除评论
*   删除问题
*   删除问题链接
*   编辑问题
*   链接问题
*   为了在吉拉的 Git 集成中利用吉拉的自动化，需要来自 Atlassian 的吉拉应用程序的付费[自动化。](https://marketplace.atlassian.com/apps/1215460/automation-for-jira-data-center-and-server?hosting=datacenter&tab=overview) [Atlassian 已经宣布](https://community.atlassian.com/t5/Data-Center-articles/Automation-for-Jira-will-be-included-in-Jira-Data-Center/ba-p/1995867)他们将在 2022 年吉拉 9 出货时为吉拉数据中心免费提供该应用。吉拉的 Automation 服务器销售也将在那时结束，因此如果您想将吉拉的 Automation 用于吉拉服务器，您需要在销售结束前购买吉拉的 Automation 应用程序。
*   链接问题

**吉拉用例的自动化**

对于吉拉触发器组合的所有可能的自动化，这里有一些常见的使用案例供考虑:

**用例**

## **触发**

**发布动作**

| 创建提交时，将吉拉问题状态更新为“进行中” | 提交已创建 | 编辑问题 |
| 创建拉式请求时创建吉拉问题。 | 已创建拉式请求 | 创建问题 |
| 创建分支机构时，将吉拉问题状态更新为“进行中” | 分支已创建 | 编辑问题 |
| 当请求合并被拒绝时，关闭吉拉问题。 | 拒绝拉取请求 | 编辑问题 |
| **Git 存储库面包屑** | 执行远程操作时，存储库名称现在将显示远程所有者或组的名称。这个面包屑应该帮助用户更好地识别要采取行动的正确回购协议，特别是对于拥有主存储库分支的团队。 | 编辑问题 |

**亚特兰蒂斯数据中心安全&性能评估**

## 每年，Git Integration for 吉拉团队都会进行一整套数据中心就绪性测试，以满足 Atlassian 市场对数据中心应用程序的性能和规模测试要求。这包括 Atlassian 数据中心安全审查和 Atlassian 性能审查。

代码分析扫描器是任何应用程序安全程序的重要组成部分，因为它们为开发人员提供了一种低成本的选择，可以在常见错误到达最终用户之前识别它们。

我们自豪地报告，我们的测试结果得到了 Atlassian 的认可，证实我们已经满足了以下要求:

对其容量相当于数据中心客户的主机执行性能测试。测试环境是影响测试结果的关键点，它显示了对软件本身的性能影响。

## 使用预加载到主机的大型数据集运行测试。基础数据集必须模仿[数据中心应用性能工具包](https://github.com/atlassian/dc-app-performance-toolkit)附带的数据集。

代码分析扫描器是任何应用程序安全程序的重要组成部分，因为它们为开发人员提供了一种低成本的选择，可以在常见错误到达最终用户之前识别它们。

**为 GitHub 吉拉集成实施 Git 服务许可**

使用吉拉 GitHub 集成的吉拉管理员现在可能要求吉拉用户提供 GitHub 个人访问令牌(PAT)来验证存储库权限。这使得管理员能够查看 Git Integration for 吉拉应用程序提供的所有开发信息。

1.  启用安全模式后，用户需要吉拉 GitHub 权限才能查看:
2.  仓库

承诺

## 分支

拉取请求

标签

*   `GitHub Permission Enforced`标签表示启用了安全模式，并且为当前用户配置了有效的 PAT。
*   分支
*   拉取请求
*   即将推出:支持 GitLab、Azure DevOps 和 AWS CodeCommit 上的回购。
*   即将推出:支持 GitLab、Azure DevOps 和 AWS CodeCommit 上的回购。

**吉拉 Git 集成的更多改进**

通过为节点编制索引和 SSH 配置提供大量新选项，吉拉数据中心 Git Integration 中的集成管理变得更好。

通过为节点编制索引和 SSH 配置提供大量新选项，吉拉数据中心 Git Integration 中的集成管理变得更好。

**吉拉数据中心专用索引节点**

用户现在可以指定数据中心节点来为吉拉索引作业执行 Git 集成。要访问此选项，请导航至常规设置菜单。

**吉拉 Git 集成的更多改进**

## 通过为节点编制索引和 SSH 配置提供大量新选项，吉拉数据中心 Git Integration 中的集成管理变得更好。

**支持 SSH 配置文件**

对于通过 SSH 使用存储库进行身份验证的用户，吉拉数据中心的 Git 集成现在支持 [SSH 配置文件](https://linuxize.com/post/using-the-ssh-config-file/)，以便更好地为您连接的每台远程机器存储不同的 SSH 选项。

## 用户现在可以指定数据中心节点来为吉拉索引作业执行 Git 集成。要访问此选项，请导航至常规设置菜单。

**更新吉拉数据中心的 Git 集成，享受此次发布**

要访问所有这些新特性和更多特性，请将您的 [Git Integration for 吉拉数据中心](https://bigbrassband.atlassian.net/wiki/spaces/GIJDC/pages/1930395997/Installation+and+updating)实例更新到最新版本！

**支持 SSH 配置文件**

## 对于通过 SSH 使用存储库进行身份验证的用户，吉拉数据中心的 Git 集成现在支持 [SSH 配置文件](https://linuxize.com/post/using-the-ssh-config-file/)，以便更好地为您连接的每台远程机器存储不同的 SSH 选项。

For users authenticating with repositories over SSH, Git Integration for Jira Data Center now supports the [SSH Config File](https://linuxize.com/post/using-the-ssh-config-file/) to better store different SSH options for each remote machine you connect to.

**更新吉拉数据中心的 Git 集成，享受此次发布**

## 要访问所有这些新特性和更多特性，请将您的 [Git Integration for 吉拉数据中心](https://bigbrassband.atlassian.net/wiki/spaces/GIJDC/pages/1930395997/Installation+and+updating)实例更新到最新版本！

To access all these new features and more, update your [Git Integration for Jira Data Center](https://bigbrassband.atlassian.net/wiki/spaces/GIJDC/pages/1930395997/Installation+and+updating) instance to the latest version!