# Firebase Hosting -部署到 Firebase Hosting -部署 Gatsby 站点| CircleCI

> 原文：<https://circleci.com/blog/automatically-deploy-a-gatsby-site-to-firebase-hosting/>

[Firebase Hosting](https://firebase.google.com/products/hosting) 是 Google 的一个 web 应用托管平台。通过这项服务，你可以在谷歌的基础设施上托管你的网络应用。它支持简单的一步部署，并具有其他很酷的功能，如从 cdn 快速托管和回滚。在 [Firebase 托管文档](https://firebase.google.com/docs/hosting/)中有一个很好的服务概述。

Gatsby 是一个框架，可以让你快速创建基于 React 的应用和网站。它允许你用从各种来源获取的数据建立这些网站，包括降价文件、API，甚至 CMS。然后，它将这些数据与基于 React 的前端架构相结合，构建速度极快的交互式网站。Gatsby 将 web 应用程序编译成优化的静态文件，我们将把这些文件部署到 Firebase 主机上。我觉得很神奇，很高兴和大家分享！

在本文中，我们将设置一个简单的 Gatsby 站点，将代码托管在 GitHub 存储库中，并使用 CircleCI 将 web 应用程序自动部署到 Firebase 主机。

## 先决条件

为了完成本教程，您需要安装以下软件:

1.  [去](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
2.  [Node.js](https://nodejs.org)

***注意:**你还需要有一个谷歌账户才能使用 Firebase 主机。*

## 为什么是盖茨比？

我选择盖茨比只是因为它能让我们专注于高层次的细节。例如，我们不会从头开始构建页面、找出路径、添加 404 页面等等，而是将所有这些都内置到我们即将生成的 starter 项目中。Gatsby 为我们提供了这些现成的优势，但托管的概念仍然适用于任何其他类型的 web 应用程序，这些应用程序可以编译为静态文件，包括 Vue 和 Angular 应用程序，甚至是由静态站点生成器生成的网站。

## Gatsby 项目设置

首先，我们需要在本地开发环境中安装 Gatsby。我们可以通过运行以下命令来实现:

```
npm install --global gatsby-cli 
```

安装完成后，我们将可以使用`gatsby`命令。现在，让我们使用 Gatsby CLI 来生成一个新站点:

```
gatsby new gatsby-site 
```

接下来，我们需要将目录更改为新创建的`gatsby-site`文件夹:

```
cd gatsby-site 
```

最后，我们可以通过启动开发服务器来浏览我们生成的站点:

```
gatsby develop 
```

您的新网站现在可以在`http://localhost:8000`访问。

如果一切运行成功，您现在就有了一个在本地运行的 Gatsby 站点。继续探索这个网站。看起来是这样的:

![Gatsby starter page](img/8d522b97f8d3106c5b902a2fd2d51935.png)

如果您浏览一下生成的文件，您会发现 Gatsby 的文件夹结构很容易理解。例如，主页的代码可以在`src/pages/index.js`中找到。还要注意，不同页面之间的链接工作正常，我们还设置了一个 404 页面。你可以通过去一个不存在的路线来测试 404 页面。

Gatsby 提供了这些底层细节，比如路由，并为我们提供了一个功能性的 web 应用程序，我们现在可以将它部署到 Firebase 主机上。

### 推送至 GitHub

此时，让我们初始化一个新的 Git 存储库，并将代码推送到 GitHub。继续在`gatsby-site`文件夹中初始化一个新的 Git 存储库，并使用以下代码行创建一个初始提交:

```
git init
git add -all
git commit -m "Generate Gatsby site" 
```

之后，继续在 GitHub 上创建一个新的存储库，并将代码推送到存储库。

如果你不熟悉 GitHub，这个指南是一个很好的参考资料。

## Firebase 设置

现在，我们有了一个功能性的网站，可以部署到 Firebase 主机上了。在此之前，我们需要使用以下三个简单的步骤在 Firebase 上创建一个新项目:

![](img/6d555f06f5e5ec3fe327f8742c1251f0.png)


*   在弹出的模式中给你的项目命名，然后点击**创建项目**。

![](img/50d421b4157bbcce1cddcf5e59462d9e.png)

一旦创建了项目，我们需要在本地设置 Firebase，以便将我们的本地存储库链接到 Firebase 项目。通过运行以下命令安装 Firebase 命令行工具:

```
npm install -g firebase-tools 
```

我们还需要在本地将`firebase-tools`包作为`devDependency`安装到我们的项目中。这将在以后与 CircleCI 集成时派上用场，circle ci 默认不允许全局安装包。所以让我们现在就安装它:

```
npm install -D firebase-tools 
```

之后，我们需要登录 Firebase，将 CLI 连接到在线 Firebase 帐户。我们可以通过运行以下命令来实现:

```
firebase login 
```

一旦您登录，我们现在可以初始化我们的项目:

```
firebase init 
```

此操作将产生此提示，我们将在其中选择托管的**:**

![Firebase init prompt](img/898a7a52bf70e8261feb8e5064349874.png)

对于其余的提示，选择如下一个屏幕截图所示的选项:

![](img/df1e6c444845623919f242b0f0f44655.png)

提示完成后，Firebase CLI 会生成两个文件:

*   `.firebaserc`
*   `firebase.json`

***注:**`firebase.json`文件支持配置自定义托管行为。要了解这方面的更多信息，请访问 Firebase [全配置](https://firebase.google.com/docs/hosting/full-config)文档。*

如果 Firebase CLI 没有加载您的项目，您可以在生成的`.firebaserc`文件中手动添加项目 ID:

```
{
  "projects": {
    "default": "gatsby-site-43ac5"
  }
} 
```

这也是将新文件提交到我们的存储库并将代码推送到 GitHub 的好时机。

这样，我们就将代码连接到了 Firebase 项目，现在我们可以在开发环境中尝试手动部署了。

### 手动部署到 Firebase

手动部署的第一步是生成优化的生产版本。在我们的例子中，`gatsby`包含了我们，因为它默认包含了这个。要生成它，请运行以下命令:

```
gatsby build 
```

这将在`public`目录中生成一个优化的静态站点。这是我们将部署到 Firebase 主机的目录。要手动将`public`目录部署到 Firebase 主机，只需要一个命令:

```
firebase deploy 
```

如果一切按预期运行，Firebase 将部署我们的站点，并给我们一个已部署站点的 URL 链接。

![](img/bde8147199aac975993485707e011688.png)

您还会注意到 Firebase 创建了一个新的`.firebase`文件夹来存储它的缓存。由于我们不希望这个文件夹出现在我们的存储库中，我们可以将文件夹名添加到`.gitignore`文件中，这样 Git 就会忽略它。

在下一步中，我们将使用 CircleCI 自动化部署，这样我们就可以立即部署新的变更到存储库中。

## 圆形构型

要用 CircleCI 构建我们的项目，我们需要添加一个配置文件，指示 CircleCI 构建我们的 web 应用程序，并在每次修改代码时自动将其部署到 Firebase。

在我们项目的根文件夹中，创建一个名为`.circleci`的文件夹，并在其中创建一个`config.yml`文件。CircleCI 要求配置文件位于此处。

下面是我们将在项目中使用的配置文件:

```
# CircleCI Firebase Deployment Config
version: 2
jobs:
  build:
    docker:
      - image: circleci/node:10
    working_directory: ~/gatsby-site
    steps:
      - checkout
      - restore_cache:
          keys:
            # Find a cache corresponding to this specific package-lock.json
            - v1-npm-deps-{{ checksum "package-lock.json" }}
            # Fallback cache to be used
            - v1-npm-deps-
      - run:
          name: Install Dependencies
          command: npm install
      - save_cache:
          key: v1-npm-deps-{{ checksum "package-lock.json" }}
          paths:
            - ./node_modules
      - run:
          name: Gatsby Build
          command: npm run build
      - run:
          name: Firebase Deploy
          command: ./node_modules/.bin/firebase deploy --token "$FIREBASE_TOKEN" 
```

让我们快速回顾一下配置文件。

*   首先，`version`键使我们能够指定我们正在使用 CircleCI 2.0。
*   接下来，我们指定代码将在其中运行的基本 Docker 映像。在这种情况下，是一个基于`Node 10`的容器，这是撰写本文时的当前版本。如果有更新的版本，您可以使用。
*   `working_directory`选项指定了我们的代码将被克隆的位置。
*   接下来是`restore_cache`部分，它指示 CircleCI 恢复任何以前安装的依赖项。这里我们使用`package-lock.json`文件的校验和来检测是重新安装依赖项还是使用缓存来恢复之前下载的包。
*   下一步是通过`npm install`命令安装依赖项。
*   `save_cache`部分指示 CircleCI 在安装完依赖项后保存它们。
*   然后我们运行`Gatsby Build`命令。这将构建站点的优化生产版本，并准备好进行部署。
*   最后，我们运行`Firebase Deploy`命令，将我们的代码部署到 Firebase 主机。在这一步中，您会注意到我们需要一个 Firebase 令牌来允许将应用程序部署到我们的帐户。该命令指定应该从环境变量`FIREBASE_TOKEN`中获取令牌。我们一会儿会得到这个代币。

此外，请注意我们如何从本地安装的依赖项运行`firebase`命令的变化，而不是作为一个全局命令。如前所述，用 CircleCI 全局安装软件包可能是一个问题，所以我们在项目中本地安装我们需要的所有软件包。

### 集成 CircleCI 和 GitHub

我们现在有了一个配置文件，可以继续将 CircleCI 与我们之前创建的 GitHub 存储库集成。

*   如果您还没有，请在 [CircleCI](https://circleci.com/) 上创建一个帐户。
*   登录后，确保在左上角选择您的帐户。

![](img/ecae1fedb8adf74f8eb78712bbd9f0fc.png)


*   点击左侧工具条上的**添加项目**。
*   在下一页，搜索您的 GitHub 库的名称，然后点击它旁边的`Set Up Project`。

![](img/fdd82a39019c728b7f2d0a28abe64c09.png)


*   在下一页，有一个构建我们的项目所需的步骤列表，其中最重要的一个是添加 CircleCI 配置文件。因为我们的回购中已经有了这个文件，所以让我们一直滚动到底部，然后单击`Start Building`。

![](img/7c8b63f93e1726a816d74ba89da3229f.png)


我们的构建将最终开始运行，但是它在 Firebase 部署步骤中不出所料地失败了。😢![](img/f9de7d127c0d0bb82cf21f91e2092e51.png)

幸运的是，我知道部署失败的原因。这是因为我们还没有将 Firebase 部署令牌添加到 CircleCI 中。让我们在下一部分解决这个问题。

### 获取用于部署的 Firebase 登录令牌

在最后一步，我们将需要生成一个 Firebase 令牌，我们将使用它来允许访问我们的帐户。这个令牌将使 CircleCI 能够代表我们部署到 Firebase，因为我们不能在 CI 环境中使用 Firebase 的交互提示符登录。

在我们的本地开发环境中，让我们运行以下命令来生成令牌:

```
firebase login:ci 
```

这将打开一个浏览器窗口，提示您登录 Firebase 帐户。登录后，将会生成一个令牌。通过 web 浏览器进行身份验证后，您应该会得到类似以下的结果。

![Obtaining a Firebase token](img/23ee8e902acf1c4fe006d833b1d07617.png)

现在我们有了令牌，剩下的就是在 CircleCI 中将令牌作为一个[环境变量](https://circleci.com/docs/env-vars/#setting-an-environment-variable-in-a-project)添加，这样我们就可以在我们的项目中使用它了。我们的部署命令期望在`FIREBASE_TOKEN`环境变量中找到值。

### 将 Firebase 令牌添加到 CircleCI

我们需要采取以下步骤来添加令牌:

*   通过点按项目旁边的齿轮图标，转到项目的设置。
*   在**构建设置**部分下，点击**环境变量**。
*   点击**添加变量**。
*   在出现的模态中，在`name`字段中输入 **FIREBASE_TOKEN** ，在`value`字段中添加我们从 FIREBASE 获得的令牌，最后点击**添加变量**完成变量的添加。

![Adding an Environment variable to CircleCI](img/467e2b63ed14b5f0681d866b22e92f3b.png)


*   完成这一步后，我们现在可以通过点击 CircleCI 项目页面右侧的**重新运行工作流**来重新运行我们的构建。

![](img/6f6a5659b138e173c208432489f55fb0.png)


我们现在已经使用 CircleCI 成功地将我们的 web 应用程序部署到 Firebase 主机上了！🎉

![Firebase deployment successful](img/75d28aa375acfed7b32da27608a5b30d.png)

## 结论

我们对使用 CircleCI 将 web 应用程序部署到 Firebase 的探索到此结束。从现在开始，当我们对 Gatsby 站点进行更新并将这些更改推送到 GitHub 时，它们将自动部署到 Firebase 主机上。这真是一个伟大的组合。

这种方法适用于任何其他前端项目，并不是 Gatsby 特有的。Firebase 为 web 应用程序提供主机服务，CircleCI 帮助实现自动化并简化流程。前进并展开！🚀

有关这些技术的更多信息，请参见以下资源:

* * *

Kevin Ndung'u 是一名软件开发人员和开源爱好者，目前在 [Andela](https://andela.com/) 担任软件工程师。他热衷于通过博客帖子和开源代码分享他的知识。当不构建 web 应用程序时，您可以发现他在看足球比赛。

[阅读凯文·恩东乌的更多帖子](/blog/author/kevin-ndungu/)