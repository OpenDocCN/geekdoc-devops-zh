# Python 特性标志:新手和专家教程|线束

> 原文：<https://www.harness.io/blog/feature-flags-with-python>

如果您想快速入门使用 Python 代码库中的线束特性标志，这是为您准备的！

有许多关于最流行的编程语言的调查和测量，但是贯穿每一个[排名](https://www.northeastern.edu/graduate/blog/most-popular-programming-languages/)的一个共同线索是 Python 是最受欢迎的语言之一。这是一种初学者友好的语言，是数据科学的默认语言，通常是最实用的语言之一。如果您是 Python 的新手，或者是经验丰富的开发人员，想要快速入门在代码库中使用 Harness 特性标志，那么这是为您准备的！

## 先决条件

下载并安装 [PyCharm CE](https://www.jetbrains.com/pycharm/download) (如果不想创建账号，一定要选择社区版)。您选择的 IDE 通常可以工作，但是我的说明和截图将针对 PyCharm 的体验。

建议您对 Python 有一些经验，但是如果您已经熟悉另一种语言，它是一种非常友好的语言，并不难使用。下面的所有代码片段都包含在这个[要点](https://gist.github.com/chrisjws-harness/c462a6d1f9ee78181b216e8b11a7fd0c)中，所以如果你讨厌在博客上复制/粘贴代码块，我可以帮你。

**警告:**空格和缩进在 Python 中很重要，所以复制/粘贴时要小心。

**另一个警告:**带引号的复制/粘贴可能会导致编码问题，所以如果您的 IDE 在带引号的行上显示错误，请尝试删除并重新添加引号。

## 辅导的

用 Python 3 创建一个新项目(在这个例子中我使用的是 3.8.2)。

如果在项目中自动创建了 main.py 文件，请在继续之前将其删除。

在终端中，克隆 repo 并运行设置步骤。

#克隆 repo，移动文件并清理文件夹
git 克隆 https://github.com/chrisjws-harness/flaskSaaS.git&&mv flask SaaS/*(DN)。& & rm -rf 烧瓶 aaS

#设置应用程序
make install&&make dev
python manage . py initdb

...

…

### 设置功能标志 SDK

在 requirements.txt 中，在底部添加新的一行:

线束-特征标志==1.0.5

从终端安装更新的要求:

pip3 install -r requirements.txt

将以下导入语句添加到 app/views/main.py 的顶部:

从 featureflags.client 导入 CfClient，Target

在 app/views/main.py 中的导入语句下添加以下几行，并添加您的帐户 id:

meta = {
"account": " <你的账号 id 这里>"，
"org": "default "，
"environment": "PyCharm "，
"project": "Python_FF"
}

### 设置您的功能标志项目

在线束中，创建一个新项目。我的就叫 *Python FF* 。

导航到环境(左侧栏)并单击“创建环境”

将您的环境命名为“PyCharm”，将其保留为“非生产”，然后单击“创建”

在新环境中，单击“添加密钥”

命名为“my_key ”,并选择“Server”。点击“创建”

在生成的页面上复制您的令牌并保存在安全的地方。一旦您离开，此令牌将不可用。

导航至功能标志(位于左侧“目标”上方)。

单击“+旗帜”创建您的第一面旗帜。

选择“多变量”，将标志命名为“消息”，并给它一个描述(可选)。点击“下一步”

提供以下消息作为名称、值对，为关闭和打开设置不同消息的默认规则，然后单击“保存并关闭”

**名称值**友好欢迎！欢迎，我想我该走了

### 绑上你的旗帜

在 app/views/main.py 中，找到注释“Feature flags init goes here！”并在下面添加以下内容。用您为环境生成的密钥替换<your key="">:</your>

try:
API _ key = "<your key>"
cf = cf client(API _ key)
ff _ identifier = " guest "
ff _ name = " guest "
Target = Target(identifier = ff _ identifier，name=ff_name，* * meta)
异常为 e:
print(e)

在同一个文件中，在注释“Flag goes here！”：

flags[" welcome _ text "]= cf . string _ variation(" message "，target，"默认消息")

### 尝试您的功能标志

在 CLI 中，运行以下命令:

python manage.py runserver

在您的浏览器中，导航到 [http://localhost:5000](http://localhost:5000) ，在这里您将看到您的标志关闭消息。功能标志可能需要一些时间来初始化，因此您可能需要刷新页面。

将“消息”的标志切换为真。

刷新页面，查看主页上的消息更新:

神奇！

## 下一步是什么？

你已经完成了大部分工作。在 UI 中创建新的标志，并在代码中添加相应的语句。为了更好地理解 Python 中的特征标志，接下来要研究一些东西:

*   查看完整的 Python 文档[这里](https://ngdocs.harness.io/article/hwoxb6x2oe-python-sdk-reference)和[这里](https://github.com/drone/ff-python-server-sdk/blob/main/README.rst)。
*   使用“bool_variation”而不是字符串变体。
*   添加关于如何提供标志的附加规则。
*   我们为 guest 返回了一个简单的标识符，但是看看当您识别其他用户并基于身份和属性提供不同的行为时会发生什么。
*   探索 RBAC 以及如何限制用户在功能标志中的行为。

下次见！

克里斯