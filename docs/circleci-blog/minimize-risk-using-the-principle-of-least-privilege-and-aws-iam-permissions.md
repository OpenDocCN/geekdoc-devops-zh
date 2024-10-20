# 使用最小特权原则和 AWS IAM 权限将风险降至最低

> 原文：<https://circleci.com/blog/minimize-risk-using-the-principle-of-least-privilege-and-aws-iam-permissions/>

在评估一个人的风险因素时，拥有适当的安全控制不再是唯一的考虑因素。即使实现了安全控制，它们也经常没有得到适当的许可。例如，关键服务级别帐户被授予过多的系统权限并不罕见。如果帐户受到威胁，这将增加系统内的攻击媒介。然后，这个被侵入的帐户可能会执行恶意代码或访问非常敏感的数据，这可能会给组织带来许多问题。防止这类安全事件的一种方法是实现最小特权原则的概念。最小特权原则意味着只给一个帐户那些执行其预定功能所必需的特权。例如，仅用于从亚马逊 S3 存储桶读取文件的服务帐户不需要将文件写入存储桶。任何其他权限，如列出、更新或写入文件都会被阻止。拥有最低权限的帐户可以保护系统不被拥有过多权限的受损帐户利用。

在这篇文章中，我将向你展示如何收紧你的 AWS IAM CircleCI 服务帐户的权限，以便它只对一个特定的 AWS S3 桶中的`/build`文件夹具有访问和权限。这些权限将限制该帐户只能操作`/build`文件夹，并阻止访问其他资源。

这篇文章假设你有:

## 确定\创建一个 AWS S3 存储桶来存放您的构建

