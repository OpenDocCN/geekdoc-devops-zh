# 无服务器系列第 3/3 部分-无服务器和线束-更好地结合在一起|线束

> 原文：<https://www.harness.io/blog/serverless-and-harness>

在第一部第一集第一集和第二部第三集《T2》之后，我们在这个没有服务器的世界开始变得危险。我们已经创建了我们的第一个或前几个 [AWS Lambda 函数](https://aws.amazon.com/lambda/)，并且知道围绕利用无服务器实现的架构问题。有了无服务器实现，尤其是 AWS Lambda，动态编辑变得很容易。让我们不要忘记我们的 SDLC 规程。

## 不要忘记 SDLC！

还记得商业控制限制人们生产的原因吗？主要原因是为了避免在生产中进行热修补。同样的理由也适用于λ。尽管与传统系统相比，修补或修改 Lambda 非常容易。我们可以直接修改 Lambda 将要运行的代码。如果我们反思我们在第一部分中调用的 Lambda，我们可以用 AWS Lambda 编辑器编辑 NodeJS 脚本，哦，我的天，或者哦，是的！由于 Lambda 易于修改，重要的是不要忘记我们的 SDLC 规程。通过将 AWS Lambda 集成为您的线束管道的一部分，您可以确保遵循您的 SDLC /连续交付流程所灌输的勤奋和信心。

## 你的第一次 Lambda 集成

如果您还没有，您可以在 Harness 平台中将 [AWS 设置为云提供商](https://developer.harness.io/docs/first-gen/firstgen-platform/account/manage-connectors/add-amazon-web-services-cloud-provider)。尽管这需要安装一个线束代理，T2 可以通过几种方式安装在 AWS 上。我最近在 Harness 社区上发布了如何安装并让 [AWS ECS Fargate 安装并运行一个 Harness 委托](https://community.harness.io/t/aws-ecs-fargate-delegate-install-from-0-to-hero/182)。喜欢总是能摘你的毒药；浏览帖子或观看视频。

在我们尊敬的销售工程师 Greg Kroon 的帮助下，让我们来看看他简单而优雅的例子。在这个例子中，您将需要设置一个 [AWS S3 桶](https://aws.amazon.com/s3/)来存放我们的示例 NodeJS Lambda。能为水桶想出一个有创意的名字。

在您创建了 S3 桶之后，您可以上传 [Greg 的简单 NodeJS](https://github.com/gregkroon/lambdatraining) 函数。可以从 GitHub 下载[的压缩文件并上传到你的 S3 桶中。](https://github.com/gregkroon/lambdatraining/raw/master/hello-world.zip)

上传之后，我们应该得到我们的对象 URL，在我的例子中是“*https://S3 . amazonaws . com/harness . first . lambda . ravi/hello-world . zip*”。因为我们在 AWS 中，如果您没有身份和访问管理[IAM]角色来在[管理委托](https://developer.harness.io/docs/continuous-delivery/onboard-cd/cd-quickstarts/serverless-lambda-cd-quickstart/)或另一个现有角色上执行 Lambda，您可以提前创建一个。

我们现在可以创建一个新的[线束应用](https://docs.harness.io/article/bucothemly-application-configuration)，例如，My Lambda。可以在设置时创建新的应用->+添加应用。

现在在 My Lambda 应用程序中，让我们创建一个名为 Lambda Service 的[服务](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/setup-services/add-service-level-config-variables/)。将 Lambda 服务创建为 AWS Lambda 部署类型。这可以通过进入设置- >我的 Lambda - >服务找到

一旦创建了 Lambda 服务，我们就可以开始定义 Lambda 部署规范了。例如，将目标运行时设置为 NodeJS 8.10，内存大小为 128mb。我们还可以将执行超时设置为三秒。对于函数名，将函数命名为“HelloWorld”，将处理程序命名为“hello-world.handler”。

接下来让我们添加工件源为 S3。既然已经上传了神器，那就简单链接吧。

既然我们已经将 Lambda 服务作为一个线束服务，让我们创建一个线束[环境](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/environments/environment-configuration/)来部署。让我们创建一个非生产线束环境。可以通过进入设置- >我的 Lambda - >环境导航到环境

一旦创建了环境，我们现在就可以连接服务基础设施。确保选择适当的 IAM 角色和区域。

现在我们可以创建一个包含新 Lambda 的工作流。可以通过设置- >我的 Lambda - >工作流进行配置。我们将创建一个基本部署类型。选择环境、服务和服务基础设施。

在查看我们的基本部署工作流时，让我们添加一个验证。让我们用 AWS Lambda 验证来寻找有效载荷。

接下来可以添加有效负载为“{”status“:“OK”}”的 AWS Lambda 验证。

现在，您的工作流应该如下所示:

对于我们的工作流，让我们创建一个管道来执行工作流。可以通过进入设置- >我的λ->管道来设置线束[管道](https://developer.harness.io/docs/platform/templates/create-pipeline-template/)

我们可以在这里连接我们的基本部署。

一个非常简单的一级管道！

可以在管道之外开始新的线束部署。导航到连续部署并开始新的部署。

一旦你点击提交，奇迹就开始了。

成功！

看一个新的λ出现了！

## 无服务器和线束——真正的完美结合

希望您喜欢我们关于无服务器的三部分系列。现在你在无服务器的世界里更加危险，可以看到对一个健壮的管道的需要。有了 Harness，您可以通过持续的交付渠道轻松树立信心。请务必关注我们即将于 10 月 4 日举行的网络研讨会，届时我们将讨论所有无服务器的事情，并查看我们的[官方文档](https://developer.harness.io/docs/first-gen/first-gen-quickstarts/aws-lambda-deployments/)。干杯-拉维