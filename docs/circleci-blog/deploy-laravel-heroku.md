# 将 Laravel 应用程序部署到 Heroku | CircleCI

> 原文：<https://circleci.com/blog/deploy-laravel-heroku/>

> 本教程涵盖:
> 
> 1.  克隆 Laravel 应用程序并在本地运行它
> 2.  在 Heroku 上创建一个应用程序，包括 ClearDB MySQL 插件
> 3.  为将 Laravel 应用程序部署到 Heroku 创建管道配置

在本教程中，我将向您展示如何建立一个连续部署管道，以最少的麻烦将 Laravel 应用程序部署到 Heroku 平台。自动化部署有助于团队限制部署过程中的人工干预，降低错误风险，并简化整个软件发布过程。

## 先决条件

对于这篇文章，你需要:

> 我们的教程是平台无关的，但是使用 CircleCI 作为例子。如果你没有 CircleCI 账号，请在 注册一个免费的 [**。**](https://circleci.com/signup/)

## 入门指南

首先，克隆一个用 Laravel 构建的作者应用程序。使用这个应用程序，您可以通过陈述作者的`name`、`email`、`github`用户名和`location`来创建一个作者列表。从终端输入以下命令下载项目:

```
git clone --single-branch --branch template https://github.com/CIRCLECI-GWP/laravel-heroku-app.git laravel-heroku-app 
```

进入应用程序，安装其依赖项并生成应用程序密钥:

```
cd laravel-heroku-app

composer install 
```

接下来，创建一个`.env`文件，并使用以下命令生成应用程序密钥:

```
cp .env.example .env

php artisan key:generate 
```

## 在本地运行应用程序

在本地运行应用程序可以确认它是否按预期工作。首先，更新数据库凭证:

```
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=YOUR_DATABASE_NAME
DB_USERNAME=YOUR_DATABASE_USERNAME
DB_PASSWORD=YOUR_DATABASE_PASSWORD 
```

用您计划使用的值替换`YOUR_DATABASE_NAME`、`YOUR_DATABASE_USERNAME`和`YOUR_DATABASE_PASSWORD`。

接下来，创建数据库表并运行应用程序:

```
php artisan migrate

php artisan serve 
```

进入`http://localhost:8000`查看应用主页。

![Application homepage](img/16542a1263b275cd5b113db8cc190b72.png)

点击**作者**查看作者列表。如果没有创建作者，列标签下面的空间将是空白的。

![View authors page](img/c74d023d456cd04e81c1b6c310e87e28.png)

点击**创建新作者**添加新作者。

![Add authors page](img/00f0de4cea7124328367925e5c8d7b3e.png)

输入详细信息，点击**提交**。

![Author created](img/00832289cee609ea1de43cedd34a14f3.png)

既然您已经确认了演示应用程序可以工作，那么您就可以开始设置 Heroku 的部署了。

## 在 Heroku 上创建应用程序

要在 Heroku 上创建应用程序，请转到 [Heroku 仪表板](https://dashboard.heroku.com/login)。点击**新增**，然后点击**新增 App** 。在表单中填写您的应用程序和您所在地区的名称。

![Create Heroku app](img/5c1a57a6220343f267195ea66749e1fd.png)

然后，点击**创建 app** 。您将被重定向到新创建的应用程序的**部署**视图。

## 添加 ClearDB MySQL 附加组件

默认情况下， [Laravel 为五个数据库](https://laravel.com/docs/9.x/database)提供第一方支持。为了保持一致性，最好在 Heroku 上维护与本地相同的 MySQL 数据库。Heroku 默认使用 PostgreSQL，但是因为这个教程项目使用 MySQL，所以您需要安装一个名为 [ClearDB](https://elements.heroku.com/addons/cleardb#pricing) 的 MySQL 附加组件。对于本教程来说，免费计划应该足够好。

要安装 ClearDB，点击 **Resources，**搜索 ClearDB，并选择它。

![Add database](img/b1b4bdc911af704fcd0c97bcf52dc8b9.png)

选择免费计划，然后点击**提交订单**。

![Submit database form](img/71c3a4398030fe71ff1ff2c4d9d66557.png)

一旦该过程完成，ClearDB 将生成一个连接字符串，并将其包含在应用程序的配置变量中。

要更新应用程序的其他环境变量，点击**设置**，然后点击**显示配置变量**。复制`CLEARDB_DATABASE_URL`并粘贴到您可以轻松提取凭证的地方。

数据库 URL 应该是这样的格式:`mysql://YOUR_DB_USERNAME:YOUR_DB_PASSWORD@YOUR_DB_HOST/YOUR_DB_NAME?reconnect=true`。使用生成的数据库 URL，添加以下变量:

*   `APP_DEBUG`打开和关闭 Laravel 应用程序的调试模式。
*   `APP_ENV`表示应用程序是本地的还是生产中的。
*   `APP_KEY`是生成的应用密钥。您可以使用本地机器上的副本。
*   `APP_NAME`指定应用程序名称。
*   `APP_URL`用于说明您的应用程序的 URL。在本例中，它是我们的 Heroku 应用程序的 URL。

![Create env variables](img/0452184b721ab592f63d8ceb073390e2.png)

现在您需要一个 API 密钥。您将使用此密钥和应用程序名称将您的 CircleCI 管道连接到 Heroku。要获取您的 API 密钥，请打开[帐户设置](https://dashboard.heroku.com/account)页面。滚动到 **API 键**部分。

![Account settings](img/3a9367553147c96e786564fa4fd1271b.png)

点击**显示**按钮，复制 API 密钥。将它保存在您以后可以轻松找到的地方。

## 添加 CircleCI 配置文件

在本部分教程中，我将指导您为 CircleCI 添加管道配置。对于这个项目，管道由将我们的应用程序部署到 Heroku 的步骤组成。

在项目的根目录下，创建一个名为`.circleci`的文件夹。在该文件夹中，创建一个名为`config.yml`的文件。在新创建的文件中，添加以下配置:

```
version: 2.1
orbs:
  heroku: circleci/heroku@1.2.6
jobs:
  build:
    docker:
      - image: cimg/php:8.0

    steps:
      - checkout
      # Download and cache dependencies
      - restore_cache:
          keys:
            - v1-dependencies-{{ checksum "composer.json" }}
      - run:
          name: "Install Dependencies"
          command: composer install -n --prefer-dist

      - save_cache:
          key: v1-dependencies-{{ checksum "composer.json" }}
          paths:
            - ./vendor
  deploy_app:
    executor: heroku/default
    steps:
      - checkout
      - heroku/install
      - heroku/deploy-via-git:
          force: true
      - run:
          command: |
            heroku run --app=${HEROKU_APP_NAME} php artisan migrate --force
workflows:
  deploy:
    jobs:
      - build
      - deploy_app 
```

这个配置文件为项目指定了 CircleCI 的版本。在撰写本文时，`orbs`键调用最新版本的 [Heroku orb](https://circleci.com/developer/orbs/orb/circleci/heroku) 。这个 [orb](https://circleci.com/orbs/) 抽象了设置 Heroku CLI 时通常涉及的复杂性。orb 会自动安装它，并使用它将应用程序部署到 Heroku。

`build`任务从 [CircleCI Docker Hub](https://hub.docker.com/u/cimg) 注册中心获取 [PHP Docker 图像](https://hub.docker.com/r/cimg/php)。您的代码从存储库中签出，并且安装了项目的依赖项。如果为您的项目配置了测试，这将是运行测试的好地方。我们将跳过本教程的测试，只关注 Heroku 平台的部署。

`deploy_app`作业使用`heroku/default`执行器来检查项目并安装 Heroku CLI。部署应用程序并运行迁移命令，以便在 Heroku 上为您的应用程序创建数据库。

**注意:** *仅运行一次迁移命令(在第一次部署时)。然后编辑配置文件以删除 migrate 命令。*

## 创建配置文件

[Procfile](https://devcenter.heroku.com/articles/procfile) 帮助定义启动应用程序时应该执行的过程类型和命令。对于 PHP 运行时，您可以选择使用 [Apache2](https://httpd.apache.org/) 或 Nginx 作为 web 服务器来运行这个应用程序。对于本教程，我们将使用 Apache2 来服务我们的应用程序。要设置它，导航到您的项目的根目录，创建一个名为`Procfile`的新文件，并用以下内容填充它:

```
web: vendor/bin/heroku-php-apache2 public/ 
```

这个命令将 web 服务器设置为 Apache，并指定应用程序的服务文件夹。对于这个项目，它是`public`文件夹。

接下来，在 GitHub 上建立一个存储库，并将项目链接到 CircleCI。查看[将您的项目推送到 GitHub](https://circleci.com/blog/pushing-a-project-to-github/) 以获得逐步说明。

登录您的 CircleCI 帐户。如果你注册了你的 GitHub 账户，你所有的库都可以在你项目的仪表盘上看到。

点击**为您的`laravel-heroku-app`项目设置项目**。

![Set up project](img/ec39ff756e9e2a0932c969d3714f5233.png)

系统将提示您编写新的配置文件，或者在项目中使用现有的配置文件。选择现有选项。在 GitHub 上输入您的代码所在的分支的名称，然后点击 **Let's Go** 按钮。

![Select configuration file](img/911c8171f648dcffc21b8f408a029bf8.png)

您的第一个工作流将开始运行，但会失败。

![Pipeline failed](img/6bc5769348cdd7631f3865b90b54bb0c.png)

此部署过程失败，因为您尚未提供 Heroku API 密钥。你现在可以弥补了。点击**项目设置**按钮，然后点击**环境变量**。添加这两个新变量:

*   `HEROKU_APP_NAME`是 Heroku 中的 app 名称(`laravel-heroku-application`)。
*   `HEROKU_API_KEY`是您从帐户设置页面获取的 Heroku API 密钥。

从失败的中选择**重新运行工作流以重新运行 Heroku 部署。这一次，您的工作流将成功运行。**

您可以从应用的迁移中确认这一点。

![Applied migrations](img/4218a3c4e97865b237ca7b7d90da89fe.png)

要确认工作流成功，请在浏览器中打开新部署的应用程序。您的应用程序的 URL 应该是这样的格式`https://<HEROKU_APP_NAME>.herokuapp.com/`

![Application live on Heroku](img/7853aa1542ad00ca1848eedd3d9f4034.png)

## 结论

在本教程中，您已经学习了使用 CircleCI 和 Heroku 为 Laravel 应用程序设置连续部署管道所需的步骤和过程。使用这个管道可以确保特性部署的质量，并大大降低影响生产环境的人为错误的风险。

我希望本教程对你有所帮助。[点击 GitHub](https://github.com/yemiwebby/new-heroku-app) 查看这里构建的项目的完整源代码。下次见，好好享受吧！

* * *

[Oluyemi](https://twitter.com/yemiwebby) 是一名拥有电信工程背景的技术爱好者。出于对解决用户日常遇到的问题的浓厚兴趣，他冒险进入编程领域，并从那时起将他解决问题的技能用于构建 web 和移动软件。Oluyemi 是一名热衷于分享知识的全栈软件工程师，他在世界各地的几个博客上发表了大量技术文章和博客文章。由于精通技术，他的爱好包括尝试新的编程语言和框架。

* * *

Oluyemi 是一名拥有电信工程背景的技术爱好者。出于对解决用户日常遇到的问题的浓厚兴趣，他冒险进入编程领域，并从那时起将他的问题解决技能用于构建 web 和移动软件。Oluyemi 是一名热衷于分享知识的全栈软件工程师，他在世界各地的几个博客上发表了大量技术文章和博客文章。作为技术专家，他的爱好包括尝试新的编程语言和框架。

[阅读更多 Olususi Oluyemi 的帖子](/blog/author/olususi-oluyemi/)