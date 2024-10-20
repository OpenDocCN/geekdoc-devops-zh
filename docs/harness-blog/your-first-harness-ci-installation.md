# 您的第一个线束配置项安装、构建和发布|线束

> 原文：<https://www.harness.io/blog/your-first-harness-ci-installation>

您的驾驭 CI 之旅从 Drone 开始，这是一个流行的持续集成平台。了解如何开始第一次构建和推送。

我们非常兴奋地向世界介绍线束 CI。你的驾驭 CI 之旅从[无人机](https://drone.io/)开始，一个流行的[持续集成平台](https://harness.io/platform/continuous-integration/)。如果你不熟悉无人机，无人机是一个现代平台，旨在降低进入连接到你的源代码管理[SCM]平台的持续集成空间的门槛。如果你对[持续集成](https://harness.io/blog/what-is-continuous-integration/)不熟悉，不要担心，我们会帮助每个人装备并加强他们的持续集成能力。Harness CI / Drone 易于启动和运行，从架构上看，只有几个移动部件。

### 利用 CI /无人机架构

线束 CI / Drone 有一个[服务器](https://docs.drone.io/server/overview/)到[跑步者](https://docs.drone.io/runner/overview/)的模型。服务器协调执行任务的运行程序。

Harness CI / Drone 服务器向您的 SCM 提供者认证，然后通过一个共享的秘密与运行者通信。连接这些部件非常简单。

### 我们开始吧

在本例中，移动的部分将是一个 GitHub 帐户、一个正在运行的 CentOS 机器、一个 Kubernetes 集群和一个 Docker Hub 帐户。示例 GitHub 存储库可以在此示例之前在[https://github.com/ravilach/firstdrone](https://github.com/ravilach/firstdrone)被克隆/分叉。

### GitHub / SCM 准备

Harness CI / Drone 服务器需要通过 SCM 提供商的认证。这是在 [GitHub](https://docs.drone.io/server/provider/github/) 中通过制作 OAuth 应用程序来完成的。这是通过导航到设置- >开发者设置- > OAuth 应用程序+注册一个新的应用程序来完成的。

当您注册一个新的 OAuth 应用程序时，您需要准备好无人机服务器的 DNS/IP 地址。对我来说，这是 34.207.222.178。回调 URL 在主页 URL 后附加了/login。

当您注册应用程序时，请务必记下客户端 ID 和客户端密码，这是连接 Harness CI / Drone 服务器所需要的。

### CentOS/harnesci/无人机准备

根据你从哪里获得 CentOS，你可能需要安装 [Docker 引擎](https://docs.docker.com/engine/install/centos/)。假设您已经安装了 Docker 引擎，那么在安装之前，您在服务器上需要做的唯一准备工作就是生成共享密钥，该密钥将用于服务器到运行程序的通信。确保复制共享机密[RPC 机密]。

*openssl rand -hex 16*

### 线束 CI /无人机安装。

要在您的机器上获得 Harness CI / Drone，只需在图像上运行 Docker Pull。

*sudo docker 拉无人机/无人机:1*

一旦拉动，您可以[配置 Docker 运行的属性](https://docs.drone.io/server/provider/github/#configuration)。准系统需要填写的属性是机密、服务器地址和协议。让我们继续创建一个管理员用户。admin 用户使用您的 GitHub 用户名。

sudo dock run \
卷=/var/lib/drone:/data \
-env = drone _ github _ client _ id = your client id \
-env = drone _ github _ client _ secret = your pcsecret \
-env = drone _ server _ host = your stop domain \
-env = drone _ server _ proto = http \

### 无人机 UI

发出带有属性的 Docker Run 命令，您可以进入 Harness CI / Drone 服务器的 IP 或 DNS 条目。在我的情况下，导航到 https://34.207.222.178。你将被提示通过 GitHub 授权你的用户。

授权您的 GitHub 凭证。

一旦获得授权，Harness CI / Drone 将构建一个存储库列表。

现在您已经准备好部署跑步者了，在本例中是 Kubernetes。

### Kubernetes 集群准备

在这个例子中，我们将利用 Kubernetes 跑步者。为了让 Kubernetes Runners 代表您请求资源，除了 Runner 部署之外，还需要角色绑定。

一个简单的方法是构建两个 YAML 文件，一个用于角色和角色绑定，另一个用于部署。

*kube CTL 申请-f 无人机 _ role。YAML〔t1〕*

在 Runner 部署规范中，确保更新 Drone 服务器、共享机密和协议以匹配您的安装。

当项目填写好后，就可以部署部署了。

*ku bectl apply-f 无人机 _ 部署。YAML*

您可以选择验证跑步者是否连接到服务器。获取 Pod 名称，然后运行日志命令。

*ku bectl 获取 pods-all-namespaces*

*kube CTL logs drone-6564 db 8 f9f-949 S2-c runner*

成功！现在是时候开始迈向你的第一个建筑了。

### 将 CI 和 SCM 结合在一起

线束 CI / Drone 利用[配置作为代码](https://docs.drone.io/pipeline/overview/)来描述管道。Harness CI / Drone 可以处理来自源代码管理解决方案的 webhook 请求，以便在需要启动新构建时进行编排。

#### 准备存储库

可以利用 Harness CI / Drone UI 来“激活”存储库。在我的“第一无人机”仓库中，网络挂钩是空的。

在 Harness CI / Drone UI 中，导航到您选择的存储库，然后导航到活动存储库。

例如，默认情况下可以激活存储库。如果需要，要修改的配置是要监控的无人机配置文件的名称。在本例中，您将监视存储库中的“Drone.yaml”。

然后，您可以验证已经在 GitHub 中成功创建了 webhook。

CI 和 SCM 连线已经完成，接下来的步骤是定义您的第一个管道并开始构建。

### 您的第一个线束 CI /无人机项目

如果从一个空的存储库开始，你可以利用一个简单的[项目结构](https://github.com/ravilach/firstdrone)的例子，或者连接你自己的项目/存储库。

如果按照这个例子，您连接到 Harness CI / Drone 来监视“Drone.yaml”。声明性管道格式很容易连接到所需的步骤中，并且不需要担心插件。在示例管道中，我们利用 Docker 插件进行发布设置，并在执行期间自动下载。

突出显示的行利用秘密来连接我的 Docker Hub 凭据。您需要将回购位置更新为您自己的位置，并创建线束配置项/无人机机密来连接。

回到 Harness CI / Drone UI，导航到存储库设置，并添加所需的秘密。

添加适当的机密以匹配示例的格式。

有了这些连接，您现在就可以开始您的第一个构建并将其发布到 Docker Hub 了。

### 您的首次构建和发布

布线完成后，剩下的就是提交 Drone.yaml 进行一些更改，然后就可以开始比赛了。

一旦点击了 Commit changes，CI 魔术就开始了，您的构建和发布就结束了！

成功！祝贺您的第一个线束 CI /无人机构建和发布！

在 Docker Hub 上验证成功。

### 马具和无人机，一起更好

我们很高兴能进一步推动线束 CI /无人机社区的发展。如果你想与无人机社区交流，请随时跳上[无人机 Gitter](https://gitter.im/drone/drone) 向所有优秀的人学习。我们有很多很好的计划来提高 CI 和 CD 的能力，使它们变得简单和大众化。我们将发布更多更新，敬请关注。欢迎加入线束家族，无人机！