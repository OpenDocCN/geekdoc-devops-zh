# 3\. Git 和声

上一篇的各个章节是从个人的视角研究和使用 Git，通过连续的实践不但学习了 Git 的基本用法，还深入地了解了 Git 的奥秘，这些都将成为学习本篇内容的基础。从本章开始，不再是一个人的独奏，而是多人的和声，我们将从团队的视角对 Git 进行研究，要知道 Git 作为版本控制系统，其主要工作就是团队协作。

团队协作和个人之间有何不同？关键就在于团队成员之间存在着数据交换：

*   数据交换需要协议，就是本章要介绍的内容。
*   数据交换可能会因为冲突造成中断，第十六章将专题介绍如何解决冲突。
*   里程碑为数据建立标识，是数据交换的参照点，这将在第十七章中介绍。
*   分支会为数据交换开辟不同的通道，从而减少冲突和混乱的发生，第十八章会系统地介绍不同的分支应用模式。
*   与远程版本库进行数据交换，是 Git 协同的主要手段，这将在第十九章中介绍。
*   本篇的最后（第二十章）会介绍在不能直接进行版本库交互的情况下，如何使用补丁文件的方式进行数据交换。

目录:

*   3.1\. Git 协议与工作协同
    *   3.1.1\. Git 支持的协议
    *   3.1.2\. 多用户协同的本地模拟
    *   3.1.3\. 强制非快进式推送
    *   3.1.4\. 合并后推送
    *   3.1.5\. 禁止非快进式推送
*   3.2\. 冲突解决
    *   3.2.1\. 拉回操作中的合并
    *   3.2.2\. 合并一：自动合并
        *   3.2.2.1\. 修改不同的文件
        *   3.2.2.2\. 修改相同文件的不同区域
        *   3.2.2.3\. 同时更改文件名和文件内容
    *   3.2.3\. 合并二：逻辑冲突
    *   3.2.4\. 合并三：冲突解决
        *   3.2.4.1\. 手工编辑完成冲突解决
        *   3.2.4.2\. 图形工具完成冲突解决
    *   3.2.5\. 合并四：树冲突
        *   3.2.5.1\. 手工操作解决树冲突
        *   3.2.5.2\. 交互式解决树冲突
    *   3.2.6\. 合并策略
    *   3.2.7\. 合并相关的设置
*   3.3\. Git 里程碑
    *   3.3.1\. 显示里程碑
        *   3.3.1.1\. 1\. 命令**git tag**
        *   3.3.1.2\. 2\. 命令**git log**
        *   3.3.1.3\. 3\. 命令**git describe**
        *   3.3.1.4\. 4\. 命令**git name-rev**
    *   3.3.2\. 创建里程碑
        *   3.3.2.1\. 轻量级里程碑
        *   3.3.2.2\. 带说明的里程碑
        *   3.3.2.3\. 带签名的里程碑
    *   3.3.3\. 删除里程碑
    *   3.3.4\. 不要随意更改里程碑
    *   3.3.5\. 共享里程碑
    *   3.3.6\. 删除远程版本库的里程碑
    *   3.3.7\. 里程碑命名规范
*   3.4\. Git 分支
    *   3.4.1\. 代码管理之殇
        *   3.4.1.1\. 发布分支
        *   3.4.1.2\. 特性分支
        *   3.4.1.3\. 卖主分支
    *   3.4.2\. 分支命令概述
    *   3.4.3\. Hello World 开发计划
    *   3.4.4\. 基于特性分支的开发
        *   3.4.4.1\. 创建分支`user1/getopt`
        *   3.4.4.2\. 创建分支`user2/i18n`
        *   3.4.4.3\. 开发者 user1 完成功能开发
        *   3.4.4.4\. 将`user1/getopt`分支合并到主线
    *   3.4.5\. 基于发布分支的开发
        *   3.4.5.1\. 创建发布分支
        *   3.4.5.2\. 开发者 user1 工作在发布分支
        *   3.4.5.3\. 开发者 user2 工作在发布分支
        *   3.4.5.4\. 开发者 user2 合并推送
        *   3.4.5.5\. 发布分支的提交合并到主线
    *   3.4.6\. 分支变基
        *   3.4.6.1\. 完成`user2/i18n`特性分支的开发
        *   3.4.6.2\. 分支`user2/i18n`变基
*   3.5\. 远程版本库
    *   3.5.1\. 远程分支
    *   3.5.2\. 分支追踪
    *   3.5.3\. 远程版本库
    *   3.5.4\. PUSH 和 PULL 操作与远程版本库
    *   3.5.5\. 里程碑和远程版本库
    *   3.5.6\. 分支和里程碑的安全性
*   3.6\. 补丁文件交互
    *   3.6.1\. 创建补丁
    *   3.6.2\. 应用补丁
    *   3.6.3\. StGit 和 Quilt
        *   3.6.3.1\. StGit
        *   3.6.3.2\. Quilt

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 3.1\. Git 协议与工作协同

要想团队协作使用 Git，就需要用到 Git 协议。

## 3.1.1\. Git 支持的协议

首先来看看数据交换需要使用的协议。

Git 提供了丰富的协议支持，包括：SSH、GIT、HTTP、HTTPS、FTP、FTPS、RSYNC 及前面已经看到的本地协议等。各种不同协议的 URL 写法如表 15-1 所示。

表 15-1：Git 支持的协议一览表

| 协议名称 | 语法格式 | 说明 |
| --- | --- | --- |
| SSH 协议（1） | `ssh://[user@]example.com[:port]/path/to/repo.git/` | 可在 URL 中设置用户名和端口。 默认端口 22。 |
| SSH 协议（2） | `[user@]example.com:path/to/repo.git/` | 更为精简的 SCP 格式表示法，更简洁。 但是非默认端口需要通过其他方式（如地址别名方式）设定。 |
| GIT 协议 | `git://example.com[:port]/path/to/repo.git/` | 最常用的只读协议。 |
| HTTP[S]协议 | `http[s]://example.com[:port]/path/to/repo.git/` | 兼有智能协议和哑协议。 |
| FTP[S]协议 | `ftp[s]://example.com[:port]/path/to/repo.git/` | 哑协议。 |
| RSYNC 协议 | `rsync://example.com/path/to/repo.git/` | 哑协议。 |
| 本地协议（1） | `file:///path/to/repo.git` |   |
| 本地协议（2） | `/path/to/repo.git` | 和`file://`格式的本地协议类似。但有细微差别。 例如克隆时不支持浅克隆，且采用直接的硬连接实现克隆。 |

上面介绍的各种协议如果按照其聪明程度划分，可分为两类：智能协议和哑协议。

**1\. 智能协议**

在通讯时使用智能协议，会在两个通讯的版本库的各自一端分别打开两个程序进行数据交换。使用智能协议最直观的印象就是在数据传输过程中会有清晰的进度显示，而且因为是按需传输所以传输量更小，速度更快。图 15-1 显示的就是在执行 PULL 和 PUSH 两个最常用的操作时，两个版本库各自启动辅助程序的情况。

![../_images/git-smart-protocol.png](img/git-smart-protocol.png)

图 15-1：Git 智能协议通讯示意图

上述协议中 SSH、GIT 及本地协议（file://）属于智能协议。HTTP 协议需要特殊的配置（用`git-http-backend`配置 CGI），并且客户端需要使用 Git 1.6.6 或更高的版本才能够使用智能协议。

**2\. 哑协议**

和智能协议相对的是哑协议。使用哑协议在访问远程版本库的时候，远程版本库不会运行辅助程序，而是完全依靠客户端去主动“发现”。客户端需要访问文件`.git/info/refs`获取当前版本库的引用列表，并根据引用对应的提交 ID 直接访问对象库目录下的文件。如果对象文件被打包而不以松散对象形式存在，则 Git 客户端还要去访问文件`.git/objects/info/packs`以获得打包文件列表，并据此读取完整的打包文件，从打包文件中获取对象。由此可见哑协议的效率非常之低，甚至会因为要获取一个对象而去访问整个 pack 包。

使用哑协议最直观的感受是：传输速度非常慢，而且传输进度不可见，不知道什么时候才能够完成数据传输。上述协议中，像 FTP、RSYNC 都是哑协议，对于没有通过`git-http-backend`或类似 CGI 程序配置的 HTTP 服务器提供的也是哑协议。 因为哑协议需要索引文件`.git/info/refs`和`.git/objects/info/packs`以获取引用和包列表，因此要在版本库的钩子脚本`post-update`中设置运行**git update-server-info**以确保及时更新哑协议需要的索引文件。不过如果不使用哑协议，运行**git update-server-info**就没有什么必要了。

以 Git 项目本身为例，看看如何使用不同的协议地址进行版本库克隆。

*   Git 协议（智能协议）：

    ```
    git clone git://git.kernel.org/pub/scm/git/git.git

    ```

*   HTTP(S)哑协议：

    ```
    git clone http://www.kernel.org/pub/scm/git/git.git

    ```

*   HTTP(S)智能协议：

    使用 Git 1.6.6 或更高版本访问。

    ```
    git clone https://github.com/git/git.git

    ```

## 3.1.2\. 多用户协同的本地模拟

在本篇（“Git 和声”）的学习过程中，需要一个能够提供多人访问的版本库，显然要找到一个公共服务器，并且能让所有人都尽情发挥不太容易，但幸好可以使用本地协议来模拟。在后面的内容中，会经常使用本地协议地址`file:///path/to/repos/<project>.git`来代表对某一公共版本库的访问，您可以把`file://`格式的 URL（比直接使用路径方式更逼真）想象为`git://`或者`http://`格式，并且想象它是在一台远程的服务器上，而非本机。

同样的，为了模拟多人的操作，也不再使用`/path/to/my/workspace`作为工作区，而是分别使用`/path/to/user1/workspace`和`/path/to/user2/workspace`等工作区来代表不同用户的工作环境。同样想象一下`/path/to/user1/`和`/path/to/user2/`是在不同的主机上，并由不同的用户进行操作。

下面就来演示一个共享版本库的搭建过程，以及两个用户 user1 和 user2 在各自的工作区中如何工作并进行数据交换的，具体过程如下。

*   创建一个共享的版本库，于`/path/to/repos/shared.git`。

    别忘了在第 2 篇的第十三章“Git 克隆”一章中介绍的，以裸版本库方式创建。

    ```
    $ git init --bare /path/to/repos/shared.git
    Initialized empty Git repository in /path/to/repos/shared.git/

    ```

*   用户 user1 克隆版本库。

    克隆完成之后，在版本库级别设置`user.name`和`user.email`环境，以便和全局设置区分开，因为我们的模拟环境中所有用户都共享同一全局设置和系统设置。克隆及设置过程如下：

    ```
    $ cd /path/to/user1/workspace
    $ git clone file:///path/to/repos/shared.git project
    Cloning into project...
    warning: You appear to have cloned an empty repository.
    $ cd project
    $ git config user.name user1
    $ git config user.email user1@sun.ossxp.com

    ```

*   用户 user1 创建初始数据并提交。

    ```
    $ echo Hello. > README
    $ git add README
    $ git commit -m "initial commit."
    [master (root-commit) 5174bf3] initial commit.
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 README

    ```

*   用户 user1 将本地版本库的提交推送到上游。

    在下面的推送指令中使用了 origin 别名，其实际指向就是`file:///path/to/repos/shared.git`。可以从`.git/config`配置文件中看到是如何实现对 origin 远程版本库注册的。关于远程版本库的内容要在第十九章介绍。

    ```
    $ git push origin master
    Counting objects: 3, done.
    Writing objects: 100% (3/3), 210 bytes, done.
    Total 3 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (3/3), done.
    To file:///path/to/repos/shared.git
     * [new branch]      master -> master

    ```

*   用户 user2 克隆版本库。

    ```
    $ cd /path/to/user2/workspace
    $ git clone file:///path/to/repos/shared.git project
    Cloning into project...
    remote: Counting objects: 3, done.
    remote: Total 3 (delta 0), reused 0 (delta 0)
    Receiving objects: 100% (3/3), done.

    ```

*   同样在 user2 的本地版本库中，设置`user.name`和`user.email`环境，以区别全局环境设置。

    ```
    $ cd /path/to/user2/workspace/project
    $ git config user.name user2
    $ git config user.email user2@moon.ossxp.com

    ```

*   用户 user2 的本地版本库现在拥有和 user1 用户同样的提交。

    ```
    $ git log
    commit 5174bf33ab31a3999a6242fdcb1ec237e8f3f91a
    Author: user1 <user1@sun.ossxp.com>
    Date:   Sun Dec 19 15:52:29 2010 +0800

        initial commit.

    ```

## 3.1.3\. 强制非快进式推送

现在用户 user1 和 user2 的工作区是相同的。思考一个问题：如果两人各自在本地版本库中进行独立的提交，然后再分别向共享版本库推送，会互相覆盖么？为了回答这个问题，进行下面的实践。

首先，用户 user1 先在本地版本库中进行提交，然后将本地的提交推送到“远程”共享版本库中。操作步骤如下：

*   用户 user1 创建`team/user1.txt`文件。

    假设这个项目约定：每个开发者在`team`目录下写一个自述文件。于是用户 user1 创建文件`team/user1.txt`。

    ```
    $ cd /path/to/user1/workspace/project/
    $ mkdir team
    $ echo "I'm user1." > team/user1.txt
    $ git add team
    $ git commit -m "user1's profile."
    [master b4f3ae0] user1's profile.
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 team/user1.txt

    ```

*   用户 user1 将本地提交推送到服务器上。

    ```
    $ git push
    Counting objects: 5, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (2/2), done.
    Writing objects: 100% (4/4), 327 bytes, done.
    Total 4 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (4/4), done.
    To file:///path/to/repos/shared.git
       5174bf3..b4f3ae0  master -> master

    ```

*   当前 user1 版本库中的日志

    ```
    $ git log --oneline --graph
    * b4f3ae0 user1's profile.
    * 5174bf3 initial commit.

    ```

通过上面的操作步骤，可以看到用户 user1 成功的更新了“远程”共享版本库。如果用户 user2 在不知道用户 user1 所做的上述操作的前提下，在基于“远程”版本库旧的数据同步过来的本地版本库中进行改动，然后用户 user2 向“远程”共享版本库推送会有什么结果呢？用下面的操作验证一下。

*   用户 user2 创建`team/user2.txt`文件。

    ```
    $ cd /path/to/user2/workspace/project/
    $ mkdir team
    $ echo "I'm user1?" > team/user2.txt
    $ git add team
    $ git commit -m "user2's profile."
    [master 8409e4c] user2's profile.
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 team/user2.txt

    ```

*   用户 user2 将本地提交推送到服务器时出错。

    ```
    $ git push
    To file:///path/to/repos/shared.git
     ! [rejected]        master -> master (non-fast-forward)
    error: failed to push some refs to 'file:///path/to/repos/shared.git'
    To prevent you from losing history, non-fast-forward updates were rejected
    Merge the remote changes (e.g. 'git pull') before pushing again.  See the
    'Note about fast-forwards' section of 'git push --help' for details.

    ```

*   用户 user2 的推送失败了。错误日志翻译如下：

    ```
    到版本库 file:///path/to/repos/shared.git
     ! [被拒绝]        master -> master (非快进)
    错误：部分引用向 'file:///path/to/repos/shared.git' 推送失败
    为防止您丢失历史，非快进式更新被拒绝。
    在推送前请先合并远程改动，例如执行 'git pull'。

    ```

推送失败了。但这不是坏事情，反倒是一件好事情，因为这避免了用户提交的相互覆盖。Git 通过对推送操作是否是“快进式”操作进行检查，从而保证用户的提交不会相互覆盖。一般情况下，推送只允许“快进式”推送。所谓快进式推送，就是要推送的本地版本库的提交是建立在远程版本库相应分支的现有提交基础上的，即远程版本库相应分支的最新提交是本地版本库最新提交的祖先提交。但现在用户 user2 执行的推送并非如此，是一个非快进式的推送。

*   此时用户 user2 本地版本库的最新提交及其历史提交可以用**git rev-list**命令显示，如下所示：

    ```
    $ git rev-list HEAD
    8409e4c72388a18ea89eecb86d68384212c5233f
    5174bf33ab31a3999a6242fdcb1ec237e8f3f91a

    ```

*   用**git ls-remote**命令显示远程版本库的引用对应的 SHA1 哈希值，会发现远程版本库所包含的最新提交的 SHA1 哈希值是 b4f3ae0...，而不是本地最新提交的祖先提交。

    ```
    $ git ls-remote origin
    b4f3ae0fcadce8c343f3cdc8a69c33cc98c98dfd        HEAD
    b4f3ae0fcadce8c343f3cdc8a69c33cc98c98dfd        refs/heads/master

    ```

实际上当用户 user2 执行推送的时候，Git 就是利用类似方法判断出当前的推送不是一个快进式推送，于是产生警告并终止。

那么如何才能成功推送呢？一个不见得正确的解决方案称为：**强制推送**。

在推送命令的后面使用`-f`参数可以进行强制推送，即使是非快进式的推送也会成功执行。用户 user2 执行强制推送，会强制涮新服务器中的版本。

```
$ git push -f
Counting objects: 7, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (7/7), 503 bytes, done.
Total 7 (delta 0), reused 3 (delta 0)
Unpacking objects: 100% (7/7), done.
To file:///path/to/repos/shared.git
 + b4f3ae0...8409e4c master -> master (forced update)

```

注意到了么，在强制推送的最后一行输出中显示了“强制更新（forced update）”字样。这样用户 user1 向版本库推送的提交由于用户 user2 的强制推送被覆盖了。实际上在这种情况下 user1 也可以强制的推送，从而用自己（user1）的提交再去覆盖用户 user2 的提交。这样的工作模式不是协同，而是战争！

**合理使用非快进式推送**

在上面用户 user2 使用非快进式推送强制更新版本库，实际上是危险和错误的。滥用非快进式推送可能造成提交覆盖大战（战争是霸权的滥用）。正确地使用非快进式推送，应该是在不会造成提交覆盖“战争”的前提下，对历史提交进行修补。

下面的操作也许是一个使用非快进式推送更好的例子。

*   用户 user2 改正之前错误的录入。

    细心的读者可能已经发现，用户 user2 在创建的个人描述文件中把自己的名字写错了。假设用户 user2 在刚刚完成向服务器的推送操作后也发现了这个错误，于是 user2 进行了下面的更改。

    ```
    $ echo "I'm user2." > team/user2.txt
    $ git diff
    diff --git a/team/user2.txt b/team/user2.txt
    index 27268e2..2dcb7b6 100644
    --- a/team/user2.txt
    +++ b/team/user2.txt
    @@ -1 +1 @@
    -I'm user1?
    +I'm user2.

    ```

*   然后用户 user2 将修改好的文件提交到本地版本库中。

    采用直接提交还是使用修补式提交，这是一个问题。因为前次提交已经被推送到共享版本库中，如果采用修补提交会造成前一次提交被新提交抹掉，从而在下次推送操作时造成非快进式推送。这时用户 user2 就要评估“战争”的风险：“我刚刚推送的提交，有没有可能被其他人获取了（通过**git pull**、**git fetch**或**git clone**操作）？”如果确认不会有他人获取，例如现在公司里只有 user2 自己一个人在加班，那么可以放心的进行修补操作。

    ```
    $ git add -u
    $ git commit --amend -m "user2's profile."
    [master 6b1a7a0] user2's profile.
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 team/user2.txt

    ```

*   采用强制推送，更新远程共享版本库中的提交。这个操作越早越好，在他人还没有来得及和服务器同步前将修补提交强制更新到服务器上。

    ```
    $ git push -f
    Counting objects: 5, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (2/2), done.
    Writing objects: 100% (4/4), 331 bytes, done.
    Total 4 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (4/4), done.
    To file:///path/to/repos/shared.git
     + 8409e4c...6b1a7a0 master -> master (forced update)

    ```

## 3.1.4\. 合并后推送

理性的工作协同要避免非快进式推送。一旦向服务器推送后，如果发现错误，不要使用会更改历史的操作（变基、修补提交），而是采用不会改变历史提交的反转提交等操作。

如果在向服务器推送过程中遇到了非快进式推送的警告，应该进行如下操作才更为理性：执行**git pull**获取服务器端最新的提交并和本地提交进行合并，合并成功后再向服务器提交。

例如用户 user1 在推送时遇到了非快进式推送错误，可以通过如下操作将本地版本库的修改和远程版本库的最新提交进行合并。

*   用户 user1 发现推送遇到了非快进式推送。

    ```
    $ cd /path/to/user1/workspace/project/
    $ git push
    To file:///path/to/repos/shared.git
     ! [rejected]        master -> master (non-fast-forward)
    error: failed to push some refs to 'file:///path/to/repos/shared.git'
    To prevent you from losing history, non-fast-forward updates were rejected
    Merge the remote changes (e.g. 'git pull') before pushing again.  See the
    'Note about fast-forwards' section of 'git push --help' for details.

    ```

*   执行**git pull**实现获取远程版本库的最新提交，以及实现获取到的远程版本库提交与本地提交的合并。

    ```
    $ git pull
    remote: Counting objects: 5, done.
    remote: Compressing objects: 100% (2/2), done.
    remote: Total 4 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (4/4), done.
    From file:///path/to/repos/shared
     + b4f3ae0...6b1a7a0 master     -> origin/master  (forced update)
    Merge made by recursive.
     team/user2.txt |    1 +
     1 files changed, 1 insertions(+), 0 deletions(-)
     create mode 100644 team/user2.txt

    ```

*   合并之后，看看版本库的提交关系图。

    合并之后远程服务器中的最新提交`6b1a7a0`成为当前最新提交（合并提交）的父提交。如果再推送，则不再是非快进式的了。

    ```
    $ git log --graph --oneline
    *   bccc620 Merge branch 'master' of file:///path/to/repos/shared
    |\
    | * 6b1a7a0 user2's profile.
    * | b4f3ae0 user1's profile.
    |/
    * 5174bf3 initial commit.

    ```

*   执行推送，成功完成到远程版本库的推送。

    ```
    $ git push
    Counting objects: 10, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (5/5), done.
    Writing objects: 100% (7/7), 686 bytes, done.
    Total 7 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (7/7), done.
    To file:///path/to/repos/shared.git
       6b1a7a0..bccc620  master -> master

    ```

## 3.1.5\. 禁止非快进式推送

非快进式推送如果被滥用，会成为项目的灾难：

*   团队成员之间的提交战争取代了本应的相互协作。
*   造成不必要的冲突，为他人造成麻烦。
*   在提交历史中引入包含修补提交前后两个版本的怪异的合并提交。

Git 提供了至少两种方式对非快进式推送进行限制。一个是通过版本库的配置，另一个是通过版本库的钩子脚本。

将版本库的参数`receive.denyNonFastForwards`设置为`true`可以禁止任何用户进行非快进式推送。下面的示例中，可以看到针对一个已经预先设置为禁止非快进式推送的版本库执行非快进式推送操作，将会被禁止，即使使用强制推送操作。

*   更改服务器版本库`/path/to/repos/shared.git`的配置。

    ```
    $ git --git-dir=/path/to/repos/shared.git config receive.denyNonFastForwards true

    ```

*   在用户 user1 的工作区执行重置操作。

    ```
    $ git reset --hard HEAD¹
    $ git log --graph --oneline
    * b4f3ae0 user1's profile.
    * 5174bf3 initial commit.

    ```

*   用户 user1 使用强制推送也会失败。

    在出错信息中看到服务器端拒绝执行：`[remote rejected]`。

    ```
    $ git push -f
    Total 0 (delta 0), reused 0 (delta 0)
    remote: error: denying non-fast-forward refs/heads/master (you should pull first)
    To file:///path/to/repos/shared.git
     ! [remote rejected] master -> master (non-fast-forward)
    error: failed to push some refs to 'file:///path/to/repos/shared.git'

    ```

另外一个方法是通过钩子脚本进行设置，可以仅对某些情况下的非快进式推送进行限制，而不是不分青红皁白地一概拒绝。例如：只对部分用户进行限制，而允许特定用户执行非快进式推送，或者允许某些分支可以进行强制提交而其他分支不可以。第 5 篇第三十章会介绍 Gitolite 服务架设，通过授权文件（实际上通过版本库的`update`钩子脚本实现）对版本库非快进式推送做出更为精细的授权控制。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 3.2\. 冲突解决

上一章介绍了 Git 协议，并且使用本地协议来模拟一个远程的版本库，以两个不同用户的身份检出该版本库，和该远程版本库进行交互——交换数据、协同工作。在上一章的协同中只遇到了一个小小的麻烦——非快进式推送，可以通过执行 PULL（拉回）操作，成功完成合并后再推送。

但是在真实的运行环境中，用户间协同并不总是会一帆风顺，只要有合并就可能会有冲突。本章就重点介绍冲突解决机制。

## 3.2.1\. 拉回操作中的合并

为了降低难度，上一章的实践中用户 user1 执行**git pull**操作解决非快进式推送问题似乎非常的简单，就好像直接把共享版本库中最新提交直接拉回到本地，然后就可以推送了，其他好像什么都没有发生一样。真的是这样么？

*   用户 user1 向共享版本库推送时，因为 user2 强制推送已经改变了共享版本库中的提交状态，导致 user1 推送失败，如图 16-1 所示。

    ![../_images/git-merge-pull-1.png](img/git-merge-pull-1.png)

    图 16-1：非快进式推送被禁止

*   用户 user1 执行 PULL 操作的第一阶段，将共享版本库 master 分支的最新提交拉回到本地，并更新到本地版本库特定的引用`refs/remotes/origin/master`（简称为`origin/master`），如图 16-2 所示。

    ![../_images/git-merge-pull-2.png](img/git-merge-pull-2.png)

    图 16-2：执行获取操作

*   用户 user1 执行 PULL 操作的第二阶段，将本地分支 master 和共享版本库本地跟踪分支`origin/master`进行合并操作，如图 16-3 所示。

    ![../_images/git-merge-pull-3.png](img/git-merge-pull-3.png)

    图 16-3：执行合并操作

*   用户 user1 执行 PUSH 操作，将本地提交推送到共享版本库中，如图 16-4 所示。

    ![../_images/git-merge-pull-4.png](img/git-merge-pull-4.png)

    图 16-4：执行推送操作

实际上拉回（PULL）操作是由两个步骤组成的，一个是获取（FETCH）操作，一个是合并（MERGE）操作，即：

```
git pull = git fetch + git merge

```

图 16-2 示意的获取（FETCH）操作看似很简单，实际上要到第十九章介绍远程版本库的章节才能够讲明白，现在只需要根据图示将获取操作理解为将远程的共享版本库的对象（提交、里程碑、分支等）复制到本地即可。

合并（MERGE）操作是本章要介绍的重点。合并操作可以由拉回操作（**git pull**）隐式的执行，将其他版本库的提交和本地版本库的提交进行合并。还可以针对本版本库中的其他分支（将在第十八章中介绍）进行显示的合并操作，将其他分支的提交和当前分支的提交进行合并。

合并操作的命令行格式如下：

