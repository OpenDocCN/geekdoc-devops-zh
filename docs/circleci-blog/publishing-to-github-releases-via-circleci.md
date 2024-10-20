# 使用 CircleCI - CircleCI 自动发布 GitHub

> 原文：<https://circleci.com/blog/publishing-to-github-releases-via-circleci/>

Releases 是 GitHub 的一个特性，它允许你在 GitHub 漂亮的用户界面上展示代码的重要快照，用 git 标签标记。如果您目前没有使用发行版，我想向您展示为什么您可能想要使用，以及如何自动实现它们。

对于 release，您可以获得标签所提供的内容——版本号和描述——但是您也可以获得一个更长的部分用于发布说明，以及一个存储和显示发布工件的地方。这意味着你的软件是二进制的。黛比。每个版本的 rpm 和 AppImage 文件都将由 GitHub 托管，为用户安装您的软件提供了一个方便的地方。

在这篇文章中，我将向你展示如何在 CircleCI 中创建发布。关于更全面的概述，请参见 [GitHub 关于创建版本的文档](https://help.github.com/articles/creating-releases/)。

其核心是，GitHub 版本只是一个 GitHub 特性，位于 git 标签之上。让我们来分解一下:

### Git 标签

Git 标签给你一个版本号来标记一个特定的 git 提交和一个简短的描述。信不信由你，只要把一个 git 标签推送到 GitHub，就会在 GitHub 上你的项目的“Releases”标签里创建一个新的发布。这是一个只包含标签信息的准系统版本。

### 放

发布版通过提供更长的、“丰富的”描述、将标签标记为普通发布版或预发布版的能力，以及最重要的上传工件(如该发布版的二进制文件)的能力，为 git 标签增添了新的功能。我们将从 CircleCI 上传这些艺术品。

## 工具

有几种方法可以将 CircleCI 版本的工件发布到 GitHub 版本:

*   手动使用`curl`和 GitHub API
*   使用 [GitHub 发布](https://github.com/aktau/github-release)
*   使用[github](https://github.com/itchio/gothub)_ 一个 github 释放叉)
*   并且使用 [ghr](https://github.com/tcnksm/ghr) ，这就是我们将在我们的例子中使用的

### 使用 ghr

`ghr`命令的全部细节可以在 GitHub 上的[中找到，但是，我们真的只需要学习一个子命令:](https://github.com/tcnksm/ghr)

```
ghr -t ${GITHUB_TOKEN} -u ${CIRCLE_PROJECT_USERNAME} -r ${CIRCLE_PROJECT_REPONAME} -c ${CIRCLE_SHA1} -delete ${VERSION} ./artifacts/ 
```

这个命令将特定的工件(通常是二进制文件)上传到我们在 GitHub 上发布的 GitHub 版本。让我们来分解一下:

*   这里我们启动命令，并传递一个 GitHub 令牌给它进行认证。这是一个 [GitHub 个人访问令牌](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/)，而不是 OAuth 令牌/密钥。

这是通过您的个人 GitHub 帐户专门创建的，您会希望保护这个令牌，并且只通过一个私有环境变量将它加载到 CircleCI 环境中。

*   这是我们传递 GitHub 用户/组织名称和存储库名称的地方。这两个值都通过内置的 CircleCI 环境变量传递。不需要额外的工作。

*   `-c ${CIRCLE_SHA1}` -这里我们向`ghr`提供 Git 提交散列(在 CircleCI 中作为`$CIRCLE_SHA1`提供)。这告诉它释放是针对哪一个提交，以及标签。

*   `-delete` -删除 git 标签，如果已经存在，则释放。

*   `${VERSION}` -我们通过一个`$VERSION`环境变量设置 git 标签/发布版本。这可以是您想要的任何变量或字符串。通常，这与您的软件版本相同。

*   `./artifacts/`——然后我们设定路径寻找神器。在这种情况下，当前目录内名为`artifacts`的目录中的所有内容。

### 普通 CircleCI 2.0 示例

我们已经介绍了如何使用`ghr`创建 GitHub 版本，但是让我们看看它在 CircleCI 2.0 配置文件中的样子:

```
 publish-github-release:
    docker:
      - image: circleci/golang:1.8
    steps:
      - attach_workspace:
          at: ./artifacts
      - run:
          name: "Publish Release on GitHub"
          command: |
            go get github.com/tcnksm/ghr
            VERSION=$(my-binary --version)
            ghr -t ${GITHUB_TOKEN} -u ${CIRCLE_PROJECT_USERNAME} -r ${CIRCLE_PROJECT_REPONAME} -c ${CIRCLE_SHA1} -delete ${VERSION} ./artifacts/ 
```

### CI 构建 CircleCI 2.0 示例

这是一个名为`publish-github-release`的 CircleCI 作业，它使用了一个 [CircleCI Go 便利图像](https://circleci.com/docs/circleci-images/#go-golang)。这是细目分类:

*   使用[工作空间](https://circleci.com/docs/workflows/#using-workspaces-to-share-data-among-jobs)，我们从之前的工作中为我们的项目引入二进制文件(不在本文中讨论)。
*   这个作业通过`go get`拉取并编译`ghr`(因为`ghr`是用 Go 写的)。
*   它用`my-binary --version`的输出填充`$VERSION`，这是我们这篇文章的示例应用程序。
*   然后我们使用`ghr`命令将`artifacts`目录中的二进制文件上传到 GitHub。

通过使用来自 [CI 构建](https://github.com/cibuilds)的 [`ghr` Docker 图像](https://github.com/cibuilds/github)，我们可以将上述工作缩短几秒钟。这是一个已经安装了`ghr`的轻量级 Docker 映像。下面是新的配置示例:

```
 publish-github-release:
    docker:
      - image: cibuilds/github:0.10
    steps:
      - attach_workspace:
          at: ./artifacts
      - run:
          name: "Publish Release on GitHub"
          command: |
            VERSION=$(my-binary --version)
            ghr -t ${GITHUB_TOKEN} -u ${CIRCLE_PROJECT_USERNAME} -r ${CIRCLE_PROJECT_REPONAME} -c ${CIRCLE_SHA1} -delete ${VERSION} ./artifacts/ 
```

### 工作流示例

下面是一个例子，说明如何在一个[工作流](https://circleci.com/docs/workflows/)中实现上述工作的一个变体，以将标记的提交发布到 GitHub 版本:

```
workflows:
  version: 2
  main:
    jobs:
      - build:
          filters:
            tags:
              only: /^\d+\.\d+\.\d+$/
      - publish-github-release:
          requires:
            - build
          filters:
            branches:
              ignore: /.*/
            tags:
              only: /^\d+\.\d+\.\d+$/ 
```

在这个例子中，当 git 标签被推送时，CircleCI 启动了`publish-github-release`任务。具体如何选择何时发布 GitHub 版本取决于您，但是这里我们将使用一个类似 SemVer 的 git 标签来完成。我们说 SemVer-like 是因为我们使用的标签 regex`/^\d+\.\d+\.\d+$/`匹配诸如`1.2.3`的标签，但是没有考虑到 [SemVer](https://semver.org/) 的所有规则(语义版本)。为了完整起见，下面是根据[rgxdb.com](https://rgxdb.com/r/40OZ1HN5)的说法，一个完整的 SemVer 正则表达式应该是什么样子:

```
/(?<=^[Vv]|^)(?:(?<major>(?:0|[1-9](?:(?:0|[1-9])+)*))[.](?<minor>(?:0|[1-9](?:(?:0|[1-9])+)*))[.](?<patch>(?:0|[1-9](?:(?:0|[1-9])+)*))(?:-(?<prerelease>(?:(?:(?:[A-Za-z]|-)(?:(?:(?:0|[1-9])|(?:[A-Za-z]|-))+)?|(?:(?:(?:0|[1-9])|(?:[A-Za-z]|-))+)(?:[A-Za-z]|-)(?:(?:(?:0|[1-9])|(?:[A-Za-z]|-))+)?)|(?:0|[1-9](?:(?:0|[1-9])+)*))(?:[.](?:(?:(?:[A-Za-z]|-)(?:(?:(?:0|[1-9])|(?:[A-Za-z]|-))+)?|(?:(?:(?:0|[1-9])|(?:[A-Za-z]|-))+)(?:[A-Za-z]|-)(?:(?:(?:0|[1-9])|(?:[A-Za-z]|-))+)?)|(?:0|[1-9](?:(?:0|[1-9])+)*)))*))?(?:[+](?<build>(?:(?:(?:[A-Za-z]|-)(?:(?:(?:0|[1-9])|(?:[A-Za-z]|-))+)?|(?:(?:(?:0|[1-9])|(?:[A-Za-z]|-))+)(?:[A-Za-z]|-)(?:(?:(?:0|[1-9])|(?:[A-Za-z]|-))+)?)|(?:(?:0|[1-9])+))(?:[.](?:(?:(?:[A-Za-z]|-)(?:(?:(?:0|[1-9])|(?:[A-Za-z]|-))+)?|(?:(?:(?:0|[1-9])|(?:[A-Za-z]|-))+)(?:[A-Za-z]|-)(?:(?:(?:0|[1-9])|(?:[A-Za-z]|-))+)?)|(?:(?:0|[1-9])+)))*))?)$/ 
```

阅读更多信息: