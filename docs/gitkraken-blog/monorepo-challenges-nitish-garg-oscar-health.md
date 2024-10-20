# Monorepo 挑战 CI | GitKon 2022 | Nitish Garg，Oscar Health

> 原文：<https://www.gitkraken.com/gitkon/monorepo-challenges-nitish-garg-oscar-health>

[https://www.youtube.com/embed/neP8TVHr4pc?feature=oembed](https://www.youtube.com/embed/neP8TVHr4pc?feature=oembed)

视频

使用大型 Git monorepo 会带来持续集成的挑战，但是您和您的团队可以使用一些技术来克服常见的障碍。

## 什么是单向回购？

作为 monorepo 的一个例子，让我们看看 Oscar Health 的资料库背后的数字，这是一家全栈健康保险公司。该组织的 Git monorepo 包含:

*   10 万份文件
    *   一千九百万行代码
        *   各 500 万行 Python 和 SQL
        *   77 万行戈朗

*   微服务
    *   2300 个二元目标
    *   12，000 次库测试
    *   6100 个测试目标

*   CI/CD
    *   每月 5000 次部署
    *   一个月 400 万次测试
    *   每月 15TB 工件

**与 Monorepos 的持续集成挑战**

## 随着大型 monorepos 的持续增长，团队可能会面临持续集成(CI)的挑战。例如，每天运行整个测试套件不再是一个选项。

monorepos 常见的持续集成挑战包括:

在 [Git diff](https://www.gitkraken.com/learn/git/git-diff) (或 pull 请求)上计算更改的目标

*   远程 CI 服务器上的缓存:
*   [Git 克隆](https://www.gitkraken.com/learn/git/git-clone)整个 monorepo 以及在远程 CI 服务器上从零开始获取相关第三方随着您的 repo 增长需要更长时间
    *   您必须找出缓存技术，以确保可以在远程 CI 服务器上访问大型对象
    *   您必须找出缓存技术，以确保可以在远程 CI 服务器上访问大型对象

**计算变更目标**

### 第一步是在 Git monorepo 中找到更改的文件。接下来，您将需要从更改的文件中找到所有依赖于测试和二进制目标，并对它们进行 repo 测试。后一步可能会变得复杂。

计算单一回购变更目标的结构如下所示:

你有四个改变的目标:A -> B -> C

因此，如果文件 C 有变化；这将改变目标 A、B、C 和 D；如果文件 B 有变化；这将改变 A & B 的目标；如果文件 A 有变化；这会改变目标 a。

例如，在 Oscar Health monorepo 中，每个构建或 diff 都可能有 10，000 个需要测试的目标，但平均来说，他们只需要使用这个计算模型测试几百个目标。

例如，在 Oscar Health monorepo 中，每个构建或 diff 都可能有 10，000 个需要测试的目标，但平均来说，他们只需要使用这个计算模型测试几百个目标。

**远程 CI 服务器上的缓存**

### 当您使用 monorepo 时，两种类型的对象将随着时间的推移继续线性增长:monorepo 本身和 monorepo 中定义的第三方。

根据远程 CI 服务器的类型，缓存这些类型的大型对象的技术会有所不同，无论是长期运行的还是临时的，后者更复杂。

对于长期运行的服务器，您可以采用以下策略:

克隆 monorepo 一次，并使用 [Git 命令](https://www.gitkraken.com/learn/git/commands)获取新的更改

*   必要时同步第三方
*   必要时同步第三方

Ad hoc 服务器本质上是自动伸缩的，在工作流中使用它们时，您需要更加慎重，并且需要使用缓存技术。

对于 ad hoc 服务器，您可以采取的一个步骤是在半夜构建一个 Docker 映像来备份基础包，克隆 monorepo，然后 [Git fetch](https://www.gitkraken.com/learn/git/git-fetch) 所有第三方。接下来，将它提交给一个容器以获得一个图像，然后缓存该图像。

对于 ad hoc 服务器，您可以采取的一个步骤是在半夜构建一个 Docker 映像来备份基础包，克隆 monorepo，然后 [Git fetch](https://www.gitkraken.com/learn/git/git-fetch) 所有第三方。接下来，将它提交给一个容器以获得一个图像，然后缓存该图像。