# 现代测井层|线束

> 原文：<https://www.harness.io/blog/modern-log-layers>

作为一名专业工程师，你首先要学习的一件事是关于日志的存在(和过度存在)。在您的个人笔记本电脑上，您可能会遇到操作系统供应商收集的崩溃报告中的日志。虽然作为工程师，当我们进入专业世界时，我们大学项目的控制台输出需要在某个地方被捕获；输入日志。

日志当然是一个大行业，并且有大量的日志框架。在过去的日子里，日志并不是最有趣的非功能性需求。随着我们的系统变得更加分散，了解我们系统的健康和性能需要系统本身。日志可以被视为洞察我们系统中事件的基础。

### 什么是日志？

不要与木材品种或对数函数混淆， [logs](https://en.wikipedia.org/wiki/Log_file) 是我们的系统产生的事件输出片段。由于代码实际上是一组指令，我们有机会记录所有这些指令的交互。

某些互动比其他互动更重要。日志记录语句的详细程度或粒度是一个工程决策点。通常，在较低的环境中，当您创建新的东西时，您会有一组更详细的日志，这意味着您需要捕获更多的日志来进行验证和调试。随着我们转向生产，我们倾向于使用不太详细的日志来仅捕获关键事件。如果出现问题，通常会在环境中短时间内再次增加日志详细度，例如在解决生产问题之前。

作为系统化的记录，日志可以由人来处理，或者通常由其他系统来处理。有专门构建的日志聚合工具，如 [Splunk](https://www.splunk.com/) ，我们可以将日志转发到其他系统，如 ELK [ [Elastic、Logstash、Kabana](https://www.elastic.co/what-is/elk-stack)stack。随着系统变得越来越复杂，简单的日志文件也变得越来越复杂。

### 原木为什么坚硬？

原木有各种形状和大小。通常由工程师决定记录什么和什么时候记录。最有可能作为开发标准的一部分，日志标准和覆盖范围被设置。尽管在构建应用程序或基础设施时，日志记录的实际实现取决于工程师。

我们生成了如此多的事件，日志也可以看作是一个瓶颈。设想一个高价值、低延迟的消息传递系统，如果您要记录每条消息的内容，维护日志所需的系统速度将比消息传递系统本身需要更多的计算能力。

每个应用程序、平台、系统等在不同的地方生成不同的日志。例如，您的应用程序可能具有写入 web 服务器日志的特定于应用程序的事件，这些日志与数据库进行交互，该数据库还具有驻留在操作系统上的日志/记录，这些日志/记录写入 [systemd](https://www.digitalocean.com/community/tutorials/how-to-use-journalctl-to-view-and-manipulate-systemd-logs) 。在您的基础设施堆栈中，很可能有比应用程序中更多的日志。您可以看到日志的累积是多么容易。

对于需要生成日志的系统来说，日志内容也是一项繁重的工作。例如，如果您包括一个完整的堆栈跟踪[正在进行的调用的范围]，它比一个简单的消息需要更多的开销。对于两个事件中的一个来说，这没什么大不了的，但是对于分布式系统中的多个用户来说，开销肯定会增加。

由于需要如此强大的火力来恰当地捕捉每一个事件，随着[统计显著性](https://en.wikipedia.org/wiki/Statistical_significance)的出现，数学开始悄然出现。现代应用程序和平台被设计为具有弹性，可以根据工作负载进行伸缩。因为规模，全面监控每一个运行的容器有意义吗？也许我们可以取一小部分仍然具有统计学意义的样本，例如每三个容器中取一个。

根据语言的不同，可能有其他的方法可以检测，比如在 JAVA 中，你可以检测字节码，这样应用性能管理(APM)厂商就诞生了。除了查看日志数据，APM 供应商还有复杂的采样方法。日志需要时间来写入和聚合，这是日志记录层的一部分。

### 记录层

日志方法和架构当然会考虑。不像我年轻工程师认为的那样，日志会神奇地出现，它们是系统的一部分，有设计上的考虑。在这篇[优秀的中篇文章](https://medium.com/trendyol-tech/forwarding-kubernetes-containers-logs-to-elasticsearch-with-fluent-bit-and-showing-logs-with-411587e54e22)中，作者介绍了如何登录一个现代应用程序堆栈，以及一旦有了日志后该如何处理。

我们可以将测井系统分为三层。在功能上，您需要截取、路由/转发和聚合您的日志。

*拦截器:*日志拦截器的工作是决定需要捕获哪些事件或指令，以及需要捕获的详细程度。应该捕获致命的项目，还是应该捕获更具信息性的项目。这通常在拦截器中配置，以便在代码库中查找日志语句。

*路由器/转发器:*您的日志必须放在某个地方，即使它是本地系统上的一个文本文件。日志路由器的工作就是这么做的，路由日志被写入并最终传递到的位置。通常，日志会被转移到另一个系统进行处理和保留。可能需要一个大数据系统来更快地发现异常，或者一个数据库来实现合规性和保留。

*聚合器:*在生活中，有几个绝对与死亡，税收，你会有不止一个日志。聚合器现在很普遍，因为我们的用户穿越的系统是分布式的。日志聚合器的工作是发现几十个潜在日志之间的关系，并以简洁或基于结论的方式呈现它们。

我们可以用最新的日志软件包 Fluentd 来看看这些层是如何一起工作的。

### 用 Fluentd 连接线束代理

许多人的目标是拥有一致的日志格式和日志提供者。去年毕业的一个 CNCF 项目是[浮动](https://www.fluentd.org/)。Fluentd 的目标就是成为一个普通的日志提供商。作为一个项目，Fluentd 和子项目如 [Fluent Bit](https://fluentbit.io/) 希望实现测井卓越。

为了了解现代日志记录的强大功能，例如可以连接一个[线束 ECS 委托](https://developer.harness.io/docs/first-gen/continuous-delivery/aws-deployments/ecs-deployment/harness-ecs-delegate/)来利用 [AWS Firelens](https://aws.amazon.com/about-aws/whats-new/2019/11/aws-launches-firelens-log-router-for-amazon-ecs-and-aws-fargate/) 进行日志输出。通过[遵循 AWS](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_firelens.html) 的步骤，我们可以很快起床并开始工作。务必记下日志路由器映像的可用区域的 [ECR](https://aws.amazon.com/ecr/) 信息。

这个例子确实需要稍微熟悉一下 Amazon ECS。利用文档是了解如何部署 ECS 代表的好地方。在 [Harness 社区](https://community.harness.io/t/aws-ecs-fargate-delegate-install-from-0-to-hero/182)中，如果你以前没有使用过任何 ECS 服务，我们也有一个很好的帖子。这个例子也适用于您自己的 Amazon ECS 部署，而不仅仅是 Harness ECS 委托。

根据您利用的 [AWS ECS](https://aws.amazon.com/ecs/) 的类型，用于 [Fluent Bit](https://fluentbit.io/) 日志路由器的[容器定义](https://docs.aws.amazon.com/AmazonECS/latest/APIReference/API_ContainerDefinition.html)的位置可能会有所不同。在我下载的线束*中，ecs-task-spec.json* 将 Fluent Bit 配置放在线束 ecs 配置之前。

{
"本质":真，
"像":" 906394416424。dkr。ECR。美国东部 1 号公路。亚马逊鹦鹉。com/AWS-for-fluent-bit:latest "、
"name": "log_router "、
" fire lens configuration ":{
" type ":" fluent bit "
}、
" log configuration ":{
" log driver ":":"

接下来，确保执行 ECS 任务的 [IAM 角色](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)拥有 Firelens 和 CloudWatch 的权限。

在 AWS 示例中， [ECS 任务](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html)将需要创建 CloudWatch 日志和日志组，因此[执行 ARN 角色](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_execution_IAM_role.html)需要拥有对 [Firelens/Firehose](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazonkinesisfirehose.html) 和 [CloudWatch](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/iam-identity-based-access-control-cw.html) 的权限，这些权限可以通过 IAM 角色进行修改。

使用修改后的 *ecs-task-spec.json* ，您可以注册任务定义并让 ecs 启动服务。下载的 Harness ECS 委托具有任务定义和服务规范。

*aws ecs 注册-任务-定义-CLI-输入-json 文件://ecs-task-spec.json*

AWS ECS create-service-CLI-input-JSON file://service-spec-for-AWS VPC 模式。JSON



ECS 任务启动后，转到 AWS CloudWatch UI，看到开始出现精彩的日志数据。

您可以通过转到线束 UI 并设置->线束代理来验证您的线束 ECS 代理是否已注册。

你现在正在通往日志涅槃的路上。

### 挽具在这里搭档

Harness 平台能够根据您的日志作为输入来源，帮助您对部署做出判断。凭借我们的[持续验证](https://harness.io/platform/continuous-verification/)能力，深入挖掘 Splunk 或您的 ELK 堆栈提供的见解可以增强信心。

有了 Amazon ECS 部署和 Harness，我们可以为 Fluentd 和其他应用程序简单地选择和配置日志驱动程序。

如果您想知道如何开始将日志转化为可操作的项目，那么在组合中添加工具将有助于协调所需的步骤。今天就去报名吧。

干杯！

-拉维