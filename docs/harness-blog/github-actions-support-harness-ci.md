# 线束配置项|线束中的 GitHub 操作支持

> 原文：<https://www.harness.io/blog/github-actions-support-harness-ci>

GitHub 动作允许您创建可以执行预定义任务的自定义动作。了解如何在利用 CI 中使用它们来增加可扩展性。

在本文中，我们将了解 Harness CI 中的 GitHub 动作支持，以及插件可扩展性如何帮助将动作模板化为插件步骤。

GitHub 动作允许您创建可以执行预定义任务的自定义动作。这些预定义的任务包括从克隆代码库到构建 Docker 映像和安全扫描映像。以前创建的动作出现在 GitHub marketplace 上，对超过 10k 个动作提供了丰富的支持。

Harness CI 增加了对运行 GitHub 动作的支持。这意味着 GitHub 动作可以通过 CI 管道中的插件步骤使用。

## 使用

GitHub Actions YAML 包含三个属性:

1.  **name** :指的是动作的 GitHub 库以及分支或标签。
2.  **with** :一个键和值为字符串的 map。这些是行动输入。
3.  **env** :传递给动作的环境变量。

您必须在插件步骤设置中复制 with、uses 和 env 属性，以将 GitHub 动作用作 Harness CI 中的插件。您还必须在特权模式下运行该步骤，因为 GitHub 操作插件使用 Docker 中的 Docker (dind)。

以下是 GitHub actions 中的 action YAML 与 Harness CI 的对比:

A comparison of action YAML in GitHub actions vs. Harness CI

以下是在线束配置项中使用操作的一些示例。

### 繁琐的扫描动作

Trivy 是一个开源的扫描器，用于检测容器图像、git 存储库等等中的漏洞。

以下示例使用 Harness CI 中的 trivy 扫描“drone/git”容器图像。

###### -step:
identifier:Trivy
name:Run Trivy 漏洞扫描器
type:Plugin
spec:
connector ref:docker hub
image:plugins/github-actions
privileged:true
settings:
uses:aquasecurity/Trivy-action @ master
with:
image-ref:drone/git
format:table
exit-code:" 1 "
ignore-unfixed:" true "

‍

### GCS 上传活动

GCS 上传操作可用于将文件上传到 Google 云存储。

-步骤:
标识符:GCS-上传者
名称:上传文件到 GCS
类型:插件
规格:
连接器 Ref: dockerhub
图片:插件/github-动作
特权:真
设置:
用途:Google-github-动作/上传-cloud-storage @ main
with:
路径:'/path/to/file'
目的地:demo/gcs
凭证:< +stage

###### ‍

Git 签出操作

### Git checkout 操作用于克隆 GitHub 存储库代码库。该操作可用于在 Harness CI 的一个阶段中克隆一个或多个 git 存储库。

以下示例克隆触发器有效负载中存在的主存储库。需要指定 GITHUB_TOKEN 作为克隆私有存储库步骤的环境变量。

-步骤:
标识:checkout
名称:checkout GitHub action
类型:插件
规格:
connectorRef: dockerhub
图片:插件/github-actions
特权:真
设置:
用途:actions/check out @ v2
with:
ref:$ { { GitHub . event . pull _ request . head . sha } }
event _ payload:<+trigger . event payload【t5t 5】

###### ‍

您必须在插件步骤设置中指定存储库名称，以克隆第二个存储库。

-步骤:
标识符:checkout-repo-by-name
名称:check out GitHub repository by name
类型:Plugin
规格:
connectorRef: dockerhub
图像:plugins/github-actions
特权:true
设置:
用途:actions/check out @ v2
with:
repository:my-org/my-private-tools
路径:my-tools
ref: ${

###### 履行

## GitHub Actions 的工作方式是克隆“ **uses** ”属性中指定的存储库，并从克隆的 action 代码中执行“ **action.yml** ”文件中的步骤。

GitHub 动作的 CI 插件使用 [nektos/act](https://github.com/nektos/act) ，这是一个在本地执行 GitHub 动作的开源项目。Nektos/act 运行 Docker 容器，GitHub 动作工作流在该容器上执行。CI 插件为输入操作步骤创建一个工作流，然后通过 nektos/act 执行它。以下是插件源代码的链接:[https://GitHub.com/drone-plugins/GitHub-actions](https://github.com/drone-plugins/github-actions)

结论

## 我们已经展示了 Harness CI 中插件的可扩展性和简单性，使得只需一个插件就可以添加许多动作。这展示了可插拔的线束配置项有多强。我希望你在 Harness CI 中尝试你最喜欢的动作，并可能为你定制的任务创建你自己的插件。

要进一步阅读 Harness CI，为什么不看看我们的[从 Jenkins 迁移到 Harness CIE](https://harness.io/blog/jenkins-to-cie-journey/) 的文章呢？

要进一步阅读 Harness CI，为什么不看看我们的[从 Jenkins 迁移到 Harness CIE](https://harness.io/blog/jenkins-to-cie-journey/) 的文章呢？