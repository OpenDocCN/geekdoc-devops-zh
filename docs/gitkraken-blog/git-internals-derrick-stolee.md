# Git 内部| git kon 2022 | GitHub 的 Derrick Stolee

> 原文：<https://www.gitkraken.com/gitkon/git-internals-derrick-stolee>

[https://www.youtube.com/embed/6b6iOPvspPs?feature=oembed](https://www.youtube.com/embed/6b6iOPvspPs?feature=oembed)

视频

开发人员使用 Git 进行代码协作:共享代码，在本地机器上独立工作，然后将工作整合到一个公共存储库中。对于许多人来说，这是使用非常传统的步骤和工作流程来完成的，在大多数情况下效果非常好。

但是，当你需要打破传统模式，做一些新的事情时，会发生什么呢？了解更多关于 Git 内部的知识会让您更加得心应手，特别是当您的项目扩展时，您必须深入到更高级的特性中，比如索引和查询计划。

## **Git 是您工程系统的核心分布式数据库**

Git 与您的应用程序数据库共享一些非常基本的概念:

1.  数据保存在磁盘上。
2.  查询允许用户基于该数据请求信息。
3.  数据存储针对这些查询进行了优化。
4.  查询算法被优化以利用这些结构。
5.  分布式节点需要同步并在一些公共状态上达成一致。

虽然这些概念对于所有数据库都是通用的，但是 Git 是特别专用的，因为它是为存储纯文本源代码文件而构建的。

**Git 对象**

## Git 对象可以说是 Git 中最基本的概念，就像你的 [Git 库](https://www.gitkraken.com/learn/git/tutorials/what-is-a-git-repository)的原子一样，可以以有趣的方式组合。

了解更多关于 [Git 对象](https://github.blog/2022-08-29-gits-database-internals-i-packed-object-store/)以及如何使用它们的信息。

了解更多关于 [Git 对象](https://github.blog/2022-08-29-gits-database-internals-i-packed-object-store/)以及如何使用它们的信息。

**Git 历史查询**

## Git 在允许开发人员搜索和调查他们的存储库的历史方面起着非常重要的作用。当您开始将 Git 视为一个数据库时，这些历史调查形成了一种有趣的查询类型。

Git 在允许开发人员搜索和调查他们的存储库的历史方面起着非常重要的作用。当您开始将 Git 视为一个数据库时，这些历史调查形成了一种有趣的查询类型。

**Git 提交历史查询**

## 历史查询不仅是一种有趣的查询类型，而且 [Git commit](https://www.gitkraken.com/learn/git/commit) history 呈现了有趣的数据形状，告知 Git 的算法如何满足这些查询。

了解更多关于基于提交的 Git 历史查询的信息。

了解更多关于基于提交的 Git 历史查询的信息。

**Git 文件历史查询**

## 在对大型软件系统进行修改之前，理解代码为什么处于当前状态是至关重要的。仅查看 [Git 提交消息](https://www.gitkraken.com/learn/git/best-practices/git-commit-message)不足以发现修改了特定文件或该文件中某些行的更改。Git 的文件历史命令帮助用户找到引入变更的重要时间点。

了解更多关于 [Git 文件历史命令](https://github.blog/2022-08-31-gits-database-internals-iii-file-history-queries/)以及如何使用它们。

下载 CTA:使用 GitKraken Client 可以轻松直观地搜索您的项目历史，无论是提交还是文件信息，为您提供真正理解代码库所需的可见性。

下载 CTA:使用 GitKraken Client 可以轻松直观地搜索您的项目历史，无论是提交还是文件信息，为您提供真正理解代码库所需的可见性。

**Git 中的分布式同步**

## Git 的分布式本质来自于它的去中心化架构。每个存储库可以独立工作，无需连接到中央服务器。Git 使用几种机制来高效地计算一小组可共享对象，而不需要交换双方的完整对象列表。这样做需要利用对象存储的形状，包括提交历史、树遍历和自定义数据结构。

对于大多数分布式系统来说，网络分区应该很少而且很短，即使是不可避免的。对于 Git，分区是默认状态。每个用户选择何时跨这些分布式副本同步信息。即使它们连接上了，也可能只是部分更新，例如当用户将本地分支之一推送到远程时。

有了默认断开连接的想法，Git 需要以不同于其他数据库的方式考虑其同步机制。每个副本可以有非常不同的状态，每个同步可以有不同的目标状态。

了解更多关于 [Git 的分布式同步](https://github.blog/2022-09-01-gits-database-internals-iv-distributed-synchronization/)的信息。

了解更多关于 [Git 的分布式同步](https://github.blog/2022-09-01-gits-database-internals-iv-distributed-synchronization/)的信息。

**使用 Git 扩展您的数据库**

## 当数据库接近单个数据库节点的规模限制时，一种常见的策略是通过将数据库分成多个组件来对其进行分片。与 Git 类似，当存储库变得太大时，一些人选择分割存储库。

了解如何通过共享一个 Git 存储库来安全地[扩展您的 Git 数据库](https://github.blog/2022-09-02-gits-database-internals-v-scalability/),以及在开始之前您想要考虑的因素。

了解如何通过共享一个 Git 存储库来安全地[扩展您的 Git 数据库](https://github.blog/2022-09-02-gits-database-internals-v-scalability/),以及在开始之前您想要考虑的因素。

**了解 Git 内部机制**

## 随着您作为软件开发人员的成长，获得对 Git 及其内部工作方式的更全面的理解将会带来巨大的好处。使用像 [GitKraken Client](https://www.gitkraken.com/git-client) 这样增加项目历史可见性的工具可以让你对工作流程更有信心，对代码有更深入的了解。

随着您作为软件开发人员的成长，获得对 Git 及其内部工作方式的更全面的理解将会带来巨大的好处。使用像 [GitKraken Client](https://www.gitkraken.com/git-client) 这样增加项目历史可见性的工具可以让你对工作流程更有信心，对代码有更深入的了解。