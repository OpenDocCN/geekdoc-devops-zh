# 为 orbs | CircleCI 创建自动化的构建、测试和部署工作流

> 原文：<https://circleci.com/blog/creating-automated-build-test-and-deploy-workflows-for-orbs/>

距离 CircleCI orbs 的[发布已经过去了 15 周，在](https://circleci.com/blog/announcing-orbs-technology-partner-program/)[orbs Registry](https://circleci.com/developer/orbs)中有超过 405 个 orbs。也就是说每周有 25 个以上的新球体！

令人惊讶的是，这些球体中的大部分都是由您，我们令人惊叹的用户社区，或者是由我们新的[技术合作伙伴计划](https://circleci.com/partners/technology/)中的人们贡献的。然而，CircleCI 已经发布了近 30 个 orb，作为 CircleCI 的第一个社区&合作伙伴工程师，我在过去的几个月里一直致力于为这些 orb 开发一个可持续发展的管道——这个管道将允许在尽可能少的人工干预下进行构建、测试和部署。

这是一个不小的壮举，因为每个球都是不同的。有些安装和配置第三方工具和服务。有些允许您部署到各种云提供商，或者在外部环境中测试您的代码。而且，有些只是现有 CircleCI 配置特性的语法糖，让人们更容易使用我们的平台。

我们可以使用哪种[持续集成](https://circleci.com/continuous-integration/)和持续部署工具来支持所有这些不同类型的 orb 的开发？更重要的是，如何在自己的 orb 开发中利用这个工具呢？在一天结束的时候，没有人希望每次有人将一个拉请求合并到一个 orb 存储库时，坐在他们的计算机前输入`git pull`和`circleci orb publish`。

如果你正在读这篇文章，我会假设你已经知道一些关于球体的知识。也许你已经出版了一本，或者你已经开始在你的 CircleCI 项目中使用它们。如果没有，我鼓励你看看我们以前的一些 orbs 博客帖子！支持工程师 Kyle Tryon 为编写你的第一个球体提供了一些奇妙的[技巧；他还发表了](https://circleci.com/blog/tips-for-writing-your-first-orb/)[一篇关于他创造](https://circleci.com/blog/how-to-make-an-easy-and-valuable-open-source-contribution-with-circleci-orbs/) [CircleCI 的 Slack orb](https://circleci.com/developer/orbs/orb/circleci/slack) ( [GitHub link](https://github.com/circleci-public/slack-orb) )的经历的概述，这是迄今为止我们最受欢迎的 orb。对于那些寻找更基础的入门者来说，[创建球体](https://circleci.com/docs/creating-orbs/)和[重用配置](https://circleci.com/docs/reusing-config/)文档非常有帮助。

在这篇文章中，我将讨论为一个新的 orb 设置理想的自动化 CI/CD 过程。在我的下一篇文章中，我将全面介绍 Azure CLI orb 和我们内置的自动化。

现在，让我们进入新 orb 的自动化流程。这样一个过程可能有什么样的目标？

## 版本控制

CircleCI 位于 DevOps 生态系统的中心，是“基础设施即代码”概念的核心信奉者。orb 是基础设施，表示为代码，因此它们应该像任何其他软件一样被构建、测试和部署。为此，您的 orb 需要驻留在 git 存储库中。

有些球体非常小。你的球可能只会做一件简单的事情。其他的要大得多(查看我们的 [AWS ECS orb](https://circleci.com/developer/orbs/orb/circleci/aws-ecs) )。对于较小的 orb，尤其是那些不太依赖外部服务的 orb，某种 orbs monorepo 可能是合适的；实际上有很多东西可以测试，即使是在有多个 orb 的存储库中。你可以在我们的 [circleci-orbs 知识库](https://github.com/CircleCI-Public/circleci-orbs)中看到这种方法的例子，这里是一些 circleci 发布的第一批 orbs 的所在地。

随着时间的推移，我们很快发现，对于许多更复杂的 orb，例如那些专注于第三方部署的 orb，需要更多的粒度。在这些情况下，1:1-orb:repository 关系是理想的。请记住，我们所有的 CI/CD 逻辑都必须存在于我们的`config.yml`文件中，并且试图在一个配置文件中管理多个大型 orb 的集成测试和自动化发布将很快变得难以承受。

## 多级测试

正如您会在开发管道的多个点上以多种方式测试一个网站或应用程序一样，我们也应该对 orb 做同样的事情。在 orbs 生态系统中，这些不同级别的测试是什么？

套用最初的 orbs SDK 预览版:

1.  **模式验证**:这可以用一个 CLI 命令来完成，它检查 orb 是否是用格式良好的 YAML 编写的，是否符合 orb 模式。
2.  运行时测试:这需要建立单独的测试，并在 CircleCI 构建中运行它们。
3.  **集成测试**:这可能是相当高级的 orb 或专门设计为第三方服务的公共、稳定接口的 orb 所需的唯一测试。进行 orb 集成测试需要定制构建和您自己的外部测试环境。

还有扩展测试的概念:由于 orb 本质上是 CircleCI 配置的抽象片段，我们可以测试给定的 orb 是否生成了我们期望的 CircleCI 配置语法。在实践中，我发现成本效益比有点低，因为不仅需要维护 orb 源代码本身，还需要维护其相应的所需扩展配置语法，然后手动保持两者同步，以便扩展测试保持准确。不过，对某些球体来说，这可能是有帮助的。CircleCI 的解决方案总监 Eddie Webb 发布了一篇关于这种方法的精彩的[解释](https://discuss.circleci.com/t/testing-orbs/26500/2) ( [并附有示例！](https://github.com/eddiewebb/circleci-dmz-orb))在 [CircleCI 讨论](https://discuss.circleci.com/)上的一个测球讨论中。

## 自动化部署

对我来说，这是自动化 orb 开发过程的最终目标。没有这种自动化，我不可能维持 CircleCI 不断增长的球体舰队！

作为一个新的 orb 作者，一开始简单地手动发布可能很有诱惑力。毕竟，我们只是在讨论一些简单的 shell 命令。让我警告你不要屈服于这种诱惑。在内部，我们已经回答了 orb 作者的多个问题，他们想知道为什么在将一个 pull 请求合并到他们的 orb 存储库后，他们在注册表中看不到他们更新的 orb，只是意识到他们从未真正自动化他们的 orb 测试和发布过程的部署部分。**相信我，因为根据我的经验，从一开始就实现自动化比回头再添加要顺利得多。**

自动化 orb 部署有一些细微差别。CircleCI CLI 提供了三种不同的发布生产 orb 的方式:

1.  您可以通过`circleci orb publish`[*将*](https://circleci-public.github.io/circleci-cli/circleci_orb_publish.html) 的`orb.yml`文件直接发布到注册表中
2.  您可以 [*通过`circleci orb publish promote`将*](https://circleci-public.github.io/circleci-cli/circleci_orb_publish_promote.html) 一个先前发布的`dev`版本的 orb 提升为一个语义版本化的生产 orb
3.  你可以 [*增加*](https://circleci-public.github.io/circleci-cli/circleci_orb_publish_increment.html) 一个现有的生产 orb 版本，例如，从`foo/bar@1.1.0`到`foo/bar@1.1.1`(补丁发布)，或者`1.2.0`(小版本)，或者`2.0.0`(大版本)

在实践中，我发现*将*`dev`orb 升级为生产 orb 是最有用的自动化部署机制。它允许您在每次提交时对 orb 源代码进行基本的测试/验证，然后有条件地发布 orb 的`dev`版本，然后在`dev` orb 上运行更广泛的测试，最后，有条件地将您刚刚发布的`dev` orb 升级到生产版本。

## 把所有的放在一起

那么，对于以这种方式构建、测试和部署的 orb 来说,`config.yml`看起来像什么呢？首先，没那么复杂！我们已经将自动化 orb 开发的大部分基本机制抽象成了一个 orb 。要查看功能示例，请查看我们上个月发布的 [Azure CLI orb](https://circleci.com/developer/orbs/orb/circleci/azure-cli) ，因为它足够小和简单，容易理解，但也有点复杂，因为它确实与第三方云提供商 Microsoft Azure 集成。这个 orb 的 [`config.yml`](https://github.com/CircleCI-Public/azure-cli-orb/blob/master/.circleci/config.yml) 包含两个不同的工作流:一个用于基本验证和`dev`发布，另一个用于集成/使用测试和可能的 orb 生产部署。在[的后续文章](https://circleci.com/blog/creating-automated-build-test-and-deploy-workflows-for-orbs-part-2/)中，我将对 Azure CLI orb 和这两个工作流做一个全面的介绍。

Orbs 是 CircleCI 不断增长的开源生态系统的核心，因此，当*你*参与进来时，它们会更好。用我们的`circleci-orbs` [话题标签](https://github.com/search?q=topic%3Acircleci-orbs&type=Repositories)在 GitHub 或 Bitbucket 上标记你的宝珠，这样其他人可以更容易地发现它们，并且请在 CircleCI 讨论的[宝珠部分发表任何问题或评论。](https://discuss.circleci.com/c/orbs)

* * *

阅读更多信息: