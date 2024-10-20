# CI/CD 管道中的漏洞扫描-第二部分|利用

> 原文：<https://www.harness.io/blog/automated-vulnerability-scanning-ci-cd-pipeline>

继续本系列的第一部分，我们将利用 Harness 平台在您的 CI/CD 管道中进一步实施漏洞扫描。

在您的部署管道中实现 DevSecOps 实践是一个谨慎的举措。有句谚语说软件组件像牛奶而不是葡萄酒一样老化，这是非常正确的。DevSecOps 领域的一家供应商 Sonatype 推出了广受欢迎的 Nexus IQ 平台，用于漏洞扫描。

继续本系列的第一部分，我们将利用 Harness 平台在您的 [CI/CD 管道](https://harness.io/blog/ci-cd-pipeline//)中进一步实施漏洞扫描。该示例希望将 [OWASP 的 WebGoat](https://owasp.org/www-project-webgoat/) Docker 映像部署到一个 Kubernetes 集群中，并解释扫描结果，以便在符合要求的情况下进行部署。

像往常一样，可以跟随博客和/或视频。所有的资产都在一个 [GitHub 仓库](https://github.com/ravilach/sonatype-harness)中。

## 利用 Harness 实施漏洞扫描

对于第一遍，到目前为止，显示可以调用 Nexus IQ，而不会出现线束问题。但是如果将它扩展到一个组织，还需要几个步骤。

从扫描的角度来看，将在 Harness Secrets Manager 中屏蔽 Nexus 凭据，然后在提交和解释扫描之前提示用户输入一些详细信息。

要为 Nexus IQ 用户和密码添加一对秘密，请利用 [Harness Secrets Manager](https://docs.harness.io/article/au38zpufhr-secret-management) 。

安全->机密管理->机密->加密文本+添加加密文本

名称:nexusiq _ 用户

值:管理员

单击提交后，添加另一个密码。

名称:nexusiq_password

值:admin123

添加了秘密之后，您可以利用[工作流变量](https://docs.harness.io/article/766iheu1bk-add-workflow-variables-new-template)来提示用户输入某些参数。

设置-> Webgoat ->工作流->扫描工件。在右侧，点击">工作流变量"

点击工作流变量旁边的铅笔，进行+添加。

例如，我们可以提示用户输入 Nexus IQ 应用程序 ID，以便扫描与正确的规则集相关联，等等。对于这个例子，我们只添加了一个变量，这是对所有输入进行参数化的艺术。

变量名:nexusiqappid

类型:文本

默认值:web goat-应用程序

单击“保存”后，您将看到工作流变量已填充。

使用 Harness，您可以使用语法$ { Secrets . get vaule(" secret _ name ")}引用[机密](https://docs.harness.io/article/ygyvp998mu-use-encrypted-text-secrets#step_4_reference_the_encrypted_text)，并使用${workflow.variables.name}访问[工作流变量](https://docs.harness.io/article/766iheu1bk-add-workflow-variables-new-template)。

使用以下更新来更新 Shell 脚本。

设置-> Webgoat ->工作流->扫描工件-> Shell 脚本

#下载扫描模板
mkdir-p ~/nexus-scans
CD ~/nexus-scans
sudo docker pull web goat/web goat-8.0
sudo docker save web goat/web goat-8.0:最新>webgoat-8.0.tar
Java-jar ~/nexus-CLI/nexus-IQ-CLI . jar-I $ { workflow . variables . nexusiqappid }-s http://<public _ IP>:8077

单击提交，您的更改已保存。您可以运行另一个部署来验证所做的更改。在工作流的右上角，单击部署。

点击提交，你将运行一个更具操作性的扫描方式。

一旦我们有了扫描数据，下一步就是解释结果，然后进行部署。我们可以生成另一对线束工作流(我们稍后将在[线束管道](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#pipelines)中缝合在一起)来收集结果。

创建新的工作流来解释扫描结果。

设置-> Webgoat ->工作流+添加工作流。

名称:解释扫描

工作流类型:滚动部署

环境:Nexus IQ 服务器

服务:Nexus IQ 外壳

基础设施定义:Nexus IQ CentOS

点击 Submit 后，创建一个工作流来部署 Webgoat。您将很快回来并填写扫描解释的详细信息。

设置-> Webgoat ->工作流+添加工作流

名称:部署 Webgoat

工作流类型:滚动部署

环境:Webgoat 农场

服务:Webgoat

基础设施定义:Goat K8s 集群

点击提交并返回工作流程列表后，您将看到三个线束工作流程。

下一步是构建某种解释逻辑来解析扫描结果。

下一步是将 shell 脚本添加到解释扫描工作流中。

## 以编程方式解释扫描结果

自动化 DevSecOps 实践的一部分是在扫描中发现违规漏洞时影响部署。目前，Nexus IQ 和 Harness 之间没有本机集成，但可以通过几个 cURL 命令访问和解析扫描结果。

需要的命令是获取内部 Nexus IQ 应用程序 ID、报告 ID，并对报告中的违规进行计数。如果存在严重违规，请停止部署。

要开始构建它，回到解释 Scan 并添加一个工作流变量，然后在部署步骤中添加一个 shell 脚本，类似于[第一部分](https://harness.io/blog/featured/vulnerability-scanning-ci-cd-pipeline/)。

设置-> Webgoat ->工作流->解释扫描

要添加的第一项是一个工作流变量，用于提示在 Nexus IQ 中违反[策略的严重程度，以停止部署。](https://help.sonatype.com/iqserver/managing/policy-management#PolicyManagement-GettingStartedwithPolicies)

单击工作流变量下的铅笔图标。

变量名:minfailpoilcyviolation

类型:数量

允许的值:5，6，7，8，9

默认值:7

必填:真

单击保存后，您可以在解释扫描工作流中的部署服务下添加 shell 脚本。

在添加步骤 UI 中，搜索 shell 并添加 Shell 脚本。单击下一步。

假设您的用户/密码仍在第一部分的线束密码管理器中，在配置 Shell 脚本提示符下，输入以下内容:

名称:Shell 脚本

类型:Bash

超时:5m

代表选择器:NexusIQScan

脚本:

#解释扫描结果-模板
echo“解释扫描结果”

# Get AppID
export AppID = $(curl-u $ { secrets . getvalue(" nexusiq _ user "))}:$ { secrets . getvalue(" nexusiq _ password ")}-X Get ' http:/<public _ IP>:8070/API/v2/applications？public id = web goat-应用程序“| jq”。应用程序[0]。id ' | tr-d ' " ')
echo " AppID:" $ AppID

# Get Report IDs
export Report id = $(curl-u $ { secrets . getvalue(" nexusiq _ user ")}:$ { secrets . getvalue(" nexusiq _ password ")}-X Get ' http://<public _ IP>:8070/API/v2/reports/applications/' $ AppID ' ' | jq '。[0] .reportDataUrl ' | cut-d '/'-f 6)
echo " report id:" $ report id

# Get 违例计数
export total violations = $(curl-u $ { secrets . getvalue(" nexusiq _ user "))}:$ { secrets . getvalue(" nexusiq _ password ")}-X Get " http://<public _ IP>:8070/API/v2/applications/webgoat-application/reports/" $ report id "/policy " | jq '。组件[]。违规[]。policyThreatLevel ' | grep-c '[$ { workflow . variables . minfailpoilcyviolation }-9]')
echo " Total Sev Violations:" $ Total Violations

" # Exit if Violations Match
if[[$ Total Violations-gt 0]]；然后
回显“违反阻止部署”
出口 1
否则
回显“向前推进部署”
出口 0
fi

点击 Submit，您的 Shell 和工作流变量应该会出现。你可以随意修改这个脚本，它是基于 [Nexus IQ REST API](https://help.sonatype.com/iqserver/automating/rest-apis/report-related-rest-apis---v2#Report-relatedRESTAPIs-v2-PolicyViolationsbyReportRESTAPI(v2)) 的。

下一步是将所有的工作流程整合到一个线束管道中。

## 自动化 CI/CD 开发人员管道-带线束

到目前为止，我们的三个独立的工作流是相互独立运行的。通过利用[线束管道](https://docs.harness.io/article/4o7oqwih6h-harness-key-concepts#pipelines)，您可以将三个工作流缝合在一起。

设置-> Webgoat ->管道+添加管道。将其命名为“DevSecOps”。

点击提交后，添加与每个工作流相对应的管道阶段。

一旦你点击+添加，第一步将是扫描工件。

执行工作流:扫描工件

点击 Submit 后，以类似的方式浏览并添加解释和部署步骤。

点击两个额外的阶段。首先，添加解释器。

然后，添加部署。

在部署阶段点击提交后，您的管道将有三个阶段。

现在，您已经准备好运行您的管道，并看到您的劳动成果。

## 运行您的第一个自动化 DevSecOps 管道

有几种方法可以利用管线。导航至左侧导航，并选择持续部署。

连续部署->部署->开始新的部署

选择 Webgoat 的标签[最新]并点击提交。

一旦点击了 Submit，您就可以看到您的第一个自动化 DevSecOps 管道运行了。不出所料，管道会失败，因为 Webgoat 有很多违规行为。

看看 Nexus IQ 报告，你会发现威胁确实存在。

http:// <public_ip>:8070 ->报表-> Webgoat ->查看报表</public_ip>

## Harness，您在 DevSecOps 的合作伙伴

无论您在 DevSecOps 的旅程中处于哪个阶段，Harness 都会与您携手合作。应用程序安全测试方法正在发展，更多的项目转向开发人员和自助服务。在您的管道中加入 DevSecOps 步骤是一个谨慎的举动。感谢您观看这两部分的系列，并确保今天就注册 [Sonatype Nexus IQ 试用](https://www.sonatype.com/nexus/lifecycle-demo)和[线束账户](https://harness.io/free-trial/)！

干杯，

拉维