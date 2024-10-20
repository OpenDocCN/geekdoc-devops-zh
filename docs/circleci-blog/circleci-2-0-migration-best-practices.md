# 从 CircleCI 1.0 迁移到 2.0:成功秘诀

> 原文：<https://circleci.com/blog/circleci-2-0-migration-best-practices/>

作为一名高级成功工程师，在过去的几个月里，我一直在帮助我们的许多客户将他们的项目从 CircleCI 1.0 迁移到 2.0。在迁移了数百个项目之后，我知道团队最容易遇到问题的地方。在这篇博文中，我收集了我在将客户的项目迁移到 CircleCI 2.0 时最常与客户分享的技巧，以及一些在迁移过程中需要注意的问题。迁徙快乐！

### 增量测试 CircleCI 2.0

你知道你可以在 CircleCI 1.0 和 2.0 上构建同一个项目吗？当开始迁移到 CircleCI 2.0 时，您不必马上全部投入。要让您的项目构建在 1.0 之上并测试 2.0，请尝试以下方法:

*   为测试 CircleCI 2.0 创建一个新分支。
*   从该分支中删除 circle.yml 并添加一个. circleci/config.yml 文件。
*   在那个分支上写一些最小的 2.0 配置，然后推它直到你得到一个绿色的构建。
*   我们建议一次做一点点配置，这样你就可以感受一下它是如何工作的。最初，只需检查代码，然后尝试安装依赖项，然后尝试运行测试。稍后，您可以开始研究如何缓存依赖项并使用更高级的功能，如工作流。一点一点地建立你的配置。
*   当一切工作正常时，您可以将带有新配置的分支合并到您的主项目中。

### CircleCI 2.0 快速提示

*   `steps`中列出的命令只能在`docker`部分列出的第一个容器中运行。
*   经常运行构建来测试到目前为止的配置。如果出现问题，您会知道自上次构建以来发生了什么变化。
*   最初不要添加工作流；等到你有了一个功能性的构建。
*   从头手动构建配置，但是使用配置转换端点作为参考。
*   您不能在配置的`environment`部分用环境变量定义环境变量。
    *   解决方法是将变量回显到 BASH _ ENV 中
        *   这只适用于 bash，不适用于 sh (Alpine 图像只有 sh)
*   用 bash if 语句有条件地运行命令。
    *   if[$ CIRCLE _ BRANCH = " master "]；然后。/ci . sh；船方不负担装货费用
*   使用`circleci step halt`有条件地在该步骤停止构建
    *   允许您通过暂停有条件地使用`setup_remote_docker`
*   时区可以通过定义一个 env 变量来改变。
    *   TZ:"/usr/share/zoneinfo/America/New _ York "
*   运行/dev/shm(即，/dev/shm/project)可以加速某些项目。
    *   像 Ruby gems 这样的东西是不能从共享内存中加载出来的。它们可以安装在系统的其他地方(~/vendor)并在。
*   不要在很多命令前面加上 sudo，而是考虑将 shell 设置为 sudo。
    *   shell: sudo bash -eo pipefail
*   Docker 构建和 docker-compose 一般应该在`machine`中运行，除非特定于语言的工具(Ruby、Node、PHP 等。)是预先需要的，那么远程环境就足够了。
*   有些任务可以设置为在后台运行，以节省总体构建时间，但要小心耗尽资源。
*   不同的 resource_class 大小可能是有益的，值得尝试一下，看看它们的影响。可能一点影响都没有。
*   可以将$PATH 设置为字符串。如果您不知道 Docker 图像的$PATH，只需运行它并回显$PATH，或者看看`env`的输出。
*   图像的 sha 可以在`Spin up Environment`步骤下引用。可以将图像硬编码为 sha 值，使其不可变。
*   服务容器可以使用自定义标志运行。
    *   命令:[mysqld，–character-SET-server = utf8mb 4，–collation-server = utf8mb 4 _ unicode _ ci，–init-connect = ' SET NAMES utf8mb 4；']
