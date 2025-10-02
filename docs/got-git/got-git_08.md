# 8\. Git 杂谈

Git 有着非常庞杂的命令集和功能，到目前为止尚有一些命令及要点还没有介绍。在构思本书的过程中，我尝试用 FreeMind 软件将准备讲述的 Git 的各个命令和要点在各个章节之间拖动，以期在内容上更加充实，组织上更加合理，讲述上更加方便，但最终还是剩下了一些 Git 命令和要点没有被安排在前面的章节中。于是在本书的最后，对这些不常用的 Git 命令和要点（缺乏他们会对于一本被冠以“权威指南”的书的完备性有影响），放在称为“Git 杂谈”的一遍予以讲述。

首先用一章的内容介绍跨平台项目在使用 Git 时的注意事项，包括字符集问题、文件名大小写问题、文本文件换行符问题。接下来的一章概要性的介绍本书到目前为止尚未涉猎到的 Git 话题和相关命令，如：属性、钩子、模板、稀疏检出、浅克隆及嫁接，还会介绍`git replace`和`git notes`等命令。

目录:

*   8.1\. 跨平台操作 Git
    *   8.1.1\. 字符集问题
    *   8.1.2\. 文件名大小写问题
    *   8.1.3\. 换行符问题
*   8.2\. Git 的其他特性
    *   8.2.1\. 属性
        *   8.2.1.1\. 属性定义
        *   8.2.1.2\. 属性文件及优先级
        *   8.2.1.3\. 常用属性介绍
    *   8.2.2\. 钩子和模板
        *   8.2.2.1\. Git 钩子
        *   8.2.2.2\. Git 模板
    *   8.2.3\. 稀疏检出和浅克隆
        *   8.2.3.1\. 稀疏检出
        *   8.2.3.2\. 浅克隆
    *   8.2.4\. 嫁接和替换
        *   8.2.4.1\. 提交嫁接
        *   8.2.4.2\. 提交替换
    *   8.2.5\. Git 评注
        *   8.2.5.1\. 评注的奥秘
        *   8.2.5.2\. 评注相关命令
        *   8.2.5.3\. 评注相关配置

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 8.1\. 跨平台操作 Git

读者是在什么平台（操作系统）中使用 Git 呢？图 40-1 是网上一个 Git 调查结果的截图，从中可以看出排在前三位的是：Linux、Mac OS X 和 Windows。而 Windows 用户中又以使用 msysGit 的用户居多。

![../_images/git-survs-os.png](img/git-survs-os.png)