```
git merge [选项...] <commit>...

```

合并操作的大多数情况，只须提供一个`<commit>`（提交 ID 或对应的引用：分支、里程碑等）作为参数。合并操作将`<commit>`对应的目录树和当前工作分支的目录树的内容进行合并，合并后的提交以当前分支的提交作为第一个父提交，以`<commit>`为第二个父提交。合并操作还支持将多个`<commit>`代表的分支和当前分支进行合并，过程类似。合并操作的选项很多，这会在本章及第二十四章“子树合并”中予以介绍。

默认情况下，合并后的结果会自动提交，但是如果提供`--no-commit`选项，则合并后的结果会放入暂存区，用户可以对合并结果进行检查、更改，然后手动提交。

合并操作并非总会成功，因为合并的不同提交可能同时修改了同一文件相同区域的内容，导致冲突。冲突会造成合并操作的中断，冲突的文件被标识，用户可以对标识为冲突的文件进行冲突解决操作，然后更新暂存区，再提交，最终完成合并操作。

根据合并操作是否遇到冲突，以及不同的冲突类型，可以分为以下几种情况：成功的自动合并、逻辑冲突、真正的冲突和树冲突。下面分别予以介绍。

## 3.2.2\. 合并一：自动合并

Git 的合并操作非常智能，大多数情况下会自动完成合并。不管是修改不同的文件，还是修改相同的文件（文件的不同位置），或者文件名变更。

### 3.2.2.1\. 修改不同的文件

如果用户 user1 和 user2 各自的本地提交中修改了不同的文件，当一个用户将改动推送到服务器后，另外一个用户推送就遇到非快进式推送错误，需要先合并再推送。因两个用户修改了不同的文件，合并不会遇到麻烦。

在上一章的操作过程中，两个用户的本地版本库和共享版本库可能不一致，为确保版本库状态的一致性以便下面的实践能够正常执行，分别在两个用户的本地版本库中执行下面的操作。

```
$ git pull
$ git reset --hard origin/master

```

下面的实践中，两个用户分别修改不同的文件，其中一个用户要尝试合并操作将本地提交和另外一个用户的提交合并。

*   用户 user1 修改`team/user1.txt`文件，提交并推送到共享服务器。

    ```
    $ cd /path/to/user1/workspace/project/
    $ echo "hack by user1 at `date -R`" >> team/user1.txt
    $ git add -u
    $ git commit -m "update team/user1.txt"
    $ git push

    ```

*   用户 user2 修改`team/user2.txt`文件，提交。

    ```
    $ cd /path/to/user2/workspace/project/
    $ echo "hack by user2 at `date -R`" >> team/user2.txt
    $ git add -u
    $ git commit -m "update team/user2.txt"

    ```

*   用户 user2 在推送的时候，会遇到非快进式推进的错误而被终止。

    ```
    $ git push
    To file:///path/to/repos/shared.git
     ! [rejected]        master -> master (non-fast-forward)
    error: failed to push some refs to 'file:///path/to/repos/shared.git'
    To prevent you from losing history, non-fast-forward updates were rejected
    Merge the remote changes (e.g. 'git pull') before pushing again.  See the
    'Note about fast-forwards' section of 'git push --help' for details.

    ```

*   用户 user2 执行获取（**git fetch**）操作。获取到的提交更新到本地跟踪共享版本库 master 分支的本地引用`origin/master`中。

    ```
    $ git fetch
    remote: Counting objects: 7, done.
    remote: Compressing objects: 100% (4/4), done.
    remote: Total 4 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (4/4), done.
    From file:///path/to/repos/shared
       bccc620..25fce74  master     -> origin/master

    ```

*   用户 user2 执行合并操作，完成自动合并。

    ```
    $ git merge origin/master
    Merge made by recursive.
     team/user1.txt |    1 +
     1 files changed, 1 insertions(+), 0 deletions(-)

    ```

*   用户 user2 推送合并后的本地版本库到共享版本库。

    ```
    $ git push
    Counting objects: 12, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (7/7), done.
    Writing objects: 100% (7/7), 747 bytes, done.
    Total 7 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (7/7), done.
    To file:///path/to/repos/shared.git
       25fce74..0855b86  master -> master

    ```

*   通过提交日志，可以看到成功合并的提交和其两个父提交的关系图。

    ```
    $ git log -3 --graph --stat
    *   commit 0855b86678d1cf86ccdd13adaaa6e735715d6a7e
    |\  Merge: f53acdf 25fce74
    | | Author: user2 <user2@moon.ossxp.com>
    | | Date:   Sat Dec 25 23:00:55 2010 +0800
    | |
    | |     Merge remote branch 'origin/master'
    | |
    | * commit 25fce74b5e73b960c42e4a463d03d462919b674d
    | | Author: user1 <user1@sun.ossxp.com>
    | | Date:   Sat Dec 25 22:54:53 2010 +0800
    | |
    | |     update team/user1.txt
    | |
    | |  team/user1.txt |    1 +
    | |  1 files changed, 1 insertions(+), 0 deletions(-)
    | |
    * | commit f53acdf6a76e0552b562f5aaa4d40ff19e8e2f77
    |/  Author: user2 <user2@moon.ossxp.com>
    |   Date:   Sat Dec 25 22:56:49 2010 +0800
    |
    |       update team/user2.txt
    |
    |    team/user2.txt |    1 +
    |    1 files changed, 1 insertions(+), 0 deletions(-)

    ```

### 3.2.2.2\. 修改相同文件的不同区域

当用户 user1 和 user2 在本地提交中修改相同的文件，但是修改的是文件的不同位置时，则两个用户的提交仍可成功合并。

*   为确保两个用户的本地版本库和共享版本库状态一致，先分别对两个用户的本地版本库执行拉回操作。

    ```
    $ git pull

    ```

*   用户 user1 在自己的工作区中修改`README`文件，在文件的第一行插入内容，更改后的文件内容如下。

    ```
    User1 hacked.
    Hello.

    ```

*   用户 user1 对修改进行本地提交并推送到共享版本库。

    ```
    $ git add -u
    $ git commit -m "User1 hack at the beginning."
    $ git push

    ```

*   用户 user2 在自己的工作区中修改`README`文件，在文件的最后插入内容，更改后的文件内容如下。

    ```
    Hello.
    User2 hacked.

    ```

*   用户 user2 对修改进行本地提交。

    ```
    $ git add -u
    $ git commit -m "User2 hack at the end."

    ```

*   用户 user2 执行获取（**git fetch**）操作。获取到的提交更新到本地跟踪共享版本库 master 分支的本地引用`origin/master`中。

    ```
    $ git fetch
    remote: Counting objects: 5, done.
    remote: Compressing objects: 100% (2/2), done.
    remote: Total 3 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (3/3), done.
    From file:///path/to/repos/shared
       0855b86..07e9d08  master     -> origin/master

    ```

*   用户 user2 执行合并操作，完成自动合并。

    ```
    $ git merge refs/remotes/origin/master
    Auto-merging README
    Merge made by recursive.
     README |    1 +
     1 files changed, 1 insertions(+), 0 deletions(-)

    ```

*   用户 user2 推送合并后的本地版本库到共享版本库。

    ```
    $ git push
    Counting objects: 10, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (4/4), done.
    Writing objects: 100% (6/6), 607 bytes, done.
    Total 6 (delta 0), reused 3 (delta 0)
    Unpacking objects: 100% (6/6), done.
    To file:///path/to/repos/shared.git
       07e9d08..2a67e6f  master -> master

    ```

*   如果追溯一下`README`文件每一行的来源，可以看到分别是 user1 和 user2 更改的最前和最后的一行。

    ```
    $ git blame README
    07e9d082 (user1 2010-12-25 23:12:17 +0800 1) User1 hacked.
    ⁵¹⁷⁴bf3 (user1 2010-12-19 15:52:29 +0800 2) Hello.
    bb0c74fa (user2 2010-12-25 23:14:27 +0800 3) User2 hacked.

    ```

### 3.2.2.3\. 同时更改文件名和文件内容

如果一个用户将文件移动到其他目录（或修改文件名），另外一个用户针对重命名前的文件进行了修改，还能够实现自动合并么？这对于其他版本控制系统可能是一个难题，例如 Subversion 就不能很好地处理，还为此引入了一个“树冲突”的新名词。Git 对于此类冲突能够很好地处理，可以自动解决冲突实现自动合并。

*   为确保两个用户的本地版本库和共享版本库状态一致，先分别对两个用户的本地版本库执行拉回操作。

    ```
    $ git pull

    ```

*   用户 user1 在自己的工作区中将文件`README`进行重命名，本地提交并推送到共享版本库。

    ```
    $ cd /path/to/user1/workspace/project/
    $ mkdir doc
    $ git mv README doc/README.txt
    $ git commit -m "move document to doc/."
    $ git push

    ```

*   用户 user2 在自己的工作区中修改`README`文件，在文件的最后插入内容，并本地提交。

    ```
    $ cd /path/to/user2/workspace/project/
    $ echo "User2 hacked again." >> README
    $ git add -u
    $ git commit -m "User2 hack README again."

    ```

*   用户 user2 执行获取（**git fetch**）操作。获取到的提交更新到本地跟踪共享版本库 master 分支的本地引用`origin/master`中。

    ```
    $ git fetch
    remote: Counting objects: 5, done.
    remote: Compressing objects: 100% (2/2), done.
    remote: Total 3 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (3/3), done.
    From file:///path/to/repos/shared
       0855b86..07e9d08  master     -> origin/master

    ```

*   用户 user2 执行合并操作，完成自动合并。

    ```
    $ git merge refs/remotes/origin/master
    Merge made by recursive.
     README => doc/README.txt |    0
     1 files changed, 0 insertions(+), 0 deletions(-)
     rename README => doc/README.txt (100%)

    ```

*   用户 user2 推送合并后的本地版本库到共享版本库。

    ```
    $ git push
    Counting objects: 10, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (5/5), done.
    Writing objects: 100% (6/6), 636 bytes, done.
    Total 6 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (6/6), done.
    To file:///path/to/repos/shared.git
       9c51cb9..f73db10  master -> master

    ```

*   使用`-m`参数可以查看合并操作所做出的修改。

    ```
    $ git log -1 -m --stat
    commit f73db106c820f0c6d510f18ae8c67629af9c13b7 (from 887488eee19300c566c272ec84b236026b0303c6)
    Merge: 887488e 9c51cb9
    Author: user2 <user2@moon.ossxp.com>
    Date:   Sat Dec 25 23:36:57 2010 +0800

        Merge remote branch 'refs/remotes/origin/master'

     README         |    4 ----
     doc/README.txt |    4 ++++
     2 files changed, 4 insertions(+), 4 deletions(-)

    commit f73db106c820f0c6d510f18ae8c67629af9c13b7 (from 9c51cb91bfe12654e2de1d61d722161db0539644)
    Merge: 887488e 9c51cb9
    Author: user2 <user2@moon.ossxp.com>
    Date:   Sat Dec 25 23:36:57 2010 +0800

        Merge remote branch 'refs/remotes/origin/master'

     doc/README.txt |    1 +
     1 files changed, 1 insertions(+), 0 deletions(-)

    ```

## 3.2.3\. 合并二：逻辑冲突

自动合并如果成功地执行，则大多数情况下就意味着完事大吉，但是在某些特殊情况下，合并后的结果虽然在 Git 看来是完美的合并，实际上却存在着逻辑冲突。

一个典型的逻辑冲突是一个用户修改了一个文件的文件名，而另外的用户在其他文件中引用旧的文件名，这样的合并虽然能够成功但是包含着逻辑冲突。例如：

*   一个 C 语言的项目中存在头文件`hello.h`，该头文件定义了一些函数声明。
*   用户 user1 将`hello.h`文件改名为`api.h`。
*   用户 user2 写了一个新的源码文件`foo.c`并在该文件中包含了`hello.h`文件。
*   两个用户的提交合并后，会因为源码文件`foo.c`找不到包含的`hello.h`文件而导致项目编译失败。

再举一个逻辑冲突的示例。假如一个用户修改了函数返回值而另外的用户使用旧的函数返回值，虽然成功合并但是存在逻辑冲突：

*   函数`compare(obj1, obj2)`用于比较两个对象`obj1`和`obj2`。返回`1`代表比较的两个对象相同，返回`0`代表比较的两个对象不同。
*   用户 user1 修改了该函数的返回值，返回`0`代表两个对象相同，返回`1`代表`obj1`大于`obj2`，返回`-1`则代表`obj1`小于`obj2`。
*   用户 user2 不知道 user1 对该函数的改动，仍以该函数原返回值判断两个对象的异同。
*   两个用户的提交合并后，不会出现编译错误，但是软件中会潜藏着重大的 Bug。

上面的两个逻辑冲突的示例，尤其是最后一个非常难以捕捉。如果因此而贬低 Git 的自动合并，或者对每次自动合并的结果疑神疑鬼，进而花费大量精力去分析合并的结果，则是因噎废食、得不偿失。一个好的项目实践是每个开发人员都为自己的代码编写可运行的单元测试，项目每次编译时都要执行自动化测试，捕捉潜藏的 Bug。在 2010 年 OpenParty 上的一个报告中，我介绍了如何在项目中引入单元测试及自动化集成，可以参考下面的链接：

