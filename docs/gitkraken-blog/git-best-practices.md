# 你为什么对这件事这么迟钝？| Git 最佳实践

> 原文：<https://www.gitkraken.com/gitkon/git-best-practices>

## 你为什么对这件事这么迟钝？

[https://www.youtube.com/embed/AN-iaHcXgYI?feature=oembed](https://www.youtube.com/embed/AN-iaHcXgYI?feature=oembed)

视频

Git 是当今世界上最流行的源代码控制管理系统。Git 可以确保您的代码得到备份和版本控制，还可以让其他开发人员访问您的工作。如果使用得当，Git 不仅仅是一种存储或组织代码的方式。Git 可以帮助简化复杂的过程，比如发布管理、特性开发，以及与多人同时工作。

在 2021 [GitKon Git Conference](https://gitkon.com/) 的会议中，Joe Glombek 分享了他最喜欢的 Git 最佳实践。剧透:对如何使用 Git 非常挑剔是可以的！这其实是一件好事。

无论您喜欢在 GUI 或 CLI 中工作，GitKraken Client 都可以在您已经工作的地方会见开发人员，并帮助您轻松开发有效的工作流程最佳实践。

不幸的是，Git 利用率低是一个比您想象的更普遍的问题。无数人将 Git 用作代码的垃圾场，而不是管理良好的软件档案库。

这里有五个简单的提示，可以确保您通过这些 Git 工作流最佳实践充分利用 Git:

## **提示 1:提交消息就是一切**

在下面展示的精彩 XKCD 网络漫画中，作者说“随着项目的拖延，提交信息变得越来越少。”

我们看到评论从描述性的“创建主循环和定时控制”下降到我的“手在打字”和“哈哈哈”的绝对混乱状态。乔想说这只是连环漫画中的一个笑话，但它可能太真实了。

在另一个例子中，他分享了一个 Twitter 用户的实际提交历史记录。

Dennis 在 Git 历史中浏览“有用的”提交消息来调查一个问题。很明显，这些提交消息没有提供足够的信息来知道在引入 bug 后从哪里开始查找。

想象一个场景，你在代码库中看到一些奇怪的东西，但是没有代码内注释。使用 Git 责备，您可以看到是谁修改了它，但是您会看到一条提交消息，上面只写着“test”不幸的是，做出改变的开发人员外出度假了，所以你不能立即找到他们改变背后的原因。现在，如果你改变了它，或者在别的地方破坏了某些东西，你就有重新引入一个 bug 的风险。相反，假设你看到了一段代码，自从你最后一次工作以来，它发生了变化，你对它现在的编写方式不满意。再一次，你利用 Git 责备，你看到一个提交消息，说“修复 chrome 中的日期格式问题。”在这种情况下，您最好保持原样。一个好的提交消息实际上救了你！

如果您想要编写一个好的提交消息并遵循 [Git commit](https://www.gitkraken.com/learn/git/commit) 最佳实践，提交消息应该像回答问题“这个提交做什么？”还可以考虑用动词开始提交消息。例如“修复恢复”或“集成服务”虽然这是一个很好的背景，但它本身是不够的，因为这些短消息仍然没有完全告诉我们在这些提交中发生了什么。

### **细节，细节，细节！**

GitKraken 客户端为您的提交消息提供了两个字段:标题和描述。最好保持标题简短，不超过 72 个字符，并在描述字段中添加更多细节。

例如，“修复 Chrome 中的日期格式问题”比“修复提交消息标题的错误”要好得多。在提交消息描述中，您可以提供更多的细节。在描述中，你可以这样说:“Chrome 总是采用 MM/DD 格式，不管浏览器的区域设置是什么。”虽然这是一个虚构的 bug，但是这个例子说明了提交消息可以更加健壮和信息丰富。

为 [Git 回复](https://www.gitkraken.com/learn/git/problems/revert-git-commit)提供更多信息也是非常有用的。默认情况下，Git revert 生成的消息填充了提交消息标题:“reverts*last commit message title*”比如`Reverts new “Paytastic Checkout” checkout flow`。

如果我们添加描述“客户已经改变主意，希望恢复使用 LegacyCart ”,那么我们在顶部会有一个精彩的概述，在描述中会有更多的细节。

您也可以在命令行上添加提交消息描述。您只需要传入两个消息参数:

```
Git commit -m “Title” -m “Description”
```

### **将提交链接到问题**

如果您有一个票务系统或待办事项列表，在提交消息中包含项目的标题或票证 ID 号有时会有所帮助。您甚至可以在描述中提供初始问题的链接。

如果你已经将 GitKraken 客户端连接到一个问题跟踪器，比如 [GitHub Issues](https://www.gitkraken.com/integrations/github) ，你可以输入搜索找到你想要处理的问题，右击该问题，[为该问题创建一个 Git 分支](https://www.gitkraken.com/learn/git/problems/create-git-branch)。通过这种方式添加额外的信息非常容易，因此当您将来回到某个问题时，您可以很快看到围绕该提交发生的所有事情。所有这些都不需要工具之间的上下文切换！

## **提示 2:提交和推送，少而频繁**

当谈到人们应该多久犯一次错误时，最有说服力的论点是“少而常”因为 Git 是你工作和进度的备份，所以只有在特性完成时才提交是没有意义的。提交部分完成的工作比什么都不做要有用得多。

尽管确保每个提交都处于可构建或工作状态是一个好的实践，但是只要您在提交消息中声明了这一点，让“正在进行的工作”提交不准备合并也是可以的。

“少而常”的策略也可以帮助您恢复不需要的功能。从一个更大的提交中提取一小块代码确实很痛苦，但是如果您只需要恢复一个小的更改，就会容易得多。

## **提示 3: Git 壁球**

一个缺点是，所有那些“少量且经常”提交的消息可能会很快变得非常嘈杂。虽然较小的进行中的提交在本地是有帮助的，但是您不希望所有这些提交消息都出现在最终的 Git 历史中。清理历史的一个解决方案是合并多个提交，这就是 [Git squash](https://www.gitkraken.com/learn/git/git-squash) 动作的用武之地。挤压是 [Git rebase](https://www.gitkraken.com/learn/git/git-rebase) 命令的一部分。

GitKraken Client 使选择您想要组合的多个提交变得非常简单，右键单击选择“挤压 x 个提交”，其中 x 是您选择的提交数。

注意不要掩盖你的错误！你的错误和你的返工讲述了一个故事。它们可以为未来的开发人员提供有价值的解释，同时也记录了您的学习成果。我们应该为这些感到骄傲，因为它显示了你在发展你的手艺。不要隐藏错误。用壁球来整理东西，而不是把东西扫到地毯下。

## **提示 4:使用 Git 分支策略**

有许多 Git 分支策略;虽然 Joe 个人最喜欢的是 GitHub Flow，但是还有很多。无论您选择哪一种，Git 分支策略通常都遵循特性驱动的开发模式。您为您正在处理的每个特性创建一个分支，然后有一个特定版本的状态的一些概念。通常，一个 [Git 分支](https://www.gitkraken.com/learn/git/branch)或者一个标签会告诉你部署了什么以及在什么时候部署的。

确保一次只有一个人负责任何一个分支也是很常见的，所以如果你有两个人在处理一个特性，你最好将这个特性分解成子特性。这使得每个开发人员都可以有效地执行“少而多”的提交策略。

GitKraken 客户端具有高级功能，如合并冲突检测，当两个团队成员同时处理同一个文件时，它会提醒用户。与 Git 的合作从未如此之好。

仔细看看 GitHub 流分支策略，您会发现它涉及到为您的特性创建一个分支，该分支映射到一个 GitHub 问题。然后，您将您的提交添加到该分支，打开一个 [GitHub pull 请求](https://www.gitkraken.com/learn/git/problems/github-pull-requests)，引发一场讨论，然后一位同事对您的更改执行一次[代码审查](https://www.gitkraken.com/blog/code-review)。

您可能会添加更多的提交，但是一旦那个 [Git pull request](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git) 过程结束，您就可以部署这些更改以进行测试。一旦测试完成，每个人都满意了，你的代码就被合并到主分支中了。

有理由使用不同的工作流和 Git 分支最佳实践。各有各的优点需要考虑。选择一个作为你的基线，然后使它适应你的组织、你的需求和你的项目。无论你选择哪一个，关键是要有一个分支策略。

## **提示 5:使用 Rebase 和 Merge**

在具有多个并发工作流的较大项目中，您可能会以混乱的分支和合并而告终。分支变得相互关联，很难跟踪发生了什么。GitHub 实际上在创建拉取请求时提供了三个选项:

1.  创建合并提交
2.  挤压和合并
3.  重设基础并合并

“创建合并提交”是不言自明的。如果 pull 请求成功，只需[将建议的分支](https://www.gitkraken.com/learn/git/problems/merge-git-branch)及其变更直接合并到指定的目标分支中。这是最简单的选择，却留下了最乱的历史。

“压缩并合并”选项意味着你的整个特性被压缩到一个提交中，然后直接放到主分支或开发分支上。虽然它非常整洁，但是您丢失了许多关于何时、如何以及为什么进行代码更改的文档。这使得改变变得非常困难，也更难理解为什么会发生一些事情。

“rebase and merge”选项允许您在保持树整洁的同时避免丢失数据。这是乔最喜欢的选择。注意，如果你使用的是 [Azure DevOps](https://www.gitkraken.com/integrations/azure-devops) ，这个选项被称为“半线性合并”，但是它会产生相同的结果。

The “create a merge commit” is fairly self-explanatory. If the pull request is successful, just [merge the suggested branch](https://www.gitkraken.com/learn/git/problems/merge-git-branch) with its changes directly into the specified target branch.  This is the simplest option, but it leaves the messiest history.

The “squash and merge” option means your entire feature gets squashed into one commit and then plopped straight onto the main or developed branch. While it’s very neat and tidy, you lose a lot of the documentation of when, how, and why code changes were made. This makes reversing changes very difficult and it’s harder to see why something might have happened.

The “rebase and merge” option allows you to avoid losing data while keeping the tree clean and tidy. This is Joe’s favorite option. Note that if you’re using [Azure DevOps](https://www.gitkraken.com/integrations/azure-devops), this option is called “semi-linear merge,” but it generates the same result.

你可以[跳到 GitKon 会议视频](https://youtu.be/AN-iaHcXgYI?t=1346)中的 22:26，听听 Joe 对这个选项更透彻的解释。

**改写历史**

对于技巧四和技巧五，您正在重写历史，不是作为一种粉饰历史暴行的行为，而是简单地改变已经被推送到 Git 存储库的内容。维护您的真实历史当然有它的好处，因为它对学习和代码审查很有用。如果你从不篡改你的历史，也不可能真的把它弄乱。但是，要知道，从长远来看，这种方法是以回购可读性为代价的。一个快速的整理过程会让整个历史更加清晰。

## **Rewriting History**

**推动你改写的历史**

安全重写历史的关键是确保在开始篡改历史之前，你已经推送了你的库*。这可以确保您在外部服务器上有备份，并在推送前给您一个复查的方法。重要的是要注意:因为你正在改变历史，你将不得不执行一个强制推动。如果你正在使用 GitKraken 客户端，你会收到一个有用的弹出窗口，有一点警告，确定你真的想做一个强制推重写历史。

GitKraken 客户端让你轻松查看你的 Git 历史，制作和[修改 Git 提交消息](https://www.gitkraken.com/learn/git/problems/git-commit-amend)，挤压提交，利用 Git 让你的提交历史尽可能的好。像 Joe 一样，成为一个真正的 git，使用 [GitKraken Client](https://www.gitkraken.com/git-client) 轻松安全地使用世界领先版本控制系统的高级功能。[今天免费下载 GitKraken 客户端](https://www.gitkraken.com/git-client/try-free)！*

## **Pushing Your Rewritten History**

The key to rewriting history safely is to make sure you’ve pushed your repository *before* you start messing with history. This ensures you’ve got your backup on an external server, and it gives you a way to double-check before you push. It’s important to note: because you’re changing history, you will have to perform a force push. If you’re using GitKraken Client, you will receive a helpful pop-up with a little warning, making sure you really want to do a force push rewriting history.

GitKraken Client makes it easy to see your Git history, make and [amend Git commit messages](https://www.gitkraken.com/learn/git/problems/git-commit-amend), squash commits, and leverage Git to make your commit history the best it can be.  Be like Joe and be a real git about your Git history, and use [GitKraken Client](https://www.gitkraken.com/git-client) to make it easy and safe to use the advanced functionality of the world’s leading version control system.  [Download GitKraken Client free today](https://www.gitkraken.com/git-client/try-free)!