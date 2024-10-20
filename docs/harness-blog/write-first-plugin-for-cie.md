# 如何为 CI 编写第一个插件——定制 Git 插件| Harness

> 原文：<https://www.harness.io/blog/write-first-plugin-for-cie>

在这篇文章中，我们将学习更多关于插件的步骤，并且我们将创建一个定制的 git 插件。

插件是执行预定义任务的 Docker 容器。它们可以用来克隆 git 存储库、构建 Docker 容器、上传工件等等。插件配置是 CI 流程中的一个步骤。

## 为什么插件很重要

插件是高度可扩展和基于容器的。它们本质上是模板化的脚本，可以用任何编程语言编写。与管道步骤中的脚本相比，管理插件更加干净和容易。在本文中，我们将讨论如何为 CI 编写第一个插件。这很简单，只是让你适应这个过程。当谈到插件时，世界真的是你的牡蛎，所以我们希望你能在这个主题上了解更多，并继续写作！让我们深入到插件步骤。

## 插件步骤

首先:什么是插件步骤？简而言之，CI 阶段由三种不同类型的步骤组成:运行步骤、运行测试步骤和插件步骤。插件步骤将插件的 Docker 映像作为 Docker 容器运行，以执行由其定义的任务。

插件步骤有 3 个输入:

1.  **图片**:这是插件 Docker 图片。插件步骤通过运行 Docker 容器来执行插件 Docker 映像中存在的入口点。
2.  ConnectorRef :这是一个 Docker 连接器，提供了获取插件图像的凭证。如果图像是公开的，可以使用匿名 Docker 连接器。
3.  **设置**:设置是插件参数，作为环境变量提供给插件。环境变量名大写并以 PLUGIN_ 为前缀，以防止命名冲突。示例环境变量可以是:

让我们来看一个示例插件步骤。下面的步骤使用“插件/下载”插件图像，它下载源到目标路径中提到的 url。在这里，它将 AWS CLI 下载到工作区目录，文件名为 *awscli.zip* :

###### -step:type:plugin
name:download
identifier:download
spec:download
connector:docking
image:plugins/download
settings:https://awscli . Amazon ws . com/awscli-exe-Linux 来源:https://awscli . Amazon ws . com/awscli-Linux

下面是一个 CI 管道，上面的插件步骤正在运行:

## 为 CI 编写第一个插件

现在我们已经介绍了插件步骤的基础，让我们来编写一个自定义插件！

我们将编写一个定制的 git 插件，克隆一个公共的 git 库并打印最后的提交信息。它将接受以下设置作为输入:

1.  repo_url: Git 存储库 url。
2.  分支:要签出到的存储库的分支。
3.  路径:克隆存储库的目录路径。默认情况下，它被假定为工作区目录。

下面是实现定制 git 插件功能的 bash 脚本 *clone.sh* :

###### #!/bin/sh
set -xe

#如果没有设置路径，那么使用当前目录
path=${PLUGIN_PATH:-。}
mkdir-p $ { path }
CD $ { path }

#克隆公共 git repo 并签出到分支
git 克隆${PLUGIN_REPO_URL}。
git check out $ { PLUGIN _ BRANCH }

#打印最后一次提交
git log -1 - stat

现在，我们必须创建一个运行上述脚本的 Docker 映像。这可以通过将这个脚本指定为 [Dockerfile](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) 中的入口点来实现。以下是完整的 Dockerfile 文件:

###### 从 alpine/git

#将克隆脚本复制到 Docker 映像
复制 clone . sh/usr/local/bin/

#使克隆脚本可执行
运行 chmod+x/usr/local/bin/clone . sh

entry point["/usr/local/bin/clone . sh "]

之后，我们需要构建这个映像并将其发布到 Docker 注册中心，并在 CI 管道中使用它。对于这个例子，图像被推送到“shubham 149/git-plugin”Docker Hub repo。

让我们在 CI 管道中使用这个定制的 git 插件。下面的插件步骤将在 codebase 目录中克隆“shubham 149/git-plugin”github 存储库。

###### -步骤:
类型:插件
名称:自定义 git 克隆
标识符:克隆
规格:
connectorRef: dockerhub
图像:shubham149/git-plugin
设置:
路径:代码库
repo _ URL:https://github.com/shubham149/git-plugin.git
分支:主

下面是使用我们的定制 git 插件步骤运行的 CI 管道:

定制 git 插件源代码的链接:[https://github.com/shubham149/git-plugin](https://github.com/shubham149/git-plugin)

上面的例子使用一个 bash 脚本来创建一个插件。但是写插件就没有这个限制了。插件可以用你选择的任何编程语言编写。这里有一个用 golang 写插件的样板模板:[https://github.com/drone-plugins/boilr-plugin](https://github.com/drone-plugins/boilr-plugin)

### 在本地测试插件

插件可以作为 Docker 容器在本地环境中测试。下面是一个在本地运行定制 git 插件的 Docker 命令示例:

###### dock run-RM \
和 plugin _ path = base code \
和 PLUGIN _ REPO _ URL = https://github。com/shubham 149/git 插件。git \
和 plugin _ branch = main \
shubham 149/git 插件

## 结论

我们希望你喜欢你对插件的第一次尝试！从我们的[插件注册表](http://plugins.drone.io/)可以看出，插件可以实现如此大的扩展性。从通知到 Slack 再到完整的 Datadog 集成，插件可以让一个伟大的产品变得更好。我们希望通过学习更多关于插件的知识，你会受到启发去编写你自己的插件。谁知道你能解决什么常见问题？

为了进一步阅读插件，让我们看看 Jenkins 是如何错过我们关于[依赖地狱](https://harness.io/blog/dependency-plugin-hell-jenkins/)的文章的！