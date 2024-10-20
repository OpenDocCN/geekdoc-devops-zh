# 持续的 Drupal:用 Docker、Git 和 Composer - CircleCI 维护一个 Drupal 网站

> 原文：<https://circleci.com/blog/continuous-drupal-p1-maintaining-with-docker-git-composer/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

很久以前，我曾经在 GoDaddy 共享主机上托管过一个 Drupal 网站，用 FTP 管理文件，每隔一段时间复制一次 MySQL 数据库作为“备份”。你能在那句话里找到多少错误？在 2017 年，有许多工具和最佳实践允许我们高效地维护 Drupal 站点，并跨团队成员以及基础设施进行扩展。现在使用这些工具和实践创建一个 Drupal 网站，可以加快使用 Drupal 进行开发的速度。此外，如果您决定以后在 Drupal 站点中实现 CI，那么用这个堆栈来设置您的站点将使这成为可能。

在这个由三部分组成的系列的第一篇文章中，我们将介绍如何使用 Docker、Git、Composer 和 Drush 来智能高效地维护 Drupal 8 网站。

## 使用的软件

需要以下软件和工具:

*   Apache(有图片)
*   作曲者(图像中可用)
*   docker(1 . 13 . 0 版或更新版本)
*   Docker 编写(1.10.0 版或更高版本)
*   Drupal 8(通过 Composer 安装)
*   Drush(通过 Composer 安装)
*   Git (v2.10 或更新版本)
*   MariaDB(或 MySQL，Drupal 8 通过 Docker 支持的版本)
*   PHP7(在图像中可用，技术上可以使用 PHP5，但是为什么 PHP7 要好得多)

### 在 Ubuntu 和其他基于 Debian 的发行版上安装

如果你用的是 Ubuntu，我建议 Ubuntu 16.04 或更新版本。

```
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install docker-ce git
sudo curl -L https://github.com/docker/compose/releases/download/1.16.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose 
```

### 在 macOS 上安装