图 40-1：Git 用户操作系统使用分布图（摘自：[`www.survs.com/results/33Q0OZZE/MV653KSPI2`](http://www.survs.com/results/33Q0OZZE/MV653KSPI2)）

在如今手持设备争夺激烈的年代，进行软件开发工作在什么操作系统上已经变得不那么重要了，很多手持设备都提供可以运行在各种主流操作系统的虚拟器，因此一个项目团队的成员根据各自习惯，可能使用不同的操作系统。当一个团队中不同成员在不同平台中使用 Git 进行交互时，可能会遇到平台兼容性问题。

即使团队成员都在同一种操作系统上工作（如 Windows），但是 Git 服务器可能架设在另外的平台上（如 Linux），也同样会遇到平台兼容性问题。

*   8.1.1\. 字符集问题
*   8.1.2\. 文件名大小写问题
*   8.1.3\. 换行符问题

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 8.1.1\. 字符集问题

在本书第 1 篇“第三章安装 Git”中，就已经详细介绍了不同平台对本地字符集（如中文）的支持情况，本章再做以简单的概述。

Linux、Mac OS X 以及 Windows 下的 Cygwin 缺省使用 UTF-8 字符集。Git 运行这这些平台下，能够使用本地语言（如中文）写提交说明、命名文件，甚至使用本地语言命名分支和里程碑。在这些平台上唯一要做的就是对 Git 进行如下设置，以便使用了本地语言（如中文）命名文件时，能够在状态查看、差异比较时，正确显示文件名。

```
$ git config --global core.quotepath false

```

但是如果在 Windows 平台使用 msysGit 或者其他平台使用了非 UTF-8 字符集，要想使用本地语言撰写提交说明、命名文件名和目录名就非常具有挑战性了。例如对于使用 GBK 字符集的中文 Windows，需要为 Git 进行如下设置，以便能够在提交说明中正确使用中文。

```
$ git config --system core.quotepath false
$ git config --system i18n.commitEncoding gbk
$ git config --system i18n.logOutputEncoding gbk

```

当像上面那样对`i18n.commitEncoding`进行设置后，如果执行提交，就会在提交对象中嵌入编码设置的指令。例如在 Windows 中使用 msysGit 执行一次提交，在 Linux 上使用**git cat-file**命令查看提交时会出现乱码，需要使用**iconv**命令对输出进行字符集转换才能正确查看该提交对象。下面输出的倒数第三行可以看到`encoding gbk`这条对字符集设置的指令。

```
$ git cat-file -p HEAD | iconv -f gbk -t utf-8
tree 00e814cda96ac016bcacabcf4c8a84156e304ac6
parent 52e6454db3d99c85b1b5a00ef987f8fc6d28c020
author Jiang Xin <jiangxin@ossxp.com> 1297241081 +0800
committer Jiang Xin <jiangxin@ossxp.com> 1297241081 +0800
encoding gbk

添加中文说明。

```

因为在提交对象中声明了正确的字符集，因此 Linux 下可以用**git log**命令正确显示 msysGit 生成的包含中文的提交说明。

但是对于非 UTF-8 字符集平台（如 msysGit）下，使用本地字符（如中文）命名文件或目录，Git 当前版本（1.7.4）的支持尚不完善。文件名和目录名实际上是写在树对象中的，Git 没能在创建树对象时，将本地字符转换为 UTF-8 字符进行保存，因而在跨平台时造成文件名乱码。例如下面的示例显示的是在 Linux 平台（UTF-8 字符集）下查看由 msysGit 提交的包含中文文件的树对象。注意要在**git cat-file**命令的后面通过管道符号调用**iconv**命令进行字符转换，否则不能正确地显示中文。如果直接在 Linux 平台检出，检出的文件名显示为乱码。

```
$ git cat-file -p HEAD^{tree} | iconv -f gbk -t utf-8
100644 blob 8c0b112f56b3b9897007031ea38c130b0b161d5a    说明.txt

```

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 8.1.2\. 文件名大小写问题

Linux、Solaris、BSD 及其他类 Unix 操作系统使用的是大小写敏感的文件系统，而 Windows 和 Mac OS X（默认安装）的文件系统则是大小写不敏感的文件系统。即用文件名`README`、`readme`以及`Readme`（混合大小写）进行访问，在 Linux 等操作系统上访问的是不同的文件，而在 Windows 和 Mac OS X 上则指向同一个文件。换句话说，两个不同文件`README`和`readme`在 Linux 等操作系统上可以共存，而在 Windows 和 Mac OS X 上，这两个文件只能同时存在一个，另一个会被覆盖，因为在大小写不敏感的操作系统看来，这两个文件是同一个文件。

如果在 Linux 上为 Git 版本库添加了两个文件名仅大小写不同的文件（文件内容各不相同），如`README`和`readme`。当推送到服务器的共享版本库上，并在文件名大小写不敏感的操作系统中克隆或同步时，就会出现问题。例如在 Windows 和 Mac OS X 平台上执行**git clone**后，在本地工作区中出现两个同名文件中的一个，而另外一个文件被覆盖，这就会导致 Git 对工作区中文件造成误判，在提交时会导致文件内容被破坏。

当一个项目存在跨平台开发的情况时，为了避免这类问题的发生，在一个文件名大小写敏感的操作系统（如 Linux）中克隆版本库后，应立即对版本库进行如下设置，让版本库的行为好似对文件大小写不敏感。

```
$ git config core.ignorecase true

```

Windows 和 Mac OS X 在初始化版本库或者克隆一个版本库时，会自动在版本库中包含配置变量`core.ignorecase`为`true`的设置，除非版本库不是通过克隆而是直接从 Linux 上拷贝而来。

当版本库包含了`core.ignorecase`为`true`的配置后，文件名在添加时便被唯一确定。如果之后修改文件内容及修改文件名中字母的大小写，再提交亦不会改变文件名。如果对添加文件时设置的文件名的大小写不满意，需要对文件重命名，对于 Linux 来说非常简单。例如执行下面的命令就可以将`changelog`文件的文件名修改为`ChangeLog`。

```
$ git mv changelog ChangeLog
$ git commit

```

但是对于 Windows 和 Mac OS X，却不能这么操作，因为会拒绝这样的重命名操作：

```
$ git mv changelog ChangeLog
fatal: destination exists, source=changelog, destination=ChangeLog

```

而需要像下面这样，先将文件重命名为另外的一个名称，再执行一次重命名改回正确的文件名，如下：

```
$ git mv changelog non-exist-filename
$ git mv non-exist-filename ChangeLog
$ git commit

```

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 8.1.3\. 换行符问题

每一个通用的版本控制系统，无论是 CVS、Subversion、Git 或是其他，都要面对换行符转换的问题。这是因为作为通用的版本控制系统要面对来自不同操作系统的文件，而不同的操作系统在处理文本文件时，可能使用不同的换行符。

**不同的操作系统可能使用不同的换行符**

文本文件的每一行结尾用一个或者两个特殊的 ASCII 字符进行标识，这个标识就是换行符。主要的换行符有三种：LF（Line Feed 即换行，C 语言等用“`\\n`”表示，相当于十六进制的`0x0A`），CR（Carriage Return 即回车，C 语言等用“`\\r`”表示，相当于十六进制的`0x0D`）和 CRLF（即由两个字符“CR+LF”组成，即“`\\r\\n`”，相当于十六进制的`0x0D 0x0A`），分别用在不同的操作系统中。（以下内容摘自[`en.wikipedia.org/wiki/Newline`](http://en.wikipedia.org/wiki/Newline)。）

*   LF 换行符：用于 Multics、Unix、类 Unix（如 GNU/Linux、AIX、Xenix、Mac OS X、FreeBSD 等）、BeOS、Amiga、RISC OS 等操作系统中。
*   CRLF 换行符：用于 DEC TOPS-10、RT-11 和其他早期的非 Unix，以及 CP/M、MP/M、DOS（MS-DOS、PC-DOS 等）、Atari TOS、OS/2、Microsoft Windows、Symbian OS、Palm OS 等系统中。
*   CR 换行符：用于 Commodore 8 位机、TRS-80、苹果 II 家族、Mac OS 9 及更早版本。

实际上，自从苹果的 Mac OS 从第 10 版转向 Unix 内核开始，依据不同的文本文件换行符，主流的操作系统可以划分为两大阵营，一个是微软 Windows 作为一方，使用 CRLF 作为换行符，另外一方包括 Unix、类 Unix（如 Linux 和 Mac OS X 等）使用 LF 作为换行符。分属不同阵营的操作系统之间交换文本文件会因为换行符的不同造成障碍，而对于使用版本控制系统，也同样会遇到换行符的麻烦。

*   编辑器不能识别换行符，可能会显示为特殊字符，如 Linux 上的编辑器显示的`^M`特殊字符，就是拜 Windows 的 CRLF 换行符所赐。或者丢弃换行符，如来自 Linux 的文本文件，在 Windows 上打开可能会因为识别不了换行符，导致所有的行合并。
*   版本库中的文件被来自不同操作系统的用户改来改去，在某一次提交中换行符为 LF，在下一次提交中被替换为 CRLF，这不但会在查看文件版本间差异时造成困惑（所有的行都显示为变更），还给版本库的存储带来不必要的冗余。
*   可能会在一个文件中引入混杂的换行符，即有的行是 LF，而有的行是 CRLF。无论在那个操作系统用编辑器打开这样的文件，都会或多或少感到困惑。
*   如果版本控制系统提供文本文件换行符的自动转换，在 Windows 平台进行版本库文件导出为源码包并发布，当该源码包被 Linux 用户下载，编译、运行可能会有问题，反之亦然。

**文本文件和二进制文件的判别，是换行符转换的基础**

几乎所有的版本库控制系统都采用这样的解决方案：对于**文本文件**，在版本库中保存时换行符使用 LF，当从版本库检出到工作区时，则根据平台的不同或者用户的设置的不同，对文本文件的换行符进行转换（转换为 LF、CR 或 CRLF）。

为什么换行符转换要特意强调文本文件呢？这是因为如果对二进制文件（程序或者数据）当中出现的换行符进行上述转换，会导致二进制文件被破坏。因此判别文件类型是文本文件还是二进制文件，是正确进行文件换行符转换的基础。

有的版本控制系统，如 CVS，必须在添加文件时人为的设定文件类型（用`-kb`参数设定二进制文件），一旦用户忘记对二进制文件进行标记，就会造成二进制文件被破坏。这种破坏有时藏的比较深，例如在 Linux 上检出文件一切正常，因为版本库中被误判为文本文件的图形文件中所包含字符`0x0A`在 Linux 上检出没有改变，但是在 Windows 上检出会导致图形文件中的`0x0A`字符被转换为`0x0D 0x0A`两个字符，造成图片被破坏。

有的版本控制系统可以自动识别文本文件和二进制文件，但是识别算法存在问题。例如 Subversion 检查文件的前 1024 字节的内容，如果其中包含 NULL 字符（`0x00`），或者超过 15%是非 ASCII 字符，则 Subversion 认定此文件为二进制文件（参见 Subversion 源代码`subversion/libsvn_subr/io.c`中的`svn_io_detect_mimetype2`函数）。这种算法会将包含大量中文的文本文件当作二进制文件，不进行换行符转换，也不能进行版本间的比较（除非强制执行）。

Git 显然比 Subversion 更了解这个世界上文字的多样性，因此在判别二进制文件上没有多余的判别步骤，只对 blob 对象的前 8000 个字符进行检查，如果其中出现 NULL 字符（`0x00`）则当作二进制文件，否则为文本文件（参见 Git 源代码`xdiff-interface.c`中的`buffer_is_binary`函数）。Git 还允许用户通过属性文件对文件类型进行设置，属性文件设置优先。

Git 缺省并不开启文本文件的换行符转换，因为毕竟 Git 对文件是否是二进制文件所做的猜测存在误判的可能。如果用户通过属性文件或者其他方式显式的对文件类型进行了设置，则 Git 就会对文本文件开启换行符转换。

下面是一个属性文件的示例，为方便描述标以行号。

```
1  *.txt           text
2  *.vcproj        eol=crlf
3  *.sh            eol=lf
4  *.jpg           -text
5  *.jpeg          binary

```

包含了上面属性文件的版本库，会将`.txt`、`.vcproj`、`.sh`为扩展名的文件视为文本文件，在处理过程中会进行换行符转换，而将`.jpg`、`.jpeg`为扩展名的文件视为二进制文件，不进行换行符转换。

**依据属性文件进行换行符转换**

关于属性文件，会在后面的章节详细介绍，现在可以将其理解为工作区目录下的`.gitattributes`文件，其文件匹配方法及该文件的作用范围和`.gitignore`文件非常类似。

像上面的属性文件示例中，第 1 行设置了扩展名为`.txt`的文件具有 text 属性，则所有扩展名为`.txt`的文件添加到版本库时，在版本库中创建的 blob 文件的换行符一律转换为 LF。而当扩展名为`.txt`的文件检出到工作区时，则根据平台的不同使用不同的换行符，如在 Linux 上检出使用 LF 换行符，在 Windows 上检出使用 CRLF 换行符。

示例中的第 2 行设置扩展名为`.vcproj`的文件的属性`eol`的值为`crlf`，隐含着该文件属于文本文件的含义，当向版本库添加扩展名为`.vcproj`文件时，在版本库中创建的 blob 文件的换行符一律转换为 LF。而当该类型的文件检出到工作区时，则一律使用 CRLF 作为换行符，不管是在 Windows 上检出，还是在 Linux 上检出。

同理示例中的第 3 行设置的扩展名为`.sh`的文件也会进行类似的换行符转换，区别在于该类型文件无论在哪个平台检出，都使用 LF 作为换行符。

向上面那样逐一为不同类型的文件设置换行符格式显得很麻烦，可以在属性文件中添加下面的设置，为所有文件开启自动文件类型判断。

```
*  text=auto

```

当为所有文件设置了`text=auto`的属性后，Git 就会在文件检入和检出时对文件是否是二进制进行判断，采用前面提到的方法：如果文件头部 8000 个字符中出现 NULL 字符则为二进制文件，否则为文本文件。如果判断文件是文本文件就会启用换行符转换。至于本地检出文件采用什么换行符格式，实际上是由`core.eol`配置变量进行设置的，不过因为`core.eol`没有设置时采用缺省值`native`，才使得工作区文本文件的检出采用操作系统默认的换行符格式。配置变量`core.eol`除了默认的`native`外，还可以使用`lf`和`crlf`，不过一般较少用到。

**使用 Git 配置变量控制换行符转换**

在 Git 1.7.4 之前，用属性文件的方式来设置文件的换行符转换，只能逐一为版本库进行设置，如果要为本地所有的版本库设定文件换行符转换就非常的麻烦。Git 1.7.4 提供了全局可用的属性文件，实现了对换行符转换设定的全局控制，我们会在后面的章节加以介绍。现在介绍另外一个方法，即通过配置变量`core.autocrlf`来开启文本文件换行符转换的功能。例如执行下面的命令，对配置变量`core.autocrlf`进行设置：

```
$ git config --global core.autocrlf true

```

默认 Git 不对配置变量`core.autocrlf`进行设置，因此在也没有通过属性文件指定文件类型的情况下，Git 不对文件进行换行符转换。但是将配置变量`core.autocrlf`设置为下列值时，会开启 Git 对文件类型的智能判别并对文本文件执行换行符转换。

*   设置配置变量`core.autocrlf`为`true`。

    效果就相当于为版本库中所有文件设置了`text=auto`的属性。即通过 Git 对文件类型的自动判定，对文本文件进行换行符转换。在版本库的 blob 文件中使用 LF 作为换行符，而检出到工作区时无论是什么操作系统都使用 CRLF 为换行符。注意当设置了`core.autocrlf`为`true`时，会忽略`core.eol`的设置，工作区文件始终使用 CRLF 作为换行符，这对于 Windows 下的 Git 非常适合，但不适用于 Linux 等操作系统。

*   设置配置变量`core.autocrlf`为`input`。

    同样开启文本文件的换行符转换，但只是在文件提交到版本库时，将新增入库的 blob 文件的换行符转换为 LF。当从版本库检出文件到工作区，则不进行文件转换，即版本库中文件若是采用 LF 换行符，检出仍旧是 LF 作为换行符。这个设置对 Linux 等操作系统下的 Git 非常适合，但不适合于 Windows。

**配制``core.safecrlf``捕捉异常的换行符转换**

无论是用户通过属性文件设定文件的类型，还是通过 Git 智能判别，都可能错误的将二进制文件识别为文本文件，在转换过程中造成文件的破坏。有一种情况下破坏最为严重，就是误判的文件中包含不一致的换行符（既有 CRLF，又有 LF），这就会导致保存到版本库中的 blob 对象无论通过何种转换方式都不能还原回原有的文件。

Git 提供了名为`core.safecrlf`的配置变量，可以用于捕捉这种不可逆的换行符转换，提醒用户注意。将配置变量`core.safecrlf`设置为`true`时，如果发现存在不可逆换行符转换时，会报错退出，拒绝执行不可逆的换行符转换。如果将配置变量`core.safecrlf`设置为`warn`则允许不可逆的转换，但会发出警告。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 8.2\. Git 的其他特性

*   8.2.1\. 属性
    *   8.2.1.1\. 属性定义
    *   8.2.1.2\. 属性文件及优先级
    *   8.2.1.3\. 常用属性介绍
*   8.2.2\. 钩子和模板
    *   8.2.2.1\. Git 钩子
    *   8.2.2.2\. Git 模板
*   8.2.3\. 稀疏检出和浅克隆
    *   8.2.3.1\. 稀疏检出
    *   8.2.3.2\. 浅克隆
*   8.2.4\. 嫁接和替换
    *   8.2.4.1\. 提交嫁接
    *   8.2.4.2\. 提交替换
*   8.2.5\. Git 评注
    *   8.2.5.1\. 评注的奥秘
    *   8.2.5.2\. 评注相关命令
    *   8.2.5.3\. 评注相关配置

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 8.2.1\. 属性

Git 通过属性文件为版本库中的文件或目录添加属性。设置了属性的文件或目录，例如之前介绍换行符转换时设置了文本属性（`text`）的文件，在执行 Git 相关操作时会做特殊处理。

## 8.2.1.1\. 属性定义

属性文件是一个普通的文本文件，每一行对一个路径（可使用通配符）设置相应的属性。语法格式如下：

```
<pattern> <attr1> <attr2> ...

```

其中路径由可以使用通配符的`<pattern>`定义，属性可以设置一个或多个，不同的属性之间用空格分开。路径中通配符的用法和文件忽略（file:.gitignore）的语法格式相同，参见本书第 2 篇“第 10.8 节文件忽略”相关内容。下面以`text`属性为例，介绍属性的不同写法：

*   text

    直接通过属性名进行设置，相当于设置`text`属性的值为`true`。

    对于设置了`text`属性的文件，不再需要 Git 对文件类型进行猜测，而直接判定为文本文件并进行相应的换行符转换。

*   -text

    在属性名前用减号标识，相当于设置`text`属性值为`false`。

    对于设置了取反`text`属性的文件，直接判定为二进制文件，在文件检入和检出时不进行换行符转换。

*   !text

    在属性名前面添加感叹号，相当于该属性没有设置，即不等于`true`，也不等于`false`。

    对于未定义`text`属性的文件，根据 Git 是否配置了`core.autocrlf`配置变量，决定是否进行换行符转换。因此对于`text`属性没有定义和进行取反`text`属性设置，两者存在差异。

*   text=auto

    属性除了上述`true`、`false`、未设置三个状态外，还可以对属性用相关的枚举值（预定义的字符串）进行设置。不同的属性值可能有不同的枚举值，对于`text`属性可以设置为`auto`。

    对于`text`属性设置为`auto`的文件，文件类型实际上尚未确定，需要 Git 读取文件内容进行智能判别，判别为文本文件则进行换行符转换。显然当设置`text`属性为`auto`时，并不等同于`true`。

## 8.2.1.2\. 属性文件及优先级

属性文件可以以`.gitattributes`文件名保存在工作区目录中，提交到版本库后就可以和其他用户共享项目文件的属性设置。属性文件也可以保存在工作区之外，例如保存在文件`.git/info/attributes`中，仅对本版本库生效，若保存在`/etc/gitattributes`文件中则全局生效。在查询某个工作区某一文件的属性时，在不同位置的属性文件具有不同的优先级，Git 依据下列顺序依次访问属性文件。

*   文件`.git/info/attributes`具有最高的优先级。
*   接下来检查工作区同一目录下的`.gitattributes`，并依次向上递归查找`.gitattributes`文件，直到工作区的根目录。
*   然后查询由 Git 的配置变量`core.attributesfile`指定的全局属性文件。
*   最后是系统属性文件，即文件`$(prefix)/etc/gitattributes`。不同的 Git 安装方式这个文件的位置可能不同，但是该文件始终和 Git 的系统配置文件（可以通过**git config --system -e**命令打开进而知道位置）位于同一目录中。

注意只有在 1.7.4 或更新版本的 Git 才提供后两种（全局和系统级的）属性文件。可以通过下面的例子来理解属性文件的优先级和属性设置方法。

首先来看看某个版本库即系统中所包含的属性文件：

*   其一是位于版本库中的文件`.git/info/attributes`，内容如下：

    ```
    a*      foo !bar -baz

    ```

*   其二是位于工作区子目录`t`下的属性文件，即`t/.gitattributes`，内容如下：

    ```
    ab*     merge=filfre
    abc     -foo -bar
    *.c     frotz

    ```

*   再一个是位于工作区根目录下的属性文件`.gitattributes`，内容如下：

    ```
    abc     foo bar baz

    ```

*   系统文件`/etc/gitconfig`中包含如下配置，则每个用户主目录下的`.gitattributes`文件做为全局属性文件。

    ```
    [core]
      attributesfile = ~/.gitattributes

    ```

*   位于用户主目录下的属性文件，即文件`~/.gitattributes`的内容如下：

    ```
    * text=auto

    ```

当查询工作区文件`t/abc`的属性时，根据属性文件的优先级，按照下列顺序进行检索：

1.  先检查属性文件`.git/info/attributes`。显然该文件中唯一的一行就和文件`t/abc`匹配，因此文件`t/abc`的属性如下：

> ```
> foo   : true
> bar   : 未设置
> baz   : false
> 
> ```

2.  再检查和文件`t/abc`同目录的属性文件`t/.gitattributes`。该属性文件的前两行和路径`t/abc`相匹配，但是因为第二行设置`foo`和`bar`属性已经由属性文件`.git/info/attributes`提供，因此第二行的设置不起作用。经过这一步，文件`t/abc`获得的属性为：

> ```
> foo   : true
> bar   : 未设置
> baz   : false
> merge : filfre
> 
> ```

3.  然后沿工作区当前目录向上遍历属性文件，找到工作区根目录下的属性文件`.gitattributes`，进行检查。因为该属性文件设置的属性已经由前面的属性文件提供，所以文件`t/abc`的属性和上面第 2 步的结果一样。
4.  因为设置了`core.attributesfile`为`~/.gitattributes`文件，因此接下来查找用户主目录下文件即`.gitattributes`。该文件唯一的一行匹配所有文件，因此`t/abc`又被附加了新的属性值`text=auto`。最终文件`t/abc`的属性如下。

> ```
> foo   : true
> bar   : 未设置
> baz   : false
> merge : filfre
> text  : auto
> 
> ```

## 8.2.1.3\. 常用属性介绍

### 8.2.1.3.1\. text

属性`text`用于显式的指定文件的类型：二进制（`-text`）、文本文件（`text`）或是开启文件类型的智能判别（`text=auto`）。对于文本文件，Git 会对其进行换行符转换。本书第四十章“40.3 换行符问题”中已经详细介绍了属性`text`的用法，并且在本章“40.1.1 属性定义”的示例中对属性`text`的取值做了总结，在此不再赘述。

在“40.3 换行符问题”一节，我们还知道可以通过在 Git 配置文件中设置`core.autocrlf`配置变量，来开启 Git 对文件类型的智能判别，并对文本文件开启换行符转换。那么 Git 的配置变量`core.autocrlf`和属性`text`有什么异同呢？

当设置了 Git 了配置变量`core.autocrlf`为`true`或者`input`后，相当于设置了属性`text=auto`。但是 Git 配置文件中的配置变量只能在本地进行设置并且只对本地版本库有效，不能通过共享版本库传递到其他用户的本地版本库中，因而`core.autocrlf`开启换行符转换不能跟其他用户共享，或者说不能将换行符转换策略设置为整个项目（版本库）的强制规范。属性文件则不同，可以被检入到版本库中并通过共享版本库传递给其他用户，因此可以通过在检入的`.gitattributes`文件中设置`text`属性，或者干脆设置`text=auto`属性，强制同一项目的所有用户在提交文本文件时都要规范换行符。

建议所有存在跨平台开发可能的项目都在项目根目录中检入一个`.gitattributes`文件，根据文件扩展名设置文件的`text`属性，或者设置即将介绍的`eol`属性。

### 8.2.1.3.2\. eol

属性`eol`用于设定文本文件的换行符格式。对于设置了`eol`属性的文件，如果没有设定`text`属性时，默认会设置`text`属性为`true`。属性`eol`的取值如下：

*   eol=crlf

    当文件检入版本库时，blob 对象使用 LF 作为换行符。当检出到工作区时，使用 CRLF 作为换行符。

*   eol=lf

    当文件检入版本库时，blob 对象使用 LF 作为换行符，检出的时候工作区文件也使用 LF 作为换行符。

除了通过属性设定换行符格式外，还可以在 Git 的配置文件通过`core.eol`配置变量来设定。两者的区别在于配置文件中的`core.eol`配置变量设置的换行符是一个默认值，没有通过`eol`属性指定换行符格式的文本文件会采用`core.eol`的设置。变量`core.eol`的值可以设定为`lf`、`crlf`和`native`。默认`core.eol`的取值为`native`，即采用操作系统标准的换行符格式。

下面的示例通过属性文件设置文件的换行符格式。

```
*.vcproj        eol=crlf
*.sh            eol=lf

```

扩展名为`.vcproj`的文件使用 CRLF 作为换行符，而扩展名为`.sh`的文件使用 LF 作为换行符。在版本库中检入类似的属性文件，会使得 Git 客户端无论在什么操作系统中都能够在工作区检出一致的换行符格式，这样无论是在 Windows 上还是在 Linux 上使用**git archive**命令将工作区文件打包，导出的文件都会保持正确的换行符格式。

### 8.2.1.3.3\. ident

属性`ident`开启文本文件中的关键字扩展，即关键字`$Id$`的自动扩展。当检出到工作区时，`$Id$`自动扩展为`$Id:`，后面紧接着 40 位 SHA1 哈希值（相应 blob 对象的哈希值），然后以一个`$`字符结尾。当文件检入时，要对内容中出现的以`$Id:`开始，以`$`结束的内容替换为`$Id$`再保存到 blob 对象中。

这个功能可以说是对 CVS 相应功能的模仿。自动扩展的内容使用的是 blob 的哈希值而非提交本身的哈希值，因此并无太大实际意义，不建议使用。如果希望在文本文件中扩展出提交者姓名、提交 ID 等更有实际意义的内容，可以参照后面介绍的属性`export-subst`。

### 8.2.1.3.4\. filter

属性`filter`为文件设置一个自定义转换过滤器，以便文件在检入版本库及检出到工作区时进行相应的转换。定义转换过滤器通过 Git 配置文件来完成，因此这个属性应该只在本地进行设置，而不要通过检入到版本库中的`.gitattributes`文件传递。

例如下面的属性文件设置了所有的 C 语言源文件在检入和检出的时候使用名为 indent 的代码格式化过滤器。

```
*.c     filter=indent

```

然后还要通过 Git 配置文件设定 indent 过滤器，示例如下：

```
[filter "indent"]
        clean = indent
        smudge = cat

```

定义过滤器只要设置两条命令，一条是名为 clean 的配置设定的的命令，用于在文件检入时执行，另外一条是名为 smudge 的配置设定的命令，用于将文件检出到工作区时使用的命令。对于本例，在代码检入时执行**indent**命令对代码格式化后，再保存到版本库中。当检出到工作区执行**cat**命令，实际上相当于直接将 blob 对象复制到工作区。

### 8.2.1.3.5\. diff

和前面介绍的属性不同，属性`diff`不会对文件检入检出造成影响，而只是在查看文件历史变更时起作用。属性`diff`可以取值如下：

*   diff

    进行版本间比较时，以文本方式进行比较，即使文件看起来像是二进制文件（包含 NULL 字符），或者被设置为二进制文件（`-text`）。

*   -diff

    不以文本方式进行差异比较，而以二进制方式进行比较。因为默认查看版本间差异时只显示文本文件的差异不显示二进制文件差异，因此包含`-diff`属性设置的文件在差异比较时不显示内容上的差异。对于有些文本文件（如 postscript 文件）进行差异比较没有意义，可以对其设置`-diff`属性，避免在显示提交间差异时造成干扰。

*   !diff

    不设置`diff`属性，相当于在执行差异比较时要对文件内容进行智能判别，如果文件看起来像是文本文件，则显示文本格式的差异比较。

*   diff=<driver>

    设定一个外部的驱动用于文件的差异比较。例如对于 Word 文档的差异比较就可以通过这种方式进行配置。

Word 文档属于二进制文件，默认不显示差异比较。在 Linux 上有一个名为`antiword`的应用软件可以将 Word 文档转换为文本文件显示，借助该软件就可以实现在 Linux（包括 Mac OS X）上显示 Word 文件的版本间差异。

下面的 Git 配置就定义了一个名为 antiword 的适用于 Word 差异比较的驱动：

```
[diff "antiword"]
  textconv=antiword

```

其中`textconv`属性用于设定一个文件转换命令行，这里设置为`antiword`，用于将 Word 文档转换为纯文本。

然后还需要设置属性，修改版本库下的`.git/info/attributes`文件就可以，新增属性设置如下：

```
*.doc  diff=antiword

```

关于更多的差异比较外部驱动的设置，执行`git help --web attributes`参见相关的帮助。

### 8.2.1.3.6\. merge

属性`merge`用于为文件设置指定的合并策略，受影响的 Git 命令有：**git merge**、**git revert**和**git cherry-pick**等。属性`merge`可以取值如下：

*   merge

    使用内置的三向合并策略。

*   -merge

    将当前分支的文件版本设置为暂时的合并结果，并且声明合并发生了冲突，这实际上是二进制文件默认的合并方式。可以对文本文件设置该属性，使得在合并时的行为类似二进制文件。

*   !merge

    和定义了`merge`属性效果类似，使用内置的三向合并策略。然而当通过 Git 配置文件的`merge.default`配置变量设置了合并策略后，如果没有为文件设置`merge`属性，则使用`merge.default`设定的策略。

*   merge=<driver>

    使用指定的合并驱动执行三向文件合并。驱动可以是内置的三个驱动，也可以是用户通过 Git 配置文件自定义的驱动。

下面重点说一说通过枚举值来指定在合并时使用的内置驱动和自定义驱动。先来看看 Git 提供的三个内置驱动：

*   merge=text

    默认文本文件在进行三向合并时使用的驱动。会在合并后的文本文件中用特殊的标识`<<<<<<<`、`=======`和`>>>>>>>`来标记冲突的内容。

*   merge=binary

    默认二进制文件在进行三向合并时使用的驱动。会在工作区中保持当前分支中的版本不变，但是会通过在三个暂存区中进行冲突标识使得文件处于冲突状态。

*   merge=union

    在文本文件三向合并过程中，不使用冲突标志符标识冲突，而是将冲突双方的内容简单的罗列在文件中。用户应该对合并后的文件进行检查。请慎用此合并驱动。

用户还可以自定义驱动。例如 Topgit 就使用自定义合并驱动的方式来控制两个 Topgit 管理文件`.topmsg`和`.topdeps`的合并行为。

Topgit 会在版本库的配置文件`.git/info/config`中添加下面的设置定义一个名为 ours 的合并驱动。注意不要将此 ours 驱动和本书第 3 篇第十六章“16.6 合并策略”一节中介绍的 ours 合并策略弄混淆。

```
[merge "ours"]
  name = \"always keep ours\" merge driver
  driver = touch %A

```

定义的合并驱动的名称由`merge.*.name`给出，合并时执行的命令则由配置`merge.*.driver`给出。本例中使用了命令**touch %A**，含义为对当前分支中的文件进行简单的触碰（更新文件时间戳），亦即合并冲突时采用本地版本，丢弃其他版本。

Topgit 还会在版本库`.git/info/attributes`属性文件中包含下面的属性设置：

```
.topmsg merge=ours
.topdeps  merge=ours

```

含义为对这两个 Topgit 管理文件，采用在 Git 配置文件中设定的 ours 合并驱动。Topgit 之所以要这么实现是因为不同特性分支的管理文件之间并无关联，也不需要合并，在遇到冲突时只使用自己的版本即可。这对于 Topgit 要经常地执行变基和分支合并来说，设置这个策略可以简化管理，但是这个合并设置在特定情况下也存在不合理之处。例如两个用户工作在同一分支上同时更改了`.topmsg`文件以修改特性分支的描述，在合并时会覆盖对方的修改，这显然是不好的行为。但是权衡利弊，还是如此实现最好。

### 8.2.1.3.7\. whitespace

Git 可以对文本文件中空白字符的使用是否规范做出检查，在文件差异比较时，将使用不当的空白字符用红色进行标记（开启`color.diff.whitespace`）。也可以在执行**git apply**时通过参数`--whitespace=error`防止错误的空白字符应用到提交中。

Git 默认开启对下面三类错误空白字符的检查。

*   blank-at-eol

    在行尾出现的空白字符（换行符之前）被视为误用。

*   space-before-tab

    在行首缩进中出现在 TAB 字符前面的空白字符视为误用。

*   blank-at-eof

    在文件末尾的空白行视为误用。

Git 还支持对更多空白字符的误用做出检测，包括：

*   indent-with-non-tab

    用 8 个或者更多的空格进行缩进视为误用。

*   tab-in-indent

    在行首的缩进中使用 TAB 字符视为误用。显然这个设置和上面的`indent-with-non-tab`互斥。

*   trailing-space

    相当于同时启用`blank-at-eol`和`blank-at-eof`。

*   cr-at-eol

    将行尾的 CR（回车）字符视为换行符的一部分。也就是说，在行尾前出现的 CR 字符不会引起`trailing-space`报错。

*   tabwidth=<n>

    设置一个 TAB 字符相当于几个空格，缺省为 8 个。

可以通过 Git 配置文件中的`core.whitespace`配置变量，设置开启更多的空白字符检查，将要开启的空白字符检查项用逗号分开即可。

如果希望对特定路径进行空白字符检查，则可以通过属性`whitespace`进行。属性`whitespace`可以有如下设置：

*   whitespace

    开启所有的空白字符误用检查。

*   -whitespace

    不对空白字符进行误用检查。

*   !whitespace

    使用`core.whitespace`配置变量的设置进行空白字符误用检查。

*   whitespace=...

    和`core.whitespace`的语法一样，用逗号分隔各个空白字符检查项。

### 8.2.1.3.8\. export-ignore

设置了该属性的文件和目录在执行**git archive**时不予导出。

### 8.2.1.3.9\. export-subst

如果为文件设置了属性`export-subst`，则在使用**git archive**导出项目文件时，会对相应文件内容中的占位符展开，然后再添加到归档中。注意如果在使用**git archive**导出时使用树 ID，而没有使用提交或者里程碑，则不会发生占位符展开。

占位符的格式为`$Format:PLACEHOLDERS$`，其中`PLACEHOLDERS`使用**git log --pretty=format:**相同的参数（具体参见**git help log**显示的帮助页）。例如：`$Format:%H$`将展开为提交的哈希值，`$Format:%an$`将展开为提交者姓名。

### 8.2.1.3.10\. delta

如果设置属性`delta`为`false`，则不对该路径指向的 blob 文件执行 Delta 压缩。

### 8.2.1.3.11\. encoding

设置文件所使用的字符集，以便使用 GUI 工具（如`gitk`和`git-gui`）能够正确显示文件内容。基于性能上的考虑，`gitk`默认不检查该属性，除非通过`gitk`的偏好设置启用“Support per-file encodings”。

如果没有为文件设置`encoding`属性，则使用`git.encoding`配置变量。

### 8.2.1.3.12\. binary

属性`binary`严格来说是一个宏，相当于`-text -diff`。即禁止换行符转换及禁止文本方式显示文件差异。

用户也可以自定义宏。自定义宏只能在工作区根目录中的`.gitattributes`文件中添加，以内置的`binary`宏为例，相当于在属性文件中进行了如下的设置：

```
[attr]binary -diff -text

```

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 8.2.2\. 钩子和模板

## 8.2.2.1\. Git 钩子

Git 的钩子脚本位于版本库`.git/hooks`目录下，当 Git 执行特定操作时会调用特定的钩子脚本。当版本库通过**git init**或者**git clone**创建时，会在`.git/hooks`目录下创建示例脚本，用户可以参照示例脚本的写法开发适合的钩子脚本。

钩子脚本要设置为可运行，并使用特定的名称。Git 提供的示例脚本都带有`.sample`扩展名，是为了防止被意外运行。如果需要启用相应的钩子脚本，需要对其重命名（去掉`.sample`扩展名）。下面分别对可用的钩子脚本逐一介绍。

### 8.2.2.1.1\. applypatch-msg

该钩子脚本由**git am**命令调用。在调用时向该脚本传递一个参数，即保存有提交说明的文件的文件名。如果该脚本运行失败（返回非零值），则**git am**命令在应用该补丁之前终止。

这个钩子脚本可以修改文件中保存的提交说明，以便对提交说明进行规范化以符合项目的标准（如果有的话）。如果提交说明不符合项目标准，脚本直接以非零值退出，拒绝提交。

Git 提供的示例脚本`applypatch-msg.sample`只是简单的调用`commit-msg`钩子脚本（如果存在的话）。这样通过**git am**命令应用补丁和执行**git commit**一样都会执行`commit-msg`脚本，因此如须定制，请更改`commit-msg`脚本。

### 8.2.2.1.2\. pre-applypatch

该钩子脚本由**git am**命令调用。该脚本没有参数，在补丁应用后但尚未提交之前运行。如果该脚本运行失败（返回非零值），则已经应用补丁的工作区文件不会被提交。

这个脚本可以用于对应用补丁后的工作区进行测试，如果测试没有通过则拒绝提交。

Git 提供的示例脚本`pre-applypatch.sample`只是简单的调用`pre-commit`钩子脚本（如果存在的话）。这样通过**git am**命令应用补丁和执行**git commit**一样都会执行`pre-commit`脚本，因此如须定制，请更改`pre-commit`脚本。

### 8.2.2.1.3\. post-applypatch

该钩子脚本由**git am**命令调用。该脚本没有参数，在补丁应用并且提交之后运行，因此该钩子脚本不会影响**git am**的运行结果，可以用于发送通知。

### 8.2.2.1.4\. pre-commit

该钩子脚本由**git commit**命令调用。可以向该脚本传递`--no-verify`参数，此外别无参数。该脚本在获取提交说明之前运行。如果该脚本运行失败（返回非零值），Git 提交被终止。

该脚本主要用于对提交数据的检查，例如对文件名进行检查（是否使用了中文文件名），或者对文件内容进行检查（是否使用了不规范的空白字符）。

Git 提供的示例脚本`pre-commit.sample`禁止提交在路径中使用了非 ASCII 字符（如中文字符）的文件。如果确有使用的必要，可以在 Git 配置文件中设置配置变量`hooks.allownonascii`为`true`以允许在文件名中使用非 ASCII 字符。Git 提供的该示例脚本也对不规范的空白字符进行检查，如果发现则终止提交。

Topgit 为所管理的版本库设置了自己的`pre-commit`脚本，检查工作的 Topgit 特性分支是否正确设置了两个 Topgit 管理文件`.topdeps`和`.topmsg`，以及定义的分支依赖是否存在着重复依赖和循环依赖等。

### 8.2.2.1.5\. prepare-commit-msg

该钩子脚本由**git commit**命令调用，在默认的提交信息准备完成后但编辑器尚未启动之前运行。

该脚本有 1 到 3 个参数。第一个参数是包含提交说明的文件的文件名。第二个参数是提交说明的来源，可以是`message`（由`-m`或者`-F`参数提供），可以是`template`（如果使用了`-t`参数或由`commit.template`配置变量提供），或者是`merge`（如果提交是一个合并或存在`.git/MERGE_MSG`文件），或者是`squash`（如果存在`.git/SQUASH_MSG`文件），或者是`commit`并跟着一个提交 SHA1 哈希值（如果使用`-c`、`-C`或者`--amend`参数）。

如果该脚本运行失败（返回非零值），Git 提交被终止。

该脚本用于对提交说明进行编辑，并且该脚本不会因为`--no-verify`参数被禁用。

Git 提供的示例脚本`prepare-commit-msg.sample`可以用于向提交说明中嵌入提交者签名，或者将来自`merge`的提交说明中的含有“Conflicts:”的行去掉。

### 8.2.2.1.6\. commit-msg

该钩子脚本由**git commit**命令调用，可以通过传递`--no-verify`参数而禁用。该脚本有一个参数，即包含有提交说明的文件的文件名。如果该脚本运行失败（返回非零值），Git 提交被终止。

该脚本可以直接修改提交说明，可以用于对提交说明规范化以符合项目的标准（如果有的话）。如果提交说明不符合标准，可以拒绝提交。

Git 提供的示例脚本`commit-msg.sample`检查提交说明中出现的相同的`Signed-off-by`行，如果发现重复签名即报错、终止提交。

Gerrit 服务器需要每一个向其进行推送的 Git 版本库在本地使用 Gerrit 提供的`commit-msg`钩子脚本，以便在创建的提交中包含形如“Change-Id: I...”的变更集标签。

### 8.2.2.1.7\. post-commit

该钩子脚本由**git commit**命令调用，不带参数运行，在提交完成之后被触发执行。

该钩子脚本不会影响**git commit**的运行结果，可以用于发送通知。

### 8.2.2.1.8\. pre-rebase

该钩子脚本由**git rebase**命令调用，用于防止某个分支参与变基。

Git 提供的示例脚本`pre-rebase.sample`是针对 Git 项目自身情况而开发的，当一个功能分支已经合并到`next`分支后，禁止该分基进行变基操作。

### 8.2.2.1.9\. post-checkout

该钩子脚本由**git checkout**命令调用，是在完成工作区更新之后触发执行。该钩子脚本有三个参数：前一个 HEAD 的引用，新 HEAD 的引用（可能和前一个一样也可能不一样），以及一个用于表示此次检出是否是分支检出的标识（分支检出为 1，文件检出是 0）。该钩子脚本不会影响**git checkout**命令的结果。

除了由**git checkout**命令调用外，该钩子脚本也在**git clone**命令执行后被触发执行，除非在克隆时使用了禁止检出的`--no-checkout (-n)`参数。在由**git clone**调用时，第一个参数给出的引用是空引用，则第二个和第三个参数都为 1。

这个钩子一般用于版本库的有效性检查，自动显示和前一个 HEAD 的差异，或者设置工作区属性。

### 8.2.2.1.10\. post-merge

该钩子脚本由**git merge**命令调用，当在本地版本库完成**git pull**操作后触发执行。该钩子脚本有一个参数，标识合并是否是一个压缩合并。该钩子脚本不会影响**git merge**命令的结果。如果合并因为冲突而失败，该脚本不会执行。

该钩子脚本可以与`pre-commit`钩子脚本一起实现对工作区目录树属性（如权限/属主/ACL 等）的保存和恢复。参见 Git 源码文件`contrib/hooks/setgitperms.perl`中的示例。

### 8.2.2.1.11\. pre-receive

该钩子脚本由远程版本库的**git receive-pack**命令调用。当从本地版本库完成一个推送之后，在远程服务器上开始批量更新引用之前，该钩子脚本被触发执行。该钩子脚本的退出状态决定了更新引用的成功与否。

该钩子脚本在接收（receive）操作中只执行一次。传递参数不通过命令行，而是通过标准输入进行传递。通过标准输入传递的每一行的语法格式为：

```
<old-value> <new-value> <ref-name>

```

`<old-value>`是引用更新前的老的对象 ID，`<new-value>`是引用即将更新到的新的对象 ID，`<ref-name>`是引用的全名。当创建一个新引用时，`<old-value>`是 40 个 0。

如果该钩子脚本以非零值退出，一个引用也不会更新。如果该脚本正常退出，每一个单独的引用的更新仍有可能被`update`钩子所阻止。

标准输出和标准错误都重定向到在另外一端执行的**git send-pack**，所以可以直接通过**echo**命令向用户传递信息。

### 8.2.2.1.12\. update

该钩子脚本由远程版本库的**git receive-pack**命令调用。当从本地版本库完成一个推送之后，在远程服务器上更新引用时，该钩子脚本被触发执行。该钩子脚本的退出状态决定了更新引用的成功与否。

该钩子脚本在每一个引用更新的时候都会执行一次。该脚本有三个参数。

*   参数 1：要更新的引用的名称。
*   参数 2：引用中保存的旧对象名称。
*   参数 3：将要保存到引用中的新对象名称。

正常退出（返回 0）允许引用的更新，而以非零值退出禁止**git-receive-pack**更新该引用。

该钩子脚本可以用于防止对某些引用的强制更新，因为该脚本可以通过检查新旧引用对象是否存在继承关系，从而提供更为细致的“非快进式推送”的授权。

该钩子脚本也可以用于记录（如用邮件）引用变更历史`old..new`。然而因为该脚本不知道整个的分支，所以可能会导致每一个引用发送一封邮件。因此如果要发送通知邮件，可能`post-receive`钩子脚本更适合。

另外，该脚本可以实现基于路径的授权。

标准输出和标准错误都重定向到在另外一端执行的**git send-pack**，所以可以直接通过**echo**命令向用户传递信息。

Git 提供的示例脚本`update.sample`展示了对多种危险的 Git 操作行为进行控制的可行性。

*   只有将配置变量`hooks.allowunannotated`设置为`true`才允许推送轻量级里程碑（不带说明的里程碑）。
*   只有将配置变量`hooks.allowdeletebranch`设置为`true`才允许删除分支。
*   如果将配置变量`hooks.denycreatebranch`设置为`true`则不允许创建新分支。
*   只有将配置变量`hooks.allowdeletetag`设置为`true`才允许删除里程碑。
*   只有将配置变量`hooks.allowmodifytag`设置为`true`才允许修改里程碑。

相比 Git 的示例脚本，Gitolite 服务器为其管理的版本库设置的`update`钩子脚本更实用也更强大。Gitolite 实现了用户认证，并通过检查授权文件，实现基于分支和路径的写操作授权，等等。具体参见本书第 5 篇“第三十章 Gitolite 服务架设”相关内容。

### 8.2.2.1.13\. post-receive

该钩子脚本由远程版本库的**git receive-pack**命令调用。当从本地版本库完成一个推送，并且在远程服务器上所有引用都更新完毕后，该钩子脚本被触发执行。

该钩子脚本在接收（receive）操作中只执行一次。该脚本不通过命令行传递参数，但是像`pre-receive`钩子脚本那样，通过标准输入以相同格式获取信息。

该钩子脚本不会影响`git-receive-pack`的结果，因为调用该脚本时工作已经完成。

该钩子脚本胜过`post-update`脚本之处在于可以获得所有引用的老的和新的值，以及引用的名称。

标准输出和标准错误都重定向到在另外一端执行的**git send-pack**，所以可以直接通过**echo**命令向用户传递信息。

Git 提供的示例脚本`post-receive.sample`引入了`contrib/hooks`目录下的名为`post-receive-email`的示例脚本（默认被注释），以实现发送通知邮件的功能。

Gitolite 服务器要对其管理的 Git 版本库设置`post-receive`钩子脚本，以实现当版本库有变更后进行到各个镜像版本库的数据传输。

### 8.2.2.1.14\. post-update

该钩子脚本由远程版本库的**git receive-pack**命令调用。当从本地版本库完成一个推送之后，即当所有引用都更新完毕后，在远程服务器上该钩子脚本被触发执行。

该脚本接收不定长的参数，每一个参数实际上就是已成功更新的引用名。

该钩子脚本不会影响**git-receive-pack**的结果，因此主要用于通知。

钩子脚本`post-update`虽然能够提供那些引用被更新了，但是该脚本不知道引用更新前后的对象 SHA1 哈希值，所以在这个脚本中不能记录形如`old..new`的引用变更范围。而钩子脚本`post-receive`知道更新引用前后的对象 ID，因此更适合此种场合。

标准输出和标准错误都重定向到在另外一端执行的**git send-pack**，所以可以直接通过**echo**命令向用户传递信息。

Git 提供的示例脚本`post-update.sample`会运行**git update-server-info**命令，以更新哑协议需要的索引文件。如果通过哑协议共享版本库，应该启用该钩子脚本。

### 8.2.2.1.15\. pre-auto-gc

该钩子脚本由**git gc --auto**命令调用，不带参数运行，如果以非零值退出会导致**git gc --auto**被中断。

### 8.2.2.1.16\. post-rewrite

该钩子脚本由一些重写提交的命令调用，如**git commit --amend**、**git rebase**，而**git-filter-branch**当前尚未调用该钩子脚本。

该脚本的第一个参数用于判断调用来自哪个命令，当前有`amend`和`rebase`两个取值，也可能将来会有其他更多命令相关参数传递。

该脚本通过标准输入接收一个重写提交列表，每一行输入的格式如下：

```
<old-sha1> <new-sha1> [<extra-info>]

```

前两个是旧的和新的对象 SHA1 哈希值。而`<extra-info>`参数是和调用命令相关的，而当前该参数为空。

## 8.2.2.2\. Git 模板

当执行**git init**或**git clone**创建版本库时，会自动在版本库中创建钩子脚本（`.git/hooks/*`）、忽略文件（`.git/info/exclude`）及其他文件，实际上这些文件均拷贝自模板目录。如果需要本地版本库使用定制的钩子脚本等文件，直接在模板目录内创建（文件或符号链接）会事半功倍。

Git 按照以下列顺序第一个确认的路径即为模板目录。

*   如果执行**git init**或**git clone**命令时，提供`--template=<DIR>`参数，则使用指定的目录作为模板目录。

*   由环境变量`$GIT_TEMPLATE_DIR`指定的模板目录。

*   由 Git 配置变量`init.templatedir`指定的模板目录。

*   缺省的模板目录，根据 Git 安装路径的不同可能位于不同的目录下。可以通过下面命令确认其实际位置：

    ```
    $ ( cd $(git --html-path)/../../git-core/templates; pwd )
    /usr/share/git-core/templates

    ```

如果在执行版本库初始化时传递了空的模板路径，则不会在版本库中创建钩子脚本 等文件。

```
$ git init --template= no-template
Initialized empty Git repository in /path/to/my/workspace/no-template/.git/

```

执行下面的命令，查看新创建的版本库`.git`目录下的文件。

```
$ ls -F no-template/.git/
HEAD     config   objects/ refs/

```

可以看到不使用模板目录创建的版本库下面的文件少的可怜。而通过对模板目录下的文件的定制，可以实现在建立的版本库中包含预先设置好的钩子脚本、忽略文件、属性文件等。这对于服务器或者对版本库操作有特殊要求的项目带来方便。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 8.2.3\. 稀疏检出和浅克隆

## 8.2.3.1\. 稀疏检出

从 1.7.0 版本开始 Git 提供稀疏检出的功能。所谓稀疏检出就是本地版本库检出时不检出全部，只将指定的文件从本地版本库检出到工作区，而其他未指定的文件则不予检出（即使这些文件存在于工作区，其修改也会被忽略）。

要想实现稀疏检出的功能，必须同时设置`core.sparseCheckout`配置变量，并存在文件`.git/info/sparse-checkout`。即首先要设置 Git 配置变量`core.sparseCheckout`为`true`，然后编辑`.git/info/sparse-checkout`文件，将要检出的目录或文件的路径写入其中。其中文件`.git/info/sparse-checkout`的格式就和`.gitignore`文件格式一样，路径可以使用通配符。

稀疏检出是如何实现的呢？实际上 Git 在 index（即暂存区）中为每个文件提供一个名为`skip-worktree`标志位，缺省这个标识位处于关闭状态。如果该标识位开启，则无论工作区对应的文件存在与否，或者是否被修改，Git 都认为工作区该文件的版本是最新的、无变化。Git 通过配置文件`.git/info/sparse-checkout`定义一个要检出的目录和/或文件列表，当前 Git 的**git read-tree**命令及其他基于合并的命令（**git merge**，**git checkout**等等）能够根据该配置文件更新 index 中文件的`skip-worktree`标志位，实现版本库文件的稀疏检出。

先来在工作区`/path/to/my/workspace`中创建一个示例版本库 sparse1，创建后的 sparse1 版本库中包含如下内容：

```
$ ls -F
doc1/ doc2/ doc3/
$ git ls-files -s -v
H 100644 ce013625030ba8dba906f756967f9e9ca394464a 0     doc1/readme.txt
H 100644 ce013625030ba8dba906f756967f9e9ca394464a 0     doc2/readme.txt
H 100644 ce013625030ba8dba906f756967f9e9ca394464a 0     doc3/readme.txt

```

即版本库 sparse1 中包含三个目录`doc1`、`doc2`和`doc3`。命令**git ls-files**的`-s`参数用于显示对象的 SHA1 哈希值以及所处的暂存区编号。而`-v`参数则还会显示工作区文件的状态，每一行命令输出的第一个字符即是文件状态：字母`H`表示文件已被暂存，如果是字母`S`则表示该文件`skip-worktree`标志位已开启。

下面我们就来体验一下稀疏检出的功能。

*   修改版本库的 Git 配置变量`core.sparseCheckout`，将其设置为`true`。

    ```
    $ git config core.sparseCheckout true

    ```

*   设置`.git/info/sparse-checkout`的内容，如下：

    ```
    $ printf "doc1\ndoc3\n" > .git/info/sparse-checkout
    $ cat .git/info/sparse-checkout
    doc1
    doc3

    ```

*   执行**git checkout**命令后，会发现工作区中`doc2`目录不见了。

    ```
    $ git checkout
    $ ls -F
    doc1/ doc3/

    ```

*   这时如果用**git ls-files**命令查看，会发现`doc2`目录下的文件被设置了`skip-worktree`标志。

    ```
    $ git ls-files -v
    H doc1/readme.txt
    S doc2/readme.txt
    H doc3/readme.txt

    ```

*   修改`.git/info/sparse-checkout`的内容，如下：

    ```
    $ printf "doc3\n" > .git/info/sparse-checkout
    $ cat .git/info/sparse-checkout
    doc3

    ```

*   执行**git checkout**命令后，会发现工作区中`doc1`目录也不见了。

    ```
    $ git checkout
    $ ls -F
    doc3/

    ```

*   这时如果用**git ls-files**命令查看，会发现`doc1`和`doc2`目录下的文件都被设置了`skip-worktree`标志。

    ```
    $ git ls-files -v
    S doc1/readme.txt
    S doc2/readme.txt
    H doc3/readme.txt

    ```

*   修改`.git/info/sparse-checkout`的内容，使之包含一个星号，即在工作区检出所有的内容。

    ```
    $ printf "*\n" > .git/info/sparse-checkout
    $ cat .git/info/sparse-checkout
    *

    ```

*   执行**git checkout**，会发现所有目录又都回来了。

    ```
    $ git checkout
    $ ls -F
    doc1/ doc2/ doc3/

    ```

文件`.git/info/sparse-checkout`的文件格式类似于`.gitignore`的格式，也支持用感叹号实现反向操作。例如不检出目录`doc2`下的文件，而检出其他文件，可以使用下面的语法（注意顺序不能写反）：

```
*
!doc2/

```

注意如果使用命令**git checkout -- <file>...**，即不是切换分支而是用分支中的文件替换暂存区和工作区的话，则忽略`skip-worktree`标志。例如下面的操作中，虽然`doc2`被设置为不检出，但是执行**git checkout .**命令后，还是所有的目录都被检出了。

```
$ git checkout .
$ ls -F
doc1/ doc2/ doc3/
$ git ls-files -v
H doc1/readme.txt
S doc2/readme.txt
H doc3/readme.txt

```

如果修改`doc2`目录下的文件，或者在`doc2`目录下添加新文件，Git 会视而不见。

```
$ echo hello >> doc2/readme.txt
$ git status
# On branch master
nothing to commit (working directory clean)

```

若此时通过取消`core.sparseCheckout`配置变量的设置而关闭稀疏检出，也不会改变目录`doc2`下的文件的`skip-worktree`标志。这种情况或者通过**git update-index --no-skip-worktree -- <file>...**来更改 index 中对应文件的`skip-worktree`标志，或者重新启用稀疏检出更改相应文件的检出状态。

在克隆一个版本库时只希望检出部分文件或目录，可以在执行克隆操作的时候使用`--no-checkout`或`-n`参数，不进行工作区文件的检出。例如下面的操作从前面示例的 sparse1 版本库克隆到 sparse2 中，不进行工作区文件的检出。

```
$ git clone -n sparse1 sparse2
Cloning into sparse2...
done.

```

检出完成后可以发现 sparse2 的工作区是空的，而且版本库中也不存在`index`文件。如果执行**git status**命令会看到所有文件都被标识为删除。

```
$ cd sparse2
$ git status -s
D  doc1/readme.txt
D  doc2/readme.txt
D  doc3/readme.txt

```

如果希望通过稀疏检出的功能，只检出其中一个目录如`doc2`，可以用如下方法实现：

```
$ git config core.sparseCheckout true
$ printf "doc2\n" > .git/info/sparse-checkout
$ git checkout

```

之后看到工作区中检出了`doc2`目录，而其他文件被设置了`skip-worktree`标志。

```
$ ls -F
doc2/
$ git ls-files -v
S doc1/readme.txt
H doc2/readme.txt
S doc3/readme.txt

```

## 8.2.3.2\. 浅克隆

上一节介绍的稀疏检出，可以部分检出版本库中的文件，但是版本库本身仍然包含所有的文件和历史。如果只对一个大的版本库的最近的部分历史提交感兴趣，而不想克隆整个版本库，稀疏检出是解决不了的，而是要采用本节介绍的浅克隆。

实现版本库的浅克隆的非常简单，只需要在执行**git clone**或者**git fetch**操作时用`--depth <depth>`参数设定要获取的历史提交的深度（`<depth>`大于 0），就会把源版本库分支上最近的`<depth> + 1`个历史提交作为新版本库的全部历史提交。

通过浅克隆方式克隆出来的版本库，每一个提交的 SHA1 哈希值和源版本库的相同，包括提交的根节点也是如次，但是 Git 通过特殊的实现，使得浅克隆的根节点提交看起来没有父提交。正因为浅克隆的提交对象的 SHA1 哈希值和源版本库一致，所以浅克隆版本库可以执行**git fetch**或者**git pull**从源版本库获取新的提交。但是浅克隆版本库也存在着很多限制，如：

*   不能从浅克隆版本库克隆出新的版本库。
*   其他版本库不能从浅克隆获取提交。
*   其他版本库不能推送提交到浅克隆版本库。
*   不要从浅克隆版本库推送提交至其他版本库，除非确认推送的目标版本库包含浅克隆版本库中缺失的全部历史提交，否则会造成目标版本库包含不完整的提交历史导致版本库无法操作。
*   在浅克隆版本库中执行合并操作时，如果所合并的提交出现在浅克隆历史中，则可以顺利合并，否则会出现大量的冲突，就好像和无关的历史进行合并一样。

由于浅克隆包含上述限制，因此浅克隆一般用于对远程版本库的查看和研究，如果在浅克隆版本库中进行了提交，最好通过**git format-patch**命令导出为补丁文件再应用到远程版本库中。

下面的操作使用**git clone**命令创建一个浅克隆。注意：源版本库如果是本地版本库要使用`file://`协议，若直接接使用本地路径则不会实现浅克隆。

```
$ git clone --depth 2 file:///path/to/repos/hello-world.git shallow1

```

然后进入到本地克隆目录中，会看到当前分支上只有 3 个提交。

```
$ git log  --oneline
c4acab2 Translate for Chinese.
683448a Add I18N support.
d81896e Fix typo: -help to --help.

```

查看提交的根节点`d81896e`，则会看到该提交实际上也包含父提交。

```
$ git cat-file -p HEAD^^
tree f9d7f6b0af6f3fffa74eb995f1d781d3c4876b25
parent 10765a7ef46981a73d578466669f6e17b73ac7e3
author user1 <user1@sun.ossxp.com> 1294069736 +0800
committer user2 <user2@moon.ossxp.com> 1294591238 +0800

Fix typo: -help to --help.

```

而查看该提交的父提交，Git 会报错。

```
$ git log 10765a7ef46981a73d578466669f6e17b73ac7e3
fatal: bad object 10765a7ef46981a73d578466669f6e17b73ac7e3

```

对于正常的 Git 版本库来说，如果对象库中一个提交丢失绝对是大问题，版本库不可能被正常使用。而浅克隆之所以看起来一切正常，是因为 Git 使用了类似嫁接（下一节即将介绍）的技术。

在浅克隆版本库中存在一个文件`.git/shallow`，这个文件中罗列了应该被视为提交根节点的提交 SHA1 哈希值。查看这个文件会看到提交`d81896e`正在其中：

```
$ cat .git/shallow
b56bb510a947651e4717b356587945151ac32166
d81896e60673771ef1873b27a33f52df75f70515
e64f3a216d346669b85807ffcfb23a21f9c5c187

```

列在`.git/shallow`文件中的提交会构建出对应的嫁接提交，使用类似嫁接文件`.git/info/grafts`（下节讨论）的机制，当 Git 访问这些对象时就好像这些对象是没有父提交的根节点一样。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 8.2.4\. 嫁接和替换

## 8.2.4.1\. 提交嫁接

提交嫁接可以实现在本地版本库上将两条完全不同提交线（分支）嫁接（连接）到一起。对于一些项目将版本控制系统迁移到 Git 上，该技术会非常有帮助。例如 Linux 本身的源代码控制在转移到 Git 上时，尚没有任何工具可以将 Linux 的提交历史从旧的 Bitkeeper 版本控制系统中导出，后来 Linux 旧的代码通过 bkcvs 导入到 Git 中，如何将新旧两条开发线连接到一起呢？于是发明了提交嫁接，实现新旧两条开发线的合并，这样 Linux 开发者就可以在一个开发分支中由最新的提交追踪到原来位于 Bitkeeper 中的提交。（参考：[`git.wiki.kernel.org/index.php/GraftPoint`](https://git.wiki.kernel.org/index.php/GraftPoint)）

提交嫁接是通过在版本库中创建`.git/info/grafts`文件实现的。该文件每一行的格式为：

```
<commit sha1> <parent sha1> [<parent sha1>]*

```

用空格分开各个字段，其中第一个字段是一个提交的 SHA1 哈希值，而后面用空格分开的其他 SHA1 哈希值则作为该提交的父提交。当把一个提交线的根节点作为第一个字段，将第二个提交线顶节点作为第二个字段，就实现了两个提交线的嫁接，看起来像是一条提交线了。

在本书第 6 篇“第 35.4 节 Git 版本库整理”中介绍的**git filter-branch**命令在对版本库整理时，如果发现存在`.git/info/grafts`则会在物理上完成提交的嫁接，实现嫁接的永久生效。

## 8.2.4.2\. 提交替换

提交替换是在 1.6.5 或更新版本的 Git 提供的功能，和提交嫁接类似，不过提交替换不是用一个提交来伪成装另外一个提交的父提交，而是直接替换另外的提交，实现在不影响其他提交的基础上实现对历史提交的修改。

提交替换是通过在特殊命名空间`.git/refs/replace/`下定义引用来实现的。引用的名称是要被替换掉的提交 SHA1 哈希值，而引用文件的内容（引用所指向的提交）就是用于替换的（正确的）提交 SHA1 哈希值。由于提交替换通过引用进行定义，因此可以在不同的版本库之间传递，而不像提交嫁接只能在本地版本库中使用。

Git 提供**git replace**命令来管理提交替换，用法如下：

```
用法 1： git replace [-f] <object> <replacement>
用法 2： git replace -d <object>...
用法 3： git replace -l [<pattern>]

```

其中：

*   用法 1 用于创建提交替换，即在`.git/refs/replace`目录下创建名为`<object>`的引用，其内容为`<replacement>`。如果使用`-f`参数，还允许级联替换，用于替换的提交来可以是另外一个已经在`.git/refs/replace`中定义的替换。
*   用法 2 用于删除已经定义的替换。
*   用法 3 显示已经存在的提交替换。

提交替换可以被大部分 Git 命令理解，除了一些针对被替换的提交使用`--no-replace-objects`参数的命令。例如：

*   当提交`foo`被提交`bar`替换后，显示未被替换前的`foo`提交：

    ```
    $ git --no-replace-objects cat-file commit foo
    ...foo 的内容...

    ```

*   不使用`--no-replace-objects`参数，则访问`foo`会显示替换后的`bar`提交：

    ```
    $ git cat-file commit foo
    ...bar 的内容...

    ```

提交替换使用引用进行定义，因此可以通过**git fetch**和**git push**在版本库之间传递。但因为默认 Git 只同步里程碑和分支，因此需要在命令中显式的给出提交替换的引用表达式，如：

```
$ git fetch origin refs/replace/*
$ git push  origin refs/replace/*

```

提交替换也可以实现两个分支的嫁接。例如要将分支 A 嫁接到 B 上，就相当于将分支 A 的根提交`<BRANCH_A_ROOT>`的父提交设置为分支 B 的最新提交`<BRANCH_B_CURRENT>`。可以先创建一个新提交`<BRANCH_A_NEW_ROOT>`，其父提交设置为`<BRANCH_B_CURRENT>`而提交的其他字段完全和`<BRANCH_A_ROOT>`一致。然后设置提交替换，用`<BRANCH_A_NEW_ROOT>`替换`<BRANCH_A_ROOT>`即可。

创建`<BRANCH_A_NEW_ROOT>`可以使用下面的命令，注意要用实际值替换下面命令中的`<BRANCH_A_ROOT>`和`<BRANCH_B_CURRENT>`。

```
$ git cat-file commit <BRANCH_A_ROOT> |
  sed -e "/^tree / a \
          parent $(git rev-parse <BRANCH_B_CURRENT>)" |
  git hash-object -t commit -w --stdin

```

其中**git cat-file commit**命令用于显示提交的原始信息，**sed**命令用于向原始提交中插入一条`parent SHA1...`的语句，而命令**git hash-object**是一个 Git 底层命令，可以将来自标准输入的内容创建一个新的提交对象。

上面命令的输出即是`<BRANCH_A_NEW_ROOT>`的值。执行下面的替换命令，完成两个分支的嫁接。

```
$ git replace <BRANCH_A_ROOT> <BRANCH_A_NEW_ROOT>

```

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 8.2.5\. Git 评注

从 1.6.6 版本开始，Git 提供了一个**git notes**命令可以为提交添加评注，实现在不改变提交对象的情况下在提交说明的后面附加评注。图 41-1 展示了 Github（[`github.com/ossxp-com/gitdemo-commit-tree/commit/6652a0dce6a5067732c00ef0a220810a7230655e`](https://github.com/ossxp-com/gitdemo-commit-tree/commit/6652a0dce6a5067732c00ef0a220810a7230655e)）利用**git notes**实现的在提交显示界面中显示评注（如果存在的话）和添加评注的界面。

![../_images/github-notes.png](img/github-notes.png)

图 41-1：Github 上显示和添加评注

## 8.2.5.1\. 评注的奥秘

实际上 Git 评注可以针对任何对象，而且评注的内容也不限于文字，因为评注的内容是保存在 Git 对象库中的一个 blob 对象中。不过评注目前最主要的应用还是在提交说明后添加文字评注。

在第 2 篇“第 11.4.6 节二分查找”中用到的`gitdemo-commit-tree`版本库实际上就包含了提交评注，只不过之前尚未将评注获取到本地版本库而已。如果工作区中的`gitdemo-commit-tree`版本库已经不存在，可以使用下面的命令从 GitHub 上再克隆一个：

```
$ git clone -q git://github.com/ossxp-com/gitdemo-commit-tree.git
$ cd gitdemo-commit-tree

```

执行下面的命令，查看最后一次提交的提交说明：

```
$ git log -1
commit 6652a0dce6a5067732c00ef0a220810a7230655e
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Thu Dec 9 16:07:11 2010 +0800

    Add Images for git treeview.

    Signed-off-by: Jiang Xin <jiangxin@ossxp.com>

```

下面为默认的 origin 远程版本库再添加一个引用获取表达式，以便在执行**git fetch**命令时能够同步评注相关的引用。命令如下：

```
$ git config --add remote.origin.fetch refs/notes/*:refs/notes/*

```

执行**git fetch**会获取到一个新的引用`refs/notes/commits`，如下：

```
$ git fetch
remote: Counting objects: 6, done.
remote: Compressing objects: 100% (5/5), done.
remote: Total 6 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (6/6), done.
From git://github.com/ossxp-com/gitdemo-commit-tree
 * [new branch]      refs/notes/commits -> refs/notes/commits

```

当获取到新的评注相关的引用之后，再来查看最后一次提交的提交说明。下面的命令输出中提交说明的最后两行就是附加的提交评注。

```
$ git log -1
commit 6652a0dce6a5067732c00ef0a220810a7230655e
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Thu Dec 9 16:07:11 2010 +0800

    Add Images for git treeview.

    Signed-off-by: Jiang Xin <jiangxin@ossxp.com>

Notes:
    Bisect test: Bad commit, for doc/B.txt exists.

```

附加的提交评注来自于哪里呢？显然应该和刚刚获取到的引用相关。查看一下获取到的最新引用，会发现引用`refs/notes/commits`指向的是一个提交对象。

```
$ git show-ref refs/notes/commits
6f01cdc59004892741119318ceb2330d6dc0cef1 refs/notes/commits
$ git cat-file -t refs/notes/commits
commit

```

既然新获取的评注引用是一个提交对象，那么就应该能够查看评注引用的提交日志：

```
$ git log --stat refs/notes/commits
commit 6f01cdc59004892741119318ceb2330d6dc0cef1
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Tue Feb 22 09:32:10 2011 +0800

    Notes added by 'git notes add'

 6652a0dce6a5067732c00ef0a220810a7230655e |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)

commit 9771e1076d2218922acc9800f23d5e78d5894a9f
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Tue Feb 22 09:31:54 2011 +0800

    Notes added by 'git notes add'

 e80aa7481beda65ae00e35afc4bc4b171f9b0ebf |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)

```

从上面的评注引用的提交日志可以看出，存在两次提交，并且从提交说明可以看出是使用**git notes add**命令添加的。至于每次提交添加的文件却很让人困惑，所添加文件的文件名居然是 40 位的哈希值。

您当然可以通过**git checkout -b**命令检出该引用来研究其中所包含的文件，不过也可以运用我们已经学习到的 Git 命令直接对其进行研究。

*   用**git show**命令显示目录树。

    ```
    $ git show -p refs/notes/commits^{tree}
    tree refs/notes/commits^{tree}

    6652a0dce6a5067732c00ef0a220810a7230655e
    e80aa7481beda65ae00e35afc4bc4b171f9b0ebf

    ```

*   用**git ls-tree**命令查看文件大小及对应的 blob 对象的 SHA1 哈希值。

    ```
    $ git ls-tree -l refs/notes/commits
    100644 blob 80b1d249069959ce5d83d52ef7bd0507f774c2b0      47    6652a0dce6a5067732c00ef0a220810a7230655e
    100644 blob e894f2164e77abf08d95d9bdad4cd51d00b47845      56    e80aa7481beda65ae00e35afc4bc4b171f9b0ebf

    ```

*   文件名既然是一个 40 位的 SHA1 哈希值，那么文件名一定有意义，通过下面的命令可以看到文件名包含的 40 位哈希值实际对应于一个提交。

    ```
    $ git cat-file -p 6652a0dce6a5067732c00ef0a220810a7230655e
    tree e33be9e8e7ca5f887c7d5601054f2f510e6744b8
    parent 81993234fc12a325d303eccea20f6fd629412712
    author Jiang Xin <jiangxin@ossxp.com> 1291882031 +0800
    committer Jiang Xin <jiangxin@ossxp.com> 1291882892 +0800

    Add Images for git treeview.

    Signed-off-by: Jiang Xin <jiangxin@ossxp.com>

    ```

*   用**git cat-file**命令查看该文件的内容，可以看到其内容就是附加在相应提交上的评注。

    ```
    $ git cat-file -p refs/notes/commits:6652a0dce6a5067732c00ef0a220810a7230655e
    Bisect test: Bad commit, for doc/B.txt exists.

    ```

综上所述，评注记录在一个 blob 对象中，并且以所评注对象的 SHA1 哈希值命名。因为对象 SHA1 哈希值的唯一性，所以可以将评注都放在同一个文件系统下而不会相互覆盖。针对这个包含所有评注的特殊的文件系统的更改被提交到一个特殊的引用`refs/notes/commits`当中。

## 8.2.5.2\. 评注相关命令

Git 提供了**git notes**命令，对评注进行管理。如果执行**git notes list**或者像下面这样不带任何参数进行调用，会显示和上面**git ls-tree**类似的输出：

```
$ git notes
80b1d249069959ce5d83d52ef7bd0507f774c2b0 6652a0dce6a5067732c00ef0a220810a7230655e
e894f2164e77abf08d95d9bdad4cd51d00b47845 e80aa7481beda65ae00e35afc4bc4b171f9b0ebf

```

右边的一列是要评注的提交对象，而左边一列是附加在对应提交上的包含评注内容的 blob 对象。显示附加在某个提交上的评注可以使用**git notes show**命令。如下：

```
$ git notes show G⁰
Bisect test: Good commit, for doc/B.txt does not exist.

```

注意上面的命令中使用`G⁰`而非`G`，是因为`G`是一个里程碑对象，而评注是建立在由里程碑对象所指向的一个提交对象上。

添加评注可以使用下面的**git notes add**和**git notes append**子命令：

```
用法 1：git notes add [-f] [-F <file> | -m <msg> | (-c | -C) <object>] [<object>]
用法 2：git notes append [-F <file> | -m <msg> | (-c | -C) <object>] [<object>]

```

用法 1 是添加评注，而用法 2 是在已有评注后面追加。两者的命令行格式和**git commit**非常类似，可以用类似写提交说明的方法写提交评注。如果省略最后一个`<object>`参数，则意味着向头指针 HEAD 添加评注。子命令**git notes add**中的参数`-f`意味着强制添加，会覆盖对象已有的评注。

使用**git notes copy**子命令可以将一个对象的评注拷贝到另外一个对象上。

```
用法：git notes copy [-f] ( --stdin | <from-object> <to-object> )

```

修改评注可以使用下面的**git notes edit**子命令：

```
用法：git notes edit [<object>]

```

删除评注可以使用的**git notes remote**子命令，而**git notes prune**则可以清除已经不存在的对象上的评注。用法如下：

```
用法 1：git notes remove [<object>]
用法 2：git notes prune [-n | -v]

```

评注以文件形式保存在特殊的引用中，如果该引用被共享并且同时有多人撰写评注时，有可能出现该引用的合并冲突。可以用**git notes merge**命令来解决合并冲突。评注引用也可以使用其他的引用名称，合并其他的评注引用也可以使用本命令。下面是**git notes merge**命令的语法格式，具体操作参见**git help notes**帮助。

```
用法 1：git notes merge [-v | -q] [-s <strategy> ] <notes_ref>
用法 2：git notes merge --commit [-v | -q]
用法 3：git notes merge --abort [-v | -q]

```

## 8.2.5.3\. 评注相关配置

默认提交评注保存在引用`refs/notes/commits`中，这个默认的设置可以通过`core.notesRef`配置变量修改。如须更改，要在`core.notesRef`配置变量中使用引用的全称而不能使用缩写。

在执行**git log**命令显示提交评注的时候，如果配置了`notes. displayRef`配置变量（可以使用通配符，并且可以配置多个），则在显示提交评注时，除了会参考`core.notesRef`设定的引用（或默认的`refs/notes/commits`引用）外，还会参考`notes.displayRef`指向的引用（一个或多个）来显示评注。

配置变量`notes.rewriteRef`用于配置哪个/哪些引用中的提交评注会随着提交的修改而复制到新的提交之上。这个配置变量可以使用多次，或者使用通配符，但该配置变量没有缺省值，因此为了使得提交评注能够随着提交的修改（修补提交、变基等）继续保持，必须对该配置变量进行设定。如：

```
$ git config --global notes.rewriteRef refs/notes/*

```

还有`notes.rewrite.amend`和`notes.rewrite.rebase`配置变量可以分别对两种提交修改模式（amend 和 rebase）是否启用评注复制进行设置，默认启用。配置变量`notes.rewriteMode`默认设置为`concatenate`，即提交评注复制到修改后的提交时，如果已有评注则对评注进行合并操作。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.