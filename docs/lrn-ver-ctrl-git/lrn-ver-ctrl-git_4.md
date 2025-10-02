# Part 3 - 远程仓库

# 关于远程仓库

在版本控制系统上，大约 90%的操作都是在本地仓库（local repository）中进行的：暂存，提交，查看状态或者历史记录等等。除此之外，如果仅仅只有你一个人在这个项目里工作，你永远没有机会需要设置一个远程仓库（remote repository）。

只有当你需要和你的开发团队**共享数据**时，设置一个远程仓库才有意义。你可以把它想象成一个 “文件管理服务器”，利用这个服务器可以与开发团队的其他成员进行数据交换。

现在让我们来看一下本地仓库与远程仓库之间的区别吧：

### 位置

本地仓库位于每一个团队成员的本地计算机上。相反，远程仓库则被设置在一个能被所有团队成员访问到的远程服务器上，基本上都是在互联网上或者是本地局域网中。

### 特点

从技术上讲，一个远程仓库和一个本地仓库并不存在很大差异。和本地仓库一样，它包含分支，提交和标记。然而，本地存储库存在一个与之相关的工作副本（working copy），就是你当前的工作目录。其中包括了你从项目中签出的一些版本文件。而一个远程仓库就不包含任何工作目录，它仅仅是由一系列 “.git” 目录组成的。

### 创建

你有两种方法来创建一个本地仓库到你的本地计算机上，你可以新建一个空的仓库，或者是克隆一个已存在的远程仓库到你的本地计算机中。 创建一个新的远程仓库同样的也有两种方法。方法一，假如已经存在了一个本地仓库，并且你想以这个本地仓库作为远程仓库的基准，你使用 “--bare” 参数来可以克隆这个本地仓库。方法二，假如你想要新建一个空的远程仓库，你可以使用 “git init” 命令，并且同时也要加上 “--bare” 参数。

## 本地 / 远程工作流程

在 Git 中，仅仅存在一小部分应用在远程仓库上的命令。

绝大多数工作都是针对本地存仓库的。到现在为止（除去克隆命令 “git clone” 外），我们一直只是工作在我们本地仓库上，并且从没有离开过本地计算机。我们不依赖于任何互联网或局域网连接，完全是脱机工作的。

我们将会在本章的之后小节中来一起学习一些远程仓库上的命令。

![](img/basic-remote-workflow.png)

# 连接一个远程仓库

当你克隆一个远程仓库的同时，Git 会自动为你的记录下它的链接。默认使用这个名字 “origin” 来标识你所克隆的原始仓库。 如果你是直接在计算机上创建了一个本地仓库，这样就没有任何一个远程链接被记录下来。这种情况下，当你尝试做任何远程操作之前你就必须先把它连接到一个远程仓库上去：

```
$ git remote add crash-course-remote
    https://github.com/gittower/git-crash-course-remote.git 
```

来让我们来看看它的结果：

```
$ git remote -v
crash-course-remote   https://github.com/gittower/git-crash-course-remote.git (fetch)
crash-course-remote   https://github.com/gittower/git-crash-course-remote.git (push)
origin   https://github.com/gittower/git-crash-course (fetch)
origin   https://github.com/gittower/git-crash-course (push) 
```

请注意，每个远程仓库包含两行，第一个是用来进行抓取的 “fetch URL”，第二个是用来把本地仓库中的数据推送到远程仓库 “push URL”。很多情况下这两个 URLs 都是相同的。然而你当然也可以对抓取（fetch）和推送（push）使用两个不同的 URLs（例如出于安全和性能方面的考虑）。

此外还要注意到，你可以对一个本地仓库设置很多个远程链接，这是没有数量限制的。在上面的例子中你已经看到了一个已经存在的链接 “origin” ，其实我们从来也没有设置过它！在完成某个远程仓库的克隆之后，Git 会默认的建立一个名为 “origin” 的远程仓库链接（还记得吗？我们曾在本书的开始部分做过这个操作）。和被命名为 “master” 的分支一样的道理，“origin” 这个名字是默认的。它和其他的远程仓库并没有什么区别。

# 查看远程数据

在我们新建了与某个远程仓库的连接之后，它到底改变了什么？现在让我们来看看分支列表：

```
$ git branch -va
  contact-form           56eddd1 Add new contact form page
* master                 56eddd1 Add new contact form page
  remotes/origin/HEAD    -> origin/master
  remotes/origin/master  2b504be Change headlines for about and imprint 
```

显然并没有太大的变化，始终是我们那两个本地分支（“master” 和 “contact-form”）以及两个在 “origin” 上的远程分支（“remotes/origin/HEAD” 和 “remotes/origin/master”）。为什么我们没有看到那个新的远程链接 “crash-course-remote” 呢？因为通过命令 “git remote add”，我们仅仅建立了一种关系，还没有进行任何数据交换。

##### 概念

#### 远程数据是一个快照（Snapshot）

