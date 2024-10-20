# 使用对象参数| CircleCI 管理可重用管道配置

> 原文：<https://circleci.com/blog/parameters-in-pipeline-config/>

CircleCI 管道是使用 [YAML 语法](https://circleci.com/docs/writing-yaml/)定义的，它已经被许多软件工具和解决方案广泛采用。YAML 是一种人类可读的声明性[数据结构](https://en.wikipedia.org/wiki/Data_structure)，用于[配置文件](https://circleci.com/docs/config-intro/#getting-started-with-circleci-config)(类似于 CircleCI 管道的配置文件)以及存储或传输数据的应用程序中。管道配置文件中的数据指定并控制工作流和作业在平台上触发时的执行方式。配置文件中的这些管道指令往往会变得重复，这可能会导致配置语法量增加。随着时间的推移，这种数量的增加使得配置更难维护。因为 YAML 是一种数据结构，所以只有最低限度的语法可重用性能力([锚和别名](https://yaml.org/spec/1.2/spec.html#id2765878))可用于处理增加的数量。锚和别名太有限，不能用于定义 CI/CD 管道。幸运的是，CircleCI 配置[参数特性](https://circleci.com/docs/pipeline-variables/#pipeline-parameters-in-configuration)为封装和重用冗余的功能数据提供了强大的功能。

在这篇文章中，我将介绍管道[配置参数](https://circleci.com/docs/pipeline-variables/#pipeline-parameters-in-configuration)，并解释在您的管道配置中采用它们的一些好处。

## 什么是配置参数？

[执行者](https://circleci.com/docs/configuration-reference/#executors-requires-version-21)、[作业](https://circleci.com/docs/configuration-reference/#jobs)和[命令](https://circleci.com/docs/configuration-reference/#commands-requires-version-21)被认为是流水线配置文件中的对象。像[面向对象编程(OOP)](https://en.wikipedia.org/wiki/Object-oriented_programming) 范例中的对象一样，管道对象可以被扩展以提供定制的功能。CircleCI 配置[参数](https://circleci.com/docs/pipeline-variables/#pipeline-parameters-in-configuration)通过提供创建、封装和重用管道配置语法的方法，让开发人员扩展执行器、作业和命令的功能。

执行器、作业和命令是具有各自属性的对象。参数也有自己独特的属性，可以与对象交互。参数的组成和属性包括:

```
parameters:
  |_ parameter name: (Specify a name the parameter)
    |_ description: (Optional. Describes the parameter)
    |_ type: (Required. datatype string, boolean, integer, enum)
    |_ default: (The default value for the parameter) 
```

## 何时应该在管道配置中使用参数？

当数据和功能在管道中重复时，使用参数。换句话说，如果您的管道中有任何执行器、作业或命令在您的管道中被多次定义或执行，我建议识别这些模式或元素，并在您的配置文件语法中将它们定义为参数。使用参数使您能够集中管理和维护功能，并极大地减少配置文件中的冗余数据和语法行数。提供可变参数的能力也是一个好处，管道语法的整体可读性也得到了提高。

## 如何创建参数？

如前所述，[执行者](https://circleci.com/docs/configuration-reference/#executors-requires-version-21)、[作业](https://circleci.com/docs/configuration-reference/#jobs)、[命令](https://circleci.com/docs/configuration-reference/#commands-requires-version-21)是可以用参数扩展的配置元素。决定扩展这些元素中的哪一个将取决于您的特定用例。

**注意:** *参数功能仅在 CircleCI 及以上版本中可用。必须在配置文件的顶部定义版本，如下所示:* `version: 2.1` *，以便平台识别参数。*

让我给你举一个在`jobs:`对象中为[并行度](https://circleci.com/docs/configuration-reference/#parallelism)量定义参数的例子:

```
version: 2.1
jobs:
  build_artifact:
    parameters:
      parallelism_qty:
        type: integer
        default: 1
    parallelism: << parameters.parallelism_qty >>
    machine: true
    steps:
      - checkout
workflows:
  build_workflow:
    jobs:
      - build_artifact:
          parallelism_qty: 2 
```

在这个例子中，`build-artifact:`作业有一个用名称`parallelism_qty:`定义的`parameters:`键。此参数的数据类型为整数，默认值为 1。 [parallelism:](https://circleci.com/docs/configuration-reference/#parallelism) 键是`jobs:`对象的一个属性，它定义了在`steps:`列表中产生和执行命令的执行器数量。在这种情况下，特殊的`checkout`命令将在所有产生的执行器上执行。作业的`parallelism:`键已被赋予值` parameters . parallelism _ qty`, which references the` parallelism _ qty:上面定义的参数定义。此示例显示了参数如何为管道构造增加灵活性，并提供了一种方便的方式来集中管理管道语法中重复出现的功能。

## 在作业对象中使用参数

使用前面的例子，工作流块中的`parallelism_qty:`参数演示了如何在配置语法中使用参数。因为`parallelism_qty:`是在作业对象中定义的，所以它可以作为工作流中指定的作业来执行。

`workflows:`块有一个指定了`build_artifact:`的`jobs:`列表。它还为`parallelism_qty:`赋值 2 个执行器，这将产生 2 个执行器并执行`steps:`列表中的命令。如果该值为 3，那么 build_artifact 作业将产生 3 个执行器，并运行命令 3 次。

> 执行器、作业和命令是具有属性的对象，可以通过管道配置语法进行定义、自定义和重用。

## 在管道配置中重用执行器对象

上一节演示了如何在 jobs 对象中定义和使用参数。在这一节中，我将描述如何在执行器中使用参数。执行器定义用于执行管道作业和命令的运行时或环境。执行器对象有一组它们自己的独特属性，参数可以与之交互。这是一个定义和实现[可重用执行器](https://circleci.com/docs/reusing-config/#authoring-reusable-executors)的例子:

```
version: 2.1
executors:
  docker-executor:
    docker:
      - image: cimg/ruby:3.0.2-browsers
   ubuntu_20-04-executor:
    machine:
      image: 'ubuntu-2004:202010-01'
jobs:
  run-tests-on-docker:
    executor: docker-executor
    steps:
      - checkout
      - run: ruby unit_test.rb 
  run-tests-on-ubuntu-2004:
    executor: ubuntu_20-04
    steps:
      - checkout
      - run: ruby unit_test.rb 
workflows:
  test-app-on-diff-os:
    jobs:
      - run-tests-on-docker
      - run-tests-on-ubuntu-2004 
```

这个例子展示了如何在你的管道配置中定义和实现可重用的执行器。我在文件开头使用了`executors:`键来定义 2 个执行者，一个名为`docker-executor:`，一个名为`ubuntu_20-04-executor:`。第一个指定使用 Docker 执行器，第二个指定使用 Ubuntu 20.04 操作系统映像的机器执行器。通过这种方式预定义执行器，开发人员可以创建一个要在管道中使用的执行器资源列表，并集中管理与执行器类型相关的各种属性。例如，[码头执行器](https://circleci.com/docs/reusing-config/#authoring-reusable-executors)具有不属于[机器执行器](https://circleci.com/docs/configuration-reference/#machine)且不可用的属性，因为机器执行器不属于`docker`类型。

> 重用对象将语法量保持在最低限度，同时提供简洁的对象实现，优化代码可读性和功能的集中管理。

`jobs:`块定义了`run-tests-on-docker:`和`run-tests-on-ubuntu-2004:`；两者都有一个指定了值的`executor:`键，作为该作业的适当执行者。`run-tests-on-docker:`作业使用`docker-executor`定义执行其步骤，而`run-tests-on-ubuntu-2004:`作业根据`ubuntu_20-04`定义执行。正如您所看到的，在它们自己的节中预定义这些执行器使得 config 语法更容易阅读，这将使它更容易使用和维护。对执行器的任何更改都可以在各自的定义中进行，并将传播到实现它们的任何作业。这种对定义的执行器的集中管理也可以应用于以类似方式定义的作业和命令对象。

在`workflows:`块中，`test-app-on-diff-os:`工作流并行触发两个作业，这两个作业在各自的执行器环境中执行单元测试。当您想了解应用程序在不同操作系统中的行为时，使用不同的执行器运行这些测试是很有帮助的。这种测试是常见的做法。这里的要点是，我只定义了一次执行者，并在多个作业中轻松地实现了它们。

## 可重复使用的命令对象

命令也可以在配置语法中定义和实现，就像执行器和作业一样。尽管命令对象属性不同于执行器和作业，但是它们的定义和实现是相似的。以下是显示可重复使用的命令的示例:

```
version: 2.1

commands:
  install-wget:
    description: "Install the wget client"
    parameters:
      version:
        type: string
        default: "1.20.3-1ubuntu1"
    steps:
      - run: sudo apt install -y wget=<< parameters.version >>
jobs:
  test-web-site:
    docker:
      - image: "cimg/base:stable"
        auth:
          username: $DOCKERHUB_USER
          password: $DOCKERHUB_PASSWORD
    steps:
      - checkout
      - install-wget:
          version: "1.17.0-1ubuntu1"
      - run: wget --spider https://www.circleci.com
workflows:
  run-tests:
    jobs:
      - test-web-site 
```

在这个例子中，一个可重用的命令已经由一个作业定义和实现。配置顶部的`command:`键定义了一个名为`install-wget:`的命令，该命令安装特定版本的 wget 客户端。在这种情况下，定义一个参数来指定要安装哪个 wget 版本号。如果没有指定值，`default:`键安装`1.20.3-1ubuntu1`的默认值。`steps:`键列出了 1 个`run:`命令，用于安装在`versions:`参数中指定的 wget 版本。`versions:`参数被`<< parameters.version >>`变量引用。

如示例所示，作业对象可以实现和使用定义的命令。`test-web-site:`作业中的步骤节实现了`- install-wget:`命令。它的`version:`参数被设置为 wget 的早期版本，而不是默认的版本值。作业中的最后一个`run:`命令使用 wget 测试来自给定 URL 的响应。这个例子运行一个简单的测试来检查一个网站是否响应请求。

像往常一样，`workflows:`块触发`- test-web-site`作业，该作业执行可重用的`install-wget`命令。就像执行器和作业一样，命令带来了重用代码、集中管理更改以及提高管道配置文件中语法可读性的能力。

## 结论

在这篇文章中，我描述了使用管道参数和可重用管道对象的基础:执行器、作业和命令。关键要点:

*   执行器、作业和命令被视为具有属性的对象，可以在整个管道配置语法中进行定义、自定义和重用
*   重用这些对象有助于保持最少的语法量，同时提供简洁的对象实现，优化代码可读性和功能的集中管理

虽然我写这篇文章是为了向您介绍参数和可重用对象的概念，但我还是想鼓励您阅读一下[可重用配置参考指南](https://circleci.com/docs/reusing-config/)。它将帮助您更深入地了解这些功能，以便您能够充分利用这些出色的功能。

我很想知道你的想法和观点，所以请发推特给我 [@punkdata](https://twitter.com/punkdata) 加入讨论。

感谢阅读！