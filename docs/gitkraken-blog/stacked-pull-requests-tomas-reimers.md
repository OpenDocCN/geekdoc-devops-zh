# 堆叠拉动请求| GitKon 2022 | Tomas Reimers，Graphite

> 原文：<https://www.gitkraken.com/gitkon/stacked-pull-requests-tomas-reimers>

[https://www.youtube.com/embed/U8wJoxxmskM?feature=oembed](https://www.youtube.com/embed/U8wJoxxmskM?feature=oembed)

视频

### **保持畅通并快速发展的工作流程**

假设你是一家社交媒体公司的软件开发人员。您的公司有帖子，现在想添加评论和反应。评论是对帖子的回应，反应是对评论的回应。

## **大量拉取请求的问题**

作为一名勤奋的开发人员，您为评论特性发布了一个 [Git pull 请求](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git) (PR)。假设你想在你的 PR 评论里增加 2000 行左右；在这种情况下，您有几个选择。

1.  添加对评论的反应。这使得 PR 更长，甚至更不可能被审查。
2.  麻烦一个评论家接受你的公关，这样你就可以合并它，继续发展。在这种情况下，您已经中断了您的团队成员的工作流程，并请求加速评审，这导致了在低质量的评审中错误被合并的风险。
3.  你可以继续做别的事情。

因为当前的特性分支模型要求你从主分支中分支出来，所以当开发人员需要将代码合并回主项目时，他们会感到受阻。

许多开发人员不知道这里实际上有一个隐藏的选项 4:如果您不必从 main 分支会怎么样？相反，如果我们从评论分支分出并开始反应分支会怎么样？

更进一步说，您甚至需要对拉取请求进行注释吗？在大多数情况下，注释描述了一组相关的更改:一个针对数据库，一个针对服务器，一个针对客户端。如果你能把它们分解成单个的 PRs 会怎么样？

## **堆叠拉取请求**

这将产生一组较小的拉请求，其好处众所周知。根据 [GitHub Pull 请求作者指南](https://google.github.io/eng-practices/review/developer/small-cls.html)，较小的 PRs 会导致:

1.  更好的评论，因为评论者不太可能沮丧和盲目盖章。
2.  更快的合并，因为审阅者将能够更快地审阅。
3.  更少的 [Git 合并冲突](https://www.gitkraken.com/learn/git/tutorials/how-to-resolve-merge-conflict-in-git)，因为更少的工作意味着更少的冲突。

所有这些听起来都很理想，那么为什么堆栈式拉取请求工作流没有变得更常见呢？有几个原因:

1.  Git rebases 是一场噩梦。到目前为止，最大的障碍是，如果你需要改变你分支的公关会发生什么。如果你这样做了，任何依赖的分支都需要在新的分支上重定基础，并且任何依赖于那些分支的分支本身也需要重定基础。
2.  很难跟踪依赖关系。对于作者和审阅者来说，一旦您开始管理具有三个或更多拉取请求的堆栈，就很难记住 PR 的位置。
3.  怎么一次合并很多东西？你应该如何用现代的[代码审查](https://www.gitkraken.com/blog/code-review)工具做到这一点呢？您可以合并一个，必须将 back base upstack 放在上面，然后合并下一个。

好消息是有质量开发工具[可以帮助管理堆叠的拉请求工作流。](https://www.gitkraken.com/)

管理栈的工具有很多；例如， [Graphite](https://graphite.dev/) 将自动为您处理重定基础，可视化堆栈，并为合并堆栈提供自动化。

比使用任何特定工具更重要的是开始堆叠您的更改，这样您就可以编写更小的拉式请求，获得更好的评论，并更快地发布特性。

## **获得石墨堆叠& GitKraken** 客户端

除了使用 Graphite 管理堆叠的 PRs 之外，还可以考虑将 [GitKraken 客户端](https://www.gitkraken.com/git-client)添加到您的工具带上。交互式拉式请求管理允许您在不离开应用程序的情况下查看、评论、合并和批准拉式请求。