Git 会在你的本地仓库中保存远程数据的信息（例如分支，提交等等）。但是它并不是 “实时地” 连接到你的远程仓库上的。例如，其他团队成员在这个远程仓库中所提交的新改动或是发布的分支，是不能自动地与你分享的，因为你必须**明确地**告诉 Git 去升级你的本地仓库！

关于远程的分支，远程的提交等等信息只会按照你的要求更新到最新的一个快照。Git 不会在后台 “自动” 升级这些信息。

要更新有关远程的信息，你必须明确地请求这个数据。在这里可以使用最为常用的，“抓取（Fetch）” 操作来完成：

```
$ git fetch crash-course-remote
From https://github.com/gittower/git-crash-course-remote
 * [new branch]     faq-content -> crash-course-remote/faq-content
 * [new branch]     master -> crash-course-remote/master 
```

“抓取” 操作不会改动你任何的本地分支或是在你工作副本中的文件。这个操作仅仅为你从一个特定远程仓库下载你所需要的数据，并设置为可见。你可以在之后决定是否需要整合这些新的改动本地项目中来。

在我们完成了关于 “crash-course-remote” 升级后，让我们一起来看一下现在发生了什么变化：

```
$ git branch -va
  contact-form           56eddd1 Add new contact form page
* master                 56eddd1 Add new contact form page
  remotes/crash-course-remote/faq-content  e29fb3f Add FAQ questions
  remotes/crash-course-remote/master       2b504be Change headlines f...
  remotes/origin/HEAD    -> origin/master
  remotes/origin/master  2b504be Change headlines for about and imprint 
```

很好，我们看到了这个远程分支的 “crash-course-remote” 的信息了。

现在准备开始在分支 “faq-content” 上工作吧！但是现在它只是一个远程分支的指针。为了能够真正地在这个分支上工作，并且切换当前工作副本（working copy）的内容，我们需要创建一个基于这个远程的本地分支。执行 “git checkout” 命令来切换到这个远程分支：

```
$ git checkout --track crash-course-remote/faq-content
Branch faq-content set up to track remote branch faq-content from crash-course-remote.
Switched to a new branch 'faq-content'

$ git branch -va
  contact-form           56eddd1 Add new contact form page
* faq-content            e29fb3f Add FAQ questions
  master                 56eddd1 Add new contact form page
  remotes/crash-course-remote/faq-content  e29fb3f Add FAQ questions
  remotes/crash-course-remote/master       2b504be Change headlines f...
  remotes/origin/HEAD    -> origin/master
  remotes/origin/master  2b504be Change headlines for about and imprint 
```

这个命令完成了一系列的操作：

*   （a） 它创建了一个同名的本地分支（“faq-content”）。
*   （b） 它签出（check out）了这个新建的的分支，把它设置成当前的本地 HEAD ，然后更新了你的工作副本，并且关联到分支文件的最新版本上去。
*   （c） 由于我们使用了 “--track” 参数，它会在新的本地分支和它所位于的远程分支之间创建一个跟踪联系 “tracking relationship”。

#### 跟踪分支

一般来说，分支之间并无任何关系。然而我们可以定义一个本地分支去 “跟踪 （track）” 一个远程分支。这样 Git 就会通知你，如果那个被跟踪的远程分支发生了一些新的提交，而它们并不存在于这个关联的本地分支中时：

*   如果在你的本地分支上提交了一些改动，而且你也并没有发布它和推送到远程仓库中。相对于这些提交来说你的本地分支就 “**领先（ahead）**” 于那些它所对应的远程分支。
*   如果团队的其他开发人员提交并且发布了一些改动到远程仓库中，这时远程仓库就拥有了那些你还没有下载到本地仓库的提交。你的本地仓库就 “**落后（behind）**” 于它所关联的远程仓库。

![tracking-ahead-behind](img/tracking-ahead-behind.gif)

如果分支间存在 “跟踪” 联系，当你使用 “git status” 命令时，Git 显示出所有关联分支上的差异：

```
$ git status
# On branch dev
# Your branch and 'origin/dev' have diverged,
# and have 1 and 2 different commits each, respectively.
#
nothing to commit (working directory clean) 
```

当在一个已存在的远程分支的基础上来建立本地分支时，创建这个 “跟踪” 联系是很简单的，可以使用 “git checkout” 命令加 “--track” 参数来完成。

在切换到那个新创建的本地分支 “faq-content” 后， 我们已经自动拥有了这个 “跟踪” 联系。来让我们对这个工作副本中的文件 “faq.html” 进行一些修改吧！（如何更改这个文件就不详细介绍了，发挥你的想象力吧）：

```
$ git status
# On branch faq-content
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working 
#    directory)
#
#       modified:   faq.html
#
no changes added to commit (use "git add" and/or "git commit -a")

$ git add faq.html

$ git commit -m "Add new question"
[faq-content 814927a] Add new question
 1 file changed, 1 insertion(+) 
```

现在，是时候把这些新的改动共享给其他开发人员了：

```
$ git push 
```

##### 注释

