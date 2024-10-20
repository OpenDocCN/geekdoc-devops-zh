# 教程:使用 Kubernetes 代理和共享卷的 Vault Agent 高级用例|利用

> 原文：<https://www.harness.io/blog/vault-agent-kubernetes-delegates>

本教程是我关于 Vault 代理集成和利用的文章的高级延续。我收到了一些专家客户的反馈，我决定创建另一个教程，重点介绍 Kubernetes 代理和我们支持 Vault Agent 作为 Harness Secrets Manager 集成方法的新功能。

本教程是我关于 Vault Agent [集成](https://harness.io/blog/devops/vault-agent-secrets-management/)和利用的文章的高级延续。我收到了一些专家客户的反馈，我决定创建另一个教程，重点关注 **Kubernetes 代表**和我们支持**保险库代理**的新功能，作为线束秘密管理器的集成方法。我们将利用配置映射、持久卷、机密等。创建非常可靠的存储库代理部署。

记住保险库代理**不是**Harness 的一个组件是非常重要的。它在哈希公司的王国里。在一天结束时，我们的代表只需要能够够到包含好令牌的接收器文件，即使万一 Tom Marvolo Riddle 自己把它放在那里。金库特工只是一个协助者。Harness 不对您的 Vault Secrets Manager 负责。

### 重要说明:管理多个 Vault 服务器

假设每个环境(如开发、QA、生产)都有一个 Vault 服务器。

我决定在一些**清单模板**中利用**线束环境名**。这是使用多台 Vault 服务器进行设置的好方法。

此外，为了保持原子性并避免单点故障，我考虑了保险库服务器和保险库代理之间的一对一关系。

因此，如果每个环境中有一个存储库，则该存储库代理可以有多个部署。

为了帮助我们解决这个用例，我们将利用一些服务配置变量，这些变量将被环境服务配置覆盖所覆盖(这是本教程的预览，但让我们来看一看):

这是服务配置变量:

这是来自开发环境的覆盖:

## 教程第 1 部分:在 K8s 中创建更专业的 Vault Agent 部署

### 要求

一点 K8s 经验，一个目标集群，一个不错的老马具账号。因为我们将使用持久卷，所以 Vault Agent 工作负载必须驻留在与代理相同的 Kubernetes 集群名称空间中。

### Vault 上的任务

请参考我在本文开头的第一个教程，在您的 Vault 服务器中配置一个好的程序。

### 安全问题

由于 RoleID 和 SecretID 也是超级重要的秘密，我决定将它们存储在默认的由 Harness 管理的 Google KMS 秘密管理器中。然后，我使用模板引擎为我检索它们。

当然，这是你必须和你的安全顾问或安全架构师一起决定的事情。我在我的用例中展示了我所做的事情。

### 保险库代理库本内特的清单来源

我会在这个 [GitHub repo](https://github.com/gacerioni/vault-agent-kubernetes) 中保存所有与我们的 Vault Agent K8s 服务清单相关的文件。我不会破坏这个回购，但这是一个实验室回购。为了安全起见，请把它叉开。

### 第一步

第一步是在 Harness 中存储 RoleID 和 SecretID。如果您想采用相同的方法，它们是在我的第一篇教程中创建的。同样，底层 Auth 方法与 Harness 没有关系，但这是您应该与 Vault 管理员一起做出的决定。

**重要提示:**如果您想要添加另一个安全层，您可以将其存储为 base64，并使用一个数据秘密(在当前使用 stringData 的 Secret Kubernetes 清单中)，而不是 stringData。看你的了！

### 第二步

让我们创建将托管 Vault Agent 工作负载的 Harness 服务。

添加工件源中可用的 Vault Docker 图像:

确保链接您的远程清单:

请添加我们之前讨论过的服务配置变量:

### 第三步

现在，创建一个环境、一个基础设施定义，并添加覆盖。这是处理多个 Vault 服务器的技巧。我们将在模板技巧中使用这些变量。

### 第四步

现在，让我们研究一些与存储库代理清单文件相关的内容。

如果您看一下 values.yaml 文件，您会发现其中有一些重要的内容:

*   持久卷声明名称，这是我选择的在这个部署和委托部署之间共享接收器文件的机制；
*   基础身份验证方法配置 HCL 文件；
*   RoleID 和 SecretID 函数从默认的机密管理器中检索它们。

注意，当我需要为每个环境保留一个对象时，我使用了＄{ env . name}。这将有助于我们设计多保险库服务器策略。

您可以在 templates 文件夹中的 deployments.yaml 文件中看到我是如何处理所有这些内容的。

### 第五步

现在，让我们在 Harness 中创建一个滚动部署工作流。

### 第六步

如果您照原样运行它，您已经在发送一个存储库代理了！

### 第七步

那么，你怎么知道这是有效的呢？您可以将日志输出到您最喜欢的具有日志功能的工具中。就我而言:Splunk、ELK 和 Graylog。

因为我们已经离家很远了，所以让我们把派对留在 K8s 的世界里吧。让我们使用 **ReadinessProbes** 来确保接收器文件是可用的！由于接收器文件只有在这个过程结束时才会出现并填充一个令牌，所以这是监视 Pod 是否良好的好方法。

尽管如此，探针上的任何东西都无法判断令牌是否是好的。即使您的身份验证方法包含错误的凭据，存储库代理也会创建文件。和其他东西一样，这是哈希公司的保险库设计——与脊甲无关。

您可以创建一个自定义脚本来导出 VAULT_ADDR 和 VAULT_TOKEN，并使用以下命令测试该令牌:

VAULT_TOKEN= <the_token_generated_in_sink>保险库机密列表</the_token_generated_in_sink> 

我建议您使用以下命令检查日志:

ku bectl-n harness-delegate logs pod/vault-agent-<...>

这是一个很好的部署:

这是个坏消息:

### 第一部分结果

使用您将在高级就绪探测中使用的相同逻辑，令牌必须是好的！

## 教程第 2 部分:与委托部署共享令牌(具体来说是 StatefulSet)

我们几乎结束了艰苦的工作。现在，是时候改变我们的马具委托清单了。是的，就是您用来在集群中安装代理的那个。

唯一的要求是，Vault 代理和代理必须位于同一个 K8s 集群名称空间中。我们使用 PVC 来共享接收器文件。

### 第一步

让我们更改我们的委托清单，以便它可以通过卷到达我们的接收器文件。我使用的是来自委托 UI 向导的完全相同的清单。这里没有诡计:

第一个技巧是在 Harness Delegate StatefulSet 块的末尾添加卷定义:

卷:
-名称:vault-agent-sink-shared
persistentVolumeClaim:
声明名称:vault-agent-sink-PVC-dev

当然，添加挂载以使文件对我们的线束委托容器可用:

别急，这里可以得到一个很好的例子: [GitHub repo](https://github.com/gacerioni/vault-agent-kubernetes/blob/main/EXAMPLE-harness-delegate-with-vault-agent-mountpoint/harness-delegate.yaml) 。

### 第二步:

我们可以看到，现在委托容器可以看到接收器文件。这太棒了。

让我们继续检查 Harness 是否能够通过新的代理方法与 Vault 集成——但是使用 Kubernetes 委托:

### 第三步:

现在，我们可以创建和编辑一些秘密来稍微强调一下令牌。

不错！

有任何问题或意见吗？让我知道-我总是乐意帮忙。

加百列