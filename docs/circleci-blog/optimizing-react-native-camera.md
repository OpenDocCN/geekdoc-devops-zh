# 优化 React 原生相机的构建| CircleCI

> 原文：<https://circleci.com/blog/optimizing-react-native-camera/>

上个月，[我的一个拉取请求](https://github.com/react-native-community/react-native-camera/pull/2550)被合并到流行的社区项目 [React Native Camera](https://github.com/react-native-community/react-native-camera) 中，以回应[一个维护者的推文](https://twitter.com/sseraphini/status/1184824377206067200)。React Native Camera 的构建被阻止，因为他们试图使用自由/OSS 计划中不可用的资源类。除了解除阻塞，我还做了一些额外的优化。

我在这篇 PR 中使用的策略可以被推广并用于优化许多其他项目。在这篇文章中，我将分解我对 React 原生相机项目所做的一些更改，解释它们如何提高性能，并建议您可以使用类似的技术来改进自己的项目。

## 缓存步骤放置

一般来说，人们习惯于将他们的`save_cache`语句放在构建的末尾。React Native Camera 也是这样做的，就像这样:

```
jobs:
  build-app:
    # ...environment setup here
    steps:
      - checkout
      - restore_cache:
          keys:
            - v1-npm-{{ .Branch }}-{{ checksum "yarn.lock" }}
            - v1-npm
      - run:
          name: Install Dependencies
          command: yarn install --ignore-engines
      # ...other steps, etc.
      - save_cache:
          key: v1-npm
          paths:
            - node_modules/
      - save_cache:
          key: v1-npm-{{ .Branch }}-{{ checksum "yarn.lock" }}
          paths:
            - node_modules/
      # ...rest of config 
```

这不是最佳选择，因为如果作业中的任何其他步骤在`save_cache`步骤有机会执行之前失败，将不会保存缓存。最好将`save_cache`步骤放在依赖步骤之后——这保证了只要依赖步骤工作，缓存就会被保存:

```
jobs:
  build-app:
    # ...environment setup
    steps:
      - checkout
      - restore_cache:
          keys:
            - v1-npm-{{ .Branch }}-{{ checksum "yarn.lock" }}
            - v1-npm
      - run:
          name: Install Dependencies
          command: yarn install --ignore-engines
      #### MOVED
      - save_cache:
          key: v1-npm
          paths:
            - node_modules/
      - save_cache:
          key: v1-npm-{{ .Branch }}-{{ checksum "yarn.lock" }}
          paths:
            - node_modules/
      #### /MOVED
      # ...rest of config 
```

您可以找到更多关于缓存的信息，并在我们的文档中查看示例[。](https://circleci.com/docs/caching/)

## 为 Gradle 添加缓存

我做的另一个改变是为 Gradle 依赖项添加缓存。他们在 CircleCI 中的`Run Checks`和`Build Sample App`步骤是 Gradle 包装器执行，它们占用了大部分构建时间。在下面的配置中，我存储了 Gradle 缓存和包装器。

```
jobs:
  build-app:
    # ...environment setup
    steps:
      # ...previous build steps
      #### ADDED
      - restore_cache:
          keys: 
            - v1-gradle-{{ checksum "android/gradle/wrapper/gradle-wrapper.properties" }}-{{ checksum "examples/basic/android/gradle/wrapper/gradle-wrapper.properties" }}
            - v1-gradle-wrapper
      - restore_cache:
          keys:
            - v1-gradle-cache-{{ checksum "android/build.gradle" }}-{{ checksum "examples/basic/android/build.gradle" }}
            - v1-gradle-cache
      #### /ADDED
      - run:
          name: Run Checks
          command: |
            cd android
            chmod +x ./gradlew && ./gradlew check
      # ...other step(s)
      - run:
          name: Run Yarn to Generate react.gradle
          command: cd examples/basic/android && yarn
      - run:
          name: Build Sample App
          command: |
            cd examples/basic/android && chmod +x ./gradlew && ./gradlew build
      #### ADDED
      - save_cache:
          key: v1-gradle-wrapper-{{ checksum "examples/basic/android/gradle/wrapper/gradle-wrapper.properties" }}
          paths:
            - ~/.gradle/wrapper
      - save_cache:
          key: v1-gradle-cache-{{ checksum "examples/basic/android/build.gradle" }}
          paths:
            - ~/.gradle/caches
      #### /ADDED
      # ...rest of config 
```

## 从报表制作工件

您知道我们的工件就像一个静态文件服务器吗？这对于存储测试套件的 HTML 报告和像访问网页一样访问它们是很有用的。我添加了一个工件步骤来存储林挺命令生成的输出。这允许你在[工件标签](https://app.circleci.com/pipelines/github/react-native-community/react-native-camera/jobs/1143)上浏览工件，并像访问网页一样访问那些文件。

```
jobs:
  build-app:
    # ...environment setup
    steps:
      # ...previous build steps
      - run:
          name: Run Checks
          command: |
            cd android
            chmod +x ./gradlew && ./gradlew check
      #### ADDED
      - store_artifacts:
          path: android/build/reports
      #### /ADDED
      # ...other step(s)
      - run:
          name: Build Sample App
          command: |
            cd examples/basic/android && chmod +x ./gradlew && ./gradlew build
      #### ADDED
      - store_artifacts:
          path: examples/basic/android/app/build/reports
          destination: app
      #### /ADDED
      # ...rest of config 
```

你可以在我们的文档中找到更多关于创建工件[的信息。](https://circleci.com/docs/artifacts/)

## 结论

在这个 pull 请求中所做的小改动只是您可以用来优化 CircleCI 构建的一些特性的例子。[我们平台](https://circleci.com/docs/)的文档内容丰富，包含各种示例和教程。T2 的 CircleCI 博客也是一个很好的资源。如果你被困住了，试着发微博给我们 [@CircleCI](https://twitter.com/CircleCI) ！

要了解更多优化特性和技术，请阅读我们在 CircleCI 上发布的[优化开源项目。](https://circleci.com/blog/optimizing-open-source-projects-on-circleci/)

感谢阅读，并快乐建设！