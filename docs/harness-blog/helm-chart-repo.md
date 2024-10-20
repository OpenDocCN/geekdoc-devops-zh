# 教程:将 GitHub Repo 转换为舵图 Repo | Harness

> 原文：<https://www.harness.io/blog/helm-chart-repo>

今天我想分享一下我和一个客户做的事情，让他的 GitHub 回购变成了自动化的 HTTP Helm Chart 回购。这是一个非常好的选择，因为有一个 GitHub 动作可以让回购指数自动更新。系好安全带。

今天我想分享一下我和一个客户做的事情，让他的 GitHub 回购变成了自动化的 HTTP Helm Chart 回购。这是一个非常好的选择，因为有一个 GitHub 动作可以让回购指数自动更新。系好安全带。

## 辅导的

### 要求

一个 GitHub 账户，和一些舵图模型。

### 第一步

让我们创建一个超级简单的 GitHub Repo/项目:

### 第二步

现在，让我们创建一个名为 **gh-pages** 的分支。我已经在 UI 上了，所以我现在不会克隆/签出。

### 第三步

让我们遵循[图表存储库指南](https://helm.sh/docs/topics/chart_repository/)来确保我们的回购为托管 Helm Chart 回购做好准备。您需要确保您的 **gh-pages 分支**被设置为 GitHub Pages。点击你的回购**设置**，向下滚动到 **GitHub 页面**部分，设置如下:

### 第四步

现在，是时候按照这个 GitHub 动作指南赋予这个回购超能力了: [Chart Releaser 动作自动化 GitHub 页面图表](https://helm.sh/docs/howto/chart_releaser_action/)。

**重要:**除了动作源，重要的是在**主**分支中保留一个*/海图*路径来托管你的舵海图源。

让我们克隆回购:

➜线束 _ 实验室 git 克隆 git **@github** 。com:gabrielcerioni/harness-helm-charts . git
克隆成‘harness-helm-charts’...
远程:枚举对象:3，完成。
远程:清点对象:100% (3/3)，完成。
远程:总计 3 (delta 0)，复用 0 (delta 0)，打包复用 0
接收对象:100% (3/3)，完成。

并添加舵图教程链接中建议的更改。

这个过程在 **gh-pages** 分支中创建或更新 index.yaml。所以，让我们用这个技巧来确保这个动作是有效的。

### 第五步

现在是时候添加我们的图表源路径，将项目推到原点，并祈祷一切顺利。我发现关于这部分的 Helm 文档有点差，这也是我写这篇文章的原因之一。

这是我们目前的状态:

让我们创建一个路径来存放我的非常简单和虚拟的 NGINX 舵图表源，然后我将添加我拥有的非常漂亮的舵图表:

mkdir 图表

这是一个 Helm 源码，我是通过跟随文档教程得到的。我什么都没改变。

让我们在 Chart.yaml 中添加一个随机版本，以确保它正常工作:

### 第六步

不错！是时候把它推到我们的原点了！

### 第七步

让我们确保这是可行的。

我来看看 GitHub 的动作标签。在我看来很可靠:

我不相信它。我再测试一次。现在是第 14 版！

有用！

**重要:**你也可以在 **gh-pages** 分支中检查这个动作是否处理好了我们的 index.yaml。这个文件对任何 HTTP Repo 都是超级重要的吧？

您可以在您的实验室中通过相同的路径来验证这一点:
[https://gabrielcerioni . github . io/harness-helm-charts/index . YAML](https://gabrielcerioni.github.io/harness-helm-charts/index.yaml)

## 大的最后一步——利用它:

让我们在 Harness UI(连接器)中创建一个好的 Helm Repo 工件服务器:

并添加回购作为我的服务远程掌舵清单！

这需要一段时间，但它会加载可用的版本:

超级好看。让我们在模板滚动更新工作流中运行它！

没人能阻止我们！

我希望你喜欢这篇关于把 GitHub 回购变成 Helm Chart 回购的教程。如果您有任何问题，请联系我！

加百列