在你的本地计算机上，这个 “git push” 命令将会被远程仓库**拒绝**掉，因为你现在还没有获得这个远程仓库的修改权限。如果你想要继续的尝试这个操作，我建议你建立一个自己的远程仓库，例如：[GitHub](http://www.github.com) 或者 [Beanstalk](http://beanstalkapp.com/pricing?ad=tower)。

“git push” 命令将会把当前 HEAD 分支上所有新的提交上传到它所关联的远程分支上去。

##### 概念

#### 回顾 “跟踪” 联系

“git push” 命令默认地要求我们为它提供两个信息：

*   （a） 你想要推送到哪一个远程仓库上去？
*   （b） 你想要推送到那个远程仓库上的哪一个分支上去？

这个完整的命令应该是这样的： `$ git push crash-course-remote faq-content` 我们已经设置了 “跟踪” 联系，也就是说我们已经为那个本地分支定义了一个 “ 远程对应的（remote counterpart）” 分支。在我们使用 “git push” and “git pull” 命令时，我们不需要特别地给出那些参数， Git 会自动地使用这些已经定义好的 “跟踪” 信息。

# 整合远程的改动

开发团队的其他成员在你们共同的远程仓库上共享了他的改动，在这些改动被整合到你的本地副本之前，你需要首先要检查一下这些改动：

```
$ git fetch origin
$ git log origin/master 
```

##### 注释

因为在大部分的项目中你都会拥有一个名为 “origin” 的远程链接，所以我们也将在本书中使用这个远程链接的名称作为例子。

现在使用 “git log” 命令来查看最近在 “origin” 的 “master” 分支上所有改动。 如果你决定要整合这些改动到你的本地副本中来， 你就可以使用 “git pull” 命名来完成这个操作：

```
$ git pull 
```

这个命令将会从远程分支下载所有的新的提交到你的本地副本中来。它实际上就是一个 “抓取（fetch）” 命令（下载数据） 和 一个 “ 合并（merge）” 命令（整合那些下载的数据到你的本地副本）的组合。

和 “git push” 命令一样，如果你本地的 HEAD 分支还没有创建任何一个 “跟踪” 链接，你就必须告诉 Git，你要从哪一个远程仓库上的哪一分支中抓取数据（例如 “git pull origin master”）。如果已经存在了一个链接，只需要简单键入 “git pull” 就足够了.

整合的目标并不基于存在什么样的跟踪链接，它总是会被整合到你的本地 HEAD 分支中，也就是你的工作副本。

# 发布一个本地分支

在你明确地决定将一个本地分支发布到远程仓库之前，这些在你本地计算机上创建的分支是不能被其他的团队成员看到的，它只是你的私有分支。这就意味着，你可以保留某些改动仅仅在你私有的本地分支上，而与其他团队成员分享一些其它分支上的改动。

现在让我们来分享 “contact-form” 分支（它直到现在还仅仅是个私有的本地分支）到 “origin” 远程上：

```
$ git checkout contact-form
Switched to branch 'contact-form'

$ git push -u origin contact-form
Counting objects: 36, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (31/31), done.
Writing objects: 100% (36/36), 90.67 KiB, done.
Total 36 (delta 12), reused 0 (delta 0)
Unpacking objects: 100% (36/36), done.
To file://Users/tobidobi/Desktop/GitCrashkurs/remote-test.git
 * [new branch]    contact-form -> contact-form
Branch contact-form set up to track remote branch contact-form from origin. 
```

这个命令会告诉 Git 来发布你当前本地的 HEAD 分支到 “origin”上，并命名为 “contact-form”（保持相同的分支名在本地和其对应的远程分支是非常有必要的）。 这个 “-u” 参数会自动地在你本地的 “contact-form” 分支和新建的远程分支之间创建一个 “跟踪” 链接。执行 “git branch” 命令来显示分支信息，并且附带上一些特定的参数，在方括号中就会显示出这个建立的跟踪联系：

```
$ git branch -vva
* contact-form           56eddd1 [origin/contact-form] Add new contact..
  faq-content            814927a [crash-course-remote/faq-content: ahead
                                    1] Add new question
  master                 2dfe283 Implement the new login box
  remotes/crash-course-remote/faq-content e29fb3f Add FAQ questions
  remotes/crash-course-remote/master      2b504be Change headlines f...
  remotes/origin/contact-form             56eddd1 Add new contact fo...
  remotes/origin/master  56eddd1 Add new contact form page 
```

当创建了这个新的远程分支后，发布新的本地提交将会非常简单，执行 “git push” 命令就完成这个操作。

如果某个开发人员拥有对这个远程仓库的操作权限，而且他想在这个你发布的 “contact-form” 上工作，他可以在自己的本地计算机上新建一个本地分支，并跟踪到这个远程分支上。这样他也就同样可以提交自己的改动到 “contact-form” 上了。

# 删除分支

假设我们在 “contact-form” 分支上的工作已经完成了。并且我们也已经把最终的改动整合到了 “master” 分支。现在我们就不再需要这个分支了。把它删除掉吧：

```
$ git branch -d contact-form 
```

为了保持一致，我们也有必要删除它所对应的远程分支。附加上一个 “-r” 参数就可以了：

```
$ git branch -dr origin/contact-form 
```