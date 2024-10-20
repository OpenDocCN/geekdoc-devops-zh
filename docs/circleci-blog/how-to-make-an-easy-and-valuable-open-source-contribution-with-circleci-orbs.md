# 如何用 CircleCI 做开源贡献

> 原文：<https://circleci.com/blog/how-to-make-an-easy-and-valuable-open-source-contribution-with-circleci-orbs/>

## 或者，作为一名支持工程师，我是如何编写我们迄今为止最常用的 orb 的(你也可以这样做)

### 开源经验

如果你是一个新的开发者，我在这里鼓励你考虑写一个 orb。orb 是在你的简历中添加一些开源经验的好方法，同时展示了软件工程角色寻找的大部分技能。

*   开源开发经验
*   具有持续集成和持续部署的经验
*   Git 和 Bash 体验

用一个简单的 orb 实现所有这些要点真的很容易，即使它只是将消息发布到一个 Slack 通道。事实上，这就是我所做的，也是我将在这篇博文中讨论的。

### 我是谁？

嘿，我是凯尔，CircleCI 的支持工程师。如果您以前联系过支持部门，我们可能已经见过面了。我也尝试在邮件系统之外提供支持，写支持中心的文章，创作视频，现在还有 Orbs！我想我们也可以添加博客帖子。

我有机会与我们的许多用户交谈，并观察你们所有人提出的简化开发的创造性解决方案，我试图利用这些观察，向你们提出可能的最佳解决方案，或者有时我们应该对我们的团队做出的改变。

### 一些关于球体的背景

几个月前，我们从产品团队那里得知，我们将很快发布" [Orbs](https://circleci.com/blog/announcing-orbs-technology-partner-program/) "，一个 CircleCI 配置文件的包管理器。例如，您可以使用 orbs 轻松配置 AWS S3 上传，而无需担心安装 AWS CLI 或创建 AWS 配置文件。有了这样一个 orb，你根本不需要担心 AWS 这边，也不应该有！你的重点是你的项目；你希望 S3 上传不需要维护就能正常工作。对于球体来说，几乎所有东西都是如此。

比如上传到 S3，就是这么简单:

导入球体

```
orbs:
 	 aws-s3: circleci/aws-s3@volatile 
```

然后，您可以随时在任何作业中分步运行同步命令

```
- aws-s3/sync:
  from: bucket
  to: 's3://my-s3-bucket-name/prefix'
  overwrite: true 
```

