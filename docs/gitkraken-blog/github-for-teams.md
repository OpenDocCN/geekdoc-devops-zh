# 为团队使用 Git 和 GitHub

> 原文：<https://www.gitkraken.com/gitkon/github-for-teams>

[https://www.youtube.com/embed/OozPdrrhpKg?feature=oembed](https://www.youtube.com/embed/OozPdrrhpKg?feature=oembed)

视频

**Git 是什么？**

## 回顾 Git 的[历史，这个分布式版本控制系统是在 2005 年引入的，作为一种监控项目历史和随时间变化的方法。分布式版本控制系统与本地控制系统的不同之处在于，它们使从事项目工作的任何人都能够访问项目文件的相同副本，包括相同的版本和相同的历史。这种分布式模型使得](https://www.gitkraken.com/gitkon/history-of-git) [Git 成为团队](https://www.gitkraken.com/git-client/team-features)一起工作和轻松协作的一个很好的工具。

协作不是一件新鲜事；早在 Git 引入之前，人们就已经开始合作了。因为团队合作和协作已经存在了很长时间，研究人员已经能够研究并确定如何有效地利用它们。

Collaboration is not a new thing; people have been collaborating long before Git was introduced. Because teamwork and collaboration have been around for so long, researchers have been able to study and identify how to utilize them effectively.

GitKraken Git 客户端提供了一套特定于团队的功能和优势，如灵活的许可管理、显示谁在处理哪个文件的团队视图，以及预测性合并冲突检测，以便您可以在问题发生前避免问题。

The GitKraken Git Client offers a suite of features and benefits specific for teams, like flexible license management, Team View that shows who is working on which file, and predictive merge conflict detection so you can avoid problems before they happen.

为什么团队合作很重要？

## 我们对如何促进个人、团体甚至公司的成功了解得越多，我们就越意识到协作和团队合作对实现目标至关重要。

最近，全球形势迫使我们将这些有效团队合作的原则转化为数字环境。自从疫情以来，远程工作已经变得越来越普遍，并且很可能将继续成为许多工作场所的主流。无论您是在家工作、在办公室工作、在大型团队工作还是在小型团队工作，Git 都有适合每个人的东西。

Recently, global circumstances have necessitated that we take those principles of effective teamwork and translate them into a digital environment. Remote work has become more and more common since the pandemic, and likely will continue to be a mainstay for many workplaces moving forward. Regardless of if you are working from home, in the office, on a huge team or small one, Git truly has something for everyone. 

**Git &通信**

## 有效协作的一个关键方面是沟通。如果团队没有很好地沟通，他们就不能很好地合作，最终，这个团队就不能很好地向他们的客户交付他们的产品。

"编码是一项团队运动."

> 无论你是更喜欢体育运动的人还是喜欢电子竞技的人，你都会知道良好的团队合作是胜利的关键。通常情况下，合作良好的团队是沟通最好的团队。一个技术一般但沟通很好的团队打败一个技术一流但沟通很差的团队并不罕见。
> 
> [Nat Friedman, GitHub CEO](https://nat.github.io/hello/)

假设您的组织有一个工程团队、一个销售团队、一个社区团队和一个管理团队。如果这些团队想要成功，他们应该在他们的组织内部和组织之间协同工作。如果所有这些团队只专注于他们自己的小事情，工程团队就会去构建东西，因为他们可以。销售团队会销售他们想要的任何东西，甚至可能不是工程团队正在构建的东西。社区人员和营销人员会出去对产品做出承诺，这些承诺可能是真的，也可能是假的。最后，一位经理问:“这到底是怎么回事？”所有人都知道他们自己的小泡泡里发生了什么。

没有人希望这种情况成为他们的现实。为了避免这种僵化的思维模式，每个人和团队都应该学会认识到部门之间存在的相互依赖的关系。

想象一下所有这些团队现在一起工作。开发人员开发出真正好的产品，营销人员精心打造一个信息灵通的市场，社区团队能够从用户那里收集有价值的想法和信息，销售人员能够开拓新的市场，经理们也非常高兴，因为每个人都赚了更多的钱。这是一种由沟通团队培养的复合成功。

如果你想现在就开始，Slack、微软团队、GitHub for teams、Zoom 等工具以及更多用于远程开发团队的[工具](https://www.gitkraken.com/blog/best-tools-remote-dev-teams)有助于促进团队和公司内部的远程沟通。然而，不要走出去，采用一个中央沟通工具，只是假设你现在有效地沟通。相反，关注你能做些什么来培养一种重视协作和团队精神的文化，然后把交流工具作为一种资源来使用。

If you want to get started now, tools like Slack, Microsoft Teams, GitHub for teams, Zoom, and more [tools for remote dev teams](https://www.gitkraken.com/blog/best-tools-remote-dev-teams) help facilitate remote communication within your teams and company. However, don’t go out there and adopt a central communication tool and just assume that you’re now communicating effectively. Rather, focus on what you can do to foster a culture that values collaboration and teamwork and then use the communication tool as a resource.

**基本 Git 流程& Git 命令**

## 这张图片展示了使用 [GitHub 流模型](https://github.com/SvanBoxel/release-based-workflow/issues/1)的 Git 中的项目流，这是最受认可的 [Git 分支策略](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy)之一。在顶部，您会发现主分支(这里显示为“主”)。这代表您当前正在进行的项目。当你想为项目创建一个新特性或者一个新版本时，你将[创建一个 Git 分支](https://www.gitkraken.com/learn/git/problems/create-git-branch)。

This image shows the flow of a project in Git using the [GitHub flow model](https://github.com/SvanBoxel/release-based-workflow/issues/1), one of the most recognized [Git branching strategies](https://www.gitkraken.com/learn/git/best-practices/git-branch-strategy). Across the top, you’ll find the main branch (shown here as “Master”). This represents the project that you’re currently working on. When you want to create a new feature for the project or a new version, you will [create a Git branch](https://www.gitkraken.com/learn/git/problems/create-git-branch). 

在此阶段，您应该与您的团队讨论该特性，以及它如何在高层次上融入项目。一旦您和您的团队对您的 [Git 分支](https://www.gitkraken.com/learn/git/branch)感到满意，您就可以将它合并回主分支。通常，一旦你 [Git 将](https://www.gitkraken.com/learn/git/git-merge)你的特性分支合并回主分支，你将[删除你的本地 Git 分支](https://www.gitkraken.com/learn/git/problems/delete-local-git-branch)以保持项目的整洁。

有许多方法可以查看和修改项目的 Git 历史。方法之一是通过 [Git 命令](https://www.gitkraken.com/learn/git/commands)。Git 命令可用于添加文件、添加提交点、为文件创建消息以及查看项目历史。请注意，当您对[远程存储库](https://www.gitkraken.com/learn/git/git-remote)上的项目进行变更时，其他所有能够访问该存储库的人都可以看到这些变更。这真的很有帮助，因为它允许您和您的团队成员除了查看项目的整体状态之外，还可以查看其他人在做什么。

There are a number of ways to view and alter a project’s Git history. One of the ways to do that is via [Git commands](https://www.gitkraken.com/learn/git/commands). Git commands can be used to add files, add commit points, create messages for your files, and see the project history. Be aware that when you make changes to a project on the [remote repository](https://www.gitkraken.com/learn/git/git-remote), everyone else with access to that repo can see those changes. This can be really helpful as it allows you and your team members to view what everyone else is doing in addition to the project’s overall status.

有了 GitKraken Git 客户端，团队成员可以根据自己的选择高效地协作工作流——无论他们喜欢终端还是 GUI，还是 Mac、Linux 或 Windows。GitKraken 在开发人员已经在工作的地方与他们会面。

**Git 命令**

**Git 入门**

## Git init:将现有目录转换成一个 [Git 存储库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)。

[Git 克隆](https://www.gitkraken.com/learn/git/git-clone):将 Git 存储库克隆到您的本地机器上。

### Git config:在 [C](https://www.gitkraken.com/cli) [L](https://www.gitkraken.com/cli) [I](https://www.gitkraken.com/cli) 上配置你的用户名、电子邮件，甚至是你提交的颜色，这样你就可以很容易地分辨出什么是你的。

Git status:显示 Git repo 的状态和整个项目的状态。

[Git clone](https://www.gitkraken.com/learn/git/git-clone): clones the Git repository to your local machine. 

**更改 Git 回购**

Git add:将文件添加到包含准备提交的更改的 repo 中。

[Git commit](https://www.gitkraken.com/learn/git/commit) :提交您所做的更改

### **Changing a Git Repo**

**同步命令**

Git remote:允许您创建和删除到其他存储库的连接。

Git fetch:将您的所有 Git 历史从远程 repo 下载到您的本地 repo。

### [Git checkout](https://www.gitkraken.com/learn/git/git-checkout) :允许你切换到一个特定的分支。

Git pull:将所有的更改从远程机器下载到本地机器。

Git push:将所有更改推送到远程。

[Git checkout](https://www.gitkraken.com/learn/git/git-checkout): allows you to switch to a particular branch.

**工作和更改版本**

Git branch:返回该 repo 中当前所有分支的列表。

Git merge:允许您将更改合并回主。

### **Working and Changing Versions**

**保持干净的 Git 历史记录**

当使用 Git 进行协作时，保持事物整洁非常重要。一种方法是保存详细的提交历史。当你定期提交时，你给自己一个潜在错误的良好缓冲。每次提交时，把这个过程想象成为自己创造“保存点”会很有帮助。

创建保存点对于管理有效的工作流程至关重要。当创建这些点时，确保它们是具体的、及时的和可理解的。如果你犯了一个错误并且经常提交，修复这个错误就像恢复到一个旧版本一样简单，比如[恢复一个 Git 提交](https://www.gitkraken.com/learn/git/problems/revert-git-commit)。

## 明确具体的承诺；你不想以数百个随机和不清楚的保存点结束。

When collaborating using Git, it’s important to keep things really tidy. One way to do this is by keeping a detailed commit history. When you commit regularly, you give yourself a good cushion for potential errors. It can be helpful to think of this process as creating “save points” for yourself each time you commit.

Creating save points is crucial to managing an effective workflow. When creating those points, be sure that they are specific, timely, and understandable. If you make an error and have been committing often, fixing the mistake is as easy as reverting to an older version, such as [reverting a Git commit](https://www.gitkraken.com/learn/git/problems/revert-git-commit). 

在上图中，你可以看到如果不使用自定义提交消息，GitHub 中的 Git 日志是什么样子的；它看起来像一团乱麻。拥有一堆默认文件名的文件:“不管文件名是什么，都要更新”，这使得理解每个提交代表什么几乎是不可能的。

另一方面，如果您花时间包含高质量的自定义提交消息，如下图所示，当您将来查看项目历史时，您可以为您和您的团队添加有用的上下文。

In the image above, you can see what a Git log looks like in GitHub if you don’t use custom commit messages; it looks like an absolute mess. Having a bunch of files with the default file name: “Update whatever the file name is,” makes understanding what each commit represents virtually impossible. 

很容易看出区别。这不仅对你，而且对你的所有团队成员来说，都使你的项目工作更加清晰和容易。

您可能会注意到，一些提交消息旁边有省略号。这是一种很有用的方式，可以让您注意到该提交有更多的信息。这只是另一个可以帮助你的团队更清晰、更有效地协作的实践。

GitKraken Git 客户端提供的对项目历史的可见性将允许您看到您的团队中谁正在处理哪个文件，从而在整个过程中导致更好的协作和更少的合并冲突。

You might notice that some of the commit messages have ellipses next to them. This is a helpful way of noting that there’s even more information for that commit. This is just another practice that can help your team collaborate clearly and more effectively. 

在一天结束时，您决定哪些实践对您和您的团队最有效。最重要的是，你实际上建立了一套标准实践，以及一种确保每个团队成员都遵循它们的方法。花时间分享想法、达成共识并保持相互负责的团队能够更快、更有效地工作，并最终构建更好的产品。

如果你想提升你团队的表现，看看 GitKraken 就知道了！GitKraken 为用户提供教育和 [Git 培训资源](https://www.gitkraken.com/learn/git)，提高使用 Git 的[团队的可见性，以及灵活的许可管理。使用 GitKraken 的团队最终会变得更加有组织、高效、协作和成功。](https://www.gitkraken.com/git-client/team-features)

At the end of the day, you decide which practices work best for you and your team. What’s most important is that you actually establish a set of standard practices, as well as a way to ensure each team member is following them. Teams that take the time to share ideas, come to a consensus, and keep one another accountable are able to work faster, more effectively, and ultimately build better products. 

If you’re looking to level up your team’s performance, look no further than GitKraken! GitKraken provides users with educational and [Git training resources](https://www.gitkraken.com/learn/git), improved visibility for [teams using Git](https://www.gitkraken.com/git-client/team-features), and flexible license management. Teams that use GitKraken ultimately become more organized, efficient, collaborative, and successful using Git.