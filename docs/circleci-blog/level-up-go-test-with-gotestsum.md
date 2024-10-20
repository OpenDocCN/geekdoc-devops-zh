# 用 gotestsum - CircleCI 升级 go 测试

> 原文：<https://circleci.com/blog/level-up-go-test-with-gotestsum/>

嘿地鼠们！

Go (Golang)的众多优点之一是运行测试非常简单。有了我们在 Go v1.11 中看到的[变化，运行测试就像:](https://circleci.com/blog/go-v1.11-modules-and-circleci/)

```
# To test only your current module
go test ./...

# To test your module as well as its direct and indirect dependencies
go test all 
```

在这篇文章中，我将向你展示如何让`go test`的输出对本地开发和 CircleCI 更有用。

## gotestsum

`gotestsum`是一个用 Go 编写的 [CLI](https://en.wikipedia.org/wiki/Command-line_interface) 工具，为你运行`go test`。它提高了产量，使它更可口。正如其 [GitHub 库](https://github.com/gotestyourself/gotestsum)上所描述的，它“运行测试，并打印友好的测试输出和测试运行的摘要。”

它看起来是这样的:

![](img/b84945c75a80883d3ceebf61ce88dd50.png)

这对于本地开发来说很酷，但是将它用于持续集成怎么样呢？

## CircleCI / JUnit 支持

大多数 CI 提供者支持以 JUnit 格式保存测试结果。CI 提供者用它做什么可以有所不同，但是这通常意味着访问历史测试数据、很酷的图表等。CircleCI 支持[收集测试元数据](https://circleci.com/docs/collect-test-data/)，只要它是 JUnit 格式的。由于`go test`本身没有以 JUnit 格式输出，我们将使用`gotestsum`来完成这项工作。

```
gotestsum --junitfile unit-tests.xml 
```

上面的命令将以 JUnit 格式输出您的测试结果。然后，您可以通过您的 CircleCI `config.xml`文件告诉 CircleCI 使用该文件作为测试元数据。

```
 - store_test_results:
        path: /tmp/test-results 
```

## 安装 gotestsum

### 要求

### 本地安装

与大多数 Go 应用程序一样，运行`gotestsum`可以像在`$PATH`上放置二进制文件一样简单:

```
curl -sSL "https://github.com/gotestyourself/gotestsum/releases/download/v0.3.1/gotestsum_0.3.1_linux_amd64.tar.gz" | sudo tar -xz -C /usr/local/bin gotestsum 
```

上面的命令安装了`gotestsum`的 v0.3.1，这是本文撰写时的最新版本。如果有新版本，请更新 URL 中的版本号。对于 macOS 用户，您也可以将 URL 中的`linux`替换为`darwin`。可用的二进制包适用于 64 位 x86 系统。请参阅下一节来编译您自己的代码。

### 编译二进制文件

如果您有 32 位系统、ARM 或其他需求，以下是编译和安装自定义二进制文件的方法:

```
go get -u gotest.tools/gotestsum 
```

### 安装在 CircleCI 上

如果你使用的是 [Golang CircleCI Docker 便利镜像](https://hub.docker.com/r/circleci/golang/) ( `circleci/golang`)，那么恭喜你，`gotestsum`已经为你预装了！如果没有，我建议遵循[本地安装](#installing-locally)指令，并将其应用到您的 CircleCI `config.yml`文件中，或者对于自定义映像，应用到 docker 文件中。

`gotestsum`是由 CircleCI 的工程师[丹尼尔·内芬](https://github.com/dnephin)维护的项目。