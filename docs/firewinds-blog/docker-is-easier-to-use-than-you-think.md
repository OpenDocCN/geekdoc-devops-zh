# Docker 比你想象的更容易使用

> 原文：<https://www.fairwinds.com/blog/docker-is-easier-to-use-than-you-think>

 在我的[上一篇博文](http://blog.reactiveops.com/docker-is-a-valuable-devops-tool-one-thats-worth-using)中，我关注了为什么 [Docker](https://www.docker.com/) 对使用有益，以及为什么它在广泛的用例中提供了重要的价值。在本系列的第二部分中，我将重点讨论为什么使用它没有您想象的那么困难。

下面，我将分享一些基本的命令和例子来告诉你 Docker 是多么容易使用。首先，您需要在您的系统上运行 Docker。转到此处，按照与您的系统相匹配的说明进行操作。准备好了，我们就从最常见的 Docker 函数开始，  `docker run`。

## 如何运行 Docker

Docker 在隔离的容器中运行进程(在本地或远程主机上运行的进程)。当您执行 docker run 命令时，运行的容器进程是独立的——它有自己的文件系统、自己的网络和自己独立于主机的进程树。

基本的  `docker run` 命令采用这种形式:

`$ docker run [OPTIONS] IMAGE[:TAG|@DIGEST] [COMMAND] [ARG...]`

下面是一个运行 redis 映像的非常简单的示例:

`docker run redis`

如果您的主机上还没有 redis 映像，它会为您提取映像。看着每个文件系统层被并行下载，然后一个运行 redis 的容器启动。Ctrl-C 并再次运行该命令，您将会看到由于已经下载了映像，后续运行的速度有多快。

下面是另一个更复杂的例子，展示了如何运行一个 [Python 3.0](https://www.python.org/download/releases/3.0/) (python3)图像:

`docker run -p 8080:8080 python:3 python3 -m http.server 8080`

我们来分解一下上面的命令:

- `docker run: run a docker container`

- `p 8080:8080`:将容器内部的端口 8080 映射到容器外部的端口 8080。

- `python:3`:使用标签为 3 的 python 图像。本例中的  `image:tag` 组合表明该图像正在运行 python3。

- `python3 -m http.server 8080`:在容器内部运行这个命令。

该命令运行 python3 二进制文件，加载  `http.server` 模块，并创建一个在端口 8080 上可用的 web 服务器。上面的  `-p` 选项映射了端口，应该允许您访问  `http://localhost:8080` 并查看运行容器内的目录列表。

如果您还没有在您的开发环境中运行 Python3，那么要执行多少步才能得到这个命令的等价物？

## 其他常见的 Docker 命令

还有更多的选项和命令，但这些将使您快速开始使用 Docker:

- `docker images`:展示你的主持人是什么形象

- `docker ps`:显示哪些容器

- `docker ps -a`:显示所有正在运行或已经运行但尚未移除的容器

- `docker stop <container id>`:停止正在运行的容器

- `docker rm <container id>`:移除容器

- `docker rmi <image>`:删除图像

## 有用的 Docker 运行标志

要在分离模式下启动容器，请使用  `-d` 标志。按照设计，当用于运行容器的根进程退出时，以分离模式启动的容器也会退出。

`docker run -d -p 6379:6379 redis`

将启动一个 redis 容器，在后台运行它并使其在  `localhost:6379`可用。

使用  `docker ps` 查找容器 ID，  `docker stop` 停止它，使用  `docker rm` 删除它。

要发布或公开端口，请使用  `-p` 标志。

- `docker run python:3 python3 -m http.server 8080`

- `http://localhost:8080/`

- `http://localhost:8080/`

在上面的第一个示例中，您没有打开容器中的端口(您无法访问正在运行的 Python 服务器)，而第二个示例演示了网络地址转换(您可以访问服务器)。

要绑定挂载卷，请使用  `-v` 标志。

下面的示例展示了这三个标志的用法:

`docker run -d -v /docker/redis/data/:/data/ -p 3306:3306 redis`

在这种情况下，容器内的路径'/data '被映射到主机上的  `/docker/redis/data` 。这是在 docker 运行之间保存状态的一种方式。

## 如何从 Docker 获取日志

`docker logs` 命令获取容器的日志。

该命令采用以下形式:

`docker logs [OPTIONS] CONTAINER`

## 如何运行 Docker 交互式 Shell

标签  `–it` 使您能够在运行的容器上交互并打开外壳。它创建了一个交互式的附加终端。

下面的例子展示了如何在 Ubuntu 映像中运行交互式 shell:

`docker run -it ubuntu bash`

## 图像、容器和存储库

Docker 映像是文件系统层的集合，相当于一个固定的起点。运行映像时，它会创建一个容器。从初始映像所做的一组更改稍后会成为一个附加的文件系统，并且可以提交容器，此时它会成为一个新的映像。存储库是存储图像的地方。

`docker pull` 和  `docker push` 命令将使您能够与您的存储库进行交互，以执行对接拉或对接推。使用  `docker pull` 将容器映像从注册表下载到本地缓存中，这样您就可以基于映像启动容器。使用  `docker push` 将您的图像共享到 [Docker Hub](https://hub.docker.com/) 注册表或自托管注册表。

## 如何使用 Dockerfile 构建图像

一个图像通常用一个[*docker file*](https://docs.docker.com/engine/reference/builder/)来定义，它包含了你可以在命令行上调用的所有命令来组装一个图像。每个图像都从一个基础图像开始，比如 ubuntu(一个基础 [Ubuntu](https://www.ubuntu.com/) 图像)。

`docker build . -t <whatever you want to name the image>`

Docker 通过读取 docker 文件中的指令来构建映像。这一部分很重要:docker 文件中的每个 docker 命令代表一个文件系统层。每次构建映像时，它将使用以前构建的缓存层。docker 文件中的任何一行发生变化都会使其下所有命令(和层)的缓存失效。

那是什么意思？

让我们假设你有一个简单的项目。你要把一个  `simple_script.sh` 放入 Docker 镜像，而  `simple_script.sh` 依赖于 telnet。您需要将脚本复制到映像中并安装 telnet。

如果我们的 docker 文件是:

- `FROM ubuntu`

- `COPY simple_script.sh /`

- `RUN apt-get update`

- `RUN apt-get install -y vim`

- `CMD “bash simple_script.sh”`

然后每次更新  `simple_script.sh`，构建过程都会再次运行  `apt-get update` 和  `apt-get install` 。这将很快过时，因此您可以将 docker 文件重组为:

- `FROM ubuntu`

- `RUN apt-get update && apt-get install -y vim`

- `COPY simple_script.sh /`

- `CMD “bash simple_script.sh”`

你已经完成了两件事。首先，通过使用  `&&` 操作符并结合  `apt-get update` 和  `apt-get install` 命令，文件系统的层数减少了一层。第二，当你现在更改  `simple_script.sh` 并运行  `docker build` 时，apt 命令会被缓存，后续构建只需要添加  `simple_script.sh`。这加速了发展，让每个人都更开心。

## 开始使用 Docker

你可能仍然对使用 Docker 犹豫不决。可以理解。要克服任何感觉到的障碍，从小事做起，从简单做起。从简单的流程开始，在有限的范围内使用这些流程——记住，您希望每个容器运行一个流程。

Docker 是一个非常棒的工具。我希望您能尝试一下上面的命令，并决定利用 Docker 提供的所有好处。