# 让 GitHub 企业支持成为现实

> 原文：<https://www.gitkraken.com/blog/github-enterprise-support>

你要求的，所以我们在 GitKraken v1.9 中让 GitHub Enterprise 支持成为现实！这意味着有了 GitKraken Pro，您在 GitKraken 中了解和喜爱的所有酷 GitHub.com 集成，现在也可以为您的 GitHub 企业存储库所用。

想了解更多关于 GitHub 企业支持的信息吗？接着读下去！升级到 GitKraken Pro 后，您还可以体验所有其他[酷功能](https://www.gitkraken.com/pro)。

> 开发团队对集成带来的新问题进行了一系列潜在的解决方案，并提出了几个可行的解决方案。

**选项**

## 使 GitHub Enterprise 与我们的 GitHub.com 集成不同的一个原因是，我们不能依赖 GitKraken 的 API 服务器作为客户端和 GitKraken 试图与之交互的服务之间的中介。

GitKraken 使用 OAuth 与 GitHub.com 通信，这意味着该应用程序代表 GitKraken 请求用户允许在服务中为用户执行部分操作。这一切都是可行的，因为 GitKraken 可以向服务证明，它确实是为用户请求这些权限的人，通过使用“秘密”密钥，保存在 API 服务器上，而不是与应用程序本身共享。

在 GitHub Enterprise 的例子中，这种逻辑在某种程度上是不成立的，因为 API 服务器不能真正知道它可能需要认证的所有可能的 GitHub Enterprise 服务器。即使可以，也必须在每个 GitHub Enterprise 实例上生成一个“秘密”密钥。

GitKraken dev 团队针对潜在的 GitHub 企业集成所带来的新问题，研究了一系列潜在的解决方案，并提出了几个可行的解决方案:

The GitKraken dev team went through a litany of potential solutions for the new problems posed by the potential GitHub Enterprise integration and came up with a couple feasible solutions:

**1。发布一个独立的 API 服务器。**

### 一种想法是部署一个独立的 API 服务器，可以为每个 GitHub 企业实例设置，大概是由 GitHub 企业所有者的 IT 部门或负责维护 GitHub 企业实例的任何人设置。这里的缺点是，在用户端(或者至少是他们的 IT 部门)设置这样的东西需要大量的工作。

每个用户的 GitKraken 实例除了已经需要知道 GitHub 企业实例在哪里之外，还需要知道 API 服务器在哪里。我们还必须构建前面提到的独立 API 服务器，测试它，并为它制作文档。基本上，每个人都有很多工作要做。如果你问我，我会说这是非常不合逻辑的。

Each user’s GitKraken instance would also need to know where that API server is, in addition to already needing to know where the GitHub Enterprise instance is. We’d also have to build the aforementioned standalone API server, test it, and make documentation for it. Basically, it’d be a lot of work for everyone. Highly illogical if you ask me.

**2。让 GitKraken 充当 API 服务器，并允许用户系统上的配置文件处理所需的配置。**

### 这个想法有一些非常有趣的成分。这将通过允许在用户的系统上放置一个 JSON 配置文件(可能是由他们的 IT 部门)来实现，该文件将包含允许用户的 GitKraken 应用程序充当 API 服务器的必要信息。这实际上可能听起来比实际简单得多。

OAuth 的一个优点是它允许用户给应用程序权限来代表他们做事情，而不需要与应用程序共享他们的密码。只有当用户没有被强迫在完全由应用程序控制的地方输入他们的凭证时，这才真正起作用(几乎完全违背了 OAuth 的目的)。这意味着 GitKraken 需要将认证过程交给用户可以信任的外部应用程序(通常是他们的网络浏览器)。有趣的部分就在这一点上来了:*当用户完成认证过程时，浏览器如何将信息传回 app？*

有很多方法可以做到这一点，但是最优雅和有趣的方法(以我的拙见)是使用一个定制的协议处理器。我们实际上是告诉用户的系统，如果一个带有指定协议的 URL 被访问(比如`gitkraken://)`)，GitKraken 应该被用来处理这个动作。

这将允许浏览器将身份验证过程生成的代码返回给 GitKraken，因此 OAuth 过程可以在应用程序中完成。

这个流程的过程很简单:用户(或者在更大范围内，用户的 IT 部门)将在 GitKraken 设置目录中添加一个配置文件，然后在应用程序中单击一个`connect`按钮，这与当前的 GitHub.com 集成方式非常相似。这种想法最适合大规模应用，因为 IT 部门愿意并且能够为用户实现流程自动化。

The process for this flow would simply be: the user (or on a larger scale, the user’s IT department) would add a single configuration file within the GitKraken settings directory, and then click a `connect` button in-app, in a fashion very similar to the current GitHub.com integration. This idea works best on a large scale where there’s an IT department willing and able to automate the process for the user.

**3。使用个人访问令牌。**

### GitHub(实际上还有许多其他服务)允许您生成一个令牌，它可以被赋予与 OAuth 令牌相同的权限子集，并且可以以几乎相同的方式使用。一旦我们有了其中的一个令牌，就可以用它来代替 OAuth 令牌，我们可以通过 GitKraken 的 API 服务器执行身份验证过程来获得 OAuth 令牌。

它要求用户进行一些额外的配置，但是考虑到我们需要知道 GitHub Enterprise server 的位置，一些额外的配置是必要的，并且有一些方法可以使配置变得非常容易。

**决定**

IT 辅助配置途径(2)和个人访问令牌(3)是考虑最多的两个选项。我们最终决定将个人访问令牌作为该特性的一个很好的起点。

## 如果说使用 GitHub APIs 教会了我们什么的话，那就是它们非常擅长让很多事情变得非常简单。

这不仅是因为实现相对容易，还因为它不是基于这样的假设，即所有 GitHub Enterprise 用户都有一个 it 部门能够通过创建配置文件并将其注入到用户的 GitKraken 设置中来为他们自动化该过程。

> 个人访问令牌也不排除配置文件的使用，因此，如果对稍微简化的流程有很大的需求，我们可以在未来对更大的团队进行扩展。

使个人访问令牌尽可能简单的另一个重要部分是确保它需要尽可能少的用户交互。不幸的是，我们不得不把用户引导到一个有着太多选项的页面:

如果能通过为用户填写这个表单来简化这个过程，而不是让他们来回跑来跑去以确保他们拥有所有必要的范围，那就太好了。

我们找不到任何文档来支持可能有方法自动检查这些框的理论，但如果使用 GitHub APIs 教会了我们什么，那就是它们真的很擅长让许多事情变得非常简单，甚至在没有文档的情况下。

经过一点小小的改动，我们发现您可以选中这些框并指定一个描述默认值，只需传递几个查询字符串参数。谢谢 GitHub！

**执行**

一旦个人访问令牌到位，GitHub Enterprise 几乎就是 API 级别的 GitHub.com 的替代产品。说真的，我们用来处理获取和更新用户信息、存储库、拉请求和 SSH 密钥的包，只是在指向 GitHub 企业服务器而不是 GitHub.com 时，以及在给定个人访问令牌而不是 OAuth 令牌时才工作。太棒了。这意味着我们添加到 GitHub.com 的大多数未来集成点可能会免费自动在 GitHub Enterprise 中工作。

## **Execution**

Once personal access tokens were in place, GitHub Enterprise was almost literally a drop-in replacement for GitHub.com at the API level. Seriously, the package that we use to handle fetching and updating user info, repositories, pull requests and SSH Keys, just worked when pointed at a GitHub Enterprise server, instead of GitHub.com, and when given a personal access token instead of an OAuth token. Which is awesome! It means most future integration points we add to GitHub.com will likely automatically work in GitHub Enterprise for free.