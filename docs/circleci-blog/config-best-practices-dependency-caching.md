# 配置最佳实践:依赖项缓存| CircleCI

> 原文：<https://circleci.com/blog/config-best-practices-dependency-caching/>

让我们面对现实:创建最佳 CI/CD 工作流并不总是一项简单的任务。事实上，编写有效和高效的配置代码是许多开发人员在 DevOps 之旅中面临的最大障碍。但是，您不需要成为专家就可以建立快速、可靠的测试和部署基础设施。通过一些简单的技巧，您可以优化您的`config.yml`文件并释放您的 CI/CD 管道的全部潜力。

在本系列中，我们将深入探讨我们的解决方案工程师在与企业级客户进行一对一配置审查时提出的一些常见建议。今天，我们重点关注**依赖缓存**，这是一种强大的技术，用于保存作业之间的构建数据，并加速您的工作流。

## 什么是依赖缓存？

用一般的计算术语来说，**缓存**是一个过程，其中频繁使用的数据被存储在内存中，以便可以快速检索供将来使用。在 CI/CD 管道中，您通常需要在构建阶段安装相同的依赖项，即运行应用程序所需的库或包。为了避免每次运行工作流时重复下载相同的依赖项，您可以使用**依赖项缓存**来保存这些包，以便在将来的作业中使用。

缓存作业之间的依赖关系可以减少几秒甚至几分钟的构建时间。提高构建速度会导致更少的时间浪费和更快的测试反馈，允许您和您的团队快速有效地向您的用户发布变更。

## 什么时候应该使用依赖项缓存？

缓存依赖项是我们向希望加快工作流速度的用户提出的最常见的建议之一。这种技术对于依赖于包管理器的项目特别有用，例如 Node.js 的 npm 和 Yarn、Python 的 pip 和 Ruby 的 Bundler，以便在构建过程中安装多个二进制文件。

例如，使用 React 和 Node.js 编写的全栈 web 应用程序可以快速积累数十、数百甚至数千个依赖项和子依赖项。如果没有依赖项缓存，下面的`config.yml`文件将在每次`build_and_test`作业运行时下载应用程序所需的每个包:

```
jobs:
  build_and_test:
    docker:
      - image: cimg/node:16.11.1
    steps:
      - checkout
      # install dependencies
      - run:
          name: install dependencies
          command: npm install
      # run test suite	
      - run:
          name: test
          command: npm run test 
```

此配置缺少依赖项缓存。它将 CircleCI 的 [Node.js Docker image](https://circleci.com/developer/images/image/cimg/node) 设置为执行环境，并在安装必要的依赖项和运行为项目定义的测试之前，从相关的 Git 存储库中检出应用程序代码。因为没有设置依赖项缓存，`build_and_test`作业将在每次工作流运行时从头开始，不必要地一遍又一遍安装相同的依赖项。

## 如何向管道添加依赖项缓存

向 CircleCI 工作流添加依赖关系缓存非常简单，只需为您创建的每个版本的缓存设置`restore_cache`和`save_cache`步骤以及唯一标识符或键。这是您在上面看到的相同配置，这次使用依赖项缓存进行了优化:

```
...
jobs:
  build_and_test:
    docker:
      - image: cimg/node:16.11.1
    steps:
      - checkout
      # look for existing cache and restore if found
      - restore_cache:
          key: v1-deps-{{ checksum "package-lock.json" }}
      # install dependencies    
      - run:
          name: install dependencies
          command: npm install
      # save any changes to the cache
      - save_cache:
          key: v1-deps-{{ checksum "package-lock.json" }}
          paths: 
            - node_modules   
      # run test suite
      - run:
          name: test
          command: npm run test 
```

这个配置现在包括了依赖缓存。`restore_cache`步骤检查现有的缓存，如果找到，就将其恢复到作业中。在这种情况下，`npm install`将只安装那些不在缓存中的依赖项。然后，在加载完项目的所有依赖项之后，`save_cache`步骤会将更新后的依赖树保存到`node_modules`目录中的一个新缓存中。

注意，`restore_cache`和`save_cache`都包含密钥标识符。键的`{{ checksum "package-lock.json" }}`部分是一个动态值，称为**模板**。这个特殊的模板计算项目的`package-lock.json`文件内容的 SHA256 散列，并将`v1-deps-`添加到结果中。如果`package-lock.json`(或者您在 cache-key 中指定的任何依赖关系管理文件)发生变化，那么`restore_cache`将会丢失，并且`save_cache`将会创建一个新的缓存。

关于这个例子和一般的依赖关系缓存，还有几件重要的事情需要注意:

*   缓存是不可变的。一旦`save_cache`写入给定的键，它就不能被覆盖。

*   在运行测试之前包含`save_cache`步骤*是很重要的，这样即使测试失败，你的依赖项也会被保存。*

*   除了依赖关系管理文件的校验和值之外，还可以在模板中使用其他动态信息，包括正在构建的 VCS 分支、CircleCI 作业号和构建的纪元时间。欲了解更多信息，请使用键和模板查阅关于[的文档。](https://circleci.com/docs/caching/#using-keys-and-templates)

最后，如果只有少数依赖关系发生了变化，但缓存的其余部分是有效的，则可以通过设置一个**回退键**来恢复缓存的一部分:

```
- restore_cache:
        keys:
          - v1-deps-{{ checksum "package-lock.json" }}
          - v1-deps- 
```

在这个例子中，CircleCI 将首先尝试加载与当前版本的`package-lock.json`相关联的缓存。如果锁文件由于添加了依赖项而发生了更改，那么将找不到缓存。接下来，CircleCI 将使用静态回退键`v1-deps-`加载带有`v1-deps-`前缀的最新有效缓存。一旦加载了之前的缓存，`npm install`将下载任何缺失的依赖项。

有关部分缓存恢复的更多信息，请查看[文档](https://circleci.com/docs/caching/#partial-dependency-caching-strategies)。

## 结论

依赖缓存是优化 CircleCI 配置的最直接有效的方法之一。通过对您的工作流程进行一些小的调整，您可以节省大量的构建时间，让您专注于做您最擅长的事情:快速地向您的用户交付价值。

依赖项缓存只是您可以对配置进行的许多不同优化中的一种。其他基于缓存的优化包括将数据保存在工作区中，以及设置 Docker 层缓存来加速 Docker 构建。在本系列的后续文章中，请关注对这些特性和其他特性的深入研究。

如果您对 CircleCI 配置的专家级审查感兴趣，您可以通过注册[高级支持计划](https://circleci.com/support/plans/)从专门的支持工程师那里获得个性化的一对一评估。