# 什么是 ChartMuseum，我们如何在 CD 中使用它？装具

> 原文：<https://www.harness.io/blog/what-is-chart-museum>

有时候，舵轮图是不够的，我们需要一点帮助。进入 ChartMuseum。

通过部署可扩展的内部和外部基础设施和解决方案，利用连续交付使组织能够更快地对市场和客户做出响应。为了构建这些解决方案，Kubernetes 集群启动并运行后安装的第一批包之一可能是 Helm。

## 什么是头盔？

Helm 是第一个在 Kubernetes 上运行的应用程序包管理器。它允许用户通过方便的舵图描述应用程序结构，并使用简单的命令管理它。这是服务器端应用程序定义、存储和管理方式的巨大转变。

使用 Helm 这样的包管理器可以减少编排包管理器中定义的步骤的重复和复杂性。Helm 中的图表是 Helm 运行的主要格式。Helm Chart 是描述一组 Kubernetes 资源的文件集合。像其他基于惯例的包管理器格式一样，舵图遵循一个目录结构/树。舵图可以存档并发送到舵图存储库。Helm 是一个安装在 Kubernetes 集群外部的客户端，它利用 kubectl 来连接和交互 Kubernetes 集群。这些图表可以很容易地存储在图表库中。

图表存储库是一个 HTTP 服务器，它包含一个名为 index.yaml 的文件，也可以包含一些打包的图表。当您第一次构建图表存储库并准备上传图表时，存储库将生成一个名为 index.yaml 的文件。存储库索引(index.yaml)是根据存储中找到的包动态生成的。该文件包含图表的元数据，当新图表上传到存储库时，这些元数据将被写入该文件。它将在部署时用于获取图表，因为它包含所有必需的信息。如果您存储自己版本的 index.yaml，它将被完全忽略——所以这一点要记住！

如果需要协调不止一个 Kubernetes 资源，并且有多个配置不同的集群，那么就有充分的理由利用 Helm。软件供应商和开源项目都可以通过使用 Helm resources 作为客户将应用程序安装到 Kubernetes 集群的一种方式而受益。

## 什么是 ChartMuseum？

考虑到以上所有的好处，人们可能会问为什么需要图表博物馆。让我们从了解什么是 ChartMuseum 开始。

ChartMuseum 是一个用 Go (Golang)编写的开源 Helm Chart 知识库，支持云存储后端，包括谷歌云存储、亚马逊 S3、微软 Azure Blob 存储、阿里云 OSS 存储和 Openstack 对象存储。它用于存储和提供舵图，以将应用程序部署到 Kubernetes 集群。ChartMuseum 执行图表管理工具的任务。当图表上传到云商店时，ChartMuseum 就像一个二进制程序一样获取和维护图表信息。

## 图表博物馆的好处

