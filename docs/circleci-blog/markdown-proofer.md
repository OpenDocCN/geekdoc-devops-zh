# 我们如何使用新的 markdown proofer - CircleCI 发现并修复文档中的 11 个错误

> 原文：<https://circleci.com/blog/markdown-proofer/>

静态站点生成器(SSG)如[雨果](http://gohugo.io/)和[杰基尔](https://jekyllrb.com/)如今风靡一时。这些后端的静态和前端网站的 JavaScript 被称为 JAMStacks。通常，我们用两种方式测试它们:

*   经由 SSG 成功地建立了网站
*   和 [HTMLProofer](https://github.com/gjtorikian/html-proofer)

如果我们想做得更多呢？让我们来看看我为测试 markdown 文件制作的新工具，以及它如何提高 CircleCI 文档示例的准确性。

作为 CircleCI 的开发人员，Hugo 社区的成员，以及 CircleCI docs ( [用 Jekyll](https://github.com/circleci/circleci-docs) 构建)的经常撰稿人，我一直在思考改进静态站点测试的方法。拥有一个定制的小型 Docker 映像可以缩短构建时间，如果可能的话，并行化作业也会有所帮助。更多的原始测试来确保页面看起来和工作正常是最重要的，但有时也是最难做到的。CircleCI 客户对 CircleCI Docs 的频繁反馈给了我一个想法。

## 问题是

像大多数技术文档一样，CircleCI 文档是用 Markdown 编写的，包含数百个示例。这些例子是用 GitHub 风格的 Markdown [fenced 代码块](https://help.github.com/articles/creating-and-highlighting-code-blocks/#fenced-code-blocks)编写的。我们的大多数例子都是使用 YAML 语法的片段或完整的 CircleCI 配置文件。许多用户在阅读我们的文档时，会看到一些对他们有用的 YAML 示例，自然会将&复制粘贴到他们自己的配置中。然后我们破坏了他们的配置。

怎么会？

YAML 非常严格。契约意味着一切。缩进量告诉 YAML 解释器如何解组所有数据。错误的缩进可以创建或破坏配置文件。此外，YAML 规范很久以前就做出了——在笔者看来——只通过空格缩进的糟糕决定。YAML 行开头的制表符是语法错误。

回到 docs，当 CircleCI docs 团队、其他员工甚至社区成员为 repo 添加或更新示例时，有时会出现这些错误。这可能是一个简单的人为错误——我们都会犯错——或者有时可能是由文本编辑器在不应该的时候假设了一些事情造成的。无论哪种方式，人眼可能看不到的语法错误(制表符看起来像空格)被合并进来，稍后用户出现，复制它，然后出现错误。可以理解的是，这可能会导致挫败感，而 HTMLProofer 这个伟大的工具却无法捕捉到这一点。

## 解决方案

除了 HTMLProofer，还有其他工具可以测试网站，其中一些可以覆盖 markdown 文件。为了帮助提高 CircleCI 文档、我的个人项目和其他静态网站的质量，我写了一个新工具，叫做 [Markdown Proofer](https://github.com/felicianotech/md-proofer) 。这是一个用 Go 编写的小型开源命令行界面(CLI ),旨在测试 markdown 文件。受 CircleCI docs 的启发，Markdown Proofer 的第一个也是迄今为止唯一的功能是在 Markdown 文件中寻找 YAML 防护代码块，然后验证 YAML 以确保其语法正确。这允许一些人在他们的 markdown 中自动测试 YAML 代码块，有希望在他们的站点中产生更少的充满错误的例子。让我们用 CircleCI docs 测试这个新工具。

## 使用降价检验器测试 CircleCI 文档

在推出 Markdown Proofer 的第一个版本 v0.1.0 后，我在 CircleCI docs 中创建了一个 PR，将其添加到构建过程中，看看我们会发现什么。正如所料，由于 Markdown Proofer 发现了几个错误，PR 构建失败。你可以在 GitHub [这里](https://github.com/circleci/circleci-docs/pull/2323)看到这个 PR，在这里看到失败的 CircleCI build [。](https://circleci.com/gh/circleci/circleci-docs/6270?utm_campaign=vcs-integration-link&utm_medium=referral&utm_source=github-build-link)

所以，太好了！该工具完成了它的工作，并发现了一些错误。现在我需要修复它们，这样构建就可以通过，我们就可以正式将 Markdown Proofer 放入 CircleCI docs 的`master`分支。我开发了一个新的 PR 来修复所有被发现的 YAML 错误，并且，不算任何表面上的改变，能够修复我们 YAML 例子中的 11 个错误。这个 PR 可以在 GitHub [这里](https://github.com/circleci/circleci-docs/pull/2324)找到。

随着所有 PRs 的合并，CircleCI docs 的配置示例中修复了 11 个 YAML 错误，未来的 YAML 错误将被捕获，而无需手动检查所有内容，并且仅增加了 1 秒或更少的总构建时间。虽然不是开创性的，但我能够在 CircleCI 文档构建过程中添加一个小工具，以提高我们提供给用户的文档的质量。听起来我赢了。

## 下一步是什么？

### 用于减价打样机

Markdown Proofer 仍然是一个全新的工具，只检查 YAML 围栏代码块。在不久的将来，我计划:

*   添加对 JSON 代码块的支持。
*   添加对 JavaScript 代码块的支持。
*   使用 CircleCI [本地 CLI](https://circleci.com/docs/local-cli/) 验证 CircleCI 配置示例(一个完整的文件),不仅验证 YAML 语法，还验证 CircleCI 配置方案。
*   对 CLI 输出和 README.md 进行更多润色。
*   改进错误消息(Go YAML 库没有提供很好的消息传递)。

由于 Markdown Proofer 是开源的，您可以跟踪项目并公开问题或在 GitHub 上提交 PRs。在这里找到它[。](https://github.com/felicianotech/md-proofer)

### 对于静态网站测试

许多网站只是使用 HTMLProofer 进行测试，我们可以做得更多。也许 Markdown Proofer 是你的事，也许不是。用 [Selenium](https://www.seleniumhq.org/) 测试你网站的 UI 是另一种测试大多数网站的方法。了解需要什么来[获得测试过程的更好日志](https://circleci.com/blog/without-logs-did-it-actually-happen-logging-selenium-browser-testing-on-circleci-2-0/)以帮助解决挂起和失败的构建。还有针对您所拥有的内容类型的独特测试。例如，如果您有一个普通的 RSS 提要、一个 sitemap.xml 文件、一个播客提要等等。有一些聪明的方法来测试这些页面，以确保它们不只是返回一个 HTTP 200“Status OK”响应，而是实际工作并有意义。

如果你有自己的想法或任何问题，请在[circle ci discuse](https://discuss.circleci.com)上继续讨论。