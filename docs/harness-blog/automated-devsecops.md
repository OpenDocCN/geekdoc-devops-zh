# 带堆栈和线束的自动化开发工具|线束

> 原文：<https://www.harness.io/blog/automated-devsecops>

在本例中，我们将在将应用程序部署到 Kubernetes 之前扫描应用程序。

DevSecOps 运动就是左移；授权开发团队做出更卫生的决策。随着工程团队创建变更/功能的步伐和速度，在过去的日子里，安全性可能被视为 SDLC 中的事后想法。今天，现代团队和组织试图在整个开发过程中传播应用程序安全专业知识。

StackHawk 平台旨在为您的 CI/CD 管道带来动态应用安全测试(DAST)。DAST 工具通常针对正在运行的应用程序运行，以识别正在运行的应用程序中存在的安全漏洞。随着应用程序容器化程度的提高，拥有一个独立运行的应用程序实例是很容易实现的。结合您的容器化应用程序，StackHawk 和 Harness 是您的 DevSecOps 之旅中的明智选择。在本例中，我们将在将应用程序部署到 Kubernetes 之前扫描应用程序。所有的代码片段也可以在 [GitHub Gist](https://gist.github.com/ravilach/28aa446dd81031de006545b3288ecc9e) 上找到。

该示例的目标是用 DAST 工具对正在运行的容器/应用程序进行自省，并让结果影响部署。一个很好的示例应用程序是 StackHawk 专门构建的[易受攻击的节点应用程序](https://github.com/kaakaww/vuln_node_express)，如果您手头没有这样的应用程序的话。

利用 Harness 平台部署到 Kubernetes，最简单的方法是将 Harness 委托部署到 Kubernetes 集群中。有几种方法可以对正在运行的容器调用 StackHawk 扫描。在本例中，我们将利用一个静态 EC2 实例来构建示例 Docker 映像并调用 StackHawk 扫描。一个 Harness SSH 委托将使 EC2 实例上的交互变得简单。

### 平台先决条件

如果这是你第一次，一定要在 [StackHawk](https://auth.stackhawk.com/signup) 和 [Harness](https://harness.io/free-trial/) 上注册账户。

#### 堆栈警告设置

登录 StackHawk 帐户后，请务必记下您将用于验证的 [API 密钥](https://app.stackhawk.com/settings/apikeys)。

StackHawk 提出了一个应用的概念。对于本例，单击 Add an App 并创建一个名为“Node App”的应用程序。

单击“下一步”后，您可以配置环境。示例应用程序绑定到端口 3000。将主机设置为“ [http://localhost:3000](http://localhost:3000/) ”但是，当使用 StackHawk 时，可以在创建应用程序后修改/创建配置[stackhawk.yml]作为谨慎的主机地址。

单击“下一步”后，您将看到一个应用程序 ID。请确保保存该 ID；此外，您可以保存生成的 stackhawk.yml，它将包含 ID。

现在，您已经准备好安装线束代理了。

#### 线束设置

对于这个例子，您将安装一个 SSH 和 Kubernetes 线束委托。

##### EC2/SSH

设置->线束代理->安装代理

下载类型:Shell 脚本

名称:ec2

单击复制下载链接。然后，登录到 EC2 实例的终端会话。

创建一个名为“harness”的目录，并将 cd 放入该目录。

*mkdir 线束* ‍ *cd 线束*

然后，将复制的下载链接粘贴到“harness”文件夹中进行下载。

下载完成后，就可以安装 SSH 委托了。

#安装 SSH delegate tar xfvz harness * . tar . gzcd harness-delegate。/start.sh

稍后，SSH 委托将出现在 Harness UI 中。

下一步将是安装线束 Kubernetes 代理。

##### 库伯内特斯

设置->线束代理->安装代理

下载类型:Kubernetes YAML

名称:k8s

将 tar.gz 下载并展开到您的本地机器上。

kubectl 命令将在 READEME.txt 中应用清单。

运行"*ku bectl apply-f harness-delegate . yam*l "

过一会儿，Kubernetes 代表就准备好了。

现在，您已经准备好装配管道了。

## 您的第一个 DevSecOps 管道

Harness 基于[抽象模型](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#cd_abstraction_model)的概念工作。基本上是组装管道所需的所有部件。

### 将 Kubernetes 集群连接到线束

将 Kubernetes 集群连接到 Harness 非常容易，尤其是在部署了 Harness 委托的情况下。

设置->云提供商->添加云提供商

类型:Kubernetes 集群

显示名称:StackHawkKubernetes

集群详细信息:从所选代理继承

代表选择器:k8s

单击测试进行验证，然后单击下一步。根据您的帐户，系统会提示您是否希望分析集群工作负载以节省成本。你可以选择加入或退出。添加后，Harness 现在可以与集群交互。

### 创建线束应用程序

线束的命脉是一个[线束应用](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#applications)。

设置->添加应用程序

姓名:StackHawk

一旦您点击 Submit，您将会看到一个空白的连续交付抽象模型。

### 创建线束环境

一个[利用环境](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#environments)是“我将部署到哪里？”环境可以跨越多个基础设施。在本例中，我们将指向 Kubernetes 集群。在 StackHawk Harness 应用程序中，添加一个环境。

设置->堆栈工作->环境+添加环境

名称:产品

描述:产品

环境类型:生产。

点击提交后，可以将 Kubernetes 集群添加为一个[基础设施定义](https://docs.harness.io/article/v3l3wqovbe-infrastructure-definitions)。

设置->堆栈工作->环境->生产+添加基础架构定义

名称:ProdKubernetes

云提供商类型:Kubernetes 集群

部署类型:Kubernetes

云提供商:StackHawkKubernetes

点击 Submit 后，现在就可以部署到 Kubernetes 集群了。如果利用来自 GitHub 的[示例应用程序](https://github.com/kaakaww/vuln_node_express)，您将需要构建该应用程序或带来您自己的映像。

## 构建 StackHawk 示例应用程序

在 [StackHawk 的 GitHub 项目](https://github.com/kaakaww/vuln_node_express)中有构建示例应用程序的说明。如果您没有过多使用 Docker，一个简单的方法是将他们的项目分支到您的个人 GitHub 帐户中，然后将该项目的内容下载到 EC2 实例中，并从那里开始构建。当然，可能利用 CI 平台(如无人机)或利用持续集成来构建的艺术。

### 克隆和修改 Docker 撰写

你可以把 StackHawk 的项目转入你自己的账户。为了更好地控制图像名称，请使用您自己的图像名称修改 Docker Compose[Docker-Compose . yml]。对于这个例子，我将发布到我自己的 Docker 注册表[rlachhman/demos:stackHawk]，您也可以从 Docker 获取它。

一旦完成了这些，您就可以将文件的 zip 文件保存到 EC2 实例中，或者利用 CI 平台。要了解 zip 的地址，可以在 GitHub 中点击 Code 并下载 ZIP。

# Get Files sudo yum 安装 unzip wget https://github。com/ravi lach/vuln _ node _ express/archive/refs/heads/main。zip 解压缩主。活力

### 使用 Docker 合成或 Docker 拉式就绪映像构建

您可以使用 Docker Compose 从源代码构建，也可以利用已经构建好的映像。

#### 码头工人拉动

如果您想跳过 Docker 合成步骤，您可以 Docker 提取一个现成的图像。

# docker pull sudo docker pull la chh man/demos:stack hawk

您也可以使用 Docker Compose 构建图像并修改名称。

#### Docker 撰写

如果这是你第一次使用 [Docker Compose](https://docs.docker.com/compose/) ，你将需要在 EC2 机器上安装[Docker Compose](https://docs.docker.com/compose/install/)。下面是 CentOS 的 EC2 实例的说明，假设 Docker 运行时已经启动。潜在地，您可以将它添加到委托概要文件中，这样它就可以与 Harness SSH 委托一起安装。

如果您的 CentOS 机器上没有运行 Docker CE:

# install dock https://docs . docker . com/engine/install/centos/sudo yum install-and yum-utils sudo-config-manager \-add-repo \ https://download . docker . com/Linux/centos/docker-ce . repo sudo install docker-ce docker-CLI container d . io sudo system CTL start docker

Docker 编写安装程序:

# Docker 复合安装 sudo curl-L " https://github。com/dock/compose/releases/download/1 档案。29 .1/docker-合成-(uname-s)-(uname-m)"-o/usr/local/bin/docker-合成 sudo chmod+x/usr/local/bin/docker-合成 sudo ln-s/usr/local/bin/docker-合成/usr/bin/docker-合成 docker-合成版本

完成后，回到 GitHub 项目下载的根目录，现在就可以运行 Docker Compose 了。

运行 Docker 合成。

sudo docker-构建-构建-分离

您需要将工件发布到 Docker 注册中心。最简单的方法是发布到个人 Docker Hub 账户。简单地用你的 Docker Hub 凭证运行一个 *docker 登录*，然后运行一个 *docker 推送*来发布。

Docker Compose 步骤已经完成，现在是时候开始构建管道了。

## 从线束呼叫 StackHawk

有几种方法可以将软件集成到线束工作流程中。对于这个例子，我们希望在 EC2 实例上运行 Shell 脚本来调用 StackHawk，然后解析结果。最简单的方法是将 EC2 实例定义为端点[云提供商]，这样我们就可以在一个工作流中管理这些 shell 脚本。

设置->云提供商->添加云提供商。类型是物理数据中心。您可以将提供者命名为“斯塔克豪·DC”

一旦你点击提交，斯塔克豪·DC 就会出现。

回到 StackHawk 应用程序，您可以添加一个映射到新云提供商的 Harness 环境。

设置->堆栈工作->环境+添加环境

名称:斯塔克豪扫描仪

环境类型:非生产

像前面的 Kubernetes Harness 环境一样，您需要创建一个基础设施定义。

姓名:斯塔克豪·森特斯

云提供商类型:物理数据中心

部署类型:SSH

云提供商:斯塔克豪·DC

主机名:本地主机

连接属性:任何

点击 Submit——您现在已经连接到 EC2 实例了。

Harness 能够协调来自 Shell、Serverless、Kubernetes 等多种类型的部署。您可以利用[线束服务](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#services)作为管道来跨管道执行 StackHawk 命令。

为 StackHawk 命令创建新的线束服务。

设置->堆栈工作->服务+添加服务

名称:StackHawk 命令

部署类型:安全外壳(SSH)

工件类型:其他

单击提交–现在您可以使用此服务来部署。

为了让 Harness 管理执行 StackHawk 扫描所需的命令，创建一个代表它的 [Harness 工作流](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#workflows)。

设置->堆栈工作->工作流+添加工作流

名称:StackHawk 扫描

工作流类型:滚动部署

环境:StackHawk 扫描仪

服务:StackHawk 命令

基础设施定义:StackHawk CentOS

点击提交后，将会生成一个模板化的工作流。阶段、名称和顺序可以被改变、压缩等。

您可以通过单击+ Add Step，在步骤 3“部署服务”下插入 StackHawk 调用。在添加步骤 UI 中，搜索“Shell”并选择 Shell 脚本。

单击下一步。围绕填写一个 [shell 脚本，说明 StackHawk 扫描需要什么。确保选择分配给 Shell(例如 CentOS)代理的代理选择器(ec2)。这些部分将生成 stackhawk.yml，运行 stackhawk 扫描器，并调用 Docker 映像来运行易受攻击的应用程序的容器。该脚本接受来自线束秘密管理器和工作流本身的变量。在此步骤之后，将对其进行配置。](https://docs.stackhawk.com/hawkscan/running-hawkscan.html)

根据您在 Docker 合成中对 Docker 映像的命名，您需要修改 Docker Run 命令以反映新的映像名称和标签。

名称:Shell 脚本

类型:Bash

对委托执行:选中

代表选择器:ec2

脚本:

#Make DIR 和 CDmkdir-p ~/stack hawk-scan SCD ~/stack hawk-scans # Docker Run if needs Udo Docker Run-RM-publish 3000:3000-name nodeexpressvulny rlachhman/demos:stack hawk # Create stack hawk . YAML cat > stack hawk . yml<< 'EOF'# stackhawk configuration for Node Appapp:  # An applicationId obtained from the StackHawk platform.  applicationId: ${workflow.variables.stackhawkappid} # (required)  # The environment for the applicationId defined in the StackHawk platform.  env: Production # (required)  # The url of your application to scan  host: ${workflow.variables.stackhawkhost} # (required)EOF#Run Scansudo docker run --rm -v $(pwd):/hawk:rw -e API_KEY=${secrets.getValue("stackhawkapikey")} -i stackhawk/hawkscan:latest stackhawk.yml 2>& 1 | tee scan results . txt

单击提交–现在工作流有了执行扫描的命令。

上面的脚本将接收 StackHawk API 键、AppID、应用程序的运行地址的输入，然后将扫描结果导出到本地文件中。

将它包装在 Harness 工作流中的好处是，您可以提示用户输入项目。您可以在工作流中设置两个变量来提示 StackHawk AppID 和运行图像的地址。

在右侧的 StackHawk 扫描工作流中，单击工作流变量并添加以下内容。

变量名:stackhawkappid

类型:文本

必填:已勾选

变量名:stackhawkhost

类型:文本

必填:已勾选

单击保存，工作流变量就就位了。

如果您想添加其他变量，可以通过以下格式访问它们:

*$ {工作流程。变量。变量名称}*

接下来，使用 Harness 的 Secret Manager 来存储您的 StackHawk API 密钥。

安全->机密管理+添加加密文本

姓名:stackhawkapikey

值:您的 api 密钥

单击提交。你现在可以知道这个秘密了。访问机密类似于访问工作流变量。

*$ {秘密。getvalue(" key _ name ")*

基本的扫描已经完成，现在是时候创建一些部署逻辑了。

## 部署管道

最终，您会想要部署应用程序。回到 Harness 平台的第一步是为 Kubernetes 部署创建一个 Harness 服务。

设置->服务+添加服务

名称:节点应用程序

部署类型:Kubernetes

单击 Submit 后，Harness 将创建部署所需的 Kubernetes 脚手架。

Harness 需要通过在服务中单击+ Add Artifact Source 并选择 Docker Registry 来了解要部署什么工件。

根据您是否从头开始运行 Docker 合成，您需要用您的名称替换图像名称。如果使用预烘烤的，利用以下。

源服务器:Harness Docker Hub[默认情况下启用公共 Docker Hub]。

Docker 镜像名称:rlachhman/demos[或您的 repo/name]。

点击 Submit 后，您的服务就可以部署了。

您将需要添加一个定义如何部署步骤的线束工作流。对 Kubernetes 来说相当简单。

设置->堆栈工作->工作流+添加工作流

名称:部署节点应用程序

工作流类型:滚动部署

环境:产品

服务:节点应用程序

基础设施定义:产品

单击 Submit 后，您的节点应用程序就有了要部署的工作流。

最后剩下的步骤是解释结果，并将工作流缝合到一个内聚的 DevSecOps 管道中。

## 自动化您的 DevSecOps 管道

自动化是 DevSecOps 的关键。由于 StackHawk 扫描是在管道中以编程方式执行的，因此它们也可以以编程方式进行解释。

类似于在 Shell 脚本中执行扫描，您可以创建另一个 Harness 工作流来解释扫描结果。

设置->堆栈工作->工作流+添加工作流

名称:解释扫描

工作流类型:滚动部署

环境:StackHawk 扫描仪

服务:StackHawk 命令

基础设施定义:StackHawk CentOS

与 StackHawk 扫描类似，将 interpet 脚本添加到工作流的步骤 3 中。

+添加步骤->外壳脚本

名称:Shell 脚本

脚本类型:Bash

对委托执行:选中

代表选择器:ec2

脚本:

# Change Foldercd ~/stack hawk-scans # Parse results export totalhighfroviderations = $(grep-I ' \

点击提交，解释就完成了。该脚本将解释来自 StackHawk 扫描控制台输出的扫描结果，如果存在严重违规，则停止部署。

最后一步是将扫描、解释和部署结合在一起。

## 运营开发部门管道

The power of the [Harness Pipeline](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#pipelines)是将粒度步骤缝合在一起，成为一个功能齐全/可操作的 DevSecOps 管道。

创建线束管线很简单。顺序将是扫描的管道阶段，然后是解释，如果符合卫生标准，则是部署。

设置->堆栈工作->管道+添加管道

名称:DevSecOps

点击“提交”后，您可以向每个工作流添加一个阶段。

单击+圆圈添加第一阶段，即 StackHawk 扫描。

点击提交，添加另外两个；解释和部署。

一旦完成，你的 CD 抽象模型就完成了。你准备好执行了！

## 运行您的 DevSecOps 管道

要运行您的示例，请转到左侧导航，单击 Continuous Deployment，然后启动 New Deployment。

您将需要部署“StackHawk”应用程序，并填写您的 StackHawk AppID 和运行容器地址。

应用:StackHawk

管道:DevSecOps

stack hawk appid:your _ stack hawk _ app _ id

stackhawkhost:您的运行主机或 ip 地址

工件/节点应用程序标签:your _ Tag[或者#stackHawk，如果使用 rlachhman 的例子的话]

点击“提交”——你现在可以开始比赛了！注意:管道将失败，因为这个应用程序很容易受到攻击。在第二阶段，您可以看到部署已经被阻止。

回到 StackHawk 控制台——您现在可以对漏洞进行分类。

开发愉快！使用 StackHawk，您可以确认漏洞并重新运行管道以成功部署。

## DevSecOps 和 Harness:一起更好

Harness 作为首要的软件交付平台，可以帮助您实现您的 DevSecOps 目标。作为一种整合新技术和扫描方法的不偏不倚的方式，Harness 平台可以成为软件交付中发生的范式转变的渠道。今天一定要注册一个[驾驭账号](https://harness.io/free-trial/)！

干杯，

拉维

‍