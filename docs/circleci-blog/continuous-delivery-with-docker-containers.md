# 宣布 Docker containers - CircleCI 连续交货

> 原文：<https://circleci.com/blog/continuous-delivery-with-docker-containers/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

容器是云基础设施中的新标准，CircleCI 上的 Docker 允许您用它们构建整个 CI 和 CD 工作流。

有什么新消息？您现在可以在我们的执行环境中使用 Docker 的所有功能。所有常用的 Docker 命令都按预期工作，因此您可以随心所欲地构建和运行 Docker 容器。

![circle+docker](img/05ccd0bce48402ae14629eb0e1c5521d.png)

为什么这很酷？ Docker 容器允许您删除测试和生产环境中几乎所有不同的变量。您可以在 Docker 文件中指定从 Linux 发行版到启动时运行的可执行文件的所有内容，将所有这些信息构建到 Docker 映像中，对其进行测试，并将完全相同的映像逐字节部署到生产环境中。你现在可以在 CircleCI 上运行整个过程。

**在幕后**我们已经使用容器技术来创建我们所有客户的执行环境，并提供 docker 集成，我们必须支持 docker 在我们的多租户环境中安全运行，并在我们自己的容器中以嵌套模式运行(始终是 docker-in-docker！)，这并非完全无关紧要。我们还进行了第三方安全审查，并花了一些时间让 Docker 在 CircleCI 上通过测试，包括我们自己的测试和有限的测试。

**使用 CircleCI 上的 Docker**为了帮助任何对 CircleCI 上的 Docker 感兴趣的人快速入门，我们有[完整的文档](https://circleci.com/docs/1.0/docker/)，涵盖了将映像部署到 Docker Hub 以及将应用程序持续部署到 AWS Elastic Beanstalk 和 Google Compute Engine 上的 Kubernetes。

下一步是什么？Docker 是一个如此灵活的工具，我们知道有许多令人兴奋的应用程序是我们甚至还没有想到的。如果你认为你有一个特别棒的 Docker 的用法，你想向全世界炫耀，那么请发一封电子邮件给 kevin@circleci.com，让他在我们的博客上发表一篇关于它的文章。在那之前，祝大家快乐！

*[在 HN 上讨论这个！](https://news.ycombinator.com/item?id=8143424)*