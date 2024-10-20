# 线束工具提示框架|线束

> 原文：<https://www.harness.io/blog/harness-tooltip-framework>

看看我们是如何选择工具提示框架的，我们是如何实现它的，我们是如何从工具提示中获益的，并看看格式良好的工具提示的例子。

对于开发人员和文档工程师来说，添加和管理工具提示可能是一场噩梦。在 Harness，我们设计了一个框架，使文档团队能够在没有开发人员干预的情况下动态添加工具提示。

如果你也在试图解决类似的问题，那么通读这篇博客，了解我们的成功之旅！

## 为什么是工具提示？

现代的自我可靠的产品应该需要最少的手握。如果复杂的表单向用户传达了表单所期望的内容，那么使用起来就会容易得多。

看看 GCP 云控制台仪表板上工具提示用法的一些例子。工具提示为用户提供了关于不同字段的足够信息。他们还提供了详细信息的<learn more="">链接。</learn>

Tooltip Example 1

Tooltip Example 2

## 挑战

对于 UI 工程师来说，有数百万个库可以让你将这些帮助文本添加到你的 web 应用程序中。如果您有一个小的应用程序，并且只有很少的表单可以与之交互，那么这种方式非常有效。

在像 Harness 这样拥有多个产品和数千个包含多个字段的表单的组织中，维护工具提示对于工程师来说是一项真正的开销:

*   内容可以反复变化。
*   这可能会影响单元和集成测试快照。

我们很早就意识到，我们需要一个可伸缩的解决方案来满足我们的需求，而且这个要求很明确:工具提示应该由文档团队来维护，尽可能少地引入开发人员的干预。以下是我们尝试过的一些选项:

1.  考虑了工具提示,但它只是解决了我们的一部分问题。
2.  将 DOM 悬停在某个字段上，然后显示源代码中的工具提示。这由于不安全而被拒绝，并且难以实现和维护。

根据我们的调查结果，我们决定采取以下行动:

1.  文档团队应该有一个单独的库来维护工具提示。
2.  上述存储库将与核心 UI 存储库握手以显示工具提示。
3.  高度、宽度、颜色等属性。的内容也应该是可定制的。
4.  对样式或标签中字段的任何修改都不应影响绑定的工具提示。
5.  工具提示不应该改变/影响原始表单字段。

## 这对开发者来说意味着什么？

在分析了一些工具提示库是如何工作的之后，我们认为唯一地标识每个 DOM 节点是必要的。如果 web 页面中的所有节点元素都可以用一个惟一的 ID 来标识，那么我们就可以根据这些 ID 来映射内容。

从 Harness 的“部署概述”页面查看以下一些快照。

The heading `Failed Deployments (7)` is being uniquely identified by an attribute `data-tooltip-id` which is “overview_FAILED_DEPLOYMENT.”

Similarly, `Active Deployments` is uniquely tied with the data-tooltip-id “overview_ACTIVE_DEPLOYMENTS.”

在这里，属性“数据-工具提示-id”是关键因素。开发人员必须将该属性添加到所有可能包含工具提示内容的 HTML 元素中。

## 这对文档团队意味着什么？

下一个挑战是为文档团队提供一个框架来管理这个 ID 的工具提示内容。我们创建了一个工具提示编辑器来解决这个问题。

The editor pops up by running a simple command in the console

但是工具提示编辑器是如何工作的呢？

编辑器扫描现有的 DOM 并列出所有具有“data-tooltip-id”的节点。在这种情况下，我们可以看到“overview_FAILED_DEPLOYMENT”和“overview_ACTIVE_DEPLOYMENT”被列出。

文档工程师只需打开这个编辑器，根据 IDs 添加内容，然后复制最新的数据集。我们希望内容是 markdown 格式的，这样它就能支持现代浏览器的所有标准格式。我们还支持工具提示的自定义“宽度”,因此处理固定大小的图像变得很容易。

## 下一步？

添加和编辑内容后，整个数据集将以 YAML 格式复制。然后，这个更新的数据集被复制并合并到工具提示报告中的字典中。

在 Harness，经过几次讨论，我们决定使用 YAML 格式，因为它易于使用和处理。也可以考虑其他格式，比如 JSON。

## 工具提示的发布和呈现

一旦 PR 被合并，我们就运行我们自己的 [CI](https://harness.io/products/continuous-integration/) 并为同一个回购生成一个 npm 包。这个包安装在主 repo 中时，有最新的数据集，它本质上是 tooltip-id 到内容的映射。

然后是渲染部分，其实挺简单的。我们利用 React 上下文的强大功能创建一个“TooltipContextProvider ”,并将最新的数据集传递给它。

Passing the dictionary to tooltip context in the main repo

Creating the tooltip context provider to be used above

A function that returns a tooltip content from the dictionary for a particular ID

Updating the HTML field to render label with the tooltip icon and content

这种集成可以无缝地工作。只要我们有 data-tooltip-id，该字段旁边就很容易有帮助文本。此外，即使内容多次更改，职能团队也不会为此承担负担。

## Harness 如何从使用这个工具提示框架中获益？

我们已经使用这个框架添加了大约 1.5K 的工具提示，随着我们发布的越来越多，这个列表还会继续增加。

以下是一些格式良好的线束工具提示片段:)

## 结论

创新是 Harness UI 团队的一个关键部分，我们试图尽可能多地在内部构建。这个框架是一个完美的例子，因为我们使用自己的 CI 来发布、构建和发布这个包。如需了解更多 CI 知识，请参见[介绍驾驭 CI 企业](https://harness.io/blog/introducing-harness-ci-enterprise/)。

工具提示快乐:)

奇拉格