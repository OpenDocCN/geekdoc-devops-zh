# 如何用 Docker | CircleCI 构建 CI/CD 管道

> 原文：<https://circleci.com/blog/build-cicd-piplines-using-docker/>

一年中，我在会议和其他活动中与我的许多工程师同事交谈。我喜欢演示的一件事是他们如何毫不费力地将[持续集成](/continuous-integration/)/持续部署(CI/CD)管道实现到代码库中。在这篇文章中，我将介绍一些演示代码和我在演示中使用的 CircleCI 配置。遵循这些步骤将向您展示如何将 [CI/CD 管道](https://circleci.com/blog/what-is-a-ci-cd-pipeline/)实现到您的代码库中。

这篇文章将涵盖:

*   Python Flask 应用程序的简单单元测试
*   如何在项目中使用 CircleCI 配置文件在代码库中实现 CI/CD 管道
*   建立码头工人形象
*   将 Docker 图像推送到 [Docker Hub](https://hub.docker.com)
*   启动一个部署脚本，该脚本将在数字海洋服务器上运行 Docker 容器中的应用程序

## 先决条件

在我们开始之前，您需要:

完成所有先决条件后，您就可以开始下一部分了。

## 使用示例应用程序

在这篇文章中，我将使用一个简单的 Python [Flask](http://flask.pocoo.org/) ，你可以在这里和本地`git clone`找到这个项目的完整[源代码。该应用程序是一个简单的 web 服务器，当向它发出请求时，它会呈现 html。Flask 应用程序位于`hello_world.py`文件中:](https://github.com/punkdata/python-circleci-docker)

```
from flask import Flask

app = Flask(__name__)

def wrap_html(message):
    html = """
        <html>
        <body>
            <div style='font-size:120px;'>
            <center>
                <image height="200" width="800" src="https://infosiftr.com/wp-content/uploads/2018/01/unnamed-2.png">
                <br>
                {0}<br>
            </center>
            </div>
        </body>
        </html>""".format(message)
    return html

@app.route('/')
def hello_world():
    message = 'Hello DockerCon 2018!'
    html = wrap_html(message)
    return html

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000) 
```

这段代码中的关键是去掉`hello_world()`函数中的`message`变量。此变量指定一个字符串值，并将在单元测试中测试此变量的值是否匹配。

## 测试代码

代码必须经过测试，以确保向公众发布高质量、稳定的代码。Python 附带了一个名为 [unittest](https://docs.python.org/2/library/unittest.html) 的测试框架，我将在本教程中使用它。现在我们有了一个完整的 Flask 应用程序，它需要一个配套的单元测试来测试应用程序并确保它按照设计的那样运行。单元测试文件`test_hello_world.py`是我们 hello_world.py 应用程序的单元测试。现在，让我们浏览一下代码。

```
import hello_world
import unittest

class TestHelloWorld(unittest.TestCase):

    def setUp(self):
        self.app = hello_world.app.test_client()
        self.app.testing = True

    def test_status_code(self):
        response = self.app.get('/')
        self.assertEqual(response.status_code, 200)

    def test_message(self):
        response = self.app.get('/')
        message = hello_world.wrap_html('Hello DockerCon 2018!')
        self.assertEqual(response.data, message)

if __name__ == '__main__':
    unittest.main() 
```

```
import hello_world
import unittest 
```

使用`import`语句导入`hello_world`应用程序使得测试可以访问`hello_world.py`中的代码。接下来，导入`unittest`模块，并开始为应用程序定义测试覆盖。

`class TestHelloWorld(unittest.TestCase):`testhello world 从基类`unittest.Test`实例化而来，基类是测试的最小单元。它检查对一组特定输入的特定响应。unittest 框架提供了一个基类 TestCase，您将使用它来创建新的测试用例。

```
def setUp(self):
        self.app = hello_world.app.test_client()
        self.app.testing = True 
```

调用类级方法`setUp()`来准备测试夹具。在调用测试方法之前立即调用它。在这个例子中，我们创建并定义了一个名为`app`的变量，并从 hello_world.py 代码中将它实例化为`app.test_client()`对象。

```
def test_status_code(self):
    response = self.app.get('/')
    self.assertEqual(response.status_code, 200) 
```

方法`test_status_code()`在代码中指定了一个实际的测试用例。这个测试用例向 Flask 应用程序发出一个`get`请求，并在`response`变量中捕获应用程序的响应。`self.assertEqual(response.status_code, 200)`将`response.status_code`结果的值与`200`的期望值进行比较，这表示`get`请求成功。如果服务器响应的 status_code 不是 200，测试将失败。

```
def test_message(self):
    response = self.app.get('/')
    message = hello_world.wrap_html('Hello DockerCon 2018!')
    self.assertEqual(response.data, message) 
```

另一个方法`test_message()`，指定了一个不同的测试用例。这个测试用例被设计来检查在 hello_world.py 代码的`hello_world()`方法中定义的`message`变量的值。像前面的测试一样，对应用程序进行了一个 **get** 调用，结果被捕获到一个`response`变量中:

```
message = hello_world.wrap_html('Hello DockerCon 2018!') 
```

按照 hello_world 应用程序中的定义，`message`变量被赋予从`hello_world.wrap_html()` helper 方法得到的 html。字符串`Hello DockerCon 2018`被提供给`wrap_html()`方法，然后在 html 中注入并返回。`test_message()`验证应用程序中的消息变量是否与测试用例中的预期字符串匹配。如果字符串不匹配，测试将失败。

## 实施 CI/CD 管道

既然我们已经清楚了应用程序及其单元测试，那么是时候将 CI/CD 管道实现到代码库中了。使用 CircleCI 实现 CI/CD 管道非常简单，但是在继续之前，请确保执行以下操作:

### 设置 CI/CD 管道

一旦您的项目在 CircleCI 平台中建立，任何向上游推送的提交都将被检测到，CircleCI 将执行您的`config.yml`文件中定义的作业。

在 repo 的根目录中创建一个新目录，并在这个新目录中添加一个 yaml 文件。新资产必须遵循项目的 git 存储库中的这些命名模式-目录:`.circleci/`文件:`config.yml`。这个目录和文件基本上定义了 CircleCI 平台的 CI/CD 管道和配置。

### 配置文件

config.yml 文件是所有 CI/CD 奇迹发生的地方。在显示示例文件的代码块之后，我将简要解释语法中发生了什么。

```
version: 2
jobs:
  build:
    docker:
      - image: circleci/python:2.7.14
        environment:
          FLASK_CONFIG: testing
    steps:
      - checkout
      - run:
          name: Setup VirtualEnv
          command: |
            echo 'export TAG=0.1.${CIRCLE_BUILD_NUM}' >> $BASH_ENV
            echo 'export IMAGE_NAME=python-circleci-docker' >> $BASH_ENV 
            virtualenv helloworld
            . helloworld/bin/activate
            pip install --no-cache-dir -r requirements.txt
      - run:
          name: Run Tests
          command: |
            . helloworld/bin/activate
            python test_hello_world.py
      - setup_remote_docker:
          docker_layer_caching: true
      - run:
          name: Build and push Docker image
          command: |
            . helloworld/bin/activate
            pyinstaller -F hello_world.py
            docker build -t ariv3ra/$IMAGE_NAME:$TAG .
            echo $DOCKER_PWD | docker login -u $DOCKER_LOGIN --password-stdin
            docker push ariv3ra/$IMAGE_NAME:$TAG
      - run:
          name: Deploy app to Digital Ocean Server via Docker
          command: |
            ssh -o StrictHostKeyChecking=no root@hello.dpunks.org "/bin/bash ./deploy_app.sh ariv3ra/$IMAGE_NAME:$TAG" 
```

`jobs:`键代表将要运行的任务列表。作业封装了要执行的操作。如果您只有一个作业要运行，那么您必须给它一个键名`build:`。这里有更多关于[工作和建造的细节。](https://circleci.com/docs/configuration-reference/#jobs)

`build:`键由几个元素组成:

`docker:`键告诉 CircleCI 使用一个 [Docker 执行器](https://circleci.com/docs/configuration-reference/#docker)，这意味着我们的构建将使用 Docker 容器来执行。

`image: circleci/python:2.7.14`指定构建必须使用的 Docker 映像。

#### 步骤:

`steps:`键是一个集合，它指定了将在这个构建中执行的所有命令。发生的第一个动作是`- checkout`命令，它在执行环境中执行代码的 git 克隆。

`- run:`键指定在构建中执行的命令。运行键有一个`name:`参数，您可以在这里标记一组命令。例如，`name: Run Tests`对测试相关的动作进行分组。这种分组有助于在 CircleCI 仪表板中组织和显示构建数据。

**注:** *每个`run`块相当于单独的、个体的壳或端子。已配置或已执行的命令将不会在以后的运行块中持续。*使用文档的[提示&技巧部分](https://circleci.com/docs/migration/#tips-for-setting-up-circleci-20)中的`$BASH_ENV`解决方法。

```
- run:
    name: Setup VirtualEnv
    command: |
      echo 'export TAG=0.1.${CIRCLE_BUILD_NUM}' >> $BASH_ENV
      echo 'export IMAGE_NAME=python-circleci-docker' >> $BASH_ENV 
      virtualenv helloworld
      . helloworld/bin/activate
      pip install --no-cache-dir -r requirements.txt 
```

这个运行块的`command:`键有一个要执行的命令列表。这些命令设置了`$TAG` & `IMAGE_NAME`自定义环境变量，这些变量将在整个构建过程中使用。剩下的命令设置 [python virtualenv](https://virtualenv.pypa.io/en/stable/) 并安装在`requirements.txt`文件中指定的 python 依赖项。

```
- run:
    name: Run Tests
    command: |
      . helloworld/bin/activate
      python test_hello_world.py 
```

在这个运行块中，命令对我们的应用程序执行测试。如果这些测试失败，整个构建将会失败。开发人员需要修改他们的代码并重新提交。

```
- setup_remote_docker:
    docker_layer_caching: true 
```

这个运行块指定了 [setup_remote_docker:](https://circleci.com/docs/glossary/#remote-docker) 键，这是一个支持从 docker executor 作业中构建、运行和推送映像到 Docker 注册表的特性。当 docker_layer_caching 设置为 true 时，CircleCI 将尝试重用在之前的作业或工作流中构建的 docker 图像(层)。也就是说，您在之前的作业中构建的每个图层都可以在远程环境中访问。但是，在某些情况下，即使配置指定 docker_layer_caching: true，您的作业也可能在干净的环境中运行。

`setup_remote_docker:`特性是必需的，因为我们正在为我们的应用程序构建 Docker 映像，并将该映像推送到 Docker Hub。

```
- run:
    name: Build and push Docker image
    command: |
      . helloworld/bin/activate
      pyinstaller -F hello_world.py
      docker build -t ariv3ra/$IMAGE_NAME:$TAG .
      echo $DOCKER_PWD | docker login -u $DOCKER_LOGIN --password-stdin
      docker push ariv3ra/$IMAGE_NAME:$TAG 
```

**构建和推送 Docker 映像**运行块指定了使用 pyinstaller 将应用程序打包成一个二进制文件的命令。然后继续进行 Docker 映像构建过程。

```
docker build -t ariv3ra/$IMAGE_NAME:$TAG .
echo $DOCKER_PWD | docker login -u $DOCKER_LOGIN --password-stdin
docker push ariv3ra/$IMAGE_NAME:$TAG 
```

这些命令基于 repo 中包含的`Dockerfile`构建 docker 映像。构建 Docker 映像的说明可以在这里找到: [Dockerfile](https://docs.docker.com/engine/reference/builder/) 。

```
FROM python:2.7.14

RUN mkdir /opt/hello_word/
WORKDIR /opt/hello_word/

COPY requirements.txt .
COPY dist/hello_world /opt/hello_word/

EXPOSE 80

CMD [ "./hello_world" ] 
```

`echo $DOCKER_PWD | docker login -u $DOCKER_LOGIN --password-stdin`命令使用 CircleCI 仪表板中设置的$DOCKER_LOGIN 和$DOCKER_PWD 环境变量作为登录凭证&将此映像推送到 Docker Hub。

```
- run:
    name: Deploy app to Digital Ocean Server via Docker
    command: |
      ssh -o StrictHostKeyChecking=no root@hello.dpunks.org "/bin/bash ./deploy_app.sh ariv3ra/$IMAGE_NAME:$TAG" 
```

最后一个运行块将我们的新代码部署到运行在数字海洋平台上的实时服务器上，确保您已经在远程服务器上创建了部署脚本。ssh 命令访问远程服务器并执行`deploy_app.sh`脚本，包括 **ariv3ra/$IMAGE_NAME:$TAG** ，它指定要从 Docker Hub 拉取和部署的映像。

作业成功完成后，新的应用程序应该在 config.yml 文件中指定的目标服务器上运行。

## 摘要

本教程的目标是指导您在代码中实现 CI/CD 管道。虽然这个示例是使用 Python 技术构建的，但是一般的构建、测试和部署概念可以使用您喜欢的任何语言或框架轻松实现。您可以扩展本教程中的简单示例，并根据您自己的管道进行定制。CircleCI 有很棒的[文档](https://circleci.com/docs/)请确保研究我们的文档网站。如果你真的遇到困难，你可以联系 https://discuss.circleci.com/社区/论坛网站[上的 CircleCI 社区。](https://discuss.circleci.com/)

阅读更多信息: