# circle ci for Google Cloud Platform | circle ci 和 GCP 集成

> 原文：<https://circleci.com/blog/circleci-for-google-cloud-platform-scalable-flexible-and-efficient/>

为了最好地支持我们客户的个性化需求，CircleCI 为各种云提供商提供简单的集成解决方案。对于使用谷歌云平台(GCP)的客户，我们刚刚发布了一套集成，以帮助用户实现完全支持的端到端测试。我们在设计这些集成时考虑了速度、灵活性、安全性和可扩展性，以帮助支持具有各种用例的团队。

对于希望开始使用谷歌云的团队，我们有一套现成的集成，其启动时间比我们的竞争对手更快。有了这些 orb，您可以轻松地安装和配置 Google Cloud 命令行界面(CLI)，部署到 Google Kubernetes 引擎，或者在 Google Container Registry 中存储图像——所有这些都只需要在配置中添加几行代码。

对于更复杂的用例，开发人员会发现自己受到集成新工具所需的时间投资的限制。对于新的架构来说尤其如此——比如 Google Cloud Run，它需要团队构建高级集成来设置自动化部署。orb 不是处理复杂的基础设施，而是通过为 Google Cloud Run 等工具提供完全支持的无服务器模型，并通过将 CI/CD 管道与 Google Binary Authorization 等现代技术轻松连接，帮助团队专注于他们最重要的工作。

查看我们最新的 GCP orbs，帮助将 GCP 服务与您的 CI/CD 渠道相集成:

### 谷歌云平台 orbs

[谷歌云 CLI](https://circleci.com/developer/orbs/orb/circleci/gcp-cli)
安装并配置谷歌云 CLI (gcloud)

[谷歌云 GKE](https://circleci.com/developer/orbs/orb/circleci/gcp-gke)
创建集群，构建并发布 Docker 映像，并将映像部署到谷歌 Kubernetes 引擎(GKE)集群。

[谷歌云 GCR](https://circleci.com/developer/orbs/orb/circleci/gcp-gcr)
在谷歌容器注册表(GCR)上构建、推送和标记图像。

[Google Cloud Run](https://circleci.com/developer/orbs/orb/circleci/gcp-cloud-run)
构建无状态映像并部署到 Google Cloud Run 作为无服务器应用程序。

[GCP 二进制授权](https://circleci.com/developer/orbs/orb/circleci/gcp-binary-authorization)
配置 Google 的二进制授权服务来签名和认证容器映像以便部署。

### 您可以做什么:

你还想对 GCP 做些什么在球体上无法得到的事情吗？orb 是开源的，所以在一个现有的 orb 上增加功能只是获得你的 PR 批准和合并的问题。查看 [orbs 注册表](https://circleci.com/developer/orbs)中所有可用的 orbs。你有没有一个用例让你觉得与当前的 GCP 球体集不同？你可以[自己创作一个球体](https://circleci.com/blog/how-to-make-an-easy-and-valuable-open-source-contribution-with-circleci-orbs/)并将其贡献给社区。我们甚至发布了为 orb 创建自动化构建、测试和部署管道的[最佳实践](https://github.com/CircleCI-Public/Orb-Policies/blob/master/Orb%20Best%20Practices%20Guidelines.md)([第 1 部分](https://circleci.com/blog/creating-automated-build-test-and-deploy-workflows-for-orbs/)和[第 2 部分](https://circleci.com/blog/creating-automated-build-test-and-deploy-workflows-for-orbs-part-2/))来帮助您。

开始用 GCP 圆球在 CircleCI 建造。