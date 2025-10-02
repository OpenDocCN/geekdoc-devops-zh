# 4\. Git 协同模型

分布式的版本控制会不会造成开发中的无序，导致版本管理的崩溃？对于习惯了如 Subversion 这类的集中式版本控制系统的用户，脑子里一定会有这个疑问。

作为分布式版本控制系统，每一个 Git 克隆都是一个完整的版本库，可以提供一个版本控制服务器所能提供的一切服务，即每个人的机器都是一台服务器。与之相反，像 Subversion 那样的集中式版本控制系统，只拥有唯一的版本控制服务器，所有团队成员都使用客户端与之交互，大部分操作要通过网络传输来实现。对于习惯了和唯一服务器交互的团队，转换到 Git 后，该如何协同团队的工作呢？在第二十一章“经典 Git 协同模型”中会介绍集中式和金字塔式两种主要的协同工作模型。

基于某个项目进行二次开发，需要使用不同的工作模型。原始的项目称为上游项目，不能直接在上游项目中提交，可能是因为授权的原因或者是因为目标用户的需求不同。这种基于上游项目进行二次开发，实际上是对各个独特的功能分支进行管理，同时又能对上游项目的开发进度进行兼收并蓄式的合并。第二十二章“Topgit 协同模型”会重点介绍这一方面的内容。

多个版本库组成一个项目，在实际应用中并不罕见。一部分原因可能是版本库需要依赖第三方的版本库，这时第二十三章介绍的“子模组协同模型”就可以派上用场。有的时候还要对第三方的版本库进行定制，第二十四章“子树合并”提供了一个解决方案。有的时候，为了管理方便（授权或者项目确实太庞杂），多个版本库共同组成一个大的项目，例如 Google Android 项目就是由近 200 个版本库组成的。第二十五章“Android 式多版本库协同”提供了一个非常有趣的解决方案，解决了“子模组协同模型”的管理上的难题。

在本篇的最后（第二十六章），会介绍 git-svn 这一工具。可能因为公司对代码严格的授权要求，而不能将公司的版本控制服务器从 Subversion 迁移到 Git（实际可以通过对 Git 版本库细粒度拆分实现授权管理），可是这并不能阻止个人使用 git-svn 作为前端工具操作 Subversion 版本库。git-svn 可以让 Git 和 Subversion 完美的协同工作。

目录:

*   4.1\. 经典 Git 协同模型
    *   4.1.1\. 集中式协同模型
        *   4.1.1.1\. 使用集中协同模型
        *   4.1.1.2\. Gerrit 特殊的集中协同模型
*   4.2\. 金字塔式协同模型
    *   4.2.1\. 贡献者开放只读版本库
    *   4.2.2\. 以补丁方式贡献代码
*   4.3\. Topgit 协同模型
    *   4.3.1\. 作者版本控制系统三个里程碑
    *   4.3.2\. Topgit 原理
    *   4.3.3\. Topgit 的安装
    *   4.3.4\. Topgit 的使用
        *   4.3.4.1\. **tg help**命令
        *   4.3.4.2\. **tg create**命令
        *   4.3.4.3\. **tg info**命令
        *   4.3.4.4\. **tg update**命令
        *   4.3.4.5\. **tg summary**命令
        *   4.3.4.6\. **tg remote**命令
        *   4.3.4.7\. **tg push**命令
        *   4.3.4.8\. **tg depend**命令
        *   4.3.4.9\. **tg base**命令
        *   4.3.4.10\. **tg delete**命令
        *   4.3.4.11\. **tg patch**命令
        *   4.3.4.12\. **tg export**命令
        *   4.3.4.13\. **tg import**命令
        *   4.3.4.14\. **tg log**命令
        *   4.3.4.15\. **tg mail**命令
        *   4.3.4.16\. **tg graph**命令
    *   4.3.5\. Topgit hacks
    *   4.3.6\. Topgit 使用中的注意事项
*   4.4\. 子模组协同模型
    *   4.4.1\. 创建子模组
    *   4.4.2\. 克隆带子模组的版本库
    *   4.4.3\. 在子模组中修改和子模组的更新
    *   4.4.4\. 隐性子模组
    *   4.4.5\. 子模组的管理问题
*   4.5\. 子树合并
    *   4.5.1\. 引入外部版本库
    *   4.5.2\. 子目录方式合并外部版本库
    *   4.5.3\. 利用子树合并跟踪上游改动
    *   4.5.4\. 子树拆分
    *   4.5.5\. `git subtree`插件
        *   4.5.5.1\. **git subtree add**
        *   4.5.5.2\. **git subtree merge**
        *   4.5.5.3\. **git subtree pull**
        *   4.5.5.4\. **git subtree split**
        *   4.5.5.5\. **git subtree push**
*   4.6\. Android 式多版本库协同
    *   4.6.1\. 关于 repo
    *   4.6.2\. 安装 repo
    *   4.6.3\. repo 和清单库的初始化
    *   4.6.4\. 清单库和清单文件
    *   4.6.5\. 同步项目
    *   4.6.6\. 建立 Android 代码库本地镜像
    *   4.6.7\. Repo 的命令集
        *   4.6.7.1\. **repo init**命令
        *   4.6.7.2\. **repo sync**命令
        *   4.6.7.3\. **repo start**命令
        *   4.6.7.4\. **repo status**命令
        *   4.6.7.5\. **repo checkout**命令
        *   4.6.7.6\. **repo branches**命令
        *   4.6.7.7\. **repo diff**命令
        *   4.6.7.8\. **repo stage**命令
        *   4.6.7.9\. **repo upload**命令
        *   4.6.7.10\. **repo download**命令
        *   4.6.7.11\. **repo rebase**命令
        *   4.6.7.12\. **repo prune**命令
        *   4.6.7.13\. **repo abandon**命令
        *   4.6.7.14\. 其他命令
    *   4.6.8\. Repo 命令的工作流
    *   4.6.9\. 好东西不能 Android 独享
        *   4.6.9.1\. Repo+Gerrit 模式
        *   4.6.9.2\. Repo 无审核模式
        *   4.6.9.3\. 改进的 Repo 无审核模式
*   4.7\. Git 和 SVN 协同模型
    *   4.7.1\. 使用 git-svn 的一般流程
    *   4.7.2\. git-svn 的奥秘
        *   4.7.2.1\. Git 库配置文件的扩展及分支映射
        *   4.7.2.2\. Git 工作分支和 Subversion 如何对应？
        *   4.7.2.3\. 其他辅助文件
    *   4.7.3\. 多样的 git-svn 克隆模式
    *   4.7.4\. 共享 git-svn 的克隆库
    *   4.7.5\. git-svn 的局限

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 4.1\. 经典 Git 协同模型

## 4.1.1\. 集中式协同模型

可以像集中式版本控制系统那样使用 Git，在一个大家都可以访问到的服务器上架设 Git 服务器，每个人从该服务器克隆代码，本地提交推送到服务器上。如图 21-1 所示。

> ![../_images/central-model.png](img/central-model.png)
> 
> 图 21-1：集中式协同模型

回忆一下在使用 Subversion 等集中式版本控制系统时，对服务器管理上的要求：

*   只允许拥有帐号的用户访问版本库。
*   甚至只允许用户访问版本库中的某些路径，其他路径不能访问。
*   特定目录只允许特定用户执行写操作。
*   服务器可以通过钩子实现特殊功能，如对提交说明（commit log）的检查，数据镜像等。

对于这些需求，Git 大部分都能支持，甚至能够做到更多：

*   能够设置谁能够访问版本库，谁不能访问版本库。
*   具有更为丰富的写操作授权。可以限制哪些分支不允许写，哪些路径不允许写。
*   可以设置谁可以创建新的分支。
*   可以设置谁可以创建新的版本库。
*   可以设置谁可以强制更新。
*   服务器端同样支持钩子脚本。

但是也要承认，在“读授权”上 Git 做不到很精细，这是分布式版本控制系统的机制使然。按模块分解 Git 版本库，并结合后面介绍的多版本库协同解决方案可以克服 Git 读授权的局限。

*   Git 不支持对版本库读取的精确授权，只能是非零即壹的授权。即或者能够读取一个版本库的全部，或者什么也读不到。
*   因为 Git 的提交是一个整体，提交中包含了完整目录树（tree）的哈希，因此完整性不容破坏。
*   Git 是分布式版本控制系统，如果允许不完整的克隆，那么本地就是截然不同的版本库，在向服务器推送的时候，会被拒绝或者产生新的分支。

**用 Gitolite 架设集中式的 Git 服务器**

对于集中式的工作模型的核心就是架设集中式的 Git 服务器，而且尽量能够满足前面提到的对授权和版本库管理上的需求。在本书第 5 篇介绍服务器部署的时候，会介绍用 Gitolite 架设 Git 服务器，可以实现集中式协同模型对版本库授权和管理上的要求。

### 4.1.1.1\. 使用集中协同模型

对于简单的代码修改，可以像传统集中式版本控制系统（Subversion）中那样工作，参照图 21-2 所示的工作流程图。

![../_images/central-model-workflow-1.png](img/central-model-workflow-1.png)

图 21-2：集中式协同模型工作流 1

但是对于复杂的修改（代码重构/增加复杂功能），这个工作模式就有些不合适了。

第一个问题是：很容易将不成熟代码带入共享的版本库，破坏共享版本库相应分支的代码稳定性。例如破坏编译、破坏每日集成。这是因为开发者克隆版本库后，直接工作在缺省的跟踪分支上，当不小心执行**git push**命令，就会将自己的提交推送到服务器上。

为了避免上面的问题，开发者可能会延迟推送，例如在新功能定制的整个过程（一个月）只在本地提交，而不向服务器推送，这样产生更严重的问题：数据丢失。开发者可能因为操作系统感染病毒，或者不小心的目录删除，或者硬盘故障导致工作成果的彻底丢失，这对个人和团队来说都是灾难。

解决这个问题的方法也很简单，就是在本地创建本地分支（功能分支），并且同时在服务器端（共享版本库）也创建自己独享的功能分支。本地提交推送到共享版本库的自己独享的分支上。当开发完成之后，将功能分支合并到主线上，推送到共享版本库，完成开发。当然如果该特性分支不再需要时需要作些清理工作。参见图 21-3 所示的工作流程图。

![../_images/central-model-workflow-2.png](img/central-model-workflow-2.png)

图 21-3：集中式协同模型工作流 2

### 4.1.1.2\. Gerrit 特殊的集中协同模型

**传统集中式协同模型的缺点**

传统集中式协同模型的主要问题是在管理上：谁能够向版本库推送？可以信赖某人向版本库推送么？

对于在一个相对固定的团队内部使用集中式协同模型没有问题，因为大家彼此信赖，都熟悉项目相关领域。但是对于公开的项目（开源项目）来说，采用集中式的协同模型，必然只能有部分核心人员具有“写”权限，很多有能力的参与者被拒之门外，这不利于项目的发展。因此集中式协同模型主要应用在公司范围内和商业软件开发中，而不会成为开源项目的首选。

**强制代码审核的集中式协同模型**

Android 项目采用了独树一帜的集中式管理模型——通过 Gerrit 架设的审核服务器对提交进行强制审核。Android 是由大约近 200 个 Git 版本库组成的庞大的项目，为了对庞大的版本库进行管理，Android 项目开发了两个工具`repo`和`Gerrit`进行版本库的管理。其中 Gerrit 服务器为 Android 项目引入了特别的集中式协同模型。

Gerrit 服务器通过 SSH 协议管理 Git 版本库，并实现了一个 Web 界面的评审工作流。任何注册用户都可以参与到项目中来，都可以推送 Git 提交到 Gerrit 管理下的 Git 版本库（通过 Gerrit 启动的特殊 SSH 端口）。Git 推送不能直接推送到分支，而是推送到特殊的引用`refs/for/<branch-name>`，此提交会自动转换为形如`refs/changes/<nn>/<review-id>/<patch-set>`的补丁集，此补丁集在 Gerrit 的 Web 界面中显示为对应的评审任务。评审任务进入审核流程，当通过相关负责人的审核后，才被接受，被合并到正式的版本库中。

在本书的第 5 篇第三十二章“Gerrit 代码审核服务器”中会详细介绍 Gerrit 代码审核服务器的部署和使用。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 4.2\. 金字塔式协同模型

自从分布式版本库控制系统（Mercurial/Hg、Bazaar、Git 等）诞生之后，有越来越多的开源项目迁移了版本控制系统，例如从 Subversion 或 CVS 迁移到分布式版本控制系统。因为众多的开源项目逐渐认识到，集中式的版本控制管理方式阻止了更多的人参与项目的开发，对项目的发展不利。

集中式版本控制系统的最大问题是，如果没有在服务器端授权，就无法提交，也就无法保存自己的更改。开源项目虽然允许所有人访问代码库，但是不可能开放“写操作”授权给所有的人，否则代码质量无法控制（Gerrit 审核服务器是例外）。于此相对照的是，在使用了分布式版本控制系统之后，任何人都可以在本地克隆一个和远程版本库一模一样的版本库，本地的版本库允许任何操作，这就极大的调动了开发者投入项目研究的积极性。

分布式的开发必然带来协同的问题，如何能够让一个素不相识的开发者将他的贡献提交到项目中？如何能够最大化的发动和汇聚全球智慧？开源社区逐渐发展出金字塔模型，而这也是必然之选。

> ![../_images/distrabute-model.png](img/distrabute-model.png)
> 
> 图：金字塔式协同模型

金字塔模型的含义是，虽然理论上每个开发者的版本库都是平等的，但是会有一个公认的权威的版本库，这个版本库由一个或者多个核心开发者负责维护（具有推送的权限）。核心的开发人员负责审核其他贡献者的提交，审核可以通过邮件传递的补丁或者访问（PULL）贡献者开放的代码库进行。由此构成了由核心开发团队为顶层的所有贡献者共同参与的开发者金字塔。

Linux 社区就是典型的金字塔结构。Linus Torvalds 的版本库被公认为是官方的版本库，允许核心成员的提交。其他贡献者的提交必须经过一个或多个核心成员的审核后，才能经由核心成员代为推送的到官方版本库。

采用这种金字塔式协同模型不需要复杂的 Git 服务器设置，只需要项目管理者提供一个让其他人只读访问的版本库。当然管理者要能够通过某种方法向该版本库推送，以便其他人能够通过该版本库获得更新。

## 4.2.1\. 贡献者开放只读版本库

因为不能直接向项目只读共享的版本库提交，为了能让项目的管理者获取自己的提交，贡献者需要提供项目管理者访问自己版本库的方法。建立一个自己所有的只读共享版本库是一个简单易行的方法。在第 5 篇搭建 Git 服务器的相关章节，会介绍几种快速搭建只读 Git 版本库的方法，包括：用 HTTP 智能协议搭建 Git 服务器，用 Git 协议搭建 Git 服务器。

贡献者在自己的只读共享版本库建立后，需要对自己贡献的提交进行一下检查和整理。

*   贡献的提交要处于一个单独的特性分支中，并且要为该特性分支取一个有意义的名字。

    使用贡献者的名字以及简单的概括性文字是非常好的特性分支名。例如为自己的特性分支命名为`jiangxin/fix-bug-xxx`。

*   贡献的提交是否是基于上游对应分支的最新提交？如果不是需要变基到上游最新提交，以免产生合并。

    项目的管理者会尽量避免不必要的合并，因此会要求贡献者的提交尽量基于项目的最新提交。使用下面的方式建立跟踪远程分支的本地分支，可以很简单的实现在执行**git pull**操作时使用变基操作取代合并操作。

    ```
    $ git checkout -b jiangxin/fix-bug-xxx origin/master
    $ git config branch.jiangxin/fix-bug-xxx.rebase true
    hack, hack, hack
    $ git pull

    ```

然后贡献者就可以向项目管理者发送通知邮件，告诉项目管理者有贡献的代码等待他/她的审核。邮件中大致包括以下内容：

*   为什么要修改项目的代码。
*   相应的修改是否经过了测试，或者提交中是否包含了单元测试。
*   自己版本库的访问地址。
*   特性分支名称。

## 4.2.2\. 以补丁方式贡献代码

使用补丁文件方式贡献代码也是开源项目常用的协同方式。Git 项目本身就是采用该方式运作的。

*   每个用户先在本地版本库修改代码。
*   修改完成后，通过执行**git format-patch**命令将提交转换为补丁。
*   如果提交很多且比较杂乱，可以考虑使用 StGit 对提交进行重整。
*   调用**git send-email**命令或者通过图形界面的邮件客户端软件将补丁发到邮件列表以及项目维护者。
*   项目维护者认可贡献者提交的补丁后，执行**git am**命令应用补丁。

在第 3 篇第二十章“补丁文件交互”已经详细介绍了该模式的工作流程，请参考相关章节。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 4.3\. Topgit 协同模型

如果没有 Topgit，就不会有此书。因为发现了 Topgit，才让作者下定决心在公司大范围推广 Git；因为 Topgit，激发了作者对 Git 的好奇之心。

## 4.3.1\. 作者版本控制系统三个里程碑

从 2005 年开始作者专心于开源软件的研究、定制开发和整合，在这之后的几年，一直使用 Subversion 做版本控制。对于定制开发工作，Subversion 有一种称为卖主分支（Vendor Branch）的模式。

![../_images/topgit-branch-vendor-branch.png](img/topgit-branch-vendor-branch.png)

图 22-1：卖主分支工作模式图

卖主分支的工作模式如图 22-1 所示：

*   图 22-1 由左至右，提交随着时间而递增。
*   主线 trunk 用于对定制开发的过程进行跟踪。
*   主线的第一个提交`v1.0`是导入上游（该开源软件官方版本库）发布的版本。
*   之后在`v1.0`提交之处建立分支，是为卖主分支（vendor branch）。
*   主线上依次进行了 c1、c2 两次提交，是基于 v1.0 进行的定制开发。
*   当上游有了新版本，提交到卖主分支上，即`v2.0`提交。和`v1.0`相比除了大量的文件更改外，还可能有文件增加和删除。
*   然后在主线上执行从卖主分支到主线的合并，即提交`M1`。因为此时主线上的改动相对少，合并`v2.0`并不太费事。
*   主线继续开发。可能同时有针对不同需求的定制开发，在主线上会有越来越多的提交，如上图从 c3 到 c99 近百次提交。
*   如果在卖主分支上导入上游的新版本`v3.0`，合并将会非常痛苦。因为主线上针对不同需求的定制开发已经混在在一起！

实践证明，Subversion 的卖主分支对于大规模的定制开发非常不适合。向上游新版本的迁移随着定制功能和提交的增多越来越困难。

在 2008 年，我们的版本库迁移到 Mercurial（水银，又称为 Hg），并工作在“Hg+MQ”模式下，我自以为找到了定制开发版本控制的终极解决方案，那时我们已被 Subversion 的卖主分支折磨的太久了。

Hg 和 Git 一样也是一种分布式版本控制系统，MQ 是 Hg 的一个扩展，可以实现提交和补丁两种模式之间的转换。Hg 版本库上的提交可以通过**hg qimport**命令转化为补丁列表，也可以通过**hg qpush**、**hg qpop**等命令在补丁列表上游移（出栈和入栈），入栈的补丁转化为 Hg 版本库的提交，补丁出栈会从 Hg 版本库移走最新的提交。

使用“Hg+MQ”相比 Subversion 的卖主分支的好处在于：

*   针对不同需求的定制开发，其提交被限定在各自独立的补丁文件之中。

    针对同一个需求的定制开发，无论多少次的更改都体现为补丁文件的变化，而补丁文件本身也是被版本控制的。

*   各个补丁之间是顺序依赖关系，形成一个 Quilt 格式的补丁列表。

*   迁移至上游新版本的过程是：先将所有补丁“出栈”，再将上游新版本提交到主线，然后依次将补丁“入栈”。

    因为上游新版本的代码上下文改变等原因，补丁入栈可能会遇到冲突，只要在解决冲突完毕后，执行**hg qref**即可。

*   向上游新版本迁移过程的工作量降低了，是因为提交都按照定制的需求分类了 （不同的补丁），每个补丁都可以视为一个功能分支。

但是当需要在定制开发上进行多人协作的时候，“Hg+MQ”弊病就显现了。因为“Hg+MQ”工作模式下，定制开发的成果是一个补丁库，在补丁库上进行协作难度非常大，当发生冲突的时候，补丁文件本身的冲突解决难度相当大。这就引发了我们第三次版本控制系统大迁移。

2009 年，目光锁定在 Topgit 上。TopGit 的项目名称是来自于 Topic Git 的简写，是基于 Git 用脚本语言开发的辅助工具，是用于管理多个 Git 的特性分支的工具。Topgit 可以非常简单的实现“变基”——迁移至上游新版本。

Topgit 的主要特点有：

*   上游代码库位于开发主线（如：`master`分支），每一个定制开发都对应于一条 Git 分支（`refs/heads/t/feature_name`）。
*   特性分支之间的依赖关系不像“Hg+MQ”简单的逐一依赖模式，而是可以任意设定分支之间的依赖。
*   特性分支和其依赖的分支可以转出为 Quilt 格式的补丁列表。
*   因为针对某一需求的定制开发在特定的分支中，可以多人协同参与，和正常的 Git 开发别无二致。

## 4.3.2\. Topgit 原理

图 22-2 是一个近似的 Topgit 实现图（略去了重要的`top-bases`分支）。

![../_images/topgit-topic-branch.png](img/topgit-topic-branch.png)

图 22-2：Topgit 特性分支关系图

在图 22-2 中，主线上的`v1.0`是上游的版本的一次提交。特性分支 A 和 C 都直接依赖主线 master，而特性分支 B 则依赖特性分支 A。提交 M1 是特定分支 B 因为特性分支 A 更新而做的一次迁移。提交 M2 和 M4，则分别是特性分支 A 和 C 因为上游出现了新版本`v2.0`而做的迁移。当然特性分支 B 也要做相应的迁移，是为 M3。

