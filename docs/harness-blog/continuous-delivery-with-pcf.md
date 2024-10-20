# 通过 Pivotal Cloud Foundry & Harness CD | Harness 持续交付

> 原文：<https://www.harness.io/blog/continuous-delivery-with-pcf>

我们在这个 PasS 系列的第一部分中了解了[平台即服务](https://harness.io/blog/what-is-paas/)以及在 Kubernetes 生态系统中 thas PaaS 解决方案面临的竞争。将 Harness 与 PaaS 结合使用，无疑可以为您和您的组织带来持续交付和高效应用基础设施部署的好处。在我们博客系列的第三部分中，我们将利用 Pivotal 堆栈和 Harness。准备好潜水吧！

## 一头扎进去

如果您无法访问 PaaS 基础架构，也不用担心。OpenShift 和 T2 的 Pivotal Cloud Foundry 都有在线平台，你可以免费学习。在下面的例子中，我们将利用 Pivotal Web 服务(正式名称为 Pivotal Cloud Foundry Foundations)和一个亚马逊 EC2 Centos 实例作为[管理代理](https://developer.harness.io/docs/getting-started/harness-platform-architecture/)。喜欢总是可以跟随博客和/或观看视频来获得你的学习。

第一步是注册一个 [Pivotal Web Services](https://account.run.pivotal.io/z/uaa/sign-up) 账户。注册过程很简单，只需提供一个电子邮件地址和手机号码进行双重身份验证。你将创建一个[组织](https://docs.pivotal.io/platform/application-service/2-7/concepts/roles.html#orgs)[这里我称之为我们的 [CaptainCanary](https://community.harness.io/c/Captain-Canary/5) ]，如果需要的话可以使用默认的[空间](https://docs.pivotal.io/platform/application-service/2-7/concepts/roles.html#spaces)【开发】。

设置完成后，我们就可以开始设置 PCF 集群和线束平台之间的布线了。 [Harness Delegate](https://developer.harness.io/docs/platform/delegates/get-started-with-delegates/delegate-installation-overview/) 是 Harness 平台中的 worker 节点，它为您运行您的命令。设置代理非常简单。登录你的[马具账号](https://app.harness.io/)或者注册一个[马具账号](https://harness.io/try-for-free-continuous-delivery-as-a-service/)。

我通常在一个亚马逊 EC2 实例上运行我的委托。刚刚启动了一个新的 [Centos](https://www.centos.org/) EC2 实例，我们首先需要安装 Harness Delegate。最简单的方法是登录一个线束账户，然后进入设置->->下载代理。单击复制图标下载 Shell 代理。

通过复制该命令(curl ),我们可以简单地将该命令粘贴到一个等待的 Centos 实例中。

可以用*tar-xvf harness-delegate.tar.gz*解开线束委托下载

最后，将 CD 放入 harness-delegate 文件夹并运行*。/start.sh* 很快就会安装线束代理。对于 EC2 Centos 实例上的这个例子，我们不需要修改 [ulimit](https://linuxhint.com/linux_ulimit_command/) ，因为只有我们。

甜蜜，成功！安装完成后，我们可以在 Harness Web UI 中验证我们的代理是否在那里。

[Cloud Foundry 命令行界面的时间](https://docs.pivotal.io/platform/application-service/2-7/cf-cli/index.html)【CF CLI】。安装 [Cloud Foundry CLI](https://docs.pivotal.io/platform/application-service/2-7/cf-cli/install-go-cli.html) 有几种方法。您需要将 Harness Delegate EC2 实例连接到 CF CLI。为了快速测试布线，您还可以在本地机器上安装 CF CLI。在我的 Mac(本地机器)上，我利用了[自制软件](https://brew.sh/)。在我的 Mac 上安装 CF CLI 就像运行*brew install cloud foundry/tap/CF-CLI*一样简单

可以使用 *cf -help* 验证安装是否成功。

接下来，您需要使用 CF CLI[登录](https://docs.pivotal.io/platform/application-service/2-7/cf-cli/getting-started.html#login)。*cf log in-a API _ URL-u USERNAME-p PASSWORD-o ORG-s SPACE*导航到 [Pivotal Web 服务控制台](https://console.run.pivotal.io/)底部导航栏的 [Tools](https://console.run.pivotal.io/tools) ，查看您的实例的 API 端点。

在 Pivotal Web 服务的情况下，在您本地机器上的终端中运行:*cf log in-a https://donotus eapi . run . Pivotal . io-u USERNAME-p PASSWORD-o " captain canary "-s development*成功！

接下来，让我们安装 CF CLI(远程),作为我们的[代表配置文件](https://docs.harness.io/article/nxhlbmbgkj-common-delegate-profile-scripts)的一部分。Delegate Profile 是一个很好的地方，可以让 Harness Delegate 为我们安装所需的包，这些包运行在 Delegate 基础结构上。回到我们的代表设置[Setup - > Harness Delegates]可以点击管理代表档案+添加代表档案

可以利用我们的[文档中的脚本，通过代表配置文件安装 CF CLI](https://docs.harness.io/article/nxhlbmbgkj-common-delegate-profile-scripts#cloud_foundry_cli) 。创建一个名为“CF CLI Install”的委派配置文件。我用来旋转的 Centos Amazon 机器映像没有附带 [Wget](https://en.wikipedia.org/wiki/Wget) ，所以可以轻松安装。

*# Install CF CLI # Install WGet since AMI 不附带 CHO " installing WGet " sudo yum Install WGet echo " adding repo " sudo WGet-O/etc/yum . repos . d/cloud foundry-CLI . repo https://packages . cloud foundry . org/fedora/cloud foundry-CLI . repo echo " installing " sudo yum-y Install CF-CLI*一旦您单击提交，就可以通过将鼠标悬停在代表信息的配置文件部分并选择“CF CLI Install”来运行配置文件。

应用后，您可以通过单击“查看日志”从 Harness UI 查看代理配置文件日志输出。

看起来不错，您也可以在您的 Centos (Delegate)实例上运行相同的命令来验证您在本地机器上使用了 *cf -help。*

现在，您已经拥有了所需的基础设施。接下来，让我们将您的 Pivotal Web 服务帐户连接成一个[云提供商](https://developer.harness.io/docs/first-gen/continuous-delivery/pcf-deployments/pcf-tutorial-overview/)。导航至设置- >云提供商+添加云提供商

添加类型“Pivotal Cloud Foundry ”,您的凭据名称为“pcf”。省略端点 URL 中的 HTTP(S)部分。

单击测试进行验证，然后提交。现在，您将拥有作为 pcf 云提供商的“PCF”。

接下来，您将需要部署某种应用程序。Tomcat 项目中的 [Tomcat 示例应用程序](https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/)是测试部署和查看 [Pivotal 构建包](https://docs.pivotal.io/platform/application-service/2-8/buildpacks/index.html)运行情况的一个优秀应用程序。访问 [WAR](https://en.wikipedia.org/wiki/WAR_(file_format)) 的最佳方式是下载[示例 WAR](https://tomcat.apache.org/tomcat-7.0-doc/appdev/sample/sample.war) 并将其发布到您喜欢的[工件库](https://developer.harness.io/docs/first-gen/firstgen-platform/account/manage-connectors/configuring-artifact-server/)。我正在利用[JFrog artifact](https://jfrog.com/open-source/#artifactory)。如果您没有访问工件存储库的权限，您可以在[云提供商](https://bitnami.com/stack/artifactory/cloud/aws)中构建一个，或者利用您的 Harness Delegate 作为安装位置。我们可以在 Harness Delegate 配置文件中运行 [Centos 安装步骤](https://nallarameshreddy.wordpress.com/2017/05/30/steps-to-download-and-install-artifactory-in-linux/)。回到我们的代表设置[设置- >管理代表]可以点击管理代表档案+添加代表档案

创建名为“安装 Artifactory”的配置文件

*#Install OpenJDK 和 Artifactoryecho " Installing JDK " sudo yum Install Java-1 . 8 . 0-open JDK-develoecho " Installing arti factory " wget https://bintray.com/jfrog/artifactory-rpms/rpm-O bin tray-jfrog-arti factory-rpms . repo Udo mv bin tray-jfrog-arti factory-rpms . repo/etc/yum . repos . d/sudo yum Install jfrog-arti factory-ossecho " Starting arti factory " sudo service arti factory start*保存后，您可以通过转到 Profile 来运行线束委托配置文件

一旦运行了代表档案，就进入[的 Web UI](https://localhost:8081/artifactory) 【通常是[的 https://public hostip:8081/artifactory](https://publichostip:8081/artifactory)】。如果您在 EC2 委托上安装，IP 地址就是 EC2 实例的[公共 IP](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/using-instance-addressing.html#concepts-public-addresses) 。默认用户为*管理员*，默认密码为*密码*。

登录后，可以导航到管理->存储库->本地。在这里，您可以创建一个本地通用存储库。

选择“通用”作为包类型，并将存储库命名为。我把我的命名为“ravi-generic”。

一旦点击提交，存储库就准备好了。

我们现在可以将示例 WAR 上传到 Artifactory。导航回到 Artifacts->“your-generic-repository”并点击右边的 Deploy。

上传您已经下载的 sample.war。

一旦您点击了 Deploy，您就为您的工件做好了准备。

接下来，您需要将 Artifactory 实例连接到线束。你需要得到你的[外部 IP](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/using-instance-addressing.html#concepts-public-addresses) 地址来连接它。导航到设置- >连接器- >工件服务器+添加工件服务器。

添加一个工件服务器，类型为 *Artifactory* ，显示名称为“ *My Artifactory* ”。例如，我的公共 IP 是 54.88.202.177，在我的例子中，Artifactory 服务器的 URL 将是 https://54.88.202.177:8081/artifactory/。如果您没有修改用户名和密码，将会默认为[ *admin/password* ]。

单击测试并提交。一旦成功，你将可以使用“*我的工厂*”。

## PCF 连续交货点

随着线束和 PCF 布线的方式，现在是时候让我们用线束部署创造一些奇迹了。首先，让我们转到设置- >应用程序+添加应用程序，创建一个新的[线束应用程序](https://docs.harness.io/article/bucothemly-application-configuration)。我们可以将应用程序称为“PCF_Perfection”。

让我们添加一个[服务](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/setup-services/add-service-level-config-variables/)，它将代表一个可部署的。在 PCF_Perfection 应用程序中，导航至服务，然后+添加服务。

添加一个名为“My_PCF”的“Pivotal Cloud Foundry”类型的服务。

一旦您点击了 Submit，接下来您将把 sample.war 作为工件源连接到服务中。

+添加工件源。从“我的 Artifactory*”*添加 Artifactory 源。在我们的例子中，我们将连接 sample.war，它位于“ravi-generic”存储库的根目录下。

单击 submit，工件源就会出现。

你需要设置一个[驾驭环境](https://developer.harness.io/docs/continuous-delivery/onboard-cd/cd-concepts/services-and-environments-overview/)。设置->PCF _ 完美-环境+添加环境。

命名为“Staging”并将环境类型设置为*非生产*。

接下来，在暂存环境中，您需要通过转至+添加基础架构定义来配置基础架构定义。

在基础设施定义中，将定义命名为“Pivotal Web 服务”。选择云提供商类型为 *Pivotal Cloud Foundry* ，部署类型为 *Pivotal Cloud Foundry* 。选择之前连接的 PCF 云提供商，并选择/添加您的 Cloud Foundry 组织和空间详细信息。

接下来，您将添加一个[线束工作流](https://developer.harness.io/docs/first-gen/continuous-delivery/model-cd-pipeline/workflows/workflow-configuration/)。设置- > PCF_Perfection - >工作流+添加工作流

添加一个名为“基本 PCF”的工作流，工作流类型为*基本部署*。将环境、服务和基础架构定义与创建的内容相匹配。

点击提交后，修改部署下的应用程序调整步骤。将目标定为 100%理想的实例。

点击提交后，您的工作流程将如下所示。

接下来，您将通过进入设置->PCF _ Perfection->Pipelines+添加管道来添加[线束管道](https://developer.harness.io/docs/platform/templates/create-pipeline-template/)

添加一个名为“简单 PCF”的线束管道。

添加名为“淘金”的管道阶段。执行您创建的 PCF 工作流。

一旦你点击提交，你将有一个单阶段的管道。

现在，在橡胶与道路交汇的地方，是时候铺设管道了！导航到持续部署->开始新部署

选择 PCF_Perfection 应用程序和适当的构建/版本[如果使用 sample.war，只需 sample.war]。

点击提交，观看奇迹发生！

一旦奇迹发生，可以导航回 Pivotal Web 服务控制台并点击[路线](https://docs.pivotal.io/platform/application-service/2-8/services/route-services.html)。

点击路线后，坐下来享受应用程序。

祝贺您安装了一条非常现代化的连续输送管道！

## 线束和 PaaS，更好地结合在一起

在博客系列的第[部分、第一](https://harness.io/2019/12/platform-as-a-service-series-part-1-3-paas-tastic/)部分和第二[部分，我们学到了很多关于 PaaS 的知识。随着技术的进步，PaaS 市场也在不断发展。在平台即服务(PaaS)供应商格局中，有一种向](https://harness.io/2019/12/platform-as-a-service-series-part-2-3-trouble-in-paas-land/) [Kubernetes](https://kubernetes.io/) 靠拢的趋势。PaaS 可以为您的组织提供巨大的价值，再加上驾驭的力量，工程效率可以更上一层楼。一如既往，请随意报名参加培训，并以此为例进行一次尝试。干杯！拉维