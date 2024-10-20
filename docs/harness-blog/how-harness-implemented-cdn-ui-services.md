# 如何为其 UI 服务实现 CDN

> 原文：<https://www.harness.io/blog/how-harness-implemented-cdn-ui-services>

在这篇博客中，我们将介绍什么是 CDN，以及 Harness 如何在我们的环境中实现它。

由于复杂的功能和多个协作者，web 应用程序变得越来越难以构建和管理。随着部署复杂性的增加和技术在各行各业的普及，这种效率对于组织的业务目标至关重要。Harness 通过为我们的用户界面(UI)服务实施内容交付网络(CDN)来管理这种复杂性。借助 CDN，我们可以确保平台的速度和稳定性。

在这篇博客中，我们将介绍什么是 CDN，以及 Harness 如何在我们的环境中实现它。我们将讨论我们遇到的一些挑战以及我们如何应对它们。

## 什么是“CDN？”

那么什么是 CDN 呢？CDN 就是分布在全球的一组服务器。根据 Gartner 的数据:

> 内容交付网络(cdn)是一种分布式计算基础设施，其中设备(服务器或装置)驻留在多跳分组路由网络(如因特网)或专用广域网上的多个存在点中。CDN 可用于分发富媒体下载或流，交付软件包和更新，并通过 WAN 优化技术提供诸如全局负载平衡、安全套接字层加速和动态应用加速等服务。

## **为什么需要 CDN 服务器？**

使用案例很简单:

*   **缓存和更快的加载**:资源缓存在 CDN 中，并从地理上离用户最近的服务器上交付，因此资源加载更快。
*   **减少应用服务器上的负载:**从 CDN 缓存和提供资源。这意味着下游应用服务器永远不会收到对这些静态资源的请求，从而降低了负载。

对于 Harness 的 UI 服务，如果没有 CDN，所有的捆绑文件(包括 index.html 和其他静态文件，如 Javascript、样式和图像)都来自应用服务器。但是有了 CDN，只有*index.html*来自应用服务器。剩下的所有静态文件都来自 CDN 服务器(这里是 *static.harness.io* )。

## 你如何实现一个 CDN？

首先，你需要列出你的需求和限制。在 Harness，我们的是:

1.  CDN 应该为我们的下一代 UI 服务提供多种资源文件——js、css、图像和视频
2.  我们应该能够在多种环境下运行它，比如公开的生产环境，还有我们的测试/QA 环境
3.  我们应该为 CDN 提供一个简单的开关来打开或关闭它(以防出错)

记录了这些信息后，我们开始实施，并遇到了一些挑战。

## CDN 面临的挑战

### 挑战 1:运行时与构建时公共路径

