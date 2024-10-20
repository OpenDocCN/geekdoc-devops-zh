# 线束基础设施供应器和 ARM 模板-第 2 部分|线束

> 原文：<https://www.harness.io/blog/harness-infrastructure-provisioners-and-arm-templates-part-2>

在这个系列的第二部分，也是最后一部分，我们将学习如何使用 Harness 变量来模板化你的 ARM 模板，以及 Harness 如何提供回滚，而 Azure 没有。

在本系列的第[部分，我们学习了如何使用线束供应器部署 ARM 模板。我们还学习了 Azure ARM 的基础知识，并且讨论了在部署 ARM 模板时，Harness 服务如何相互通信。我会推荐你通过](https://harness.io/blog/continuous-delivery/arm-templates-pt1/)[阅读第一篇博客](https://harness.io/blog/continuous-delivery/arm-templates-pt1/)来重温这个信息。

在这个系列的第二部分也是最后一部分，我们将学习如何使用 Harness 变量来模板化你的 ARM 模板，以及 Harness 如何提供回滚，而 Azure 没有。

## ARM 模板中的线束变量

假设我们想为不同的微服务，比如订单服务、支付服务、用户服务等，创建一个不同的 Azure web app。我们可以有一个 ARM 模板，web 应用程序的名称可以定义为一个参数。

ARM 模板 web app 创建部分:

{
"apiVersion": "2016-08-01 "，
"type": "Microsoft.Web/sites "，
" name ":"[parameters(' siteName ')]"，
"location": "[resourceGroup()。位置]"，
"属性":{
" site config ":{
" name ":"[参数(' siteName')]"，
" appSettings ":[
{
" name ":" WEBSITES _ ENABLE _ APP _ SERVICE _ STORAGE "，
"value": "false"
}
，
" linuxfxv version ":" DOCKER | nginx:alpine "
}，【location "Web/serverfarms '，变量(' service planname ')]"
}，
" depends on ":[
"[resourceId(' Microsoft .Web/serverfarms '，变量(' service planname ')]"
]
}

如你所见，名称被定义为*" name ":"[parameters(' siteName ')]"*

参数文件会是这样的:

现在，对于每个微服务，我们需要这些参数文件:

想象一下拥有 30-50 个微服务:你需要维护所有这些文件，这很容易成为可维护性问题。它就是不可扩展。

通过使用线束变量，你可以把这个负担减少到只有一个参数文件。用户可以在他们的 ARM 模板中利用内置变量，如服务、环境、秘密和工作流。查看我们关于[内置变量](https://docs.harness.io/article/aza65y4af6-built-in-variables-list)的官方文档了解更多细节。

Harness 在向代理发送手臂展开请求之前解析所有这些变量。这是 Harness 提供的一个强大的工具，可以在 ARM 参数之上模板化您的 ARM 模板。

您也可以将参数文件中的参数定义为工作流变量，在部署时，您可以将它们的值提供为 OrderService、PaymentService 等。

在部署期间，您可以提供这些变量的值。您可以创建一个具有多个阶段的管道，数量与您要部署的微服务一样多，并且您可以使用相同的工作流并提供不同的运行时值。

支付服务第一阶段-webapp:

订单服务-webapp 的第 2 阶段:

在 Azure 门户上，你将能够看到不同的 web 应用程序是使用相同的 ARM 模板和参数文件创建的。

## Harness 如何为 ARM 部署提供回滚支持？

在 ARM 部署期间，我们需要处理以下场景的故障:

*   如果从 Git 下载 ARM 模板时出现问题。
*   如果在处理 ARM 模板时出现问题。
*   如果在 ARM 部署开始后 Azure 端出现故障。

对于第一点和第二点，我们不需要做任何事情，因为部署尚未开始。对于第三点，Harness 执行回滚。让我们来探究一下是如何做到的。

假设您有一个资源组(姑且称之为 RG1 ),它有两个资源，R1 和 R2。假设您正在尝试使用 RG1 中的 ARM 模板以增量模式部署额外的资源(R3、R4)。R3 创建成功，但在创建 R4 时，出现了问题。理想情况下，您应该能够回到以前的状态(在这种情况下，只有两个资源:R1 和 R2)。然而，Azure 让您的部署保持原样，而不是回滚。现在 RG1 中有了 R1，R2 和 R3。这是因为 Azure 本身不支持回滚。

Harness 通过在对现有基础设施进行任何更改之前保存资源组的当前状态，克服了 Azure 的局限性。在回滚过程中，现有模板以 **COMPLETE** 模式部署，这样可以确保只部署模板中提到的资源。例如，任何新部署的都将被删除。

这里有一个在你开始手臂展开之前会发生什么的图表:

*   管理器服务发送启动 ARM 部署的请求。
*   委托向 Azure 发送请求，为现有资源组生成模板。
*   Azure 将 ARM 模板发送给代表。
*   代理将 JSON 响应转发给管理器。
*   管理器将 ARM 模板保存到 MongoDB。

如果机械臂展开过程中出现故障:

*   管理器发送一个查询，在执行开始前保存 JSON 模板。
*   管理器使用当前执行 ID 获取现有模板。
*   经理将新的 ARM 部署任务发送给代表。
*   委托在完整模式下使用现有的 ARM 模板运行新的部署。
*   代理轮询部署状态。
*   部署状态会定期从代表处接收，并显示在 UI 上。

## 结论

我们关于 Harness 基础设施供应器和 ARM 模板的博客系列到此结束。

在这篇文章中，我们学习了在 ARM 模板中使用 Harness 变量，以及 Harness 如何提供回滚支持。

要了解基础知识，请随时参考本系列的第一篇[博客。有兴趣了解更多信息吗？](https://harness.io/blog/continuous-delivery/arm-templates-pt1/)[安排今天的演示](https://harness.io/demo/)。