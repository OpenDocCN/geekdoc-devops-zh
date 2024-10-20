# 将您的节点应用程序|线束归档

> 原文：<https://www.harness.io/blog/dockerizing-your-node-application>

在这个简短但温馨的一步一步的教程中，通过代码块了解如何编写节点应用程序。今天就试试吧！

Docker 彻底改变了我们构建、打包和发布软件的方式。Docker 使得开发者可以打包他们的应用程序并与他人分享。正是因为有了 Docker，我们现在才看到云计算的如此多的进步。很多新兴的创业公司都以 Docker 技术为基础。此外，Docker 通过弥合开发和运营团队之间的差距，增强了 DevOps 方法。今天，我们将通过一个简单的教程来演示如何构建一个简单的“Hello World”应用程序，我们将对它进行分类。

## 先决条件:

*   从官方[网站](https://docs.docker.com/get-docker/)下载安装 Docker。
*   从官网[这里](https://nodejs.org/en/)安装 Node.js。
*   从官网[这里](https://docs.npmjs.com/downloading-and-installing-node-js-and-npm)安装 npm。

## 辅导的

### 创建应用程序

创建一个目录来存储您的应用程序和依赖项。你可以选择任何你想要的名字。我选择了“expapp”这个名字。

mkdir expapp

使用 npm init -y 命令初始化应用程序。这将创建包含您的依赖项的 package.json 文件，以及应用程序的名称和描述。

{
" name ":" expap "，
"version": "1.0.0 "，
" description ":"
" main ":" index . js "，
" scripts ":{
" test ":" mocha "
}，
"keywords": []，
"author ":，
"license": "ISC "，
}

我们将通过在项目的根目录上运行以下命令来添加 Express framework 作为一个依赖项。

npm 快速安装-保存

一旦添加了这个依赖项，就可以返回并检查 package.json 文件。它现在应该列出了 express 依赖项。

{
" name ":" expap "、
"version": "1.0.0 "、
" description ":" index . js "、
" scripts ":{
" test ":" mocha "
}、
"keywords": []、
"author ":"、
"license": "ISC "、
" dependencies ":{
" express "::"

一旦将 express 添加为依赖项，您应该会看到在主目录中又创建了两个文件:package-lock.json 和 node_modules。我们不会深入细节，因为这不是本教程的目标。如果你真的想了解更多，可以去翻翻这本[入门指南](https://nodejs.org/en/docs/guides/getting-started-guide/)。

现在，是时候将我们的主要代码添加到 app.js 文件中了。创建名为 app.js 的文件，并添加以下代码:

const express = require(' express ')；

const app = express()；
const PORT = process . env . PORT | | 3002；

app.get('/'，(请求，响应)= > {
response.status(200)。json({
消息:“你好，Docker！”，
})；
})；

app.listen(PORT，()=>{
console . log(` server on localhost:$ { PORT } `))；
})；

在上面的文件中，我们正在配置应用程序——基本上使 express 成为必需的依赖项，并使应用程序启动一个服务器并在端口 3002 上监听连接。app 回复{“消息”:“Docker 你好！”}用于对根 URL (/)或路由的请求。对于其他每条路径，它都将响应 404 Not Found。

我们的简单应用程序已经准备好了。现在，我们可以通过运行它来测试它是否正常工作。运行命令 node app.js，当点击 localhost([http://localhost:3002/](http://localhost:3002/))时，应该会看到下面的输出。

### 创建 Dockerfile 文件

让我们创建一个 docker 文件来制作应用程序的图像。转到应用程序的根目录，创建一个名为 Dockerfile 的文件，并向其中添加以下代码。

FROM NODE:14-alpine AS development
ENV NODE _ ENV development
#添加一个工作目录
WORKDIR /app
#缓存并安装依赖关系
复制 package.json。
运行 npm 安装
#复制 app 文件
复制。。
# Expose port
Expose 3002
#启动 app
CMD [ "node "，" app.js" ]

在 docker 文件中，我们使用 Node:14-alpine 作为我们图像的模板。

将容器中的工作目录设置为/app。我们将使用这个目录来存储文件，运行 npm，并启动我们的应用程序，暴露端口 3002 供我们的应用程序运行。

最后，最后一行指定了我们的应用程序将如何启动。

**可选:**

创建一个. dockerignore 文件，以防止本地模块和日志被复制到 Docker 映像上。在您的。dockerignore 文件。

### 构建应用程序的映像

将文件保存到根目录后，让我们用刚刚编写的 docker 文件构建一个映像。这可以通过下面的 Docker build 命令来完成。

docker build -t express-app .

-t 参数设置 Docker 图像的名称。

我将我的图像命名为 exp-app。你想给你的起什么名字都可以。:)

命令成功运行后，您应该使用命令 docker images 验证构建的映像

### 启动集装箱

形象建好了！是时候使用以下命令启动带有指定端口的 Docker 容器了:

码头运行-p 3002:3002 快递-应用程序

### 推送至码头中心

现在，让我们将此图像推送到 Docker Hub。确保您从终端登录到 Docker Hub。您可以通过使用 docker login 命令来实现这一点。

您需要使用 Docker Hub 凭据再次构建映像。

docker build-t[用户名]/express-app。

在上面指定的字段中添加您的用户名。这是您的 Docker Hub 用户名。

一旦你用你的 Docker Hub 凭证建立了它，使用 Docker push[USERNAME]/express-app 命令把它推送到你的 Docker Hub。

您应该会看到一个新图像被推送到您的 Docker Hub。您可以访问您的 Docker Hub 仪表板来验证这一点:

太好了！现在，你可以在任何地方使用这张图片。与您的开发伙伴共享它，减少在他们的机器上重新构建它的时间和精力。

通过运行以下命令，验证我们是否已将正确的映像推送到 Docker Hub:

docker run[用户名]/express-app

您应该看到应用程序在指定的端口上运行，输出如下:

请访问您的 [http://localhost:3002/](http://localhost:3002/) 进行检查。

## 结论:你离成为职业码头工人又近了一步

恭喜你！您创建了一个应用程序，构建了应用程序的映像，并将其推送到 Docker Hub 容器注册中心。

不喜欢从博客文章中复制和粘贴代码块？你可以在这个应用程序的 [GitHub 库](https://github.com/pavanbelagatti/Simple-Node-App)中看到所有的代码。

请加入我们的下一篇博客教程，我们将讨论如何将 Node.js 应用程序部署到 Kubernetes。一旦链接可用，我们将在此分享。