您需要确定一个现有的 S3 存储桶或[为您的 CircleCI 服务帐户创建一个新的存储桶](https://docs.aws.amazon.com/AmazonS3/latest/gsg/CreatingABucket.html)。我举的例子 S3 的斗名是`devops.datapunks.org`。您可以随意命名您的 S3 存储桶，但是一个常见的、最好的做法是使用熟悉的[完全限定域名(FQDN)](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) 格式来命名存储桶，这将有助于随着您的基础架构的增长而保持原样。我的 bucket 的`devops.`部分表示这个 bucket 将与我的`datapunks.org`域的 devops 进程相关。

现在您已经有了一个 bucket，接下来创建一个名为`builds`的文件夹，它将保存 CircleCI 构建生成的所有构建工件。

## 创建新的 IAM 组

AWS IAM 有一个[组](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_groups.html)的概念，它是 IAM 用户的集合，使您能够为多个用户设置权限。这些组减轻了用户&对他们权限的管理。

从 IAM 控制台:

*   点击`Groups` > `Create New Group`按钮。
*   指定新的组名。使用有意义的描述性名称。我将使用`circleci_devops`。
*   在`Attach Policy`屏幕>点击`Next Step`。
*   点击`Create Group`按钮。

现在，您应该可以在列表中看到新创建的组。

## 创建 IAM 策略

[IAM 策略](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)是与定义其权限的身份或资源相关联的 AWS 实体。当主体(如用户)发出请求时，AWS 会评估这些策略。策略中的权限决定是允许还是拒绝请求。策略作为 JSON 文档存储在 AWS 中，作为基于身份的策略附加到主体，或者作为基于资源的策略附加到资源。

AWS 基本上有两类策略:**托管策略**和**内联策略**。

**托管策略**是基于身份的独立策略，您可以将其附加到 AWS 帐户中的多个用户、组和角色。您可以使用两种类型的托管策略:

*   AWS 托管策略:这些是由 AWS 创建和管理的托管策略。
*   客户管理的策略:您在 AWS 帐户中创建和管理的管理策略。与 AWS 托管策略相比，客户托管策略可以更精确地控制您的策略。

**内联策略**是您创建和管理的策略，直接嵌入到单个用户、组或角色中。

在这篇文章中，您将创建一个新的`customer managed`策略，因为我们想要限制我们的服务帐户用户对我们的 bucket 中的`build`文件夹的访问。

在 IAM 控制台中:

*   点击`Policies` >点击`Create Policy`
*   点击`JSON`选项卡
*   将以下 JSON AWS 策略粘贴到文本字段>单击`Review Policy`

**重要**

您必须将以下 JSON 中的 bucket 名称替换为您之前创建的 bucket 的名称。因此，在文本的所有实例中，桶名`devops.dpunks.org`必须替换为您的桶名，否则帐户将无法访问 S3 桶。

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListAllMyBuckets"
            ],
            "Resource": "arn:aws:s3:::*"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetBucketLocation"
            ],
            "Resource": "arn:aws:s3:::devops.dpunks.org"
        },
        {
            "Effect": "Allow",
            "Action": "*",
            "Resource": "arn:aws:s3:::devops.dpunks.org/builds/*"
        }
    ]
} 
```

*   输入策略的名称。我的例子使用了`s3_full_access_devops_builds`。
*   输入策略的描述。
*   点击>`Create Policy`。

您刚刚创建的策略基本上允许对我们的 S3 存储桶中的`build`文件夹的完全访问，这相当于只允许对这个文件夹**的读/写权限**。

## 将新策略附加到 IAM 组

新的`s3_full_access_devops_builds`策略已经准备好附加到我们之前创建的`circleci_devops` IAM 组。在 AWS IAM 控制台中:

*   点击`Groups` >点击`Permissions`标签。
*   点击`Attach Policy`按钮。
*   在过滤器字段中键入`s3_full_access_devops_builds`或您策略的名称。
*   在列表中选中要附加的策略。
*   点击`Attach Policy`按钮。

新策略现已附加到`circleci_devops`组，并将应用于分配到该组的任何用户。

## 创建新的 IAM 用户

您已经创建了一个新的 IAM 组 IAM 客户管理策略，并且还将该策略附加到了该组。接下来，您将创建一个新用户，并将其分配到`circleci_devops`组，该组将授予该帐户在 bucket 的 S3 `build`文件夹中的读/写权限。

在 AWS IAM 控制台中:

*   点击`Users` >点击`Add User`按钮。
*   在`User Name`文本框中输入一个名称。我的示例用户是`circleci_service`。
*   检查访问类型部分的`Programmatic access`选项。
*   点击`Next Permissions`按钮。
*   选中将用户添加到组部分中的`circleci_devops`选项。
*   点击`Next: Review`按钮>点击`Create User`按钮。

您现在应该会看到一条`Success! Account created` …消息，在这里您必须获取帐户的访问密钥 ID 和秘密访问密钥。

**重要**

访问密钥 ID 和秘密访问密钥值非常敏感，必须加以保护。你必须能够以. csv 格式下载这些凭证，但请注意，这将是你能够访问密钥的唯一时间。如果您此时不保存密钥或丢失密钥，您将不得不[创建新密钥并管理](https://docs.aws.amazon.com/general/latest/gr/managing-aws-access-keys.html)先前创建的密钥。

## 在 CircleCI 项目中配置 AWS IAM 密钥

您创建了一个新的编程用户，该用户对您的 S3 存储桶中的`build`文件夹只有读/写权限，现在您必须将该帐户与您的 CircleCI 帐户中的项目相关联。登录 [CircleCI 控制台仪表板](https://app.circleci.com/dashboard)。下面是关联 AWS IAM 证书的简要说明，您还可以在这里看到更详细的说明

*   点击您想要链接 IAM 账户的项目的`Settings` (cog 按钮)
*   找到`Permissions`章节>点击`AWS Permissions`
*   输入您的`Access Key ID`和`Secret Access Key` >点击`Save AWS Keys`按钮

您的 IAM 帐户密钥现在保存在 CircleCI 项目中，您现在可以对 S3 存储桶中的`build\`文件夹进行读/写访问。你的 CircleCI 版本现在可以安全地上传版本包到你的 S3 桶了。

现在，您可以安全地管理和使用这些 AWS 凭证来对严格定义的资源执行特定的操作。

## 结论

这篇文章简要解释了最小特权原则的概念，并向您展示了如何在您的 CI\CD 管道中实现这一实践。这些概念将极大地减少 IAM 凭证受损时的安全风险和攻击媒介。这些做法对用户管理、事件管理和安全审计也很有帮助。

既然您已经对最小特权原则有了基本的了解，那么就开始负责任地计算吧！