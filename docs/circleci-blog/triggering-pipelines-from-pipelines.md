# 通过从其他管道触发管道来管理复杂的开发项目| CircleCI

> 原文：<https://circleci.com/blog/triggering-pipelines-from-pipelines/>

众所周知，软件开发正变得越来越复杂。软件的单个元素，如应用程序、库和服务，是相互联系的，并依赖于许多其他元素。开发团队处理他们开发、维护或依赖的整个服务生态系统，而服务生态系统又依赖于由独立团队维护的其他软件生态系统。维护这个生态系统就像你想象的那样复杂。确保整个系统正常运转，并按照预期保持运转，是一项极具挑战性的工作。

从其他管道触发管道是众多技术之一，可以帮助您管理复杂的相互连接和相互依赖的开发项目。在本教程中，我将介绍一些使用这种技术的用例，以及为什么它会有帮助。我将展示一个如何使用 CURL 实现管道到管道触发器的例子，您可以跟着我做。

## 先决条件

本中级到高级教程需要对 CircleCI 及其管道有所了解。如果您使用 CircleCI 并且有一些已经配置好的项目，这将特别有用。本文涵盖的所有内容将适用于所有的 CircleCI 计划。

## 管道到管道触发器的用例

考虑从现有管道触发新管道有几个原因。微服务编排就是一个典型的例子。一种常见的模式是在每个版本控制存储库中都有微服务，每个微服务都需要一个单独的 CircleCI 管道配置。随着一个服务的部署，您可以重新部署依赖于这个新版本的其他服务。或者你可以触发一个扩展的[集成测试](/blog/unit-testing-vs-integration-testing/)套件，独立于更广泛的测试和部署流程运行。

如果不使用微服务，可以使用管道到管道触发器来发布和测试与下游使用者集成的库。

## 实现管道触发器

触发管道的一个工作流程遵循以下步骤:

*   执行第一个管道，进行构建，运行一些测试，并触发部署。
*   该过程成功完成后，第一个管道对 CircleCI 进行 API 调用，以触发不同项目中的另一个管道。
*   第二个管道执行构建、运行测试并触发部署。

在这种情况下，第一个管道还可以传递一些管道参数，如部署的名称和版本，以便新触发的管道有一些上下文。

## 使用管道触发器协调单独的服务

要编排两个独立的服务，可以从单个作业中调用 CircleCI API。您需要知道项目的名称，以及您希望从该作业中触发的管道。要用 API 处理管道，您需要组织名称(`zmarkan`)和项目名称(`pinging-me-softly`，因为我有古怪的幽默感)。

接下来，获取个人 API 令牌。您必须拥有对这两个项目的推送访问权限才能工作。您可以将 API 令牌存储在这个项目环境变量中。为了增加安全性，将其存储在[环境](https://circleci.com/docs/contexts/)中。在我的例子中，我使用了一个名为`circleci-api`的上下文，环境变量的名称为`CIRCLE_API_TOKEN`。

要触发管道，您需要向项目的管道端点发送 POST 请求。这是我在本教程中使用的 URL:

```
https://circleci.com/api/v2/project/gh/zmarkan/pinging-me-softly/pipeline 
```

有效负载必须包含您想要用来触发管道的分支或标记的详细信息，以及您想要传递给它的任何管道参数。

以下是一个作业示例:

```
 trigger-new-pipeline:
      docker: 
        - image: cimg/base:2021.11
      resource_class: small
      steps:
        - run:
            name: Ping another pipeline
            command: |
              curl --request POST \
                --url https://circleci.com/api/v2/project/gh/zmarkan/pinging-me-softly/pipeline \
                --header "Circle-Token: $CIRCLECI_API_KEY" \
                --header "content-type: application/json" \
                --data '{"branch":"main","parameters":{"image-name":"'$DOCKER_LOGIN"/"$CIRCLE_PROJECT_REPONAME":"0.1.<< pipeline.number >>'"}}' 
```

该作业使用了`cimg/base`图像和`small`资源类，这对于发送一个 HTTP 请求来说已经足够了。唯一的依赖是所有 CircleCI 图像附带的卷曲工具，包括`cimg/base`。

要触发管道，只需运行一个命令:`curl`。API 密钥在`Circle-Token`头中传递，在包含令牌的环境变量中传递。有效载荷主体是一个 JSON 对象，它包含一个`branch`和参数:

```
 { 
    "branch":"main",
    "parameters": {
        "image-name":"'$DOCKER_LOGIN"/"$CIRCLE_PROJECT_REPONAME":"0.1.<< pipeline/ number>>'"
    }
  } 
```

`image-name`参数的值是由三个变量构成的字符串:

*   `$DOCKER_LOGIN`是在该项目或上下文中设置的环境变量。
*   `$CIRCLE_PROJECT_REPONAME`是 CircleCI 中指定这个项目的内置环境变量。
*   `pipeline.number`是另一个内置的环境变量。

总的来说，这个值提供了我们在上一步中构建的 Docker 映像的名称。我在[中使用了`zmarkan/demo-full:0.1.119`这个例子](https://app.circleci.com/pipelines/github/zmarkan/pinging-me-softly/9/workflows/b17cb199-cc43-4275-967c-686b47d5824a/jobs/9)。

## 处理管道触发器

CircleCI 会自动为您处理管道触发器，就像它处理存储库中新的 git 提交一样。默认情况下，您的所有工作流都将运行，并且将执行您在 API 调用中指定的分支或标记的最后一次提交。对于本教程，我指定了`main`。

您可以在您的作业和工作流中处理您传递的任何参数(在我的例子中是 T0)。只需确保在配置的`parameters`部分声明它们:

```
 version: 2.1

  parameters:
    image-name:
      type: string
      default: "Not a real image name"

  jobs:
    print-image-name:
      docker: 
        - image: cimg/base:2021.11
      steps:
        - checkout
        - run:
            name: Echo Image name
            command: echo << pipeline.parameters.image-name >>

  workflows:
    run-workflow:
      jobs:
        - print-image-name 
```

这个管道示例只有一个工作流。它打印出我们在前面的管道中构建并通过 API 传递给这个管道的图像的名称。

## 结论

在本文中，我演示了如何使用 CircleCI API 在第一个项目的管道完成时触发另一个管道。这是一个简单的过程，只需要从正在运行的作业中调用一个 API，所有 CircleCI 用户和组织都可以使用。

这种技术为编排复杂的工作流开辟了新的可能性，包括微服务架构，并将昂贵的集成测试工作从构建和部署场景中分离出来。对于你和你的团队来说，还有很多很多的可能性可以尝试。

如果你对这篇文章有任何问题或建议，或者对未来的文章和指南有什么想法，请通过 [Twitter - @zmarkan](https://twitter.com/zmarkan) 或 [email 给我](mailto:zan@circleci.com)联系我。