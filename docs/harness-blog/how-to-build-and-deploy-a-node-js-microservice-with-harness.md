# 如何使用 Harness | Harness 构建和部署 Node.js 微服务

> 原文：<https://www.harness.io/blog/how-to-build-and-deploy-a-node-js-microservice-with-harness>

说到现代软件开发，微服务是最热门的趋势之一。这篇博文将解释什么是微服务，为什么应该使用它们，它们的好处，以及如何使用 Harness 部署它们。

说到现代软件开发，微服务是最热门的趋势之一。这些小型的独立软件服务允许开发人员构建和部署更小的代码块，这些代码块可以更频繁地更新，并更快地响应用户需求。此外，通过微服务，团队可以根据自己的计划更新单个服务，而不是等待整个应用程序的新版本或操作系统升级。每个服务也更小，更容易理解、测试、记录和维护。此外，微服务可以帮助您解决开发过程中可能出现的任何挑战。

这篇博文将解释什么是微服务，为什么应该使用它们，它们的好处，以及如何使用 Harness 部署它们。

## 什么是微服务？

微服务是大型应用程序中的小型自治软件服务。传统的整体式应用程序是由构建在单一代码库中的模块组成的。在基于微服务的架构中，服务被设计为独立的模块化部分，可以单独部署。每个微服务通常处理一个特定的业务功能，并独立构建、部署和管理