上述的描述非常粗糙，因为这样的设计很难实现特性分支导出为补丁文件。例如特性分支 B 的补丁，实际上应该是 M3 和 M2 之间的差异，而绝不是 M3 到 a2 之间的差异。Topgit 为了能够实现分支导出为补丁，又为每个特性的开发引入了一个特殊的引用（`refs/top-bases/*`），用于追踪分支依赖的“变基”。这些特性分支的基准分支也形成了复杂的分支关系图，如图 22-3 所示。

![../_images/topgit-topic-base-branch.png](img/topgit-topic-base-branch.png)

图 22-3：Topgit 特性分支的基准分支关系图

把图 22-2 和图 22-3 两张分支图重合，就可以获得各个特性分支在任一点的特性补丁文件。

上面的特性分支 B 还只是依赖一个分支，如果出现一个分支依赖多个特性分支的话，情况就会更加的复杂，更会体现出这种设计方案的精妙。

Topgit 还在每个特性分支工作区的根目录引入两个文件，用以记录分支的依赖以及关于此分支的说明。

*   文件`.topdeps`记录该分支所依赖的分支列表。

    该文件通过**tg create**命令在创建特性分支时自动创建，或者通过**tg depend add**命令来添加新依赖。

*   文件`.topmsg`记录该分支的描述信息。

    该文件通过**tg create**命令在创建特性分支时创建，也可以手动编辑。

## 4.3.3\. Topgit 的安装

