# GraphQL:驾驭你的方式|驾驭

> 原文：<https://www.harness.io/blog/graphql-harness-your-way>

几乎所有 Harness 有意义的实体都通过 API 公开，包括应用程序、服务、云提供商、环境、工作流、管道、部署的实例和部署数据。这篇文章分享了如何使用 GraphQL 与 Harness API 进行交互。

今年早些时候，我们宣布了对 [Harness API](https://developer.harness.io/docs/platform/apis/api-quickstart/) 的更新。我们介绍了通过 GraphQL 与 Harness API 交互的能力。这篇博文将分享对 GraphQL 的更多见解，为您提供与 Harness API 交互所需的知识。

我们先从一个 graphQL 入门开始吧！

## GraphQL 是什么？

GraphQL ，它是一种由脸书创建的开源 API 查询语言，支持声明性数据提取。自从引入以来，GraphQL 已经将 developer mindshare 作为 REST APIs 的替代方案，为 API 开发人员和用户在使用 RESTful 架构时遇到的问题提供解决方案。

使用 GraphQL 的最大优势是它的灵活性。GraphQL 不使用返回固定数据结构的多个端点，而是公开一个端点并响应多个查询。顾名思义，GraphQL 以图形结构对您的业务领域建模。表示为图结构的节点和边的字段和关系最小化了每个请求在客户机和服务器之间传递的数据量。

客户机在查询中精确地指定它需要的数据，响应的结构精确地遵循由查询定义的嵌套结构。这可以防止常见的数据过度提取(返回一个查询，其中客户端拥有的信息多于所需信息)或数据不足提取(因此需要多次查询)的问题。

为了定义 API 的能力，GraphQL 要求使用 [GraphQL 模式定义语言](https://graphql.org/learn/schema/) (SDL)在模式中定义对象类型。这个模式充当客户机和服务器之间的契约，定义客户机如何访问数据。这有利于开发团队，因为他们已经定义了一种共享的语言和对领域模型的理解。

这里有一些 SDL 的例子。

//定义一个人:

类型人{
姓名:字符串！
年龄:Int！
}

//表达帖子与人的关系:

类型帖子{
标题:字符串！
作者:人！
}

//表达关系的另一端。一人多岗:

类型人{
姓名:字符串！
年龄:Int！
帖子:【帖子！]!
}

这些示例显示了对象具有强类型字段。结尾带有感叹号的字段表示必填字段，这些字段将是客户端请求的一部分。

一旦 GraphQL 服务开始运行，它就可以接收查询以根据模式进行验证和执行。检查查询以确保它只引用定义的类型和字段，然后运行函数产生结果。例如查询:

{
帖子{
标题
作者
}
}

可能会产生 JSON 结果:

{
"post": {
"title ":《星球大战》，
"author ":《卢克·天行者》
}
}

理解了 SDL 之后，您就可以开始使用 Harness 的 API Explorer 来执行查询和变异了。让我们讨论一下 API Explorer 的一些特性和例子。

## 线束 API 资源管理器

Harness API Explorer 使用户能够对 Harness 实体执行 CRUD 操作。API Explorer 基于一个名为 [GraphQL](https://github.com/graphql/graphiql) 的 GraphQL 集成开发环境(IDE)。在屏幕左侧的顶部可以找到功能按钮，一个编辑器，和一个“查询变量”,用于传递突变变量。

Use the right side of the screen to look up query and mutation schemas. Use the left side of the screen to run GraphQL queries against Harness.

文档资源管理器让您可以轻松地对查询进行建模。单击浏览器右侧的文档超链接，访问文档浏览器。您可以利用搜索栏功能快速访问特定于 Harness 的模式详细信息。

下面是如何在 API explorer 中做一个简单的查询。如视频所示，我编辑了查询以返回关于应用程序的更多信息。

如果您想获取基于 applicationID 的服务列表，您可以在如下所示的查询中指定。

{
服务(
过滤器:[
{应用:{ operator: EQUALS，values:["<application id>"]}
}
限制:1000
) {
pageInfo {
合计
}
节点{
id
名称
}
}
}

请注意，query.services 函数返回分页的数据，因此我们在查询中包含 pageinfo 和 nodes 对象来形成返回。

关于基本查询的更多示例，请参见文档章节[此处](https://developer.harness.io/docs/platform/apis/api-quickstart/#notes)。

突变提供了一种修改(创建、更新、删除)服务器端数据的方法。下面是一个在 Harness 中创建应用程序的变异示例。

我首先定义了变异，我称之为 createApp，它将一个名为 App 的查询变量作为必需的 CreateApplicationInput。我使用 createApplication 功能并传递 **app** 作为输入。输入至少需要一个名称字段，因此我使用必填字段以及 clientMutationId 和应用程序描述来定义查询变量。我希望获得创建的应用程序的名称和 id 以及 clientMutationId，所以我在第 3-8 行中指定了该返回。

通过变异，您可以创建、更新和删除线束对象，包括用户、应用程序、机密和管道。如果你想要更多的查询和变异的例子，请看 Harness API 文档[这里](https://developer.harness.io/docs/platform/APIs/harness-rest-api-reference)。

## 驾驭你的方式

几乎所有 Harness 有意义的实体都通过 API 公开，包括应用程序、服务、工件、云提供商、环境、工作流、管道、部署的实例和部署数据。我们在二月份的[产品更新](https://harness.io/blog/product-updates/harness-product-update-february-2020/)中宣布了使用 GraphQL 与 Harness API 交互的能力。GraphQL 为您的消费应用程序提供了这些效率和可靠性特性:

*   范围——每个请求可以查询您想要的所有资源和数据，并且只查询您想要的数据。
*   自省——您的客户端应用程序可以查询 API 模式以获得关于 API 的详细信息。
*   层次结构——查询的嵌套字段反映了查询返回的 JSON 数据的结构。
*   强类型化——应用程序可以为每个字段指定预期的数据类型，并接收清晰具体的错误通知。
*   面向未来——graph QL 允许我们逐步公开新的字段和类型，并淘汰过时的字段，而无需对 API 进行版本控制或中断您现有的查询。

这篇博客分享了 GraphQL 的基础知识以及如何使用 Harness API Explorer。您也可以通过 Postman 执行[线束 GraphQL 查询，参见线束文档进行设置。](https://developer.harness.io/docs/first-gen/firstgen-fa-qs/harness-graph-ql-api-faqs/)

如果你想了解更多关于 GraphQL 的知识[，点击这里](https://graphql.org/learn/)获取官方学习链接。我希望这能很好地为您提供使用 GraphQL 的资源，以您自己的方式使用 Harness。

干杯！
- Tiff