当我们看一看使用 Helm 的局限性时，我们可以观察到，当我们设置存储库来存储图表时，Helm 只支持基于 HTTP 的存储库，不支持 [Secrets](https://harness.io/blog/secrets-management-ci-cd/) ，这导致 Helm 无法处理 OAuth。在 ChartMuseum 的情况下，当它运行时，有一个本地图表服务器在您选择的端口上启动，它具有处理和存储凭证的能力。这允许进行身份验证。它还处理更多的 API，而 Helm 只能处理少数 API。

当 Chart Museum 启动时，它会公开一个 API，您可以使用它来操作和获取图表。Helm 不是这样的，因为它需要获取 index.yaml 文件来列出可用的图表。如果用户删除了 index.yaml 文件，他们将无法根据请求获取图表。为了解决这个问题，当 index.yaml 被删除并重新生成时，ChartMuseum 将在名为 index-cache.yaml 的存储中保存一个状态文件，用于缓存优化。该文件仅供内部使用，但可用于迁移到简单存储。

它做得非常好的另一件事是 index.yaml 文件的转换。详细说明:为了获取可用图表列表，Helm 真正理解的唯一文件是 index.yaml 文件。在云后端的情况下，它可以像 S3 桶一样用作图表存储库，当我们开始将图表上传到 S3 桶时，它会生成一个 index-cache.yaml(不像其他基于 HTTP 的 repos，它们会生成一个 index.yaml 文件)。这个文件包含图表元数据，但是 index-cache.yaml 不能被 Helm 理解，因为它只能理解像 index.yaml 这样的文件。

在这种情况下，ChartMuseum 扮演了一个伟大的调解人。当 Helm 调用获取图表存储库的索引细节时，它会访问 ChartMuseum，因为 Helm 无法直接访问 S3 存储桶(它无法对其进行身份验证并理解协议)。因此，ChartMuseum 调用 S3 存储桶，并使用存储机密的功能对其进行验证并获取 index-cache.yaml。但由于 Helm 无法理解这个 index-cache.yaml 文件，ChartMuseum 将其转换为 Helm 可以理解的格式，并向其发送所有详细信息。

## 我们如何利用图表博物馆

当我们计划部署存储在像 GCS 或 S3 这样的云存储库中的舵图时，我们需要 ChartMuseum 的帮助。当我们创建一个将被用作图表存储库的 S3 连接器，然后在我们的服务中添加该连接器时，后台进程将如下所示:

1.首先选择能够执行任务的代表。然后它下载 ChartMuseum 库二进制文件(类似于 kubectl 二进制文件的下载方式)并运行 ChartMuseum。

2.ChartMuseum 一旦启动，就在一个带有公开 API 的端口上运行，该 API 可用于从存储库中获取图表。

3.赫尔姆然后在这个港口与海图博物馆联系。对于 Helm 来说，ChartMuseum 只是一个 HTTP 服务器，帮助它访问云回购端点并向其进行身份验证。

4.一旦 ChartMuseum 完成了对云回购的认证，它就会获取索引文件并发送给 Helm。这导致图表列表显示在 Harness' end 上。

5.获取图表列表后，ChartMuseum 在委托上停止。Repo 在本地从委托中移除，就像从缓存文件中删除条目一样。整个过程如下所示:

ChartMuseum 支持各种各样的后端，如 GCS、S3、微软 Azure 和 Openstack，而 Helm 不支持将它们用作图表存储库所需的协议。对于云提供的后端，还存在一层额外的安全性，这需要使用 Secrets & OAuth 对端点进行身份验证。目前 Helm Repo 的标准是使用匿名 HTTP 或者 HTTP 基本认证，所以 Helm 并不了解什么是秘密，如何使用秘密。Helm 本身并不是一个可行的选择，因为它无法理解不同云后端使用的各种协议。它也无法对他们进行身份验证。这就是海图博物馆发挥关键作用的地方。

ChartMuseum 确实有一些内部缓存，可以在停止后保留数据。我们在与 ChartMuseum 相关的 Harness 端没有任何缓存。

## 常见问题

当使用 ChartMuseum 时，我们确实会遇到一些与图表未按预期获取相关的常见问题。这会导致用户界面出错。让我们来看看可能发生的常见错误。

### 图表博物馆未启动

我们注意到 ChartMuseum 最常见的问题是，当代表在后台从云报告中获取图表时，Helm 需要与 ChartMuseum 通信以获得这些细节。代理在后台运行 ChartMuseum，我们必须等待它完成。

ChartMuseum 作为基于 HTTP 的服务器运行在 Helm 用来与 Repos 通信的端口上。当 ChartMuseum 启动时，虽然进程已经启动，但是端口并没有分配给服务器。这导致上面的错误出现。

可以通过将连接器重新添加到服务中来解决此错误。当然，这将要求代理创建一个新任务，并再次启动 ChartMuseum 服务器，然后从那里分配端口。

### 超时设定

与上述问题类似，当我们在服务级别上链接一个连接器以从云存储库中获取图表时，可能会发生超时。这些超时可能与存储库或其上的 index.yaml 文件的大小有关。

当 ChartMuseum 与 cloud Repo 通信时，我们有一个特定的超时时间。如果我们在分配的时间内没有得到响应，就会导致超时。

## 结论

正如在这篇简短的文章中所看到的，使用 Helm 对于我们在 Kubernetes 中处理应用程序有很多好处。随着 ChartMuseum 的加入，存储和版本控制我们的应用程序包变得轻而易举。此外，部署这个工具很简单，这就是为什么我们有各种各样的客户端能够使用他们选择的多个云后端来存储舵图。

关于头盔的进一步阅读，请查看我们的[头盔 vs. Kustomize](https://harness.io/blog/helm-vs-kustomize/) 博客，以及我们的[什么是头盔](https://harness.io/blog/what-is-helm/)的文章，其中包含了如何启动你的第一个头盔部署的教程。如果你还没有注册线束，今天就可以自由地[注册](https://app.harness.io/auth/#/signup/)-开始是免费的。来成为火箭飞船的一部分吧！

干杯！