*   [`www.beijing-open-party.org/topic/9`](http://www.beijing-open-party.org/topic/9)
*   [`wenku.baidu.com/view/63bf7d160b4e767f5acfcef6.html`](http://wenku.baidu.com/view/63bf7d160b4e767f5acfcef6.html)

## 3.2.4\. 合并三：冲突解决

如果两个用户修改了同一文件的同一区域，则在合并的时候会遇到冲突导致合并过程中断。这是因为 Git 并不能越俎代庖的替用户做出决定，而是把决定权交给用户。在这种情况下，Git 显示为合并冲突，等待用户对冲突做出抉择。

下面的实践非常简单，两个用户都修改`doc/README.txt`文件，在第二行“Hello.”的后面加上自己的名字。

*   为确保两个用户的本地版本库和共享版本库状态一致，先分别对两个用户的本地版本库执行拉回操作。

    ```
    $ git pull

    ```

*   用户 user1 在自己的工作区修改`doc/README.txt`文件（仅改动了第二行）。修改后内容如下：

    ```
    User1 hacked.
    Hello, user1.
    User2 hacked.
    User2 hacked again.

    ```

*   用户 user1 对修改进行本地提交并推送到共享版本库。

    ```
    $ git add -u
    $ git commit -m "Say hello to user1."
    $ git push

    ```

*   用户 user2 在自己的工作区修改`doc/README.txt`文件（仅改动了第二行）。修改后内容如下：

    ```
    User1 hacked.
    Hello, user2.
    User2 hacked.
    User2 hacked again.

    ```

*   用户 user2 对修改进行本地提交。

    ```
    $ git add -u
    $ git commit -m "Say hello to user2."

    ```

*   用户 user2 执行拉回操作，遇到冲突。

    `git pull`操作相当于`git fetch`和`git merge`两个操作。

    ```
    $ git pull
    remote: Counting objects: 7, done.
    remote: Compressing objects: 100% (3/3), done.
    remote: Total 4 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (4/4), done.
    From file:///path/to/repos/shared
       f73db10..a123390  master     -> origin/master
    Auto-merging doc/README.txt
    CONFLICT (content): Merge conflict in doc/README.txt
    Automatic merge failed; fix conflicts and then commit the result.

    ```

执行**git pull**时所做的合并操作由于遇到冲突导致中断。来看看处于合并冲突状态时工作区和暂存区的状态。

执行**git status**命令，可以从状态输出中看到文件`doc/README.txt`处于未合并的状态，这个文件在两个不同的提交中都做了修改。

```
$ git status
# On branch master
# Your branch and 'refs/remotes/origin/master' have diverged,
# and have 1 and 1 different commit(s) each, respectively.
#
# Unmerged paths:
#   (use "git add/rm <file>..." as appropriate to mark resolution)
#
#       both modified:      doc/README.txt
#
no changes added to commit (use "git add" and/or "git commit -a")

```

那么 Git 是如何记录合并过程及冲突的呢？实际上合并过程是通过`.git`目录下的几个文件进行记录的：

*   文件`.git/MERGE_HEAD`记录所合并的提交 ID。
*   文件`.git/MERGE_MSG`记录合并失败的信息。
*   文件`.git/MERGE_MODE`标识合并状态。

版本库暂存区中则会记录冲突文件的多个不同版本。可以使用**git ls-files**命令查看。

```
$ git ls-files -s
100644 ea501534d70a13b47b3b4b85c39ab487fa6471c2 1       doc/README.txt
100644 5611db505157d312e4f6fb1db2e2c5bac2a55432 2       doc/README.txt
100644 036dbc5c11b0a0cefc8247cf0e9a3e678f8de060 3       doc/README.txt
100644 430bd4314705257a53241bc1d2cb2cc30f06f5ea 0       team/user1.txt
100644 a72ca0b4f2b9661d12d2a0c1456649fc074a38e3 0       team/user2.txt

```

在上面的输出中，每一行分为四个字段，前两个分别是文件的属性和 SHA1 哈希值。第三个字段是暂存区编号。当合并冲突发生后，会用到 0 以上的暂存区编号。

*   编号为 1 的暂存区用于保存冲突文件修改之前的副本，即冲突双方共同的祖先版本。可以用`:1:<filename>`访问。

    ```
    $ git show :1:doc/README.txt
    User1 hacked.
    Hello.
    User2 hacked.
    User2 hacked again.

    ```

*   编号为 2 的暂存区用于保存当前冲突文件在当前分支中修改的副本。可以用`:2:<filename>`访问。

    ```
    $ git show :2:doc/README.txt
    User1 hacked.
    Hello, user2.
    User2 hacked.
    User2 hacked again.

    ```

*   编号为 3 的暂存区用于保存当前冲突文件在合并版本（分支）中修改的副本。可以用`:3:<filename>`访问。

    ```
    $ git show :3:doc/README.txt
    User1 hacked.
    Hello, user1.
    User2 hacked.
    User2 hacked again.

    ```

对暂存区中冲突文件的上述三个副本无须了解太多，这三个副本实际上是提供冲突解决工具，用于实现三向文件合并的。

工作区的版本则可能同时包含了成功的合并及冲突的合并，其中冲突的合并会用特殊的标记（<<<<<<< ======= >>>>>>>）进行标识。查看当前工作区中冲突的文件：

```
$ cat doc/README.txt
User1 hacked.
<<<<<<< HEAD
Hello, user2.
=======
Hello, user1.
>>>>>>> a123390b8936882bd53033a582ab540850b6b5fb
User2 hacked.
User2 hacked again.

```

特殊标识`<<<<<<<`（七个小于号）和`=======`（七个等号）之间的内容是当前分支所更改的内容。在特殊标识`=======`（七个等号）和`>>>>>>>`（七个大于号）之间的内容是所合并的版本更改的内容。

冲突解决的实质就是通过编辑操作，将冲突标识符所标识的冲突内容替换为合适的内容，并去掉冲突标识符。编辑完毕后执行**git add**命令将文件添加到暂存区（标号 0），然后再提交就完成了冲突解决。

当工作区处于合并冲突状态时，无法再执行提交操作。此时有两个选择：放弃合并操作，或者对合并冲突进行冲突解决操作。放弃合并操作非常简单，只须执行**git reset**将暂存区重置即可。下面重点介绍如何进行冲突解决的操作。有两个方法进行冲突解决，一个是对少量冲突非常适合的手工编辑操作，另外一个是使用图形化冲突解决工具。

### 3.2.4.1\. 手工编辑完成冲突解决

先来看看不使用工具，直接手动编辑完成冲突解决。打开文件`doc/README.txt`，将冲突标识符所标识的文字替换为`Hello, user1 and user2.`。修改后的文件内容如下：

```
User1 hacked.
Hello, user1 and user2.
User2 hacked.
User2 hacked again.

```

然后添加到暂存区，并提交：

```
$ git add -u
$ git commit -m "Merge completed: say hello to all users."

```

查看最近三次提交的日志，会看到最新的提交就是一个合并提交：

```
$ git log --oneline --graph -3
*   bd3ad1a Merge completed: say hello to all users.
|\
| * a123390 Say hello to user1.
* | 60b10f3 Say hello to user2.
|/

```

提交完成后，会看到`.git`目录下与合并相关的文件`.git/MERGE_HEAD`、`.git/MERGE_MSG`、`.git/MERGE_MODE`文件都自动删除了。

如果查看暂存区，会发现冲突文件在暂存区中的三个副本也都清除了（实际在对编辑完成的冲突文件执行**git add**后就已经清除了）。

```
$ git ls-files -s
100644 463dd451d94832f196096bbc0c9cf9f2d0f82527 0       doc/README.txt
100644 430bd4314705257a53241bc1d2cb2cc30f06f5ea 0       team/user1.txt
100644 a72ca0b4f2b9661d12d2a0c1456649fc074a38e3 0       team/user2.txt

```

### 3.2.4.2\. 图形工具完成冲突解决

上面介绍的通过手工编辑完成冲突解决并不复杂，对于简单的冲突是最快捷的解决方法。但是如果冲突的区域过多、过大，并且缺乏原始版本作为参照，冲突解决过程就会显得非常的不便，这种情况下使用图形工具就显得非常有优势。

还以上面的冲突解决为例介绍使用图形工具进行冲突解决的方法。为了制造一个冲突，首先把 user2 辛辛苦苦完成的冲突解决提交回滚，再执行合并进入冲突状态。

*   将冲突解决的提交回滚，强制重置到前一个版本。

    ```
    $ git reset --hard HEAD^

    ```

*   这时查看状态，会显示当前工作分支的最新提交和共享版本库的 master 分支的最新提交出现了偏离。

    ```
    $ git status
    # On branch master
    # Your branch and 'refs/remotes/origin/master' have diverged,
    # and have 1 and 1 different commit(s) each, respectively.
    #
    nothing to commit (working directory clean)

    ```

*   那么执行合并操作吧。冲突发生了。

    ```
    $ git merge refs/remotes/origin/master
    Auto-merging doc/README.txt
    CONFLICT (content): Merge conflict in doc/README.txt
    Automatic merge failed; fix conflicts and then commit the result.

    ```

下面就演示使用图形工具如何解决冲突。使用图形工具进行冲突解决需要事先在操作系统中安装相关的工具软件，如：kdiff3、meld、tortoisemerge、araxis 等。而启动图形工具进行冲突解决也非常简单，只须执行命令**git mergetool**即可。

```
$ git mergetool
merge tool candidates: opendiff kdiff3 tkdiff xxdiff meld tortoisemerge
gvimdiff diffuse ecmerge p4merge araxis emerge vimdiff
Merging:
doc/README.txt

Normal merge conflict for 'doc/README.txt':
  {local}: modified
  {remote}: modified
Hit return to start merge resolution tool (kdiff3):

```

运行**git mergetool**命令后，会显示支持的图形工具列表，并提示用户选择可用的冲突解决工具。默认会选择系统中已经安装的工具软件，如**kdiff3**。直接按下回车键，自动打开**kdiff3**进入冲突解决界面：

启动**kdiff3**后，如图 16-5，上方三个窗口由左至右显示冲突文件的三个版本，分别是：

1.  暂存区 1 中的版本（共同祖先版本）。
2.  暂存区 2 中的版本（当前分支更改的版本）。
3.  暂存区 3 中的版本（他人更改的版本）。

![../_images/kdiff3-1.png](img/kdiff3-1.png)

图 16-5：kdiff3 冲突解决界面

kdiff3 下方的窗口是合并后文件的编辑窗口。如图 16-6 所示，点击标记为“合并冲突”的一行，在弹出菜单中出现 A、B、C 三个选项，分别代表从 A、B、C 三个窗口拷贝相关内容到当前位置。

![../_images/kdiff3-2.png](img/kdiff3-2.png)

图 16-6：kdiff3 合并冲突行的弹出菜单

当通过图 16-6 显示的弹出菜单选择了 B 和 C 后，可以在图 16-7 中看到在合并窗口出现了标识 B 和 C 的行，分别代表 user2 和 user1 对该行的修改。

![../_images/kdiff3-3.png](img/kdiff3-3.png)

图 16-7：在 kdiff3 冲突区域同时选取 B 和 C 的修改

在合并窗口进行编辑，将“Hello, user1.”修改为“Hello, user1 and user2.”，如图 16-8。修改后，可以看到该行的标识由`C`改变为`m`，含义是该行是经过手工修改的行。

![../_images/kdiff3-4.png](img/kdiff3-4.png)

图 16-8：在 kdiff3 的冲突区域编辑内容

在合并窗口删除标识为从 B 窗口引入的行“Hello, user2.”，如图 16-9。保存退出即完成图形化冲突解决。

![../_images/kdiff3-5.png](img/kdiff3-5.png)

图 16-9：完成 kdiff3 冲突区域的编辑

图形工具保存退出后，显示工作区状态，会看到冲突已经解决。在工作区还会遗留一个以`.orig`结尾的合并前文件副本。

```
$ git status
# On branch master
# Your branch and 'refs/remotes/origin/master' have diverged,
# and have 1 and 1 different commit(s) each, respectively.
#
# Changes to be committed:
#
#       modified:   doc/README.txt
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#       doc/README.txt.orig

```

查看暂存区会发现暂存区中的冲突文件的三个副本都已经清除。

```
$ git ls-files -s
100644 463dd451d94832f196096bbc0c9cf9f2d0f82527 0       doc/README.txt
100644 430bd4314705257a53241bc1d2cb2cc30f06f5ea 0       team/user1.txt
100644 a72ca0b4f2b9661d12d2a0c1456649fc074a38e3 0       team/user2.txt

```

执行提交和推送。

```
$ git commit -m "Say hello to all users."
[master 7f7bb5e] Say hello to all users.
$ git push
Counting objects: 14, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (6/6), done.
Writing objects: 100% (8/8), 712 bytes, done.
Total 8 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (8/8), done.
To file:///path/to/repos/shared.git
   a123390..7f7bb5e  master -> master

```

查看最近三次的提交日志，会看到最新的提交是一个合并提交。

```
$ git log --oneline --graph -3
*   7f7bb5e Say hello to all users.
|\
| * a123390 Say hello to user1.
* | 60b10f3 Say hello to user2.
|/

```

## 3.2.5\. 合并四：树冲突

如果一个用户将某个文件改名，另外一个用户将同样的文件改为另外的名字，当这两个用户的提交进行合并操作时，Git 显然无法替用户做出裁决，于是就产生了冲突。这种因为文件名修改造成的冲突，称为树冲突。这种树冲突的解决方式比较特别，因此专题介绍。

仍旧使用前面的版本库进行此次实践。为确保两个用户的本地版本库和共享版本库状态一致，先分别对两个用户的本地版本库执行拉回操作。

```
$ git pull

```

下面就分别以两个用户的身份执行提交，将同样的一个文件改为不同的文件名，制造一个树冲突。

*   用户 user1 将文件`doc/README.txt`改名为`readme.txt`，提交并推送到共享版本库。

    ```
    $ cd /path/to/user1/workspace/project
    $ git mv doc/README.txt readme.txt
    $ git commit -m "rename doc/README.txt to readme.txt"
    [master 615c1ff] rename doc/README.txt to readme.txt
     1 files changed, 0 insertions(+), 0 deletions(-)
     rename doc/README.txt => readme.txt (100%)
    $ git push
    Counting objects: 3, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (2/2), done.
    Writing objects: 100% (2/2), 282 bytes, done.
    Total 2 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (2/2), done.
    To file:///path/to/repos/shared.git
       7f7bb5e..615c1ff  master -> master

    ```

*   用户 user2 将文件`doc/README.txt`改名为`README`，并做本地提交。

    ```
    $ cd /path/to/user2/workspace/project
    $ git mv doc/README.txt README
    $ git commit -m "rename doc/README.txt to README"
    [master 20180eb] rename doc/README.txt to README
     1 files changed, 0 insertions(+), 0 deletions(-)
     rename doc/README.txt => README (100%)

    ```

*   用户 user2 执行**git pull**操作，遇到合并冲突。

    ```
    $ git pull
    remote: Counting objects: 3, done.
    remote: Compressing objects: 100% (2/2), done.
    remote: Total 2 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (2/2), done.
    From file:///path/to/repos/shared
       7f7bb5e..615c1ff  master     -> origin/master
    CONFLICT (rename/rename): Rename "doc/README.txt"->"README" in branch "HEAD" rename "doc/README.txt"->"readme.txt" in "615c1ffaa41b2798a56854259caeeb1020c51721"
    Automatic merge failed; fix conflicts and then commit the result.

    ```

因为两个用户同时更改了同一文件的文件名并且改成了不同的名字，于是引发冲突。此时查看状态会看到：

```
$ git status
# On branch master
# Your branch and 'refs/remotes/origin/master' have diverged,
# and have 1 and 1 different commit(s) each, respectively.
#
# Unmerged paths:
#   (use "git add/rm <file>..." as appropriate to mark resolution)
#
#       added by us:        README
#       both deleted:       doc/README.txt
#       added by them:      readme.txt
#
no changes added to commit (use "git add" and/or "git commit -a")

```

此时查看一下用户 user2 本地版本库的暂存区，可以看到因为冲突在编号为 1、2、3 的暂存区出现了相同 SHA1 哈希值的对象，但是文件名各不相同。

```
$ git ls-files -s
100644 463dd451d94832f196096bbc0c9cf9f2d0f82527 2       README
100644 463dd451d94832f196096bbc0c9cf9f2d0f82527 1       doc/README.txt
100644 463dd451d94832f196096bbc0c9cf9f2d0f82527 3       readme.txt
100644 430bd4314705257a53241bc1d2cb2cc30f06f5ea 0       team/user1.txt
100644 a72ca0b4f2b9661d12d2a0c1456649fc074a38e3 0       team/user2.txt

```

其中在暂存区 1 中是改名之前的`doc/README.txt`，在暂存区 2 中是用户 user2 改名后的文件名`README`，而暂存区 3 是其他用户（user1）改名后的文件`readme.txt`。

此时的工作区中存在两个相同的文件`README`和`readme.txt`分别是用户 user2 和 user1 对`doc/README.txt`重命名之后的文件。

```
$ ls -l readme.txt README
-rw-r--r-- 1 jiangxin jiangxin 72 12 月 27 12:25 README
-rw-r--r-- 1 jiangxin jiangxin 72 12 月 27 16:53 readme.txt

```

### 3.2.5.1\. 手工操作解决树冲突

这时 user2 应该和 user1 商量一下到底应该将该文件改成什么名字。如果双方最终确认应该采用 user2 重命名的名称，则 user2 应该进行下面的操作完成冲突解决。

*   删除文件`readme.txt`。

    在执行**git rm**操作过程会弹出三条警告，说共有三个文件待合并。

    ```
    $ git rm readme.txt
    README: needs merge
    doc/README.txt: needs merge
    readme.txt: needs merge
    rm 'readme.txt'

    ```

*   删除文件`doc/README.txt`。

    执行删除过程，弹出的警告少了一条，因为前面的删除操作已经将一个冲突文件撤出暂存区了。

    ```
    $ git rm doc/README.txt
    README: needs merge
    doc/README.txt: needs merge
    rm 'doc/README.txt'

    ```

*   添加文件`README`。

    ```
    $ git add README

    ```

*   这时查看一下暂存区，会发现所有文件都在暂存区 0 中。

    ```
    $ git ls-files -s
    100644 463dd451d94832f196096bbc0c9cf9f2d0f82527 0       README
    100644 430bd4314705257a53241bc1d2cb2cc30f06f5ea 0       team/user1.txt
    100644 a72ca0b4f2b9661d12d2a0c1456649fc074a38e3 0       team/user2.txt

    ```

*   提交完成冲突解决。

    ```
    $ git commit -m "fixed tree conflict."
    [master e82187e] fixed tree conflict.

    ```

*   查看一下最近三次提交日志，看到最新的提交是一个合并提交。

    ```
    $ git log --oneline --graph -3 -m --stat
    *   e82187e (from 615c1ff) fixed tree conflict.
    |\
    | |  README     |    4 ++++
    | |  readme.txt |    4 ----
    | |  2 files changed, 4 insertions(+), 4 deletions(-)
    | * 615c1ff rename doc/README.txt to readme.txt
    | |  doc/README.txt |    4 ----
    | |  readme.txt     |    4 ++++
    | |  2 files changed, 4 insertions(+), 4 deletions(-)
    * | 20180eb rename doc/README.txt to README
    |/
    |    README         |    4 ++++
    |    doc/README.txt |    4 ----
    |    2 files changed, 4 insertions(+), 4 deletions(-)

    ```

### 3.2.5.2\. 交互式解决树冲突

树冲突虽然不能像文件冲突那样使用图形工具进行冲突解决，但还是可以使用**git mergetool**命令，通过交互式问答快速解决此类冲突。

首先将 user2 的工作区重置到前一次提交，再执行**git merge**引发树冲突。

*   重置到前一次提交。

    ```
    $ cd /path/to/user2/workspace/project
    $ git reset --hard HEAD^
    HEAD is now at 20180eb rename doc/README.txt to README
    $ git clean -fd

    ```

*   执行**git merge**引发树冲突。

    ```
    $ git merge refs/remotes/origin/master
    CONFLICT (rename/rename): Rename "doc/README.txt"->"README" in branch "HEAD" rename "doc/README.txt"->"readme.txt" in "refs/remotes/origin/master"
    Automatic merge failed; fix conflicts and then commit the result.
    $ git status -s
    AU README
    DD doc/README.txt
    UA readme.txt

    ```

上面操作所引发的树冲突，可以执行**git mergetool**命令进行交互式冲突解决，会如下逐一提示用户进行选择。

*   执行**git mergetool**命令。忽略其中的提示和警告。

    ```
    $ git mergetool
    merge tool candidates: opendiff kdiff3 tkdiff xxdiff meld tortoisemerge gvimdiff diffuse ecmerge p4merge araxis emerge vimdiff
    Merging:
    doc/README.txt
    README
    readme.txt

    mv: 无法获取"doc/README.txt" 的文件状态(stat): 没有那个文件或目录
    cp: 无法获取"./doc/README.txt.BACKUP.13869.txt" 的文件状态(stat): 没有那个文件或目录
    mv: 无法将".merge_file_I3gfzy" 移动至"./doc/README.txt.BASE.13869.txt": 没有那个文件或目录

    ```

*   询问对文件`doc/README.txt`的处理方式。输入`d`选择将该文件删除。

    ```
    Deleted merge conflict for 'doc/README.txt':
      {local}: deleted
      {remote}: deleted
    Use (m)odified or (d)eleted file, or (a)bort? d

    ```

*   询问对文件`README`的处理方式。输入`c`选择将该文件保留（创建）。

    ```
    Deleted merge conflict for 'README':
      {local}: created
      {remote}: deleted
    Use (c)reated or (d)eleted file, or (a)bort? c

    ```

*   询问对文件`readme.txt`的处理方式。输入`d`选择将该文件删除。

    ```
    Deleted merge conflict for 'readme.txt':
      {local}: deleted
      {remote}: created
    Use (c)reated or (d)eleted file, or (a)bort? d

    ```

*   查看当前状态，只有一些尚未清理的临时文件，而冲突已经解决。

    ```
    $ git status -s
    ?? .merge_file_I3gfzy
    ?? README.orig

    ```

*   提交完成冲突解决。

    ```
    $ git commit -m "fixed tree conflict."
    [master e070bc9] fixed tree conflict.

    ```

*   向共享服务器推送。

    ```
    $ git push
    Counting objects: 5, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 457 bytes, done.
    Total 3 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (3/3), done.
    To file:///path/to/repos/shared.git
       615c1ff..e070bc9  master -> master

    ```

## 3.2.6\. 合并策略

Git 合并操作支持很多合并策略，默认会选择最适合的合并策略。例如，和一个分支进行合并时会选择`recursive`合并策略，当和两个或两个以上的其他分支进行合并时采用`octopus`合并策略。可以通过传递参数使用指定的合并策略，命令行如下：

```
git merge [-s <strategy>] [-X <strategy-option>] <commit>...

```

其中参数`-s`用于设定合并策略，参数`-X`用于为所选的合并策略提供附加的参数。

下面分别介绍不同的合并策略：

*   resolve

    该合并策略只能用于合并两个头（即当前分支和另外的一个分支），使用三向合并策略。这个合并策略被认为是最安全、最快的合并策略。

*   recursive

    该合并策略只能用于合并两个头（即当前分支和另外的一个分支），使用三向合并策略。这个合并策略是合并两个头指针时的默认合并策略。

    当合并的头指针拥有一个以上的祖先的时候，会针对多个公共祖先创建一个合并的树，并以此作为三向合并的参照。这个合并策略被认为可以实现冲突的最小化，而且可以发现和处理由于重命名导致的合并冲突。

    这个合并策略可以使用下列选项。

    *   ours

        在遇到冲突的时候，选择我们的版本（当前分支的版本），而忽略他人的版本。如果他人的改动和本地改动不冲突，会将他人改动合并进来。

        不要将此模式和后面介绍的单纯的`ours`合并策略相混淆。后面介绍的`ours`合并策略直接丢弃其他分支的变更，无论冲突与否。

    *   theirs

        和`ours`选项相反，遇到冲突时选择他人的版本，丢弃我们的版本。

    *   subtree[=path]

        这个选项使用子树合并策略，比下面介绍的`subtree`（子树合并）策略的定制能力更强。下面的`subtree`合并策略要对两个树的目录移动进行猜测，而`recursive`合并策略可以通过此参数直接对子树目录进行设置。

*   octopus

    可以合并两个以上的头指针，但是拒绝执行需要手动解决的复杂合并。主要的用途是将多个主题分支合并到一起。这个合并策略是对三个及三个以上头指针进行合并时的默认合并策略。

*   ours

    可以合并任意数量的头指针，但是合并的结果总是使用当前分支的内容，丢弃其他分支的内容。

*   subtree

    这是一个经过调整的 recursive 策略。当合并树 A 和 B 时，如果 B 和 A 的一个子树相同，B 首先进行调整以匹配 A 的树的结构，以免两棵树在同一级别进行合并。同时也针对两棵树的共同祖先进行调整。

    关于子树合并会在第 4 篇的第二十四章“子树合并”中详细介绍。

## 3.2.7\. 合并相关的设置

可以通过**git config**命令设置与合并相关的环境变量，对合并进行配置。下面是一些常用的设置。

*   merge.conflictstyle

    该变量定义冲突文件的显示风格，有两个可用的风格，默认的“merge”或“diff3”。

    默认的“merge”风格使用标准的冲突分界符（`<<<<<<<`、`=======`、`>>>>>>>`）对冲突内容进行标识，其中的两个文字块分别是本地的修改和他人的修改。

    如果使用“diff3”风格，则会在冲突中出现三个文字块，分别是：`<<<<<<<`和`|||||||`之间的本地更改版本、`|||||||`和`=======`之间的原始（共同祖先）版本和`=======`和`>>>>>>>`之间的他人更改的版本。例如：

    ```
    User1 hacked.
    <<<<<<< HEAD
    Hello, user2.
    ||||||| merged common ancestors
    Hello.
    =======
    Hello, user1.
    >>>>>>> a123390b8936882bd53033a582ab540850b6b5fb
    User2 hacked.
    User2 hacked again.

    ```

*   merge.tool

    执行**git mergetool**进行冲突解决时调用的图形化工具。变量`merge.tool`可以设置为如下内置支持的工具：“kdiff3”、“tkdiff”、“meld”、“xxdiff”、“emerge”、“vimdiff”、“gvimdiff”、“diffuse”、“ecmerge”、“tortoisemerge”、“p4merge”、“araxis”和“opendiff”。

    ```
    $ git config --global merge.tool kdiff3

    ```

    如果将`merge.tool`设置为其他值，则使用自定义工具进行冲突解决。自定义工具需要通过`mergetool.<tool>.cmd`对自定义工具的命令行进行设置。

*   mergetool.<tool>.path

    如果**git mergetool**支持的冲突解决工具安装在特殊位置，可以使用`mergetool.<tool>.path`对工具`<tool>`的安装位置进行设置。例如：

    ```
    $ git config --global mergetool.kdiff3.path /path/to/kdiff3

    ```

*   mergetool.<tool>.cmd

    如果所用的冲突解决工具不在内置的工具列表中，还可以使用`mergetool.<tool>.cmd`对自定义工具的命令行进行设置，同时要将`merge.tool`设置为`<tool>`。

    自定义工具的命令行可以使用 Shell 变量。例如：

    ```
    $ git config --global merge.tool mykdiff3
    $ git config --global mergetool.mykdiff3.cmd '/usr/bin/kdiff3
                 -L1 "$MERGED (Base)" -L2 "$MERGED (Local)" -L3 "$MERGED (Remote)"
                 --auto -o "$MERGED" "$BASE" "$LOCAL" "$REMOTE"'

    ```

*   merge.log

    是否在合并提交的提交说明中包含合并提交的概要信息。默认为`false`。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 3.3\. Git 里程碑

里程碑即 Tag，是人为对提交进行的命名。这和 Git 的提交 ID 是否太长无关，使用任何数字版本号无论长短，都没有使用一个直观的表意的字符串来得方便。例如：用里程碑名称“v2.1”对应于软件的 2.1 发布版本就比使用提交 ID 要直观得多。

对于里程碑，实际上我们并不陌生，在第 2 篇的“第十章 Git 基本操作”中，就介绍了使用里程碑来对工作进度“留影”纪念，并使用**git describe**命令显示里程碑和提交 ID 的组合来代表软件的版本号。本章将详细介绍里程碑的创建、删除和共享，还会介绍里程碑存在的三种不同形式：轻量级里程碑、带注释的里程碑和带签名的里程碑。

接下来的三章，将对一个使用`Hello, World`作为示例程序的版本库进行研究，这个版本库不需要我们从头建立，可以直接从 Github 上克隆。先使用下面的方法在本地创建一个镜像，用作本地用户的共享版本库。

*   进入本地版本库根目录下。

    ```
    $ mkdir -p /path/to/repos/
    $ cd /path/to/repos/

    ```

*   从 Github 上镜像`hello-world.git`版本库。

    如果 Git 是 1.6.0 或更新的版本，可以使用下面的命令建立版本库镜像。

    ```
    $ git clone --mirror git://github.com/ossxp-com/hello-world.git

    ```

    否则使用下面的命令建立版本库镜像。

    ```
    $ git clone --bare \
          git://github.com/ossxp-com/hello-world.git \
          hello-world.git

    ```

完成上面操作后，就在本地建立了一个裸版本库`/path/to/repos/hello-world.git`。接下来用户 user1 和 user2 分别在各自工作区克隆这个裸版本库。使用如下命令即可：

```
$ git clone file:///path/to/repos/hello-world.git \
            /path/to/user1/workspace/hello-world
$ git clone file:///path/to/repos/hello-world.git \
            /path/to/user2/workspace/hello-world
$ git --git-dir=/path/to/user1/workspace/hello-world/.git \
      config user.name user1
$ git --git-dir=/path/to/user1/workspace/hello-world/.git \
      config user.email user1@sun.ossxp.com
$ git --git-dir=/path/to/user2/workspace/hello-world/.git \
      config user.name user2
$ git --git-dir=/path/to/user2/workspace/hello-world/.git \
      config user.email user2@moon.ossxp.com

```

## 3.3.1\. 显示里程碑

里程碑可以使用**git tag**命令来显示，里程碑还可以在其他命令的输出中出现，下面分别对这些命令加以介绍。

### 3.3.1.1\. 1\. 命令**git tag**

不带任何参数执行**git tag**命令，即可显示当前版本库的里程碑列表。

```
$ cd /path/to/user1/workspace/hello-world
$ git tag
jx/v1.0
jx/v1.0-i18n
jx/v1.1
jx/v1.2
jx/v1.3
jx/v2.0
jx/v2.1
jx/v2.2
jx/v2.3

```

里程碑创建的时候可能包含一个说明。在显示里程碑的时候同时显示说明，使用`-n<num>`参数，显示最多`<num>`行里程碑的说明。

```
$ git tag -n1
jx/v1.0         Version 1.0
jx/v1.0-i18n    i18n support for v1.0
jx/v1.1         Version 1.1
jx/v1.2         Version 1.2: allow spaces in username.
jx/v1.3         Version 1.3: Hello world speaks in Chinese now.
jx/v2.0         Version 2.0
jx/v2.1         Version 2.1: fixed typo.
jx/v2.2         Version 2.2: allow spaces in username.
jx/v2.3         Version 2.3: Hello world speaks in Chinese now.

```

还可以使用通配符对显示进行过滤。只显示名称和通配符相符的里程碑。

```
$ git tag -l jx/v2*
jx/v2.0
jx/v2.1
jx/v2.2
jx/v2.3

```

### 3.3.1.2\. 2\. 命令**git log**

在查看日志时使用参数`--decorate`可以看到提交对应的里程碑及其他引用。

```
$ git log --oneline --decorate
3e6070e (HEAD, tag: jx/v1.0, origin/master, origin/HEAD, master) Show version.
75346b3 Hello world initialized.

```

### 3.3.1.3\. 3\. 命令**git describe**

使用命令**git describe**将提交显示为一个易记的名称。这个易记的名称来自于建立在该提交上的里程碑，若该提交没有里程碑则使用该提交历史版本上的里程碑并加上可理解的寻址信息。

*   如果该提交恰好被打上一个里程碑，则显示该里程碑的名字。

    ```
    $ git describe
    jx/v1.0
    $ git describe 384f1e0
    jx/v2.2

    ```

*   若提交没有对应的里程碑，但是在其祖先版本上建有里程碑，则使用类似`<tag>-<num>-g<commit>`的格式显示。

    其中`<tag>`是最接近的祖先提交的里程碑名字，`<num>`是该里程碑和提交之间的距离，`<commit>`是该提交的精简提交 ID。

    ```
    $ git describe 610e78fc95bf2324dc5595fa684e08e1089f5757
    jx/v2.2-1-g610e78f

    ```

*   如果工作区对文件有修改，还可以通过后缀`-dirty`表示出来。

    ```
    $ echo hacked >> README; git describe --dirty; git checkout -- README
    jx/v1.0-dirty

    ```

*   如果提交本身没有包含里程碑，可以通过传递`--always`参数显示精简提交 ID，否则出错。

    ```
    $ git describe master^ --always
    75346b3

    ```

命令**git describe**是非常有用的命令，可以将该命令的输出用作软件的版本号。在之前曾经演示过这个应用，马上还会看到。

### 3.3.1.4\. 4\. 命令**git name-rev**

命令**git name-rev**和**git describe**类似，会显示提交 ID 及其对应的一个引用。默认优先使用分支名，除非使用**--tags**参数。还有一个显著的不同是，如果提交上没有相对应的引用，则会使用最新提交上的引用名称并加上向后回溯的符号**~<num>**。

*   默认优先显示分支名。

    ```
    $ git name-rev HEAD
    HEAD master

    ```

*   使用`--tags`优先使用里程碑。

    之所以对应的里程碑引用名称后面加上后缀`⁰`，是因为该引用指向的是一个 tag 对象而非提交。用`⁰`后缀指向对应的提交。

    ```
    $ git name-rev HEAD --tags
    HEAD tags/jx/v1.0⁰

    ```

*   如果提交上没有对应的引用名称，则会使用新提交上的引用名称并加上后缀**~<num>**。后缀的含义是第<num>个祖先提交。

    ```
    $ git name-rev --tags 610e78fc95bf2324dc5595fa684e08e1089f5757
    610e78fc95bf2324dc5595fa684e08e1089f5757 tags/jx/v2.3~1

    ```

*   命令**git name-rev**可以对标准输入中的提交 ID 进行改写，使用管道符号对前一个命令的输出进行改写，会显示神奇的效果。

    ```
    $ git log --pretty=oneline origin/helper/master | git name-rev --tags --stdin
    bb4fef88fee435bfac04b8389cf193d9c04105a6 (tags/jx/v2.3⁰) Translate for Chinese.
    610e78fc95bf2324dc5595fa684e08e1089f5757 (tags/jx/v2.3~1) Add I18N support.
    384f1e0d5106c9c6033311a608b91c69332fe0a8 (tags/jx/v2.2⁰) Bugfix: allow spaces in username.
    e5e62107f8f8d0a5358c3aff993cf874935bb7fb (tags/jx/v2.1⁰) fixed typo: -help to --help
    5d7657b2f1a8e595c01c812dd5b2f67ea133f456 (tags/jx/v2.0⁰) Parse arguments using getopt_long.
    3e6070eb2062746861b20e1e6235fed6f6d15609 (tags/jx/v1.0⁰) Show version.
    75346b3283da5d8117f3fe66815f8aaaf5387321 (tags/jx/v1.0~1) Hello world initialized.

    ```

## 3.3.2\. 创建里程碑

创建里程碑依然是使用**git tag**命令。创建里程碑的用法有以下几种：

```
用法 1： git tag             <tagname> [<commit>]
用法 2： git tag -a          <tagname> [<commit>]
用法 3： git tag -m <msg>    <tagname> [<commit>]
用法 4： git tag -s          <tagname> [<commit>]
用法 5： git tag -u <key-id> <tagname> [<commit>]

```

其中：

*   用法 1 是创建轻量级里程碑。
*   用法 2 和用法 3 相同，都是创建带说明的里程碑。其中用法 3 直接通过`-m`参数提供里程碑创建说明。
*   用法 4 和用法 5 相同，都是创建带 GPG 签名的里程碑。其中用法 5 用`-u`参数选择指定的私钥进行签名。
*   创建里程碑需要输入里程碑的名字`<tagname>`和一个可选的提交 ID`<commit>`。如果没有提供提交 ID，则基于头指针`HEAD`创建里程碑。

### 3.3.2.1\. 轻量级里程碑

轻量级里程碑最简单，创建时无须输入描述信息。我们来看看如何创建轻量级里程碑：

*   先创建一个空提交。

    ```
    $ git commit --allow-empty -m "blank commit."
    [master 60a2f4f] blank commit.

    ```

*   在刚刚创建的空提交上创建一个轻量级里程碑，名为`mytag`。

    省略了`<commit>`参数，相当于在`HEAD`上即最新的空提交上创建里程碑。

    ```
    $ git tag mytag

    ```

*   查看里程碑，可以看到该里程碑已经创建。

    ```
    $ git tag -l my*
    mytag

    ```

**轻量级里程碑的奥秘**

当创建了里程碑`mytag`后，会在版本库的`.git/refs/tags`目录下创建一个新文件。

*   查看一下这个引用文件的内容，会发现是一个 40 位的 SHA1 哈希值。

    ```
    $ cat .git/refs/tags/mytag
    60a2f4f31e5dddd777c6ad37388fe6e5520734cb

    ```

*   用**git cat-file**命令检查轻量级里程碑指向的对象。轻量级里程碑实际上指向的是一个提交。

    ```
    $ git cat-file -t mytag
    commit

    ```

*   查看该提交的内容，发现就是刚刚进行的空提交。

    ```
    $ git cat-file -p mytag
    tree 1d902fedc4eb732f17e50f111dcecb638f10313e
    parent 3e6070eb2062746861b20e1e6235fed6f6d15609
    author user1 <user1@sun.ossxp.com> 1293790794 +0800
    committer user1 <user1@sun.ossxp.com> 1293790794 +0800

    blank commit.

    ```

**轻量级里程碑的缺点**

轻量级里程碑的创建过程没有记录，因此无法知道是谁创建的里程碑，是何时创建的里程碑。在团队协同开发时，尽量不要采用此种偷懒的方式创建里程碑，而是采用后两种方式。

还有**git describe**命令默认不使用轻量级里程碑生成版本描述字符串。

*   执行**git describe**命令，发现生成的版本描述字符串，使用的是前一个版本上的里程碑名称。

    ```
    $ git describe
    jx/v1.0-1-g60a2f4f

    ```

*   使用`--tags`参数，也可以将轻量级里程碑用作版本描述符。

    ```
    $ git describe --tags
    mytag

    ```

### 3.3.2.2\. 带说明的里程碑

带说明的里程碑，就是使用参数`-a`或者`-m <msg>`调用**git tag**命令，在创建里程碑的时候提供一个关于该里程碑的说明。下面来看看如何创建带说明的里程碑：

*   还是先创建一个空提交。

    ```
    $ git commit --allow-empty -m "blank commit for annotated tag test."
    [master 8a9f3d1] blank commit for annotated tag test.

    ```

*   在刚刚创建的空提交上创建一个带说明的里程碑，名为`mytag2`。

    下面的命令使用了`-m <msg>`参数在命令行给出了新建里程碑的说明。

    ```
    $ git tag -m "My first annotated tag." mytag2

    ```

*   查看里程碑，可以看到该里程碑已经创建。

    ```
    $ git tag -l my* -n1
    mytag           blank commit.
    mytag2          My first annotated tag.

    ```

**带说明里程碑的奥秘**

当创建了带说明的里程碑`mytag2`后，会在版本库的`.git/refs/tags`目录下创建一个新的引用文件。

*   查看一下这个引用文件的内容：

    ```
    $ cat .git/refs/tags/mytag2
    149b6344e80fc190bda5621cd71df391d3dd465e

    ```

*   用**git cat-file**命令检查该里程碑（带说明的里程碑）指向的对象，会发现指向的不再是一个提交，而是一个 tag 对象。

    ```
    $ git cat-file -t mytag2
    tag

    ```

*   查看该提交的内容，会发现 mytag2 对象的内容不是之前我们熟悉的提交对象的内容，而是包含了创建里程碑时的说明，以及对应的提交 ID 等信息。

    ```
    $ git cat-file -p mytag2
    object 8a9f3d16ce2b4d39b5d694de10311207f289153f
    type commit
    tag mytag2
    tagger user1 <user1@sun.ossxp.com> Sun Jan 2 14:10:07 2011 +0800

    My first annotated tag.

    ```

由此可见使用带说明的里程碑，会在版本库中建立一个新的对象（tag 对象），这个对象会记录创建里程碑的用户（tagger），创建里程碑的时间，以及为什么要创建里程碑。这就避免了轻量级里程碑因为匿名创建而无法追踪的缺点。

带说明的里程碑是一个 tag 对象，在版本库中以一个对象的方式存在，并用一个 40 位的 SHA1 哈希值来表示。这个哈希值的生成方法和前面介绍的 commit 对象、tree 对象、blob 对象一样。至此，Git 对象库的四类对象我们就都已经研究到了。

```
$ git cat-file tag mytag2 | wc -c
148
$ (printf "tag 148\000"; git cat-file tag mytag2) | sha1sum
149b6344e80fc190bda5621cd71df391d3dd465e  -

```

虽然 mytag2 本身是一个 tag 对象，但在很多 Git 命令中，可以直接将其视为一个提交。下面的**git log**命令，显示 mytag2 指向的提交日志。

```
$ git log -1 --pretty=oneline mytag2
8a9f3d16ce2b4d39b5d694de10311207f289153f blank commit for annotated tag test.

```

有时，需要得到里程碑指向的提交对象的 SHA1 哈希值。

*   直接用**git rev-parse**命令查看 mytag2 得到的是 tag 对象的 ID，并非提交对象的 ID。

    ```
    $ git rev-parse mytag2
    149b6344e80fc190bda5621cd71df391d3dd465e

    ```

*   使用下面几种不同的表示法，则可以获得 mytag2 对象所指向的提交对象的 ID。

    ```
    $ git rev-parse mytag2^{commit}
    8a9f3d16ce2b4d39b5d694de10311207f289153f
    $ git rev-parse mytag2^{}
    8a9f3d16ce2b4d39b5d694de10311207f289153f
    $ git rev-parse mytag2⁰
    8a9f3d16ce2b4d39b5d694de10311207f289153f
    $ git rev-parse mytag2~0
    8a9f3d16ce2b4d39b5d694de10311207f289153f

    ```

### 3.3.2.3\. 带签名的里程碑

带签名的里程碑和上面介绍的带说明的里程碑本质上是一样的，都是在创建里程碑的时候在 Git 对象库中生成一个 tag 对象，只不过带签名的里程碑多做了一个工作：为里程碑对象添加 GnuPG 签名。

创建带签名的里程碑也非常简单，使用参数`-s`或`-u <key-id>`即可。还可以使用`-m <msg>`参数直接在命令行中提供里程碑的描述。创建带签名里程碑的一个前提是需要安装 GnuPG，并且建立相应的公钥/私钥对。

GnuPG 可以在各个平台上安装。

*   在 Linux，如 Debian/Ubuntu 上安装，执行：

    ```
    $ sudo aptitude install gnupg

    ```

*   在 Mac OS X 上，可以通过 Homebrew 安装：

    ```
    $ brew install gnupg

    ```

*   在 Windows 上可以通过 cygwin 安装 gnupg。

为了演示创建带签名的里程碑，还是事先创建一个空提交。

```
$ git commit --allow-empty -m "blank commit for GnuPG-signed tag test."
[master ebcf6d6] blank commit for GnuPG-signed tag test.

```

直接在刚刚创建的空提交上创建一个带签名的里程碑`mytag2`很可能会失败。

```
$ git tag -s -m "My first GPG-signed tag." mytag3
gpg: “user1 <user1@sun.ossxp.com>”已跳过：私钥不可用
gpg: signing failed: 私钥不可用
error: gpg failed to sign the tag
error: unable to sign the tag

```

之所以签名失败，是因为找不到签名可用的公钥/私钥对。使用下面的命令可以查看当前可用的 GnuPG 公钥。

```
$ gpg --list-keys
/home/jiangxin/.gnupg/pubring.gpg
---------------------------------
pub   1024D/FBC49D01 2006-12-21 [有效至：2016-12-18]
uid                  Jiang Xin <worldhello.net@gmail.com>
uid                  Jiang Xin <jiangxin@ossxp.com>
sub   2048g/448713EB 2006-12-21 [有效至：2016-12-18]

```

可以看到 GnuPG 的公钥链（pubring）中只包含了`Jiang Xin`用户的公钥，尚没有`uesr1`用户的公钥。

实际上在创建带签名的里程碑时，并非一定要使用邮件名匹配的公钥/私钥对进行签名，使用`-u <key-id>`参数调用就可以用指定的公钥/私钥对进行签名，对于此例可以使用`FBC49D01`作为`<key-id>`。但如果没有可用的公钥/私钥对，或者希望使用提交者本人的公钥/私钥对进行签名，就需要为提交者:`user1 <user1@sun.ossxp.com>`创建对应的公钥/私钥对。

使用命令**gpg --gen-key**来创建公钥/私钥对。

```
$ gpg --gen-key

```

按照提示一步一步操作即可。需要注意的有：

*   在创建公钥/私钥对时，会提示输入用户名，输入`User1`，提示输入邮件地址，输入`user1@sun.ossxp.com`，其他可以采用默认值。
*   在提示输入密码时，为了简单起见可以直接按下回车，即使用空口令。
*   在生成公钥私钥对过程中，会提示用户做一些随机操作以便产生更好的随机数，这时不停的晃动鼠标就可以了。

创建完毕，再查看一下公钥链。

```
$ gpg --list-keys
/home/jiangxin/.gnupg/pubring.gpg
---------------------------------
pub   1024D/FBC49D01 2006-12-21 [有效至：2016-12-18]
uid                  Jiang Xin <worldhello.net@gmail.com>
uid                  Jiang Xin <jiangxin@ossxp.com>
sub   2048g/448713EB 2006-12-21 [有效至：2016-12-18]

pub   2048R/37379C67 2011-01-02
uid                  User1 <user1@sun.ossxp.com>
sub   2048R/2FCFB3E2 2011-01-02

```

很显然用户 user1 的公钥私钥对已经建立。现在就可以直接使用`-s`参数来创建带签名里程碑了。

```
$ git tag -s -m "My first GPG-signed tag." mytag3

```

查看里程碑，可以看到该里程碑已经创建。

```
$ git tag -l my* -n1
mytag           blank commit.
mytag2          My first annotated tag.
mytag3          My first GPG-signed tag.

```

和带说明的里程碑一样，在 Git 对象库中也建立了一个 tag 对象。查看该 tag 对象可以看到其中包含了 GnuPG 签名。

```
$ git cat-file tag mytag3
object ebcf6d6b06545331df156687ca2940800a3c599d
type commit
tag mytag3
tagger user1 <user1@sun.ossxp.com> 1293960936 +0800

My first GPG-signed tag.
-----BEGIN PGP SIGNATURE-----
Version: GnuPG v1.4.10 (GNU/Linux)

iQEcBAABAgAGBQJNIEboAAoJEO9W1fg3N5xn42gH/jFDEKobqlupNKFvmkI1t9d6
lApDFUdcFMPWvxo/eq8VjcQyRcb1X1bGJj+pxXk455fDL1NWonaJa6HE6RLu868x
CQIWqWelkCelfm05GE9FnPd2SmJsiDkTPZzINya1HylF5ZbrExH506JyCFk//FC2
8zRApSbrsj3yAWMStW0fGqHKLuYq+sdepzGnnFnhhzkJhusMHUkTIfpLwaprhMsm
1IIxKNm9i0Zf/tzq4a/R0N8NiFHl/9M95iV200I9PuuRWedV0tEPS6Onax2yT3JE
I/w9gtIBOeb5uAz2Xrt5AUwt9JJTk5mmv2HBqWCq5wefxs/ub26iPmef35PwAgA=
=jdrN
-----END PGP SIGNATURE-----

```

要验证签名的有效性，如果直接使用 gpg 命令会比较麻烦，因为需要将这个文件拆分为两个，一个是不包含签名的里程碑内容，另外一个是签名本身。还好可以使用命令**git tag -v**来验证里程碑签名的有效性。

```
$ git tag -v mytag3
object ebcf6d6b06545331df156687ca2940800a3c599d
type commit
tag mytag3
tagger user1 <user1@sun.ossxp.com> 1293960936 +0800

My first GPG-signed tag.
gpg: 于 2011 年 01 月 02 日 星期日 17 时 35 分 36 秒 CST 创建的签名，使用 RSA，钥匙号 37379C67

```

## 3.3.3\. 删除里程碑

如果里程碑建立在了错误的提交上，或者对里程碑的命名不满意，可以删除里程碑。删除里程碑使用命令**git tag -d**，下面用命令删除里程碑`mytag`。

```
$ git tag -d mytag
Deleted tag 'mytag' (was 60a2f4f)

```

里程碑没有类似 reflog 的变更记录机制，一旦删除不易恢复，慎用。在删除里程碑`mytag`的命令输出中，会显示该里程碑所对应的提交 ID，一旦发现删除错误，赶紧补救还来得及。下面的命令实现对里程碑`mytag`的重建。

```
$ git tag mytag 60a2f4f

```

**为什么没有重命名里程碑的命令？**

Git 没有提供对里程碑直接重命名的命令，如果对里程碑名字不满意的话，可以删除旧的里程碑，然后重新用新的里程碑进行命名。

为什么没有提供重命名里程碑的命令呢？按理说只要将`.git/refs/tags/`下的引用文件改名就可以了。这是因为里程碑的名字不但反映在`.git/refs/tags`引用目录下的文件名，而且对于带说明或签名的里程碑，里程碑的名字还反映在 tag 对象的内容中。尤其是带签名的里程碑，如果修改里程碑的名字，不但里程碑对象 ID 势必要变化，而且里程碑也要重新进行签名，这显然难以自动实现。

在第 6 篇第三十五章的“Git 版本库整理”一节中会介绍使用**git filter-branch**命令实现对里程碑自动重命名的方法，但是那个方法也不能毫发无损地实现对签名里程碑的重命名，被重命名的签名里程碑中的签名会被去除从而成为带说明的里程碑。

## 3.3.4\. 不要随意更改里程碑

里程碑建立后，如果需要修改，可以使用同样的里程碑名称重新建立，不过需要加上`-f`或`--force`参数强制覆盖已有的里程碑。

更改里程碑要慎重，一个原因是里程碑从概念上讲是对历史提交的一个标记，不应该随意变动。另外一个原因是里程碑一旦被他人同步，如果修改里程碑，已经同步该里程碑的用户不会自动更新，这就导致一个相同名称的里程碑在不同用户的版本库中的指向不同。下面就看看如何与他人共享里程碑。

## 3.3.5\. 共享里程碑

现在看看用户 user1 的工作区状态。可以看出现在的工作区相比上游有三个新的提交。

```
$ git status
# On branch master
# Your branch is ahead of 'origin/master' by 3 commits.
#
nothing to commit (working directory clean)

```

那么如果执行**git push**命令向上游推送，会将本地创建的三个里程碑推送到上游么？通过下面的操作来试一试。

*   向上游推送。

    ```
    $ git push
    Counting objects: 3, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (3/3), done.
    Writing objects: 100% (3/3), 512 bytes, done.
    Total 3 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (3/3), done.
    To file:///path/to/repos/hello-world.git
       3e6070e..ebcf6d6  master -> master

    ```

*   通过执行**git ls-remote**可以查看上游版本库的引用，会发现本地建立的三个里程碑，并没有推送到上游。

    ```
    $ git ls-remote origin my*

    ```

创建的里程碑，默认只在本地版本库中可见，不会因为对分支执行推送而将里程碑也推送到远程版本库。这样的设计显然更为合理，否则的话，每个用户本地创建的里程碑都自动向上游推送，那么上游的里程碑将有多么杂乱，而且不同用户创建的相同名称的里程碑会互相覆盖。

**那么如何共享里程碑呢？**

如果用户确实需要将某些本地建立的里程碑推送到远程版本库，需要在**git push**命令中明确地表示出来。下面在用户 user1 的工作区执行命令，将`mytag`里程碑共享到上游版本库。

```
$ git push origin mytag
Total 0 (delta 0), reused 0 (delta 0)
To file:///path/to/repos/hello-world.git
 * [new tag]         mytag -> mytag

```

如果需要将本地建立的所有里程碑全部推送到远程版本库，可以使用通配符。

```
$ git push origin refs/tags/*
Counting objects: 2, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (2/2), 687 bytes, done.
Total 2 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (2/2), done.
To file:///path/to/repos/hello-world.git
 * [new tag]         mytag2 -> mytag2
 * [new tag]         mytag3 -> mytag3

```

再用命令**git ls-remote**查看上游版本库的引用，会发现本地建立的三个里程碑，已经能够在上游中看到了。

```
$ git ls-remote origin my*
60a2f4f31e5dddd777c6ad37388fe6e5520734cb        refs/tags/mytag
149b6344e80fc190bda5621cd71df391d3dd465e        refs/tags/mytag2
8a9f3d16ce2b4d39b5d694de10311207f289153f        refs/tags/mytag2^{}
5dc2fc52f2dcb84987f511481cc6b71ec1b381f7        refs/tags/mytag3
ebcf6d6b06545331df156687ca2940800a3c599d        refs/tags/mytag3^{}

```

**用户从版本库执行拉回操作，会自动获取里程碑么？**

用户 user2 的工作区中如果执行**git fetch**或**git pull**操作，能自动将用户 user1 推送到共享版本库中的里程碑获取到本地版本库么？下面实践一下。

*   进入 user2 的工作区。

    ```
    $ cd /path/to/user2/workspace/hello-world/

    ```

*   执行**git pull**命令，从上游版本库获取提交。

    ```
    $ git pull
    remote: Counting objects: 5, done.
    remote: Compressing objects: 100% (5/5), done.
    remote: Total 5 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (5/5), done.
    From file:///path/to/repos/hello-world
       3e6070e..ebcf6d6  master     -> origin/master
     * [new tag]         mytag3     -> mytag3
    From file:///path/to/repos/hello-world
     * [new tag]         mytag      -> mytag
     * [new tag]         mytag2     -> mytag2
    Updating 3e6070e..ebcf6d6
    Fast-forward

    ```

*   可见执行**git pull**操作，能够在获取远程共享版本库的提交的同时，获取新的里程碑。下面的命令可以看到本地版本库中的里程碑。

    ```
    $ git tag -n1 -l my*
    mytag           blank commit.
    mytag2          My first annotated tag.
    mytag3          My first GPG-signed tag.

    ```

**里程碑变更能够自动同步么？**

里程碑可以被强制更新。当里程碑被改变后，已经获取到里程碑的版本库再次使用获取或拉回操作，能够自动更新里程碑么？答案是不能。可以看看下面的操作。

*   用户 user2 强制更新里程碑`mytag2`。

    ```
    $ git tag -f -m "user2 update this annotated tag." mytag2 HEAD^
    Updated tag 'mytag2' (was 149b634)

    ```

*   里程碑`mytag2`已经是不同的对象了。

    ```
    $ git rev-parse mytag2
    0e6c780ff0fe06635394db9dac6fb494833df8df
    $ git cat-file -p mytag2
    object 8a9f3d16ce2b4d39b5d694de10311207f289153f
    type commit
    tag mytag2
    tagger user2 <user2@moon.ossxp.com> Mon Jan 3 01:14:18 2011 +0800

    user2 update this annotated tag.

    ```

*   为了更改远程共享服务器中的里程碑，同样需要显式推送。即在推送时写上要推送的里程碑名称。

    ```
    $ git push origin mytag2
    Counting objects: 1, done.
    Writing objects: 100% (1/1), 171 bytes, done.
    Total 1 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (1/1), done.
    To file:///path/to/repos/hello-world.git
       149b634..0e6c780  mytag2 -> mytag2

    ```

*   切换到另外一个用户 user1 的工作区。

    ```
    $ cd /path/to/user1/workspace/hello-world/

    ```

*   用户 user1 执行拉回操作，没有获取到新的里程碑。

    ```
    $ git pull
    Already up-to-date.

    ```

*   用户 user1 必须显式地执行拉回操作。即要在**git pull**的参数中使用引用表达式。

    所谓引用表达式就是用冒号分隔的引用名称或通配符。用在这里代表用远程共享版本库的引用`refs/tag/mytag2`覆盖本地版本库的同名引用。

    ```
    $ git pull origin refs/tags/mytag2:refs/tags/mytag2
    remote: Counting objects: 1, done.
    remote: Total 1 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (1/1), done.
    From file:///path/to/repos/hello-world
     - [tag update]      mytag2     -> mytag2
    Already up-to-date.

    ```

关于里程碑的共享和同步操作，看似很繁琐，但用心体会就会感觉到 Git 关于里程碑共享的设计是非常合理和人性化的：

*   里程碑共享，必须显式的推送。即在推送命令的参数中，标明要推送哪个里程碑。

    显式推送是防止用户随意推送里程碑导致共享版本库中里程碑泛滥的方法。当然还可以参考第 5 篇“第三十章 Gitolite 服务架设”的相关章节为共享版本库添加授权，只允许部分用户向服务器推送里程碑。

*   执行获取或拉回操作，自动从远程版本库获取新里程碑，并在本地版本库中创建。

    获取或拉回操作，只会将获取的远程分支所包含的新里程碑同步到本地，而不会将远程版本库的其他分支中的里程碑获取到本地。这既方便了里程碑的取得，又防止本地里程碑因同步远程版本库而泛滥。

*   如果本地已有同名的里程碑，默认不会从上游同步里程碑，即使两者里程碑的指向是不同的。

    理解这一点非常重要。这也就要求里程碑一旦共享，就不要再修改。

## 3.3.6\. 删除远程版本库的里程碑

假如向远程版本库推送里程碑后，忽然发现里程碑创建在了错误的提交上，为了防止其他人获取到错误的里程碑，应该尽快将里程碑删除。

删除本地里程碑非常简单，使用**git tag -d <tagname>**就可以了，但是如何撤销已经推送到远程版本库的里程碑呢？需要登录到服务器上么？或者需要麻烦管理员么？不必！可以直接在本地版本库执行命令删除远程版本库的里程碑。

使用**git push**命令可以删除远程版本库中的里程碑。用法如下：

```
命令： git push <remote_url>  :<tagname>

```

该命令的最后一个参数实际上是一个引用表达式，引用表达式一般的格式为`<ref>:<ref>`。该推送命令使用的引用表达式冒号前的引用被省略，其含义是将一个空值推送到远程版本库对应的引用中，亦即删除远程版本库中相关的引用。这个命令不但可以用于删除里程碑，在下一章还可以用它删除远程版本库中的分支。

下面演示在用户 user1 的工作区执行下面的命令删除远程共享版本库中的里程碑`mytag2`。

*   切换到用户 user1 工作区。

    ```
    $ cd /path/to/user1/workspace/hello-world

    ```

*   执行推送操作删除远程共享版本库中的里程碑。

    ```
    $ git push origin :mytag2
    To file:///path/to/repos/hello-world.git
     - [deleted]         mytag2

    ```

*   查看远程共享库中的里程碑，发现`mytag2`的确已经被删除。

    ```
    $ git ls-remote origin my*
    60a2f4f31e5dddd777c6ad37388fe6e5520734cb        refs/tags/mytag
    5dc2fc52f2dcb84987f511481cc6b71ec1b381f7        refs/tags/mytag3
    ebcf6d6b06545331df156687ca2940800a3c599d        refs/tags/mytag3^{}

    ```

## 3.3.7\. 里程碑命名规范

在正式项目的版本库管理中，要为里程碑创建订立一些规则，诸如：

*   对创建里程碑进行权限控制，参考后面 Git 服务器架设的相关章节。
*   不能使用轻量级里程碑（只用于本地临时性里程碑），必须使用带说明的里程碑，甚至要求必须使用带签名的里程碑。
*   如果使用带签名的里程碑，可以考虑设置专用账户，使用专用的私钥创建签名。
*   里程碑的命名要使用统一的风格，并很容易和最终产品显示的版本号相对应。

Git 的里程碑命名还有一些特殊的约定需要遵守。实际上，下面的这些约定对于下一章要介绍的分支及任何其他引用均适用：

*   不能以符号“-”开头。以免在命令行中被当成命令的选项。

*   可以包含路径分隔符“/”，但是路径分隔符不能位于最后。

    使用路径分隔符创建 tag 实际上会在引用目录下创建子目录。例如名为`demo/v1.2.1`的里程碑，就会创建目录`.git/refs/tags/demo`并在该目录下创建引用文件`v1.2.1`。

*   不能出现两个连续的点“..”。因为两个连续的点被用于表示版本范围，当然更不能使用三个连续的点。

*   如果在里程碑命名中使用了路径分隔符“/”，就不能在任何一个分隔路径中以点“.”开头。

    这是因为里程碑在用简写格式表达时，可能造成以一个点“.”开头。这样的引用名称在用作版本范围的最后一个版本时，本来两点操作符变成了三点操作符，从而造成歧义。

*   不能在里程碑名称的最后出现点“.”。否则作为第一个参数出现在表示版本范围的表达式中时，本来版本范围表达式可能用的是两点操作符，结果被误作三点操作符。

*   不能使用特殊字符，如：空格、波浪线“~”、脱字符“^”、冒号“:”、问号“?”、星号“*”、方括号“[”，以及字符`\\177`（删除字符）或小于`\\040`（32）的 Ascii 码都不能使用。

    这是因为波浪线“~”和脱字符“^”都用于表示一个提交的祖先提交。

    冒号被用作引用表达式来分隔两个不同的引用，或者用于分隔引用代表的树对象和该目录树中的文件。

    问号、星号和方括号在引用表达式中都被用作通配符。

*   不能以“.lock”为结尾。因为以“.lock”结尾的文件是里程碑操作过程中的临时文件。

*   不能包含“@{”字串。因为 reflog 采用“@{<num>”作为语法的一部分。

*   不能包含反斜线“\”。因为反斜线用于命令行或 shell 脚本会造成意外。

**Linux 中的里程碑**

Linux 项目无疑是使用 Git 版本库时间最久远，也是最重量级的项目。研究 Linux 项目本身的里程碑命名和管理，无疑会为自己的项目提供借鉴。

*   首先看看 Linux 中的里程碑命名。可以看到里程碑都是以字母`v`开头。

    ```
    $ git ls-remote --tags \
      git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-2.6-stable.git \
      v2.6.36*
    25427f38d3b791d986812cb81c68df38e8249ef8        refs/tags/v2.6.36
    f6f94e2ab1b33f0082ac22d71f66385a60d8157f        refs/tags/v2.6.36^{}
    8ed88d401f908a594cd74a4f2513b0fabd32b699        refs/tags/v2.6.36-rc1
    da5cabf80e2433131bf0ed8993abc0f7ea618c73        refs/tags/v2.6.36-rc1^{}
    ...
    7619e63f48822b2c68d0e108677340573873fb93        refs/tags/v2.6.36-rc8
    cd07202cc8262e1669edff0d97715f3dd9260917        refs/tags/v2.6.36-rc8^{}
    9d389cb6dcae347cfcdadf2a1ec5e66fc7a667ea        refs/tags/v2.6.36.1
    bf6ef02e53e18dd14798537e530e00b80435ee86        refs/tags/v2.6.36.1^{}
    ee7b38c91f3d718ea4035a331c24a56553e90960        refs/tags/v2.6.36.2
    a1346c99fc89f2b3d35c7d7e2e4aef8ea4124342        refs/tags/v2.6.36.2^{}

    ```

*   以`-rc<num>`为后缀的是先于正式版发布的预发布版本。

    可以看出这个里程碑是一个带签名的里程碑。关于此里程碑的说明也是再简练不过了。

    ```
    $ git show v2.6.36-rc1
    tag v2.6.36-rc1
    Tagger: Linus Torvalds <torvalds@linux-foundation.org>
    Date:   Sun Aug 15 17:42:10 2010 -0700

    Linux 2.6.36-rc1
    -----BEGIN PGP SIGNATURE-----
    Version: GnuPG v1.4.10 (GNU/Linux)

    iEYEABECAAYFAkxoiWgACgkQF3YsRnbiHLtYKQCfQSIVcj2hvLj6IWgP9xK2FE7T
    bPoAniJ1CjbwLxQBudRi71FvubqPLuVC
    =iuls
    -----END PGP SIGNATURE-----

    commit da5cabf80e2433131bf0ed8993abc0f7ea618c73
    Author: Linus Torvalds <torvalds@linux-foundation.org>
    Date:   Sun Aug 15 17:41:37 2010 -0700

        Linux 2.6.36-rc1

    diff --git a/Makefile b/Makefile
    index 788111d..f3bdff8 100644
    --- a/Makefile
    +++ b/Makefile
    @@ -1,7 +1,7 @@
     VERSION = 2
     PATCHLEVEL = 6
    -SUBLEVEL = 35
    -EXTRAVERSION =
    +SUBLEVEL = 36
    +EXTRAVERSION = -rc1
     NAME = Sheep on Meth

     # *DOCUMENTATION*

    ```

*   正式发布版去掉了预发布版的后缀。

    ```
    $ git show v2.6.36
    tag v2.6.36
    Tagger: Linus Torvalds <torvalds@linux-foundation.org>
    Date:   Wed Oct 20 13:31:18 2010 -0700

    Linux 2.6.36

    The latest and greatest, and totally bug-free.  At least until 2.6.37
    comes along and shoves it under a speeding train like some kind of a
    bully.
    -----BEGIN PGP SIGNATURE-----
    Version: GnuPG v1.4.10 (GNU/Linux)

    iEYEABECAAYFAky/UcwACgkQF3YsRnbiHLvg/ACffKjAb1fD6fpqcHbSijHHpbP3
    4SkAnR4xOy7iKhmfS50ZrVsOkFFTuBHG
    =JD3z
    -----END PGP SIGNATURE-----

    commit f6f94e2ab1b33f0082ac22d71f66385a60d8157f
    Author: Linus Torvalds <torvalds@linux-foundation.org>
    Date:   Wed Oct 20 13:30:22 2010 -0700

        Linux 2.6.36

    diff --git a/Makefile b/Makefile
    index 7583116..860c26a 100644
    --- a/Makefile
    +++ b/Makefile
    @@ -1,7 +1,7 @@
     VERSION = 2
     PATCHLEVEL = 6
     SUBLEVEL = 36
    -EXTRAVERSION = -rc8
    +EXTRAVERSION =
     NAME = Flesh-Eating Bats with Fangs

     # *DOCUMENTATION*

    ```

*   正式发布后的升级/修正版本是通过最后一位数字的变动体现的。

    ```
    $ git show v2.6.36.1
    tag v2.6.36.1
    Tagger: Greg Kroah-Hartman <gregkh@suse.de>
    Date:   Mon Nov 22 11:04:17 2010 -0800

    This is the 2.6.36.1 stable release
    -----BEGIN PGP SIGNATURE-----
    Version: GnuPG v2.0.15 (GNU/Linux)

    iEYEABECAAYFAkzqvrIACgkQMUfUDdst+ym9VQCgmE1LK2eC/LE9HkscsxL1X62P
    8F0AnRI28EHENLXC+FBPt+AFWoT9f1N8
    =BX5O
    -----END PGP SIGNATURE-----

    commit bf6ef02e53e18dd14798537e530e00b80435ee86
    Author: Greg Kroah-Hartman <gregkh@suse.de>
    Date:   Mon Nov 22 11:03:49 2010 -0800

        Linux 2.6.36.1

    diff --git a/Makefile b/Makefile
    index 860c26a..dafd22a 100644
    --- a/Makefile
    +++ b/Makefile
    @@ -1,7 +1,7 @@
     VERSION = 2
     PATCHLEVEL = 6
     SUBLEVEL = 36
    -EXTRAVERSION =
    +EXTRAVERSION = .1
     NAME = Flesh-Eating Bats with Fangs

     # *DOCUMENTATION*

    ```

**Android 项目**

看看其他项目的里程碑命名，会发现不同项目关于里程碑的命名各不相同。但是对于同一个项目要在里程碑命名上遵照同一标准，并能够和软件版本号正确地对应。

Android 项目是一个非常有特色的使用 Git 版本库的项目，在后面会用两章介绍 Android 项目为 Git 带来的两个新工具。看看 Android 项目的里程碑编号对自己版本库的管理有无启发。

*   看看 Android 项目中的里程碑命名，会发现其里程碑的命名格式为`android-<大版本号>_r<小版本号>`。

    ```
    $ git ls-remote --tags \
      git://android.git.kernel.org/platform/manifest.git \
      android-2.2*
    6a03ae8f564130cbb4a11acfc49bd705df7c8df6        refs/tags/android-2.2.1_r1
    599e242dea48f84e2f26054b0d1721e489043440        refs/tags/android-2.2.1_r1^{}
    656ba6fdbd243153af6ec31017de38641060bf1e        refs/tags/android-2.2_r1
    27cd0e346d1f3420c5747e01d2cb35e9ffd025ea        refs/tags/android-2.2_r1^{}
    f6b7c499be268f1613d8cd70f2a05c12e01bcb93        refs/tags/android-2.2_r1.1
    bd3e9923773006a0a5f782e1f21413034096c4b1        refs/tags/android-2.2_r1.1^{}
    03618e01ec9bdd06fd8fe9afdbdcbaf4b84092c5        refs/tags/android-2.2_r1.2
    ba7111e1d6fd26ab150bafa029fd5eab8196dad1        refs/tags/android-2.2_r1.2^{}
    e03485e978ce1662a1285837f37ed39eadaedb1d        refs/tags/android-2.2_r1.3
    7386d2d07956be6e4f49a7e83eafb12215e835d7        refs/tags/android-2.2_r1.3^{}

    ```

*   里程碑的创建过程中使用了专用帐号和 GnuPG 签名。

    ```
    $ git show android-2.2_r1
    tag android-2.2_r1
    Tagger: The Android Open Source Project <initial-contribution@android.com>
    Date:   Tue Jun 29 11:28:52 2010 -0700

    Android 2.2 release 1
    -----BEGIN PGP SIGNATURE-----
    Version: GnuPG v1.4.6 (GNU/Linux)

    iD8DBQBMKjtm6K0/gZqxDngRAlBUAJ9QwgFbUL592FgRZLTLLbzhKsSQ8ACffQu5
    Mjxg5X9oc+7N1DfdU+pmOcI=
    =0NG0
    -----END PGP SIGNATURE-----

    commit 27cd0e346d1f3420c5747e01d2cb35e9ffd025ea
    Author: The Android Open Source Project <initial-contribution@android.com>
    Date:   Tue Jun 29 11:27:23 2010 -0700

        Manifest for android-2.2_r1

    diff --git a/default.xml b/default.xml
    index 4f21453..aaa26e3 100644
    --- a/default.xml
    +++ b/default.xml
    @@ -3,7 +3,7 @@
       <remote  name="korg"
                fetch="git://android.git.kernel.org/"
                review="review.source.android.com" />
    -  <default revision="froyo"
    +  <default revision="refs/tags/android-2.2_r1"
                remote="korg" />
    ...

    ```

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 3.4\. Git 分支

分支是我们的老朋友了，第 2 篇中的“第六章 Git 对象库”、“第七章 Git 重置”和“第八章 Git 检出”等章节中，就已经从实现原理上理解了分支。您想必已经知道了分支`master`的存在方式无非就是在目录`.git/refs/heads`下的文件（或称引用）而已。也看到了分支`master`的指向如何随着提交而变化，如何通过**git reset**命令而重置，以及如何使用**git checkout**命令而检出。

之前的章节都只用到了一个分支：`master`分支，而在本章会接触到多个分支。会从应用的角度上介绍分支的几种不同类型：发布分支、特性分支和卖主分支。在本章可以学习到如何对多分支进行操作，如何创建分支，如何切换到其他分支，以及分支之间的合并、变基等。

## 3.4.1\. 代码管理之殇

分支是代码管理的利器。如果没有有效的分支管理，代码管理就适应不了复杂的开发过程和项目的需要。在实际的项目实践中，单一分支的单线开发模式还远远不够，因为：

*   成功的软件项目大多要经过多个开发周期，发布多个软件版本。每个已经发布的版本都可能发现 bug，这就需要对历史版本进行更改。
*   有前瞻性的项目管理，新版本的开发往往是和当前版本同步进行的。如果两个版本的开发都混杂在 master 分支中，肯定会是一场灾难。
*   如果产品要针对不同的客户定制，肯定是希望客户越多越好。如果所有的客户定制都混杂在一个分支中，必定会带来混乱。如果使用多个分支管理不同的定制，但如果管理不善，分支之间定制功能的迁移就会成为头痛的问题。
*   即便是所有成员都在为同一个项目的同一个版本进行工作，每个人领受任务却不尽相同，有的任务开发周期会很长，有的任务需要对软件架构进行较大的修改，如果所有人都工作在同一分支中，就会因为过多过频的冲突导致效率低下。
*   敏捷开发（不管是极限编程 XP 还是 Scrum 或其他）是最有效的项目管理模式，其最有效的一个实践就是快速迭代、每晚编译。如果不能将项目的各个功能模块的开发通过分支进行隔离，在软件集成上就会遭遇困难。

### 3.4.1.1\. 发布分支

**为什么 bug 没完没了？**

在 2006 年我接触到一个项目团队，使用 Subversion 做版本控制。最为困扰项目经理的是刚刚修正产品的一个 bug，马上又会接二连三地发现新的 bug。在访谈开发人员，询问开发人员是如何修正 bug 的时候，开发人员的回答让我大吃一惊：“当发现产品出现 bug 的时候，我要中断当前的工作，把我正在开发的新功能的代码注释掉，然后再去修改 bug，修改好就生成一个 war 包（Java 开发网站项目）给运维部门，扔到网站上去。”

于是我就画了下面的一个图（图 18-1），大致描述了这个团队进行 bug 修正的过程，从中可以很容易地看出问题的端倪。这个图对于 Git 甚至其他版本库控制系统同样适用。

![../_images/branch-release-branch-question.png](img/branch-release-branch-question.png)

图 18-1：没有使用分支导致越改越多的 bug

说明：

*   图 18-1 中的图示①，开发者针对功能 1 做了一个提交，编号“F1.1”。这时客户报告产品出现了 bug。
*   于是开发者匆忙地干了起来，图示②显示了该开发者修正 bug 的过程：将已经提交的针对功能 1 的代码“F1.1”注释掉，然后提交一个修正 bug 的提交（编号：fix1）。
*   开发者编译出新的产品交给客户，接着开始功能 1 的开发。图示③显示了开发者针对功能 1 做出了一个新的提交“F1.2”。
*   客户再次发现一个 bug。开发者再次开始 bug 修正工作。
*   图示④和图示⑤显示了此工作模式下非常容易在修复一个 bug 的时候引入新的 bug。
*   图示④的问题在于开发者注释功能 1 的代码时，不小心将“fix1”的代码也注释掉了，导致曾经修复的 bug 在新版本中重现。
*   图示⑤的问题在于开发者没有将功能 1 的代码剔出干净，导致在产品的新版本中引入了不完整和不需要的功能代码。用户可能看到一个新的但是不能使用的菜单项，甚至更糟。

使用版本控制系统的分支功能，可以避免对已发布的软件版本进行 bug 修正时引入新功能的代码，或者因误删其他 bug 修正代码导致已修复问题重现。在这种情况下创建的分支有一个专有的名称：bugfix 分支或发布分支（Release Branch）。之所以称为发布分支，是因为在软件新版本发布后经常使用此技术进行软件维护，发布升级版本。

图 18-2 演示了如何使用发布分支应对 bug 修正的问题。

![../_images/branch-release-branch-answer.png](img/branch-release-branch-answer.png)

图 18-2：使用发布分支的 bug 修正过程

说明：

*   图 18-2 中的图示②，可以看到开发者创建了一个发布分支（bugfix 分支），在分支中提交修正代码“fix1”。注意此分支是自上次软件发布时最后一次提交进行创建的，因此分支中没有包含开发者为新功能所做的提交“F1.1”，是一个“干净”的分支。
*   图示③可以看出从发布分支向主线做了一次合并，这是因为在主线上也同样存在该 bug，需要在主线上也做出相应的更改。
*   图示④，开发者继续开发，针对功能 1 执行了一个新的提交，编号“F1.2”。这时，客户报告有新的 bug。
*   继续在发布分支上进行 bug 修正，参考图示⑤。当修正完成（提交“fix2”）时，基于发布分支创建一个新的软件版本发给客户。不要忘了向主线合并，因为同样的 bug 可能在主线上也存在。

关于如何基于一个历史提交创建分支，以及如何在分支之间进行合并，在本章后面的内容中会详细介绍。

### 3.4.1.2\. 特性分支

**为什么项目一再的拖延？**

有这么一个软件项目，项目已经延期了可是还是看不到一点要完成的样子。最终老板变得有些不耐烦了，说道：“那么就砍掉一些功能吧”。项目经理听闻，一阵眩晕，因为项目经理知道自己负责的这个项目采用的是单一主线开发，要将一个功能从中撤销，工作量非常大，而且可能会牵涉到其他相关模块的变更。

图 18-3 就是这个项目的版本库示意图，显然这个项目的代码管理没有使用分支。

![../_images/branch-feature-branch-question.png](img/branch-feature-branch-question.png)

图 18-3：没有使用分支导致项目拖延

说明：

*   图 18-3 中的图示①，用圆圈代表功能 1 的历次提交，用三角代替功能 2 的历次提交。因为所有开发者都在主线上工作，所以提交混杂在一起。
*   当老板决定功能 2 不在这一版本的产品中发布，延期到下一个版本时，功能 2 的开发者做了一个（或者若干个）反向提交，即图示②中的倒三角（代号为“F2.X”）标识的反向提交，将功能 2 的所有历史提交全部撤销。
*   图示③表示除了功能 2 外的其他开发继续进行。

那么负责开发功能 2 的开发者干什么呢？或者放一个长假，或者在本地开发，与版本库隔离，即不向版本库提交，直到延期的项目终于发布之后再将代码提交。这两种方法都是不可取的，尤其是后一种隔离开发最危险，如果因为病毒感染、文件误删、磁盘损坏，就会导致全部工作损失殆尽。我的项目组就曾经遇到过这样的情况。

采用分支将某个功能或模块的开发与开发主线独立出来，是解决类似问题的办法，这种用途的分支被称为特性分支（Feature Branch）或主题分支（Topic Branch）。图 18-4 就展示了如何使用特性分支帮助纠正要延期的项目，协同多用户的开发。

![../_images/branch-feature-branch-answer.png](img/branch-feature-branch-answer.png)

图 18-4：使用特性分支协同多功能开发

说明：

*   图 18-4 中的图示①和前面的一样，都是多个开发者的提交混杂在开发主线中。
*   图示②是当得知功能 2 不在此次产品发布中后，功能 2 的开发者所做的操作。
*   首先，功能 2 的开发者提交一个（或若干个）反向提交，将功能 2 的相关代码全部撤销。图中倒三角（代号为“F2.X”）的提交就是一个反向提交。
*   接着，功能 2 的开发者从反向提交开始创建一个特性分支。
*   最后，功能 2 的开发者将功能 2 的历史提交拣选到特性分支上。对于 Git 可以使用拣选命令**git cherry-pick**。
*   图示③中可以看出包括功能 2 在内的所有功能和模块都继续提交，但是提交的分支各不相同。功能 2 的开发者将代码提交到特性分支上，其他开发者还提交到主线上。

那么在什么情况下使用特性分支呢？试验性、探索性的功能开发应该为其建立特性分支。功能复杂、开发周期长（有可能在本次发布中取消）的模块应该为其建立特性分支。会更改软件体系架构，破坏软件集成，或者容易导致冲突、影响他人开发进度的模块，应该为其建立特性分支。

在使用 CVS 或 Subversion 等版本控制系统建立分支时，或者因为太慢（CVS）或者因为授权原因需要找管理员进行操作，非常的不方便。Git 的分支管理就方便多了，一是开发者可以在本地版本库中随心所欲地创建分支，二是管理员可以对共享版本库进行设置允许开发者创建特定名称的分支，这样开发者的本地分支可以推送到服务器实现数据的备份。关于 Git 服务器的分支授权参照本书第 5 篇的 Gitolite 服务器架设的相关章节。

### 3.4.1.3\. 卖主分支

有的项目要引用到第三方的代码模块并且需要对其进行定制，有的项目甚至整个就是基于某个开源项目进行的定制。如何有效地管理本地定制和第三方（上游）代码的变更就成为了一个难题。卖主分支（Vendor Branch）可以部分解决这个难题。

所谓卖主分支，就是在版本库中创建一个专门和上游代码进行同步的分支，一旦有上游代码发布就检入到卖主分支中。图 18-5 就是一个典型的卖主分支工作流程。

![../_images/branch-vendor-branch.png](img/branch-vendor-branch.png)

图 18-5：卖主分支工作流程

说明：

*   在主线检入上游软件版本 1.0 的代码。在图中标记为`v1.0`的提交即是。
*   然后在主线上进行定制开发，c1、c2 分别代表历次定制提交。
*   当上游有了新版本发布，例如 2.0 版本，就将上游新版本的源代码提交到卖主分支中。图中标记为`v2.0`的提交即是。
*   然后在主线上合并卖主分支上的新提交，合并后的提交显示为`M1`。

如果定制较少，使用卖主分支可以工作得很好，但是如果定制的内容非常多，在合并的时候就会遇到非常多的冲突。定制的代码越多，混杂的越厉害，冲突解决就越困难。

本章的内容尚不能针对复杂的定制开发给出满意的版本控制解决方案，本书第 4 篇的“第二十二章 Topgit 协同模型”会介绍一个针对复杂定制开发的更好的解决方案。

## 3.4.2\. 分支命令概述

在 Git 中分支管理使用命令**git branch**。该命令的主要用法如下：

```
用法 1： git branch
用法 2： git branch <branchname>
用法 3： git branch <branchname> <start-point>
用法 4： git branch -d <branchname>
用法 5： git branch -D <branchname>
用法 6： git branch -m <oldbranch> <newbranch>
用法 7： git branch -M <oldbranch> <newbranch>

```

说明：

*   用法 1 用于显示本地分支列表。当前分支在输出中会显示为特别的颜色，并用星号 “*” 标识出来。

*   用法 2 和用法 3 用于创建分支。

    用法 2 基于当前头指针（HEAD）指向的提交创建分支，新分支的分支名为`<branchname>`。

    用法 3 基于提交`<start-point>`创建新分支，新分支的分支名为`<branchname>`。

*   用法 4 和用法 5 用于删除分支。

    用法 4 在删除分支`<branchname>`时会检查所要删除的分支是否已经合并到其他分支中，否则拒绝删除。

    用法 5 会强制删除分支`<branchname>`，即使该分支没有合并到任何一个分支中。

*   用法 6 和用法 7 用于重命名分支。

    如果版本库中已经存在名为`<newbranch>`的分支，用法 6 拒绝执行重命名，而用法 7 会强制执行。

下面就通过`hello-world`项目演示 Git 的分支管理。

## 3.4.3\. Hello World 开发计划

上一章从 Github 上检出的`hello-world`包含了一个 C 语言开发的应用，现在假设项目`hello-world`做产品发布，版本号定为 1.0，则进行下面的里程碑操作。

*   为`hello-world`创建里程碑`v1.0`。

    ```
    $ cd /path/to/user1/workspace/hello-world/
    $ git tag -m "Release 1.0" v1.0

    ```

*   将新建的里程碑推送到远程共享版本库。

    ```
    $ git push origin refs/tags/v1.0
    Counting objects: 1, done.
    Writing objects: 100% (1/1), 158 bytes, done.
    Total 1 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (1/1), done.
    To file:///path/to/repos/hello-world.git
     * [new tag]         v1.0 -> v1.0

    ```

到现在为止还没有运行`hello-world`程序呢，现在就在开发者 user1 的工作区中运行一下。

*   进入`src`目录，编译程序。

    ```
    $ cd src
    $ make
    version.h.in => version.h
    cc    -c -o main.o main.c
    cc -o hello main.o

    ```

*   使用参数`--help`运行`hello`程序，可以查看帮助信息。

    说明：hello 程序的帮助输出中有一个拼写错误，本应该是`--help`的地方写成了`-help`。这是有意为之。

    ```
    $ ./hello --help
    Hello world example v1.0
    Copyright Jiang Xin <jiangxin AT ossxp DOT com>, 2009.

    Usage:
        hello
                say hello to the world.

        hello <username>
                say hi to the user.

        hello -h, -help
                this help screen.

    ```

*   不带参数运行，向全世界问候。

    说明：最后一行显示版本为“v1.0”，这显然是来自于新建立的里程碑“v1.0”。

    ```
    $ ./hello
    Hello world.
    (version: v1.0)

    ```

*   执行命令的时候，后面添加用户名作为参数，则向该用户问候。

    说明：下面在运行`hello`的时候，显然出现了一个 bug，即用户名中间如果出现了空格，输出的欢迎信息只包含了部分的用户名。这个 bug 也是有意为之。

    ```
    $ ./hello Jiang Xin
    Hi, Jiang.
    (version: v1.0)

    ```

**新版本开发计划**

既然 1.0 版本已经发布了，现在是时候制订下一个版本 2.0 的开发计划了。计划如下：

*   多语种支持。

    为`hello-world`添加多语种支持，使得软件运行的时候能够使用中文或其他本地化语言进行问候。

*   用 getopt 进行命令行解析。

    对命令行参数解析框架进行改造，以便实现更灵活、更易扩展的命令行处理。在 1.0 版本中，程序内部解析命令行参数使用了简单的字符串比较，非常不灵活。从源文件`src/main.c`中可以看到当前实现的简陋和局限。

    ```
    $ git grep -n argv
    main.c:20:main(int argc, char **argv)
    main.c:24:    } else if ( strcmp(argv[1],"-h") == 0 ||
    main.c:25:                strcmp(argv[1],"--help") == 0 ) {
    main.c:28:        printf ("Hi, %s.\n", argv[1]);

    ```

最终决定由开发者 user2 负责多语种支持的功能，由开发者 user1 负责用 getopt 进行命令行解析的功能。

## 3.4.4\. 基于特性分支的开发

有了前面“代码管理之殇”的铺垫，在领受任务之后，开发者 user1 和 user2 应该为自己负责的功能创建特性分支。

### 3.4.4.1\. 创建分支`user1/getopt`

开发者 user1 负责用 getopt 进行命令行解析的功能，因为这个功能用到`getopt`函数，于是将这个分支命名为`user1/getopt`。开发者 user1 使用**git branch**命令创建该特性分支。

*   确保是在开发者 user1 的工作区中。

    ```
    $ cd /path/to/user1/workspace/hello-world/

    ```

*   开发者 user1 基于当前 HEAD 创建分支`user1/getopt`。

    ```
    $ git branch user1/getopt

    ```

*   使用**git branch**创建分支，并不会自动切换。查看当前分支可以看到仍然工作在`master`分支（用星号 “*” 标识）中。

    ```
    $ git branch
    * master
      user1/getopt

    ```

*   执行**git checkout**命令切换到新分支上。

    ```
    $ git checkout user1/getopt
    Switched to branch 'user1/getopt'

    ```

*   再次查看分支列表，当前工作分支的标记符（星号）已经落在`user1/getopt`分支上。

    ```
    $ git branch
      master
    * user1/getopt

    ```

**分支的奥秘**

分支实际上是创建在目录`.git/refs/heads`下的引用，版本库初始时创建的`master`分支就是在该目录下。在第 2 篇“Git 重置”的章节中，已经介绍过 master 分支的实现，实际上这也是所有分支的实现方式。

*   查看一下目录`.git/refs/heads`目录下的引用。

    可以在该目录下看到`master`文件，和一个`user1`目录。而在`user1`目录下是文件`getopt`。

    ```
    $ ls -F .git/refs/heads/
    master  user1/
    $ ls -F .git/refs/heads/user1/
    getopt

    ```

*   引用文件`.git/refs/heads/user1/getopt`记录的是一个提交 ID。

    ```
    $ cat .git/refs/heads/user1/getopt
    ebcf6d6b06545331df156687ca2940800a3c599d

    ```

*   因为分支`user1/getopt`是基于头指针 HEAD 创建的，因此当前该分支和`master`分支指向是一致的。

    ```
    $ cat .git/refs/heads/master
    ebcf6d6b06545331df156687ca2940800a3c599d

    ```

*   当前的工作分支为`user1/getopt`，记录在头指针文件`.git/HEAD`中。

    切换分支命令**git checkout**对文件`.git/HEAD`的内容进行更新。可以参照第 2 篇“第八章 Git 检出”的相关章节。

    ```
    $ cat .git/HEAD
    ref: refs/heads/user1/getopt

    ```

### 3.4.4.2\. 创建分支`user2/i18n`

开发者 user2 要完成多语种支持的工作任务，于是决定将分支定名为`user2/i18n`。每一次创建分支通常都需要完成以下两个工作：

1.  创建分支：执行**git branch <branchname>**命令创建新分支。
2.  切换分支：执行**git checkout <branchname>**命令切换到新分支。

有没有简单的操作，在创建分支后立即切换到新分支上呢？有的，Git 提供了这样一个命令，能够将上述两条命令所执行的操作一次性完成。用法如下：

```
用法： git checkout -b <new_branch> [<start_point>]

```

即检出命令**git checkout**通过参数`-b <new_branch>`实现了创建分支和切换分支两个动作的合二为一。下面开发者 user2 就使用**git checkout**命令来创建分支。

*   进入到开发者 user2 的工作目录，并和上游同步一次。

    ```
    $ cd /path/to/user2/workspace/hello-world/
    $ git pull
    remote: Counting objects: 1, done.
    remote: Total 1 (delta 0), reused 0 (delta 0)
    Unpacking objects: 100% (1/1), done.
    From file:///path/to/repos/hello-world
     * [new tag]         v1.0       -> v1.0
    Already up-to-date.

    ```

*   执行**git checkout -b**命令，创建并切换到新分支`user2/i18n`上。

    ```
    $ git checkout -b user2/i18n
    Switched to a new branch 'user2/i18n'

    ```

*   查看本地分支列表，会看到已经切换到`user2/i18n`分支上了。

    ```
    $ git branch
      master
    * user2/i18n

    ```

### 3.4.4.3\. 开发者 user1 完成功能开发

开发者 user1 开始在`user1/getopt`分支中工作，重构`hello-world`中的命令行参数解析的代码。重构时采用`getopt_long`函数。

您可以试着更改，不过在`hello-world`中已经保存了一份改好的代码，可以直接检出。

*   确保是在 user1 的工作区中。

    ```
    $ cd /path/to/user1/workspace/hello-world/

    ```

*   执行下面的命令，用里程碑`jx/v2.0`标记的内容（已实现用 getopt 进行命令行解析的功能）替换暂存区和工作区。

    下面的**git checkout**命令的最后是一个点“.”，因此检出只更改了暂存区和工作区，而没有修改头指针。

    ```
    $ cd /path/to/user1/workspace/hello-world/
    $ git checkout jx/v2.0 -- .

    ```

*   查看状态，会看到分支仍保持为`user1/getopt`，但文件`src/main.c`被修改了。

    ```
    $ git status
    # On branch user1/getopt
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       modified:   src/main.c
    #

    ```

*   比较暂存区和 HEAD 的文件差异，可以看到为实现用 getopt 进行命令行解析功能而对代码的改动。

    ```
    $ git diff --cached
    diff --git a/src/main.c b/src/main.c
    index 6ee936f..fa5244a 100644
    --- a/src/main.c
    +++ b/src/main.c
    @@ -1,4 +1,6 @@
     #include <stdio.h>
    +#include <getopt.h>
    +
     #include "version.h"

     int usage(int code)
    @@ -19,15 +21,44 @@ int usage(int code)
     int
     main(int argc, char **argv)
     {
    -    if (argc == 1) {
    +    int c;
    +    char *uname = NULL;
    +
    +    while (1) {
    +        int option_index = 0;
    +        static struct option long_options[] = {
    +            {"help", 0, 0, 'h'},
    +            {0, 0, 0, 0}
    +        };
    ...

    ```

*   开发者 user1 提交代码，完成开发任务。

    ```
    $ git commit -m "Refactor: use getopt_long for arguments parsing."
    [user1/getopt 0881ca3] Refactor: use getopt_long for arguments parsing.
     1 files changed, 36 insertions(+), 5 deletions(-)

    ```

*   提交完成之后，可以看到这时`user1/getopt`分支和`master`分支的指向不同了。

    ```
    $ git rev-parse user1/getopt master
    0881ca3f62ddadcddec08bd9f2f529a44d17cfbf
    ebcf6d6b06545331df156687ca2940800a3c599d

    ```

*   编译运行`hello-world`。

    注意输出中的版本号显示。

    ```
    $ cd src
    $ make clean
    rm -f hello main.o version.h
    $ make
    version.h.in => version.h
    cc    -c -o main.o main.c
    cc -o hello main.o
    $ ./hello
    Hello world.
    (version: v1.0-1-g0881ca3)

    ```

### 3.4.4.4\. 将`user1/getopt`分支合并到主线

既然开发者 user1 负责的功能开发完成了，那就合并到开发主线`master`上吧，这样测试团队（如果有的话）就可以基于开发主线`master`进行软件集成和测试了。

*   为将分支合并到主线，首先 user1 将工作区切换到主线，即`master`分支。

    ```
    $ git checkout master
    Switched to branch 'master'

    ```

*   然后执行**git merge**命令以合并`user1/getopt`分支。

    ```
    $ git merge user1/getopt
    Updating ebcf6d6..0881ca3
    Fast-forward
     src/main.c |   41 ++++++++++++++++++++++++++++++++++++-----
     1 files changed, 36 insertions(+), 5 deletions(-)

    ```

*   本次合并非常的顺利，实际上合并后`master`分支和`user1/getopt`指向同一个提交。

    这是因为合并前的`master`分支的提交就是`usr1/getopt`分支的父提交，所以此次合并相当于分支`master`重置到`user1/getopt`分支。

    ```
    $ git rev-parse user1/getopt master
    0881ca3f62ddadcddec08bd9f2f529a44d17cfbf
    0881ca3f62ddadcddec08bd9f2f529a44d17cfbf

    ```

*   当前本地`master`分支比远程共享版本库的`master`分支领先一个提交。

    可以从状态信息中看到本地分支和远程分支的跟踪关系。

    ```
    $ git status
    # On branch master
    # Your branch is ahead of 'origin/master' by 1 commit.
    #
    nothing to commit (working directory clean)

    ```

*   执行推送操作，完成本地分支向远程分支的同步。

    ```
    $ git push
    Counting objects: 7, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (4/4), done.
    Writing objects: 100% (4/4), 689 bytes, done.
    Total 4 (delta 3), reused 0 (delta 0)
    Unpacking objects: 100% (4/4), done.
    To file:///path/to/repos/hello-world.git
       ebcf6d6..0881ca3  master -> master

    ```

*   删除`user1/getopt`分支。

    既然特性分支`user1/getopt`已经合并到主线上了，那么该分支已经完成了历史使命，可以放心地将其删除。

    ```
    $ git branch -d user1/getopt
    Deleted branch user1/getopt (was 0881ca3).

    ```

开发者 user2 对多语种支持功能有些犯愁，需要多花些时间，那么就先不等他了。

## 3.4.5\. 基于发布分支的开发

用户在使用 1.0 版的`hello-word`过程中发现了两个错误，报告给项目组。

*   第一个问题是：帮助信息中出现文字错误。本应该写为“–help”却写成了“-help”。

*   第二个问题是：当执行`hello-world`的程序，提供带空格的用户名时，问候语中显示的是不完整的用户名。

    例如执行**./hello Jiang Xin**，本应该输出“`Hi, Jiang Xin.`”，却只输出了“`Hi, Jiang.`”。

为了能够及时修正 1.0 版本中存在的这两个 bug，将这两个 bug 的修正工作分别交给两个开发者 user1 和 user2 完成。

*   开发者 user1 负责修改文字错误的 bug。
*   开发者 user2 负责修改显示用户名不完整的 bug。

现在版本库中`master`分支相比 1.0 发布时添加了新功能代码，即开发者 user1 推送的用 getopt 进行命令行解析相关代码。如果基于`master`分支对用户报告的两个 bug 进行修改，就会引入尚未经过测试、可能不稳定的新功能的代码。在之前“代码管理之殇”中介绍的发布分支，恰恰适用于此场景。

### 3.4.5.1\. 创建发布分支

要想解决在 1.0 版本中发现的 bug，就需要基于 1.0 发行版的代码创建发布分支。

*   软件`hello-world`的 1.0 发布版在版本库中有一个里程碑相对应。

    ```
    $ cd /path/to/user1/workspace/hello-world/
    $ git tag -n1 -l v*
    v1.0            Release 1.0

    ```

*   基于里程碑`v1.0`创建发布分支`hello-1.x`。

    注：使用了**git checkout**命令创建分支，最后一个参数`v1.0`是新分支`hello-1.x`创建的基准点。如果没有里程碑，使用提交 ID 也是一样。

    ```
    $ git checkout -b hello-1.x v1.0
    Switched to a new branch 'hello-1.x'

    ```

*   用**git rev-parse**命令可以看到`hello-1.x`分支对应的提交 ID 和里程碑`v1.0`指向的提交一致，但是和`master`不一样。

    提示：因为里程碑 v1.0 是一个包含提交说明的里程碑，因此为了显示其对应的提交 ID，使用了特别的记法“`v1.0^{}`”。

    ```
    $ git rev-parse hello-1.x v1.0^{} master
    ebcf6d6b06545331df156687ca2940800a3c599d
    ebcf6d6b06545331df156687ca2940800a3c599d
    0881ca3f62ddadcddec08bd9f2f529a44d17cfbf

    ```

*   开发者 user1 将分支`hello-1.x`推送到远程共享版本库，因为开发者 user2 修改 bug 时也要用到该分支。

    ```
    $ git push origin hello-1.x
    Total 0 (delta 0), reused 0 (delta 0)
    To file:///path/to/repos/hello-world.git
     * [new branch]      hello-1.x -> hello-1.x

    ```

*   开发者 user2 从远程共享版本库获取新的分支。

    开发者 user2 执行**git fetch**命令，将远程共享版本库的新分支`hello-1.x`复制到本地引用`origin/hello-1.x`上。

    ```
    $ cd /path/to/user2/workspace/hello-world/
    $ git fetch
    From file:///path/to/repos/hello-world
     * [new branch]      hello-1.x  -> origin/hello-1.x

    ```

*   开发者 user2 切换到`hello-1.x`分支。

    本地引用`origin/hello-1.x`称为远程分支，第十九章将专题介绍。该远程分支不能直接检出，而是需要基于该远程分支创建本地分支。第十九章会介绍一个更为简单的基于远程分支建立本地分支的方法，本例先用标准的方法建立分支。

    ```
    $ git checkout -b hello-1.x origin/hello-1.x
    Branch hello-1.x set up to track remote branch hello-1.x from origin.
    Switched to a new branch 'hello-1.x'

    ```

### 3.4.5.2\. 开发者 user1 工作在发布分支

开发者 user1 修改帮助信息中的文字错误。

*   编辑文件`src/main.c`，将“-help”字符串修改为“–help”。

    ```
    $ cd /path/to/user1/workspace/hello-world/
    $ vi src/main.c
    ...

    ```

*   开发者 user1 的改动可以从下面的差异比较中看到。

    ```
    $ git diff
    diff --git a/src/main.c b/src/main.c
    index 6ee936f..e76f05e 100644
    --- a/src/main.c
    +++ b/src/main.c
    @@ -11,7 +11,7 @@ int usage(int code)
                "            say hello to the world.\n\n"
                "    hello <username>\n"
                "            say hi to the user.\n\n"
    -           "    hello -h, -help\n"
    +           "    hello -h, --help\n"
                "            this help screen.\n\n", _VERSION);
         return code;
     }

    ```

*   执行提交。

    ```
    $ git add -u
    $ git commit -m "Fix typo: -help to --help."
    [hello-1.x b56bb51] Fix typo: -help to --help.
     1 files changed, 1 insertions(+), 1 deletions(-)

    ```

*   推送到远程共享版本库。

    ```
    $ git push
    Counting objects: 7, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (4/4), done.
    Writing objects: 100% (4/4), 349 bytes, done.
    Total 4 (delta 3), reused 0 (delta 0)
    Unpacking objects: 100% (4/4), done.
    To file:///path/to/repos/hello-world.git
       ebcf6d6..b56bb51  hello-1.x -> hello-1.x

    ```

### 3.4.5.3\. 开发者 user2 工作在发布分支

开发者 user2 针对问候时用户名显示不全的 bug 进行更改。

*   进入开发者 user2 的工作区，并确保工作在`hello-1.x`分支中。

    ```
    $ cd /path/to/user2/workspace/hello-world/
    $ git checkout hello-1.x

    ```

*   编辑文件`src/main.c`，修改代码中的 bug。

    ```
    $ vi src/main.c

    ```

*   实际上在`hello-world`版本库中包含了我的一份修改，可以看看和您的更改是否一致。

    下面的命令将我对此 bug 的修改保存为一个补丁文件。

    ```
    $ git format-patch jx/v1.1..jx/v1.2
    0001-Bugfix-allow-spaces-in-username.patch

    ```

*   应用我对此 bug 的改动补丁。

    如果您已经自己完成了修改，可以先执行**git stash**保存自己的修改进度，然后执行下面的命令应用补丁文件。当应用完补丁后，再执行**git stash pop**将您的改动合并到工作区。如果我们的改动一致（英雄所见略同），将不会有冲突。

    ```
    $ patch -p1 < 0001-Bugfix-allow-spaces-in-username.patch
    patching file src/main.c

    ```

*   看看代码的改动吧。

    ```
    $ git diff
    diff --git a/src/main.c b/src/main.c
    index 6ee936f..f0f404b 100644
    --- a/src/main.c
    +++ b/src/main.c
    @@ -19,13 +19,20 @@ int usage(int code)
     int
     main(int argc, char **argv)
     {
    +    char **p = NULL;
    +
         if (argc == 1) {
             printf ("Hello world.\n");
         } else if ( strcmp(argv[1],"-h") == 0 ||
                     strcmp(argv[1],"--help") == 0 ) {
                     return usage(0);
         } else {
    -        printf ("Hi, %s.\n", argv[1]);
    +        p = &argv[1];
    +        printf ("Hi,");
    +        do {
    +            printf (" %s", *p);
    +        } while (*(++p));
    +        printf (".\n");
         }

         printf( "(version: %s)\n", _VERSION );

    ```

*   本地测试一下改进后的软件，看看 bug 是否已经被改正。如果运行结果能显示出完整的用户名，则 bug 成功修正。

    ```
    $ cd src/
    $ make
    version.h.in => version.h
    cc    -c -o main.o main.c
    cc -o hello main.o
    $ ./hello Jiang Xin
    Hi, Jiang Xin.
    (version: v1.0-dirty)

    ```

*   提交代码。

    ```
    $ git add -u
    $ git commit -m "Bugfix: allow spaces in username."
    [hello-1.x e64f3a2] Bugfix: allow spaces in username.
     1 files changed, 8 insertions(+), 1 deletions(-)

    ```

### 3.4.5.4\. 开发者 user2 合并推送

开发者 user2 在本地版本库完成提交后，不要忘记向远程共享版本库进行推送。但在推送分支`hello-1.x`时开发者 user2 没有开发者 user1 那么幸运，因为此时远程共享版本库的`hello-1.x`分支已经被开发者 user1 推送过一次，因此开发者 user2 在推送过程中会遇到非快进式推送问题。

```
$ git push
To file:///path/to/repos/hello-world.git
 ! [rejected]        hello-1.x -> hello-1.x (non-fast-forward)
error: failed to push some refs to 'file:///path/to/repos/hello-world.git'
To prevent you from losing history, non-fast-forward updates were rejected
Merge the remote changes (e.g. 'git pull') before pushing again.  See the
'Note about fast-forwards' section of 'git push --help' for details.

```

就像在“第十五章 Git 协议和工作协同”一章中介绍的那样，开发者 user2 需要执行一个拉回操作，将远程共享服务器的改动获取到本地并和本地提交进行合并。

```
$ git pull
remote: Counting objects: 7, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 3), reused 0 (delta 0)
Unpacking objects: 100% (4/4), done.
From file:///path/to/repos/hello-world
   ebcf6d6..b56bb51  hello-1.x  -> origin/hello-1.x
Auto-merging src/main.c
Merge made by recursive.
 src/main.c |    2 +-
 1 files changed, 1 insertions(+), 1 deletions(-)

```

通过显示分支图的方式查看日志，可以看到在执行**git pull**操作后发生了合并。

```
$ git log --graph --oneline
*   8cffe5f Merge branch 'hello-1.x' of file:///path/to/repos/hello-world into hello-1.x
|\
| * b56bb51 Fix typo: -help to --help.
* | e64f3a2 Bugfix: allow spaces in username.
|/
* ebcf6d6 blank commit for GnuPG-signed tag test.
* 8a9f3d1 blank commit for annotated tag test.
* 60a2f4f blank commit.
* 3e6070e Show version.
* 75346b3 Hello world initialized.

```

现在开发者 user2 可以将合并后的本地版本库中的提交推送给远程共享版本库了。

```
$ git push
Counting objects: 14, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (8/8), done.
Writing objects: 100% (8/8), 814 bytes, done.
Total 8 (delta 6), reused 0 (delta 0)
Unpacking objects: 100% (8/8), done.
To file:///path/to/repos/hello-world.git
   b56bb51..8cffe5f  hello-1.x -> hello-1.x

```

### 3.4.5.5\. 发布分支的提交合并到主线

当开发者 user1 和 user2 都相继在`hello-1.x`分支将相应的 bug 修改完后，就可以从`hello-1.x`分支中编译新的软件产品交给客户使用了。接下来别忘了在主线`master`分支也做出同样的更改，因为在`hello-1.x`分支修改的 bug 同样也存在于主线`master`分支中。

使用 Git 提供的拣选命令，就可以直接将发布分支上进行的 bug 修正合并到主线上。下面就以开发者 user2 的身份进行操作。

*   进入 user2 工作区并切换到 master 分支。

    ```
    $ cd /path/to/user2/workspace/hello-world/
    $ git checkout master

    ```

*   从远程共享版本库同步 master 分支。

    同步后本地`master`分支包含了开发者 user1 提交的命令行参数解析重构的代码。

    ```
    $ git pull
    remote: Counting objects: 7, done.
    remote: Compressing objects: 100% (4/4), done.
    remote: Total 4 (delta 3), reused 0 (delta 0)
    Unpacking objects: 100% (4/4), done.
    From file:///path/to/repos/hello-world
       ebcf6d6..0881ca3  master     -> origin/master
    Updating ebcf6d6..0881ca3
    Fast-forward
     src/main.c |   41 ++++++++++++++++++++++++++++++++++++-----
     1 files changed, 36 insertions(+), 5 deletions(-)

    ```

*   查看分支`hello-1.x`的日志，确认要拣选的提交 ID。

    从下面的日志可以看出分支`hello-1.x`的最新提交是一个合并提交，而要拣选的提交分别是其第一个父提交和第二个父提交，可以分别用`hello-1.x¹`和`hello-1.x²`表示。

    ```
    $ git log -3 --graph --oneline hello-1.x
    *   8cffe5f Merge branch 'hello-1.x' of file:///path/to/repos/hello-world into hello-1.x
    |\
    | * b56bb51 Fix typo: -help to --help.
    * | e64f3a2 Bugfix: allow spaces in username.
    |/

    ```

*   执行拣选操作。先将开发者 user2 提交的修正代码拣选到当前分支（即主线）。

    拣选操作遇到了冲突，见下面的命令输出。

    ```
    $  git cherry-pick hello-1.x¹
    Automatic cherry-pick failed.  After resolving the conflicts,
    mark the corrected paths with 'git add <paths>' or 'git rm <paths>'
    and commit the result with:

            git commit -c e64f3a216d346669b85807ffcfb23a21f9c5c187

    ```

*   拣选操作发生冲突，通过查看状态可以看到是在文件`src/main.c`上发生了冲突。

    ```
    $ git status
    # On branch master
    # Unmerged paths:
    #   (use "git reset HEAD <file>..." to unstage)
    #   (use "git add/rm <file>..." as appropriate to mark resolution)
    #
    #       both modified:      src/main.c
    #
    no changes added to commit (use "git add" and/or "git commit -a")

    ```

**冲突发生的原因**

为什么发生了冲突呢？这是因为拣选`hello-1.x`分支上的一个提交到`master`分支时，因为两个甚至多个提交在重叠的位置更改代码所致。通过下面的命令可以看到到底是哪些提交引起的冲突。

```
$ git log master...hello-1.x¹
commit e64f3a216d346669b85807ffcfb23a21f9c5c187
Author: user2 <user2@moon.ossxp.com>
Date:   Sun Jan 9 13:11:19 2011 +0800

    Bugfix: allow spaces in username.

commit 0881ca3f62ddadcddec08bd9f2f529a44d17cfbf
Author: user1 <user1@sun.ossxp.com>
Date:   Mon Jan 3 22:44:52 2011 +0800

    Refactor: use getopt_long for arguments parsing.

```

可以看出引发冲突的提交一个是当前工作分支`master`上的最新提交，即开发者 user1 的重构命令行参数解析的提交，而另外一个引发冲突的是要拣选的提交，即开发者 user2 针对用户名显示不全所做的错误修正提交。一定是因为这两个提交的更改发生了重叠导致了冲突的发生。下面就来解决冲突。

**冲突解决**

冲突解决可以使用图形界面工具，不过对于本例直接编辑冲突文件，手工进行冲突解决也很方便。打开文件`src/main.c`就可以看到发生冲突的区域都用特有的标记符标识出来，参见表 18-1 中左侧一列中的内容。

表 18-1：冲突解决前后对照

| 冲突文件 src/main.c 标识出的冲突内容 | 冲突解决后的内容对照 |
| --- | --- |
|  
```
21 int
22 main(int argc, char **argv)
23 {
24 <<<<<<< HEAD
25     int c;
26     char *uname = NULL;
27
28     while (1) {
29         int option_index = 0;
30         static struct option long_options[] = {
31             {"help", 0, 0, 'h'},
32             {0, 0, 0, 0}
33         };
34
35         c = getopt_long(argc, argv, "h",
36                         long_options, &option_index);
37         if (c == -1)
38            break;
39
40         switch (c) {
41         case 'h':
42             return usage(0);
43         default:
44             return usage(1);
45         }
46     }
47
48     if (optind < argc) {
49         uname = argv[optind];
50     }
51
52     if (uname == NULL) {
53 =======
54     char **p = NULL;
55
56     if (argc == 1) {
57 >>>>>>> e64f3a2... Bugfix: allow spaces in username.
58         printf ("Hello world.\n");
59     } else {
60 <<<<<<< HEAD
61         printf ("Hi, %s.\n", uname);
62 =======
63         p = &argv[1];
64         printf ("Hi,");
65         do {
66             printf (" %s", *p);
67         } while (*(++p));
68         printf (".\n");
69 >>>>>>> e64f3a2... Bugfix: allow spaces in username.
70     }
71
72     printf( "(version: %s)\n", _VERSION );
73     return 0;
74 }

```

 |  
```
21 int
22 main(int argc, char **argv)
23 {

24     int c;
25     char **p = NULL;
26
27     while (1) {
28         int option_index = 0;
29         static struct option long_options[] = {
30             {"help", 0, 0, 'h'},
31             {0, 0, 0, 0}
32         };
33
34         c = getopt_long(argc, argv, "h",
35                         long_options, &option_index);
36         if (c == -1)
37            break;
38
39         switch (c) {
40         case 'h':
41             return usage(0);
42         default:
43             return usage(1);
44         }
45     }
46
47     if (optind < argc) {
48         p = &argv[optind];
49     }
50
51     if (p == NULL &#124;&#124; *p == NULL) {

52         printf ("Hello world.\n");
53     } else {

54         printf ("Hi,");
55         do {
56             printf (" %s", *p);
57         } while (*(++p));
58         printf (".\n");

59     }
60
61     printf( "(version: %s)\n", _VERSION );
62     return 0;
63 }

```

 |

在文件`src/main.c`冲突内容中，第 25-52 行及第 61 行是`master`分支中由开发者 user1 重构命令行解析时提交的内容，而第 54-56 行及第 63-68 行则是分支`hello-1.x`中由开发者 user2 提交的修正用户名显示不全的 bug 的相应代码。

表 18-1 右侧的一列则是冲突解决后的内容。为了和冲突前的内容相对照，重新进行了排版，并对差异内容进行加粗显示。您可以参照完成冲突解决。

将手动编辑完成的文件`src/main.c`添加到暂存区才真正地完成了冲突解决。

```
$ git add src/main.c

```

因为是拣选操作，提交时最好重用所拣选提交的提交说明和作者信息，而且也省下了自己写提交说明的麻烦。使用下面的命令完成提交操作。

```
$ git commit -C hello-1.x¹
[master 10765a7] Bugfix: allow spaces in username.
 1 files changed, 8 insertions(+), 4 deletions(-)

```

接下来再将开发者 user1 在分支`hello-1.x`中的提交也拣选到当前分支。所拣选的提交非常简单，不过是修改了提交说明中的文字错误而已，拣选操作也不会引发异常，直接完成。

```
$ git cherry-pick hello-1.x²
Finished one cherry-pick.
[master d81896e] Fix typo: -help to --help.
 Author: user1 <user1@sun.ossxp.com>
 1 files changed, 1 insertions(+), 1 deletions(-)

```

现在通过日志可以看到`master`分支已经完成了对已知 bug 的修复。

```
$ git log -3 --graph --oneline
* d81896e Fix typo: -help to --help.
* 10765a7 Bugfix: allow spaces in username.
* 0881ca3 Refactor: use getopt_long for arguments parsing.

```

查看状态可以看到当前的工作分支相对于远程服务器有两个新提交。

```
$ git status
# On branch master
# Your branch is ahead of 'origin/master' by 2 commits.
#
nothing to commit (working directory clean)

```

执行推送命令将本地`master`分支同步到远程共享版本库。

```
$ git push
Counting objects: 11, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (8/8), done.
Writing objects: 100% (8/8), 802 bytes, done.
Total 8 (delta 6), reused 0 (delta 0)
Unpacking objects: 100% (8/8), done.
To file:///path/to/repos/hello-world.git
   0881ca3..d81896e  master -> master

```

## 3.4.6\. 分支变基

### 3.4.6.1\. 完成`user2/i18n`特性分支的开发

开发者 user2 针对多语种开发的工作任务还没有介绍呢，在最后就借着“实现”这个稍微复杂的功能来学习一下 Git 分支的变基操作。

*   进入 user2 的工作区，并切换到`user2/i18n`分支。

    ```
    $ cd /path/to/user2/workspace/hello-world/
    $ git checkout user2/i18n
    Switched to branch 'user2/i18n'

    ```

*   使用`gettext`为软件添加多语言支持。您可以尝试实现该功能。不过在`hello-world`中已经保存了一份实现该功能的代码（见里程碑`jx/v1.0-i18n`），可以直接拿过来用。

*   里程碑`jx/v1.0-i18n`最后的两个提交实现了多语言支持功能。

    ```
    $ git log --oneline -2 --stat jx/v1.0-i18n
    ade873c Translate for Chinese.
     src/locale/zh_CN/LC_MESSAGES/helloworld.po |   30 +++++++++++++++++++++------
     1 files changed, 23 insertions(+), 7 deletions(-)
    0831248 Add I18N support.
     src/Makefile                               |   21 +++++++++++-
     src/locale/helloworld.pot                  |   46 ++++++++++++++++++++++++++++
     src/locale/zh_CN/LC_MESSAGES/helloworld.po |   46 ++++++++++++++++++++++++++++
     src/main.c                                 |   18 ++++++++--
     4 files changed, 125 insertions(+), 6 deletions(-)

    ```

*   可以通过拣选命令将这两个提交拣选到`user2/i18n`分支中，相当于在分支`user2/i18n`中实现了多语言支持的开发。

    ```
    $ git cherry-pick jx/v1.0-i18n~1
    ...
    $ git cherry-pick jx/v1.0-i18n
    ...

    ```

*   看看当前分拣选后的日志。

    ```
    $ git log --oneline -2
    7acb3e8 Translate for Chinese.
    90d873b Add I18N support.

    ```

*   为了测试刚刚“开发”完成的多语言支持功能，先对源码执行编译。

    ```
    $ cd src
    $ make
    version.h.in => version.h
    cc    -c -o main.o main.c
    msgfmt -o locale/zh_CN/LC_MESSAGES/helloworld.mo locale/zh_CN/LC_MESSAGES/helloworld.po
    cc -o hello main.o

    ```

*   查看帮助信息，会发现帮助信息已经本地化。

    注意：帮助信息中仍然有文字错误，`--help`误写为`-help`。

    ```
    $ ./hello --help
    Hello world 示例 v1.0-2-g7acb3e8
    版权所有 蒋鑫 <jiangxin AT ossxp DOT com>, 2009

    用法:
        hello
                世界你好。

        hello <username>
                向用户问您好。

        hello -h, -help
                显示本帮助页。

    ```

*   不带用户名运行`hello`，也会输出中文。

    ```
    $ ./hello
    世界你好。
    (version: v1.0-2-g7acb3e8)

    ```

*   带用户名运行`hello`，会向用户问候。

    注意：程序仍然存在只显示部分用户名的问题。

    ```
    $ ./hello Jiang Xin
    您好, Jiang.
    (version: v1.0-2-g7acb3e8)

    ```

*   推送分支`user2/i18n`到远程共享服务器。

    推送该特性分支的目的并非是与他人在此分支上协同工作，主要只是为了进行数据备份。

    ```
    $ git push origin user2/i18n
    Counting objects: 21, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (13/13), done.
    Writing objects: 100% (17/17), 2.91 KiB, done.
    Total 17 (delta 6), reused 1 (delta 0)
    Unpacking objects: 100% (17/17), done.
    To file:///path/to/repos/hello-world.git
     * [new branch]      user2/i18n -> user2/i18n

    ```

### 3.4.6.2\. 分支`user2/i18n`变基

在测试刚刚完成的具有多语种支持功能的`hello-world`时，之前改正的两个 bug 又重现了。这并不奇怪，因为分支`user2/i18n`基于`master`分支创建的时候，这两个 bug 还没有发现呢，更不要说改正了。

在最早刚刚创建`user2/i18n`分支时，版本库的结构非常简单，如图 18-6 所示。

![../_images/branch-i18n-initial.png](img/branch-i18n-initial.png)

图 18-6：分支 user2/i18n 创建初始版本库分支状态

但是当前`master`分支中不但包含了对两个 bug 的修正，还包含了开发者 user1 调用`getopt`对命令行参数解析进行的代码重构。图 18-7 显示的是当前版本库`master`分支和`user2/i18n`分支的关系图。

![../_images/branch-i18n-complete.png](img/branch-i18n-complete.png)

图 18-7：当前版本库分支示意图

开发者 user2 要将分支`user2/i18n`中的提交合并到主线`master`中，可以采用上一节介绍的分支合并操作。如果执行分支合并操作，版本库的状态将会如图 18-8 所示：

![../_images/branch-i18n-merge.png](img/branch-i18n-merge.png)

图 18-8：使用分支合并时版本库分支状态

这样操作有利有弊。有利的一面是开发者在`user2/i18n`分支中的提交不会发生改变，这一点对于提交已经被他人共享时很重要。再有因为`user2/i18n`分支是基于`v1.0`创建的，这样可以很容易将多语言支持功能添加到 1.0 版本的`hello-world`中。不过这些对于本项目来说都不重要。至于不利的一面，就是这样的合并操作会产生三个提交（包括一个合并提交），对于要对提交进行审核的项目团队来说增加了代码审核的负担。因此很多项目在特性分支合并到开发主线的时候，都不推荐使用合并操作，而是使用变基操作。如果执行变基操作，版本库相关分支的关系图如图 18-9 所示。

![../_images/branch-i18n-rebase-complete.png](img/branch-i18n-rebase-complete.png)

图 18-9：使用变基操作版本库分支状态

很显然，采用变基操作的分支关系图要比采用合并操作的简单多了，看起来更像是集中式版本控制系统特有的顺序提交。因为减少了一个提交，也会减轻代码审核的负担。

下面开发者 user2 就通过变基操作将特性分支`user2/i18n`合并到主线。

*   首先确保开发者 user2 的工作区位于分支`user2/i18n`上。

    ```
    $ cd /path/to/user2/workspace/hello-world/
    $ git checkout user2/i18n

    ```

*   执行变基操作。

    ```
    $ git rebase master
    First, rewinding head to replay your work on top of it...
    Applying: Add I18N support.
    Using index info to reconstruct a base tree...
    Falling back to patching base and 3-way merge...
    Auto-merging src/main.c
    CONFLICT (content): Merge conflict in src/main.c
    Failed to merge in the changes.
    Patch failed at 0001 Add I18N support.

    When you have resolved this problem run "git rebase --continue".
    If you would prefer to skip this patch, instead run "git rebase --skip".
    To restore the original branch and stop rebasing run "git rebase --abort".

    ```

变基遇到了冲突，看来这回的麻烦可不小。冲突是在合并`user2/i18n`分支中的提交“Add I18N support”时遇到的。首先回顾一下变基的原理，参见第 2 篇“第十二章 改变历史”相关章节。对于本例，在进行变基操作时会先切换到`user2/i18n`分支，并强制重置到`master`分支所指向的提交。然后再将原`user2/i18n`分支的提交一一拣选到新的`user2/i18n`分支上。运行下面的命令可以查看可能导致冲突的提交列表。

```
$ git rev-list --pretty=oneline user2/i18n^...master
d81896e60673771ef1873b27a33f52df75f70515 Fix typo: -help to --help.
10765a7ef46981a73d578466669f6e17b73ac7e3 Bugfix: allow spaces in username.
90d873bb93cd7577b7638f1f391bd2ece3141b7a Add I18N support.
0881ca3f62ddadcddec08bd9f2f529a44d17cfbf Refactor: use getopt_long for arguments parsing

```

刚刚发生的冲突是在拣选提交“Add I18N suppport”时出现的，所以在冲突文件中标识为他人版本的是 user2 添加多语种支持功能的提交，而冲突文件中标识为自己版本的是修正两个 bug 的提交及开发者 user1 提交的重构命令行参数解析的提交。下面的两个表格（表 18-2 和表 18-3）是文件`src/main.c`发生冲突的两个主要区域，表格的左侧一列是冲突文件中的内容，右侧一列则是冲突解决后的内容。为了方便参照进行了适当排版。

表 18-2：变基冲突区域一解决前后对照

| 变基冲突区域一内容（文件 src/main.c） | 冲突解决后的内容对照 |
| --- | --- |
|  
```
12 int usage(int code)
13 {
14     printf(_("Hello world example %s\n"
15            "Copyright Jiang Xin <jiangxin AT ossxp ...\n"
16            "\n"
17            "Usage:\n"
18            "    hello\n"
19            "            say hello to the world.\n\n"
20            "    hello <username>\n"
21            "            say hi to the user.\n\n"
22 <<<<<<< HEAD
23            "    hello -h, --help\n"
24            "            this help screen.\n\n", _VERSION);
25 &#124;&#124;&#124;&#124;&#124;&#124;&#124; merged common ancestors
26            "    hello -h, -help\n"
27            "            this help screen.\n\n", _VERSION);
28 =======
29            "    hello -h, -help\n"
30            "            this help screen.\n\n"), _VERSION);
31 >>>>>>> Add I18N support.
32     return code;
33 }

```

 |  
```
12 int usage(int code)
13 {
14     printf(_("Hello world example %s\n"
15            "Copyright Jiang Xin <jiangxin AT ossxp ...\n"
16            "\n"
17            "Usage:\n"
18            "    hello\n"
19            "            say hello to the world.\n\n"
20            "    hello <username>\n"
21            "            say hi to the user.\n\n"

22            "    hello -h, --help\n"
23            "            this help screen.\n\n"), _VERSION);

24     return code;
25 }

```

 |

表 18-3：变基冲突区域二解决前后对照

| 变基冲突区域二内容（文件 src/main.c） | 冲突解决后的内容对照 |
| --- | --- |
|  
```
38 <<<<<<< HEAD
39     int c;
40     char **p = NULL;
41

42     while (1) {
43         int option_index = 0;
44         static struct option long_options[] = {
45             {"help", 0, 0, 'h'},
46             {0, 0, 0, 0}
47         };
48
49         c = getopt_long(argc, argv, "h",
50                         long_options, &option_index);
51         if (c == -1)
52            break;
53
54         switch (c) {
55         case 'h':
56             return usage(0);
57         default:
58             return usage(1);
59         }
60     }
61
62     if (optind < argc) {
63         p = &argv[optind];
64     }
65
66     if (p == NULL &#124;&#124; *p == NULL) {
67         printf ("Hello world.\n");
68 &#124;&#124;&#124;&#124;&#124;&#124;&#124; merged common ancestors
69     if (argc == 1) {
70         printf ("Hello world.\n");
71     } else if ( strcmp(argv[1],"-h") == 0 &#124;&#124;
72                 strcmp(argv[1],"--help") == 0 ) {
73                 return usage(0);
74 =======
75     setlocale( LC_ALL, "" );
76     bindtextdomain("helloworld","locale");
77     textdomain("helloworld");
78
79     if (argc == 1) {
80         printf ( _("Hello world.\n") );
81     } else if ( strcmp(argv[1],"-h") == 0 &#124;&#124;
82                 strcmp(argv[1],"--help") == 0 ) {
83                 return usage(0);
84 >>>>>>> Add I18N support.
85     } else {
86 <<<<<<< HEAD
87         printf ("Hi,");
88         do {
89             printf (" %s", *p);
90         } while (*(++p));
91         printf (".\n");
92 &#124;&#124;&#124;&#124;&#124;&#124;&#124; merged common ancestors
93         printf ("Hi, %s.\n", argv[1]);
94 =======
95         printf (_("Hi, %s.\n"), argv[1]);
96 >>>>>>> Add I18N support.
97     }

```

 |  
```
30     int c;
31     char **p = NULL;
32
33     setlocale( LC_ALL, "" );
34     bindtextdomain("helloworld","locale");
35     textdomain("helloworld");
36
37     while (1) {
38         int option_index = 0;
39         static struct option long_options[] = {
40             {"help", 0, 0, 'h'},
41             {0, 0, 0, 0}
42         };
43
44         c = getopt_long(argc, argv, "h",
45                         long_options, &option_index);
46         if (c == -1)
47            break;
48
49         switch (c) {
50         case 'h':
51             return usage(0);
52         default:
53             return usage(1);
54         }
55     }
56
57     if (optind < argc) {
58         p = &argv[optind];
59     }
60
61     if (p == NULL &#124;&#124; *p == NULL) {
62         printf ( _("Hello world.\n") );

63     } else {
64         printf (_("Hi,"));
65         do {
66             printf (" %s", *p);
67         } while (*(++p));
68         printf (".\n");

69     }

```

 |

将完成冲突解决的文件`src/main.c`加入暂存区。

```
$ git add -u

```

查看工作区状态。

```
$ git status
# Not currently on any branch.
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   src/Makefile
#       new file:   src/locale/helloworld.pot
#       new file:   src/locale/zh_CN/LC_MESSAGES/helloworld.po
#       modified:   src/main.c
#

```

现在不要执行提交，而是继续变基操作。变基操作会自动完成对冲突解决的提交，并对分支中的其他提交继续执行变基，直至全部完成。

```
$ git rebase --continue
Applying: Add I18N support.
Applying: Translate for Chinese.

```

图 18-10 显示了版本库执行完变基后的状态。

![../_images/branch-i18n-rebase.png](img/branch-i18n-rebase.png)

图 18-10：变基操作完成后版本库分支状态

现在需要将`user2/i18n`分支的提交合并到主线`master`中。实际上不需要在`master`分支上再执行繁琐的合并操作，而是可以直接用推送操作——用本地的`user2/i18n`分支直接更新远程版本库的`master`分支。

```
$ git push origin user2/i18n:master
Counting objects: 21, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (13/13), done.
Writing objects: 100% (17/17), 2.91 KiB, done.
Total 17 (delta 6), reused 1 (delta 0)
Unpacking objects: 100% (17/17), done.
To file:///path/to/repos/hello-world.git

```

仔细看看上面运行的**git push**命令，终于看到了引用表达式中引号前后使用了不同名字的引用。含义是用本地的`user2/i18n`引用的内容（提交 ID）更新远程共享版本库的`master`引用内容（提交 ID）。

执行拉回操作，可以发现远程共享版本库的`master`分支的确被更新了。通过拉回操作本地的`master`分支也随之更新。

*   切换到`master`分支，会从提示信息中看到本地`master`分支落后远程共享版本库`master`分支两个提交。

    ```
    $ git checkout master
    Switched to branch 'master'
    Your branch is behind 'origin/master' by 2 commits, and can be fast-forwarded.

    ```

*   执行拉回操作，将本地`master`分支同步到和远程共享版本库相同的状态。

    ```
    $ git pull
    Updating d81896e..c4acab2
    Fast-forward
     src/Makefile                               |   21 ++++++++-
     src/locale/helloworld.pot                  |   46 ++++++++++++++++++++
     src/locale/zh_CN/LC_MESSAGES/helloworld.po |   62 ++++++++++++++++++++++++++++
     src/main.c                                 |   18 ++++++--
     4 files changed, 141 insertions(+), 6 deletions(-)
     create mode 100644 src/locale/helloworld.pot
     create mode 100644 src/locale/zh_CN/LC_MESSAGES/helloworld.po

    ```

特性分支`user2/i18n`也完成了历史使命，可以删除了。因为之前`user2/i18n`已经推送到远程共享版本库，如果想要删除分支不要忘了也将远程分支同时删除。

*   删除本地版本库的`user2/i18n`分支。

    ```
    $ git branch -d user2/i18n
    Deleted branch user2/i18n (was c4acab2).

    ```

*   删除远程共享版本库的`user2/i18n`分支。

    ```
    $ git push origin :user2/i18n
    To file:///path/to/repos/hello-world.git
     - [deleted]         user2/i18n

    ```

* * *

补充：实际上变基之后`user2/i18n`分支的本地化模板文件（helloworld.pot）和汉化文件（helloworld.po）都需要做出相应更新，否则`hello-world`的一些输出不能进行本地化。

*   更新模板需要删除文件`helloworld.pot`，再执行命令**make po**。
*   重新翻译中文本地化文件，可以使用工具**lokalize**或者**kbabel**。

具体的操作过程就不再赘述了。

* * *

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 3.5\. 远程版本库

Git 作为分布式版本库控制系统，每个人都是本地版本库的主人，可以在本地的版本库中随心所欲地创建分支和里程碑。当需要多人协作时，问题就出现了：

*   如何避免因为用户把所有的本地分支都推送到共享版本库，从而造成共享版本库上分支的混乱？
*   如何避免不同用户针对不同特性开发创建了相同名字的分支而造成分支名称的冲突？
*   如何避免用户随意在共享版本库中创建里程碑而导致里程碑名称上的混乱和冲突？
*   当用户向共享版本库及其他版本库推送时，每次都需要输入长长的版本库 URL，太不方便了。
*   当用户需要经常从多个不同的他人版本库中获取提交时，有没有办法不要总是输入长长的版本库 URL？
*   如果不带任何其他参数执行**git fetch**、**git pull**和**git push**到底是和哪个远程版本库及哪个分支进行交互？

本章介绍的**git remote**命令就是用于实现远程版本库的便捷访问，建立远程分支和本地分支的对应，使得**git fetch**、**git pull**和**git push**能够更为便捷地进行操作。

## 3.5.1\. 远程分支

上一章介绍 Git 分支的时候，每一个版本库最多只和一个上游版本库（远程共享版本库）进行交互，实际上 Git 允许一个版本库和任意多的版本库进行交互。首先执行下面的命令，基于`hello-world.git`版本库再创建几个新的版本库。

```
$ cd /path/to/repos/
$ git clone --bare hello-world.git hello-user1.git
Cloning into bare repository hello-user1.git...
done.
$ git clone --bare hello-world.git hello-user2.git
Cloning into bare repository hello-user2.git...
done.

```

现在有了三个共享版本库：`hello-world.git`、`hello-user1.git`和`hello-user2.git`。现在有一个疑问，如果一个本地版本库需要和上面三个版本库进行互操作，三个共享版本库都存在一个`master`分支，会不会互相干扰、冲突或覆盖呢？

先来看看`hello-world`远程共享版本库中包含的分支有哪些：

```
$ git ls-remote --heads file:///path/to/repos/hello-world.git
8cffe5f135821e716117ee59bdd53139473bd1d8        refs/heads/hello-1.x
bb4fef88fee435bfac04b8389cf193d9c04105a6        refs/heads/helper/master
cf71ae3515e36a59c7f98b9db825fd0f2a318350        refs/heads/helper/v1.x
c4acab26ff1c1125f5e585ffa8284d27f8ceea55        refs/heads/master

```

原来远程共享版本库中有四个分支，其中`hello-1.x`分支是开发者 user1 创建的。现在重新克隆该版本库，如下：

```
$ cd /path/to/my/workspace/
$ git clone file:///path/to/repos/hello-world.git
...
$ cd /path/to/my/workspace/hello-world

```

执行**git branch**命令检查分支，会吃惊地看到只有一个分支`master`。

```
$ git branch
* master

```

那么远程版本库中的其他分支哪里去了？为什么本地只有一个分支呢？执行**git show-ref**命令可以看到全部的本地引用。

```
$ git show-ref
c4acab26ff1c1125f5e585ffa8284d27f8ceea55 refs/heads/master
c4acab26ff1c1125f5e585ffa8284d27f8ceea55 refs/remotes/origin/HEAD
8cffe5f135821e716117ee59bdd53139473bd1d8 refs/remotes/origin/hello-1.x
bb4fef88fee435bfac04b8389cf193d9c04105a6 refs/remotes/origin/helper/master
cf71ae3515e36a59c7f98b9db825fd0f2a318350 refs/remotes/origin/helper/v1.x
c4acab26ff1c1125f5e585ffa8284d27f8ceea55 refs/remotes/origin/master
3171561b2c9c57024f7d748a1a5cfd755a26054a refs/tags/jx/v1.0
aaff5676a7c3ae7712af61dfb9ba05618c74bbab refs/tags/jx/v1.0-i18n
e153f83ee75d25408f7e2fd8236ab18c0abf0ec4 refs/tags/jx/v1.1
83f59c7a88c04ceb703e490a86dde9af41de8bcb refs/tags/jx/v1.2
1581768ec71166d540e662d90290cb6f82a43bb0 refs/tags/jx/v1.3
ccca267c98380ea7fffb241f103d1e6f34d8bc01 refs/tags/jx/v2.0
8a5b9934aacdebb72341dcadbb2650cf626d83da refs/tags/jx/v2.1
89b74222363e8cbdf91aab30d005e697196bd964 refs/tags/jx/v2.2
0b4ec63aea44b96d498528dcf3e72e1255d79440 refs/tags/jx/v2.3
60a2f4f31e5dddd777c6ad37388fe6e5520734cb refs/tags/mytag
5dc2fc52f2dcb84987f511481cc6b71ec1b381f7 refs/tags/mytag3
51713af444266d56821fe3302ab44352b8c3eb71 refs/tags/v1.0

```

从**git show-ref**的输出中发现了几个不寻常的引用，这些引用以`refs/remotes/origin/`为前缀，并且名称和远程版本库的分支名一一对应。这些引用实际上就是从远程版本库的分支拷贝过来的，称为远程分支。

Git 的**git branch**命令也能够查看这些远程分支，不过要加上`-r`参数：

```
$ git branch -r
  origin/HEAD -> origin/master
  origin/hello-1.x
  origin/helper/master
  origin/helper/v1.x
  origin/master

```

Git 这样的设计是非常巧妙的，在向远程版本库执行获取操作时，不是把远程版本库的分支原封不动地复制到本地版本库的分支中，而是复制到另外的命名空间。如在克隆一个版本库时，会将远程分支都复制到目录`.git/refs/remotes/origin/`下。这样向不同的远程版本库执行获取操作，因为远程分支相互隔离，所以就避免了相互的覆盖。

那么克隆操作产生的远程分支为什么都有一个名为“origin/”的前缀呢？奥秘就在配置文件`.git/config`中。下面的几行内容出自该配置文件，为了说明方便显示了行号。

```
6 [remote "origin"]
7   fetch = +refs/heads/*:refs/remotes/origin/*
8   url = file:///path/to/repos/hello-world.git

```

这个小节可以称为`[remote]`小节，该小节以 origin 为名注册了一个远程版本库。该版本库的 URL 地址由第 8 行给出，会发现这个 URL 地址就是执行**git clone**命令时所用的地址。最具魔法的配置是第 7 行，这一行设置了执行**git fetch origin**操作时使用的默认引用表达式。

*   该引用表达式以加号（+）开头，含义是强制进行引用的替换，即使即将进行的替换是非快进式的。
*   引用表达式中使用了通配符，冒号前面的含有通配符的引用指的是远程版本库的所有分支，冒号后面的引用含义是复制到本地的远程分支目录中。

正因为有了上面的`[remote]`配置小节，当执行**git fetch origin**操作时，就相当于执行了下面的命令，将远程版本库的所有分支复制为本地的远程分支。

```
git fetch origin +refs/heads/*:refs/remotes/origin/*

```

远程分支不是真正意义上的分支，是类似于里程碑一样的引用。如果针对远程分支执行检出命令，会看到大段的错误警告。

```
$ git checkout origin/hello-1.x
Note: checking out 'origin/hello-1.x'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b new_branch_name

HEAD is now at 8cffe5f... Merge branch 'hello-1.x' of file:///path/to/repos/hello-world into hello-1.x

```

上面的大段的错误信息实际上告诉我们一件事，远程分支类似于里程碑，如果检出就会使得头指针`HEAD`处于分离头指针状态。实际上除了以`refs/heads`为前缀的引用之外，如果检出任何其他引用，都将使工作区处于分离头指针状态。如果对远程分支进行修改就需要创建新的本地分支。

## 3.5.2\. 分支追踪

为了能够在远程分支`refs/remotes/origin/hello-1.x`上进行工作，需要基于该远程分支创建本地分支。远程分支可以使用简写`origin/hello-1.x`。如果 Git 的版本是 1.6.6 或者更新的版本，可以使用下面的命令同时完成分支的创建和切换。

```
$ git checkout hello-1.x
Branch hello-1.x set up to track remote branch hello-1.x from origin.
Switched to a new branch 'hello-1.x'

```

如果 Git 的版本比较老，或注册了多个远程版本库，因此存在多个名为`hello-1.x`的远程分支，就不能使用上面简洁的分支创建和切换命令，而需要使用在上一章中学习到的分支创建命令，显式地从远程分支中创建本地分支。

```
$ git checkout -b hello-1.x origin/hello-1.x
Branch hello-1.x set up to track remote branch hello-1.x from origin.
Switched to a new branch 'hello-1.x'

```

在上面基于远程分支创建本地分支的过程中，命令输出的第一行说的是建立了本地分支和远程分支的跟踪。和远程分支建立跟踪后，本地分支就具有下列特征：

*   检查工作区状态时，会显示本地分支和被跟踪远程分支提交之间的关系。
*   当执行**git pull**命令时，会和被跟踪的远程分支进行合并（或者变基），如果两者出现版本偏离的话。
*   当执行**git push**命令时，会推送到远程版本库的同名分支中。

下面就在基于远程分支创建的本地跟踪分支中进行操作，看看本地分支是如何与远程分支建立关联的。

*   先将本地`hello-1.x`分支向后重置两个版本。

    ```
    $ git reset --hard HEAD^^
    HEAD is now at ebcf6d6 blank commit for GnuPG-signed tag test.

    ```

*   然后查看状态，显示当前分支相比跟踪分支落后了 3 个版本。

    之所以落后三个版本而非两个版本是因为`hello-1.x`的最新提交是一个合并提交，包含两个父提交，因此上面的重置命令丢弃掉三个提交。

    ```
    $ git status
    # On branch hello-1.x
    # Your branch is behind 'origin/hello-1.x' by 3 commits, and can be fast-forwarded.
    #
    nothing to commit (working directory clean)

    ```

*   执行**git pull**命令，会自动与跟踪的远程分支进行合并，相当于找回最新的 3 个提交。

    ```
    $ git pull
    Updating ebcf6d6..8cffe5f
    Fast-forward
     src/main.c |   11 +++++++++--
     1 files changed, 9 insertions(+), 2 deletions(-)

    ```

但是如果基于本地分支创建另外一个本地分支则没有分支跟踪的功能。下面就从本地的`hello-1.x`分支中创建`hello-jx`分支。

*   从`hello-1.x`分支中创建新的本地分支`hello-jx`。

    下面的创建分支操作只有一行输出，看不到分支间建立跟踪的提示。

    ```
    $ git checkout -b hello-jx hello-1.x
    Switched to a new branch 'hello-jx'

    ```

*   将`hello-jx`分支重置。

    ```
    $ git reset --hard HEAD^^
    HEAD is now at ebcf6d6 blank commit for GnuPG-signed tag test.

    ```

*   检查状态看不到分支间的跟踪信息。

    ```
    $ git status
    # On branch hello-jx
    nothing to commit (working directory clean)

    ```

*   执行**git pull**命令会报错。

    ```
    $ git pull
    You asked me to pull without telling me which branch you
    want to merge with, and 'branch.hello-jx.merge' in
    your configuration file does not tell me, either. Please
    specify which branch you want to use on the command line and
    try again (e.g. 'git pull <repository> <refspec>').
    See git-pull(1) for details.

    If you often merge with the same branch, you may want to
    use something like the following in your configuration file:

        [branch "hello-jx"]
        remote = <nickname>
        merge = <remote-ref>

        [remote "<nickname>"]
        url = <url>
        fetch = <refspec>

    See git-config(1) for details.

    ```

*   将上面命令执行中的错误信息翻译过来，就是：

    ```
    $ git pull
    您让我执行拉回操作，但是没有告诉我您希望与哪个远程分支进行合并，
    而且也没有通过配置 'branch.hello-jx.merge' 来告诉我。

    请在命令行提供足够的参数，如 'git pull <repository> <refspec>' 。
    或者如果您经常与同一个分支进行合并，可以和该分支建立跟踪。在配置
    中添加如下配置信息：

        [branch "hello-jx"]
        remote = <nickname>
        merge = <remote-ref>

        [remote "<nickname>"]
        url = <url>
        fetch = <refspec>

    ```

为什么用同样方法建立的分支`hello-1.x`和`hello-jx`，差距咋就那么大呢？奥秘就在于从远程分支创建本地分支，自动建立了分支间的跟踪，而从一个本地分支创建另外一个本地分支则没有。看看配置文件`.git/config`中是不是专门为分支`hello-1.x`创建了相应的配置信息？

```
 9 [branch "master"]
10   remote = origin
11   merge = refs/heads/master
12 [branch "hello-1.x"]
13   remote = origin
14   merge = refs/heads/hello-1.x

```

其中第 9-11 行是针对`master`分支设置的分支间跟踪，是在版本库克隆的时候自动建立的。而第 12-14 行是前面基于远程分支创建本地分支时建立的。至于分支`hello-jx`则没有建立相关配置。

如果希望在基于一个本地分支创建另外一个本地分支时也能够使用分支间的跟踪功能，就要在创建分支时提供`--track`参数。下面实践一下。

*   删除之前创建的`hello-jx`分支。

    ```
    $ git checkout master
    Switched to branch 'master'
    $ git branch -d hello-jx
    Deleted branch hello-jx (was ebcf6d6).

    ```

*   使用参数`--track`重新基于`hello-1.x`创建`hello-jx`分支。

    ```
    $ git checkout --track -b hello-jx hello-1.x
    Branch hello-jx set up to track local branch hello-1.x.
    Switched to a new branch 'hello-jx'

    ```

*   从 Git 库的配置文件中会看到为`hello-jx`分支设置的跟踪。

    因为跟踪的是本版本库的本地分支，所以第 16 行设置的远程版本库的名字为一个点。

    ```
    15 [branch "hello-jx"]
    16   remote = .
    17   merge = refs/heads/hello-1.x

    ```

## 3.5.3\. 远程版本库

名为`origin`的远程版本库是在版本库克隆时注册的，那么如何注册新的远程版本库呢？下面将版本库`file:///path/to/repos/hello-user1.git`以`new-remote`为名进行注册。

```
$ git remote add new-remote file:///path/to/repos/hello-user1.git

```

如果再打开版本库的配置文件`.git/config`会看到新的配置。

```
12 [remote "new-remote"]
13   url = file:///path/to/repos/hello-user1.git
14   fetch = +refs/heads/*:refs/remotes/new-remote/*

```

执行**git remote**命令，可以更为方便地显示已经注册的远程版本库。

```
$ git remote -v
new-remote      file:///path/to/repos/hello-user1.git (fetch)
new-remote      file:///path/to/repos/hello-user1.git (push)
origin  file:///path/to/repos/hello-world.git (fetch)
origin  file:///path/to/repos/hello-world.git (push)

```

现在执行**git fetch**并不会从新注册的 new-remote 远程版本库获取，因为当前分支设置的默认远程版本库为 origin。要想从 new-remote 远程版本库中获取，需要为**git fetch**命令增加一个参数`new-remote`。

```
$ git fetch new-remote
From file:///path/to/repos/hello-user1
 * [new branch]      hello-1.x  -> new-remote/hello-1.x
 * [new branch]      helper/master -> new-remote/helper/master
 * [new branch]      helper/v1.x -> new-remote/helper/v1.x
 * [new branch]      master     -> new-remote/master

```

从上面的命令输出中可以看出，远程版本库的分支复制到本地版本库前缀为`new-remote`的远程分支中去了。用**git branch -r**命令可以看到新增了几个远程分支。

```
$ git branch -r
  new-remote/hello-1.x
  new-remote/helper/master
  new-remote/helper/v1.x
  new-remote/master
  origin/HEAD -> origin/master
  origin/hello-1.x
  origin/helper/master
  origin/helper/v1.x
  origin/master

```

**更改远程版本库的地址**

如果远程版本库的 URL 地址改变，需要更换，该如何处理呢？手工修改`.git/config`文件是一种方法，用**git config**命令进行更改是第二种方法，还有一种方法是用**git remote**命令，如下：

```
$ git remote set-url new-remote file:///path/to/repos/hello-user2.git

```

可以看到注册的远程版本库的 URL 地址已经更改。

```
$ git remote -v
new-remote      file:///path/to/repos/hello-user2.git (fetch)
new-remote      file:///path/to/repos/hello-user2.git (push)
origin  file:///path/to/repos/hello-world.git (fetch)
origin  file:///path/to/repos/hello-world.git (push)

```

从上面的输出中可以发现每一个远程版本库都有两个 URL 地址，分别是执行**git fetch**和**git push**命令时用到的 URL 地址。既然有两个地址，就意味着这两个地址可以不同，用下面的命令可以为推送操作设置单独的 URL 地址。

```
$ git remote set-url --push new-remote /path/to/repos/hello-user2.git
$ git remote -v
new-remote      file:///path/to/repos/hello-user2.git (fetch)
new-remote      /path/to/repos/hello-user2.git (push)
origin  file:///path/to/repos/hello-world.git (fetch)
origin  file:///path/to/repos/hello-world.git (push)

```

当单独为推送设置了 URL 后，配置文件`.git/config`的对应`[remote]`小节也会增加一条新的名为`pushurl`的配置。如下：

```
12 [remote "new-remote"]
13   url = file:///path/to/repos/hello-user2.git
14   fetch = +refs/heads/*:refs/remotes/new-remote/*
15   pushurl = /path/to/repos/hello-user2.git

```

**更改远程版本库的名称**

如果对远程版本库的注册名称不满意，也可以进行修改。例如将 new-remote 名称修改为 user2，使用下面的命令：

```
$ git remote rename new-remote user2

```

完成改名后，不但远程版本库的注册名称更改过来了，就连远程分支名称都会自动进行相应的更改。可以通过执行**git remote**和**git branch -r**命令查看。

```
$ git remote
origin
user2
$ git branch -r
  origin/HEAD -> origin/master
  origin/hello-1.x
  origin/helper/master
  origin/helper/v1.x
  origin/master
  user2/hello-1.x
  user2/helper/master
  user2/helper/v1.x
  user2/master

```

**远程版本库更新**

当注册了多个远程版本库并希望获取所有远程版本库的更新时，Git 提供了一个简单的命令。

```
$ git remote update
Fetching origin
Fetching user2

```

如果某个远程版本库不想在执行**git remote update**时获得更新，可以通过参数关闭自动更新。例如下面的命令关闭远程版本库 user2 的自动更新。

```
$ git config remote.user2.skipDefaultUpdate true
$ git remote update
Fetching origin

```

**删除远程版本库**

如果想要删除注册的远程版本库，用**git remote**的**rm**子命令可以实现。例如删除注册的 user2 版本库。

```
$ git remote rm user2

```

## 3.5.4\. PUSH 和 PULL 操作与远程版本库

在 Git 分支一章，已经介绍过对于新建立的本地分支（没有建立和远程分支的追踪），执行**git push**命令是不会被推送到远程版本库中，这样的设置是非常安全的，避免了因为误操作将本地分支创建到远程版本库中。当不带任何参数执行**git push**命令，实际的执行过程是：

*   如果为当前分支设置了`<remote>`，即由配置`branch.<branchname>.remote`给出了远程版本库代号，则不带参数执行**git push**相当于执行了**git push <remote>**。

*   如果没有为当前分支设置`<remote>`，则不带参数执行**git push**相当于执行了**git push origin**。

*   要推送的远程版本库的 URL 地址由`remote.<remote>.pushurl`给出。如果没有配置，则使用`remote.<remote>.url`配置的 URL 地址。

*   如果为注册的远程版本库设置了`push`参数，即通过`remote.<remote>.push`配置了一个引用表达式，则使用该引用表达式执行推送。

*   否则使用“:”作为引用表达式。该表达式的含义是同名分支推送，即对所有在远程版本库有同名分支的本地分支执行推送。

    这也就是为什么在一个本地新建分支中执行**git push**推送操作不会推送也不会报错的原因，因为远程不存在同名分支，所以根本就没有对该分支执行推送，而推送的是其他分支（如果远程版本库有同名分支的话）。

在 Git 分支一章中就已经知道，如果需要在远程版本库中创建分支，则执行命令：**git push <remote> <new_branch>**。即通过将本地分支推送到远程版本库的方式在远程版本库中创建分支。但是在接下来的使用中会遇到麻烦：不能执行**git pull**操作（不带参数）将远程版本库中其他人推送的提交获取到本地。这是因为没有建立本地分支和远程分支的追踪，即没有设置`branch.<branchname>.remote`的值和`branch.<branchname>.merge`的值。

关于不带参数执行**git pull**命令解释如下：

*   如果为当前分支设置了`<remote>`，即由配置`branch.<branchname>.remote`给出了远程版本库代号，则不带参数执行**git pull**相当于执行了**git pull <remote>**。
*   如果没有为当前分支设置`<remote>`，则不带参数执行**git pull**相当于执行了**git pull origin**。
*   要获取的远程版本库的 URL 地址由`remote.<remote>.url`给出。
*   如果为注册的远程版本库设置了`fetch`参数，即通过`remote.<remote>.fetch`配置了一个引用表达式，则使用该引用表达式执行获取操作。
*   接下来要确定合并的分支。如果设定了`branch.<branchname>.merge`，则对其设定的分支执行合并，否则报错退出。

在执行**git pull**操作的时候可以通过参数`--rebase`设置使用变基而非合并操作，将本地分支的改动变基到跟踪分支上。为了避免因为忘记使用`--rebase`参数导致分支的合并，可以执行如下命令进行设置。注意将`<branchname>`替换为对应的分支名称。

```
$ git config branch.<branchname>.rebase true

```

有了这个设置之后，如果是在`<branchname>`工作分支中执行**git pull**命令，在遇到冲突（本地和远程分支出现偏离）的情况下，会采用变基操作，而不是默认的合并操作。

如果为本地版本库设置参数`branch.autosetuprebase`，值为`true`，则在基于远程分支建立本地追踪分支时，会自动配置`branch.<branchname>.rebase`参数，在执行**git pull**命令时使用变基操作取代默认的合并操作。

## 3.5.5\. 里程碑和远程版本库

远程版本库中的里程碑同步到本地版本库，会使用同样的名称，而不会像分支那样移动到另外的命名空间（远程分支）中，这可能会给本地版本库中的里程碑带来混乱。当和多个远程版本库交互时，这个问题就更为严重。

前面的 Git 里程碑一章已经介绍了当执行**git push**命令推送时，默认不会将本地创建的里程碑带入远程版本库，这样可以避免远程版本库上里程碑的泛滥。但是执行**git fetch**命令从远程版本库获取分支的最新提交时，如果获取的提交上建有里程碑，这些里程碑会被获取到本地版本库。当删除注册的远程版本库时，远程分支会被删除，但是该远程版本库引入的里程碑不会被删除，日积月累本地版本库中的里程碑可能会变得愈加混乱。

可以在执行**git fetch**命令的时候，设置不获取里程碑只获取分支及提交。通过提供`-n`或`--no-tags`参数可以实现。示例如下：

```
$ git fetch --no-tags file:///path/to/repos/hello-world.git \
      refs/heads/*:refs/remotes/hello-world/*

```

在注册远程版本库的时候，也可以使用`--no-tags`参数，避免将远程版本库的里程碑引入本地版本库。例如：

```
$ git remote add --no-tags hell-world \
      file:///path/to/repos/hello-world.git

```

## 3.5.6\. 分支和里程碑的安全性

通过前面章节的探讨，会感觉到 Git 的使用真的是太方便、太灵活了，但是需要掌握的知识点和窍门也太多了。为了避免没有经验的用户在团队共享的 Git 版本库中误操作，就需要对版本库进行一些安全上的设置。本书第 5 篇 Git 服务器搭建的相关章节会具体介绍如何配置用户授权等版本库安全性设置。

实际上 Git 版本库本身也提供了一些安全机制避免对版本库的破坏。

*   用 reflog 记录对分支的操作历史。

    默认创建的带工作区的版本库都会包含`core.logallrefupdates`为`true`的配置，这样在版本库中建立的每个分支都会创建对应的 reflog。但是创建的裸版本库默认不包含这个设置，也就不会为每个分支设置 reflog。如果团队的规模较小，可能因为分支误操作导致数据丢失，可以考虑为裸版本库添加`core.logallrefupdates`的相关配置。

*   关闭非快进式提交。

    如果将配置`receive.denyNonFastForwards`设置为`true`，则禁止一切非快进式推送。但这个配置有些矫枉过正，更好的方法是搭建基于 SSH 协议的 Git 服务器，通过钩子脚本更灵活的进行配置。例如：允许来自某些用户的强制提交，而其他用户不能执行非快进式推送。

*   关闭分支删除功能。

    如果将配置`receive.denyDeletes`设置为`true`，则禁止删除分支。同样更好的方法是通过架设基于 SSH 协议的 Git 服务器，配置分支删除的用户权限。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 3.6\. 补丁文件交互

之前各个章节版本库间的交互都是通过**git push**和/或**git pull**命令实现的，这是 Git 最主要的交互模式，但并不是全部。使用补丁文件是另外一种交互方式，适用于参与者众多的大型项目进行分布式开发。例如 Git 项目本身的代码提交就主要由贡献者通过邮件传递补丁文件实现的。作者在写书过程中发现了 Git 的两个 bug，就是以补丁形式通过邮件贡献给 Git 项目的，下面两个链接就是相关邮件的存档。

*   关于 Git 文档错误的 bugfix：

    [`marc.info/?l=git&m=129248415230151`](http://marc.info/?l=git&m=129248415230151)

*   关于**git-apply**的一个 bugfix：

    [`article.gmane.org/gmane.comp.version-control.git/162100`](http://article.gmane.org/gmane.comp.version-control.git/162100)

这种使用补丁文件进行提交的方式可以提高项目的参与度。因为任何人都可以参与项目的开发，只要会将提交转化为补丁，会发邮件即可。

## 3.6.1\. 创建补丁

Git 提供了将提交批量转换为补丁文件的命令：**git format-patch**。该命令后面的参数是一个版本范围列表，会将包含在此列表中的提交一一转换为补丁文件，每个补丁文件包含一个序号并从提交说明中提取字符串作为文件名。

下面演示一下在 user1 工作区中，如何将`master`分支的最近 3 个提交转换为补丁文件。

*   进入 user1 工作区，切换到`master`分支。

    ```
    $ cd /path/to/user1/workspace/hello-world/
    $ git checkout master
    $ git pull

    ```

*   执行下面的命令将最近三个提交转换为补丁文件。

    ```
    $ git format-patch -s HEAD~3..HEAD
    0001-Fix-typo-help-to-help.patch
    0002-Add-I18N-support.patch
    0003-Translate-for-Chinese.patch

    ```

在上面的**git format-patch**命令中使用了`-s`参数，会在导出的补丁文件中添加当前用户的签名。这个签名并非 GnuPG 式的数字签名，不过是将作者姓名添加到提交说明中而已，和在本书第 2 篇开头介绍的**git commit -s**命令的效果相同。虽然签名很不起眼，但是对于以补丁方式提交数据却非常重要，因为以补丁方式提交可能因为合并冲突或其他原因使得最终提交的作者显示为管理员（提交者）的 ID，在提交说明中加入原始作者的署名信息大概是作者唯一露脸的机会。如果在提交时忘了使用`-s`参数添加签名，可以在用**git format-path**命令创建补丁文件的时候补救。

看一下补丁文件的文件头，在下面代码中的第 7 行可以看到新增的签名。

```
 1 From d81896e60673771ef1873b27a33f52df75f70515 Mon Sep 17 00:00:00 2001
 2 From: user1 <user1@sun.ossxp.com>
 3 Date: Mon, 3 Jan 2011 23:48:56 +0800
 4 Subject: [PATCH 1/3] Fix typo: -help to --help.
 5
 6
 7 Signed-off-by: user1 <user1@sun.ossxp.com>
 8 ---
 9  src/main.c |    2 +-
10  1 files changed, 1 insertions(+), 1 deletions(-)

```

补丁文件有一个类似邮件一样的文件头（第 1-4 行），提交日志的第一行作为邮件标题（Subject），其余提交说明作为邮件内容（如果有的话），文件补丁用三个横线和提交说明分开。

实际上这些补丁文件可以直接拿来作为邮件发送给项目的负责人。Git 提供了一个辅助邮件发送的命令**git send-email**。下面用该命令将这三个补丁文件以邮件形式发送出去。

```
$ git send-email *.patch
0001-Fix-typo-help-to-help.patch
0002-Add-I18N-support.patch
0003-Translate-for-Chinese.patch
The following files are 8bit, but do not declare a Content-Transfer-Encoding.
    0002-Add-I18N-support.patch
    0003-Translate-for-Chinese.patch
Which 8bit encoding should I declare [UTF-8]?
Who should the emails appear to be from? [user1 <user1@sun.ossxp.com>]

Emails will be sent from: user1 <user1@sun.ossxp.com>
Who should the emails be sent to? jiangxin
Message-ID to be used as In-Reply-To for the first email?
...
Send this email? ([y]es|[n]o|[q]uit|[a]ll): a
...

```

命令**git send-email**提供交互式字符界面，输入正确的收件人地址，邮件就批量地发送出去了。

## 3.6.2\. 应用补丁

在前面通过**git send-email**命令发送邮件给`jiangxin`用户。现在使用 Linux 上的**mail**命令检查一下邮件。

```
$ mail
Mail version 8.1.2 01/15/2001\.  Type ? for help.
"/var/mail/jiangxin": 3 messages 3 unread
>N  1 user1@sun.ossxp.c  Thu Jan 13 18:02   38/1120  [PATCH 1/3] Fix typo: -help to --help.
 N  2 user1@sun.ossxp.c  Thu Jan 13 18:02  227/6207  =?UTF-8?q?=5BPATCH=202/3=5D=20Add=20I18N=20support=2E?=
 N  3 user1@sun.ossxp.c  Thu Jan 13 18:02   95/2893  =?UTF-8?q?=5BPATCH=203/3=5D=20Translate=20for=20Chinese=2E?=
&

```

如果邮件不止这三封，需要将三个包含补丁的邮件挑选出来保存到另外的文件中。 在 mail 命令的提示符(&)下输入命令。

```
& s 1-3 user1-mail-archive
"user1-mail-archive" [New file]
& q

```

上面的操作在本地创建了一个由开发者 user1 的补丁邮件组成的归档文件`user1-mail-archive`，这个文件是 mbox 格式的，可以用**mail**命令打开。

```
$ mail -f user1-mail-archive
Mail version 8.1.2 01/15/2001\.  Type ? for help.
"user1-mail-archive": 3 messages
>   1 user1@sun.ossxp.c  Thu Jan 13 18:02   38/1121  [PATCH 1/3] Fix typo: -help to --help.
    2 user1@sun.ossxp.c  Thu Jan 13 18:02  227/6208  =?UTF-8?q?=5BPATCH=202/3=5D=20Add=20I18N=20support=2E?=
    3 user1@sun.ossxp.c  Thu Jan 13 18:02   95/2894  =?UTF-8?q?=5BPATCH=203/3=5D=20Translate=20for=20Chinese=2E?=
& q

```

保存在 mbox 中的邮件可以批量的应用在版本库中，使用**git am**命令。`am`是`apply email`的缩写。下面就演示一下如何应用补丁。

*   基于`HEAD~3`版本创建一个本地分支，以便在该分支下应用补丁。

    ```
    $ git checkout -b user1 HEAD~3
    Switched to a new branch 'user1'

    ```

*   将 mbox 文件`user1-mail-archive`中的补丁全部应用在当前分支上。

    ```
    $ git am user1-mail-archive
    Applying: Fix typo: -help to --help.
    Applying: Add I18N support.
    Applying: Translate for Chinese.

    ```

*   补丁成功应用上了，看看提交日志。

    ```
    $ git log -3 --pretty=fuller
    commit 2d9276af9df1a2fdb71d1e7c9ac6dff88b2920a1
    Author:     Jiang Xin <jiangxin@ossxp.com>
    AuthorDate: Thu Jan 13 18:02:03 2011 +0800
    Commit:     user1 <user1@sun.ossxp.com>
    CommitDate: Thu Jan 13 18:21:16 2011 +0800

        Translate for Chinese.

        Signed-off-by: Jiang Xin <jiangxin@ossxp.com>
        Signed-off-by: user1 <user1@sun.ossxp.com>

    commit 41227f492ad37cdd99444a5f5cc0c27288f2bca4
    Author:     Jiang Xin <jiangxin@ossxp.com>
    AuthorDate: Thu Jan 13 18:02:02 2011 +0800
    Commit:     user1 <user1@sun.ossxp.com>
    CommitDate: Thu Jan 13 18:21:15 2011 +0800

        Add I18N support.

        Signed-off-by: Jiang Xin <jiangxin@ossxp.com>
        Signed-off-by: user1 <user1@sun.ossxp.com>

    commit 4a3380fb7ae90039633dec84acc2aab85398efad
    Author:     user1 <user1@sun.ossxp.com>
    AuthorDate: Thu Jan 13 18:02:01 2011 +0800
    Commit:     user1 <user1@sun.ossxp.com>
    CommitDate: Thu Jan 13 18:21:15 2011 +0800

        Fix typo: -help to --help.

        Signed-off-by: user1 <user1@sun.ossxp.com>

    ```

从提交信息上可以看出：

*   提交的时间信息使用了邮件发送的时间。
*   作者（Author）的信息被保留，和补丁文件中的一致。
*   提交者（Commit）全都设置为`user1`，因为提交是在`user1`的工作区完成的。
*   提交说明中的签名信息被保留。实际上**git am**命令也可以提供`-s`参数，在提交说明中附加执行命令用户的签名。

对于不习惯在控制台用**mail**命令接收邮件的用户，可以通过邮件附件，U 盘或其他方式获取**git format-patch**生成的补丁文件，将补丁文件保存在本地，通过管道符调用**git am**命令应用补丁。

```
$ ls *.patch
0001-Fix-typo-help-to-help.patch  0002-Add-I18N-support.patch  0003-Translate-for-Chinese.patch
$ cat *.patch | git am
Applying: Fix typo: -help to --help.
Applying: Add I18N support.
Applying: Translate for Chinese.

```

Git 还提供一个命令**git apply**，可以应用一般格式的补丁文件，但是不能执行提交，也不能保持补丁中的作者信息。实际上**git apply**命令和 GNU**patch**命令类似，细微差别在本书第 7 篇第三十八章“补丁中的二进制文件”予以介绍。

## 3.6.3\. StGit 和 Quilt

一个复杂功能的开发一定是由多个提交来完成的，对于在以接收和应用补丁文件为开发模式的项目中，复杂的功能需要通过多个补丁文件来完成。补丁文件因为要经过审核才能被接受，因此针对一个功能的多个补丁文件一定要保证各个都是精品：补丁 1 用来完成一个功能点，补丁 2 用来完成第二个功能点，等等。一定不能出现这样的情况：补丁 3 用于修正补丁 1 的错误，补丁 10 改正了补丁 7 中的文字错误，等等。这样就带来补丁管理的难题。

实际上基于特性分支的开发又何尝不是如此？在将特性分支归并到开发主线前，要接受团队的评审，特性分支的开发者一定想将特性分支上的提交进行重整，把一些提交合并或者拆分。使用变基命令可以实现提交的重整，但是操作起来会比较困难，有什么好办法呢？

### 3.6.3.1\. StGit

Stacked Git（[`www.procode.org/stgit/`](http://www.procode.org/stgit/)）简称 StGit 就是解决上述两个难题的答案。实际上 StGit 在设计上参考了一个著名的补丁管理工具 Quilt，并且可以输出 Quilt 兼容的补丁列表。在本节的后半部分会介绍 Quilt。

StGit 是一个 Python 项目，安装起来还是很方便的。在 Debian/Ubuntu 下，可以直接通过包管理器安装：

```
$ sudo aptitude install stgit stgit-contrib

```

下面还是用`hello-world`版本库，进行 StGit 的实践。

*   首先检出`hello-world`版本库。

    ```
    $ cd /path/to/my/workspace/
    $ git clone file:///path/to/repos/hello-world.git stgit-demo
    $ cd stgit-demo

    ```

*   在当前工作区初始化 StGit。

    ```
    $ stg init

    ```

*   现在补丁列表为空。

    ```
    $ stg series

    ```

*   将最新的三个提交转换为 StGit 补丁。

    ```
    $ stg uncommit -n 3
    Uncommitting 3 patches ...
      Now at patch "translate-for-chinese"
    done

    ```

*   现在补丁列表中有三个文件了。

    第一列是补丁的状态符号。加号(+)代表该补丁已经应用在版本库中，大于号(>)用于标识当前的补丁。

    ```
    $ stg ser
    + fix-typo-help-to-help
    + add-i18n-support
    > translate-for-chinese

    ```

*   现在查看`master`分支的日志，发现和之前没有两样。

    ```
    $ git log -3 --oneline
    c4acab2 Translate for Chinese.
    683448a Add I18N support.
    d81896e Fix typo: -help to --help.

    ```

*   执行 StGit 补丁出栈的命令，会将补丁撤出应用。使用`-a`参数会将所有补丁撤出应用。

    ```
    $ stg pop
    Popped translate-for-chinese
    Now at patch "add-i18n-support"
    $ stg pop -a
    Popped add-i18n-support -- fix-typo-help-to-help
    No patch applied

    ```

*   再来看版本库的日志，会发现最新的三个提交都不见了。

    ```
    $ git log -3 --oneline
    10765a7 Bugfix: allow spaces in username.
    0881ca3 Refactor: use getopt_long for arguments parsing.
    ebcf6d6 blank commit for GnuPG-signed tag test.

    ```

*   查看补丁列表的状态，会看到每个补丁前都用减号(-)标识。

    ```
    $ stg ser
    - fix-typo-help-to-help
    - add-i18n-support
    - translate-for-chinese

    ```

*   执行补丁入栈，即应用补丁，使用命令**stg push**或者**stg goto**命令，注意**stg push**命令和**git push**命令风马牛不相及。

    ```
    $ stg push
    Pushing patch "fix-typo-help-to-help" ... done (unmodified)
    Now at patch "fix-typo-help-to-help"
    $ stg goto add-i18n-support
    Pushing patch "add-i18n-support" ... done (unmodified)
    Now at patch "add-i18n-support"

    ```

*   现在处于应用`add-i18n-support`补丁的状态。这个补丁有些问题，本地化语言模板有错误，我们来修改一下。

    ```
    $ cd src/
    $ rm locale/helloworld.pot
    $ make po
    xgettext -s -k_ -o locale/helloworld.pot main.c
    msgmerge locale/zh_CN/LC_MESSAGES/helloworld.po locale/helloworld.pot -o locale/temp.po
    . 完成。
    mv locale/temp.po locale/zh_CN/LC_MESSAGES/helloworld.po

    ```

*   现在查看工作区，发现工作区有改动。

    ```
    $ git status -s
     M locale/helloworld.pot
     M locale/zh_CN/LC_MESSAGES/helloworld.po

    ```

*   不要将改动添加暂存区，也不要提交，而是执行**stg refresh**命令，更新补丁。

    ```
    $ stg refresh
    Now at patch "add-i18n-support"

    ```

*   这时再查看工作区，发现本地修改不见了。

    ```
    $ git status -s

    ```

*   执行**stg show**会看到当前的补丁`add-i18n-support`已经更新。

    ```
    $ stg show
    ...

    ```

*   将最后一个补丁应用到版本库，遇到冲突。这是因为最后一个补丁是对中文本地化文件的翻译，因为翻译前的模板文件被更改了所以造成了冲突。

    ```
    $ stg push
    Pushing patch "translate-for-chinese" ... done (conflict)
    Error: 1 merge conflict(s)
           CONFLICT (content): Merge conflict in
           src/locale/zh_CN/LC_MESSAGES/helloworld.po
    Now at patch "translate-for-chinese"

    ```

*   这个冲突文件很好解决，直接编辑冲突文件`helloworld.po`即可。编辑好之后，注意一下第 50 行和第 62 行是否像下面写的一样。

    ```
    50 "    hello -h, --help\n"
    51 "            显示本帮助页。\n"
    ...
    61 msgid "Hi,"
    62 msgstr "您好,"

    ```

*   执行**git add**命令完成冲突解决。

    ```
    $ git add locale/zh_CN/LC_MESSAGES/helloworld.po

    ```

*   不要提交，而是使用**stg refresh**命令更新补丁，同时更新提交。

    ```
    $ stg refresh
    Now at patch "translate-for-chinese"
    $ git status -s

    ```

*   看看修改后的程序，是不是都能显示中文了。

    ```
    $ ./hello
    世界你好。
    (version: v1.0-5-g733c6ea)
    $ ./hello Jiang Xin
    您好, Jiang Xin.
    (version: v1.0-5-g733c6ea)
    $ ./hello -h
    ...

    ```

*   导出补丁，使用命令**stg export**。导出的是 Quilt 格式的补丁集。

    ```
    $ cd /path/to/my/workspace/stgit-demo/
    $ stg export -d patches
    Checking for changes in the working directory ... done

    ```

*   看看导出补丁的目标目录。

    ```
    $ ls patches/
    add-i18n-support  fix-typo-help-to-help  series  translate-for-chinese

    ```

*   其中文件`series`是补丁文件的列表，列在前面的补丁先被应用。

    ```
    $ cat patches/series
    # This series applies on GIT commit d81896e60673771ef1873b27a33f52df75f70515
    fix-typo-help-to-help
    add-i18n-support
    translate-for-chinese

    ```

通过上面的演示可以看出 StGit 可以非常方便的对提交进行整理，整理提交时无需使用复杂的变基命令，而是采用：提交 StGit 化，修改文件，执行**stg refresh**的工作流程即可更新补丁和提交。StGit 还可以将补丁导出为补丁文件，虽然导出的补丁文件没有像**git format-patch**那样加上代表顺序的数字前缀，但是用文件`series`标注了补丁文件的先后顺序。实际上可以在执行**stg export**时添加`-n`参数为补丁文件添加数字前缀。

StGit 还有一些功能，如合并补丁/提交，插入新补丁/提交等，请参照 StGit 帮助，恕不一一举例。

### 3.6.3.2\. Quilt

Quilt 是一款补丁列表管理软件，用 Shell 语言开发，安装也很简单，在 Debian/Ubuntu 上直接用下面的命令即可安装：

```
$ sudo aptitude install quilt

```

Quilt 约定俗成将补丁集放在项目根目录下的子目录`patches`中，否则需要通过环境变量`QUILT_PATCHES`对路径进行设置。为了减少麻烦，在上面用**stg export**导出补丁的时候就导出到了`patches`目录下。

简单说一下 Quilt 的使用，会发现真的和 StGit 很像，实际上是先有的 Quilt，后有的 StGit。

*   重置到三个提交前的版本，否则应用补丁的时候会失败。还不要忘了删除`src/locale`目录。

    ```
    $ git reset --hard HEAD~3
    $ rm -rf src/locale/

    ```

*   显示补丁列表

    ```
    $ quilt series
    01-fix-typo-help-to-help
    02-add-i18n-support
    03-translate-for-chinese

    ```

*   应用一个补丁。

    ```
    $ quilt push
    Applying patch 01-fix-typo-help-to-help
    patching file src/main.c

    Now at patch 01-fix-typo-help-to-help

    ```

*   下一个补丁是什么？

    ```
    $ quilt next
    02-add-i18n-support

    ```

*   应用全部补丁。

    ```
    $ quilt push -a
    Applying patch 02-add-i18n-support
    patching file src/Makefile
    patching file src/locale/helloworld.pot
    patching file src/locale/zh_CN/LC_MESSAGES/helloworld.po
    patching file src/main.c

    Applying patch 03-translate-for-chinese
    patching file src/locale/zh_CN/LC_MESSAGES/helloworld.po

    Now at patch 03-translate-for-chinese

    ```

Quilt 的功能还有很多，请参照 Quilt 的联机帮助，恕不一一举例。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.