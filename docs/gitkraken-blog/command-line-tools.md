# 命令行工具 CLI 简介第 3 部分

> 原文：<https://www.gitkraken.com/blog/command-line-tools>

点击下面阅读我们 CLI 简介系列的其他文章:

在本系列的开始，我们看到了在您的机器上使用 CLI shell 的好处。其中一个好处是有大量的开发者工具可以使用，这些工具只有一个命令行界面。

在 CLI 简介系列的这一部分中，我们将重点关注:

对于那些完全不熟悉开发术语的人来说，命令行工具是一个通用术语，用于描述完全从 CLI Shell 运行的程序、应用程序或脚本。命令行工具不包括 GUI，只是简单地接受用户的文本输入并将结果打印到屏幕上。

无论你如何使用命令行，GitKraken 客户端的 CLI 将帮助你轻松地自动完成你的命令！

在关于 shell 命令的上一节的最后，我们[分享了一个方便的 shell 命令列表](https://www.gitkraken.com/blog/shell-commands#handy-commands)，比如`head`和`tail`。您在 shell 中键入的命令会触发与您的 CLI shell 自动附带的同名应用程序。Shell 命令实际上是对计算机上程序的调用。

有很多实用程序，就像那些内置在默认体验中的程序，给你提供了与你的机器进行交互的强大方法。

### **命令结构**

所有 CLI shell 命令和应用程序都遵循相同的基本结构。那个结构看起来是这样的:

`$ program-name specific-action -options parameters`

第一部分:`$ program-name`指定你调用的是哪个应用。

第二部分:`specific-action`告诉应用你想做什么。可选地，您可以提供标志，这里显示为`-options`，以引起某些行为或设置。

最后，参数是您需要输入来完成命令的任何内容。参数有时也是可选的。随着你开始更多地使用命令行，这将变得更有意义。实践是学习任何东西的最好方法，所以在另一个窗口打开一个终端，也许是通过 [GitKraken 客户端终端标签](https://www.gitkraken.com/cli)，让我们看看 [Zsh](https://www.gitkraken.com/blog/cli-stands-for-a-cli-intro-series#zsh) 附带的一些内置命令行工具。

### **系统实用程序**

CLI shell 中包含的执行特定操作的程序，如`ls`或`pwd`，通常被称为系统实用程序。除了我们在上一篇文章中提到的，shell 中还内置了许多其他组件。

#### **命令行日期工具**

要将当前日期打印到屏幕上，使用:

`$ date`

通过 CLI 显示日期有很多选项，包括以不同的格式打印，或只显示日期的一部分，如一个月或一天中的时间。

使用`man date`查看选项的完整列表。当您在终端中工作时，Date 是一个方便的 CLI 程序，但是当您开始编写脚本时，它变得非常有用。

#### **命令行时间工具**

你可能认为一个叫做时间的工具会告诉你时间，就像`date`告诉你日期一样。没有。时间执行一个命令，并计算运行时间。这个 CLI 程序还提供了关于执行所花费的处理时间和系统资源的有用信息。

要定时任何命令，使用下面的命令行工具:

`$ time command`

在上面的例子中，`time` CLI 程序检查了安装了哪个版本的 npm。您可以看到它花费了 0.27 秒的用户时间，0.16 秒的系统时间，在命令执行期间使用了 66%的 CPU，从开始到结束总共花费了 0.651 秒。

用户时间和系统时间有什么区别？如果你想了解更多，堆栈溢出上有一个关于 [`time output`这个令人惊讶的深刻主题的很好的解释。当您开始构建脚本和编写程序时，这个命令行工具变得非常方便。](https://stackoverflow.com/questions/556405/what-do-real-user-and-sys-mean-in-the-output-of-time1)

#### **命令行比较工具**

Git 借用了 CLI shell 的很多概念。它借用的一个想法是在文件之间运行一个 diff 来显示不同之处。在 Git 中， [Git diff](https://www.gitkraken.com/learn/git/git-diff) 比较同一个文件的不同版本；但是早在 Git 版本控制出现之前，CLI 用户就可以轻松地比较多个文件。

要显示 2 个文件的区别，使用下面的命令行工具:

`$ diff file1 file2`

**Tar 和 Gzip**

内置于现代 CLI shells 中的是一种将文件打包并压缩到单个归档文件中的方法，这使得传输任何规模的项目都很容易。虽然这里有两个命令，但它们是一起使用的，很少会看到一个命令没有另一个命令。

#### 第一个命令行工具是:`tar`，它将事物捆绑在一起成为单个对象，通常被称为 [`tarball`](https://en.wikipedia.org/wiki/Tar_(computing)) 。

第二个命令行工具是:`gzip`，它将 tarball 压缩成易于共享的压缩文件。

已经“打包”和压缩的文件将有一个`.tar.gz`扩展名。看到许多 Linux、开源和一些 macOS 可下载文件以这种方式存档是很常见的。

要把一个文件夹的内容和 zip 文件捆绑成一个可移植的存档，使用下面的:

`$ tar -czvf name-of-archive.tar.gz directory-or-file`

第二个命令行工具是:`gzip`，它将 tarball 压缩成易于共享的压缩文件。

已经“打包”和压缩的文件将有一个`.tar.gz`扩展名。看到许多 Linux、开源和一些 macOS 可下载文件以这种方式存档是很常见的。

要把一个文件夹的内容和 zip 文件捆绑成一个可移植的存档，使用下面的:

`$ tar -czvf name-of-archive.tar.gz directory-or-file`

好多旗子啊！不要惊慌。虽然这可能看起来相当过时，但这些选项中的每一个都代表了一个易于理解的想法。他们的意思是:

`-c`:创建一个档案。

`-z`:用 Gzip 压缩存档。

*   `-v`:创建归档文件时在终端显示进度，也称为“verbose”模式。`v`在这些命令中是可选的，但它有助于确保工具按预期运行。
*   `-f`:允许您指定档案的文件名。
*   `-v`:创建归档文件时在终端显示进度，也称为“verbose”模式。`v`在这些命令中是可选的，但它有助于确保工具按预期运行。
*   要提取 tar.gz 归档文件，请使用以下命令:

`$ tar -xzvf name-of-archive.tar.gz`

要提取 tar.gz 归档文件，请使用以下命令:

`$ tar -xzvf name-of-archive.tar.gz`

因为您已经使用了`-c`来创建归档文件，所以您将使用`-x`来提取归档文件。

**Grep**

“Grep”代表全局正则表达式打印。这是有史以来最强大的文本搜索工具之一。学习使用这个命令行工具将帮助您非常快速地找到单独的代码片段，加快您的工作流程，尤其是对于大型存储库。

Grep 搜索任何指定的文件，选择匹配一个或多个所需模式的行，这些模式通过[正则表达式匹配](https://en.wikipedia.org/wiki/Regular_expression)来识别。匹配至少一个模式的每个输入行将被写入标准输出。

在单个文件中查找一个字符串，使用下面的:

`$ grep ‘search_string’ filename`

#### **Grep**

“Grep”代表全局正则表达式打印。这是有史以来最强大的文本搜索工具之一。学习使用这个命令行工具将帮助您非常快速地找到单独的代码片段，加快您的工作流程，尤其是对于大型存储库。

要在同一目录中共享同一文件类型的任何文件中查找字符串，请使用以下命令:

`$ grep ‘search_string’ *.extension`

在这个例子中，我们使用通配符`*`来指定我们指的是以扩展名`.txt`结尾的任何文件。您也可以单独使用通配符来搜索所有文件。

要在同一目录下的任意文件中查找一个字符串，使用如下:

`$ grep ‘search_string’ *`

在这个例子中，我们使用通配符`*`来指定我们指的是以扩展名`.txt`结尾的任何文件。您也可以单独使用通配符来搜索所有文件。

要在同一目录下的任意文件中查找一个字符串，使用如下:

默认情况下，Grep 只在当前的工作目录中查找，但是告诉 Grep 搜索子目录也很容易。

要在当前任意文件或任意子目录中查找字符串，使用以下:

`$ grep -r ‘search_string’ *`

Grep 有很多很多选项，可以提供各种信息，给你很多搜索选项。值得花时间通读 Grep 手册，通过运行`$ man grep`来熟悉什么是可能的。这是一个使用 Grep 的高级示例，它使用了多个标志。

要查找一个字符串出现的次数，使用不区分大小写的搜索，从当前目录开始搜索所有子目录，使用以下命令:

`$ grep -Ric ‘search_string' .`

Grep 有很多很多选项，可以提供各种信息，给你很多搜索选项。值得花时间通读 Grep 手册，通过运行`$ man grep`来熟悉什么是可能的。这是一个使用 Grep 的高级示例，它使用了多个标志。

要查找一个字符串出现的次数，使用不区分大小写的搜索，从当前目录开始搜索所有子目录，使用以下命令:

在这个例子中，我们组合了多个选项来给出字符串在每个文件中出现的次数(`-c`)，不考虑大小写(`-i`)，来自当前目录中的所有文件和当前位置(`-R`)的任何子目录。

**管道 Grep 输出**

在本系列的第一部分中，您了解了 Bourne 的 Shell。Sh”和后来的 Bash 变得流行起来，因为它能够将一个命令的输出作为另一个命令的输入。

这种能力被称为管道，使用`|`字符作为操作符。这是一个非常强大的命令行工具，可以让你做一些非常复杂的搜索。让我们看一个实际的例子:

##### 在前面的命令`$ grep -Ric`示例中，您看到 Grep 列出了它搜索的每个文件，以及每个搜索词出现的次数。

在这个例子中，直观地扫描输出并不困难，因为只有几个文件。但是想象一下有成百上千个文件的情况。手动读取输出来查找搜索词出现两次的文件几乎是不可能的。但对 Grep 来说不是。

要使用 Grep 来搜索 Grep 搜索的结果，使用下面的:

`$ grep ‘search_string' directory/file | grep ‘search_string'`

这种能力被称为管道，使用`|`字符作为操作符。这是一个非常强大的命令行工具，可以让你做一些非常复杂的搜索。让我们看一个实际的例子:

在前面的命令`$ grep -Ric`示例中，您看到 Grep 列出了它搜索的每个文件，以及每个搜索词出现的次数。

在上面的例子中，我们使用 Grep 搜索第一个 Grep 命令的输出，寻找字符串`:2`。现在我们知道了当使用不区分大小写的搜索来搜索时，哪个文件包含了原始术语`Line 2`。这种递归搜索在检查日志文件或调试代码时非常有用。

在 Bash 和 Zsh 中，您可以将任何命令的输出通过管道传递给任何其他命令的输入。我们将在脚本部分详细讨论这一点。

当你在 VS 代码中工作时，在 repos 中搜索你想要的东西会变得容易得多。强大的用户界面将释放一些真正的 Git 超级用户的力量！立即安装

在上面的例子中，我们使用 Grep 搜索第一个 Grep 命令的输出，寻找字符串`:2`。现在我们知道了当使用不区分大小写的搜索来搜索时，哪个文件包含了原始术语`Line 2`。这种递归搜索在检查日志文件或调试代码时非常有用。

在 Bash 和 Zsh 中，您可以将任何命令的输出通过管道传递给任何其他命令的输入。我们将在脚本部分详细讨论这一点。

**命令行联网工具**

**乒**

[Install GitLens for VS Code](vscode:extension/eamodio.gitlens)

Ping 工具用于测试特定主机是否可以通过 IP 网络到达。“ping”告诉您数据包从本地主机发送到目的计算机并返回需要多长时间，同时显示沿途的任何数据包丢失。

### 当构建 web 应用程序或在代码中利用 web 服务时，这是一个非常方便的命令行工具。

要测试主机在互联网上是否可达，请使用以下命令:

`$ ping URL`

**乒**

#### 在上面的例子中，您将看到`gitkraken.com`显示 Ping 信息，没有任何问题，这意味着有问题的站点正在工作。然而，当 pinging】时，您会看到一个错误，告诉您该 URL 是不可解析的，这意味着有一个问题到达它。

**卷曲**

cURL 代表“客户端 URL”。这是一个命令行工具，用于从 URL 传输数据，它有很多选项。虽然 cURL 有很多用例，但这里有两个开发人员经常使用的非常有用的命令。

cURL 代表“客户端 URL”。这是一个命令行工具，用于从 URL 传输数据，它有很多选项。虽然 cURL 有很多用例，但这里有两个开发人员经常使用的非常有用的命令。

**从 URL 复制文件**

要从一个 URL 复制一个文件并将内容存储在一个同名的本地文件中，请使用下面的:

`$ curl -O URL-of-file`

**卷曲**

#### cURL 代表“客户端 URL”。这是一个命令行工具，用于从 URL 传输数据，它有很多选项。虽然 cURL 有很多用例，但这里有两个开发人员经常使用的非常有用的命令。

在上面的例子中，位于[https://raw . githubusercontent . com/MCD Wayne/Moby-dick/main/chapter/001-loomings . MD](https://raw.githubusercontent.com/mcdwayne/moby-dick/main/chapter/001-loomings.md)的文件的全部内容被复制到一个本地文件中，也命名为`001-loomings.md`。注意:我们使用了大写的`O`，而不是零或小写。另外，请注意:该文件的内容是从文件的`raw`版本卷曲而来的，这意味着没有标题或样式信息，只有内容的纯文本。如果您尝试从默认的 GitHub 视图中复制该文件，它也会将该文件周围的所有 HTML 都拉出来。

**从 URL 复制文件**

##### **检查网站标题**

要查看一个网站的标题，使用下面的:

`$ curl –head URL`

要查看一个网站的标题，使用下面的:

`$ curl –head URL`

在上面的例子中，位于[https://raw . githubusercontent . com/MCD Wayne/Moby-dick/main/chapter/001-loomings . MD](https://raw.githubusercontent.com/mcdwayne/moby-dick/main/chapter/001-loomings.md)的文件的全部内容被复制到一个本地文件中，也命名为`001-loomings.md`。注意:我们使用了大写的`O`，而不是零或小写。另外，请注意:该文件的内容是从文件的`raw`版本卷曲而来的，这意味着没有标题或样式信息，只有内容的纯文本。如果您尝试从默认的 GitHub 视图中复制该文件，它也会将该文件周围的所有 HTML 都拉出来。

在上面的例子中返回了很多信息，但是很快你就可以知道:

响应来自哪个服务器，在本例中是 GitHub.com，这很有意义，因为这是一个 GitHub 页面

##### 该网站最后一次更新是在 2022 年 1 月 31 日，星期一

该网站使用 Varnish 作为缓存服务器

缓存被命中，命中时已有 86 秒

在上面的例子中返回了很多信息，但是很快你就可以知道:

在调试缓存和 web 应用程序问题时,`curl –head`命令行工具非常有用。

*   **宋承宪**
*   SSH 代表[安全外壳协议](https://en.wikipedia.org/wiki/Secure_Shell)，简单来说就是一种建立连接的方式，允许你在另一台计算机上使用外壳。这是管理远程服务器或连接到远程服务的安全方式。
*   像 [GitHub](https://www.gitkraken.com/integrations/github) 、 [GitLab](https://www.gitkraken.com/integrations/gitlab) 和 [Bitbucket](https://www.gitkraken.com/integrations/bitbucket) 这样的 Git 托管服务依靠 SSH 在用户之间高效地来回传输数据。虽然大多数情况下，服务需要现有的帐户和特定的登录凭证，但也有一些公共服务允许 SSH 连接。本指南中的这个使用了 [Telehack](https://telehack.com/telehack.html) ，这是一个惊人的资源，可以安全地探索命令行能做什么！

    要使用 [SSH](https://www.gitkraken.com/learn/git/tutorials/how-git-ssh-works) 连接到远程服务器，请使用以下命令:

    `$ ssh username:hostname-or-ip-address -p port-number`
*   像 [GitHub](https://www.gitkraken.com/integrations/github) 、 [GitLab](https://www.gitkraken.com/integrations/gitlab) 和 [Bitbucket](https://www.gitkraken.com/integrations/bitbucket) 这样的 Git 托管服务依靠 SSH 在用户之间高效地来回传输数据。虽然大多数情况下，服务需要现有的帐户和特定的登录凭证，但也有一些公共服务允许 SSH 连接。本指南中的这个使用了 [Telehack](https://telehack.com/telehack.html) ，这是一个惊人的资源，可以安全地探索命令行能做什么！

    要使用 [SSH](https://www.gitkraken.com/learn/git/tutorials/how-git-ssh-works) 连接到远程服务器，请使用以下命令:

    `$ ssh username:hostname-or-ip-address -p port-number`

在调试缓存和 web 应用程序问题时,`curl –head`命令行工具非常有用。

在上面的例子中，我们以来宾身份连接，这在大多数主机上是不允许的。大多数情况下，你需要有一个用户账户，设置一个密码，或者用一个 [SSH 密钥对](https://www.ssh.com/academy/ssh/public-key-authentication)进行连接。

虽然这里不深入讨论，但本质上 SSH 密钥对是由两个非常长的字母和数字组成的一组字符串，一个是公共的，一个是私有的，它允许您安全地通过互联网进行身份验证。您与远程服务共享公钥，而私钥则保存在您的设备上。当你第一次连接时，一个基于陷门函数的复杂算法[会运行，以查看公钥和私钥是否对应。如果它们一致，则建立安全的加密连接。](https://en.wikipedia.org/wiki/Trapdoor_function)

#### **ssh-keygen**

当通过 SSH 连接到远程服务时，您很可能需要使用 SSH 密钥进行身份验证。这种设置非常简单，并且内置于所有 CLI shells 中。

要生成一个新的 SSH 密钥对，使用下面的:

`$ ssh-keygen`

当通过 SSH 连接到远程服务时，您很可能需要使用 SSH 密钥进行身份验证。这种设置非常简单，并且内置于所有 CLI shells 中。

要生成一个新的 SSH 密钥对，使用下面的:

`$ ssh-keygen`

除了命令之外，您还可以提供许多可选参数，但是输入命令本身会提示您输入完成创建过程所需的信息。上面的例子只显示了第一步。

生成密钥后，默认情况下，您可以在您机器的`~/.ssh`文件夹中找到新密钥。会有两个文件输出:`id_rsa`和`id_rsa.pub`。`.pub`代表 public，您应该将它添加到远程服务中。每项服务都将提供如何最好地上传和存储您的公钥的说明。

关于 SSH 密钥的一些最终提示:

永远不要与任何人或任何服务分享你的私人密钥。如果服务要求您上传私钥，最佳做法是为该服务创建一个新的专用密钥对。RedHat 有一个很好的多 SSH 密钥指南来管理这种模式。

不要将任何密钥复制/粘贴到任何代码库中。

#### 经常更改您的 SSH 密钥和密码。尽量确保如果一个坏演员确实发现了一个泄漏的密钥，它在他们有机会使用它之前就过期了。

撤销不再使用的服务上的 SSH 密钥。只有经常使用的服务才应该随时拥有您的公钥。结合定期轮换您的密钥，这是保持安全的最佳安全实践。

撤销不再使用的服务上的 SSH 密钥。只有经常使用的服务才应该随时拥有您的公钥。结合定期轮换您的密钥，这是保持安全的最佳安全实践。

**使用命令行工具编辑文本**

**Vim**

Vim 代表 Vi 改进编辑器，是最古老的文本编辑器之一，也是最难学的。Vim 有一种永远不需要将手从键盘上移开的世界观，一旦你跨越了非常陡峭的学习曲线，它就是一个非常强大的编辑器。

如果您曾经提交了一个[Git](https://www.gitkraken.com/learn/git/commit)并且没有提供一个 [Git 提交消息](https://www.gitkraken.com/learn/git/best-practices/git-commit-message)，那么您很可能会进入 Vim 编辑器。在您将会遇到的许多系统上，它被设置为默认的文本编辑器，所以最好至少对这个命令行工具有一些了解。

人们对 Vim 最常见的问题是:“我如何退出 Vim？”这个问题的答案实际上揭示了一点 Vim 是如何工作的。

要退出 Vim，请遵循以下步骤:

按下`esc`键

*   键入`:wq`并按下`enter`/回车键。
*   经常更改您的 SSH 密钥和密码。尽量确保如果一个坏演员确实发现了一个泄漏的密钥，它在他们有机会使用它之前就过期了。
*   撤销不再使用的服务上的 SSH 密钥。只有经常使用的服务才应该随时拥有您的公钥。结合定期轮换您的密钥，这是保持安全的最佳安全实践。
*   Revoke SSH keys on services you are no longer using. Only actively used services should have your public key at any time. Combined with rotating your keys on a regular basis, this is a great security best practice to stay safe and secure.  

当您按下`esc`键时，您是在告诉 Vim 您想进入“命令模式”只需要担心另一种模式，“插入模式”，它允许您键入文件。命令模式允许您输入影响编辑器本身的高级命令。

### 所有命令都以冒号“:”开头。命令`:wq`代表“将屏幕上的内容写入文件”，意思是保存文件，并退出 Vim。

学习 Vim 有很多好处，从人机工程学到更快的文件导航，最终能够更快地编写可发布的代码。学习 Vim 的一个很好的资源是 Drew Neil 的书《实用 Vim 》。

**纳米**

#### 幸运的是，在大多数系统上有更多的文本编辑命令行工具可供选择，其中之一就是 Nano。这个文本编辑器也非常强大，有许多可用的命令和选项，但与 Vim 不同，它使用起来更友好，因为它在屏幕上显示了一些选项，告诉您如何保存和退出应用程序。

人们对 Vim 最常见的问题是:“我如何退出 Vim？”这个问题的答案实际上揭示了一点 Vim 是如何工作的。

要退出 Vim，请遵循以下步骤:

按下`esc`键

1.  键入`:wq`并按下`enter`/回车键。
2.  从上面的例子可以看出，Nano 界面的底部提供了一个选项列表，您可以通过按下`cmd`键和所需选项的字母来执行。

例如:要退出 Nano，键入`cmd+x`，或者，要剪切一行文本键入`cmd+k`。

例如:要退出 Nano，键入`cmd+x`，或者，要剪切一行文本键入`cmd+k`。

自 Vim 以来，文本编辑器已经走过了漫长的道路。以 VS 代码为例，它对用户友好得多，并且可以用 GitLens 这样的强大工具进行扩展。

**Sed**

另一个值得注意的编辑选项是“流编辑器”sed。stream editor 是指一种命令行工具，它可以通读文件，并在找到您想要查找和替换的术语的实例时进行更改。

像 Vim 一样，一开始理解起来可能有点棘手，但是这是一个非常强大的命令行工具，可以一次编辑多个文件。你可以把它看作一个全局搜索和替换工具，因为这是主要的用例之一，它可以跨多个文件和目录运行。

要搜索和替换文件中的一个字符串，使用下面的:

`$ sed -I ‘.ext’ ‘s/old-string/new-string/options’ filename`

#### 幸运的是，在大多数系统上有更多的文本编辑命令行工具可供选择，其中之一就是 Nano。这个文本编辑器也非常强大，有许多可用的命令和选项，但与 Vim 不同，它使用起来更友好，因为它在屏幕上显示了一些选项，告诉您如何保存和退出应用程序。

Fortunately, there are more options for a text editing command line tools available on most systems, one of which is Nano. This text editor is also pretty powerful, with many commands and options available, but unlike Vim, it is much friendlier to use, as it presents onscreen options that inform you how to do things like save and quit the application.

上例中运行的确切命令是:
`sed -i '.bak' 's/Line 2/Line X/gi' t1.txt`。

让我们快速分解一下。

`-i`代表“编辑文件 **i** n-place”。

`.bak`告诉 sed，您想要制作一个以`.bak`结尾的原始文件的副本，这是“备份”的简写。

sed 命令的核心是中间的正则表达式搜索，即`'s/Line 2/Line X/gi`部分。

第一个`s`代表“替代”,这是我们希望 sed 最终要做的。还有其他选择，但`s`是最常见的。

`/Line 2/Line X/`告诉 sed 要查找什么以及用什么替换它。

[Install GitLens for VS Code](vscode:extension/eamodio.gitlens)

结尾的选项`gi`代表全局，意味着文件中的每一个事件，不区分大小写。

#### 最后一部分只是搜索所针对的文件的名称。

像 Vim 一样，一开始理解起来可能有点棘手，但是这是一个非常强大的命令行工具，可以一次编辑多个文件。你可以把它看作一个全局搜索和替换工具，因为这是主要的用例之一，它可以跨多个文件和目录运行。

要搜索和替换文件中的一个字符串，使用下面的:

`$ sed -I ‘.ext’ ‘s/old-string/new-string/options’ filename`

乍一看，这似乎有点晦涩难懂，但是值得花时间去理解这个命令行工具。想象一下这样一个场景，对计费系统的更改意味着您需要修改代码库中变量名的所有实例。手动在数千个文件中查找和替换该字符串可能需要几天或几周的时间，并且非常容易出错。对整个代码库运行 sed 需要几分钟的时间，并且会无一例外地替换每个实例。

乍一看，这似乎有点晦涩难懂，但是值得花时间去理解这个命令行工具。想象一下这样一个场景，对计费系统的更改意味着您需要修改代码库中变量名的所有实例。手动在数千个文件中查找和替换该字符串可能需要几天或几周的时间，并且非常容易出错。对整个代码库运行 sed 需要几分钟的时间，并且会无一例外地替换每个实例。

**安装更多命令行工具**

有大量的命令行工具，你现在就可以安装它们来扩展你的计算机的功能。在深入研究命令行工具的几个例子之前，最好先了解您的计算机一般如何管理应用程序，以及如何安装和卸载各种应用程序。

`-i`代表“编辑文件 **i** n-place”。

*   **包管理器**
*   在 Linux 和 macOS 等*NIX 系统上，CLI 的软件主要由[包管理器](https://en.wikipedia.org/wiki/Package_manager)管理。这些 CLI 程序为您提供了数以千计的免费命令行工具，安装起来不费吹灰之力。这些包管理器还处理每个工具所需的所有依赖性管理，这意味着如果需要另一个库或资源，它也会自动安装和配置它们。
*   如果你在 macOS 上，你可以使用[家酿](https://brew.sh/)。如果你是一个 Linux 用户，你可以利用各种各样的包管理器，其中 [APT](https://en.wikipedia.org/wiki/APT_(software)) 和 [yum](https://en.wikipedia.org/wiki/Yum_(software)) 是最受欢迎的 Linux 发行版。下面的例子是在 macOS 上用家酿显示的。

    要用 Homebrew 安装 CLI 程序，请使用以下命令:

    `$ brew install application-name`
    *   `/Line 2/Line X/`告诉 sed 要查找什么以及用什么替换它。
    *   结尾的选项`gi`代表全局，意味着文件中的每一个事件，不区分大小写。
    *   要删除一个带有自制软件的程序，使用下面的:

        `$ brew remove application-name`
*   要删除一个带有自制软件的程序，使用下面的:

    `$ brew remove application-name`

乍一看，这似乎有点晦涩难懂，但是值得花时间去理解这个命令行工具。想象一下这样一个场景，对计费系统的更改意味着您需要修改代码库中变量名的所有实例。手动在数千个文件中查找和替换该字符串可能需要几天或几周的时间，并且非常容易出错。对整个代码库运行 sed 需要几分钟的时间，并且会无一例外地替换每个实例。

**寻找软件**

Homebrew 维护着一个不到 6000 个软件包的列表，所有这些软件包都可以免费下载、安装和使用。APT 没有这样一个集中的列表，但是有多个[开源社区站点维护 APT 包的列表](https://www.fossmint.com/most-used-linux-applications/)。然而，当考虑获得新的应用程序时，查看所有可能的用例的所有可用包是令人难以承受的，并且肯定不是找到新的命令行工具的最佳方法。

对于你能想到的任何命令行工具的用例，世界上很可能有另一个开发者试图围绕它创建另一个工具。在网上快速搜索一下你想做的事情，加上关键词 APT 或 Homebrew，会帮助你缩小搜索范围。

此外，[还有超过 1 . 28 亿个](https://towardsdatascience.com/githubs-path-to-128m-public-repositories-f6f656ab56b1)公共在线知识库，其中许多都有命令行工具，你可以下载并安装。这是对包管理器的补充！但是有时，在某些情况下，您可能只想或需要构建自己的工具。我们将在本系列的另一部分更详细地讨论这个问题。现在，让我们向前看，看看您可以立即安装并在终端上运行的命令行工具的一个小样本！

## 有大量的命令行工具，你现在就可以安装它们来扩展你的计算机的功能。在深入研究命令行工具的几个例子之前，最好先了解您的计算机一般如何管理应用程序，以及如何安装和卸载各种应用程序。

不管你在 Git 中做什么，或者你需要什么命令行工具，GitKraken Client 都有 GitKraken CLI 有用的自动完成建议！

**包管理器**

### 这绝不是一个详尽的列表，而是作为有用工具的一个小样本，让您习惯于安装和使用 CLI 工具。你可以使用`$ brew install program-name`在 macOS 上安装这些程序。

**Calc**

Calc 是一个简单易用但功能强大的计算器。为什么不在你的电脑或手机上打开一个 GUI 计算器呢？首先，Calc 允许您在调用所需的计算时传递它，如果您已经在终端上工作，这将节省时间。

要用 Calc 执行计算，使用以下:

`$ calc first-number operator second-number`

Calc 是一个简单易用但功能强大的计算器。为什么不在你的电脑或手机上打开一个 GUI 计算器呢？首先，Calc 允许您在调用所需的计算时传递它，如果您已经在终端上工作，这将节省时间。

要用 Calc 执行计算，使用以下:

`$ calc first-number operator second-number`

要删除一个带有自制软件的程序，使用下面的:

`$ brew remove application-name`

这个命令行工具非常方便和简单。它适用于所有基本运算，您甚至可以使用`^`字符计算指数幂。Calc 也是完全可定制的。参见`man calc`了解可用选项的全部信息。

Calc 也有一个交互模式，你可以执行多个计算，直到你输入`exit`或`quit`。

要在交互模式下运行 Calc，使用以下命令:

`$ calc`

**树**

[Tree](https://linuxhint.com/bash-tree-command/) 以树状格式列出一个目录及其子目录的内容。这对于快速理解存储库的文件结构非常有用。

要查看一个文件夹的树形视图，使用以下:

`$ tree path/to/folder`

### Homebrew 维护着一个不到 6000 个软件包的列表，所有这些软件包都可以免费下载、安装和使用。APT 没有这样一个集中的列表，但是有多个[开源社区站点维护 APT 包的列表](https://www.fossmint.com/most-used-linux-applications/)。然而，当考虑获得新的应用程序时，查看所有可能的用例的所有可用包是令人难以承受的，并且肯定不是找到新的命令行工具的最佳方法。

对于你能想到的任何命令行工具的用例，世界上很可能有另一个开发者试图围绕它创建另一个工具。在网上快速搜索一下你想做的事情，加上关键词 APT 或 Homebrew，会帮助你缩小搜索范围。

此外，[还有超过 1 . 28 亿个](https://towardsdatascience.com/githubs-path-to-128m-public-repositories-f6f656ab56b1)公共在线知识库，其中许多都有命令行工具，你可以下载并安装。这是对包管理器的补充！但是有时，在某些情况下，您可能只想或需要构建自己的工具。我们将在本系列的另一部分更详细地讨论这个问题。现在，让我们向前看，看看您可以立即安装并在终端上运行的命令行工具的一个小样本！

Homebrew maintains a list of [just under 6,000 packages](https://formulae.brew.sh/formula/), all of which can be downloaded, installed, and used for free. APT does not have such a centralized list, but there are multiple [open source community sites that maintain lists](https://www.fossmint.com/most-used-linux-applications/) of APT packages. However, when considering obtaining new applications, looking at all of the available packages for all possible use cases is overwhelming at best, and is definitely not the best approach to finding new command line tools. 

For just about any use case you can imagine for a command line tool, there is likely another developer in the world who has attempted to create another tool around it. A quick Internet search about the general nature of what you want to do, plus the keyword APT or Homebrew, will help you start narrowing down your search. 

Additionally, there [are over 128 million](https://towardsdatascience.com/githubs-path-to-128m-public-repositories-f6f656ab56b1) public online repositories, many of which have command line tools you can download and install.  This is in addition to the package managers!  But sometimes, there are situations when you will just want to or need to build your own tools.  We will look at that in more detail in another part of this series.  For the time being, let’s move forward and look at a small sampling of command line tools you can install right now to run in your terminal!

**ffmpeg 命令行工具**

ffmpeg 命令行工具是一个非常快的视频和音频转换器。因为没有 GUI 呈现在屏幕上，这个命令行工具可以更快地直接对媒体文件执行操作。

对于您想要调用的格式、大小、速度、每秒帧数以及任何其他视频转换选项，几乎没有任何限制。有很多关于如何最好地开始使用 ffmpeg 的指南，这是一个需要更多空间的主题，我们无法在这里给出。

虽然 ffmpeg 命令行工具有很多使用案例，但其中最常见的是将视频转换为使用 h265 视频编码。这种高质量的格式在不牺牲分辨率的情况下占用了大约十分之一的空间。

要以每秒 28 帧的速度将视频转换为 h265 编码，请使用以下代码:

`$ ffmpeg -i source-video-file -vcodec libx265 -crf 28 uutput-file-name`

### Calc 是一个简单易用但功能强大的计算器。为什么不在你的电脑或手机上打开一个 GUI 计算器呢？首先，Calc 允许您在调用所需的计算时传递它，如果您已经在终端上工作，这将节省时间。

要用 Calc 执行计算，使用以下:

`$ calc first-number operator second-number`

Calc is a calculator that is easy to use but also very powerful.  Why not just open a GUI calculator on your computer or phone? For starters, Calc lets you pass through the desired calculation while you are calling it, which saves time if you’re already working in a terminal.

To perform a calculation with Calc, use the following:

`$ calc first-number operator second-number`

在上面的例子中发生了很多事情，所以如果它看起来让你不知所措，不要惊慌！您对这个工具和所有命令行工具研究得越多，就越容易理解屏幕上显示的内容。

运行该命令后，文件大小从最初的 86MB 缩小到 13MB(见下文)。

**Imagemagick** 的缩写

[Imagemagick](https://imagemagick.org/index.php) 本质上是针对命令行的 Photoshop。与 ffmpeg 一样，这里缺少 GUI 是一项资产，因为消除了将图像绘制到屏幕上的需要意味着它需要的处理能力要少得多。

这是一个功能非常丰富的图像操作命令行工具，可以调整大小，模糊，裁剪，去斑点，翻转，重新采样，等等。与此列表中的其他命令行工具不同，应用程序名称不是您调用应用程序的方式。

### 要调用 Imagemagick，您需要调用`convert`。

要使用 Imagemagick 调整图像大小，请使用以下命令:

`$ convert -resize new-widthxnew-height original-image name-of-new-output-image`

要使用 Imagemagick 调整图像大小，请使用以下命令:

`$ convert -resize new-widthxnew-height original-image name-of-new-output-image`

**ffmpeg 命令行工具**

#### 在上面的例子中，我们试图将一个 2694×382 像素的图像调整为 300×240 像素，这会以不自然的方式拉伸图像。相反，Imagemagick 会自动为我们保存比例，并输出尺寸为 300×43 的图像。通过让 Imagemagick 列出图像信息，我们可以很容易地从它那里获得这些信息。

要使用 Imagemagick 获取图像的宽度和高度，请使用以下代码:

`$ convert image-name -format "%wx%h" info:`

使用 Imagemagick 还可以做更多的事情。对于使用 Imagemagick 的各种[用例](https://opensource.com/article/17/8/imagemagick)，有很多[指南](https://www.digitalocean.com/community/tutorials/workflow-resizing-images-with-imagemagick)。这个命令行工具的一个更常见的用例是调整图像的大小。

**ddgr**

如果你是浏览器中的搜索工具 DuckDuckGo 的粉丝，你可能会喜欢在命令行中使用 [DuckDuckGo。

从终端使用 DuckDuckGo 执行网络搜索，使用以下:

`$` `ddgr searchterm`](https://github.com/jarun/ddgr)

如果你是浏览器中的搜索工具 DuckDuckGo 的粉丝，你可能会喜欢在命令行中使用 [DuckDuckGo。

从终端使用 DuckDuckGo 执行网络搜索，使用以下:

`$` `ddgr searchterm`](https://github.com/jarun/ddgr)

一旦它显示了搜索结果，您可以选择想要在系统的默认浏览器中打开哪个结果。虽然这看起来有点新奇，但是从命令行执行 web 搜索的能力可以帮助您停留在开发流程中，并通过减少上下文切换来保持专注。

如果你以正常方式打开浏览器，有可能会有很多干扰，因为提醒和通知会偷走你的注意力。使用 ddgr，您可以查找文档或示例代码，直接打开浏览器，获得您需要的内容，然后立即返回工作。如果你更喜欢谷歌的搜索结果，那么你可以查看 [`googler`](https://github.com/jarun/googler) 。

WP-CLI

如果你正在管理一个 WordPress 网站，这是你工作流程中一个非常方便的命令工具。 [WordPress 命令行界面](https://wp-cli.org/)让你从终端管理 WordPress。你可以通过管理界面做的任何事情，你都可以通过 WP-CLI 来完成，还有更多。

要运行 WP-CLI，使用下面的:

`$ wp`

**背刺**

### [BackstopJS](https://garris.github.io/BackstopJS/) 是一款针对网页的可视化回归测试命令行工具。本质上，它截取 URL 的截图，并逐个像素地进行比较，看它们是否相同，如果不相同，就抛出一个错误。这最常用于网站开发的测试阶段，以确保准备的内容与开发环境相匹配，在代码最终进入生产环境之前捕捉意外的 CSS 错误。

BacktopJS，不像我们到目前为止看到的所有其他命令行工具，需要您在配置文件中指定设置来使用它。用例越高级，或者工具的概念越复杂，你就越会遇到这种情况。

对于 BackstopJS，您需要在一个配置文件中设置测试 URL、参考 URL、与之比较的网站图像、屏幕分辨率和一些其他因素。有一点学习曲线，但是如果您需要测试您的 web 应用程序部署管道，BackstopJS 可以挽救这一天。

要检查两个 URL 是否包含完全相同的网站图像，请使用以下命令:

`$ backstop test`

**npm**

npm 不是一个命令行工具，更接近于基于 JavaScript 的应用程序的包管理器和编排套件。它包含在这个列表中是为了展示命令行工具的能力范围，npm 可以做很多事情。根据[官方文件](https://docs.npmjs.com/about-npm)，npm 允许您:

为您的应用程序改编代码包，或者按原样合并包。

下载可以立即使用的独立命令行工具。

使用 [npx](https://www.npmjs.com/package/npx) 运行包而不下载。

与任何地方的任何 npm 用户共享代码。

### 将代码限于特定的开发人员。

创建组织来协调包维护、编码和开发人员。

利用组织组建虚拟团队。

管理多个版本的代码和代码依赖项。

当底层代码更新时，轻松更新应用程序。

发现解决同一难题的多种方法。

找到其他正在处理类似问题和项目的开发人员。

### 如果你正在管理一个 WordPress 网站，这是你工作流程中一个非常方便的命令工具。 [WordPress 命令行界面](https://wp-cli.org/)让你从终端管理 WordPress。你可以通过管理界面做的任何事情，你都可以通过 WP-CLI 来完成，还有更多。

要运行 WP-CLI，使用下面的:

`$ wp`

这在这里涉及太多了，但是安装 npm ，或者确保这个命令行工具安装在你的机器上，是第一步。

要检查您正在使用的 npm 的版本，或者只是要判断它是否已安装，请使用以下命令:

`$ npm –version`

**去**

我们列表的最后一个，但在 GitKraken 我们心中的第一个，是 Git，愚蠢的内容跟踪器。这就是官方 Git 手册如何引用这个命令行工具的(见下文)。

### [BackstopJS](https://garris.github.io/BackstopJS/) 是一款针对网页的可视化回归测试命令行工具。本质上，它截取 URL 的截图，并逐个像素地进行比较，看它们是否相同，如果不相同，就抛出一个错误。这最常用于网站开发的测试阶段，以确保准备的内容与开发环境相匹配，在代码最终进入生产环境之前捕捉意外的 CSS 错误。

BacktopJS，不像我们到目前为止看到的所有其他命令行工具，需要您在配置文件中指定设置来使用它。用例越高级，或者工具的概念越复杂，你就越会遇到这种情况。

对于 BackstopJS，您需要在一个配置文件中设置测试 URL、参考 URL、与之比较的网站图像、屏幕分辨率和一些其他因素。有一点学习曲线，但是如果您需要测试您的 web 应用程序部署管道，BackstopJS 可以挽救这一天。

关于在命令行上使用 Git 有很多要说的，你可以在我们的 [learn Git](https://www.gitkraken.com/learn) 库中读到更多。无论你处于什么样的技能水平，GitKraken 网站上的内容都可以帮助你进一步了解 Git 概念，如 [Git rebase](https://www.gitkraken.com/learn/git/problems/git-rebase-branch) 、 [Git push](https://www.gitkraken.com/learn/git/problems/git-push-to-remote-branch) 、 [Git cherry-pick](https://www.gitkraken.com/learn/git/tutorials/how-to-cherry-pick-git) 以及许多其他概念。

GitKraken 客户端通过终端标签提供了一个 CLI，它可以显示命令提示符，让您运行任何 Git 命令，甚至在您工作时提出[自动完成建议](https://support.gitkraken.com/working-with-repositories/terminal/)。使用 GitKraken Client，您可以充分利用 GUI 和 CLI 的优势；当您在 Terminal 选项卡中时，您仍然可以获得非常有用的图形可视化面板来轻松跟踪分支和提交！

如果您还没有尝试过 [GitKraken Client](https://gitkraken.com/download) ，它是免费的，我们相信您会喜欢它的！

`$ backstop test`

**使用命令行工具定制用户体验**

到目前为止，在本系列中，您已经了解了 CLI shell 的历史、一些基本的导航命令、现在可以运行的一些 CLI 程序，以及如何在您的计算机上加载更多的命令行工具。在本系列的下一期中，您将学习如何定制您的用户体验，并使之成为您自己的体验——敬请关注！

**npm**

### npm 不是一个命令行工具，更接近于基于 JavaScript 的应用程序的包管理器和编排套件。它包含在这个列表中是为了展示命令行工具的能力范围，npm 可以做很多事情。根据[官方文件](https://docs.npmjs.com/about-npm)，npm 允许您:

为您的应用程序改编代码包，或者按原样合并包。

*   下载可以立即使用的独立命令行工具。
*   使用 [npx](https://www.npmjs.com/package/npx) 运行包而不下载。
*   与任何地方的任何 npm 用户共享代码。
*   将代码限于特定的开发人员。
*   创建组织来协调包维护、编码和开发人员。
*   利用组织组建虚拟团队。
*   管理多个版本的代码和代码依赖项。
*   当底层代码更新时，轻松更新应用程序。
*   发现解决同一难题的多种方法。
*   找到其他正在处理类似问题和项目的开发人员。
*   Find other developers who are working on similar problems and projects.

这在这里涉及太多了，但是安装 npm ，或者确保这个命令行工具安装在你的机器上，是第一步。

要检查您正在使用的 npm 的版本，或者只是要判断它是否已安装，请使用以下命令:

`$ npm –version`

**去**

我们列表的最后一个，但在 GitKraken 我们心中的第一个，是 Git，愚蠢的内容跟踪器。这就是官方 Git 手册如何引用这个命令行工具的(见下文)。

### **Git**

关于在命令行上使用 Git 有很多要说的，你可以在我们的 [learn Git](https://www.gitkraken.com/learn) 库中读到更多。无论你处于什么样的技能水平，GitKraken 网站上的内容都可以帮助你进一步了解 Git 概念，如 [Git rebase](https://www.gitkraken.com/learn/git/problems/git-rebase-branch) 、 [Git push](https://www.gitkraken.com/learn/git/problems/git-push-to-remote-branch) 、 [Git cherry-pick](https://www.gitkraken.com/learn/git/tutorials/how-to-cherry-pick-git) 以及许多其他概念。

GitKraken 客户端通过终端标签提供了一个 CLI，它可以显示命令提示符，让您运行任何 Git 命令，甚至在您工作时提出[自动完成建议](https://support.gitkraken.com/working-with-repositories/terminal/)。使用 GitKraken Client，您可以充分利用 GUI 和 CLI 的优势；当您在 Terminal 选项卡中时，您仍然可以获得非常有用的图形可视化面板来轻松跟踪分支和提交！

如果您还没有尝试过 [GitKraken Client](https://gitkraken.com/download) ，它是免费的，我们相信您会喜欢它的！

**使用命令行工具定制用户体验**

到目前为止，在本系列中，您已经了解了 CLI shell 的历史、一些基本的导航命令、现在可以运行的一些 CLI 程序，以及如何在您的计算机上加载更多的命令行工具。在本系列的下一期中，您将学习如何定制您的用户体验，并使之成为您自己的体验——敬请关注！

## **Customizing User Experience with Command Line Tools**

So far in this series, you’ve looked at the history of the CLI shell, some basic navigation commands, and now some CLI programs you can run, and how to load more command line tools on your computers. In the next installment of this series, you’re going to learn about customizing your user experience and making it your own – so stay tuned!