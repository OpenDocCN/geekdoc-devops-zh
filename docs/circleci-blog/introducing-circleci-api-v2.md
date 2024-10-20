# CircleCI API v2 简介

> 原文：<https://circleci.com/blog/introducing-circleci-api-v2/>

今天，我们宣布我们 API 的 v2，它开放了九个新的稳定端点，允许控制[管道](https://circleci.com/docs/pipelines/)和[工作流](https://circleci.com/docs/workflows/)。v2 API 的预览版已经发布了几个月，随着时间的推移，我们将继续添加新的端点，包括一些目前作为不稳定预览版提供的端点。

在这篇文章中，我将概述 v2 API 中的第一个端点，它们现在对于您的生产使用是稳定的。

## 管道现在在 API 中是一流的

2019 年 3 月，我们宣布将“构建处理”简单更名为“管道”，并告诉你，在你再次考虑管道之前，还需要一段时间。从那时起，你们中的许多人已经看到管道出现在 v2 API 的预览版和我们新的 UI 预览版中。通过 v2 API，我们扩展了管道，为您提供了触发和检索管道的编程方式。

管道封装了你在 CircleCI 上运行的东西。当您通过对 git 存储库的提交或对项目的 API 调用来触发管道时，该管道表示您在 CircleCI 中执行工作的请求的开始，并持久保存一个稳定的管道 ID 以及关于触发原因、我们将把管道的运行归因于谁的参与者以及用于在管道中运行工作流的配置的信息。

> 管道是 CircleCI 要求的工作单元。每一个都是在给定的项目、给定的分支(您的默认分支作为默认值应用)上由具有特定配置的特定参与者触发的(或者以隐含的方式检索上游配置，就像我们历史上的 GitHub 集成一样)。

如果你在 2018 年 9 月之后添加你的项目，你一直在使用管道，甚至可能没有意识到这一点。

### “项目 _slug”

CircleCI v2 API 为您的项目使用一个与其上游存储库相对应的标识符。例如，如果您想从 CircleCI 获取关于 GitHub 存储库[https://github.com/CircleCI-Public/circleci-cli](https://github.com/CircleCI-Public/circleci-cli)的信息，您可以在 CircleCI API 中将其称为`gh/CircleCI-Public/circleci-cli`，它是项目类型、您的“组织”名称和存储库名称的“三元组”。对于项目类型，您可以使用`github`或`bitbucket`以及更短的形式`gh`或 `bb`，这些现在在 API v2 中都得到了支持。组织是您在版本控制系统中的用户名或组织名称。

在 API v2 中，我们引入了称为 project_slug 的三元组的字符串表示，其形式为:`<project_type>/<org_name>/<repo_name>`。当提取有关项目的信息以及通过 ID 查找管道或工作流时，project_slug 包含在有效负载中。然后可以使用 project_slug 来获取关于项目的信息。将来我们有可能改变 project_slug 的形状，但是在任何情况下，它都可以作为给定项目的可读标识符。

### 管道参数

v2 API 现在允许您向管道触发器添加参数。

要使用参数触发管道，您需要在 POST 主体中传递 JSON 包中的`parameters`键。对于上面的配置，这可能类似于:

```
curl -u ${CIRCLECI_TOKEN}: -X POST --header "Content-Type: application/json" -d '{
  "parameters": {
    "workingdir": "./myspecialdir",
    "image-tag": "4.8.2"
  }
}' https://circleci.com/api/v2/project/:project_slug/pipeline 
```

与 1.1 版作业触发端点中直接注入到正在运行的作业环境中的参数不同，管道参数在配置处理过程中解析，使它们可用于配置的大多数部分。

在`.circleci/config.yml`文件的顶层关键字中使用`parameters`节声明管道参数。然后在`pipeline.parameters`范围内引用参数的值。

下面的示例显示了一个带有两个管道参数的配置，`image-tag`和`workingdir`，这两个参数都用在后续的配置节中:

```
version: 2.1
parameters:
  image-tag:
    type: enum
    default: "latest"
    enum: ["latest","bleeding","stable201911"]
    # NOTE use of enum allows only the options you want
    # You could, instead, choose to make it an arbitrary string
  workingdir:
    type: string
    default: "main-app"

jobs:
  build:
    docker:
      - image: circleci/node:<< pipeline.parameters.image-tag >>
    environment:
      IMAGETAG: << pipeline.parameters.image-tag >>
    working_directory: /Code/<< pipeline.parameters.workingdir >>
    steps:
      - run: echo "Image tag used was ${IMAGETAG}"
      - run: echo "$(pwd) == << pipeline.parameters.workingdir >>" 
```

请记住，如果您声明了一个管道参数，那么任何拥有触发凭证(例如，一个与拥有上游 repo 提交权限的用户相关联的 API 令牌)的人都能够有效地将该值注入到您的配置中。在上面的例子中，如果你不想允许 docker 标签的注入，你就不要使用这个技术。

### 使用管道参数的条件工作流

我们在一个工作流声明下添加了一个新的`when`子句(我们也支持反向子句`unless`),用一个布尔值来决定是否运行该工作流。

这种构造最常见的用途是使用管道参数作为值，允许 API 触发器传递该参数来确定要运行哪些工作流。

下面是使用两个不同管道参数的配置示例，一个用于驱动特定工作流是否运行，另一个用于确定特定步骤是否运行。

```
version: 2.1

parameters:
  run_integration_tests:
    type: boolean
    default: false
  deploy:
    type: boolean
    default: false

workflows:
  version: 2
  integration_tests:
    when: << pipeline.parameters.run_integration_tests >>
    jobs:
      - mytestjob

jobs:
  mytestjob:
    steps:
      - checkout
      - when:
          condition: << pipeline.parameters.deploy >>
          steps:
            - run: echo "deploying" 
```

### 九个新端点

今天发布的 v2 中的明星是 CircleCI API v2(管道和工作流)中第一组端点的稳定性和普遍可用性。

以下管道端点分别允许您，

*   触发项目分支上的管道，并接收其 ID 供以后使用
*   检索给定项目的最新管道
*   筛选出请求者最近运行的管道

```
POST /project/{project-slug}/pipeline
GET  /project/{project-slug}/pipeline
GET  /project/{project-slug}/pipeline/mine 
```

你也可以

*   按 ID 检索单个管道
*   检索管道的工作流
*   检索用于运行管道的源配置和已处理配置

```
GET  /pipeline/{pipeline-id}
GET  /pipeline/{pipeline-id}/workflow
GET  /pipeline/{pipeline-id}/config 
```

其他三个新的端点允许您

*   检索单个工作流
*   检索单个工作流的作业
*   还有一个取消工作流的端点

```
GET  /workflow/{id}
GET  /workflow/{id}/job
POST /workflow/{id}/cancel 
```

## CircleCI API v2 支持 OpenAPI

有了 API v2，我们现在完全支持 [OpenAPI 3](https://swagger.io/specification/) 。您可以从这里下载反映当前生产 API 的实时规范:

## 了解更多信息