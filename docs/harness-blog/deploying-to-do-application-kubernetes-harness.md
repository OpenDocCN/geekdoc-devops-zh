# 使用 Harness | Harness 将待办事项应用程序部署到 Kubernetes

> 原文：<https://www.harness.io/blog/deploying-to-do-application-kubernetes-harness>

本教程展示了如何使用 Harness CI/CD 轻松部署 Kubernetes 待办事项应用程序。

持续集成和[交付](https://harness.io/blog/what-is-continuous-delivery) (CI/CD)是任何成功的 DevOps 方法的一个非常重要的部分。DevOps 确保使用微服务、容器化应用程序、使用 CI/CD、使用 Kubernetes 等云原生技术部署应用程序等等。所有这些都加速并简化了软件开发过程，并帮助开发人员协作。 [Harness](https://harness.io/products/continuous-delivery) 是一个现代软件交付平台，帮助成千上万的开发者采用 CI/CD。

我们希望展示使用 Harness 设置 CI/CD 并简化您的[软件交付流程](https://harness.io/blog/best-practices-software-delivery)是多么容易。以一个简单且易于开发人员理解的待办应用程序为例，我将向您展示如何使用 Harness 将它部署到 [Kubernetes。待办应用是每个开发者都熟悉的东西，涉及创建任务的简单操作。按照教程学习如何使用 Harness CI/CD 轻松部署这个应用程序。](https://docs.harness.io/category/uj8bqz9j0q-kubernetes-executions)

## 先决条件:

*   下载并安装 [Node.js](https://nodejs.org/en/download/)
*   [驾驭平台](https://app.harness.io/auth/#/signup/?utm_source=internal&utm_medium=social&utm_campaign=community&utm_content=kubernetes-toto-article&utm_term=get-started)免费账号访问
*   Kubernetes 从任何云提供商处进行集群访问。您还可以使用 [Minikube](https://minikube.sigs.k8s.io/docs/start/) 或 [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/) 来创建单节点集群。

## 教程:

使用命令 git clone[https://github.com/pavanbelagatti/todo-app-example.git](https://github.com/pavanbelagatti/todo-app-example.git)将回购克隆到您的本地机器上。

然后，使用以下命令进入主文件夹:

###### cd 待办事项-应用程序

然后使用命令 **npm install** 安装项目所需的所有依赖项。

使用命令节点 **app.js** 运行应用程序，并在浏览器中访问 [http://localhost:8080](http://localhost:8080/) 以查看应用程序的工作情况。要运行测试，您可以简单地使用命令 **npm test。**我们将 **deployment.yaml** 和 **service.yaml** 文件设置为通过 Kubernetes 部署和公开应用程序。

下面是 **deployment.yaml** 文件:

###### apiVersion: apps/v1

###### 种类:部署

###### 元数据:

###### 标签:

###### 应用程序:待办事项-应用程序

###### 名称:todo-app

###### 规格:

###### 副本:2

###### 选择器:

###### 匹配标签:

###### 应用程序:待办事项-应用程序

###### 模板:

###### 元数据:

###### 标签:

###### 应用程序:待办事项-应用程序

###### 规格:

###### 容器:

###### - image: dockerhub 图像名称

###### 名称:todo-app

###### 端口:

###### -集装箱港口:8080

下面是 **service.yaml** 文件:

###### apiVersion: v1

###### #表示这是一项服务

###### 种类:服务

###### 元数据:

###### #服务名称

###### 名称:todo-app

###### 规格:

###### 选择器:

###### Pods 选择器

###### 应用程序:待办事项-应用程序

###### 端口:

###### #端口映射

###### -端口:80

###### 目标港口:8080

###### 协议:TCP

###### 类型:负载平衡器

## CI/CD:使用线束

Harness 有一个非常漂亮的 UI，可以轻松地帮助开发人员毫不费力地进行 CI/CD。一旦您[在 Harness](https://app.harness.io/auth/#/signup/) 注册，请确保您可以访问 Kubernetes 集群来部署您的应用程序。登录线束平台，确保基本连接器准备就绪。另外，确保在目标 Kubernetes 集群上安装了代理。

Harness Delegate 是一个服务/软件，您需要在目标集群(在我们的例子中是 Kubernetes)上安装/运行，以将您的工件、基础设施、协作、验证和其他提供者与 Harness Manager 连接起来。第一次设置线束时，安装线束委托。

如果你想了解更多关于 Delegate 的信息，你可以[阅读这里](https://docs.harness.io/article/2k7lnc7lvl-delegates-overview)。

在 Harness 上创建一个项目以设置 CI/CD。首先，选择持续集成模块，然后选择交付模块。

‍

正如您在上面看到的，我们将首先进行测试和构建，然后将应用程序部署到 Kubernetes 集群。

测试和构建设置如下所示:

‍

基本上，我们正在配置基础设施(在我们的例子中是 Kubernetes)，指定执行测试，最后在成功的测试用例之后将构建映像推送到 Docker Hub。

当您单击测试步骤时，您将看到我们的配置:

同样，交付模块的详细信息和配置如下所示:

完成所有配置后，您可以保存并运行管道，以查看管道步骤的成功执行。首先，您将看到测试和构建阶段完成，然后是部署阶段。

部署阶段的执行如下所示:

## 了解更多信息

现在，您可以使用 Harness 平台在 Kubernetes 上测试、构建和部署您的应用程序。如果您有兴趣了解有关使用 Harness 和智能软件交付的更多信息，请查看我们的[开发者中心](https://developer.harness.io/)以获得更多分步教程、视频和参考文档。

‍