# 装具

> 原文：<https://www.harness.io/blog/node-js-unit-testing-automation-with-drone-ci-using-mocha-framework>

了解如何使用 Drone CI 进行持续集成(CI ),并以一个简单的 Node.js 应用程序和 Mocha 作为单元测试框架。

没有别的办法:单元测试是任何软件开发生命周期的重要部分。在当今的现代云原生空间中，自动化测试的需求很高，有许多平台和工具可用。这种测试自动化可以通过持续集成(CI)过程来完成。CI 是整个 DevOps 流程中的一个基本方法。CI 帮助开发人员测试和构建他们的代码，因此他们可以确保没有任何东西被破坏，并且通过审批者的简单批准，代码可以自信地集成到主分支中。CI 流程可以通过 Drone CI 无缝自动化，以测试、构建和增强开发人员的信心。CI 让开发人员节省了手动测试的时间，并专注于构建客户需要的丰富特性。今天，我们将演示如何使用 Drone CI 进行 CI，并将一个简单的 Node.js 应用程序和 Mocha 作为我们的单元测试框架。

## 先决条件

*   从官网[这里](https://nodejs.org/en/download/)下载安装 Node.js。
*   [无人机 CI](https://www.drone.io/) 调教。

## 辅导的

**第一步:为你所有的申请文件创建一个文件夹:**

`mkdir myapp
cd myapp`

**第二步:编写一个简单的 Node.js 应用程序。**

通过运行以下命令初始化您的项目:

`npm init`

这将创建一个 package.json 文件，其中包含您为项目下载的所有依赖项。

**第三步:安装 express，Node.js web 应用框架:**

`npm install express --save`

**步骤 4:接下来，我们必须用下面的代码创建一个名为 app.js 的文件:**

`var express = require('express');
var app = express();
app.get('/', function (req, res) {
res.send('Hello World!');
});
app.listen(3000, function () {
console.log('Example app listening on port 3000!');
});` 

**第五步:运行应用:**

`node app.js`

### Mocha 单元测试框架

***Image credit:*** [***openbase***](https://openbase.com/categories/js/best-nodejs-testing-framework-libraries)

Mocha 测试框架是一个 JavaScript 库，用于测试 JavaScript 代码。这是一个开源框架，给了开发者很大的灵活性。它有一个简单的 API，可以用来测试同步和异步代码。它还具有链接、回调和承诺等特性。此外，它提供了并行运行测试的 API，这加快了为大型代码库编写测试的过程。Mocha 测试框架可以通过使用节点包管理器(NPM)来安装。

步骤 6:让我们将测试文件夹添加到我们的应用程序目录中，我们所有的测试都将驻留在这个目录中。

创建一个名为 test 的文件夹，并在该文件夹下创建一个文件 test.js。

您可以将 Mocha 和 Chai 安装为 Node.js 测试框架的一部分:

`npm install --save-dev mocha`

在特定项目的 package.json 文件中添加 Mocha 作为依赖项。

**步骤 7:将下面的基本 Mocha 字符串测试规范添加到 test.js 文件中:**

`var assert = require('assert');
describe('Basic Mocha String Test', function () {
it('should return number of characters in a string', function () {
assert.equal("Hello".length, 4);
});
it('should return first charachter of the string', function () {
assert.equal("Hello".charAt(0), 'H');
});
});` 

**第八步:你可以回去查看你的 package.json 文件。它应该包括摩卡部分，应该是这样的:**

`"scripts": {
"test": "mocha"
}` 

**步骤 9:现在，运行测试。**

转到您的应用程序目录并运行以下命令:

`npm test `

我的输出如下:

它失败了，因为运行 Mocha 时似乎出现了错误。你可以看到 1 失败了。

示例项目的源代码在这里共享[。](https://github.com/pavanbelagatti/myapp)

## Node.js 应用程序的 CI

到目前为止，我们在本地测试了 Node.js 应用程序，这可以通过 CI 服务器自动完成。无人机 CI 是我们首选的 CI 平台，我们已经知道如何在本地运行。它是开源的，安装简单，只需几分钟。

如果你不知道如何在你的本地机器上设置一个无人机服务器，那么这里是我同事的一篇文章，它将帮助你[建立和运行你自己的无人机 CI](https://dev.to/jimsheldon/run-your-own-drone-ci-4335)。

每次开发人员将代码推送到应用程序的主分支时，使用像 Drone 这样的 CI 服务器可以帮助公司找到 bug，并在错误到达客户之前纠正错误。

让我们继续下一步。

**第十步:将你的应用推送到 GitHub。**

我们应该确保没有文件或存储库丢失。在我们的 GitHub 存储库中，我们仍然需要添加两个文件:

1.  . drone.yml 作为无人机配置文件的一部分。通过在 Git 存储库的根目录中放置一个. drone.yml 文件来配置管道。
2.  作为构建管道中的一个步骤，构建并发布 Docker 映像。因此，添加一个简单的 docker 文件，因为它包含构建容器的指令。

从节点:14-阿尔卑斯山

环境节点 _ 环境开发

#添加工作目录

工作目录/应用程序

#缓存和安装依赖项

复制 package.json。

运行 npm 安装

#复制应用程序文件

收到。。

#暴露端口

曝光 3000

#启动应用程序

CMD [ "node "，" app.js" ]

. drone.yml 文件如下所示:

种类:管道

类型:码头工人

名称:默认

‍

平台:

操作系统:linux

拱门:arm64

步骤:

-名称:测试

图像:节点

命令:

- npm 安装

- npm 测试

-姓名:码头工人

图片:插件/docker

设置:

#注册表:docker.io

回购:pavansa/myapp

用户名:

from _ secret:docker _ 用户名

密码:

发件人 _ 秘密:docker _ 密码

标签:

-帕万人

通过在 Git 存储库的根目录中放置一个. drone.yml 文件来配置管道。YAML 语法旨在易于阅读和表达，以便任何查看存储库的人都能理解工作流。简单的节点示例见这里的[。](https://docs.drone.io/pipeline/docker/examples/languages/node/)

**步骤 11:现在，让我们打开无人机仪表盘，同步我们的新回购。然后，我们用秘密激活它。**

同步回购后，您应该会看到您的所有回购如下，并选择一个我们正在建设的。

‍

下一步是通过添加秘密来激活存储库。

‍

‍

‍

‍

完成上述所有步骤后，选择 NEW BUILD 按钮，它将开始构建管道。

在这里，您将看到 Drone 通过构建代码并根据我们为应用程序代码指定的测试来测试它(记住，我们已经为我们的应用程序添加了一个普通的 Mocha 测试框架)。如果测试通过，那么它应该继续将新建的图像推送到我们在. drone.yml 步骤中指定的 DockerHub。

此外，如果测试失败，那么应用程序管道不应该构建，应该在那里停止。

让我们看看我们的建筑发生了什么。

‍

不出所料，管道在没有执行后续步骤的情况下发生了故障。您可以返回，纠正错误，无人机会自动运行管道。如果一切正常，那么它会如约将新的构建推送到 Docker Hub。

‍

‍

当一切都通过后，去检查你在. drone.yml 中提到的 Docker Hub repo，它应该已经把图像推送到那里了。这是我们用帕万语写的标签，你可以在这里看到:

‍

恭喜你！我们已经成功构建了一个简单的 Node.js 应用程序，添加了一个简单的 Mocha 测试，使用 Drone 做了 CI，并将新图像推送到 Docker Hub。

要了解 Harness 如何帮助您完成 CI 流程，请[申请一个个性化演示](https://harness.io/demo)。

‍