我们假设您已经安装了 [Brew](https://brew.sh/) 。Docker 建议在 macOS 上使用他们的安装程序。这里是[稳定版](https://download.docker.com/mac/stable/Docker.dmg)的下载链接。这个安装程序还包括 Docker Compose。

```
brew install bash-completion git 
```

### 预构建的虚拟机

DrupalVM 是一个预构建的虚拟机，包括我们想要使用的一切以及更多。本指南中的许多步骤可以跳过，因为 DrupalVM 会为您完成这些步骤。如果你仍然想了解工具选择背后的原因或者内部工作原理，通读这篇文章仍然是有用的。

## 设置项目目录

出于本文的目的，我们将创建一个虚构的基于 Drupal 的博客，名为“持续博客”。首先，我们需要创建一个根目录来保存我们的整个项目。这个目录将使用 Git 进行版本控制。我更喜欢把我所有基于 GitHub 的回购放在一个`Repos`目录下，然后放在 GitHub 用户名下。

```
mkdir -p ~/Repos/circleci/continuous-blog
cd ~/Repos/circleci/continuous-blog 
```

### 创建 Dockerfile 文件

现在，在开始安装 Drupal 之前，我们需要创建一些东西。第一个是 docker 文件，它将包含我们实际的 Drupal 网站。您可以将下面的`Dockerfile`复制粘贴到`./Dockerfile`中，这是我们项目的根。

```
FROM drupal:8.4-apache

RUN apt-get update && apt-get install -y \
	curl \
	git \
	mysql-client \
	vim \
	wget

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
	php composer-setup.php && \
	mv composer.phar /usr/local/bin/composer && \
	php -r "unlink('composer-setup.php');"

RUN wget -O drush.phar https://github.com/drush-ops/drush-launcher/releases/download/0.4.2/drush.phar && \
	chmod +x drush.phar && \
	mv drush.phar /usr/local/bin/drush

RUN rm -rf /var/www/html/*

COPY apache-drupal.conf /etc/apache2/sites-enabled/000-default.conf

WORKDIR /app 
```

#### 走过码头文件

```
FROM drupal:8.4-apache 
```

我们从官方的 Docker 库 Drupal 图像开始。这意味着我们不需要自己设置 Apache 和 PHP。更具体地说，我们不必去寻找和安装 Drupal 需要的特定 PHP 插件。为 Drupal 的最新版本使用一个标签很重要，但是它不需要特别是您想要运行的 Drupal 版本。这是因为我们不打算使用图像中提供的 Drupal 文件，而是来自 Composer(在本文后面)。

```
RUN apt-get update && apt-get install -y \
	curl \
	git \
	mysql-client \
	vim \
	wget 
```

我们正在映像中安装一些我们需要的工具，以进行后续步骤。看起来是一个奇怪的选择，但是如果没有它，Drush 将无法正确连接到我们网站的数据库。提示，我们用多行、单个`RUN`步骤创建这样的命令，因为它改进了 Docker 层缓存。

```
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
	php composer-setup.php && \
	mv composer.phar /usr/local/bin/composer && \
	php -r "unlink('composer-setup.php');" 
```

在这里我们安装 Composer，*PHP 依赖管理器。您或 Docker 映像尚未安装的所有内容都将由 Composer 处理。为用户`root`安装了 Composer。您会看到一个又一个警告，提示您不应该在 Composer 中使用 root 用户。我们在 Docker 映像中工作，其中唯一的常规用户是`root`用户。因此，尽管有警告，在这种情况下这是没问题的。在你自己的电脑上，不要使用`root`用户。*

```
RUN wget -O drush.phar https://github.com/drush-ops/drush-launcher/releases/download/0.4.2/drush.phar && \
	chmod +x drush.phar && \
	mv drush.phar /usr/local/bin/drush 
```

我们正在安装 [Drush 发射器](https://github.com/drush-ops/drush-launcher)。使在命令行上使用 Drush 变得更加容易，因为不再建议全局安装 Drush。

```
RUN rm -rf /var/www/html/* 
```

这将删除 Docker 映像附带的 Drupal 副本。如果你愿意，你可以使用它，但是在这篇文章中，我们在 Git 的帮助下用 Composer 安装和跟踪 Drupal。

```
COPY apache-drupal.conf /etc/apache2/sites-enabled/000-default.conf 
```

复制定制的 Apache VirtualHost 配置文件，告诉 Apache 我们希望在文件系统中的什么位置托管我们的网站。

### 创建 Apache VHost 配置

我们刚刚提到的 Apache VirtualHost 配置文件，让我们创建它。这个文件应该位于`./apache-drupal.conf`。

```
<VirtualHost *:80>
	ServerAdmin webmaster@localhost
	DocumentRoot /app/web

	<Directory /app/web>
		AllowOverride All
		Require all granted
	</Directory>

	ErrorLog ${APACHE_LOG_DIR}/error.log
	CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
# vim: syntax=apache ts=4 sw=4 sts=4 sr noet 
```

### 创建 Docker 合成文件

我们的 Drupal 站点将由两个 Docker 图像组成。包含 Drupal 特定内容的主映像，我们用自己制作的`Dockerfile`来指定，还有一个数据库映像。Docker Compose 将允许使用这两个图像创建容器，并将它们相互连接，以便它们“正常工作”。

Docker Compose 文件也应该创建在我们的项目的根目录下，`./docker-compose.yml`。

```
version: '3'
services:
  db:
    image: mariadb:10.2
    environment:
      MYSQL_DATABASE: drupal
      MYSQL_ROOT_PASSWORD: AppleSucks
    volumes:
      - db_data:/var/lib/mysql
    restart: always
  drupal:
    depends_on:
      - db
    build: .
    ports:
      - "8080:80"
    volumes:
      - ./app:/app
    restart: always
volumes:
  db_data: 
```

#### 浏览 Docker 撰写文件

```
version: '3' 
```

互联网上许多带有示例撰写文件的博客帖子将使用版本 2。我们正在使用 Docker Compose 的新功能，因此需要版本`3`或更高。

```
services:
  db:
    image: mariadb:10.2
    environment:
      MYSQL_DATABASE: drupal
      MYSQL_ROOT_PASSWORD: WaterfallSucks 
```

我们在这里创建一个名为“db”的容器，它是由 MariaDB v10.2 Docker 映像制成的。可以使用 MySQL 和 Postgress，只要是 Drupal 支持的。我们还告诉 MariaDB 在启动时创建一个名为“drupal”的新数据库，并将 MariaDB `root`用户的密码设置为“WaterfallSucks”。随意使用您喜欢的任何数据库名称和密码。MariaDB 的 [Docker Hub 页面提供了关于可用环境变量的更多信息。](https://hub.docker.com/_/mariadb/)

```
 volumes:
      - db_data:/var/lib/mysql 
```

我们指定想要使用一个名为`db_data`的 Docker 卷来存储`/var/lib/mysql`的内容。这个目录是 MariaDB 存储它所包含的数据库信息的地方。它允许我们关闭容器并在以后再次启动它们，而不会丢失保存在数据库中的任何数据。

```
 drupal:
    depends_on:
      - db 
```

我们指定了第二个名为 Drupal 的容器。`depends_on`键告诉 Docker Compose 在`db`容器启动之前不要启动这个容器。最好不要启动 Drupal，因为数据库是存在的。

```
 build: . 
```

这告诉 Docker Compose 使用本地 Docker 文件作为我们图像的基础，而不是像我们对 DB 那样使用`image`然后指定 Docker 图像。这意味着我们可以在不运行单独命令的情况下进行动态构建，或者更糟的是，推到公共 Docker 存储库中进行快速开发更改。

```
 ports:
      - "8080:80" 
```

我们将本地机器上的端口 8080 映射到 Docker 容器中的端口 80。这允许我们在浏览器中访问`http://localhost:8080`,并在容器中查看我们正在运行的站点。端口 8080 可以是本地的任何可用端口。

```
 volumes:
      - ./app:/app 
```

我们在这里创建绑定装载，而不是传统的卷。这将获取本地机器上存储库中的`./app`目录，并通过 Docker 容器使其作为`/app`可用。这就是我们如何将 Drupal 站点以及任何主题和模块提供到容器中。

```
volumes:
  db_data: 
```

这告诉 Docker Compose 创建我们前面在配置中指定的卷。

### 创建应用程序目录

这是最难的一步，在我们项目的根目录下创建一个`app`目录。

```
mkdir app 
```

哇哇。

### 用 Git 保存我们的进度

这时，我们可以创建我们的 Git repo 并保存我们的进度。现在这样做是不必要的，但是它允许我们在接下来的几个步骤中可视化文件系统如何变化。

```
git init .
git add .
git commit -m "Initial commit." 
```

## 构建 Drupal 8 网站

随着最初的脚手架的方式，我们可以继续让我们的实际网站建设。我们首先用 Docker Compose 打开我们的容器。

```
docker-compose up -d --build 
```

`--build`告诉 Docker Compose 在我们运行这个命令时构建我们的`Dockerfile`。我们需要登录到我们的主容器来运行 Composer，但是我们需要容器名来这样做。我们可以通过运行`docker-compose ps`找到它。连接到端口 80 的容器是正确的。下面是一个输出示例:

```
$ docker-compose ps

 Name                        Command               State          Ports        
 ---------------------------------------------------------------------------------------
 continuousblog_db_1       docker-entrypoint.sh mysqld      Up      3306/tcp            
 continuousblog_drupal_1   docker-php-entrypoint apac ...   Up      0.0.0.0:8080->80/tcp 
```

这里正确的容器名是`continuousblog_drupal_1`。我们使用`docker exec`命令登录它。

```
docker exec -it continuousblog_drupal_1 bash 
```

这将把您放入位于`/app`目录的容器中。现在我们可以使用`composer`来安装 Drupal。

```
/app #  composer create-project drupal-composer/drupal-project:8.x-dev /app --stability dev --no-interaction
/app #  mkdir -p /app/config/sync
/app #  chown -R www-data:www-data /app/web 
```

现在在您的浏览器中访问 http://localhost:8080 并浏览 Drupal 安装程序。

一旦所有的东西都安装好了，你的站点就可以运行了，我们将使用 Drush 来导出你的 Drupal 配置。然后，退出容器。

```
/app #  drush config-export
/app #  exit 
```

我们将编辑现有的`.gitignore`文件，删除第 11 行和第 15 行。这些行忽略了`settings.php`文件和`*/files/*`目录。有些人会说不要这样做，在某些情况下他们是对的。为了简单起见，我们将在这里对`files`目录进行版本化，我们正在对`settings.php`进行版本化。如果您的回购将要公开，请不要对此文件进行版本控制，因为每个人都将拥有您的数据库凭据。

运行`git status`将显示 Composer 安装的所有内容。我们可以用 Git 提交所有东西，这样就完成了。我们现在已经保存了 Drupal 网站的初始状态。

## 从这里去哪里？

我们现在有了一个用 Git 和 Composer 跟踪的基本 Drupal 8 网站。我们甚至可以使用 Docker Compose 在任何位置启动和运行站点(如果我们还导出和导入数据库)。

我们本系列的下一篇博文将会介绍如何让我们的 Drupal 网站在 CircleCI 上的持续集成工作流中，在我们将网站发布到生产环境之前做一些基本的测试。

与此同时，您还可以使用我们的设置做其他事情。

### 更新 Drupal 8 核心

我建议在更新 Core 之前创建一个新的 Git 分支。Drupal 8 核心可以更新如下:

```
git checkout -b update-drupal
docker exec -it continuousblog_drupal_1 bash
/app #  composer update drupal/core --with-dependencies
/app #  drush updb  # updates the DB with any schema changes
/app #  git diff    # to check for changes
/app #  git status  # also to help review changes 
```

在浏览器中测试我们的站点(在我们的下一篇文章中，使用 CI ),以确保一切按预期运行。

```
/app #  exit
git add .
git commit -m "Updated Drupal Core."
git push 
```

### 安装“Contrib”模块或主题

“Contrib”模块是由某人发布或“贡献”到 Drupal.org 上的 Drupal 官方注册中心的公共模块。这些模块也可以用 Git 和 Composer 安装和跟踪。在这里，我们将安装 Pathauto Drupal 模块。

```
git checkout -b install-pathauto
docker exec -it continuousblog_drupal_1 bash
/app #  composer require drupal/pathauto
/app #  drush en pathauto -y  # enables the module with Drush rather than visiting the Drupal Admin page 
```

在浏览器中测试我们的站点(在我们的下一篇文章中，使用 CI ),以确保一切按预期运行。如果您为新模块做了任何配置，不要忘记用 Drush 导出您的配置。

```
/app #  drush config-export  # if you made config changes
/app #  exit
git add .
git commit -m "Installed Pathauto."
git push 
```

### 更新 Drupal“Contrib”模块和主题

Contrib 模块可以像 Core 一样更新。在任何时候，您都可以运行`composer outdated`来查看您的 Drupal 站点中有可用更新的每一部分。

```
git checkout -b update-pathauto
docker exec -it continuousblog_drupal_1 bash
/app #  composer update drupal/pathauto --with-dependencies
/app #  drush updb  # updates the DB with any schema changes 
```

在浏览器中测试我们的站点(在我们的下一篇文章中，使用 CI ),以确保一切按预期运行。

```
/app #  exit
git add .
git commit -m "Updated Pathauto."
git push 
```

### 维护定制的 Drupal 模块和主题

有两种方法可以做到。如果您有一个特定于该站点的模块，那么您可以将所有代码保存在同一个 Git repo 中，并在这里进行维护。但是考虑一下这一点，因为很多时候模块可以服务于同一公司内的多个站点，或者最终获得开源，这总是一件很棒的事情。主题更可能是特定于站点的，但同样的警告也适用。

对于不特定于站点的模块和主题，您可能会将它们保存在自己的存储库中，并作为子模块添加到您的 Drupal 站点，而不是在这个 repo 中维护代码。

如果您这样做了，那么每当您签出 Drupal 8 站点的存储库时，您将希望运行以下命令来确保您的定制模块也得到签出:

```
git submodule sync
git submodule update --init 
```

要更新它们，你需要`cd`进入它们的目录，然后运行普通的`git pull`或`git checkout <tagname>`。然后，备份到这个存储库的根目录，并提交您的更改。

## 问题&保持这篇文章的更新

如果你有问题，或者对这篇文章有什么补充，请通过 [CircleCI 讨论](https://discuss.circleci.com)让我们知道。*