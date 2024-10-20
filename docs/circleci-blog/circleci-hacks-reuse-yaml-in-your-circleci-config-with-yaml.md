# YAML - config.yml |循环

> 原文：<https://circleci.com/blog/circleci-hacks-reuse-yaml-in-your-circleci-config-with-yaml/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

【2018 年 9 月更新: *写这篇博文是为了演示如何使用一个名为锚和别名的原生 YAML 特性让 CircleCI 配置文件变得更加干燥。现在 CircleCI 配置 v2.1 可以完成这个目标。[来看看](https://circleci.com/docs/reusing-config/)。*

[工作流](https://circleci.com/docs/workflows/)让许多人在他们的配置文件中获得了“工作快乐”。一项工作是“构建”，另一项是“测试”，还有一项是“部署”？也许，那还不算太坏。如果一个扇出、扇入的工作流总共有 5 个作业，每个作业都使用相同的 Docker 图像，会怎么样？在那个 [config.yml](https://circleci.com/docs/sample-config/) 文件中会有冗余。我们可以减少它，让 YAML 为我们工作。

今天，我们将介绍 YAML 1.2 版规范中一个更酷、更鲜为人知的特性，[主播&别名](http://yaml.org/spec/1.2/spec.html#id2765878)。我们将使用它们来减少对以下示例 CircleCI [config.yml](https://circleci.com/docs/sample-config/) 文件的重复编辑:

```
# .circleci/config.yml
version: 2
jobs:
  build:
    docker:
      - image: felicianotech/docker-hugo:0.27.1
    working_directory: ~/project
    steps:
      - checkout
      - run:
          name: "Build Our Statically Generated Docs Website with Hugo"
          command: HUGO_ENV=production hugo -v -s src/
      - persist_to_workspace:
          root: /root/project
          paths:src/public/
  html-testing:
    docker:
      - image: felicianotech/docker-hugo:0.27.1
    working_directory: ~/project
    steps:
      - attach_workspace:
          at: /root/project
      - run:
          name: "Test With HTML Proofer"
          command: echo "This is where we'd do testing with HTML Proofer."
  ui-testing:
    docker:
      - image: felicianotech/docker-hugo:0.27.1
    working_directory: ~/project
    steps:
      - attach_workspace:
          at: /root/project
      - run:
          name: "Test With Selenium or Behat"
          command: echo "This is where we'd run a headless browser and test our site's UI."
  process-testing:
    docker:
      - image: felicianotech/docker-hugo:0.27.1
    working_directory: ~/project
    steps:
      - attach_workspace:
          at: /root/project
      - run:
          name: "Test the commit for org or team processes"
          command: |
            echo "This is where we'd test this commit for things our project wants to enforce"
            echo " such as code formatting, the signing of a CLA, security requirements, etc."
  deploy:
    docker:
      - image: felicianotech/docker-hugo:0.27.1
    working_directory: ~/project
    steps:
      - attach_workspace:
          at: /root/project
      - run:
          name: "Deploy Our Docs Site"
          command: echo "This is where we'd deploy using rsync, awscli, etc."
# Below would be the Workflows specifc config, which we're leaving off for brevity (this example as already lengthy. 
```

## YAML 锚&别名

YAML 允许将一个节点声明为锚点。这意味着该节点将在 YAML 中的某个地方被引用。在上面的配置示例中，您将看到每个作业声明的开头都是为每个作业重复的。具体来说，我们在配置文件中重复以下行 5 次:

```
...
    docker:
      - image: felicianotech/docker-hugo:0.27.1
    working_directory: ~/project
... 
```

相反，我们可以在`.circleci/config.yml`的开头使用一个**定位符**，将这些行设置为作业的默认行:

```
defaults: &defaults
  docker:
    - image: felicianotech/docker-hugo:0.27.1
  working_directory: ~/project
... 
```

然后我们用一个**别名**来写更少的行。这里有一个使用前面的`build`作业的例子:

```
...
  build:
    <<: *defaults
    steps:
      - checkout
      - run:
          name: "Build Our Statically Generated Docs Website with Hugo"
          command: HUGO_ENV=production hugo -v -s src/
      - persist_to_workspace:
          root: /root/project
          paths:src/public/ 
```

## 结果呢

如果我们在整个配置文件中重复这个过程，我们会从最初例子中的 56 行配置变成 47 行。

也许，在这个例子中，减少 9 行并不值得称道，但是我们不仅仅减少了行数:我们已经使维护 YAML 文件变得更加容易。如果我们使用的 Docker 图像用一个新的标签更新，比如说`felicianotech/docker-hugo:0.28`，我们可以在配置的顶部更新图像标签一次，然后就完成了。此工作流中的 5 个作业现在都将使用更新的映像，只有一处更改。

你可以在 [CircleCI 讨论](https://discuss.circleci.com)中继续关于 YAML 主播和别名的讨论。更多高级 YAML 用法的例子可以在[维基百科](https://en.wikipedia.org/wiki/YAML#Advanced_components)上找到。您可以在下面看到修改后的完整配置示例。

* * *

我们使用 YAML 锚点和别名的示例配置文件:

```
# .circleci/config.yml
defaults: &defaults
  docker:
    - image: felicianotech/docker-hugo:0.27.1
  working_directory: ~/project

version: 2
jobs:
  build:
    <<: *defaults
    steps:
      - checkout
      - run:
          name: "Build Our Statically Generated Docs Website with Hugo"
          command: HUGO_ENV=production hugo -v -s src/
      - persist_to_workspace:
          root: /root/project
          paths: src/public/
  html-testing:
    <<: *defaults
    steps:
      - attach_workspace:
          at: /root/project
      - run:
          name: "Test With HTML Proofer"
          command: echo "This is where we'd do testing with HTML Proofer."
  ui-testing:
    <<: *defaults
    steps:
      - attach_workspace:
          at: /root/project
      - run:
          name: "Test With Selenium or Behat"
          command: echo "This is where we'd run a headless browser and test our site's UI."
  process-testing:
    <<: *defaults
    steps:
      - attach_workspace:
          at: /root/project
      - run:
          name: "Test the commit for org or team processes"
          command: |
            echo "This is where we'd test this commit for things our project wants to enforce"
            echo " such as code formatting, the signing of a CLA, security requirements, etc."
  deploy:
    <<: *defaults
    steps:
      - attach_workspace:
          at: /root/project
      - run:
          name: "Deploy Our Docs Site"
          command: echo "This is where we'd deploy using rsync, awscli, etc."
# Below would be the Workflows specific config, which we're leaving off for brevity (this example as already lengthy. 
```