*   当在 CI 中运行时，配置您的测试运行程序，只产生两个线程/工作线程(或者更多，如果您使用 resource_class 的话)。有时候他们会基于不正确的价值观来优化自己。查看[此视频](https://www.youtube.com/watch?v=CKDVkqIMpHM)了解更多信息。

### 迁移提示:1.0–2.0

*   注意$CIRCLE_ARTIFACTS 和$CIRCLE_TEST_REPORTS 在 2.0 中没有定义。
    *   你可以自己定义它们，但是如果你这样做的话，一定要确定。
*   在一个存储库中迁移 Linux 和 macOS(像 React Native)应该包括在将两个配置合并到一个工作流之前为每个 Linux 和 macOS 打开一个分支。
*   你不能`sudo echo` -像这样管道:`echo "192.168.44.44 git.example.com" | sudo tee -a /etc/hosts`
*   Ubuntu 和 Debian 系统的字体是不同的。
*   Apache 2.2 和 2.4 的配置有很大的不同——请确保升级您的 2.2 配置。
*   不要忘记所有 1.0 自动推断的命令和 UI 中手动存储的命令。

### 特定于语言的提示

*Python*

*   通常期望类名而不是文件名来运行测试

*红宝石*

*   在 AUFS 上，Ruby 文件的加载顺序可能与预期的不同
*   将$RAILS_ENV 和$RACK_ENV 定义为`test`(这在 1.0 中是自动的)

*基于 Java 的*

*   Java(应用、工具和服务)会 OOM(内存不足)，因为它不能识别有多少内存可用。应该定义一个环境变量。如果它还在飞，那就需要一个更大的容器。
    *   https://circleci.com/blog/how-to-handle-java-oom-errors/
*   Scala 项目的文件名可能太长，包括`-Xmax-classfile-name`标志。
    *   https://discuse . circle ci . com/t/Scala-SBT-assembly-does-not-work/10499/10 Scala options++ = Seq("-encoding "、" utf-8 "、"-target:jvm-1.8 "、"-deprecation "、"-unchecked "、"-Xlint "、"-feature "、"-Xmax-classfile-name "、" 242" <=在此添加)，

### 浏览器测试技巧

*   测试有时会不可靠，看似毫无理由地失败。有些人选择自动重新运行失败的浏览器测试。缺点是破坏了定时数据。
*   对失败的测试进行截图，使调试更容易。
*   VNC 可以安装和使用。安装`metacity`后，该浏览器可以在 VNC 随意拖动。从我们的一个浏览器中运行这个映像:ssh-p PORT Ubuntu @ IP _ ADDRESS-L 5902:localhost:5901 #通过 SSH 连接 sudo 安装 vnc4server 元城市 VNC 4 server-geometry 1280 x 1024-depth 24 export DISPLAY =:1.0 元城市& firefox &

### 码头专用提示

*   在 cron 作业上构建 Docker 映像。
    *   每周、每天或任何需要的时候构建。
        *   通过 API 很容易触发一个新的 Docker 映像构建
    *   在映像中包含像 node_modules 这样的依赖项。
        *   它们有助于缓解 DNS 中断带来的问题。
        *   他们保持依赖版本受控。
        *   即使一个模块从节点的 repos 中消失，运行应用程序所必需的依赖关系也是安全的。
    *   私有映像可以包括私有 gem 和私有源缓存。
*   数据库没有套接字文件，因此需要定义主机变量($PGHOST，$MYSQL_HOST)来指定 127.0.0.1，强制使用 TCP。
*   使用 CircleCI 便利版或官方 Docker Hub 镜像增加了在主机上缓存镜像的机会。
    *   构建这些图像将减少需要下载的图像图层的数量。
*   使用容器的-ram 变体将在/dev/shm 中运行给定的守护进程。
*   使用预安装的所有组件构建自定义映像可以加快构建速度并增加可靠性。
    *   Heroku(作为一个例子)可以把一个坏的更新推到他们的安装程序，破坏你的构建。
*   dockerize 实用程序可用于在运行测试之前等待服务容器可用。
    *   https://github.com/jwilder/dockerize
*   ElasticSearch 有自己的 Docker 注册表。
    *   https://www.docker.elastic.co/
*   容器可以有名称，因此多个给定的服务可以在同一个端口上以不同的主机名运行。
*   特权容器可以在远程环境和`machine`中运行。
*   卷不能从基本 Docker executor 装载到远程环境中。
    *   `docker cp`可以传输文件。
    *   引用的卷将从远程环境中装载到容器中。

有关迁移到 CircleCI 2.0 的更多信息，请参见我们的[文档](https://circleci.com/docs/)。如果您需要更多帮助，请访问我们的[支持页面](https://support.circleci.com/hc/en-us)。