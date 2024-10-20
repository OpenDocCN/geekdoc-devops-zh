# 什么是 GitOps？|了解方法、工作流程和工具

> 原文：<https://www.gitkraken.com/blog/what-is-gitops>

这篇文章是一位客座作者写的。

不久前，如果我们想将代码投入生产，我们需要手动配置服务器，即我们的基础设施，来托管我们的应用程序或数据库。这种手动过程不仅耗时，而且容易出错。这就是为什么目前开发人员选择创建负责配置基础设施的“脚本”。这些“脚本”被称为代码基础设施(IaC)。

随着应用程序的发展，我们的代码基础设施也在发展。简单的 Bash 脚本或 Python 脚本已经不够了；这就是为什么像 Terraform、Ansible、Chef 或 Puppet 这样的工具来拯救我们。所有这些工具都允许我们轻松地指出我们的基础设施需要执行的指令，以便为托管我们的代码做好准备。

这就是为什么今天，IaC 是标准。它让我们的生活变得更容易，但是当…

*   我们有这种令人敬畏的基础设施作为代码，可以自动化这么多事情，但我们仍然在我们的云提供商( [GCP](https://cloud.google.com/) 、 [AWS](https://aws.amazon.com/) 、 [Azure](https://azure.microsoft.com/) 等)中手动运行它们？

*   我们对它进行了修改，然后不经过代码审查或测试就直接运行了？

可以想象，这是一个缓慢的过程，在这个过程中，很多事情都会出错。

我们能为 IaC 做的最好的事情就是检查它，测试它并自动化部署过程。简而言之，使用 GitOps 实现代码❤️.一样漂亮的基础设施

## **什么是 GitOps？**

GitOps 是交付 IaC 的现代方法。在这种方法中，我们使用 Git 来跟踪代码的发展，我们审查变更，我们批准它们，然后我们部署这些变更，就像我们处理应用程序代码一样。

GitOps 流程并不复杂，所以没有理由不使用它。

GitOps 的流程如下所示:

在这里，您可以看到 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 处于暂存和提交阶段，因为它允许我们暂存和提交我们的更改。此外，GitKraken 将允许我们打开一个拉请求， [GitHub](https://www.gitkraken.com/integrations/github) ， [GitLab](https://www.gitkraken.com/integrations/gitlab) ，以及 [Bitbucket](https://www.gitkraken.com/integrations/bitbucket) 。

GitKraken 在 GitOps 流程中起着至关重要的作用，并且能够在生产流水线的其他阶段之间实现无缝过渡。

对于代码开发人员来说，这个流程听起来很熟悉，对吗？但是流动并没有就此结束；我们需要将 IaC 放在我们的服务器集群中。我们有两个选择来实现这一点:

1.  推送模型
2.  拉模型⭐️

**推送模式**

### 这时，我们的 CI/CD 服务器在集群上执行一个命令来猜测…将新代码推送到部署环境。🥳

**拉动模式**

在另一个模型中，我们在集群中安装了一个代理(如 Argo CD 或 Flux CD)。这个代理负责从我们的存储库中提取变更。🥳🥳

### **Pull Model**

**gitop 的工具**

你可能在想:什么工具能够让 GitOps 流发生？

嗯，我已经(随机)提到了其中的一些，但我会列出一些行业内的标准。

## 当然，Git 是这个流程的基石。记住，GitOps 是关于交付高质量的代码。Git 将允许我们跟踪和观察代码的发展。

当您完成对 IaC 的更改后，您可以在提交之前使用 [GitKraken Git GUI](https://www.gitkraken.com/git-client) 轻松查看它们。

此外，GitKraken 允许您轻松检查文件中的更改历史。下面显示的是 GitKraken Diff 视图，它使用颜色来显示添加和删除的行，因此您可以快速了解您正在查看的内容。

Of course, Git is the cornerstone for this flow. Remember, that GitOps it’s about delivering quality code. Git will allow us to track and observe the evolution of our code. 

When you finish your changes to your IaC, you can effortlessly review them with the [GitKraken Git GUI](https://www.gitkraken.com/git-client) before you commit them. 

提交后，你可以从 GitKraken 打开一个 [Git pull 请求，不管你使用的是 GitHub、GitLab 还是 Bitbucket。同样在 GitKraken 中，您可以查看团队成员创建的拉动请求。](https://www.gitkraken.com/learn/git/tutorials/what-is-a-pull-request-in-git)

💡不需要浏览器:从 GitKraken，你可以批准或者评论 [GitHub pull 请求](https://www.gitkraken.com/learn/git/problems/github-pull-requests)。这样节省了很多时间。

*GitKraken File History – Diff View*

对于**推模型，**我们可以使用: [Jenkins](https://www.jenkins.io/) 是一个开源的自动化服务器，它使世界各地的开发人员能够可靠地构建、测试和部署他们的软件。

对于**拉模型，**我们可以使用: [ArgoCD](https://argo-cd.readthedocs.io/en/stable/) 是一个持续部署工具，它只通过执行拉来实现对存储库的监控。

有了 GitKraken 的力量在你身边，在代码审查中没有任何变化会逃过你的眼睛！你可以从这里下载 GitKraken。

For the **Push Model,** we can use: [Jenkins](https://www.jenkins.io/) is an open source automation server that enables developers around the world to reliably build, test, and deploy their software.

准备好潜入 GitOps 了吗？

请记住，如果您的 IaC 不如您的应用程序代码好，您的基础设施很可能会失败。没有基础设施，任何人都不能使用您的应用程序。因此，最好通过只引入经过测试和审查的代码来保证 IaC 的安全。

在您的项目中实现 GitOps 流并不复杂，即使您以前从未使用过 Git，也有一些工具可以让它变得简单得多，比如 GitKraken。使用 GitOps 带来了许多其他优势，例如:

## 质量代码

自动化过程

对所有团队透明

*   快速部署和回滚
*   另一方面，你可能已经在你的项目中应用了 GitOps，但是如果你还没有使用 GitOps，那么是时候开始了。🚀
*   Transparency to all the team
*   Fast Deploy and Rollback

你可以在作者个人网站上用西班牙语阅读这篇帖子: [*Qué es GitOps？*](https://www.tanx.dev/2021/09/01/que-es-gitops.html)

On the other hand you may be already applying GitOps to your project, but if you are not using GitOps yet, it’s definitely time to start. 🚀

如果你用正确的工具装备自己，在 GitOps 取得成功要容易得多。GitKraken 将帮助完成 GitOps 流程的多个阶段，包括准备、提交、拉请求和代码审查。

You can read this post in Spanish on the author’s personal website: [*¿Qué es GitOps?*](https://www.tanx.dev/2021/09/01/que-es-gitops.html)

Succeeding at GitOps is far easier if you equip yourself with the right tools. GitKraken will help fulfill multiple stages of the GitOps flow, including staging, committing, pull requests, and code review.