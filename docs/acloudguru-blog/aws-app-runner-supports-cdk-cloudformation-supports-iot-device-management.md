# CloudFormation 支持物联网设备管理

> 原文：<https://acloudguru.com/blog/engineering/aws-app-runner-supports-cdk-cloudformation-supports-iot-device-management>

本周 AWS 怎么了？本周，App Runner 现在支持 CDK 构建和部署应用程序，Step Functions 工作流现在支持 PrivateLink，CloudFormation 现在支持物联网设备管理！敬请关注本周 AWS 的完整报道！

请继续阅读所有细节！

* * *

## 加速您的职业发展

[从 ACG 开始](https://acloudguru.com/pricing)通过 AWS、Microsoft Azure、Google Cloud 等领域的课程和实际动手实验室改变你的职业生涯。

* * *

## App Runner 支持 CDK

本周早些时候，AWS 宣布 App Runner now [支持使用 AWS 云开发套件(CDK)](https://aws.amazon.com/about-aws/whats-new/2021/11/aws-app-runner-aws-cdk-build-deploy-applications/) 来构建和部署您的应用。

AWS App Runner 是一个全面管理和自动扩展的服务，允许大规模快速部署容器化的 web 应用程序和 API。

AWS CDK 是使用 Python 和 Node.js 等语言的单一启动源。现在，您可以通过指定源代码位置(如亚马逊弹性容器注册中心(ECR) Public and Private，或 CDK 的服务构造中的 GitHub)来创建应用程序运行器资源。您还可以使用 CDK 来指定那些资源需要使用的 [IAM 角色](https://docs.aws.amazon.com/cdk/api/latest/docs/aws-apprunner-readme.html)。

## 步骤功能工作流现在支持 PrivateLink

AWS 步骤功能的同步快速工作流现在支持 PrivateLink！

此更新现在允许您的工作流程从您的 VPC *开始，而无需*穿越公共互联网空间。Step Functions 帮助您构建分布式应用程序、自动化流程并构建机器学习管道，快速响应，无需轮询其他服务或创建定制解决方案。

现在有了 PrivateLink 支持，您可以在 VPCs、AWS 服务和本地网络之间建立私有连接，并降低 DDoS 攻击或中间人攻击的风险。您将需要创建一个新的 [VPC 端点](https://docs.aws.amazon.com/step-functions/latest/dg/vpc-endpoints.html)来连接到 Synchronous Express 工作流*，但是*如果在 VPC 和 VPCe 中启用了私有 DNS 解析，则不需要对您的 SDK 配置进行任何代码更改。

在尝试实施这一新产品之前，请务必查看您的使用情形所支持地区的完整列表。

* * *

[![2021 re:Invent Pre-Show](img/6994ee477a5fa539a7bf90638d62f67e.png)](https://acloudguru.com/content/reinvent-2021-aws-heroes-on-what-to-know-before-the-show-webinar)

*观看 [re:Invent 2021: AWS Heroes 关于展会前应该知道的事情](https://acloudguru.com/content/reinvent-2021-aws-heroes-on-what-to-know-before-the-show-webinar)——关于我们期待什么和我们最期待的主题演讲的大赌注。*

* * *

## CloudFormation 现在支持物联网设备管理

物联网(IoT)用户，把耳朵借给我！[物联网设备管理](https://aws.amazon.com/iot-device-management/)现在[在 CloudFormation 上得到支持！](https://aws.amazon.com/about-aws/whats-new/2021/11/aws-Iot-device-management-supported-aws-cloudformation/)

只需几次点击，您就可以使用 [CloudFormation 模板](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/AWS_IoT.html)来预配置和部署物联网车队管理基础设施！这意味着您的工作模板、车队指标和物联网日志记录设置现在可以实现标准化，并且可以跨地区和 AWS 客户复制。需要部署新的物联网车队管理资源？用一个 JSON 文件来做，瞧！简单的豌豆米饭和奶酪！您的资源使用 CloudFormation 进行部署。

在尝试使用这个新功能之前，一定要确保您的用例所需要的区域是受支持的。

## AWS re:Invent 2021 的云专家

我们正在为从 11 月 29 日到 12 月 3 日的 [AWS re: invent 2021](https://acloudguru.com/blog/tag/reinvent2021) 做准备！如果您在拉斯维加斯，请到我们的 1561 号展位来见见我和一些您最喜欢的培训建筑师，获得超酷的免费赠品，玩玩游戏，甚至可能与我们合影！

无论你是亲自去还是虚拟去，一定要看看我们的[终极指南，重新发明 2021](https://acloudguru.com/blog/business/the-ultimate-guide-to-aws-reinvent-2021) 。

如果你参加虚拟(或只是想 TL；本周事件的博士版本)，你可以在推特[上关注 ACG](https://twitter.com/acloudguru)和[脸书](https://www.facebook.com/acloudguru)和[在 YouTube 上订阅一位云专家](https://www.youtube.com/c/AcloudGuru/?sub_confirmation=1)的每日更新。你也可以加入我们令人敬畏的 [Discord 社区](https://discord.com/invite/acloudguru)，在那里你可以和所有你喜欢的 AWS 培训架构师和志同道合的人一起闲逛。

继续牛逼吧，云大师们，我们下次再见！