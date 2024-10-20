# 线束自定义部署简介-第一部分|线束

> 原文：<https://www.harness.io/blog/introducing-harness-custom-deployments>

Harness 最近推出了一项名为定制部署的全新功能。顾名思义，定制部署是用户执行任何类型的部署的一种方式，这种部署可能不是本机支持的，也不是现成的 Harness 提供的。

Harness 最近推出了一项名为[定制部署](https://developer.harness.io/docs/first-gen/continuous-delivery/custom-deployments/create-a-custom-deployment/)的全新功能。顾名思义，定制部署是用户执行任何类型的部署的一种方式，这种部署可能不是本机支持的，也不是现成的 Harness 提供的。这些技术包括传统技术，如 Weblogic、WebSphere、Google Cloud Functions 等。与 Harness 中其他本机支持的部署类型一样，定制部署允许在服务仪表板上跟踪实例(或工作负载),并随着实际底层基础架构的变化而保持更新。这些部署的实例也是受益于我们的[持续验证](https://developer.harness.io/docs/first-gen/continuous-delivery/continuous-verification/continuous-verification-overview/concepts-cv/what-is-cv/)集成的候选者。此外，用户可以捕获部署的实例，并在他们选择的 CV 工具中利用它。

## **让我们看看这是如何实现的**

### **定义定制部署类型**

部署类型应该像一个可以在一个帐户中使用的模板。因此，我们决定利用帐户级模板库来定义这些部署类型。与模板库中的其他模板类型一样，这些模板也是有版本的。如果以下任何内容发生更改，将生成新版本:

*   基础设施变量
*   获取实例命令脚本
*   主机对象数组路径
*   主机属性

*在线束 UI 中创建自定义部署模板*:

## 字段说明

**显示自定义部署类型的名称**基础设施变量帮助识别基础设施的变量。示例:。项目、集群名称、名称空间、秘密令牌、密码等。这些就像一个模板，值可以在以后提供。可以在此指定默认值。Fetch Instances 命令脚本脚本需要查询基础结构以获取实时实例。CV 可以使用这些实例进行验证，服务仪表板也可以使用这些实例。主机对象数组路径包含实例信息的 JSON 数组的完整路径。在下面的例子中，他的值应该是“items”。如果这是一个嵌套字段，我们可以使用“.”作为分隔符来指定完整路径。示例:" foo.bar.items . "每个 JSON 对象中的 Host AttributesKey 可用于存储实例属性。主机名是一个强制属性，因为它是跨平台标识实例的基础。示例:主机名应设置为*列表地址*，我们可以设置另一个属性，如分段模式。字段名可以设置为更容易阅读的字符串，比如 staging_mode，JSON 路径应该设置为 *Mode.staging.*

*主机对象数组路径配置示例:*

{
"项目":[
{
"模式":{
"分期":" nostage"
}，
" listen address ":"
}，
{
"模式":{
"分期":" external_stage"
}，
" listen address ":" 172 . 17 . 0 . 3 "
}，
{。

现在，我们已经创建了部署类型，可以跨应用程序使用它进行部署了。

### **定义服务**

在线束中，我们考虑“您正在部署什么？”被称为服务。类似于在 Harness 中配置任何其他服务的方式，他们可以为自己的定制部署创建服务。只需从*部署类型*下拉列表中选择新部署类型的名称，就可以开始了。您可以添加任何种类的工件并配置服务配置变量。

### **定义基础设施**

*使用自定义部署类型在线束中创建基础架构定义*:

## 字段说明

**自定义部署类型的显示名称名称**云提供商将此设置为自定义，因为在这种情况下没有专用的云提供商。部署类型选择帐户中存在的自定义部署类型之一。VersionDeployment 类型是模板库中的模板。它是有版本的。基础结构定义可以引用静态版本或最新版本来获取最新的更改。该版本的变量将显示在这里。变量为基础设施变量设置值，以定义具体的基础设施。值可以使用任何内置的线束表达式。示例:服务变量、机密、配置文件等。

### **设置工作流程**

可以创建多阶段(Canary)和单阶段(Basic)工作流。在 ***部署*** 环节步骤中，有一个名为 ***获取实例*** 的步骤被默认添加到工作流中。让我们详细讨论一下这一步，因为这是整个定制部署流程中最关键的部分。

*带有基本步骤设置的示例自定义部署工作流*:

该步骤将运行与基础设施定义中配置的版本相对应的 ***获取实例命令脚本*** ，并替换定义的值。这将在代理上运行，响应将被发送到线束管理器，在那里响应将使用自定义部署模板中定义的 ***主机对象数组路径*** 和 ***主机属性*** 进行解析。实例对象将被创建，并通过表达式***$ { instance . hostname }***供后续步骤使用。在随后的部分中，步骤如下:

*   命令过程
*   服务命令
*   日志分析(ELK，Google Operations 等。)
*   度量分析(AppDynamics，Datadog，NewRelic 等。)

可以添加以在已部署的实例上执行所需的操作。通常，用户希望验证他们部署的实例是否成功接收流量，或者运行负载测试以确保应用程序能够处理大量流量。

*注意:这些工作流程是可定制的。阶段步骤，如部署、验证等。，可以根据需要添加、删除和重新排序。*

### **维修仪表板**

服务控制面板允许我们的用户查看他们部署的实例，并跟踪已经部署的实例、部署时间以及部署位置。Harness 存储了 ***获取实例命令脚本*** ，以便能够在后台运行它，从而更新服务仪表板。例如，如果脚本调用 API 获取基础设施的当前状态，并以所需的 JSON 格式将其转储，则服务仪表板将反映对已部署实例/工作负载的任何更新。

如果需要更新脚本，可以重新运行工作流来更新后台脚本。

### **该收尾了**

我们的 Harness 团队已经开始创造性地使用我们的定制部署特性，并提出了一些非常简洁的部署用例。我们的一些用户已经在他们的遗留工作负载中使用这个特性，比如 Websphere 和 Weblogic。查看 Harness 社区，我们的团队在那里与我们的用户分享这些实现。以下是最近的一些例子:

**谷歌功能**
[https://community . harness . io/t/Google-cloud-Functions-with-harness-custom-deployment/598](https://community.harness.io/t/google-cloud-functions-with-harness-custom-deployment/598)

**谷歌云运行**
[https://community。挽具。io/t/Google-Cloud-Run-workloads-with-custom-deployment-in-harness/597](https://community.harness.io/t/google-cloud-run-workloads-with-custom-deployments-in-harness/597)

**AWS 弹性豆茎**
[https://community . harness . io/t/AWS-Elastic-Beanstalk-with-harness-custom-deployment/619](https://community.harness.io/t/aws-elastic-beanstalk-with-harness-custom-deployment/619)

我们迫不及待地想看看你们在使用定制部署时都有什么想法。请随时与社区分享反馈。看到我们的用户提出的很酷的想法是令人兴奋的！