# dock file-dock build-dock build name | circles-dock 样式-dock build-dock build name | circles

> 原文：<https://circleci.com/blog/multi-stage-docker-builds/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

Docker 在 2017 年 5 月推出了多阶段构建。用最简单的话来说，这些是包含不止一个`FROM`语句的 docker 文件。稍加调整，您就可以在 CircleCI 2.0 上构建多级 Dockerfiles。

*查看[为您的 CI/CD 管道使用 Docker 的指南](https://circleci.com/blog/guide-to-using-docker-for-your-ci-cd-pipelines/)*

## 什么是多阶段 Docker 构建？

Docker 新的多阶段构建允许 docker 文件变得更加强大，用一个 docker 文件提供复杂的构建。一个用例可能是，您通常有一个 docker 文件用于构建应用程序的源代码，然后有另一个文件用于运行和测试应用程序。这里有一个例子`Dockerfile`用多阶段构建来解决这个问题，这个例子是从[docs.docker.com](https://docs.docker.com)那里借来的:

```
FROM golang:1.8.3
WORKDIR /go/src/github.com/alexellis/href-counter/
RUN go get -d -v golang.org/x/net/html  
COPY app.go .
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o app .

FROM alpine:latest  
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=0 /go/src/github.com/alexellis/href-counter/app .
CMD ["./app"] 
```

这个 Docker 文件的第一个“阶段”是使用 Docker 库中的官方 Go 映像构建 Go 应用程序。然后，第二阶段从第一阶段(用`COPY --from=0`)复制新构建的二进制文件，并将其放在一个定制的 Alpine 映像中。结果是一个基于 Alpine 的 Docker 映像，任何人都可以使用它来运行这个 Go 应用程序，而不需要任何最初用于构建该应用程序的额外软件，例如 Go 工具链。

### 命名阶段

为了将我们构建的二进制文件复制到第二个阶段，我们使用了`COPY --from=0`,因为我们没有命名阶段，所以我们使用了它的索引号。相反，我们可以在`FROM`语句中命名一个阶段，然后在以后使用这个名称。所以我们可以从`FROM golang:1.8.3 as compile-stage`开始，在`COPY`语句中我们将使用`COPY --from=compile-stage`。

## 基于 CircleCI 2.0 构建

上面的 Dockerfile 示例可以用 CircleCI 2.0 构建，但是需要根据您使用的 [`executor`](https://circleci.com/docs/glossary/#executor) 进行配置更改。这是因为 CircleCI 上默认使用的 Docker 版本是 Docker 17 . 03 . 0 Community Edition(CE)。Docker v17.05.0-ce 及更新版本支持多阶段构建。`machine`和`docker`执行者都允许我们请求更新的 Docker 版本。

### `docker` Executor

我们可以在`setup_remote_docker`下设置 Docker 版本用于远程引擎:

```
 - setup_remote_docker:
	      version: 17.05.0-ce 
```

记住，当使用`docker`执行器和`setup_remote_docker`时，这只是为您提供了一个远程 Docker 引擎来连接。您的构建所使用的基本 Docker 映像仍然需要安装 Docker 客户端。更多关于这个以及 Docker 支持哪个版本的信息可以在[这里](https://circleci.com/docs/building-docker-images/)找到。

### `machine` Executor

`machine`执行器现在支持指定一个映像，类似于`docker`执行器的工作方式，但是使用我们的基本 VM 映像。默认图像不足以满足我们的用例，但“边缘”图像是:

```
jobs:
  build:
    machine:
      image: circleci/classic:edge 
```

这为我们提供了 Docker v17.06.0-ce 的机器 VM。更多关于`machine`的执行者[这里](https://circleci.com/docs/configuration-reference/#machine)。

使用 Docker v17.05.0-ce 和更新版本，您现在可以在 CircleCI 2.0 上构建多级 Docker 文件。