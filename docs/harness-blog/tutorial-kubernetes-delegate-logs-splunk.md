# 教程:[监控和可观察性]如何将 Kubernetes 委托日志发送到 Splunk | Harness

> 原文：<https://www.harness.io/blog/tutorial-kubernetes-delegate-logs-splunk>

所以…你活在梦里，对吗？您的工作负载在 Kubernetes 上运行，您的 SRE/监控/可观察性技术体系中有 Splunk。最棒的是:你是一个快乐的线束用户。很自然，您决定将 Harness 委托作为 K8s StatefulSet 运行。

所以…你活在梦里，对吗？您的工作负载在 **Kubernetes** 中运行，您的 SRE/监控/可观察性**技术栈**中有 **Splunk** 。最棒的是:你是一个快乐的**线束**用户。

很自然，您决定将 Harness 委托作为 K8s StatefulSet 运行。您正在阅读来自 [Kubernetes.io](http://kubernetes.io/) 的这个[惊人的文档](https://kubernetes.io/docs/concepts/cluster-administration/logging/)，但是您仍然不确定如何实现一个好的日志转发设计。

别担心，我高贵的 SRE。集成离你只有一个文件描述符了！啊，自动字段检测+去除可能干扰事件的颜色和其他控制字符。

系好安全带。

## 辅导的

### 要求

### 第一步

让我们配置我们的 Splunk HEC，然后将令牌作为机密存储在 Harness 中。

1-) HEC UI -示例:

2-)然后是安全存储的令牌(我们在 Splunk 的 HEC 设置中获得的那个):

这样，您可以看到 index=harness_deployed_apps 将成为我们的代理日志的主目录。

### 第二步

好了，是时候给 [Splunk](https://github.com/splunk/splunk-connect-for-kubernetes/tree/develop/helm-chart/splunk-connect-for-kubernetes/charts/splunk-kubernetes-logging) 配置线束了。

您可以看到，这是一个非常容易理解的 Splunk 管理的掌舵图。要更深入地了解该机制，请查看 README.md。

在这一步中，我们创建了一个**服务**:

魔术从这里开始:

1-)让我们创建一个指向 Splunk 项目的源回购:

2-)回到服务 UI——让我们链接一下那个叫做 splunk-kubernetes 的漂亮的掌舵图——日志记录如下:

**分支:**主
**文件/文件夹路径:**Helm-chart/splunk-connect-for-kubernetes/charts/splunk-kubernetes-logging/
**Helm 版本:** v3

好吧，看起来不错！

### 第三步

我们将使用 Harness 强大的覆盖引擎，这样我们就不需要管理那个项目的分叉。我们可以通过覆盖缺省值(GH 项目中的值)中的一些条目来完成所有需要的步骤。

重要:Harness Delegate Pod 将输出日志，因此我们只需很少的定制就可以使用它。

你可以阅读 [values.yaml](https://github.com/splunk/splunk-connect-for-kubernetes/blob/develop/helm-chart/splunk-connect-for-kubernetes/charts/splunk-kubernetes-logging/values.yaml) 来确认你是否需要改变什么。您可能还需要请 Splunk 管理员与您一起查看，但这非常简单！

**这是我的超驰部分，在线束 UI:**

# Local splunk 配置
SPLUNK:
# Configurations**for**HEC(HTTP**Event**Collector)
HEC:
# host 是必需的，应由用户提供
host:"<SPLUNK _ HEC _ HOSTNAME>"
# port**to**HEC，**可选**， **默认** 8088
端口:【8088】
#令牌是必需的，应由用户
令牌提供**:" $ { secrets . getvalue(" splunk _ HEC _ delegate _ logs ")} "
#协议有两个选项:" http "和" https "，**默认**是" https"
协议:" http"
# indexName 告诉哪个索引 ****如果**它*不存在，将使用“主”。*
index name:" harness _ deployed _ apps "

fluentd:
# path**of**log files，**default**/var/log/containers/*。日志
路径:/var/log/containers/* **委托** *。日志**** 

### 第四步

嘿，让我们使用 props.conf 来删除任何可能影响 Splunk 搜索头可读性的着色日志字符。

为了使这更容易，我将把我们的 Harness 委托源类型(Splunk)映射到一个非常好的 SED 命令，在“System Local”中。这对我们会有用的！您的 Splunk 管理员可能有一个 Deployer + Apps +等来为您组织它。

让我们继续前进！

1-)如果你没有从初始值 YAML 改变太多，这是我们的源类型:

2-)请编写这个文件:

vim /opt/splunk/etc/ **系统** / **本地** /props.conf

3-)让我们添加我们非常聪明和神奇的 sed 命令:

[kube:container:Harness-delegate-instance]
SEDCMD-Harness = s/\ x1B \[[0-9*；]*[a-zA-Z]//g*

### 第五步

现在让我们部署它！我将使用一个非常简单的滚动部署！

这是这样的:

### 最后一档

好了，是时候检查这是否有效了。我会跳进我的搜索头实验室。

我将运行一个错误的 Splunk 查询，但是我是我的实验室集群中唯一的一个。所以，没有伤害！这里:

## 结果:

看起来太棒了！让我们部署一个虚拟的 NGINX，然后我们可以放置我们众所周知的 TaskID 来确保我们没有做任何疯狂的事情:

让我们得到我们的任务 ID:

我很幸运拥有这一系列技术！看看这个:

有任何问题或意见吗？让我知道-我总是乐意帮忙。

加百列