***镜像信用:*** [***微服务. io***](https://microservices.io/)

虽然微服务有许多不同的定义，但它们都具有一些特征:

*   解耦和自治——它不依赖其他服务来完成它的工作。
*   轻量级——它专注于做好一件事，以便于维护和开发。
*   无状态——在请求之间不保留任何数据。
*   松散耦合——它不依赖其他服务来完成工作
*   模块化–它可以用作其他服务的构建模块。
*   可扩展–它可以独立扩展
*   可交付——您可以独立于其他服务来构建和部署它

## 为什么要用微服务？

正如我们前面提到的，这些小服务可以更频繁地更新。当需求经常变化、需要添加新功能，或者某个特定的服务因大量请求而陷入困境并需要升级时，这一点尤其有利。借助微服务，团队可以根据自己的计划更新单个服务，而不是等待整个应用程序的新版本或操作系统升级。

使用微服务的另一个原因是，它们可以帮助您解决软件开发中的新挑战，例如可伸缩性、持续集成(CI)、部署和维护。让我们更详细地看一下这些挑战。

### 微服务实现可扩展性

可伸缩性是系统、应用程序或流程处理增加的工作量的能力。当应用程序的用户群增长时，您需要能够添加更多的服务器来扩展系统，以便它能够处理增加的负载。当应用程序的用户群增长时，你需要添加更多的服务器，因为在应用程序变慢或完全停止工作之前，它只能处理这么多工作。

借助微服务，您可以独立扩展每项服务。您不必为整个应用程序添加更多的服务器；您可以为受打击最严重的特定服务添加更多服务器。这种方法被称为分片，常用于 Oracle、Postgres 和 MySQL 等数据库系统中，以扩展系统。

### 微服务支持 CI

CI 是定期构建和测试新代码的实践。当您持续测试代码时，您可以在 bug 和其他问题发布到产品之前捕捉到它们。CI 过程通常包括版本控制、依赖关系、代码质量度量以及其他帮助您的团队更好更快地构建软件的工具。icroservices 最适合用于设计您的应用程序，这样您可以轻松地集成和独立测试每项服务。这通常被称为“服务驱动的开发”

### 微服务部署

部署是将应用程序从开发环境转移到生产环境的过程。您应该在设计应用程序时考虑到微服务，以便每个服务都易于部署。借助微服务，您可以轻松地在环境之间移动每个服务或更新一个服务，而不会影响其他服务。

## 辅导的

我们来搭建一个 node.js 微服务，了解两个邮编之间的距离。

创建一个 app 目录来存储您的应用程序代码，并使用命令 npm init -y 来初始化项目。然后，让我们在 server.js 文件中添加主应用程序代码。

###### const express = require('express ')

###### const app = express()；

###### 常量端口=进程。环境。端口| | 3002；

###### const routes = require(' ./API/routes’)；

###### 路线(app)；

###### app.listen(port，function() {

###### console.log('服务器在端口上启动:'+port)；

###### });

让我们定义路由和控制器逻辑。首先，在根文件夹中创建一个名为 **api** 的新文件夹来保存我们的路由和控制器逻辑。

完整的项目文件夹结构如下:

‍

在 api 文件夹中，创建两个文件——controller . js 和 routes.js

将以下代码添加到 routes.js 文件中:

###### 使用严格的'；

###### 常量控制器=要求(./controller’)；

###### module.exports = function(app) {

###### app.route('/about ')

###### 。获取(控制器。关于)；

###### app。路线('/distance/:邮政编码 1/:邮政编码 2 ')

###### 。获取(控制器。获取距离)；

###### };

###### 将以下代码添加到 controller.js 文件中:

###### 使用严格的'；

###### var properties = require('../package.json ')

###### var distance = require('../服务/距离’)；

###### var 控制器= {

###### 关于:function(req，res) {

###### var aboutInfo = {

###### 名称:properties.name

###### 版本:属性.版本

###### }

###### RES . JSON(关于信息)；

###### },

###### getDistance: function(req，res) {

###### distance.find(req，res，function(err，dist) {

###### 如果(错误)

###### RES . send(err)；

###### RES . JSON(区)；

###### });

###### },

###### };

###### module.exports =控制器；

###### 现在，是时候编写一些代码来处理外部 API 了。我们将使用[zipcodeapi.com](http://zipcodeapi.com)通过密码计算两个位置之间的距离。去 http://www.zipcodeapi.com[拿 api 密匙。](http://www.zipcodeapi.com/index.php)

‍

‍

现在，在根文件夹中创建一个新文件夹，并将其命名为 **service** 。在服务文件夹中，创建一个名为 distance.js 的新文件，并添加以下代码:

var request = require(' request ')；

###### const apikey = process。env(环境)。zip code _ API _ key | " demoonly 00 XM8 gfwliqi 2 JF 2 fnebr 1 uzbpgcw 3 w8 dzyjkocopavgambarvrr "；

###### const 邮政编码 URL = ' https://www。邮政编码 API。com/rest/'；

###### const zip code URL = ' https://www . zip code API . com/rest/'；

###### var 距离= & gt

###### 查找:函数(req，res，next)}

###### request(zipCodeURL + apiKey

###### +'/距离。JSON/'+req。参数。邮政编码 1+'/'

###### +req . params . zip code 2+'/英里'，

###### 函数(错误、响应、正文){

###### 如果(！error & response . status code = = 200){

###### 响应= JSON。parse(正文)；

###### res.send(响应)；

###### }否则{

###### 控制台。日志(响应。状态代码+响应。体)；

###### RES . send({距离:-1 })；

###### }

###### });

######       });

###### }

###### };

###### };

###### module.exports =距离；

###### 现在，转到应用程序的主文件夹，使用 npm start 命令启动应用程序。

###### 转到您的[http://localhost:3002/about](http://localhost:3002/about)，您应该会看到文件夹的名称和版本。

‍

接下来，检查我们的下一条路线，即距离。去[http://localhost:3000/distance/pincode 1/pincode 2](http://localhost:3000/distance/pincode1/pincode2)。

添加 pincode1 和 pincode2，您应该会看到这两个邮政编码区域之间的距离。

例子如下:

再次尝试找出两个邮政编码之间的距离- 35004 和 86556。

‍

‍

因此，我们已经成功地构建了一个简单的微服务来了解两个邮政编码之间的距离。

使用 Harness 部署应用程序:

Harness 是一个现代软件交付平台，帮助组织轻松部署他们的应用程序。现在我们将看到如何使用 Harness 在 Kubernetes 上部署这个应用程序。

## 先决条件

我们需要做的第一件事是使用 Dockerfile 对我们的应用程序进行 dockerize 化。让我们为我们的应用程序编写一个简单的 docker 文件。

### 从节点:14-阿尔卑斯山作为发展

环境节点 _ 环境开发

###### #添加工作目录

###### 工作目录/应用程序

###### #缓存和安装依赖项

###### 复制 package.json。

###### 运行 npm 安装

###### #复制应用程序文件

###### 收到。。

###### #暴露端口

###### 曝光 3002

###### #启动应用程序

###### CMD [ "npm "，" start" ]

###### 使用下面的命令，构建 Docker 映像:

###### docker build -t 微服务-app。

使用以下命令运行 Docker 映像:

###### docker run -p 3000:3000 微服务-app

###### 使用 DockerHub 凭据再次构建映像:

###### docker build-t pavansa/微服务-app。

###### 现在，使用下面的命令将映像推送到 Docker Hub:

###### docker push pavansa/微服务-应用程序

###### 既然我们已经将应用程序作为映像推送到 Dcoker Hub，我们就可以使用 Harness CD 模块在 Kubernetes 上部署应用程序了。

###### 但是在此之前，我们需要创建 Kubernetes 清单文件 deployment.yaml 和 service.yaml 来部署我们的应用程序。

让我们创建 deployment.yaml 文件。

apiVersion: apps/v1

种类:部署

###### 元数据:

###### 标签:

###### app:微服务-app

###### 名称:应用程序部署

###### 规格:

###### 副本:2

###### 选择器:

###### 匹配标签:

###### app:微服务-app

###### 模板:

###### 元数据:

###### 标签:

###### app:微服务-app

###### 规格:

###### 容器:

###### -图片:pavansa/microservice-app

###### 名称:应用程序部署

###### 端口:

###### -集装箱港口:3000

###### 将应用程序代码推到一个新的 GitHub 存储库中。

###### 注册[线束平台](https://app.harness.io/auth/#/signup/?utm_source=internal&utm_medium=social&utm_campaign=community&utm_content=microservices-article&utm_term=get-started)并选择 CD(连续交付)模块。

设置所需的连接器，如 Docker Hub、GitHub 等和 delegate。

‍

[Harness Delegate](https://docs.harness.io/article/h9tkwmkrm7-delegate-installation) 是一个工具，您需要在目标集群(在我们的例子中是 Kubernetes 集群)上安装和运行它，以便将您的工件、基础设施、协作、验证和其他提供者与 Harness Manager 连接起来。第一次设置线束时，安装线束委托。

为应用程序配置一个简单的管道。在这里，您将定义服务、基础设施和执行类型。

一个服务代表你正在部署的东西，比如你的微服务的 Kubernetes 清单和 Docker 映像。基础设施告诉 Harness 您将在哪里部署应用程序。执行是这个阶段将服务部署到基础设施的方式。

确保您已经正确配置了所有内容，保存并运行管道以将您的应用程序部署到 Kubernetes 上。

您应该会看到一条成功部署的消息。

恭喜你！我们在 Harness CD 的帮助下成功搭建了一个 node.js 微服务，并部署在 Kubernetes 上。

微服务的利与弊

最终，使用微服务的利弊将取决于您的特定情况。然而，一般来说，微服务对于需要扩展的大型复杂应用程序来说是非常优秀的。因此，它们通常与容器和面向服务的体系结构结合使用。另一方面，微服务并不总是对小型应用有效。然而，微服务对大型和小型应用程序都适用，其有用性取决于如何实现它们。

## 微服务是现代软件发展的新趋势。它们允许开发人员构建和部署更小、更独立的代码块，这些代码块可以更频繁地更新，并更好地响应用户需求。虽然有许多不同类型的微服务架构，但我们建议您选择适合您的业务和技术堆栈的微服务架构。

了解线束和微服务如何让您的应用更上一层楼！

了解线束和微服务如何让您的应用更上一层楼！

Check out how Harness and microservices can take your application to the next level!