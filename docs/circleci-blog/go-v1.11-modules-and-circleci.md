# 在 CircleCI - CircleCI 中启用 Go v1.11 的模块支持

> 原文：<https://circleci.com/blog/go-v1.11-modules-and-circleci/>

Go v1.11 于 2018 年 8 月 24 日发布。有相当多的变化！虽然有几个变化我真的很喜欢，比如能够[分配嵌套变量](https://golang.org/doc/go1.11#text/template)，但也有一些变化会影响你的 CircleCI 构建。

## 在版本中启用模块支持

CircleCI 上一个典型的 Go 项目的配置是这样开始的:

```
version: 2
jobs:
  build:
    docker:
      - image: circleci/golang:1.10
    working_directory: /go/src/github.com/my-org/my-repo
    steps:
      - checkout 
```

简单地将 Docker 映像更改为`circleci/golang:1.11`将**而不是**启用模块支持。相反，有两种不同的方法可供您使用:

1.  将您的代码移出`GOPATH`
2.  将`GO111MODULE`环境变量设置为`"on"`

### 方法 1 -将代码移出 GOPATH

使用 Go v1.11 Docker 映像，通过将代码移动到`GOPATH`之外的路径来启用模块支持。例如:

```
version: 2
jobs:
  build:
    docker:
      - image: circleci/golang:1.11
    working_directory: /my-app
    steps:
      - checkout 
```

更好的是，除非你绝对需要指定一个`working_directory`，否则我会把它完全从配置中去掉。当你省略它时，CircleCI 默认为`~/project`，并且在你的配置中少了一行。这是我推荐的启用模块支持的方法:

```
version: 2
jobs:
  build:
    docker:
      - image: circleci/golang:1.11
    steps:
      - checkout 
```

### 方法 2 -使用 go 111 模块

如果您喜欢将项目代码保存在`GOPATH`中，可以通过环境变量启用模块支持。这里有一个例子:

```
version: 2
jobs:
  build:
    docker:
      - image: circleci/golang:1.11
        environment:
          GO111MODULE: "on"
    working_directory: /go/src/github.com/my-org/my-repo
    steps:
      - checkout 
```

注意用于包装`"on"`的双引号。对于许多人来说，不使用引号会导致 YAML 将值解释为`true`，而 Go 不接受这个值为有效值。

## 在存储库中启用模块支持

为了让 CircleCI 构建工作，您的存储库也必须构建为支持 Go 模块。关于这一点有很多要说的，你可以在 [Go Wiki](https://github.com/golang/go/wiki/Modules) 上详细阅读。如果你的项目比副业项目大，我建议你通读一遍。下面是简短的版本。

准备您的代码:

```
go mod init 
```

如果您来自另一种流行的 Go vendoring 方法，如 Dep，则可以跳过下一步。否则，需要执行以下步骤来创建一个完整的`go.mod`文件:

```
go mod tidy 
```

现在，将 mod 管理文件添加到 Git 是最后一步:

```
git add go.mod go.sum
git commit -m "Add Go module support."
git push 
```

如果您之前没有使用依赖管理器，而只是使用`go get`来获取包，那么现在您可以从 CircleCI 配置中删除它。

## 测试

有两种形式的`go test`供你考虑:

```
go test ./...  # Tests only the code in your current module, typically your repo.
go test all    # This will test your code as well as direct and in-direct dependancies. 
```

## 贮藏

使用 Go 模块时，缓存位置会发生变化。以下是恢复和保存缓存的方法:

```
 - restore_cache:
          keys:
            - go-mod-v1-{{ checksum "go.sum" }}

# ...

      - save_cache:
          key: go-mod-v1-{{ checksum "go.sum" }}
          paths:
            - "/go/pkg/mod" 
```

好运，地鼠们。:)

## 更多资源