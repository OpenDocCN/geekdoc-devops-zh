# 用 Git 驱动吉拉| Adam Wride -吉拉 Git 集成

> 原文：<https://www.gitkraken.com/gitkon/jira-git-adam-wride>

[https://www.youtube.com/embed/omigX57wMM4?feature=oembed](https://www.youtube.com/embed/omigX57wMM4?feature=oembed)

视频

先说会议。会议效率低是出了名的。他们打断你的一天，打断你的工作。最糟糕的是，会议经常被用来做错误的事情:检查项目状态。

吉拉和许多生产力和协作工具都有这样的抱怨:

"如果我花时间更新吉拉，为什么我们需要有这么多的状态会议？"

问得好。

如果你在开状态会议，那你就错了。会议是为了解决棘手的问题和进行讨论，而不是为了更新团队的进展。这就是吉拉的意义所在。

但通常，如果在相关的吉拉问题上没有取得进展，吉拉就不是最新的。这通常意味着项目经理需要抓住懈怠或者对这个问题发表评论。最坏的情况，他们会召开一个状态会议。

没人想开更多的会。问题是，我们的会议比以往任何时候都多，而且自疫情会议以来，情况变得更糟。突然转向远程工作让人们感到不知所措；人们报告说，他们花更多的时间开会，花更多的时间相互协调，并说他们现在花更多的时间向客户和经理汇报。

高效协作从未如此重要。生产率的圣杯是少开会，少被打扰。对于开发人员来说，解决方案的一部分是使用开发人员已经采取的步骤，例如 Git 中的日常操作，尽可能多地自动化吉拉状态更新。

什么是吉拉？

## Atlassian 的吉拉是项目管理软件的巨头。83%的财富 500 强企业都在使用 Atlassian 产品，目前有超过 100，000 家企业在使用 atlassian 产品，其采用率每年增长 30%。吉拉无处不在；如果您以前没有使用过它，您可能会在某个时候使用它。

大约一半的吉拉用户将它用于软件开发项目，但其中一半是非技术用户。团队中的每个人都在使用吉拉来完成他们的工作。团队使用吉拉是因为它的灵活性和众所周知的不同团队之间的良好协调。

About half of Jira users are using it for software development projects, but half of them are non-technical users. Everyone on the team is using Jira to get their jobs done. Teams use Jira because it’s flexible and is well known for good coordination between various groups. 

**用 Git 驾驶吉拉**

## 对于吉拉的快速定位，吉拉问题是吉拉内部有组织工作的原子位。每个吉拉问题都属于吉拉项目，每个项目都有一个关键字。随着吉拉问题的产生，它们被赋予了一个编号；项目密钥和发行号的组合就是吉拉发行密钥。

For a quick orientation of Jira, the Jira issue is the atomic bit of organized work inside of Jira. Every Jira issue belongs in the Jira project, and each project has a key. As Jira issues are created, they’re given a number; a combination of the project key and the issue number is the Jira issue key. 

吉拉问题密钥是用于吉拉的 [Git 集成](https://www.gitkraken.com/git-integration-for-jira)工具如何将 Git 提交、 [Git 分支](https://www.gitkraken.com/learn/git/branch)和 [Git 拉请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)与吉拉问题相关联。在吉拉中查看时，提交显示在问题的上下文中。

回到手头的问题:你能更新吉拉吗？开发人员不能直接跳到吉拉来更新状态、添加评论或到处更新自定义字段吗？嗯，开发人员经常觉得吉拉增加了太多的流程，花时间更新吉拉不是他们工作的一部分。

但更重要的是，在 Git 中工作的开发人员，也许有理由认为他们在 Git 中的更改包含了每个人都在寻找的状态更新。而另一方面，客户、经理、领导和其他关心开发项目进展的人希望查看吉拉，并快速了解他们的项目与计划相比的情况，以及是否有任何新的风险。他们经常在高层次上审查吉拉的进展，因此吉拉提供的结构化状态数据必须是最新的，这一点非常重要。

那么开发者和领导者之间的中间地带是什么呢？

So what’s the middle ground between developers and leadership? 

**使用 Git 自动更新吉拉**

## 让我们谈谈自动更新吉拉。为什么要使用 Git 自动化吉拉呢？因为，自动化可以带来三个显著提高生产力的结果:

流动

一致性

1.  时间
2.  **流量**
3.  心流是动机、清晰、专注和知识神奇地结合在一起的状态。这是我们效率最高的地方。但是，这可能是一种短暂的状态，通常不需要太多就能让我们脱离心流。

基于对 10，000 个记录的编程会话的分析和对 414 名开发人员的调查，佐治亚理工学院的[研究项目发现，开发人员在从中断中恢复工作后，需要 10-15 分钟才能开始编辑代码。当在方法编辑期间被中断时，开发人员只有 10%的机会在不到一分钟的时间内恢复工作。该研究还发现，开发人员在一个完整的工作日中很可能只有一次不受干扰的 2 小时会议。](http://chrisparnin.me/pdf/parnin-sqj11.pdf)

### 会议、延期通知和许多其他事情都会打断开发人员的工作流程。开发人员通常管理他们的 IDE 构建工具、Git 工作流和其他工具，为他们的生产力提供最符合人体工程学的设置。理想情况下，这种设置通过减少上下文切换来增加流量。吉拉作为一个可以被个性化以增加流量的工具，常常是难以捉摸的。增加流量的一个方法是自动化。

**一致性**

吉拉经常被贬低为微观管理和过多过程的工具，但是没有任何过程的团队缺乏一致性。这使得新开发人员很难加入，并增加了出错、额外工作以及无法满足安全性和性能等要求的风险。

建立一致性就是在团队内部建立信任，让他们相信他们正在为同一个目标而共同努力。在团队中建立信任的一个重要方法是持续更新吉拉，但是你需要在不创建繁重流程的情况下建立一致性。繁重的流程需要很多步骤，很多切换，团队成员必须将流程记在脑子里。

建立一致性的关键是自动化。

### **时间**

虽然你实际上不能创造新的时间，但是你可以避免浪费时间更新团队中的其他人，并且可以更有效地利用你的时间。

自动更新吉拉系统节省时间的一个重要原因是它是一个异步过程；你在工作的时候更新它。状态会议对生产力如此不利的原因是，从本质上讲，会议是一个同步的过程。每个参与的人都必须同时参与。这些会议是干扰，他们打破了流动。

如果吉拉已经更新，团队成员可以在自己的时间内简单地检查状态。随着团队开发工作流程的自动化，您将会节省时间。如果自动化让你不用转到吉拉，你就节省了一分钟；但是，如果自动更新吉拉意味着你可以取消电话或会议，你可能也只是为自己节省了一个小时，一整年下来，这是很多时间。

### **自动化吉拉与吉拉的 Git 集成& Gitkraken**

现在让我们看看如何使用 [GitKraken Git 客户端](https://www.gitkraken.com/git-client)进行编辑，使用 [Git 集成吉拉](https://www.gitkraken.com/git-integration-for-jira)在吉拉创建分支并查看提交，从而实现吉拉的自动化。

首先，让我们回顾一下智能提交。智能提交是一种语法，您可以在 Git 提交消息中使用它来指示吉拉做一些事情。您可以记录花费在某个问题上的时间，为该问题添加注释，并使用转换名称在吉拉转换问题状态。例如，您可以将问题从`In Progress`移动到`Resolved`。

`#Time`

`#Comment`

## `#[Transition]`

智能提交的一个很大的好处是它们很简单，几乎可以在吉拉普遍使用，不需要任何设置。缺点是您可以采取的行动有限，并且您需要记住语法和问题转换名称。

以下是使用所有三个命令的智能提交消息的示例:

`ABC-123`

`#comment resolving bug found by Sarah`

`#time 1h`

`#resolve`

**使用智能提交通过 Git 驱动吉拉**

首先，你要做的第一件事是在吉拉创建一个分支机构。您应该注意到，吉拉会自动提出一个问题键，这对关联分支很重要，默认情况下，吉拉还会添加摘要。

创建分支后，您应该注意到 UI 右侧分支旁边有一个 GitKraken 图标。点击图标打开 GitKraken 中的相关存储库并[签出 Git 分支](https://www.gitkraken.com/learn/git/problems/git-checkout-remote-branch)。

在这里，您可以根据需要进行任何编辑，暂存文件，并添加提交消息。因为这是一个聪明的提交，所以您要做的第一件事就是添加吉拉问题密钥；这就是将提交与吉拉问题联系起来的原因。您还需要添加一些智能提交语法、花费的时间，并标记状态。

接下来，提交您的更改并推送到您的[远程存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-git-remote)。如果你回头看看吉拉，你现在应该会看到这一明智决策的证据。您应该查看提交消息，看看提交是否已经正确关联。

`#comment resolving bug found by Sarah`

`#time 1h`

[观看此视频剪辑](https://youtu.be/omigX57wMM4?t=563)了解如何为吉拉和 GitKraken 创建集成 Git 的智能提交的分步演示。

## **Using Smart Commits to Drive Jira with Git**

To get started, the first thing you’re going to do is create a branch in Jira. You should notice that Jira automatically brings up an issue key, which is important for associating the branch, and by default, Jira also adds the summary. 

After you have created the branch, you should notice a GitKraken icon next to the branch on the right side of the UI. Click on the icon to open the related repository in GitKraken and [checkout the Git branch](https://www.gitkraken.com/learn/git/problems/git-checkout-remote-branch).

From here, you can make any edits you wish, stage the file, and add the commit message. Because this is a smart commit, the first thing you’re going to do is add that Jira issue key; this is what’s going to associate the commit with the Jira issue. You will also want to add some of the smart commit syntax, how long it took you, and mark the status.

Next, commit your changes and push to your [remote repository](https://www.gitkraken.com/learn/git/tutorials/what-is-git-remote). If you look back in Jira, you should now see evidence of that smart commit. You should look at the commit message to see that the commit has been associated correctly. 

**吉拉自动化**

吉拉自动化是捆绑在吉拉云中的一个应用程序，在您的数据中心和服务器上作为一个单独的应用程序提供。根据您的吉拉权限，您应该有权创建和更新您的自动化规则。

触发器是启动规则的原因。条件和分支允许您对规则设置一些限制，而动作是您选择对触发器做的事情。您甚至可以向单个规则添加多个操作。

吉拉自动化支持提交、分支和拉请求来触发自动化规则。吉拉自动化和智能提交之间的一个很大区别是您可以配置的操作数量。有超过 30 种不同的操作，比如转移一个问题，发送一个 web 请求或 Slack 消息，甚至发送一封电子邮件。

自动化对于吉拉规则来说是极其强大的。您只需对它们进行一次配置，就可以采取许多措施将它们连接在一起。一旦配置完成，开发人员就不必记住吉拉问题处于什么状态，或者转换语法是什么；他们只需要记得添加吉拉问题的关键。

记住这一点很重要:吉拉的自动化确实需要设置，根据您对工作流的要求，这可能需要一些时间。同样，自动化对吉拉的一个重要好处是，它不需要任何特殊的语法和提交消息，只需要包括吉拉问题密钥。

## **在吉拉设置自动化规则**

首先，你将从进入吉拉的`Project Settings`→`Automation`开始。您将被定向到自动化规则库。从这里，您可以点击右上角的按钮`Create rule`，开始在吉拉创建一个新的自动化规则。

[观看此视频剪辑](https://youtu.be/omigX57wMM4?t=793)了解如何在吉拉建立自动化规则的分步演示。

如果您真的想利用吉拉自动化的力量，智能值允许您在检查特定字段后设置自动化规则的运行条件。例如，如果提交了与`develop`分支相关的拉请求，您可以运行一个规则。

**吉拉自动化与 Git 集成**

如果您的提交、分支和拉请求还没有在吉拉，请将它们连接到吉拉，以便快速取胜，并在此基础上进行构建。当您的团队使用 Git 自动更新吉拉时，您将发现更大的流量和一致性，并且您将收回您的时间。而且，你可以避免被要求更新吉拉。💯

It’s important to keep in mind: automation for Jira does require setup, and depending on how thorough you want the workflows, this can take some time. Again, an important benefit of automation for Jira is that it doesn’t require any special syntax and commit messages other than including the Jira issue key. 

## **关于 Adam Wride，BigBrassBand 的联合创始人**

早在 2009 年，Adam Wride 就与人共同创立了 BigBrassBand，2012 年，该公司在 Atlassian Marketplace 上发布了第一个版本的 Git Integration for 吉拉工具。他现在是 GitKraken 的规划解决方案总经理，致力于构建一套基于 Git 的生产力工具。

[Watch this video clip](https://youtu.be/omigX57wMM4?t=793) to see a step-by-step demo on how to set up an automation rule in Jira. 

If you really want to tap into the power of automation for Jira, smart values allow you to condition an automation rule to run after checking specific fields. For example, you could run a rule if a pull request is submitted related to the `develop` branch. 

## **Automation with Git Integration for Jira**

If your commits, branches, and pull requests aren’t in Jira yet, connect them to Jira for a quick win and build on it.  When your team is updating Jira automatically using Git, you will find greater capacity for flow, consistency, and you will reclaim your time. And, you can avoid being asked to update Jira. 💯

## **About Adam Wride, Co-Founder of BigBrassBand**

Adam Wride co-founded BigBrassBand back in 2009, and in 2012, the company released the first version of the Git Integration for Jira tool on the Atlassian Marketplace. He is now the General Manager of Planning Solutions at GitKraken working to build a suite of Git-based productivity tools.