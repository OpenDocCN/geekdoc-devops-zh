# Symfony 应用程序与 Behat | CircleCI 的持续集成

> 原文：<https://circleci.com/blog/continuous-integration-symfony-behat/>

[Behat](https://docs.behat.org) 是一个支持[行为驱动开发](https://en.wikipedia.org/wiki/Behavior-driven_development)的开源测试框架。专注于需求沟通，它以帮助工程师构建伟大的系统而闻名，而不是构建系统并测试它们的伟大。Symfony 仍然是顶级的 PHP 框架之一。它是不可知的，允许你使用任何测试框架。在本教程中，我们将为 Symfony 应用程序建立一个[持续集成](https://circleci.com/continuous-integration/)管道，并对 Behat 提供的功能进行测试。该应用程序将返回一个客户列表。为了简单起见，我们不与数据库交互。相反，我们将硬编码客户的详细信息。

按时间顺序，我们将:

1.  创建新的 [Symfony](https://symfony.com/) 应用程序
2.  通过 Composer 安装 Behat，并在我们的应用程序中初始化它
3.  创建 GitHub 存储库
4.  用默认数据创建一个端点，并为其编写一个测试
5.  在本地运行测试，然后配置 CircleCI 使其自动化

## 先决条件

为了成功实现本教程的目标，您需要以下内容:

## 安装 Symfony 应用程序

使用 Composer，通过运行以下命令创建新的 Symfony 应用程序:

```
composer create-project symfony/website-skeleton symfony-behat 
```

一旦安装过程完成，您将拥有一个新的应用程序，它的所有依赖项都安装在一个名为`symfony-behat`的文件夹中。

进入项目文件夹，安装所需的工具，以便通过运行以下命令对应用程序进行功能测试:

```
// move into project
cd symfony-behat

// install web server and Behat
composer require behat/behat symfony/web-server-bundle --dev ^4.4.2 
```

上面的最后一个命令将安装:

*   `behat/behat`:Behat 的最新版本
*   `symfony/web-server-bundle`:本地运行 Symfony 应用程序的网络服务器

## 初始化行为

安装 Behat 后，要做的第一件事是在我们的应用程序中初始化它。这是至关重要的。它附带了一个样板文件，我们可以在上面构建，它配置了测试套件，将告诉 Behat 在哪里找到以及如何测试我们的应用程序。使用以下命令初始化:

```
vendor/bin/behat --init 
```

您将看到以下输出:

```
+d features - place your *.feature files here
+d features/bootstrap - place your context classes here
+f features/bootstrap/FeatureContext.php - place your definitions, transformations and hooks here 
```

Behat 创建了一个`features`目录来保存特性的测试脚本。它还会在`features/bootstrap`文件夹中创建一个`FeatureContext`类。

### 行为中的一些重要概念

如果您是 Behat 的新手，以下定义会很有帮助:

*   特性(Feature):表示一个功能单元的文件，包括定义、场景和步骤，以便于测试特定的功能。
*   **场景**:重新创建条件和用户行为模式的步骤集合。
*   第**步**:设置先决条件、触发事件或断言应用程序的简单语言模式。步骤负责站点的真实行为。
*   **关键字**:一组特定的单词，用作步骤模式的开始，以提高可读性，并将步骤分组为前提条件、动作和断言，例如`Given`、`When`、`Then`。
*   **Context** :这个类提供了与应用程序交互的新方法。这主要意味着提供额外的步骤。

## 为客户端点创建特征文件

我们的应用程序的一个预期特性是，任何用户都应该能够以未经身份验证的用户身份访问`customer`端点，然后能够毫无问题地查看客户列表。这是特性的故事，在这一节中，我们将创建一个特性文件，详细说明我们希望`customer`端点如何工作。导航到`features`文件夹，并在其中创建一个名为`customer.feature`的文件。将以下内容粘贴到新文件中:

```
Feature: List of customers
  In order to retrieve the list of customers
  As a user
  I must visit the customers page

  Scenario: I want a list of customers
    Given I am an unauthenticated user
    When I request a list of customers from "http://localhost:8000/customer"
    Then The results should include a customer with ID "1" 
```

Behat 用来描述应用程序中特性集的预期行为的语言被称为[小黄瓜](https://cucumber.io/docs/gherkin/reference/)。它是一种[商业可读的领域特定语言](http://martinfowler.com/bliki/BusinessReadableDSL.html)，专门为行为描述而创建。从上面的文件中，我们描述了我们的应用程序所期望的特性之一，并创建了一个上下文来描述所提议的特性将为我们的系统带来的商业价值。然后我们使用`Scenario`关键字来定义该特性的可确定的业务情况。下面的步骤描述了实现该特性需要做的事情。

## 创建功能场景的步骤定义

既然我们已经恰当地概括了应用程序的特性，我们需要在`FeatureContext`中定义步骤定义。如果您现在使用以下命令执行 Behat:

```
vendor/bin/behat 
```

您将看到以下输出:

```
Feature: List of customers
  In order to retrieve the list of customers
  As a user
  I must visit the customers page

  Scenario: I want a list of customers
    Given I am an unauthenticated user
    When I request a list of customers from "http://localhost:8000/customer"
    Then The results should include a customer with ID "1"

1 scenario (1 undefined)
3 steps (3 undefined)
0m0.02s (9.58Mb)

 >> default suite has undefined steps. Please choose the context to generate snippets:

  [0] None
  [1] FeatureContext
 > 
```

输出表明，Behat 通过我们定义的三个步骤识别了我们的场景。然而，`FeatureContext`类有一些缺失的方法，它们代表了在`customer.feature`文件中创建的每个步骤。Behat 提供了一条路线，通过称为步骤定义的实际方法轻松映射每个场景步骤。

您可以手动创建这些方法，也可以让 Behat 自动为您生成这些方法。对于本教程，我们选择后者。要继续，选择选项`1`。

```
--- FeatureContext has missing steps. Define them with these snippets:

    /**
     * @Given I am an unauthenticated user
     */
    public function iAmAnUnauthenticatedUser()
    {
        throw new PendingException();
    }

    /**
     * @When I request a list of customers from :arg1
     */
    public function iRequestAListOfCustomersFrom($arg1)
    {
        throw new PendingException();
    }

    /**
     * @Then The results should include a customer with ID :arg1
     */
    public function theResultsShouldIncludeACustomerWithId($arg1)
    {
        throw new PendingException();
    } 
```

复制这些方法并用它们更新`FeatureContext.php`文件:

```
<?php

use Behat\Behat\Context\Context;
use Behat\Gherkin\Node\PyStringNode;
use Behat\Gherkin\Node\TableNode;
use Behat\Behat\Tester\Exception\PendingException;
use Symfony\Component\HttpClient\HttpClient;

/**
 * Defines application features from the specific context.
 */
class FeatureContext implements Context
{
    /**
     * Initializes context.
     *
     * Every scenario gets its own context instance.
     * You can also pass arbitrary arguments to the
     * context constructor through behat.yml.
     */
    public function __construct()
    {
    }

    /**
     * @Given I am an unauthenticated user
     */
    public function iAmAnUnauthenticatedUser()
    {
        throw new PendingException();
    }

    /**
     * @When I request a list of customers from :arg1
     */
    public function iRequestAListOfCustomersFrom($arg1)
    {
        throw new PendingException();
    }

    /**
     * @Then The results should include a customer with ID :arg1
     */
    public function theResultsShouldIncludeACustomerWithId($arg1)
    {
        throw new PendingException();
    }
} 
```

这些只是从`customer.feature`文件中的每一步派生的方法的定义。正确定义了方法之后，我们仍然需要添加所需的代码来完成我们的场景。用以下代码替换`features/bootstrap/FeatureContext.php`的内容:

```
<?php

use Behat\Behat\Context\Context;
use Symfony\Component\HttpClient\HttpClient;

/**
 * Defines application features from the specific context.
 */
class FeatureContext implements Context
{
    protected $response;
    /**
     * Initializes context.
     *
     * Every scenario gets its own context instance.
     * You can also pass arbitrary arguments to the
     * context constructor through behat.yml.
     */
    public function __construct()
    {
    }

    /**
     * @Given I am an unauthenticated user
     */
    public function iAmAnUnauthenticatedUser()
    {
        $httpClient = HttpClient::create();
        $this->response = $httpClient->request("GET", "http://localhost:8000/customer");

        if ($this->response->getStatusCode() != 200) {
            throw new Exception("Not able to access");
        }

        return true;
    }

    /**
     * @When I request a list of customers from :arg1
     */
    public function iRequestAListOfCustomersFrom($arg1)
    {
        $httpClient = HttpClient::create();
        $this->response = $httpClient->request("GET", $arg1);

        $responseCode = $this->response->getStatusCode();

        if ($responseCode != 200) {
            throw new Exception("Expected a 200, but received " . $responseCode);
        }

        return true;
    }

    /**
     * @Then The results should include a customer with ID :arg1
     */
    public function theResultsShouldIncludeACustomerWithId($arg1)
    {
        $customers = json_decode($this->response->getContent());

        foreach($customers as $customer) {
            if ($customer->id == $arg1) {
                return true;
            }
        }

        throw new Exception('Expected to find customer with an ID of ' . $arg1 . ' , but didnt');
    }
} 
```

从在`customer.feature`文件中创建的场景开始，我们首先为第一步创建一个名为`iAmAnUnauthenticatedUser()`的方法。这将决定是否已经创建了`customer`端点，以及未经身份验证的用户是否可以访问它。

```
public function iAmAnUnauthenticatedUser()
{
    $httpClient = HttpClient::create();
    $this->response = $httpClient->request("GET", "http://localhost:8000/customer");

    if ($this->response->getStatusCode() != 200) {
        throw new Exception("Not able to access");
    }

    return true;
} 
```

接下来，我们创建了一个方法来断言我们可以从`customer`端点检索客户列表。

```
public function iRequestAListOfCustomersFrom($arg1)
{
    $httpClient = HttpClient::create();
    $this->response = $httpClient->request("GET", $arg1);

    $responseCode = $this->response->getStatusCode();

    if ($responseCode != 200) {
        throw new Exception("Expected a 200, but received " . $responseCode);
    }

    return true;
} 
```

最后，为了确保检索到的客户列表包含预期的记录，我们将编写另一个方法来检查带有特定`id`的条目。

```
public function theResultsShouldIncludeACustomerWithId($arg1)
{
    $customers = json_decode($this->response->getContent());

    foreach($customers as $customer) {
        if ($customer->id == $arg1) {
            return true;
        }
    }

    throw new Exception('Expected to find customer with an ID of ' . $arg1 . ' , but didnt');
} 
```

现在运行 Behat 肯定会失败。我们还没有创建`customer`端点来返回适当的记录。

## 创建客户控制器

通过运行以下命令为客户端点生成控制器:

```
php bin/console make:controller CustomerController 
```

用以下代码替换`src/Controller/CustomerController.php`文件的内容:

```
<?php

namespace App\Controller;

use Symfony\Bundle\FrameworkBundle\Controller\AbstractController;
use Symfony\Component\HttpFoundation\Response;
use Symfony\Component\Routing\Annotation\Route;

class CustomerController extends AbstractController
{
    /**
     * @Route("/customer", name="customer")
     */
    public function index()
    {
        $customers = [
            [
                'id' => 1,
                'name' => 'Olususi Oluyemi',
                'description' => 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation',
            ],
            [
                'id' => 2,
                'name' => 'Camila Terry',
                'description' => 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation',
            ],
            [
                'id' => 3,
                'name' => 'Joel Williamson',
                'description' => 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation',
            ],
            [
                'id' => 4,
                'name' => 'Deann Payne',
                'description' => 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation',
            ],
            [
                'id' => 5,
                'name' => 'Donald Perkins',
                'description' => 'Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation',
            ]
        ];

        $response = new Response();

        $response->headers->set('Content-Type', 'application/json');
        $response->headers->set('Access-Control-Allow-Origin', '*');

        $response->setContent(json_encode($customers));

        return $response;
    }
} 
```

这里，我们定义了一个路由`/customer`，创建了一个默认的客户列表，并以 JSON 格式返回它。

## 在本地运行功能测试

我们的特性测试要求我们调用一个特定的端点。为此，我们需要保持服务器运行。运行以下命令启动服务器:

```
php bin/console server:run 
```

完成后，在终端的另一个选项卡或窗口中，使用以下命令执行 Behat:

```
vendor/bin/behat 
```

您将看到以下输出。

```
Feature: List of customers
  In order to retrieve the list of customers
  As a user
  I must visit the customers page

  Scenario: I want a list of customers
    Given I am an unauthenticated user
    When I request a list of customers from "http://localhost:8000/customer"
    Then The results should include a customer with ID "1"

1 scenario (1 passed)
3 steps (3 passed)
0m0.10s (10.01Mb) 
```

我们的测试现在像预期的那样运行。是时候创建一个 GitHub 存储库，并将这个应用程序的代码库推送给它了。遵循这个指南，学习如何[将项目推送到 GitHub](https://circleci.com/blog/pushing-a-project-to-github/) 。

## 添加 CircleCI 配置

我们首先需要更新`.env.test`文件，因为我们的管道需要它。将内容替换为以下内容:

```
# define your env variables for the test env here
KERNEL_CLASS='App\Kernel'
APP_SECRET='$ecretf0rt3st'
SYMFONY_DEPRECATIONS_HELPER=999999
PANTHER_APP_ENV=panther
APP_ENV=dev
DATABASE_URL=mysql://db_user:db_password@127.0.0.1:3306/db_name?serverVersion=5.7 
```

要添加 CircleCI 配置，在应用程序的根目录下创建一个`.circleci`文件夹，并在其中添加一个名为`config.yml`的新文件。打开新创建的文件并粘贴以下代码:

```
version: 2
jobs:
  build:
    docker:
      - image: circleci/php:7.4-node-browsers

    steps:
      - checkout

      - run: sudo apt update
      - run: sudo docker-php-ext-install zip

      - restore_cache:
          keys:
            - v1-dependencies-{{ checksum "composer.json" }}
            - v1-dependencies-

      - run:
          name: "Create Environment file"
          command: mv .env.test .env

      - run:
          name: "Install Dependencies"
          command: composer install -n --prefer-dist

      - save_cache:
          key: v1-dependencies-{{ checksum "composer.json" }}
          paths:
            - ./vendor
      - run:
          name: Run web server
          command: php bin/console server:run
          background: true

      # run Behat test
      - run:
          name: "Run Behat test"
          command: vendor/bin/behat 
```

在上面的配置文件中，我们从 CircleCI 映像注册表中提取了`circleci/php:7.4-node-browsers` [Docker 映像](https://circleci.com/docs/circleci-images/)，并安装了测试环境所需的所有包。然后，我们继续为我们的项目安装所有的依赖项。

包括启动本地服务器并在后台运行它的命令。

```
- run:
    name: Run web server
    command: php bin/console server:run
    background: true 
```

最后一部分是为我们的特性测试执行 Behat 的命令。

```
# run Behat test
- run:
    name: "Run Behat test"
    command: vendor/bin/behat 
```

继续用新代码更新存储库。在下一节中，我们将在 CircleCI 上设置我们的项目。

## 将项目连接到 CircleCI

在 [CircleCI](https://circleci.com/) 上登录您的帐户。在控制台中，找到在 GitHub 上创建的项目，点击**设置项目**。

![Set up project](img/75d35d2a5699b5f474a1e1f81ced7c39.png)

您将被重定向到一个新页面。点击**开始建造**。

![Start building](img/f69eaef360cfc869d08429cc4f9ace2e.png)

将出现一个提示，让您选择是将配置文件添加到存储库的新分支，还是手动添加。点击**手动添加**继续。

![Add config manually](img/54e40e2ec2a2910e9d9e4fc40a29a40d.png)

点击**开始建造**。

![Added config](img/281d91ac1820a5f381afdeb3a2549183.png)

这将成功运行，没有故障。

![Successful build](img/4727abfaaee8f433eec4ab8c6932651b.png)

单击作业以查看构建的详细信息。

![Successful build result](img/5ea3a7a71b4bd1bd946ae91d1655641e.png)

## 结论

在本教程中，我们遵循行为驱动开发(BDD)的基本原理，用 Behat 构建 Symfony 应用程序，并用 CircleCI 进行自动化测试。对于您的应用程序，您可以使用 Behat 做更多的事情。这里的信息足以让您开始为自己的应用程序构建合适的特性，同样的方法也可以用于任何其他 PHP 项目。

* * *

Oluyemi 是一个技术爱好者、编程狂和热爱新技术的网络开发迷。

* * *

Oluyemi 是一名拥有电信工程背景的技术爱好者。出于对解决用户日常遇到的问题的浓厚兴趣，他冒险进入编程领域，并从那时起将他的问题解决技能用于构建 web 和移动软件。Oluyemi 是一名热衷于分享知识的全栈软件工程师，他在世界各地的几个博客上发表了大量技术文章和博客文章。作为技术专家，他的爱好包括尝试新的编程语言和框架。

[阅读更多 Olususi Oluyemi 的帖子](/blog/author/olususi-oluyemi/)