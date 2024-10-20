# 什么是 Kubernetes 容器？装具

> 原文：<https://www.harness.io/blog/what-is-kubernetes-container>

和我们一起踏上六个部分的旅程，让你做好准备，成为库伯内特斯新领地的一把尖刀。从最基本的到未来，让我们把事情做好。

开始我们的六部分系列的第一部分，我们在 Harness 将帮助你成为 Kubernetes 世界的一把锋利的刀。随着越来越多的组织经历容器革命，让我们从整体上看一下生态系统和需要克服的挑战。许多组织正在踏上容器革命的征途；一场应用和应用基础设施容器化的革命。如果你今天就开始你的旅程，当你向某人询问容器技术时，他们说的第一句话通常是 Docker 或与 Docker 相关的东西。尽管 Docker 并不是 2008 年第一个对名为 [Linux 容器](https://en.wikipedia.org/wiki/LXC)又名 LXC 的标准进行解释的人，尽管他们肯定是最著名的。

## 咚咚，是 Docker！

几年前，我第一次听说 Docker 时，我正在 Red Hat 工作。具有讽刺意味的是，当时我正穿着 Docker 的卡其裤，不明白为什么有人在谈论我在 Kohl's 买的裤子。当我的同事开始解释他不是在谈论裤子时，我的脑海里迅速回想起 JAVA 容器，如[Catalina](https://tomcat.apache.org/tomcat-9.0-doc/api/org/apache/catalina/startup/Catalina.html)(Tomcat 的 Servlet 容器)。尽管在对话和介绍 Docker 的过程中，让我印象深刻的是在各种环境中保持不变的能力和非常好的可移植性。想象一下，一个超负荷的应用程序发行版拥有您运行所需的一切。作为 [JEE](https://en.wikipedia.org/wiki/Java_Platform,_Enterprise_Edition) 的开发者，我将 [Spring Boot](https://spring.io/projects/spring-boot) 的口头禅类比为“只管跑”。Docker 的历史相对较新，由 [dotCloud 在 2013 年](https://containerjournal.com/2017/03/23/docker-4-milestones-docker-history/)开源，Docker 生态系统继续蓬勃发展，包含使容器格式健壮和可伸缩所需的工具。

## 容器，图像，天啊！

随着我们在计算世界中从大型机迁移到 x86，然后迁移到虚拟机，现在革命已经集中在强大的容器上[有些人可能会认为无服务器是下一个]。一些行话，用 Docker 术语来说, [image](https://docs.docker.com/engine/reference/commandline/image/) 是变成运行容器或者更多时候不是运行容器的东西。从咒语开始，把容器想象成应用程序监狱。应用程序只能拥有由映像清单/规范定义的资源。就像您的汽油动力汽车需要火花、空气、燃料和压缩才能运行一样，您的应用程序需要计算、内存、存储和网络才能发挥价值。容器规范有助于定义所有这些必需的组件。

## 为什么这么大张旗鼓？

容器最大的吸引力在于一致性和可移植性。在软件世界中，由于抽象，我们的目标是[写一次，在任何地方运行](https://en.wikipedia.org/wiki/Write_once,_run_anywhere)【WORA】。JAVA 是这个跨平台目标的事实上的标准。虽然编写应用程序只是应用程序运行所需的一部分。最典型的[引导问题](https://en.wikipedia.org/wiki/Bootstrapping#Software_development)又名先运行什么；在较低的级别，您的应用程序需要系统库来运行。Docker/containers 帮助打包这些系统库。我当然可以看到好处，如果您能够准确地打包您需要运行的内容、系统库等，您就可以准确地获得您需要的内容，例如正确的规模。这样，你可以开始增加密度。按照设计，容器是不可变的，这意味着它们不能改变。由于容器是一个运行中的映像，如果您需要进行更改，您需要重新创建映像并构建新的容器。如果你在做改变，你就是在做新的改变；不再需要热修补，这当然让变更控制人员很高兴，尽管有可能将 SSH 整合到一个运行容器中。尽管对于热补丁来说这不是一个好主意，但是容器的另一个特点是它们是短暂的，也就是说它们本质上是短暂的或可传递的。所以，即使你做了一个补丁，容器也是死的，所以你的改变不会只存在于那个容器中。

## 我现在需要一个容器！

由于开源的大量部件可供我们使用，Docker 图像的可用性一直在爆炸式增长，这要感谢像 [Docker Hub](https://hub.docker.com/) 这样的公共图库。你需要一个 [MongoDB](https://www.mongodb.com/) ，可以快速从 100 张甚至 1000 张包含 MongoDB 的图片中提取。不像应用程序依赖，例如，你的 JAVA 应用程序需要与你从 [Maven Central](https://search.maven.org/) 获得的 MongoDB 对话的数据库驱动程序 [JDBC，你可以从 Docker Hub 获得 MongoDB 本身的工作实例。开始就像下载一个 Docker 引擎运行时一样简单。如果你在 Mac 上，最简单的方法就是安装](https://mongodb.github.io/mongo-java-driver/) [Docker for Mac Desktop](https://docs.docker.com/docker-for-mac/install/) 。在你安装并启动服务后，你可以进入终端并输入“docker ”,奇迹就开始了。

## 让我们开始魔法吧！

使用 Docker 命令很容易。目前，两个基础是拉和运行。和那两个人在一起你很危险。准备好运行您的第一个容器了吗？用 [Docker Pull](https://docs.docker.com/engine/reference/commandline/pull/) 下拉 [nginx](https://www.nginx.com/) (web 服务器)然后就可以立即运行那个了。Docker Mac 客户端预先配置为指向 Docker Hub 作为默认注册表。通过运行“ *docker pull nginx* ”，您将在 Docker Hub 上获得 nginx 的最新发布版本。快到了！接下来，准备运行新创建的 nginx 映像。可以利用 [Docker 运行](https://docs.docker.com/engine/reference/run/)命令。尽管我们将添加一些参数，这样我们就可以用-p 标志公开和绑定默认的 nginx 端口 80，并用- name 属性命名容器。docker run-name your-first-nginx-p 80:80 nginx。一旦你运行了它，你就可以进入 nginx 启动页面，只需进入你的浏览器并输入 localhost[也就是你的 IP]。要停止正在运行的容器可以只运行[停止](https://docs.docker.com/engine/reference/commandline/stop/)命令。最简单的方法是找到容器 ID，并将 ID 传递给 stop 命令。用“ *docker ps -a* ”运行 [list container](https://docs.docker.com/engine/reference/commandline/ps/) 命令，复制“ *your-first-nginx* 的容器 ID。你现在要做的就是运行“*docker stop your _ container _ ID”。*

## 有奖游戏-制作和分享一些东西

制作你自己的 Docker 应用程序的主要机制之一是 [Docker Compose](https://docs.docker.com/compose/) 。自己做也没那么难。一个简单的 Docker 作曲，你就上路了。您需要定义一些关于如何访问容器的规范，并为您的应用程序定义资源限制。制作您自己的映像的主要原因可能是修改将在容器内运行的应用程序基础结构上的配置。这篇关于 [dev.to 的优秀且非常详细的文章介绍了如何将 nginx](https://dev.to/domysee/setting-up-a-reverse-proxy-with-nginx-and-docker-compose-29jg) 配置成一个[反向代理](https://en.wikipedia.org/wiki/Reverse_proxy)，并将所有这些配置更改打包成一个新的映像。一旦你做了某样东西，轻轻一推就可能与世界分享。Docker Push 需要一个 Docker 注册表来发布。例如，如果你在 Nexus 中没有私有的，那么公共的 Docker Hub one 就很棒。

## 为什么我的所有工作负载都不在一个容器中？

回到容器是短暂的这一点，某些工作负载不适合这种特别是有状态的工作负载。需要状态或具有特定持久性要求的项可能没有被构建到容器中。由于容器的来去速度快于集群的再平衡速度，应用集群机制可能会不堪重负。甚至我最喜欢的语言 JAVA 也在经历一场进化，变得更容易容器化。当容器化一个运行时或语言时，需要考虑很多内部因素，例如语言如何考虑资源限制。幸运的是，四大需求支柱(请记住，应用程序需要计算、内存、存储和网络才能有价值)的生态系统已经有了很多改进，并且允许更多的应用程序基础架构实现容器化。看看[云原生计算基金会的](https://landscape.cncf.io/)格局是如何逐年变化的，在这个过程中，有如此多的项目可以帮助我们。

## 如果我们有不止一个这样的容器呢？

关于应该在一个容器中运行的并发进程的数量有很多争论。因为容器很容易旋转，它们逃避自己成为一个进程的家，就像魔术一样，你正朝着你眼前的微服务前进。但是应用程序肯定不会自己运行，并且有扩展需求，因此需要一个平台来协调所有这些运行的容器。音乐响起，欢迎 Kubernetes！既然我们现在已经有足够多的关于容器的危险了，接下来的五个部分将集中在 800 磅的容器编排大猩猩 Kubernetes 上。

## 马具是你的朋友

我们很高兴能在您的集装箱/Kubernetes 之旅中与您合作。理解技术只是拼图的一部分，因为让您的应用程序重新平台化或迁移可能需要时间，尤其是在您的管道过程中需要跨越的所有障碍。有了你新的或者不是新的 Docker Hub 证书，准备好开始 Kubernetes 之旅吧！