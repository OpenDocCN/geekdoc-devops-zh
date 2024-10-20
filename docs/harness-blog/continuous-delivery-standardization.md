# 解决连续交付中的标准化问题|线束

> 原文：<https://www.harness.io/blog/continuous-delivery-standardization>

从平台工程学科来看，拥有坚实的平台来进一步提高工程效率肯定会带来好处。开发软件需要大量的反复试验，拥有支持反复试验的平台是至关重要的。在我们获得正确的特性之前，通过您的管道多次提交构建和工件是我们经历 SDLC 时常见的。尽管经历潮起潮落来构建我们提交应用程序的管道，例如集成、测试和部署分布式应用程序可能是痛苦的。与我们合作的大部分[客户和潜在客户](https://harness.io/wp-content/uploads/2019/05/Harness_Ruckus.pdf)都面临着管道标准化的挑战。很明显，一个尺寸并不适合所有的应用，但是挑战是保持创新速度，同时仍然有护栏/激励来避免反模式。在 Harness，我们专为解决这一问题而打造。

## 智能客户端和(非)哑管道

根据你看的微服务宣言，一个共同的主题是“[智能客户端和哑管道](https://martinfowler.com/articles/microservices.html#SmartEndpointsAndDumbPipes)”。这个概念将逻辑移向发布或消费端点。在消息世界中，利用一个[轻量级协议](https://en.wikipedia.org/wiki/Streaming_Text_Oriented_Messaging_Protocol)来发送许多消息，并让接收者/消费者(也称为客户端)解密如何处理这些消息。你的[连续输送管道](https://harness.io/2019/12/continuous-delivery-cheat-sheet/)也不应该这样说。在以前的版本中，我们的现代分布式应用程序依赖哑管道(想想詹金斯管道)将你的想法转化为我们今天部署的应用程序。创建所需的智能管道需要插件和冗长的编排脚本的大杂烩，就像您“钟爱的”Jenkins 环境一样。今天的现代和健壮的分布式应用以类似的方式需要健壮且易于利用的基础设施，以将您的想法转化为我们今天部署的应用。

## 想象建造一个新家

几年前，我搬进了一栋新建筑。这个社区是全新的，站在新铺的道路上，你会注意到许多污水管从地下伸出来。每个地方都有这样的下水道。尽管随着时间的推移，每个地块上都建起了独一无二的住宅，污水管道也隐藏了起来，与住宅融为一体。房屋建筑商不是从下水道获得价值，而是从他们建造的房屋中获得价值。尽管没有下水管道，这些房子还是可以居住的。与当今许多企业的前端和中心应用程序类似，管道本身不会带来太多价值，但像家庭这样的应用程序是创意和增值所在。借助 Harness 平台，您的团队可以轻松实现自助服务，创建满足您当前和未来企业标准的管道。

## 你的第一个标准化管道

有几个途径可以开始在 Harness 平台中标准化您的管道。现代平台需要允许您保持创新，同时允许观点和护栏到位以进行业务控制。找到一种有效的方法来做到这一点是大多数软件工程团队面临的众所周知的挑战。通过将[管道治理](https://developer.harness.io/docs/first-gen/firstgen-platform/security/governance-howtos/pipeline-governance/)、[模板](https://developer.harness.io/docs/platform/templates/template/)和[配置组合为代码](https://docs.harness.io/article/htvzryeqjw-configuration-as-code)，您和您的组织将顺利地解决众所周知的管道挑战。

### 管道治理

我们最近在 Harness 平台中引入了管道治理。管道治理可以总结为一种度量一致性的方法。我们有一篇非常详细的关于管道治理的艺术的博客文章。为了在管道治理中快速旋转，转向持续安全- >治理。

然后点击+添加治理标准。可以制定一个一致性标准来应用于您的[线束管道](https://developer.harness.io/docs/platform/templates/create-pipeline-template/)。创建一个称为“金金丝雀标准”的管道治理标准。

添加规则将允许我们对一致性进行评分。例如，假设我们想要进行某种一致性测试，例如，如果我们的应用程序是可伸缩的。我们可以加上“秤 App”这个名字。

单击提交后，标准将显示在列表中。

当您返回到治理屏幕时，您可以将开关移动到上的*来启用。*

一旦启用了管道治理，下一步就是连接到一个线束管道。可以导航到[线束应用](https://docs.harness.io/article/bucothemly-application-configuration)。在线束工作流程中，您可以利用[线束标签](https://developer.harness.io/docs/platform/references/tags-reference/)来关联线束间的信息。由于我们值得信赖的 Pivotal Cloud Foundry 工作流程[ [来自我们的 PCF 博客](https://harness.io/2020/01/platform-as-a-service-series-part-3-3-paas-and-harness-better-together/) ]有一个缩放/调整大小的步骤，所以添加一个“缩放应用程序”的标签。

创建工作流后，如果还没有管线，请添加名为“黄金管线”的线束管线。设置->应用程序->管道+添加管道。

点击提交后，通过点击 *+* 添加一个管道阶段。

添加一个管道阶段，调用步骤名称“First ”,并选择您想要执行的工作流。

点击提交后，底部将显示管道可用的管道治理规则。由于我们没有限制管道治理，将对我们的所有管道可用。在这里，我们符合我们的标准。光荣！

符合标准是 Harness 平台帮助实现的一个支柱。在橡胶遇到路面的地方，你也可以将利用预制件的任务模板化。

### 模板

[模板](https://developer.harness.io/docs/platform/templates/template/)是 Harness 平台的标志。在我们的专业版中，你可能已经在你的模板库中看到一些种子。通过导航至设置- >模板库，可以查看您有哪些可用的模板

重温我们关于模板的博文，连接和利用模板非常简单。通过创建一个快速 Shell 脚本作为模板，您可以看到模板是多么容易连接。在+添加模板下拉列表中，选择 Shell 脚本。

举个例子，让我们重复我们的名字[在管道上留下你的印记是很重要的！].将模板命名为“*简单模板*”。

*回显“你好我的名字是$ { name }”*在底部，添加一个名为“ *name* 的变量，这样就可以输入一个名字了。

单击提交后，模板就可以使用了。

现在，您可以将模板添加到您的工作流程中。利用 PCF 应用程序，在应用程序上留下我们的印记，让我们在 Shell 脚本中回显我们的名字。在一个工作流中，在第一个部分下添加一个命令，在 PCF_Perfection 的*基本 PCF* 工作流案例中是在“1。Setup" + Add 命令

在“添加命令”框中，选择“从共享模板库中选择”。你的“简单模板”应该在那里。

点击简单模板上的*链接*。

然后点击提交。现在，您的“简单模板”是执行步骤的一部分。

一旦您执行了工作流，您就可以看到您的步骤被执行。

这是一个非常简单的例子。模板有很多可能的艺术。接下来让我们看一下 GitOps 聚焦的方式，用 Harness 的配置作为代码。

### 配置为代码

最后，您可以用 Harness 作为代码配置来连接一个[基于 Git 的同步](https://docs.harness.io/article/htvzryeqjw-configuration-as-code)。深入我们的 [Pivotal Cloud Foundry 博客的](https://harness.io/2020/01/platform-as-a-service-series-part-3-3-paas-and-harness-better-together/)树，可以设置 Git Sync。最简单的方法就是使用像 [GitHub](https://github.com/) 这样的服务。第一步是通过 Setup->Connectors->Source Repo Providers+Add Source Repo Provider 将 GitHub 存储库连接到 Harness。

输入您的 GitHub 资源库的详细信息。确保选中*生成 Webhook URL。*

您的 Webhook URL 将在一个对话框中生成，您可以复制或返回到您的配置进行查看。您最终将需要 Webhook URL 来连接到您的 GitHub 存储库。

是时候按照代码配置来设置线束树了。导航至设置->配置为代码。

在线束树上选择椭圆来设置 Git Sync。

使用刚刚设置的 Git 连接器启用 Git Sync。在我的例子中，我没有一个[分支](https://git-scm.com/docs/git-branch)，所以只使用 master。

一旦点击 submit，就可以验证工件现在是否在 GitHub 存储库中。

通过在线束树上导航到一个应用并点击 Git 图标，例如 *PCF_Perfection* 旁边的图标，可以将单个应用添加到。

单击 Git 图标，在应用程序上设置 Git Sync。

点击提交后，将会出现同步图标。

要连接 Webhook 以获得事件通知，请访问您的 GitHub 帐户并选择 Repository -> Settings

网页挂钩->添加网页挂钩

将线束中的 WebHook URL 粘贴为有效负载 URL。确保将内容类型更改为*应用程序/json* 。

神奇的是，如果您想创建另一个 Harness 环境，例如，Git 可以在您给新环境取了一个不同的名称后克隆这个环境。GitHub 有一个[桌面客户端](https://desktop.github.com/)如果你像我一样喜欢 GUI，你可以使用。可以开始使用[克隆](https://git-scm.com/docs/git-clone)你的库吗？我的私人例子 repo 叫做“captaincanary”。

一旦克隆，这个用户界面将出现。

可以在你的文件浏览器中复制并粘贴一个环境，例如 Finder。复制并粘贴暂存。

粘贴并给出一个新的名称，如“性能”

您可以[将变更提交](https://git-scm.com/docs/git-commit)回分支，在我的例子中是 master。

下一步点击[推动](https://git-scm.com/docs/git-push)原点。

您的性能环境现在将出现在 GitHub 中。

也反映在线束树中。

如果您浏览到具有该环境的应用程序结构，您的新性能环境就在那里。

如果您想克隆一个应用程序，也可以用类似的方式复制整个 Harness 应用程序。在你的文件浏览器中，比如 Finder，复制整个“PCF_Perfection”文件夹。

使用新名称粘贴，例如 PCF _ Prowess。

在 Git 中提交更改。

推原点。

哒哒！PCF 威力在 GitHub 和线束树中，UI 在设置->应用程序下。

也在线束用户界面中。

如果有需要在整个组织中共享的工具资产，克隆的方法会非常有用。在 Harness，我们将帮助您解决这些挑战。

### 驾驭，与您一起解决挑战

对于任何一个提供工程效率的平台来说，在创新和政策之间找到平衡点都是一件困难的事情。有了 Harness 平台和 Harness 提供的专业知识，调整收音机表盘变得很简单。如果在工程效率方面获得正确的无线电拨号盘对您/您的组织很重要，那么今天就注册吧。干杯！拉维