我们对不同的环境和*部署类型* (SaaS 和本地)使用相同的构建。我们使用 [Webpack](https://webpack.js.org/) 来捆绑和创建我们的构建。

[公共路径](https://webpack.js.org/guides/public-path/)帮助我们为应用程序中的所有资产设置基本路径。更简单地说，公共路径定义了应该为您的资产提供服务的主机或目录。

早先，我们使用了一个构建时公共路径。Webpack 内置了对定义运行时动态公共路径的支持，所以我们稍微修改了一下代码。我们添加了 CDN 服务器的主机名作为公共路径。

‍

这里 *static.harness.io* 是你的 CDN 主机名。

## 挑战 2:上传到 GCS 步骤与 gsutil

我们的 CDN 托管在谷歌云平台上。我们将静态资产存储在 Google 云存储(GCS)桶中。我们使用[线束 CD](https://harness.io/products/continuous-delivery) 来展开线束。

Harness 内置了对“将工件上传到 GCS”的支持，这是一个基本的“快速启动”的便利步骤。

我们的使用案例还包括需要:

*   压缩文件(g-zip)
*   添加上传文件的自定义权限

为了确保使用上述用例将最新的捆绑文件上传到 GCS，我们不使用“上传工件到 GCS”步骤，而是使用“运行”步骤来添加一个定制脚本。在那个脚本中，我们使用 [*gsutil*](https://cloud.google.com/storage/docs/gsutil) 。

‍ [**gsutil**](https://cloud.google.com/storage/docs/gsutil) 是谷歌创建的一个 Python 应用程序，可以让你从命令行访问云存储。我们用它来压缩(g-zip)捆绑的资产并设置它们的权限。这里有一个片段供参考:

### 挑战 3:网络工作者

我们大量使用 Monaco Editor 的代码编辑功能。如果你用过 Harness，你很有可能见过目视对比 YAML 视图。YAML 观点需要 MonacoEditor。

Monaco Editor 使用 Web Workers 在一个独立于主 JS 线程的线程中运行。

然而，网络浏览器不允许跨域的网络工作者。

这种限制意味着，如果您的网站地址以“app.harness.io”开头，但您的 MonacoEditor 语言的 Javascript 文件试图从“static . harness . io”(CDN)加载，web 浏览器将会阻止它们。一些 MonacoEditor 功能，如自动完成，将无法工作。

#### **试错**

我们尝试了以下方法来解决这个问题:[摩纳哥-编辑器-web pack-插件](https://github.com/microsoft/monaco-editor/tree/main/webpack-plugin#options)和[工人-加载器](https://github.com/webpack-contrib/worker-loader#cross-origin-policy)。

这个[Monaco-editor-webpack-plugin](https://github.com/microsoft/monaco-editor/tree/main/webpack-plugin#options)插件提供了一个类似于 web pack 的 **publicPath** ，它告诉从哪个主机加载 worker 脚本。

但是…这对我们没用。它没有“战胜”Webpack 的运行时公共路径。

接下来，我们尝试了[工人装载机](https://github.com/webpack-contrib/worker-loader#cross-origin-policy)。这是同样的故事。它也没有“战胜”Webpack 的运行时公共路径。

我们也想过更新 monaco-editor-web pack-plugin 的版本，但我们发现新版本与 MonacoEditor 的版本紧密相关——我们必须更新 Monaco-editor 和 monaco-editor-webpack-plugin 的版本。这将成为一个更长的任务。

尽管对 custom languages 的支持让我们感觉很容易，但我们并没有走上这条路。

然后我们想到将 CDN 配置在“app.harness.io”之后——换句话说，不走红色路径，走绿色路径。(如下图。)

这对“*customer . harness . io”*这样的虚荣心网址不起作用，因为由于我们在“ *app.harness.io* 后面配置了 CDN，所以浏览器仍然会阻止工作人员的呼叫

#### 解决办法

我们分两步想出了解决方案。

[工人装载器](https://github.com/webpack-contrib/worker-loader#cross-origin-policy):工人作为斑点装载。

但是 [worker-loader](https://github.com/webpack-contrib/worker-loader#cross-origin-policy) 被弃用/存档以支持 webpack v5，所以我们没有使用它。

我们问自己:如果我们单独构建 worker 文件并手动尝试更改 base-path 会怎么样？这需要自定义 webpack 配置。webpack 中的一个新“入口”,但这意味着摩纳哥文件的加载不再迟缓。

但是我们把这个解决方案做了进一步的改进。我们构建了 workers，但是使用了单独的 webpack 配置。

当从组件内部引用构建的文件时，我们必须使用当前的*窗口。我们去掉了 worker-loader，使用了推荐的 web pack V5 workers 方式(使用 *getWorker()* 和 *new Worker* )。* 

存在潜在的问题，比如 workers 的捆绑文件中没有 hash，所以我们在文件名中手动添加了版本" *2"* (文件名为 *editorWorker2* 和 *yamlWorker2* )。如果我们需要更新工人文件，我们将需要手动更改版本 2——它充当我们的散列。我们认识到这一限制，并且我们接受它，因为自动化这可能是复杂的。

## CDN 和 Harness 的下一步是什么？

解决复杂的挑战并确保成千上万用户的正常运行时间绝非易事。通过实施 CDN，Harness 已经能够通过这种分布式网络模型提高速度并确保正常运行时间。

请关注我们的 CDN 实施的第 2 部分，我们将分享如何为我们的微前端子应用实施 CDN。