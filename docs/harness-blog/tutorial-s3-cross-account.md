# 教程:[工件服务器] S3 -如何通过存储桶策略提供跨帐户访问

> 原文：<https://www.harness.io/blog/tutorial-s3-cross-account>

Harness 有很多客户，他们有非常不同的工件来源。其中最著名的是使用一个 S3 桶作为各种文物的家。

Harness 有很多客户，他们有非常不同的工件来源。其中最著名的是使用一个 S3 桶作为各种文物的家。

我们的工程师一直在努力使 S3 在我们可用的许多其他集成中发挥作用。S3 和 GCS 是我们世界的一等公民，对吗？

在 AWS 领域，我知道许多**解决方案架构师**将采用**多账户**方法。有时，根据业务规模，给定的 **AWS 组织**下的每个团队会建议 4 个账户:

*   发展账户；
*   STG 账户；
*   珠三角账户；
*   DevTools 和 DevOps 帐户。

如果你擅长 DevOps 实践，你将有一个你的工件的真实的单一来源。这就是跨帐户访问发挥作用的地方！

让我们看看 Harness 如何克服 S3 SDK/CLI 的当前设计。超级简单。

系好安全带。

## 辅导的

### 要求

我们将在一个有两个帐户的场景中工作:

*   **账户 A** 将成为桶的所有者。姑且称之为 **DevOps/DevTools 账户**。
*   **账户 B** 将成为我们的**珠三角账户**。需要访问跨 acc S3 存储桶对象的那个。

### 第一步

让我们定义一个好的工件源。我将存储几个虚拟舵图表(。tgz)在 S3 桶。我可以使用 GitHub Pages 为我的图表启动一个免费的 HTTP 服务器，但现在不是这样了。

所有文件仅用于学习目的，它们是模拟文件:

### 第二步

是时候使用最小特权原则来定义一个好的 S3 木桶策略了。

值得一提的是，您可以让我们的 Harness 代表在另一个帐户中担任角色。更好的是，如果您在 EKS 使用您的代理人，您可以使用 IRSA 使线束代理 Pod 服务帐户映射到 IAM 角色。

但对 S3 来说，没必要矫枉过正，对吧？

在我们的文档中，我们建议[这个策略](https://docs.harness.io/article/wt1gnigme7-add-amazon-web-services-cloud-provider#policies_required_amazon_s3)。

使用 Harness 所需的 IAM 角色和策略将 Harness 连接到您的 AWS 帐户。

但目前，一项更具限制性的政策也在发挥作用。请记住，最起码的特权是在一个不断变化的世界中不断努力。

我将使用一个条件来允许 Account B 的主体到达该桶，并给出 Harness 当前执行任务所需的所有动作。这里:

{
"Version": "2008-10-17 "，
" Statement ":[
{
" Sid ":" denyuencryptedobjectuploads "，
"Effect": "Deny "，
"Principal": "* "，
"Action": "s3:PutObject "，
" Resource ":" arn:AWS:S3:::::<your _ bucket>/* "，
"Condition": {【条件
"Effect": "Allow "，
"Principal": "* "，
" Action ":[
" S3:GetBucketLocation "，
"s3:GetBucketVersioning "，
"s3:ListBucket "，
"s3:ListBucketVersions "，
"s3:GetObject"
，
" Resource ":[
" arn:AWS:S3::::::::::<

### 第三步

好吧，让我们来看看！我创建了这个服务作为例子，因为它允许 S3 作为工件源:

非常重要:目前，Harness 和 AWS CLI 不会列出跨帐户存储桶，但这并不意味着 API 不能检索对象。我们可以在工件源配置步骤中明确定义。

很自然，我选择的云提供商来自账户 B(符合策略的组织条件)，而不是来自 S3 存储桶所有者账户:

### 最后一档

现在让我们等待 Harness 异步任务来获取我们心爱的工件。

导入时间
time.sleep(90)

开个玩笑！这是这样的:

## 结果:

现在你不害怕交叉帐户 S3 策略，我希望！

有任何问题或意见吗？让我知道-我总是乐意帮忙。

加百列