# 3\. 基本用法

# 获得一个 Git 仓库

既然我们现在把一切都设置好了，那么我们需要一个 Git 仓库。有两种方法可以得到它：一种是从已有的 Git 仓库中　*clone* (克隆，复制)；还有一种是新建一个仓库，把未进行版本控制的文件进行版本控制。

## Clone 一个仓库

为了得一个项目的拷贝(copy),我们需要知道这个项目仓库的地址(Git URL). Git 能在许多协议下使用，所以 Git URL 可能以 ssh://, http(s)://, git://,或是只是以一个用户名（git 会认为这是一个 ssh 地址）为前辍. 有些仓库可以通过不只一种协议来访问，例如，Git 本身的源代码你既可以用 git:// 协议来访问：

```
git clone git://git.kernel.org/pub/scm/git/git.git

```

也可以通过 http 协议来访问:

```
git clone http://www.kernel.org/pub/scm/git/git.git

```

git://协议较为快速和有效,但是有时必须使用 http 协议,比如你公司的防火墙阻止了你的非 http 访问请求.如果你执行了上面两行命令中的任意一个,你会看到一个新目录: 'git',它包含所有的 Git 源代码和历史记录.

在默认情况下，Git 会把"Git URL"里目录名的'.git'的后辍去掉,做为新克隆(clone)项目的目录名: (例如. *git clone [`git.kernel.org/linux/kernel/git/torvalds/linux-2.6.git`](http://git.kernel.org/linux/kernel/git/torvalds/linux-2.6.git)* 会建立一个目录叫'linux-2.6')

## 初始化一个新的仓库

现在假设有一个叫”project.tar.gz”的压缩文件里包含了你的一些文件，你可以用下面的命令让它置于 Git 的版本控制管理之下.

```
$ tar xzf project.tar.gz
$ cd project
$ git init

```

Git 会输出:

```
Initialized empty Git repository in .git/

```

如果你仔细观查会发现 project 目录下会有一个名叫”.git” 的目录被创建，这意味着一个仓库被初始化了。

# 正常的工作流程

修改文件，将它们更新的内容添加到索引中。

```
$ git add file1 file2 file3

```

你现在为 commit 做好了准备，你可以使用 [git diff](http://www.kernel.org/pub/software/scm/git/docs/git-diff.html) 命令再加上 --cached 参数 ,看看哪些文件将被提交(commit)。

```
$ git diff --cached

```

(如果没有--cached 参数，git diff 会显示当前你所有已做的但没有加入到索引里的修改.) 你也可以用 git status 命令来获得当前项目的一个状况:

```
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#   modified:   file1
#   modified:   file2
#   modified:   file3
#

```

如果你要做进一步的修改, 那就继续做, 做完后就把新修改的文件加入到索引中. 最后把他们提交：

```
$ git commit

```

这会提示你输入本次修改的注释，完成后就会记录一个新的项目版本。

除了用`git add` 命令，我还可以用

```
$ git commit -a

```

这会自动把所有内容被修改的文件(不包括新创建的文件)都添加到索引中，并且同时把它们提交。

这里有一个关于写 commit 注释的技巧和大家分享:commit 注释最好以一行短句子作为开头，来简要描述一下这次 commit 所作的修改(最好不要超过 50 个字符)；然后空一行再把详细的注释写清楚。这样就可以很方便的用工具把 commit 注释变成 email 通知，第一行作为标题，剩下的部分就作 email 的正文.

### Git 跟踪的是内容不是文件

很多版本控制系统都提供了一个 "add" 命令：告诉系统开始去跟踪某一个文件的改动。但是 Git 里的 ”add” 命令从某种程度上讲更为简单和强大. `git add` 不但是用来添加不在版本控制中的新文件，也用于添加已在版本控制中但是刚修改过的文件; 在这两种情况下, Git 都会获得当前文件的快照并且把内容暂存(stage)到索引中，为下一次 commit 做好准备。

# 分支与合并@基础

一个 Git 仓库可以维护很多开发分支。现在我们来创建一个新的叫”experimental”的分支：

```
$ git branch experimental

```

如果你运行下面这条命令：

```
$ git branch

```

你会得到当前仓库中存在的所有分支列表：

```
  experimental
* master

```

“experimental” 分支是你刚才创建的，“master”分支是 Git 系统默认创建的主分支。星号(“*”)标识了你当工作在哪个分支下，输入：

```
$ git checkout experimental

```

切换到”experimental”分支，先编辑里面的一个文件，再提交(commit)改动，最后切换回 “master”分支。

```
(edit file)
$ git commit -a
$ git checkout master

```

你现在可以看一下你原来在“experimental”分支下所作的修改还在不在；因为你现在切换回了“master”分支，所以原来那些修改就不存在了。

你现在可以在“master”分支下再作一些不同的修改:

```
(edit file)
$ git commit -a

```

这时，两个分支就有了各自不同的修改(diverged)；我们可以通过下面的命令来合并“experimental”和“master”两个分支:

```
$ git merge experimental

```

如果这个两个分支间的修改没有冲突(conflict), 那么合并就完成了。如有有冲突，输入下面的命令就可以查看当前有哪些文件产生了冲突:

```
$ git diff

```

当你编辑了有冲突的文件，解决了冲突后就可以提交了：

```
$ git commit -a

```

提交(commit)了合并的内容后就可查看一下:

```
$ gitk

```

执行了 gitk 后会有一个很漂亮的图形的显示项目的历史。

这时你就可以删除掉你的 “experimental” 分支了(如果愿意)：

```
$ git branch -d experimental

```

git branch -d 只能删除那些已经被当前分支的合并的分支. 如果你要强制删除某个分支的话就用 git branch –D；下面假设你要强制删除一个叫”crazy-idea”的分支：

```
$ git branch -D crazy-idea

```

分支是很轻量级且容易的，这样就很容易来尝试它。

## 如何合并

你可以用下面的命令来合并两个分离的分支：[git merge](http://www.kernel.org/pub/software/scm/git/docs/git-merge.html):

```
$ git merge branchname

```

这个命令把分支"branchname"合并到了当前分支里面。如有冲突(冲突--同一个文件在远程分支和本地分支里按不同的方式被修改了）；那么命令的执行输出就像下面一样

```
$ git merge next
 100% (4/4) done
Auto-merged file.txt
CONFLICT (content): Merge conflict in file.txt
Automatic merge failed; fix conflicts and then commit the result.

```

在有问题的文件上会有冲突标记，在你手动解决完冲突后就可以把此文件添 加到索引(index)中去，用 git commit 命令来提交，就像平时修改了一个文件 一样。

如果你用 gitk 来查看 commit 的结果，你会看到它有两个父分支：一个指向当前 的分支，另外一个指向刚才合并进来的分支。

## 解决合并中的冲突

如果执行自动合并没有成功的话，git 会在索引和工作树里设置一个特殊的状态， 提示你如何解决合并中出现的冲突。

有冲突(conflicts)的文件会保存在索引中，除非你解决了问题了并且更新了索引，否则执行 [git commit](http://www.kernel.org/pub/software/scm/git/docs/git-commit.html)都会失败:

```
$ git commit
file.txt: needs merge

```

如果执行 [git status](http://www.kernel.org/pub/software/scm/git/docs/git-status.html) 会显示这些文件没有合并(unmerged),这些有冲突的文件里面会添加像下面的冲突标识符:

```
<<<<<<< HEAD:file.txt
Hello world
=======
Goodbye
>>>>>>> 77976da35a11db4580b80ae27e8d65caf5208086:file.txt

```

你所需要的做是就是编辑解决冲突，（接着把冲突标识符删掉），再执行下面的命令:

```
$ git add file.txt
$ git commit

```

注意：提交注释里已经有一些关于合并的信息了，通常是用这些默认信息，但是你可以添加一些你想要的注释。

上面这些就是你要做一个简单合并所要知道的，但是 git 提供更多的一些信息来 帮助解决冲突。

## 撒销一个合并

如果你觉得你合并后的状态是一团乱麻，想把当前的修改都放弃，你可以用下面的命令回到合并之前的状态：

```
$ git reset --hard HEAD

```

或者你已经把合并后的代码提交，但还是想把它们撒销：

```
$ git reset --hard ORIG_HEAD

```

但是刚才这条命令在某些情况会很危险，如果你把一个已经被另一个分支合并的分支给删了，那么 以后在合并相关的分支时会出错。

## 快速向前合并

还有一种需要特殊对待的情况，在前面没有提到。通常，一个合并会产生一个合并提交(commit), 把两个父分支里的每一行内容都合并进来。

但是，如果当前的分支和另一个分支没有内容上的差异，就是说当前分支的每一个提交(commit)都已经存在另一个分支里了，git 就会执行一个“快速向前"(fast forward)操作；git 不创建任何新的提交(commit),只是将当前分支指向合并进来的分支。

# 查看历史 －Git 日志

[git log](http://www.kernel.org/pub/software/scm/git/docs/git-log.html)命令可以显示所有的提交(commit)。 ......

```
$ git log v2.5..        # commits since (not reachable from) v2.5
$ git log test..master  # commits reachable from master but not test
$ git log master..test  # commits reachable from test but not master
$ git log master...test # commits reachable from either test or
                        #    master, but not both
$ git log --since="2 weeks ago" # commits from the last 2 weeks
$ git log Makefile      # commits that modify Makefile
$ git log fs/           # commits that modify any file under fs/
$ git log -S'foo()'     # commits that add or remove any file data
                        # matching the string 'foo()'
$ git log --no-merges   # dont show merge commits

```

当然你也可以组合上面的命令选项；下面的命令就是找出所有从"v2.5“开 始在 fs 目录下的所有 Makefile 的修改.

```
$ git log v2.5.. Makefile fs/

```

Git 会根据 git log 命令的参数，按时间顺序显示相关的提交(commit)。

```
commit f491239170cb1463c7c3cd970862d6de636ba787
Author: Matt McCutchen <matt@mattmccutchen.net>
Date:   Thu Aug 14 13:37:41 2008 -0400

    git format-patch documentation: clarify what --cover-letter does

commit 7950659dc9ef7f2b50b18010622299c508bfdfc3
Author: Eric Raible <raible@gmail.com>
Date:   Thu Aug 14 10:12:54 2008 -0700

    bash completion: 'git apply' should use 'fix' not 'strip'
    Bring completion up to date with the man page.

```

你也可以让 git log 显示补丁(patchs):

```
$ git log -p

commit da9973c6f9600d90e64aac647f3ed22dfd692f70
Author: Robert Schiele <rschiele@gmail.com>
Date:   Mon Aug 18 16:17:04 2008 +0200

    adapt git-cvsserver manpage to dash-free syntax

diff --git a/Documentation/git-cvsserver.txt b/Documentation/git-cvsserver.txt
index c2d3c90..785779e 100644
--- a/Documentation/git-cvsserver.txt
+++ b/Documentation/git-cvsserver.txt
@@ -11,7 +11,7 @@ SYNOPSIS
 SSH:

 [verse]
-export CVS_SERVER=git-cvsserver
+export CVS_SERVER="git cvsserver"
 'cvs' -d :ext:user@server/path/repo.git co <HEAD_name>

 pserver (/etc/inetd.conf):

```

## 日志统计

如果用`--stat`选项使用'git log',它会显示在每个提交(commit)中哪些文件被修改了, 这些文件分别添加或删除了多少行内容.

```
$ git log --stat

commit dba9194a49452b5f093b96872e19c91b50e526aa
Author: Junio C Hamano <gitster@pobox.com>
Date:   Sun Aug 17 15:44:11 2008 -0700

    Start 1.6.0.X maintenance series

 Documentation/RelNotes-1.6.0.1.txt |   15 +++++++++++++++
 RelNotes                           |    2 +-
 2 files changed, 16 insertions(+), 1 deletions(-)

```

## 格式化日志

你可以按你的要求来格式化日志输出。‘--pretty'参数可以使用若干表现格式，如‘oneline':

```
$ git log --pretty=oneline
a6b444f570558a5f31ab508dc2a24dc34773825f dammit, this is the second time this has reverted
49d77f72783e4e9f12d1bbcacc45e7a15c800240 modified index to create refs/heads if it is not 
9764edd90cf9a423c9698a2f1e814f16f0111238 Add diff-lcs dependency
e1ba1e3ca83d53a2f16b39c453fad33380f8d1cc Add dependency for Open4
0f87b4d9020fff756c18323106b3fd4e2f422135 merged recent changes: * accepts relative alt pat
f0ce7d5979dfb0f415799d086e14a8d2f9653300 updated the Manifest file

```

或者你也可以使用 'short' 格式:

```
$ git log --pretty=short
commit a6b444f570558a5f31ab508dc2a24dc34773825f
Author: Scott Chacon <schacon@gmail.com>

    dammit, this is the second time this has reverted

commit 49d77f72783e4e9f12d1bbcacc45e7a15c800240
Author: Scott Chacon <schacon@gmail.com>

    modified index to create refs/heads if it is not there

commit 9764edd90cf9a423c9698a2f1e814f16f0111238
Author: Hans Engel <engel@engel.uk.to>

    Add diff-lcs dependency

```

你也可用‘medium','full','fuller','email' 或‘raw'. 如果这些格式不完全符合你的相求， 你也可以用‘--pretty=format'参数(参见：[git log](http://www.kernel.org/pub/software/scm/git/docs/git-log.html))来创建你自己的"格式“.

```
$ git log --pretty=format:'%h was %an, %ar, message: %s'
a6b444f was Scott Chacon, 5 days ago, message: dammit, this is the second time this has re
49d77f7 was Scott Chacon, 8 days ago, message: modified index to create refs/heads if it i
9764edd was Hans Engel, 11 days ago, message: Add diff-lcs dependency
e1ba1e3 was Hans Engel, 11 days ago, message: Add dependency for Open4
0f87b4d was Scott Chacon, 12 days ago, message: merged recent changes:

```

另一个有趣的事是：你可以用'--graph'选项来可视化你的提交图(commit graph),就像下面这样:

```
$ git log --pretty=format:'%h : %s' --graph
* 2d3acf9 : ignore errors from SIGCHLD on trap
*   5e3ee11 : Merge branch 'master' of git://github.com/dustin/grit
|\  
| * 420eac9 : Added a method for getting the current branch.
* | 30e367c : timeout code and tests
* | 5a09431 : add timeout protection to grit
* | e1193f8 : support for heads with slashes in them
|/  
* d6016bc : require time for xmlschema

```

它会用 ASCII 字符来画出一个很漂亮的提交历史(commit history)线。

## 日志排序

你也可以把日志记录按一些不同的顺序来显示。注意，git 日志从最近的提交(commit)开始，并且从这里开始向它们父分支回溯。然而 git 历史可能包括多个互不关联的开发线路，这样有时提交(commits)显示出来就有点杂乱。

如果你要指定一个特定的顺序，可以为 git log 命令添加顺序参数(ordering option).

按默认情况，提交(commits)会按逆时间(reverse chronological)顺序显示。

但是你也可以指定‘--topo-order'参数，这就会让提交(commits)按拓朴顺序来显示(就是子提交在它们的父提交前显示). 如果你用 git log 命令按拓朴顺序来显示 git 仓库的提交日志，你会看到“开发线"(development lines)都会集合在一起.

```
$ git log --pretty=format:'%h : %s' --topo-order --graph
*   4a904d7 : Merge branch 'idx2'
|\  
| *   dfeffce : merged in bryces changes and fixed some testing issues
| |\  
| | * 23f4ecf : Clarify how to get a full count out of Repo#commits
| | *   9d6d250 : Appropriate time-zone test fix from halorgium
| | |\  
| | | * cec36f7 : Fix the to_hash test to run in US/Pacific time
| | * | decfe7b : fixed manifest and grit.rb to make correct gemspec
| | * | cd27d57 : added lib/grit/commit_stats.rb to the big list o' files
| | * | 823a9d9 : cleared out errors by adding in Grit::Git#run method
| | * |   4eb3bf0 : resolved merge conflicts, hopefully amicably
| | |\ \  
| | | * | d065e76 : empty commit to push project to runcoderun
| | | * | 3fa3284 : whitespace
| | | * | d01cffd : whitespace
| | | * | 7c74272 : oops, update version here too
| | | * | 13f8cc3 : push 0.8.3
| | | * | 06bae5a : capture stderr and log it if debug is true when running commands
| | | * | 0b5bedf : update history
| | | * | d40e1f0 : some docs
| | | * | ef8a23c : update gemspec to include the newly added files to manifest
| | | * | 15dd347 : add missing files to manifest; add grit test
| | | * | 3dabb6a : allow sending debug messages to a user defined logger if provided; tes
| | | * | eac1c37 : pull out the date in this assertion and compare as xmlschemaw, to avoi
| | | * | 0a7d387 : Removed debug print.
| | | * | 4d6b69c : Fixed to close opened file description.

```

你也可以用'--date-order'参数，这样显示提交日志的顺序主要按提交日期来排序. 这个参数和'--topo-order'有一点像，没有父分支会在它们的子分支前显示，但是其它的东东还是按交时间来排序显示。你会看到"开发线"(development lines)没有集合一起，它们会像并行开发(parallel development)一样跳来跳去的：

```
$ git log --pretty=format:'%h : %s' --date-order --graph
*   4a904d7 : Merge branch 'idx2'
|\  
* | 81a3e0d : updated packfile code to recognize index v2
| *   dfeffce : merged in bryces changes and fixed some testing issues
| |\  
| * | c615d80 : fixed a log issue
|/ /  
| * 23f4ecf : Clarify how to get a full count out of Repo#commits
| *   9d6d250 : Appropriate time-zone test fix from halorgium
| |\  
| * | decfe7b : fixed manifest and grit.rb to make correct gemspec
| * | cd27d57 : added lib/grit/commit_stats.rb to the big list o' file
| * | 823a9d9 : cleared out errors by adding in Grit::Git#run method
| * |   4eb3bf0 : resolved merge conflicts, hopefully amicably
| |\ \  
| * | | ba23640 : Fix CommitDb errors in test (was this the right fix?
| * | | 4d8873e : test_commit no longer fails if you're not in PDT
| * | | b3285ad : Use the appropriate method to find a first occurrenc
| * | | 44dda6c : more cleanly accept separate options for initializin
| * | | 839ba9f : needed to be able to ask Repo.new to work with a bar
| | * | d065e76 : empty commit to push project to runcoderun
* | | | 791ec6b : updated grit gemspec
* | | | 756a947 : including code from github updates
| | * | 3fa3284 : whitespace
| | * | d01cffd : whitespace
| * | | a0e4a3d : updated grit gemspec
| * | | 7569d0d : including code from github updates

```

最后，你也可以用 ‘--reverse'参数来逆向显示所有日志。

# 比较提交 - Git Diff

你可以用 [git diff](http://www.kernel.org/pub/software/scm/git/docs/git-diff.html) 来比较项目中任意两个版本的差异。

```
$ git diff master..test

```

上面这条命令只显示两个分支间的差异，如果你想找出‘master’,‘test’的共有 父分支和'test'分支之间的差异，你用 3 个‘.'来取代前面的两个'.' 。

```
$ git diff master...test

```

[git diff](http://www.kernel.org/pub/software/scm/git/docs/git-diff.html) 是一个难以置信的有用的工具，可以找出你项目上任意两点间 的改动，或是用来查看别人提交进来的新分支。

## 哪些内容会被提交(commit)

你通常用[git diff](http://www.kernel.org/pub/software/scm/git/docs/git-diff.html)来找你当前工作目录和上次提交与本地索引间的差异。

```
$ git diff

```

上面的命令会显示在当前的工作目录里的，没有 staged(添加到索引中)，且在下次提交时 不会被提交的修改。

如果你要看在下次提交时要提交的内容(staged,添加到索引中),你可以运行：

```
$ git diff --cached

```

上面的命令会显示你当前的索引和上次提交间的差异；这些内容在不带"-a"参数运行 "git commit"命令时就会被提交。

```
$ git diff HEAD

```

上面这条命令会显示你工作目录与上次提交时之间的所有差别，这条命令所显示的 内容都会在执行"git commit -a"命令时被提交。

## 更多的比较选项

如果你要查看当前的工作目录与另外一个分支的差别，你可以用下面的命令执行:

```
$ git diff test

```

这会显示你当前工作目录与另外一个叫'test'分支的差别。你也以加上路径限定符，来只 比较某一个文件或目录。

```
$ git diff HEAD -- ./lib 

```

上面这条命令会显示你当前工作目录下的 lib 目录与上次提交之间的差别(或者更准确的 说是在当前分支)。

如果不是查看每个文件的详细差别，而是统计一下有哪些文件被改动，有多少行被改 动，就可以使用‘--stat' 参数。

```
$>git diff --stat
 layout/book_index_template.html                    |    8 ++-
 text/05_Installing_Git/0_Source.markdown           |   14 ++++++
 text/05_Installing_Git/1_Linux.markdown            |   17 +++++++
 text/05_Installing_Git/2_Mac_104.markdown          |   11 +++++
 text/05_Installing_Git/3_Mac_105.markdown          |    8 ++++
 text/05_Installing_Git/4_Windows.markdown          |    7 +++
 .../1_Getting_a_Git_Repo.markdown                  |    7 +++-
 .../0_ Comparing_Commits_Git_Diff.markdown         |   45 +++++++++++++++++++-
 .../0_ Hosting_Git_gitweb_repoorcz_github.markdown |    4 +-
 9 files changed, 115 insertions(+), 6 deletions(-)

```

有时这样全局性的查看哪些文件被修改，能让你更轻轻一点。

# 分布式的工作流程

假设 Alice 现在开始了一个新项目，在/home/alice/project 建了一个新的 git 仓库(repository)；另一个叫 Bob 的工作目录也在同一台机器，他要提交代码。

Bob 执行了这样的命令:

```
$ git clone /home/alice/project myrepo

```

这就建了一个新的叫"myrepo"的目录，这个目录里包含了一份 Alice 的仓库的 克隆(clone). 这份克隆和原始的项目一模一样，并且拥有原始项目的历史记 录。

Bob 做了一些修改并且提交(commit)它们:

```
(edit files)
$ git commit -a
(repeat as necessary)

```

当他准备好了，他告诉 Alice 从仓库/home/bob/myrepo 中把他的修改给拉 (pull)下来。她执行了下面几条命令:

```
$ cd /home/alice/project
$ git pull /home/bob/myrepo master

```

这就把 Bob 的主(master)分支合并到了 Alice 的当前分支里了。如果 Alice 在 Bob 修改文件内容的同时也做了修改的话，她可能需要手工去修复冲突. (注意："master"参数在上面的命令中并不一定是必须的，因为这是一个 默认参数)

git pull 命令执行两个操作: 它从远程分支(remote branch)抓取修改 的内容，然后把它合并进当前的分支。

如果你要经常操作远程分支(remote branch),你可以定义它们的缩写:

```
$ git remote add bob /home/bob/myrepo

```

这样，Alic 可以用"git fetch"" 来执行"git pull"前半部分的工作， 但是这条命令并不会把抓下来的修改合并到当前分支里。

```
$ git fetch bob

```

我们用`git remote`命令建立了 Bob 的运程仓库的缩写，用这个(缩写) 名字我从 Bob 那得到所有远程分支的历史记录。在这里远程分支的名 字就叫`bob/master`.

```
$ git log -p master..bob/master

```

上面的命令把 Bob 从 Alice 的主分支(master)中签出后所做的修改全部显示出来。

当检查完修改后,Alice 就可以把修改合并到她的主分支中。

```
$ git merge bob/master

```

这种合并(merge)也可以用 pull 来完成，就像下面的命令一样：

```
$ git pull . remotes/bob/master

```

注意：git pull 会把远程分支合并进当前的分支里，而不管你在命令 行里指定什么。

其后，Bob 可以更新它的本地仓库--把 Alice 做的修改拉过来(pull):

```
$ git pull

```

如果 Bob 从 Alice 的仓库克隆(clone)，那么他就不需要指定 Alice 仓库的地 址；因为 Git 把 Alice 仓库的地址存储到 Bob 的仓库配库文件，这个地址就是 在 git pull 时使用：

```
$ git config --get remote.origin.url
/home/alice/project

```

(如果要查看 git clone 创建的所有配置参数，可以使用"git config -l", [git config](http://www.kernel.org/pub/software/scm/git/docs/git-config.html) 的帮助文件里解释了每个参数的含义.)

Git 同时也保存了一份最初(pristine)的 Alice 主分支(master)，在 "origin/master"下面。

```
$ git branch -r
  origin/master

```

如果 Bob 打算在另外一台主机上工作，他可以通过 ssh 协议来执行"clone" 和"pull"操作：

```
$ git clone alice.org:/home/alice/project myrepo

```

git 有他自带的协议(native protocol),还可以使用 rsync 或 http; 你可以点 这里 [git pull](http://www.kernel.org/pub/software/scm/git/docs/git-pull.html) 看一看更詳細的用法。

Git 也可以像 CVS 一样来工作：有一个中心仓库，不同的用户向它推送(push) 自己所作的修改；你可以看看这里： [git push](http://www.kernel.org/pub/software/scm/git/docs/git-push.html)[gitcvs-migration](http://www.kernel.org/pub/software/scm/git/docs/gitcvs-migration.html).

## 公共 Git 仓库

另外一个提交修改的办法，就是告诉项目的维护者(maintainer)用 [git pull](http://www.kernel.org/pub/software/scm/git/docs/git-pull.html) 命令从你的仓库里把修改拉下来。这和从主仓库"里更新代码类似，但是是从 另外一个方向来更新的。

如果你和维护者(maintainer)都在同一台机器上有帐号，那么你们可以互相从对 方的仓库目录里直接拉(pull)所作的修改；git 命令里的仓库地址也可以是本地 的某个目录名：

```
$ git clone /path/to/repository
$ git pull /path/to/other/repository

```

也可以是一个 ssh 地址：

```
$ git clone ssh://yourhost/~you/repository

```

如果你的项目只有很少几个开发者，或是只需要同步很少的几个私有仓库， 上面的方法也许够你用的。

然而，更通用的作法是维护几个不同的公开仓库(public repository). 这样可以把每个人的工作进度和公开仓库清楚的分开。

你还是每天在你的本地私人仓库里工作，但是会定期的把本地的修改推(push) 到你的公开仓库中；其它开发者就可以从这个公开仓库来拉(pull)最新的代码。 如果其它开发者也有他自己的公共仓库，那么他们之间的开发流程就如下图 所示：

```
                  you push
  your personal repo ------------------> your public repo
    ^                                     |
    |                                     |
    | you pull                            | they pull
    |                                     |
    |                                     |
        |               they push             V
  their public repo <------------------- their repo

```

## 将修改推到一个公共仓库

通过 http 或是 git 协议，其它维护者可以抓取(fetch)你最近的修改，但是他们 没有写权限。这样，这需要将本地私有仓库的最近修改上传公共仓库中。

译者注: 通过 http 的 WebDav 协议是可以有写权限的,也有人配置了 git over http.

最简单的办法就是用 [git push](http://www.kernel.org/pub/software/scm/git/docs/git-push.html)命令 和 ssh 协议; 用你本地的"master" 分支去更新远程的"master"分支，执行下面的命令:

```
$ git push ssh://yourserver.com/~you/proj.git master:master

```

或是:

```
$ git push ssh://yourserver.com/~you/proj.git master

```

和 git-fetch 命令一样 git-push 如果命令的执行结果不是"快速向前"(fast forward) 就会报错; 下面的章节会讲如何处理这种情况.

推(push)命令的目地仓库一般是个裸仓库(bare respository). 你也可以推到一 个签出工作目录树(checked-out working tree)的仓库，但是工作目录中内 容不会被推命令所更新。如果你把自己的分支推到一个已签出的分支里，这 会导致不可预知的后果。

在用 git-fetch 命令时，你也可以修改配置参数，让你少打字:)。

下面这些是例子:

```
$ cat >>.git/config <<EOF
[remote "public-repo"]
    url = ssh://yourserver.com/~you/proj.git
EOF

```

你可以用下面的命令来代替前面复杂的命令:

```
$ git push public-repo master

```

你可以点击这里: [git config](http://www.kernel.org/pub/software/scm/git/docs/git-config.html)，查看 remote..url, branch..remote, 和 remote..push 等选项的解释.

## 当推送代码失败时要怎么办

如果推送(push)结果不是"快速向前"(fast forward),那么它 可能会报像下面一样的错误：

```
error: remote 'refs/heads/master' is not an ancestor of
local  'refs/heads/master'.
Maybe you are not up-to-date and need to pull first?
error: failed to push to 'ssh://yourserver.com/~you/proj.git'

```

这种情况通常由以下的原因产生：

```
- 用 `git-reset --hard` 删除了一个已经发布了的一个提交，或是

- 用 `git-commit --amend` 去替换一个已经发布的提交，或是

- 用 `git-rebase` 去 rebase 一个已经发布的提交. 

```

你可以强制 git-push 在上传修改时先更新，只要在分支名前面加一个加号。

```
$ git push ssh://yourserver.com/~you/proj.git +master

```

Normally whenever a branch head in a public repository is modified, it is modified to point to a descendant of the commit that it pointed to before. By forcing a push in this situation, you break that convention.

Nevertheless, this is a common practice for people that need a simple way to publish a work-in-progress patch series, and it is an acceptable compromise as long as you warn other developers that this is how you intend to manage the branch.

It's also possible for a push to fail in this way when other people have the right to push to the same repository. In that case, the correct solution is to retry the push after first updating your work: either by a pull, or by a fetch followed by a rebase; see the next section and [gitcvs-migration](http://www.kernel.org/pub/software/scm/git/docs/gitcvs-migration.html) for more.

# Git 标签

## 轻量级标签

我们可以用 [git tag](http://www.kernel.org/pub/software/scm/git/docs/git-tag.html)不带任何参数创建一个标签(tag)指定某个提交(commit):

```
$ git tag stable-1 1b2e1d63ff

```

这样，我们可以用 stable-1 作为提交(commit) "1b2e1d63ff" 的代称(refer)。

前面这样创建的是一个“轻量级标签"，这种分支通常是从来不移动的。

如果你想为一个标签(tag)添加注释，或是为它添加一个签名(sign it cryptographically), 那么我们就需要创建一个 ”标签对象".

## 标签对象

如果有 "-a", "-s" 或是 "-u " 中间的一个命令参数被指定，那么就会创建 一个标签对象，并且需要一个标签消息(tag message). 如果没有"-m " 或是 "-F " 这些参数，那么就会启动一个编辑器来让用户输入标签消息(tag message).

译者注：大家觉得这个标签消息是不是提交注释(commit comment)比较像。

当这样的一条命令执行后，一个新的对象被添加到 Git 对象库中，并且标签引用就指向了 一个标签对象，而不是指向一个提交(commit). 这样做的好处就是：你可以为一个标签 打处签名(sign), 方便你以后来查验这是不是一个正确的提交(commit).

下面是一个创建标签对象的例子:

```
$ git tag -a stable-1 1b2e1d63ff

```

标签对象可以指向任何对象，但是在通常情况下是一个提交(commit). (在 Linux 内核代 码中，第一个标签对象是指向一个树对象(tree),而不是指向一个提交(commit)).

## 签名的标签

如果你配有 GPG key,那么你就很容易创建签名的标签.首先你要在你的 *.git/config 或 *~.gitconfig 里配好 key.

下面是示例:

```
[user]
    signingkey = <gpg-key-id>

```

你也可以用命令行来配置:

```
$ git config (--global) user.signingkey <gpg-key-id>

```

现在你可以直接用"-s" 参数来创“签名的标签”。

```
$ git tag -s stable-1 1b2e1d63ff

```

如果没有在配置文件中配 GPG key,你可以用"-u“ 参数直接指定。

```
$ git tag -u <gpg-key-id> stable-1 1b2e1d63ff

```