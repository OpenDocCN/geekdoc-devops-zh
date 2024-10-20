# 宣布 CircleCI 的 Jenkinsfile 转换器| CircleCI

> 原文：<https://circleci.com/blog/convert-jenkinsfile-circleci-config/>

作为开发人员，我们都能体会到快速使用新工具或框架的经历，以及挫折(和快乐！)那随它来。就个人而言，我可以很容易地回忆起我第一次构建 CircleCI `config.yml`文件的经历，包括 1.0 和 2.0，以及在单独的屏幕上打开 CircleCI [配置参考](https://circleci.com/docs/configuration-reference/)时经历的多次迭代。有比我愿意承认的更多的尝试和错误。

自 CircleCI 2.0 发布以来，我们的团队一直在不断改进 onboarding 体验，以使其更容易上手，包括在 [CircleCI CLI](https://circleci.com/docs/local-cli/#validate-a-circleci-config) 中的功能，以在本地验证`config.yml`文件。我们最新发布的 Jenkinsfile 转换器让这种体验变得更好。

## 什么是 Jenkinsfile 转换器？

Jenkinsfile 转换器是一个将 Jenkinsfile 转换成 CircleCI `config.yml`文件的工具。通过将 Jenkinsfile 加载到转换器中，您将立即收到一个格式化的`config.yml`文件，该文件将带您通过 CircleCI 获得最佳配置。

值得注意的是，尽管该工具尽最大努力转换尽可能多的 Jenkinsfile，但仍有一些关键的差异无法通过该工具解决。例如，CircleCI 没有插件的概念，我们无法支持 Jenkins 宇宙中超过 1400 个不断发展的插件。虽然为了让你的体形达到最佳状态还需要做一点额外的工作，但是 Jenkinsfile converter 会让你一切顺利。让我们看一个例子。

这里我们有一个简单的 Python 项目的 Jenkinsfile:

```
pipeline {
    agent none
    options {
        skipStagesAfterUnstable()
    }
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
            }
        }
        stage('Test') {
            agent {
                docker {
                    image 'qnib/pytest'
                }
            }
            steps {
                sh 'py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py'
            }
            post {
                always {
                    junit 'test-reports/results.xml'
                }
            }
        }
    }
} 
```

将这个 Jenkinsfile 加载到转换器后，我们看到了下面的`config.yml`文件:

```
version: 2.1
jobs:
  build:
    docker:
      - image: cimg/base:2020.08
    steps:
      - run:
          command: python -m py_compile sources/add2vals.py sources/calc.py
  test:
    docker:
      - image: cimg/base:2020.08
    steps:
      - run:
          command: py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py

workflows:
  version: 2
  build-and-test:
    jobs:
      - build
      - test:
          requires:
            - build 
```

这里，发布测试结果的`post`命令没有被传输到 CircleCI `config.yml`文件。我们需要将`store_test_results` [键](https://circleci.com/docs/configuration-reference/#store_test_results)添加到我们的`config.yml`文件中，以便将该功能引入 CircleCI。

```
 test:
    docker:
      - image: qnib/pytest
    steps:
      - run:
          command: py.test --verbose --junit-xml test-reports/results.xml sources/test_calc.py
	- store_test_results:
	    path: test-reports 
```

Jenkinsfile 转换器位于 CircleCI [开发者中心](https://circleci.com/developer)，现在对所有人开放。这个工具将帮助您节省开始使用 CircleCI 的关键时间，无论您只是尝试它，还是转移一个具有 2，000 行 Jenkinsfile 的项目，并且将您从使用试错法来解决两个服务之间的配置问题的诱惑中解救出来。

**注意** : *目前只支持声明式的 Jenkins file，目前还没有计划支持脚本化的 Jenkins file。这是我们的第一个版本，当我们继续迭代这个工具的功能时，我们希望听到你的[反馈](https://support.circleci.com/hc/en-us/requests/new?ticket_form_id=855268&ticket_fields_360009708494=JFC)关于什么工作得好，什么工作得不好。*

要了解有关此功能的更多信息，请访问以下链接: