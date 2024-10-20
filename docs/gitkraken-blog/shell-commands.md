# Shell 命令 CLI 简介第 2 部分

> 原文：<https://www.gitkraken.com/blog/shell-commands>

在 CLI 介绍系列的第 1 部分:[CLI 代表什么？](https://www.gitkraken.com/blog/cli-stands-for-a-cli-intro-series)，我们概述了您为什么想要使用 CLI，这样做的一些好处，以及一些 CLI 历史和术语。在第 2 部分 Shell 命令介绍中，准备好卷起袖子深入了解终端。

在我们继续之前，花点时间庆祝一下你自己。🎉很多人都不敢冒险走到这一步。你正在学习一些令人敬畏的技能，我的朋友，这是值得承认的。虽然在概念变得清晰之前，您可能需要多次运行我们将要探索的一些命令，但要知道这是完全正常的，事实上大多数刚接触 Git CLI 的人都会经历同样的经历。请相信您很快就会成为 CLI 专家，因为这是事实！

在 shell 命令系列的这一部分中，我们将讨论:

1.[壳牌基本面](#Shell-Fundamentals)
2。[用 Shell 命令](#moving-around)在文件系统中移动
3。[用 Shell 命令与文件和文件系统交互](#shell-commands)
4。[阅读手册](#read-the-manual)
5。[便捷的 Shell 命令列表](#handy-commands)

让我们开始吧。

从命令行学习 Git 基础知识？GitKraken 客户端为文件路径和常用命令提供了有用的自动完成建议。

从命令行学习 Git 基础知识？GitKraken 客户端为文件路径和常用命令提供了有用的自动完成建议。

**外壳命令基础**

## **外壳命令基础**

**贝壳以为我是谁？**

### 当与命令行 shell 交互时，定义“谁”正在与 shell 交互是很重要的。对您来说，显然**您**正在与 shell 交互，但是每个系统都有用户角色和 root 或管理员角色的概念。这些不同的角色有不同的权限，每个角色对系统的影响也不同。你作为用户所做的任何事情都将被限制在你的帐户之内，而作为根用户可以影响整个机器和所有帐户。小心 root 访问。

要看电脑认为你是谁，用:

`$ whoami`

当与命令行 shell 交互时，定义“谁”正在与 shell 交互是很重要的。对您来说，显然**您**正在与 shell 交互，但是每个系统都有用户角色和 root 或管理员角色的概念。这些不同的角色有不同的权限，每个角色对系统的影响也不同。你作为用户所做的任何事情都将被限制在你的帐户之内，而作为根用户可以影响整个机器和所有帐户。小心 root 访问。

要看电脑认为你是谁，用:

`$ whoami`

在这里，我们可以看到询问`whoami`时返回的用户名`dwaynemcdaniel`。这在以后查看权限和安装程序时变得很重要。现在，只要确保你用你的用户名而不是`root`登录。

在这里，我们可以看到询问`whoami`时返回的用户名`dwaynemcdaniel`。这在以后查看权限和安装程序时变得很重要。现在，只要确保你用你的用户名而不是`root`登录。

**导航您的文件系统**

### 在命令行环境中，相对于您正在工作的文件夹执行操作。知道你在操作系统文件夹结构中的位置是非常重要的。你工作的地方被称为你的“当前工作目录”

使用 CLI 查看你所在操作系统的文件结构，使用以下命令:

`$ pwd`

`pwd`命令使用格式`/User/username/foldername`返回当前工作目录的绝对或完整路径。这个完整路径可以从左到右读:`/`的根目录位置；接下来是`Users`目录，它跟踪系统上的所有用户；其次是用户的配置文件目录；最后，路径的其余部分是指定用户文件系统中的文件夹。知道完整路径以及如何访问它对移动或复制文件、安装程序或[克隆库](https://www.gitkraken.com/learn/git/git-clone)非常有帮助。

**关于提示的说明**

此时，您可能对上面示例中出现在`pwd`之前的文本感到疑惑。这些信息被称为 shell 的“命令提示符变量”默认情况下，它们会告诉您谁登录了以及机器的名称。

`%`只是指示你应该从哪里开始输入。命令的例子通常以一个`$`或一个`%`开始，告诉你实际的命令从哪里开始。在本文中，我们将使用`$`。您的提示可能与这些示例中的有所不同，这是意料之中的。您可以完全控制在该提示之前打印的内容。在本系列的后面，我们将研究定制，包括修改那些命令提示符变量和创建您自己的快捷方式。现在，只要知道不管您的命令提示符是什么样子，本文中的示例命令都是在`$`之后开始的。

#### **使用 CLI Shell 命令列出目录的内容**

知道你在哪里是一个好的开始，但是你需要一种方法来查看当前工作目录中有什么。

要列出当前工作目录的内容，使用以下 shell 命令:

`$ ls`

### 这相当于用 Mac 的 Finder 或 Window 的文件浏览器打开文件夹。它只显示可见文件夹的名称。文件夹或目录通常在其名称和文件末尾没有文件扩展名，但并非总是如此。在这个例子中，您可以看到`docs`、`build`和`overrides`没有文件扩展名，这意味着您假设它们都是目录是正确的。还可以看到`README.md`和`mkdocs.yml`都有文件扩展名，确实都是文件。

知道你在哪里是一个好的开始，但是你需要一种方法来查看当前工作目录中有什么。

要列出当前工作目录的内容，使用以下 shell 命令:

**使用 Shell 命令查看所有文件夹内容**

要查看文件夹的所有内容，包括隐藏的文件夹和文件，请使用以下 shell 命令:

`$ ls -a`

使用选项`-a`意味着“列出所有目录内容，包括隐藏的文件夹和文件。”隐藏的文件夹和文件名称以句号`.`开头。例如，一个隐藏文件夹是`.git`文件夹，一个隐藏文件是`.gitignore`。`-a`选项显示所有内容，包括隐藏的文件和文件夹。

#### **特殊隐藏字符**

项目`.`和`..`实际上是非常方便的特殊字符，它们总是出现在文件系统的每个目录中。读作“点”的`.`表示“当前目录”，而`..`或“点点”表示“上一级目录”

`$ ls -a`

**更详细地查看所有文件夹内容**

关于如何通过使用选项来显示`ls`命令所显示的信息，有很多不同的方式。要查看关于当前工作目录内容的更多信息，请使用下面的 shell 命令:

`$ ls -l`

使用选项`-a`意味着“列出所有目录内容，包括隐藏的文件夹和文件。”隐藏的文件夹和文件名称以句号`.`开头。例如，一个隐藏文件夹是`.git`文件夹，一个隐藏文件是`.gitignore`。`-a`选项显示所有内容，包括隐藏的文件和文件夹。

##### 信息量真大！让我们把它分解一下，看看每一部分。

`-l`标志代表“长格式选项”这些信息比您通常工作所需的信息要多，但是在您需要时能够获得这些信息是很有帮助的。现在让我们继续，但重点是，对于如何在 CLI 中列出目录的内容，有更多的选项来最大限度地满足您的需求。

#### **CLI Shell 命令的组合选项**

在继续学习其他 CLI shell 命令之前，最后一点需要注意:您可以组合任何选项，有时称为标志。对于任何有选项的 shell 命令都是如此。

要查看关于**目录中所有**文件的更多信息，使用以下 shell 命令:

`$ ls -al`

**使用 Shell 命令在文件系统中移动**

要在其他目录中工作，您需要移动您在系统中的参考点。

要改变当前的工作目录，使用下面的 shell 命令:

`$ cd <target-directory>`

下面是一个向下移动一级目录到`build`文件夹的例子:

#### **CLI Shell 命令的组合选项**

通过在 shell 命令中指定完整路径，可以移动到文件系统中的任何目录。例如，macOS 上应用程序文件夹的完整路径是`/Applications`。无论您在文件系统的哪个位置，如果您使用的是 macOS，您都可以使用以下 shell 命令切换到应用程序目录:

`$ cd /Applications`

`$ ls -al`

导航文件系统意味着记住其他目录的路径。GitKraken 客户端中的 CLI 可以通过自动建议可用文件夹来提供帮助！

**使用 Shell 命令在文件系统中移动**

## **上移一个目录**

还记得之前我们运行`ls -a`时的那个【点点】`..`？这些点对于在文件系统中上移一级非常方便。

要在文件系统中向上移动一个目录，使用下面的 shell 命令:

`$ cd ..`

还记得之前我们运行`ls -a`时的那个【点点】`..`？这些点对于在文件系统中上移一级非常方便。

要在文件系统中向上移动一个目录，使用下面的 shell 命令:

`$ cd ..`

**向上移动多个目录级别**

因为每个文件夹都包含`..`，所以您可以调用这个标志来移动到比任何目录更高的一级，即使您不知道更高一级目录的名称。

要向上移动两级目录，请使用以下 shell 命令:

`$ cd ../..`

要上移 3 级目录，使用下面的 shell 命令:

`$ cd ../../..`

`$ cd ../..`

要上移 3 级目录，使用下面的 shell 命令:

`$ cd ../../..`

**上移一个目录**

### **切换到最后一个目录**

如果你曾经使用过电视遥控器，那么你可能熟悉“最后一个频道”按钮，它将频道切换回前一个频道。CLI shells 也有这个选项和`-`符号。

要将目录改回上一个，使用下面的 shell 命令:

`$ cd -`

如果你曾经使用过电视遥控器，那么你可能熟悉“最后一个频道”按钮，它将频道切换回前一个频道。CLI shells 也有这个选项和`-`符号。

要将目录改回上一个，使用下面的 shell 命令:

`$ cd -`

**向上移动多个目录级别**

### **相对于原点的变化**

在 CLI shell 中,`~`符号具有非常特殊的含义；这是您的个人文件夹路径的简写。不必从整个操作系统的根目录`/`指定文件夹路径，您可以使用`~`从您的`/username`文件夹开始一个路径，它被称为您的`Home`文件夹。你会在网上的例子中看到这种速记符号，它节省了大量的打字时间。

要将目录更改到您的`/Users/*username*/Documents/`文件夹中，请使用以下 shell 命令:

`cd ~/Documents`

**使用 Shell 命令与文件和文件系统交互**

在文件结构中移动并能够查看是任何 shell 的主要目标之一。使用 shell 的另一个主要目标是能够操作文件系统并与文件交互。这包括创建、移动、重命名、更新和删除文件。

**切换到最后一个目录**

### **制作新目录**

要新建一个目录或文件夹，使用下面的 shell 命令:

`$ mkdir <newfoldername>`

要新建一个目录或文件夹，使用下面的 shell 命令:

`$ mkdir <newfoldername>`

**相对于原点的变化**

### 默认情况下，运行`mkdir`会在当前工作目录中创建新文件夹。但是，您可以通过提供想要创建它的路径来指定系统上的任何位置。

要在子目录中创建一个新目录，使用以下 shell 命令:

`$ mkdir path/to/target/location/newdirectoryname`

`$ mkdir path/to/target/location/newdirectoryname`

**制作新文件**

## 创建新文件有多种方法。您可以使用文本编辑器打开并保存一个新文件，这将在本系列的下一部分中讨论，或者使用您的 GUI 创建一个文件。但是创建新的空白文件的最快方法之一是使用`touch`命令。

创建一个新的空白文件，使用下面的 shell 命令:

`$ touch f<ile-name.extention>`

**制作新目录**

### 要新建一个目录或文件夹，使用下面的 shell 命令:

`$ mkdir <newfoldername>`

你不仅仅局限于`.txt`扩展名。您可以在这里添加任何您想要的[文件扩展名](https://en.wikipedia.org/wiki/Filename_extension)，如`.js`、`.php`、`.sh`，甚至是`.jpg`或`.mp4`。对于 shell 来说，这些仍然是可编辑的文本文件。其他程序将使用文件扩展名来确定如何处理文件内容以及应该用什么来打开它们。

**移动文件**

要将文件移动到文件系统中的新位置，请使用以下 shell 命令:

`$ mv file newfile-location`

`$ mv file newfile-location`

您也可以移动与当前工作目录不在同一目录中的文件，但您需要指定其路径。

### 创建新文件有多种方法。您可以使用文本编辑器打开并保存一个新文件，这将在本系列的下一部分中讨论，或者使用您的 GUI 创建一个文件。但是创建新的空白文件的最快方法之一是使用`touch`命令。

创建一个新的空白文件，使用下面的 shell 命令:

`$ touch f<ile-name.extention>`

在上面的例子中，您会看到没有指定文件夹的路径，而是使用了特殊字符`.`。圆点的意思是“我的当前位置”但是由于我们想要移动的文件在另一个目录中，我们使用了文件系统中的相对路径。给出完整路径也可以，但更常见的是只使用相对路径。

您还可以使用 shell 命令:`mv`移动整个目录，如下所示。

在上面的例子中，您会看到没有指定文件夹的路径，而是使用了特殊字符`.`。圆点的意思是“我的当前位置”但是由于我们想要移动的文件在另一个目录中，我们使用了文件系统中的相对路径。给出完整路径也可以，但更常见的是只使用相对路径。

您还可以使用 shell 命令:`mv`移动整个目录，如下所示。

你不仅仅局限于`.txt`扩展名。您可以在这里添加任何您想要的[文件扩展名](https://en.wikipedia.org/wiki/Filename_extension)，如`.js`、`.php`、`.sh`，甚至是`.jpg`或`.mp4`。对于 shell 来说，这些仍然是可编辑的文本文件。其他程序将使用文件扩展名来确定如何处理文件内容以及应该用什么来打开它们。

**用‘mv’给东西重新命名**

要使用`mv` shell 命令重命名文件，请使用以下命令:

### `$ mv filename newfilename`

`$ mv file newfile-location`

`$ mv file newfile-location`

起初，这似乎不是一个显而易见的用例，但是如果你退后一步，想想计算机如何将文件从 A 点移动到 B 点，这实际上是很有意义的。不像在房子里移动物理物品，操作系统不会把它们捡起来带走，它只是改变了物品被引用的方式。更改引用的名称就像更改位置一样简单。

您也可以移动与当前工作目录不在同一目录中的文件，但您需要指定其路径。

**使用 Shell 命令复制文件**

要复制一个文件，使用下面的 shell 命令:

`$ cp filename newfilename`

在上面的例子中，您会看到没有指定文件夹的路径，而是使用了特殊字符`.`。圆点的意思是“我的当前位置”但是由于我们想要移动的文件在另一个目录中，我们使用了文件系统中的相对路径。给出完整路径也可以，但更常见的是只使用相对路径。

您还可以使用 shell 命令:`mv`移动整个目录，如下所示。

In the above example, you will see that instead of specifying a path to a folder, the special character `.` was used. Dot just means “my current location.” But since the file we wanted to move was in another directory, we used the relative path to where we were in the file system. Giving the full path would have also worked, but it’s much more common to just use relative paths. 

You can also move entire directories using the shell command: `mv`, as shown below. 

就像移动文件一样，您可以将文件从任何路径复制到任何路径，只要您指定它们的位置。

**用‘mv’给东西重新命名**

### **使用 Shell 命令复制目录**

与移动文件和目录不同，默认情况下，shell 不允许您复制整个目录。如果你尝试，你会得到一个错误`cp: foldername is a directory (not copied).`幸运的是，有一个选项我们可以用来安全地覆盖它。

要复制一个目录，使用下面的 shell 命令:

`$ cp -r foldername newfoldername`

与移动文件和目录不同，默认情况下，shell 不允许您复制整个目录。如果你尝试，你会得到一个错误`cp: foldername is a directory (not copied).`幸运的是，有一个选项我们可以用来安全地覆盖它。

要复制一个目录，使用下面的 shell 命令:

`$ cp -r foldername newfoldername`

在上面的例子中，`-r`代表递归。

在上面的例子中，`-r`代表递归。

**使用 Shell 命令删除文件**

### 要删除文件，请使用以下 shell 命令:

`$ rm -i filename`

这里有几件事需要注意。在上面的例子中，您将看到一个确认步骤:`remove text1.txt?`，用户必须输入`yes`才能继续。这被称为交互模式操作，通过使用`-i`标志触发。这是一个非常好的主意，尤其是当你第一次学习使用命令行来使用`rm`的交互模式时，每次运行它只是为了确保你正在删除你想要删除的东西。

要删除一个没有仔细检查的文件，使用下面的 shell 命令:

`$ rm filename`

还要注意文件去了哪里。与桌面 GUI 将文件移动到垃圾箱不同，CLI shell 只是将它们从文件系统中删除。没有办法拿回那些文件。这是关于使用终端键入 shell 命令的更“危险”的部分之一，也是在使用版本控制时尽早并且经常提交的另一个好理由。

就像移动文件一样，您可以将文件从任何路径复制到任何路径，只要您指定它们的位置。

**用 Shell 命令删除目录**

移除整个目录需要的不仅仅是`rm`。就像 cp shell 命令一样，您需要指定 `-r`标志。如果你非常非常确定要删除一个目录，你可以跳过交互模式，用强制选项强制删除:`-f`。但是要小心，这个命令不能撤销。一旦文件被删除，如果没有版本控制的备份，它们将永远消失。

要强制删除一个目录，使用下面的 shell 命令:

`rm -rf directoryname`

### 与移动文件和目录不同，默认情况下，shell 不允许您复制整个目录。如果你尝试，你会得到一个错误`cp: foldername is a directory (not copied).`幸运的是，有一个选项我们可以用来安全地覆盖它。

要复制一个目录，使用下面的 shell 命令:

`$ cp -r foldername newfoldername`

Unlike moving files and directories, the shell does not allow you to copy entire directories by default.  If you try, you will get an error of `cp: foldername is a directory (not copied).` Luckily, there is an option we can use to override this safely.

To copy a directory, use the following shell command:

`$ cp -r foldername newfoldername`

注意:这是一个非常强大的命令，无论何时使用`-f`的强制选项都应该非常小心。安全护栏的存在是有原因的，如果有其他可行的方法，我们总是建议避免超越它们。

在上面的例子中，`-r`代表递归。

随着您需要学习的命令列表的增长，GitKraken 客户端的 CLI 可以简化找到正确命令的过程！它将帮助您可视化地跟踪您的 Git 项目。

**使用 Shell 命令删除文件**

### **阅读手册**

在本文的前面，你已经了解了一些选项，或者说标志，它们是用`ls`、`cp`和`rm`命令来完成特定技能所需要的。这是很多人开始担心要记住一长串可能选项的地方。不要慌！除了记住一些方便的标志，大多数用户，像本系列的作者一样，在做更复杂的事情时需要查找选项。CLI shell 最棒的一点是文档是内置的！

要访问任何 shell 命令的手册，使用以下命令:

`$ man command`

例如，下面是来自`$ man ls`的输出

这里有几件事需要注意。在上面的例子中，您将看到一个确认步骤:`remove text1.txt?`，用户必须输入`yes`才能继续。这被称为交互模式操作，通过使用`-i`标志触发。这是一个非常好的主意，尤其是当你第一次学习使用命令行来使用`rm`的交互模式时，每次运行它只是为了确保你正在删除你想要删除的东西。

要删除一个没有仔细检查的文件，使用下面的 shell 命令:

`$ rm filename`

还要注意文件去了哪里。与桌面 GUI 将文件移动到垃圾箱不同，CLI shell 只是将它们从文件系统中删除。没有办法拿回那些文件。这是关于使用终端键入 shell 命令的更“危险”的部分之一，也是在使用版本控制时尽早并且经常提交的另一个好理由。

There are a couple of things to note here. In the above example, you will see a confirmation step: `remove text1.txt?`where the user had to enter `yes` to continue. This is called operating in interactive mode, triggered by using the `-i` flag. It is a really good idea, especially when first learning to use the command line to use the interactive mode of `rm` every time you run it just to make very sure you are removing what you intend to remove. 

To delete a file without double checking, use the following shell command:

`$ rm filename`

Also of note is where the file went.  Unlike your desktop GUI that moves things to a trash folder, the CLI shell simply deletes them from the file system.  There is no way to get those files back. This is one of the more ‘dangerous’ parts about using the terminal to type shell commands and another good reason to [Git commit](https://www.gitkraken.com/learn/git/tutorials/how-to-git-commit) early and often when using version control. 

每个内置的 shell 命令都有一个这样的条目。它甚至可以在你的电脑完全离线的情况下工作。虽然这听起来像一些枯燥的阅读，但它实际上是一种非常令人满意的方式，可以了解所有的选项是什么，哪些可能有一天会派上用场。你越深入具体的用例，`man`将会成为一个非常有用的盟友。

### 移除整个目录需要的不仅仅是`rm`。就像 cp shell 命令一样，您需要指定 `-r`标志。如果你非常非常确定要删除一个目录，你可以跳过交互模式，用强制选项强制删除:`-f`。但是要小心，这个命令不能撤销。一旦文件被删除，如果没有版本控制的备份，它们将永远消失。

要强制删除一个目录，使用下面的 shell 命令:

`rm -rf directoryname`

**一个简单的 Shell 命令列表**

现在，您已经了解了导航文件系统以及创建、移动和删除文件和目录的基础知识，您可以放心地开始探索内置于 CLI shell 中的其他命令了。有很多 shell 命令，我们没有时间在这里讨论，但是让我们来讨论几个非常有用的命令。

有用的 Shell 命令:

`$ clear`–这只是清除终端的屏幕。

`$ reset`–重启终端会话，清除进程中的屏幕。

`$ history`–显示您最近运行的命令列表。

`$ ps`–显示正在运行的进程。

`$ cat filename`–将文件内容连接到屏幕上。基本上，这将为您打印出文件内容。有些命令的名字比其他命令更奇怪。

`$ head`–在终端窗口打印出一个文件的前 10 行。您可以通过使用`-n`命令后跟您想要查看的行数来指定您想要打印的行数。例如`head -n 50 file.txt`会将 file.txt 的前 50 行打印到屏幕上。

`$ tail`–打印出文件的最后 10 行。像`head`一样，您可以指定要显示的行数，但是`tail`还有一个非常有用的跟随选项:`-f`。使用此选项时，终端会在文件更新时连续打印文件的底部行。这在查看日志文件时特别[有用。](https://linuxcommando.blogspot.com/2007/11/log-watching-using-tail-or-less.html)

`$ tail`–打印出文件的最后 10 行。像`head`一样，您可以指定要显示的行数，但是`tail`还有一个非常有用的跟随选项:`-f`。使用此选项时，终端会在文件更新时连续打印文件的底部行。这在查看日志文件时特别[有用。](https://linuxcommando.blogspot.com/2007/11/log-watching-using-tail-or-less.html)

**外壳命令的键盘提示**

## 在我们结束本节之前，这里有一些每个 CLI shell 用户都应该知道的有用的键盘技巧。注:`ctl`指键盘上的控制键。

`ctl+r`:Bash 中的“reverse-i-search”或者 Zsh 中的“bck-i-search”。这个热键选项将允许您键入部分命令并重新执行它们，而无需完全重新键入它们。这对于有很多选项的非常复杂的命令来说非常方便。

`ctl+c`:结束当前正在执行的事情。如果你想停止或放弃某个动作，这个键盘组合是你最好的朋友。

`The up arrow`:按向上箭头🔼使用上一个命令填充命令提示符。再往上推，它会在你的整个历史中循环。这样可以节省很多打字的时间。

键入命令、文件名或目录路径的一部分后，按 tab 键将告诉 shell 尝试自动完成您正在键入的内容。假设您有一个包含`index.html`的文件夹。你可以键入`in`并按下`tab`键，而不是键入`index.html`。系统假设您想用`index.html`自动完成该行并填充它。GitKraken Client 实际上提供了相同的功能，并在您键入时自动建议自动完成选项！

键入命令、文件名或目录路径的一部分后，按 tab 键将告诉 shell 尝试自动完成您正在键入的内容。假设您有一个包含`index.html`的文件夹。你可以键入`in`并按下`tab`键，而不是键入`index.html`。系统假设您想用`index.html`自动完成该行并填充它。GitKraken Client 实际上提供了相同的功能，并在您键入时自动建议自动完成选项！

**掌握命令行需要练习**

干得好，走了这么远！您正在成为一名命令行向导！但是掌握这项技能，就像生活中的许多事情一样，需要的不仅仅是阅读理论和尝试一两次命令。这需要练习和坚持。但是不要害怕——没有帮助你不需要这样做！GitKraken CLI 是深入终端的一种令人惊叹的方式，这要归功于自动完成的建议，它可以帮助您探索 shell 命令和选项。

在下一节中，我们将深入研究可以通过 CLI 使用的工具，包括 Git，以及如何获取和安装更多工具！

**一个简单的 Shell 命令列表**

## GitKraken 客户端旨在帮助开发人员专注于重要的事情，而不是记住命令参数和用法。它是管理您的 Git 项目的最佳工具，并释放了命令行的全部力量！

`$ cat filename`–将文件内容连接到屏幕上。基本上，这将为您打印出文件内容。有些命令的名字比其他命令更奇怪。

`$ head`–在终端窗口打印出一个文件的前 10 行。您可以通过使用`-n`命令后跟您想要查看的行数来指定您想要打印的行数。例如`head -n 50 file.txt`会将 file.txt 的前 50 行打印到屏幕上。

`$ tail`–打印出文件的最后 10 行。像`head`一样，您可以指定要显示的行数，但是`tail`还有一个非常有用的跟随选项:`-f`。使用此选项时，终端会在文件更新时连续打印文件的底部行。这在查看日志文件时特别[有用。](https://linuxcommando.blogspot.com/2007/11/log-watching-using-tail-or-less.html)

`$ tail` – Print out the last 10 lines of a file. Like `head`, you can specify the number of lines to show, but `tail` also has a very useful follow option: `-f`. When this option is in use, the terminal will continually print the bottom lines of a file as it’s updated. This is exceptionally [helpful when watching log files](https://linuxcommando.blogspot.com/2007/11/log-watching-using-tail-or-less.html).

**外壳命令的键盘提示**

## 在我们结束本节之前，这里有一些每个 CLI shell 用户都应该知道的有用的键盘技巧。注:`ctl`指键盘上的控制键。

`ctl+r`:Bash 中的“reverse-i-search”或者 Zsh 中的“bck-i-search”。这个热键选项将允许您键入部分命令并重新执行它们，而无需完全重新键入它们。这对于有很多选项的非常复杂的命令来说非常方便。

`ctl+c`:结束当前正在执行的事情。如果你想停止或放弃某个动作，这个键盘组合是你最好的朋友。

`The up arrow`:按向上箭头🔼使用上一个命令填充命令提示符。再往上推，它会在你的整个历史中循环。这样可以节省很多打字的时间。

键入命令、文件名或目录路径的一部分后，按 tab 键将告诉 shell 尝试自动完成您正在键入的内容。假设您有一个包含`index.html`的文件夹。你可以键入`in`并按下`tab`键，而不是键入`index.html`。系统假设您想用`index.html`自动完成该行并填充它。GitKraken Client 实际上提供了相同的功能，并在您键入时自动建议自动完成选项！

`tab` : Pressing the tab key after typing part of a command, filename, or directory path will tell the shell to attempt to autocomplete what you’re typing. Imagine you have a folder containing `index.html`. Instead of typing out `index.html` you could type `in` and hit the `tab` key. The system assumes you want to autocomplete the line with `index.html` and fills it in. GitKraken Client actually offers the same functionality as well as automatically suggesting the autocomplete options as you type!  

**掌握命令行需要练习**

## 干得好，走了这么远！您正在成为一名命令行向导！但是掌握这项技能，就像生活中的许多事情一样，需要的不仅仅是阅读理论和尝试一两次命令。这需要练习和坚持。但是不要害怕——没有帮助你不需要这样做！GitKraken CLI 是深入终端的一种令人惊叹的方式，这要归功于自动完成的建议，它可以帮助您探索 shell 命令和选项。

在下一节中，我们将深入研究可以通过 CLI 使用的工具，包括 Git，以及如何获取和安装更多工具！

In the next section, we will dive into tools you can use through the CLI, including Git, and how to get and install many more!

GitKraken 客户端旨在帮助开发人员专注于重要的事情，而不是记住命令参数和用法。它是管理您的 Git 项目的最佳工具，并释放了命令行的全部力量！