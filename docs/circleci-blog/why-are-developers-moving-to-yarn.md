# 为什么开发商要转向 Yarn - CircleCI

> 原文：<https://circleci.com/blog/why-are-developers-moving-to-yarn/>

**来自出版商的说明:**您已经找到了我们的一些旧内容，这些内容可能已经过时和/或不正确。尝试在[我们的文档](https://circleci.com/docs/)或[博客](https://circleci.com/blog/)中搜索最新信息。

* * *

随着 Npm@5 和 NodeJS V8 的发布，下面的一些信息已经过时。

JavaScript 世界每秒都在变化。说过去一年发生了很多事情是轻描淡写。这些变化包括 [Angular2 发布](https://blog.thoughtram.io/angular/2016/09/15/angular-2-final-is-out.html)， [Node.js 7.0.0 发布](https://nodejs.org/en/blog/release/v7.0.0/)，以及 [ECMAScript 2016 (ES7)](http://www.ecma-international.org/ecma-262/7.0/index.html) 的特性集最终确定。2016 年 10 月，脸书、Exponent、谷歌和 Tilde 发布了一个意想不到的东西，一个被他们称为[纱](https://yarnpkg.com/)的 [npm](https://github.com/npm/npm) 替代品。

## Npm 问题和纱线解决方案

Yarn 不是 npm 的一个分支，而是它的一个重新想象。大型项目——比如脸书和谷歌的项目——放大了开发人员在使用`npm`时可能面临的问题。

### 国家预防机制的潜在问题

*   嵌套依赖关系(在 npm 3 中已修复)
*   程序包的串行安装
*   单包注册表(npmjs.com 有没有为你倒下过？)
*   需要网络来安装软件包(虽然我们可以创建一个临时的缓存)
*   允许包在安装时运行代码(不利于安全)
*   不确定的包状态(您不能确定项目的所有副本都将使用相同的包版本)

### 纱线解决方案

*   多个注册表——Yarn 从`npmjs.com`和 [Bower](https://bower.io/) 读取并安装包。如果其中一个发生故障，您的项目可以继续在 CI 中构建，不会出现问题
*   平面依赖结构——更简单的依赖解析意味着纱线完成得更快，并且可以被告知使用特定包的单一版本，这使用更少的磁盘空间
*   自动重试-单个网络请求失败不会导致安装失败。失败时会重试请求，从而减少由于临时网络问题而导致的红色构建
*   并行下载——Yarn 可以并行下载包，减少构建运行的时间
*   与 npm 完全兼容-从 npm 转换到纱线是一个无摩擦的过程
*   `yarn.lock` -将依赖关系锁定到特定版本，类似于 Ruby 世界中的 Gemfile.lock

### 纱线锁

Yarn 引入了一个锁文件`yarn.lock`，用来管理 JavaScript 包。对于团队工作的开发人员来说，这可能是 Yarn 最有用的特性。在`package.json`中，包版本可以被指定为一个范围，或者根本没有版本。这可能会导致一个问题，即同一个团队的不同开发人员使用同一个包的不同版本。任何接受过 CI 培训的工程师都知道，用完全相同的依赖关系复制一个环境的能力，对于有效的调试和新团队成员的加入是至关重要的。Yarn 从包管理器如 [Bundler](http://bundler.io/) 那里借用，创建了一个`yarn.lock`文件，记录你在项目中使用的每个包的确切版本。将这个锁文件提交到您的 VCS 可以确保所有从事该项目的开发人员，如果他们使用 Yarn，将会使用每个包的相同版本。

## 在圈圈上使用纱线

从 npm 切换到 Yarn 是没有痛苦的，因为它们是兼容的。我们第一次写关于 Yarn 的文章是在[12 月](https://circleci.com/blog/install-and-use-yarn-the-npm-replacement-on-circleci/)，在那里我们展示了下载和安装 Yarn 用于您的构建的最佳方式。从那以后，我们继续展示我们对 Yarn 的支持:Yarn 现在已经预装在我们的 [CircleCI Ubuntu 14.04(可信)映像](https://circleci.com/docs/1.0/build-image-trusty/#yarn)中。通过简单地使用`yarn`命令，Yarn 可以像您的本地环境一样在 CircleCI 上使用。缓存说明可在我们的[纱线文档](https://circleci.com/docs/1.0/yarn/)中找到。

## npm 至纱线备忘单

| npm | 故事 |
| npm 安装 | 故事 |
| -从 package.json 安装项目依赖项 |
| npm 安装[包] | 不适用的 |
| -安装特定的包，但不将其作为依赖项保存在 package.json 中 |
| npm 安装-保存[包] | 纱线添加[包装] |
| -安装一个包，并将其作为依赖项保存在 package.json 中 |
| npm 安装-保存-开发[包] | 纱线添加[包装] [ - dev/-D] |
| -安装一个包，并将其专门作为开发依赖项保存在 package.json 中 |
| 不适用的 | 纱线添加[包装] [ - peer/-P] |
| -安装一个包并将其保存在 package.json 中，专门作为一个 Yarn 对等依赖项 |
| npm 安装-保存-可选[软件包] | 纱线添加[包装] [ -可选/-O] |
| -安装一个包，并将其保存在 package.json 中，作为可选的依赖项 |
| npm 安装-保存-精确[包] | 纱线添加[包装] [ -精确/-E] |
| -安装软件包的特定版本，而不是安装最新版本的默认行为 |
| 不适用的 | 纱线添加[包装] [ -颚化符/-T] |
| -安装具有指定主要版本的软件包的最新次要版本(即 yarn add foo@1.2.3 -T 将安装与 1.2.x 匹配的最新版本) |
| npm 安装-全局[软件包] | 纱线全局添加[包] |
| -在您的本地计算机上全局安装一个包，通常用于开发人员工具 |
| npm 重建 | 纱线安装力 |
| -重建所有软件包，即使已经下载 |
| npm 卸载[软件包] | 不适用的 |
| -卸载软件包，但不将其从 package.json 中删除 |
| npm 卸载-保存[包] | 纱线去除器[包装] |
| -卸载一个包并将其从 package.json 中删除(不考虑 Yarn 中的依赖类型) |
| npm 卸载-保存-开发[包] | 不适用的 |
| -卸载软件包，并将其作为开发依赖项从 package.json 中移除 |
| npm 卸载-保存-可选[软件包] | 不适用的 |
| -卸载软件包，并将其作为可选依赖项从 package.json 中删除 |
| rm -rf 节点模块和 npm 安装 | 纱线升级 |
| -将软件包升级到最新版本 |

## 摘要

Yarn 解决了诸如不确定的依赖性、网络问题/npmjs 关闭以及并行下载等问题，以便提供比 npm 更大的价值。然而，新政治机制是自身成功的受害者。随着越来越多的人转向 Yarn 和其他注册中心，npm 服务器将变得更加可用。两个包经理都很棒，最终会互相提高。

***好玩的事实:*** *在你的本地机器上，npm 可以安装 Yarn！`npm install --global yarn`。纱线团队不推荐这种安装方法。*