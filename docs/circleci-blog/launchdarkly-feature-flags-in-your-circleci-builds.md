# 如何在 CircleCI 版本中使用 LaunchDarkly 特性标志

> 原文：<https://circleci.com/blog/launchdarkly-feature-flags-in-your-circleci-builds/>

利用 CircleCI 来支持其持续集成和交付的组织了解尽早和经常部署的好处:降低风险、减少冲突和更快地为客户实现价值。将 CI/CD 与特性标志结合起来，允许现代开发团队和组织以他们自己的速度编写和发布代码，同时仍然确保客户和用户获得最佳的体验。

## 代码参考

我们构建了一个名为代码引用的新功能，它可以在您的代码中显示 LaunchDarkly 功能标志的所有实例。我们通过利用一个名为`ld-find-code-refs`的作为 orb 可用的实用程序，以不允许对您的源代码进行启动加密访问的方式构建代码引用。这个`ld-find-code-refs`实用程序扫描您部署的代码，并将代码片段发送回 LaunchDarkly，以轻松确保标记在您期望的位置，并在不再需要标记时确保它们被删除。

## 发射黑暗之球

每次进行部署时，launch crystally orb 都使用`ld-find-code-refs`将更新的代码片段发送回 launch crystally。通过使用 orb 将代码引用嵌入到您的标准部署工作流中，您团队中的每个人都有信心在部署时将所有新代码包装在一个标志中，并且在向所有客户和用户发布新功能时安全地删除了所有功能标志。

## 易于集成

这是一个标准 orb 配置的示例:

```
version: 2.1
orbs:
  launchdarkly: launchdarkly/ld-find-code-refs@1.0.0
workflows:
  main:
    jobs:
      - launchdarkly/find-code-references:
          debug: true
          access_token: '${LD_ACCESS_TOKEN}'
          proj_key: YOUR_LAUNCHDARKLY_PROJECT_KEY
          repo_type: github
          repo_url: YOUR_REPO_URL
          context_lines: 3 
```

在这个例子中，我们有一个配置，将`repo_type`设置为 GitHub，将`repo_url`设置为 GitHub URL。我们建议配置这些参数，以便 launch crystally 能够生成指向您的源代码的引用链接。

在这里了解更多关于 launch crystally 的代码引用[，在这里](https://docs.launchdarkly.com/v2.0/docs/git-code-references)获取宝珠[。](https://circleci.com/developer/orbs/orb/launchdarkly/ld-find-code-refs)

## 包扎

在 launch crystally，我们使用 CircleCI 一天多次部署到生产中。随着我们的成长，我们希望让开发团队更容易确保他们部署的代码在部署时安全地包装在功能标志中，并帮助团队在功能向所有用户和客户公开后删除功能标志。有了代码引用，就很容易确定 Git 存储库中的哪些文件引用了您的特性标志，这使得清理和移除技术债务变得容易。

如果您想了解更多信息，请务必注册参加 4 月 3 日的特别[launch blackly+circle ci 网络研讨会](https://www2.circleci.com/CircleCI-LaunchDarkly-Orbs-Webinar.html)。