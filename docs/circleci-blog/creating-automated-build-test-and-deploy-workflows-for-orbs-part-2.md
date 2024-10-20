# 为 orbs 创建自动化构建、测试和部署工作流第 2 部分| CircleCI

> 原文：<https://circleci.com/blog/creating-automated-build-test-and-deploy-workflows-for-orbs-part-2/>

在我的[上一篇文章](https://circleci.com/blog/creating-automated-build-test-and-deploy-workflows-for-orbs/)中，我讨论了如何为一个新的 [orb](https://circleci.com/docs/orb-intro/) 建立一个理想的自动化 CI/CD 流程。这个过程的目标是版本控制、多级测试和自动化部署。现在，让我们看一个例子。我将使用我们上个月刚刚发布的 [Azure CLI orb](https://circleci.com/developer/orbs/orb/circleci/azure-cli) ，因为它足够小和简单，相当容易理解，但也有点复杂，因为它确实与第三方云提供商微软 Azure 集成。

用这个过程构建、测试和部署的 orb 的`config.yml`文件并不复杂。我们的 [Orb 工具](https://circleci.com/developer/orbs/orb/circleci/orb-tools) orb 已经抽象出自动化 Orb 开发的大部分基本机制。Azure CLI orb 的 [`config.yml`](https://github.com/CircleCI-Public/azure-cli-orb/blob/master/.circleci/config.yml) 包含两个不同的工作流。一个工作流用于基本验证和`dev`发布，另一个用于集成/使用测试和可能的 orb 生产部署:

```
workflows:
  lint_pack-validate_publish-dev:

  # …

  integration_tests-prod_deploy: 
```

这种设置允许您在单个`config.yml`文件、单个存储库中对 orb 进行使用和集成测试，并绑定到单个`git`提交。它允许绕过一些替代的 orb 测试方法，这些方法可能更复杂或更麻烦(尽管绝不是无效的)，例如[内联您的 orb](https://github.com/CircleCI-Public/config-preview-sdk/blob/master/docs/inline-orbs.md) ，使用外部存储库进行测试，或者在本质上是运行在 CircleCI 上的[本地作业](https://circleci.com/docs/local-cli/#running-a-job)中评估您的新 orb 源代码，使用`machine`执行器。

默认情况下，`config.yml`文件中定义的所有工作流程同时运行。然而，我们已经设置了这两个，一个在每次提交时运行，另一个仅由 git 标签触发[。通过控制如何创建这些 git 标签，我们可以确保我们的集成测试工作流仅在我们完成了基本的林挺/验证并发布了我们想要进一步测试的 orb 的`dev`版本之后才运行。](https://circleci.com/docs/workflows/#executing-workflows-for-a-git-tag)

`lint_pack-validate_publish-dev`

我们的第一个工作流程完全是圆形的！也就是说，除了调用我们的 [orb 工具](https://circleci.com/developer/orbs/orb/circleci/orb-tools) orb 中定义的一系列作业并向它们传递必要的参数之外，您不需要做任何事情就可以在您自己的 Orb 库的配置文件中使用它。让我们快速浏览一下这些工作，一个接一个:

```
 - orb-tools/lint: 
```

`orb-tools/lint`作业将使用`yamllint` [CLI 工具【lint 给定目录下的所有 YAML 文件，由`singapore/lint-condo`](https://yamllint.readthedocs.io/en/stable/) [Docker 镜像](https://hub.docker.com/r/singapore/lint-condo)打包。您可以提供一个定制的`.yamllint`配置文件，或者使用作业中包含的一些基本默认值。

```
 - orb-tools/pack:
      requires:
        - orb-tools/lint 
```

`orb-tools/pack`是为那些用[解构 YAML 格式](https://circleci.com/docs/local-cli/#packing-a-config)写球体的人制作的，我强烈推荐使用！对于任何具有一个或两个以上独立命令或作业的 orb，以这种方式组织 orb 源代码将使您的代码更容易解析、调试和开发。这类似于将一个整体分成更小、更模块化的服务，或者编写 React 组件，而不是充斥着 HTML、JavaScript 和 CSS 代码的单个 HTML 文件。

[orbs](https://circleci.com/blog/designing-a-package-manager-from-the-ground-up/)的设计利用了一些更高级的对象类型(即命令、执行器、作业、示例)，非常直观地形成了一个文件系统树，其中每种类型都有自己的文件夹，每个文件夹都包含该类型的所有实例，每个实例都在自己的 YAML 文件中。一个单独的`@orb.yml`文件作为这个系统的“入口点”,包含版本信息和描述。您的 orb 引用的其他 orb 也可以放在这里，或者每个 orb 也可以在一个`orbs`目录中有自己的文件(因为 orb 可以引用其他 orb)。[看一看](https://github.com/CircleCI-Public/azure-cli-orb/tree/master/src)我们的 Azure CLI orb 示例的`/src`目录，应该会清楚这一切看起来像什么。

默认情况下，`orb-tools/pack`将执行以下操作:

1.  签出您的项目
2.  将 orb 源文件夹打包到一个文件中
3.  验证这个新的 orb.yml 文件(`circleci orb validate orb.yml`)
4.  将`orb.yml`文件保存到一个工作空间，这样它就可以在下游作业中使用

正如您从上面的 YAML 片段中看到的，该作业有默认值，因此您通常可以简单地调用该作业，不带任何参数，orb 会处理其余的事情。

*   [工作三](https://circleci.com/developer/orbs/orb/circleci/orb-tools#jobs-publish-dev) : `orb-tools/publish-dev`

```
- orb-tools/publish-dev:
    orb-name: circleci/azure-cli
    context: orb-publishing
    requires:
      - orb-tools/pack 
```

我们的第一个工作流程就要结束了。这个作业只是发布了我们的 orb 的一个`dev`版本，它已经在之前的作业中对其 YAML 进行了标记和验证。同样，提供了合理的缺省值，因此可能只需要为`orb-name`参数提供一个值。我们已经为这项工作附加了一个[上下文](https://circleci.com/docs/contexts/)，因为发布一个 orb 需要一个 [CircleCI API 令牌](https://circleci.com/docs/managing-api-tokens/)，但是如果你已经将你的令牌存储为一个[项目环境变量](https://circleci.com/docs/env-vars/#setting-an-environment-variable-in-a-project)，那么上下文可能就不需要了。

*   [工作 4](https://circleci.com/developer/orbs/orb/circleci/orb-tools#jobs-trigger-integration-workflow) : `orb-tools/trigger-integration-workflow`

```
- orb-tools/trigger-integration-workflow:
    name: trigger-integration-dev
    context: orb-publishing
    ssh-fingerprints: 23:d1:63:44:ad:e7:1a:b0:45:5e:1e:e4:49:ea:63:4e
    requires:
      - orb-tools/publish-dev
    filters:
      branches:
        ignore: master

- orb-tools/trigger-integration-workflow:
    name: trigger-integration-master
    context: orb-publishing
    ssh-fingerprints: 23:d1:63:44:ad:e7:1a:b0:45:5e:1e:e4:49:ea:63:4e
    tag: master
    requires:
      - orb-tools/publish-dev
    filters:
      branches:
        only: master 
```

在这里，事情变得有点复杂。我们已经完成了所有基本的 orb 验证，并发布了 orb 的`dev`版本。现在我们要触发第二个工作流，它只有在 git 标签被推回到我们的 orb 存储库时才会运行。这正是这份`orb-tools`工作所要做的！这就是为什么该作业需要 SSH 指纹作为参数。该作业创建一个 git 标记，并通过 SSH 将它推回我们的 orb 存储库。

[我们的文档](https://circleci.com/docs/add-ssh-key/#steps)介绍了创建一个密钥并将其添加到 CircleCI 的过程，但简而言之:生成一个无密码的 OpenSSL(不是 OpenSSH)密钥对，将公共部分作为读/写密钥存储在您的 GitHub 或 Bitbucket 存储库中，将私有部分存储在 CircleCI 中。这将使您的 CircleCI 作业能够对您的 repo 进行更改，例如，推送 git 标记。

这种类型的方法是必要的，因为在内部，我们的构建系统在运行时处理配置文件，包括 orb。因此，如果没有一个新的 webhook 事件(比如一个 git 标签),我们将无法测试 orb 的新版本`dev`,除非手动进行第二次提交。git 标签使我们的系统重新处理我们的`config.yml`文件，拉入我们的 orb 的刚刚发布的`dev`版本。

至关重要的是，因为 git 标签与我们从本地机器推送的初始提交附加在同一个提交上，所以这个新的 CircleCI 构建被视为同一套 VCS 状态检查的一部分。也就是说，对于特定的拉请求，两组工作流作业将被附加到相同的提交，这使得开发人员可以很容易地评估给定的代码更改。

您会注意到我们调用了两次`trigger-integration-workflow`作业。这是因为我们的集成测试工作流使用[正则表达式过滤](https://circleci.com/docs/workflows/#using-regular-expressions-to-filter-tags-and-branches)来控制哪个作业将运行，这取决于 git 标签的内容。这样，我们可以实现以下目标:

1.  每当提交被推送到非主分支时，只运行我们的集成测试作业；而对主分支的提交将触发集成测试作业，如果测试作业成功，还可能触发生产部署。
2.  根据我们的 orb 源代码的哪些部分在给定的提交中被修改，动态地发布补丁、次要版本或主要版本。(例如:一个新的命令或作业将触发一个可能的主要版本；一个新的执行者，或者一个 orb 的整体`description`域的改变，只会触发一个小的释放。)这个特性在默认情况下是打开的，但是如果您想要更多地手动控制何时发布各种类型的 orb 版本，可以禁用它(将`use-git-diff`参数设置为`false`)。

第二个`trigger-integration-workflow`作业，只在提交给 master 时运行，本质上是将字符串“master”附加到我们的 git 标签上，导致我们完整的第二个工作流(可能还有生产部署作业)运行。

`integration-tests_prod-deploy`

Orb 集成测试总是比你在我们的第一个工作流中看到的更加自由，基本上可以拖放到几乎任何 orb repo 的 CircleCI 配置文件中。我的基本方法是，作为一个管理大型(而且还在增长！)的目的是关注使用测试，覆盖尽可能多的边缘情况。

换句话说，使用我们刚刚在之前的工作流中发布的 orb 的`dev`版本，并运行它的所有命令和作业。我们这样做是为了确保他们不会失败，并确保他们按照我们的期望行事。如果一个命令或任务可以以多种截然不同的方式使用，我们就运行它多次，这样我们就可以覆盖所有的方式。如果一个命令或作业涉及到与第三方服务的交互，在这种情况下，微软 Azure，我们建立一个测试环境，这样我们就可以有信心 orb 将为其他用户工作。

您甚至可以采用我们在这个 Azure CLI orb 上采用的方法，在不同的运行时环境中测试相同的命令或作业(如果作业被配置为采用自定义执行器的话)(下面，您将看到我们使用基于 Go 的 Docker 映像、基于 Python 的 Docker 映像和微软自己的第三方 Azure Docker 映像来测试 orb 的命令)。

由于这个例子的细节可能不适用于其他 orb，所以我将包括第二个工作流的完整 YAML，但是在继续讨论生产部署工作之前，只简单地触及几个要点，生产部署工作也被抽象到我们的 orb 工具 Orb 中，并且被设计为可供任何希望实现类似开发过程的 Orb 开发人员使用。

[在我们的集成测试工作中使用的 YAML 锚定过滤器](https://circleci.com/blog/circleci-hacks-reuse-yaml-in-your-circleci-config-with-yaml/):

```
integration-dev_filters: &integration-dev_filters
  branches:
    ignore: /.*/
  tags:
    only: /integration-.*/

integration-master_filters: &integration-master_filters
  branches:
    ignore: /.*/
  tags:
    only: /master-.*/

prod-deploy_requires: &prod-deploy_requires
  [test-orb-python_master, test-orb-azure_master, test-orb-golang_master] 
```

完整的集成-测试和部署工作流程:

```
integration-tests_prod-deploy:
  jobs:
    # triggered by non-master branch commits
    - test-orb-python:
        name: test-orb-python_dev
        context: orb-publishing
        filters: *integration-dev_filters

    - test-orb-azure-docker:
        name: test-orb-azure_dev
        context: orb-publishing
        filters: *integration-dev_filters

    - test-orb-golang:
        name: test-orb-golang_dev
        context: orb-publishing
        filters: *integration-dev_filters

    # triggered by master branch commits
    - test-orb-python:
        name: test-orb-python_master
        context: orb-publishing
        filters: *integration-master_filters

    - test-orb-azure-docker:
        name: test-orb-azure_master
        context: orb-publishing
        filters: *integration-master_filters

    - test-orb-golang:
        name: test-orb-golang_master
        context: orb-publishing
        filters: *integration-master_filters

    # patch, minor, or major publishing
    - orb-tools/dev-promote-prod:
        name: dev-promote-patch
        orb-name: circleci/azure-cli
        context: orb-publishing
        requires: *prod-deploy_requires
        filters:
          branches:
            ignore: /.*/
          tags:
            only: /master-patch.*/

    - orb-tools/dev-promote-prod:
        name: dev-promote-minor
        orb-name: circleci/azure-cli
        release: minor
        context: orb-publishing
        requires: *prod-deploy_requires
        filters:
          branches:
            ignore: /.*/
          tags:
            only: /master-minor.*/

    - orb-tools/dev-promote-prod:
        name: dev-promote-major
        orb-name: circleci/azure-cli
        release: major
        context: orb-publishing
        requires: *prod-deploy_requires
        filters:
          branches:
            ignore: /.*/
          tags:
            only: /master-major.*/ 
```

各种`test-orb`任务的内容在这个 orb 的`config.yml`文件中已经定义好了。如果你好奇，我鼓励你去[看一看](https://github.com/CircleCI-Public/azure-cli-orb/blob/master/.circleci/config.yml#L19)。现在，我只想说明我们对 YAML 锚的使用，主要是为了简化一些不太干燥的分支/标签过滤器。我们使用这些过滤器来确保我们在第一个工作流中推送的 git 标签将正确地控制我们的集成测试工作是否会导致生产部署。

最重要的是，我想检查一下我们称之为三次分开的工作。

为什么我们给这份工作打了三次电话？

这项工作旨在将`dev` orb 升级为语义/生产 orb 版本。因此，它需要一个参数来确定升级后的版本是补丁(0.0.x)、次要版本(0.x.0)还是主要版本(x.0.0)。因为我们如何在`trigger-integration-workflow`作业中使用 git 标签来确定最终发布的版本类型，我们需要调用这个作业三次，每次使用不同的正则表达式过滤 git 标签。

除此之外，这份工作超级直接！它的设计要求尽可能少的样板文件。所需要的只是一个 orb 名称和一个 CircleCI API 令牌。如果没有提供发布类型，则默认为`patch`。

## 包扎

这就是我们自动化 orb 开发工作流程的结束！我们只需对包含 orb 源代码的存储库进行一次提交，通过林挺、基本 orb 验证、发布`dev` orb 版本、通过 git 标签触发整个第二次集成测试工作流，以及动态插入`patch`、`minor`或`major` orb 版本的生产部署。

**几个结束语:**

[这个过程使用`@dev:alpha`标签的](https://github.com/CircleCI-Public/azure-cli-orb/blob/5d352d75be7fe7ee1f56e336445b15fc5cc6544a/.circleci/config.yml#L4)来标记你的球体。鉴于生产 orb 版本是不可变的，`dev`版本[可以被覆盖](https://circleci.com/docs/creating-orbs/#using-development-versions)。这使得我们的配置可以用这个 orb 的旧版本`@dev:alpha`处理一次，然后在我们用新版本`@dev:alpha`推送 git 标签后重新处理。因此，您将需要手动发布一个初始的`@dev:alpha` orb 版本来“引导”这个过程，以便您的配置可以被处理。

如果提交给定的提交已经改变了您的 orb 源，以至于现在正在调用先前的`@dev:alpha`版本中不存在的作业和命令，那么您可能还需要在提交给定的提交之前偶尔重新发布`@dev:alpha`版本。如果无法处理 CircleCI 作业的配置，它们将不会运行，并且调用尚不存在的 orb 命令将导致配置处理错误。

因为这里描述的大多数测试工作都驻留在我们的 orb 工具 Orb 中，所以我们在这个库的 README 中添加了这个过程的完整示例。

感谢阅读！

* * *

阅读更多信息: