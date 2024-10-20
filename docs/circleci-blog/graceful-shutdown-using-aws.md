# 使用 AWS 自动缩放组和 Terraform | CircleCI 正常关机

> 原文：<https://circleci.com/blog/graceful-shutdown-using-aws/>

***来自发布者**的说明:此内容由 Devops 高级客户工程师本·范·豪登于 2020 年 9 月 3 日更新。*

* * *

![AWS logo](img/2262d18d0d469dab28fb48a0332f25f0.png)

理论上，亚马逊的自动伸缩小组(ASG)是一种很好的伸缩方式。这个想法是，你给他们一个期望的能力，以及如何启动更多机器的知识，他们将完全自动旋转你的车队上下随着期望的能力变化。不幸的是，在实践中，有几个关键的原因，我们不能使用它们来管理我们的 CircleCI.com 舰队，其中最重要的是默认的 ASG 终止策略杀死实例太快。因为我们的实例正在为我们的客户运行构建，我们不能简单地立即杀死它们。我们必须等待所有构建完成，然后才能终止实例。

正常关机，在终止一个实例之前，您必须等待一些条件，这不是我们独有的问题。但这个问题的解决方式会因车队规模和使用模式的不同而不同。例如，我们使用一个定制系统来扩展我们的 CircleCI.com 车队，因为 CircleCI.com 的负载遵循一个很大程度上可预测的使用模式，该模式基于我们的客户在世界各地工作的时间，其中夹杂着一点随机性。ASG 提供的简单的基于度量的扩展策略不足以对此建模。

