# 使用 CI | CircleCI 将文档部署到 GitHub 页面

> 原文：<https://circleci.com/blog/deploying-documentation-to-github-pages-with-continuous-integration/>

[持续集成](https://circleci.com/continuous-integration/) (CI)工具一直朝着灵活的通用计算环境发展。它们不仅用于运行测试和报告结果，还经常运行完整的构建并将工件发送到外部系统。如果您已经依赖于 CI 系统来满足这些需求，那么使用相同的平台来构建和部署您的文档可能会更方便，而不是引入额外的工具或服务。

这篇文章在深入研究使用 CircleCI 将文档部署到 GitHub 页面的细节之前，概述了当前可用于构建和部署文档的一些流行选项，对于已经使用这些工具来托管代码和运行自动化测试的团队来说，这是一个非常方便的工作流。

## 部署文档的选项

API 文档通常使用特定于语言的文档工具(Python 的`sphinx`、Java 的`javadoc`等)从代码库呈现。).文档的构建可以在开发人员的本地机器上、在 CI 环境中或者在特定于文档的托管服务中完成。

托管文档的服务通常是特定于语言的，对于倾向于用单一语言编写项目的团队来说，这是一个非常好的低摩擦选择。例如，[阅读文档](https://readthedocs.org/)多年来一直是 Python 社区的标准工具。read Docs 使用 webhooks 来监视对托管存储库的提交，并将为每次代码更新自动构建和呈现文档，并提供一些在您自己的管道中难以复制的便利，例如部署多个版本的文档和维护从呈现的文档到源代码的链接。如果团队需要为其他语言部署文档，或者构建需要不常见的系统依赖，而这些系统依赖又不能通过`pip`或`conda`包管理器安装，那么它的局限性就会显现出来。使用特定于文档的服务还意味着为该附加服务维护另一组用户帐户和权限。

相反，构建文档最不依赖于基础设施的工作流是开发人员在本地构建文档，并将结果签入项目存储库。大多数团队更喜欢将生成的内容放在源代码控制之外，以使代码审查更简单，并减轻开发人员构建和提交内容的责任，但有些团队可能喜欢在代码旁边查看文档的修订历史。GitHub 通过提供将`docs`目录的内容呈现到 GitHub 页面的选项，开发了对该工作流的支持。对于 CI 系统中的文档，其他设置可能仍然需要单独的部署步骤。

相反，如果一个团队决定将文档作为 CI 流程的一部分来构建，那么内容可以被部署到各种各样的目的地，比如本地维护的服务器、像亚马逊 S3 这样的对象存储、GitHub Pages 或者其他一些外部托管服务。在大多数情况下，CI 作业需要某种形式的凭据，以便向目标进行身份验证，这可能是流程中最复杂的部分。GitHub Pages 作为文档宿主的主要优势之一是权限的合并；任何对存储库拥有管理员权限的开发人员都可以设置 GitHub 页面的部署，并提供 CI 服务提交内容所需的部署密钥。

## 部署到 GitHub 页面的选项

GitHub 为将站点部署到 GitHub 页面提供了三个选项，对于工作流和凭证有不同的含义。

最古老的选项，也是我们将在演练中使用的选项，是推送到一个特殊的`gh-pages`分支来触发部署。这通常被维护为一个“孤儿”分支，与`master`的修订历史完全分离，这可能有点难以维护。在我们的例子中，我们将构建一个 CircleCI 工作流，该工作流构建文档，使用库提交对`gh-pages`分支的更改，然后使用我们将提供的部署密钥将分支推送到 GitHub。

第二种选择是让 GitHub 页面呈现`master`分支。这对于仅用于存放文档的存储库很有用，但是如果您的目标是通过将代码和呈现的文档紧密地放在一个单一的权限模型中来获益，这就没什么帮助了。

最后，GitHub Pages 可以在`master`分支上呈现一个`docs`目录，该目录支持开发人员生成和提交文档作为其本地工作流一部分的工作流。这不需要 CI 平台，也不需要额外的凭证，但是大多数团队不喜欢将生成的内容包含在他们的`master`分支中，如前一节所讨论的。

## 逐步演练

### 创建基本 Python 项目

让我们构建一个小的 Python 包，它使用标准的 Python 生态系统工具进行测试(`pytest`)和文档记录(`sphinx`)。我们将配置 CircleCI 来运行测试、构建文档，并最终通过`gh-pages`分支部署到 GitHub 页面。该项目的完整代码可在 [jklukas/docs-on-gh-pages](https://github.com/jklukas/docs-on-gh-pages) 获得。

在一个新的目录中，我们将创建一个名为`mylib`的简单包，其中只有一个`hello`函数。`mylib/__init__.py`长相:

```
def hello():
    return 'Hello' 
```

我们还需要用一个空的`__init__.py`文件和包含以下内容的`test_hello.py`创建一个`test`目录:

```
import mylib

def test_hello():
    assert mylib.hello() == 'Hello' 
```

为了实际运行测试，我们需要安装`pytest`，所以让我们在一个`requirements.txt`文件中指定它。我们还将请求`sphinx`，这是我们将在下一节使用的文档工具:

```
sphinx==1.8.1
pytest==3.10.0 
```

此时，我们可以编写一个非常简单的 CircleCI 工作流，其中包含一个运行测试的作业。我们创建一个看起来像这样的`.circleci/config`:

```
version: 2

jobs:
  test:
    docker:
      - image: python:3.7
    steps:
      - checkout
      - run:
          name: Install dependencies
          command: pip install -r requirements.txt
      - run:
          name: Test
          command: pytest

workflows:
  version: 2
  build:
    jobs:
      - test 
```

我们提交所有这些结果，将它们推到一个新的 GitHub 存储库中，并在 CircleCI 中启用该存储库。CircleCI 应该为应该返回绿色的`master`分支生成一个初始构建。

### 添加文档

现在我们有了一个包含测试的基本库，让我们来建立文档框架。此时，您需要在本地安装`sphinx`，因此您可能希望使用`venv`工具创建一个虚拟环境，然后调用`pip install -r requirements.txt`，这使得`sphinx-quickstart`命令行工具可用于生成文档框架。我们将像这样调用它:

```
 sphinx-quickstart docs/ --project 'mylib' --author 'J. Doe'
    # accept defaults at all the interactive prompts 
```

`sphinx-quickstart`为我们生成了一个`Makefile`，所以构建文档就像从`docs/`目录中调用`make html`一样简单。让我们在 CircleCI 流程中的一项新工作中对其进行整理。我们可以在`jobs:`下面添加以下内容

```
 docs-build:
    docker:
      - image: python:3.7
    steps:
      - checkout
      - run:
          name: Install dependencies
          command: pip install -r requirements.txt
      - run:
          name: Build docs
          command: cd docs/ && make html
      - persist_to_workspace:
          root: docs/_build
          paths: html 
```

调用`make html`会填充一个包含我们想要部署的内容的`docs/_build/html`目录。我们新的`docs-build`作业的最后一个`persist_to_workspace`步骤将该目录的内容保存到一个中间位置，我们工作流中的后续作业可以访问该位置。现在，我们将把这个新任务添加到我们的工作流中:

```
workflows:
  version: 2
  build:
    jobs:
      - test
      - docs-build 
```

并提交结果。

即使没有部署呈现的内容，这项工作现在也可以检查我们文档的完整性。如果`sphinx`无法成功运行，该作业将失败，让您知道有问题。

### 将呈现的文档部署到 gh-pages 分支

此时，我们已经准备好开始构建 CI 工作流的最后一部分，这项工作将通过将构建的文档推送到我们存储库的`gh-pages`分支来部署它。

我们希望`gh-pages`成为一个“孤儿”分支，它只跟踪已呈现的文档，并且在`master`中有一个独立于源代码的时间线。可以创建这样一个分支，并使用简单的`git`命令行调用将内容复制到其中，但是它可能充满了边缘情况，如果出现任何问题，很容易导致工作环境损坏。在这种情况下，引入一个专门构建的工具是一个合理的选择，有几个开源项目可以使用。目前其中最流行的实际上是一个名为`gh-pages` 的 [Node.js 模块，它包括一个命令行界面，这就是我们在这里要使用的。](https://github.com/tschaub/gh-pages)

您完全有理由质疑我们为什么选择一个需要 JavaScript 环境来部署 Python 文档的应用程序。乍一看，这似乎增加了复杂性，但它实际上非常无缝地融入了我们的工作流，因为我们的 CI 环境本身支持 Docker 容器，我们可以为每个作业选择独立的基本映像。我们用 Python 运行时在容器中构建文档，然后用 Node.js 运行时在新容器中共享输出。

让我们继续在我们的`config.yml`文件的`jobs`部分下面编写第一个版本的`docs-deploy`作业，并逐步完成以下步骤:

```
 docs-deploy:
    docker:
      - image: node:8.10.0
    steps:
      - checkout
      - attach_workspace:
          at: docs/_build
      - run:
          name: Install and configure dependencies
          command: |
            npm install -g --silent gh-pages@2.0.1
            git config user.email "ci-build@klukas.net"
            git config user.name "ci-build"
      - run:
          name: Deploy docs to gh-pages branch
          command: gh-pages --dist docs/_build/html 
```

我们使用一个`node`基础映像，这样`npm`包管理器和 Node.js 运行时就可用了。`attach_workspace`步骤将来自`docs-build`步骤的渲染文档装载到我们的容器中，然后我们调用`npm install`来下载目标模块，其中包括一个命令行实用程序`gh-pages`，我们将在下一步中调用它。模块文档中要求使用`git config`命令。最后，`gh-pages --dist docs/_build/html`的调用将`html`目录的内容复制到`gh-pages`分支的根，并将结果推送到 GitHub。

让我们将这个新步骤添加到我们的工作流程中。`workflows`部分现在看起来像这样:

```
workflows:
  version: 2
  build:
    jobs:
      - test
      - docs-build
      - docs-deploy:
          requires:
            - test
            - docs-build
          filters:
            branches:
              only: master 
```

我们使`docs-deploy`作业依赖于另外两个步骤，这意味着在这两个步骤成功完成之前，它不会运行。这确保我们不会意外地发布没有通过测试的存储库状态的文档。我们还设置了一个过滤器，指定除了构建`master`分支之外，应该跳过`docs-deploy`作业。这样，我们就不会因为其他分支上仍在进行的变更而覆盖已发布的文档。

如果我们签入所有这些更改并让 CircleCI 运行我们的作业，我们的新作业将会失败:

> 错误:您用来验证的密钥已被标记为只读。

因此，我们需要做更多的工作来清理这些问题，并确保我们的 CI 工作拥有必要的证书。

### 提供部署密钥

正如在[https://circleci.com/docs/gh-bb-integration/](https://circleci.com/docs/gh-bb-integration/)中所讨论的，GitHub 提供了几个选项来给一个任务改变一个库的访问权。一般来说，GitHub 权限是与用户绑定的，所以一个凭证要么必须绑定到一个人类用户帐户，要么必须提供一个特殊的机器用户帐户。在跨存储库授予访问权限方面有很大的灵活性，但这可能会变得有些复杂。

我们选择[提供一个读/写部署密钥](https://circleci.com/docs/gh-bb-integration/#deployment-keys-and-user-keys)。这是一个特定于单个存储库而非用户的 ssh 密钥对。这对团队来说很好，因为这意味着如果提供密钥的用户离开组织或删除他们的帐户，访问权不会消失。这也意味着，作为帐户管理员的任何用户都可以按照下面的步骤进行集成设置。

让我们遵循 CircleCI 文档中的说明，并将它们应用到我们的案例中。

我们首先在本地机器上创建一个 ssh 密钥对:

```
ssh-keygen -t rsa -b 4096 -C "ci-build@klukas.net"
# Accept the default of no password for the key (This is a special case!)
# Choose a destination such as 'docs_deploy_key_rsa' 
```

我们最终得到一个私钥`docs_deploy_key_rsa`和一个公钥`docs_deploy_key_rsa.pub`。我们将私钥交给 CircleCI，方法是导航到 https://circleci.com/gh/jklukas/docs-on-gh-pages/edit#ssh 的[，点击“添加 SSH 密钥”，输入“github.com”作为主机名，然后粘贴私钥文件的内容。此时，我们可以继续从系统中删除私钥，因为只有我们的 CircleCI 项目应该可以访问:](https://circleci.com/gh/jklukas/docs-on-gh-pages/edit#ssh)

```
rm docs_deploy_key_rsa 
```

[https://circleci.com/gh/jklukas/docs-on-gh-pages/edit#ssh](https://circleci.com/gh/jklukas/docs-on-gh-pages/edit#ssh)页面将向我们展示密钥的指纹，这是一个可以安全公开的唯一标识符(不像私钥本身，它足以让攻击者对您的存储库进行写访问)。我们在我们的`docs-deploy`作业中添加了一个步骤，以授予作业对具有该指纹的键的访问权:

```
 - add_ssh_keys:
          fingerprints:
            - "59:ad:fd:64:71:eb:81:01:6a:d7:1a:c9:0c:19:39:af" 
```

当我们谈到安全问题时，我们将前往[https://circle ci . com/GH/jklukas/docs-on-GH-pages/edit # advanced-settings](https://circleci.com/gh/jklukas/docs-on-gh-pages/edit#advanced-settings)并仔细检查“将秘密从分叉的 pull 请求传递到构建”是否设置为默认值“Off”。SSH 密钥是一种我们只希望在信任正在运行的代码时才公开的秘密；如果我们允许 forks 使用这个密钥，攻击者就可以创建一个 pull 请求，将我们的私钥内容打印到 CircleCI 日志中。

现在，我们需要将公钥上传到 GitHub，以便它知道信任来自 CircleCI 的用我们的私钥发起的连接。我们前往>添加部署密钥，将其标题设为“CircleCI write key”并粘贴到`docs_deploy_key_rsa.pub`的内容中。如果您还没有删除私钥，请格外小心，不要意外地从`docs_deploy_key_rsa`复制！

### 一些最终修正

在我们测试 CircleCI 工作流能否成功地将更改推送到 GitHub 之前，让我们解决一些最后的细节问题。

首先，我们构建的文档包含以`_`开头的目录，这些目录对`jekyll`有特殊的意义，它是 GitHub 页面中内置的静态站点引擎。我们不希望 jekyll 更改我们的内容，所以我们需要添加一个`.nojekyll`文件并将`--dotfiles`标志传递给`gh-pages`，因为否则该实用程序将忽略所有的点文件。

其次，我们需要提供一个包含`[skip ci]`的定制提交消息，该消息指示 CircleCI 当我们将该内容推送到`gh-pages`分支时不应该重新启动。`gh-pages`分支只包含渲染的 HTML 内容，而不包含源代码和`config.yml`，所以构建将没有任何关系，只会在 CircleCI 中显示为失败。我们的全部工作现在看起来像:

```
 docs-deploy:
    docker:
      - image: node:8.10.0
    steps:
      - checkout
      - attach_workspace:
          at: docs/_build
      - run:
          name: Disable jekyll builds
          command: touch docs/_build/html/.nojekyll
      - run:
          name: Install and configure dependencies
          command: |
            npm install -g --silent gh-pages@2.0.1
            git config user.email "ci-build@klukas.net"
            git config user.name "ci-build"
      - add_ssh_keys:
          fingerprints:
            - "59:ad:fd:64:71:eb:81:01:6a:d7:1a:c9:0c:19:39:af"
      - run:
          name: Deploy docs to gh-pages branch
          command: gh-pages --dotfiles --message "[skip ci] Updates" --dist docs/_build/html 
```

我们准备提交更新后的配置，让 CircleCI 运行工作流。一旦它显示为绿色，我们应该注意到[我们的存储库现在有了一个`gh-pages`分支](https://github.com/jklukas/docs-on-gh-pages/tree/gh-pages)，并且呈现的内容现在可以在[https://jklukas.github.io/docs-on-gh-pages/](https://jklukas.github.io/docs-on-gh-pages/)获得。

## 结论

没有一种明显的“最佳方式”来构建和部署文档。对您的团队来说，阻力最小的路径将取决于您已经熟悉的工作流、工具和基础设施的特定组合。您的组织结构也很重要，因为它将影响到谁需要参与提供凭证并使系统相互通信。

这里介绍的特定解决方案目前非常适合 Mozilla 的数据平台团队(参见实践中的一个例子，位于[Mozilla/python _ moz telemetry](https://github.com/mozilla/python_moztelemetry))，因为它适用于不同的语言(我们的团队也维护 Java 和 Scala 中的项目)，它最大限度地减少了需要熟悉的工具的数量(我们已经投资了 GitHub 和 CircleCI)，权限模型让我们的团队在设置和控制文档工作流方面拥有自主权，并且我们还没有发现需要特定于文档的托管提供商提供的任何更高级的功能。

* * *

杰夫·克鲁卡斯有实验粒子物理学的背景，他既是教师又是帮助发现希格斯玻色子的研究人员。他现在在俄亥俄州哥伦布市的 Mozilla 公司的 Firefox 数据平台上远程工作，之前是 Simple 公司数据平台的技术负责人，Simple 是一家云端无分支银行。

[阅读更多杰夫·克鲁卡斯的文章](/blog/author/jeff-klukas/)