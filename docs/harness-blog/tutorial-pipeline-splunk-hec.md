# 教程:[可观察性]将管道事件发布到 Splunk HEC | Harness

> 原文：<https://www.harness.io/blog/tutorial-pipeline-splunk-hec>

在这个简短的教程中，我们将利用 Splunk HEC 与 Harness 集成。

让我们使用这个非常好的[文档](https://docs.harness.io/article/scrsak5124-publish-pipeline-events-to-an-http-endpoint)向 Splunk 发送线束管道输出。

***提示*** *:您必须要求我们启用的特征标志名称为****APP _ 遥测*** 。

我是 Splunk 的忠实粉丝，但你可能很快就会看到针对 ELK 的相同教程。

系好安全带。

## 场景描述-将 Splunk HEC 与线束一起使用

在这个简短的教程中，我们将利用 Splunk HEC 与 Harness 集成。如果你不熟悉 Splunk 的 **HTTP 事件收集器**，你可以查看他们关于这个主题的[文档。](https://docs.splunk.com/Documentation/SplunkCloud/8.2.2106/Data/UsetheHTTPEventCollector) 

## 教程-将管道事件发布到 Splunk HEC

### 第一步

您需要创建 Splunk HEC。请注意你的场景。就我而言，我有:

*   HTTP 而不是 HTTPS(因为这是一个快速实验)；
*   我不需要索引器确认；
*   我需要在 HEC UI 中启用令牌。

这是我的 Splunk HEC:

这是全局设置屏幕:

### 第二步

是时候启用集成了！

启用功能标志后，您可以转到要启用遥测的应用程序。您将在底部看到一个名为事件规则的新选项。

这是我将用来适应我的用例:

*注意:我们不能把令牌作为秘密，因为这还不是 GA**——擦亮你的眼睛！*

### 第三步

我将单击“测试”按钮，然后在我的 Splunk 搜索头中进行检查。

### 最后一步:

运行管道的时间:

这很有效！

这是一个非常快速的教程！但是就像我说的，我可能也会为麋鹿创建一个，所以请继续关注更新！

有任何问题或意见吗？让我知道-我总是乐意帮忙。

加百列