但是，随着[circle ci Enterprise](https://circleci.com/enterprise/)的发布，我们意识到，当我们的每个客户的车队规模和负载要求通常要简单得多时，要求他们发明自己的完全定制的扩展系统是没有意义的。因此，我们想看看是否有一种方法可以创建一个简单的解决方案，涵盖我们大多数客户的常见扩展模式，同时如果基础不够，还允许他们进行更多的定制。

## CircleCI 1.x 解决方案

![Terraform Logo](img/831664e388e642ca09afa3d4b7751983.png)

概括地说，我们的解决方案包括四个部分:

1.  管理 EC2 实例的 ASG。
2.  一个自动扩展生命周期挂钩在关闭实例时发布通知，并将服务器置于“*终止:等待*状态，并触发 SNS 通知。
3.  触发 Lambda 函数实现生命周期钩子动作的 SNS 主题。
4.  Lambda 将通过指定节点上的 AWS SSM 执行“nomad node drain -enable”命令。这将确保将要终止的 nomad 客户端不会构建更多的作业。
5.  给定的节点将被标记为`ineligible`,以防止新的作业排队，并且所有的作业将从该节点打包，以便优雅地终止。
6.  一旦命令成功，该节点将被 ASG 完全终止。

[我们使用 Terraform 管理我们的 CircleCI.com AWS 基础设施](https://www.terraform.io/docs/providers/aws/)，并在我们向客户推荐的 CircleCI 企业脚本中使用。我们是它的超级粉丝，因为它允许我们以一种比 CloudFormation 更简洁、更不容易出错的方式用代码声明性地列出我们的基础设施。下面的资源将作为 terraform 示例列出，但是如果您想要手动构建它，或者使用 CloudFormation，这种方法仍然有效。[如果你想亲自尝试的话，我们已经提供了完整的地形文件。](https://drive.google.com/file/d/1qwnmngvmcolVcFK6e_KvwHMUueJZOkOc/view?usp=sharing)

## 自动缩放组

我们不想为 CircleCI 企业客户自动化我们的手动扩展策略，而是希望提供现有最佳实践的挂钩。对于 AWS，这意味着自动扩展组。助理秘书长提供了很大的灵活性。最简单的形式是，它们充当非常基本的容错功能，如果任何一台机器停止工作，它们就会自动启动新的机器。它们还允许您附加一个[扩展时间表](http://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-as-scheduledaction.html)以实现基于时间的简单扩展，或者附加一个[扩展策略](https://drive.google.com/file/d/1qwnmngvmcolVcFK6e_KvwHMUueJZOkOc/view?usp=sharing)以实现基于指标的反应式扩展。出于这篇博文的目的，我们假设您已经有了一个名为`nomad_clients_asg`的 ASG 设置。

## 自动扩展生命周期挂钩

但是，正如我们之前提到的，ASG 不会给你很长的时间来终止一个实例。对我们来说，我们的正常关闭必须等待构建完成后才能终止实例，这个过程可能需要半个小时或更长时间。因此，我们转向一个相对不为人知的 ASGs 附加物，生命周期挂钩。

生命周期挂钩允许我们在 ASG 执行某些操作时从 Amazon 获得通知。在这种情况下，我们关心的是`autoscaling:EC2_INSTANCE_TERMINATING`，它告诉我们 ASG 何时试图终止一个实例。我们选择了 1 小时的`heartbeat_timeout`和`CONTINUE`的`default_result`。由于我们的正常关机通常不到一个小时，这意味着如果出现问题，而我们在一个小时后仍在运行，亚马逊将强制终止我们。

```
resource "aws_autoscaling_lifecycle_hook" "graceful_shutdown_asg_hook" {
    name = "graceful_shutdown_asg"
    autoscaling_group_name = "${aws_autoscaling_group.nomad_clients_asg.name}"
    default_result = "CONTINUE"
    hearbeat_timeout = 3600
    lifecycle_transition = "autoscaling:EC2_INSTANCE_TERMINATING"
    notification_target_arn = "${aws_sns_topic.graceful_termination_topic.arn}"
    role_arn = "${aws_iam_role.autoscaling_role.arn}"
} 
```

## 亚马逊社交网络通知

您会注意到在上面的例子中，生命周期挂钩的`notification_target_arn`是一个 SQS 队列。这是因为亚马逊需要某个地方来发送终止消息。我们没有编写自己的端点，而是决定让 Amazon 来维护需要为我们终止的实例的状态。

```
resource "aws_sns_queue" "graceful_termination_queue" {
  name = "graceful_termination_queue"
} 
```

SNS 主题本身的示例代码非常简单，但是您还需要创建一个 IAM 角色和相关联的策略，以允许生命周期挂钩发布到 SNS 主题(在上面的`role_arn`小节中进行了配置)。

```
resource "aws_iam_role" "autoscaling_role" {
    name = "autoscaling_role"
    assume_role_policy = <<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "",
      "Effect": "Allow",
      "Principal": {
        "Service": "autoscaling.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
}
EOF
}

resource "aws_iam_role_policy" "lifecycle_hook_autoscaling_policy" {
    name = "lifecycle_hook_autoscaling_policy"
    role = "${aws_iam_role.autoscaling_role.id}"
    policy = <<EOF
{
    "Version": "2012-10-17",
    "Statement": [
        {
          "Sid": "",
          "Effect": "Allow",
          "Action": [
              "sns:Publish",
          ],
          "Resource": [
             "*"
          ]
        }
    ]
}
EOF
} 
```

在这里使用 SNS 主题对我们来说是正确的解决方案，因为我们的架构允许多个工作人员使用关机通知。如果你的架构需要一个可以关闭任何追随者的领导者，因此我们只有一个消费者，那么你应该考虑在这里使用 SQS 队列(亚马逊的发布订阅服务)。

## 优雅的关机

唯一剩下的部分是来自队列的实际消耗，以及机器的正常关闭。根据您如何进行正常关机，您使用队列的解决方案可能与我们的非常不同。但是您仍然需要知道队列中通知的一些情况。

首先，当您第一次连接生命周期挂钩时，Amazon 会发送一个测试通知，以确保连接正常工作:

```
{  
  "AutoScalingGroupName":"example_asg",
  "Service":"AWS Auto Scaling",
  "Time":"2016-02-26T21:06:40.843Z",
  "AccountId":"some-account-id",
  "Event":"autoscaling:TEST_NOTIFICATION",
  "RequestId":"some-request-id-1",
  "AutoScalingGroupARN":"some-arn"
} 
```

您可以安全地忽略消费者中的消息。

实际的关闭通知是您需要知道的唯一的另一部分数据，它们看起来像这样:

```
{  
  "AutoScalingGroupName":"example_asg",
  "Service":"AWS Auto Scaling",
  "Time":"2016-02-26T21:09:59.517Z",
  "AccountId":"some-account-id",
  "LifecycleTransition":"autoscaling:EC2_INSTANCE_TERMINATING",
  "RequestId":"some-request-id-2",
  "LifecycleActionToken":"some-token",
  "EC2InstanceId":"i-nstanceId",
  "LifecycleHookName":"graceful_shutdown_asg"
} 
```

**注:** *这种特殊的解决方案仅适用于 CircleCI Enterprise 2.0。*

如何使用这些通知完全取决于你。在我们的例子中，我们只是在客户可以定制的 Lambda 函数中添加了一个 Node.js 脚本运行器。我们这样做是因为我们想给我们的客户一些预打包的东西，并且易于配置。

## AWS Lambda 函数和 SSM 代理文档

我们的 Lambda 函数利用 AWS Javascript SDK 来利用系统服务管理代理，并与指定的自动缩放组进行通信。AWS SSM 代理是亚马逊软件，可以在 EC2 实例上安装和配置，如果您使用我们的基本企业设置 terraform 脚本 CircleCI 利用亚马逊的 Linux AMIs，它将预安装 AWS SSM 代理。

为了继续使用我们的解决方案，您需要上传一个 SSM 文档，该文档允许您定义希望 System Manager 对您的 AWS 资源(在本例中为我们的 nomad 客户端)执行的操作。对于此解决方案，SSM 文档如下所示，它执行 nomad drain 命令，然后检查节点是否合格:

```
{
   "schemaVersion": "1.2",
   "description": "Draining Node",
   "parameters":{ 
     "nodename":{ 
       "type":"String",
       "description":"Specify the Node name to drain"
     }
   },
   "runtimeConfig": {
     "aws:runShellScript": {
       "properties": [
         {
           "id": "0.aws:runShellScript",
           "runCommand": [
             "#!/bin/bash",
             "nomad node drain -enable -self -y",
             "isEligible=$(nomad node-status -self -json | jq '.SchedulingEligibility | contains (\"ineligible\")')",
             "if (( ${isEligible} == true )) ; then exit 0 ; else exit 129; fi"
           ]
         }
       ]
     }
   }
 } 
```

您可以将该文件保存为 document.json，并执行以下命令在 SSM 中创建它:

```
aws ssm create-document --content "file://drain-document.json" --name "CircleCiDrainNodes" --document-type "Command" 
```

最后，您需要创建一个 Lambda 函数来处理生命周期挂钩。将 Lambda 代码下载到您的本地工作站，并安装代码依赖项。安装时，请确保您使用的是与 Lambda 相同的版本，并定制 Lambda 脚本以匹配您的环境。

压缩代码并创建一个 Lambda 函数(或者你可以参考我们的 Zip 文件[这里](https://drive.google.com/file/d/1peG6cVPoqmCCLUUUkeMyy2Sgrwtunm1F/view?usp=sharing))。

## 结论

长期以来，自动缩放组一直是管理缩放的最佳选择，因为它们在如何缩放方面提供了如此大的灵活性。此外，随着生命周期挂钩的增加，它们还为您提供了终止方式的灵活性，允许平稳关闭。

这意味着，当我们在寻找一种解决方案，让 CircleCI 企业客户能够以最少的运营团队开销扩展其车队时，带生命周期挂钩的 ASG 是一个很好的选择。他们提供了一个即插即用的解决方案，不仅能带来立竿见影的价值，还允许我们的客户在实际扩展车队的方式上进行高级定制。