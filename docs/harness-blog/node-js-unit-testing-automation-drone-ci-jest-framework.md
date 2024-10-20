# Node.js 使用 Jest 框架| Harness 通过 Drone CI 进行单元测试自动化

> 原文：<https://www.harness.io/blog/node-js-unit-testing-automation-drone-ci-jest-framework>

了解如何使用 Jest 框架通过 Drone CI 进行 node.js 单元测试自动化。

这些年来，软件开发过程发生了巨大的变化，从敏捷开发到我们今天看到的 DevOps 实践。此外，通过新的工具和自动化，软件测试获得了巨大的动力。测试软件在很大程度上依赖于现代云世界中的持续集成(CI)最佳实践和工具。组织需要关注自动化测试，让开发人员更容易进行测试，这样他们就可以专注于构建丰富的功能。因此，软件测试是软件开发生命周期(SDLC)中的一个重要过程。那么，在将来进行更改时，确保代码继续工作的最佳方式是什么呢？编写单元测试。单元测试是软件开发的一个关键部分，在过去的几年里越来越受欢迎。为什么？因为它有助于您在潜在的错误或问题发生之前识别它们，并阻止它们进入生产环境。

在这篇博客文章中，我们将以 Jest 为例回顾单元测试的基础知识，了解单元测试为什么重要，并向您展示如何在您的项目中实现它。

## 什么是单元测试？

单元测试是可以验证单个功能、特性或方法的小块代码。这些测试侧重于测试代码背后的逻辑，而不是它的行为。他们不测试用户界面或性能，而是测试你写的代码是否符合预期。单元测试也被称为“测试驱动开发”(TDD)。您可能想知道为什么每个代码/特性都要用单元测试来测试，因为它有很多好处:它可以在 bug 投入生产之前捕捉它们，防止回归，并随着时间的推移提高代码质量。单元测试的主要目标是确保源代码的单个单元按预期运行，并且没有任何隐藏的错误或缺陷。单元测试不是你可以像开关一样开关的东西；这是一个需要从一开始就成为你的开发工作流程的一部分的过程。

## Jest 单元测试框架

