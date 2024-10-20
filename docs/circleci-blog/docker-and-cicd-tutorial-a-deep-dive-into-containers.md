# Docker 和 CI/CD 教程:深入了解容器

> 原文：<https://circleci.com/blog/docker-and-cicd-tutorial-a-deep-dive-into-containers/>

这是我在 CircleCI 2.0 发布后写的上一篇的后续。这个版本使用 Docker 执行器实现了构建支持。发布后，我们意识到 CircleCI 用户遇到的最大障碍之一是缺乏使用 Docker 的经验。这一点通过我在社区和活动中与众多开发者的对话得到了证实，他们在[持续集成](https://circleci.com/continuous-integration/)管道中使用了容器技术。这些对话强调了许多开发人员并没有完全理解如何使用容器技术。

在这篇文章中，我将扩展并演示我在[上一篇文章](/blog/docker-what-you-should-know/)中讨论的一些更有用的 Docker 命令。

## Docker 是什么？

以下是我在上一篇文章中使用的 Docker 的简化定义:

> Docker 是开发人员和系统管理员使用容器开发、部署和运行应用程序的平台。

Docker 也被称为应用程序打包工具，它使应用程序能够被配置并打包到一个 [Docker 映像](https://docs.docker.com/get-started/#images-and-containers)中，该映像可用于生成运行应用程序实例的 [Docker 容器](https://docs.docker.com/get-started/#images-and-containers)。它提供了许多好处，包括运行时环境隔离、代码一致性和可移植性。Docker 容器可以在任何支持 [Docker 引擎](https://www.docker.com/products/docker-engine)的操作系统上运行。

## 基本码头术语

这里有一个基本 Docker 命令和术语的列表，带有更多信息的链接。这些有助于你了解 Docker，控制执行人。这些命令可以在任何安装了 Docker 引擎的计算机上本地运行。

### 构建 Docker 图像

*   [`Dockerfile`](https://docs.docker.com/engine/reference/builder/) 一个文本文档，包含用户可以在命令行上调用的所有命令来组合一个图像。

Docker 文件是构建 Docker 映像的蓝图。Docker 文件模板包含一些元素，例如用作基础的基本操作系统映像、安装/配置依赖项的执行命令以及将本地源代码或工件推入目标 Docker 映像的复制命令。下面是一个简单 Node.js 应用程序的 docker 文件示例:

```
FROM node:10

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install --only=production
# If you are building your code for production
# RUN npm install --only=production

# Bundle app source
COPY . .

EXPOSE 5000
CMD [ "npm", "start" ] 
```

该命令用于列出本地机器上当前存在的所有 Docker 映像和相关数据。它类似于 linux `ls`或 windows `dir`命令，用于显示终端中目录的内容。该命令对于理解如何在本地管理、维护和构建 Docker 映像非常有用。下面是一个`docker images`执行的结果示例:

```
REPOSITORY                          TAG                 IMAGE ID            CREATED             SIZE
ariv3ra/nodejs-circleci             latest              f419e4a6b1b8        11 days ago         943MB
node                                10                  01b816051d34        2 weeks ago         911MB
circleci/python                     3.7.6               0d2975896c73        5 weeks ago         1.43GB
ariv3ra/infrastructure-as-code101   latest              cb21a36a2973        8 weeks ago         929MB
python                              3.7.6               879165535a54        2 months ago        919MB
circleci/rust                       1-buster            218329b929cf        2 months ago        1.68GB
rust                                1-buster            f5fde092a1cd        2 months ago        1.19GB
python                              3.7.4               9fa56d0addae        6 months ago        918MB 
```

**Docker 命名约定**上面的命令，结合一个有效的`Dockerfile`，基于`Dockerfile`中定义的执行命令构建一个 Docker 镜像。构建映像和启动容器的一个关键要素是理解 [Docker 命名约定](https://docs.docker.com/engine/reference/commandline/tag/)。使用该命令构建 Docker 映像指定了目标 Docker 映像的名称。Docker images 使用由斜杠分隔的名称组成的命名约定，其中可能包含小写字母、数字和分隔符。分隔符定义为句点、一个或两个下划线、一个或多个破折号。名称组件不能以分隔符开头或结尾。一个 [docker 标签](https://docs.docker.com/engine/reference/commandline/tag/)名称必须是有效的 ASCII，并且可以包含小写和大写字母、数字、下划线、句点和破折号。标记名不能以句点或破折号开头，最多可以包含 128 个字符。您可以使用名称和标签将图像分组。在本帖中，我们将使用这个命名约定。

既然我们已经讨论了命名约定，让我们基于上面例子中的`Dockerfile`构建一个 Docker 映像。在本帖中，我们可以利用[punk data/nodejs-circle ci](https://github.com/punkdata/nodejs-circleci)git repo。在本地克隆它并将`$ cd`放入项目目录。

然后运行这个命令，根据项目源代码和示例`Dockerfile`构建一个新的 Docker 映像:

```
docker build -t tutorial/circleci-node:10 . 
```

运行此构建命令后，您将看到类似如下的结果:

```
Sending build context to Docker daemon  98.07MB
Step 1/7 : FROM node:10
 ---> 01b816051d34
Step 2/7 : WORKDIR /usr/src/app
 ---> Using cache
 ---> 12b2edc2b97c
Step 3/7 : COPY package*.json ./
 ---> Using cache
 ---> 53b5b8e4e654
Step 4/7 : RUN npm install --only=production
 ---> Using cache
 ---> eefdf560bc4d
Step 5/7 : COPY . .
 ---> aa7d54e955c6
Step 6/7 : EXPOSE 5000
 ---> Running in cc427dbafdcc
Removing intermediate container cc427dbafdcc
 ---> 4ce9084e39eb
Step 7/7 : CMD [ "npm", "start" ]
 ---> Running in f6e854599ddc
Removing intermediate container f6e854599ddc
 ---> 79a8d94cbf42
Successfully built 79a8d94cbf42
Successfully tagged tutorial/circleci-node:10 
```

运行`docker images`命令，您将会在结果中看到您新创建的 Docker 映像。

```
$ docker images

REPOSITORY                          TAG                 IMAGE ID            CREATED             SIZE
tutorial/circleci-node              10                  79a8d94cbf42        4 minutes ago       1.03GB 
```

现在，您已经构建了一个非常棒的 Docker 映像，并可以使用了。在下一节中，我将以 [`Docker containers`](https://docs.docker.com/engine/reference/commandline/container/) 的形式创建这个 Docker 图像的新实例

### Docker 容器

*   Docker 容器旨在隔离和大规模运行应用程序。它们允许简化应用程序的管理和实现。

在我们开始创建和运行 Docker 容器之前，我将添加一些上下文。Docker 容器是基于并衍生自 Docker 图像的对象。Docker 图像是模板。我把它们比作饼干模具。饼干模具使您能够快速、一致地从面团中制作出单个的饼干。我把这些饼干比作 Docker 容器，它们在形状上通过切割/建造过程中使用的饼干模具或 Docker 容器的类型来区分。因此，使用我的 cookie cutter 类比，Docker 图像是 cookie cutter，使用该 cutter 切割的单个 cookie 相当于 Docker 容器。

现在我可能已经为你做了一点匈牙利饼干，让我们开始运行一些容器。

这个命令是 Docker 运行时最重要的命令，负责创建和启动 Docker 容器。

让我们开始一个名为`nodetest01`的新容器:

```
docker run -d -p 5000:5000 --name nodetest01 tutorial/circleci-node:10 
```

恭喜你。现在，您应该让 Node.js 映像在刚刚创建的新容器中运行，该容器在端口 5000 上可用。打开网络浏览器并转到此地址:`http://localhost:5000`。它将静态显示“欢迎使用 CircleCI 学习 CI/CD 101！”此应用程序提供的网页。您还可以通过在终端中执行一个`docker ps`命令来验证您的容器正在运行。我们将在下一节讨论这一点。在此之前，让我们通过运行另一个容器来启动应用程序的另一个实例。

我们用名称`nodetest01`开始了我们的第一个容器，由于容器名称必须是惟一的，我们将把我们的新容器命名为`nodetest02`。我们的新容器中必须进行的另一个更改是端口号。我们将把它从`5000`改为`5001`，因为应用程序不能在同一个网络接口上占用同一个端口号。

在终端中执行以下`docker run`命令:

```
docker run -d -p 5001:5000 --name nodetest02 tutorial/circleci-node:10 
```

厉害！现在，您在本地机器上运行了两个容器。你可以在浏览器中访问这个地址`http://localhost:5001`,并在你的第二个 Docker 容器中查看应用程序的运行情况。

`docker run`命令非常健壮，有许多属性和配置标志，我在这篇文章中无法解决或演示。我强烈建议您阅读并熟悉可以执行和运行 Docker 容器的许多方法。你可以在这里阅读 Docker 容器命令。

*   [`docker ps`](https://docs.docker.com/engine/reference/commandline/ps/) 列出了所有正在运行的 Docker 容器。该命令的作用与`docker image`命令相似，列出了正在运行的容器。使用`-a`标志显示所有正在运行和没有运行的容器。

在终端中运行这个命令，您应该能够在结果中看到前面创建的两个容器正在运行:

```
docker ps 
```

结果将与此类似，显示`node01`和`node02`容器仍在运行。

```
CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS              PORTS                    NAMES
dfd30181e15c        tutorial/circleci-node:10   "docker-entrypoint.s…"   12 minutes ago      Up 12 minutes       0.0.0.0:5001->5000/tcp   nodetest02
0efaf7f11780        tutorial/circleci-node:10   "docker-entrypoint.s…"   28 minutes ago      Up 28 minutes       0.0.0.0:5000->5000/tcp   nodetest01 
```

该命令仅用于启动通过`docker run`命令创建的**现有**容器。除非用`docker run`命令指定了`--rm=true`标志，否则新创建的容器将持续存在，并可以用`docker start`和`docker stop`命令重用。

让我们通过执行以下命令来停止一些正在运行的容器:

```
docker stop nodetest01 nodetest02 
```

这些正在运行的容器现在应该处于非活动状态并已停止。由于它们已经停止，运行`docker ps -a`命令，您将会看到类似下面的结果，指示停止的容器。

```
CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS                      PORTS               NAMES
dfd30181e15c        tutorial/circleci-node:10   "docker-entrypoint.s…"   31 minutes ago      Exited (0) 38 seconds ago                       nodetest02
0efaf7f11780        tutorial/circleci-node:10   "docker-entrypoint.s…"   About an hour ago   Exited (0) 38 seconds ago                       nodetest01 
```

尽管这些容器被停止，但它们将持续存在，并且可以使用`docker start`命令重新启动。

让我们启动`nodetest01`容器，这样我们可以在下一节学习如何使用`docker logs`特性。

```
docker start nodetest01 
```

现在运行`docker ps -a`命令。注意这个容器的状态类似于`Up about a 30 seconds`或者类似的意思。

该命令使开发人员能够查看执行时出现的日志。因为您的应用程序是在这些容器中运行的，所以 logs 命令对于读取关键的应用程序输出非常有用。这有助于了解应用程序的运行情况以及可能需要的任何调试/故障排除。

让我们对`nodetest01`容器运行`docker logs`命令:

```
docker logs nodetest01 
```

运行该命令后，您应该会看到如下所示的结果。这表明容器内的应用程序已经启动并运行。

```
Node server is running..

> nodejs-circleci@0.0.1 start /usr/src/app
> node app.js

Node server is running.. 
```

持久化 Docker 容器会消耗其主机上的磁盘空间和资源。从操作上来说，将每个容器都保存在磁盘上没有多大意义，因为在大多数情况下，容器都是一次性的，应该这样使用。因此，当不再需要容器时，应该将其从宿主中永久丢弃。`docker rm`命令从主机上永久删除容器。不再需要`nodetest02`容器，所以让我们节省一些磁盘空间并删除它。注意此命令将只删除非活动/停止的容器，如果对活动/运行的容器运行，将会出错。

对`nodetest02`容器运行`docker rm`命令:

```
docker rm nodetest02 
```

运行`docker ps -a`之后，您应该在主机上不再看到`nodetest02`容器。这个命令有其他的属性和标志，你应该[在这里熟悉它们](https://docs.docker.com/engine/reference/commandline/rm/)。

此命令适用于从主机中删除 Docker 映像。如果主机上存在基于特定映像的现有容器，则此命令不允许删除基于映像的容器。在使用此`docker rmi`命令删除 Docker 图像之前，您必须首先确保停止并删除容器。[在这里阅读 docker rmi 命令](https://docs.docker.com/engine/reference/commandline/rmi/)

### Docker 映像部署

许多 Docker 图像是公开可用的，并托管在 [Docker Hub Registry](https://docs.docker.com/docker-hub/) 上，这是 Docker 图像的在线中央托管解决方案。Docker Hub 使任何有互联网连接的人都可以将公开的图片从注册表下载到他们的本地机器或服务器上。它还允许注册用户上传并公开分享他们的任何容器，这样任何人都可以从 [Docker Hub](https://hub.docker.com/) 下载图片。

在接下来的部分中，我将简要讨论一些与 [Docker Hub](https://docs.docker.com/docker-hub/) 相关的命令。

*   [`docker pull`](https://docs.docker.com/engine/reference/commandline/pull/) 从注册表中取出一个映像或存储库。

此命令使您能够从 Docker Hub 下载有效的 Docker 映像。公开可用的 Docker 图像不需要认证。如果注册中心是私有的，你需要使用一个指定的证书进行认证，通常是用户名和密码。

该命令使您能够根据 Docker 注册表进行身份验证。如果你使用这个登录命令，你应该[用你的 Docker Hub 账户用户名和密码填充一个. txt 文件](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin),以保护这些凭证不被暴露。

*   [`docker push`](https://docs.docker.com/engine/reference/commandline/push/) 将图像或存储库推送到注册表。

此命令使用户能够将图像上传到 Docker 注册表，并且需要有效的凭据。

## 摘要

所以你有它。在这篇文章中，我深入解释了 Docker 术语和命令以及容器和它们的用法。我讨论和演示的命令是 Docker 运行时中最广泛使用的命令，了解更多关于它们及其属性和标志的知识肯定会帮助您提高 Docker 和容器技能。

熟悉这些基本命令将会消除 CircleCI 新用户遇到的一个更大的障碍。

如果你想了解更多关于 CircleCI 的信息，请查看[文档网站](https://circleci.com/docs/)，如果你真的遇到困难，你可以通过 https://discuss.circleci.com/[社区/论坛网站联系 CircleCI 社区。](https://discuss.circleci.com/)