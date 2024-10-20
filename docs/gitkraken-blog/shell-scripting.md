# Shell 脚本 CLI 简介第 5 部分

> 原文：<https://www.gitkraken.com/blog/shell-scripting>

在 CLI 介绍系列的前几篇文章中，我们已经讨论了为什么要掌握使用命令行、一些命令行基础知识和工具，以及定制 CLI shell 的技巧。如果您错过了任何内容，请随时使用以下链接进行补充:

[第 1 部分:CLI](https://www.gitkraken.com/blog/cli-stands-for-a-cli-intro-series) [的作用和原因第 2 部分:Shell 命令介绍](https://www.gitkraken.com/blog/shell-commands) [第 3 部分:命令行工具](https://www.gitkraken.com/blog/command-line-tools) [第 4 部分:定制您的 CLI Shell](https://www.gitkraken.com/blog/cli-shell)

现在，您将学习 shell 脚本的基础知识，以自动化复杂的工作并构建全新的应用程序！

如果您足够高级，能够编写 shell 脚本，您可能不认为您需要 Git 客户机。虽然这可能是真的，但有了它，生活会变得更好，尤其是有内置终端的手机。🤯

## Shell 脚本–自动化和构建新应用

当你看一部电影或一个流媒体系列时，你真正看到的是一个演员和工作人员在执行一个剧本。该脚本从上到下从左到右阅读，直到他们到达结尾。类似地，计算机已经变得非常擅长阅读脚本并完全按照它们说的去做。

这些脚本可以很长，做的事情比如通过管理 Docker 提供本地开发环境，就像 [Docksal](https://docksal.io/) 的情况一样，这些脚本也可以很简单，比如“ [hello world](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program) ”。

在本文中，我们将讨论一些与 shell 脚本相关的主题以及一些您可以运行的特定 shell 脚本。

Hello World 脚本

## 要编写一个 hello world 脚本，完成以下步骤:

1 .在文本编辑器中打开一个名为`hello-world.sh`的新文件。

2.在新文件中键入以下内容:

````

`#!/bin/bash
# This script prints Hello World to the screen
echo "Hello World"`

````

3.保存文件。

4。在与`hello-world.sh`相同的目录中，从终端运行以下命令:

`$ chmod 755 hello-world.sh:`

5.接下来，运行命令:

`$ ./hello-world.sh`

6.你现在应该看到`Hello World`打印在屏幕上。

6.你现在应该看到`Hello World`打印在屏幕上。

恭喜你！您刚刚从头开始编写并执行了您的第一个 shell 脚本！这是成为一名经验丰富的开发人员的一大步！

尽管 hello world 脚本很简单，但这里有很多东西需要解开。真正令人惊讶的消息是，所有的 shell 脚本本质上都以相同的方式工作，因此理解`hello-world.sh`和 CLI shell 基础意味着您将能够阅读几乎任何脚本。

尽管 hello world 脚本很简单，但这里有很多东西需要解开。真正令人惊讶的消息是，所有的 shell 脚本本质上都以相同的方式工作，因此理解`hello-world.sh`和 CLI shell 基础意味着您将能够阅读几乎任何脚本。

用 Shebang 调用 Shell 解释器

### 让我们从`hello-world.sh` :

`#!/bin/bash`

的第一行开始，那第一部分，`#!`被称为[射棒](https://en.wikipedia.org/wiki/Shebang_(Unix))。shebang 告诉操作系统接下来应该调用的程序是读取和执行文件的其余内容。这个程序被称为脚本解释器。在这种情况下，它调用位于`/bin`文件夹中的`bash`。`bin`代表二进制。

你的`/bin`文件夹里有很多文件。您可以通过运行以下命令来查看计算机上该文件夹的所有内容:

`$ ls /bin`

在您的`/bin`文件夹中，除了`bash`和`zsh`之外，您可能还会识别出一些您的操作系统可以执行的文件，比如`date`、`cp`、`ls`和`mv`等等。这是这些命令的可执行二进制文件在系统中的位置。

当您运行以`#!/bin/bash`开头的 shell 脚本时，您实际上是在告诉计算机打开一个非交互式、非登录的 shell 来运行文件中的命令。

即使它是非交互式的，计算机仍然可以访问您执行 shell 脚本的终端，并且能够接受输入和打印运行它的会话的输出。这可能有点令人困惑，所以如果你想深入了解，这里有一个关于非交互式 shell 和 shell 脚本的更加详细的解释。出于本文的目的，我们需要知道的是，在第一行之后，一切都只是 CLI shell 命令和一些 Bash 脚本语法。

Bash 脚本与 Zsh 脚本

您可能会问自己，为什么对 shell 使用 Zsh，对 shell 脚本语言使用 Bash。非常简单的答案是，Bash 更像是一个脚本工具，所以编写 Bash 脚本意味着您的代码将在更多的机器上工作。

无论你是在 Fedora、Ubuntu、macOS、FreeBSD 还是任何其他*NIX 操作系统上，hello world 脚本都会以同样的方式运行。您可以放心地在几乎任何*nix 服务器或您可以访问的远程系统上使用 Bash 脚本。这是一种将命令行工具捆绑在一起的通用语言。Zsh 是一个令人惊叹的交互式 CLI shell，但它并没有被普遍采用。

即使它是非交互式的，计算机仍然可以访问您执行 shell 脚本的终端，并且能够接受输入和打印运行它的会话的输出。这可能有点令人困惑，所以如果你想深入了解，这里有一个关于非交互式 shell 和 shell 脚本的更加详细的解释。出于本文的目的，我们需要知道的是，在第一行之后，一切都只是 CLI shell 命令和一些 Bash 脚本语法。

GitKraken 客户端配备了 GUI 和 CLI，为所有操作系统的高级开发人员提供了最佳体验。⬇️自己试试

## 其他 Shell 脚本选项

如果您已经精通另一种语言，如 Perl、Python、Ruby，甚至 PHP，您完全可以使用它们来解释和运行您的 shell 脚本。您可以自由地告诉您的计算机使用任何已安装的程序来尝试评估文件的内容。

举个例子，用 Python3 评估一个文件，你需要做的就是在你的 Python 脚本顶端加上这一行:

`#!/usr/bin/python3`

你的 hello world 脚本的第二行是:

`# This script prints Hello World to the screen`

在 Bash 脚本中，`#`字符表示同一行中该符号之后的所有内容都是 shell 脚本注释，在执行脚本时应该忽略。您会注意到，当您运行`hello-world.sh`时，shell 脚本注释不会打印到屏幕上。

## 代码中添加的 Shell 脚本注释被称为*行内注释*。它们对于解释一段代码是如何工作的或者预期的结果是什么非常有帮助。Shell 脚本注释是一种方便的方式，可以向以后将处理相同代码的任何其他开发人员解释您的工作。

外壳脚本回显

hello world 脚本的第三行也是最后一行是:

`echo "Hello World"`

shell 脚本 echo 告诉计算机将随后出现的内容打印到标准输出，默认情况下是终端屏幕。echo 命令也可以在 shell 脚本之外工作。如果您现在尝试将该行脚本作为命令粘贴到您的终端中，您应该会看到:

重定向外壳输出

默认情况下，命令执行的输出会打印到终端屏幕上。这就是“标准输出”。在 CLI 系列介绍的第 3 部分[中，我们回顾了重定向相同的输出，特别是在使用管道操作符`|`时。](https://www.gitkraken.com/blog/command-line-tools)

例如，如果您运行`$ ls -a | grep .git`，那么`ls -a`命令的输出将被传送到`grep`的输入中，并且只搜索该输入或匹配模式`.git`。

外壳脚本回显

## 重定向输出的另一种方法是使用重定向操作符`>`和`>>`。这些运算符最常用于将命令的输出传输到文件中。
T5`>`覆盖任何文件内容。例如，如果您运行:

`$ echo "Hello" > hello.txt`

…您将得到一个新文件，其中只有“Hello”一行。

然后，如果您要运行以下命令:

`$ echo "World" > hello.txt`

….该文件将包含单行“World ”,覆盖了该文件以前的内容。小心这个操作员！

### 默认情况下，命令执行的输出会打印到终端屏幕上。这就是“标准输出”。在 CLI 系列介绍的第 3 部分[中，我们回顾了重定向相同的输出，特别是在使用管道操作符`|`时。](https://www.gitkraken.com/blog/command-line-tools)

例如，如果您运行`$ ls -a | grep .git`，那么`ls -a`命令的输出将被传送到`grep`的输入中，并且只搜索该输入或匹配模式`.git`。

`>>`操作符将命令的输出附加到现有内容下的文件中。以

为例，继续上一个例子，如果你要运行:

`$ echo 'Hello World' >> hello.txt`

…当您查看内容时，应该会看到两行:第一行是“World”，第二行是“Hello World”。

重定向输出的另一种方法是使用重定向操作符`>`和`>>`。这些运算符最常用于将命令的输出传输到文件中。
T5`>`覆盖任何文件内容。例如，如果您运行:

`$ echo "Hello" > hello.txt`

…您将得到一个新文件，其中只有“Hello”一行。

然后，如果您要运行以下命令:

重定向操作符的一个非常常见的用途是为应用程序创建和更新日志文件。当您开始调试自己的应用程序时，特别是当它们的规模和复杂性增加时，将脚本输出重定向到一个您可以搜索和查看的文件中是非常有帮助的。

sh 文件扩展名

既然您已经查看了 hello world 脚本示例的内部，那么让我们后退一步，查看文件本身以及它是如何执行的。

文件扩展名`.sh`代表“shell 脚本”,实际上只是为了便于阅读。对于 shell 来说，sh 文件扩展名只是一个文本文件。因为操作系统从第一行开始读取以确定如何解释文件内容，所以文件的实际名称对机器来说并不重要，但是当您使用文件扩展名时，将文件的性质传达给其他开发人员甚至您自己会容易得多。

让我们通过将`hello-world.sh`重命名为`hello-world`来快速测试一下，并尝试以与之前相同的方式再次运行它。

…当您查看内容时，应该会看到两行:第一行是“World”，第二行是“Hello World”。

…when you look at the contents, you should see two lines: the first being “World”, and the second being “Hello World”.

运行外壳脚本的权限

创建并保存`hello-world.sh`后的下一步是运行这个看起来很奇怪的命令:

`chmod 755 hello-world.sh`

sh 文件扩展名

## 了解 Shell 中的文件系统权限

文件系统中的每个文件都有权限设置，也称为模式，允许三类不同的潜在用户读取、写入或执行文件。潜在用户的类别有:

1.用户-文件所有者
2。group–一组指定的用户，他们在机器
3 上共享相同的文件权限。其他——上述两个类别之外的任何人

1.用户-文件所有者
2。group–一组指定的用户，他们在机器
3 上共享相同的文件权限。其他——上述两个类别之外的任何人

`ls`命令允许您查看文件和目录的详细信息。`ls -l`实际上向我们展示了谁拥有什么权限。

运行外壳脚本的权限

## 创建并保存`hello-world.sh`后的下一步是运行这个看起来很奇怪的命令:

与 hello world 脚本相关联的行是:

`-rwxr-xr-x  1 dwaynemcdaniel  staff   79 Feb  4 09:46 hello-world`

让我们快速分解一下上面所说的内容。

第一列:`-rwxr-xr-x`说明你的文件权限。第一个字符告诉你这是一个文件`-`还是一个目录`d`。

接下来的三个字符:`rwx`告诉你文件所有者可以读、写、执行文件。

中间三个字符:`r-x`表示一个组中指定的任意用户的权限。在这种情况下，组中的任何人都可以读取或执行该文件，但不能写入。组起源于 Unix 系统，它涉及所有用户共享同一个物理机器。

第一列中的最后三个字符:`r-x`表示任何其他用户或代理的权限，比如 Bash 同样，在这种情况下，他们可以读取或执行文件，但不能写入文件。这一行的其余部分告诉您:

#### –第 2 列:从系统的其他地方到该文件有多少个链接
–第 3 列:文件所有者的名称
–第 4 列:哪个组拥有该文件
–第 5 列:以[字节](https://simple.wikipedia.org/wiki/Byte)为单位的文件大小
–第 6 列:文件的最后修改日期
–第 7 列:文件或目录名

1.用户-文件所有者
2。group–一组指定的用户，他们在机器
3 上共享相同的文件权限。其他——上述两个类别之外的任何人

更改文件访问以运行 Shell 脚本

`chmod`让我们重新分配每个文件夹或目录的权限，影响读取、写入或执行(可能时)文件或文件夹的能力。

运行`hello-world.sh`的指令中使用的`755`告诉计算机为用户、组和其他人设置权限，遵循一个非常简洁的数学符号。

可能的文件权限有:

–0 =无权限
–1 =仅执行
–2 =仅写入
–3 =写入并执行(1+2 = 3)
–4 =只读
–5 =读取并执行(1+4 = 5)
–6 =读取并写入(2+4 = 6)
–7 =读取并执行(1 + 2 + 4 = 7)

与 hello world 脚本相关联的行是:

[用`chmod`](https://en.wikipedia.org/wiki/Chmod) 设置文件访问模式还有其他多种方式，但是编号系统容易记忆，非常方便。`755`通常被认为是本地运行的安全选择，因为它为 Bash 和任何其他用户提供了一种读取和执行 shell 脚本的方法，而无需修改它正在读取的文件。

第一列中的最后三个字符:`r-x`表示任何其他用户或代理的权限，比如 Bash 同样，在这种情况下，他们可以读取或执行文件，但不能写入文件。这一行的其余部分告诉您:

评估文件以运行 Shell 脚本

运行`$ ./hello-world.sh`实际上是执行 shell 脚本。第一部分:`./`，告诉操作系统评估路径末尾的文件。

`.`指的是 shell 当前工作的目录。当操作系统评估一个文件时，它读取第一行并寻找 shebang，`#!`，以便打开一个新的非交互式 shell 来执行它在下面指定路径的末尾找到的任何命令。在这种情况下，`/bin/bash`，这一点我们已经在上面介绍过了。

## 你可能想知道为什么不能像调用其他命令如`ls`、`mv`或`bash`一样直接调用命令`hello-world.sh`。嗯，您可以通过将您的 shell 脚本添加到`$PATH`的一个文件夹中来实现这一点。

路径环境变量

path 环境变量或`$PATH`很重要，因为 shell 使用它来跟踪包含可执行文件的目录列表。当您键入类似于`ls`的命令时，shell 会在`$PATH`中列出的默认文件夹列表中进行搜索，直到找到一个名为`ls`的可执行文件，然后执行该文件。这是您的 CLI shell 运行所有命令的方式，除了您告诉它特别评估的文件，就像您对脚本示例所做的那样。

要查看当前存储在您计算机上的`$PATH`中的内容，您可以运行以下命令:

`echo $PATH`

如果您使用的是 macOS，默认情况下，您的路径可能会是这样的:

`/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin`

## $PATH 存储为冒号分隔的列表，这意味着每个条目由一个`:`分隔。这些是操作系统检查的默认目录:

–/usr/local/bin
–/usr/bin
–/bin
–/usr/sbin
–/sbin

你可能想知道为什么不能像调用其他命令如`ls`、`mv`或`bash`一样直接调用命令`hello-world.sh`。嗯，您可以通过将您的 shell 脚本添加到`$PATH`的一个文件夹中来实现这一点。

花点时间看看每个文件夹里都有什么。了解哪些命令是可用的总是一个好主意，即使您不经常运行一些命令。

路径环境变量

## 终端的环境变量

我们刚刚讨论了 path 环境变量(或`$PATH`)。作为一个环境变量意味着 shell 脚本存储了一个值，该值可以被整个操作系统环境中的任何进程访问。

`$Path`只是众多环境变量中的一个。例如，另一个是`$HOME`，尽管在本系列中我们通过它的自动特殊别名(代字号`~`)来介绍它，作为导航文件系统的一部分。

如果你在你的 CLI shell 中运行`$ echo $HOME`，你会看到它指向什么目录。最好不要修改`$HOME`。另一方面，T5 经常被修改。

要向`$PATH`添加一个新目录，您需要向您的`.zshrc`文件夹中添加一行，如下所示:

`export PATH=path/to/directory:$PATH`

`export`命令使环境变量对外壳和外壳产生的任何进程可用。该命令的其余部分将`$PATH`设置为您新指定的目录加上$PATH 的现有内容。

您安装的一些程序会让您添加一行代码，将路径导出到您的`.zshrc`，这又会在每次加载 shell 时更新`$PATH`。例如，像 PHP、Python3、Mamp、NodeJS 和 npm 这样的工具会自动添加或者要求您添加特定的行到您的`.zshrc`文件中，作为它们安装过程的一部分。

$PATH 存储为冒号分隔的列表，这意味着每个条目由一个`:`分隔。这些是操作系统检查的默认目录:

从文件系统中的任何地方运行 Shell 脚本

为了将`hello-world.sh`作为命令执行，无论你的 PWD 是什么，你有三种选择:

1.将文件的目录添加到`$PATH`
2。将文件移动到已经在`$PATH`
3 中的系统目录中。结合其他选项，新建一个文件夹，把可执行文件移到那里，添加到`$PATH`

1.将文件的目录添加到`$PATH`
2。将文件移动到已经在`$PATH`
3 中的系统目录中。结合其他选项，新建一个文件夹，把可执行文件移到那里，添加到`$PATH`

所有这三种方法产生了相同的最终结果:你将能够从任何地方调用`hello-world.sh`。

## 因为经常重用一个 shell 脚本将需要处理`$PATH`，所以许多开发人员选择在这一点上放弃`.sh`，就像我们在前面的例子中所做的那样。

以下是将脚本重命名为`hello-world`后，任何选项的结果:

外壳脚本逻辑

现在，您已经创建了一个简单的 shell 脚本，更改了权限，并执行了 shell 脚本。下一步是理解如何向脚本添加编程逻辑。简单的 shell 脚本只是执行一个又一个命令，并在完成后停止。然而，一个好的 CLI shell 脚本的真正威力来自于这样一个事实，即您可以添加逻辑，以便只在某些情况下多次运行命令，甚至等待用户输入继续运行。

虽然这不是一个详尽的列表，也不意味着是一个完整的教程，但让我们快速介绍一些最常见和最有用的脚本逻辑操作。

从文件系统中的任何地方运行 Shell 脚本

## 用一个工具来执行命令，这个工具为常见的动作提供了自动建议和自动完成…这才是逻辑。

外壳脚本变量

要创建一个变量(即保存赋值的已定义实体)，只需将其声明为等于一个值:

`VARIABLE1=1`

要在 shell 脚本中使用一个变量，您需要在前面添加一个`$`来标记它为一个在命令执行中读取的变量。

`echo $VARIABLE1`

虽然不是必需的，但是 shell 脚本变量的一个常见的最佳实践是用大写字母命名它们。一些开发人员认为这使得阅读脚本和理解正在发生的事情变得更加容易。

虽然不是必需的，但是 shell 脚本变量的一个常见的最佳实践是用大写字母命名它们。一些开发人员认为这使得阅读脚本和理解正在发生的事情变得更加容易。

Shell 脚本中的 If Else 语句

## shell 脚本中的 if/else 语句使用以下过程:

测试一个条件，看它是否为真，如果为真，执行某些命令，否则，执行其他操作:

```
if [[ true ]]; then
command
else
command
fi
```

Shell 脚本 While 循环

shell 脚本中的 while 循环使用以下过程:

测试某个条件是否为真，如果为真，则重新执行命令，否则，停止并转到脚本的下一部分:

## ```
while [[ true ]]; do
command

done

```

循环的外壳脚本

shell 脚本中的 for 循环使用以下过程:

评估变量或文件，并重复运行某些命令，直到满足定义的条件或到达列表的末尾。

为 loop 编写 shell 脚本最常见的两种语法是:

1。定义变量`i`，每次执行列表时，将`i`的值增加 1。如果`i`等于或大于 10 则停止:

```
for (( i=0; i<10; i++)
do
    Commands
done
```

2。对列表中的每个值运行一次命令，当没有值时停止。您可以引用命令中的值:

```
for value in list
do
    commands
done
```

虽然不是必需的，但是 shell 脚本变量的一个常见的最佳实践是用大写字母命名它们。一些开发人员认为这使得阅读脚本和理解正在发生的事情变得更加容易。

接收用户输入的 Shell 脚本

接受用户输入的 shell 脚本的工作方式如下:

## `read`命令将在继续之前等待 shell 用户的输入，并将输入保存到一个变量中。

`read VARIABLE`

高级 Shell 脚本示例

尽管 hello world 脚本示例相当不错，但让我们来看看一个更复杂的高级 shell 脚本示例，它采用了我们刚刚介绍的逻辑。每一步都将通过行内注释来解释。虽然您可以复制/粘贴整个示例，但我们鼓励您实际输入它以获得一些肌肉记忆。

```
#!/bin/bash
# Clear the screen when this script runs
clear `

## `# Ask for the user name and await user input, store the input in NAME
echo "What is your first name" && read NAME`

`# Print NAME to screen in a greeting
echo "Hi $NAME"
echo ""`

`#Create a list of things to like or dislike
LIKELIST='dogs cats movies books'`

`# Loop through each value on the list that just was created
for LIKE in $LIKELIST
do
    # Ask the user if they like the current value from the list and store the answer in LIKEANSWER
    echo "Do you like $LIKE? (Answer only y or n)" && read LIKEANSWER`

`# If they reply with a lower case 'y', print a positive reply
    if [[ $LIKEANSWER == 'y' ]]; then
        echo "Awesome! I like $LIKE too!"
        echo ""     
        # Echoing "" prints a new line to the screen.`



## `echo "That's OK. I understand not everyone is a $LIKE person "
        echo ""`

`fi`

`done`

`# Tell the user thank you
echo "Thanks for running this script!"
echo ""```

## 在保存文件、运行`chmod 755`并执行脚本之后，您应该会看到如下内容:

`read`命令将在继续之前等待 shell 用户的输入，并将输入保存到一个变量中。

`read VARIABLE`

上面的例子使用了本系列中没有介绍的一些语法和命令，旨在鼓励您更多地探索 shell 脚本。有许多很棒的指南和大量的文档可用——只需在网上搜索一下！

Git 挂钩是 Shell 脚本

## 在您的存储库的`.git`文件夹中有一个名为`hooks`的目录。默认情况下，该文件夹包含 13 个文件名为`commit-msg.sample`、`pre-push.sample`、`pre-rebase.sample`的文件。

```
#!/bin/bash
# Clear the screen when this script runs
clear `

`# Ask for the user name and await user input, store the input in NAME
echo "What is your first name" && read NAME`

所有这些文件都是对应于 Git 工作流中特定事件的 shell 脚本。要允许 Git 在相关触发发生时运行这些 shell 脚本，只需从文件名中删除`.sample`。Git 专门寻找没有扩展名的文件。

这些文件中有各种各样的例子来帮助你更好地利用 Git，你可以编写自己的 shell 脚本来自动化任何事情。

为了好玩，试着在`commit-msg`钩子上添加下面一行:

`curl -s [https://icanhazdadjoke.com](https://icanhazdadjoke.com) && echo ""`

`#Create a list of things to like or dislike
LIKELIST='dogs cats movies books'`

`# Loop through each value on the list that just was created
for LIKE in $LIKELIST
do
    # Ask the user if they like the current value from the list and store the answer in LIKEANSWER
    echo "Do you like $LIKE? (Answer only y or n)" && read LIKEANSWER`

虽然可能不是最专业的例子，但您现在知道如何向 Git 挂钩添加逻辑了！爱德华·汤普森从他的工具 git-dad 中获得的灵感[归功于](https://github.com/ethomson/git-dad)[。](https://www.gitkraken.com/gitkon/history-of-git)

`echo "That's OK. I understand not everyone is a $LIKE person "
        echo ""`

GitKraken 客户端支持各种各样的钩子操作，比如提交、重置、推送等等，使得设置 Git 钩子变得轻而易举，甚至对于脚本初学者也是如此。

`done`

CI/CD 管道依赖于 Shell 脚本

作为现代开发人员，您需要 shell 脚本技能的一个更实际的原因是因为它们在 CI/CD 管道中的使用。从简化的角度来看，CI/CD 服务只是在远程服务器上运行 shell 脚本来引发操作。当然，还有许多附加功能，如内置于 CI/CD 平台的秘密管理和应用程序监控，如 [CircleCI](https://circleci.com/) 、 [GitHub Actions](https://github.blog/2022-02-02-build-ci-cd-pipeline-github-actions-four-steps/) 和 [GitLab](https://docs.gitlab.com/ee/ci/) ，但在本质上，它们只是执行脚本。

例如，下面是 GitLab 的 [Bash 工作流模板的一部分:](https://gitlab.com/gitlab-org/gitlab/-/blob/master/lib/gitlab/ci/templates/Bash.gitlab-ci.yml)

```` 
`before_script:
  - echo "Before script section"`
`- echo "For example you might run an update here or install a build dependency"
  - echo "Or perhaps you might print out some debugging details"after_script:
  - echo "After script section"`

虽然您可能没有使用 Bash 作为实际的 shell 脚本语言，但是一旦您了解了一种脚本语言的工作原理，您就基本上理解了所有脚本语言的工作原理，尽管语法会有所不同。

每一个 CI/CD 平台的细节都需要花时间去深入学习，但是拥有基本的 shell 脚本知识意味着您可以走得更远、更快。

每一个 CI/CD 平台的细节都需要花时间去深入学习，但是拥有基本的 shell 脚本知识意味着您可以走得更远、更快。

考虑为开源做贡献

## 在我们结束 CLI 系列介绍之前，我想最后鼓励大家分享您的工作，并考虑为开源做出贡献。本系列中的所有内容都来自于研究开源工具的文档和阅读许多代码示例。

所有的开发人员都站在巨人的肩膀上，大多数时候不知道你的共享代码的哪一部分可能会对其他人有所帮助。

但是，注意不要在公共存储库中共享密码和配置信息，但是如果您编写了一个简洁的脚本来解决特定的问题，可以考虑在公共存储库中共享它，并在自述文件中写下它是如何帮助您的。

Shell 脚本资源

有许多资源可以用来学习更多关于 shell 脚本的知识。以下是四点建议:

[Codecademy 的 Bash/Shell 课程](https://www.codecademy.com/catalog/language/bash)

[巴什学院](https://guide.bash.academy/)

[Ryans 教程–Bash 脚本教程](https://ryanstutorials.net/bash-scripting-tutorial/)

[LinuxHint 的 30 个 Bash 脚本示例](https://linuxhint.com/30_bash_script_examples/)

[LinuxHint 的 30 个 Bash 脚本示例](https://linuxhint.com/30_bash_script_examples/)

已经踏上探索和学习之旅的开发人员社区也是一个惊人的资源；我们都曾经是个彻头彻尾的菜鸟。永远不要害怕寻求帮助。

## 感谢您了解 CLI 的含义。无论你是谁，如果你使用计算机，学习使用命令行界面是变革性的。没什么好害怕的，学习这些工具的好处远远超过了成本。GitKraken 的一套 [Git 工具](https://www.gitkraken.com/)旨在帮助开发人员加速学习之旅。 [GitKraken CLI](https://www.gitkraken.com/cli) 的自动完成建议将帮助你浏览文件系统，并像专业人士一样使用 Git。GitLens 将帮助你更好地与其他开发者合作，让每一行的作者和提交历史更加清晰。我们邀请您下载并安装这两者，以便在您的旅程中充分利用 CLI shell！

```` 
`before_script:
  - echo "Before script section"`
`- echo "For example you might run an update here or install a build dependency"
  - echo "Or perhaps you might print out some debugging details"after_script:
  - echo "After script section"`

虽然您可能没有使用 Bash 作为实际的 shell 脚本语言，但是一旦您了解了一种脚本语言的工作原理，您就基本上理解了所有脚本语言的工作原理，尽管语法会有所不同。

每一个 CI/CD 平台的细节都需要花时间去深入学习，但是拥有基本的 shell 脚本知识意味着您可以走得更远、更快。

The ins and outs of every CI/CD platform takes time to learn deeply, but having basic shell scripting knowledge means you can get a lot further, a lot faster.  

考虑为开源做贡献

## 在我们结束 CLI 系列介绍之前，我想最后鼓励大家分享您的工作，并考虑为开源做出贡献。本系列中的所有内容都来自于研究开源工具的文档和阅读许多代码示例。

所有的开发人员都站在巨人的肩膀上，大多数时候不知道你的共享代码的哪一部分可能会对其他人有所帮助。

但是，注意不要在公共存储库中共享密码和配置信息，但是如果您编写了一个简洁的脚本来解决特定的问题，可以考虑在公共存储库中共享它，并在自述文件中写下它是如何帮助您的。

Shell 脚本资源

有许多资源可以用来学习更多关于 shell 脚本的知识。以下是四点建议:

## [Codecademy 的 Bash/Shell 课程](https://www.codecademy.com/catalog/language/bash)

[巴什学院](https://guide.bash.academy/)

1.  [Ryans 教程–Bash 脚本教程](https://ryanstutorials.net/bash-scripting-tutorial/)
2.  [LinuxHint 的 30 个 Bash 脚本示例](https://linuxhint.com/30_bash_script_examples/)
3.  [Ryans Tutorials – Bash Scripting Tutorial](https://ryanstutorials.net/bash-scripting-tutorial/)
4.  已经踏上探索和学习之旅的开发人员社区也是一个惊人的资源；我们都曾经是个彻头彻尾的菜鸟。永远不要害怕寻求帮助。

感谢您了解 CLI 的含义。无论你是谁，如果你使用计算机，学习使用命令行界面是变革性的。没什么好害怕的，学习这些工具的好处远远超过了成本。GitKraken 的一套 [Git 工具](https://www.gitkraken.com/)旨在帮助开发人员加速学习之旅。 [GitKraken CLI](https://www.gitkraken.com/cli) 的自动完成建议将帮助你浏览文件系统，并像专业人士一样使用 Git。GitLens 将帮助你更好地与其他开发者合作，让每一行的作者和提交历史更加清晰。我们邀请您下载并安装这两者，以便在您的旅程中充分利用 CLI shell！

The community of developers who have been on this journey of discovery and learning is an amazing resource as well; we’ve all been a complete noob at one point.  Never be afraid to ask for help. 

Thank you for learning what the CLI stands for. No matter who you are, if you use a computer, learning to use the CLI shell is transformative. There is nothing to fear and the benefits of learning these tools far and away outweigh the costs.

GitKraken’s set of [Git tools](https://www.gitkraken.com/) are designed to help speed developers on your learning journey.  The [GitKraken CLI](https://www.gitkraken.com/cli)‘s autocomplete suggestions will help you navigate the file system and use Git like a pro in no time. [GitLens](https://www.gitkraken.com/gitlens) will help you collaborate better with other developers by making each line’s authorship and commit history more clear.  We invite you to download and install both to make the best use of the CLI shell on your journey!