# 5\. 高级技能

# 创建新的空分支

在偶尔的情况下，你可能会想要保留那些与你的代码没有共同祖先的分支。例如在这些分支上保留生成的文档或者其他一些东西。如果你需要创建一个不使用当前代码库作为父提交的分支，你可以用如下的方法创建一个空分支：

```
git symbolic-ref HEAD refs/heads/newbranch 
rm .git/index 
git clean -fdx 
<do work> 
git add your files 
git commit -m 'Initial commit'

```

# 修改你的历史

交互式洐合是修改单个提交的好方法。

[git filter-branch](http://www.kernel.org/pub/software/scm/git/docs/git-filter-branch.html)是修改大量提交的好方法。

# 高级分支与合并

## 在合并过程中得到解决冲突的协助

git 会把所有可以自动合并的修改加入到索引中去, 所以[git diff](http://www.kernel.org/pub/software/scm/git/docs/git-diff.html)只会显示有冲突的部分. 它使用了一种不常见的语法:

```
$ git diff
diff --cc file.txt
index 802992c,2b60207..0000000
--- a/file.txt
+++ b/file.txt
@@@ -1,1 -1,1 +1,5 @@@
++<<<<<<< HEAD:file.txt
 +Hello world
++=======
+ Goodbye
++>>>>>>> 77976da35a11db4580b80ae27e8d65caf5208086:file.txt

```

回忆一下, 在我们解决冲突之后, 得到的提交会有两个而不是一个父提交: 一个父提交会成为 HEAD, 也就是当前分支的 tip; 另外一个父提交会成为另一分支的 tip, 被暂时存在 MERGE_HEAD。

在合并过程中, 索引中保存着每个文件的三个版本. 三个"文件暂存(file stage)"中的每一个都代表了文件的不同版本:

```
$ git show :1:file.txt  # 两个分支共同祖先中的版本.
$ git show :2:file.txt  # HEAD 中的版本.
$ git show :3:file.txt  # MERGE_HEAD 中的版本.

```

当你使用[git diff](http://www.kernel.org/pub/software/scm/git/docs/git-diff.html)去显示冲突时, 它在工作树(work tree), 暂存 2(stage 2)和暂存 3(stage 3)之间执行三路 diff 操作, 只显示那些两方都有的块(换句话说, 当一个块的合并结果只从暂存 2 中得到时, 是不会被显示出来的; 对于暂存 3 来说也是一样)。

上面的 diff 结果显示了 file.txt 在工作树, 暂存 2 和暂存 3 中的差异. git 不在每行前面加上单个'+'或者'-', 相反地, 它使用两栏去显示差异: 第一栏用于显示第一个父提交与工作目录文件拷贝的差异, 第二栏用于显示第二个父提交与工作文件拷贝的差异. (参见[git diff-files](http://www.kernel.org/pub/software/scm/git/docs/git-diff-files.html)中的"COMBINED DIFF FORMAT"取得此格式详细信息.)

在用直观的方法解决冲突之后(但是在更新索引之前), diff 输出会变成下面的样子:

```
$ git diff
diff --cc file.txt
index 802992c,2b60207..0000000
--- a/file.txt
+++ b/file.txt
@@@ -1,1 -1,1 +1,1 @@@
- Hello world
-Goodbye
++Goodbye world

```

上面的输出显示了解决冲突后的版本删除了第一个父版本提供的"Hello world"和第二个父版本提供的"Goodbye", 然后加入了两个父版本中都没有的"Goodbye world"。

一些特别 diff 选项允许你对比工作目录和三个暂存中任何一个的差异:

```
$ git diff -1 file.txt      # 与暂存 1 进行比较
$ git diff --base file.txt          # 与上相同
$ git diff -2 file.txt      # 与暂存 2 进行比较
$ git diff --ours file.txt          # 与上相同
$ git diff -3 file.txt      # 与暂存 3 进行比较
$ git diff --theirs file.txt    # 与上相同.

```

[git log](http://www.kernel.org/pub/software/scm/git/docs/git-log.html)和[gitk](http://www.kernel.org/pub/software/scm/git/docs/gitk.html)命令也为合并操作提供了特别的协助:

```
$ git log --merge
$ gitk --merge

```

这会显示所有那些只在 HEAD 或者只在 MERGE_HEAD 中存在的提交, 还有那些更新(touch)了未合并文件的提交.

你也可以使用[git mergetool](http://www.kernel.org/pub/software/scm/git/docs/git-mergetool.html), 它允许你使用外部工具如 emacs 或 kdiff3 去合并文件。

每次你解决冲突之后, 应该更新索引:

```
$ git add file.txt

```

完成索引更新之后, git-diff(缺省地)不再显示那个文件的差异, 所以那个文件的不同暂存版本会被"折叠"起来。

## 多路合并

你可以一次合并多个头, 只需简单地把它们作为[git merge](http://www.kernel.org/pub/software/scm/git/docs/git-merge.html)的参数列出. 例如,

```
$ git merge scott/master rick/master tom/master

```

相当于:

```
$ git merge scott/master
$ git merge rick/master
$ git merge tom/master

```

## 子树

有时会出现你想在自己项目中引入其他独立开发项目的内容的情况. 在没有路径冲突的前提下, 你只需要简单地从其他项目拉取内容即可。

如果有冲突的文件, 那么就会出现问题. 可能的例子包括 Makefile 和其他一些标准文件名. 你可以选择合并这些冲突的文件, 但是更多的情况是你不愿意把它们合并. 一个更好解决方案是把外部项目作为一个子目录进行合并. 这种情况不被递归合并策略所支持, 所以简单的拉取是无用的。

在这种情况下, 你需要的是子树合并策略。

这下面例子中, 我们设定你有一个仓库位于/path/to/B (如果你需要的话, 也可以是一个 URL). 你想要合并那个仓库的 master 分支到你当前仓库的 dir-B 子目录下。

下面就是你所需要的命令序列:

```
$ git remote add -f Bproject /path/to/B (1)
$ git merge -s ours --no-commit Bproject/master (2)
$ git read-tree --prefix=dir-B/ -u Bproject/master (3)
$ git commit -m "Merge B project as our subdirectory" (4)
$ git pull -s subtree Bproject master (5)

```

子树合并的好处就是它并没有给你仓库的用户增加太多的管理负担. 它兼容于较老(版本号小于 1.5.2)的客户端, 克隆完成之后马上可以得到代码。

然而, 如果你使用子模块(submodule), 你可以选择不传输这些子模块对象. 这可能在子树合并过程中造成问题。

> 译者注: submodule 是 Git 的另一种将别的仓库嵌入到本地仓库方法.

另外, 若你需要修改内嵌外部项目的内容, 使用子模块方式可以更容易地提交你的修改.

# 查找问题的利器 - Git Bisect

假设你在项目的'2.6.18'版上面工作, 但是你当前的代码(master)崩溃(crash)了. 有时解决这种问题的最好办法是: 手工逐步恢复(brute-force regression)项目历史,　找出是哪个提交(commit)导致了这个问题. 但是 linkgit:git-bisect[1](http://gitbook.liuhui998.com/%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE) 可以更好帮你解决这个问题:

```
$ git bisect start
$ git bisect good v2.6.18
$ git bisect bad master
Bisecting: 3537 revisions left to test after this
[65934a9a028b88e83e2b0f8b36618fe503349f8e] BLOCK: Make USB storage depend on SCSI rather than selecting it [try #6]

```

如果你现在运行"git branch",　会发现你现在所在的是"no branch"(译者注:这是进行 git bisect 的一种状态). 这时分支指向提交（commit):"69543", 此提交刚好是在"v2.6.18"和“master"中间的位置. 现在在这个分支里,　编译并测试项目代码, 查看它是否崩溃(crash). 假设它这次崩溃了, 那么运行下面的命令:

```
$ git bisect bad
Bisecting: 1769 revisions left to test after this
[7eff82c8b1511017ae605f0c99ac275a7e21b867] i2c-core: Drop useless bitmaskings

```

现在 git 自动签出(checkout)一个更老的版本. 继续这样做, 用"git bisect good","git bisect bad"告诉 git 每次签出的版本是否没有问题; 你现在可以注意一下当前的签出的版本, 你会发现 git 在用"二分查找(binary search)方法"签出"bad"和"good"之间的一个版本(commit or revison)。

在这个项目(case)中, 经过 13 次尝试, 找出了导致问题的提交(guilty commit). 你可以用 [git show](http://www.kernel.org/pub/software/scm/git/docs/git-show.html) 命令查看这个提交(commit), 找出是谁做的修改，然后写邮件给 TA. 最后, 运行:

```
$ git bisect reset

```

这会到你之前(执行 git bisect start 之前)的状态。

注意: git-bisect 每次所选择签出的版本, 只是一个建议; 如果你有更好的想法, 也可以去试试手工选择一个不同的版本。

运行:

```
$ git bisect visualize

```

这会运行 gitk, 界面上会标识出"git bisect"命令自动选择的提交(commit). 你可以选择一个相邻的提交(commit), 记住它的 SHA 串值, 用下面的命令把它签出来:

```
$ git reset --hard fb47ddb2db...

```

然后进行测试, 再根据测试結果执行”bisect good"或是"bisect bad"; 就这样反复执行, 直到找出问题为止.

译者注: 关于"git bisect start"后的分支状态, 译文和原文不一致. 原文是说执行"git bisect start"后会创建一个名为"bisect"的分支, 但是实际情况却是处于"no branch"的状态。

# 查找问题的利器 - Git Blame

如果你要查看文件的每个部分是谁修改的, 那么 [git blame](http://www.kernel.org/pub/software/scm/git/docs/git-blame.html) 就是不二选择. 只要运行'git blame [filename]', 你就会得到整个文件的每一行的详细修改信息:包括 SHA 串,日期和作者:

译者注: Git 采用 SHA1 做为 hash 签名算法, 在本书中,作者为了表达方便,常常使用 SHA 来代指 SHA1\. 如果没有特别说明, 本书中的 SHA 就是 SHA1 的代称.

```
$ git blame sha1_file.c
...
0fcfd160 (Linus Torvalds  2005-04-18 13:04:43 -0700    8)  */
0fcfd160 (Linus Torvalds  2005-04-18 13:04:43 -0700    9) #include "cache.h"
1f688557 (Junio C Hamano  2005-06-27 03:35:33 -0700   10) #include "delta.h"
a733cb60 (Linus Torvalds  2005-06-28 14:21:02 -0700   11) #include "pack.h"
8e440259 (Peter Eriksen   2006-04-02 14:44:09 +0200   12) #include "blob.h"
8e440259 (Peter Eriksen   2006-04-02 14:44:09 +0200   13) #include "commit.h"
8e440259 (Peter Eriksen   2006-04-02 14:44:09 +0200   14) #include "tag.h"
8e440259 (Peter Eriksen   2006-04-02 14:44:09 +0200   15) #include "tree.h"
f35a6d3b (Linus Torvalds  2007-04-09 21:20:29 -0700   16) #include "refs.h"
70f5d5d3 (Nicolas Pitre   2008-02-28 00:25:19 -0500   17) #include "pack-revindex.h"628522ec (Junio C Hamano              2007-12-29 02:05:47 -0800   18) #include "sha1-lookup.h"
...

```

如果文件被修改了(reverted),或是编译(build)失败了; 这个命令就可以大展身手了.

你也可以用"-L"参数在命令(blame)中指定开始和结束行:

```
$>git blame -L 160,+10 sha1_file.c 
ace1534d (Junio C Hamano 2005-05-07 00:38:04 -0700       160)}
ace1534d (Junio C Hamano 2005-05-07 00:38:04 -0700       161)
0fcfd160 (Linus Torvalds 2005-04-18 13:04:43 -0700       162)/*
0fcfd160 (Linus Torvalds 2005-04-18 13:04:43 -0700       163) * NOTE! This returns a statically allocate
790296fd (Jim Meyering   2008-01-03 15:18:07 +0100       164) * careful about using it. Do an "xstrdup()
0fcfd160 (Linus Torvalds 2005-04-18 13:04:43 -0700       165) * filename.
ace1534d (Junio C Hamano 2005-05-07 00:38:04 -0700       166) *
ace1534d (Junio C Hamano 2005-05-07 00:38:04 -0700       167) * Also note that this returns the location
ace1534d (Junio C Hamano 2005-05-07 00:38:04 -0700       168) * SHA1 file can happen from any alternate 
d19938ab (Junio C Hamano 2005-05-09 17:57:56 -0700       169) * DB_ENVIRONMENT environment variable if i

```

# Git 和 Email

## 向一个项目提交补丁

如果你只做了少量的改动, 最简单的提交方法就是把它们做成补丁(patch)用邮件发出去:

首先, 使用[git format-patch](http://www.kernel.org/pub/software/scm/git/docs/git-format-patch.html); 例如:

```
$ git format-patch origin

```

这会在当前目录生成一系统编号的补丁文件, 每一个补丁文件都包含了当前分支和 origin/HEAD 之间的差异内容。

然后你可以手工把这些文件导入你的 Email 客户端. 但是如果你需要一次发送很多补丁, 你可能会更喜欢使用[git send-email](http://www.kernel.org/pub/software/scm/git/docs/git-send-email.html)脚本去自动完成这个工作. 在发送之前, 应当先到项目的邮件列表上咨询一下项目管理者, 了解他们管理这些补丁的方式。

## 向一个项目中导入补丁

Git 也提供了一个名为[git am](http://www.kernel.org/pub/software/scm/git/docs/git-am.html)的工具(am 是"apply mailbox"的缩写)去应用那些通过 Email 寄来的系列补丁. 你只需要按顺序把所有包含补丁的消息存入单个的 mailbox 文件, 比如说"patches.mbox", 然后运行

```
$ git am -3 patches.mbox

```

Git 会按照顺序应用每一个补丁; 如果发生了冲突, git 会停下来让你手工解决冲突从而完成合并. ("-3"选项会让 git 执行合并操作; 如果你更喜欢中止并且不改动你的工作树和索引, 你可以省略"-3"选项.)

在解决冲突和更新索引之后, 你不需要再创建一个新提交, 只需要运行

```
$ git am --resolved

```

这时 git 会为你创建一个提交, 然后继续应用 mailbox 中余下的补丁.

最后的效果是, git 产生了一系列提交, 每个提交是原来 mailbox 中的一个补丁, 补丁中的作者信息和提交日志也一并被记录下来。

# 定制 Git

[git config](http://www.kernel.org/pub/software/scm/git/docs/git-config.html)

## 更改你的编辑器

```
$ git config --global core.editor emacs

```

## 添加别名

```
$ git config --global alias.last 'cat-file commit HEAD'

$ git last
tree c85fbd1996b8e7e5eda1288b56042c0cdb91836b
parent cdc9a0a28173b6ba4aca00eb34f5aabb39980735
author Scott Chacon <schacon@gmail.com> 1220473867 -0700
committer Scott Chacon <schacon@gmail.com> 1220473867 -0700

fixed a weird formatting problem

$ git cat-file commit HEAD
tree c85fbd1996b8e7e5eda1288b56042c0cdb91836b
parent cdc9a0a28173b6ba4aca00eb34f5aabb39980735
author Scott Chacon <schacon@gmail.com> 1220473867 -0700
committer Scott Chacon <schacon@gmail.com> 1220473867 -0700

fixed a weird formatting problem

```

## 添加颜色

所有的 color.*选项请参见[git config](http://www.kernel.org/pub/software/scm/git/docs/git-config.html)的文档

```
$ git config color.branch auto
$ git config color.diff auto
$ git config color.interactive auto
$ git config color.status auto

```

或者你可以通过 color.ui 选项把颜色全部打开:

```
$ git config color.ui true

```

## 提交模板

```
$ git config commit.template '/etc/git-commit-template'

```

## 日志格式

```
$ git config format.pretty oneline

```

## 其他配置选项

除上面提到的选项外, 还有很多很有趣的选项去配置打包, 垃圾回收, 合并, 分支, http 传输, diff, 分页, 空白字符等等的行为. 如果你需要更加深入地调教 git, 请阅读[git config](http://www.kernel.org/pub/software/scm/git/docs/git-config.html)文档。

# Git Hooks

钩子(hooks)是一些在"$GIT-DIR/hooks"目录的脚本, 在被特定的事件(certain points)触发后被调用。当"git init"命令被调用后, 一些非常有用的示例钩子文件(hooks)被拷到新仓库的 hooks 目录中; 但是在默认情况下这些钩子(hooks)是不生效的。 把这些钩子文件(hooks)的".sample"文件名后缀去掉就可以使它们生效了。

## applypatch-msg

```
GIT_DIR/hooks/applypatch-msg

```

当'git-am'命令执行时，这个钩子就被调用。它只有一个参数：就是存有提交消息(commit log message)的文件的名字。如果钩子的执行结果是非零，那么补丁(patch)就不会被应用(apply)。

The hook is allowed to edit the message file in place, and can be used to normalize the message into some project standard format (if the project has one). It can also be used to refuse the commit after inspecting the message file. The default applypatch-msg hook, when enabled, runs the commit-msg hook, if the latter is enabled.

这个钩子用于在其它地方编辑提交消息，并且可以把这些消息规范成项目的标准格式(如果项目些类的标准的话)。它也可以在分析(inspect)完消息文件后拒绝此次提交(commit)。在默认情况下，当 applypatch-msg 钩子被启用时。。。。

()

## pre-applypatch

```
GIT_DIR/hooks/pre-applypatch

```

当'git-am'命令执行时，这个钩子就被调用。它没有参数，并且是在一个补丁(patch)被应用后还未提交(commit)前被调用。如果钩子的执行结果是非零，那么刚才应用的补丁(patch)就不会被提交。

It can be used to inspect the current working tree and refuse to make a commit if it does not pass certain test. The default pre-applypatch hook, when enabled, runs the pre-commit hook, if the latter is enabled.

它用于检查当前的工作树，当提交的补丁不能通过特定的测试就拒绝将它提交(commit)进仓库。 ()

## post-applypatch

```
GIT_DIR/hooks/post-applypatch

```

This hook is invoked by 'git-am'. It takes no parameter, and is invoked after the patch is applied and a commit is made.

当'git-am'命令执行时，这个钩子就被调用。它没有参数，并且是在一个补丁(patch)被应用且在完成提交(commit)情况下被调用。

This hook is meant primarily for notification, and cannot affect the outcome of 'git-am'.

这个钩子的主要用途是通知提示(notification)，它并不会影响'git-am'的执行和输出。

## pre-commit

```
GIT_DIR/hooks/pre-commit

```

这个钩子被 'git-commit' 命令调用, 而且可以通过在命令中添加`\--no-verify` 参数来跳过。这个钩子没有参数，在得到提交消息和开始提交(commit)前被调用。如果钩子执行结果是非零，那么 'git-commit' 命令就会中止执行。

译注：此钩子可以用来在提交前检查代码错误(运行类似 lint 的程序)。

当默认的'pre-commit'钩子开启时，如果它发现文件尾部有空白行，那么就会中止此次提交。

译注：新版的默认钩子和这里所说有所有不同。

All the 'git-commit' hooks are invoked with the environment variable `GIT_EDITOR=:` if the command will not bring up an editor to modify the commit message.

下面是一个运行 Rspec 测试的 Ruby 脚本，如果没有通过这个测试，那么不允许提交(commit)。

html_path = "spec_results.html"
`spec -f h:#{html_path} -f p spec` # run the spec. send progress to screen. save html results to html_path

# find out how many errors were found

html = open(html_path).read
examples = html.match(/(\d+) examples/)[0].to_i rescue 0
failures = html.match(/(\d+) failures/)[0].to_i rescue 0
pending = html.match(/(\d+) pending/)[0].to_i rescue 0

if failures.zero?
puts "0 failures! #{examples} run, #{pending} pending"
else
puts "\aDID NOT COMMIT YOUR FILES!"
puts "View spec results at #{File.expand_path(html_path)}"
puts
puts "#{failures} failures! #{examples} run, #{pending} pending"
exit 1
end

## prepare-commit-msg

```
GIT_DIR/hooks/prepare-commit-msg

```

当'git-commit'命令执行时：在编辑器(editor)启动前，默认提交消息准备好后，这个钩子就被调用。

It takes one to three parameters. The first is the name of the file that the commit log message. The second is the source of the commit message, and can be: `message` (if a `-m` or `-F` option was given); `template` (if a `-t` option was given or the configuration option `commit.template` is set); `merge` (if the commit is a merge or a `.git/MERGE_MSG` file exists); `squash` (if a `.git/SQUASH_MSG` file exists); or `commit`, followed by a commit SHA1 (if a `-c`, `-C` or `\--amend` option was given).

它有三个参数。第一个是提交消息文件的名字。第二个是提交消息的来源，它可以是：().

如果钩子的执行結果是非零的话，那么'git-commit'命令就会被中止执行。

The purpose of the hook is to edit the message file in place, and it is not suppressed by the `\--no-verify` option. A non-zero exit means a failure of the hook and aborts the commit. It should not be used as replacement for pre-commit hook.

The sample `prepare-commit-msg` hook that comes with git comments out the `Conflicts:` part of a merge's commit message.

## commit-msg

```
GIT_DIR/hooks/commit-msg

```

当'git-commit'命令执行时，这个钩子被调用；也可以在命令中添加`\--no-verify`参数来跳过。这个钩子有一个参数：就是被选定的提交消息文件的名字。如这个钩子的执行結果是非零，那么'git-commit'命令就会中止执行。

The hook is allowed to edit the message file in place, and can be used to normalize the message into some project standard format (if the project has one). It can also be used to refuse the commit after inspecting the message file.

这个钩子的是为提交消息更适当，可以用于规范提交消息使之符合项目的标准(如果有的话)；如果它检查完提交消息后，发现内容不符合某些标准，它也可以拒绝此次提交(commit)。

The default 'commit-msg' hook, when enabled, detects duplicate "Signed-off-by" lines, and aborts the commit if one is found.

默认的'commit-msg'钩子启用后，它后检查里面是否有重复的签名结束线(Signed-off-by lines)，如果找到它就是中止此次提交(commit)操作。

## post-commit

```
GIT_DIR/hooks/post-commit

```

当'git-commit'命令执行时，这个钩子就被调用。它没有参数，并且是在一个提交(commit)完成时被调用。

这个钩子的主要用途是通知提示(notification)，它并不会影响'git-commit'的执行和输出。

## pre-rebase

```
GIT_DIR/hooks/pre-rebase

```

当'git-base'命令执行时，这个钩子就被调用；主要目的是阻止那不应被 rebase 的分支被 rebase(例如，一个已经发布的分支提交就不应被 rebase)。

## post-checkout

```
GIT_DIR/hooks/post-checkout

```

当'git-checkout'命令更新完整个工作树(worktree)后，这个钩子就会被调用。这个钩子有三个参数：前一个 HEAD 的 ref，新 HEAD 的 ref，判断一个签出是分支签出还是文件签出的标识符(分支签出＝1，文件签出＝0)。这个钩子不会影响'git-checkout'命令的输出。

这个钩子可以用于检查仓库的一致性，自动显示签出前后的代码的区别，也可以用于设置目录的元数据属性。

## post-merge

```
GIT_DIR/hooks/post-merge

```

This hook is invoked by 'git-merge', which happens when a 'git-pull' is done on a local repository. The hook takes a single parameter, a status flag specifying whether or not the merge being done was a squash merge. This hook cannot affect the outcome of 'git-merge' and is not executed, if the merge failed due to conflicts.

它有一个参数：

This hook can be used in conjunction with a corresponding pre-commit hook to save and restore any form of metadata associated with the working tree (eg: permissions/ownership, ACLS, etc). See contrib/hooks/setgitperms.perl for an example of how to do this.

## pre-receive

```
GIT_DIR/hooks/pre-receive

```

This hook is invoked by 'git-receive-pack' on the remote repository, which happens when a 'git-push' is done on a local repository. Just before starting to update refs on the remote repository, the pre-receive hook is invoked. Its exit status determines the success or failure of the update.

当用户在本地仓库执行'git-push'命令时，服务器上运端仓库就会对应执行'git-receive-pack'命令，而'git-receive-pack'命令会调用 pre-receive 钩子。在开始更新远程仓库上的 ref 之前，这个钩子被调用。钩子的执行结果(exit status)决定此次更新能否成功。

This hook executes once for the receive operation. It takes no arguments, but for each ref to be updated it receives on standard input a line of the format:

每执行一个接收(receive)操作都会调用一次这个钩子。它没有命令行参数，但是它会从标准输入(standard input)读取需要更新的 ref，格式如下：

```
SP SP LF

```

译者注：SP 是空格，LF 是回车。

where ` is the old object name stored in the ref, ` is the new object name to be stored in the ref and` is the full name of the ref. When creating a new ref, ` is 40 `0`.

`是保存在 ref 里的老对象的名字，`是保存在 ref 里的新对象的名字，`就是此次要更新的 ref 的全名。如果是创建一个新的 ref，那么`就是由 40 个`0`组成的字符串表示。

If the hook exits with non-zero status, none of the refs will be updated. If the hook exits with zero, updating of individual refs can still be prevented by the > hook.

如果钩子的执行结果是非零，那么没有引用(ref)会被更新。如果执行结果为零，更新操作还可以被后面的 > 钩子所阻止。

Both standard output and standard error output are forwarded to 'git-send-pack' on the other end, so you can simply `echo`messages for the user.

钩子(hook)的标准输出和标准错误输出(stdout & stderr)都会通'git-send-pack'转发给客户端(other end)，你可以把这个信息回显(echo)给用户。

If you wrote it in Ruby, you might get the args this way:

如果你用 ruby,那么可以像下面的代码一样得到它们的参数。

rev_old, rev_new, ref = STDIN.read.split(" ")

Or in a bash script, something like this would work:

在 bash 脚本中，下面代码也可能得到参数。

```
#!/bin/sh
# <oldrev> <newrev> <refname>
# update a blame tree
while read oldrev newrev ref
do
    echo "STARTING [$oldrev $newrev $ref]"
    for path in `git diff-tree -r $oldrev..$newrev | awk '{print $6}'`
    do
      echo "git update-ref refs/blametree/$ref/$path $newrev"
      `git update-ref refs/blametree/$ref/$path $newrev`
    done
done

```

## update

```
GIT_DIR/hooks/update

```

当用户在本地仓库执行'git-push'命令时，服务器上运端仓库就会对应执行'git-receive-pack'，而'git-receive-pack'会调用 update 钩子。在更新远程仓库上的 ref 之前，这个钩子被调用。钩子的执行结果(exit status)决定此次 update 能否成功。

每更新一个引用(ref)，钩子就会被调用一次，并且使用三个参数:

*   the name of the ref being updated, # 要被更的 ref 的名字
*   the old object name stored in the ref, # ref 中更新前的对象名
*   and the new objectname to be stored in the ref. # ref 中更新后的对象名

如果 update hook 的执行结果是零，那么引用(ref)就会被更新。如果执行结果是非零，那么’git-receive-pack'就不会更新这个引用(ref)。

This hook can be used to prevent 'forced' update on certain refs by making sure that the object name is a commit object that is a descendant of the commit object named by the old object name. That is, to enforce a "fast forward only" policy.

这个钩子也可以用于防止强制更新某些 refs，确保 old object 是 new object 的父对象。这样也就是强制执行"fast forward only"策略。

It could also be used to log the old..new status. However, it does not know the entire set of branches, so it would end up firing one e-mail per ref when used naively, though. The > hook is more suited to that.

它也可以用于跟踪(log)更新详情。但是由于它不知道每次更新的 ref 全体集合，尽管可以傻傻的每个 ref 更新就发送 email；但是>钩子更适合这种情况。

在邮件列表(mailing list)上讲了另外一种用法：用这个 update hook 实现细粒度(finer grained)权限控制。

钩子(hook)的标准输出和标准错误输出(stdout & stderr)都会通'git-send-pack'转发给客户端(other end)，你可以把这个信息回显(echo)给用户。

当默认的 update hook 被启用，且`hooks.allowunannotated`选项被打开时，那么没有注释(unannotated)的标签就不能被推送到服务器上。

## post-receive

```
GIT_DIR/hooks/post-receive

```

This hook is invoked by 'git-receive-pack' on the remote repository, which happens when a 'git-push' is done on a local repository. It executes on the remote repository once after all the refs have been updated.

当用户在本地仓库执行'git-push'命令时，服务器上运端仓库就会对应执行'git-receive-pack'命令；在所有远程仓库的引用(ref)都更新后，这个钩子就会被'git-receive-pack'调用。

This hook executes once for the receive operation. It takes no arguments, but gets the same information as the > hook does on its standard input.

服务器端仓库每次执行接收(receive)操作时，这个钩子就会被调用。此钩子执行不带任何命令行参数，但是和>钩子一样从标准输入(standard input)读取信息，并且读取的信息内容也是一样的。

This hook does not affect the outcome of 'git-receive-pack', as it is called after the real work is done.

这个钩子不会影响'git-receive-pack'命令的输出，因为它是在命令执行完后被调用的。

This supersedes the > hook in that it gets both old and new values of all the refs in addition to their names.

这个钩子可以取代 >钩子；因为后者只能得到需要更新的 ref 的名字，而没有更新前后的对象的名字。

Both standard output and standard error output are forwarded to 'git-send-pack' on the other end, so you can simply `echo`messages for the user.

钩子(hook)的标准输出和标准错误输出(stdout & stderr)都会通'git-send-pack'转发给客户端(other end)，你可以把这个信息回显(echo)给用户。

The default 'post-receive' hook is empty, but there is a sample script `post-receive-email` provided in the `contrib/hooks`directory in git distribution, which implements sending commit emails.

默认的'post-receive'的钩子是空的，但是在 git distribution `contrib/hooks` 目录里有一个名为 `post-receive-email` 的示例脚本，实实了发送 commit emails 的功能。

## post-update

```
GIT_DIR/hooks/post-update

```

This hook is invoked by 'git-receive-pack' on the remote repository, which happens when a 'git-push' is done on a local repository. It executes on the remote repository once after all the refs have been updated.

当用户在本地仓库执行'git-push'命令时，服务器上运端仓库就会对应执行'git-receive-pack'。在所有远程仓库的引用(ref)都更新后，post-update 就会被调用。

It takes a variable number of parameters, each of which is the name of ref that was actually updated.

它的参数数目是可变的，每个参数代表实际被更新的 ref。

This hook is meant primarily for notification, and cannot affect the outcome of 'git-receive-pack'.

这个钩子的主要用途是通知提示(notification)，它并不会影响'git-receive-pack'的输出。

The 'post-update' hook can tell what are the heads that were pushed, but it does not know what their original and updated values are, so it is a poor place to do log old..new. The > hook does get both original and updated values of the refs. You might consider it instead if you need them.

'post-update'可以行诉我们哪些 heads 被更新了，但是它不知道 head 更新前后的值，所以这里不大适合记录更新详情。而>钩子可以得到 ref(也可说是 head)更新前后的值，如果你要记录更详情的话，可以考虑使用这个钩子。

When enabled, the default 'post-update' hook runs 'git-update-server-info' to keep the information used by dumb transports (e.g., HTTP) up-to-date. If you are publishing a git repository that is accessible via HTTP, you should probably enable this hook.

如果默认的'post-update'钩子启用的话，它们执行‘git-update-server-info'命令去更新一些 dumb 协议(如 http)所需要的信息。如果你的 git 仓库是通 http 协议来访问，那么你就应该开启它。

Both standard output and standard error output are forwarded to 'git-send-pack' on the other end, so you can simply `echo`messages for the user.

钩子(hook)的标准输出和标准错误输出(stdout & stderr)都会通'git-send-pack'转发给客户端(other end)，你可以把这个信息回显(echo)给用户。

## pre-auto-gc

```
GIT_DIR/hooks/pre-auto-gc

```

当调用'git-gc --auto'命令时，这个钩子(hook)就会被调用。它没有调用参数，如果钩子的执行結果是非零的话，那么'git-gc --auto'命令就会中止执行。

## 参考

[Git Hooks](http://www.kernel.org/pub/software/scm/git/docs/githooks.html) * [`probablycorey.wordpress.com/2008/03/07/git-hooks-make-me-giddy/`](http://probablycorey.wordpress.com/2008/03/07/git-hooks-make-me-giddy/)

# 找回丢失的对象

译者注: 原书这里只有两个链接： [Recovering Lost Commits Blog Post](http://programblings.com/2008/06/07/the-illustrated-guide-to-recovering-lost-commits-with-git)，　 [Recovering Corrupted Blobs by Linus](http://www.kernel.org/pub/software/scm/git/docs/howto/recover-corrupted-blob-object.txt)

我根据第一个链接，整理了一篇[博文](http://liuhui998.com/2010/10/22/recover_lost_commits_with_git/)，并把它做为原书补充。

在玩 git 的过程中，常有失误的时候，有时把需要的东东给删了。 不过没有关系，git 给了我们一层安全网，让们能有机会把失去的东东给找回来。

Let's go!

## 准备

我们先创建一个用以实验的仓库，在里面创建了若干个提交和分支。 BTW：你可以直接把下面的命令复制到 shell 里执行。

```
mkdir recovery;cd recovery
git init
touch file
git add file
git commit -m "First commit"
echo "Hello World" > file
git add .
git commit -m "Greetings"
git branch cool_branch　
git checkout cool_branch
echo "What up world?" > cool_file
git add .
git commit -m "Now that was cool"
git checkout master
echo "What does that mean?" >> file

```

## 恢复已删除分支提交

现在 repo 里有两个 branch

```
$ git branch
cool_branch
* master

```

存储当前仓库未提交的改动

```
$ git stash save "temp save"
Saved working directory and index state On master: temp save
HEAD is now at e3c9b6b Greetings

```

删除一个分支

```
$ git branch -D cool_branch
Deleted branch cool_branch (was 2e43cd5).

$ git branch
 * master

```

用`git fsck --lost-found`命令找出刚才删除的分支里面的提交对象。

```
$git fsck --lost-found
  dangling commit 2e43cd56ee4fb08664cd843cd32836b54fbf594a

```

用 git show 命令查看一个找到的对象的内容，看是否为我们所找的。

```
git show 2e43cd56ee4fb08664cd843cd32836b54fbf594a

  commit 2e43cd56ee4fb08664cd843cd32836b54fbf594a
  Author: liuhui <liuhui998[#]gmail.com>
  Date:   Sat Oct 23 12:53:50 2010 +0800

  Now that was cool

  diff --git a/cool_file b/cool_file
  new file mode 100644
  index 0000000..79c2b89
  --- /dev/null
  +++ b/cool_file
  @@ -0,0 +1 @@
  +What up world?

```

这个提交对象确实是我们在前面删除的分支的内容；下面我们就要考虑一下要如何来恢复它了。

### 使用 git rebase　进行恢复

```
  $git rebase 2e43cd56ee4fb08664cd843cd32836b54fbf594a
  First, rewinding head to replay your work on top of it...
  Fast-forwarded master to 2e43cd56ee4fb08664cd843cd32836b54fbf594a.

```

现在我们用 git log 命令看一下，看看它有没有恢复:

```
  $ git log

  commit 2e43cd56ee4fb08664cd843cd32836b54fbf594a
  Author: liuhui <liuhui998[#]gmail.com>
  Date:   Sat Oct 23 12:53:50 2010 +0800

  Now that was cool

  commit e3c9b6b967e6e8c762b500202b146f514af2cb05
  Author: liuhui <liuhui998[#]gmail.com>
  Date:   Sat Oct 23 12:53:50 2010 +0800

  Greetings

  commit 5e90516a4a369be01b54323eb8b2660545051764
  Author: liuhui <liuhui998[#]gmail.com>
  Date:   Sat Oct 23 12:53:50 2010 +0800

  First commit

```

提交是找回来，但是分支没有办法找回来：

```
  liuhui@liuhui:~/work/test/git/recovery$ git branch
  * master

```

### 使用 git merge　进行恢复

我们把刚才的恢复的提交删除

```
  $ git reset --hard HEAD^
  HEAD is now at e3c9b6b Greetings

```

再把刚删的提交给找回来：

```
  git fsck --lost-found
  dangling commit 2e43cd56ee4fb08664cd843cd32836b54fbf594a

```

不过这回我们用是合并命令进行恢复：

```
  $ git merge 2e43cd56ee4fb08664cd843cd32836b54fbf594a
  Updating e3c9b6b..2e43cd5
  Fast-forward
  cool_file |    1 +
  1 files changed, 1 insertions(+), 0 deletions(-)
  create mode 100644 cool_file

```

## git stash 的恢复

前面我们用 git stash 把没有提交的内容进行了存储，如果这个存储不小心删了怎么办呢？

当前 repo 里有的存储：

```
$ git stash list
stash@{0}: On master: temp save

```

把它们清空：

```
$git stash clear
liuhui@liuhui:~/work/test/git/recovery$ git stash list

```

再用 git fsck --lost-found 找回来：

```
$git fsck --lost-found
dangling commit 674c0618ca7d0c251902f0953987ff71860cb067

```

用 git show 看一下回来的内容对不对：

```
$git show 674c0618ca7d0c251902f0953987ff71860cb067

commit 674c0618ca7d0c251902f0953987ff71860cb067
Merge: e3c9b6b 2b2b41e
Author: liuhui <liuhui998[#]gmail.com>
Date:   Sat Oct 23 13:44:49 2010 +0800

    On master: temp save

diff --cc file
index 557db03,557db03..f2a8bf3
--- a/file
+++ b/file
@@@ -1,1 -1,1 +1,2 @@@
  Hello World
  ++What does that mean?

```

看起来没有问题，好的，那么我就把它恢复了吧：

```
$ git merge 674c0618ca7d0c251902f0953987ff71860cb067
Merge made by recursive.
 file |    1 +
  1 files changed, 1 insertions(+), 0 deletions(-)

```

## 备注

这篇文章主要内容来自这里：[The illustrated guide to recovering lost commits with Git](http://programblings.com/2008/06/07/the-illustrated-guide-to-recovering-lost-commits-with-git/),我做了一些整理的工作。

如果对于文中的一些命令不熟，可以参考[Git Community Book 中文版](http://gitbook.liuhui998.com/)

其实这里最重要的一个命令就是：git fsck --lost-found，因为 git 中把 commit 删了后，并不是真正的删除，而是变成了悬空对象（dangling commit）。我们只要把把这悬空对象（dangling commit）找出来，用[git rebase](http://gitbook.liuhui998.com/4_2.html)也好，用[git merge](http://gitbook.liuhui998.com/3_3.html)也行就能把它们给恢复。

# 子模块

一个大项目通常由很多较小的, 自完备的模块组成. 例如, 一个嵌入式 Linux 发行版的代码树会包含每个进行过本地修改的软件的代码; 一个电影播放器可能需要基于一个知名解码库的特定版本完成编译; 数个独立的程序可能会共用同一个创建脚本.

在集中式版本管理系统中, 可以通过把每个模块放在一个单独的仓库中来完成上述的任务. 开发者可以把所有模块都签出(checkout), 也可以选择只签出他需要的模块. 在移动文件, 修改 API 和翻译时, 他们甚至可以在一个提交中跨多个模块修改文件.

Git 不允许部分签出(partial checkout), 所以采用上面(集中式版本管理)的方法会强迫开发者们保留一份他们不感兴趣的模块的本地拷贝. 在签出量巨大时, 提交会慢得超过你的预期, 因为 git 不得不扫描每一个目录去寻找修改. 如果模块有很多本地历史, 克隆可能永远不能完成.

从好的方面看来, 分布式版本管理系统可以更好地与外部资源进行整合. 在集中化的模式中, 外部项目的一个快照从它本身的版本控制系统中被分离出来, 然后此快照作为一个提供商分支(vendor branch)导入到本地的版本控制系统中去. 快照的历史不再可见. 而分布式管理系统中, 你可以把外部项目的历史一同克隆过来, 从而更好地跟踪外部项目的开发, 便于合并本地修改.

Git 的子模块(submodule)功能使得一个仓库可以用子目录的形式去包含一个外部项目的签出版本. 子模块维护它们自己的身份标记(identity); 子模块功能仅仅储存子模块仓库的位置和提交 ID, 因此其他克隆父项目("superproject")的开发者可以轻松克隆所有子模块的同一版本. 对父项目的部分签出成为可能: 你可以告诉 git 去克隆一部分或者所有的子模块, 也可以一个都不克隆.

Git 1.5.3 中加入了[git submodule](http://www.kernel.org/pub/software/scm/git/docs/git-submodule.html)这个命令. Git 1.5.2 版本的用户可以查找仓库的子模块并且手工签出; 更早的版本不支持子模块功能.

为说明子模块的使用方法, 创建 4 个用作子模块的示例仓库:

```
$ mkdir ~/git
$ cd ~/git
$ for i in a b c d
do
    mkdir $i
    cd $i
    git init
    echo "module $i" > $i.txt
    git add $i.txt
    git commit -m "Initial commit, submodule $i"
    cd ..
done

```

现在创建父项目, 加入所有的子模块:

```
$ mkdir super
$ cd super
$ git init
$ for i in a b c d
do
    git submodule add ~/git/$i $i
done

```

注意: 如果你想对外发布你的父项目, 请不要使用本地的地址!

列出`git-submodule`创建文件:

```
$ ls -a
.  ..  .git  .gitmodules  a  b  c  d

```

`git-submodule add`命令进行了如下的操作:

*   它在当前目录下克隆各个子模块, 默认签出 master 分支.
*   它把子模块的克隆路径加入到[gitmodules](http://www.kernel.org/pub/software/scm/git/docs/gitmodules.html)文件中, 然后把这个文件加入到索引, 准备进行提交.
*   它把子模块的当前提交 ID 加入到索引中, 准备进行提交.

提交父项目:

```
$ git commit -m "Add submodules a, b, c and d."

```

现在克隆父项目:

```
$ cd ..
$ git clone super cloned
$ cd cloned

```

子模块的目录创建好了, 但是它们是空的:

```
$ ls -a a
.  ..
$ git submodule status
-d266b9873ad50488163457f025db7cdd9683d88b a
-e81d457da15309b4fef4249aba9b50187999670d b
-c1536a972b9affea0f16e0680ba87332dc059146 c
-d96249ff5d57de5de093e6baff9e0aafa5276a74 d

```

注意: 上面列出的提交对象的名字会和你的项目中看到的有所不同, 但是它们应该和 HEAD 的提交对象名字一致. 你可以运行`git ls-remote ../git/a`进行检验.

拉取子模块需要进行两步操作. 首先运行`git submodule init`, 把子模块的 URL 加入到`.git/config`:

```
$ git submodule init

```

现在使用`git-submodule update`去克隆子模块的仓库和签出父项目中指定的那个版本:

```
$ git submodule update
$ cd a
$ ls -a
.  ..  .git  a.txt

```

`git-submodule update`和`git-submodule add`的一个主要区别就是`git-submodule update`签出一个指定的提交, 而不是该分支的 tip. 它就像签出一个标签(tag): 头指针脱离, 你不在任何一个分支上工作.

```
$ git branch
* (no branch)
master

```

如何你需要对子模块进行修改, 同时头指针又是脱离的状态, 那么你应该创建或者签出一个分支, 进行修改, 发布子模块的修改, 然后更新父项目让其引用新的提交:

```
$ git checkout master

```

或者

```
$ git checkout -b fix-up

```

然后

```
$ echo "adding a line again" >> a.txt
$ git commit -a -m "Updated the submodule from within the superproject."
$ git push
$ cd ..
$ git diff
diff --git a/a b/a
index d266b98..261dfac 160000
--- a/a
+++ b/a
@@ -1 +1 @@
-Subproject commit d266b9873ad50488163457f025db7cdd9683d88b
+Subproject commit 261dfac35cb99d380eb966e102c1197139f7fa24
$ git add a
$ git commit -m "Updated submodule a."
$ git push

```

如果你想要更新子模块, 你应该在`git pull`之后运行`git submodule update`.

## 子模块方式的陷阱

你应该总是在发布父项目的修改之前发布子模块修改. 如果你忘记发布子模块的修改, 其他人就无法克隆你的仓库了:

```
$ cd ~/git/super/a
$ echo i added another line to this file >> a.txt
$ git commit -a -m "doing it wrong this time"
$ cd ..
$ git add a
$ git commit -m "Updated submodule a again."
$ git push
$ cd ~/git/cloned
$ git pull
$ git submodule update
error: pathspec '261dfac35cb99d380eb966e102c1197139f7fa24' did not match any file(s) known to git.
Did you forget to 'git add'?
Unable to checkout '261dfac35cb99d380eb966e102c1197139f7fa24' in submodule path 'a'

```

如果你暂存了一个更新过的子模块, 准备进行手工提交, 注意不要在路径后面加上斜杠. 如果加上了斜杠, git 会认为你想要移除那个子模块然后签出那个目录内容到父仓库.

```
$ cd ~/git/super/a
$ echo i added another line to this file >> a.txt
$ git commit -a -m "doing it wrong this time"
$ cd ..
$ git add a/
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       deleted:    a
#       new file:   a/a.txt
#
# Modified submodules:
#
# * a aa5c351...0000000 (1):
#   < Initial commit, submodule a
#

```

为了修正这个错误的操作, 我们应该重置(reset)这个修改, 然后在 add 的时候不要加上末尾斜杠.

```
$ git reset HEAD A
$ git add a
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   a
#
# Modified submodules:
#
# * a aa5c351...8d3ba36 (1):
#   > doing it wrong this time
#

```

你也不应该把子模块的分支回退到超出任何父项目中记录的提交的范围.

如果你在没有签出分支的情况下对子模块进行了修改并且提交, 运行`git submodule update`将会不安全. 你所进行的修改会在无任何提示的情况下被覆盖.

```
$ cat a.txt
module a
$ echo line added from private2 >> a.txt
$ git commit -a -m "line added inside private2"
$ cd ..
$ git submodule update
Submodule path 'a': checked out 'd266b9873ad50488163457f025db7cdd9683d88b'
$ cd a
$ cat a.txt
module a

```

注意: 这些修改在子模块的 reflog 中仍然可见.

如果你不想提交你的修改, 那又是另外一种情况了.