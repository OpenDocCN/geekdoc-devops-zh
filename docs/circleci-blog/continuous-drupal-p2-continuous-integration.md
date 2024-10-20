# 连续 Drupal 第二部分:Drupal 8 CI - CircleCI

> 原文：<https://circleci.com/blog/continuous-drupal-p2-continuous-integration/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

我们正在学习如何更快地使用 Drupal，将变更从开发人员的笔记本电脑快速转移到生产中，同时保持(如果不是提高)质量。在这个*连续的 Drupal* 系列的[第 1 部分](https://circleci.com/blog/continuous-drupal-p1-maintaining-with-docker-git-composer/)中，我们介绍了如何使用现代工具集来启动和运行 Drupal。在这篇文章中，我们将继续第 1 部分的内容，把我们的 Drupal 网站带入 CircleCI。我们将在网站上运行 QA 测试，以确保一切按预期运行，并可选地对我们可能有的定制模块或主题进行 PHPUnit 测试。让我们开始吧。

现在我们有了一个用 Git 和 Composer 管理的 Drupal 网站，我们可以使用 CircleCI、Behat 和 PHPUnit 等工具来自动测试我们的网站。

## 用 Behat 测试 Drupal 8 网站

Behat 允许用一种近似英语的语言来描述某些基于 UI 的特性应该如何工作。如果你熟悉“用户故事”的概念，Behat 让我们以一种可以自动测试的方式用代码编写用户故事，这被称为行为驱动开发。

### 安装行为

我们将安装 Behat 和相关工具。我们希望在 Drupal 容器内的`app`目录中完成这项工作。

```
docker-compose ps  # to get our container name
docker exec -it continuousblog_drupal_1 bash
composer require --dev behat/behat
composer require --dev behat/mink
composer require --dev behat/mink-extension
composer require --dev drupal/drupal-extension 
```

创建一个`behat.yml`文件:

```
default:
  suites:
    default:
      contexts:
        - FeatureContext
        - Drupal\DrupalExtension\Context\DrupalContext
        - Drupal\DrupalExtension\Context\MinkContext
  extensions:
    Behat\MinkExtension:
      goutte: ~
      selenium2: ~
      base_url: http://localhost/
    Drupal\DrupalExtension:
      blackbox: ~
      api_driver: 'drupal' 
      drupal: 
        drupal_root: 'web' 
```

然后我们将让 Behat 初始化并运行`-dl`来查看定义列表。能够查看此列表可以确认所有安装都是正确的。

```
vendor/bin/behat --init
vendor/bin/behat -dl 
```

### 测试用户行为

我们的 Behat 测试将放在容器中的`/app/features/`目录中。我们将创建一个快速的通用测试，以确保我们的系统在 demo 上工作，就像测试工作流一样。

创建文件`/app/features/global.feature`:

```
Feature: Test Features
	Some test scenarios to make sure the website is generally working.

@api
  Scenario: Run cron
    Given I am logged in as a user with the "administrator" role
    When I run cron
    And am on "admin/reports/dblog"
    Then I should see the link "Cron run completed" 
```

现在，我们可以使用以下工具运行我们的 Behat 测试:

```
vendor/bin/behat

Feature: Test Features                   
  Test scenarios to make sure the website is generally working.                      

  @api                                    
  Scenario: Run cron                                             # features/global.feature:4                                                                              
    Given I am logged in as a user with the "administrator" role # Drupal\DrupalExtension\Context\DrupalContext::assertAuthenticatedByRole()                              
    When I run cron                                              # Drupal\DrupalExtension\Context\DrupalContext::assertCron()                                             
    And am on "admin/reports/dblog"                              # Drupal\DrupalExtension\Context\MinkContext::visit()                                                    
    Then I should see the link "Cron run completed"              # Drupal\DrupalExtension\Context\MinkContext::assertLinkVisible()                                        

1 scenario (1 passed)                     
4 steps (4 passed)                        
0m1.25s (13.18Mb) 
```

至此，我们有了一个使用 Behat 测试 Drupal 的基本示例。下一步是将所有这些连接到 CircleCI。在我们这样做之前，让我们用 Git 保存我们的进度。

```
exit  # to exit the container
git status  # you should recognize everything outside of the */files/* directory
git add .
git commit -m "Configure Behat and first test." 
```

## 圆形构型

现在我们要在 CircleCI 上建立我们的 Drupal 网站。如果这是你第一次使用 CircleCI，请先阅读[入门文档](https://circleci.com/docs/about-circleci/)。

CircleCI 配置文件应该位于`.circleci/config.yml`处，我们的配置文件应该如下所示:

```
version: 2
jobs:
  build:
    docker:
      - image: cibuilds/drupal:latest
      - image: mariadb:10.2
        environment:
          MYSQL_DATABASE: drupal
          MYSQL_ROOT_PASSWORD: HorriblePassword
          MYSQL_ROOT_HOST: "%"
    environment:
      BLUEMIX_ORG: CircleCI
      BLUEMIX_SPACE: dev
    working_directory: /project/app
    steps:
      - checkout:
          path: /project
      - run:
          name: "Setup Database & Server"
          command: |
            echo "127.0.0.1      db" >> /etc/hosts
            sleep 20
            mysql -uroot -pHorriblePassword -h127.0.0.1 drupal < ../db/drupal-db-dump.sql
            service apache2 start
      - run:
          name: "Install Dependencies"
          command: |
            composer install
      - run:
          name: "Behat Tests"
          command: vendor/bin/behat 
```

与我们在上一篇博文中使用 Docker Compose 不同，我们使用 CircleCI 及其“Docker Executor”来创建我们的执行环境。我们将在本系列的第 3 部分对 CircleCI 配置进行演练。现在，我只想简单介绍一下“设置数据库&服务器”这一步。

### 管理数据库

当管理多个环境时，您可能有多个数据库位置。生产、 [CI](https://circleci.com/continuous-integration/) 、试运行、开发等各一个。对于某些环境(比如 dev ),您可以使用一个轻量级的假 DB。很多时候你需要处理真实的数据。这是通过复制(转储)数据库并将它们导入到其他环境中来实现的。关键是数据流动的方向。

#### 总是将数据库数据向下游移动

这是足够重要的，它保证自己的标题。当移动数据库数据的拷贝时，总是从拷贝生产数据开始。然后，您可以将它导入暂存或其他地方。然后，您甚至可以将暂存数据移动到本地计算机上。这种做法可以防止数据丢失。如果您从本地机器上的一个 DB 开始，并将其转移到生产环境中，那么您可能会丢失自上次创建本地 dev DB 以来创建的用户、帖子等。

`echo "127.0.0.1 db" >> /etc/hosts` -我们的 Drupal 设置文件知道我们之前设置的数据库主机名是`db`。这一行总是在 CircleCI 容器中工作主机名。

#### 跟踪数据库导出

对于这个例子，我们的 repo `./db`根目录下的目录被创建来保存`.sql`格式的 DB 转储。我们本地数据库里有一个来自`mysqldump`的转储。一旦您的网站投入生产，您将希望从那里检索数据库转储。对于拥有敏感数据的站点来说，在将数据库从生产环境导入到其他环境之前对其进行清理是一种常见的做法。尤其是当这些额外的环境可能不太安全时。如果数据库转储中仍然有敏感信息，您可能不想像我们在这里做的那样用 Git 跟踪它。

## 最后，托管我们的 Drupal 网站

在“持续的 Drupal”系列的第三部分，我们将使用 Kubernetes 找到一个托管 Drupal 网站的地方，随着我们的网站越来越受欢迎，我们可以扩展托管基础设施。

## 问题&保持这篇文章的更新

如果你有问题，或者对这篇文章有什么补充，请通过 [CircleCI 讨论](https://discuss.circleci.com)让我们知道。