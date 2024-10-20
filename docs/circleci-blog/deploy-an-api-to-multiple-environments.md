# 使用工作流将 API 部署到多个环境中

> 原文：<https://circleci.com/blog/deploy-an-api-to-multiple-environments/>

在将应用程序部署到生产环境之前，将应用程序部署到测试环境是一种常见的做法。首先进行测试以确保功能正常工作是一种最佳实践，许多开发人员、初创公司和组织都是这样部署他们的应用程序的。

我发现，每次对我的应用程序进行更新时，手动操作很快就会变得既耗时又低效。自动化这个过程有多好？在本教程中，我们将在试运行和生产环境中部署一个 API。这个过程也称为多重部署。

在我们的多部署流程中，我们将创建一个工作流，首先将一个 API 部署到一个暂存环境，然后使用 Postman 对暂存的 API 运行自动化测试。如果测试通过，我们的工作流将在没有我们干预的情况下将 API 部署到生产环境中。如果测试没有通过，API 将不会被部署到生产环境中。

## 先决条件

要遵循本教程，需要做一些事情:

1.  系统上安装的 [Node.js](https://nodejs.org)
2.  Postman for Desktop 安装在您的系统上(您可以在这里下载它
3.  Heroku 或其他应用托管平台
4.  一个[圆](https://circleci.com/signup/)的账户
5.  GitHub 的一个账户

安装并设置好所有这些之后，是时候开始本教程了。

## 克隆 API 项目

首先，您需要克隆 API 项目。我们将使用一个简单的 Node.js API 应用程序，它有一个根端点和另外两个端点，用于创建和获取用户。通过运行以下命令克隆项目:

```
git clone --single-branch --branch base-project https://github.com/coderonfleek/multi-deploy-api.git 
```

克隆过程完成后，转到项目的根目录并安装依赖项:

```
cd multi-deploy-api
npm install 
```

接下来，运行应用程序:

```
npm start 
```

应用程序将开始监听默认端口`3000`。

打开 Postman 并向`http://localhost:3000/users/get`端点发出一个`GET`请求。这将返回一个用户数组。

![Get Users - Postman](img/6e77d231482a35894e61d3667e35a974.png)

我们的 API 已经可以部署了。

## 在 Heroku 上设置登台和部署环境

下一个任务是创建部署环境。您将为`staging`(应用程序的更新将被部署，用于测试)和`deploy`(生产环境)创建一个。在本教程中，我们将在 Heroku 上创建这些环境，但是您可以使用您喜欢的托管平台。

进入你的 Heroku 账户仪表盘，点击**新建**，然后**创建新应用**。在应用创建页面上，创建`staging`应用环境。

![Staging App - Heroku](img/46168b207d896516e356477ad086dec2.png)

现在，重复相同的过程来创建生产应用程序。

![Production App - Heroku](img/f3cbe37128185fac5c2a04b910131e47.png)

现在已经为部署应用程序设置好了环境。最后，在 Heroku 上，从**账户设置**页面上的**账户**选项卡中获取 API 密钥。

## 用 Postman 集合设置 API 测试

在它被部署到`staging`环境之后，我们希望对 API 进行测试。对于本教程，我们将使用 Postman 来设置可以自动化的 API 测试。

第一步是为 API 请求创建一个专用环境。在 Postman 桌面上，单击**管理环境**齿轮图标(位于右上角)。**管理环境**对话框显示任何已经存在的环境。点击**添加**创建新环境。

在“新环境”对话框中，输入新环境的名称。将登台环境 API 基本 URL ( `https://users-api-staging.herokuapp.com`)作为环境变量(`api_url`)填入。您在`INITIAL VALUE`中输入的内容会被复制到`CURRENT VALUE`中。保持那样；使用`CURRENT VALUE`超出了本教程的范围。

![Create environment - Postman](img/c704bb6726a9c8cafc8bcd137d1d6f04.png)

点击**添加**完成环境创建。使用屏幕右上角的下拉菜单切换到新环境。

### 创建收藏

下一步是为您将要测试的 API 的用户端点创建一个 Postman 集合。

点击左侧边栏中的**收藏**。然后点击**新收藏**。

![Create collection button - Postman](img/8b16ae229733f07ebde4624c29cad561.png)

在**新收藏**对话框中，填写您收藏的名称(`Users`)。如果您想添加有关该收藏的更多信息，也可以添加说明。

![Create Collection - Postman](img/6d93ce509e3c4232d8c18ce8ff39b059.png)

点击**创建**完成集合设置。新收藏会立即显示在左侧栏的**收藏**下。

### 向集合中添加请求

现在是时候向您的集合添加请求了。API 由两个端点组成:

*   (GET):获取用户配置文件列表
*   `{{api_url}}/users/create` (POST):创建新的用户配置文件

您将为每个端点添加一个请求。若要开始，请单击集合旁边的弹出菜单(箭头图标)。

点击**添加请求**。在**新请求**对话框中，为`{{api_url}}/users/get`端点创建一个请求。点击**保存到用户-API-集合**将其保存到您之前创建的集合中。

![Add Request - Postman](img/2897c19833a624291fae016885a0aac6.png)

现在请求已经创建，加载了一个新的请求选项卡。使用您创建的`api_url`变量，在地址栏(`{{api_url}}/users/get`)中输入请求的端点。确保选择**获取**作为请求方法。点击**保存**。

接下来，为`{{api_url}}/users/create`端点创建一个请求。我们需要获得随机值，以便`POST`请求可以创建测试用户。为此，我们需要一个`Pre-request`剧本。

打开**预请求脚本**选项卡，添加以下脚本:

```
let random = +new Date();

pm.globals.set("name", `Test-User-${random}`); 
```

这个脚本使用当前时间戳为每个触发的请求随机创建名称。随机变量`name`被设置为请求实例的全局变量。

现在用动态`name`变量编写请求体。

![Dynamic parameters - Postman](img/160f231620ea832f2ee81e299c8ac243.png)

动态的`name`参数将被用于对`/users/create`端点的每个请求。

### 添加测试

是时候向您刚刚创建的请求添加一些测试了。

单击`{{api_url}}/users/get`请求(在左侧栏中)以确保它已被加载。点击**测试**选项卡。在窗口中，添加:

```
pm.test("Request is successful with a status code of 200", function () {
  pm.response.to.have.status(200);
});

pm.test("Check that it returns an array", function () {
  var jsonData = pm.response.json();
  pm.expect(jsonData).to.be.an("array");
}); 
```

第一个测试检查请求是否成功返回，状态代码为`200`。第二个测试确保它也返回一个数组。

点击用户创建请求**测试**选项卡进行添加:

```
pm.test("User creation was successful", function () {
  pm.expect(pm.response.code).to.be.oneOf([200, 201, 202]);
});

pm.test("Confirm response message", function () {
  var jsonData = pm.response.json();
  pm.expect(jsonData.message).to.eql("User successfully registered");
}); 
```

这里的第一个测试通过断言状态代码`200`、`201`或`202`来检查用户创建是否成功。第二个测试确保返回正确的响应消息。

**注意:** *每当您对此集合中的任一请求进行更改时，请务必点击**保存**。*

## 用 Newman 配置自动化测试

接下来，我们将使用 Postman 的 CLI 工具 Newman 来自动化测试的运行方式。

您的收藏将需要一个公共 URL。此链接将指向您在邮递员服务上托管的收藏版本。使用此链接而不是将您的收藏导出为`json`的主要优点是，只要您登录 Postman，您的收藏的更改将始终可用。当您在桌面上登录时，Postman 会将您所有的本地收藏同步到您的 Postman 帐户。如果你需要一个邮差账户，[登录页面](https://identity.getpostman.com/signup)会引导你创建一个。

要获取您的公共 URL，请从您的收藏弹出菜单中单击**共享**。您收藏的公共链接显示在**获取链接**选项卡下。

![Get Link - Postman](img/06afe536f16946e1fb2015255e48c3e3.png)

使用此链接，您不再需要导出和移动收藏文件。您确实需要在环境文件更新时将其导出，但是这种情况不太常见。

点击**管理环境**图标，下载你的邮递员环境。点击**下载**获取文件。文件名类似于`User-API-Tests.postman_environment.json`(取决于您如何命名环境)。将此环境文件添加到项目的根目录。

接下来，通过运行以下命令，在项目的根目录下安装 Newman CLI 工具:

```
npm install --save-dev newman 
```

将`package.json`中的测试脚本替换为:

```
"scripts": {
    ...
    "test": "npx newman run [YOUR_COLLECTION_PUBLIC_URL] -e ./[YOUR_ENVIRONMENT_FILENAME].json"
} 
```

这个测试脚本使用`npx`对集合的 URL 运行`newman`。集合 URL 使用环境的文件名来指定测试运行的位置。分别用您的集合公共 URL 和环境文件名替换值`[YOUR_COLLECTION_PUBLIC_URL]`和`[YOUR_ENVIRONMENT_FILENAME]`。Newman 针对请求运行集合中定义的测试，并挑选任何已经定义的请求前脚本。

现在您的测试已经为自动化设置好了。

## 将您的项目连接到 CircleCI

从[将你的项目推送到 GitHub](https://circleci.com/blog/pushing-a-project-to-github/) 开始。

现在，转到 CircleCI 仪表板上的**添加项目**页面来添加项目。

![Add Project - CircleCI](img/cd1a436beaea3bbab21a653020df520e.png)

点击**设置项目**。

![Add Config - CircleCI](img/34f7d1655f1804737919b7f2ddefe78c.png)

在设置页面上，点击 **Use Existing Config** 以指示您正在手动设置配置文件，并且不使用显示的示例。接下来，您会得到一个提示，要么下载管道的配置文件，要么开始构建。

![Build Prompt - CircleCI](img/51dc355015cfb0f73e8494f2dadb41cb.png)

点击**开始建造**。这个构建将会失败，因为我们还没有设置配置文件。我们将在下一步中这样做。

在离开 CircleCI 控制台之前，您需要设置[环境变量](https://circleci.com/docs/env-vars/),以便将应用程序部署到 Heroku 上的两个环境(登台和生产)中。

从**管道**页面，选择您的应用程序，然后点击**项目设置**。

![Project Settings - CircleCI](img/3c84b93bfa5a1990e57e5ab06dea1bfd.png)

从**项目设置**侧菜单中，点击**环境变量**，然后点击**添加环境变量**。

![Add Environment variable - CircleCI](img/8c42ac189cf95628e7a1fef4e7668b2c.png)

添加这些变量:

*   `HEROKU_STAGING_APP_NAME`:暂存环境的 Heroku 应用程序名称(在本例中为`users-api-staging`)
*   `HEROKU_PRODUCTION_APP_NAME`:生产环境的 Heroku 应用程序名称(在本例中为`users-api-production`)
*   你的 Heroku API 密钥

现在，您已经准备好部署到 Heroku 上的临时环境和生产环境了。

## 配置阶段->测试->部署工作流

正如本教程开始时所承诺的，我们的目标是创建一个自动化的工作流，其中应用程序:

1.  部署到暂存环境
2.  在分段地址上测试(如果测试通过)
3.  部署到生产环境中

我们的下一个任务是编写执行这些步骤的部署管道脚本。首先在项目的根目录下创建一个名为`.circleci`的文件夹。在刚刚创建的文件夹中添加一个名为`config.yml`的配置文件。输入以下代码:

```
version: 2.1
orbs:
  heroku: circleci/heroku@0.0.10
jobs:
  stage:
    executor: heroku/default
    steps:
      - checkout
      - heroku/install
      - heroku/deploy-via-git:
          app-name: $HEROKU_STAGING_APP_NAME

  test:
    working_directory: ~/repo
    docker:
      - image: circleci/node:10.16.3
    steps:
      - checkout
      - run:
          name: Update NPM
          command: "sudo npm install -g npm@5"
      - restore_cache:
          key: dependency-cache-{{ checksum "package-lock.json" }}
      - run:
          name: Install Dependencies
          command: npm install
      - save_cache:
          key: dependency-cache-{{ checksum "package-lock.json" }}
          paths:
            - ./node_modules
      - run:
          name: Run tests
          command: npm run test

  deploy:
    executor: heroku/default
    steps:
      - checkout
      - heroku/install
      - heroku/deploy-via-git:
          app-name: $HEROKU_PRODUCTION_APP_NAME

workflows:
  stage_test_deploy:
    jobs:
      - stage
      - test:
          requires:
            - stage
      - deploy:
          requires:
            - test 
```

这是一个很好的剧本，对吧？让我分解一下，补充一些细节。首先，拉起`heroku`球体:

```
orbs:
  heroku: circleci/heroku@0.0.10 
```

然后我们创建三个`jobs`:

*   `stage`:从 repo 中签出项目代码，并使用 Heroku CircleCI orb 将应用程序部署到临时环境中。
*   `test`:安装需要安装`newman` CLI 的项目依赖项。然后它运行`package.json`中的`test`脚本，调用`newman`来运行集合中针对刚刚部署的临时应用程序的测试。
*   `deploy`:将应用程序部署到 Heroku 上的生产环境中。

[orb](https://circleci.com/orbs/)是抽象常见任务的可重用管道包。我们正在使用的 [Heroku orb](https://circleci.com/developer/orbs/orb/circleci/heroku) 抽象了安装和设置`heroku` CLI 工具来部署应用程序的过程。

最后，脚本定义了`stage_test_deploy`工作流。该工作流运行`stage`作业。一旦该作业成功运行，`test`作业就会对刚刚部署到 staging 的应用程序运行测试。当试运行应用程序上的所有测试都通过后，脚本运行`deploy`作业将应用程序部署到生产环境中。这是一个平稳、简单的过程。

是时候测试我们的工作流程了。提交所有更改并将代码推送到远程存储库以运行管道脚本。

当前活动的`job`正在运行，而其他的在等待。

![Workflow running - CircleCI](img/8da3c070214b1f404c7c032592d0b0e4.png)

成功的结果是一个接一个地运行作业。

![Workflow complete - CircleCI](img/5db6ff38c2525f3abff69a38d0dd42f0.png)

单击工作流程(`stage_test_deploy`)链接查看流程中的步骤及其持续时间。

![Workflow complete - CircleCI](img/d6242eedde501a578728ffd4ebdcf83f.png)

您还可以单击每个构建来了解它如何运行的详细信息。

![Test Job Details - CircleCI](img/f0bfce9bf37b583ec4ebc29edc0c9904.png)

干得好！

## 结论

自动化现实生活中的手动任务可以帮助您节省时间和精力，提高您的生产力。在本教程中，我们已经自动化了将应用程序暂存、测试和部署到生产环境的过程。您可以将在此学到的知识应用到更复杂的场景中，并进一步提高工作流程的效率。

编码快乐！

* * *

Fikayo Adepoju 是 LinkedIn Learning(Lynda.com)的作者、全栈开发人员、技术作者和技术内容创建者，精通 Web 和移动技术以及 DevOps，拥有 10 多年开发可扩展分布式应用程序的经验。他为 CircleCI、Twilio、Auth0 和 New Stack 博客撰写了 40 多篇文章，并且在他的个人媒体页面上，他喜欢与尽可能多的从中受益的开发人员分享他的知识。你也可以在 Udemy 上查看他的视频课程。

[阅读 Fikayo Adepoju 的更多帖子](/blog/author/fikayo-adepoju/)