就是这样！所有的 Orb 都是开源的，所以你可以在我们新的 Orb 注册页面上看到它是如何制作的，并阅读如何使用它的文档:[https://circleci.com/developer/orbs/orb/circleci/aws-s3](https://circleci.com/developer/orbs/orb/circleci/aws-s3)

使用 orbs，任何人都可以非常容易地快速实现大量的集成、工具、实用程序、特性或任何您能想到的东西，并共享它们。

作为一名支持工程师，这可能会也可能不会让我超级开心！没有什么比拥有一个标准化的解决方案来呈现给每个人更好的了，而且这个解决方案很容易作为一个单一的包来维护。

### 第一步:解决一个简单的问题

在 orb 向公众推出之前，产品团队的任务是为发行版开发一组初始的 orb，他们邀请其他团队参与到这个过程中来，并贡献 orb。因为我是回答你关于球体问题的人之一，所以我借此机会创造了一个我认为对很多人有用的球体。

由于我直接与用户交谈，我知道你们都希望对 Slack 通知有更多的控制:何时发送，发送什么，谁被警告，甚至可能是消息的颜色。所以我决定创造一个球体来做到这一点。创建 orb 的过程非常简单，这向我展示了做一个很多人都会使用和改进的开源贡献是多么容易。事实上，我写的 Slack orb 现在是我们迄今为止使用最多的 orb。

> “当我开始的时候，我不知道这个项目会发展到什么程度，会被社区所塑造。”

### 看着社区成长

自从我写了 Slack orb 之后，我收到了很多像您这样的用户的请求，他们希望在自己的项目中使用和改进 Slack orb。最初本质上是一个 CURL 请求的包装器，现在已经发展成为 CI 和 Slack 之间的一个健壮的、由人对人的接口，具有一些高级功能，比如在部署等待您的批准时能够接收丰富的通知。当我开始的时候，我不知道这个项目会发展到什么程度，会被社区所塑造。

### 看着松弛的球体

让我们来看看 Slack orb，它是做什么的，以及它是如何做到的。您可以将这里的原则应用于任何工具或 CLI。

如果你想跟随源代码或者只是自己检查一下 Slack orb，你可以访问 [Orb 注册表](https://circleci.com/developer/orbs/orb/circleci/slack)。

### 它的作用

虽然 orb 自创建以来已经有所发展，但最初的 Slack orb 只有两个命令，“notify”和“status”:

让我们来看看[在`notify`指挥下的](https://github.com/CircleCI-Public/slack-orb)。

```
 jobs:
  alertme:
    docker:
      - image: circleci/node
    steps:
      - slack/notify:
            message: "This is a custom message notification"
            #Enter your own message
            mentions: "USERID1,USERID2"
            #Enter the Slack IDs of any users who should be alerted to this message.
            color: "#42e2f4"
            #Assign custom colors for each notification
            webhook: "webhook"
            #Enter a specific webhook here or the default will use $SLACK_WEBHOOK 
```

`notify`允许您在任何工作中轻松创建自定义时差通知。这允许您以多种方式自由发送通知。

首先，您可以设置几个参数，比如消息文本、要@提及的任何用户，甚至是显示消息的盒子的颜色。

您还拥有基于作业有条件运行通知的固有优势，这意味着您只需将`notify`命令添加到您的部署作业中，并选择向团队成员发出警报。

> 最令人兴奋的是，球体的灵活性意味着你可能会想出这个球体的创造性用途，这是我从未考虑过的。

如果你想了解更多关于 Slack orb 的信息，可以看看 orb 注册表上的[页面。想贡献和改善懈怠宝珠？是开源的！给我们发一份公关，我们会看看。](https://circleci.com/developer/orbs/orb/circleci/slack)

### 我是如何建造我的球体的(当然你也可以)

这个项目开始于我的个人 Github，后来当它准备好发布时，被转移到了 CircleCI 官方组织。如果您正在寻找任何灵感或证据来证明没有人是完美的(甚至是接近完美的)，您可以查看 git 提交历史来查看与实现这一点相关的所有试验和错误。老实说，我感觉至少有 100 个提交在引号、双引号和转义字符中挣扎。

![SlackOrbCommitHistory.png](img/a95badd0830706e870676fcdfa64f56d.png)

你可以在 CircleCI 的新主页上通过更新和拉取请求看到它今天的样子:[https://github.com/CircleCI-Public/slack-orb](https://github.com/CircleCI-Public/slack-orb)

### 现在轮到你了:为什么要创作一个球体？

为什么要为您的开源项目选择 orb？因为它很容易上手，并且提供了很多重要开发技能的经验，而且由于它是开源的，CircleCI 将免费构建它！

**加入开源社区:**我们最近开始收到关于 Slack orb repo 的第一个“问题”报告，仅仅是知道开发人员正在使用您的代码就感觉棒极了。

自从创建 Slack orb 以来，我们已经收到了几个 pull 请求，通过发布补丁或新功能对几个问题做出了响应，并使用工作流和 CLI 构建了一个更强大的开发管道来实现自动部署

**现实世界技能:**创建一个 orb 或改进一个现有的 orb 是一种非常简单的方式，既能参与开源社区，又能获得雇主所寻求的现实世界经验。

orb 本质上利用了 Git 版本控制、CI/CD 和 shell 脚本，以及在这个过程中可能与之交互的任何其他 CLI 或工具。

使用`CURL`编写一些向 Slack 通道发布消息的东西，包含了成为专业开发人员所需的几乎所有经验。如果你想进入这个领域，我想不出比这更好的开始方式了。

在我的下一篇文章中，我将分享我的最佳技巧和创建球体时需要考虑的事情。准备好了吗？[探索宝珠](https://circleci.com/orbs/)。