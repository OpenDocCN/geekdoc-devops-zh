# 无服务器系列第 1/3 部分-给我代码或给我死亡(由基础设施)|利用

> 原文：<https://www.harness.io/blog/serverless-death-by-infrastructure>

无服务器技术代表了计算领域一个看似巨大的转变。让你变得危险。

帕特里克·亨利在 1775 年发表的著名言论“不自由，毋宁死”可能与技术没有太大关系，但却帮助弗吉尼亚州加入了独立战争。随着弗吉尼亚的加入，这场战役代表了美国的形成和独立战争的开始。那些不熟悉美国故事的人，像许多其他国家一样，想要专制，想要摆脱暴政。既然这是一个技术博客，那么如果现代开发人员想要摆脱基础设施的暴政，会怎么样呢？

## 解放我，没有服务器！

无服务器的巨大吸引力在于，作为软件工程师，我们不必担心基础设施。知道努力的方向和你的方向一样重要。如果你看看 JAVA/JEE 市场的发展，你会发现过去的几年里，你会构建一个应用服务器。应用服务器的调优很重要，作为一名软件工程师，必须考虑到这一点，而且由于看门人的原因，还必须按照特定的时间表对项目进行迭代。为了让您的应用程序进入生产环境，历史上有一些把关人员要求您与这些团队保持同步。那些保持基础设施运行和调整的人。在 JAVA/JEE 世界中，这将是你的[中间件工程师](https://en.wikipedia.org/wiki/Middleware_analyst)。随着 [Kubernetes containers](https://harness.io/blog/what-is-kubernetes-container/) 和更便宜的计算基础设施的出现，通过通用基础设施扩展工作负载变得更加容易。尽管如此，如果我们要部署一个容器，还是有很多基础设施可以让我们的代码变成某种可消费的形式，例如 Kubernetes。有了[无服务器或函数](https://harness.io/blog/dude-wheres-my-server-ci-cd-for-serverless/)，你可以随时运行你的代码。想象一下，你总是在你的本地机器/笔记本电脑上，不需要等待一个漫长的构建来运行你刚刚写的东西。无服务器实现的低开销意味着通常每个用户或系统请求都有一个无服务器调用。把你的想象力再拓展一点，如果你可以让多个人访问你刚刚写的代码，然后只运行你的代码，会怎么样；我的朋友没有服务器。

## 您的首款无服务器产品

在无服务器/功能即服务提供商中当然有选择，但是对于我们的例子，我们将使用非常成熟的 [Amazon Web Services Lambda](https://aws.amazon.com/lambda/) 。无服务器功能通常有一个三方生命周期；触发、工作和输出。[触发器](https://docs.aws.amazon.com/lambda/latest/dg/lambda-services.html#intro-core-components-event-sources)是调用你的代码的东西。在我们的例子中，我们将只点击一个 URL。工作是你所有的努力工作，也就是你想要执行的魔法或功能。产出是你努力的结果；例如，可以触发其他事情或将项目保存到数据库中。喜欢总是可以看这个视频或通过博客，或两者兼而有之！

**导航至 AWS** 别担心，我们的示例将位于 AWS Lambda 的[自由层。如果这是您第一次使用 AWS，这个例子非常简单。前往](https://aws.amazon.com/lambda/pricing/) [AWS 控制台](https://aws.amazon.com/console/)(这是 [Shadow IT](https://www.gartner.com/it-glossary/shadow) 升起的方式)并登录或注册。比如买下 Amazon.com 的所有东西？您的 Amazon.com 凭据可以在亚马逊网络服务上使用。注意，构建 AWS 服务非常容易。你可能会无意中开始一些花钱的事情。降低云成本对许多组织来说都是一个挑战，从这个例子的简单程度可以看出人们为什么选择公共云。如果你坚持这个例子，减少资源，你将会很好，并且没有成本。**启动 AWS 示例**一旦你进入控制台，就可以查看[无服务器微服务示例](https://console.aws.amazon.com/lambda/home?#/create/configure-triggers?bp=microservice-http-endpoint&quickstart=true)。我们将利用快速启动。

但是，如果该示例离开了控制台快速入门区域，在创建新的 Lambda 时，总是可以浏览到 Compute 下的 Lambda 服务并搜索 blueprint。

**您的 Lambda 阻力最小的路径**我们可以在学习时使用快速入门中的默认值。我们可以将第一个函数称为“my-first-function ”,并填写一些默认值。默认情况下，应该选择 [DynamoDB](https://aws.amazon.com/dynamodb/) 策略 Lambda 与 DynamoDB 交互的示例]。

**使用默认设置的触发器**将通过为我们创建的[亚马逊 API 网关](https://aws.amazon.com/api-gateway/)进行连接。我们现在将使用“开放”安全性。通过快速入门，我们可以使用 [API 键](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-setup-api-key-with-console.html)或 [IAM](https://aws.amazon.com/iam/) 锁定。完成后，请确保删除您的函数，您应该可以“打开”了。

**某个节点。快速入门模板是一个[节点。如果您以前没有使用过 NodeJS，这是一个简单的学习方法，不需要在您的机器上设置 NodeJS 并立即看到您的结果。](https://docs.aws.amazon.com/lambda/latest/dg/programming-model.html)**

**点击橙色按钮**点击“创建功能”，你就可以开始比赛了。

使用您的第一个函数进行检查！几分钟后，你的功能和一些后台服务[ [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) 和 [Amazon CloudWatch](https://aws.amazon.com/cloudwatch/) ]也将启动。太棒了。

您可以点击 API Gateway 框并获得您的端点地址。

**与你的第一个函数互动！**随着您可信赖的 API 端点的启动和运行，您可以开始与您的第一个无服务器功能进行交互。可以通过发送 HTTP 请求以几种方式进行交互。你可以利用你喜欢的 API 测试工具，比如 [Postman](https://www.getpostman.com) 或者 [SoapUI](https://www.soapui.org/) 。下面我用的是邮递员。易于注册和使用。可以将 GET 请求连接到您的端点。添加查询字符串“TableName”和一个表名，它是 Lambda 中 NodeJS 的下一行。

输入 Lambda 的 API 地址，将“TableName”查询参数添加到 Get 请求中，然后点击 send。嘭，给你。尽管我们得到了 400 个回复？那很好，我们在 DynamoDB 中没有任何表。可以检查 HTTP Puts 如何工作的 NodeJS 代码。恭喜你，你刚刚和你的第一个 Lambda 互动了！

使用新的 NodeJS 向 DynamoDB Lambda 添加数据库条目肯定会很有趣。如果你真的很感兴趣，可以学习一下 [DynamoDB 如何将](https://docs.aws.amazon.com/lambda/latest/dg/with-ddb-example.html#with-ddb-create-buckets) wire 流式传输到 Lambda 来解构这个例子是如何创建的。可以在 [AWS CloudWatch](https://aws.amazon.com/cloudwatch/) 中验证我们进行了调用。

## 打扫

确保我们不会收到 AWS 账单，可以导航到已启动的服务并删除项目。转到 AWS 控制台并选择每个服务。在这种情况下将是 Lambda、API Gateway 和 IAM。**λ:**

**API 网关【点击进入 API 名称】:**

**Lambda IAM 的作用:**

这应该会消除您的第一个 Lambda 和 API 端点的附加资源。就像你现在的无服务器忍者，几分钟内创建一个 Lambda，删除一个 Lambda。

## 用背带装上你的无服务器游戏

现在你已经创建了你的第一个 Lambda，你可以用 Harness 升级你的游戏了。将 Lambda 作为管道的一部分至关重要。Lambdas 易于更改和重新部署，因此很容易忘记我们的 SDLC 原则。使用无服务器做一些很酷的事情？我们很想听听你在 [Harness 社区](https://community.harness.io/)的创意。查看我们的 [Lambda 文档](https://developer.harness.io/docs/first-gen/first-gen-quickstarts/aws-lambda-deployments/)，看看 Harness 如何增强你和你的团队的无服务器体验。如果你没有脊甲账号，[可以注册一个免费的吗](https://harness.io/try-continuous-delivery-as-a-service-for-free/)。干杯！拉维