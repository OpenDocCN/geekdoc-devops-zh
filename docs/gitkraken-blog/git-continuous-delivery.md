# Git 连续交付|将自动化引入 Git

> 原文：<https://www.gitkraken.com/gitkon/git-continuous-delivery>

[https://www.youtube.com/embed/jlNfUO-NKb8?feature=oembed](https://www.youtube.com/embed/jlNfUO-NKb8?feature=oembed)

视频

软件开发很难。承认这一点是可以的。编写代码非常困难，并且伴随着很大的压力，这使它成为一个压力非常大的职业。理想情况下，您应该总是在寻找减轻软件开发生活中日常痛苦的方法。一种方法是通过自动化，这正是 CircleCI 的 Angel Rivera 在 2021 Git kon Git conference 的演讲中所涵盖的内容。

## 软件的目的是什么？

安吉尔对目的问题的回答是“软件的存在是为了满足最终用户的需求。”这可以通过创建数字服务和解决方案来实现，这些服务和解决方案可以提供快速、可靠、一致和安全的结果，虽然这可以采取多种形式，但最终本质上来说，软件可以实现自动化。软件有助于具体对象和物理执行过程的数字表示，例如开灯或关灯，或者在某些情况下，在线购物和支付。

开发软件有很多方法和工具。像文本编辑器、编程语言和框架这样的工具是每个开发人员至少必须拥有的构建软件的工具。但是除此之外，开发人员必须学习许多概念和方法才能成功。有很多东西要学，而且常常感觉软件开发的工作是无止境的。

但是隧道的尽头有光；开发人员可以采用一些工具、概念和实践来减少软件开发中最困难和最乏味的部分。

GitKraken Client 是为满足用户需求而发展的工具的完美范例，具有交互式拉请求管理、合并冲突检测和解决、深度链接等高级功能。

## **Git 和连续交货**

Git 和持续交付是一对很好的组合，当它们结合在一起时会提供巨大的价值。让我们先来看看 Git 在合作关系中扮演什么角色。

Git 是一个版本化工具，使开发者能够捕捉和版本化代码变更。Git 因为占用空间小和分布式架构，很受开发者欢迎。有了 Git，可以在隔离的 [Git 分支](https://www.gitkraken.com/learn/git/branch)中开发变更；这有助于防止不需要的代码或错误被引入到生产中。Git 为我们提供了一种安全地试验代码的方法，并有助于减少错误。但是如果确实出错了，Git 可以很容易地将代码库恢复到安全状态。

## **手工 Git 开发流程和代码评审**

使用 Git 的典型开发人员工作流程如下所示:

*   您 [Git 将](https://www.gitkraken.com/learn/git/git-clone)下载到他们的本地环境中。
*   您在开发环境中本地修改代码，处理特性或修复 bug。
*   当你到达一个里程碑时，你 [Git 提交](https://www.gitkraken.com/learn/git/commit)那些本地代码变更。
*   您将这些代码更改推送到上游分支，也许是推送到 GitHub 上的 repo。
*   其他开发人员开始手动[代码审查](https://www.gitkraken.com/blog/code-review)过程。

手动代码审查过程意味着开发团队需要有人阅读和审查被提议或提交的代码变更，然后再将它们合并到产品级分支中。其他开发人员仔细检查代码，看是否有已知的 bug、错误以及是否符合编码标准。

手动代码审查是有帮助的，并且比仅仅相信如果它在本地工作，它将在生产中工作要好得多，但是手动代码审查在时间和资源方面也是非常昂贵的。由于是人在审查代码，这个过程很容易出现人为错误。

手动代码审查过程的另一个关注点是审查有问题的代码，或者甚至开始审查要花多少时间。开始审查提交的代码可能需要一个小时、一天、一周、一个月甚至更长的时间。完成审查可能需要更长的时间。如果评审人员发现任何缺陷、错误或错误，代码就会被拒绝，提交代码的开发人员必须重新开始这个循环。与此同时，项目中可能会引入其他变化，从而进一步增加复杂性。

CircleCI 估计，平均一个开发者的时间成本大约为每分钟 1.42 *美元。算一算，一个普通的代码审查大约需要 120 分钟，在一些团队中，可能有 10- 20 个开发人员参与。这一切很快变得非常昂贵。*

## **持续集成和持续交付原则**

控制代码开发和评审成本意味着使代码评审过程更加高效。持续集成和持续交付(CI/CD)实践提供了一条通过自动化使任何工作流更加高效的途径。核心上，CI/CD 本质上是使用自动化工具构建、测试和交付代码和用户环境变更的实践。

CI/CD 为我们提供了可以在软件开发团队中采用和实现的原则。在 Git 的上下文中，持续交付和持续集成原则通常意味着开发人员将经常提交代码到共享的代码库中，并在每次提交时测试代码。

### **更快的反馈循环**

CI/CD 为开发人员提供了更快的反馈回路。因为您不需要等待人工时间管理来开始评审过程，您可以在几分钟甚至几秒钟内发现您的代码是否合格。这种快速的反馈循环让你尽可能快地修复代码中的错误，最终减少代码变更到达最终用户的时间，实现软件的目标。

### **共享存储库**

Git 在这里起着核心作用。持续集成的核心原则之一是将团队中每个开发人员的所有工作副本合并到一个共享的代码库中。Git 让开发人员可以自由地在本地工作，在不妨碍其他用户的情况下，尽可能多地试验他们需要的分支。当您的代码准备好了，Git push 就是将他们的更改集成到共享存储库中所需要的一切。

有了 GitKraken 客户端，像推和拉这样的操作只需点击一下。拖放操作可以从可视化的、易于阅读的提交图中推送和拉出更改，或者在 Git 增强的终端中使用自动完成命令。

### **部署自动化**

持续交付和持续部署的另一部分是自动将新软件版本部署到目标环境的实践。不仅是最终用户将与代码交互的生产环境，还有测试和试运行环境。在测试环境中，可以部署测试工具来检查错误或其他问题，安全地远离其他环境。

CI/CD 的所有原则确实依赖于自动化来使它们有效。正是自动化通过定义和执行软件交付实践或过程，将这些原则转化为现实。

## **CI/CD 管道& CircleCI**

将 CI/CD 的这些原则转化为现实的一个途径是采用和实现一个持续的交付平台，如 [CircleCI](https://circleci.com/) ，并建立 CI/CD 管道。

管道本质上是一组需要针对代码执行的东西，以确保代码得到正确测试。当然，管道需要测试代码是否正确编译，以及工件或二进制文件是否如预期的那样输出。但是还有很多其他的东西可以测试，比如安全扫描或者吞吐量测试。无论您在软件开发评审过程中需要做什么，您都可以在 CI/CD 管道中使用 CircleCI 来实现自动化。

下图显示了 CircleCI 平台中 CI/CD 管道的视觉效果。

这是 Angel 所谓的“快乐之路”的一个例子，在自动化代码审查过程中，一切都是正确的:

*   代码从本地开发环境被推送到共享分支。
*   CircleCI 检测这些变化，然后测试代码。
*   所有测试都通过了，CircleCI 中的事务和构建工作也通过了。
*   最后，变更被推送到主分支，准备在下一个发布周期中传播。

当然，并不是每条路都是幸福的。让我们看一下使用自动化的相同过程，以及当它失败时会是什么样子。

在这种失败的情况下:

代码从本地开发环境被推送到共享分支。

三项测试中有两项失败。

*   构建了 Docker 映像，但是没有部署代码。
*   一旦系统检测到故障，它就通知开发人员这个问题。
*   开发人员开始修复问题，然后再次运行整个过程。
*   Once the system detects the failure, it notifies the developer of the problem.  
*   **自动化程度更高，手动问题更少**

手工过程存在于每个开发周期中，但是有很多你可以并且应该自动化的。减少代码评审过程中所涉及的人力资源是一种有助于加速发布周期时间和增加代码可靠性的方法。CI/CD 还意味着更快的反馈循环，因为测试和审查代码在每次提交和推送时都会自动进行。

## 另一个好处，也许不太明显，是 CI/CD 改善了围绕代码的整体协作，导致更好的团队协同。当团队能够在危险信号实际发生之前阻止或捕捉到危险信号时，他们会更有凝聚力。更少的错误意味着更顺畅的工作场景和更高的士气。

例如，让我们想象这样一种情况，两个不同的开发人员接触存储库中的相同文件。在他们开始编写代码之前，他们可以合作决定谁先编写代码的顺序，这有助于避免经常发生的 [Git 合并冲突](https://www.gitkraken.com/learn/git/tutorials/how-to-resolve-merge-conflict-in-git)。从开发人员的盘子里拿走手动代码审查的时间，对于确保他们有足够的时间来有效地合作和计划有很大的帮助。

Another benefit, perhaps less obvious, is that CI/CD improves overall collaboration around code, leading to better team synergy. Teams function more cohesively when they can stop or catch red flags before they actually happen. Fewer errors mean smoother working scenarios and higher morale. 

**合适的工具和自动化 FTW！**

[点击此处，观看 Angel 在 CircleCI 中实现 CI/CD 管道的演示](https://youtu.be/jlNfUO-NKb8?t=1034)，自动化大部分代码审查过程。

无论你最终如何实施持续交付战略，它都始于与你的团队一致而清晰的沟通。GitKraken 客户端的[团队功能可以帮助你深入了解你的团队是如何协作的。除此之外，g it 的使用变得更加简单和安全，同时 Git 的高级工作流使任何规模的团队的开发人员都可以使用。立即开始缩短部署和发布周期，并下载](https://www.gitkraken.com/git-client/features) [GitKraken 客户端](https://www.gitkraken.com/git-client/try-free)！

## **The Right Tools and Automation FTW!**

[Click here to see Angel walk through a demo](https://youtu.be/jlNfUO-NKb8?t=1034) of implementing a CI/CD pipeline in CircleCI, automating much of the code review process. 

No matter how you finally implement a continuous delivery strategy, it starts with consistent and clear communication with your team. The [team features of GitKraken Client](https://www.gitkraken.com/git-client/features) can help you get betting insight into how your team is collaborating. And that’s on top of making it much easier and safer to use Git, while unlocking the advanced workflows Git enables developers on teams of any size.  Start down the path of reduced deployment and release cycle times today and download [GitKraken Client](https://www.gitkraken.com/git-client/try-free)!