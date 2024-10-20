# 简化 Docker 多阶段构建|装具

> 原文：<https://www.harness.io/blog/docker-multi-stage-build>

尽管采用了云架构，但许多公司并没有取得最佳效果。了解如何在云中使用 Drone 简化您的多步 Dockerfile 部署。

在过去几年中，许多组织已经开始采用云原生架构。尽管采用了这些架构，许多公司并没有取得最佳结果。但这是为什么呢？一个原因是我们坚持构建和部署应用程序的传统方法。由于我们生活在云原生时代，我们构建的每个应用程序都应该可以通过容器化部署到任何云上。

为了演示我在本教程中的意思，我将使用 next.js [和-docker-example](https://github.com/vercel/next.js/tree/canary/examples/with-docker) 。你可以从我的[博客演示源](https://github.com/kameshsampath/nextjs-with-drone)中克隆这个演示源。

````shell
git clone https://github.com/kameshsampath/nextjs-with-drone
export PROJECT_HOME=$(pwd)/nextjs-with-drone
cd $PROJECT_HOME
`````

这里的目标是将这个应用程序部署到云平台上。以下是实现这一目标的步骤:

1.  使用 Dockerfile 文件将应用程序容器化
2.  将它们推送到任意一个集装箱注册中心，如 [Github](https://github.com/features/packages) 、 [Quay.io](https://quay.io) 等。
3.  将容器化的应用部署到云平台，如 [Kubernetes](https://kubernetes.io/) 、 [AWS Fargate](https://docs.aws.amazon.com/AmazonECS/latest/userguide/what-is-fargate.html) 、 [Google Cloud Run](https://cloud.google.com/run) 等。

## 入门指南

让我们从 Dockerfile 开始分析我们的代码:

###### 
` ` docker file
FROM node:16-alpine AS deps
运行 apk add-no-cache libc 6-compat
WORKDIR/app
COPY package . JSON yarn . lock。/
运行 yarn install-freezed-lock file

FROM node:16-alpine AS builder
work dir/app
COPY-FROM = deps/app/node _ modules。/node_modules
复制。。

RUN yarn build

FROM NODE:16-alpine AS runner
work dir/app

ENV NODE _ ENV production

RUN add group-system-GID 1001 nodejs
RUN adduser-system-uid 1001 nextjs

COPY-FROM = builder/app/public。/public
COPY-from = builder/app/package . JSON。/package . JSON

COPY-from = builder-chown = nextjs:nodejs/app/。下一个/独立。/
COPY-from = builder-chown = nextjs:nodejs/app/。下一个/静态
。/.下一个/静态
‍
用户 next js

expose 3000
env port 3000

cmd[" node "，" server . js "]
` `

这是一个多步骤的构建，对于不熟悉这些过程的人来说可能很困难。此外，如果其中一个步骤失败了，那么我们就很难知道它失败的原因。但是让我们停下来想一想:难道我们不觉得所有这些命令都应该按顺序流动，每个步骤都按定义的顺序运行，一个接一个吗？如果你也得出这个结论，那么你和我一起迈出了持续集成(CI)的第一步。

## 为什么构建不同于 CI

我们经常混淆构建和 CI。火上浇油的是，市场上的工具也可能相当混乱，为构建工具提供插件和扩展，例如 yarn 或 Apache Maven，以将构建与 CI 混合在一起。我是单一责任原则的忠实拥护者。当我们将其应用于应用构建和部署流程时，云原生应用中的构建应该将应用构建到容器映像，然后移交其他步骤(如推送至注册表、部署到云原生平台等。)到 CI 工具。

考虑到这一点，当我们考虑 CI 时，我已经向您介绍了两个重要的术语:

*   **步骤**——只做一件事的东西，比如构建应用，推送注册等。
*   **管道**——多步并作拥抱 CI

在我进一步探讨如何从多步 dockerfile 构建转移到 CI 管道之前，让我们先来看看一个开源工具， [Drone](https://www.drone.io/) ，一个云原生的自助式 CI 平台。虽然它已经有 10 年的历史，但 Drone 仍然提供了一个成熟的 CI 系统，利用了云原生架构的可伸缩性和容错特性。Drone 以其简单、解耦和声明性的特性而闻名，使我们能够定义一些易于理解的健壮管道。

## 将 dockerfile 添加到管道

让我们开始将我们的多步 docker 文件移动到一个无人管道。这就像 Dockerfile Drone 如何使用一个名为 *.drone.yml* 的 YAML 文件，它通常位于项目源代码的根目录中。你可以在[文档](https://docs.drone.io/)页面上阅读更多关于无人机的信息。在接下来的部分，我们将开始组装 *.drone.yml* 。

对于编写. drone.yml 的第一步，我们需要以下信息:

*   **应用程序的名称是什么？** nextjs-with-drone
*   **我们将使用什么类型的建筑？**码头工人
*   **我们会使用什么样的无人机资源？**我们将使用[管道](https://docs.drone.io/pipeline/overview/)，因为我们有多个步骤。

*   我们将在什么样的平台上构建？对于大多数容器情况，平台将是 Linux，操作系统架构将是 amd64 或 arm64。

有了这些信息，我们的. drone.yml 将如下所示:

````yaml ---kind: pipeline type: docker name: nextjs-with-drone platform: os: linux   arch: arm64 ````

所以现在我们已经填满了管道的前几条线。下一个任务是从 docker 文件中确定我们的管道步骤，我们可以推断出我们有以下三个步骤:

1.  **deps**–构建节点依赖关系
2.  **构建器**–构建 node.js 应用程序
3.  **runner**–应用程序的包，作为 linux 容器映像

‍

现在让我们开始添加这些步骤作为无人机管道[步骤](https://docs.drone.io/pipeline/exec/syntax/steps/)。无人机流水线步骤至少需要以下细节:

*   该步骤的名称是什么？
*   这个步骤将运行什么容器映像？
*   哪些命令将在容器内执行它？

我们从 docker 文件中获得了所有这些必需的信息，所以让我们开始将它们作为无人机步骤添加到. drone.yml 中:

````yaml
---
kind: pipeline
type: docker
name: nextjs-with-drone

platform:  
os: linux  
arch: arm64

steps:  
- name: yarn-install    
image: docker.io/node:lts-alpine    
commands:  
- apk add --no-cache libc6-compat  
- cd /app-build  
- cp /drone/src/package.json ./  
- cp /drone/src/yarn.lock ./  
- yarn install --frozen-lockfile  
- cp -r /drone/src/* .  
- yarn build    
volumes:  
- name: app-build-dir    
path: /app-build  
- name: build-image   
  image: gcr.io/kaniko-project/executor:debug    
commands:  
- >    
/kaniko/executor    
--context /app-build    
--dockerfile Dockerfile    
--destination ttl.sh/nextjswithdrone/my-app:1h     
volumes:  
- name: app-build-dir    
path: /app-build
volumes:  
- name: app-build-dir     
temp: {}
````

您可能已经注意到，该步骤的 commands 属性只是 RUN，复制 Dockerfile 中的指令。这些被翻译成等价的 Linux 命令。我们添加了一个名为 volumes 的额外属性，它主要用于将任何目录或文件挂载到容器中。在这种情况下，我们使用它来与另一个步骤共享构建工件。

## 最后一步:清理

最后一步是清理 docker 文件，这样我们只有一个步骤来构建最终的容器映像。清理后的 docker 文件如下所示:

````dockerfile
FROM node:lts-alpine
LABEL org.opencontainers.image.source
https://github.com/kameshsampath/fruits-app-ui

WORKDIR /app

ENV NODE_ENV production

RUN adduser --system --uid 1001 nextjs

RUN cp -r /app-build/.next/standalone/. ./ \   
&& cp -r /app-build/public ./ \   
&& cp /app-build/package.json ./package.json \   
&& cp -r /app-build/.next/static ./.next/

RUN chown -R 1001:0 /app \  
&& chmod -R g+=wrx /app

RUN ls -ltra /app

USER nextjs

EXPOSE 3000

ENV PORT 3000

CMD ["yarn", "start"]
````

现在，您已经准备好对您的应用程序进行第一次 CI 构建了！下载 [Drone CLI](https://docs.drone.io/cli/) 并将其添加到您的$PATH 中。如果运行以下命令一切顺利，那么您应该会看到类似于 drone 版本 1.5.0 的输出:

````shell
drone --version
````

让我们运行管道:

````shell
drone exec
````

如果一切顺利，那么您的容器映像将被推送到 [ttl.sh](https://ttl.sh) 存储库。让我们在本地运行容器，看看我们的构建是否有效:

````shell
docker run -it --rm -p 3000:3000 ttl.sh/nextjswithdrone/my-app:1h
````

当您在浏览器中打开 [http://localhost:3000](http://localhost:3000) 时，您应该会看到欢迎屏幕:

‍

瞧啊。您已经向使用[软件交付最佳实践](https://harness.io/blog/continuous-delivery/best-practices-software-delivery/)实践 CI 迈出了第一步。现在，您可以对管道进行改进或添加额外的步骤，使其部署到容器云平台。

准备好开始使用无人机桌面了吗？[立即下载免费试用版](https://docs.drone.io/)。