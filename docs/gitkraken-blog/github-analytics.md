# 利用 GitHub Analytics 的力量改善团队

> 原文：<https://www.gitkraken.com/gitkon/github-analytics>

[https://www.youtube.com/embed/grxnerhcZKw?feature=oembed](https://www.youtube.com/embed/grxnerhcZKw?feature=oembed)

视频

在 2021 年 [GitKon Git 大会](https://gitkon.com/)上，康卡斯特杰出的工程师 Leslie Chapman 分享了她利用 GitHub 分析和使用数据扭转一个苦苦挣扎的团队的故事。

## **观察过程**

在过去的 20 年里，Leslie 帮助 Comcast 为客户提供交互式解决方案，使他们能够通过遥控器与他们的有线电视盒进行交互。在 Comcast 内部，这被称为 X1 平台，目前为超过 3500 万客户提供服务。作为一名杰出的工程师，Leslie 的很大一部分职责是观察流程和系统，以便提出改进建议。她的目标是让同事们的工作更加轻松愉快，同时提高产品质量。

Leslie 目前的团队交付 Comcast 的 Xfinity 应用程序，作为专注于交付 Android、iOS 应用程序和其他领域的子团队，在两周的发布周期内工作。团队每隔一周在周五创建发布分支。一些从事产品工作的子团队在发布周的早期有一个早期的代码冻结，有时团队会一直进行修改，直到最后一刻。

当 Leslie 团队中的开发人员准备好 [Git merge](https://www.gitkraken.com/learn/git/git-merge) 时，他们提出一个 pull 请求，然后在一个代码评审 Slack 通道中链接到该 pull 请求。这使得整个团队都可以看到变更，而不仅仅是小团队，这有助于众包评审者及时地通过那些拉取请求。

当 Leslie 第一次加入这个团队时，监视这些 Slack 通道让她对代码有了很多了解，但也让她了解了团队是如何协作的。她注意到有些人只是粘贴 [GitHub pull request](https://www.gitkraken.com/learn/git/problems/github-pull-requests) 链接，而其他人也会围绕这些变化提供一些评论。一旦有人在 GitHub 上给了+1 分，表示同意公关，他们通常会在 Slack 频道上回复一个“同意”的表情符号。

在 GitKraken 客户端中，您不仅可以创建 GitHub pull 请求，您和您的团队还可以编辑 GitHub PRs，包括添加注释、关联构建状态、合并和批准 pull 请求，而无需离开该工具。

## **一些早期的预感**

Leslie 马上注意到的一件事是，他们的 Android 存储库在 GitHub 中只需要一个+1。她立即将此作为危险信号提出。她发现，虽然团队只在 GitHub 中执行了+1，但还有一个口碑“+ 2”步骤需要完成，但在 GitHub 中是看不到的。

她一直和各个级别的工程师交谈，从初级到高级，看看他们喜欢和不喜欢他们的代码评审过程。初级工程师的一些反馈是，人们没有在评论中添加足够的评论。回头看看 Slack 通道，她意识到有些人在 Slack 通道中添加了很多上下文，但不是在 [Git pull 请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)本身中。

初级工程师还报告说，他们觉得没有时间查看拉动式请求中的反馈，因为他们获得批准的速度太快了。人们觉得在一个发布分支被砍掉的前一天让他们的 PRs 被批准只是一个疯狂的举动。

Leslie 听到的最有争议的反馈是关于“团队内”和“非团队内”的评审，其中一些人强烈认为评审应该只由他们工作团队中的人完成，认为他们应该是唯一被允许维护和支持代码的主题专家。其他人强烈地感觉到，让团队之外的人进行代码审查是很重要的，这允许他们对代码有新的看法，并了解另一个团队正在做的可能影响未来功能工作的事情。

莱斯利对如何改善这种情况有一些预感，但她所有的都是猜测，正如你可能知道的那样，工程师不会因为猜测而改变系统和流程。她需要数据来支持这些理论。幸运的是，GitHub 有一套 API，它们都有很好的文档记录，并且易于使用。

## **使用 Git 和 GitHub Analytics 回答问题**

莱斯利有些问题需要很难回答:

*   人们真的在我们的 android 回购中获得了+2 还是他们正在与+1 融合？
*   平均[代码评审](https://www.gitkraken.com/blog/code-review)真正开多久？
*   在创建发布分支的前一天，我们会合并更多的内容吗？
*   我们在代码审查中留下了多少评论？

深入 GitHub API 意味着 Leslie 能够遍历每个回购中的提交列表。她对提出拉取请求的时间和合并发生的时间之间的差值感兴趣，这让她了解到项目需要多长时间进行评审。通过遍历 [Git 提交](https://www.gitkraken.com/learn/git/commit)的列表，也很容易看到平均一个 pull 请求中有多少评论。

坦率地说，这不是一个完美的过程。这种调查方法不能洞察人们实际上花了多长时间进行审查，只能洞察在合并之前拉请求处于审查状态的时间。由于这只是一个小实验，她也没有减去正常工作时间，正常工作时间因团队而异，因为开发人员在不同时区的办公时间也不同。

这种方法的另一个警告是 [Git cherry picks](https://www.gitkraken.com/learn/git/cherry-pick) ，这在 Leslie 评估的工作流中很常见，通常花费很少的时间，并且在审查时需要更少的审查。这意味着精选可能会扭曲 GitHub 指标，如果考虑进去的话，显示评论花费的时间会更少。

GitKraken Client 是 Git 工具的完美范例，它已经发展到可以应对新的挑战，具有交互式拉请求管理、合并冲突检测和解决、深度链接等高级功能。

## **GitHub 分析显示了什么？**

Leslie 能够确定绝大多数拉取请求在提出后的 24 小时内被合并，但也有一些在 30 分钟内被合并。总的来说，这看起来很健康，因为团队及时地结束了事情，GitHub 的数据似乎没有显示他们只是在审核上盖章。

然而，看看每周一天的评论就知道了不同的故事。周四是评论高峰，就在发行分支被砍掉之前。这是一个显著的峰值，大约占团队合并总数的 40%。这不是一幅美丽的画面。当她查看那些周四审查的关闭时间的增量时，它危险地倾向于在不到一个小时内完成大多数 PRs 审查。她觉得那不对劲。

她看到的最后一个数据点是关于评论的。团队平均每个拉取请求有两到三个评论。这是在考虑了很少有评论的樱桃选择之后。这可能意味着，如果初级开发人员没有机会审查反馈，反馈就会丢失。

## **GitHub Analytics 推动的变革**

**正式+2**

### 她解决的第一件事是更新 Android repo，要求一个正式的“+2”，可以在 GitHub 中跟踪。因为没有办法通过数据证明人们坚持得到“+2”的口碑知识，这使得更容易说服团队需要这一正式步骤。每个人都同意安全总比后悔好。

The first thing she addressed was updating the Android repo to require a formal “+2” that could be tracked in GitHub. Because there was no way to prove through data that people were adhering to the word-of-mouth knowledge of getting a “+2,” this made it easier to convince the team that this formal step was needed. Everyone agreed it was better safe than sorry.

**质量高于速度**

### GitHub 指标显示，在他们的发布周期结束时，事情很仓促，他们没有时间进行尽职调查。就团队文化而言，所有团队都进行了有意义的变革。鼓励所有开发人员在本周早些时候观察软代码冻结。从社会角度来说，所有的开发人员都清楚，坚持围绕质量的政策比在最后一分钟加入一些东西来适应两周的发布周期更重要。他们的短距离冲刺意味着不需要太长时间就能赶上下一辆发布车。

The GitHub metrics showed that at the end of their release cycles, things were being rushed and they didn’t have time for due diligence. Meaningful changes, in terms of team culture, were made across all of the squads. All devs were encouraged to observe a soft code freeze earlier in the week. Socially, it was made clear to all the developers that it’s more important to adhere to policies around quality over getting something in at the last minute just to fit within the two-week release cycle.  Their short sprints meant it wouldn’t take forever to catch the next release vehicle.

**审核的代码所有者**

### 下一个要解决的问题是标准化团队内部和团队外部的评审。为了解决这个问题，他们开始使用 [Git 代码所有者文件](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners)。在这个工作流中，开发人员创建一个名为`CODEOWNERS`的文件，并将其提交到一个目录或 [Git 库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)的基础中。这允许关于谁必须审查拉取请求的规则。

例如，对于某个存储库，有一个人员列表，至少需要列表中的一个人来批准 PR。您可以更细化，要求存储库中不同的包有特定的评审者。这确保了正确的主题专家(SME)审查给定的特性，但仍然允许不在批准者列表上的任何人也给予批准。这满足了两个阵营，那些想要团队内和团队间评审的人。

**重复性**

这种 GitHub 数据驱动的方法最棒的一点是，Leslie 可以在 6 个月内再次运行完全相同的脚本，并在 12 个月内再次运行，以查看她的团队是否在流程改进方面取得了有意义的进展。

### **Repeatability**

**Git 数据和职业发展**

莱斯利在康卡斯特的另一个重要角色是指导。虽然这有许多不同的形式，每种关系也各不相同，但学员们通常都有一个共同点，那就是渴望获得一次出色的年终考核。好消息是，您可以使用 Git 和 Git repository 托管服务工具来实施一个解决方案，以跟踪您作为团队以及个人的个人和职业发展进度。

## 莱斯利的建议是，你应该在年终回顾和评估中尽可能彻底。大多数人几乎不记得他们自己在评估年开始时做了什么，所以你的经理，一个有十几个或更多员工的人，几乎不可能记住你一整年做了什么。提醒他们是你的工作。

虽然吹嘘自己有多棒可能会让人觉得恶心，但没人会为你这么做。通过告诉你的经理你有多棒来讲述你的故事，告诉他为什么你应该得到加薪或升职。虽然谈论你所做的超出你当前工作描述的事情是很好的，但底线是证明你确实在做你当前的工作描述。

Leslie’s advice is that you should be as thorough as possible in your end-of-year reviews and assessments. Most people can barely remember what they themselves did at the beginning of their review year, so there’s very little chance your manager, who has a dozen or more employees, will have committed to memory what you did all year. It’s your job to remind them.

**使用 GitHub Analytics 进行年度评估**

对于大多数工程师来说，做这项工作的基础就是代码。无论这意味着编写代码、审查代码并在发现问题时提出问题，还是其他特定于代码的工作，Git data 都可以提供帮助。进入你的 GitHub 账户，你可以点击右上角的图标，选择“你的个人资料”,就可以看到你今年或年复一年的贡献。

### **Using GitHub Analytics for Annual Reviews**

For most engineers, that baseline of doing the job is all about code. Whether that means writing code, reviewing code and raising issues when you see them, or other code-specific work, Git data is here to help. Going into your GitHub account, you can click on your icon on the upper right corner and select “Your profile” to can see all sorts of insights into your contributions for this year, or year over year. 

显示您的年度贡献的图表让您深入了解您完成了多少代码评审，或者您提交了多少代码，以及提交给了哪些存储库。您可以看到您已经启动了多少个拉取请求，提出了多少个问题。甚至还有历史数据，所以你可以看到你是否比往年做得更多或更少，以及你与你的共享库上的其他贡献者相比如何。

了解这些 GitHub 指标不仅可以让你向你的经理炫耀你可能是今年的最佳贡献者，还可以让你提前解释为什么你可能不是。例如，如果你今年休产假或陪产假，你的承诺可能会比往年少，这没关系。另外，不要忘记在年终评估中利用其他系统数据，比如吉拉等票务系统。

The graph showing your annual contribution grants you insight into how many code reviews you completed or how many commits you made and to what repositories. You can see how many pull requests you’ve started and how many issues you’ve raised. There’s even historical data so you can see if you’re doing more or less than previous years, and how you fare in relation to other contributors on your shared repositories.

说到吉拉……Git kraken 为吉拉开发的工具 Git Integration 将在吉拉和 Git 之间架起一座桥梁，在减少上下文切换的同时为您的团队成员提供更多的可见性。

**使用 Git 工具& GitHub Analytics 讲述你的故事**

[Start a Free Trial](/git-integration-for-jira)

Git 最终是一个讲故事的工具。无论是您的代码故事，您的团队如何工作，或者您如何日复一日地工作，您的 Git 历史都是独一无二的。这取决于你去释放这个故事的力量，让这个世界和你的生活变得更好。

无论你如何驾驭 Git 的力量，GitKraken 工具如 [GitKraken Client](https://www.gitkraken.com/git-client) 、 [GitLens](https://www.gitkraken.com/gitlens) 和 [Git Integration for 吉拉](https://www.gitkraken.com/git-integration-for-jira)都可以帮助你掌握你的工作并跟踪你的进展。

## **Use Git Tools & GitHub Analytics to Tell Your Story**

Git is ultimately a tool about storytelling. Whether it’s the story of your code, how your team worked, or how you did your job day to day, your Git history is uniquely yours. And it’s up to you to unlock the power of that story to make the world and your life better.  

No matter how you harness the power of Git, GitKraken tools like [GitKraken Client](https://www.gitkraken.com/git-client), [GitLens](https://www.gitkraken.com/gitlens), and [Git Integration for Jira](https://www.gitkraken.com/git-integration-for-jira) are there to help you keep on top of your work and track your progress.