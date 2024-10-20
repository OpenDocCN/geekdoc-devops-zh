# 使用 Newman orb | CircleCI 运行邮差收集

> 原文：<https://circleci.com/blog/set-up-a-circleci-pipeline-to-run-a-postman-collection-using-the-newman-orb/>

软件业正越来越趋向于 API 优先的世界。今天构建的大型软件系统是由 API 组成的。 [Postman](https://www.getpostman.com/) 已经成为软件开发人员和测试人员设计、测试和发布 API 的可信工具。全球超过 600 万开发人员和 20 万以上的组织在其 API 开发生命周期的每个阶段都使用 Postman。

Postman 中的工作流围绕着收藏。集合是一组 HTTP 请求，它们可以有自己的测试、响应示例和文档。您可以直接从集合中生成 API 文档。保存的响应示例构成了为 API 创建模拟服务器的基础。您可以通过团队工作区在所有这些方面进行协作。

Postman 也有能力执行这些集合。运行集合会逐个触发集合中的所有请求。您还可以编写脚本来断言响应、控制执行顺序，以及在一次运行中在请求之间传输数据。当您想要测试 API、对 API 工作流执行端到端测试和/或对 API 进行健康检查时，这是非常有用的。你说吧。

## 在 CI 环境中执行集合

Postman 的一个非常常见的用例是在 CI 管道中运行集合。团队对他们的服务运行契约测试、端到端测试、安全检查或基础设施检查，作为他们构建或发布管道的一部分。这就是 Newman 命令行集合运行器有用的地方。

Newman 是一个 Node.js 应用程序，可以从本地文件系统或远程 URL 运行一个集合。纽曼还提供了多种报告选项。您可以生成 json、xml 或 html 输出形式的收集运行报告。当您将 Newman 与 CI 环境集成时，您可以根据您的 API 是否按预期执行来控制您的构建阶段。

## 一个 orb 来帮助简化您的收集运行

虽然 Newman 是作为 Docker 容器发布的，但是设置这些构建阶段可能非常耗时。借助 CircleCI 的 [Newman orb](https://circleci.com/developer/orbs/orb/postman/newman) ,我们已经构建了一个现成的集成，您只需进行一项配置，就可以在 CircleCI 管道中触发收集运行。

orb 将负责设置 Newman，执行您在配置中提到的集合，并将运行报告存储在适当的位置。

## 配置 orb

Newman orb 公开了可以在 CircleCI 配置中使用的`newman/newman-run`命令。您可以将您的集合从 Postman 导出为 JSON 并保存在您的存储库中，或者您可以使用您的集合 ID 和 API 键直接调用 Postman API，并对最新版本的集合运行 Newman。

假设您已经将您的收集文件导出到存储库根目录下的`./collection.json`,那么您的 CircleCI 配置应该是这样的:

```
version: 2.1
orbs:
  newman: postman/newman@0.0.2
jobs:
  newman-collection-run:
    executor: newman/postman-newman-docker
    steps:
      - checkout
      - newman/newman-run:
          collection: ./collection.json 
```

`newman/newman-run`命令接受许多传递给 Newman 的参数。以下是您应该知道的一些重要参数:

*   `collection`:如上例所示，这是唯一需要的参数。它采用一个指向集合的 JSON 文件的路径或一个可以访问集合的 URL。
*   `environment`:这需要一个从 Postman 导出的环境的路径，或者一个环境的 URL。
*   `iteration-data`:如果您需要在采集运行中使用 CSV 文件中的外部数据，此参数将获取 CSV 文件的文件路径或 URL。
*   `folder`:要在收藏中运行的文件夹的名称。当您想要将收集运行范围扩大到收集中的特定文件夹时，可以使用此选项。

除此之外，你可以在 [orb 的文档页面](https://circleci.com/developer/orbs/orb/postman/newman)上查看所有被接受的参数。

## 概括起来

将 CircleCI 的强大功能与 Postman 和 Postman API 结合起来，您可以为您的 APIOps 和 API 测试构建完美的工作流。我们希望这个 orb 能够帮助您在 API 自动化之旅中走得更远。

在我们将于 2019 年 5 月 14 日上午 10:00 PST/18:00 UTC 举行的网络研讨会“使用 CircleCI 和 Postman 简化端到端 API 开发”期间，了解更多关于 CircleCI 和 Postman 的信息。在这里报名[。](https://www2.circleci.com/CircleCI-Postman-Orbs-Webinar.html)