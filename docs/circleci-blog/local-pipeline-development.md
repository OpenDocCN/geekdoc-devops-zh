# 当地 CI 渠道开发| CircleCI

> 原文：<https://circleci.com/blog/local-pipeline-development/>

[CircleCI 命令行界面(CLI)工具](https://circleci.com/docs/local-cli/)使开发人员能够在其本地开发环境中构建、验证和测试他们的管道[作业](https://circleci.com/docs/jobs-steps/#jobs-overview)。本教程演示了这个过程，并讨论了这种开发模式的一些好处。我将讨论在使用 CLI 工具构建管道配置时需要理解的关键管道概念和元素。

## 先决条件

在开始之前，您可以通读 [CircleCI CLI 入门指南](https://circleci.com/docs/local-cli-getting-started/)。然后，您需要在您的本地开发环境中执行一些事情:

完成先决条件后，您就可以开始在本地构建管道作业了。在下一节中，我将讨论一些重要的 CircleCI 概念。然后，我们将开始在本地构建管道作业，并用 CLI 工具验证它们。

## CircleCI 概念

在我们开始在本地构建管道作业之前，我想分解 CircleCI 管道配置的主要元素。

| 概念 | 描述 |
| --- | --- |
| [管道](https://circleci.com/docs/concepts/#pipelines) | CircleCI 管道包含使用 CircleCI 触发项目工作时运行的全套工作流。 |
| [工作流程](https://circleci.com/docs/concepts/#workflows) | 工作流协调项目配置中定义的作业。 |
| [遗嘱执行人](https://circleci.com/docs/concepts/#executors-and-images) | 在您的配置中定义的每个单独的作业将在一个唯一的执行器/运行时中运行，比如 Docker 容器和虚拟机。 |
| [工作岗位](https://circleci.com/docs/concepts/#jobs) | 作业是您的配置的构造块。作业是步骤的集合，根据需要运行命令/脚本。 |
| [步骤](https://circleci.com/docs/concepts/#steps) | 步骤是完成工作需要采取的行动。步骤通常是可执行命令的集合。 |

熟悉这些概念有助于理解 [CI 渠道](https://circleci.com/blog/what-is-a-ci-cd-pipeline/)配置及其构成方式。

CircleCI 是一个为开发人员执行[连续集成](https://circleci.com/continuous-integration/)和连续交付相关任务提供自动化的平台，这些任务在 [CircleCI 配置文件](https://circleci.com/docs/local-cli/#limitations-of-running-jobs-locally)中定义。配置文件是代码和 CircleCI 平台之间的主要接口。它基本上告诉 CircleCI 何时、如何以及对代码执行什么操作。该平台非常健壮，可以执行诸如运行自动化测试、编译工件、构建 docker 映像、将代码部署到目标环境等过程，以及许多其他高级过程。

## CLI 限制

在您开始学习如何使用 CLI 工具之前，我想对 CLI 工具的功能和局限性提出一些期望。CLI 工具可以执行以下操作:

*   调试和验证您的 CI 配置
*   在本地运行作业
*   查询 CircleCI 的 API
*   创建、发布、查看和管理球体
*   管理上下文

我在下面列出了[的限制](https://circleci.com/docs/local-cli/#limitations-of-running-jobs-locally):

*   只有作业可以在本地运行。管道和工作流不在本地运行。
*   您不能在本地作业中使用机器执行器。这是因为机器执行器需要额外的 VM 来运行它的作业。
*   目前无法添加 SSH 密钥。
*   CLI 工具不支持运行工作流，因为工作流会在多台计算机上同时运行作业，从而使您能够实现更快、更复杂的构建。因为 CLI 仅在您的计算机上运行，所以它只能运行单个作业。
*   本地作业目前不支持缓存。当您的配置中有 save_cache 或 restore_cache 步骤时，CircleCI 会跳过它们并显示警告。
*   出于安全原因，UI 中配置的加密环境变量不会导入到本地版本中。或者，您可以使用`-e`标志指定环境变量来与 CLI 一起使用。如果有多个环境变量，则必须为每个变量使用标志。比如`circleci build -e VAR1=FOO -e VAR2=BAR`。

## 使用 CircleCI CLI

如前所述，管道配置文件是 CircleCI 自动化的接口。它定义并控制您的 CI/CD 流程。CircleCI 平台的默认行为是在每次代码变更/git 提交被推送到您的版本控制系统(VCS)上的共享存储库时，触发基于各自配置文件的构建。一旦构建被触发，平台就执行配置文件中的指令，并在管道运行时产生通过或失败的结果。这个结果对开发者来说是很好的反馈。它向他们提供了关于他们的代码/管道哪里有问题的详细信息，以便他们可以快速理解并解决它们。我在这里描述的是现有管道配置文件的典型过程，该文件已经通过了配置开发的初始阶段。

为项目开发初始管道配置文件可能会耗费大量时间和资源。尤其是如果你是这种范式的新手。通常，管道配置开发流程遵循以下模式:

*   编辑配置文件中的一些代码
*   在本地提交这些更改
*   将这些变化推给你的上游回购
*   CircleCI 执行您的配置文件
*   管道构建通过或失败
*   如果构建失败，[调试代码](https://circleci.com/blog/a-scientific-approach-to-debugging/)并重复这些步骤，直到它通过

上述模式很常见。可以想象，开发一个有意义的配置文件需要多次提交。这种配置文件开发模式也使用平台资源。这可能会导致一些不必要的自动化周期消耗。使用 CLI 工具，您可以减少不必要的代码提交量和浪费的资源周期。您可以简化配置文件开发模式，如下所示:

*   在本地编辑代码
*   验证配置文件语法
*   执行/运行您修改的特定作业
*   作业通过或失败
*   如果作业失败，调试代码，然后重复这些步骤，直到它通过

这里的主要区别在于，您减少了提交污染，并且您专注于特定的工作元素，而不是整个管道配置。这使您能够快速进行更改，并在本地测试和调试它们，而不必消耗宝贵的平台资源。它还使您能够实验和优化您的作业和命令，而不会污染您的 VCS 的版本历史。

让我们了解如何使用 CLI 工具构建和运行管道作业。

## 使用 CLI 运行作业

在这一节中，我将演示如何在本地构建一个简单的配置文件，验证语法，并使用 CLI 工具运行一个作业。

先决条件一节中的[代码报告](https://github.com/datapunkz/circelci-cli-node)有一个示例应用程序，我们将使用它在本地创建工作。确保您位于项目的根目录，并运行以下命令来创建所需的目录和一个空的配置文件:

```
mkdir .circleci/ && touch config.yml 
```

接下来，用您最喜欢的文本编辑器打开`config.yml`文件，并添加以下内容:

```
version: 2.1

jobs:

workflows: 
```

上面的语法是我们的配置文件的开始，其中定义了作业和工作流的元素。现在，让我们向配置中添加一个实际的作业。用以下代码更新您的配置文件:

```
version: 2.1

jobs:
  run_tests:
    docker:
      - image: circleci/node:12
    steps:
      - checkout
      - run:
          name: Install npm dependencies
          command: |
            sudo npm install -g
      - run:
          name: Run Unit Tests
          command: |
            ./node_modules/mocha/bin/mocha test/ --reporter mochawesome --reporter-options reportDir=test-results,reportFilename=test-results
workflows:
  build_test:
    jobs:
      - run_tests 
```

我们在管道配置文件中定义了一个新的作业和工作流。该作业被命名为`run_tests`。它对应用程序运行测试，并将结果保存到文件中。可以使用 CLI 工具在本地修改和测试`run_tests`作业，而无需提交更改或使用宝贵的平台计算周期。

在`build_test`工作流中，我们定义在这个管道中执行哪些作业。工作流可以被视为作业编排器。它们定义了各个作业在管道中运行的方式和时间。由于我们目前只有一个作业，我们将指示我们的工作流在被触发时执行我们的`run_tests`作业。

**注意:** *工作流不在本地执行。只有在配置文件中定义的作业才会从 CLI 工具中执行。*

### 验证配置文件语法

CLI 工具有一个很棒的功能，它可以验证配置文件中的 YAML，以确保语法是兼容的和有效的。验证功能应该在每次更改后运行，以确保您没有引入格式问题。每当我对我的配置文件进行重大修改时，我都会使用它。这样，我可以及早发现错误，并专注于我的配置中更紧迫的问题。使用以下命令验证您的配置文件:

```
circleci config validate 
```

运行此命令会产生以下结果:

```
Config file at .circleci/config.yml is valid. 
```

如果您的配置文件中有语法问题，validate 进程会用大量的细节为您标记出来。

### 在本地测试作业

现在您已经有了一个有效的配置文件，您可以在本地执行和测试`run_tests`作业。在终端中运行以下命令:

```
circleci local execute --job run_tests 
```

接下来，CLI 将处理`run_tests`作业。它将开始下载所需的指定 Docker 映像(第一次将比后续运行时间长)。一旦 Docker 执行器/容器启动并运行，就会处理`run`块并执行它们的命令。

```
====>> Run Unit Tests
  #!/bin/bash -eo pipefail
./node_modules/mocha/bin/mocha test/ --reporter mochawesome --reporter-options reportDir=test-results,reportFilename=test-results

Node server is running..
  Welcome to CI/CD Server
    GET /
      ✓ returns status code 200 (39ms)
    welcomeMessage
      ✓ Validate Message

  2 passing (48ms)

[mochawesome] Report JSON saved to /home/circleci/project/test-results/test-results.json

[mochawesome] Report HTML saved to /home/circleci/project/test-results/test-results.html

Success! 
```

`run_tests`作业已经成功完成，并且您已经验证了它将在 CircleCI 平台上按设计运行。这是一场了不起的胜利！正如我前面提到的，您能够在本地构建和测试管道作业，而不必向 repo 提交代码或在平台上消耗宝贵的计算。使用 CLI 是在本地环境中维护通用开发实践的一种很好的方式。

## 通过 CLI 工具使用环境变量

因此，我们定义、验证并测试了一个在我们的应用程序上运行自动化单元测试的作业。既然这些测试通过了，我们应该构建一个新的任务来对我们的应用程序执行漏洞扫描，这样我们就可以很容易地识别代码中任何危险的漏洞。在这一节中，我将演示如何构建一个新的作业，该作业将使用您在先决条件一节中创建的 [Snyk API 令牌](https://docs.snyk.io/features/snyk-cli/authenticate-the-cli-with-your-account/)。

在我们进入代码之前，我想讨论一下如何在本地处理环境变量。在作业中使用[环境变量](https://circleci.com/docs/env-vars/)是很常见的，CircleCI 平台的特性使您能够安全地定义、保护和使用配置文件中的敏感数据。因为我们在本地执行作业，所以我们无法访问存储在平台上的环境变量及其值。幸运的是，有一个影响最小的解决方法。

您可以在本地开发环境中指定在 CircleCI 中配置的相同环境变量。这提供了无缝体验。根据您本地环境的操作系统，定义环境变量可能与我在这里演示的不同。在这个例子中，我使用的是 Linux。我将在我的`$HOME/.bashrc`文件中为这个新任务定义一个环境变量:

```
export SNYK_TOKEN="<Replace this with your Snyk API Token>" 
```

一旦本地定义了这个环境变量，我就可以创建使用它们的新管道作业，并模拟从平台访问它们。现在，让我们构建新的漏洞扫描作业。

## 在本地测试漏洞扫描作业

我们准备创建一个新的管道作业，该作业将扫描应用程序代码以发现安全漏洞。首先，用下面的代码更新您的`config.yml`文件:

```
version: 2.1
orbs:
  snyk: snyk/snyk@0.0.11
jobs:
  run_tests:
    docker:
      - image: circleci/node:12
    steps:
      - checkout
      - run:
          name: Install npm dependencies
          command: |
            sudo npm install -g
      - run:
          name: Run Unit Tests
          command: |
            ./node_modules/mocha/bin/mocha test/ --reporter mochawesome --reporter-options reportDir=test-results,reportFilename=test-results
  vulnerability_scan:
    docker:
      - image: circleci/node:12
    steps:
      - checkout
      - run:
          name: Install npm dependencies
          command: |
            sudo npm install -g
      - snyk/scan
workflows:
  build_test:
    jobs:
      - run_tests
      - vulnerability_scan 
```

在我们执行新作业之前，让我们先了解一下这个配置中的一些新语法和特性。文件的顶部是新行:

```
orbs:
  snyk: snyk/snyk@0.0.11 
```

这些代码行代表了 [CircleCI orbs](https://circleci.com/orbs/) 的一个实现，它们是可重用的代码片段，有助于自动化重复的过程，加快项目设置，并使其易于与第三方工具集成。因为我们在管道工作中使用 Snyk 扫描工具，所以实现 [Snyk orb](https://circleci.com/developer/orbs/orb/snyk/snyk) 对我们的管道来说是完美的。

`vulnerability_scan:`作业中的`- snyk/scan` 行演示了如何实现 Snyk orb 的`scan`函数。这将触发扫描，并提供将识别任何漏洞并建议缓解步骤(如果存在)的结果。让我们使用 CircleCI CLI 来执行这个新任务。在终端中运行以下命令:

```
circleci local execute -e SNYK_TOKEN=$SNYK_TOKEN --job vulnerability_scan 
```

上面命令中的`-e` 标志指定了一个名为`SNYK_TOKEN`的环境变量。它被赋予我们在上一节中定义的`$SNYK_TOKEN`环境变量的值。这个环境变量的值非常敏感。在管道中使用时，必须对其进行保护。

在使用 CLI 执行此作业后，它失败了，因为我的安全扫描失败，输出如下:

```
====>> Run Snyk test to scan app for vulnerabilities
  #!/bin/bash -eo pipefail
snyk test  --severity-threshold=low    

Testing /home/circleci/project...
Tested 168 dependencies for known issues, found 2 issues, 2 vulnerable paths.

Issues to fix by upgrading:

  Upgrade mocha@5.2.0 to mocha@6.2.3 to fix
  ✗ Prototype Pollution [Medium Severity][https://snyk.io/vuln/SNYK-JS-MINIMIST-559764] in minimist@0.0.8
    introduced by mocha@5.2.0 > mkdirp@0.5.1 > minimist@0.0.8
  ✗ Regular Expression Denial of Service (ReDoS) [High Severity][https://snyk.io/vuln/SNYK-JS-MOCHA-561476] in mocha@5.2.0

Organization:      datapunkz
Package manager:   npm
Target file:       package.json
Project name:      nodejs-circleci
Open source:       no
Project path:      /home/circleci/project
Licenses:          enabled

Run `snyk wizard` to address these issues.

Error: Exited with code 1
Step failed
Error: runner failed
Task failed 
```

Mocha 版本有一个明显的问题，Snyk 工具已经识别了易受攻击的依赖项，并提供了缓解解决方案。在这种情况下，解决方案是升级`package.json`文件中定义的摩卡版本。事实上，漏洞有时是可以接受的。这是例外，不是规则。我强烈鼓励每个人尽可能修复所有已知的漏洞，但在极少数情况下这是不可能的。在这种情况下，您的管道不应该因为“可接受”的漏洞扫描失败而失败。幸运的是，Snyk 有一个标志，即使扫描失败，也可以让管道继续运行。Snyk orb 有一个名为`fail-on-issues`的参数，默认为真，如果扫描失败，作业将失败。更新您的`snyk/scan`线路以匹配以下内容:

```
 - snyk/scan:
          fail-on-issues: false 
```

现在再次执行作业:

```
circleci local execute -e SNYK_TOKEN=$SNYK_TOKEN --job vulnerability_scan 
```

这产生了以下结果:

```
Issues to fix by upgrading:
  Upgrade mocha@5.2.0 to mocha@6.2.3 to fix
  ✗ Prototype Pollution [Medium Severity][https://snyk.io/vuln/SNYK-JS-MINIMIST-559764] in minimist@0.0.8
    introduced by mocha@5.2.0 > mkdirp@0.5.1 > minimist@0.0.8
  ✗ Regular Expression Denial of Service (ReDoS) [High Severity][https://snyk.io/vuln/SNYK-JS-MOCHA-561476] in mocha@5.2.0

Explore this snapshot at https://app.snyk.io/org/datapunkz/project/6a7762c9-1447-4162-a776-de26d34ef418/history/88186524-4993-46a0-b67f-f4d4bd2331f3

Notifications about newly disclosed issues related to these dependencies will be emailed to you.

Success! 
```

Snyk 扫描结果与前一次运行相同，但这一次管道作业没有失败，因为我们将`fail-on-issues`参数值设置为`false`。我想再次强调的是，所有已发现的安全漏洞都应该尽快得到解决和缓解，将标志设置为`false`是一个危险的行为，在实施之前应该彻底考虑和审查。

下面是将`fail-on-issues:`参数设置为`false`的完整配置文件:

```
version: 2.1
orbs:
  snyk: snyk/snyk@0.0.11
jobs:
  run_tests:
    docker:
      - image: circleci/node:12
    steps:
      - checkout
      - run:
          name: Install npm dependencies
          command: |
            sudo npm install -g
      - run:
          name: Run Unit Tests
          command: |
            ./node_modules/mocha/bin/mocha test/ --reporter mochawesome --reporter-options reportDir=test-results,reportFilename=test-results
  vulnerability_scan:
    docker:
      - image: circleci/node:12
    steps:
      - checkout
      - run:
          name: Install npm dependencies
          command: |
            sudo npm install -g
      - snyk/scan:
          fail-on-issues: false
workflows:
  build_test:
    jobs:
      - run_tests
      - vulnerability_scan 
```

## 摘要

恭喜你！您刚刚学习了如何使用 CircleCI CLI 在本地开发环境中开发和测试管道作业。在这篇文章中，我重点介绍了在本地开发和测试作业的各种方法，但是我强烈建议您继续使用 CLI 工具来探索其他开发模式，以简化您的管道开发和管理工作。CLI 工具还使开发人员能够管理和执行其他 CircleCI 产品和功能上的命令，如 [orbs](https://circleci.com/orbs/) 、 [CircleCI API](https://circleci.com/docs/api/v2/#circleci-api) 和[上下文](https://circleci.com/docs/contexts/)。

感谢你关注这篇文章，希望你觉得有用。请随时在 Twitter [@punkdata](https://twitter.com/punkdata) 上寻求反馈。