Topgit 的可执行命令只有一个**tg**。其官方参考手册见：[`repo.or.cz/w/topgit.git?a=blob;f=README`](http://repo.or.cz/w/topgit.git?a=blob;f=README)。

安装官方的 Topgit 版本，直接克隆官方的版本库，执行**make**即可。

```
$ git clone git://repo.or.cz/topgit.git
$ cd topgit
$ make
$ make install

```

缺省会把可执行文件**tg**安装在`$HOME/bin`（用户主目录下的`bin`目录）下，如果没有将`~/bin`加入环境变量`$PATH`中，可能无法执行**tg**。如果具有 root 权限，也可以将**tg**安装在系统目录中。

```
$ prefix=/usr make
$ sudo prefix=/usr make install

```

作者对 Topgit 做了一些增强和改进，在后面的章节予以介绍。如果想安装改进的版本，需要预先安装**quilt**补丁管理工具。然后进行如下操作。

```
$ git clone git://github.com/ossxp-com/topgit.git
$ cd topgit
$ QUILT_PATCHES=debian/patches quilt push -a
$ prefix=/usr make
$ sudo prefix=/usr make install

```

如果用的是 Ubuntu 或者 Debian Linux 操作系统，还可以这么安装。

*   先安装 Debian/Ubuntu 打包依赖的相关工具软件。

    ```
    $ sudo aptitude install quilt debhelper build-essential fakeroot dpkg-dev

    ```

*   再调用**dpkg-buildpackage**命令，编译出 DEB 包，再安装。

    ```
    $ git clone git://github.com/ossxp-com/topgit.git
    $ cd topgit
    $ dpkg-buildpackage -b -rfakeroot
    $ sudo dpkg -i ../topgit_*.deb

    ```

*   安装完毕后，重新加载命令行补齐，可以更方便的使用**tg**命令。

    ```
    $ . /etc/bash_completion

    ```

## 4.3.4\. Topgit 的使用

通过前面的原理部分，可以发现 Topgit 为管理特性分支，所引入的配置文件和基准分支都是和 Git 兼容的。

*   在`refs/top-bases/`命名空间下的引用，用于记录分支的变基历史。
*   在特性分支的工作区根目录引入两个文件`.topdeps`和`.topmsg`，用于记录分支依赖和说明。
*   引入新的钩子脚本`hooks/pre-commit`，用于在提交时检查分支依赖有没有发生循环等。

Topgit 的命令行的一般格式为：

```
tg [global_option] <subcmd> [command_options...] [arguments...]

```

*   在子命令前为全局选项，目前可用全局选项只有`-r <remote>`。

    `-r <remote>`可选项，用于设定分支跟踪的远程服务器。默认为`origin`。

*   子命令后可以跟命令相关的可选选项，和参数。

### 4.3.4.1\. **tg help**命令

**tg help**命令显示帮助信息。当在**tg help**后面提供子命令名称，可以获得该子命令详细的帮助信息。

### 4.3.4.2\. **tg create**命令

**tg create**命令用于创建新的特性分支。用法：

```
tg [...] create NAME [DEPS...|-r RNAME]

```

其中：

*   `NAME`是新的特性分支的分支名，必须提供。一般约定俗成，`NAME`以`t/`前缀开头，以标明此分支是一个 Topgit 特性分支。
*   `DEPS...`是可选的一个或多个依赖分支名。如果不提供依赖分支名，则使用当前分支作为新的特性分支的依赖分支。
*   `-r RNAME`选项，将远程分支作为依赖分支。不常用。

**tg create**命令会创建新的特性分支`refs/heads/NAME`，跟踪变基分支`refs/top-bases/NAME`，并且在项目根目录下创建文件`.topdeps`和`.topmsg`。会提示用户编辑`.topmsg`文件，输入详细的特性分支描述信息。

例如在一个示例版本库，分支 master 下输入命令：

```
$ tg create t/feature1
tg: Automatically marking dependency on master
tg: Creating t/feature1 base from master...
Switched to a new branch 't/feature1'
tg: Topic branch t/feature1 set up. Please fill .topmsg now and make initial commit.
tg: To abort: git rm -f .top* && git checkout master && tg delete t/feature1

```

提示信息中以“tg:”开头的是 Topgit 产生的说明。其中提示用户编辑`.topmsg`文件，然后执行一次提交完成 Topgit 特性分支的创建。

如果想撤销此次操作，删除项目根目录下的`.top*`文件，切换到 master 分支，然后执行**tg delete t/feature1**命令删除`t/feature1`分支以及变基跟踪分支`refs/top-bases/t/feature1`。

输入**git status**可以看到当前已经切换到`t/feature1`分支，并且 Topgit 已经创建了`.topdeps`和`.topmsg`文件，并已将这两个文件加入到暂存区。

```
$ git status
# On branch t/feature1
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       new file:   .topdeps
#       new file:   .topmsg
#
$ cat .topdeps
master

```

打开`.topmsg`文件，会看到下面内容（前面增加了行号）：

```
1   From: Jiang Xin <jiangxin@ossxp.com>
2   Subject: [PATCH] t/feature1
3
4   <patch description>
5
6   Signed-off-by: Jiang Xin <jiangxin@ossxp.com>

```

其中第 2 行是关于该特性分支的简短描述，第 4 行是详细描述，可以写多行。

编辑完成，别忘了提交，提交之后才完成 Topgit 分支的创建。

```
$ git add -u
$ git commit -m "create tg branch t/feature1"

```

**创建时指定依赖分支**

如果这时想创建一个新的特性分支`t/feature2`，并且也是要依赖`master`，注意需要在命令行中提供`master`作为第二个参数，以设定依赖分支。因为当前所处的分支为`t/feature1`，如果不提供指定的依赖分支会自动依赖当前分子。

```
$ tg create t/feature2 master
$ git commit -m "create tg branch t/feature2"

```

下面的命令将创建`t/feature3`分支，该分支依赖`t/feature1`和`t/feature2`。

```
$ tg create t/feature3 t/feature1 t/feature2
$ git commit -m "create tg branch t/feature3"

```

### 4.3.4.3\. **tg info**命令

**tg info**命令用于显示当前分支或指定的 Topgit 分支的信息。用法：

```
tg [...] info [NAME]

```

其中`NAME`是可选的 Topgit 分支名。例如执行下面的命令会显示分支`t/feature3`的信息：

```
$ tg info
Topic Branch: t/feature3 (1/1 commit)
Subject: [PATCH] t/feature3
Base: 0fa79a5
Depends: t/feature1
         t/feature2
Up-to-date.

```

切换到`t/feature1`分支，做一些修改，并提交。

```
$ git checkout t/feature1
hack...
$ git commit -m "hacks in t/feature1."

```

然后再来看`t/feature3`的状态：

```
$ tg info t/feature3
Topic Branch: t/feature3 (1/1 commit)
Subject: [PATCH] t/feature3
Base: 0fa79a5
Depends: t/feature1
         t/feature2
Needs update from:
        t/feature1 (1/1 commit)

```

状态信息显示`t/feature3`不再是最新的状态（Up-to-date），因为依赖的分支包含新的提交，而需要从`t/feature1`获取更新。

### 4.3.4.4\. **tg update**命令

**tg update**命令用于更新分支，即从依赖的分支或上游跟踪的分支获取最新的提交合并到当前分支。同时也更新在`refs/top-bases/`命名空间下的跟踪变基分支。

```
tg [...] update [NAME]

```

其中`NAME`是可选的 Topgit 分支名。下面就对需要更新的`t/feature3`分支执行**tg update**命令。

```
$ git checkout t/feature3
$ tg update
tg: Updating base with t/feature1 changes...
Merge made by recursive.
 feature1 |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
 create mode 100644 feature1
tg: Updating t/feature3 against new base...
Merge made by recursive.
 feature1 |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
 create mode 100644 feature1

```

从上面的输出信息可以看出执行了两次分支合并操作，一次是针对`refs/top-bases/t/feature3`引用指向的跟踪变基分支，另外一次针对的是`refs/heads/t/feature3`特性分支。

执行**tg update**命令因为要涉及到分支的合并，因此并非每次都会成功。例如在`t/feature3`和`t/feature1`同时对同一个文件（如`feature1`）进行修改。然后在`t/feature3`中再执行**tg update**可能就会报错，进入冲突解决状态。

```
$ tg update t/feature3
tg: Updating base with t/feature1 changes...
Merge made by recursive.
 feature1 |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
tg: Updating t/feature3 against new base...
Auto-merging feature1
CONFLICT (content): Merge conflict in feature1
Automatic merge failed; fix conflicts and then commit the result.
tg: Please commit merge resolution. No need to do anything else
tg: You can abort this operation using `git reset --hard` now
tg: and retry this merge later using `tg update`.

```

可以看出第一次对`refs/top-bases/t/feature3`引用指向的跟踪变基分支成功合并，但在对`t/feature3`特性分支进行合并是出错。

```
$ tg info
Topic Branch: t/feature3 (3/2 commits)
Subject: [PATCH] t/feature3
Base: 37dcb62
* Base is newer than head! Please run `tg update`.
Depends: t/feature1
         t/feature2
Up-to-date.

$ tg summary
        t/feature1                      [PATCH] t/feature1
 0      t/feature2                      [PATCH] t/feature2
>     B t/feature3                      [PATCH] t/feature3

$ git status
# On branch t/feature3
# Unmerged paths:
#   (use "git add/rm <file>..." as appropriate to mark resolution)
#
#       both modified:      feature1
#
no changes added to commit (use "git add" and/or "git commit -a")

```

通过**tg info**命令可以看出当前分支状态是 Up-to-date，但是之前有提示：分支的基（Base）要比头（Head）新，请执行**tg update**命令。这时如果执行**tg summary**命令的话，可以看到`t/feature3`处于 B（Break）状态。用**git status**命令，可以看出因为两个分支同时修改了文件`feature1`导致冲突。

可以编辑`feature1`文件，或者调用冲突解决工具解决冲突，之后再提交，才真正完成此次**tg update**。

```
$ git mergetool
$ git commit -m "resolved conflict with t/feature1."

$ tg info
Topic Branch: t/feature3 (4/2 commits)
Subject: [PATCH] t/feature3
Base: 37dcb62
Depends: t/feature1
         t/feature2
Up-to-date.

```

### 4.3.4.5\. **tg summary**命令

**tg summary**命令用于显示 Topgit 管理的特性分支的列表及各个分支的状态。用法：

```
tg [...] summary [-t | --sort | --deps | --graphviz]

```

不带任何参数执行**tg summary**是最常用的 Topgit 命令。在介绍无参数的**tg summary**命令之前，先看看其他简单的用法。

使用`-t`参数只显示特性分支列表。

```
$ tg summary -t
t/feature1
t/feature2
t/feature3

```

使用`--deps`参数会显示 Topgit 特性分支，及其依赖的分支。

```
$ tg summary  --deps
t/feature1 master
t/feature2 master
t/feature3 t/feature1
t/feature3 t/feature2

```

使用`--sort`参数按照分支依赖的顺序显示分支列表，除了 Topgit 分支外，依赖的非 Topgit 分支也会显示：

```
$ tg summary  --sort
t/feature3
t/feature2
t/feature1
master

```

使用`--graphviz`会输出 GraphViz 格式文件，可以用于显示特性分支之间的关系。

```
$ tg summary --graphviz | dot -T png -o topgit.png

```

生成的特性分支关系图如图 22-4 所示。

![../_images/topgit-graphviz.png](img/topgit-graphviz.png)

图 22-4：Topgit 特性分支依赖关系图

不带任何参数执行**tg summary**会显示分支列表及状态。这是最常用的 Topgit 命令之一。

```
$ tg summary
        t/feature1                      [PATCH] t/feature1
 0      t/feature2                      [PATCH] t/feature2
>       t/feature3                      [PATCH] t/feature3

```

其中:

*   标记“>”：（`t/feature3`分支之前的大于号）用于标记当前所处的特性分支。
*   标记“0”：（`t/feature2`分支前的数字 0）含义是该分支中没有提交，这一个建立后尚未使用或废弃的分支。
*   标记“D”：表明该分支处于过时（out-of-date）状态。可能是一个或多个依赖的分支包含了新的提交，尚未合并到此特性分支。可以用**tg info**命令看出到底是由于哪个依赖分支的改动导致该特性分支处于过时状态。
*   标记“B”：之前演示中出现过，表明该分支处于 Break 状态，即可能由于冲突未解决或者其他原因导致该特性分支的基（base）相对该分支的头（head）不匹配。`refs/top-bases`下的跟踪变基分支迁移了，但是特性分支未完成迁移。
*   标记“!”：表明该特性分支所依赖的分支不存在。
*   标记“l”：表明该特性分支只存在于本地，不存在于远程跟踪服务器。
*   标记“r”：表明该特性分支既存在于本地，又存在于远程跟踪服务器，并且两者匹配。
*   标记“L”：表明该特性分支，本地的要被远程跟踪服务器要新。
*   标记“R”：表明该特性分支，远程跟踪服务器的要被本地的新。
*   如果没有出现“l/r/L/R”：表明该版本库尚未设置远程跟踪版本库（没有 remote）。
*   一般带有标记“r”的是最常见的，也是最正常的。

下面通过**tg remote**为测试版本库建立一个对应的远程跟踪版本库，然后就能在**tg summary**的输出中看到标识符“l/r”等。

### 4.3.4.6\. **tg remote**命令

**tg remote**命令用于为远程跟踪版本库设置 Topgit 的特性分支的关联，在和该远程版本库进行`fetch`、`pull`等操作时能够同步 Topgit 相关分支。

```
tg [...] remote [--populate] [REMOTE]

```

其中`REMOTE`为远程跟踪版本库的名称，如“origin”，会自动在该远程源的配置中增加`refs/top-bases`下引用的同步。下面的示例中前面用加号标记的行就是当执行**tg remote origin**后增加的设置。

```
 [remote "origin"]
        url = /path/to/repos/tgtest.git
        fetch = +refs/heads/*:refs/remotes/origin/*
+       fetch = +refs/top-bases/*:refs/remotes/origin/top-bases/*

```

如果使用`--populate`参数，除了会向上面那样设置缺省的 Topgit 远程版本库外，会自动执行**git fetch**命令，然后还会为新的 Topgit 特性分支在本地创建新的分支，以及其对应的跟踪分支。

当执行**tg**命令时，如果不用`-r remote`全局参数，默认使用缺省的 Topgit 远程版本库。

下面为前面测试的版本库设置一个远程的跟踪版本库。

先创建一个裸版本库`tgtest.git`。

```
$ git init --bare /path/to/repos/tgtest.git
Initialized empty Git repository in /path/to/repos/tgtest.git/

```

然后在测试版本库中注册名为`origin`的远程版本库为刚刚创建的版本库。

```
$ git remote add origin /path/to/repos/tgtest.git

```

执行**git push**，将主线同步到远程的版本库。

```
$ git push origin master
Counting objects: 7, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (7/7), 585 bytes, done.
Total 7 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (7/7), done.
To /path/to/repos/tgtest.git
 * [new branch]      master -> master

```

之后通过**tg remote**命令告诉 Git 这个远程版本库需要跟踪 Topgit 分支。

```
$ tg remote --populate origin

```

会在当前的版本库的`.git/config`文件中添加设置（以加号开头的行）：

```
 [remote "origin"]
        url = /path/to/repos/tgtest.git
        fetch = +refs/heads/*:refs/remotes/origin/*
+       fetch = +refs/top-bases/*:refs/remotes/origin/top-bases/*
+[topgit]
+       remote = origin

```

这时再执行**tg summary**会看到分支前面都有标记“l”，即本地提交比远程版本库要新。

```
$ tg summary
  l     t/feature1                      [PATCH] t/feature1
 0l     t/feature2                      [PATCH] t/feature2
> l     t/feature3                      [PATCH] t/feature3

```

将`t/feature2`的特性分支推送到远程版本库。

```
$ tg push t/feature2
Counting objects: 5, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (3/3), done.
Writing objects: 100% (4/4), 457 bytes, done.
Total 4 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (4/4), done.
To /path/to/repos/tgtest.git
 * [new branch]      t/feature2 -> t/feature2
 * [new branch]      refs/top-bases/t/feature2 -> refs/top-bases/t/feature2

```

再来看看**tg summary**的输出，会看到`t/feature2`的标识变为“r”，即远程和本地相同步。

```
$ tg summary
  l     t/feature1                      [PATCH] t/feature1
 0r     t/feature2                      [PATCH] t/feature2
> l     t/feature3                      [PATCH] t/feature3

```

使用**tg push --all**(改进过的 Topgit)，会将所有的 topgit 分支推送到远程版本库。之后再来看**tg summary**的输出。

```
$ tg summary
  r     t/feature1                      [PATCH] t/feature1
 0r     t/feature2                      [PATCH] t/feature2
> r     t/feature3                      [PATCH] t/feature3

```

如果版本库设置了多个远程版本库，要针对每一个远程版本库执行**tg remote <REMOTE>**，但只能有一个远程的源用`--populate`参数调用**tg remote**将其设置为缺省的远程版本库。

### 4.3.4.7\. **tg push**命令

在前面**tg remote**的介绍中，已经看到了**tg push**命令。**tg push**命令用于将 Topgit 特性分支及对应的变基跟踪分支推送到远程版本库。用法：

```
tg [...] push [--dry-run] [--no-deps] [--tgish-only] [--all|branch*]

```

**tg push**命令后面的参数指定要推送给远程服务器的分支列表，如果省略则推送当前分支。改进的**tg push**可以不提供任何分支，只提供`--all`参数就可以将所有 Topgit 特性分支推送到远程版本库。

参数`--dry-run`是测试执行效果，不真正执行。参数`--no-deps`的含义是不推送依赖的分支，缺省推送。参数`--tgish-only`的含义是只推送 Topgit 特性分支，缺省指定的所有分支都进行推送。

### 4.3.4.8\. **tg depend**命令

**tg depend**命令目前仅实现了为当前的 Topgit 特性分支增加新的依赖。用法：

```
tg [...] depend add NAME

```

会将`NAME`加入到文件`.topdeps`中，并将`NAME`分支向该特性分支以及变基跟踪分支进行合并操作。虽然 Topgit 可以检查到分支的循环依赖，但还是要注意合理的设置分支的依赖，合并重复的依赖。

### 4.3.4.9\. **tg base**命令

**tg base**命令用于显示特性分支的基（base）当前的 commit-id。

### 4.3.4.10\. **tg delete**命令

**tg delete**命令用于删除 Topgit 特性分支以及其对应的变基跟踪分支。用法：

```
tg [...] delete [-f] NAME

```

缺省只删除没有改动的分支，即标记为“0”的分支，除非使用`-f`参数。

目前此命令尚不能自动清除其分支中对删除分支的依赖，还需要手工调整`.topdeps`文件，删除不存在分支的依赖。

### 4.3.4.11\. **tg patch**命令

**tg patch**命令通过比较特性分支及其变基跟踪分支的差异，显示该特性分支的补丁。用法：

```
tg [...] patch [-i | -w] [NAME]

```

其中参数`-i`显示暂存区和变基跟踪分支的差异。参数`-w`显示工作区和变基跟踪分支的差异。

**tg patch**命令存在的一个问题是只有在工作区的根执行才能够正确显示。这个缺陷已经在我改进的 Topgit 中被改正。

### 4.3.4.12\. **tg export**命令

**tg export**命令用于导出特性分支及其依赖，便于向上游贡献。可以导出 Quilt 格式的补丁列表，或者顺序提交到另外的分支中。用法：

```
tg [...] export ([--collapse] NEWBRANCH | [--all | -b BRANCH1,BRANCH2...] --quilt DIRECTORY | --linearize NEWBRANCH)

```

这个命令有三种导出方法。

*   将所有的 Topgit 特性分支压缩为一个提交到新的分支。

    ```
    tg [...] export --collapse NEWBRAQNCH

    ```

*   将所有的 Topgit 特性分支按照线性顺序提交到一个新的分支中。

    ```
    tg [...] export --linearize NEWBRANCH

    ```

*   将指定的 Topgit 分支（一个或多个）及其依赖分支转换为 Quilt 格式的补丁，保存到指定目录中。

    ```
    tg [...] export -b BRANCH1,BRANCH2... --quilt DIRECTORY

    ```

在导出为 Quilt 格式补丁的时候，如果想将所有的分支导出，必须用`-b`参数将分支全部罗列（或者分支的依赖关系将所有分支囊括），这对于需要导出所有分支非常不方便。我改进的 Topgit 通过`--all`参数，实现导出所有分支。

### 4.3.4.13\. **tg import**命令

**tg import**命令将分支的提交转换为 Topgit 特性分支，每个分支称为一个特性分支，各个特性分支线性依赖。用法：

```
tg [...] import [-d BASE_BRANCH] {[-p PREFIX] RANGE...|-s NAME COMMIT}

```

如果不使用`-d`参数，特性分支以当前分支为依赖。特性分支名称自动生成，使用约定俗成的`t/`作为前缀，也可以通过`-p`参数指定其他前缀。可以通过`-s`参数设定特性分支的名称。

### 4.3.4.14\. **tg log**命令

**tg log**命令显示特性分支的提交历史，并忽略合并引入的提交。

```
tg [...] log [NAME] [-- GIT LOG OPTIONS...]

```

**tg log**命令实际是对:command`git log`命令的封装。这个命令通过`--no-merges`和`--first-parent`参数调用**git log**，虽然屏蔽了大量因和依赖分支合并而引入的依赖分支的提交日志，但是同时也屏蔽了合并到该特性分支的其他贡献者的提交。

### 4.3.4.15\. **tg mail**命令

**tg mail**命令将当前分支或指定特性分支的补丁以邮件型式外发。用法：

```
tg [...] mail [-s SEND_EMAIL_ARGS] [-r REFERENCE_MSGID] [NAME]

```

**tg mail**调用**git send-email**发送邮件，参数`-s`用于向该命令传递参数（需要用双引号括起来）。邮件中的目的地址从 patch 文件头中的`To`、`Cc`和`Bcc`等字段获取。参数`-r`引用回复邮件的 id 以便正确生成`in-reply-to`邮件头。

注意：此命令可能会发送多封邮件，可以通过如下设置在调用**git send-email**命令发送邮件时进行确认。

```
git config sendemail.confirm always

```

### 4.3.4.16\. **tg graph**命令

**tg graph**命令并非官方提供的命令，而是源自一个补丁，实现文本方式的 Topgit 分支图。当然这个文本分支图没有**tg summary --graphviz**生成的那么漂亮。

## 4.3.5\. Topgit hacks

在 Topgit 的使用中陆续发现一些不合用的地方，于是便使用 Topgit 特性分支的方式来改进 Topgit 自身的代码。在群英汇博客上，介绍了这几个改进，参见：[`blog.ossxp.com/tag/topgit/`](http://blog.ossxp.com/tag/topgit/)。

下面就以此为例，介绍如何参与一个 Topgit 管理下的项目的开发。改进的 Topgit 版本库地址为：git://github.com/ossxp-com/topgit.git。

首先克隆该版本库。

```
$ git clone git://github.com/ossxp-com/topgit.git
$ cd topgit

```

查看远程分支。

```
$ git branch -r
origin/HEAD -> origin/master
origin/master
origin/t/debian_locations
origin/t/export_quilt_all
origin/t/fast_tg_summary
origin/t/graphviz_layout
origin/t/tg_completion_bugfix
origin/t/tg_graph_ascii_output
origin/t/tg_patch_cdup
origin/t/tg_push_all
origin/tgmaster

```

看到远程分支中出现了熟悉的以`t/`为前缀的 Topgit 分支，说明这个版本库是一个 Topgit 管理的定制开发版本库。那么为了能够获取 Topgit 的变基跟踪分支，需要用**tg remote**命令对缺省的`origin`远程版本库注册一下。

```
$ tg remote --populate origin
tg: Remote origin can now follow TopGit topic branches.
tg: Populating local topic branches from remote 'origin'...
From git://github.com/ossxp-com/topgit
 * [new branch]      refs/top-bases/t/debian_locations -> origin/top-bases/t/debian_locations
 * [new branch]      refs/top-bases/t/export_quilt_all -> origin/top-bases/t/export_quilt_all
 * [new branch]      refs/top-bases/t/fast_tg_summary -> origin/top-bases/t/fast_tg_summary
 * [new branch]      refs/top-bases/t/graphviz_layout -> origin/top-bases/t/graphviz_layout
 * [new branch]      refs/top-bases/t/tg_completion_bugfix -> origin/top-bases/t/tg_completion_bugfix
 * [new branch]      refs/top-bases/t/tg_graph_ascii_output -> origin/top-bases/t/tg_graph_ascii_output
 * [new branch]      refs/top-bases/t/tg_patch_cdup -> origin/top-bases/t/tg_patch_cdup
 * [new branch]      refs/top-bases/t/tg_push_all -> origin/top-bases/t/tg_push_all
tg: Adding branch t/debian_locations...
tg: Adding branch t/export_quilt_all...
tg: Adding branch t/fast_tg_summary...
tg: Adding branch t/graphviz_layout...
tg: Adding branch t/tg_completion_bugfix...
tg: Adding branch t/tg_graph_ascii_output...
tg: Adding branch t/tg_patch_cdup...
tg: Adding branch t/tg_push_all...
tg: The remote 'origin' is now the default source of topic branches.

```

执行**tg summary**看一下本地 Topgit 特性分支状态。

```
$ tg summary
  r  !  t/debian_locations              [PATCH] make file locations Debian-compatible
  r  !  t/export_quilt_all              [PATCH] t/export_quilt_all
  r  !  t/fast_tg_summary               [PATCH] t/fast_tg_summary
  r  !  t/graphviz_layout               [PATCH] t/graphviz_layout
  r  !  t/tg_completion_bugfix          [PATCH] t/tg_completion_bugfix
  r     t/tg_graph_ascii_output         [PATCH] t/tg_graph_ascii_output
  r  !  t/tg_patch_cdup                 [PATCH] t/tg_patch_cdup
  r  !  t/tg_push_all                   [PATCH] t/tg_push_all

```

怎么？出现了感叹号？记得前面在介绍**tg summary**命令的章节中提到过，感叹号的出现说明该特性分支依赖的分支丢失。用**tg info**查看一下某个特性分支。

```
$ tg info t/export_quilt_all
Topic Branch: t/export_quilt_all (6/4 commits)
Subject: [PATCH] t/export_quilt_all
Base: 8b0f1f9
Remote Mate: origin/t/export_quilt_all
Depends: tgmaster
MISSING: tgmaster
Up-to-date.

```

原来该特性分支依赖`tgmaster`分支，而不是 master 分支。远程存在`tgmaster`分支而本地尚不存在。于是在本地建立`tgmaster`跟踪分支。

```
$ git checkout tgmaster
Branch tgmaster set up to track remote branch tgmaster from origin.
Switched to a new branch 'tgmaster'

```

这回**tg summary**的输出正常了。

```
$ tg summary
  r     t/debian_locations              [PATCH] make file locations Debian-compatible
  r     t/export_quilt_all              [PATCH] t/export_quilt_all
  r     t/fast_tg_summary               [PATCH] t/fast_tg_summary
  r     t/graphviz_layout               [PATCH] t/graphviz_layout
  r     t/tg_completion_bugfix          [PATCH] t/tg_completion_bugfix
  r     t/tg_graph_ascii_output         [PATCH] t/tg_graph_ascii_output
  r     t/tg_patch_cdup                 [PATCH] t/tg_patch_cdup
  r     t/tg_push_all                   [PATCH] t/tg_push_all

```

通过下面命令创建图形化的分支图。

```
$ tg summary --graphviz | dot -T png -o topgit.png

```

生成的特性分支关系图如图 22-5 所示。

![../_images/topgit-hacks.png](img/topgit-hacks.png)

图 22-5：Topgit 改进项目的特性分支依赖关系图

其中：

*   特性分支`t/export_quilt_all`，为**tg export --quilt**命令增加`--all`选项，以便导出所有特性分支。
*   特性分支`t/fast_tg_summary`，主要是改进`tg`命令补齐时分支的显示速度，当特性分支接近上百个时差异非常明显。
*   特性分支`t/graphviz_layout`，改进了分支的图形输出格式。
*   特性分支`t/tg_completion_bugfix`，解决了命令补齐的一个 Bug。
*   特性分支`t/tg_graph_ascii_output`，源自 Bert Wesarg 的贡献，非常巧妙地实现了文本化的分支图显示，展示了 gvpr 命令的强大功能。
*   特性分支`t/tg_patch_cdup`，解决了在项目的子目录下无法执行**tg patch**的问题。
*   特性分支`t/tg_push_all`，通过为**tg push**增加`--all`选项，解决了当`tg`从 0.7 升级到 0.8 后，无法批量向上游推送特性分支的问题。

下面展示一下如何跟踪上游的最新改动，并迁移到新的上游版本。分支`tgmaster`用于跟踪上游的 Topgit 分支，以`t/`开头的分支是对 Topgit 改进的特性分支，而`master`分支实际上是导出 Topgit 补丁文件并负责编译特定 Linux 平台发行包的分支。

把官方的 Topgit 分支以`upstream`的名称加入为新的远程版本库。

```
$ git remote add upstream git://repo.or.cz/topgit.git

```

然后将`upstream`远程版本的`master`分支合并到本地的`tgmaster`分支。

```
$ git pull upstream master:tgmaster
From git://repo.or.cz/topgit
   29ab4cf..8b0f1f9  master     -> tgmaster

```

此时再执行**tg summary**会发现所有的 Topgit 分支都多了一个标记“D”，表明因为依赖分支的更新导致 Topgit 特性分支过时了。

```
$ tg summary
  r D   t/debian_locations              [PATCH] make file locations Debian-compatible
  r D   t/export_quilt_all              [PATCH] t/export_quilt_all
  r D   t/fast_tg_summary               [PATCH] t/fast_tg_summary
  r D   t/graphviz_layout               [PATCH] t/graphviz_layout
  r D   t/tg_completion_bugfix          [PATCH] t/tg_completion_bugfix
  r D   t/tg_graph_ascii_output         [PATCH] t/tg_graph_ascii_output
  r D   t/tg_patch_cdup                 [PATCH] t/tg_patch_cdup
  r D   t/tg_push_all                   [PATCH] t/tg_push_all

```

依次对各个分支执行**tg update**，完成对更新的依赖分支的合并。

```
$ tg update t/export_quilt_all
...

```

对各个分支完成更新后，会发现**tg summary**的输出中，标识过时的“D”标记变为“L”，即本地比远程服务器分支要新。

```
$ tg summary
  rL    t/debian_locations              [PATCH] make file locations Debian-compatible
  rL    t/export_quilt_all              [PATCH] t/export_quilt_all
  rL    t/fast_tg_summary               [PATCH] t/fast_tg_summary
  rL    t/graphviz_layout               [PATCH] t/graphviz_layout
  rL    t/tg_completion_bugfix          [PATCH] t/tg_completion_bugfix
  rL    t/tg_graph_ascii_output         [PATCH] t/tg_graph_ascii_output
  rL    t/tg_patch_cdup                 [PATCH] t/tg_patch_cdup
  rL    t/tg_push_all                   [PATCH] t/tg_push_all

```

执行**tg push --all**就可以实现将所有 Topgit 特性分支推送到远程服务器上。当然需要具有提交权限才可以。

## 4.3.6\. Topgit 使用中的注意事项

**经常运行:command:`tg remote –populate`获取他人创建的特性分支**

运行命令**git fetch origin**和远程版本库（origin）同步，只能将他人创建的 Topgit 特性分支在本地以`refs/remotes/origin/t/<branch-name>`的名称保存，而不能自动在本地建立分支。

当版本库是使用 Topgit 维护的话，应该在和远程版本库同步的时候使用执行**tg remote --populate origin**。这条命令会做两件事情：

*   自动调用**git fetch origin**获取远程`origin`版本库的新的提交和引用。
*   检查`refs/remotes/origin/top-bases/`下的所有引用，如果是新的、在本地（`refs/top-bases/`）尚不存在，说明有其他人创建了新的特性分支。Topgit 会据此自动的在本地创建新的特性分支。

**适时的调整特性分支的依赖关系**

例如前面示例的 Topgit 库的依赖关系，各个分支可能的依赖文件内容如下。

*   分支`t/feature1`的`.topdeps`文件

    ```
    master

    ```

*   分支`t/feature2`的`.topdeps`文件

    ```
    master

    ```

*   分支`t/feature3`的`.topdeps`文件

    ```
    t/feature1
    t/feature2

    ```

如果分支`t/feature3`的`.topdeps`文件是这样的，可能就会存在问题。

> ```
> master
> t/feature1
> t/feature2
> 
> ```

问题出在`t/feature3`依赖的其他分支已经依赖了`master`分支。虽然不会造成致命的影响，但是在特定情况下这种重复会造成不便。例如在`master`分支更新后，可能由于代码重构的比较厉害，在特性分支迁移时会造成冲突，如在`t/feature1`分支执行**tg update**会遇到冲突，当辛苦完成冲突解决并提交后，在`t/feature3`执行**tg update**时因为先依赖的是`master`分支，会先在`master`分支上对`t/feature3`分支进行变基，肯定会遇到和`t/feature1`相同的冲突，还要再重复地解决一次。

如果在`.topdeps`文件中将对`master`分支的重复的依赖删除，就不会出现上面的重复进行冲突解决的问题了。

同样的道理，如果`t/feature3`的`.topdeps`文件写成这样，效果也将不同：

> ```
> t/feature2
> t/feature1
> 
> ```

依赖的顺序不同会造成变基的顺序也不同，同样也会产生重复的冲突解决。因此当发现重复的冲突时，可以取消变基操作，修改特性分支的`.topdeps`文件，调整文件内容（删除重复分支，调整分支顺序）并提交，然后在执行**tg update**继续变基操作。

**Topgit 特性分支的里程碑和分支管理**

Topgit 本身就是对特性分支进行管理的软件。Topgit 的某个时刻的开发状态是所有 Topgit 管理下的分支（包括跟踪分支）整体的状态。如果需要对 Topgit 所有相关的分支进行跟踪管理该如何实现呢？

例如 master 主线由于提交了上游的新版本而改动，在对各个 Topgit 特性分支执行**tg update**时，搞的一团糟，而又不小心执行了**tg push --all**，这下无论本地和远程都处于混乱的状态。

使用里程碑（tags）来管理是不可能的，因为 tag 只能针对一个分支做标记而不能标记所有的分支。

使用克隆是唯一的方法。即针对不同的上游建立不同的 Git 库，通过不同的克隆实现针对不同上游版本特性分支开发的管理。例如一旦上游出现新版本，就从当前版本库建立一个克隆，或者用于保存当前上游版本的特性开发状态，或者用于新的上游版本的特性开发。

也许还可以通过其他方法实现，例如将 Topgit 所有相关分支都复制到一个特定的引用目录中，如`refs/top-tags/v1.0/`用于实现特性分支的里程碑记录。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 4.4\. 子模组协同模型

项目的版本库某些情况下需要引用其他版本库中的文件，例如公司积累了一套常用的函数库，被多个项目调用，显然这个函数库的代码不能直接放到某个项目的代码中，而是要独立为一个代码库，那么其他项目要调用公共的函数库，该如何处理呢？分别把公共函数库的文件拷贝到各自的项目中，会造成冗余，丢弃了公共函数库的维护历史，显然不是好的方法。本节要讨论的子模组协同模型，就是解决这个问题的一个方案。

熟悉 Subversion 的用户马上会想起`svn:externals`属性，可以实现对外部代码库的引用。Git 的子模组（submodule）是类似的一种实现。不过因为 Git 的特殊性，二者的区别还是满大的。参见表 23-1。

> 表 23-1：SVN 和 Git 相似功能对照表
> 
> |   | svn:externals | git submodule |
> | --- | --- | --- |
> | 如何记录外部版本库地址？ | 目录的`svn:externals`属性 | 项目根目录下的`.gitmodules`文件 |
> | 缺省是否自动检出外部版本库？ | 是。在使用**svn checkout**检出时若使用参数`--ignore-externals`可以忽略对外部版本库引用不检出。 | 否。缺省不克隆外部版本库。 克隆要用**git submodule init**、**git submodule update**命令。 |
> | 是否能**部分**引用外部版本库内容？ | 是。因为 SVN 支持部分检出。 | 否。必须克隆整个外部版本库。 |
> | 是否可以指向分支而随之改变？ | 是。 | 否。固定于外部版本库某个提交。 |

## 4.4.1\. 创建子模组

在演示子模组的创建和使用之前，先作些准备工作。先尝试建立两个公共函数库（`libA.git`和`libB.git`）以及一个引用函数库的主版本库（`super.git`）。

```
$ git --git-dir=/path/to/repos/libA.git init --bare
$ git --git-dir=/path/to/repos/libB.git init --bare
$ git --git-dir=/path/to/repos/super.git init --bare

```

向两个公共的函数库中填充些数据。这就需要在工作区克隆两个函数库，提交数据，并推送。

*   克隆`libA.git`版本库，添加一些数据，然后提交并推送。

    说明：示例中显示为`hack...`的地方，做一些改动（如创建新文件等），并将改动添加到暂存区。

    ```
    $ git clone /path/to/repos/libA.git /path/to/my/workspace/libA
    $ cd /path/to/my/workspace/libA
    hack ...
    $ git commit -m "add data for libA"
    $ git push origin master

    ```

*   克隆`libB.git`版本库，添加一些数据，然后提交并推送。

    ```
    $ git clone /path/to/repos/libB.git /path/to/my/workspace/libB
    $ cd /path/to/my/workspace/libB
    hack ...
    $ git commit -m "add data for libB"
    $ git push origin master

    ```

版本库`super`是准备在其中创建子模组的。`super`版本库刚刚初始化还未包含提交，`master`分支尚未有正确的引用。需要在`super`版本中至少创建一个提交。下面就克隆`super`版本库，在其中完成一个提交（空提交即可），并推送。

```
$ git clone /path/to/repos/super.git /path/to/my/workspace/super
$ cd /path/to/my/workspace/super
$ git commit --allow-empty -m "initialized."
$ git push origin master

```

现在就可以在`super`版本库中使用**git submodule add**命令添加子模组了。

```
$ git submodule add /path/to/repos/libA.git lib/lib_a
$ git submodule add /path/to/repos/libB.git lib/lib_b

```

至此看一下`super`版本库工作区的目录结构。在根目录下多了一个`.gitmodules`文件，并且两个函数库分别克隆到`lib/lib_a`目录和`lib/lib_b`目录下。

```
$ ls -aF
./  ../  .git/  .gitmodules  lib/

```

看看`.gitmodules`的内容：

```
$ cat .gitmodules
[submodule "lib/lib_a"]
        path = lib/lib_a
        url = /path/to/repos/libA.git
[submodule "lib/lib_b"]
        path = lib/lib_b
        url = /path/to/repos/libB.git

```

此时`super`的工作区尚未提交。

```
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       new file:   .gitmodules
#       new file:   lib/lib_a
#       new file:   lib/lib_b
#

```

完成提交之后，子模组才算正式在`super`版本库中创立。运行**git push**把建立了新模组的本地库推送到远程的版本库。

```
$ git commit -m "add modules in lib/lib_a and lib/lib_b."
$ git push

```

在提交过程中，发现作为子模组方式添加的版本库实际上并没有添加版本库的内容。实际上只是以 gitlink 方式[[1]](#id8)添加了一个链接。至于子模组的实际地址，是由文件`.gitmodules`中指定的。

可以通过查看补丁的方式，看到`lib/lib_a`和`lib/lib_b`子模组的存在方式（即 gitlink）。

```
$ git show HEAD

commit 19bb54239dd7c11151e0dcb8b9389e146f055ba9
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Fri Oct 29 10:16:59 2010 +0800

    add modules in lib/lib_a and lib/lib_b.

diff --git a/.gitmodules b/.gitmodules
new file mode 100644
index 0000000..60c7d1f
--- /dev/null
+++ b/.gitmodules
@@ -0,0 +1,6 @@
+[submodule "lib/lib_a"]
+       path = lib/lib_a
+       url = /path/to/repos/libA.git
+[submodule "lib/lib_b"]
+       path = lib/lib_b
+       url = /path/to/repos/libB.git
diff --git a/lib/lib_a b/lib/lib_a
new file mode 160000
index 0000000..126b181
--- /dev/null
+++ b/lib/lib_a
@@ -0,0 +1 @@
+Subproject commit 126b18153583d9bee4562f9af6b9706d2e104016
diff --git a/lib/lib_b b/lib/lib_b
new file mode 160000
index 0000000..3b52a71
--- /dev/null
+++ b/lib/lib_b
@@ -0,0 +1 @@
+Subproject commit 3b52a710068edc070e3a386a6efcbdf28bf1bed5

```

## 4.4.2\. 克隆带子模组的版本库

之前的表 23-1 在对比 Subversion 的`svn:externals`子模组实现差异时，提到过克隆带子模组的 Git 库，并不能自动将子模组的版本库克隆出来。对于只关心项目本身数据，对项目引用的外部项目数据并不关心的用户，这个功能非常好，数据也没有冗余而且克隆的速度也更块。

下面在另外的位置克隆`super`版本库，会发现`lib/lib_a`和`lib/lib_b`并未克隆。

```
$ git clone /path/to/repos/super.git /path/to/my/workspace/super-clone
$ cd /path/to/my/workspace/super-clone
$ ls -aF
./  ../  .git/  .gitmodules  lib/
$ find lib
lib
lib/lib_a
lib/lib_b

```

这时如果运行**git submodule status**可以查看到子模组状态。

```
$ git submodule status
-126b18153583d9bee4562f9af6b9706d2e104016 lib/lib_a
-3b52a710068edc070e3a386a6efcbdf28bf1bed5 lib/lib_b

```

看到每个子模组的目录前面是 40 位的提交 ID，在最前面是一个减号。减号的含义是该子模组尚为检出。

如果需要克隆出子模组型式引用的外部库，首先需要先执行**git submodule init**。

```
$ git submodule init
Submodule 'lib/lib_a' (/path/to/repos/libA.git) registered for path 'lib/lib_a'
Submodule 'lib/lib_b' (/path/to/repos/libB.git) registered for path 'lib/lib_b'

```

执行**git submodule init**实际上修改了`.git/config`文件，对子模组进行了注册。文件`.git/config`的修改示例如下（以加号开始的行代表新增的行）。

```
 [core]
         repositoryformatversion = 0
         filemode = true
         bare = false
         logallrefupdates = true
 [remote "origin"]
         fetch = +refs/heads/*:refs/remotes/origin/*
         url = /path/to/repos/super.git
 [branch "master"]
         remote = origin
         merge = refs/heads/master
+[submodule "lib/lib_a"]
+       url = /path/to/repos/libA.git
+[submodule "lib/lib_b"]
+       url = /path/to/repos/libB.git

```

然后执行**git submodule update**才完成子模组版本库的克隆。

```
$ git submodule update
Initialized empty Git repository in /path/to/my/workspace/super-clone/lib/lib_a/.git/
Submodule path 'lib/lib_a': checked out '126b18153583d9bee4562f9af6b9706d2e104016'
Initialized empty Git repository in /path/to/my/workspace/super-clone/lib/lib_b/.git/
Submodule path 'lib/lib_b': checked out '3b52a710068edc070e3a386a6efcbdf28bf1bed5'

```

## 4.4.3\. 在子模组中修改和子模组的更新

执行**git submodule update**更新出来的子模组，都以某个具体的提交版本进行检出。进入某个子模组目录，会发现其处于非跟踪状态（分离头指针状态）。

```
$ cd /path/to/my/workspace/super-clone/lib/lib_a
$ git branch
* (no branch)
  master

```

显然这种情况下，如果修改`lib/lib_a`下的文件，提交会丢失。下面介绍一下如何在检出的子模组中修改，以及更新子模组。

在子模组中切换到 master 分支（或其他想要修改的分支）后，再进行修改。

*   切换到 master 分支，然后在工作区做一些改动。

    ```
    $ cd /path/to/my/workspace/super-clone/lib/lib_a
    $ git checkout master
    hack ...

    ```

*   执行提交。

    ```
    $ git commit

    ```

*   查看状态，会看到相对于远程分支领先一个提交。

    ```
    $ git status
    # On branch master
    # Your branch is ahead of 'origin/master' by 1 commit.
    #
    nothing to commit (working directory clean)

    ```

在**git status**的状态输出中，可以看出新提交尚未推送到远程版本库。现在暂时不推送，看看在`super`版本库中执行**git submodule update**对子模组的影响。

*   先到`super-clone`版本库查看一下状态，可以看到子模组已修改，包含更新的提交。

    ```
    $ cd /path/to/my/workspace/super-clone/
    $ git status
    # On branch master
    # Changed but not updated:
    #   (use "git add <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #       modified:   lib/lib_a (new commits)
    #
    no changes added to commit (use "git add" and/or "git commit -a")

    ```

*   通过**git submodule stauts**命令可以看出`lib/lib_a`子模组指向了新的提交 ID（前面有一个加号），而`lib/lib_b`模组状态正常（提交 ID 前是一个空格，不是加号也不是减号）。

    ```
    $ git submodule status
    +5dea2693e5574a6e3b3a59c6b0c68cb08b2c07e9 lib/lib_a (heads/master)
     3b52a710068edc070e3a386a6efcbdf28bf1bed5 lib/lib_b (heads/master)

    ```

*   这时如果不小心执行了一次**git submodule update**命令，会将`lib/lib_a`重新切换到旧的指向。

    ```
    $ git submodule update
    Submodule path 'lib/lib_a': checked out '126b18153583d9bee4562f9af6b9706d2e104016'

    ```

*   执行**git submodule status**命令查看子模组状态，看到`lib/lib_a`子模组被重置了。

    ```
    $ git submodule status
     126b18153583d9bee4562f9af6b9706d2e104016 lib/lib_a (remotes/origin/HEAD)
     3b52a710068edc070e3a386a6efcbdf28bf1bed5 lib/lib_b (heads/master)

    ```

那么刚才在`lib/lib_a`中的提交丢失了么？实际上因为已经提交到了 master 主线，因此提交没有丢失，但是如果有数据没有提交，就会造成未提交数据的丢失。

*   进到`lib/lib_a`目录，看到工作区再一次进入分离头指针状态。

    ```
    $ cd lib/lib_a
    $ git branch
    * (no branch)
      master

    ```

*   重新检出 master 分支找回之前的提交。

    ```
    $ git checkout master
    Previous HEAD position was 126b181... add data for libA
    Switched to branch 'master'
    Your branch is ahead of 'origin/master' by 1 commit.

    ```

现在如果要将`lib/lib_a`目录下子模组的改动记录到父项目（`super`版本库）中，就需要在父项目中进行一次提交才能实现。

*   进入父项目根目录，查看状态。因为`lib/lib_a`的提交已经恢复，因此再次显示为有改动。

    ```
    $ cd /path/to/my/workspace/super-clone/
    $ git status -s
     M lib/lib_a

    ```

*   查看差异比较，会看到指向子模组的 gitlink 有改动。

    ```
    $ git diff
    diff --git a/lib/lib_a b/lib/lib_a
    index 126b181..5dea269 160000
    --- a/lib/lib_a
    +++ b/lib/lib_a
    @@ -1 +1 @@
    -Subproject commit 126b18153583d9bee4562f9af6b9706d2e104016
    +Subproject commit 5dea2693e5574a6e3b3a59c6b0c68cb08b2c07e9

    ```

*   将 gitlink 的改动添加到暂存区，然后提交。

    ```
    $ git add -u
    $ git commit -m "submodule lib/lib_a upgrade to new version."

    ```

此时先不要忙着推送，因为如果此时执行**git push**将`super`版本库推送到远程版本库，会引发一个问题。即推送后的远程`super`版本库的子模组`lib/lib_a`指向了一个新的提交，而该提交还在本地的`lib/lib_a`版本库（尚未向上游推送），这会导致其他人克隆`super`版本库、更新模组时因为找不到该子模组版本库相应的提交而导致出错。下面就是这类错误的错误信息：

```
fatal: reference is not a tree: 5dea2693e5574a6e3b3a59c6b0c68cb08b2c07e9
Unable to checkout '5dea2693e5574a6e3b3a59c6b0c68cb08b2c07e9' in submodule path 'lib/lib_a'

```

为了避免这种可能性的发生，最好先对`lib/lib_a`中的新提交进行推送，然后再对`super`的子模组更新的提交进行推送。即：

*   先推送子模组。

    ```
    $ cd /path/to/my/workspace/super-clone/lib/lib_a
    $ git push

    ```

*   再推送父版本库。

    ```
    $ cd /path/to/my/workspace/super-clone/
    $ git push

    ```

## 4.4.4\. 隐性子模组

我在开发备份工具 Gistore 时遇到一个棘手的问题就是隐性子模组的问题。Gistore 备份工具的原理是将要备份的目录都挂载（mount）在工作区中，然后执行**git add**。但是如果有某个目录已经被 Git 化了，就只会以子模组方式将该目录添加进来，而不会添加该目录下的文件。对于一个备份工具来说，意味着备份没有成功。

例如当前`super`版本库下有两个子模组：

```
$ cd /path/to/my/workspace/super-clone/
$ git submodule status
 126b18153583d9bee4562f9af6b9706d2e104016 lib/lib_a (remotes/origin/HEAD)
 3b52a710068edc070e3a386a6efcbdf28bf1bed5 lib/lib_b (heads/master)

```

然后创建一个新目录`others`，并把该目录用 git 初始化并做一次空的提交。

```
$ mkdir others
$ cd others
$ git init
$ git commit --allow-empty -m initial
[master (root-commit) 90364e1] initial

```

在`others`目录下创建一个文件`newfile`。

```
$ date > newfile

```

回到上一级目录，执行**git status**，看到有一个`others`目录没有加入版本库控制，这很自然。

```
$ cd ..
$ git status
# On branch master
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#       others/
nothing added to commit but untracked files present (use "git add" to track)

```

但是如果对`others`目录执行**git add**后，会发现奇怪的状态。

```
$ git add others
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       new file:   others
#
# Changed but not updated:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#   (commit or discard the untracked or modified content in submodules)
#
#       modified:   others (untracked content)
#

```

看看`others`目录的添加方式，就会发现`others`目录以 gitlink 方式添加到版本库中，而没有把该目录下的文件添加到版本库。

```
$ git diff --cached
diff --git a/others b/others
new file mode 160000
index 0000000..90364e1
--- /dev/null
+++ b/others
@@ -0,0 +1 @@
+Subproject commit 90364e1331abc29cc63e994b4d2cfbf7c485e4ad

```

之所以**git status**的显示中`others`出现两次，就是因为目录`others`被当做子模组添加到父版本库中。因为`others`版本库本身“不干净”，存在尚未加入版本控制的文件，所以又在状态输出中显示子模组包含改动的提示信息。

执行提交，将`others`目录提交到版本库中。

```
$ git commit -m "add others as submodule."

```

执行**git submoudle status**命令，会报错。因为`others`作为子模组，没有在`.gitmodules`文件中注册。

```
$ git submodule status
 126b18153583d9bee4562f9af6b9706d2e104016 lib/lib_a (remotes/origin/HEAD)
 3b52a710068edc070e3a386a6efcbdf28bf1bed5 lib/lib_b (heads/master)
No submodule mapping found in .gitmodules for path 'others'

```

那么如何在不破坏`others`版本库的前提下，把`others`目录下的文件加入版本库呢？即避免`others`以子模组形式添加入库。

*   先删除以 gitlink 形式入库的`others`子模组。

    ```
    $ git rm --cached others
    rm 'others'

    ```

*   查看当前状态。

    ```
    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       deleted:    others
    #
    # Untracked files:
    #   (use "git add <file>..." to include in what will be committed)
    #
    #       others/

    ```

*   重新添加`others`目录，注意目录后面的斜线（即路径分隔符）非常重要。

    ```
    $ git add others/

    ```

*   再次查看状态，看到`others`下的文件被添加到`super`版本库中。

    ```
    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       deleted:    others
    #       new file:   others/newfile
    #

    ```

*   执行提交。

    ```
    $ git commit -m "add contents in others/."
    [master 1e0c418] add contents in others/.
     2 files changed, 1 insertions(+), 1 deletions(-)
     delete mode 160000 others
     create mode 100644 others/newfile

    ```

在上面的操作过程中，首先先删除了在库中的`others`子模组（使用`--cached`参数执行删除）；然后为了添加`others`目录下的文件，使用了`others/`（注意`others`后面的路径分割符“/”）。现在查看一下子模组的状态，会看到只有之前的两个子模组显示出来。

```
$ git submodule status
 126b18153583d9bee4562f9af6b9706d2e104016 lib/lib_a (remotes/origin/HEAD)
 3b52a710068edc070e3a386a6efcbdf28bf1bed5 lib/lib_b (heads/master)

```

## 4.4.5\. 子模组的管理问题

子模组最主要的一个问题是不能基于外部版本库的某一个分支进行创建，使得更新后，子模组处于非跟踪状态，不便于在子模组中进行对外部版本库进行改动。尤其对于授权或其他原因将一个版本库拆分为子模组后，使用和管理非常不方便。在第二十五章“Android 式多版本库协同”可以看到管理多版本库的另外一个可行方案。

如果在局域网内维护的版本库所引用的子模组版本库在另外的服务器，甚至在互联网上，克隆子版本库就要浪费很多时间。而且如果子模组指向的版本库不在我们的掌控之内，一旦需要对其进行定制会因为提交无法向远程服务器推送而无法实现。在下一章即第二十四章“子树合并”中，会给出针对这个问题的解决方案。

* * *

| [[1]](#id3) | gitlink: |

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 4.5\. 子树合并

使用子树合并，同样可以实现在一个项目中引用其他项目的数据。但是和子模组方式不同的是，使用子树合并模式，外部的版本库整个复制到本版本库中并建立跟踪关联。使用子树合并模型，使得对源自外部版本库的数据的访问和本版本库数据的访问没有区别，也可以对其进行本地修改，并且能够以子树合并的方式将源自外部版本库的改动和本地的修改相合并。

## 4.5.1\. 引入外部版本库

为演示子树合并，需要至少准备两个版本库，一个是将被作为子目录引入的版本库`util.git`，另外一个是主版本库`main.git`。

```
$ git --git-dir=/path/to/repos/util.git init --bare
$ git --git-dir=/path/to/repos/main.git init --bare

```

在本地检出这两个版本库：

```
$ git clone /path/to/repos/util.git
$ git clone /path/to/repos/main.git

```

需要为这两个空版本库添加些数据。非常简单，每个版本库下只创建两个文件：`Makefile`和`version`。当执行**make**命令时显示 version 文件的内容。对`version`文件多次提交以建立多个提交历史。别忘记在最后使用**git push origin master**将版本库推送到远程版本库中。

`Makefile`文件示例如下。注意第二行前面的空白是`<TAB>`字符，而非空格。

```
all:
        @cat version

```

在之前尝试的**git fetch**命令都是获取同一项目的版本库的内容。其实命令**git fetch**从哪个项目获取数据并没有什么限制，因为 Git 的版本库不像 Subversion 那样用一个唯一的 UUID 标识让 Subversion 的版本库之间势同水火。当然也可以用**git pull**来获取其他版本库中的提交，但是那样将把两个项目的文件彻底混杂在一起。对于这个示例来说，因为两个项目具有同样的文件`Makefile`和`version`，使用**git pull**将导致冲突。所以为了将不同项目的版本库引入，并在稍候以子树合并方式添加到一个子目录中，需要用**git fetch**命令从其他版本库获取数据。

*   为了便于以后对外部版本库的跟踪，在使用**git fetch**前，先在`main`版本库中注册远程版本库`util.git`。

    ```
    $ git remote add util /path/to/repos/util.git

    ```

*   查看注册的远程版本库。

    ```
    $ git remote -v
    origin  /path/to/repos/main.git/ (fetch)
    origin  /path/to/repos/main.git/ (push)
    util    /path/to/repos/util.git (fetch)
    util    /path/to/repos/util.git (push)

    ```

*   执行**git fetch**命令获取`util.git`版本库的提交。

    ```
    $ git fetch util

    ```

*   查看分支，包括远程分支。

    ```
    $ git branch -a
    * master
      remotes/origin/master
      remotes/util/master

    ```

在不同的分支：`master`分支和`remotes/util/master`分支，文件`version`的内容并不相同，因为来自不同的上游版本库。

*   master 分支中执行**make**命令，显示的是`main.git`版本库中`version`文件的内容。

    ```
    $ make
    main v2010.1

    ```

*   从`util/master`远程分支创建一个本地分支`util-branch`，并切换分支。

    ```
    $ git checkout -b util-branch util/master
    Branch util-branch set up to track remote branch master from util.
    Switched to a new branch 'util-branch'

    ```

*   执行**make**命令，显示的是`util.git`版本库中`version`文件的内容。

    ```
    $ make
    util v3.0

    ```

像这样在`main.git`中引入`util.git`显然不能满足需要，因为在`main.git`的本地克隆版本库中，`master`分支访问不到只有在`util-branch`分支中才出现的`util`版本库数据。这就需要做进一步的工作，将两个版本库的内容合并到一个分支中。即`util-branch`分支的数据作为子目录加入到`master`分支。

## 4.5.2\. 子目录方式合并外部版本库

下面就用 git 的底层命令**git read-tree**、**git write-tree**、**git commit-tree**子命令实现将`util-branch`分支所包含的`util.git`版本库的目录树以子目录（`lib/`）型式添加到`master`分支。

先来看看`util-branch`分支当前最新提交，记住最新提交所指向的目录树（tree），即 tree-id：0c743e4。

```
$ git cat-file -p util-branch
tree 0c743e49e11019678c8b345e667504cb789431ae
parent f21f9c10cc248a4a28bf7790414baba483f1ec15
author Jiang Xin <jiangxin@ossxp.com> 1288494998 +0800
committer Jiang Xin <jiangxin@ossxp.com> 1288494998 +0800

util v2.0 -> v3.0

```

查看 tree 0c743e4 所包含的内容，会看到两个文件：`Makefile`和`version`。

```
$ git cat-file -p 0c743e4
100644 blob 07263ff95b4c94275f4b4735e26ea63b57b3c9e3    Makefile
100644 blob bebe6b10eb9622597dd2b641efe8365c3638004e    version

```

切换到 master 分支，如下方式调用**git read-tree**将`util-branch`分支的目录树读取到当前分支`lib`目录下。

*   切换到 master 分支。

    ```
    $ git checkout master

    ```

*   执行**git read-tree**命令，将分支`util-branch`读取到当前分支的一个子目录下。

    ```
    $ git read-tree --prefix=lib util-branch

    ```

*   调用**git read-tree**只是更新了 index，所以查看工作区状态，会看到`lib`目录下的两个文件在工作区中还不存在。

    ```
    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       new file:   lib/Makefile
    #       new file:   lib/version
    #
    # Changed but not updated:
    #   (use "git add/rm <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #       deleted:    lib/Makefile
    #       deleted:    lib/version
    #

    ```

*   执行检出命令，将`lib`目录下的文件更新出来。

    ```
    $ git checkout -- lib

    ```

*   再次查看状态，会看到前面执行的**git read-tree**命令添加到暂存区中的文件。

    ```
    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #       new file:   lib/Makefile
    #       new file:   lib/version
    #

    ```

现在还不能忙着提交，因为如果现在进行提交就体现不出来两个分支的合并关系。需要使用 Git 底层的命令进行数据提交。

*   调用**git write-tree**将暂存区的目录树保存下来。

    要记住调用**git write-tree**后形成的新的 tree-id：2153518。

    ```
    $ git write-tree
    2153518409d218609af40babededec6e8ef51616

    ```

*   执行**git cat-file**命令显示这棵树的内容，会注意到其中`lib`目录的 tree-id 和之前查看过的`util-branch`分支最新提交对应的 tree-id 一样都是 0c743e4。

    ```
    $ git cat-file -p 2153518409d218609af40babededec6e8ef51616
    100644 blob 07263ff95b4c94275f4b4735e26ea63b57b3c9e3    Makefile
    040000 tree 0c743e49e11019678c8b345e667504cb789431ae    lib
    100644 blob 638c7b7c6bdbde1d29e0b55b165f755c8c4332b5    version

    ```

*   要手工创建一个合并提交，即新的提交要有两个父提交。这两个父提交分别是`master`分支和`util-branch`分支的最新提交。用下面的命令显示两个提交的提交 ID，并记下这两个提交 ID。

    ```
    $ git rev-parse HEAD
    911b1af2e0c95a2fc1306b8dea707064d5386c2e
    $ git rev-parse util-branch
    12408a149bfa78a4c2d4011f884aa2adb04f0934

    ```

*   执行**git commit-tree**命令手动创建提交。新提交的目录树来自上面**git write-tree**产生的目录树（tree-id 为 2153518），而新提交（合并提交）的两个父提交直接用上面**git rev-parse**显示的两个提交 ID 表示。

    ```
    $ echo "subtree merge" | \
      git commit-tree 2153518409d218609af40babededec6e8ef51616 \
      -p 911b1af2e0c95a2fc1306b8dea707064d5386c2e \
      -p 12408a149bfa78a4c2d4011f884aa2adb04f0934
    62ae6cc3f9280418bdb0fcf6c1e678905b1fe690

    ```

*   执行**git commit-tree**命令的输出是提交之后产生的新提交的提交 ID。需要把当前的 master 分支重置到此提交 ID。

    ```
    $ git reset 62ae6cc3f9280418bdb0fcf6c1e678905b1fe690

    ```

*   查看一下提交日志及分支图，可以看到通过复杂的**git read-tree**、**git write-tree**和**git commit-tree**命令制造的合并提交，的确将两个不同版本库合并到一起了。

    ```
    $ git log --graph --pretty=oneline
    *   62ae6cc3f9280418bdb0fcf6c1e678905b1fe690 subtree merge
    |\
    | * 12408a149bfa78a4c2d4011f884aa2adb04f0934 util v2.0 -> v3.0
    | * f21f9c10cc248a4a28bf7790414baba483f1ec15 util v1.0 -> v2.0
    | * 76db0ad729db9fdc5be043f3b4ed94ddc945cd7f util v1.0
    * 911b1af2e0c95a2fc1306b8dea707064d5386c2e main v2010.1

    ```

*   看看现在的 master 分支。

    ```
    $ git cat-file -p HEAD
    tree 2153518409d218609af40babededec6e8ef51616
    parent 911b1af2e0c95a2fc1306b8dea707064d5386c2e
    parent 12408a149bfa78a4c2d4011f884aa2adb04f0934
    author Jiang Xin <jiangxin@ossxp.com> 1288498186 +0800
    committer Jiang Xin <jiangxin@ossxp.com> 1288498186 +0800

    subtree merge

    ```

*   看看目录树。

    ```
    $ git cat-file -p 2153518409d218609af40babededec6e8ef51616
    100644 blob 07263ff95b4c94275f4b4735e26ea63b57b3c9e3    Makefile
    040000 tree 0c743e49e11019678c8b345e667504cb789431ae    lib
    100644 blob 638c7b7c6bdbde1d29e0b55b165f755c8c4332b5    version

    ```

整个过程非常繁琐，但是不要太过担心，只需要对原理了解清楚就可以了，因为在后面会介绍一个 Git 插件封装了复杂的子树合并操作。

## 4.5.3\. 利用子树合并跟踪上游改动

如果子树（`lib`目录）的上游（即`util.git`）包含了新的提交，如何将`util.git`的新提交合并过来呢？这就要用到名为 subtree 的合并策略。参见第 3 篇第十六章第 16.6 小节“合并策略”中相关内容。

在执行子树合并之前，先切换到`util-branch`分支，获取远程版本库改动。

```
$ git checkout util-branch

$ git pull
remote: Counting objects: 8, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 6 (delta 0), reused 0 (delta 0)
Unpacking objects: 100% (6/6), done.
From /path/to/repos/util
   12408a1..5aba14f  master     -> util/master
Updating 12408a1..5aba14f
Fast-forward
 version |    2 +-
 1 files changed, 1 insertions(+), 1 deletions(-)

$ git checkout master

```

在切换回 master 分支后，如果这时执行**git merge util-branch**，会将`uitl-branch`的数据直接合并到`master`分支的根目录下，而实际上是希望合并发生在`lib`目录中，这就需要如下方式进行调用，以 subtree 策略进行合并。

如果 Git 的版本小于 1.7，直接使用 subtree 合并策略。

```
$ git merge -s subtree util-branch

```

如果 Git 的版本是 1.7 之后（含 1.7）的版本，则可以使用缺省的 recursive 合并策略，通过参数`subtree=<prefix>`在合并时使用正确的子树进行匹配合并。避免了使用 subtree 合并策略时的猜测。

```
$ git merge -Xsubtree=lib util-branch

```

再来看看执行子树合并之后的分支图示。

```
$ git log --graph --pretty=oneline
*   f1a33e55eea04930a500c18a24a8bd009ecd9ac2 Merge branch 'util-branch'
|\
| * 5aba14fd347fc22cd8fbd086c9f26a53276f15c9 util v3.1 -> v3.2
| * a6d53dfcf78e8a874e9132def5ef87a2b2febfa5 util v3.0 -> v3.1
* |   62ae6cc3f9280418bdb0fcf6c1e678905b1fe690 subtree merge
|\ \
| |/
| * 12408a149bfa78a4c2d4011f884aa2adb04f0934 util v2.0 -> v3.0
| * f21f9c10cc248a4a28bf7790414baba483f1ec15 util v1.0 -> v2.0
| * 76db0ad729db9fdc5be043f3b4ed94ddc945cd7f util v1.0
* 911b1af2e0c95a2fc1306b8dea707064d5386c2e main v2010.1

```

## 4.5.4\. 子树拆分

既然可以将一个代码库通过子树合并方式作为子目录加入到另外一个版本库中，反之也可以将一个代码库的子目录独立出来转换为另外的版本库。不过这个反向过程非常复杂。要将一个版本库的子目录作为顶级目录导出到另外的项目，潜藏的条件是要导出历史的，因为如果不关心历史，直接文件拷贝重建项目就可以了。子树拆分的大致过程是：

1.  找到要导出的目录的提交历史，并反向排序。
2.  依次对每个提交执行下面的操作：
3.  找出提交中导出目录对应的 tree-id。
4.  对该 tree-id 执行**git commit-tree**。
5.  执行**git commit-tree**要保持提交信息还要重新设置提交的父提交（parent）。

手工执行这个操作复杂且易出错，可以用下节介绍的`git subtree`插件，或使用第 6 篇第 35.4 小节“Git 版本库整理”中介绍的**git filter-branch**命令进行子目录过滤。

## 4.5.5\. `git subtree`插件

Git subtree 插件用 shell 脚本开发，安装之后为 Git 提供了新的**git subtree**命令，支持前面介绍的子树合并和子树拆分。命令非常简单易用，将其他版本库以子树形式导入，再也不必和底层的 Git 命令打交道了。

Gitsubtree 插件的作者将代码库公布在 Github 上：[`github.com/apenwarr/git-subtree/`](http://github.com/apenwarr/git-subtree/)。

安装 Git subtree 很简单：

```
$ git clone git://github.com/apenwarr/git-subtree.git
$ cd git-subtree
$ make doc
$ make test
$ sudo make install

```

### 4.5.5.1\. **git subtree add**

命令**git subtree add**相当于将其他版本库以子树方式加入到当前版本库。用法：

```
git subtree add [--squash] -P <prefix> <commit>
git subtree add [--squash] -P <prefix> <repository> <refspec>

```

其中可选的`--squash`含义为压缩为一个版本后再添加。

对于文章中的示例，为了将`util.git`合并到`main.git`的`lib`目录。可以直接这样调用：

```
$ git subtree add -P lib /path/to/repos/util.git master

```

不过推荐的方法还是先在本地建立`util.git`版本库的追踪分支。

```
$ git remote add util /path/to/repos/util.git
$ git fetch util
$ git checkout -b util-branch util/master
$ git subtree add -P lib util-branch

```

### 4.5.5.2\. **git subtree merge**

命令**git subtree merge**相当于将子树对应的远程分支的更新重新合并到子树中，相当于完成了**git merge -s subtree**操作。用法：

```
git subtree merge [--squash] -P <prefix> <commit>

```

其中可选的`--squash`含义为压缩为一个版本后再合并。

对于文章中的示例，为了将`util-branch`分支包含的上游最新改动合并到 master 分支的`lib`目录。可以直接这样调用：

```
$ git subtree merge -P lib util-branch

```

### 4.5.5.3\. **git subtree pull**

命令**git subtree pull**相当于先对子树对应的远程版本库执行一次**git fetch**操作，然后再执行**git subtree merge**。用法：

```
git subtree pull [--squash] -P <prefix> <repository> <refspec...>

```

对于文章中的示例，为了将`util.git`版本库的 master 分支包含的最新改动合并到 master 分支的`lib`目录。可以直接这样调用：

```
$ git subtree pull -P lib /path/to/repos/util.git master

```

更喜欢用前面介绍的**git subtree merge**命令，因为**git subtree pull**存在版本库地址写错的风险。

### 4.5.5.4\. **git subtree split**

命令**git subtree split**相当将目录拆分为独立的分支，即子树拆分。拆分后形成的分支可以通过推送到新的版本库实现原版本库的目录独立为一个新的版本库。用法：

```
git subtree split -P <prefix> [--branch <branch>] [--onto ...] [--ignore-joins] [--rejoin] <commit...>

```

说明：

*   该命令的总是输出子树拆分后的最后一个 commit-id。这样可以通过管道方式传递给其他命令，如**git subtree push**命令。
*   参数`--branch`提供拆分后创建的分支名称。如果不提供，只能通过**git subtree split**命令提供的提交 ID 得到拆分的结果。
*   参数`--onto`参数将目录拆分附加于已经存在的提交上。
*   参数`--ignore-joins`忽略对之前拆分历史的检查。
*   参数`--rejoin`会将拆分结果合并到当前分支，因为采用 ours 的合并策略，不会破坏当前分支。

### 4.5.5.5\. **git subtree push**

命令**git subtree push**先执行子树拆分，再将拆分的分支推送到远程服务器。用法：

```
git subtree push -P <prefix> <repository> <refspec...>

```

该命令的用法和**git subtree split**类似，不再赘述。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 4.6\. Android 式多版本库协同

Android 是谷歌（Google）开发的适合手持设备的操作系统，提供了当前最为吸引眼球的开源的手机操作平台，大有超越苹果（Apple.com）的专有的 iOS 的趋势。而 Android 的源代码就是使用 Git 进行维护的。Android 项目在使用 Git 进行源代码管理上有两个伟大的创造，一个是用 Python 语言开发名为 repo 的命令行工具用于多版本库的管理，另外一个是用 Java 开发的名为 Gerrit 的代码审核服务器。本节重点介绍 repo 是如何管理多代码库的。

Android 的源代码的 Git 库有 160 多个（截止至 2010 年 10 月）：

*   Android 的版本库管理工具 repo：

    git://android.git.kernel.org/tools/repo.git

*   保存 GPS 配置文件的版本库

    git://android.git.kernel.org/device/common.git

*   160 多个其他的版本库...

如果要是把 160 多个版本库都列在这里，恐怕各位的下巴会掉下来。那么为什么 Android 的版本库这么多呢？怎么管理这么复杂的版本库呢？

Android 版本库众多的原因，主要原因是版本库太大以及 Git 不能部分检出。Android 的版本库有接近 2 个 GB 之多。如果所有的东西都放在一个库中，而某个开发团队感兴趣的可能就是某个驱动，或者是某个应用，却要下载如此庞大的版本库，是有些说不过去。

好了，既然接受了 Android 有多达 160 多个版本库这一事实，那么 Android 是不是用之前介绍的“子模组”方式组织起来的呢？如果真的用“子模组”方式来管理这 160 个代码库，可能就需要如此管理：

*   建立一个索引版本库，在该版本库中，通过子模组方式，将一个一个的目录对应到 160 多个版本库。
*   当对此索引版本库执行克隆操作后，再执行**git submodule init**命令。
*   当执行**git submodule update**命令时，开始分别克隆这 160 多个版本库。
*   如果想修改某个版本库中的内容，需要进入到相应的子模组目录，执行切换分支的操作。因为子模组是以某个固定提交的状态存在的，是不能更改的，必须先切换到某个工作分支后，才能进行修改和提交。
*   如果要将所有的子模组都切换到某个分支（如 master）进行修改，必须自己通过脚本对这 160 多个版本库一一切换。
*   Android 有多个版本：android-1.0、android-1.5、...、android-2.2_r1.3、...如何维护这么多的版本呢？也许索引库要通过分支和里程碑，和子模组的各个不同的提交状态进行对应。但是由于子模组的状态只是一个提交 ID，如何能够动态指定到分支，真的给不出答案。

幸好，上面只是假设。聪明的 Android 程序设计师一早就考虑到了 Git 子模组的局限性以及多版本库管理的问题，开发出了 repo 这一工具。

关于 repo 有这么一则小故事：Android 之父安迪·鲁宾在回应乔布斯关于 Android 太开放导致开发维护更麻烦的言论时，在 Twitter（[`twitter.com/Arubin`](http://twitter.com/Arubin)）上留了下面这段简短的话：

```
the definition of open: "mkdir android ; cd android ; repo init -u git://android.git.kernel.org/platform/manifest.git ; repo sync ; make"

```

是的，就是 repo 让 Android 的开发变得如此简单。

## 4.6.1\. 关于 repo

Repo 是 Google 开发的用于管理 Android 版本库的一个工具。Repo 并不是用于取代 Git，是用 Python 对 Git 进行了一定的封装，简化了对多个 Git 版本库的管理。对于 repo 管理的任何一个版本库，都还是需要使用 Git 命令进行操作。

repo 的使用过程大致如下：

*   运行**repo init**命令，克隆 Android 的一个清单库。这个清单库和前面假设的“子模组”方式工作的索引库不同，是通过 XML 技术建立的版本库清单。
*   清单库中的`manifest.xml`文件，列出了 160 多个版本库的克隆方式。包括版本库的地址和工作区地址的对应关系，以及分支的对应关系。
*   运行**repo sync**命令，开始同步，即分别克隆这 160 多个版本库到本地的工作区中。
*   同时对 160 多个版本库执行切换分支操作，切换到某个分支。

## 4.6.2\. 安装 repo

首先下载 repo 的引导脚本，可以使用 wget、curl 甚至浏览器从地址[`android.git.kernel.org/repo`](http://android.git.kernel.org/repo)下载。把 repo 脚本设置为可执行，并复制到可执行的路径中。在 Linux 上可以用下面的指令将 repo 下载并复制到用户主目录的`bin`目录下。

```
$ curl http://android.git.kernel.org/repo > ~/bin/repo
$ chmod a+x ~/bin/repo

```

为什么说下载的 repo 只是一个引导脚本（bootstrap）而不是直接称为 repo 呢？因为 repo 的大部分功能代码不在其中，下载的只是一个帮助完成整个 repo 程序的继续下载和加载工具。如果您是一个程序员，对 repo 的执行比较好奇，可以一起分析一下 repo 引导脚本。否则可以跳到下一节。

看看 repo 引导脚本的前几行（为方便描述，把注释和版权信息过滤掉了），会发现一个神奇的魔法：

```
1   #!/bin/sh
2
3   REPO_URL='git://android.git.kernel.org/tools/repo.git'
4   REPO_REV='stable'
5
6   magic='--calling-python-from-/bin/sh--'
7   """exec" python -E "$0" "$@" """#$magic"
8   if __name__ == '__main__':
9     import sys
10    if sys.argv[-1] == '#%s' % magic:
11      del sys.argv[-1]
12  del magic

```

Repo 引导脚本是用什么语言开发的？这是一个问题。

*   第 1 行，有经验的 Linux 开发者会知道此脚本是用 Shell 脚本语言开发的。
*   第 7 行，是这个魔法的最神奇之处。既是一条合法的 shell 语句，又是一条合法的 python 语句。
*   第 7 行作为 shell 语句，执行**exec**，用 python 调用本脚本，并替换本进程。三引号在这里相当于一个空字串和一个单独的引号。
*   第 7 行作为 python 语句，三引号定义的是一个字符串，字符串后面是一个注释。
*   实际上第 1 行到第 7 行，即是合法的 shell 语句又是合法的 python 语句。从第 8 行开始后面都是 python 脚本了。
*   Repo 引导脚本无论是使用 shell 执行，或是用 python 执行，效果都相当于使用 python 执行此脚本。

**Repo 真正的位置在哪里？**

在引导脚本 repo 的`main`函数，首先调用`` _FindRepo``函数，从当前目录开始依次向上递归查找`.repo/repo/main.py`文件。

```
def main(orig_args):
  main, dir = _FindRepo()

```

函数`_FindRepo`返回找到的`.repo/repo/main.py`脚本文件，以及包含`repo/main.py`的`.repo`目录。如果找到`.repo/repo/main.py`脚本，则把程序的控制权交给`.repo/repo/main.py`脚本。（省略了在 repo 开发库中执行情况的判断）

在下载 repo 引导脚本后，没有初始化之前，当然不会存在`.repo/repo/main.py`脚本，这时必须进行初始化操作。

## 4.6.3\. repo 和清单库的初始化

下载并保存 repo 引导脚本后，建立一个工作目录，这个工作目录将作为 Android 的工作区目录。在工作目录中执行**repo init -u <url>**完成 repo 完整的下载以及项目清单版本库（manifest.git）的下载。

```
$ mkdir working-directory-name
$ cd working-directory-name
$ repo init -u git://android.git.kernel.org/platform/manifest.git

```

命令**repo init**要完成如下操作：

*   完成 repo 这一工具的完整下载，因为现在有的不过是 repo 的引导程序。

    初始化操作会从 Android 的代码中克隆`repo.git`库，到当前目录下的`.repo/repo`目录下。在完成 repo.git 克隆之后，repo init 命令会将控制权交给工作区的`.repo/repo/main.py`这个刚刚从`repo.git`库克隆来的脚本文件，继续进行初始化。

*   克隆 android 的清单库 manifest.git（地址来自于 -u 参数）。

    克隆的清单库位于`.repo/manifests.git`中，并本地克隆到`.repo/manifests`。清单文件`.repo/manifest.xml`是符号链接指向`.repo/manifests/default.xml`。

*   提问用户的姓名和邮件地址，如果和 Git 缺省的用户名、邮件地址不同，则记录在`.repo/manifests.git`库的 config 文件中。

*   命令**repo init**还可以附带`--mirror`参数，以建立和上游 Android 的版本库一模一样的镜像。会在后面的章节介绍。

**从哪里下载 repo.git？**

在 repo 引导脚本的前几行，定义了缺省的 repo.git 的版本库位置以及要检出的缺省分支。

```
REPO_URL='git://android.git.kernel.org/tools/repo.git'
REPO_REV='stable'

```

如果不想从缺省任务获取 repo，或者不想获取稳定版（stable 分支）的 repo，可以在**repo init**命令中通过下面的参数覆盖缺省的设置，从指定的源地址克隆 repo 代码库。

*   参数`--repo-url`，用于设定 repo 的版本库地址。
*   参数`--repo-branch`，用于设定要检出的分支。
*   参数`--no-repo-verify`，设定不要对 repo 的里程碑签名进行严格的验证。

实际上，完成`repo.git`版本库的克隆，这个 repo 引导脚本就江郎才尽了，`init`命令的后续处理（以及其他命令）都交给刚刚克隆出来的`.repo/repo/main.py`来继续执行。

**清单库是什么？从哪里下载？**

清单库实际上只包含一个`default.xml`文件。这个 XML 文件定义了多个版本库和本地地址的映射关系，是 repo 工作的指引文件。所以在使用 repo 引导脚本进行初始化的时候，必须通过`-u`参数指定清单库的源地址。

清单库的下载，是通过**repo init**命令初始化时，用`-u`参数指定清单库的位置。例如 repo 针对 Android 代码库进行初始化时执行的命令：

```
$ repo init -u git://android.git.kernel.org/platform/manifest.git

```

Repo 引导脚本的`` init``命令可以使用下列和清单库相关的参数：

*   参数`-u`（`--manifest-url`）：设定清单库的 Git 服务器地址。
*   参数`-b`（`--manifest-branch`）：检出清单库特定分支。
*   参数`--mirror`：只在 repo 第一次初始化的时候使用，以和 Android 服务器同样的结构在本地建立镜像。
*   参数`-m`（`--manifest-name`）：当有多个清单文件，可以指定清单库的某个清单文件为有效的清单文件。缺省为`default.xml`。

Repo 初始化命令（repo init）可以执行多次：

*   不带参数的执行**repo init**，从上游的清单库获取新的清单文件`default.xml`。
*   使用参数`-u`（`--manifest-url`）执行**repo init**，会重新设定上游的清单库地址，并重新同步。
*   使用参数`-b`（`--manifest-branch`）执行**repo init**，会使用清单库的不同分支，以便在使用**repo sync**时将项目同步到不同的里程碑。
*   但是不能使用`--mirror`命令，该命名只能在第一次初始化时执行。那么如何将已经按照工作区模式同步的版本库转换为镜像模式呢？会在后面看到一个解决方案。

## 4.6.4\. 清单库和清单文件

当执行完毕**repo init**之后，工作目录内空空如也。实际上有一个`.repo`目录。在该目录下除了一个包含 repo 的实现的 repo 库克隆外，就是 manifest 库的克隆，以及一个符号链接链接到清单库中的`default.xml`文件。

```
$ ls -lF .repo/
drwxr-xr-x 3 jiangxin jiangxin 4096 2010-10-11 18:57 manifests/
drwxr-xr-x 8 jiangxin jiangxin 4096 2010-10-11 10:08 manifests.git/
lrwxrwxrwx 1 jiangxin jiangxin   21 2010-10-11 10:07 manifest.xml -> manifests/default.xml
drwxr-xr-x 7 jiangxin jiangxin 4096 2010-10-11 10:07 repo/

```

在工作目录下的`.repo/manifest.xml`文件就是 Android 项目的众多版本库的清单文件。Repo 命令的操作，都要参考这个清单文件。

打开清单文件，会看到如下内容：

```
  1  <?xml version="1.0" encoding="UTF-8"?>
  2  <manifest>
  3    <remote  name="korg"
  4             fetch="git://android.git.kernel.org/"
  5             review="review.source.android.com" />
  6    <default revision="master"
  7             remote="korg" />
  8
  9    <project path="build" name="platform/build">
 10      <copyfile src="core/root.mk" dest="Makefile" />
 11    </project>
 12
 13    <project path="bionic" name="platform/bionic" />

       ...

181  </manifest>

```

这个文件不太复杂，是么？

*   这个 XML 的顶级元素是`manifest`，见第 2 行和第 181 行。
*   第 3 行通过一个 remote 元素，定义了名为 korg（kernel.org 缩写）的源，其 Git 库的基址为`git://android.git.kernel.org/`，还定义了代码审核服务器的地址`review.source.android.com`。还可以定义更多的 remote 元素，这里只定义了一个。
*   第 6 行用于设置各个项目缺省的远程源地址（remote）为 korg，缺省的分支为`master`。当然各个项目（project 元素）可以定义自己的 remote 和 revision 覆盖该缺省配置。
*   第 9 行定义一个项目，该项目的远程版本库相对路径为：”platform/build”，在工作区克隆的位置为：”build”。
*   第 10 行，即 project 元素的子元素 copyfile，定义了项目克隆后的一个附加动作：拷贝文件从`core/root.mk`至`Makefile`。
*   第 13 行后后续的 100 多行定义了其他 160 个项目，都是采用类似的 project 元素语法。`name`参数定义远程版本库的相对路径，`path`参数定义克隆到本地工作区的路径。
*   还可以出现`manifest-server`元素，其`url`属性定义了通过 XMLRPC 提供实时更新清单的服务器 URL。只有当执行**repo sync --smart-sync**的时候，才会检查该值，并用动态获取的 manifest 覆盖掉缺省的清单。

## 4.6.5\. 同步项目

在工作区，执行下面的命令，会参照`.repo/manifest.xml`清单文件，将项目所有相关的版本库全部克隆出来。不过最好请在读完本节内容之后再尝试执行这条命令。

```
$ repo sync

```

对于 Android，这个操作需要通过网络传递接近 2 个 GB 的内容，如果带宽不是很高的话，可能会花掉几个小时甚至是一天的时间。

也可以仅克隆感兴趣的项目，在**repo sync**后面跟上项目的名称。项目的名称来自于`.repo/manifest.xml`这个 XML 文件中 project 元素的 name 属性值。例如克隆`platform/build`项目：

```
$ repo sync platform/build

```

Repo 有一个功能可以在这里展示。就是 repo 支持通过本地清单，对缺省的清单文件进行补充以及覆盖。即可以在`.repo`目录下创建`local_manifest.xml`文件，其内容会和`.repo/manifest.xml`文件的内容进行合并。

在工作目录下运行下面的命令，可以创建一个本地清单文件。这个本地定制的清单文件来自缺省文件，但是删除了`remote`元素和`default`元素，并将所有的`project`元素都重命名为`remove-project`元素。这实际相当于对原有的清单文件“取反”。

```
$ sed -e '/<remote/,+4 d' -e 's/<project/<remove-project/g' \
  -e 's/project>/remove-project>/g' \
  < .repo/manifest.xml > .repo/local_manifest.xml

```

用下面的这条命令可以看到 repo 运行时实际获取到的清单。这个清单来自于`.repo/manifest.xml`和`.repo/local_manifest.xml`两个文件的汇总。

```
$ repo manifest -o -

```

当执行**repo sync**命令时，实际上就是依据合并后的清单文件进行同步。如果清单中的项目被自定义清单全部“取反”，执行同步就不会同步任何项目，甚至会删除已经完成同步的项目。

本地定制的清单文件`local_manifest.xml`支持前面介绍的清单文件的所有语法，需要注意的是：

*   不能出现重复定义的`remote`元素。这就是为什么上面的脚本要删除来自缺省`manifest.xml`的`remote`元素。
*   不能出现`default`元素，仅为全局仅能有一个。
*   不能出现重复的`project`定义（`name`属性不能相同），但是可以通过`remove-project`元素将缺省清单中定义的`project`删除再重新定义。

试着编辑`.repo/local_manifest.xml`，在其中再添加几个`project`元素，然后试着用**repo sync**命令进行同步。

## 4.6.6\. 建立 Android 代码库本地镜像

Android 为企业提供一个新的市场，无论大企业，小企业都是处于同一个起跑线上。研究 Android 尤其是 Android 系统核心或者是驱动的开发，首先需要做的就是本地克隆建立一套 Android 版本库管理机制。因为 Android 的代码库是那么庞杂，如果一个开发团队每个人都去执行**repo init -u**，再执行**repo sync**从 Android 服务器克隆版本库的话，多大的网络带宽恐怕都不够用。唯一的办法是本地建立一个 Android 版本库的镜像。

建立本地镜像非常简单，就是在执行**repo init -u**初始化的时候，附带上`--mirror`参数。

```
$ mkdir android-mirror-dir
$ cd android-mirror-dir
$ repo init --mirror -u git://android.git.kernel.org/platform/manifest.git

```

之后执行**repo sync**就可以安装 Android 的 Git 服务器方式来组织版本库，创建一个 Android 版本库镜像。

实际上附带了`--mirror`参数执行**repo init -u**命令，会在克隆的`.repo/manifests.git`下的`config`中记录配置信息：

```
[repo]
    mirror = true

```

**从 Android 的工作区到代码库镜像**

在初始化 repo 工作区时，如果使用不带`--mirror`参数的**repo init -u**，并完成代码同步后，如果再次执行**repo init**并附带了`--mirror`参数，repo 会报错退出：“fatal: –mirror not supported on existing client”。实际上`--mirror`参数只能对尚未初始化的 repo 工作区执行。

那么如果之前没有用镜像的方法同步 Android 版本库，难道要为创建代码库镜像再重新执行一次 repo 同步么？要知道重新同步一份 Android 版本库是非常慢的。我就遇到了这个问题。

不过既然有`manifest.xml``文件，完全可以对工作区进行反向操作，将工作区转换为镜像服务器的结构。下面就是一个示例脚本，这个脚本利用了已有的 repo 代码进行实现，所以看着很简洁。8-)

脚本`work2mirror.py`如下：

```
#!/usr/bin/python
# -*- coding: utf-8 -*-

import os, sys, shutil

cwd = os.path.abspath( os.path.dirname( __file__ ) )
repodir = os.path.join( cwd, '.repo' )
S_repo = 'repo'
TRASHDIR = 'old_work_tree'

if not os.path.exists( os.path.join(repodir, S_repo) ):
    print >> sys.stderr, "Must run under repo work_dir root."
    sys.exit(1)

sys.path.insert( 0, os.path.join(repodir, S_repo) )
from manifest_xml import XmlManifest

manifest = XmlManifest( repodir )

if manifest.IsMirror:
    print >> sys.stderr, "Already mirror, exit."
    sys.exit(1)

trash = os.path.join( cwd, TRASHDIR )

for project in manifest.projects.itervalues():
    # 移动旧的版本库路径到镜像模式下新的版本库路径
    newgitdir = os.path.join( cwd, '%s.git' % project.name )
    if os.path.exists( project.gitdir ) and project.gitdir != newgitdir:
        if not os.path.exists( os.path.dirname(newgitdir) ):
            os.makedirs( os.path.dirname(newgitdir) )
        print "Rename %s to %s." % (project.gitdir, newgitdir)
        os.rename( project.gitdir, newgitdir )

    # 移动工作区到待删除目录
    if project.worktree and os.path.exists( project.worktree ):
        newworktree = os.path.join( trash, project.relpath )
        if not os.path.exists( os.path.dirname(newworktree) ):
            os.makedirs( os.path.dirname(newworktree) )
        print "Move old worktree %s to %s." % (project.worktree, newworktree )
        os.rename( project.worktree, newworktree )

    if os.path.exists ( os.path.join( newgitdir, 'config' ) ):
        # 修改版本库的配置
        os.chdir( newgitdir )
        os.system( "git config core.bare true" )
        os.system( "git config remote.korg.fetch '+refs/heads/*:refs/heads/*'" )

        # 删除 remotes 分支，因为作为版本库镜像不需要 remote 分支
        if os.path.exists ( os.path.join( newgitdir, 'refs', 'remotes' ):
            print "Delete " + os.path.join( newgitdir, 'refs', 'remotes' )
            shutil.rmtree( os.path.join( newgitdir, 'refs', 'remotes') )

# 设置 menifest 为镜像
mp = manifest.manifestProject
mp.config.SetString('repo.mirror', 'true')

```

使用方法很简单，只要将脚本放在 Android 工作区下，执行就可以了。执行完毕会将原有工作区的目录移动到`old_work_tree`子目录下，在确认原有工作区没有未提交的数据后，直接删除`old_work_tree`即可。

```
$ python work2mirror.py

```

**创建新的清单库，或修改原有清单库**

建立了 Android 代码库的本地镜像后，如果不对`manifest`清单版本库进行定制，在使用**repo sync**同步代码的时候，仍然使用 Android 官方的代码库同步代码，使得本地的镜像版本库形同虚设。解决办法是创建一个自己的`manifest`库，或者在原有清单库中建立一个分支加以修改。如果创建新的清单库，参考 Android 上游的`manifest`清单库进行创建。

## 4.6.7\. Repo 的命令集

Repo 命令实际上是 Git 命令的简单或者复杂的封装。每一个 repo 命令都对应于 repo 源码树中`subcmds`目录下的一个同名的 Python 脚本。每一个 repo 命令都可以通过下面的命令获得帮助。

```
$ repo help <command>

```

通过阅读代码，可以更加深入的了解 repo 命令的封装。

### 4.6.7.1\. **repo init**命令

**repo init**命令，主要完成检出清单版本库（`manifest.git`），以及配置 Git 用户的用户名和邮件地址的工作。

实际上，完全可以进入到`.repo/manifests`目录，用 git 命令操作清单库。对`manifests`的修改不会因为执行**repo init**而丢失，除非是处于未跟踪状态。

### 4.6.7.2\. **repo sync**命令

**repo sync**命令用于参照清单文件克隆或者同步版本库。如果某个项目版本库尚不存在，则执行**repo sync**命令相当于执行**git clone**。如果项目版本库已经存在，则相当于执行下面的两个命令：

*   git remote update

    相当于对每一个 remote 源执行`fetch`操作。

*   git rebase origin/branch

    针对当前分支的跟踪分支，执行`rebase`操作。不采用`merge`而是采用`rebase`，目的是减少提交数量，方便评审(Gerrit)。

### 4.6.7.3\. **repo start**命令

**repo start**命令实际上是对**git checkout -b**命令的封装。为指定的项目或者所有项目（若使用`--all`参数），以清单文件中为项目设定的分支或里程碑为基础，创建特性分支。特性分支的名称由命令的第一个参数指定。相当于执行**git checkout -b**。

用法:

```
repo start <newbranchname> [--all | <project>...]

```

### 4.6.7.4\. **repo status**命令

**repo status**命令实际上是对**git diff-index**、**git diff-files**命令的封装，同时显示暂存区的状态和本地文件修改的状态。

用法：

```
repo status [<project>...]

```

示例输出：

```
project repo/                                   branch devwork
 -m     subcmds/status.py
 ...

```

上面示例输出显示了 repo 项目的`devwork`分支的修改状态。

*   每个小节的首行显示项目名称，以及所在分支名称。

*   之后显示该项目中文件变更状态。头两个字母显示变更状态，后面显示文件名或者其他变更信息。

*   第一个字母表示暂存区的文件修改状态。

    其实是**git-diff-index**命令输出中的状态标识，并用大写显示。

    *   `-`：没有改变
    *   `A`：添加 （不在 HEAD 中， 在暂存区 ）
    *   `M`：修改 （ 在 HEAD 中， 在暂存区，内容不同 ）
    *   `D`：删除 （ 在 HEAD 中，不在暂存区 ）
    *   `R`：重命名 （不在 HEAD 中， 在暂存区，路径修改 ）
    *   `C`：拷贝 （不在 HEAD 中， 在暂存区，从其他文件拷贝）
    *   `T`：文件状态改变 （ 在 HEAD 中， 在暂存区，内容相同 ）
    *   `U`：未合并，需要冲突解决
*   第二个字母表示工作区文件的更改状态。

    其实是**git-diff-files**命令输出中的状态标识，并用小写显示。

    *   `-`：新/未知 （不在暂存区， 在工作区 ）
    *   `m`：修改 （ 在暂存区， 在工作区，被修改 ）
    *   `d`：删除 （ 在暂存区，不在工作区 ）
*   两个表示状态的字母后面，显示文件名信息。如果有文件重命名还会显示改变前后的文件名以及文件的相似度。

### 4.6.7.5\. **repo checkout**命令

**repo checkout**命令实际上是对**git checkout**命令的封装。检出之前由**repo start**创建的分支。

用法：

```
repo checkout <branchname> [<project>...]

```

### 4.6.7.6\. **repo branches**命令

**repo branches**命令读取各个项目的分支列表并汇总显示。该命令实际上是通过直接读取`.git/refs`目录下的引用来获取分支列表，以及分支的发布状态等。

用法：

```
repo branches [<project>...]

```

输出示例：

```
*P nocolor                   | in repo
   repo2                     |

```

*   第一个字段显示分支的状态：是否是当前分支，分支是否发布到代码审核服务器上？

*   第一个字母若显示星号(*)，含义是此分支为当前分支

*   第二个字母若为大写字母`P`，则含义是分支所有提交都发布到代码审核服务器上了。

*   第二个字母若为小写字母`p`，则含义是只有部分提交被发布到代码审核服务器上。

*   若不显示`P`或者`p`，则表明分支尚未发布。

*   第二个字段为分支名。

*   第三个字段为以竖线（|）开始的字符串，表示该分支存在于哪些项目中。

    *   `| in all projects`

        该分支处于所有项目中。

    *   `| in project1 project2`

        该分支只在特定项目中定义。如：`project1`、`project2`。

    *   `| not in project1`

    > 该分支不存在于这些项目中。即除了`project1`项目外，其他项目都包含此分支。

### 4.6.7.7\. **repo diff**命令

**repo diff**命令实际上是对**git diff**命令的封装，用以分别显示各个项目工作区下的文件差异。

用法：

```
repo diff [<project>...]

```

### 4.6.7.8\. **repo stage**命令

**repo stage**命令实际上是对**git add --interactive**命令的封装，用以对各个项目工作区中的改动（修改、添加等）进行挑选以加入暂存区。

用法：

```
repo stage -i [<project>...]

```

### 4.6.7.9\. **repo upload**命令

**repo upload**命令相当于**git push**，但是又有很大的不同。执行**repo upload**不是将版本库改动推送到克隆时的远程服务器，而是推送到代码审查服务器（由 Gerrit 软件架设）的特殊引用上，使用的是 SSH 协议（特殊端口）。代码审核服务器会对推送的提交进行特殊处理，将新的提交显示为一个待审核的修改集，并进入代码审查流程。只有当审核通过，才会合并到官方正式的版本库中。

用法：

```
repo upload [--re --cc] {[<project>]... | --replace <project>}

参数：
  -h, --help            显示帮助信息。
  -t                    发送本地分支名称到 Gerrit 代码审核服务器。
  --replace             发送此分支的更新补丁集。注意使用该参数，只能指定一个项目。
  --re=REVIEWERS, --reviewers=REVIEWERS
                        要求由指定的人员进行审核。
  --cc=CC               同时发送通知到如下邮件地址。

```

**确定推送服务器的端口**

分支改动的推送是发给代码审核服务器，而不是下载代码的服务器。使用的协议是 SSH 协议，但是使用的并非标准端口。如何确认代码审核服务器上提供的特殊 SSH 端口呢？

在执行**repo upload**命令时，repo 会通过访问代码审核 Web 服务器的 `/ssh_info`的 URL 获取 SSH 服务端口，缺省 29418。这个端口，就是**repo upload**发起推送的服务器的 SSH 服务端口。

**修订集修改后重新传送**

当已经通过**repo upload**命令在代码审查服务器上提交了一个修订集，会得到一个修订号。关于此次修订的相关讨论会发送到提交者的邮箱中。如果修订集有误没有通过审核，可以重新修改代码，再次向代码审核服务器上传修订集。

一个修订集修改后再次上传，如果修订集的 ID 不变是非常有用的，因为这样相关的修订集都在代码审核服务器的同一个界面中显示。

在执行**repo upload**时会弹出一个编辑界面，提示在方括号中输入修订集编号，否则会在代码审查服务器上创建新的 ID。有一个办法可以不用手工输入修订集，如下：

```
repo upload --replace project_name

```

当使用`--replace`参数后，repo 会检查本地版本库名为`refs/published/branch_name`的特殊引用（上一次提交的修订），获得其对应的提交 SHA1 哈希值。然后在代码审核服务器的`refs/changes/`命名空间下的特殊引用中寻找和提交 SHA1 哈希值匹配的引用，找到的匹配引用其名称中就所包含有变更集 ID，直接用此变更集 ID 作为新的变更集 ID 提交到代码审核服务器。

**Gerrit 服务器魔法**

**repo upload**命令执行推送，实际上会以类似如下的命令行格式进行调用：

```
git push --receive-pack='gerrit receive-pack --reviewer charlie@example.com' \
         ssh://review.example.com:29418/project HEAD:refs/for/master

```

当 Gerrit 服务器接收到**git push**请求后，会自动将对分支的提交转换为修订集，显示于 Gerrit 的提交审核界面中。Gerrit 的魔法破解的关键点就在于**git push**命令的`--receive-pack`参数。即提交交由**gerrit-receive-pack**命令执行，进入非标准的 Git 处理流程，将提交转换为在`refs/changes`命名空间下的引用，而不在`refs/for`命名空间下创建引用。

### 4.6.7.10\. **repo download**命令

**repo download**命令主要用于代码审核者下载和评估贡献者提交的修订。贡献者的修订在 Git 版本库中以`refs/changes/<changeid>/<patchset>`引用方式命名（缺省的 patchset 为 1），和其他 Git 引用一样，用**git fetch**获取，该引用所指向的最新的提交就是贡献者待审核的修订。使用**repo download**命令实际上就是用**git fetch**获取到对应项目的`refs/changes/<changeid>/patchset>`引用，并自动切换到对应的引用上。

用法：

```
repo download {project change[/patchset]}...

```

### 4.6.7.11\. **repo rebase**命令

**repo rebase**命令实际上是对**git rebase**命令的封装，该命令的参数也作为**git rebase**命令的参数。但 -i 参数仅当对一个项执行时有效。

用法：

```
命令行: repo rebase {[<project>...] | -i <project>...}

参数:
  -h, --help          显示帮助并退出
  -i, --interactive   交互式的变基（仅对一个项目时有效）
  -f, --force-rebase  向 git rebase 命令传递 --force-rebase 参数
  --no-ff             向 git rebase 命令传递 -no-ff 参数
  -q, --quiet         向 git rebase 命令传递 --quiet 参数
  --autosquash        向 git rebase 命令传递 --autosquash  参数
  --whitespace=WS     向 git rebase 命令传递 --whitespace  参数

```

### 4.6.7.12\. **repo prune**命令

**repo prune**命令实际上是对**git branch -d**命令的封装，该命令用于扫描项目的各个分支，并删除已经合并的分支。

用法：

```
repo prune [<project>...]

```

### 4.6.7.13\. **repo abandon**命令

相比**repo prune**命令，**repo abandon**命令更具破坏性，因为**repo abandon**是对**git branch -D**命令的封装。该命令非常危险，直接删除分支，请慎用。

用法：

```
repo abandon <branchname> [<project>...]

```

### 4.6.7.14\. 其他命令

*   **repo grep**

    相当于对**git grep**的封装，用于在项目文件中进行内容查找。

*   **repo smartsync**

    相当于用`-s`参数执行**repo sync**。

*   **repo forall**

    迭代器，可以对 repo 管理的项目进行迭代。

*   **repo manifest**

    显示`manifest`文件内容。

*   **repo version**

    显示 repo 的版本号。

*   **repo selfupdate**

    用于 repo 自身的更新。如果提供`--repo-upgraded`参数，还会更新各个

    项目的钩子脚本。

## 4.6.8\. Repo 命令的工作流

图 25-1 是 repo 的工作流，每一个代码贡献都起始于**repo start**创建本地工作分支，最终都以**repo upload**命令将代码补丁发布于代码审核服务器。

![../_images/repo-workflow.png](img/repo-workflow.png)

图 25-1：repo 工作流

## 4.6.9\. 好东西不能 Android 独享

通过前面的介绍能够体会到 repo 的精巧——repo 巧妙的实现了多 Git 版本库的管理。因为 repo 使用了清单版本库，所以 repo 这一工具并没有被局限于 Android 项目，可以在任何项目中使用。下面就介绍三种 repo 的使用模式，将 repo 引入自己的（非 Android）项目中，其中第三种 repo 使用模式是用作者改造后的 repo 实现脱离 Gerrit 服务器进行推送。

### 4.6.9.1\. Repo+Gerrit 模式

Repo 和 Gerrit 是 Android 代码管理的两大支柱。正如前面在 repo 工作流中介绍的，部分的 repo 命令从 Git 服务器读取，这个 Git 服务器可以是只读的版本库控制服务器，还有部分 repo 命令（**repo upload**、**repo download**）访问的则是代码审核服务器，其中**repo upload**命令还要向代码审核服务器进行**git push**操作。

在使用未经改动的 repo 来维护自己的项目（多个版本库组成）时，必须搭建 Gerrit 代码审核服务器。

搭建项目的版本控制系统环境的一般方法为：

*   用 git-daemon 或者 http 服务搭建 Git 服务器。具体搭建方法参见第 5 篇“搭建 Git 服务器”相关章节。
*   导入`repo.git`工具库。非必须，只是为了减少不必要的互联网操作。
*   还可以在内部 http 服务器维护一个定制的 repo 引导脚本。非必须。
*   建立 Gerrit 代码审核服务器。会在第 5 篇第三十二章“Gerrit 代码审核服务器”中介绍 Gerrit 的安装和使用。
*   将相关的子项目代码库一一创建。
*   建立一个`manifest.git`清单库，其中`remote`元素的`fetch`属性指向只读 Git 服务器地址，`review`属性指向代码审核服务器地址。

示例如下：

> ```
> <?xml version="1.0" encoding="UTF-8"?>
> <manifest>
>   <remote  name="example"
>            fetch="git://git.example.net/"
>            review="review.example.net" />
>   <default revision="master"
>            remote="example" />
> 
>   ...
> 
> ```

### 4.6.9.2\. Repo 无审核模式

Gerrit 代码审核服务器部署比较麻烦，更不要说因为 Gerrit 用户界面的学习和用户使用习惯的更改而带来的困难了。在一个固定的团队内部使用 repo 可能真的没有必要使用 Gerrit，因为团队成员都应该熟悉 Git 的操作，团队成员的编程能力都可信，单元测试质量由提交者保证，集成测试由单独的测试团队进行，即团队拥有一套完整、成型的研发工作流，引入 Gerrit 并非必要。

脱离了 Gerrit 服务器，直接跟 Git 服务器打交道，repo 可以工作么？是的，可以利用**repo forall**迭代器实现多项目代码的 PUSH，其中有如下关键点需要重点关注。

*   **repo start**命令创建本地分支时，需要使用和上游同样的分支名。

    如果使用不同的分支名，上传时需要提供复杂的引用描述。下面的示例先通过**repo manifest**命令确认上游清单库缺省的分支名为`master`，再使用该分支名（`master`）作为本地分支名执行**repo start**。示例如下：

    ```
    $ repo manifest -o - | grep default
      <default remote="bj" revision="master"/>

    $ repo start master --all

    ```

*   推送不能使用**repo upload**，而需要使用**git push**命令。

    可以利用**repo forall**迭代器实现批命令方式执行。例如：

    ```
    $ repo forall -c git push

    ```

*   如果清单库中的上游 Git 库地址用的是只读地址，需要为本地版本库一一更改上游版本库地址。

    可以使用`forall`迭代器，批量为版本库设置**git push**时的版本库地址。下面的命令使用了环境变量`$REPO_PROJECT`是实现批量设置的关键。

    ```
    $ repo forall -c \
      'git remote set-url --push bj android@bj.ossxp.com:android/${REPO_PROJECT}.git'

    ```

### 4.6.9.3\. 改进的 Repo 无审核模式

前面介绍的使用**repo forall**迭代器实现在无审核服务器情况下向上游推送提交，只是权宜之计，尤其是用**repo start**建立工作分支要求和上游一致，实在是有点强人所难。

我改造了 repo，增加了两个新的命令**repo config**和**repo push**，让 repo 可以脱离 Gerrit 服务器直接向上游推送。代码托管在 Github 上：[`github.com/ossxp-com/repo.git`](http://github.com/ossxp-com/repo.git)。下面简单介绍一下如何使用改造之后的 repo。

#### 4.6.9.3.1\. 下载改造后的 repo 引导脚本

建议使用改造后的 repo 引导脚本替换原脚本，否则在执行**repo init**命令需要提供额外的`--no-repo-verify`参数，以及`--repo-url`和`--repo-branch`参数。

```
$ curl http://github.com/ossxp-com/repo/raw/master/repo > ~/bin/repo
$ chmod a+x ~/bin/repo

```

#### 4.6.9.3.2\. 用 repo 从 Github 上检出测试项目

如果安装了改造后的 repo 引导脚本，使用下面的命令初始化 repo 及清单库。

```
$ mkdir test
$ cd test
$ repo init -u git://github.com/ossxp-com/manifest.git
$ repo sync

```

如果用的是标准的（未经改造的）repo 引导脚本，用下面的命令。

```
$ mkdir test
$ cd test
$ repo init --repo-url=git://github.com/ossxp-com/repo.git \
  --repo-branch=master --no-repo-verify \
  -u git://github.com/ossxp-com/manifest.git
$ repo sync

```

当子项目代码全部同步完成后，执行 make 命令。可以看到各个子项目的版本以及清单库的版本。

```
$ make
Version of test1:    1:0.2-dev
Version of test2:    2:0.2
Version of manifest: current

```

#### 4.6.9.3.3\. 用**repo config**命令设置 pushurl

现在如果进入到各个子项目目录，是无法成功执行**git push**命令的，因为上游 Git 库的地址是一个只读访问的 URL，无法提供写服务。可以用新增的**repo config**命令设置当执行**git push**时的 URL 地址。

```
$ repo config repo.pushurl ssh://git@github.com/ossxp-com/

```

设置成功后，可以使用**repo config repo.pushurl**查看设置。

```
$ repo config repo.pushurl
ssh://git@github.com/ossxp-com/

```

#### 4.6.9.3.4\. 创建本地工作分支

使用下面的命令创建一个工作分支`jiangxin`。

```
$ repo start jiangxin --all

```

使用**repo branches**命令可以查看当前所有的子项目都属于`jiangxin`分支

```
$ repo branches
*  jiangxin                  | in all projects

```

参照下面的方法修改`test/test1`子项目。对`test/test2`项目也作类似修改。

```
$ cd test/test1
$ echo "1:0.2-jiangxin" > version
$ git diff
diff --git a/version b/version
index 37c65f8..a58ac04 100644
--- a/version
+++ b/version
@@ -1 +1 @@
-1:0.2-dev
+1:0.2-jiangxin
$ repo status
# on branch jiangxin
project test/test1/                             branch jiangxin
 -m     version
$ git add -u
$ git commit -m "0.2-dev -> 0.2-jiangxin"

```

执行**make**命令，看看各个项目的改变。

```
$ make
Version of test1:    1:0.2-jiangxin
Version of test2:    2:0.2-jiangxin
Version of manifest: current

```

#### 4.6.9.3.5\. PUSH 到远程服务器

直接执行**repo push**就可以将各个项目的改动进行推送。

```
$ repo push

```

如果有多个项目同时进行了改动，为了避免出错，会弹出编辑器显示因为包含改动而需要推送的项目列表。

```
# Uncomment the branches to upload:
#
# project test/test1/:
#  branch jiangxin ( 1 commit, Mon Oct 25 18:04:51 2010 +0800):
#         4f941239 0.2-dev -> 0.2-jiangxin
#
# project test/test2/:
#  branch jiangxin ( 1 commit, Mon Oct 25 18:06:51 2010 +0800):
#         86683ece 0.2-dev -> 0.2-jiangxin

```

每一行前面的井号是注释，会被忽略。将希望推送的分支前的注释去掉，就可以将该项目的分支执行推送动作。下面的操作中，把其中的两个分支的注释都去掉了，这两个项目当前分支的改动会推送到上游服务器。

```
# Uncomment the branches to upload:

#
# project test/test1/:
branch jiangxin ( 1 commit, Mon Oct 25 18:04:51 2010 +0800):
#         4f941239 0.2-dev -> 0.2-jiangxin
#
# project test/test2/:
branch jiangxin ( 1 commit, Mon Oct 25 18:06:51 2010 +0800):
#         86683ece 0.2-dev -> 0.2-jiangxin

```

保存退出（如果使用 vi 编辑器，输入`<ESC>:wq`执行保存退出）后，马上开始对选择的各个项目执行**git push**。

```
Counting objects: 5, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 293 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To ssh://git@github.com/ossxp-com/test1.git
   27aee23..4f94123  jiangxin -> master
Counting objects: 5, done.
Writing objects: 100% (3/3), 261 bytes, done.
Total 3 (delta 0), reused 0 (delta 0)
To ssh://git@github.com/ossxp-com/test2.git
   7f0841d..86683ec  jiangxin -> master

--------------------------------------------
[OK    ] test/test1/     jiangxin
[OK    ] test/test2/     jiangxin

```

从推送的命令输出可以看出来本地的工作分支`jiangxin`的改动被推送的远程服务器的`master`分支（本地工作分支跟踪的上游分支）。

再次执行**repo push**，会显示没有项目需要推送。

```
$ repo push
no branches ready for upload

```

#### 4.6.9.3.6\. 在远程服务器创建新分支

如果想在服务器上创建一个新的分支，该如何操作呢？如下使用`--new_branch`参数调用**repo push**命令。

```
$ repo start feature1 --all
$ repo push --new_branch

```

经过同样的编辑操作之后，自动调用**git push**，在服务器上创建新分支`feature1`。

```
Total 0 (delta 0), reused 0 (delta 0)
To ssh://git@github.com/ossxp-com/test1.git
 * [new branch]      feature1 -> feature1
Total 0 (delta 0), reused 0 (delta 0)
To ssh://git@github.com/ossxp-com/test2.git
 * [new branch]      feature1 -> feature1

--------------------------------------------
[OK    ] test/test1/     feature1
[OK    ] test/test2/     feature1

```

用**git ls-remote**命令查看远程版本库的分支，会发现远程版本库中已经建立了新的分支。

```
$ git ls-remote git://github.com/ossxp-com/test1.git refs/heads/*
4f9412399bf8093e880068477203351829a6b1fb        refs/heads/feature1
4f9412399bf8093e880068477203351829a6b1fb        refs/heads/master
b2b246b99ca504f141299ecdbadb23faf6918973        refs/heads/test-0.1

```

注意到`feature1`和`master`分支引用指向相同的 SHA1 哈希值，这是因为`feature1`分支是直接从`master`分支创建的。

#### 4.6.9.3.7\. 通过不同的清单库版本，切换到不同分支

换用不同的清单库，需要建立新的工作区，并且在执行**repo init**时，通过`-b`参数指定清单库的分支。

```
$ mkdir test-0.1
$ cd test-0.1
$ repo init -u git://github.com/ossxp-com/manifest.git -b test-0.1
$ repo sync

```

当子项目代码全部同步完成后，执行**make**命令。可以看到各个子项目的版本以及清单库的版本不同于之前的输出。

```
$ make
Version of test1:    1:0.1.4
Version of test2:    2:0.1.3-dev
Version of manifest: current-2-g12f9080

```

可以用**repo manifest**命令来查看清单库。

```
$ repo manifest -o -
<?xml version="1.0" encoding="UTF-8"?>
<manifest>
  <remote fetch="git://github.com/ossxp-com/" name="github"/>

  <default remote="github" revision="refs/heads/test-0.1"/>

  <project name="test1" path="test/test1">
    <copyfile dest="Makefile" src="root.mk"/>
  </project>
  <project name="test2" path="test/test2"/>
</manifest>

```

仔细看上面的清单文件，可以注意到缺省的版本指向到`refs/heads/test-0.1`引用所指向的分支`test-0.1`。

如果在子项目中修改、提交，然后使用**repo push**会将改动推送的远程版本库的`test-0.1`分支中。

#### 4.6.9.3.8\. 切换到清单库里程碑版本

执行如下命令，可以查看清单库包含的里程碑版本：

```
$ git ls-remote --tags git://github.com/ossxp-com/manifest.git
43e5783a58b46e97270785aa967f09046734c6ab        refs/tags/current
3a6a6da36840e716a14d52252e7b40e6ba6cbdea        refs/tags/current^{}
4735d32613eb50a6c3472cc8087ebf79cc46e0c0        refs/tags/v0.1
fb1a1b7302a893092ce8b356e83170eee5863f43        refs/tags/v0.1^{}
b23884d9964660c8dd34b343151aaf968a744400        refs/tags/v0.1.1
9c4c287069e29d21502472acac34f28896d7b5cc        refs/tags/v0.1.1^{}
127d9789cd4312ed279a7fa683c43eec73d2b28b        refs/tags/v0.1.2
47aaa83866f6d910a118a9a19c2ac3a2a5819b3e        refs/tags/v0.1.2^{}
af3abb7ed0a9ef7063e9d814510c527287c92ef6        refs/tags/v0.1.3
99c69bcfd7e2e7737cc62a7d95f39c6b9ffaf31a        refs/tags/v0.1.3^{}

```

可以从任意里程碑版本的清单库初始化整个项目。

```
$ mkdir v0.1.2
$ cd v0.1.2
$ repo init -u git://github.com/ossxp-com/manifest.git -b refs/tags/v0.1.2
$ repo sync

```

当子项目代码全部同步完成后，执行**make**命令。可以看到各个子项目的版本以及清单库的版本不同于之前的输出。

```
$ make
Version of test1:    1:0.1.2
Version of test2:    2:0.1.2
Version of manifest: v0.1.2

```

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.

# 4.7\. Git 和 SVN 协同模型

在本篇的最后，将会在另外的一个角度上看 Git 版本库的协同。不是不同的用户在使用 Git 版本库时如何协同，也不是一个项目包含多个 Git 版本库时如何协同，而是当版本控制系统不是 Git（如 Subversion）时，如何能够继续使用 Git 的方式进行操作。

Subversion 会在商业软件开发中占有一席之地，只要商业软件公司严格封闭源代码的策略不改变。对于熟悉了 Git 的用户，一定会对 Subversion 的那种一旦脱离网络、脱离服务器便寸步难行的工作模式厌烦透顶。实际上对 Subversion 的集中式版本控制的不满和改进在 Git 诞生之前就发生了，这就是 SVK。

在 2003 年（Git 诞生的前两年），台湾的高嘉良就开发了 SVK，用分布式版本控制的方法操作 SVN。其设计思想非常朴素，既然 SVN 的用户可以看到有访问权限数据的全部历史，那么也应该能够依据历史重建一个本地的 SVN 版本库，这样很多 SVN 操作都可以通过本地的 SVN 进行，从而脱离网络。当对本地版本库的修改感到满意后，通过本地 SVN 版本和服务器 SVN 版本库之间的双向同步，将改动归并到服务器上。这种工作方式真的非常酷。

不必为 SVK 的文档缺乏以及不再维护而感到惋惜，因为有更强的工具登场了，这就是 git-svn。git-svn 是 Git 软件包的一部分，用 Perl 语言开发。它的工作原理是：

*   将 Subversion 版本库在本地转换为一个 Git 库。
*   转换可以基于 Subversion 的某个目录，或者基于某个分支，或者整个 Subversion 代码库的所有分支和里程碑。
*   远程的 Subversion 版本库可以和本地的 Git 双向同步。Git 本地库修改推送到远程 Subversion 版本库，反之亦然。

git-svn 作为 Git 软件包的一部分，当 Git 从源码包进行安装时会默认安装，提供**git svn**命令。而几乎所有的 Linux 发行版都将 git-svn 作为一个独立的软件单独发布，因此需要单独安装。例如 Debian 和 Ubuntu 运行下面命令安装 git-svn。

```
$ sudo aptitude install git-svn

```

将 git-svn 独立安装是因为 git-svn 软件包有着特殊的依赖，即依赖 Subversion 的 Perl 语言绑定接口，Debian/Ubuntu 上由`libsvn-perl`软件包提供。

当 git-svn 正确安装后，就可以使用**git svn**命令了。但如果在执行**git svn --version**时遇到下面的错误，则说明 Subversion 的 Perl 语言绑定没有正确安装。

```
$ git svn --version
Can't locate loadable object for module SVN::_Core in @INC (@INC contains: ...) at /usr/lib/perl5/SVN/Base.pm line 59
BEGIN failed--compilation aborted at /usr/lib/perl5/SVN/Core.pm line 5.
Compilation failed in require at /usr/lib/git-core/git-svn line 41.

```

遇到上面的情况，需要检查本机是否正确安装了 Subversion 以及 Subversion 的 Perl 语言绑定。

为了便于对 git-svn 的介绍和演示，需要有一个 Subversion 版本库，并且需要有提交权限以便演示用 Git 向 Subversion 进行提交。最好的办法是在本地创建一个 Subversion 版本库。

```
$ svnadmin create /path/to/svn/repos/demo

$ svn co file:///path/to/svn/repos/demo svndemo
取出版本 0

$ cd svndemo

$ mkdir trunk tags branches
$ svn add *
A         branches
A         tags
A         trunk

$ svn ci -m "initialized."
增加           branches
增加           tags
增加           trunk

提交后的版本为 1。

```

再向 Subversion 开发主线`trunk`中添加些数据。

```
$ echo hello > trunk/README
$ svn add trunk/README
A         trunk/README
$ svn ci -m "hello"
增加           trunk/README
传输文件数据.
提交后的版本为 2。

```

建立分支：

```
$ svn up
$ svn cp trunk branches/demo-1.0
A         branches/demo-1.0
$ svn ci -m "new branch: demo-1.0"
增加           branches/demo-1.0

提交后的版本为 3。

```

建立里程碑：

```
$ svn cp -m "new tag: v1.0" trunk file:///path/to/svn/repos/demo/tags/v1.0

提交后的版本为 4。

```

## 4.7.1\. 使用 git-svn 的一般流程

使用 git-svn 的一般流程参见图 26-1。

![../_images/git-svn-workflow.png](img/git-svn-workflow.png)

图 26-1：git-svn 工作流

首先用**git svn clone**命令对 Subversion 进行克隆，创建一个包含 git-svn 扩展的本地 Git 库。在下面的示例中，使用 Subversion 的本地协议（`file://`）来访问之前创立的 Subversion 示例版本库，实际上 git-svn 可以使用任何 Subversion 可用的协议，并可以对远程版本库进行操作。

```
$ git svn clone -s file:///path/to/svn/repos/demo git-svn-demo
Initialized empty Git repository in /path/to/my/workspace/git-svn-demo/.git/
r1 = 2c73d657dfc3a1ceca9d465b0b98f9e123b92bb4 (refs/remotes/trunk)
        A       README
r2 = 1863f91b45def159a3ed2c4c4c9428c25213f956 (refs/remotes/trunk)
Found possible branch point: file:///path/to/svn/repos/demo/trunk => file:///path/to/svn/repos/demo/branches/demo-1.0, 2
Found branch parent: (refs/remotes/demo-1.0) 1863f91b45def159a3ed2c4c4c9428c25213f956
Following parent with do_switch
Successfully followed parent
r3 = 1adcd5526976fe2a796d932ff92d6c41b7eedcc4 (refs/remotes/demo-1.0)
Found possible branch point: file:///path/to/svn/repos/demo/trunk => file:///path/to/svn/repos/demo/tags/v1.0, 2
Found branch parent: (refs/remotes/tags/v1.0) 1863f91b45def159a3ed2c4c4c9428c25213f956
Following parent with do_switch
Successfully followed parent
r4 = c12aa40c494b495a846e73ab5a3c787ca1ad81e9 (refs/remotes/tags/v1.0)
Checked out HEAD:
  file:///path/to/svn/repos/demo/trunk r2

```

从上面的输出可以看出，当执行了**git svn clone**之后，在本地工作目录创建了一个 Git 库（git-svn-demo），并将 Subversion 的每一个提交都转换为 Git 库中的提交。进入`git-svn-demo`目录，看看用 git-svn 克隆出来的版本库。

```
$ cd git-svn-demo/
$ git branch -a
* master
  remotes/demo-1.0
  remotes/tags/v1.0
  remotes/trunk
$ git log
commit 1863f91b45def159a3ed2c4c4c9428c25213f956
Author: jiangxin <jiangxin@f79726c4-f016-41bd-acd5-6c9acb7664b2>
Date:   Mon Nov 1 05:49:41 2010 +0000

    hello

    git-svn-id: file:///path/to/svn/repos/demo/trunk@2 f79726c4-f016-41bd-acd5-6c9acb7664b2

commit 2c73d657dfc3a1ceca9d465b0b98f9e123b92bb4
Author: jiangxin <jiangxin@f79726c4-f016-41bd-acd5-6c9acb7664b2>
Date:   Mon Nov 1 05:47:03 2010 +0000

    initialized.

    git-svn-id: file:///path/to/svn/repos/demo/trunk@1 f79726c4-f016-41bd-acd5-6c9acb7664b2

```

看到 Subversion 版本库的分支和里程碑都被克隆出来，并保存在`refs/remotes`下的引用中。在**git log**的输出中，可以看到 Subversion 的提交的确被转换为 Git 的提交。

下面就可以在 Git 库中进行修改，并在本地提交（用**git commit**命令）。

```
$ cat README
hello
$ echo "I am fine." >> README
$ git add -u
$ git commit -m "my hack 1."
[master 55e5fd7] my hack 1.
 1 files changed, 1 insertions(+), 0 deletions(-)
$ echo "Thank you." >> README
$ git add -u
$ git commit -m "my hack 2."
[master f1e00b5] my hack 2.
 1 files changed, 1 insertions(+), 0 deletions(-)

```

对工作区中的`README`文件修改了两次，并进行了本地的提交。查看这时的提交日志，会发现最新两个只在本地 Subversion 版本库的提交和之前 Subversion 中的提交的不同。区别在于最新在 Git 中的提交没有用`git-svn-id:`标签标记的行。

```
$ git log
commit f1e00b52209f6522dd8135d27e86370de552a7b6
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Thu Nov 4 15:05:47 2010 +0800

    my hack 2.

commit 55e5fd794e6208703aa999004ec2e422b3673ade
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Thu Nov 4 15:05:32 2010 +0800

    my hack 1.

commit 1863f91b45def159a3ed2c4c4c9428c25213f956
Author: jiangxin <jiangxin@f79726c4-f016-41bd-acd5-6c9acb7664b2>
Date:   Mon Nov 1 05:49:41 2010 +0000

    hello

    git-svn-id: file:///path/to/svn/repos/demo/trunk@2 f79726c4-f016-41bd-acd5-6c9acb7664b2

commit 2c73d657dfc3a1ceca9d465b0b98f9e123b92bb4
Author: jiangxin <jiangxin@f79726c4-f016-41bd-acd5-6c9acb7664b2>
Date:   Mon Nov 1 05:47:03 2010 +0000

    initialized.

    git-svn-id: file:///path/to/svn/repos/demo/trunk@1 f79726c4-f016-41bd-acd5-6c9acb7664b2

```

现在就可以向 Subversion 服务器推送改动了。但真实的环境中，往往在向服务器推 送时，已经有其他用户已经在服务器上进行了提交，而且往往更糟的是，先于我们 的提交会造成我们的提交冲突！现在就人为的制造一个冲突：使用**svn**命令在 Subversion 版本库中执行一次提交。

```
$ svn checkout file:///path/to/svn/repos/demo/trunk demo
A    demo/README
取出版本 4。
$ cd demo/
$ cat README
hello
$ echo "HELLO." > README
$ svn commit -m "hello -> HELLO."
正在发送       README
传输文件数据.
提交后的版本为 5。

```

好的，已经模拟了一个用户先于我们更改了 Subversion 版本库。现在回到用 git-svn 克隆的本地版本库，执行**git svn dcommit**操作，将 Git 中的提交推送的 Subversion 版本库中。

```
$ git svn dcommit
Committing to file:///path/to/svn/repos/demo/trunk ...
事务过时: 文件 “/trunk/README” 已经过时 at /usr/lib/git-core/git-svn line 572

```

显然，由于 Subversion 版本库中包含了新的提交，导致执行**git svn dcommit**出错。这时需执行**git svn fetch**命令，以从 Subversion 版本库获取更新。

```
$ git svn fetch
        M       README
r5 = fae6dab863ed2152f71bcb2348d476d47194fdd4 (refs/remotes/trunk)
$ git st
# On branch master
nothing to commit (working directory clean)

```

当获取了新的 Subversion 提交之后，需要执行**git svn rebase**将 Git 中未推送到 Subversion 的提交通过变基（rebase）形成包含 Subversion 最新提交的线性提交。这是因为 Subversion 的提交都是线性的。

```
$ git svn rebase
First, rewinding head to replay your work on top of it...
Applying: my hack 1.
Using index info to reconstruct a base tree...
Falling back to patching base and 3-way merge...
Auto-merging README
CONFLICT (content): Merge conflict in README
Failed to merge in the changes.
Patch failed at 0001 my hack 1.

When you have resolved this problem run "git rebase --continue".
If you would prefer to skip this patch, instead run "git rebase --skip".
To restore the original branch and stop rebasing run "git rebase --abort".

rebase refs/remotes/trunk: command returned error: 1

```

果不其然，变基时发生了冲突，这是因为 Subversion 中他人的修改和我们在 Git 库中的修改都改动了同一个文件，并且改动了相近的行。下面按照**git rebase**冲突解决的一般步骤进行，直到成功完成变基操作。

先编辑`README`文件，以解决冲突。

```
$ git status
# Not currently on any branch.
# Unmerged paths:
#   (use "git reset HEAD <file>..." to unstage)
#   (use "git add/rm <file>..." as appropriate to mark resolution)
#
#       both modified:      README
#
no changes added to commit (use "git add" and/or "git commit -a")

$ vi README

```

处于冲突状态的`REAEME`文件内容。

```
<<<<<<< HEAD
HELLO.
=======
hello
I am fine.
>>>>>>> my hack 1.

```

下面是修改后的内容。保存退出。

```
HELLO.
I am fine.

```

执行**git add**命令解决冲突

```
$ git add README

```

调用**git rebase --continue**完成变基操作。

```
$ git rebase --continue
Applying: my hack 1.
Applying: my hack 2.
Using index info to reconstruct a base tree...
Falling back to patching base and 3-way merge...
Auto-merging README

```

看看变基之后的 Git 库日志：

```
$ git log
commit e382f2e99eca07bc3a92ece89f80a7a5457acfd8
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Thu Nov 4 15:05:47 2010 +0800

    my hack 2.

commit 6e7e0c7dccf5a072404a28f06ce0c83d77988b0b
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Thu Nov 4 15:05:32 2010 +0800

    my hack 1.

commit fae6dab863ed2152f71bcb2348d476d47194fdd4
Author: jiangxin <jiangxin@f79726c4-f016-41bd-acd5-6c9acb7664b2>
Date:   Thu Nov 4 07:15:58 2010 +0000

    hello -> HELLO.

    git-svn-id: file:///path/to/svn/repos/demo/trunk@5 f79726c4-f016-41bd-acd5-6c9acb7664b2

commit 1863f91b45def159a3ed2c4c4c9428c25213f956
Author: jiangxin <jiangxin@f79726c4-f016-41bd-acd5-6c9acb7664b2>
Date:   Mon Nov 1 05:49:41 2010 +0000

    hello

    git-svn-id: file:///path/to/svn/repos/demo/trunk@2 f79726c4-f016-41bd-acd5-6c9acb7664b2

commit 2c73d657dfc3a1ceca9d465b0b98f9e123b92bb4
Author: jiangxin <jiangxin@f79726c4-f016-41bd-acd5-6c9acb7664b2>
Date:   Mon Nov 1 05:47:03 2010 +0000

    initialized.

    git-svn-id: file:///path/to/svn/repos/demo/trunk@1 f79726c4-f016-41bd-acd5-6c9acb7664b2

```

当变基操作成功完成后，再执行**git svn dcommit**向 Subversion 推送 Git 库中的两个新提交。

```
$ git svn dcommit
Committing to file:///path/to/svn/repos/demo/trunk ...
        M       README
Committed r6
        M       README
r6 = d0eb86bdfad4720e0a24edc49ec2b52e50473e83 (refs/remotes/trunk)
No changes between current HEAD and refs/remotes/trunk
Resetting to the latest refs/remotes/trunk
Unstaged changes after reset:
M       README
        M       README
Committed r7
        M       README
r7 = 69f4aa56eb96230aedd7c643f65d03b618ccc9e5 (refs/remotes/trunk)
No changes between current HEAD and refs/remotes/trunk
Resetting to the latest refs/remotes/trunk

```

推送之后本地 Git 库中最新的两个提交的提交说明中也嵌入了`git-svn-id:`标签。这个标签的作用非常重要，在下一节会予以介绍。

```
$ git log -2
commit 69f4aa56eb96230aedd7c643f65d03b618ccc9e5
Author: jiangxin <jiangxin@f79726c4-f016-41bd-acd5-6c9acb7664b2>
Date:   Thu Nov 4 07:56:38 2010 +0000

    my hack 2.

    git-svn-id: file:///path/to/svn/repos/demo/trunk@7 f79726c4-f016-41bd-acd5-6c9acb7664b2

commit d0eb86bdfad4720e0a24edc49ec2b52e50473e83
Author: jiangxin <jiangxin@f79726c4-f016-41bd-acd5-6c9acb7664b2>
Date:   Thu Nov 4 07:56:37 2010 +0000

    my hack 1.

    git-svn-id: file:///path/to/svn/repos/demo/trunk@6 f79726c4-f016-41bd-acd5-6c9acb7664b2

```

## 4.7.2\. git-svn 的奥秘

通过上面对 git-svn 的工作流程的介绍，相信读者已经能够体会到 git-svn 的强大。那么 git-svn 是怎么做到的呢？

git-svn 只是在本地 Git 库中增加了一些附加的设置，特殊的引用，和引入附加的可重建的数据库实现对 Subversion 版本库的跟踪。

### 4.7.2.1\. Git 库配置文件的扩展及分支映射

当执行**git svn init**或者**git svn clone**时，git-svn 会通过在 Git 库的配置文件中增加一个小节，记录 Subversion 版本库的 URL，以及 Subversion 分支/里程碑和本地 Git 库的引用之间的对应关系。

例如：当执行**git svn clone -s file:///path/to/svn/repos/demo**指令时，会在创建的本地 Git 库的配置文件`.git/config`中引入下面新的配置：

```
[svn-remote "svn"]
        url = file:///path/to/svn/repos/demo
        fetch = trunk:refs/remotes/trunk
        branches = branches/*:refs/remotes/*
        tags = tags/*:refs/remotes/tags/*

```

缺省`svn-remote`的名字为“svn”，所以新增的配置小节的名字为：`[svn-remote "svn"]`。在 git-svn 克隆时，可以使用`--remote`参数设置不同的`svn-remote`名称，但是并不建议使用。因为一旦使用`--remote`参数更改`svn-remote`名称，必须在`git-svn`的其他命令中都使用`--remote`参数，否则报告`[svn-remote "svn"]`配置小节未找到。

在该小节中主要的配置有：

*   `url = <URL>`

    设置 Subversion 版本库的地址

*   `fetch = <svn-path>:<git-refspec>`

    Subversion 的开发主线和 Git 版本库引用的对应关系。

    在上例中 Subversion 的`trunk`目录对应于 Git 的`refs/remotes/trunk`引用。

*   `branches = <svn-path>:<git-refspec>`

    Subversion 的开发分支和 Git 版本库引用的对应关系。可以包含多条 branches 的设置，以便将分散在不同目录下的分支汇总。

    在上例中 Subversion 的`branches`子目录下一级子目录（`branches/*`）所代表的分支在 Git 的`refs/remotes/`名字空间下建立引用。

*   `tags = <svn-path>:<git-refspec>`

    Subversion 的里程碑和 Git 版本库引用的对应关系。可以包含多条 tags 的设置，以便将分散在不同目录下的里程碑汇总。

    在上例中 Subversion 的 tags 子目录下一级子目录（tags/*）所代表的里程碑在 Git 的`refs/remotes/tags`名字空间下建立引用。

可以看到 Subversion 的主线和分支缺省都直接被映射到`refs/remotes/`下。如`trunk`主线对应于`refs/remotes/trunk`，分支`demo-1.0`对应于`refs/remotes/demo-1.0`。Subversion 的里程碑因为有可能和分支同名，因此被映射到`refs/remotes/tags/`之下，这样就里程碑和分支的映射放到不同目录下，不会互相影响。

### 4.7.2.2\. Git 工作分支和 Subversion 如何对应？

Git 缺省工作的分支是`master`，而看到上例中的 Subversion 主线在 Git 中对应的远程分支为`refs/remotes/trunk`。那么在执行**git svn rebase**时，git-svn 是如何知道当前的 HEAD 对应的分支基于哪个 Subversion 跟踪分支进行变基？还有就是执行**git svn dcommit**时，当前的工作分支应该将改动推送到哪个 Subversion 分支中去呢？

很自然的会按照 Git 的方式进行思考，期望在`.git/config`配置文件中找到类似`[branch master]`之类的配置小节。实际上，在 git-svn 的 Git 库的配置文件中可能根本就不存在`[branch ...]`小节。那么 git-svn 是如何确定当前 Git 工作分支和远程 Subversion 版本库的分支建立对应的呢？

其实奥秘就在 Git 的日志中。当在工作区执行**git log**时，会看到包含`git-svn-id:`标识的特殊日志。发现的最近的一个`git-svn-id:`标识会确定当前分支提交的 Subversion 分支。

下面继续上一节的示例，先切换到分支，并将提交推送到 Subversion 的分支`demo-1.0`中。

首先在 Git 库中会看到有一个对应于 Subversion 分支的远程分支和一个对应于 Subversion 里程碑的远程引用。

```
$ git branch -r
  demo-1.0
  tags/v1.0
  trunk

```

然后基于远程分支`demo-1.0`建立本地工作分支`myhack`。

```
$ git checkout -b myhack refs/remotes/demo-1.0
Switched to a new branch 'myhack'
$ git branch
  master
* myhack

```

在`myhack`分支做一些改动，并提交。

```
$ echo "Git" >> README
$ git add -u
$ git commit -m "say hello to Git."
[myhack d391fd7] say hello to Git.
 1 files changed, 1 insertions(+), 0 deletions(-)

```

下面看看 Git 的提交日志。

```
$ git log --first-parent
commit d391fd75c33f62307c3add1498987fa3eb70238e
Author: Jiang Xin <jiangxin@ossxp.com>
Date:   Fri Nov 5 09:40:21 2010 +0800

    say hello to Git.

commit 1adcd5526976fe2a796d932ff92d6c41b7eedcc4
Author: jiangxin <jiangxin@f79726c4-f016-41bd-acd5-6c9acb7664b2>
Date:   Mon Nov 1 05:54:19 2010 +0000

    new branch: demo-1.0

    git-svn-id: file:///path/to/svn/repos/demo/branches/demo-1.0@3 f79726c4-f016-41bd-acd5-6c9acb7664b2

commit 1863f91b45def159a3ed2c4c4c9428c25213f956
Author: jiangxin <jiangxin@f79726c4-f016-41bd-acd5-6c9acb7664b2>
Date:   Mon Nov 1 05:49:41 2010 +0000

    hello

    git-svn-id: file:///path/to/svn/repos/demo/trunk@2 f79726c4-f016-41bd-acd5-6c9acb7664b2

commit 2c73d657dfc3a1ceca9d465b0b98f9e123b92bb4
Author: jiangxin <jiangxin@f79726c4-f016-41bd-acd5-6c9acb7664b2>
Date:   Mon Nov 1 05:47:03 2010 +0000

    initialized.

    git-svn-id: file:///path/to/svn/repos/demo/trunk@1 f79726c4-f016-41bd-acd5-6c9acb7664b2

```

看到了上述 Git 日志中出现的第一个`git-svn-id:`标识的内容为：

```
git-svn-id: file:///path/to/svn/repos/demo/branches/demo-1.0@3 f79726c4-f016-41bd-acd5-6c9acb7664b2

```

这就是说，当需要将 Git 提交推送给 Subversion 服务器时，需要推送到地址：`file:///path/to/svn/repos/demo/branches/demo-1.0`。

执行**git svn dcommit**，果然是推送到 Subversion 的`demo-1.0`分支。

```
$ git svn dcommit
Committing to file:///path/to/svn/repos/demo/branches/demo-1.0 ...
        M       README
Committed r8
        M       README
r8 = a8b32d1b533d308bef59101c1f2c9a16baf91e48 (refs/remotes/demo-1.0)
No changes between current HEAD and refs/remotes/demo-1.0
Resetting to the latest refs/remotes/demo-1.0

```

### 4.7.2.3\. 其他辅助文件

在 Git 版本库中，git-svn 在`.git/svn`目录下保存了一些索引文件，便于 git-svn 更加快速的执行。

文件`.git/svn/.metadata`文件是类似于`.git/config`文件一样的 INI 文件，其中保存了版本库的 URL，版本库 UUID，分支和里程碑的最后获取的版本号等。

```
; This file is used internally by git-svn
; You should not have to edit it
[svn-remote "svn"]
        reposRoot = file:///path/to/svn/repos/demo
        uuid = f79726c4-f016-41bd-acd5-6c9acb7664b2
        branches-maxRev = 8
        tags-maxRev = 8

```

在`.git/svn/refs/remotes`目录下以各个分支和里程碑为名的各个子目录下都包含一个`.rev_map.<SVN-UUID>`的索引文件，这个文件用于记录 Subversion 的提交 ID 和 Git 的提交 ID 的映射。

目录`.git/svn`的辅助文件由 git-svn 维护，不要手工修改否则会造成 git-svn 不能正常工作。

## 4.7.3\. 多样的 git-svn 克隆模式

在前面的 git-svn 示例中，使用**git svn clone**命令完成对远程版本库的克隆，实际上**git svn clone**相当于两条命令，即：

```
git svn clone = git svn init + git svn fetch

```

命令**git svn init**只完成两个工作。一个是在本地建立一个空的 Git 版本库，另外是修改`.git/config`文件，在其中建立 Subversion 和 Git 之间的分支映射关系。在实际使用中，我更喜欢使用**git svn init**命令，因为这样可以对 Subversion 和 Git 的分支映射进行手工修改。该命令的用法是：

```
用法: git svn init [options] <subversion-url> [local-dir]

可选的主要参数有：

    --stdlayout, -s
    --trunk, -T <arg>
    --branches, --b=s@
    --tags, --t=s@
    --config-dir <arg>
    --ignore-paths <arg>
    --prefix <arg>
    --username <arg>

```

其中`--username`参数用于设定远程 Subversion 服务器认证时提供的用户名。参数`--prefix`用于设置在 Git 的`refs/remotes`下保存引用时使用的前缀。参数`--ignore-paths`后面跟一个正则表达式定义忽略的文件列表，这些文件将不予克隆。

最常用的参数是`-s`。该参数和前面演示的**git clone**命令中的一样，即使用标准的分支/里程碑部署方式克隆 Subversion 版本库。Subversion 约定俗成使用`trunk`目录跟踪主线的开发，使用`branches`目录保存各个分支，使用`tags`目录来记录里程碑。

即命令:

```
$ git svn init -s file:///path/to/svn/repos/demo

```

和下面的命令等效：

```
$ git svn init -T trunk -b branches -t tags file:///path/to/svn/repos/demo

```

有的 Subversion 版本库的分支可能分散于不同的目录下，例如有的位于`branches`目录，有的位于`sandbox`目录，则可以用下面命令：

```
$ git svn init -T trunk -b branches -b sandbox -t tags file:///path/to/svn/repos/demo git-svn-test
Initialized empty Git repository in /path/to/my/workspace/git-svn-test/.git/

```

查看本地克隆版本库的配置文件：

```
$ cat git-svn-test/.git/config
[core]
        repositoryformatversion = 0
        filemode = true
        bare = false
        logallrefupdates = true
[svn-remote "svn"]
        url = file:///path/to/svn/repos/demo
        fetch = trunk:refs/remotes/trunk
        branches = branches/*:refs/remotes/*
        branches = sandbox/*:refs/remotes/*
        tags = tags/*:refs/remotes/tags/*

```

可以看到在`[svn-remote "svn"]`小节中包含了两条`branches`配置，这就会实现将 Subversion 分散于不同目录的分支都克隆出来。如果担心 Subversion 的`branches`目录和`sandbox`目录下出现同名的分支导致在 Git 库的`refs/remotes/`下造成覆盖，可以在版本库尚未执行**git svn fetch**之前编辑`.git/config`文件，避免可能出现的覆盖。例如编辑后的`[svn-remote "svn"]`配置小节：

```
[svn-remote "svn"]
        url = file:///path/to/svn/repos/demo
        fetch = trunk:refs/remotes/trunk
        branches = branches/*:refs/remotes/branches/*
        branches = sandbox/*:refs/remotes/sandbox/*
        tags = tags/*:refs/remotes/tags/*

```

如果项目的分支或里程碑非常多，也可以修改`[svn-remote "svn"]`配置小节中的版本号通配符，使得只获取部分分支或里程碑。例如下面的配置小节：

```
[svn-remote "svn"]
        url = http://server.org/svn
        fetch = trunk/src:refs/remotes/trunk
        branches = branches/{red,green}/src:refs/remotes/branches/*
        tags = tags/{1.0,2.0}/src:refs/remotes/tags/*

```

如果只关心 Subversion 的某个分支甚至某个子目录，而不关心其他分支或目录，那就更简单了，不带参数的执行**git svn init**针对 Subversion 的某个具体路径执行初始化就可以了。

```
$ git svn init file:///path/to/svn/repos/demo/trunk

```

有的情况下，版本库太大，而且对历史不感兴趣，可以只克隆最近的部分提交。这时可以通过**git svn fetch**命令的`-r`参数实现部分提交的克隆。

```
$ git svn init file:///path/to/svn/repos/demo/trunk git-svn-test
Initialized empty Git repository in /path/to/my/workspace/git-svn-test/.git/
$ cd git-svn-test
$ git svn fetch -r 6:HEAD
        A       README
r6 = 053b641b7edd2f1a59a007f27862d98fe5bcda57 (refs/remotes/git-svn)
        M       README
r7 = 75c17ea61d8527334855a51e65ac98c981f545d7 (refs/remotes/git-svn)
Checked out HEAD:
  file:///path/to/svn/repos/demo/trunk r7

```

当然也可以使用**git svn clone**命令实现部分克隆：

```
$ git svn clone -r 6:HEAD \
      file:///path/to/svn/repos/demo/trunk git-svn-test
Initialized empty Git repository in /path/to/my/workspace/git-svn-test/.git/
        A       README
r6 = 053b641b7edd2f1a59a007f27862d98fe5bcda57 (refs/remotes/git-svn)
        M       README
r7 = 75c17ea61d8527334855a51e65ac98c981f545d7 (refs/remotes/git-svn)
Checked out HEAD:
  file:///path/to/svn/repos/demo/trunk r7

```

## 4.7.4\. 共享 git-svn 的克隆库

当一个 Subversion 版本库非常庞大而且和不在同一个局域网内，执行**git svn clone**可能需要花费很多时间。为了避免因重复执行**git svn clone**导致时间上的浪费，可以将一个已经使用 git-svn 克隆出来的 Git 库共享，其他人基于此 Git 进行克隆，然后再用特殊的方法重建和 Subversion 的关联。还记得之前提到过，`.git/svn`目录下的辅助文件可以重建么？

例如通过工作区中已经存在的 git-svn-demo 执行克隆。

```
$ git clone git-svn-demo myclone
Initialized empty Git repository in /path/to/my/workspace/myclone/.git/

```

进入新的克隆中，会发现新的克隆缺乏跟踪 Subversion 分支的引用，即`refs/remotes/trunk`等。

```
$ cd myclone/
$ git br -a
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
  remotes/origin/myhack

```

这是因为 Git 克隆缺省不复制远程版本库的`refs/remotes/`下的引用。可以用**git fetch**命令获取`refs/remotes`的引用。

```
$ git fetch origin refs/remotes/*:refs/remotes/*
From /path/to/my/workspace/git-svn-demo
 * [new branch]      demo-1.0   -> demo-1.0
 * [new branch]      tags/v1.0  -> tags/v1.0
 * [new branch]      trunk      -> trunk

```

现在这个从 git-svn 库中克隆出来的版本库已经有了相同的 Subversion 跟踪分支，但是`.git/config`文件还缺乏相应的`[svn-remote "svn"]`配置。可以通过使用同样的**git svn init**命令实现。

```
$ pwd
/path/to/my/workspace/myclone

$ git svn init -s file:///path/to/svn/repos/demo

$ git config --get-regexp 'svn-remote.*'
svn-remote.svn.url file:///path/to/svn/repos/demo
svn-remote.svn.fetch trunk:refs/remotes/trunk
svn-remote.svn.branches branches/*:refs/remotes/*
svn-remote.svn.tags tags/*:refs/remotes/tags/*

```

但是克隆版本库相比用 git-svn 克隆的版本库还缺乏`.git/svn`下的辅助文件。实际上可以用**git svn rebase**命令重建，同时这条命令也可以变基到 Subversion 相应分支的最新提交上。

```
$ git svn rebase
Rebuilding .git/svn/refs/remotes/trunk/.rev_map.f79726c4-f016-41bd-acd5-6c9acb7664b2 ...
r1 = 2c73d657dfc3a1ceca9d465b0b98f9e123b92bb4
r2 = 1863f91b45def159a3ed2c4c4c9428c25213f956
r5 = fae6dab863ed2152f71bcb2348d476d47194fdd4
r6 = d0eb86bdfad4720e0a24edc49ec2b52e50473e83
r7 = 69f4aa56eb96230aedd7c643f65d03b618ccc9e5
Done rebuilding .git/svn/refs/remotes/trunk/.rev_map.f79726c4-f016-41bd-acd5-6c9acb7664b2
Current branch master is up to date.

```

如果执行**git svn fetch**则会对所有的分支都进行重建。

```
$ git svn fetch
Rebuilding .git/svn/refs/remotes/demo-1.0/.rev_map.f79726c4-f016-41bd-acd5-6c9acb7664b2 ...
r3 = 1adcd5526976fe2a796d932ff92d6c41b7eedcc4
r8 = a8b32d1b533d308bef59101c1f2c9a16baf91e48
Done rebuilding .git/svn/refs/remotes/demo-1.0/.rev_map.f79726c4-f016-41bd-acd5-6c9acb7664b2
Rebuilding .git/svn/refs/remotes/tags/v1.0/.rev_map.f79726c4-f016-41bd-acd5-6c9acb7664b2 ...
r4 = c12aa40c494b495a846e73ab5a3c787ca1ad81e9
Done rebuilding .git/svn/refs/remotes/tags/v1.0/.rev_map.f79726c4-f016-41bd-acd5-6c9acb7664b2

```

至此，从 git-svn 克隆库二次克隆的 Git 库，已经和原生的 git-svn 库一样使用 git-svn 命令了。

## 4.7.5\. git-svn 的局限

Subversion 和 Git 的分支实现有着巨大的不同。Subversion 的分支和里程碑，是用轻量级拷贝实现的，虽然创建分支和里程碑的速度也很快，但是很难维护。即使 Subversion 在 1.5 之后引入了`svn:mergeinfo`属性对合并过程进行标记，但是也不可能让 Subversion 的分支逻辑更清晰。git-svn 无须利用`svn:mergeinfo`属性也可实现对 Subversion 合并的追踪，在合并的时候也不会对 svn:mergeinfo 属性进行更改，因此在使用 git-svn 操作时，如果在不同分支间进行合并，会导致 Subversion 的 svn:mergeinfo 属性没有相应的更新，导致 Subversion 用户进行合并时因为重复合并导致冲突。

简而言之，在使用 git-svn 时尽量不要在不同的分支之间进行合并，而是尽量在一个分支下线性的提交。这种线性的提交会很好的推送到 Subversion 服务器中。

如果真的需要在不同的 Subversion 分支之间合并，尽量使用 Subversion 的客户端（svn 1.5 版本或以上）执行，因为这样可以正确的记录 svn:mergeinfo 属性。当 Subversion 完成分支合并后，在 git-svn 的克隆库中执行**git svn rebase**命令获取最新的 Subversion 提交并变基到相应的跟踪分支中。

© Copyright 2011, 蒋鑫. Created using [Sphinx](http://sphinx-doc.org/) 1.3.4.