***Image credit:*** [***openbase***](https://openbase.com/categories/js/best-nodejs-testing-framework-libraries)

Jest 是由脸书创建的 JavaScript 测试框架，用于测试 JavaScript 代码。Jest 的许多特性使它成为比 Jasmine 或 Mocha 等其他测试框架更好的选择。它能够测试异步代码，而不会引入任何复杂性。此外，它被设计为利用结合测试运行程序使用 mocking 库的好处。它还提供了许多其他框架中没有的有用特性。

## 先决条件:

*   GitHub 账户。
*   安装 [Docker 桌面](https://www.docker.com/products/docker-desktop/)。
*   从官网[这里](https://nodejs.org/en/download/)下载安装 Node.js。
*   创建一个通往互联网的隧道，这样我们的无人机 CI 流程就可以从 GitHub 接收 webhook 流量。
*   [无人机 CI](https://www.drone.io/) 调教。按照这个[简单教程](https://dev.to/jimsheldon/run-your-own-drone-ci-4335)让无人机 CI 在你的本地机器上运行。

## 教程:

*   创建一个新目录来存储本教程的所有源代码，并进入到该目录的根目录:

`mkdir jest-unit-testing
cd jest-unit-testing`

*   使用以下命令启动节点应用程序:

`npm init -y`

在终端上运行上述命令后，您应该会看到 package.json 文件被创建。

‍

‍

*   现在，由于我们使用 [Jest](https://jestjs.io/) 作为我们的单元测试框架，我们需要添加 Jest 作为我们项目的依赖项。让我们通过运行以下命令来添加它:

`npm install --save-dev jest`

现在，您可以返回到 package.json 文件，确认 Jest 已经作为一个依赖项被添加。

*   此外，我们需要指定 package.json 文件中使用的单元测试框架。我们需要将 package.json 文件中的“test”:“echo \ " Error:no test specified \ " & & exit 1”替换为“test”:“jest”。

最后，您的 package.json 文件应该如下所示:

‍

‍

*   让我们在一个新文件 calculator.js 中创建主要的应用程序逻辑，并在其中添加以下代码:

`const mathOperations = {
sum: function(a,b) {
return a + b;
},

diff: function(a,b) {
return a - b;
},
product: function(a,b) {
return a * b
}
}
module.exports = mathOperations` 

在上面的例子中，我们基本上使用数学运算——加法、减法和乘法——作为我们的逻辑。

*   是时候使用 Jest 框架为我们上面创建的逻辑编写一个测试用例了。为此，在同一个根目录下创建一个新文件 calculator.test.js，并添加以下代码:

`const mathOperations = require('./calculator');

describe("Calculator tests", () => {
test('adding 2 + 4 should return 6', () => {
// arrange and act
var result = mathOperations.sum(2,4)

// assert
expect(result).toBe(6);
});

test("subtracting 3 from 10 should return 7", () => {
// arrange and act
var result = mathOperations.diff(10,3)

// assert
expect(result).toBe(7);
});

test("multiplying 3 and 9 should return 27", () => {
// arrange and act
var result = mathOperations.product(3,9)

// assert
expect(result).toBe(27);
});
})` 

*   最后，让我们在终端上使用下面的命令进行本地测试:

`npm test`

您应该看到我们在 calculator.test.js 文件中提供的输入成功通过了测试。您可以修改这个文件并添加您自己的值。

‍

示例项目的源代码分享到这里:[https://github.com/pavanbelagatti/jest-unit-test-drone](https://github.com/pavanbelagatti/jest-unit-test-drone)

## 与无人机 CI 的持续集成

在我们的示例项目中，代码库很小，我们可以轻松地管理它，但是如果项目越来越大，越来越多的开发人员开始开发它，该怎么办呢？

开发人员通常使用 DevOps 工具进行协作，以获得更高的生产率和效率。一个突出的工具是 GitHub，它帮助开发人员将他们的代码推送到主分支。每当开发人员推出他们的代码时，必须有一个系统来自动测试代码，并对可能的错误或错误发出警报。手动测试需要很多时间，并且在大多数情况下是不可信的。这就是持续集成发挥作用的地方。持续集成测试工具 Drone CI by Harness 已经成为市场上最好的开源工具之一。它根据指定的测试进行测试和构建。设置起来也很简单，容易理解。

让我们在本地机器上设置一个无人机 CI 服务器来自动化我们的测试。

我同事的文章将帮助您[建立并运行您自己的无人机 CI](https://dev.to/jimsheldon/run-your-own-drone-ci-4335)。

使用像 Drone 这样的 CI 服务器有助于公司在到达客户之前发现错误并纠正错误。

使用 Drone CI 时，您需要一个配置文件. drone.yml。以下是节点项目的示例文件:

`kind: pipeline
name: default

steps:
- name: test
image: node
commands:
- npm install
- npm test` 

我们的代码仓库已经有了这个配置文件。

当您打开无人机 UI 时，您将看到一条欢迎消息，您需要输入一些基本信息，作为注册过程的一部分。

‍

登录后，您会看到列出了您的所有存储库。选择我们正在处理的一个。通过转到设置并添加密码来激活存储库。秘密基本上就是你的 Docker hub 用户名和密码。

添加秘密后，单击新的构建。您应该会看到如下所示的屏幕:

当您将鼠标悬停在构建管道上时，应该会看到执行步骤:

多酷啊。您可以看到测试成功执行。现在，每当有人将任何代码推送到主分支时，无人机就会开始它的魔法，并运行指定的测试，在本例中是 Jest 单元测试框架。

你愿意尝试其他 Node.js 测试框架吗？查看我的另一篇[文章](https://docs.google.com/document/d/1msiTfvyc8CA0NFADXq4aD77d2ufTE3ZmnyL-0ivcJXs/edit?usp=sharing)，这篇文章展示了如何使用 Mocha 作为简单 Node.js 应用程序的单元测试框架，以及如何使用 Drone CI 实现自动化。

### 无人机桌面

你们中的一些人可能想知道是否有一个无需在电脑上设置无人机就可以立即使用的无人机扩展。我们为你准备好了。你只需要在你的电脑上安装 Docker Desktop，使用下面的命令，你可以在一分钟内轻松设置好无人机扩展。

`docker extension install kameshsampath/drone-desktop-extension:v1.1.0`

看，这很简单。导入管道。

然后单击“开始”按钮，该按钮位于“操作”选项卡下方。

当您单击 start 按钮时，您应该看到您的管道正在运行。

最后，您应该看到管道成功完成。

祝贺您通过 Drone 成功运行 CI 渠道。

如需个性化线束演示，请访问我们的网站[https://harness.io/demo](https://harness.io/demo)