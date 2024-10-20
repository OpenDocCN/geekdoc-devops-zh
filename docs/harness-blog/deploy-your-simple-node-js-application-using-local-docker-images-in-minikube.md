# 使用 Minikube | Harness 中的本地 Docker 映像部署简单的 Node.js 应用程序

> 原文：<https://www.harness.io/blog/deploy-your-simple-node-js-application-using-local-docker-images-in-minikube>

学习如何使用一个简单的 Node.js 应用程序，了解如何使用本地映像在 minikube 上构建和部署应用程序。

我们有一个简单的 Node.js 应用程序，我们将在本教程中使用它来演示如何使用本地映像在 minikube 上构建和部署应用程序。

### 先决条件:

### Minikube 是什么？

Minikube 是一种轻量级的 Kubernetes 方法，用于运行只有一个节点的简单集群来测试我们的应用程序。Minikube 可用于 Linux、macOS 和 Windows 系统。

辅导的

创建一个新目录来存储您的简单应用程序代码。克隆[这个回购](https://github.com/pavanbelagatti/Simple-Node-App)。

克隆存储库后，请执行“npm install”来安装所需的依赖项。

您的应用程序和 docker 文件现在可以在本地构建应用程序了。下一步是在 Minikube 上构建映像。因此，我们必须使用以下命令启动 minikube:

minikube 启动

您应该会在终端上看到类似如下的输出:

这将启动所需的机器和资源。

接下来，让 Minikube 访问您的本地 Docker 图像。

您应该在根文件夹中看到 docker 文件，它指定了构建映像的指令:

从节点:14-阿尔卑斯山作为发展

环境节点 _ 环境开发

#添加工作目录

工作目录/应用程序

#缓存和安装依赖项

复制 package.json。

运行 npm 安装

#复制应用程序文件

收到。。

#暴露端口

曝光 3002

#启动应用程序

CMD [ "node "，" app.js" ]

此外，还有一个. dockerignore 文件，它允许我们从上下文中排除文件，并使您的构建更快。在这个文件中，我刚刚包含了 node_modules。

根目录下有一个 YAML 文件(deployment.yaml ),这是在 Kubernetes 上运行 web 服务器的部署配置。打开文件，您应该看到以下内容:

apiVersion: apps/v1

种类:部署

元数据:

名称:应用程序部署

标签:

应用程序:节点应用程序

规格:

副本:2

选择器:

匹配标签:

应用程序:节点应用程序

模板:

元数据:

标签:

应用程序:节点应用程序

规格:

容器:

-名称:应用程序部署

图片:帕万萨/孟买-应用程序:最新

资源:

请求:

中央处理器:“100 米”

imagePullPolicy: IfNotPresent

端口:

-集装箱港口:3002

ImagePullPolicy 设置为“Never ”,因为我们不会从图像注册表(如 DockerHub)中提取任何图像。如果您指定为“总是”，那么图像将从注册表中搜索并最终提取。

使用以下命令创建部署:

kubectl apply -f deployment.yaml

终端上的输出应该如下所示:

deployment.apps/nodeapp 创造了

Minikube 无法访问您的本地 Docker 图像。这意味着是时候将图像加载到 minikube 上了。

让我们使用以下命令直接在 minikube 上构建映像:

minikube 映像构建-t mynode/app。

[语法:minikube image build -t 。]

您应该会看到以下输出:

让我们使用以下命令来验证这一点:

库贝克特尔日志 deployment.apps/nodeapp

运行上述命令后，您将看到应用程序运行消息，类似于以下内容:

服务器在 localhost:3002 上运行

检查浏览器，您应该会看到以下消息:

您也可以通过访问 Docker 桌面仪表板来验证这一点。您应该看到容器正在运行。

恭喜你！您已经将您的应用程序容器化并推送到 minikube 集群上。

## 使用线束 CD 展开

只要你是在一个小团队中学习和试验，Minikube 就能很好地工作，但是如果你的项目并不小，而且有相当多的开发人员在工作呢？然后，您可以利用 Harness CD 的力量来部署您的应用程序。Harness CD 使 Kubernetes 部署变得简单，并为您的管道提供了单一平台。

注册免费试用[线束](https://app.harness.io/auth/#/signin)并选择 *TryNextGen* 选项卡获得无缝体验。创建一个新项目并选择连续交付模块。开始创建新的管道，并添加管道所需的所有细节。

***注意:*** 为了让 Harness 发挥它的魔力，您需要一个叫做“委托”的东西在您的 Kubernetes 集群上运行。别担心，我们有一个简单的教程来帮助你[设置代理](https://docs.google.com/document/d/12jRJiwZ2jbtrrBqRykokpc2ai-ZN0OEqqNcrYrsHwy4/edit?usp=sharing)。

接下来，为您的应用程序指定服务、基础设施和部署策略。设置好一切后，保存配置并运行以部署应用程序。

这就对了。一旦管道成功运行，您应该看到您的应用程序部署在指定的 Kubernetes 集群上。这可以通过 kubectl 命令“kubectl get pods”来验证。

再次感谢您的阅读，别忘了[注册线束光盘免费试用](https://app.harness.io/auth/#/signup/?module=cd)。